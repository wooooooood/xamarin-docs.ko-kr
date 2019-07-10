---
title: 파트 1-Xamarin Mobile Platform 이해
description: 이 문서에서는 높은 수준에서 컴파일 프로세스, 플랫폼 SDK 액세스, 코드 공유, 사용자 인터페이스 만들기, 비주얼 디자이너 및 자세히 살펴보면 Xamarin 플랫폼을 설명 합니다.
ms.prod: xamarin
ms.assetid: FBCEF258-D3D8-A420-79ED-3AAB4A7308E4
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: c7c0f582ac4a7dc8571fbc607dba9b0ad97d49e1
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67674841"
---
# <a name="part-1--understanding-the-xamarin-mobile-platform"></a>파트 1-Xamarin Mobile Platform 이해

Xamarin 플랫폼을 iOS 및 Android 용 응용 프로그램을 개발할 수 있도록 하는 요소 수가 이루어져 있습니다.

-   **C#언어** – 익숙한 구문 및 제네릭, LINQ 및 병렬 작업 라이브러리와 같은 정교한 기능을 사용할 수 있습니다.
-   **Mono는.NET framework** – Microsoft.NET framework 기능의 광범위 한 플랫폼 간 구현을 제공 합니다.
-   **컴파일러** -플랫폼에 따라서는 네이티브 앱 (예: 생성 iOS) 또는 통합 된.NET 응용 프로그램 및 런타임 (예: Android)입니다. 컴파일러는 또한 자리 비움는 사용된 하지 않는 코드를 연결 하는 등 모바일 배포에 대 한 많은 최적화를 수행 합니다.
-   **IDE 도구** – Mac 및 Windows에서 Visual Studio를 사용 하면 만들기, 빌드 및 Xamarin 프로젝트를 배포할 수 있습니다.

또한 기본 언어가 C# .NET framework를 사용 하 여 프로젝트를 구조화할 수 있습니다도 Windows Phone 배포할 수 있는 코드를 공유 합니다.

## <a name="under-the-hood"></a>내부 살펴보기

Xamarin을 사용 하면 앱을 작성할 수 있지만 C#, 및 여러 플랫폼에서 동일한 코드를 공유, 각 시스템에 실제 구현은 매우 다릅니다.

## <a name="compilation"></a>컴파일

C# 원본이 각 플랫폼에서 매우 다른 방식에서으로 네이티브 앱에 수행 합니다.

-   **iOS** – C# 은 미리-의-시간 (AOT) ARM 어셈블리 언어로 컴파일됩니다. .NET framework 응용 프로그램 크기를 줄이기 위해 연결 하는 동안 제거 되 고 사용 하지 않는 클래스에 포함 됩니다. Apple 없도록 런타임 코드 생성 ios의 경우 몇 가지 언어 기능을 사용할 수 없는 하므로 (참조 [Xamarin.iOS 제한](~/ios/internals/limitations.md) ).
-   **Android** – C# IL로 컴파일되고 MonoVM + JIT'ing 함께 패키지 합니다. 프레임 워크에서 사용 되지 않는 클래스 링크 하는 동안 제거 됩니다. 응용 프로그램 실행-side-by-side Java/아트 (Android 런타임)을 사용 하 여 JNI 통해 네이티브 형식을 상호 작용 하 고 (참조 [Xamarin.Android 제한](~/android/internals/limitations.md) ).
-   **Windows** – C# 기본 제공 런타임에 의해 실행 되 고 Xamarin 도구는 필요 하지 않습니다 IL로 컴파일됩니다. Windows 응용 프로그램을 디자인 다음 Xamarin 지침 간편 하 게 다시 iOS 및 Android에서 코드를 사용 합니다.
  유니버설 Windows 플랫폼도 있는 참고를 **.NET 네이티브** Xamarin.iOS' AOT 컴파일 비슷하게 작동 하는 옵션입니다.


