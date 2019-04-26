---
title: 관리자 설치 및 요구 사항
description: 이 문서는 Xamarin Inspector를 설치 하는 방법에 설명 하 고 지원 되는 운영 체제, Ide 및 앱 플랫폼에 설명 합니다.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: 2bbf0bda42b7bce483d9d036ebf39314dcb73072
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61373471"
---
# <a name="inspector-installation-and-requirements"></a>관리자 설치 및 요구 사항

## <a name="download-and-installation"></a>다운로드 및 설치

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. 다운로드 및 설치 [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/) 선택 합니다 **.NET을 사용한 모바일 개발** 워크 로드.
1. [로그인](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio) Enterprise 구독을 사용 하도록 설정 합니다.
1. [검사](~/tools/inspector/inspect.md) 앱!

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. 다운로드 및 설치 [Mac 용 Visual Studio](https://visualstudio.microsoft.com/vs/mac/)합니다.
1. [로그인](https://docs.microsoft.com/visualstudio/mac/activation) Enterprise 구독을 사용 하도록 설정 합니다.
1. [검사](~/tools/inspector/inspect.md) 앱!

-----

## <a name="requirements"></a>요구 사항

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 이상
- **Windows** -Windows 7 이상 (Internet Explorer 11 이상 및.NET 4.6.1 이상)

### <a name="supported-ides"></a>지원 되는 Ide

- Visual Studio for Mac
- Visual Studio 2017 **.NET을 사용한 모바일 개발** 워크 로드

라이브 앱 검사는 엔터프라이즈 고객이 사용할 수 있습니다.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>지원 되는 앱 플랫폼

|앱 플랫폼|IDE 지원|노트|
|--- |--- |--- |
|Mac|Mac 용 Visual Studio 에서만 지원|
|iOS|Mac 용 Visual Studio 2017 및 Visual Studio에서 지원| 링커 동작으로 설정 되어 있어야 **연결 하지 않음** (아래 **iOS 빌드** 프로젝트 옵션) |
|Android|Mac 용 Visual Studio 2017 및 Visual Studio에서 지원|Android 대상 > 4.0.3을 사용 하 여 = **fastdev** 사용 하도록 설정 합니다.<br />Google, Visual Studio 또는 Xamarin Android 에뮬레이터를 사용 해야 합니다. Android 7 에뮬레이터에는이 이번에는 검사를 사용할 수 없습니다.|
|WPF|Visual Studio 2017 에서만 지원|

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>버그 보고

Visual Studio를 통해 직접 버그를 보고 해야 합니다.

- **도움말 > 피드백 > 문제 보고**

다음 정보를 모두 포함 하세요.

### <a name="platform-version-information"></a>플랫폼 버전 정보

이 정보는 중요 합니다.

Visual Studio For Mac

- **Visual Studio > Visual Studio에 대 한 > 세부 정보 표시 > 정보 복사**
- 버그 보고서에 붙여 넣습니다.

Visual Studio

- **도움말 > Visual Studio에 대 한 > 정보 복사**
- 운영 체제 버전 및 32 비트 또는 64 비트 Windows를 실행 중인지 여부를 알려 주세요.

### <a name="log-files"></a>로그 파일

항상 IDE와 검사기 클라이언트 로그 파일을 연결 합니다.

관리자 클라이언트

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

또한 1.4.x Finder (macOS) 또는 직접 탐색기 (Windows)에서 주 메뉴에서 로그 파일을 선택 하는 기능을 기능:

- **도움말 > 로그 파일 표시**

Visual Studio For Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Visual Studio의 내용을 **출력** 창도 않을 정보를 제공 합니다.

### <a name="project-settings"></a>프로젝트 설정

연결 수를 **.csproj** 검사 하려는 프로젝트에 대해 매우 유용할 것입니다. 개별 설정에 대 한 질문 보다 쉽습니다.

또한 디버그 구성에 있는지 확인 하세요.

### <a name="selected-devices"></a>선택한 장치

Android 및 iOS에 대 한 것이 중요에서 디버깅 하는 검사 하려는 경우 어떤 장치는 알 수 있습니다. 알고 있어야 합니다.

- IDE에 표시 된 대로 장치의 이름
- 장치 OS 버전
- Android: X86을 사용 하 고 있는지 확인 합니다. 에뮬레이터
- Android: 에뮬레이터 플랫폼을 사용 중 입니까? Google 에뮬레이터? Visual Studio Android Emulator? Xamarin Android Player?
- 올바르게 디버깅 하는 앱을 표시 하 고 장치에서 작동?
- 장치는 네트워크 연결 (웹 브라우저를 통해 확인)가 있습니까?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new
