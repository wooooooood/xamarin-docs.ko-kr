---
title: Xamarin.Forms 요구 사항
description: Xamarin.Forms에 대한 플랫폼 및 개발 시스템 요구 사항
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2018
ms.openlocfilehash: 504f20f6575e559d7c4965643b74b407d5e84de8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112365"
---
# <a name="xamarinforms-requirements"></a>Xamarin.Forms 요구 사항

_Xamarin.Forms에 대한 플랫폼 및 개발 시스템 요구 사항._

플랫폼 전반에 적용되는 설치 및 설정 사례에 대한 개요의 경우 [설치](~/cross-platform/get-started/installation/index.md) 글을 참조합니다.

## <a name="target-platforms"></a>대상 플랫폼

다음 운영 체제에 대한 Xamarin.Forms 응용 프로그램을 작성할 수 있습니다.

- iOS 8 이상
- Android 4.4(API 19) 이상([자세한 내용](#android))
- [Windows 10 UWP(유니버설 Windows 플랫폼)](#windows10)

개발자가 [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) 및 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)에 익숙하다고 가정합니다.

### <a name="additional-platform-support"></a>추가 플랫폼 지원

이러한 플랫폼의 상태는 [Xamarin.Forms GitHub](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support)에서 사용할 수 있습니다.

- Samsung Tizen
- macOS
- GTK#
- WPF

### <a name="platforms-from-earlier-versions"></a>이전 버전의 플랫폼

Xamarin.Forms 3.0을 사용하는 경우 이러한 플랫폼은 지원되지 않습니다.

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*

### <a name="android"></a>Android

최신 Android SDK 도구 및 Android API 플랫폼이 설치되어 있어야 합니다. [Android SDK Manager](~/android/get-started/installation/android-sdk.md)를 사용하여 최신 버전으로 업데이트할 수 있습니다.

또한 Android 프로젝트에 대한 대상/컴파일 버전은 *가장 최근에 설치된 플랫폼을 사용*하도록 설정**되어야** 합니다. 그러나 최소 버전은 Android 4.4 이상을 사용하는 장치를 계속 지원할 수 있도록 API 19로 설정할 수 있습니다. 이러한 값은 **프로젝트 옵션**에서 설정합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**프로젝트 옵션 > 응용 프로그램 > 응용 프로그램 속성**

![](installation-images/options-android-vs-sml.png "Visual Studio의 Android 빌드 옵션")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

**빌드 > 일반**

![](installation-images/options-general-sml.png "빌드 > 일반")

**빌드 > Android 응용 프로그램**

![](installation-images/options-android-sml.png "빌드 > Android 응용프로그램")

-----

## <a name="development-system-requirements"></a>개발 시스템 요구 사항

Xamarin.Forms 앱은 macOS 및 Windows에서 개발할 수 있습니다. 그러나 Windows 버전 앱을 생성하려면 Windows 및 Visual Studio가 필요합니다.

## <a name="mac-system-requirements"></a>Mac 시스템 요구 사항

Mac용 Visual Studio를 사용하여 OS X El Capitan(10.11) 이상에서 Xamarin.Forms 앱을 개발할 수 있습니다. iOS 앱을 개발하려면 적어도 iOS 10 SDK 및 Xcode 8를 설치하는 것이 좋습니다.

> [!NOTE]
>  Windows 앱은 macOS에서 개발할 수 없습니다.

## <a name="windows-system-requirements"></a>Windows 시스템 요구 사항

Xamarin 개발을 지원하는 모든 Windows 설치에서 iOS 및 Android용 Xamarin.Forms 앱을 만들 수 있습니다. 이 경우 Windows 7 이상에서 Visual Studio 2017 이상이 실행되어야 합니다. iOS 개발을 위해 네트워크로 연결된 Mac이 필요합니다.

<a name="windows10" />

### <a name="universal-windows-platform-uwp"></a>UWP(Universal Windows Platform)

UWP용 Xamarin.Forms 앱 개발에 다음이 필요합니다.

- Windows 10(Fall Creators Update 권장)

- Visual Studio 2017

- [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

UWP 프로젝트는 Visual Studio 2017에서 만들어진 Xamarin.Forms 솔루션에 포함되지만, Mac용 Visual Studio에서 만들어진 솔루션에는 포함되지 않습니다.
언제든지 [UWP(Univeral Windows Platform) 앱](~/xamarin-forms/platform/windows/installation/index.md)을 기존 Xamarin.Forms 솔루션에 추가할 수 있습니다.
