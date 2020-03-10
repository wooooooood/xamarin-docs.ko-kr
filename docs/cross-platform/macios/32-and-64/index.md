---
title: 32/64 비트 플랫폼 고려 사항
description: 이 문서에서는 Xamarin.ios 또는 Xamarin.ios 응용 프로그램에 대해 32 비트 및 64 비트 아키텍처를 대상으로 지정 하는 경우 염두에 두어야 할 다양 한 고려 사항을 설명 합니다.
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 5ba451de857444bc5b12b750ae479b62abdb75a3
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78910756"
---
# <a name="3264-bit-platform-considerations"></a>32/64 비트 플랫폼 고려 사항

IOS 및 macOS는 이전에 32 및 64 비트 앱을 모두 지원 했지만 Apple은 점점 더 이상 사용 되지 않는 32 비트 지원을 제공 합니다.

IOS 11부터 32 비트 앱은 더 이상 시작 되지 않으며, [앱 스토어에 대 한 모든 전송은 64 비트를 지원 해야](https://developer.apple.com/news/?id=06282017b)합니다.

2018 년 1 월부터 [Mac 앱 스토어에 제출 된 새 앱은 64 비트를 지원 해야](https://developer.apple.com/news/?id=06282017a)하 고, 기존 앱은 6 월 2018 일에 업데이트 되어야 합니다.

Xamarin의 Classic API (`XamMac.dll` 및 `monotouch.dll`)는 32 비트 응용 프로그램만 지원 합니다. 그러나 새 Xamarin.ios 및 Xamarin.ios 응용 프로그램은 기본적으로 [Unified API](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS` 및 `Xamarin.Mac`)를 사용 하므로 필요에 따라 32 및 64 비트를 모두 대상으로 지정할 수 있습니다.

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.ios 앱의 64 비트 빌드 사용

> [!WARNING]
> 이 섹션은 과거의 이유로 포함 되었으며 이전 Xamarin.ios 프로젝트를 Unified API로 이동 하 고 64 비트를 지원 합니다. 모든 새 Xamarin.ios 프로젝트는 기본적으로 Unified API 및 대상 64 비트를 사용 합니다.

Unified API로 변환 된 Xamarin.ios 모바일 응용 프로그램의 경우 개발자는 빌드 설정을 64 비트 대상으로 수동으로 업데이트 해야 합니다.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

1. **Solution Pad**에서 응용 프로그램의 프로젝트를 두 번 클릭 하 여 **프로젝트 옵션** 창을 엽니다.
2. **IOS 빌드**를 선택 합니다.
3. IPhone 시뮬레이터의 경우 **지원 되는 아키텍처** 드롭다운에서 **x86\_64** 또는 **i386 + x86\_64**를 선택 합니다.

   [![지원 되는 아키텍처를 x86\_64 또는 i386 + x86\_64로 설정](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. 물리적 장치의 경우 사용 가능한 **ARM64** 조합 중 하나를 선택 합니다.

   [![지원 되는 아키텍처를 ARM64 조합 중 하나로 설정](Images/Image02.png "지원 되는 아키텍처를 ARM64 조합 중 하나로 설정")](Images/Image02-large.png#lightbox)

5. **확인**을 클릭합니다.
6. 클린 빌드를 수행합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기**에서 응용 프로그램의 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.
2. **IOS 빌드**를 선택 합니다.
3. IPhone 시뮬레이터의 경우 **지원 되는 아키텍처** 를 **x86\_64** 또는 **i386 + x86\_64**으로 설정 합니다. 

   [![지원 되는 아키텍처를 x86_64 또는 i386 + x86\_64로 설정](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. 물리적 장치의 경우 사용 가능한 **ARM64** 조합 중 하나를 선택 합니다.
    
   [![지원 되는 아키텍처를 ARM64 조합 중 하나로 설정](Images/VS01.png "지원 되는 아키텍처를 ARM64 조합 중 하나로 설정")](Images/VS01-large.png#lightbox)

5. 변경 내용을 저장합니다.
6. 클린 빌드를 수행합니다.

-----

Armv7s 용 thumb-2는 iPhone 5 이상에 포함 된 A6 프로세서 에서만 지원 됩니다. ARMv7 코드는 ARMv6 보다 빠르고 작고, iPhone 3GS 이상 에서만 작동 하며, iPad 또는 최소 iOS 버전 5.0을 대상으로 하는 경우 Apple에서 필요 합니다. ARMv6는 모든 장치에서 작동 하지만 Xcode 4.5 이상 버전으로 제공 된 컴파일러에서는 더 이상 지원 되지 않습니다. 

ARM64은 iPhone 6 또는 기타 64 비트 장치에서 iOS 8을 지원 하기 위해 필요 하며, iTunes 앱 스토어에서 새로운 응용 프로그램을 제출 하거나 업데이트를 업데이트할 때 Apple에 필요 합니다.

다양 한 iOS 장치의 기능을 포괄적으로 확인 하려면 Apple의 [장치 호환성](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html) 문서를 참조 하세요.

### <a name="64-bit-and-binary-size-increases"></a>64 비트 및 이진 크기 늘리기

Apple이 32 비트에서 64 비트로 전환 되는 동안 iOS 앱은 32 비트 및 64 비트 하드웨어 둘 다에서 실행 되어야 합니다. 이로 인해 Xamarin의 Unified API를 통해 개발자는 두 가지를 모두 대상으로 지정할 수 있습니다.

32 비트와 64 비트 아키텍처를 모두 대상으로 지정 하면 응용 프로그램 크기가 크게 증가 합니다. 그러나 이렇게 하면 이전 장치를 계속 지원 하면서 새 장치에서 최적화 된 코드를 실행할 수 있습니다.

> [!IMPORTANT]
> ITunes 앱 스토어에 iOS 응용 프로그램을 제출할 때 다음과 같은 메시지가 표시 되 면 _"경고 ITMS-9000:64 비트 지원이 없습니다. 2015 년 2 월 1 일부 터 앱 스토어에 업로드 된 새 iOS 앱은 64 비트 지원을 포함 해야 하며, Xcode 6 이상에 포함 된 iOS 8 SDK를 사용 하 여 빌드해야 합니다. 프로젝트에서 64 비트를 사용 하도록 설정 하려면 "표준 아키텍처"의 기본 Xcode 빌드 설정을 사용 하 여 32 비트 및 64 비트 코드를 모두 사용 하 여 단일 이진 파일을 작성 하는 것이 좋습니다._ 지원 되는 아키텍처를 사용 가능한 **ARM64** 조합 (위와 같이) 중 하나로 전환 하 고 다시 컴파일한 후 다시 제출 해야 합니다.

## <a name="mac"></a>Mac

> [!IMPORTANT]
> 2018 년 1 월부터 Mac 앱 스토어에 제출 된 모든 새 Mac 앱은 64 비트를 지원 해야 합니다. 기존 Mac 앱 스토어 앱 및 해당 업데이트는 64 년 6 월 2018에 시작 하는 비트를 지원 해야 합니다. [Apple의 공지](https://developer.apple.com/news/?id=06282017a) 및 [xamarin.ios 앱을 64 비트로 업데이트 하는 방법을 설명 하는 가이드](~/cross-platform/macios/32-and-64/mac-64-bit.md)를 참조 하세요.

최신 Mac 컴퓨터는 32 비트 및 64 비트 응용 프로그램을 모두 지원 합니다.   MacOS 10.6 (눈 Leopard)는 32 비트 시스템에서 실행 되는 마지막 운영 체제 였습니다.   2010 이후 출시 된 대부분의 Mac은 두 시스템을 모두 지원 합니다.

IOS와 달리 최신 버전의 macOS에 도입 된 새로운 프레임 워크 중 상당수는 64 비트 모드 에서만 지원 됩니다 (CloudKit, EventKit, GameController, LocalAuthentication, MediaLibrary, MultipeerConnectivity, NotificationCenter, 글 키트, SpriteKit, 소셜, 및 MapKit).

개발자는 Unified API을 사용 하 여 생성 하려는 응용 프로그램의 종류 (32 비트 또는 64 비트)를 선택할 수 있습니다.

**32 비트 응용 프로그램** 은 32 비트 및 64 비트 Mac 컴퓨터 모두에서 실행 되 고, 주소 공간은 32 비트로 제한 되며, 모든 라이브러리가 32 비트가 되어야 합니다.

일반적으로이 모드는 64 비트 모드로 실행 되지 않는 32 비트 종속성이 있는 경우, 더 작은 다운로드를 사용 하거나 64 비트로 이동할 때 성능상의 이점이 없는 경우에 사용 합니다.

이 모드는 macOS Mavericks 및 macOS Yosemite에서 사용할 수 있는 많은 프레임 워크를 사용할 수 없기 때문에 제한 됩니다.

**64 비트 응용 프로그램** 은 64 비트 Mac 장치 에서만 실행 됩니다.

Mac의 경우 현재 사용 중인 대부분의 Mac에서 64 비트 모드를 지원 하 고 Apple에서 제공 하는 전체 프레임 워크 집합에 액세스할 수 있기 때문에이 작업은 기본 설정 모드입니다.

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Xamarin.ios 앱의 64 비트 빌드 사용

Xamarin.ios를 사용 하 여 64 비트 앱을 빌드하는 방법에 대 한 자세한 내용은 [Xamarin.ios 통합 응용 프로그램을 64 비트 가이드로 업데이트](~/cross-platform/macios/32-and-64/mac-64-bit.md) 를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [클래식 vs Unified API 차이점](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
