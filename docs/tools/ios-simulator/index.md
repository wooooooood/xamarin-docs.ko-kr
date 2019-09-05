---
title: Windows용 원격 iOS 시뮬레이터
description: Windows 용 원격 iOS 시뮬레이터를 사용 하면 Visual Studio 2019와 함께 Windows에 표시 된 iOS 시뮬레이터에서 앱을 테스트할 수 있습니다.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: conceptdev
ms.author: crdun
ms.date: 04/26/2019
ms.openlocfilehash: 3067493315c62c46f44fb94aa61a919e1449080b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289704"
---
# <a name="remoted-ios-simulator-for-windows"></a>Windows용 원격 iOS 시뮬레이터

Windows 용 원격 iOS 시뮬레이터를 사용 하면 Visual Studio 2019 및 Visual Studio 2017과 함께 Windows에 표시 된 iOS 시뮬레이터에서 앱을 테스트할 수 있습니다.

[![Windows에서 실행 되는 iOS 시뮬레이터](images/hero-sml.png "Windows에서 실행 되는 iOS 시뮬레이터")](images/hero.png#lightbox)

## <a name="getting-started"></a>시작

Windows 용 원격 iOS 시뮬레이터는 Visual Studio 2019 및 Visual Studio 2017에서 Xamarin의 일부로 자동으로 설치 됩니다. 이를 사용 하려면 다음 단계를 수행 합니다.

1. [Mac 빌드 호스트에 Visual 2019을 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md)합니다.
2. Visual Studio에서 iOS 또는 tvOS 프로젝트 디버깅을 시작 합니다. Windows 용 원격 iOS 시뮬레이터는 Windows 컴퓨터에 표시 됩니다.

단계별 가이드는 [이 비디오](deploy.md) 를 시청 하세요.

## <a name="simulator-window"></a>시뮬레이터 창

시뮬레이터 창의 맨 위에 있는 도구 모음에는 다음과 같은 여러 가지 유용한 단추가 있습니다.

- **Home** – iOS 장치에서 홈 단추를 시뮬레이션 합니다.
- **잠금** – 시뮬레이터를 잠급니다 (잠금 해제로 살짝 밀기).
- **스크린샷** – 시뮬레이터의 스크린샷 ( **Pictures\Xamarin\iOS 시뮬레이터\\** 에 저장)을 저장 합니다.
- [**설정**](#settings) – 키보드, 위치 및 기타 설정을 표시 합니다.
- [**기타 옵션**](#other-options) – 회전, 흔들기 제스처 및 터치식 ID와 같은 다양 한 시뮬레이터 옵션을 표시 합니다.

    [![iOS 시뮬레이터 맵 예제](images/maps-app-sml.png "iOS 시뮬레이터 맵 예제")](images/maps-app.png#lightbox)

## <a name="settings"></a>설정

도구 모음의 기어 아이콘을 클릭 하면 **설정** 창이 열립니다.

[![iOS 시뮬레이터 설정](images/settings-sml.png "iOS 시뮬레이터 설정")](images/settings.png#lightbox)

이러한 설정을 통해 하드웨어 키보드를 사용 하도록 설정 하 고, 장치에서 보고 해야 하는 위치를 선택 하 고 (정적 및 이동 위치가 둘 다 지원 됨), 터치 ID를 사용 하도록 설정 하 고, 시뮬레이터에 대 한 콘텐츠 및 설정을 다시 설정할 수 있습니다.

## <a name="other-options"></a>기타 옵션

도구 모음에 있는 줄임표 단추는 회전, 흔들기 제스처, 다시 부팅 등의 기타 옵션을 표시 합니다. 시뮬레이터 창의 아무 곳 이나 마우스 오른쪽 단추로 클릭 하 여 동일한 옵션을 목록으로 볼 수 있습니다.

[![iOS 시뮬레이터 추가 설정](images/more-sml.png "iOS 시뮬레이터 추가 설정")](images/more.png#lightbox)

## <a name="touchscreen-support"></a>터치 스크린 지원

대부분의 최신 Windows 컴퓨터에는 터치 스크린이 있습니다. Windows 용 원격 iOS 시뮬레이터는 터치 상호 작용을 지원 하므로 실제 iOS 장치에서 사용 하는 것과 동일한 손가락, 살짝 밀기 및 다중 손가락 터치 제스처로 앱을 테스트할 수 있습니다.

마찬가지로 Windows 용 원격 iOS 시뮬레이터는 Windows 스타일러스 입력을 Apple 연필 입력으로 취급 합니다.

## <a name="sound-handling"></a>소리 처리

시뮬레이터에서 재생 한 소리는 호스트 Mac의 스피커에서 제공 됩니다.
Windows 컴퓨터에서는 iOS 소리가 들리지 않습니다.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>Windows 용 원격 iOS 시뮬레이터를 사용 하지 않도록 설정

Windows 용 원격 iOS 시뮬레이터를 사용 하지 않도록 설정 하려면 **도구 > 옵션 > Xamarin > IOS 설정** 으로 이동 하 고 **windows에 대 한 원격 시뮬레이터**를 선택 취소 합니다.

[![시뮬레이터 사용 확인란](images/options-sml.png "시뮬레이터 사용 확인란")](images/options.png#lightbox)

이 옵션을 사용 하지 않도록 설정 하면 디버깅은 연결 된 Mac 빌드 호스트에서 iOS 시뮬레이터를 엽니다.
