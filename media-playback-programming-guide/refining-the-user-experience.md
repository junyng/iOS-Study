# Refining The User Experience

이 가이드에서 AVKit 및 AVFoundation을 사용하여 강력한 재생 앱을 구축하는 방법을 살펴보았다. 이 장에서는 앱의 재생 환경을 더욱 커스터마이징하고 세분화하는 데 사용할 수 있는 AVFoundation의 몇 가지 추가 기능을 설명한다.

### Presenting Chapter Markers 

챕터 마커를 통해 사용자가 컨텐츠를 빠르게 탐색할 수 있다. tvOS 및 macOS의  `AVPlayerViewController`는 현재 재생된 에셋에서 마커가 발견되면 자동으로 챕터 선택 인터페이스를 제공한다. 또한 사용자 정의 선택 인터페이스를 생성하려는 경우 언제든지 이 데이터를 직접 되찾을 수 있다.

챕터 마커는 _시간적 메타데이터_의 한 유형이다. 이는 본 가이드의 다른 절에서 논의된 것과 동일한 종류의 메타데이터이지만, 전체 에셋을 적용하는 대신 에셋의 일정 내에 있는 시간 범위에만 적용된다. [chapterMetadataGroupsBestMatchingPreferredLanguages:](https://developer.apple.com/documentation/avfoundation/avasset/1390909-chaptermetadatagroupsbestmatchin) 또는 [chapterMetadataGroupsWithTitleLocale:containingItemsWithCommonKeys:](https://developer.apple.com/documentation/avfoundation/avasset/1388966-chaptermetadatagroupswithtitlelo) 메서드를 사용하여 에셋의 챕터 메타데이터를 되찾을 수 있다. 에셋의 [availableChapterLocales](https://developer.apple.com/documentation/avfoundation/avasset/1388228-availablechapterlocales) 키의 값을 비동기식으로 로드한 후 이러한 메서드를 차단하지 않고 호출할 수 있다.

```swift
let asset = AVAsset(url: <# Asset URL #>)
let chapterLocalesKey = "availableChapterLocales"
 
asset.loadValuesAsynchronously(forKeys: [chapterLocalesKey]) {
    var error: NSError?
    let status = asset.statusOfValue(forKey: chapterLocalesKey, error: &error)
    if status == .loaded {
        let languages = Locale.preferredLanguages
        let chapterMetadata = asset.chapterMetadataGroups(bestMatchingPreferredLanguages: languages)
        // Process chapter metadata
    }
    else {
        // Handle other status cases
    }
}
```

이러한 메서드에서 반환되는 값은 각각 개별 챕터 마커를 나타내는 [AVTimedMetadataGroup](https://developer.apple.com/documentation/avfoundation/avtimedmetadatagroup) 객체의 배열이다. `AVTimedMetadataGroup` 객체는 `CMTimeRange`로 구성되며 메타데이터가 적용되는 시간 범위, `AVMetadataItem` 배열 객체의 챕터 제목을 나타내는 항목 객체, 선택적으로 해당 미리 보기 이미지를 정의한다. 다음 예제에서는 `AVTimedMetadataGroup` 데이터를 앱 뷰 레이어에 표시할 사용자 지정 모델 객체 배열로 변환하는 방법을 보여주며, 이는 `Chapter` 라고 한다.

```swift
func convertTimedMetadataGroupsToChapters(groups: [AVTimedMetadataGroup]) -> [Chapter] {
    return groups.map { group -> Chapter in
 
        // Retrieve AVMetadataCommonIdentifierTitle metadata items
        let titleItems =
            AVMetadataItem.metadataItems(from: group.items,
                                         filteredByIdentifier: AVMetadataCommonIdentifierTitle)
 
        // Retrieve AVMetadataCommonIdentifierTitle metadata items
        let artworkItems =
            AVMetadataItem.metadataItems(from: group.items,
                                         filteredByIdentifier: AVMetadataCommonIdentifierArtwork)
 
        var title = "Default Title"
        var image = UIImage(named: "placeholder")!
 
        if let titleValue = titleItems.first?.stringValue {
            title = titleValue
        }
 
        if let imgData = artworkItems.first?.dataValue, imageValue = UIImage(data: imgData) {
            image = imageValue
        }
 
        return Chapter(time: group.timeRange.start, title: title, image: image)
    }
}
```

