---
title: 1 부-Xamarin Mobile Platform 이해
description: 이 문서는 높은 수준의 Xamarin 플랫폼을 설명 하 고, 컴파일 프로세스, 플랫폼 SDK 액세스, 코드 공유, 사용자 인터페이스 만들기, 비주얼 디자이너 등을 살펴봅니다.
ms.prod: xamarin
ms.assetid: FBCEF258-D3D8-A420-79ED-3AAB4A7308E4
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: af2b8cd39d5fb1b0ce6c12f7d6ad87e245b9a594
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761965"
---
# <a name="part-1--understanding-the-xamarin-mobile-platform"></a>1 부-Xamarin Mobile Platform 이해

Xamarin 플랫폼은 iOS 및 Android 용 응용 프로그램을 개발 하는 데 사용할 수 있는 여러 요소로 구성 됩니다.

- language – 친숙 한 구문과 제네릭, LINQ 및 병렬 작업 라이브러리와 같은 고급 기능을 사용할 수 있습니다. **C#**
- **Mono .net framework** – Microsoft .net framework의 다양 한 기능에 대 한 플랫폼 간 구현을 제공 합니다.
- **컴파일러** – 플랫폼에 따라 네이티브 앱을 생성 합니다 (예: iOS) 또는 통합 된 .NET 응용 프로그램 및 런타임 (예: Android). 또한 컴파일러는 사용 되지 않은 코드를 연결 하는 것과 같은 모바일 배포에 대해 많은 최적화를 수행 합니다.
- **IDE 도구** – Mac 및 Windows의 Visual Studio를 사용 하 여 Xamarin 프로젝트를 생성, 빌드 및 배포할 수 있습니다.

또한 기본 언어는 .NET framework를 C# 사용 하기 때문에 Windows Phone에 배포 될 수 있는 코드를 공유 하도록 프로젝트를 구조화 할 수 있습니다.

## <a name="under-the-hood"></a>내부적으로

Xamarin을 사용 하 여에서 C#앱을 작성 하 고 동일한 코드를 여러 플랫폼에서 공유할 수 있지만 각 시스템의 실제 구현은 매우 다릅니다.

## <a name="compilation"></a>컴파일

소스 C# 는 각 플랫폼에서 매우 다양 한 방식으로 네이티브 앱에 사용 됩니다.

- **iOS** – C# ARM 어셈블리 언어로 AOT (사전)로 컴파일됩니다. 응용 프로그램 크기를 줄이기 위해 연결 하는 동안 사용 되지 않은 클래스를 제거 하는 .NET framework가 포함 되어 있습니다. Apple은 iOS에서 런타임 코드 생성을 허용 하지 않으므로 일부 언어 기능을 사용할 수 없습니다 ( [Xamarin.ios 제한 사항](~/ios/internals/limitations.md) 참조).
- **Android** – C# IL로 컴파일되고 MonoVM + JIT'ing로 패키지 됩니다. 프레임 워크에서 사용 되지 않는 클래스는 연결 중에 제거 됩니다. 응용 프로그램은 Java/ART (Android 런타임)와 나란히 실행 되며 JNI를 통해 네이티브 형식과 상호 작용 합니다 ( [Xamarin.ios 제한](~/android/internals/limitations.md) 참조).
- **Windows** – C# IL로 컴파일되고 기본 제공 런타임에 의해 실행 되며 Xamarin 도구가 필요 하지 않습니다. Xamarin의 지침에 따라 Windows 응용 프로그램을 디자인 하면 iOS 및 Android에서 코드를 보다 간단 하 게 다시 사용할 수 있습니다.
  또한 유니버설 Windows 플랫폼에는 Xamarin.ios ' AOT 컴파일과 유사 하 게 작동 하는 **.NET 네이티브** 옵션이 있습니다.

[Xamarin.ios](~/ios/deploy-test/linker.md) 및 [xamarin.ios](~/android/deploy-test/linker.md) 에 대 한 링커 설명서는 컴파일 프로세스의이 부분에 대 한 자세한 정보를 제공 합니다.

