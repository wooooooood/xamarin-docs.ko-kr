---
title: Xamarin.Forms 요구 사항
description: Xamarin.Forms에 대한 플랫폼 및 개발 시스템 요구 사항
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/19/2017
ms.openlocfilehash: e62c82b351bab759192a4fe879a3b63754cdf0af
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinforms-requirements"></a>Xamarin.Forms 요구 사항

_Xamarin.Forms에 대한 플랫폼 및 개발 시스템 요구 사항._

플랫폼 전반에 적용되는 설치 및 설정 사례에 대한 개요의 경우 [설치](~/cross-platform/get-started/installation/index.md) 아티클을 참조합니다.

## <a name="target-platforms"></a>대상 플랫폼

다음 운영 체제에 대한 Xamarin.Forms 응용 프로그램을 작성할 수 있습니다.

-  iOS 8 이상
-  Android 4.0.3(API 15) 이상([자세한 내용](#android))
-  [Windows 10 UWP(유니버설 Windows 플랫폼)](#windows10)
-  Windows 8.1/Windows Phone 8.1 WinRT([자세한 내용](#windows))
-  *Windows Phone 8 Silverlight(사용되지 않음)*

개발자가 [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md) 및 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)에 익숙하다고 가정합니다.

<a name="android" />

### <a name="android"></a>Android

최신 Android SDK 도구 및 Android API 플랫폼이 설치되어 있어야 합니다. [Android SDK Manager](~/android/get-started/installation/android-sdk.md)를 사용하여 최신 버전으로 업데이트할 수 있습니다.

또한 Android 프로젝트에 대한 대상/컴파일 버전은 *가장 최근에 설치된 플랫폼을 사용*하도록 설정**되어야** 합니다. 그러나 최소 버전은 Android 4.0.3 이상을 사용하는 장치를 계속 지원할 수 있도록 API 15로 설정될 수 있습니다. 이러한 값은 **프로젝트 옵션**에 설정됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**프로젝트 옵션 > 응용 프로그램 > 응용 프로그램 속성**

![](installation-images/options-android-vs-sml.png "Visual Studio의 Android 빌드 옵션")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**빌드 > 일반**

![](installation-images/options-general-sml.png "빌드 > 일반")

**빌드 > Android 응용 프로그램**

![](installation-images/options-android-sml.png "빌드 > Android 응용프로그램")

-----


<a name="windows10" />

### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

Windows 10 UWP 프로젝트는 솔루션을 macOS에서 만드는 경우 추가되지 않습니다. 이 프로젝트를 기존 솔루션에 추가하는 방법에 관한 지침은 [UWP(유니버설 Windows 플랫폼) 앱 추가](~/xamarin-forms/platform/windows/installation/universal.md)를 참조하세요.


<a name="windows" />

### <a name="windows-81--windows-phone-81-winrt"></a>Windows 8.1 / Windows Phone 8.1 WinRT

Windows 8.1 / Windows Phone 8.1 WinRT 프로젝트는 솔루션을 macOS에서 만드는 경우 추가되지 않습니다. 이 프로젝트를 기존 솔루션에 추가하는 방법에 대한 지침은 [Windows Phone 앱 추가](~/xamarin-forms/platform/windows/installation/phone.md) 및 [Windows 앱 추가](~/xamarin-forms/platform/windows/installation/tablet.md)를 참조하세요.


## <a name="development-system-requirements"></a>개발 시스템 요구 사항

Xamarin.Forms 앱은 macOS 및 Windows에서 개발할 수 있습니다. 그러나 Windows 버전 앱을 생성하려면 Windows 및 Visual Studio가 필요합니다.

## <a name="mac-system-requirements"></a>Mac 시스템 요구 사항

Mac용 Visual Studio를 사용하여 OS X El Capitan(10.11) 이상에서 Xamarin.Forms 앱을 개발할 수 있습니다. iOS 앱을 개발하려면 적어도 iOS 10 SDK 및 Xcode 8를 설치하는 것이 좋습니다.

> [!NOTE]
>  Windows 앱은 macOS에서 개발할 수 없습니다.

## <a name="windows-system-requirements"></a>Windows 시스템 요구 사항

Xamarin 개발을 지원하는 모든 Windows 설치에서 iOS 및 Android용 Xamarin.Forms 앱을 빌드할 수 있습니다. 이 경우 Windows 7 이상에서 실행되는 Visual Studio 2015 이상이 필요합니다. iOS 개발을 위해 네트워크로 연결된 Mac이 필요합니다.

### <a name="universal-windows-platform-uwp"></a>UWP(유니버설 Windows 플랫폼)

UWP용 Xamarin.Forms 앱 개발에 다음이 필요합니다.

* Windows 10(Fall Creators Update 권장)

* Visual Studio 2017을 사용하는 것이 좋습니다.

* [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

UWP 프로젝트는 Visual Studio 2015 및 Visual Studio 2017에서 만들어진 Xamarin.Forms 솔루션에 포함됩니다.
또한 [UWP(유니버설 Windows 플랫폼) 앱](~/xamarin-forms/platform/windows/installation/universal.md)을 기존 Xamarin.Forms 솔루션에 추가할 수 있습니다.

