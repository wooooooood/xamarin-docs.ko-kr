---
title: 32/64 비트 플랫폼 고려 사항
description: 이 문서는 Xamarin.iOS 또는 Xamarin.Mac 응용 프로그램에 대 한 32 비트 및 64 비트 아키텍처를 대상으로 할 때 염두에 다양 한 고려 사항을 설명 합니다.
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 31eb0bfae58ecdca40548e46d1d9d95828be67b4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120101"
---
# <a name="3264-bit-platform-considerations"></a>32/64 비트 플랫폼 고려 사항

IOS 및 macOS 지금까지 지원 되는 32 및 64 비트 앱을 하는 동안 Apple에 점진적으로 더 이상 사용 되지 32 비트 지원 합니다.

IOS 11 기준으로 32 비트 응용 프로그램은 더 이상 시작 하 고 [앱 스토어에 대 한 모든 전송에는 64 비트 지원 해야](https://developer.apple.com/news/?id=06282017b)합니다.

2018 년 1 월부터 [Mac 앱 스토어에 제출 된 새 앱에는 64 비트 지원 해야](https://developer.apple.com/news/?id=06282017a), 2018 년 6 월에서 기존 앱을 업데이트 해야 합니다.

Xamarin의 클래식 API (`XamMac.dll` 고 `monotouch.dll`) 32 비트 응용 프로그램만 지원 합니다. 그러나 새 Xamarin.iOS 및 Xamarin.Mac 응용 프로그램 사용 하 여는 [통합 API](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS` 및 `Xamarin.Mac`) 기본적으로 모두 32 비트 및 64 비트, 필요에 따라 대상 수 있으므로.

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS 앱의 빌드를 64 비트를 사용 하도록 설정

> [!WARNING]
> 이 섹션에서는 기록 이유로 및 64 비트 지원과 통합 API로 이전 Xamarin.iOS 프로젝트를 이동 하는 데 포함 됩니다. 모든 새 Xamarin.iOS 프로젝트는 기본적으로 통합 API 및 64 비트 대상에 사용 됩니다.

Xamarin.iOS 모바일 응용 프로그램의 통합 API로 변환 된 경우 개발자가 64 비트 대상에 빌드 설정을 수동으로 업데이트 해야 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 에 **Solution Pad**를 두 번 클릭 하 여 앱의 프로젝트를 **프로젝트 옵션** 창입니다.
2. 선택 **iOS 빌드**합니다.
3. IPhone 시뮬레이터 용에 **지원 되는 아키텍처** 드롭다운에서 선택 **x86\_64** 또는 **i386 + x86\_64**:

   [![지원 되는 아키텍처 x86으로 설정\_64 또는 i386 + x86\_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. 물리적 장치에 대 한 중 하나를 선택 **ARM64** 조합 합니다.

   [![ARM64 조합 중 하나를 지원 되는 아키텍처를 설정](Images/Image02.png "ARM64 조합 중 하나로 설정 지원 되는 아키텍처")](Images/Image02-large.png#lightbox)

5. **확인**을 클릭합니다.
6. 클린 빌드를 수행 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 에 **솔루션 탐색기**, 앱의 프로젝트를 마우스 오른쪽 단추로 **속성**합니다.
2. 선택 **iOS 빌드**합니다.
3. IPhone 시뮬레이터에 대 한 설정 **지원 되는 아키텍처** 하나로 **x86\_64** 또는 **i386 + x86\_64**: 

   [![지원 되는 아키텍처 x86_64 또는 i386 + x86으로 설정\_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. 물리적 장치에 대 한 중 하나를 선택 **ARM64** 조합 합니다.
    
   [![ARM64 조합 중 하나를 지원 되는 아키텍처를 설정](Images/VS01.png "ARM64 조합 중 하나로 설정 지원 되는 아키텍처")](Images/VS01-large.png#lightbox)

5. 변경 내용을 저장합니다.
6. 클린 빌드를 수행 합니다.

-----

ARMv7s는 iPhone 5 이상에 포함 된 A6 프로세서 에서만 지원 됩니다. ARMv7 코드 빠르고는 ARMv6 보다 작은 하며만 iPhone 3GS 이상에서 작동 하 iPad 또는 최소 iOS 버전 5.0 대상으로 하는 경우 Apple에서 필요 합니다. ARMv6 모든 장치에서 작동 하지만 Xcode 4.5 이상과 함께 제공 되는 컴파일러에서 더 이상 지원 됩니다. 

ARM64는 iOS 8 iPhone 6 또는 기타 64 비트 장치에서 지원 하는 데 필요 하 고 새로 추가 되거나 iTunes App Store에서에서 응용 프로그램을 종료 하 고 업데이트를 제출 하는 경우 Apple에서 요구 합니다.

포괄적으로 살펴볼 기능의 다양 한 iOS 장치를 Apple 확인해 [장치 호환성](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html) 문서.

### <a name="64-bit-and-binary-size-increases"></a>64 비트 및 이진 크기 증가

32 비트에서 64 비트 iOS에서 Apple의 전환 하는 동안 앱 32 비트 및 64 비트 하드웨어에서 실행 해야 합니다. 이 때문에 Xamarin의 통합 API에는 둘 다를 대상으로 할 수 있습니다.

32 비트 및 64 비트 아키텍처를 대상으로 하는 응용 프로그램의 크기를 크게 늘어납니다. 그러나 이렇게 하면 수 있게 됩니다 최신 장치 이전 장치를 계속 지원 하면서 최적화 된 코드를 실행 하도록 합니다.

> [!IMPORTANT]
> IOS 응용 프로그램을 iTunes 앱 스토어에 제출 하는 경우 다음과 같은 메시지가 표시 되 면 _"경고 ITMS 9000: 64 비트 지원은 없습니다. 2015 년 2 월 1 새 iOS 앱 스토어에 업로드 하는 앱 64 비트 지원을 포함 해야 시작한 Xcode 6에서 이상을 포함 하는 8 SDK, iOS를 사용 하 여 빌드할 수입니다. 사용 하도록 설정 하려면 프로젝트에 64 비트 좋습니다 Xcode 빌드 설정 "표준 아키텍처"의 기본값을 사용 하 여 32 비트 및 64 비트 코드를 사용 하 여 단일 이진 파일을 빌드하려면. "_ 지원 되는 아키텍처 중 하나를 전환 해야 할 **ARM64** 조합 (위와 같이), 다시 컴파일 및 다시 전송 합니다.

## <a name="mac"></a>Mac

> [!IMPORTANT]
> 2018 년 1 월부터 Mac 앱 스토어에 제출 된 모든 새 Mac 앱 64 비트를 지원 해야 합니다. 기존 Mac 앱 스토어 앱 및 해당 업데이트는 2018 년 6 월에서에서 시작 하는 64 비트 지원 해야 합니다. 참조 [Apple 공지](https://developer.apple.com/news/?id=06282017a) 하 고 [64 비트로 Xamarin.Mac 앱을 업데이트 하는 방법을 설명 하는 가이드](~/cross-platform/macios/32-and-64/mac-64-bit.md)합니다.

Mac 컴퓨터를 최신 32 비트 및 64 비트 응용 프로그램을 지원 합니다.   MacOS 10.6 (Snow Leopard) 마지막 운영 체제가 32 비트 시스템에서 실행 했습니다.   대부분의 Mac 출시 2010 두 시스템을 지원 합니다.

IOS와 달리 여러 macOS의 최신 버전에 도입 된 새 프레임 워크만 지 (CloudKit EventKit, GameController, LocalAuthentication, MediaLibrary, MultipeerConnectivity, NotificationCenter, GLKit, SpriteKit, 소셜, 64 비트 모드에서 및 다른 MapKit)입니다.

생성 하고자 하는 응용 프로그램의 종류를 선택 하는 개발자를 사용 하는 통합 API: 32 비트 또는 64 비트입니다.

**32 비트 응용 프로그램** 32 비트 및 64 비트 Mac 컴퓨터에서 실행, 32 비트로 제한 된 주소 공간 및 모든 라이브러리는 32 비트 요구 됩니다.

작은 다운로드 하려는 경우 64 비트 모드에서 실행 하지 않는 32 비트 종속성이 있는 경우 또는 64 비트 이동의 성능 이점이 없습니다 경우에 일반적으로이 모드를 사용 합니다.

이 모드는 macOS Mavericks 및 macOS Yosemite에서 사용할 수 있는 많은 프레임 워크를 사용 하는 옵션이 수를 제한 합니다.

**64 비트 응용 프로그램** 64 비트 Mac 장치 에서만 실행 됩니다.

Mac에 대 한 사용에서 대부분의 Mac에는 현재 64 비트 모드를 지원 하 고 Apple에서 제공 하는 프레임 워크의 전체 집합에 액세스할 수 있는 작업의 기본 모드입니다.

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Xamarin.Mac 앱의 빌드를 64 비트를 사용 하도록 설정

Xamarin.Mac을 사용 하 여 64 비트 앱을 빌드하는 방법에 대 한 정보를 참조 하십시오 합니다 [응용 프로그램을 64 비트로 Xamarin.Mac 통합 업데이트](~/cross-platform/macios/32-and-64/mac-64-bit.md) 가이드입니다.

## <a name="related-links"></a>관련 링크

- [클래식 vs 통합 API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
