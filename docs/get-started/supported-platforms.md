---
title: “Xamarin.Forms 지원 플랫폼” description: “Xamarin.Forms의 플랫폼 및 개발 시스템 요구 사항.”
ms.prod: xamarin ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 01/22/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinforms-supported-platforms"></a>Xamarin.Forms 지원 플랫폼

다음 운영 체제에 대한 Xamarin.Forms 애플리케이션을 작성할 수 있습니다.

- iOS 9 이상.
- Android 4.4(API 19) 이상([자세한 정보](#android-platform-support)) 그러나 최소 API로 Android 5.0(API 21)이 권장됩니다. 이렇게 하면 대부분의 Android 디바이스를 대상으로 하면서 모든 Android 지원 라이브러리와 완벽하게 호환됩니다.
- Windows 10 유니버설 Windows 플랫폼.

iOS, Android 및 UWP(유니버설 Windows 플랫폼)용 Xamarin.Forms 앱은 Visual Studio에서 빌드할 수 있습니다. 그러나 최신 버전의 Xcode와 Apple에서 지정한 macOS 최소 버전을 사용하여 iOS를 개발하려면 네트워크로 연결된 Mac이 필요합니다. 자세한 내용은 [Windows 요구 사항](~/cross-platform/get-started/requirements.md#windows-requirements)을 참조하세요.

iOS 및 Android용 Xamarin.Forms 앱은 Mac용 Visual Studio에서 빌드할 수 있습니다. 자세한 내용은 [macOS 요구 사항](~/cross-platform/get-started/requirements.md#macos-requirements)을 참조하세요.

> [!NOTE]
> Xamarin.Forms를 사용하여 앱을 개발하려면 [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)를 잘 알고 있어야 합니다.

## <a name="additional-platform-support"></a>추가 플랫폼 지원

Xamarin.Forms는 iOS, Android 및 Windows 이외의 추가 플랫폼을 지원합니다.

- Samsung Tizen
- macOS 10.13 이상
- GTK#
- WPF

해당 플랫폼의 상태는 [Xamarin.Forms GitHub 플랫폼 지원 Wiki](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support)에서 사용할 수 있습니다.

## <a name="android-platform-support"></a>Android 플랫폼 지원

최신 Android SDK 도구 및 Android API 플랫폼이 설치되어 있어야 합니다. [Android SDK Manager](~/android/get-started/installation/android-sdk.md)를 사용하여 최신 버전으로 업데이트할 수 있습니다.

또한 Android 프로젝트에 대한 대상/컴파일 버전은 *가장 최근에 설치된 플랫폼을 사용*하도록 설정**되어야** 합니다. 그러나 최소 버전은 Android 4.4 이상을 사용하는 디바이스를 계속 지원할 수 있도록 API 19로 설정할 수 있습니다. 이러한 값은 **프로젝트 옵션**에서 설정합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

**프로젝트 옵션 &gt; 애플리케이션 &gt; 애플리케이션 속성**

![Visual Studio의 Android 빌드 옵션](requirements-images/options-android-vs-sml.png)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

**빌드 > 일반**

![최신 대상 프레임워크 선택](requirements-images/options-general-sml.png)

**빌드 &gt; Android 애플리케이션**

![앱에 대한 최소 및 대상 Android 버전을 선택하세요.](requirements-images/options-android-sml.png)

-----

## <a name="deprecated-platforms"></a>사용되지 않는 플랫폼

Xamarin.Forms 3.0 이상을 사용하는 경우 다음 플랫폼이 지원되지 않습니다.

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*