에 대 한 링커 설명서 [Xamarin.iOS](~/ios/deploy-test/linker.md) 하 고 [Xamarin.Android](~/android/deploy-test/linker.md) 컴파일 프로세스의이 부분에 대 한 자세한 정보를 제공 합니다.

런타임 '컴파일' – 동적으로 코드를 생성할 `System.Reflection.Emit` – 하지 않아야 합니다.

IOS 장치에서 동적 코드 생성을 방지 하는 Apple의 커널, 따라서 즉시 코드를 내보내는 Xamarin.iOS에서 작동 하지 않습니다. 마찬가지로, Xamarin 도구를 사용 하 여 동적 언어 런타임 기능을 사용할 수 없습니다.

일부 리플렉션 기능 (예: 작업 수행 MonoTouch.Dialog 사용 리플렉션 API에 대 한), 아니라 코드를 생성 합니다.

## <a name="platform-sdk-access"></a>플랫폼 SDK 액세스

Xamarin 쉽게 액세스할 수 있는 플랫폼별 SDK에서 제공 하는 기능을 사용 하면 친숙 한을 사용 하 여 C# 구문:

-   **iOS** – Xamarin.iOS에서 참조할 수 있는 네임 스페이스와 Apple SDK CocoaTouch 프레임 워크를 노출 C#합니다. 예를 들어 모든 사용자 인터페이스 컨트롤이 들어 있는 UIKit 프레임 워크는 간단한 포함할 수 있습니다 `using UIKit;` 문입니다.
-   **Android** – 모든 부분을 사용 하 여 지원 되는 SDK 참조할 수 있도록 네임 스페이스와 Google의 Android SDK는 Xamarin.Android 노출 문을 같은 `using Android.Views;` 사용자 인터페이스 컨트롤에 액세스할 수 있습니다.
-   **Windows** – Windows 앱은 Windows에서 Visual Studio를 사용 하 여 빌드됩니다. 프로젝트 형식에는 Windows Forms, WPF, WinRT, 및 Windows 플랫폼 (UWP (유니버설) 포함 됩니다.

## <a name="seamless-integration-for-developers"></a>개발자를 위한 원활한 통합

Xamarin의 장점은 내부적인 다르기는 하지만 Xamarin.iOS 및 Xamarin.Android (Microsoft의 Windows Sdk를 사용 하 여 결합 식) 제품 작성 하기 위한 원활한 환경을입니다 C# 코드를 세 플랫폼 모두에서 다시 사용할 수 있습니다.

비즈니스 논리, 데이터베이스 사용량, 네트워크 액세스 및 기타 일반 함수를 한 번 작성 고 다시 확인 하 고 네이티브 응용 프로그램으로 수행 하는 플랫폼별 사용자 인터페이스에 대 한 기초를 제공 하는 각 플랫폼에서 사용할 수 있습니다.

## <a name="integrated-development-environment-ide-availability"></a>통합된 개발 환경 (IDE) 가용성

Mac 또는 Windows에서 Visual Studio에서 Xamarin 개발을 수행할 수 있습니다. 대상으로 하려는 IDE 선택한 플랫폼에 의해 결정 됩니다.

Windows 앱, iOS 용으로 빌드하려면 Windows에서 개발할 수 있습니다 때문에 Android, _및_ Windows Windows에 대 한 Visual Studio에 필요 합니다. 그러나 프로젝트 및 Mac에서 iOS 및 Android 앱을 빌드할 수 있습니다 하 고 공유 코드는 Windows 프로젝트를 나중에 추가할 수 있도록 Windows 및 Mac 컴퓨터 간에 파일을 공유 하는 것이 같습니다.

각 플랫폼에 대 한 개발 요구 사항에 자세히 설명 합니다 [요구 사항](~/cross-platform/get-started/requirements.md) 가이드입니다.

### <a name="ios"></a>iOS

IOS 응용 프로그램을 개발 macOS를 실행 하는 Mac 컴퓨터에 필요 합니다. 또한 작성 하 고 Visual Studio에서 Xamarin 사용 하 여 iOS 응용 프로그램을 배포 하려면 Visual Studio를 사용할 수 있습니다. 그러나 Mac 빌드 및 라이선스 용도로 여전히 필요 합니다.

Apple의 Xcode IDE 컴파일러 및 시뮬레이터 테스트용 제공 되어야 합니다. 사용자 고유의 장치에서 테스트할 수 있습니다 [무료로](~/ios/get-started/installation/device-provisioning/free-provisioning.md), 있지만 (예: 배포에 대 한 응용 프로그램을 빌드할 수 앱 스토어) Apple 개발자 프로그램 ($99 USD 연간)를 조인 해야 합니다. 제출 하거나 응용 프로그램을 업데이트 될 때마다가 검토와 다운로드 하는 고객에 게 제공 하기 전에 Apple에서 승인 합니다.

Visual Studio IDE를 사용 하 여 코드를 작성 하 고 화면 레이아웃을 프로그래밍 방식으로 작성 하거나 Xamarin iOS IDE 중 하나에서 디자이너를 사용 하 여 편집할 수 있습니다.

참조를 [Xamarin.iOS 설치 가이드](~/ios/get-started/installation/index.md) 설정에 관한 자세한 지침입니다.

### <a name="android"></a>Android

Android 응용 프로그램 개발에는 Java 및 Android Sdk 설치에 필요 합니다. 이러한 컴파일러, 에뮬레이터 및 빌드, 배포 및 테스트에 필요한 다른 도구를 제공 합니다. Java, Google의 Android SDK 및 Xamarin 도구 수 모두 설치 하 고 Windows 및 macOS에서 실행 합니다. 다음 구성은 사용 하는 것이 좋습니다.

- Visual Studio 2019를 사용 하 여 Windows 10
- macOS Mac 용 Visual Studio 2019를 사용 하 여 Mojave (10.11 이상)

Xamarin 구성 하는 시스템 필수 구성 요소는 Java를 사용 하 여 Android 및 Xamarin 도구 (화면 레이아웃을 위한 비주얼 디자이너가 포함)를 통합된 설치 관리자를 제공 합니다. 참조를 [Xamarin.Android 설치 가이드](~/android/get-started/installation/index.md) 자세한 지침입니다.

빌드 하 고 있지만 Google에서 라이선스 없이 실제 장치에서 응용 프로그램을 테스트할 수 있습니다 저장소를 통해 응용 프로그램을 배포 하려면 (Google Play, Amazon 등 Barnes &amp; Noble) 등록 요금 운영자에 게 지급 수 있습니다. Google Play 다른 저장소 Apple 비슷합니다 승인 프로세스는 즉시 앱을 게시 합니다.

### <a name="windows"></a>Windows

Windows 앱 (WinForms, WPF 또는 UWP)은 Visual Studio를 사용 하 여 빌드됩니다. 직접 Xamarin 사용 하지 않습니다. 그러나 C# Windows, iOS 및 Android에서 코드를 공유할 수 있습니다.
Microsoft의 방문 [개발자 센터](https://developer.microsoft.com/) Windows 개발에 필요한 도구에 대해 자세히 알아보려면 합니다.

## <a name="creating-the-user-interface-ui"></a>UI(사용자 인터페이스) 만들기

Xamarin을 사용 하는 주요 이점은 응용 프로그램 사용자 인터페이스 Objective-c 또는 Java로 작성 된 응용 프로그램에서 구분 되지 않는 앱을 만들고 각 플랫폼에서 네이티브 컨트롤을 사용 한다는입니다 (iOS 및 Android 용 각각).

앱에서 화면을 빌드하는 경우 코드에서 컨트롤을 배치 하거나 사용할 수 있는 디자인 도구를 사용 하 여 각 플랫폼에 대 한 전체 화면을 만듭니다.

### <a name="create-controls-programmatically"></a>컨트롤을 프로그래밍 방식으로 만들기

각 플랫폼 사용자 인터페이스 컨트롤을 코드를 사용 하 여 화면에 추가할 수 있습니다. 완료 된 디자인을 시각화 하기 어려울 수 있습니다 매우 많은 시간이 소요 될 수 있습니다이 하드 코딩 픽셀 컨트롤 위치 및 크기 조정 하는 경우.

프로그래밍 방식으로 컨트롤을 만들기는 혜택 그러나 크기를 조정 하거나 iPhone 및 iPad 화면 크기에서 다르게 렌더링 하는 뷰를 구축 하기 위한 iOS에서 특히.

### <a name="visual-designer"></a>비주얼 디자이너

각 플랫폼에는 다른 방법을 화면에 시각적으로 레이아웃:

- **iOS** – Xamarin iOS 디자이너 끌어서 놓기 기능 및 속성 필드를 사용 하 여 보기의 구축을 지원 합니다. 이러한 뷰는 스토리 보드를 구성 및 액세스할 수 있습니다 전체적으로 **합니다. 스토리 보드** 프로젝트에 포함 된 파일입니다.
- **Android** – Xamarin for Visual Studio Android 끌어서 놓기 UI 디자이너를 제공 합니다. Android 화면 레이아웃으로 저장 됩니다 **합니다. AXML** Xamarin 도구를 사용 하는 경우 파일입니다.
- **Windows** -Microsoft Visual Studio 및 Blend에서 끌어서 놓기 UI 디자이너를 제공 합니다. 화면 레이아웃으로 저장 됩니다. XAML 파일입니다.

이러한 스크린샷은 각 플랫폼에서 사용할 수 있는 시각적 화면 디자이너를 표시합니다.

 [![](understanding-the-xamarin-mobile-platform-images/designer-all1.png "이러한 스크린샷은 각 플랫폼에서 사용할 수 있는 시각적 화면 디자이너를 보여 줍니다.")](understanding-the-xamarin-mobile-platform-images/designer-all1.png#lightbox)

모든 경우에는 만든 요소를 시각적으로 코드에서 참조할 수 있습니다.

### <a name="user-interface-considerations"></a>사용자 인터페이스 고려 사항

Xamarin을 사용 하 여 플랫폼 간 응용 프로그램을 빌드하는 데 필요한 주요 이점은 사용자에 게 친숙 한 인터페이스를 제공 하는 네이티브 UI 도구 키트의 이점은 취할 수 있는 경우 또한 UI는 다른 네이티브 응용 프로그램 빨리 수행 됩니다.

여러 플랫폼 간에 UI 기능 일부 작업 (예를 들어, 세 플랫폼 모두 유사한 스크롤-목록 컨트롤을 사용) 하지만 응용 프로그램 '' 처럼 편안한 느낌을 위해에서 UI를 활용 해야 플랫폼별 사용자 인터페이스 요소를 해당 하는 경우. 다음과 같은 플랫폼별 UI 요소

- **iOS** – 소프트 뒤로 단추를 사용 하 여 계층적 탐색 화면 아래쪽에서 탭 합니다.
- **Android** – 하드웨어/시스템 소프트웨어 백 단추, 동작 메뉴, 화면 맨 위에 있는 탭입니다.
- **Windows** – Windows 앱은 데스크톱, (예: Microsoft Surface) 태블릿 및 휴대폰에서 실행할 수 있습니다. Windows 10 장치에는 하드웨어 뒤로 단추 및 라이브 타일에 예를 들어 있을 수 있습니다.

대상으로 하는 플랫폼에 관련 된 디자인 지침을 참조 하는 것이 좋습니다.

- **iOS** – [Apple Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html)
- **Android** – [Google의 사용자 인터페이스 지침](https://developer.android.com/guide/practices/ui_guidelines/index.html)
- **Windows** – [Windows에 대 한 사용자 환경 디자인 지침](https://developer.microsoft.com/windows/design)

## <a name="library-and-code-re-use"></a>라이브러리 및 코드 다시 사용 합니다.

Xamarin 플랫폼 기존 다시 사용할 수 있습니다. C# 각 플랫폼에 대해 고유 하 게 작성 된 라이브러리의 통합 뿐 아니라 모든 플랫폼 간에 코드입니다.

### <a name="c-source-and-libraries"></a>C#원본 및 라이브러리

Xamarin 제품은 사용 하기 때문에 C# .NET framework에서 기존 소스 코드 (모두 오픈 소스 및 사내 프로젝트)의 많은 Xamarin.iOS 또는 Xamarin.Android 프로젝트에서 다시 사용할 수 있습니다. 종종 원본 간단히 추가할 수 있습니다 Xamarin 솔루션을 및 즉시 작동 합니다. 지원 되지 않는.NET framework 기능을 모두 사용 된 경우를 일부 조정 해야 할 수도 있습니다.

예 C# Xamarin.iOS 또는 Xamarin.Android에서 사용할 수 있는 원본을 포함 합니다. SQLite-NET NewtonSoft.JSON 하며 SharpZipLib 합니다.

### <a name="objective-c-bindings--binding-projects"></a>Objective-c 바인딩 + 바인딩 프로젝트

Xamarin은 라는 도구를 제공 *u c h* 통해 Objective-c 라이브러리를 Xamarin.iOS 프로젝트에서 사용할 수 있도록 바인딩을 만든 합니다. 참조를 [Objective-c 바인딩 형식 설명서](~/cross-platform/macios/binding/binding-types-reference.md) 이 수행 되는 방법에 대 한 내용은 합니다.

Xamarin.iOS에서 사용할 수 있는 Objective-c 라이브러리의 예입니다. 검색, google 웹 로그 분석 및 PayPal 통합 RedLaser 바코드입니다. 사용할 수 있는 오픈 소스 Xamarin.iOS 바인딩을 [github](https://github.com/mono/monotouch-bindings)합니다.

### <a name="jar-bindings--binding-projects"></a>.jar 바인딩 + 바인딩 프로젝트

Xamarin.Android에서 기존 Java 라이브러리를 사용 하 여 Xamarin 지원 합니다. 참조를 [Java 라이브러리 설명서를 바인딩](~/android/platform/binding-java-library/index.md) 사용 하는 방법에 대 한 내용은 합니다. Xamarin.Android에서 JAR 파일입니다.

사용할 수 있는 오픈 소스 Xamarin.Android 바인딩 [github](https://github.com/mono/monodroid-bindings)합니다.

### <a name="c-via-pinvoke"></a>PInvoke 통해 C

"플랫폼 호출" 기술 (P/Invoke) 관리 되는 코드를 수 있습니다 (C#) 관리 코드로 다시 호출 네이티브 라이브러리에 대 한 지원 뿐만 아니라 네이티브 라이브러리의 메서드를 호출 합니다.

예를 들어, 합니다 [SQLite NET](https://github.com/praeclarum/sqlite-net) 라이브러리는 다음과 같은 문을 사용 합니다.

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open", CallingConvention=CallingConvention.Cdecl)]
public static extern Result Open (string filename, out IntPtr db);
```

이 iOS 및 Android에서 native C 언어 SQLite 구현에 바인딩합니다.
기존 C API를 사용 하 여 친숙 한 개발자는 집합을 생성할 수 있습니다 C# 네이티브 API에 매핑 기존 플랫폼 코드를 활용 하는 클래스입니다. 에 대 한 설명서는 [네이티브 라이브러리 연결](~/ios/platform/native-interop.md) Xamarin.iOS, Xamarin.Android에 유사한 원칙이 적용 됩니다.

### <a name="c-via-cppsharp"></a>C++CppSharp 통해

Miguel CXXI 설명 (현재의 [CppSharp](https://github.com/mono/CppSharp))에서 자신의 [블로그](https://tirania.org/blog/archive/2011/Dec-19.html)합니다. 바인딩할 대신은 C++ 라이브러리는 C 래퍼를 만들고에 바인딩하는 P/Invoke를 통해 직접는 합니다.
