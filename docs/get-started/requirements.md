---
title: Xamarin. 양식 요구 사항
description: Xamarin에 대 한 플랫폼 및 개발 시스템 요구 사항 양식에.
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2019
no-loc:
- Xamarin
- Xamarin.Forms
- Xamarin.Android
- Xamarin.Essentials
- Xamarin.iOS
- Xamarin.Mac
ms.openlocfilehash: d12daa358917399fc5fd1febf02d4f96a647f360
ms.sourcegitcommit: 6f09bc2b760e76a61a854f55d6a87c4f421ac6c8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75607856"
---
# <a name="opno-locxamarinforms-requirements"></a>Xamarin. 양식 요구 사항

_Xamarin에 대 한 플랫폼 및 개발 시스템 요구 사항 양식에._

플랫폼 전반에 적용되는 설치 및 설정 사례에 대한 개요의 경우 [설치](installation/index.md) 아티클을 참조합니다.

## <a name="target-platforms"></a>대상 플랫폼

Xamarin. 다음 운영 체제에 대해 양식 응용 프로그램을 작성할 수 있습니다.

- iOS 9 이상
- Android 4.4(API 19) 이상([자세한 내용](#android))
- [Windows 10 UWP(유니버설 Windows 플랫폼)](#windows10)

그러나 Android 5.0 (API 21)는 최소 API로 권장 됩니다. 이렇게 하면 대부분의 Android 장치를 대상으로 하면서 모든 Android 지원 라이브러리와 완벽 하 게 호환 됩니다.

개발자가 [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)에 대해 잘 알고 있다고 가정 합니다.

### <a name="additional-platform-support"></a>추가 플랫폼 지원

이러한 플랫폼의 상태는Xamarin에서 사용할 수 있습니다 [. 양식 GitHub](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support):

- Samsung Tizen
- macOS
- GTK#
- WPF

### <a name="android"></a>Android

최신 Android SDK 도구 및 Android API 플랫폼이 설치되어 있어야 합니다. [Android SDK Manager](~/android/get-started/installation/android-sdk.md)를 사용하여 최신 버전으로 업데이트할 수 있습니다.

또한 Android 프로젝트에 대한 대상/컴파일 버전은 *가장 최근에 설치된 플랫폼을 사용*하도록 설정**되어야** 합니다. 그러나 최소 버전은 Android 4.4 이상을 사용하는 디바이스를 계속 지원할 수 있도록 API 19로 설정될 수 있습니다. 이러한 값은 **프로젝트 옵션**에 설정됩니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**프로젝트 옵션 &gt; 애플리케이션 &gt; 애플리케이션 속성**

![Visual Studio의 Android 빌드 옵션](requirements-images/options-android-vs-sml.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

**빌드 > 일반**

![최신 대상 프레임 워크 선택](requirements-images/options-general-sml.png)

**빌드 &gt; Android 애플리케이션**

![앱에 대 한 최소 및 대상 Android 버전을 선택 합니다.](requirements-images/options-android-sml.png)

-----

## <a name="development-system-requirements"></a>개발 시스템 요구 사항

Xamarin. Forms 앱은 macOS 및 Windows에서 개발할 수 있습니다. 그러나 Windows 버전 앱을 생성하려면 Windows 및 Visual Studio가 필요합니다.

## <a name="mac-system-requirements"></a>Mac 시스템 요구 사항

Mac용 Visual Studio를 사용 하 여 Xamarin를 개발할 수 있습니다. MacOS의 Forms 앱-시에라리온 (10.13) 이상. IOS 앱을 개발 하려면 최신 버전의 Xcode, iOS 및 macOS를 사용 하는 것이 좋습니다. 특정 버전 요구 사항은 최신 [Xamarin. iOS 릴리스 정보](/xamarin/ios/release-notes/)를 참조 하세요.

> [!NOTE]
> Windows 앱은 macOS에서 개발할 수 없습니다.

## <a name="windows-system-requirements"></a>Windows 시스템 요구 사항

Xamarin. IOS 및 Android 용 양식 앱은 Xamarin 개발을 지 원하는 모든 Windows 설치를 기반으로 빌드할 수 있습니다. 최신 플랫폼 기능을 완벽 하 게 지원 하려면 최신 버전의 Visual Studio를 사용 합니다. 

네트워크에 연결 된 Mac은 최신 버전의 Xcode 및 Apple에서 지정 된 최소 버전의 macOS를 사용 하 여 iOS를 개발 하는 데 필요 합니다.

<a name="windows10" />

### <a name="universal-windows-platform-uwp"></a>UWP(유니버설 Windows 플랫폼)

Xamarin개발 UWP 용 Forms 앱에는 다음이 필요 합니다.

- Windows 10 (최신 버전 권장, 적합 한 작성자 업데이트 최소)

- Visual Studio 2019 권장

- [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

기존 Xamarin에 [유니버설 Windows 플랫폼 (UWP) 앱을 추가할](~/xamarin-forms/platform/windows/installation/index.md) 수 있습니다. 양식 솔루션 (언제 든 지)

## <a name="deprecated-platforms"></a>사용 되지 않는 플랫폼

Xamarin를 사용 하는 경우 이러한 플랫폼은 지원 되지 않습니다. 양식 3.0 이상:

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*
