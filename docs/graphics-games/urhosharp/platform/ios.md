---
title: "UrhoSharp iOS 및 tvOS 지원"
description: "iOS 및 tvOS 특정 설치 프로그램 및 기능"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B06567E-E789-4EA1-A2A9-F3B2212EDD23
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 9cf779b23ed830c07af0100152a44d6c3c4e317b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="urhosharp-ios-and-tvos-support"></a>UrhoSharp iOS 및 tvOS 지원

_iOS 및 tvOS 특정 설치 프로그램 및 기능_

플랫폼 특정 기능을 활용 하려는 Urho 이식 가능한 클래스 라이브러리는 고 게임 논리를 다양 한 플랫폼 전반에 걸쳐 사용할 동일한 API를 사용 하면 여전히 초기화 해야 Urho 플랫폼 특정 드라이버에서 한 경우에 따라, 장치 .

아래의 페이지 가정 `MyGame` 의 sublcass는는 `Application` 클래스입니다.

# <a name="ios-and-tvos"></a>iOS 및 tvOS

**지원 되는 아키텍처:** armv7, arm64, i386

# <a name="creating-a-project"></a>프로젝트 만들기

IOS 프로젝트를 만든 후 데이터 디렉터리에 추가 리소스를 모든 파일이 있는지 확인 **BundleResource** 로 **빌드 작업**합니다.

![설치 프로젝트](ios-images/image-4.png "Resources 디렉터리에 데이터 추가")

# <a name="configuring-and-launching-urho"></a>구성 및 Urho 시작

에 대 한 문을 사용 하 여 추가 `Urho` 및 `Urho.iOS` 네임 스페이스를 Urho, 초기화 수 있을 뿐만 아니라 응용 프로그램을 시작에 대 한이 코드를 추가 합니다.

```csharp
new MyGame().Run();
```

IOS에서는 이후 다음에 유의 `FinishedLaunching` 완료 하는 데에 대 한 호출을 대기 해야 `Run()` 를 메서드가 완료 된 후 실행할 때 일반적입니다.

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    LaunchGame();
    return true;
}

async void LaunchGame()
{
    await Task.Yield();
    new SamplyGame().Run();
}
```

이 기본 iOS PNG 최적화 프로그램에서 Urho 제대로 현재 소비 하지 수 있는 이미지를 생성 하므로 PNG 최적화 기능을 사용 하지 않도록 설정 하는 중요

# <a name="custom-embedding-of-urho"></a>Urho의 사용자 지정 포함

있습니다 수 또는를 가지도록 Urho을 전체 응용 프로그램 화면 하 고 응용 프로그램의 구성 요소를 사용 하 여 만들 수 있습니다는 `UrhoSurface` 되는 `UIView` 는 기존 응용 프로그램에 포함할 수 있습니다.

다음은 수행 해야 하는 작업입니다.

```csharp
var view = new UrhoSurface () {
    Frame = new CGRect (100,100,200,200),
    BackgroundColor = UIColor.Red
}
window.AddSubview (view);
```

이 있으므로 Urho 클래스에 호스트 하 고이 작업을 수행 합니다.

```csharp
new MyGame().Run ();
```