런타임 ' 컴파일 ' –를 사용 하 여 `System.Reflection.Emit` 동적으로 코드를 생성 하는 것은 피해 야 합니다.

Apple 커널을 사용 하면 iOS 장치에서 동적 코드를 생성할 수 없으므로 Xamarin.ios에서는 코드를 즉시 내보낼 수 없습니다. 마찬가지로 동적 언어 런타임 기능은 Xamarin 도구와 함께 사용할 수 없습니다.

일부 리플렉션 기능이 작동 합니다 (예: Monotouch.dialog은 코드를 생성 하는 것이 아니라 리플렉션 API에 사용 됩니다.

## <a name="platform-sdk-access"></a>플랫폼 SDK 액세스

Xamarin을 사용 하면 플랫폼별 SDK에서 제공 하는 기능을 친숙 C# 한 구문으로 쉽게 액세스할 수 있습니다.

- **ios** – Xamarin.ios는 Apple의 CocoaTouch SDK 프레임 워크를 참조할 수 있는 네임 스페이스로 노출 C#합니다. 예를 들어 모든 사용자 인터페이스 컨트롤을 포함 하는 uikit 프레임 워크를 간단한 `using UIKit;` 문에 포함할 수 있습니다.
- **Android** – Xamarin android는 Google의 Android SDK를 네임 스페이스로 노출 하므로 사용자 인터페이스 컨트롤에 액세스 `using Android.Views;` 하는 등의 using 문을 사용 하 여 지원 되는 SDK의 모든 부분을 참조할 수 있습니다.
- **Windows – windows** 응용 프로그램은 Windows에서 Visual Studio를 사용 하 여 빌드됩니다. 프로젝트 형식에는 Windows Forms, WPF, WinRT 및 UWP (유니버설 Windows 플랫폼)가 포함 됩니다.

## <a name="seamless-integration-for-developers"></a>개발자를 위한 원활한 통합

Xamarin의 장점에도 불구 하 고, Xamarin.ios 및 Xamarin.ios (Microsoft의 Windows Sdk와 함께 제공)의 차이점에도 불구 하 고, 세 플랫폼 모두에서 다시 C# 사용할 수 있는 코드를 작성 하기 위한 원활한 환경을 제공 합니다.

비즈니스 논리, 데이터베이스 사용, 네트워크 액세스 및 기타 일반 기능을 한 번 작성 하 여 각 플랫폼에서 다시 사용 하 여 네이티브 응용 프로그램으로 표시 하 고 수행 하는 플랫폼별 사용자 인터페이스에 대 한 기초를 제공할 수 있습니다.

## <a name="integrated-development-environment-ide-availability"></a>IDE (통합 개발 환경) 가용성

Visual Studio의 Mac 또는 Windows에서 Xamarin 개발을 수행할 수 있습니다. 선택한 IDE는 대상 플랫폼에 따라 결정 됩니다.

Windows 앱은 windows 에서만 개발할 수 있으므로 iOS, Android _및_ windows 용으로 빌드하려면 Windows 용 Visual Studio가 필요 합니다. 그러나 Windows 및 Mac 컴퓨터 간에 프로젝트와 파일을 공유할 수 있으므로 Mac에서 iOS 및 Android 앱을 빌드할 수 있으며 나중에 Windows 프로젝트에 공유 코드를 추가할 수 있습니다.

각 플랫폼에 대 한 개발 요구 사항은 [요구 사항](~/cross-platform/get-started/requirements.md) 가이드에 자세히 설명 되어 있습니다.

### <a name="ios"></a>iOS

IOS 응용 프로그램을 개발 하려면 macOS를 실행 하는 Mac 컴퓨터가 필요 합니다. Visual Studio를 사용 하 여 Visual Studio에서 Xamarin을 사용 하 여 iOS 응용 프로그램을 작성 하 고 배포할 수도 있습니다. 그러나 Mac은 빌드 및 라이선스를 위해 여전히 필요 합니다.

테스트에 대 한 컴파일러 및 시뮬레이터를 제공 하려면 Apple의 Xcode IDE가 설치 되어 있어야 합니다. 자신의 장치에서 [무료로](~/ios/get-started/installation/device-provisioning/free-provisioning.md)테스트할 수 있지만 배포를 위해 응용 프로그램을 빌드할 수 있습니다 (예: 앱 스토어) Apple의 개발자 프로그램 (연간 $99 USD)에 가입 해야 합니다. 응용 프로그램을 제출 하거나 업데이트할 때마다 고객이 다운로드 하는 데 사용할 수 있도록 하려면 먼저 Apple에서 해당 응용 프로그램을 검토 하 고 승인 해야 합니다.

Visual Studio IDE를 사용 하 여 코드를 작성 하 고, 화면 레이아웃을 프로그래밍 방식으로 작성 하거나, 두 IDE에서 Xamarin의 iOS Designer를 사용 하 여 편집할 수 있습니다.

설정 하는 방법에 대 한 자세한 지침은 [Xamarin.ios 설치 가이드](~/ios/get-started/installation/index.md) 를 참조 하세요.

### <a name="android"></a>Android

Android 응용 프로그램을 개발 하려면 Java 및 Android Sdk를 설치 해야 합니다. 이는 빌드, 배포 및 테스트에 필요한 컴파일러, 에뮬레이터 및 기타 도구를 제공 합니다. Java, Google의 Android SDK 및 Xamarin 도구는 모두 Windows 및 macOS에 설치 하 고 실행할 수 있습니다. 권장 되는 구성은 다음과 같습니다.

- Visual Studio 2019을 사용 하는 Windows 10
- Mac 용 Visual Studio 2019을 사용 하는 macOS Mojave (10.11 +)

Xamarin은 필수 Java, Android 및 Xamarin 도구 (화면 레이아웃에 대 한 비주얼 디자이너 포함)를 사용 하 여 시스템을 구성 하는 통합 설치 관리자를 제공 합니다. 자세한 지침은 [Xamarin Android 설치 가이드](~/android/get-started/installation/index.md) 를 참조 하세요.

Google의 라이선스 없이 실제 장치에서 응용 프로그램을 빌드 및 테스트할 수 있지만 스토어 (예: Google Play, Amazon 또는 barnes and &amp; Noble)를 통해 응용 프로그램을 배포 하기 위해 등록 요금은 운영자에 게 지불 될 수 있습니다. Google Play는 앱을 즉시 게시 하 고, 다른 매장에는 Apple과 비슷한 승인 프로세스가 있습니다.

### <a name="windows"></a>Windows

Windows 앱 (WinForms, WPF 또는 UWP)은 Visual Studio를 사용 하 여 빌드됩니다. Xamarin을 직접 사용 하지 않습니다. 그러나 Windows C# , IOS 및 Android에서 코드를 공유할 수 있습니다.
Microsoft [개발자 센터](https://developer.microsoft.com/) 를 방문 하 여 Windows 개발에 필요한 도구에 대해 알아보세요.

## <a name="creating-the-user-interface-ui"></a>UI(사용자 인터페이스) 만들기

Xamarin을 사용 하는 경우의 주요 혜택은 응용 프로그램 사용자 인터페이스가 각 플랫폼에서 네이티브 컨트롤을 사용 하 여 목표-C 또는 Java (각각 iOS 및 Android 용)로 작성 된 응용 프로그램에서 구별할 수 없는 앱을 만드는 것입니다.

앱에서 화면을 작성 하는 경우 각 플랫폼에 사용할 수 있는 디자인 도구를 사용 하 여 코드에서 컨트롤을 레이아웃 하거나 전체 화면을 만들 수 있습니다.

### <a name="create-controls-programmatically"></a>프로그래밍 방식으로 컨트롤 만들기

각 플랫폼에서 코드를 사용 하 여 사용자 인터페이스 컨트롤을 화면에 추가할 수 있습니다. 이는 컨트롤 위치 및 크기에 대 한 픽셀 좌표를 하드 코딩 하는 경우 완성 된 디자인을 시각화 하기 어려울 수 있기 때문에 시간이 많이 걸릴 수 있습니다.

특히 iPhone 및 iPad 화면 크기를 다르게 조정 하거나 크기를 조정 하는 보기를 빌드하기 위한 iOS의 경우 프로그래밍 방식으로 컨트롤을 만들면 이점이 있습니다.

### <a name="visual-designer"></a>비주얼 디자이너

각 플랫폼에는 화면 레이아웃을 시각적으로 레이아웃 하는 다른 방법이 있습니다.

- **ios** – Xamarin의 ios 디자이너는 끌어서 놓기 기능 및 속성 필드를 사용 하 여 뷰를 쉽게 작성할 있습니다. 전체적으로 이러한 뷰는 스토리 보드를 구성 하며에서 액세스할 수 있습니다 **.** 프로젝트에 포함 된 스토리 보드 파일입니다.
- **Android** – Xamarin은 Visual Studio 용 android 끌어서 놓기 UI 디자이너를 제공 합니다. Android 화면 레이아웃은로 저장 됩니다 **.** Xamarin tools를 사용 하는 경우 xml 파일
- **Windows** – Microsoft는 Visual Studio 및 Blend에서 끌어서 놓기 UI 디자이너를 제공 합니다. 화면 레이아웃은로 저장 됩니다. XAML 파일.

이러한 스크린샷에는 각 플랫폼에서 사용할 수 있는 시각적 화면 디자이너가 나와 있습니다.

 [![](understanding-the-xamarin-mobile-platform-images/designer-all1.png "이러한 스크린샷에는 각 플랫폼에서 사용할 수 있는 시각적 화면 디자이너가 나와 있습니다.")](understanding-the-xamarin-mobile-platform-images/designer-all1.png#lightbox)

모든 경우에서 시각적으로 만드는 요소를 코드에서 참조할 수 있습니다.

### <a name="user-interface-considerations"></a>사용자 인터페이스 고려 사항

Xamarin을 사용 하 여 플랫폼 간 응용 프로그램을 빌드하는 주요 이점은 네이티브 UI 도구 키트를 활용 하 여 사용자에 게 친숙 한 인터페이스를 제공할 수 있다는 것입니다. 또한 UI는 다른 네이티브 응용 프로그램 만큼 빠르게 수행 됩니다.

일부 UI 메타포는 여러 플랫폼에서 작동 합니다. 예를 들어 세 플랫폼 모두 유사한 스크롤 목록 컨트롤을 사용 합니다. 그러나 응용 프로그램에서 ' 느낌 '이 되도록 하려면 적절 한 경우 UI에서 플랫폼별 사용자 인터페이스 요소를 활용 해야 합니다. 플랫폼별 UI 메타포의 예는 다음과 같습니다.

- **iOS** – 화면 아래쪽에 있는 탭, 소프트 뒤로 단추를 사용 하 여 계층적 탐색
- **Android** – 하드웨어/시스템-소프트웨어 뒤로 단추, 작업 메뉴, 화면 맨 위에 있는 탭
- **Windows** – windows 앱은 데스크톱, 태블릿 (예: Microsoft Surface) 및 휴대폰에서 실행할 수 있습니다. 예를 들어 Windows 10 장치에는 하드웨어 뒤로 단추와 라이브 타일이 있을 수 있습니다.

대상 플랫폼과 관련 된 디자인 지침을 확인 하는 것이 좋습니다.

- **iOS** – [Apple의 휴먼 인터페이스 지침](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html)
- **Android** - [Google의 사용자 인터페이스 지침](https://developer.android.com/guide/practices/ui_guidelines/index.html)
- **Windows** - [windows에 대 한 사용자 환경 디자인 지침](https://developer.microsoft.com/windows/design)

## <a name="library-and-code-re-use"></a>라이브러리 및 코드 다시 사용

Xamarin 플랫폼을 사용 하면 모든 플랫폼에서 기존 C# 코드를 다시 사용할 수 있을 뿐만 아니라 각 플랫폼에 대해 기본적으로 작성 된 라이브러리를 통합할 수 있습니다.

### <a name="c-source-and-libraries"></a>C#원본 및 라이브러리

Xamarin 제품은 및 .NET C# framework를 사용 하기 때문에 많은 기존 소스 코드 (오픈 소스 및 사내 프로젝트 모두)를 Xamarin.ios 또는 xamarin Android 프로젝트에서 다시 사용할 수 있습니다. 일반적으로 소스는 Xamarin 솔루션에 추가 될 수 있으며 즉시 작동 합니다. 지원 되지 않는 .NET framework 기능이 사용 된 경우 약간의 조정 기능이 필요할 수 있습니다.

Xamarin.ios 또는 C# xamarin에서 사용할 수 있는 원본의 예는 다음과 같습니다. SQLite-NET, Newtonsoft.json 및 SharpZipLib.

### <a name="objective-c-bindings--binding-projects"></a>목표-C 바인딩 + 바인딩 프로젝트

Xamarin은 Xamarin.ios 프로젝트에서 목표 C 라이브러리를 사용할 수 있도록 하는 바인딩을 만드는 데 도움이 되는 *btouch* 라는 도구를 제공 합니다. 이 작업을 수행 하는 방법에 대 한 자세한 내용은 [바인딩 목표-C 형식 설명서](~/cross-platform/macios/binding/binding-types-reference.md) 를 참조 하세요.

Xamarin.ios에서 사용할 수 있는 목적-C 라이브러리의 예는 다음과 같습니다. RedLaser 바코드 스캔, Google 분석 및 PayPal 통합. [Github](https://github.com/mono/monotouch-bindings)에서 오픈 소스 xamarin.ios 바인딩을 사용할 수 있습니다.

### <a name="jar-bindings--binding-projects"></a>.jar 바인딩 + 바인딩 프로젝트

Xamarin은 Xamarin.ios에서 기존 Java 라이브러리를 사용 하도록 지원 합니다. 을 사용 하는 방법에 대 한 자세한 내용은 [Java 라이브러리 바인딩 설명서](~/android/platform/binding-java-library/index.md) 를 참조 하세요. Xamarin Android의 JAR 파일입니다.

[Github](https://github.com/mono/monodroid-bindings)에서 오픈 소스 Xamarin Android 바인딩을 사용할 수 있습니다.

### <a name="c-via-pinvoke"></a>PInvoke를 통한 C

"플랫폼 호출" 기술 (P/Invoke)을 통해 관리 코드C#()는 네이티브 라이브러리의 메서드를 호출 하 고 네이티브 라이브러리에서 관리 코드로 다시 호출할 수 있도록 지원 합니다.

예를 들어 [SQLite-NET](https://github.com/praeclarum/sqlite-net) library는 다음과 같은 문을 사용 합니다.

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open", CallingConvention=CallingConvention.Cdecl)]
public static extern Result Open (string filename, out IntPtr db);
```

이는 iOS 및 Android의 네이티브 C 언어 SQLite 구현에 바인딩됩니다.
기존 C API에 익숙한 개발자는 C# 클래스 집합을 생성 하 여 네이티브 API에 매핑하고 기존 플랫폼 코드를 활용할 수 있습니다. Xamarin.ios에서 [네이티브 라이브러리를 연결](~/ios/platform/native-interop.md) 하는 방법에 대 한 설명서가 있습니다. xamarin에도 유사한 원칙이 적용 됩니다.

### <a name="c-via-cppsharp"></a>C++via CppSharp

Miel el은 [블로그의](https://tirania.org/blog/archive/2011/Dec-19.html)CXXI (현재 [CppSharp](https://github.com/mono/CppSharp))에 대해 설명 합니다. C++ 라이브러리에 직접 바인딩하는 대신 C 래퍼를 만들어 P/Invoke를 통해 바인딩할 수 있습니다.
