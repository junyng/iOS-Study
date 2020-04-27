---
description: Metal 텍스처 객체를 만들고 관리하는 데 사용되는 캐시.
---

# CVMetalTextureCache

### Overview

Core Video Metal 텍스처 캐시는 [`CVMetalTexture`](https://developer.apple.com/documentation/corevideo/cvmetaltexture) 텍스처를 생성 및 관리한다. 렌더링 또는 Metal 프레임워크를 사용하는 GPU 컴퓨팅 태스크에서 `CVMetalTextureCache` 객체를 사용하여 GPU 기반 Core Video 이미지 버퍼에서 직접 읽거나 GPU에 기록하라. 예를 들어, Metal 텍스처 캐시를 사용하여 Metal로 렌더링된 3D 장면에서 장치 카메라의 라이브 출력을 표시할 수 있다.



