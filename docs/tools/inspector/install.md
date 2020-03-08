---
title: 검사기 설치 및 요구 사항
description: 이 문서에서는 Xamarin Inspector를 설치 하 고 지원 되는 운영 체제, Ide 및 앱 플랫폼에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: davidortinau
ms.author: daortin
ms.date: 06/19/2018
ms.openlocfilehash: 19c4a15fb2490c7bace4798b0cb8e062b1379a04
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78917590"
---
# <a name="inspector-installation-and-requirements"></a>검사기 설치 및 요구 사항

## <a name="download-and-installation"></a>다운로드 및 설치

# <a name="windows"></a>[Windows](#tab/windows)

1. [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/) 를 다운로드 하 여 설치 하 고 .net 워크 로드 **를 사용 하 여 모바일 개발** 을 선택 합니다.
1. [로그인](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio) 하 여 엔터프라이즈 구독을 사용 하도록 설정 합니다.
1. 응용 프로그램을 [검사](~/tools/inspector/inspect.md) 합니다.

# <a name="macos"></a>[macOS](#tab/macos)

1. [Mac용 Visual Studio](https://visualstudio.microsoft.com/vs/mac/)를 다운로드 하 고 설치 합니다.
1. [로그인](https://docs.microsoft.com/visualstudio/mac/activation) 하 여 엔터프라이즈 구독을 사용 하도록 설정 합니다.
1. 응용 프로그램을 [검사](~/tools/inspector/inspect.md) 합니다.

-----

## <a name="requirements"></a>요구 사항

### <a name="supported-operating-systems"></a>지원되는 운영 체제

- **Mac** -OS X 10.11 이상
- **Windows** -windows 7 이상 (Internet Explorer 11 이상 및 .net 4.6.1 이상)

### <a name="supported-ides"></a>지원 되는 Ide

- Mac용 Visual Studio
- .NET 워크 로드 **를 사용한 모바일 개발** 에 Visual Studio 2017

라이브 앱 검사는 기업 고객에 게 제공 됩니다.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>지원 되는 앱 플랫폼

|앱 플랫폼|IDE 지원|메모|
|--- |--- |--- |
|Mac|에서만 지원 Mac용 Visual Studio|
|iOS|Visual Studio 2017 및 Mac용 Visual Studio에서 지원 됨| 링커 동작을 **연결 안 함** 으로 설정 해야 합니다 ( **iOS 빌드** 프로젝트 옵션 아래). |
|Android|Visual Studio 2017 및 Mac용 Visual Studio에서 지원 됨|**Fastdev** 를 사용 하도록 설정한 상태에서 Android > = 4.0.3를 대상으로 해야 합니다.<br />Google, Visual Studio 또는 Xamarin Android 에뮬레이터를 사용 해야 합니다. Android 7 에뮬레이터는 현재 검사를 허용 하지 않을 수 있습니다.|
|WPF|Visual Studio 2017 에서만 지원 됨|

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>버그 보고

Visual Studio를 통해 직접 버그를 보고 해야 합니다.

- **사용자 의견을 보내 > 하 > 문제를 보고 하는 데 도움이 됩니다.**

다음 정보를 모두 포함 해 주세요.

### <a name="platform-version-information"></a>플랫폼 버전 정보

이 정보는 매우 중요 합니다.

Mac 용 Visual Studio

- **Visual studio > > 정보를 > 복사 정보를 표시 합니다.**
- 버그 보고서에 붙여넣기

Visual Studio

- **Visual Studio > 정보 복사에 대 한 도움말 >**
- 운영 체제 버전 및 32 비트 또는 64 비트 Windows를 실행 하 고 있는지 여부를 알려주세요.

### <a name="log-files"></a>로그 파일

항상 IDE와 검사기 클라이언트 로그 파일을 모두 연결 합니다.

검사기 클라이언트

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4. x에는 주 메뉴에서 직접 Finder (macOS) 또는 탐색기 (Windows)의 로그 파일을 선택 하는 기능도 있습니다.

- **로그 파일을 표시 > 도움말**

Mac 용 Visual Studio

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Visual Studio **출력** 창의 내용도 정보를 받을 수 있습니다.

### <a name="project-settings"></a>프로젝트 설정

검사 하려는 프로젝트에 **.csproj** 를 연결할 수 있으면 매우 유용 합니다. 이는 개별 설정에 대 한 정보를 요청 하는 것 보다 쉽습니다.

또한 디버그 구성에 있는지 확인 하세요.

### <a name="selected-devices"></a>선택한 장치

Android 및 iOS의 경우에는 검사 하려는 장치를 확인 하는 것이 중요 합니다. 다음 사항을 알고 있어야 합니다.

- IDE에 표시 된 장치 이름
- 장치의 OS 버전
- Android: x86 에뮬레이터를 사용 하 고 있는지 확인
- Android: 어떤 에뮬레이터 플랫폼을 사용 하 고 있나요? Google Emulator? Visual Studio Android Emulator? Xamarin Android Player?
- 디버깅 하 고 있는 앱이 장치에 제대로 표시 되 고 작동 하나요?
- 장치가 네트워크에 연결 되어 있나요 (웹 브라우저를 통해 확인)?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new
