---
title: "원격 iOS 시뮬레이터 (Windows)에"
description: "Windows에서 Visual Studio 내에 완전히 iOS 앱을 테스트 및 디버그"
ms.topic: article
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 04/07/2017
ms.openlocfilehash: 707ba5874c939fbd25f4e25a7cefd3dc5fc75131
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="remoted-ios-simulator-for-windows"></a>원격 iOS 시뮬레이터 (Windows)에

_Windows에서 Visual Studio 내에 완전히 iOS 앱을 테스트 및 디버그_

[ ![](ios-simulator-images/hero-sml.png "iOS 시뮬레이터 Windows에서 실행")](ios-simulator-images/hero.png)

## <a name="download-and-install"></a>다운로드 및 설치

다운로드는 [installer](https://dl.xamarin.com/xamarin-simulator/Xamarin.Simulator.Installer.msi) Windows 컴퓨터에 설치 합니다. Xamarin 용 도구 visual Studio를 이미 설치 되어 있어야 합니다.

> [!NOTE]
> 원격 iOS를 사용 하 여 시뮬레이터는 Visual Studio와 Xamarin이 설치 되어 네트워크에 연결 된 Mac 필요 합니다.

## <a name="getting-started"></a>시작

원격 iOS 시뮬레이터를 사용 합니다.

1. 원격 iOS 시뮬레이터를 시작 하기 전에 한 번 이상 Visual Studio가 Mac에 연결 되어 있는지 확인 합니다.
2. IOS 또는 tvOS 앱 확인는 **시작 프로젝트** 디버깅을 시작 합니다.

원격 iOS 시뮬레이터를 사용 하지 않도록 설정할 수 있습니다 **도구 > 옵션 > Xamarin > iOS 설정** 에 대 한 확인란의 선택을 취소 하 여 **Windows 원격 시뮬레이터** 여기에 표시 합니다.

[ ![](ios-simulator-images/options-sml.png "시뮬레이터를 사용 하려면이 확인란")](ios-simulator-images/options.png)

IOS 시뮬레이터는 연결 된 Mac 컴퓨터에 다음 열립니다. 원격 iOS 시뮬레이터를 다시 설정 하려면이 옵션을 선택 합니다.

## <a name="features"></a>기능

원격 iOS 시뮬레이터에서 테스트 하 고 Windows에서 Visual Studio에서 완전히 시뮬레이터에서 iOS 앱을 디버그 하는 방법을 제공 합니다.

### <a name="simulator-window"></a>시뮬레이터 창의

다양 한 시뮬레이터와 상호 작용 하는 단추를 포함 하는 창 도구 모음:

- **홈** -장치에서 홈 단추를 시뮬레이션 합니다.
- **잠금** – 시뮬레이터 (잠금을 해제 하려면 살짝 수)를 잠급니다.
- **스크린 샷** – 시뮬레이터의 스크린 샷의을 디스크에 저장 합니다.
- [**설정** ](#settings) -키보드 및 위치를 구성 합니다.
 - 다른 [ **옵션** ](#options) – 다양 한 시뮬레이터 옵션 회전, 흔들기 같은 사용할 수 있거나 다른 상태에서 시뮬레이터를 호출 합니다. 일부 옵션은 가려진 때 창에서 마우스 오른쪽 단추로 클릭 하거나 도구 모음에 표시 되는 줄임표 아이콘에서 액세스할 수 있습니다.

    [ ![](ios-simulator-images/maps-app-sml.png "iOS 시뮬레이터가 매핑합니다 예제를")](ios-simulator-images/maps-app.png)


### <a name="settings"></a>설정

"기어" 아이콘을 열립니다는 **설정을** 창:

[ ![](ios-simulator-images/settings-sml.png "iOS 시뮬레이터 설정")](ios-simulator-images/settings.png)

그러면 시뮬레이터에서 하드웨어 키보드를 사용 하도록 설정 하 고 (정적 위치 또는 기타 이동 위치 옵션 포함) 장치에 어떤 위치 보고는 선택 수 있습니다.



### <a name="other-options"></a>기타 옵션

회전, 흔들기 제스처를 트리거하는 시뮬레이터를 다시 부팅 처럼 시뮬레이터에서 사용할 수 있는 모든 옵션을 보려면 시뮬레이터 창에서 아무 곳 이나 마우스 오른쪽 단추로 클릭 합니다.

[ ![](ios-simulator-images/more-sml.png "iOS 시뮬레이터 추가 설정")](ios-simulator-images/more.png)

### <a name="touchscreen-support"></a>터치 스크린이 지원

대부분의 Windows 컴퓨터에서 터치 스크린 있고 원격 iOS 시뮬레이터를 사용 하면 iOS 앱에서 사용자 상호 작용을 테스트 하려면 시뮬레이터 창의 터치 합니다.

모으는, 넘기기가, 및 다중 손가락 터치 제스처로-이전에 물리적 장치에서 쉽게 테스트할만 있습니다 하는 것이 포함 됩니다.

또한 Apple 연필 입력 시뮬레이터에서 Windows에서 스타일러스 지원 변환 됩니다.

<!--
<a name="knownissues" />

# Known Issues

 - Apple Watch devices may show in the Visual Studio device list, but are not yet supported.
 - Launching in **Release** mode may also start Apple’s simulator on the networked Mac.
 - Closing the remote iOS Simulator on Windows will not immediately stop debugging in Visual Studio. Stop debugging manually from the menu or the red button.
 - Opening too many different simulators simultaneously will produce unexpected results.
 - Exception of type `Foundation.NSErrorException` may be thrown while launching Simulators. Workaround is to kill csproxy (server process) on the Mac host and re-deploy to the simulator.
 - Performance may be slower when using Xcode 8
-->
