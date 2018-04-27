---
title: 태블릿 및 데스크톱 응용 프로그램 레이아웃
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: bcd277145de13a95a0b19aa4945b02078af52978
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="layout-for-tablet-and-desktop-apps"></a>태블릿 및 데스크톱 응용 프로그램 레이아웃

Xamarin.Forms 전화, 외에도 응용 프로그램에서 실행할 수도 있도록 지원 되는 플랫폼에서 사용할 수 있는 모든 장치 유형에 지원 합니다.

* Ipad,
* Android 태블릿
* Windows 태블릿 및 데스크톱 컴퓨터 (Windows 10 실행).

이 페이지에 간략하게 설명 합니다.

* 지원 되는 [장치 유형에](#Device_Types), 및
* 방법 [최적화](#optimize) 휴대폰 및 태블릿에 대 한 레이아웃 합니다.

<a name="Device_Types" />

## <a name="device-types"></a>장치 유형

큰 화면 장치 Xamarin.Forms에서 지 원하는 플랫폼의 모든 수 있습니다.

### <a name="ipads-ios"></a>Ipad (iOS)

Xamarin.Forms 템플릿을 구성 하 여 iPad 지원을 자동으로 포함 된 **Info.plist > 장치** 설정을 **유니버설** (즉, iPhone 및 iPad를 모두 지원 됩니다).

쾌적 한 시작 환경을 제공 하 고 모든 장치에서 전체 화면 해상도 사용을 확인 하려면 해야는 [iPad 관련 시작 화면](~/ios/app-fundamentals/images-icons/launch-screens.md) 제공 됩니다 (스토리 보드를 사용 하 여). 이렇게 하면 앱은 iPad 미니, iPad 및 iPad Pro 장치에서 올바르게 렌더링 됩니다.

IOS 9 하기 전에 모든 앱은 장치에서 전체 화면을 걸린 일부 Ipad 수행할 수 있지만 [화면 멀티태스킹 분할](~/ios/platform/multitasking.md)합니다.
즉, 앱이 화면에서 화면 또는 전체 화면 너비의 50% 측면에 slim 열만 걸릴 수 있습니다.

[![](tablet-images/ipad-sml.png "iPad 분할 화면 예제")](tablet-images/ipad.png#lightbox "iPad 분할 화면 예제")

화면 분할 기능 의미 넓거나으로 1366 픽셀 너비를 적게 320 픽셀 함께 사용할 수 있는 응용 프로그램을 디자인 해야 합니다.

### <a name="android-tablets"></a>Android 태블릿

Android 에코 시스템의 큰 태블릿까지 작은 휴대폰에서 지원 되는 화면 크기, 깊이 있습니다. Xamarin.Forms는 모든 화면 크기를 지원할 수 있지만으로 다른 플랫폼과 상호 있습니다 수도 더 큰 장치에 대 한 사용자 인터페이스를 조정 하려면.

많은 다른 화면 해상도 지 원하는 경우 사용자 경험을 최적화 하기 위해 다양 한 크기의 네이티브 이미지 리소스를 제공할 수 있습니다.
검토는 [Android 리소스](~/android/app-fundamentals/resources-in-android/index.md) 설명서 (특히 및 [위한 다양 한 화면 크기에 따라 리소스를 만드는](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) 폴더 및 Android 앱에서 파일 이름을 구성 하는 방법에 대 한 자세한 내용은 응용 프로그램에 최적화 된 이미지 리소스를 포함 하도록 프로젝트.

### <a name="windows-tablets-and-desktops"></a>Windows 태블릿 및 데스크톱용

사용 해야 태블릿 및 Windows를 실행 하는 데스크톱 컴퓨터를 지원 하려면 [UWP Windows 지원](~/xamarin-forms/platform/windows/installation/index.md), Windows 10에서 실행 되는 유니버설 앱 빌드입니다.

Windows 태블릿 및 데스크톱에서 실행 되는 앱을 조정할 수 있는 임의의 차원에 뿐만 아니라 전체 화면을 실행 합니다.

[![](tablet-images/splitscreen-sml.png "Windows 분할 화면 예제")](tablet-images/splitscreen.png#lightbox "Windows 분할 화면 예제")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>태블릿 및 데스크톱에 대 한 최적화

여부 휴대폰에 따라 Xamarin.Forms 사용자 인터페이스를 조정할 수 있습니다 또는 태블릿/데스크톱 장치가 사용 중이면 합니다. 즉, 태블릿 및 데스크톱 컴퓨터와 같은 큰 화면 장치에 대 한 사용자 환경을 최적화할 수 있습니다.


### <a name="deviceidiom"></a>Device.Idiom

사용할 수는 [ `Device` ](~/xamarin-forms/platform/device.md) 앱 또는 사용자 인터페이스의 동작을 변경 하는 클래스입니다. 사용 하 여 `Device.Idiom` 열거형 할 수 있습니다

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

이 방법은를 개별 페이지 레이아웃의 중요 한 변경 하거나 큰 화면에서 완전히 다른 페이지를 렌더링을 확장할 수 있습니다.

### <a name="leveraging-masterdetailpage"></a>MasterDetailPage 활용

[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 큰 화면을 사용 하는 위치 iPad에서 특히 적합는 [ `UISplitViewController` ](https://developer.xamarin.com/api/type/UIKit.UISplitViewController/) 네이티브 iOS 편리 하 게 합니다.

검토 [이 Xamarin 블로그 게시물](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/) 보려면 휴대폰 한 레이아웃을 사용 하 고 다른 화면이 사용할 수 있도록 사용자 인터페이스에 조정 하는 방법 (으로 `MasterDetailPage`).



## <a name="related-links"></a>관련 링크

- [Xamarin 블로그](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [MyShoppe 샘플](https://github.com/jamesmontemagno/myshoppe)
