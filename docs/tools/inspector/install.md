---
title: 설치 관리자 및 요구 사항
description: 이 문서에서 Xamarin 검사기를 설치 하는 방법에 설명 하 고 지원 되는 운영 체제, Ide 및 응용 프로그램 플랫폼을 설명 합니다.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: 690329aa1577c66b3aa2794342a8e367477d3a74
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066925"
---
# <a name="inspector-installation-and-requirements"></a>설치 관리자 및 요구 사항

## <a name="download-and-installation"></a>다운로드 및 설치

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. 다운로드 및 설치 [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/) 선택 하 고는 **.NET을 사용한 모바일 개발** 작업 합니다.
1. [로그인](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio) 엔터프라이즈 구독할 수 있도록 합니다.
1. [검사](~/tools/inspector/inspect.md) 위해 자신의 앱!

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. 다운로드 및 설치 [Mac 용 Visual Studio](https://visualstudio.microsoft.com/vs/mac/)합니다.
1. [로그인](https://docs.microsoft.com/visualstudio/mac/activation) 엔터프라이즈 구독할 수 있도록 합니다.
1. [검사](~/tools/inspector/inspect.md) 위해 자신의 앱!

-----

## <a name="requirements"></a>요구 사항

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 이상
- **Windows** -Windows 7 이상 (Internet Explorer 11 이상 및.NET 4.6.1 이상)

### <a name="supported-ides"></a>지원 되는 Ide

- Visual Studio for Mac
- 와 visual Studio 2017 **.NET을 사용한 모바일 개발** 작업

기업 고객에 대 한 라이브 앱 검사 ´ ù입니다.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>지원 되는 앱 플랫폼

|응용 프로그램 플랫폼|IDE 지원|노트|
|--- |--- |--- |
|Mac|Visual Studio에서 Mac에 대해서만 지원|
|iOS|Mac 용 Visual Studio 2017 Visual Studio에서 지원 됨| |
|Android|Mac 용 Visual Studio 2017 Visual Studio에서 지원 됨|Android를 대상으로 해야 > 4.0.3, = **fastdev** 사용 하도록 설정 합니다.<br />Google, Visual Studio 또는 Xamarin Android 에뮬레이터를 사용 해야 합니다. Android 에뮬레이터는 7이 이번에는 검사를 사용할 수 없습니다.|
|WPF|Visual Studio 2017 에서만 지원 됩니다.|

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>버그를 보고

Visual Studio를 통해 직접 버그 보고 됩니다.

- **도움말 > 의견을 보내 > 문제 보고**

다음 정보를 모두 포함 하세요.

### <a name="platform-version-information"></a>플랫폼 버전 정보

이 정보는 필수적입니다.

Mac 용 visual Studio

- **Visual Studio > Visual Studio에 대해 > 자세한 정보 표시 > 정보 복사**
- 버그 보고서에 붙여 넣습니다.

Visual Studio

- **도움말 > Visual Studio에 대해 > 정보 복사**
- 운영 체제 버전과 32 비트 또는 64 비트 Windows를 실행 하는지 여부를 알려 주십시오.

### <a name="log-files"></a>로그 파일

항상 IDE와 검사기 클라이언트 로그 파일을 첨부 합니다.

검사기 클라이언트

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

또한 1.4.x 주 메뉴에서 Finder (macOS) 또는 직접 탐색기 (Windows) 로그 파일을 선택할 수 있는 기능을 제공 합니다.

- **도움말 > 로그 파일 표시**

Mac 용 visual Studio

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Visual Studio의 내용을 **출력** 창 하지 못할 수 정보를 제공 합니다.

### <a name="project-settings"></a>프로젝트 설정

연결할 수 있는 경우는 **.csproj** 는 검사 하려는 프로젝트에 대 한 매우 도움이 됩니다. 개별 설정에 대 한 요청 보다 쉽습니다.

또한 디버그 구성에 있는지 확인 하십시오.

### <a name="selected-devices"></a>선택한 장치

Android 및 iOS에 대 한 어떤 장치에서 디버깅 하는 검사 하려는 경우 알았으므로 필수적입니다. 알고 있어야 합니다.

- IDE에 표시 된 대로 장치의 이름
- 장치 OS 버전
- Android: x86를 사용 하 고 있는지 확인 하십시오. 에뮬레이터
- Android: 에뮬레이터 플랫폼 사용 중 인가요? Google 에뮬레이터? Visual Studio의 Android 에뮬레이터? Xamarin Android Player?
- 가 제대로 디버깅 하는 응용 프로그램 표시 하 고 장치에서 작동 하는지?
- 장치는 네트워크 연결 (웹 브라우저를 통해 확인)이 있습니까?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new
