---
description: OpenGL ES 텍스처 객체를 만들고 관리하는데 사용되는 캐시.
---

# CVOpenGLESTextureCache

### Overview

Core Video OpenGL ES 텍스처 캐시는 [`CVOpenGLESTexture`](https://developer.apple.com/documentation/corevideo/cvopenglestexture) 텍스처를 캐시하고 관리하는 데 사용된다. 이러한 텍스처 캐시는 GL ES에서 420v 또는 BGRA와 같은 다양한 픽셀 형식으로 버퍼를 직접 읽고 쓸 수 있는 방법을 제공한다.

