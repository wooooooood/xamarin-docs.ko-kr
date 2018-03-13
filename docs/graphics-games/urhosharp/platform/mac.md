---
title: "UrhoSharp Mac 지원"
description: "Mac 특정 설치 및 UrhoSharp에 대 한 기능입니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: fff96a19d5f5286f2c9483407fcaaab6d15ff2b5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="urhosharp-mac-support"></a>UrhoSharp Mac 지원

_Mac 특정 설정 및 기능_

플랫폼 특정 기능을 활용 하려는 Urho 이식 가능한 클래스 라이브러리는 고 게임 논리를 다양 한 플랫폼 전반에 걸쳐 사용할 동일한 API를 사용 하면 여전히 초기화 해야 Urho 플랫폼 특정 드라이버에서 한 경우에 따라, 장치 .

아래의 페이지 가정 `MyGame` 의 서브 클래스는 `Application` 클래스입니다.

## <a name="macos"></a>macOS

**지원 되는 아키텍처:** x86/x86-64 32 비트 및 64 비트에 대 한 합니다.

## <a name="creating-a-project"></a>프로젝트 만들기

콘솔 프로젝트를 만들고 Urho NuGet 참조를 찾을 수 있는지 자산 (데이터 디렉터리를 포함 하는 디렉터리) 되었는지 확인 합니다.

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>예

[전체 예제](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)


