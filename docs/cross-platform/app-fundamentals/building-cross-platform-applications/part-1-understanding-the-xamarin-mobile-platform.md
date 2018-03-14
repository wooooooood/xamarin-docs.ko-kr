---
title: "1 – 모바일 Xamarin 플랫폼 이해"
ms.topic: article
ms.prod: xamarin
ms.assetid: FBCEF258-D3D8-A420-79ED-3AAB4A7308E4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: d90cd8ae620f51d7ea9bac0945ccc7ec4b83bac4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2018
---
# <a name="part-1--understanding-the-xamarin-mobile-platform"></a>1 – 모바일 Xamarin 플랫폼 이해

Xamarin 플랫폼 iOS 및 Android 용 응용 프로그램을 개발할 수 있도록 하는 요소 수가 이루어져 있습니다.

-   **C# 언어** – 친숙 한 구문 및 제네릭, LINQ 및 병렬 작업 라이브러리와 같은 복잡 한 기능을 사용할 수 있습니다.
-   **.NET framework 모노** – Microsoft의.NET framework의 광범위 한 기능의 플랫폼 구현을 제공 합니다.
-   **컴파일러** -플랫폼에 따라 (예: 네이티브 응용 프로그램을 생성 합니다. iOS) 나 통합 된.NET 응용 프로그램 및 런타임 않음 (예: Android). 또한 컴파일러는 사용된 하지 않는 코드를 트래버스하여 연결과 같은 모바일 배포에 대 한 많은 최적화를 수행 합니다.
-   **IDE 도구** – Mac 및 Windows에서 Visual Studio를 사용 하면 만들기, 빌드 및 Xamarin 프로젝트를 배포할 수 있습니다.


또한.NET framework와 함께 기본 언어는 C#, 때문에 Windows Phone 배포할 수 있는 코드를 공유 프로젝트를 구조화할 수 수 있습니다.

 <a name="Under_the_Hood" />


## <a name="under-the-hood"></a>기본적인 이해

Xamarin을 사용 하면 C#에서 응용 프로그램을 작성 하 고 여러 플랫폼에서 동일한 코드를 공유할 수 있습니다, 하지만 실제 구현은 각 시스템에는 매우 다릅니다.

 <a name="Compilation" />


## <a name="compilation"></a>컴파일

C# 소스를 사용 하는 네이티브 응용 프로그램에는 각 플랫폼에서 매우 다른 방식:

-   **iOS** – C# 미리 시간 (AOT) 컴파일된 ARM 어셈블리 언어입니다. .NET framework 응용 프로그램 크기를 줄이기 위해 연결 하는 동안 제거 되 고 사용 하지 않는 클래스와 함께으로 포함 됩니다. Apple에서는 런타임 코드 생성 ios, 일부 언어 기능은 사용할 수 있도록 (참조 [Xamarin.iOS 제한](~/ios/internals/limitations.md) ).
-   **Android** – C#은 IL 컴파일되고 MonoVM + JIT'ing 함께 패키지 됩니다. 프레임 워크의 클래스를 사용 하지 않는 연결 하는 동안 제거 됩니다. 응용 프로그램 실행--와 함께 Java/아트 (Android 런타임) JNI 통해 네이티브 형식을 상호 작용 하 고 (참조 [Xamarin.Android 제한](~/android/internals/limitations.md) ).
-   **Windows** – C#는 IL 및 기본 제공 런타임에 의해 실행 컴파일되고 Xamarin 도구에 필요 하지 않습니다. Windows 응용 프로그램 디자인 다음 Xamarin 지침 간단 하 게 iOS 및 Android 코드 다시 사용 합니다.
  유니버설 Windows 플랫폼에는 한 **.NET 네이티브** Xamarin.iOS' AOT 컴파일 비슷하게 동작 하는 옵션입니다.


에 대 한 링커 설명서 [Xamarin.iOS](~/ios/deploy-test/linker.md) 및 [Xamarin.Android](~/android/deploy-test/linker.md) 컴파일 프로세스의이 부분에 대 한 자세한 정보를 제공 합니다.

