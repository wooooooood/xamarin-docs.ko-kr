---
title: 태블릿 및 데스크톱 앱에 대 한 레이아웃
description: 이 문서에서는 태블릿, 휴대폰 대신 Xamarin.Forms 응용 프로그램 레이아웃을 최적화 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: 9d1f54fa4753ba2ef44ba9b8b48a84a3ca932c4b
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233850"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>태블릿 및 데스크톱 앱에 대 한 레이아웃

Xamarin.Forms는 휴대폰 외에도 앱 에서도 실행할 수 있도록 지원 되는 플랫폼에서 사용할 수 있는 모든 장치 유형을 지원 합니다.

* Ipad,
* Android 태블릿
* Windows 태블릿 및 데스크톱 컴퓨터 (Windows 10 실행).

이 페이지에 간략하게 설명 합니다.

* 지원 되는 [디바이스](#Device_Types), 및
* 하는 방법 [최적화](#optimize) 휴대폰 및 태블릿에 대 한 레이아웃입니다.

<a name="Device_Types" />

## <a name="device-types"></a>장치 유형

큰 화면 장치 모든 Xamarin.Forms에서 지 원하는 플랫폼에 대해 사용할 수 있습니다.

### <a name="ipads-ios"></a>Ipad (iOS)

Xamarin.Forms 템플릿을 구성 하 여 iPad 지원을 자동으로 포함 된 **Info.plist > 장치** 설정을 **유니버설** (즉, iPhone 및 iPad를 모두 지원 됩니다).

즐겁고 시작 환경을 제공 하 고 모든 장치에서 전체 화면 해상도 사용 해야는 [iPad 관련 시작 화면](~/ios/app-fundamentals/images-icons/launch-screens.md) 제공 됩니다 (스토리 보드를 사용 하 여). 이렇게 하면 앱은 iPad 미니, iPad 및 iPad Pro 장치에서 올바르게 렌더링 됩니다.

IOS 9 이전 모든 앱을 장치에서 전체 화면을 수행 하지만 일부 Ipad 이제 수행할 수 있습니다 [분할 화면 멀티태스킹](~/ios/platform/multitasking.md)합니다.
즉, 앱 화면에서 화면 또는 전체 화면 너비의 50% 측면의 슬림 열만이 걸릴 수 있습니다.

[![](tablet-images/ipad-sml.png "iPad 화면 예제 분할")](tablet-images/ipad.png#lightbox "iPad 화면 예제 분할")

화면 분할 기능이 의미 넓거나 만큼 1366 픽셀 너비를 적게 320 픽셀 잘 작동 하도록 앱을 디자인 해야 합니다.

### <a name="android-tablets"></a>Android 태블릿

Android 에코 시스템에 큰 태블릿까지 작은 휴대폰에서 지원 되는 화면 크기, 수많은 있습니다. Xamarin.Forms는 모든 화면 크기를 지원할 수 있습니다 하지만으로 다른 플랫폼을 사용 하 여 하려는 큰 장치에 대 한 사용자 인터페이스를 조정 합니다.

많은 다른 화면 해상도 지 원하는 사용자 환경을 최적화 하기 위해 다양 한 크기의 네이티브 이미지 리소스를 제공할 수 있습니다.
검토 합니다 [Android 리소스](~/android/app-fundamentals/resources-in-android/index.md) 설명서 (및 특히 [화면 크기를 변경 하는 것에 대 한 리소스를 만드는](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) 폴더 및 Android 앱의 파일 이름을 구성 하는 방법에 대 한 자세한 내용은 앱에 최적화 된 이미지 리소스를 포함 하는 프로젝트입니다.

### <a name="windows-tablets-and-desktops"></a>Windows 태블릿 및 데스크톱

태블릿 및 Windows를 실행 하는 데스크톱 컴퓨터를 지원 하기 위해 사용 해야 [Windows UWP 지원](~/xamarin-forms/platform/windows/installation/index.md), Windows 10에서 실행 되는 유니버설 앱을 빌드입니다.

Windows 태블릿 및 데스크톱에서 실행 되는 앱을 조정할 수 있는 임의의 차원에 또한 전체 화면을 실행 합니다.

[![](tablet-images/splitscreen-sml.png "Windows 분할 화면 예제")](tablet-images/splitscreen.png#lightbox "Windows 분할 화면 예제")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>태블릿 및 데스크톱 최적화

태블릿/데스크톱 장치를 사용 하는 또는 휴대폰 여부를 따라 Xamarin.Forms 사용자 인터페이스를 조정할 수 있습니다. 즉, 태블릿 및 데스크톱 컴퓨터와 같은 큰 화면 장치에 대 한 사용자 환경을 최적화할 수도 있습니다.


### <a name="deviceidiom"></a>Device.Idiom

사용할 수는 [ `Device` ](~/xamarin-forms/platform/device.md) 앱 또는 사용자 인터페이스의 동작을 변경 하는 클래스입니다. 사용 하는 `Device.Idiom` 있습니다 열거형

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

이 방법은 개별 페이지 레이아웃에 대 한 중요 한 변경 또는 더 큰 화면에서 완전히 다른 페이지를 렌더링 확장할 수 있습니다.

### <a name="leveraging-masterdetailpage"></a>MasterDetailPage 활용

[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 큰 화면을 사용 하는 iPad에서 특히 적합 합니다 [ `UISplitViewController` ](xref:UIKit.UISplitViewController) 네이티브 iOS 편리 하 게 합니다.

검토 [Xamarin 블로그 포스트](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/) 휴대폰 레이아웃을 사용 하 고 더 큰 화면 간에 사용할 수 있도록 사용자 인터페이스에 적용할 수는 방법을 확인 하려면 (사용 하 여는 `MasterDetailPage`).



## <a name="related-links"></a>관련 링크

- [Xamarin 블로그](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [MyShoppe 샘플](https://github.com/jamesmontemagno/myshoppe)
