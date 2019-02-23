---
title: Xamarin Live Player 설치
description: 이 문서에서는 Xamarin Live Player를 설정 하 고 사용 하 여 실행 중인 응용 프로그램에 라이브 편집을 확인 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: 1ebc450a285d47fe1eedb0f8d72d6c9dc2b95644
ms.sourcegitcommit: 0044d04990faa0b144b8626a4fceea0fdff95cfe
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56666885"
---
# <a name="xamarin-live-player-setup"></a>Xamarin Live Player 설치

Xamarin Live Player를 사용 하면 앱에 라이브 편집을 확인 하 고가 해당 변경 내용이 반영 실시간 장치에서. 에뮬레이터를 설정 또는 케이블을 사용 하 여 배포 하지 않아도 Xamarin Live Player 앱 내에서 코드 실행! 이 문서에서는 Xamarin Live Player를 설정 하는 방법을 설명 합니다.

![미리 보기 기능](~/media/shared/preview.png)

## <a name="1-get-the-android-app"></a>1. Android 앱 다운로드

Xamarin Live Player에서 직접 intalling 하 여 Android에 대 한 사용할 수 [HockeyApp](https://aka.ms/xlp-hockeyapp)합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="2-get-visual-studio-2017"></a>2. Get Visual Studio 2017

Xamarin Live Player에는 다음이 필요합니다.

- Visual Studio 2017 15.4 이상.
- Visual Studio 컴퓨터와 동일한 WiFi 네트워크 장치입니다.

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Xamarin Live Player를 사용 하 여 처음으로

1. 오픈 **Visual Studio 2017**합니다.
2. 로 **도구 > 옵션...**  하 고 선택 합니다 **Xamarin > 기타** 탭 합니다.
3. 눈금 **Xamarin Live Player 사용**:

    ![옵션 창에서 Xamarin Live Player 사용 확인란](install-images/vs2017-options.png)

4. Xamarin 프로젝트를 열거나 만듭니다 (또는 [샘플](~/tools/live-player/samples.md)).
5. 선택할 **Live Player** 장치 목록에서:

    ![Xamarin Live Player 옵션이 포함 된 장치 목록](install-images/devices-empty-windows.png)

    - 장치 쌍을 이루는 이미, 하는 경우 옵션으로 제공 됩니다.
    - 그렇지 않으면 필요한 경우 장치 쌍에 라는 메시지가 표시 합니다.

6. 키를 눌러 합니다 **실행** 단추나에서 옵션 중 하나 선택 합니다 **실행** 또는 오른쪽 클릭 메뉴:

    - **디버깅 하지 않고 시작** – 앱 및 장치에서 변경 하는 참조를 편집할 수 있습니다 (앱이 변경 내용이 적용 하 고 파일을 저장 하면 다시 시작)입니다.
    - **디버깅 시작** – 중단점을 설정 하 고 변수를 검사 하지만 코드를 편집할 수 없습니다.

    또는 선택할 **도구 > Xamarin Live Player > Live Run 현재 보기**, 앱 및 장치에서 변경 하는 참조를 편집할 수 있습니다. 현재 보기 (응용 프로그램의 주 화면) 대신 표시 됩니다.

7. Xamarin Live Player 앱이 장치에서 실행 되는 장치에 이미 연결 되어를 코드 바로 실행 됩니다.

    없는 장치 쌍을 이루는 장치를 연결 하는 방법에 대 한 지침을 사용 하 여 QR 코드를 표시 됩니다.

    ![쌍 장치 창](install-images/manage-empty-windows.png)

    연결에 대 한 장치에 연결할 수 없는 경우 오류 메시지가 표시 됩니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="2-get-visual-studio-for-mac"></a>2. Mac 용 Visual Studio 받기

Xamarin Live Player에는 다음이 필요합니다.

- OS X 10.11, macOS 10.12를 이상
- Visual Studio for Mac
- Mac 및 동일한 WiFi 네트워크 장치

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Xamarin Live Player를 사용 하 여 처음으로

1. 오픈 **Mac 용 Visual Studio**합니다.
2. 로 **Visual Studio > 기본 설정...**  하 고 선택 합니다 **프로젝트 > Xamarin Live Player (미리 보기)** 탭 합니다.
3. 눈금 **Xamarin Live Player 사용**:

    [![옵션 창에서 Xamarin Live Player 사용 확인란](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

4. Xamarin 프로젝트를 열거나 만듭니다 (또는 [샘플](~/tools/live-player/samples.md)).
5. 선택할 **Live Player** 장치 목록에 있습니다.

    ![Xamarin Live Player 옵션이 포함 된 장치 목록](install-images/devices.png)

    - 장치 쌍을 이루는 이미, 하는 경우 옵션으로 제공 됩니다.
    - 그렇지 않으면 필요한 경우 장치 쌍에 라는 메시지가 표시 합니다.
    - 선택 **Xamarin Live Player 장치...**  Xamarin Live Player를 사용 하 여 사용 하려는 장치를 관리할 수 있습니다.

6. 키를 눌러 합니다 **실행** 단추나에서 옵션 중 하나 선택 합니다 **실행** 또는 오른쪽 클릭 메뉴:

    ![실행 메뉴 옵션](install-images/run-menu.png)

    - **디버깅 하지 않고 시작** – 앱 및 장치에서 변경 하는 참조를 편집할 수 있습니다 (앱이 변경 내용이 적용 하 고 파일을 저장 하면 다시 시작)입니다.
    - **디버깅 시작** – 중단점을 설정 하 고 변수를 검사 하지만 코드를 편집할 수 없습니다.
    - **Live Run 현재 보기** – 앱 및 장치에서 변경 하는 참조를 편집할 수 있습니다. 현재 보기 (응용 프로그램의 주 화면) 대신 표시 됩니다.

7. Xamarin Live Player 앱이 장치에서 실행 되는 장치에 이미 연결 되어를 코드 바로 실행 됩니다.

    없는 장치 쌍을 이루는 장치를 연결 하는 방법에 대 한 지침을 사용 하 여 QR 코드를 표시 됩니다.

    ![쌍 장치 창](install-images/manage-empty.png)

    연결에 대 한 장치에 연결할 수 없는 경우 오류가 표시 됩니다.

    ![장치 오류 메시지에 연결할 수 없습니다.](install-images/error-cannot-connect.png)

-----

문제가 발생 하거나 연결할 수 없습니다, 참조 [제한 사항 및 문제 해결](~/tools/live-player/troubleshooting.md)합니다.

## <a name="related-links"></a>관련 링크

- [제한 사항](~/tools/live-player/limitations.md)
- [문제 해결](~/tools/live-player/troubleshooting.md)
- [Xamarin Live Player 샘플](~/tools/live-player/samples.md)
