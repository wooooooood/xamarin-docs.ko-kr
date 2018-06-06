---
title: Xamarin Player 라이브 응용 프로그램
description: 이 문서에서는 코드 변경 내용 미리 보기를 사용할 수 있는 앱을 장치에 라이브 Xamarin 라이브 플레이어를 설명 합니다. 설치, 샘플, 로그, 장치 관리 설정에 설명 합니다.
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: topgenorth
ms.author: toopge
ms.date: 05/14/2017
ms.openlocfilehash: 88f7f62650484007c221aa7baaa684f872e0a8e9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794152"
---
# <a name="xamarin-live-player-app"></a>Xamarin Player 라이브 응용 프로그램

![미리 보기 기능](~/media/shared/preview.png)

휴대폰에 앱을 설치한 후에 따라는 [설치 지침](~/tools/live-player/install.md) 컴퓨터에 연결 합니다. 중 하나를 시도 [앱 샘플](~/tools/live-player/samples.md) 작동 하도록 합니다.

시작 시에 Xamarin Player 라이브 응용 프로그램은 다음과 같습니다 (iOS 및 Android에서 각각):

![라이브 플레이어 iOS 앱의 스크린 샷](player-images/app-iphone-sml.png) ![Android Player 라이브 응용 프로그램 스크린 샷](player-images/app-android-sml.png)

누를 때 **Visual Studio를 쌍**, 카메라를 사용 하 여 컴퓨터에 표시 된 바코드 스캔:

![IOS 바코드 스캐너의 스크린 샷](player-images/scan-iphone-sml.png) ![Android 바코드 스캐너의 스크린 샷](player-images/scan-android-sml.png)

연결에 성공한 경우 코드를 실행할지 장치에 거의 즉시 (예:는 [Calculator 샘플을](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator)):

![장치에서 실행 되는 샘플 계산기 응용 프로그램](player-images/basic-calculator-iphone-sml.png)

## <a name="options"></a>옵션

정보 단추를 눌러 **(i)** 표시 하기 위해 응용 프로그램의 아래쪽에는 **옵션** 메뉴:

[![[옵션] 메뉴의 스크린 샷](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>로그

문제를 진단 하는 로그를 봅니다.

### <a name="settings"></a>설정

- 컴파일 및 런타임 오류를 표시 하거나 숨깁니다.
- 버전 정보입니다.
- 사용자 의견 보내기

[![설정의 스크린샷](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>장치 관리

지침에 따라 처음으로 장치를 연결할 [요구 사항 및 설치](~/tools/live-player/install.md)합니다. 여러 장치 (예를 들어 iOS 및 Android)을 페어링하고 IDE를 통해 관리할 수 있습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 선택 **도구 > Xamarin Player 라이브 > 장치 관리...**

![장치 창 관리](player-images/manage-tools-menu-vs.png)

이 창에서 다음 작업을 수행할 수 있습니다.

- 코드를 스캔 하 여 새 장치 쌍
- 또는 장치 화면에 표시 된 코드를 입력 하 여 쌍으로 연결
- 목록에서 기존 장치를 제거 합니다.

장치 목록에서이 창에 액세스할 수도 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac 용 Visual Studio에서 선택 **도구 > 장치를 관리 하는 (Xamarin 라이브 플레이어)...**

![장치 창 관리](player-images/manage-tools-menu.png)

이 창에서 다음 작업을 수행할 수 있습니다.

- 코드를 스캔 하 여 새 장치 쌍
- 또는 장치 화면에 표시 된 코드를 입력 하 여 쌍으로 연결
- 목록에서 기존 장치를 제거 합니다.

![장치 창 관리](player-images/manage.png)

또한 장치 목록에서이 창에 액세스할 수 있습니다.

![장치 목록에서 Xamarin 라이브 플레이어 장치를 선택 합니다.](player-images/manage-device-menu.png)

-----

모든 문제 참조를 발생 하는 경우 [제한 사항 및 문제 해결](~/tools/live-player/troubleshooting.md)합니다.

## <a name="related-links"></a>관련 링크

- [제한 사항](~/tools/live-player/limitations.md)
- [문제 해결](~/tools/live-player/troubleshooting.md)
- [Xamarin Player 라이브 샘플](samples.md)
