---
title: 태블릿 및 데스크톱 앱의 레이아웃
description: 이 문서에서는 Xamarin.Forms 휴대폰과 달리 태블릿 용 응용 프로그램 레이아웃을 최적화 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0ecbc850960465296dc4047277bdafe78ac800a4
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573250"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>태블릿 및 데스크톱 앱의 레이아웃

Xamarin.Forms는 지원 되는 플랫폼에서 사용할 수 있는 모든 장치 유형을 지원 하므로 휴대폰 외에도 앱을 다음에서 실행할 수 있습니다.

- Ipad
- Android 태블릿,
- Windows 태블릿 및 데스크톱 컴퓨터 (Windows 10 실행).

이 페이지에 대 한 간략 한 설명:

- 지원 되는 [장치 유형](#device-types)및
- 태블릿 및 휴대폰의 레이아웃을 [최적화](#optimize-for-tablet-and-desktop) 하는 방법입니다.

## <a name="device-types"></a>장치 유형

에서 지 원하는 모든 플랫폼에 대해 더 큰 화면 장치를 사용할 수 있습니다 Xamarin.Forms .

### <a name="ipads-ios"></a>Ipad (iOS)

Xamarin.Forms **Info.plist > Devices** 설정을 **유니버설** (iPhone 및 iPad 모두 지원 됨)로 구성 하 여 템플릿에 iPad 지원이 자동으로 포함 됩니다.

편리한 시작 환경을 제공 하 고 모든 장치에서 전체 화면 해상도를 사용 하도록 하려면 [iPad 특정 시작 화면](~/ios/app-fundamentals/images-icons/launch-screens.md) (storyboard 사용)이 제공 되어 있는지 확인 해야 합니다. 이렇게 하면 iPad 미니, iPad 및 iPad Pro 장치에서 앱이 올바르게 렌더링 됩니다.

IOS 9 이전에 모든 앱은 장치에서 전체 화면을 차지 했지만 일부 Ipad는 이제 [화면 멀티태스킹 분할](~/ios/platform/multitasking.md)을 수행할 수 있습니다.
즉, 앱에서 화면의 측면에 있는 슬림 한 열, 화면 너비의 50% 또는 전체 화면을 모두 사용할 수 있습니다.

[![](tablet-images/ipad-sml.png "iPad Split Screen Example")](tablet-images/ipad.png#lightbox "iPad Split Screen Example")

화면 분할 기능을 사용 하는 경우 320 픽셀 너비 또는 1366 픽셀 너비 만큼 잘 작동 하도록 앱을 디자인 해야 합니다.

### <a name="android-tablets"></a>Android 태블릿

Android 에코 시스템은 작은 휴대폰에서 최대 규모의 태블릿까지 수많은 지원 되는 화면 크기를 가집니다. Xamarin.Forms는 모든 화면 크기를 지원할 수 있지만 다른 플랫폼과 마찬가지로 더 큰 장치에 대 한 사용자 인터페이스를 조정 하는 것이 좋습니다.

다양 한 화면 해상도를 지원할 때 네이티브 이미지 리소스를 다양 한 크기로 제공 하 여 사용자 환경을 최적화할 수 있습니다.
Android [리소스](~/android/app-fundamentals/resources-in-android/index.md) 설명서를 검토 하 고, 응용 프로그램에 최적화 된 이미지 리소스를 포함 하도록 android 앱 프로젝트에서 폴더 및 파일 이름을 구성 하는 방법에 대 한 자세한 내용은 특히 [다양 한 화면 크기에 대 한 리소스 만들기](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)를 참조 하세요.

### <a name="windows-tablets-and-desktops"></a>Windows 태블릿 및 데스크톱

Windows를 실행 하는 태블릿 및 데스크톱 컴퓨터를 지원 하려면 windows 10에서 실행 되는 유니버설 앱을 빌드하는 [WINDOWS UWP 지원을](~/xamarin-forms/platform/windows/installation/index.md)사용 해야 합니다.

Windows 태블릿 및 데스크톱에서 실행 되는 앱은 전체 화면 실행 외에도 임의의 차원으로 크기를 조정할 수 있습니다.

[![](tablet-images/splitscreen-sml.png "Windows Split Screen Example")](tablet-images/splitscreen.png#lightbox "Windows Split Screen Example")

## <a name="optimize-for-tablet-and-desktop"></a>태블릿 및 데스크톱을 위한 최적화

Xamarin.Forms휴대폰 또는 태블릿/데스크톱 장치를 사용 중인지 여부에 따라 사용자 인터페이스를 조정할 수 있습니다. 즉, 태블릿 및 데스크톱 컴퓨터와 같은 대량 화면 장치에 대 한 사용자 환경을 최적화할 수 있습니다.

### <a name="deviceidiom"></a>장치.

클래스를 사용 [`Device`](~/xamarin-forms/platform/device.md) 하 여 앱 또는 사용자 인터페이스의 동작을 변경할 수 있습니다. 열거를 사용 하 여 다음을 `Device.Idiom` 수행할 수 있습니다.

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

이 접근 방식을 확장 하 여 개별 페이지 레이아웃을 중요 하 게 변경 하거나 큰 화면에서 완전히 다른 페이지를 렌더링할 수도 있습니다.

### <a name="leverage-masterdetailpage"></a>MasterDetailPage 활용

는 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 특히를 사용 하 여 [`UISplitViewController`](xref:UIKit.UISplitViewController) 네이티브 iOS 환경을 제공 하는 iPad에서 대규모 화면에 적합 합니다.

[이 Xamarin 블로그 게시물](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/) 을 검토 하 여 휴대폰에서 하나의 레이아웃을 사용 하 고 더 큰 화면에서 기타 ()를 사용할 수 있도록 사용자 인터페이스를 조정 하는 방법을 확인할 수 있습니다 `MasterDetailPage` .

## <a name="related-links"></a>관련 링크

- [Xamarin 블로그](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/)
- [MyShoppe 샘플](https://github.com/jamesmontemagno/myshoppe)
