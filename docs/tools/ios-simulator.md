---
title: Windows용 원격 iOS 시뮬레이터
description: 원격 iOS 시뮬레이터에 대 한 Windows를 사용 하면 Visual Studio 2017 옆 창에 표시 되는 iOS 시뮬레이터에서 앱을 테스트할 수 있습니다.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: topgenorth
ms.author: toopge
ms.date: 05/11/2018
ms.openlocfilehash: b07cc24e63f4aa3ce4451e3bdb5819f1df1058c6
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2018
---
# <a name="remoted-ios-simulator-for-windows"></a>Windows용 원격 iOS 시뮬레이터

원격 iOS 시뮬레이터에 대 한 Windows를 사용 하면 Visual Studio 2017 옆 창에 표시 되는 iOS 시뮬레이터에서 앱을 테스트할 수 있습니다.

[![](ios-simulator-images/hero-sml.png "iOS 시뮬레이터 Windows에서 실행")](ios-simulator-images/hero.png#lightbox)

## <a name="getting-started"></a>시작

원격 iOS 시뮬레이터에 대 한 Windows에서 Visual Studio 2017 Xamarin의 일부로 자동으로 설치 됩니다. 를 사용 하려면 다음이 단계를 수행 합니다.

1. [Mac에서 빌드 호스트를 Visual 2017 쌍으로 연결](~/ios/get-started/installation/windows/connecting-to-mac/index.md)합니다.
2. Visual Studio 2017 iOS 또는 tvOS 프로젝트 디버깅을 시작 합니다. Windows에 대 한 원격 iOS 시뮬레이터는 Windows 컴퓨터에 표시 됩니다.

## <a name="simulator-window"></a>시뮬레이터 창의

시뮬레이터의 창 맨 위에 있는 도구 모음에 유용한 단추 수를 포함 되어 있습니다.

- **홈** – 시뮬레이션 iOS 장치에서 홈 단추
- **잠금** – 시뮬레이터 (잠금을 해제 하려면 살짝)를 잠급니다.
- **스크린 샷** – 시뮬레이터의 스크린 샷 저장
- [**설정** ](#settings) – 키보드, 위치 및 기타 설정 표시
- [**다른 옵션** ](#other-options) – 회전 및 흔들기 제스처 등의 다양 한 시뮬레이터 옵션 표시

    [![](ios-simulator-images/maps-app-sml.png "iOS 시뮬레이터가 매핑합니다 예제를")](ios-simulator-images/maps-app.png#lightbox)

## <a name="settings"></a>설정

도구 모음의 기어 아이콘을 클릭 하면 열립니다는 **설정을** 창:

[![](ios-simulator-images/settings-sml.png "iOS 시뮬레이터 설정")](ios-simulator-images/settings.png#lightbox)

이러한 설정을 사용 하면 하드웨어 키보드를 사용 하도록 설정 하려면 선택 하는 장치 해야 하는 위치 (정적 및 이동 위치 지원 둘 다), 보고서 Touch ID를 사용 하도록 설정 하 고 콘텐츠 및 시뮬레이터에 대 한 설정을 다시 설정 합니다.

## <a name="other-options"></a>기타 옵션

도구 모음의 줄임표 단추에서 회전, 흔들기 제스처 및 다시 부팅 등의 다른 옵션을 보여 줍니다. 이러한 동일한 옵션 시뮬레이터 창에서 아무 곳 이나 마우스 오른쪽 단추로 클릭 하 여 목록으로 볼 수 있습니다.

[![](ios-simulator-images/more-sml.png "iOS 시뮬레이터 추가 설정")](ios-simulator-images/more.png#lightbox)

## <a name="touchscreen-support"></a>터치 스크린이 지원

대부분의 Windows 컴퓨터는 터치 스크린이 합니다. 원격 iOS 시뮬레이터에 대 한 Windows 터치 상호 작용을 지원 하므로 동일한 축소의 통과 / 실제 iOS 장치를 사용 하는 다중 손가락 터치 제스처를 사용 하 여 앱을 테스트할 수 있습니다.

마찬가지로, 원격 iOS 시뮬레이터에 대 한 Windows Apple 연필 입력으로 Windows 스타일러스 입력을 처리합니다.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>원격 iOS 시뮬레이터에 대 한 Windows를 사용 하지 않도록 설정

원격 iOS 시뮬레이터에 대 한 Windows를 사용 하지 않으려면로 이동 **도구 > 옵션 > Xamarin > iOS 설정** 의 선택을 취소 **Windows 원격 시뮬레이터**합니다.

[![](ios-simulator-images/options-sml.png "시뮬레이터를 사용 하려면이 확인란")](ios-simulator-images/options.png#lightbox)

이 옵션을 사용 하지 않도록 설정, 열립니다 디버깅 iOS 연결 된 Mac에서 시뮬레이터가 호스트를 구축 합니다.