런타임 '컴파일' – 동적으로 코드를 생성할 `System.Reflection.Emit` -피해 야 합니다.

IOS 장치에서 동적 코드 생성을 방지 하는 Apple의 커널, 따라서 즉시 코드를 내보내는 Xamarin.iOS에서 작동 하지 않습니다. 마찬가지로, 동적 언어 런타임 기능은 Xamarin 도구로 사용할 수 없습니다.

일부 리플렉션 기능도 않음 (예: 작업 수행 MonoTouch.Dialog 사용 하 여 리플렉션 API에 대 한), 하지 않을 코드를 생성 합니다.

 <a name="Platform_SDK_Access" />


## <a name="platform-sdk-access"></a>플랫폼 SDK 액세스

Xamarin에서는 친숙 한 C# 구문으로 쉽게 액세스할 수 있는 플랫폼 관련 SDK에서 제공 하는 기능을 수행 합니다.

-   **iOS** – Xamarin.iOS CocoaTouch SDK 프레임 워크 Apple의 C#에서 참조할 수 있는 네임 스페이스를 표시 합니다. 예를 들어 UIKit 프레임 워크 사용자 인터페이스 컨트롤을 포함 하는 간단한 함께 포함 될 수 `using UIKit;` 문.
-   **Android** –을 사용 하 여 지원 되는 SDK의 일부를 참조할 수 있도록 네임 스페이스,으로 Google의 Android SDK는 Xamarin.Android 노출 문와 같은 `using Android.Views;` 사용자 인터페이스 컨트롤에 액세스할 수 있습니다.
-   **Windows** – Windows 앱은 Visual Studio를 사용 하 여 Windows에서 빌드됩니다. 프로젝트 형식에는 Windows Forms, WPF, WinRT, 및 유니버설 Windows 플랫폼 (UWP) 포함 됩니다.


 <a name="Seamless_Integration_for_Developers" />


## <a name="seamless-integration-for-developers"></a>개발자를 위한의 원활한 통합

Xamarin의 장점은 하 내부적인 차이 있지만 Xamarin.iOS 및 Xamarin.Android (Microsoft의 Windows Sdk와 결합 되어) 하는 원활한 환경을 제공 세 플랫폼 모두에서 다시 사용할 수 있는 C# 코드를 작성 하기 위한입니다.

비즈니스 논리, 데이터베이스 사용량, 네트워크 액세스 및 기타 일반 함수는 한 번 쓸 및 모양과 네이티브 응용 프로그램으로 수행 하는 플랫폼 관련 사용자 인터페이스에 대 한 기반으로 제공 하는 각 플랫폼에서 다시 사용할 수 있습니다.

 <a name="Integrated_Development_Environment_(IDE)_Availability" />


## <a name="integrated-development-environment-ide-availability"></a>통합된 개발 환경 (IDE) 가용성

Windows 또는 Mac에서 Visual Studio에서 Xamarin 개발을 수행할 수 있습니다. 대상으로 할 IDE 선택한 플랫폼에 의해 결정 됩니다.

IOS 용 빌드를 위해 Windows에서 Windows 앱을 개발만 수 때문에 Android, _및_ Windows Visual Studio for Windows는 필요 합니다. 그러나 프로젝트 및 Mac에서 iOS 및 Android 앱을 빌드할 수 있으며 공유 코드 나중에 Windows 프로젝트에 추가할 수 있도록 Windows 및 Mac 컴퓨터 간에 파일을 공유할 수는 있습니다.

각 플랫폼에 대 한 개발 요구 사항에 보다 자세히 설명 된 [요구 사항](~/cross-platform/get-started/requirements.md) 가이드입니다.


<a name="iOS" />

### <a name="ios"></a>iOS

IOS 응용 프로그램 개발 macOS를 실행 하는 Mac 컴퓨터를 필요 합니다. 작성 하 고 Visual Studio에서 Xamarin으로 iOS 응용 프로그램을 배포 하려면 Visual Studio를 사용할 수도 있습니다. 그러나 Mac 빌드 및 라이선스 목적으로 여전히 필요 합니다.

