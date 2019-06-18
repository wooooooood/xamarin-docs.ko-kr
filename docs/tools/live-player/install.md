---
title: Xamarin Live Player Visual Studio 구성
description: 이 문서에서는 실행 중인 응용 프로그램에 라이브 편집 하려면 Xamarin Live Player를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: lobrien
ms.author: laobri
ms.date: 06/13/2019
ms.openlocfilehash: a29a637526c2829b44ae89d505dac37a648dee77
ms.sourcegitcommit: 93b1e2255d59c8ca6674485938f26bd425740dd1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67157743"
---
# <a name="xamarin-live-player-visual-studio-configuration"></a>Xamarin Live Player Visual Studio 구성

![미리 보기 기능](~/media/shared/preview.png)

> [!WARNING]
> Xamarin Live Player 미리 보기를 마쳤습니다. 앱을 사용할 수 없습니다. 아래 지침에 따라 고객은 미리 보기를 사용 하 여 Visual Studio 2017을 사용 하 여 계속 제공 됩니다.

> [!TIP]
> 사용할 수는 [XAML 미리 보기](~/xamarin-forms/xaml/xaml-previewer/index.md) 편집 하는 동안 화면 디자인을 보려면 Visual Studio 2019 또는 Mac 용 Visual Studio에서.

# <a name="visual-studio-2017tabwindows"></a>[Visual Studio 2017](#tab/windows)

## <a name="using-xamarin-live-player"></a>Xamarin Live Player를 사용 하 여

Xamarin Live Player 앱을 장치에 이미 있어야 합니다. 더 이상 다운로드할 수 없습니다.

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

## <a name="using-xamarin-live-player"></a>Xamarin Live Player를 사용 하 여

Xamarin Live Player 앱을 장치에 이미 있어야 합니다. 더 이상 다운로드할 수 없습니다.

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

- [문제 해결](~/tools/live-player/troubleshooting.md)
