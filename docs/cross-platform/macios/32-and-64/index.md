---
title: 32/64 비트 플랫폼 고려 사항
description: 응용 프로그램에 대 한 32 비트 및 64 비트 아키텍처를 대상으로 할 때의 고려 사항
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 223da6b490e09b2fa27ab3bbf8fa123b5fa8070c
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="3264-bit-platform-considerations"></a>32/64 비트 플랫폼 고려 사항

IOS 및 macOS가 32 / 64 비트 앱 지금까지 지원 되지만, Apple은 점진적으로 않으며 32 비트 지원

IOS 11부터 32 비트 응용 프로그램이 더 이상 시작 되 고 [앱 스토어에 제출할 때 모든 64 비트를 지원 해야](https://developer.apple.com/news/?id=06282017b)합니다.

1 월 2018부터 [Mac 앱 스토어에 제출 하는 새로운 앱 64 비트를 지원 해야](https://developer.apple.com/news/?id=06282017a), 년 6 월 2018 하 여 기존 앱을 업데이트 해야 합니다.

Xamarin의 클래식 API (`XamMac.dll` 및 `monotouch.dll`) 32 비트 응용 프로그램만 지원 합니다. 그러나 새 Xamarin.iOS 및 Xamarin.Mac 응용 프로그램 사용 하 여는 [통합 API](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS` 및 `Xamarin.Mac`) 기본적으로 32와 필요에 따라 64 비트 대상 따라서 수 있습니다.

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS 앱의 빌드를 64 비트를 사용 하도록 설정

> [!WARNING]
> 이 섹션 기록상 이유로 및 이전 Xamarin.iOS 프로젝트는 통합 API를 이동 하 고 64 비트 지원에 포함 됩니다. 모든 새 Xamarin.iOS 프로젝트는 통합 API 및 64 비트 대상 기본적으로 사용 합니다.

Xamarin.iOS 모바일 응용 프로그램의 통합 API로 변환 된 경우 개발자가 64 비트 대상에 빌드 설정을 수동으로 업데이트 해야:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 에 **솔루션 패드**, 앱 프로젝트를 열려면 두 번 클릭은 **프로젝트 옵션** 창.
2. 선택 **iOS 빌드**합니다.
3. Iphone 시뮬레이터에는 **지원 되는 아키텍처** 드롭다운을 선택 **x86\_64** 또는 **i386 + x86\_64**:

   [![지원 되는 아키텍처 x86 설정\_64 또는 i386 + x86\_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. 물리적 장치에 대 한 사용 가능한 중 하나를 선택 **ARM64** 조합:

   [![지원 되는 아키텍처를 ARM64 조합 중 하나로 설정](Images/Image02.png "설정은 지원 아키텍처 ARM64 조합 중 하나를")](Images/Image02-large.png#lightbox)

5. **확인**을 클릭합니다.
6. 클린 빌드를 수행 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 에 **솔루션 탐색기**앱 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **속성**합니다.
2. 선택 **iOS 빌드**합니다.
3. IPhone 시뮬레이터에 대 한 설정 **지원 되는 아키텍처** 을 **x86\_64** 또는 **i386 + x86\_64**: 

   [![지원 되는 아키텍처 x86_64 또는 i386 + x86 설정\_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. 물리적 장치에 대 한 사용 가능한 중 하나를 선택 **ARM64** 조합:
    
   [![지원 되는 아키텍처를 ARM64 조합 중 하나로 설정](Images/VS01.png "설정은 지원 아키텍처 ARM64 조합 중 하나를")](Images/VS01-large.png#lightbox)

5. 변경 내용을 저장합니다.
6. 클린 빌드를 수행 합니다.

-----

ARMv7s는 iPhone 5 (또는 그 이상)에 포함 된 A6 프로세서 에서만 지원 됩니다. ARMv7 코드 빠르고는 ARMv6 보다 작은 작동 하는 iPhone 3GS 및 이후 버전 있고, iPad 또는 최소 iOS 버전 5.0 대상으로 하는 경우 Apple에 필요 합니다. ARMv6 모든 장치에서 작동 하지만 Xcode 4.5 이상과 함께 제공 되는 컴파일러에서 더 이상 지원 합니다. 

ARM64는 iPhone 6 또는 기타 64 비트 장치에서 iOS 8을 지원 하기 위해 필요 하 고 새롭거나 업데이트 iTunes App Store에서에서 응용 프로그램 종료를 전송할 때 Apple에서 요구 합니다.

Apple의 다양 한 iOS 장치의 기능을 살펴보려면 포괄적인 확인해 [장치 호환성](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html) 문서.

### <a name="64-bit-and-binary-size-increases"></a>증가 하는 64 비트 및 이진 크기

Apple의 전환과 32 비트에서 64 비트 iOS 앱 32 비트 및 64 비트 하드웨어에서 실행 해야 합니다. 이 때문에 Xamarin 통합 API에는 둘 다를 대상으로 개발자가 있습니다.

32 비트 및 64 비트 아키텍처를 대상으로 응용 프로그램의 크기를 크게 향상 됩니다. 그러나 이렇게 하면 되므로 하면 오래 된 장치를 계속 지원 하면서 최적화 된 코드를 실행 하려면 새로운 장치.

> [!IMPORTANT]
> ITunes App Store에 iOS 응용 프로그램을 제출할 때 다음과 같은 메시지가 나타나면 _"경고 ITMS 9000: 64 비트 지원은 없습니다. 2015 년 2 월 1 일, 새 iOS 앱 스토어에 업로드 된 앱 64 비트 지원을 포함 해야 시작과 Xcode 6 이상 버전을 포함 하는 8 SDK는 iOS를 사용 하 여 빌드한 수입니다. 사용 하도록 설정 하려면 64 비트 프로젝트에 사용할 수 있는 권장 Xcode 빌드 설정 아키텍처"Standard"의 기본 32 비트 및 64 비트 코드와 함께 단일 바이너리를 빌드할 수 있습니다. "_ 지원 되는 아키텍처에서 사용 가능한 중 하나로 전환 해야 **ARM64** 조합 (위와 같이), 다시 컴파일 및 다시 전송 합니다.

## <a name="mac"></a>Mac

> [!IMPORTANT]
> Mac 앱 스토어에 제출 하는 모든 새 Mac 앱 년 1 월 2018 년부터 64 비트를 지원 해야 합니다. 기존 Mac 앱 스토어 앱 및에 대 한 업데이트가 2018 년 6 월에에서 시작 하는 64 비트 지원 해야 합니다. 참조 [Apple의 announcment](https://developer.apple.com/news/?id=06282017a) 및 [를 64 비트로 Xamarin.Mac 앱을 업데이트 하는 방법에 설명 하는 지침](~/cross-platform/macios/32-and-64/mac-64-bit.md)합니다.

대부분의 요즘 Mac 컴퓨터는 32 비트 및 64 비트 응용 프로그램을 지원 합니다.   MacOS 10.6 (Snow Leopard)에서 32 비트 시스템에서 실행 하도록 마지막 운영 체제.   대부분 Mac 2010 지원 시스템 모두 해제 합니다.

IOS의 경우와 달리 여러 macOS의 최신 버전에 도입 된 새 프레임 워크만 지원 됩니다 (CloudKit, EventKit, GameController, LocalAuthentication, MediaLibrary, MultipeerConnectivity, NotificationCenter, GLKit, SpriteKit, 사회, 64 비트 모드에서 및 다른 규칙 으로부터 MapKit)입니다.

개발자가 어떤 종류의 응용 프로그램 생성 하기 위해 원하는 수를 허용 하는 통합 API: 32 비트 또는 64 비트입니다.

**32 비트 응용 프로그램** 32 비트 및 64 비트 Mac 컴퓨터에서 실행, 32 비트로 제한 된 주소 공간 및 모든 라이브러리는 32 비트 요구 됩니다.

일반적으로 많은 경우 성능 이점은 없습니다 64 비트를 이동 또는 더 작은 다운로드 하려는 경우 64 비트 모드에서 실행 하지 않는 32 비트 종속성이 있는 경우이 모드를 사용 합니다.

이 모드 제한 하 고 서버 macOS Mavericks와 macOS Yosemite에서 사용할 수 있는 많은 프레임 워크를 사용할 수 있습니다.

**64 비트 응용 프로그램** 64 비트 Mac 장치 에서만 실행 됩니다.

Mac에서 Mac을 사용 중인 대부분 현재 64 비트 모드를 지원 하 고 Apple에서 제공한 프레임 워크의 전체 집합에 액세스할 수 있는 작업의 기본 모드입니다.

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Xamarin.Mac 앱의 빌드를 64 비트를 사용 하도록 설정

Xamarin.Mac를 사용 하 여 64 비트 응용 프로그램을 작성 하는 방법에 대 한 정보를 참조 하십시오는 [64 비트 응용 프로그램 업데이트 Xamarin.Mac 통합](~/cross-platform/macios/32-and-64/mac-64-bit.md) 가이드입니다.

## <a name="related-links"></a>관련된 링크

- [클래식 vs 통합 API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
