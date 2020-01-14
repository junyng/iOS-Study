# Export

시청각 에셋을 읽고 쓰기 위해서는 AVFoundation 프레임워크에서 제공하는 내보내기 API를 사용해야 한다. [`AVAssetExportSession`](https://developer.apple.com/documentation/avfoundation/avassetexportsession) 클래스는 파일 형식을 수정하거나 에셋의 길이를 자르는 등의 간단한 내보내기 요구에 대한 인터페이스를 제공한다 \([Trimming and Transcoding a Movie](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/01_UsingAssets.html#//apple_ref/doc/uid/TP40010188-CH7-SW8) 참조\). 자세한 내보내기 요구 사항은 [`AVAssetReader`](https://developer.apple.com/documentation/avfoundation/avassetreader) 및 [`AVAssetWriter`](https://developer.apple.com/documentation/avfoundation/avassetwriter) 클래스를 사용하라.

에셋의 내용에 대한 작업을 수행하려면 `AVAssetReader`를 사용하라. 예를 들어, 에셋의 오디오 트랙을 읽어 파형을 시각적으로 표시할 수 있다. 샘플 버퍼나 스틸 이미지와 같은 미디어에서 에셋을 생성하려면 `AVAssetWriter` 객체를 사용하라.

> 참고: asset reader 및 writer 클래스는 실시간 처리에 사용되도록 의도되지 않는다. 실제로 asset reader는 HTTP 라이브 스트림과 같은 실시간 소스에서 읽기 위해 사용할 수도 없다. 그러나 [`AVCaptureOutput`](https://developer.apple.com/documentation/avfoundation/avcaptureoutput) 객체와 같은 실시간 데이터 소스를 가진 asset writer를 사용하고 있는 경우 asset writer의 입력에 대한 [`expectsMediaDataInRealTime`](https://developer.apple.com/documentation/avfoundation/avassetwriterinput/1387827-expectsmediadatainrealtime) 속성을 `YES`로 설정한다. 이 속성을 실시간이 아닌 데이터 소스에 대해 `YES`로 설정하면 파일이 제대로 삽입되지 않는다.

### Reading an Asset

각 AVAssetReader 객체는 한 번에 하나의 에셋에 연결할 수 있다. 하지만 이 에셋에는 여러 개의 트랙이 포함될 수 있다. 따라서 읽기 전에 [`AVAssetReaderOutput`](https://developer.apple.com/documentation/avfoundation/avassetreaderoutput) 클래스의 구체적인 하위 클래스를 asset reader에 할당해야 미디어 데이터를 읽는 방법을 구성할 수 있다. 에셋 읽기 요구에 사용할 수 있는 `AVAssetReaderOutput` 기본 클래스의 세 가지 구체적인 하위 클래스가 있다. \([`AVAssetReaderTrackOutput`](https://developer.apple.com/documentation/avfoundation/avassetreadertrackoutput), [`AVAssetReaderAudioMixOutput`](https://developer.apple.com/documentation/avfoundation/avassetreaderaudiomixoutput) 및[`AVAssetReaderVideoCompositionOutput`](https://developer.apple.com/documentation/avfoundation/avassetreadervideocompositionoutput)\)

#### Creating the Asset Reader

`AVAssetReader` 객체를 초기화하기 위해 필요한 모든 것은 읽기를 원하는 에셋이다.

```objectivec
NSError *outError;
AVAsset *someAsset = <#AVAsset that you want to read#>;
AVAssetReader *assetReader = [AVAssetReader assetReaderWithAsset:someAsset error:&outError];
BOOL success = (assetReader != nil);
```

> **참고**: asset reader가 성공적으로 초기화되었는지 확인하기 위해 반환된 asset reader가 `nil`이 아닌지 항상 확인하라. 그렇지 않으면 error 매개변수\(이전 예제에서는 `outError`\)가 관련 오류 정보를 포함할 것이다.

#### Setting Up the Asset Reader Outputs

asset reader를 만든 후, 읽는 중인 미디어 데이터를 수신하기 위해 적어도 하나의 출력을 설정한다. 출력을 설정할 때 [`alwaysCopiesSampleData`](https://developer.apple.com/documentation/avfoundation/avassetreaderoutput/1389189-alwayscopiessampledata) 속성을 `NO`로 설정하라. 이런 식으로, 성능 향상의 이점을 얻을 수 있다. 이 장의 모든 사례에서 이 속성은 `NO`로 설정될 수 있고, 또한 `NO`로 설정되어야 한다.

하나 이상의 트랙에서 미디어 데이터만 읽고 해당 데이터를 다른 형식으로 변환하려면 에셋에서 읽으려는 각 [`AVAssetTrack`](https://developer.apple.com/documentation/avfoundation/avassettrack) 객체에 대해 단일 출력 객체를 사용하여 `AVAssetReaderTrackOutput` 클래스를 사용하라. 오디오 트랙을 asset reader로 선형 PCM으로 압축 해제하려면 트랙 출력을 다음과 같이 설정하라:

```objectivec
AVAsset *localAsset = assetReader.asset;
// Get the audio track to read.
AVAssetTrack *audioTrack = [[localAsset tracksWithMediaType:AVMediaTypeAudio] objectAtIndex:0];
// Decompression settings for Linear PCM
NSDictionary *decompressionAudioSettings = @{ AVFormatIDKey : [NSNumber numberWithUnsignedInt:kAudioFormatLinearPCM] };
// Create the output with the audio track and decompression settings.
AVAssetReaderOutput *trackOutput = [AVAssetReaderTrackOutput assetReaderTrackOutputWithTrack:audioTrack outputSettings:decompressionAudioSettings];
// Add the output to the reader if possible.
if ([assetReader canAddOutput:trackOutput])
    [assetReader addOutput:trackOutput];
```

> 참고: 특정 에셋 트랙에서 저장된 형식으로 미디어 데이터를 읽으려면 `outputSettings` 매개 변수에 `nil` 을 전달하라.

`AVAssetReaderAudioMixOutput` 과 `AVAssetReaderVideoCompositionOutput` 클래스를 사용하여 각각 [`AVAudioMix`](https://developer.apple.com/documentation/avfoundation/avaudiomix) 객체 또는 [`AVVideoComposition`](https://developer.apple.com/documentation/avfoundation/avvideocomposition) 객체를 사용하여 혼합되거나 합성된 미디어 데이터를 읽는다. 일반적으로 이러한 출력은 asset reader가 [`AVComposition`](https://developer.apple.com/documentation/avfoundation/avcomposition) 객체에서 읽을 때 사용된다.

단일 오디오 믹스 출력을 사용하면 `AVAudioMix` 객체를 사용하여 함께 혼합된 에셋의 여러 비디오 트랙을 읽을 수 있다. 오디오 트랙을 혼합하는 방법을 지정하려면 초기화 후 혼합을 `AVAssetReaderAudioMixOutput` 객체에 할당하라. 다음 코드는 에셋에서 모든 오디오 트랙으로 오디오 믹스 출력을 만들고 오디오 트랙을 선형 PCM으로 압축 해제하고 오디오 믹스 객체를 출력에 할당하는 방법을 표시한다. 오디오 믹스를 구성하는 방법에 대한 자세한 내용은 [Editing](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/03_Editing.html#//apple_ref/doc/uid/TP40010188-CH8-SW1)를 참조하라.

```objectivec
AVAudioMix *audioMix = <#An AVAudioMix that specifies how the audio tracks from the AVAsset are mixed#>;
// Assumes that assetReader was initialized with an AVComposition object.
AVComposition *composition = (AVComposition *)assetReader.asset;
// Get the audio tracks to read.
NSArray *audioTracks = [composition tracksWithMediaType:AVMediaTypeAudio];
// Get the decompression settings for Linear PCM.
NSDictionary *decompressionAudioSettings = @{ AVFormatIDKey : [NSNumber numberWithUnsignedInt:kAudioFormatLinearPCM] };
// Create the audio mix output with the audio tracks and decompression setttings.
AVAssetReaderOutput *audioMixOutput = [AVAssetReaderAudioMixOutput assetReaderAudioMixOutputWithAudioTracks:audioTracks audioSettings:decompressionAudioSettings];
// Associate the audio mix used to mix the audio tracks being read with the output.
audioMixOutput.audioMix = audioMix;
// Add the output to the reader if possible.
if ([assetReader canAddOutput:audioMixOutput])
    [assetReader addOutput:audioMixOutput];
```

> 참고: `audioSettings` 매개변수에 대해 `nil`을 통과하면 asset reader가 편리하게 압축되지 않은 형식으로 샘플을 반환할 수 있다. `AVAssetReaderVideoCompositionOutput` 클래스에서도 마찬가지다.

비디오 합성 출력은 거의 같은 방식으로 동작한다: 당신은 `AVVideoComposition` 객체를 사용하여 함께 합성된 에셋의 여러 비디오 트랙을 읽을 수 있다. 여러 합성된 비디오 트랙에서 미디어 데이터를 읽고 ARGB로 압축을 풀려면 다음과 같이 출력을 설정하라.

```text
AVVideoComposition *videoComposition = <#An AVVideoComposition that specifies how the video tracks from the AVAsset are composited#>;
// Assumes assetReader was initialized with an AVComposition.
AVComposition *composition = (AVComposition *)assetReader.asset;
// Get the video tracks to read.
NSArray *videoTracks = [composition tracksWithMediaType:AVMediaTypeVideo];
// Decompression settings for ARGB.
NSDictionary *decompressionVideoSettings = @{ (id)kCVPixelBufferPixelFormatTypeKey : [NSNumber numberWithUnsignedInt:kCVPixelFormatType_32ARGB], (id)kCVPixelBufferIOSurfacePropertiesKey : [NSDictionary dictionary] };
// Create the video composition output with the video tracks and decompression setttings.
AVAssetReaderOutput *videoCompositionOutput = [AVAssetReaderVideoCompositionOutput assetReaderVideoCompositionOutputWithVideoTracks:videoTracks videoSettings:decompressionVideoSettings];
// Associate the video composition used to composite the video tracks being read with the output.
videoCompositionOutput.videoComposition = videoComposition;
// Add the output to the reader if possible.
if ([assetReader canAddOutput:videoCompositionOutput])
    [assetReader addOutput:videoCompositionOutput];
```

#### Reading the Asset’s Media Data

필요한 출력을 모두 설정한 후 읽기를 시작하려면 asset reader에서 [`startReading`](https://developer.apple.com/documentation/avfoundation/avassetreader/1390286-startreading) 메서드를 호출하라. 그런 다음 [`copyNextSampleBuffer`](https://developer.apple.com/documentation/avfoundation/avassetreaderoutput/1385732-copynextsamplebuffer) 메서드 사용하여 각 출력에서 미디어 데이터를 개별적으로 검색하라. asset reader를 단일 출력으로 시작하고 해당 미디어 샘플을 모두 읽으려면 다음을 수행하라:

```objectivec
// Start the asset reader up.
[self.assetReader startReading];
BOOL done = NO;
while (!done)
{
  // Copy the next sample buffer from the reader output.
  CMSampleBufferRef sampleBuffer = [self.assetReaderOutput copyNextSampleBuffer];
  if (sampleBuffer)
  {
    // Do something with sampleBuffer here.
    CFRelease(sampleBuffer);
    sampleBuffer = NULL;
  }
  else
  {
    // Find out why the asset reader output couldn't copy another sample buffer.
    if (self.assetReader.status == AVAssetReaderStatusFailed)
    {
      NSError *failureError = self.assetReader.error;
      // Handle the error here.
    }
    else
    {
      // The asset reader output has read all of its samples.
      done = YES;
    }
  }
}
```

### Writing an Asset

여러 소스에서 지정된 파일 형식의 단일 파일로 미디어 데이터를 쓰기 위한 [`AVAssetWriter`](https://developer.apple.com/documentation/avfoundation/avassetwriter) 클래스이다. asset writer 객체를 특정 에셋과 연결할 필요는 없지만 생성할 각 출력 파일에 대해 별도의 asset writer를 사용해야 한다. asset writer는 여러 소스에서 미디어 데이터를 쓸 수 있으므로 출력 파일에 쓸 각 트랙에 대해 [`AVAssetWriterInput`](https://developer.apple.com/documentation/avfoundation/avassetwriterinput) 객체를 작성해야 한다. 각 `AVAssetWriterInput` 객체는 [`CMSampleBufferRef`](https://developer.apple.com/documentation/coremedia/cmsamplebuffer) 객체의 형태로 데이터를 수신할 것으로 예상하지만, 그러나 asset writer 입력에 [`CVPixelBufferRef`](https://developer.apple.com/documentation/corevideo/cvpixelbuffer) 객체를 추가하려면 [`AVAssetWriterInputPixelBufferAdaptor`](https://developer.apple.com/documentation/avfoundation/avassetwriterinputpixelbufferadaptor) 클래스를 사용하라.

#### Creating the Asset Writer

asset writer를 작성하려면 출력 파일과 원하는 파일 유형에 대한 URL을 지정한다. 다음 코드는 asset writer를 초기화하여 QuickTime 영화를 만드는 방법을 표시한다.

```objectivec
NSError *outError;
NSURL *outputURL = <#NSURL object representing the URL where you want to save the video#>;
AVAssetWriter *assetWriter = [AVAssetWriter assetWriterWithURL:outputURL
                                                      fileType:AVFileTypeQuickTimeMovie
                                                         error:&outError];
BOOL success = (assetWriter != nil);
```

#### Setting Up the Asset Writer Inputs

asset writer가 미디어 데이터를 쓸 수 있으려면 asset writer 입력을 하나 이상 설정해야 한다. 예를 들어, 미디어 데이터 소스가 이미 미디어 샘플을 `CMSampleBufferRef` 객체로 자동 판매하는 경우 `AVAssetWriterInput` 클래스를 사용하라. 오디오 미디어 데이터를 128kbps AAC로 압축하고 asset writer에 연결하는 asset writer 입력을 설정하려면 다음을 수행하라:

```objectivec
// Configure the channel layout as stereo.
AudioChannelLayout stereoChannelLayout = {
    .mChannelLayoutTag = kAudioChannelLayoutTag_Stereo,
    .mChannelBitmap = 0,
    .mNumberChannelDescriptions = 0
};
 
// Convert the channel layout object to an NSData object.
NSData *channelLayoutAsData = [NSData dataWithBytes:&stereoChannelLayout length:offsetof(AudioChannelLayout, mChannelDescriptions)];
 
// Get the compression settings for 128 kbps AAC.
NSDictionary *compressionAudioSettings = @{
    AVFormatIDKey         : [NSNumber numberWithUnsignedInt:kAudioFormatMPEG4AAC],
    AVEncoderBitRateKey   : [NSNumber numberWithInteger:128000],
    AVSampleRateKey       : [NSNumber numberWithInteger:44100],
    AVChannelLayoutKey    : channelLayoutAsData,
    AVNumberOfChannelsKey : [NSNumber numberWithUnsignedInteger:2]
};
 
// Create the asset writer input with the compression settings and specify the media type as audio.
AVAssetWriterInput *assetWriterInput = [AVAssetWriterInput assetWriterInputWithMediaType:AVMediaTypeAudio outputSettings:compressionAudioSettings];
// Add the input to the writer if possible.
if ([assetWriter canAddInput:assetWriterInput])
    [assetWriter addInput:assetWriterInput];
```

> Note: 미디어 데이터를 저장된 형식으로 쓰려면 `outputSettings` 매개 변수에서 `nil` 을 전달하라. asset writer가 [`AVFileTypeQuickTimeMovie`](https://developer.apple.com/documentation/avfoundation/avfiletype/1386770-mov) 의 `fileType`으로 초기화되었을 때 통과하라.

asset writer 입력은 선택적으로 일부 메타데이터를 포함하거나 각각 [`metadata`](https://developer.apple.com/documentation/avfoundation/avassetwriterinput/1386328-metadata)와 [`transform`](https://developer.apple.com/documentation/avfoundation/avassetwriterinput/1390183-transform) 속성을 사용하여 특정 트랙에 대해 다른 변환을 지정할 수 있다. 데이터 소스가 비디오 트랙인 asset writer 입력의 경우 다음을 수행하여 출력 파일에서 비디오의 원래 변환을 유지하라:

```objectivec
AVAsset *videoAsset = <#AVAsset with at least one video track#>;
AVAssetTrack *videoAssetTrack = [[videoAsset tracksWithMediaType:AVMediaTypeVideo] objectAtIndex:0];
assetWriterInput.transform = videoAssetTrack.preferredTransform;
```

> 참고: asset writer와 함께 쓰기 시작하기 전에 `metadata` 및 `transform` 속성을 설정하여 적용하라.

미디어 데이터를 출력 파일에 쓸 때 픽셀 버퍼를 할당할 수도 있다. 그러려면 `AVAssetWriterInputPixelBufferAdaptor` 클래스를 사용하라. 효율성을 극대화하려면 별도의 풀을 사용하여 할당된 픽셀 버퍼를 추가하는 대신 픽셀 버퍼 어댑터에서 제공하는 픽셀 버퍼 풀을 사용하라. 다음 코드는 RGB 도메인에서 작동하는 픽셀 버퍼 객체를 생성하여 [`CGImage`](https://developer.apple.com/documentation/uikit/uiimage/1624159-cgimage) 객체를 사용하여 픽셀 버퍼를 생성한다.

```text
NSDictionary *pixelBufferAttributes = @{
     kCVPixelBufferCGImageCompatibilityKey : [NSNumber numberWithBool:YES],
     kCVPixelBufferCGBitmapContextCompatibilityKey : [NSNumber numberWithBool:YES],
     kCVPixelBufferPixelFormatTypeKey : [NSNumber numberWithInt:kCVPixelFormatType_32ARGB]
};
AVAssetWriterInputPixelBufferAdaptor *inputPixelBufferAdaptor = [AVAssetWriterInputPixelBufferAdaptor assetWriterInputPixelBufferAdaptorWithAssetWriterInput:self.assetWriterInput sourcePixelBufferAttributes:pixelBufferAttributes];
```

> 참고: 모든 `AVAssetWriterInputPixelBufferAdaptor` 개체는 단일 자산 작성자 입력에 연결해야 한다. 이 asset writer 입력은 [`AVMediaTypeVideo`](https://developer.apple.com/documentation/avfoundation/avmediatypevideo) 유형의 미디어 데이터를 수용해야 한다.

#### Writing Media Data

asset writer에 필요한 모든 입력을 구성하면 미디어 데이터 작성을 시작할 준비가 된다. asset reader와 마찬가지로 쓰기 프로세스를 [`startWriting`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1386724-startwriting) 메서드를 호출하여 시작하라. 그런 다음 [`startSessionAtSourceTime:`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1389908-startsessionatsourcetime) 메서드를 호출하여 샘플 작성 세션을 시작하라. asset writer가 수행한 모든 쓰기는 이러한 세션 중 하나 내에서 발생해야하며 각 세션의 시간 범위는 소스 내에서 포함된 미디어 데이터의 시간 범위를 정의한다. 예를 들어, 소스가 [`AVAsset`](https://developer.apple.com/documentation/avfoundation/avasset) 객체에서 읽은 미디어 데이터를 제공하는 asset reader이고 에셋의 전반부에서 미디어 데이터를 포함하지 않으려면 다음을 수행하라:

```objectivec
CMTime halfAssetDuration = CMTimeMultiplyByFloat64(self.asset.duration, 0.5);
[self.assetWriter startSessionAtSourceTime:halfAssetDuration];
//Implementation continues.
```

일반적으로 쓰기 세션을 끝내려면 [`endSessionAtSourceTime:`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1389921-endsession)메서드를 호출해야 한다. 그러나 쓰기 세션이 파일 끝까지 바로 올라가면 [`finishWriting`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1426644-finishwriting) 쓰기 메서드를 호출하여 쓰기 세션을 종료할 수 있다. 단일 입력으로 asset writer를 시작하고 해당 미디어 데이터를 모두 쓰려면 다음을 수행하라:

```objectivec
// Prepare the asset writer for writing.
[self.assetWriter startWriting];
// Start a sample-writing session.
[self.assetWriter startSessionAtSourceTime:kCMTimeZero];
// Specify the block to execute when the asset writer is ready for media data and the queue to call it on.
[self.assetWriterInput requestMediaDataWhenReadyOnQueue:myInputSerialQueue usingBlock:^{
     while ([self.assetWriterInput isReadyForMoreMediaData])
     {
          // Get the next sample buffer.
          CMSampleBufferRef nextSampleBuffer = [self copyNextSampleBufferToWrite];
          if (nextSampleBuffer)
          {
               // If it exists, append the next sample buffer to the output file.
               [self.assetWriterInput appendSampleBuffer:nextSampleBuffer];
               CFRelease(nextSampleBuffer);
               nextSampleBuffer = nil;
          }
          else
          {
               // Assume that lack of a next sample buffer means the sample buffer source is out of samples and mark the input as finished.
               [self.assetWriterInput markAsFinished];
               break;
          }
     }
}];
```

위의 코드에서 복사한 `copyNextSampleBufferToWrite` 메서드는 단순한 스텁이다. 이 스텁의 위치는 쓰려는 미디어 데이터를 나타내는 `CMSampleBufferRef` 객체를 반환하기 위해 어떤 로직을 삽입해야 하는 곳이다. 샘플 버퍼의 한 가지 가능한 소스는 asset writer 출력이다.

### Reencoding Assets

asset reader와 asset writer 객체를 사용하여 한 표현에서 다른 표현으로 에셋을 변환할 수 있다. 이러한 객체를 사용하면 `AVAssetExportSession` 객체보다 변환을 더 잘 제어할 수 있다. 예를 들어 출력 파일에 표시할 트랙을 선택하거나, 자신의 출력 형식을 지정하거나, 변환 프로세스 중에 에셋을 수정할 수 있다. 이 프로세스의 첫 번째 단계는 asset reader 출력과 asset writer 입력을 원하는 대로 설정하는 것이다. asset reader와 writer가 완전히 구성된 후에는 각각 `startReading` 및 `startWriting` 메서드에 대한 호출을 사용하여 두 항목을 모두 시작하라. 다음 코드 조각은 asset writer 출력이 제공하는 미디어 데이터를 쓰기 위해 단일 asset reader 입력을 사용하는 방법을 보여준다:

```objectivec
NSString *serializationQueueDescription = [NSString stringWithFormat:@"%@ serialization queue", self];
 
// Create a serialization queue for reading and writing.
dispatch_queue_t serializationQueue = dispatch_queue_create([serializationQueueDescription UTF8String], NULL);
 
// Specify the block to execute when the asset writer is ready for media data and the queue to call it on.
[self.assetWriterInput requestMediaDataWhenReadyOnQueue:serializationQueue usingBlock:^{
     while ([self.assetWriterInput isReadyForMoreMediaData])
     {
          // Get the asset reader output's next sample buffer.
          CMSampleBufferRef sampleBuffer = [self.assetReaderOutput copyNextSampleBuffer];
          if (sampleBuffer != NULL)
          {
               // If it exists, append this sample buffer to the output file.
               BOOL success = [self.assetWriterInput appendSampleBuffer:sampleBuffer];
               CFRelease(sampleBuffer);
               sampleBuffer = NULL;
               // Check for errors that may have occurred when appending the new sample buffer.
               if (!success && self.assetWriter.status == AVAssetWriterStatusFailed)
               {
                    NSError *failureError = self.assetWriter.error;
                    //Handle the error.
               }
          }
          else
          {
               // If the next sample buffer doesn't exist, find out why the asset reader output couldn't vend another one.
               if (self.assetReader.status == AVAssetReaderStatusFailed)
               {
                    NSError *failureError = self.assetReader.error;
                    //Handle the error here.
               }
               else
               {
                    // The asset reader output must have vended all of its samples. Mark the input as finished.
                    [self.assetWriterInput markAsFinished];
                    break;
               }
          }
     }
}];
```

### Putting It All Together: Using an Asset Reader and Writer in Tandem to Reencode an Asset

이 간단한 코드 예는 asset reader와 writer를 사용하여 에셋의 첫번째 비디오 및 오디오 트랙을 새 파일로 다시 인코딩하는 방법을 보여준다. 이 문서에서는 다음을 수행하는 방법을 보여준다:

* 시리얼라이제이션 큐를 사용하여 시청각 데이터를 읽고 쓰는 비동기 특성을 처리한다.
* asset reader를 초기화하고 두 개의 asset reader 출력\(오디오용 및 비디오용\)을 구성한다.
* asset reader를 초기화하고 두 개의 asset reader 입력\(오디오용 및 비디오용\)을 구성한다.
* asset reader를 사용하여 두 가지 서로 다른 출력/입력 조합을 통해 asset writer에게 미디어 데이터를 비동기적으로 제공한다.
* 디스패치 그룹을 사용하여 재인코딩 프로세스 완료를 알린다.
*  사용자가 재인코딩 프로세스가 시작되면 취소할 수 있도록 허용한다.

참고: 가장 관련성이 높은 코드에 초점을 맞추기 위해, 이 예에서는 전체 애플리케이션의 몇 가지 측면을 생략한다. AVFoundation을 이용하기 위해서, 당신은 Cocoa에서 잃어버린 조각들을 유추할 수 있을 만큼 충분할 경험을 할 것으로 기대된다.

#### Handling the Initial Setup

asset reader와 writer를 생성하고 해당 출력과 입력을 구성하기 전에 먼저 몇 가지 초기 설정을 처리해야 한다. 이 설정의 첫 번째 부분은 읽기 및 쓰기 프로세스를 조정하기 위해 세 개의 개별 직렬화 대기열을 만드는 것을 포함한다.

```objectivec
NSString *serializationQueueDescription = [NSString stringWithFormat:@"%@ serialization queue", self];
 
// Create the main serialization queue.
self.mainSerializationQueue = dispatch_queue_create([serializationQueueDescription UTF8String], NULL);
NSString *rwAudioSerializationQueueDescription = [NSString stringWithFormat:@"%@ rw audio serialization queue", self];
 
// Create the serialization queue to use for reading and writing the audio data.
self.rwAudioSerializationQueue = dispatch_queue_create([rwAudioSerializationQueueDescription UTF8String], NULL);
NSString *rwVideoSerializationQueueDescription = [NSString stringWithFormat:@"%@ rw video serialization queue", self];
 
// Create the serialization queue to use for reading and writing the video data.
self.rwVideoSerializationQueue = dispatch_queue_create([rwVideoSerializationQueueDescription UTF8String], NULL);
```

메인 직렬화 큐는 asset reader와 writer의 시작과 중지를 조정하는 데 사용되며\(취소 때문일 수도 있다\) 나머지 두 개의 직렬화 큐는 잠재적인 취소와 함께 각 출력/입력 조합에 의한 읽기 및 쓰기를 직렬화하기 위해 사용된다.

이제 일부 직렬화 큐가 있으므로 에셋의 트랙을 로드하고 재코딩 프로세스를 시작한다.

```objectivec
self.asset = <#AVAsset that you want to reencode#>;
self.cancelled = NO;
self.outputURL = <#NSURL representing desired output URL for file generated by asset writer#>;
// Asynchronously load the tracks of the asset you want to read.
[self.asset loadValuesAsynchronouslyForKeys:@[@"tracks"] completionHandler:^{
     // Once the tracks have finished loading, dispatch the work to the main serialization queue.
     dispatch_async(self.mainSerializationQueue, ^{
          // Due to asynchronous nature, check to see if user has already cancelled.
          if (self.cancelled)
               return;
          BOOL success = YES;
          NSError *localError = nil;
          // Check for success of loading the assets tracks.
          success = ([self.asset statusOfValueForKey:@"tracks" error:&localError] == AVKeyValueStatusLoaded);
          if (success)
          {
               // If the tracks loaded successfully, make sure that no file exists at the output path for the asset writer.
               NSFileManager *fm = [NSFileManager defaultManager];
               NSString *localOutputPath = [self.outputURL path];
               if ([fm fileExistsAtPath:localOutputPath])
                    success = [fm removeItemAtPath:localOutputPath error:&localError];
          }
          if (success)
               success = [self setupAssetReaderAndAssetWriter:&localError];
          if (success)
               success = [self startAssetReaderAndWriter:&localError];
          if (!success)
               [self readingAndWritingDidFinishSuccessfully:success withError:localError];
     });
}];
```