Apple의 Xcode IDE 컴파일러 및 테스트에 대 한 시뮬레이터 제공 하기 위해 설치 되어야 합니다. 자신의 장치에서 테스트할 수 [무료로](~/ios/get-started/installation/device-provisioning/free-provisioning.md), 있지만 (예: 배포 응용 프로그램을 개발 하려면. 앱 스토어) Apple 개발자 프로그램 ($99 USD 1 년)를 조인 해야 합니다. 제출 하거나 응용 프로그램을 업데이트 될 때마다 그 해야 검토 하 고 다운로드가 제공 될 수 전에 Apple에서 승인 합니다.

Visual Studio IDE와 코드를 작성 하 고 화면 레이아웃을 프로그래밍 방식으로 빌드되 또는 Xamarin iOS IDE 중 하나에서 디자이너를 사용 하 여 편집할 수 있습니다.

참조는 [Xamarin.iOS 설치 가이드](~/ios/get-started/installation/index.md) 에 대 한 자세한 지침은 설정 합니다.

<a name="Android" />

### <a name="android"></a>Android

Android 응용 프로그램 개발 Java 및 Android Sdk를 설치 해야 합니다. 이러한 컴파일러, 에뮬레이터 및 빌드, 배포 및 테스트 하는 데 필요한 기타 도구를 제공 합니다. Java, Google의 Android SDK 및 Xamarin 도구가 모두 설치 및 실행할 수 있습니다 다음 구성에서:

-  Mac OS X El Capitan 이상 (10.11 이상) Mac 용 Visual Studio와 함께
-  Windows 7 및 위의 Visual Studio 2015 또는 2017


Xamarin 구성 하 여 시스템 필수 Java를 통해 Android 및 xamarin이 설치 도구 (화면 레이아웃에 대 한 비주얼 디자이너 포함) 통합된 설치 관리자를 제공 합니다. 참조는 [Xamarin.Android 설치 가이드](~/android/get-started/installation/index.md) 자세한 지침에 대 한 합니다.

그러나 만들고 Google에서 어떠한 사용권도 없이 실제 장치에서 응용 프로그램을 테스트할 수 있습니다 저장소를 통해 응용 프로그램 배포에 (예: Google Play, Amazon 또는 Barnes &amp; Noble) 등록 요금 운영자에 게 지급 수 있습니다. Apple의 유사한가 승인 프로세스를 보유 하는 다른 저장소 Google Play 응용 프로그램을 즉시 게시 합니다.

 <a name="Windows_Phone" />


### <a name="windows"></a>Windows

