---
title: Xamarin Player 라이브 설치
description: 편집 하 고 iOS 또는 Android 장치에서 실시간으로에서 앱을 테스트 합니다.
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/22/2017
ms.openlocfilehash: 6a721eedc278864b79d5f2b3cb16fb7075bfb15d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarin-live-player-setup"></a>Xamarin Player 라이브 설치

이러한 변경 내용을 반영 않았습니다 실시간 장치에서 및 Xamarin Player 라이브 앱에 라이브를 편집할 수 있습니다. 사용자 내에서 실행 되는 Xamarin Player 라이브 앱 – 에뮬레이터를 설정 하거나 케이블을 사용 하 여 배포할 필요가 없습니다! 이 문서에서는 Xamarin 라이브 플레이어를 설정 하는 방법을 설명 합니다.

![미리 보기 기능](~/media/shared/preview.png)

## <a name="1-get-the-app"></a>1. 앱 다운로드

# <a name="androidtabandroid"></a>[Android](#tab/android)

Xamarin Player Live는 Google Play에서 Android 용 제공 됩니다.

[ ![Google Play에서 사용 가능한](install-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Xamarin 라이브 플레이어를 통해 사용할 수 있는 Google Play 없이 Android 장치에 대 한 [HockeyApp](https://aka.ms/xlp-hockeyapp) 배포 합니다. 초기 미리 보기 Android 옵트인 하 여 Google Play에서 직접 설치에 대를 작성 하는 또한는 [열기 베타 프로그램](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="iostabios"></a>[iOS](#tab/ios)

사용자가 의견을 교환하실는 [Xamarin Player 라이브 앱 _iOS 미리 보기_ ](https://aka.ms/liveplayeralpha) 즐길 TestFlight 통해 최신 향상 기능에 빠르게 액세스할 수 있습니다.

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="2-get-visual-studio-2017"></a>2. Get Visual Studio 2017

Xamarin Player 라이브에 다음 사항이 필요합니다.

- Visual Studio 2017 15.4 이상.
- Visual Studio 컴퓨터와 동일한 WiFi 네트워크 장치가 있습니다.

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Xamarin 라이브 플레이어를 사용 하 여 처음으로

1. 열기 **Visual Studio 2017**합니다.
2. 로 이동 **도구 > 옵션...**  선택 하 고는 **Xamarin > 기타** 탭 합니다.
3. 눈금 **라이브 Xamarin Player를 사용 하도록 설정**:

  ![옵션 창에서 Xamarin 라이브 Player 사용 확인란](install-images/vs2017-options.png)

2. Xamarin 프로젝트를 열거나 만듭니다 (또는 [샘플](~/tools/live-player/samples.md)).
3. 선택 **라이브 플레이어** 장치 목록에서:

  ![장치 목록에 Xamarin Player 라이브 옵션을 포함 됩니다.](install-images/devices-empty-windows.png)

  * 장치를 이미 쌍으로 하는 경우 옵션으로 사용할 수 있습니다.
  * 그렇지 않으면 필요할 때 장치와 쌍으로 연결 하 라는 메시지가 표시 됩니다.
4. 키를 눌러는 **실행** 단추나는 다음 옵션 중 하나에서 선택 된 **실행** 또는 오른쪽 클릭 메뉴:

  - **디버깅 하지 않고 시작** -앱 및 장치에 변경이 발생할 참조 편집할 수 있습니다 (앱을 변경 하 고 파일을 저장 하면 다시 시작)입니다.
  - **디버깅 시작** – 중단점을 설정 하 고 변수를 검사 하지만 코드를 편집할 수 없습니다.

  또는 선택 **도구 > Xamarin Player 라이브 > 현재 보기 실행 라이브**, 앱 및 장치에 변경이 발생할 참조 편집할 수 있습니다. 현재 보기 (응용 프로그램의 주 화면) 하는 대신 표시 됩니다.

5. 장치가 이미 페어링 되었습니다 경우 Xamarin Player 라이브 앱이 장치에서 실행 되는 코드는 곧바로 실행!

  장치가 없어 경우 쌍을 이루 면 QR 코드는 방법에 대 한 설명이 나타납니다 장치와 쌍으로 연결 하려면:

  ![쌍 장치 창](install-images/manage-empty-windows.png)

  쌍을 이루기 위해 장치를 연결할 수 없는 경우 오류가 나타날 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="2-get-visual-studio-for-mac"></a>2. Mac 용 Visual Studio를 가져오려면

Xamarin Player 라이브에 다음 사항이 필요합니다.

- OS X 10.11 macOS 10.12, 이상
- Visual Studio for Mac
- Mac과 같은 WiFi 네트워크 장치가

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. 처음으로 라이브 Xamarin Player를 사용 하 여

1. 열기 **Mac 용 Visual Studio**합니다.
2. 로 이동 **Visual Studio > 기본 설정 중...**  선택 하 고는 **프로젝트 > Xamarin 라이브 플레이어 (미리 보기)** 탭 합니다.
3. 눈금 **라이브 Xamarin Player를 사용 하도록 설정**:

  [![옵션 창에서 Xamarin 라이브 Player 사용 확인란](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

2. Xamarin 프로젝트를 열거나 만듭니다 (또는 [샘플](~/tools/live-player/samples.md)).
3. 선택 **라이브 플레이어** 장치 목록에 있습니다.

  ![장치 목록에 Xamarin Player 라이브 옵션을 포함 됩니다.](install-images/devices.png)

  * 장치를 이미 쌍으로 하는 경우 옵션으로 사용할 수 있습니다.
  * 그렇지 않으면 필요할 때 장치와 쌍으로 연결 하 라는 메시지가 표시 됩니다.
  * 선택 **Xamarin 라이브 플레이어 장치...**  Xamarin 라이브 플레이어로 사용 하려는 장치를 관리할 수 있습니다.

4. 키를 눌러는 **실행** 단추나는 다음 옵션 중 하나에서 선택 된 **실행** 또는 오른쪽 클릭 메뉴:

  ![실행 메뉴 옵션](install-images/run-menu.png)

  - **디버깅 하지 않고 시작** -앱 및 장치에 변경이 발생할 참조 편집할 수 있습니다 (앱을 변경 하 고 파일을 저장 하면 다시 시작)입니다.
  - **디버깅 시작** – 중단점을 설정 하 고 변수를 검사 하지만 코드를 편집할 수 없습니다.
  - **현재 보기 실행 라이브** -앱 및 장치에 변경이 발생할 참조 편집할 수 있습니다. 현재 보기 (응용 프로그램의 주 화면) 하는 대신 표시 됩니다.

5. 장치가 이미 페어링 되었습니다 경우 Xamarin Player 라이브 앱이 장치에서 실행 되는 코드는 곧바로 실행!

  장치가 없거나 쌍을 이루 면 QR 코드를 장치와 쌍으로 연결 하는 방법에 대 한 설명이 표시 됩니다.

  ![쌍 장치 창](install-images/manage-empty.png)

  쌍을 이루기 위해 장치를 연결할 수 없는 경우 오류가 표시 됩니다.

  ![장치 오류 메시지에 연결할 수 없습니다.](install-images/error-cannot-connect.png)


-----

관련 문제가 발생 하거나 연결할 수 없습니다. 참조 [제한 사항 및 문제 해결](~/tools/live-player/troubleshooting.md)합니다.


## <a name="related-links"></a>관련 링크

- [제한 사항](~/tools/live-player/limitations.md)
- [문제 해결](~/tools/live-player/troubleshooting.md)
- [Xamarin Player 라이브 샘플](~/tools/livehttps://developer.xamarin.com/samples.md)
