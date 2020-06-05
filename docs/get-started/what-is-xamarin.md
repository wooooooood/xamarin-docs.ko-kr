---
title: “Xamarin이란?”
description: “이 문서에서는 Xamarin 플랫폼 및 관련 라이브러리를 소개합니다.”
ms.prod: xamarin ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6 ms.custom: video author: profexorgeek ms.author: jusjohns ms.date: 05/28/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="what-is-xamarin"></a>Xamarin이란?

[![iOS 및 Android의 Xamarin 애플리케이션 사례 스크린샷](what-is-xamarin-images/xamarin-app-cropped.png)](what-is-xamarin-images/xamarin-app.png#lightbox)

Xamarin은 .NET.으로 iOS, Android 및 Windows용 최신 고성능 애플리케이션을 빌드하기 위한 오픈 소스 플랫폼입니다. Xamarin은 일종의 추상화 계층으로서 공유 코드와 기본 플랫폼 코드의 통신을 관리합니다. Xamarin은 메모리 할당 및 가비지 수집 같은 편의를 제공하는 관리형 환경에서 실행됩니다.

Xamarin은 개발자가 자신의 애플리케이션을 플랫폼 전반에 걸쳐 평균 90%나 공유하는 데 있어 큰 도움이 됩니다. 이러한 패턴에 힘입어 개발자는 모든 비즈니스 논리를 단일 언어로(또는 기존의 애플리케이션 코드를 재사용하여) 작성하면서도 플랫폼마다 네이티브한 성능과 모양, 느낌을 실현할 수 있습니다.

Xamarin 애플리케이션은 PC 또는 Mac에 작성되고 Android의 **.apk** 파일 또는 iOS의 **.ipa** 파일 등의 네이티브 애플리케이션 패키지로 컴파일될 수 있습니다.

> [!NOTE]
> 현재 iOS용 애플리케이션을 컴파일 및 배포하려면 macOS 컴퓨터가 필요합니다. 배포 요구 사항에 대한 자세한 내용은 [시스템 요구 사항](~/cross-platform/get-started/requirements.md#macos-requirements)을 참조하세요.

## <a name="who-xamarin-is-for"></a>Xamarin 사용 대상

Xamarin의 사용 대상은 다음을 목표로 하는 개발자입니다.

- 플랫폼 전반에서 코드, 테스트 및 비즈니스 논리를 공유합니다.
- Visual Studio로 C#의 플랫폼 간 애플리케이션을 작성합니다.

## <a name="how-xamarin-works"></a>Xamarin 작동 방식

![Xamarin 아키텍처 다이어그램](what-is-xamarin-images/xamarin-architecture.png)

이 다이어그램에서는 플랫폼 간 Xamarin 애플리케이션의 전반적 아키텍처를 확인할 수 있습니다. Xamarin을 사용하면 각 플랫폼에서 네이티브 UI를 만들고 플랫폼 전반에서 공유되는 C#의 비즈니스 논리를 작성할 수 있습니다. 대부분의 경우 80%의 애플리케이션 코드는 Xamarin으로 공유가 가능합니다.

Xamarin은 메모리 할당, 가비지 수집, 기본 플랫폼과의 상호 운용성 같은 작업을 자동으로 처리하는 .NET 상위에 빌드됩니다.

플랫폼별 아키텍처에 대한 자세한 내용은 [Xamarin.Android](#xamarinandroid) 및 [Xamarin.iOS](#xamarinios)를 참조하세요.

### <a name="added-features"></a>추가된 기능

Xamarin은 네이티브 플랫폼의 기능을 결합하고 다음을 포함한 여러 기능을 추가합니다.

1. **기본 SDK에 대한 전체적인 바인딩** - Xamarin에는 iOS 및 Android에서 거의 모든 기본 플랫폼 SDK에 대한 바인딩이 포함되어 있습니다. 또한 이러한 바인딩은 강력한 형식이므로 탐색 및 사용하기 쉽고 개발 중에도 강력한 컴파일 시간 형식 확인을 제공합니다. 강력한 형식의 바인딩에 따라 런타임 오류가 줄어들고 애플리케이션의 품질이 향상됩니다.
1. **Objective-C, Java, C 및 C++ Interop** - Xamarin은 Objective-C, Java, C 및 C++ 라이브러리를 직접 호출할 수 있는 기능을 제공하므로 다양한 타사 코드를 사용할 수 있습니다. 이 기능은 Objective-C, Java 또는 C/C++로 작성된 기존 iOS 및 Android 라이브러리를 활용하는 데 도움이 됩니다. 또한 Xamarin은 선언적 구문을 사용하여 네이티브 Objective-C 및 Java 라이브러리를 바인딩할 수 있는 바인딩 프로젝트를 제공합니다.
1. **현대적인 언어 구문** - Xamarin 애플리케이션은 Lambdas, LINQ, 병렬 프로그래밍, 제네릭과 같은 기능적 구문, 동적 언어 기능과 같은 Objective-C 및 Java보다 크게 향상된 기능을 포함하는 현대적인 언어인 C#으로 작성됩니다.
1. **강력한 BCL(기본 클래스 라이브러리)** - Xamarin 애플리케이션은 강력한 XML, 데이터베이스, 직렬화, IO, 문자열 및 네트워킹 지원 등과 같은 포괄적이고 간소화된 기능을 갖춘 방대한 클래스의 컬렉션인 .NET BCL을 사용합니다. 기존 C# 코드를 앱에서 사용할 수 있도록 컴파일할 수 있으므로 BCL에서 다루지 않는 기능을 추가하는 수천 개의 라이브러리에 액세스할 수 있습니다.
1. **최신 IDE(통합 개발 환경)** - Xamarin은 코드 자동 완성, 정교한 프로젝트 및 솔루션 관리 시스템, 포괄적인 프로젝트 템플릿 라이브러리, 통합 소스 컨트롤 등의 기능이 포함된 최신 IDE인 Visual Studio를 사용합니다.
1. **모바일 플랫폼 간 지원** - Xamarin은 iOS, Android 및 Windows의 세 가지 주요 플랫폼에 대해 정교한 플랫폼 간 지원을 제공합니다. 애플리케이션은 코드의 최대 90%를 공유하도록 작성할 수 있으며 Xamarin.Essentials는 세 가지 모든 플랫폼 간에 공통의 리소스에 액세스할 수 있는 통합 API를 제공합니다. 공유된 코드는 모바일 개발자의 개발 비용과 출시 소요 기간을 크게 줄일 수 있습니다.

### <a name="xamarinandroid"></a>Xamarin.Android

[![Xamarin.Android 아키텍처 다이어그램](what-is-xamarin-images/android-architecture-cropped.png)](what-is-xamarin-images/android-architecture.png#lightbox)

Xamarin.Android 애플리케이션은 C#에서 **IL(중간 언어)** 로 컴파일되며, 이후에는 애플리케이션 시작 시 네이티브 어셈블리로의 **JIT(Just-in-Time)** 컴파일링이 진행됩니다. Xamarin.Android 애플리케이션은 ART(Android 런타임) 가상 머신과 함께 Mono 실행 환경에서 실행됩니다. Xamarin은 Android.* 및 Java.* 네임스페이스에 .NET 바인딩을 제공합니다. Mono 실행 환경은 **MCW(관리형 호출 가능 래퍼)** 를 통해 이러한 네임스페이스를 호출하고 ART에 **ACW(Android 호출 가능 래퍼)** 를 제공하여 두 환경이 코드를 서로 호출할 수 있도록 합니다.

자세한 내용은 [Xamarin.Android 아키텍처](~/android/internals/architecture.md)를 참조하세요.

### <a name="xamarinios"></a>Xamarin.iOS

[![Xamarin.iOS 아키텍처 다이어그램](what-is-xamarin-images/ios-architecture-cropped.png)](what-is-xamarin-images/ios-architecture.png#lightbox)

Xamarin.iOS 애플리케이션은 C#에서 네이티브 ARM 어셈블리 코드로 완전한 **AOT(Ahead-of-Time)** 컴파일링이 진행됩니다. Xamarin은 **선택기**를 사용하여 Objective-C를 관리형 C#에 공개하고 **등록자**를 사용하여 관리형 C# 코드를 Objective-C에 공개합니다. 선택기와 등록자는 함께 ‘바인딩’으로 불리며 Objective-C와 C#의 통신을 지원합니다.

자세한 내용은 [Xamarin.iOS 아키텍처](~/ios/internals/architecture.md)를 참조하세요.

### Xamarin.Essentials

Xamarin.Essentials는 네이티브 디바이스 기능을 대상으로 플랫폼 간 API를 제공하는 라이브러리입니다. Xamarin와 마찬가지로 Xamarin.Essentials 또한 일종의 추상화로서 네이티브 기능 액세스 프로세스를 간소화합니다. Xamarin.Essentials에서 제공하는 기능의 예로는 다음 몇 가지를 들 수 있습니다.

- 디바이스 정보
- 파일 시스템
- 가속도계
- 전화 걸기
- 텍스트 음성 변환
- 화면 잠금

자세한 내용은 [Xamarin.Essentials](~/essentials/index.md)를 참조하세요.

### Xamarin.Forms

Xamarin.Forms는 오픈 소스 UI 프레임워크입니다. 개발자는 Xamarin.Forms로 단일한 공유 코드베이스에서 Xamarin.iOS와 Xamarin.Android 및 Windows 애플리케이션을 빌드할 수 있습니다. 개발자는 Xamarin.Forms로 C#의 코드 숨김이 있는 XAML에서 사용자 인터페이스를 만들 수 있습니다. 이러한 사용자 인터페이스는 각 플랫폼에서 성능 네이티브 컨트롤로서 렌더링됩니다. Xamarin.Forms가 제공하는 기능의 예로는 다음 몇 가지를 들 수 있습니다.

- XAML 사용자 인터페이스 언어
- 데이터 바인딩
- 제스처
- 효과
- 스타일 지정

자세한 내용은 [Xamarin.Forms](~/xamarin-forms/index.yml)를 참조하세요.

## <a name="get-started"></a>시작

다음 가이드는 Xamarin을 사용해 앱을 처음 빌드할 때 유용합니다.

- [Xamarin.Forms 시작하기](~/xamarin-forms/index.yml)
- [Xamarin.Android 시작](~/android/index.yml)
- [Xamarin.iOS 시작](~/ios/index.yml)
- [Xamarin.Mac 시작](~/mac/index.yml)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Series/Xamarin-101/What-is-Xamarin-1-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
