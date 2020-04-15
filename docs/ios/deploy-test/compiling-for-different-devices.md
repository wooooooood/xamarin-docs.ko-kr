---
title: Xamarin.iOS에서 다른 디바이스용으로 컴파일
description: 이 문서에서는 다양한 디바이스에 대한 Xamarin.iOS 빌드를 사용자 지정하는 데 사용할 수 있는 다양한 빌드 구성 옵션을 설명합니다.
ms.prod: xamarin
ms.assetid: 3B259248-887E-3E4F-E09C-7AD28C2A8CEE
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 1f71179ccafc2daf65e792c4538bf47ea2df1e7d
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "75663717"
---
# <a name="compiling-for-different-devices-in-xamarinios"></a>Xamarin.iOS에서 다른 디바이스용으로 컴파일 중

프로젝트의 **iOS 빌드** 속성 페이지에서 실행 파일의 빌드 속성을 구성할 수 있으며, 이 페이지는 Mac용 Visual Studio에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 **옵션 > iOS 빌드**를 찾거나 Visual Studio의 **속성**에서 찾을 수 있습니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

[![](compiling-for-different-devices-images/image1.png "The Projects iOS Build properties page")](compiling-for-different-devices-images/image1.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![](compiling-for-different-devices-images/image1a.png "The Projects iOS Build properties page")](compiling-for-different-devices-images/image1a.png#lightbox)

-----

UI에서 사용할 수 있는 구성 옵션 외에도, 개발자 고유의 명령줄 옵션 집합을 [Xamarin.iOS 빌드 도구(mtouch)](~/ios/deploy-test/mtouch.md)에 전달할 수 있습니다.

## <a name="sdk-options"></a>SDK 옵션

Mac용 Visual Studio를 사용하면 SDK와 관련된 두 가지 중요한 속성인 소프트웨어를 빌드하는 데 사용되는 iOS SDK 버전 및 배포 대상(또는 필요한 최소 iOS 버전)을 구성할 수 있습니다.

iOS **SDK 버전** 옵션을 통해 Apple에서 게시한 다른 SDK 버전을 사용할 수 있으며, 이렇게 하면 Xamarin.iOS가 빌드 중에 참조해야 하는 컴파일러, 링커 및 라이브러리로 이동됩니다. 프로젝트를 마우스 오른쪽 단추로 클릭하고 **옵션**을 선택한 다음 옵션 창에서 **iOS 빌드**를 선택합니다.

[![옵션 창에서 SDK 버전 선택](compiling-for-different-devices-images/sdk-version-sml.png)](compiling-for-different-devices-images/sdk-version.png#lightbox)

**배포 대상** 설정은 애플리케이션이 실행될 운영 체제에서 필요한 최소 버전을 선택하는 데 사용됩니다. 또한 프로젝트의 **Info.plist** 파일에서 설정되어 있습니다. 애플리케이션을 실행하는 데 필요한 모든 API가 있는 최소 버전을 선택해야 합니다.

[![Info.plist 파일에서 배포 대상 설정](compiling-for-different-devices-images/deployment-target-sml.png)](compiling-for-different-devices-images/deployment-target.png#lightbox)

일반적으로 Xamarin.iOS API는 최신 버전의 SDK에서 사용할 수 있는 모든 메서드를 노출하며, 필요한 경우 런타임에 기능을 사용할 수 있는지 여부를 감지할 수 있는 편의 속성을 제공해 드립니다(예를 들어 `UIDevice.UserInterfaceIdiom` 및 `UIDevice.IsMultitaskingSupported`는 Xamarin.iOS에서 항상 작동하며 모든 작업이 보이지 않는 곳에서 처리됨).

## <a name="linking"></a>연결

링커로 실행 파일의 크기를 줄이는 방법 및 링커를 효율적으로 사용하는 방법에 대한 자세한 내용은 [링커](~/ios/deploy-test/linker.md)에 대한 전용 페이지를 참조하세요.

## <a name="code-generation-engine"></a>코드 생성 엔진

Xamarin.iOS 4.0부터 Xamarin.iOS에 두 가지 코드 생성 백 엔드가 있습니다. 하나는 일반적인 Mono 코드 생성 엔진이고, 다른 하나는 LLVM 최적화 컴파일러를 기반으로 하는 엔진입니다. 엔진마다 장단점이 있습니다.

일반적으로 개발 프로세스에서는 신속한 반복이 가능한 Mono 코드 생성 엔진이 주로 사용됩니다. 릴리스 빌드 및 AppStore 배포 시에는 LLVM 코드 생성 엔진으로 전환하게 됩니다.

LLVM 최적화 백 엔드 엔진은 Mono 엔진보다 빠르고 엄격한 코드를 생성하지만, 컴파일 시간이 깁니다.

이 방법은 Mac용 Visual Studio 또는 Visual Studio의 iOS 빌드 옵션에서 사용할 수 있습니다.

[![](compiling-for-different-devices-images/image2.png "Enabling LLVM")](compiling-for-different-devices-images/image2.png#lightbox)

[![](compiling-for-different-devices-images/image2a.png "Enabling LLVM")](compiling-for-different-devices-images/image2a.png#lightbox)

## <a name="architecture-support"></a>아키텍처 지원

### <a name="armv6-xamarinios-discontinued-support-for-armv6-with-v810"></a>ARMv6(Xamarin.iOS는 v8.10부터 ARMv6 지원 중단)

- iPhone(초기 모델), 3G
- iPod 1, 2세대

### <a name="armv7"></a>ARMv7

- iPhone 3GS, 4, 4S
- iPad 1, 2, 3, Mini
- iPod 3, 4, 5세대

### <a name="armv7s"></a>ARMv7s

- iPhone 5
- iPhone 5c
- iPad 4

ARMv7s 프로세서만 대상으로 하는 경우 생성된 코드는 약간 더 빠르지만 패키지에 여러 실행 파일을 포함하고 있는 이진 파일을 컴파일하지 않는 이상, 더 이상 ARMv7 또는 ARMv6 시스템에서 실행되지 않습니다.

### <a name="arm64-xamarinios-started-supporting-arm64-in-v86"></a>ARM64(Xamarin.iOS는 v8.6부터 ARM64 지원 시작)

- iPhone 5s
- iPhone SE
- iPhone 6, 6 Plus
- iPhone 6s, 6s Plus
- iPhone 7, 7 Plus
- iPhone 8, 8 Plus
- iPhone X
- iPad Air
- iPad Air 2
- iPad Mini 2, 3, 4
- iPad Pro(모두)

앱 스토어에 제출된 모든 빌드는 64비트를 지원해야 하며, 이는 [Apple](https://developer.apple.com/news/?id=12172014b)에서 설정한 요구 사항입니다. 또한 iOS 11은 64비트 애플리케이션만 지원합니다.

### <a name="arm-thumb-2-support"></a>ARM Thumb-2 지원

Thumb는 ARM 프로세서에서 사용하는 보다 간단한 명령 집합입니다. Thumb을 지원하면 실행 파일의 크기를 줄일 수 있지만, 실행 시간이 느려집니다. Thumb은 ARMv7 및 ARMv7s에서 지원됩니다.

## <a name="conditional-framework-usage"></a>조건부 프레임워크 사용

프로젝트에 최신 iOS 릴리스의 일부 기능을 활용하려면 조건에 따라 특정 새 프레임워크를 사용해야 할 수도 있습니다. 가장 대표적인 예로 iOS 4.0 이상에서 실행 중일 때 iAd를 사용하고 싶지만 계속해서 3.x 디바이스를 지원하려는 경우를 들 수 있습니다. 이렇게 하려면 iAd 프레임워크에 "약하게" 연결해야 한다는 사실을 Xamarin.iOS가 알 수 있게 해야 합니다. 약한 바인딩은 프레임워크의 클래스가 처음으로 필요할 때 프레임워크만 필요에 따라 로드되도록 보장합니다.

이렇게 하려면 다음 단계를 수행해야 합니다.

- **프로젝트 옵션**을 열고 **iOS 빌드** 창으로 이동합니다.
- 약하게 연결하려는 각 구성의 **추가 옵션**에 `'-gcc_flags "-weak_framework iAd"'`를 추가합니다.

[![](compiling-for-different-devices-images/image3.png "Additional Options")](compiling-for-different-devices-images/image3.png#lightbox)

그 외에도 사용하는 유형이 존재하지 않는 이전 iOS 버전에서 실행되지 않도록 보호해야 합니다. 이렇게 하는 여러 가지 방법이 있는데, 그 중 하나는 `UIDevice.CurrentDevice.SystemVersion`을 구문 분석하는 것입니다.

## <a name="related-links"></a>관련 링크

- [링커](~/ios/deploy-test/linker.md)
