---
title: Xamarin Live Player 앱
description: 이 문서는 Xamarin Live Player 장치에서 라이브 코드 변경 내용 미리 보기를 사용할 수 있는 앱을 설명 합니다. 설치, 샘플, 로그, 장치 등을 관리 하는 설정을 설명 합니다.
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: lobrien
ms.author: laobri
ms.date: 08/08/2017
ms.openlocfilehash: 89795e5df00b426c0f11c04a0844993071df1e25
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58855200"
---
# <a name="xamarin-live-player-app"></a>Xamarin Live Player 앱

![미리 보기 기능](~/media/shared/preview.png)

> [!NOTE]
> 실시간 플레이어 미리 보기에만 Visual Studio 2017에서 제공 됩니다.

휴대폰에서 앱을 설치한 후에 따라를 [설치 지침](~/tools/live-player/install.md) 컴퓨터에 연결 합니다. 중 하나를 시도 합니다 [샘플 앱](~/tools/live-player/samples.md) 작동 시켜야 합니다.

시작 시에 Xamarin Live Player 앱은 다음과 같습니다.

![라이브 Android 플레이어 앱 스크린 샷](player-images/app-android-sml.png)

누르면 **Visual Studio를 쌍**, 카메라를 사용 하 여 컴퓨터에 표시 된 바코드를 스캔 합니다.

![Android 바코드 스캐너의 스크린샷](player-images/scan-android-sml.png)

연결이 성공 하면 코드 실행 장치에서 거의 즉시 (같은 합니다 [Calculator 샘플을](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator)):

![장치에서 실행 되는 샘플 계산기 앱](player-images/basic-calculator-sml.png)

## <a name="options"></a>옵션

정보 단추를 눌러 **(i)** 표시 하기 위해 앱의 맨 아래에는 **옵션** 메뉴:

[![[옵션] 메뉴의 스크린샷](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>로그

문제를 진단 하는 로그를 봅니다.

### <a name="settings"></a>설정

- 컴파일 및 런타임 오류를 표시 하거나 숨깁니다.
- 버전 정보입니다.
- 피드백을 보냅니다.

[![설정의 스크린샷](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>장치 관리

처음으로 장치를 연결할의 지침을 따릅니다 [요구 사항 및 설치](~/tools/live-player/install.md)합니다. 여러 장치를 연결 하 고 IDE를 통해 관리할 수 있습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual studio에서 **도구 > Xamarin Live Player > 장치를 관리 하는 중...**

![장치 창 관리](player-images/manage-tools-menu-vs.png)

이 창에서는 다음을 수행할 수 있습니다.

- 코드를 스캔 하 여 새 장치 쌍
- 또는 해당 화면에 표시 된 코드를 입력 하 여 장치 쌍
- 목록에서 기존 장치를 제거 합니다.

또한 장치 목록에서이 창에 액세스할 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac 용 Visual studio **도구 > 장치를 관리 하는 (Xamarin Live Player)...**

![장치 창 관리](player-images/manage-tools-menu.png)

이 창에서는 다음을 수행할 수 있습니다.

- 코드를 스캔 하 여 새 장치 쌍
- 또는 해당 화면에 표시 된 코드를 입력 하 여 장치 쌍
- 목록에서 기존 장치를 제거 합니다.

![장치 창 관리](player-images/manage.png)

또한 장치 목록에서이 창에 액세스할 수 있습니다.

![장치 목록에서 Xamarin Live Player 장치를 선택 합니다.](player-images/manage-device-menu.png)

-----

모든 문제 참조 하는 경우 [제한 사항 및 문제 해결](~/tools/live-player/troubleshooting.md)합니다.

## <a name="related-links"></a>관련 링크

- [문제 해결](~/tools/live-player/troubleshooting.md)
- [Live Player를 사용 하는 샘플](https://developer.xamarin.com/samples/xamarin-live-player/all/)
