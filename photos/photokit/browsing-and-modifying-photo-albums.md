# Browsing and Modifying Photo Albums

사용자가 PhotoKit을 사용하여 사진을 앨범으로 구성하고 그리드 기반 레이아웃에서 사진 컬렉션을 찾아보도록 지원한다.

## Overview

iOS의 포토 앱은 썸네일 그리드에 에셋을 표시한다. 이 샘플은 커스텀 `UICollectionViewController` 를 사용하여 유사한 레이아웃을 달성하는 방법을 보여준다. PhotoKit을 사용하여 에셋 미리 보기를 가져온 다음 단일 사진, 비디오 또는 라이브 사진 에셋으로 표시한다.

샘플 앱 PhotoBrowse는 또한 사용자의 사진을 앨범 및 기본 제공 컬렉션\(예: 최근 추가 및 즐겨찾기\)으로 구성하는 방법을 설명한다. 앨범 생성, 삭제, 수정은 물론 개별 에셋의 편집과 즐겨찾기를 지원한다.

## List Albums and Built-in Collections

앱이 처음 시작되면 사용자의 사진 에셋을 모두 가져온다. 사진, 앨범 및 사용자 컬렉션 등 에셋 데이터에 대한 모든 요청은 PhotoKit의 shared PHPhotoLibrary 객체를 통해 이루어지므로, 앱은 뷰가 로드되면 메인 뷰 컨트롤러를 등록한다.

```swift
PHPhotoLibrary.shared().register(self)
```

사용자의 모든 앨범과 컬렉션을 나열하려면 앱에서 PHFetchOptions 객체를 생성하고 다음과 같은 몇가지 가져오기 요청을 발송하라.

```swift
let allPhotosOptions = PHFetchOptions()allPhotosOptions.sortDescriptors = [NSSortDescriptor(key: "creationDate", ascending: true)]allPhotos = PHAsset.fetchAssets(with: allPhotosOptions)smartAlbums = PHAssetCollection.fetchAssetCollections(with: .smartAlbum, subtype: .albumRegular, options: nil)userCollections = PHCollectionList.fetchTopLevelUserCollections(with: nil)
```

`PHFetchResult` 의 결과는 사용자 인터페이스에서 각 앨범의 사진 수를 표시하는 것을 허용함으로 사용자의 사진 라이브러리 구조에 대해 앱에 알린다.

## Display Assets in a Thumbnail Grid

샘플 앱은 AssetGridController의 UICollectionViewController 서브 클래스를 통해 썸네일 그리드 탐색을 구현한다.

그리드 뷰 컨트롤러가 로드된 후 이미지 캐시를 업데이트하여 사용자가 스크롤할 때 미리 보기를 빠르게 로드할 수 있다.

```swift
let (addedRects, removedRects) = differencesBetweenRects(previousPreheatRect, preheatRect)let addedAssets = addedRects    .flatMap { rect in collectionView!.indexPathsForElements(in: rect) }    .map { indexPath in fetchResult.object(at: indexPath.item) }let removedAssets = removedRects    .flatMap { rect in collectionView!.indexPathsForElements(in: rect) }    .map { indexPath in fetchResult.object(at: indexPath.item) }// Update the assets the PHCachingImageManager is caching.imageManager.startCachingImages(for: addedAssets,                                targetSize: thumbnailSize, contentMode: .aspectFill, options: nil)imageManager.stopCachingImages(for: removedAssets,                               targetSize: thumbnailSize, contentMode: .aspectFill, options: nil)
```

전체 에셋 대신 미리보기를 사용하도록 UICollectionView delegate 메서드 `cellForItem(at:)` 를 구현하라. PhotoKit을 사용하면 에셋을 직접 요청할 수 있으며 라이브 포토에 배지를 부착하여 에셋을 구분한다.

```swift
// Dequeue a GridViewCell.guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "GridViewCell", for: indexPath) as? GridViewCell    else { fatalError("Unexpected cell in collection view") }// Add a badge to the cell if the PHAsset represents a Live Photo.if asset.mediaSubtypes.contains(.photoLive) {    cell.livePhotoBadgeImage = PHLivePhotoView.livePhotoBadgeImage(options: .overContent)}// Request an image for the asset from the PHCachingImageManager.cell.representedAssetIdentifier = asset.localIdentifierimageManager.requestImage(for: asset, targetSize: thumbnailSize, contentMode: .aspectFill, options: nil, resultHandler: { image, _ in    // UIKit may have recycled this cell by the handler's activation time.    // Set the cell's thumbnail image only if it's still showing the same asset.    if cell.representedAssetIdentifier == asset.localIdentifier {        cell.thumbnailImage = image    }})
```

## Show a Single Photo, Video, or Live Photo

AssetViewController 는 단일 에셋의 뷰를 구현한다. 에셋이 비디오 또는 라이브 사진인 경우, 뷰 컨트롤러는 `UIBarButtonItem` 을 통한 재생도 지원한다.

