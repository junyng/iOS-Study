# HTTP Live Streaming Overview

## Introduction <a id="pageTitle"></a>

다음 중 하나에 관심이 있는 경우:

* iPhone, iPod Touch, iPad 또는 Apple TV로 오디오 또는 비디오 스트리밍
* 특별한 서버 소프트웨어 없이 라이브 이벤트 스트리밍
* 암호화 및 인증을 통해 온디맨드 비디오 전송

당신은 HTTP Live Streaming 에 대해 배워야 한다.

HTTP Live Streaming 을 통해 iPhone, iPad, iPod touch, Apple TV, 데스크탑 컴퓨터 \(Mac OS X\)를 포함한 iOS 기반 장치에서 재생할 수 있도록 일반 웹 서버에서 HTTP를 통해 오디오 및 비디오 전송이 가능하다. HTTP Live Streaming 은 라이브 방송과 사전 녹화된 콘텐츠를 모두 지원한다. \(VOD\) HTTP Live Streaming 은 다양한 비트 전송률로 여러 개의 대체 스트림을 지원하며, 클라이언트 소프트웨어는 네트워크 대역폭의 변화에 따라 스트림을 지능적으로 전환할 수 있다. HTTP Live Streaming은 또한 HTTPS를 통한 미디어 암호화 및 사용자 인증을 제공하여 출판사가 자신의 작업을 보호할 수 있도록 한다.

![](../.gitbook/assets/transport_stream_2x.png)

iOS 3.0 이상을 실행하는 모든 장치에는 HTTP Live Streaming을 위한 내장 클라이언트 소프트웨어를 포함한다. 사파리 브라우저는 아이패드와 데스크탑 컴퓨터에서 웹페이지 내 HTTP 스트림을 재생할 수 있으며, 사파리는 아이폰과 아이팟 터치 등 작은 화면이 있는 iOS 기기에서 HTTP 스트림을 위한 전체 화면 미디어 플레이어를 시작한다. 애플 TV 2 이상에는 HTTP Live Streaming 클라이언트가 포함되어 있다.

> **중요**: HTTP Live Streaming을 사용하려면 휴대폰 네트워크를 통해 대량의 오디오 또는 비디오 데이터를 전송하는 iPhone 및 iPad 앱이 필요하다. [Requirements for Apps](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/StreamingMediaGuide/UsingHTTPLiveStreaming/UsingHTTPLiveStreaming.html#//apple_ref/doc/uid/TP40008332-CH102-SW5) 를 참조하라.

Safari는 기본적으로 `<video>` 태그의 소스로 HTTP Live 스트림을 재생한다. Mac OS X 개발자는 QTKit 및 AVFoundation 프레임워크를 사용하여 HTTP Live Streams 를 재생하는 데스크탑 애플리케이션을 만들 수 있으며, iOS 개발자는 MediaPlayer 및 AVFoundation 프레임워크를 사용하여 iOS 앱을 만들 수 있다.

> **중요**: 가능하면 `<video>` 태그를 사용하여 HTTP Live Streaming을 내장하고, `<object>` 또는 `<embed>` 태그를 사용하여 fallback 콘텐츠를 지정한다.

HTTP를 사용하기 때문에 이러한 종류의 스트리밍은 거의 모든 엣지 서버, 미디어 공급자, 캐싱 시스템, 라우터 및 방화벽에서 자동으로 지원된다.

> **참고**: 많은 기존 스트리밍 서비스가 최종 사용자에게 컨텐츠를 배포하기 위해 전문 서버를 필요로 한다. 이러한 서버는 설정 및 유지보수를 위해 전문화된 기술이 필요하며 대규모 구축에서는 비용이 많이 들 수 있다. HTTP Live Streaming은 표준 HTTP를 사용하여 미디어를 전달함으로써 이 문제를 방지한다. 또한 HTTP Live Streaming은 대규모 작업을 위해 미디어 배포 네트워크와 원할하게 연동되도록 설계된다.

HTTP Live Streaming 사양은 IETF Internet-Draft이다. 사양에 대한 링크는 아래 참조 섹션을 참조하라.

### At a Glance

HTTP Live Streaming은 웹 서버에서 데스크탑의 클라이언트 소프트웨어 또는 iOS 기반 장치로 오디오와 비디오를 HTTP를 통해 전송하는 방법이다.

#### You Can Send Audio and Video Without Special Server Software

당신은 일반 웹 서버에서 HTTP Live Streaming 오디오와 비디오를 제공할 수 있다. 클라이언트 소프트웨어는 iOS 또는 Mac OS X용으로 작성한 Safari 브라우저 또는 앱일 수 있다.

HTTP Live Streaming은 오디오와 비디오를 일련의 작은 파일\(일반적으로 미디어 세그먼트 파일이라고 하는 10초 정도의 시간\)으로 보낸다. 인덱스 파일 또는 재생 목록은 클라이언트에 미디어 세그먼트 파일의 URL을 제공한다. 미디어 세그먼트 파일이 지속적으로 생성되는 라이브 방송을 수용할 수 있도록 재생 목록을 정기적으로 업데이트 할 수 있다. 웹 페이지에 재생 목록에 대한 링크를 포함하거나 작성한 앱으로 보낼 수 있다.

> **관련 챕터:** [HTTP Streaming Architecture](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/StreamingMediaGuide/HTTPStreamingArchitecture/HTTPStreamingArchitecture.html#//apple_ref/doc/uid/TP40008332-CH101-SW2)

### Prerequisites

일반적인 오디오 및 비디오 파일 형식에 대한 일반적인 이해와 웹 서버와 브라우저 작동 방식을 숙지해야 한다.

### See Also

* _iOS Human Interface Guidelines_—iOS 기반 기기에 웹 컨텐츠 디자인하는 방법
* [HTTP Live Streaming protocol](http://tools.ietf.org/html/draft-pantos-http-live-streaming)—HTTP Live Streaming 규격의 IETF Internet-Draft
* [HTTP Live Streaming Resources](https://developer.apple.com/streaming/)—시작에 도움이 되는 정보 및 도구 모음
* [_MPEG-2 Stream Encryption Format for HTTP Live Streaming_](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/HLS_Sample_Encryption/Intro/Intro.html#//apple_ref/doc/uid/TP40012862)—암호 형식에 대한 상세한 설명

###  

