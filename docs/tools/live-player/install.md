---
title: Visual Studio 구성 Xamarin Live Player
description: 이 문서에서는 Xamarin Live Player를 사용 하 여 실행 중인 응용 프로그램에 대 한 라이브 편집을 수행 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: conceptdev
ms.author: crdun
ms.date: 06/13/2019
ms.openlocfilehash: 94f1d36bf97aab7eabb57e6f2712c9850b390ab1
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290482"
---
# <a name="xamarin-live-player-visual-studio-configuration"></a>Visual Studio 구성 Xamarin Live Player

![미리 보기 기능](~/media/shared/preview.png)

> [!WARNING]
> Xamarin Live Player 미리 보기가 종료 되었습니다. 앱을 더 이상 사용할 수 없습니다. 아래 지침은 Visual Studio 2017에서 미리 보기를 계속 사용 하는 고객을 위해 제공 됩니다.

> [!TIP]
> Visual Studio 2019 또는 Mac용 Visual Studio의 [XAML 미리 보기](~/xamarin-forms/xaml/xaml-previewer/index.md) 를 사용 하 여 편집 하는 동안 화면 디자인을 볼 수 있습니다.

# <a name="visual-studio-2017tabwindows"></a>[Visual Studio 2017](#tab/windows)

## <a name="using-xamarin-live-player"></a>Xamarin Live Player 사용

장치에 Xamarin Live Player 앱이 이미 있어야 합니다. 더 이상 다운로드할 수 없습니다.

1. **Visual Studio 2017**을 엽니다.
2. **도구 > 옵션 ...** 로 이동 하 여 **Xamarin > 기타** 탭을 선택 합니다.
3. 틱 **사용 Xamarin Live Player**:

    ![옵션 창에서 사용 Xamarin Live Player 상자를 선택 합니다.](install-images/vs2017-options.png)

4. Xamarin 프로젝트 (또는 [샘플](~/tools/live-player/samples.md))를 만들거나 엽니다.
5. 장치 목록에서 **라이브 플레이어** 를 선택 합니다.

    ![장치 목록에 Xamarin Live Player 옵션이 포함 되어 있습니다.](install-images/devices-empty-windows.png)

    - 이미 장치를 페어링된 경우에는 옵션으로 사용할 수 있습니다.
    - 그렇지 않으면 필요한 경우 장치를 페어링 하 라는 메시지가 표시 됩니다.

6. **실행** 단추를 누르거나 **실행** 또는 오른쪽 클릭 메뉴에서 다음 옵션 중 하나를 선택 합니다.

    - **디버깅 하지 않고 시작** – 앱을 편집 하 고 장치에서 변경이 발생 하는 것을 볼 수 있습니다. 변경 내용이 적용 되 고 파일이 저장 되 면 앱이 다시 시작 됩니다.
    - **디버깅 시작** – 중단점을 설정 하 고 변수를 검사할 수 있지만 코드를 편집할 수는 없습니다.

    또는 **도구 > Xamarin Live Player > 라이브 실행 현재 보기**를 선택 하 여 앱을 편집 하 고 장치에서 변경 내용이 발생 하는 것을 볼 수 있습니다. 현재 뷰가 표시 됩니다 (응용 프로그램의 주 화면 대신).

7. 장치가 이미 연결 되어 있고 Xamarin Live Player 앱이 장치에서 실행 되 고 있으면 코드가 바로 실행 됩니다.

    페어링된 장치가 없으면 장치를 페어링 하는 방법에 대 한 지침이 포함 된 QR 코드가 표시 됩니다.

    ![장치 창 페어링](install-images/manage-empty-windows.png)

    페어링을 위해 장치에 연결할 수 없는 경우 오류가 나타날 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="using-xamarin-live-player"></a>Xamarin Live Player 사용

장치에 Xamarin Live Player 앱이 이미 있어야 합니다. 더 이상 다운로드할 수 없습니다.

1. **Mac용 Visual Studio**를 엽니다.
2. **Visual Studio > 기본 설정 ...** 로 이동 하 여 **프로젝트 > Xamarin Live Player (미리 보기)** 탭을 선택 합니다.
3. 틱 **사용 Xamarin Live Player**:

    [![옵션 창에서 사용 Xamarin Live Player 상자를 선택 합니다.](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

4. Xamarin 프로젝트 (또는 [샘플](~/tools/live-player/samples.md))를 만들거나 엽니다.
5. 장치 목록에서 **라이브 플레이어** 를 선택 합니다.

    ![장치 목록에 Xamarin Live Player 옵션이 포함 되어 있습니다.](install-images/devices.png)

    - 이미 장치를 페어링된 경우에는 옵션으로 사용할 수 있습니다.
    - 그렇지 않으면 필요한 경우 장치를 페어링 하 라는 메시지가 표시 됩니다.
    - **Xamarin Live Player 장치 ...** Xamarin Live Player를 사용 하 여 사용 하려는 장치를 관리할 수 있습니다.

6. **실행** 단추를 누르거나 **실행** 또는 오른쪽 클릭 메뉴에서 다음 옵션 중 하나를 선택 합니다.

    ![메뉴 옵션 실행](install-images/run-menu.png)

    - **디버깅 하지 않고 시작** – 앱을 편집 하 고 장치에서 변경이 발생 하는 것을 볼 수 있습니다. 변경 내용이 적용 되 고 파일이 저장 되 면 앱이 다시 시작 됩니다.
    - **디버깅 시작** – 중단점을 설정 하 고 변수를 검사할 수 있지만 코드를 편집할 수는 없습니다.
    - **현재 라이브 실행 보기** – 앱을 편집 하 고 장치에서 변경이 발생 하는 것을 볼 수 있습니다. 현재 뷰가 표시 됩니다 (응용 프로그램의 주 화면 대신).

7. 장치가 이미 연결 되어 있고 Xamarin Live Player 앱이 장치에서 실행 되 고 있으면 코드가 바로 실행 됩니다.

    페어링된 장치가 없으면 장치를 페어링 하는 방법에 대 한 지침이 포함 된 QR 코드가 표시 됩니다.

    ![장치 창 페어링](install-images/manage-empty.png)

    페어링을 위해 장치에 연결할 수 없는 경우 다음과 같은 오류가 표시 됩니다.

    ![장치에 연결할 수 없음 오류 메시지](install-images/error-cannot-connect.png)

-----

문제가 발생 하거나 연결할 수 없는 경우 [제한 사항 및 문제 해결](~/tools/live-player/troubleshooting.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [문제 해결](~/tools/live-player/troubleshooting.md)
