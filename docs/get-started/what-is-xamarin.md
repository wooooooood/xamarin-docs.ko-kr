---
title: Xamarin(이)란 무엇인가요?
description: 이 문서에서는 Xamarin 및 관련 라이브러리를 소개 합니다.
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
ms.custom: video
author: profexorgeek
ms.author: jusjohns
ms.date: 09/16/2019
no-loc:
- Xamarin
- Xamarin.Forms
- Xamarin.Android
- Xamarin.Essentials
- Xamarin.iOS
- Xamarin.Mac
ms.openlocfilehash: d4abb59cac117314239c669df454a786a3720ff5
ms.sourcegitcommit: 6f09bc2b760e76a61a854f55d6a87c4f421ac6c8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75607882"
---
# <a name="what-is-opno-locxamarin"></a>Xamarin(이)란 무엇인가요?

[예: ![스크린샷 [! OP. IOS 및 Android의 a-LOC (Xamarin)] 응용 프로그램](what-is-xamarin-images/xamarin-app-cropped.png)](what-is-xamarin-images/xamarin-app.png#lightbox)

Xamarin는 iOS, Android 및 Windows 용 .NET을 사용 하는 최신의 고성능 응용 프로그램을 빌드하기 위한 오픈 소스 플랫폼입니다. Xamarin은 기본 플랫폼 코드와 공유 코드의 통신을 관리 하는 추상화 계층입니다. Xamarin는 메모리 할당 및 가비지 수집과 같은 편리 하며을 제공 하는 관리 환경에서 실행 됩니다.

Xamarin를 통해 개발자는 응용 프로그램의 평균 90%를 플랫폼 간에 공유할 수 있습니다. 이 패턴을 통해 개발자는 모든 비즈니스 논리를 단일 언어로 작성할 수 있습니다 (또는 기존 응용 프로그램 코드를 다시 사용할 수 있음). 하지만 각 플랫폼에서 네이티브 성능, 모양 및 느낌을 달성할 수 있습니다.

Xamarin 응용 프로그램을 PC 또는 Mac에 작성 하 고 Android의 **.apk** 파일 또는 iOS의 **ipa** 파일 등의 네이티브 응용 프로그램 패키지를 컴파일할 수 있습니다.

> [!NOTE]
> IOS 용 응용 프로그램을 컴파일 및 배포 하려면 현재 MacOS 컴퓨터가 필요 합니다. 개발 요구 사항에 대 한 자세한 내용은 [시스템 요구 사항](~/cross-platform/get-started/requirements.md#macos-requirements)을 참조 하세요.

## <a name="who-opno-locxamarin-is-for"></a>Xamarin 대상

Xamarin는 다음과 같은 목표를 가진 개발자를 위한 것입니다.

- 플랫폼 간에 코드, 테스트 및 비즈니스 논리를 공유 합니다.
- Visual Studio C# 를 사용 하 여 플랫폼 간 응용 프로그램을 작성 합니다.

## <a name="how-opno-locxamarin-works"></a>Xamarin 작동 방법

![다이어그램 [! OP. 비-LOC (Xamarin)] 아키텍처](what-is-xamarin-images/xamarin-architecture.png)

다이어그램은 플랫폼 간 Xamarin 응용 프로그램의 전반적인 아키텍처를 보여 줍니다. Xamarin를 사용 하 여 각 플랫폼에서 네이티브 UI를 만들고 플랫폼 간에 공유 C# 되는의 비즈니스 논리를 작성할 수 있습니다. 대부분의 경우 Xamarin를 사용 하 여 80%의 응용 프로그램 코드를 공유할 수 있습니다.

Xamarin는 .NET ECMA 표준을 기반으로 하는 오픈 소스 버전인 .NET Framework를 **Mono**위에 구축 합니다. Mono는 .NET Framework 자체와 거의 같은 기간 동안 있었으며 Linux, Unix, FreeBSD 및 macOS를 비롯 한 대부분의 플랫폼에서 실행 됩니다. Mono 실행 환경에서는 메모리 할당, 가비지 수집 및 기본 플랫폼과의 상호 운용성과 같은 작업을 자동으로 처리 합니다.

플랫폼별 아키텍처에 대 한 자세한 내용은Xamarin를 참조 [하세요. Android](#xamarinandroid) 및 [Xamarin. iOS](#xamarinios).

### <a name="added-features"></a>추가 된 기능

Xamarin는 네이티브 플랫폼의 기능을 결합 하 고 다음을 비롯 한 다양 한 기능을 추가 합니다.

1. **기본 sdk에 대 한 완전 한 바인딩** – Xamarin는 IOS 및 Android 모두에서 거의 모든 기본 플랫폼 sdk에 대 한 바인딩을 포함 합니다. 또한 이러한 바인딩은 강력한 형식이므로 탐색 및 사용하기 쉽고 개발 중에도 강력한 컴파일 시간 형식 확인을 제공합니다. 강력한 형식의 바인딩은 런타임 오류 및 고품질 응용 프로그램을 더 줄입니다.
1. **목적-C, Java, C 및 C++ Interop** – Xamarin는 목표 c, java, c 및 C++ 라이브러리를 직접 호출 하는 기능을 제공 하 여 광범위 한 타사 코드를 사용할 수 있는 기능을 제공 합니다. 이 기능을 사용 하면 목적-C, Java 또는 C/C++로 작성 된 기존 IOS 및 Android 라이브러리를 사용할 수 있습니다. 또한 Xamarin는 선언적 구문을 사용 하 여 네이티브 목표 C 및 Java 라이브러리를 바인딩할 수 있는 바인딩 프로젝트를 제공 합니다.
1. **최신 언어 구문** Xamarin – 응용 프로그램은 ( C#예: 동적 언어 기능, 람다, LINQ, 병렬 프로그래밍, 제네릭 등의 기능 생성자와 같은 목적-C 및 Java에 비해 크게 향상 된 기능을 포함 하는 최신 언어)로 작성 되었습니다.
1. **강력한 BCL (기본 클래스 라이브러리)** Xamarin – 응용 프로그램은 .net bcl, 강력한 XML, 데이터베이스, SERIALIZATION, IO, 문자열 및 네트워킹 지원과 같이 포괄적이 고 간소화 된 기능을 갖춘 클래스의 많은 컬렉션을 사용 합니다. 기존 C# 코드를 앱에서 사용 하기 위해 컴파일할 수 있습니다 .이를 통해 BCL 이외의 기능을 추가 하는 수천 개의 라이브러리에 액세스할 수 있습니다.
1. **최신 ide (통합 개발 환경)** – Xamarin는 코드 자동 완성, 정교한 프로젝트 및 솔루션 관리 시스템, 포괄적인 프로젝트 템플릿 라이브러리, 통합 소스 제어 등의 기능을 포함 하는 최신 Ide 인 Visual Studio를 사용 합니다.
1. **모바일 플랫폼 간 지원** – Xamarin는 IOS, Android 및 Windows의 세 가지 주요 플랫폼에 대 한 정교한 플랫폼 간 지원을 제공 합니다. 응용 프로그램을 작성 하 여 최대 90%의 코드를 공유 하 고 Xamarin수 있습니다. Essentials는 세 플랫폼 모두에서 공통 리소스에 액세스 하는 통합 API를 제공 합니다. 공유 코드는 개발 비용과 모바일 개발자를 위한 출시 시간을 크게 줄일 수 있습니다.

### <a name="opno-locxamarinandroid"></a>Xamarin. 용

[![[! OP. NO-LOC (Xamarin)]. Android 아키텍처 다이어그램](what-is-xamarin-images/android-architecture-cropped.png)](what-is-xamarin-images/android-architecture.png#lightbox)

Xamarin. Android 응용 프로그램은 C# 에서 **IL (중간 언어)** 로 컴파일됩니다. 그러면 응용 프로그램이 시작 될 때 네이티브 어셈블리에 **JIT (just-in-time)** 컴파일됩니다. Xamarin. Android 응용 프로그램은 Android 런타임 (ART) 가상 머신과 함께 Mono 실행 환경에서 실행 됩니다. Xamarin는 Android. * 및 Java. * 네임 스페이스에 대 한 .NET 바인딩을 제공 합니다. Mono 실행 환경은 **MCW (관리 되는 호출 가능 래퍼)** 를 통해 이러한 네임 스페이스를 호출 하 고, **Acw (Android 호출 가능 래퍼** )를 아트에 제공 하 여 두 환경에서 서로 코드를 호출할 수 있도록 합니다.

자세한 내용은Xamarin를 참조 [하세요. Android 아키텍처](~/android/internals/architecture.md).

### <a name="opno-locxamarinios"></a>Xamarin. iOS

[![[! OP. NO-LOC (Xamarin)]. iOS 아키텍처 다이어그램](what-is-xamarin-images/ios-architecture-cropped.png)](what-is-xamarin-images/ios-architecture.png#lightbox)

Xamarin. iOS 응용 프로그램은에서 C# 네이티브 ARM 어셈블리 코드로 컴파일된 **AOT (전체)** 입니다. Xamarin는 **선택기** 를 사용 하 여 관리 되는 C# C# 코드를 **등록 기관** 에 노출 하 고 관리 되는 코드를 목표-c에 노출 합니다. 선택기 및 등록 기관를 통칭 하 여 "바인딩" 이라고 하며, 목표- C# C와의 통신을 허용 합니다.

자세한 내용은 [Xamarin. iOS 아키텍처](~/ios/internals/architecture.md)를 참조 하세요.

### <a name="opno-locxamarinessentials"></a>Xamarin. 기능

Xamarin. Essentials는 네이티브 장치 기능에 대 한 플랫폼 간 Api를 제공 하는 라이브러리입니다. Xamarin와 같이 Xamarin합니다. Essentials는 네이티브 기능에 액세스 하는 프로세스를 간소화 하는 추상화입니다. Xamarin에서 제공 하는 기능의 몇 가지 예입니다. Essentials는 다음과 같습니다.

- 디바이스 정보
- 파일 시스템
- 가속도계
- 전화 걸기
- 텍스트 음성 변환
- 화면 잠금

자세한 내용은Xamarin를 참조 [하세요. Essentials](~/essentials/index.md).

### <a name="opno-locxamarinforms"></a>Xamarin. 양식에

Xamarin. 양식은 오픈 소스 UI 프레임 워크입니다. Xamarin. 개발자는 폼을 사용 하 여 단일 공유 코드 베이스에서 iOS, Android 및 Windows 응용 프로그램을 빌드할 수 있습니다. Xamarin. 개발자는 폼을 사용 하 여의 C#코드 숨김으로 XAML로 사용자 인터페이스를 만들 수 있습니다. 이러한 사용자 인터페이스는 각 플랫폼에서 성능의 네이티브 컨트롤로 렌더링 됩니다. Xamarin에서 제공 하는 기능의 몇 가지 예입니다. 양식은 다음과 같습니다.

- XAML 사용자 인터페이스 언어
- 데이터 바인딩
- 제스처
- 효과
- 스타일 지정

자세한 내용은Xamarin를 참조 [하세요. 양식](~/xamarin-forms/index.yml).

## <a name="get-started"></a>시작

다음 가이드는 Xamarin를 사용 하 여 첫 번째 앱을 빌드하는 데 도움이 됩니다.

- [Xamarin를 시작 합니다. 양식에](~/xamarin-forms/index.yml)
- [Xamarin를 시작 합니다. 용](~/android/index.yml)
- [Xamarin시작 하기. iOS](~/ios/index.yml)
- [Xamarin를 시작 합니다. 용](~/mac/index.yml)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Series/Xamarin-101/What-is-Xamarin-1-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