```swift
// Set the appropriate toolbar items based on the media type of the asset.#if os(iOS)navigationController?.isToolbarHidden = falsenavigationController?.hidesBarsOnTap = trueif asset.mediaType == .video {    toolbarItems = [favoriteButton, space, playButton, space, trashButton]} else {    // In iOS, present both stills and Live Photos the same way, because    // PHLivePhotoView provides the same gesture-based UI as in the Photos app.    toolbarItems = [favoriteButton, space, trashButton]}#elseif os(tvOS)if asset.mediaType == .video {    navigationItem.leftBarButtonItems = [playButton, favoriteButton, trashButton]} else {    // In tvOS, PHLivePhotoView doesn't support playback gestures,    // so add a play button for Live Photos.    if asset.mediaSubtypes.contains(.photoLive) {        navigationItem.leftBarButtonItems = [favoriteButton, trashButton]    } else {        navigationItem.leftBarButtonItems = [livePhotoPlayButton, favoriteButton, trashButton]    }}#endif
```

tvOS에서 PhotoKit은 실시간 사진 재생을 지원한다.

```swift
#if os(tvOS)@IBAction func playLivePhoto(_ sender: Any) {    livePhotoView.startPlayback(with: .full)}#endif
```

뷰 컨트롤러는 PHImageManager가 비디오를 가져오면 AVPlayer를 만들어 항목 위에 계층화하여 재생을 지원한다.

```swift
// Request an AVPlayerItem for the displayed PHAsset.// Then configure a layer for playing it.PHImageManager.default().requestPlayerItem(forVideo: asset, options: options, resultHandler: { playerItem, info in    DispatchQueue.main.sync {        guard self.playerLayer == nil else { return }        // Create an AVPlayer and AVPlayerLayer with the AVPlayerItem.        let player = AVPlayer(playerItem: playerItem)        let playerLayer = AVPlayerLayer(player: player)        // Configure the AVPlayerLayer and add it to the view.        playerLayer.videoGravity = AVLayerVideoGravity.resizeAspect        playerLayer.frame = self.view.layer.bounds        self.view.layer.addSublayer(playerLayer)        player.play()        // Cache the player layer by reference, so you can remove it later.        self.playerLayer = playerLayer    }})
```

## Apply Canned Filters in an Editing Interface

AssetViewController 뷰를 사용하면 사진을 편집하고 변경사항을 사진 라이브러리에 다시 저장할 수 있다. alert 컨트롤러를 사용하여 세피아 톤, chrome, revert를 포함한 사전 설정 편집 옵션 목록을 표시한다.

```swift
// Allow editing only if the PHAsset supports edit operations.if asset.canPerform(.content) {    // Add actions for some canned filters.    alertController.addAction(UIAlertAction(title: NSLocalizedString("Sepia Tone", comment: ""),                                            style: .default, handler: getFilter("CISepiaTone")))    alertController.addAction(UIAlertAction(title: NSLocalizedString("Chrome", comment: ""),                                            style: .default, handler: getFilter("CIPhotoEffectChrome")))    // Add actions to revert any edits that have been made to the PHAsset.    alertController.addAction(UIAlertAction(title: NSLocalizedString("Revert", comment: ""),                                            style: .default, handler: revertAsset))}// Present the UIAlertController.present(alertController, animated: true)
```

각 옵션은 편집 요청을 생성하고 편집 결과를 데이터로 캡슐화하도록 PHContentEditingOutput을 준비한다. PHAssetChangeRequest 객체는 편집 내용을 사용자의 사진 라이브러리에 다시 전달한다.

```swift
DispatchQueue.global(qos: .userInitiated).async {    // Create adjustment data describing the edit.    let adjustmentData = PHAdjustmentData(formatIdentifier: self.formatIdentifier,                                          formatVersion: self.formatVersion,                                          data: filterName.data(using: .utf8)!)    // Create content editing output, write the adjustment data.    let output = PHContentEditingOutput(contentEditingInput: input)    output.adjustmentData = adjustmentData    // Select a filtering function for the asset's media type.    let applyFunc: (String, PHContentEditingInput, PHContentEditingOutput, @escaping () -> Void) -> Void    if self.asset.mediaSubtypes.contains(.photoLive) {        applyFunc = self.applyLivePhotoFilter    } else if self.asset.mediaType == .image {        applyFunc = self.applyPhotoFilter    } else {        applyFunc = self.applyVideoFilter    }    // Apply the filter.    applyFunc(filterName, input, output, {        // When the app finishes rendering the filtered result, commit the edit to the photo library.        PHPhotoLibrary.shared().performChanges({            let request = PHAssetChangeRequest(for: self.asset)            request.contentEditingOutput = output        }, completionHandler: { success, error in            if !success { print("Can't edit the asset: \(String(describing: error))") }        })    })}
```

