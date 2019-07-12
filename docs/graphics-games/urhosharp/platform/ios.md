---
title: UrhoSharp iOS 및 tvOS 지원
description: 이 문서에서는 iOS 및 tvOS UrhoSharp에 대 한 지원. 프로젝트 만들기, 구성 및 Urho, 시작 Urho의 사용자 지정 포함을 수행 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7B06567E-E789-4EA1-A2A9-F3B2212EDD23
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 3efdbfcfdd670525dbf3198deb17c4631a889c56
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832468"
---
# <a name="urhosharp-ios-and-tvos-support"></a>UrhoSharp iOS 및 tvOS 지원

플랫폼 특정 기능을 활용 하려는 동안 Urho 이식 가능한 클래스 라이브러리 이며, 게임 논리에 대 한 다양 한 플랫폼에서 사용할 동일한 API를 사용 하면 여전히 초기화 해야 Urho 플랫폼 특정 드라이버에 일부 경우, .

페이지 아래에서 가정 `MyGame` 의 sublcass는는 `Application` 클래스입니다.

## <a name="ios-and-tvos"></a>iOS 및 tvOS

**지원 되는 아키텍처:** armv7, arm64, i386

## <a name="creating-a-project"></a>프로젝트 만들기

IOS 프로젝트를 만든 후 리소스 디렉터리에 데이터 추가 모든 파일이 있어야 **BundleResource** 으로 **빌드 작업**합니다.

![설치 프로젝트](ios-images/image-4.png "리소스 디렉터리에 데이터 추가")

## <a name="configuring-and-launching-urho"></a>구성 및 Urho 시작

문을 사용 하 여 추가 `Urho` 및 `Urho.iOS` 네임 스페이스 Urho를 초기화할 수 있을 뿐만 아니라 응용 프로그램을 시작에 대 한이 코드를 추가 합니다.

```csharp
new MyGame().Run();
```

IOS 필요 하므로 있음을 `FinishedLaunching` 를 완료 하려면에 대 한 호출을 큐 해야 `Run()` 이것이 일반적인 방법은 메서드 완료를 실행 하려면:

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

기본 iOS PNG 최적화 프로그램이 Urho 제대로 현재 이용 하는 이미지를 생성 하기 때문에 PNG 최적화를 해제 하는 것

## <a name="custom-embedding-of-urho"></a>사용자 지정 Urho 포함

있습니다 걸릴 수 있습니다 또는 것 Urho 전체 응용 프로그램 화면에서 및를 사용 하려면 응용 프로그램의 구성 요소로 만들 수 있습니다는 `UrhoSurface` 되는 `UIView` 기존 응용 프로그램에 포함할 수 있는 합니다.

다음은 수행 해야 하는 작업입니다.

```csharp
var view = new UrhoSurface () {
    Frame = new CGRect (100,100,200,200),
    BackgroundColor = UIColor.Red
}
window.AddSubview (view);
```

이 호스팅할 Urho 클래스 이므로 다음 수행 하는 합니다.

```csharp
new MyGame().Run ();
```
