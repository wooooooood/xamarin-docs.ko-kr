---
title: Xamarin Live Player 앱
description: 이 문서에서는 장치에서 라이브 코드 변경을 미리 보는 데 사용할 수 있는 Xamarin Live Player 앱에 대해 설명 합니다. 설정, 샘플, 로그, 설정, 장치 관리 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: conceptdev
ms.author: crdun
ms.date: 06/13/2019
ms.openlocfilehash: d725cb0687cce85f10dbf6915e4eeec785c0ae54
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290209"
---
# <a name="xamarin-live-player-app"></a>Xamarin Live Player 앱

![미리 보기 기능](~/media/shared/preview.png)

> [!WARNING]
> Xamarin Live Player 미리 보기가 종료 되었습니다. 앱을 더 이상 사용할 수 없습니다. 아래 지침은 Visual Studio 2017에서 미리 보기를 계속 사용 하는 고객을 위해 제공 됩니다.

> [!TIP]
> Visual Studio 2019 또는 Mac용 Visual Studio의 [XAML 미리 보기](~/xamarin-forms/xaml/xaml-previewer/index.md) 를 사용 하 여 편집 하는 동안 화면 디자인을 볼 수 있습니다.

시작할 때 Xamarin Live Player 앱은 다음과 같습니다.

![Live Player Android 앱 스크린샷](player-images/app-android-sml.png)

**Visual Studio에 페어링을**누를 때 카메라를 사용 하 여 컴퓨터에 표시 되는 바코드를 검색 합니다.

![Android 바코드 스캐너의 스크린샷](player-images/scan-android-sml.png)

연결에 성공 하면 코드는 거의 즉시 장치에서 실행 되어야 합니다 (예: [계산기 샘플](https://github.com/xamarin/mobile-samples/tree/master/LivePlayer/BasicCalculator)).

![장치에서 실행 되는 샘플 계산기 앱](player-images/basic-calculator-sml.png)

## <a name="options"></a>옵션

앱 **아래쪽의 정보 단추를** 눌러 **옵션** 메뉴를 표시 합니다.

[![옵션 메뉴의 스크린샷](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>로그

로그를 확인 하 여 문제를 진단 합니다.

### <a name="settings"></a>설정

- 컴파일 및 런타임 오류 표시를 전환 합니다.
- 버전 정보입니다.
- 사용자 의견을 보냅니다.

[![설정의 스크린샷](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>장치 관리

장치를 처음으로 연결 하려면 [요구 사항 & 설정](~/tools/live-player/install.md)의 지침을 따르세요. 여러 장치를 페어링 하 고 IDE를 통해 관리할 수 있습니다.

# <a name="visual-studio-2017tabwindows"></a>[Visual Studio 2017](#tab/windows)

Visual Studio에서 **도구 > Xamarin Live Player > 관리** ...를 선택 합니다.

![장치 관리 창](player-images/manage-tools-menu-vs.png)

이 창에서 다음을 수행할 수 있습니다.

- 코드를 검사 하 여 새 장치를 페어링 합니다.
- 또는 화면에 표시 된 코드를 입력 하 여 장치를 페어링 합니다.
- 목록에서 기존 장치를 제거 합니다.

장치 목록에서이 창에 액세스할 수도 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac용 Visual Studio에서 **도구 > (Xamarin Live Player) 장치 관리** ...를 선택 합니다.

![장치 관리 창](player-images/manage-tools-menu.png)

이 창에서 다음을 수행할 수 있습니다.

- 코드를 검사 하 여 새 장치를 페어링 합니다.
- 또는 화면에 표시 된 코드를 입력 하 여 장치를 페어링 합니다.
- 목록에서 기존 장치를 제거 합니다.

![장치 관리 창](player-images/manage.png)

장치 목록에서이 창에 액세스할 수도 있습니다.

![장치 목록에서 Xamarin Live Player 장치를 선택 합니다.](player-images/manage-device-menu.png)

-----

문제가 발생 하는 경우 [제한 사항 및 문제 해결](~/tools/live-player/troubleshooting.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [문제 해결](~/tools/live-player/troubleshooting.md)

