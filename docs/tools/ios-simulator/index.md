---
title: Windows용 원격 iOS 시뮬레이터
description: 원격 iOS 시뮬레이터에 대 한 Windows를 사용 하면 Visual Studio 2019와 함께 Windows에 표시 되는 iOS 시뮬레이터에서 앱을 테스트할 수 있습니다.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: lobrien
ms.author: laobri
ms.date: 04/02/2019
ms.openlocfilehash: b962390d5a5a365ada93d1778e3efb65839f41c5
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58854953"
---
# <a name="remoted-ios-simulator-for-windows"></a>Windows용 원격 iOS 시뮬레이터

원격 iOS 시뮬레이터에 대 한 Windows를 사용 하면 Visual Studio 2019 및 Visual Studio 2017과 함께 Windows에 표시 되는 iOS 시뮬레이터에서 앱을 테스트할 수 있습니다.

[![iWindows에서 실행 되는 OS 시뮬레이터](images/hero-sml.png "Windows에서 실행 되는 iOS 시뮬레이터")](images/hero.png#lightbox)

## <a name="getting-started"></a>시작

Windows에 대 한 원격 iOS 시뮬레이터는 Visual Studio 2019 및 Visual Studio 2017에서 Xamarin의 일부로 자동으로 설치 됩니다. 를 사용 하려면 다음이 단계를 수행 합니다.

1. [Mac 빌드 호스트에 Visual 2019 쌍](~/ios/get-started/installation/windows/connecting-to-mac/index.md)합니다.
2. Visual Studio에서 iOS 또는 tvOS 프로젝트 디버깅을 시작 합니다. Windows에 대 한 원격 iOS 시뮬레이터는 Windows 컴퓨터에 표시 됩니다.

조사식 [이 비디오](deploy.md) 단계별 가이드입니다.

## <a name="simulator-window"></a>시뮬레이터 창

유용한 단추 수를 포함 하는 시뮬레이터 창의 맨 위에 있는 도구 모음:

- **홈** -iOS 장치의 홈 단추를 시뮬레이션 합니다.
- **잠금** – 시뮬레이터 (잠금을 해제 하려면 살짝 밀기)를 잠급니다.
- **스크린 샷** – 시뮬레이터의 스크린 샷을 저장 (에 저장 **Pictures\Xamarin\iOS 시뮬레이터\\**).
- [**설정을** ](#settings) -키보드, 위치 및 기타 설정을 표시 합니다.
- [**기타 옵션** ](#other-options) – 회전, 흔들기 제스처, 터치 ID 등 다양 한 시뮬레이터 옵션 표시

    [![iOS 시뮬레이터 매핑합니다 예제](images/maps-app-sml.png "iOS 시뮬레이터 매핑합니다 예제")](images/maps-app.png#lightbox)

## <a name="settings"></a>설정

도구 모음에서의 기어 아이콘을 클릭 하면 열립니다는 **설정을** 창:

[![iOS 시뮬레이터 설정](images/settings-sml.png "iOS 시뮬레이터 설정")](images/settings.png#lightbox)

이러한 설정을 사용 하면 하드웨어 키보드를 사용 하도록 설정 해야 하는 장치 위치 선택 (고정 및 이동 위치 모두 지원 됨) 보고서 Touch ID를 사용 하도록 설정 및 시뮬레이터에 대 한 설정을 확인 하 고 콘텐츠를 다시 설정 합니다.

## <a name="other-options"></a>기타 옵션

도구 모음에서의 줄임표 단추 회전, 흔들기 제스처 및 다시 부팅 등의 다른 옵션을 보여 줍니다. 시뮬레이터 창의 아무 곳 이나 마우스 오른쪽 단추로 클릭 하 여 이러한 동일한 옵션 목록을 볼 수 있습니다.

[![iOS 시뮬레이터 추가 설정](images/more-sml.png "iOS 시뮬레이터에 대 한 추가 설정")](images/more.png#lightbox)

## <a name="touchscreen-support"></a>터치 스크린 지원

최신 Windows 컴퓨터 터치 스크린에 있습니다. 원격 iOS 시뮬레이터에 대 한 Windows는 터치 상호 작용을 지원 하므로 동일한 축소, 살짝 및 실제 iOS 장치를 사용 하는 다중 손가락 터치 제스처를 사용 하 여 앱을 테스트할 수 있습니다.

마찬가지로, 원격 iOS 시뮬레이터에 대 한 Windows Apple 연필 입력으로 Windows 스타일러스 입력을 처리합니다.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>원격 iOS 시뮬레이터에 대 한 Windows를 사용 하지 않도록 설정

이동할 원격 iOS 시뮬레이터에 대 한 Windows를 사용 하지 않으려면 **도구 > 옵션 > Xamarin > iOS 설정** 의 선택을 취소 **Windows 원격 시뮬레이터**합니다.

[![c시뮬레이터를 사용 하려면 heckbox](images/options-sml.png "시뮬레이터를 사용 하는 확인란")](images/options.png#lightbox)

열립니다 디버깅 사용 안 함,이 옵션을 사용 하 여 연결된 된 Mac의 iOS 시뮬레이터 빌드 호스트를 사용 합니다.