사용자가 필터를 선택하면 앱이 필터를 적용하고 저장된 에셋을 즉시 출력한다. 편집을 선택했지만 아직 커밋되지 않은 경우 UI 상태가 없다. 따라서, PhotoBrowse는 진행 중인 편집 작업을 다시 시작 할 수 있는 조정 데이터를 읽는데 아무런 역할이 없다. 그러나 조정 데이터를 작성하여 미래의 앱 버전\(또는 사용자의 조정 데이터 형식을 이해하는 다른 앱\)을 활용할 수 있도록 하는 것은 여전히 좋은 습관이다.

## Create a New Album

alert 컨트롤러는 사용자에게 새로운 앨범을 만드는 것을 허용한다.

```swift
PHPhotoLibrary.shared().performChanges({    PHAssetCollectionChangeRequest.creationRequestForAssetCollection(withTitle: title)}, completionHandler: { success, error in    if !success { print("Error creating album: \(String(describing: error)).") }})
```

## Add an Asset to a Collection

사용자가 탐색 모음에서 추가 버튼\(+\)을 눌러 에셋 추가를 선택했을때 PhotoBrowse는 임의의 방향, 임의의 색상으로 목 사진을 만든다. 사용자의 사진 라이브러리의 다른 변경 사항과 마찬가지로, 에셋을 추가하려면 다음과 같이 PHAssetChangeRequest 내부에 추가사항을 포함해야 한다.

```swift
// Add the asset to the photo library.PHPhotoLibrary.shared().performChanges({    let creationRequest = PHAssetChangeRequest.creationRequestForAsset(from: image)    if let assetCollection = self.assetCollection {        let addAssetRequest = PHAssetCollectionChangeRequest(for: assetCollection)        addAssetRequest?.addAssets([creationRequest.placeholderForCreatedAsset!] as NSArray)    }}, completionHandler: {success, error in    if !success { print("Error creating the asset: \(String(describing: error))") }})
```

## Delete Assets and Albums

사용자는 AssetViewController의 오른쪽 아래에 있는 휴지통 버튼을 통해 에셋을 삭제할 수 있다. 앨범에서 제거하기 위해 PhotoBrowse가 PHAssetCollectionChangeRequest 객체 내에서 삭제 작업을 래핑한다. 전체 사진 라이브러리에서 제거하기 위해 PhotoBrowse는 삭제 작업을 PHAssetChangeRequest 객체 내에 래핑한다.

```swift
if assetCollection != nil {    // Remove the asset from the selected album.    PHPhotoLibrary.shared().performChanges({        let request = PHAssetCollectionChangeRequest(for: self.assetCollection)!        request.removeAssets([self.asset] as NSArray)    }, completionHandler: completion)} else {    // Delete the asset from the photo library.    PHPhotoLibrary.shared().performChanges({        PHAssetChangeRequest.deleteAssets([self.asset] as NSArray)    }, completionHandler: completion)}
```

## Favorite an Asset

사용자는 PHAsset 매개 변수 `isFavorite` 를 토글하여 에셋 즐겨찾기를 할 수 있다.

```swift
PHPhotoLibrary.shared().performChanges({    let request = PHAssetChangeRequest(for: self.asset)    request.isFavorite = !self.asset.isFavorite}, completionHandler: { success, error in    if success {        DispatchQueue.main.sync {            sender.title = self.asset.isFavorite ? "♥︎" : "♡"        }    } else {        print("Can't mark the asset as a Favorite: \(String(describing: error))")    }})
```

## Observe and Respond to Changes

메인 뷰 컨트롤러를 등록하여 사진 라이브러리의 변경 사항을 관찰하여 에셋 변경시 앱이 알림을 수신하고 응답할 수 있도록 하라. 이러한 변경은 반드시 앱의 기능 내에서 발생하는 것이 아니라 다른 앱, 다른 기기, iCloud Photos 또는 Shared Album에서 발생할 수 있다.

```swift
if let changeDetails = changeInstance.changeDetails(for: allPhotos) {    // Update the cached fetch result.    allPhotos = changeDetails.fetchResultAfterChanges    // Don't update the table row that always reads "All Photos."}// Update the cached fetch results, and reload the table sections to match.if let changeDetails = changeInstance.changeDetails(for: smartAlbums) {    smartAlbums = changeDetails.fetchResultAfterChanges    tableView.reloadSections(IndexSet(integer: Section.smartAlbums.rawValue), with: .automatic)}if let changeDetails = changeInstance.changeDetails(for: userCollections) {    userCollections = changeDetails.fetchResultAfterChanges    tableView.reloadSections(IndexSet(integer: Section.userCollections.rawValue), with: .automatic)}
```

앱의 메인 뷰 컨트롤러가 사라진 후에는 옵저버의 등록을 취소하라.

```swift
PHPhotoLibrary.shared().unregisterChangeObserver(self)
```

## 출처

### Apple documentation

[**Browsing and Modifying Photo Albums**](https://developer.apple.com/documentation/photokit/browsing_and_modifying_photo_albums)