Windows 앱 (WinForms, WPF 또는 UWP)은 Visual Studio에 빌드됩니다. Xamarin를 직접 사용 하지 않습니다. 그러나 Windows, iOS 및 Android에서 C# 코드를 공유할 수 있습니다.
Microsoft의 방문 [개발자 센터](https://developer.microsoft.com/) Windows 개발 하는 데 필요한 도구에 대 한 자세한 내용은 합니다.

 <a name="Creating_the_User_Interface_(UI)" />


## <a name="creating-the-user-interface-ui"></a>UI(사용자 인터페이스) 만들기

Xamarin을 사용 하 여의 주요 이점은 응용 프로그램 사용자 인터페이스 Objective-c 또는 Java로 작성 된 응용 프로그램을 구별할 수 없는 앱을 만들고 각 플랫폼에 네이티브 컨트롤을 사용 한다는입니다 (iOS 및 Android 용 각각).

응용 프로그램에서 화면을 빌드할 때 코드에서 컨트롤을 배치 하거나 사용할 수 있는 디자인 도구를 사용 하 여 각 플랫폼에 대 한 전체 화면 만들기.

 <a name="Programmatically_Create_Controls" />


### <a name="create-controls-programmatically"></a>컨트롤을 프로그래밍 방식으로 만들기

각 플랫폼 사용자 인터페이스 컨트롤을 코드를 사용 하 여 화면에 추가할 수 있습니다. 완료 된 설계를 시각화 하기 어려울 수 시간이 아주 오래 소요 될 수 있습니다 컨트롤 위치 및 크기에 대 한 하드 코딩 픽셀을 조정 하는 경우.

프로그래밍 방식으로 컨트롤을 만드는 이점 하지만 크기를 조정 하거나 iPhone 및 iPad를 한 화면 크기에서 다르게 렌더링 하는 뷰를 작성 하기 위한 iOS에서 특히.

 <a name="Visual_Designer" />


### <a name="visual-designer"></a>비주얼 디자이너

각 플랫폼에는 화면으로 시각적으로 배치 하기 위한 다른 방법을:

-   **iOS** – Xamarin iOS 디자이너 뷰 끌어서 놓기 기능 및 속성 필드를 사용 하 여 작성을 용이 하 게 합니다. 이러한 뷰는 스토리 보드를 구성 하 고에서 액세스할 수 있습니다 전체적으로 **합니다. 스토리 보드** 프로젝트에 포함 되어 있는 파일입니다.
-   **Android** – Xamarin은 Visual Studio에 대 한 Android 끌어서 놓기 UI 디자이너를 제공 합니다. 로 저장 되는 android 화면 레이아웃 **합니다. AXML** Xamarin 도구를 사용 하는 경우 파일입니다.
-   **Windows** -Microsoft Visual Studio 및 Blend에서 끌어서 놓기 UI 디자이너를 제공 합니다. 화면 레이아웃으로 저장 됩니다. XAML 파일입니다.


각 플랫폼에서 사용할 수 있는 시각적 화면 디자이너를 표시 하는 이러한 스크린샷:

 [ ![](part-1-understanding-the-xamarin-mobile-platform-images/designer-all1.png "이러한 스크린샷 각 플랫폼에서 사용할 수 있는 시각적 화면 디자이너를 보여 줍니다.")](part-1-understanding-the-xamarin-mobile-platform-images/designer-all1.png#lightbox)

모든 경우에에서는 만든 요소를 시각적으로 코드에서 참조할 수 있습니다.

 <a name="User_Interface_Considerations" />


### <a name="user-interface-considerations"></a>사용자 인터페이스 고려 사항

플랫폼 간 응용 프로그램을 빌드하려면 Xamarin을 사용 하 여의 주요 이점은 사용자에 게 친숙 한 인터페이스를 제공 하 여 네이티브 UI 도구 키트의 장점은 취할 수 있는입니다. 또한 UI는 다른 네이티브 응용 프로그램 만큼 빠르게 수행 됩니다.

여러 플랫폼 간에 작동 일부 UI 요소 (예를 들어 세 플랫폼 모두 사용 하 여 비슷한 스크롤 목록 컨트롤) 있지만 '생각' 오른쪽에 응용 프로그램에 대 한 순서 대로 UI 활용 해야 플랫폼 관련 사용자 인터페이스 요소의 경우에 한 합니다. 다음과 같은 플랫폼 특정 UI 요소

-   **iOS** -화면 아래에서 소프트 뒤로 단추를 사용 하 여 계층적 탐색 탭 합니다.
-   **Android** – 하드웨어/시스템 소프트웨어 단추, 동작 메뉴, 화면 위쪽에 탭을 백업 합니다.
-   **Windows** – Windows 앱이 데스크톱, (예: Microsoft Surface) 태블릿 및 휴대폰에서 실행할 수 있습니다. Windows 10 장치에는 하드웨어 뒤로 단추 및 라이브 타일에 예를 들어 있을 수 있습니다.


대상으로 하는 플랫폼에 관련 된 디자인 지침을 읽어보는 것이 좋습니다.

-   **iOS** – [Apple Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html)
-   **Android** – [Google의 사용자 인터페이스 지침](http://developer.android.com/guide/practices/ui_guidelines/index.html)
-   **Windows** – [Windows에 대 한 사용자 환경 디자인 지침](https://developer.microsoft.com/en-us/windows/design)


 <a name="Library_and_Code_Re-use" />


## <a name="library-and-code-re-use"></a>라이브러리 및 코드 다시 사용 합니다.

Xamarin 플랫폼 기본적으로 각 플랫폼에 대해 작성 된 라이브러리의 통합 뿐 아니라 모든 플랫폼 간에 기존 C# 코드 다시 사용을 허용 합니다.

 <a name="C#_Source_and_Libraries" />


### <a name="c-source-and-libraries"></a>C# 소스 및 라이브러리

Xamarin 제품 C# 및.NET framework를 사용 하므로 기존 소스 코드 (둘 다 오픈 소스 및 사내 프로젝트)의 많은 Xamarin.iOS 또는 Xamarin.Android 프로젝트에서 다시 사용할 수 있습니다. Xamarin 솔루션에 소스 단순히 추가 수 종종 하 고 즉시 작동 합니다. 지원 되지 않는.NET framework 기능을 사용 하는 경우에 대해 몇 가지 변경 필요할 수 있습니다.

Xamarin.iOS 또는 Xamarin.Android에서 사용할 수 있는 C# 소스의 예로: SQLite NET, NewtonSoft.JSON 및 SharpZipLib 합니다.

 <a name="Objective-C_Bindings_+_Binding_Projects" />


### <a name="objective-c-bindings--binding-projects"></a>Objective C 바인딩 + 바인딩 프로젝트

라는 도구를 제공 하는 Xamarin *btouch* Objective-c 라이브러리 Xamarin.iOS 프로젝트에서 사용할 수를 허용 하는 바인딩을 만들 수 있도록 합니다. 참조는 [바인딩 Objective C 형식 설명서](~/cross-platform/macios/binding/binding-types-reference.md) 이 수행 되는 방법에 대 한 내용은 합니다.

Xamarin.iOS에 사용할 수 있는 Objective C 라이브러리의 예로: RedLaser 바코드 스캔 Google 웹 로그 분석 및 PayPal 통합 합니다. 사용할 수 있는 오픈 소스 Xamarin.iOS 바인딩을 [github](https://github.com/mono/monotouch-bindings)합니다.

 <a name=".jar_Bindings_+_Binding_Projects" />


### <a name="jar-bindings--binding-projects"></a>.jar 바인딩 + 바인딩 프로젝트

Xamarin은 Xamarin.Android에서 기존 Java 라이브러리를 사용 하 여 지원 합니다. 참조는 [Java 라이브러리 설명서 바인딩](~/android/platform/binding-java-library/index.md) 사용 하는 방법에 대 한 자세한 내용은 합니다. Xamarin.Android에서 JAR 파일입니다.

사용할 수 있는 오픈 소스 Xamarin.Android 바인딩을 [github](https://github.com/mono/monodroid-bindings)합니다.

 <a name="C_via_PInvoke" />


### <a name="c-via-pinvoke"></a>PInvoke 통해 C

관리 코드로 다시 "플랫폼 호출" 기술 (P/Invoke)에 관리 코드를 (C#)를 호출 하는 네이티브 라이브러리에 대 한 지원 뿐만 아니라 네이티브 라이브러리의 메서드를 호출할 수 있습니다.

예를 들어는 [SQLite NET](https://github.com/praeclarum/sqlite-net) 라이브러리는 다음과 같은 문을 사용 합니다.

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open", CallingConvention=CallingConvention.Cdecl)]
public static extern Result Open (string filename, out IntPtr db);
```

이 iOS 및 Android에서 native C 언어 SQLite 구현에 바인딩합니다.
기존 C API에 익숙한 개발자의 C# 클래스 네이티브 API에 매핑되고 기존 플랫폼 코드를 사용 하는 집합을 생성할 수 있습니다. 에 대 한 설명서가 [네이티브 라이브러리 연결](~/ios/platform/native-interop.md) Xamarin.iOS, Xamarin.Android에 유사한 원칙이 적용 됩니다.

 <a name="C++_via_Cxxi" />


### <a name="c-via-cppsharp"></a>CppSharp 통해 c + +

Miguel 설명 CXXI (이라고 [CppSharp](https://github.com/mono/CppSharp)) his에 [블로그](http://tirania.org/blog/archive/2011/Dec-19.html)합니다. C + + 라이브러리를 직접 바인딩 대신 C 래퍼를 만들고 바인딩하는 P/Invoke를 통해 되려고 합니다.

