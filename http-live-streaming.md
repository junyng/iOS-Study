---
description: 'iOS, tvOS 및 macOS 장치로 오디오 및 비디오를 전송하라.'
---

# HTTP Live Streaming

### Overview

HLS\(HTTP Live Streaming\)은 일반 웹 서버에서 HTTP로 오디오와 비디오를 전송하여 iPhone, iPad, iPod Touch, Apple TV 등 iOS 기반 장치와 데스크탑 컴퓨터\(macOS\)에서 재생한다. HLS는 웹에 전원을 공급하는 동일한 프로토콜을 사용하여 일반 웹 서버의 콘텐츠 전달 네트워크를 사용하여 콘텐츠를 배포한다. HLS는 유무선 연결의 사용 가능한 속도에 맞게 재생을 최적화하여 네트워크 상태에 동적으로 적응하도록 설계되었다.

HLS는 다음을 지원한다:

* 라이브 방송과 녹음 콘텐츠\(주문형 비디오, 또는 VOD\)
* 다른 비트 전송률의 여러 대체 스트림
* 네트워크 대역폭 변화에 따른 스트림의 지능적 전환
* 미디어 암호화 및 사용자 인증

다음 그림은 HTTP Live Stream의 구성 요소를 보여준다.

![](.gitbook/assets/http-live-streaming-overview.png)

애플은 [AVKit](https://developer.apple.com/documentation/avkit), [AVFoundation](https://developer.apple.com/documentation/avfoundation), [WebKit](https://developer.apple.com/documentation/webkit) 등 HTTP Live Streaming을 지원하는 몇가지 프레임워크를 제공한다.

