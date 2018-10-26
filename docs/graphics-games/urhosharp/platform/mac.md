---
title: UrhoSharp Mac 지원
description: 이 문서에서는 macOS UrhoSharp 지원을 설명 합니다. 프로젝트를 만드는 방법에 설명 하 고 몇 가지 샘플 코드에 대 한 링크를 제공 합니다.
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 6d0a048020284319682c1bee0f9a1d7f9af00977
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113874"
---
# <a name="urhosharp-mac-support"></a>UrhoSharp Mac 지원

_Mac 특정 설정 및 기능_

플랫폼 특정 기능을 활용 하려는 동안 Urho 이식 가능한 클래스 라이브러리 이며, 게임 논리에 대 한 다양 한 플랫폼에서 사용할 동일한 API를 사용 하면 여전히 초기화 해야 Urho 플랫폼 특정 드라이버에 일부 경우, .

페이지 아래에서 가정 `MyGame` 의 서브 클래스는 `Application` 클래스.

## <a name="macos"></a>macOS

**지원 되는 아키텍처:** x86/x86-64 32 비트 및 64 비트에 대 한 합니다.

## <a name="creating-a-project"></a>프로젝트 만들기

콘솔 프로젝트를 만들고 Urho NuGet 참조를 찾을 수 있습니다 자산 (데이터 디렉터리를 포함 하는 디렉터리) 있는지 확인 합니다.

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>예제

[전체 예제](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)


