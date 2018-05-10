---
title: Xamarin Player 라이브 응용 프로그램
description: 편집 하 고 iOS 또는 Android 장치에서 실시간으로에서 앱을 테스트 합니다.
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: topgenorth
ms.author: toopge
ms.date: 05/10/2017
ms.openlocfilehash: 2e2a3e80c121688f9a8bfff0afe4184f19fc6619
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="xamarin-live-player-app"></a>Xamarin Player 라이브 응용 프로그램

![미리 보기 기능](~/media/shared/preview.png)

## <a name="get-the-app"></a>앱 다운로드

# <a name="androidtabandroid"></a>[Android](#tab/android)

Xamarin Player Live는 Google Play에서 Android 용 제공 됩니다.

[![Google Play에서 사용 가능한](images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Xamarin 라이브 플레이어를 통해 사용할 수 있는 Google Play 없이 Android 장치에 대 한 [HockeyApp](https://aka.ms/xlp-hockeyapp) 배포 합니다. 초기 미리 보기 Android 옵트인 하 여 Google Play에서 직접 설치에 대를 작성 하는 또한는 [열기 베타 프로그램](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="iostabios"></a>[iOS](#tab/ios)

사용자가 의견을 교환하실는 [Xamarin Player 라이브 앱 _iOS 미리 보기_ ](https://aka.ms/liveplayeralpha) 즐길 TestFlight 통해 최신 향상 기능에 빠르게 액세스할 수 있습니다.

-----

## <a name="using-the-app"></a>응용 프로그램을 사용 하 여

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
