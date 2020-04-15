---
title: Windows용 원격 iOS 시뮬레이터
description: Windows용 원격 iOS 시뮬레이터를 사용하면 Visual Studio 2019와 함께 Windows에 표시된 iOS 시뮬레이터에서 앱을 테스트할 수 있습니다.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: davidortinau
ms.author: daortin
ms.date: 04/26/2019
ms.openlocfilehash: d5898f9c6ee30eb1f12bf6480b93a609e762e6ea
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "75886595"
---
# <a name="remoted-ios-simulator-for-windows"></a>Windows용 원격 iOS 시뮬레이터

Windows용 원격 iOS 시뮬레이터를 사용하면 Visual Studio 2019 및 Visual Studio 2017과 함께 Windows에 표시된 iOS 시뮬레이터에서 앱을 테스트할 수 있습니다.

[![Windows에서 실행 중인 iOS 시뮬레이터](images/hero-sml.png "Windows에서 실행 중인 iOS 시뮬레이터")](images/hero.png#lightbox)

## <a name="getting-started"></a>시작

Windows용 원격 iOS 시뮬레이터는 Visual Studio 2019 및 Visual Studio 2017에서 Xamarin의 일부로 자동으로 설치됩니다. 이를 사용하려면 다음 단계를 수행합니다.

1. [Visual 2019를 Mac 빌드 호스트에 페어링합니다](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Visual Studio에서 iOS 또는 tvOS 프로젝트 디버깅을 시작합니다. Windows용 원격 iOS 시뮬레이터가 Windows 컴퓨터에 표시됩니다.

단계별 가이드는 [이 비디오](deploy.md)를 시청하세요.

## <a name="simulator-window"></a>시뮬레이터 창

시뮬레이터 창의 맨 위에 있는 도구 모음에는 여러 가지 유용한 단추가 있습니다.

- **홈** – iOS 디바이스에서 홈 단추를 시뮬레이션합니다.
- **잠금** – 시뮬레이터를 잠급니다(잠금 해제는 살짝 밀기).
- **스크린샷** – 시뮬레이터의 스크린샷을 저장합니다(**Pictures\Xamarin\iOS Simulator\\** 에 저장됨).
- [**설정**](#settings) – 키보드, 위치 및 기타 설정을 표시합니다.
- [**기타 옵션**](#other-options) – 회전, 흔들기 제스처 및 터치 ID와 같은 다양한 시뮬레이터 옵션을 제공합니다.

    [![iOS 시뮬레이터 맵 예제](images/maps-app-sml.png "iOS 시뮬레이터 맵 예제")](images/maps-app.png#lightbox)

## <a name="settings"></a>설정

도구 모음의 기어 아이콘을 클릭하면 **설정** 창이 열립니다.

[![iOS 시뮬레이터 설정](images/settings-sml.png "iOS 시뮬레이터 설정")](images/settings.png#lightbox)

이러한 설정을 통해 하드웨어 키보드를 사용하도록 설정하고, 디바이스에서 보고해야 하는 위치를 선택하고(정적 및 이동 위치 둘 다 지원됨), 터치 ID를 사용하도록 설정하고, 시뮬레이터에 대한 콘텐츠 및 설정을 초기화할 수 있습니다.

## <a name="other-options"></a>기타 옵션

도구 모음에 있는 줄임표 단추는 회전, 흔들기 제스처, 다시 부팅 등의 기타 옵션을 표시합니다. 시뮬레이터 창의 아무 곳이나 마우스 오른쪽 단추로 클릭하여 동일한 옵션을 목록으로 볼 수 있습니다.

[![iOS 시뮬레이터 추가 설정](images/more-sml.png "iOS 시뮬레이터 추가 설정")](images/more.png#lightbox)

## <a name="touchscreen-support"></a>터치 스크린 지원

대부분의 최신 Windows 컴퓨터에는 터치 스크린이 있습니다. Windows용 원격 iOS 시뮬레이터는 터치 상호 작용을 지원하므로 실제 iOS 디바이스에서 사용하는 것과 동일한 손가락 모으기, 살짝 밀기 및 멀티 핑거 터치 제스처로 앱을 테스트할 수 있습니다.

마찬가지로 Windows용 원격 iOS 시뮬레이터는 Windows 스타일러스 입력을 Apple Pencil 입력으로 취급합니다.

## <a name="sound-handling"></a>소리 처리

시뮬레이터에서 재생한 소리는 호스트 Mac의 스피커를 통해 나옵니다.
Windows 컴퓨터에서는 iOS 소리가 들리지 않습니다.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>Windows용 원격 iOS 시뮬레이터 비활성화

Windows용 원격 iOS 시뮬레이터를 사용하지 않도록 설정하려면 **도구 > 옵션 > Xamarin > iOS 설정**으로 이동하여 **Windows에 대한 원격 시뮬레이터**를 선택 취소합니다.

[![시뮬레이터 사용 확인란](images/options-sml.png "시뮬레이터 사용 확인란")](images/options.png#lightbox)

이 옵션을 사용하지 않도록 설정하면 디버깅은 연결된 Mac 빌드 호스트에서 iOS 시뮬레이터를 엽니다.

## <a name="troubleshooting"></a>문제 해결

원격 iOS 시뮬레이터에 문제가 발생하는 경우 다음 위치에서 로그를 볼 수 있습니다.

- **Mac** – `~/Library/Logs/Xamarin/Simulator.Server`
- **Windows** – `%LOCALAPPDATA%\Xamarin\Logs\Xamarin.Simulator`

[Visual Studio에서 문제를 보고](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio)하는 경우 이러한 로그를 첨부하는 것이 도움이 될 수 있습니다(업로드를 비공개로 유지하는 옵션이 있음).
