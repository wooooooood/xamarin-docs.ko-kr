---
title: 통합 설치 및 요구 사항
description: 이 문서에는 다운로드 하 여 Xamarin Workbooks, 지원 되는 플랫폼 및 시스템 요구 사항 논의 설치 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 9D4E10E8-A288-4C6C-9475-02969198C119
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: 7bef5de57b7ac709ebab4c39feedbec369e6bd14
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122428"
---
# <a name="workbooks-installation-and-requirements"></a>통합 설치 및 요구 사항

<a name="install" />

## <a name="download-and-install"></a>다운로드 및 설치

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. 확인 합니다 [요구 사항](#requirements) 아래.
2. 다운로드 및 설치 [Xamarin 통합 문서에 대 한 Windows](https://dl.xamarin.com/interactive/XamarinInteractive.msi)합니다.
3. 시작 [생길](~/tools/workbooks/workbook.md) 통합 문서에 사용 된 [샘플](https://developer.xamarin.com/workbooks)

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. 확인 합니다 [요구 사항](#Requirements) 아래.
2. 다운로드 및 설치 [Mac 용 Xamarin Workbooks](https://dl.xamarin.com/interactive/XamarinInteractive.pkg)합니다.
3. 시작 [생길](~/tools/workbooks/workbook.md) 통합 문서에 사용 된 [샘플](https://developer.xamarin.com/workbooks)

-----

## <a name="requirements"></a>요구 사항

#### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 이상
- **Windows** -Windows 7 이상 (Internet Explorer 11 이상 및.NET 4.6.1 이상)

#### <a name="supported-app-platforms"></a>지원 되는 앱 플랫폼

|앱 플랫폼|OS 지원|노트|
|--- |--- |--- |
|Mac|Mac 에서만 지원|
|iOS|Mac 및 Windows에서 지원|Mac에서 Xamarin.iOS 11.0 및 Xcode 9.0 이상 설치 해야 합니다. 위의 모든를 실행 하는 Mac 빌드 호스트를 Windows에서 iOS 통합 문서를 실행 하려면 하며 [원격 iOS 시뮬레이터](~/tools/ios-simulator/index.md) Windows에 설치 합니다.|
|Android|Mac 및 Windows에서 지원|가상 장치를 사용 하 여 Google, Visual Studio 또는 Xamarin Android 에뮬레이터를 사용 해야 > 5.0 =|
|WPF|Windows 에서만 지원|
|콘솔 (.NET Framework)|Mac 및 Windows에서 지원|
|콘솔 (.NET Core)|Mac 및 Windows에서 지원|


## <a name="reporting-bugs"></a>버그 보고

하세요 [GitHub에서 문제를 보고][bugs], 다음 정보를 모두 포함 합니다.

### <a name="log-files"></a>로그 파일

항상 통합 문서 클라이언트 로그 파일을 첨부 합니다.

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

또한 1.4.x Finder (macOS) 또는 직접 탐색기 (Windows)에서 주 메뉴에서 로그 파일을 선택 하는 기능을 기능:

- **도움말 > 로그 파일 표시**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>통합 문서 1.3 및 이전 버전의 로그 경로:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>플랫폼 버전 정보

운영 체제에 대 한 내용은 매우 유용 하 고 Xamarin 제품을 설치 합니다.

주 메뉴에서 통합 문서:

* **도움말 > 버전 정보 복사**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>통합 문서 1.3 및 이전 버전 지침:

Mac 용 visual Studio

- **Visual Studio > Visual Studio에 대 한 > 세부 정보 표시 > 정보 복사**
- 버그 보고서에 붙여 넣습니다.

Visual Studio

- **도움말 > Visual Studio에 대 한 > 정보 복사**
- 운영 체제 버전 및 32 비트 또는 64 비트 Windows를 실행 중인지 여부를 알려 주세요.

### <a name="samples"></a>샘플

연결 하거나 연결할 수 있으면 합니다 **.workbooks** 버그를 더 빠르게 해결 하는 데 도움이 되는 파일을 사용 하 여 문제가 있는 합니다.

### <a name="devices"></a>장치

IOS 또는 Android 통합 문서 연결 문제가 있는 경우 이미 체크 아웃 [문제 해결 페이지](~/tools/workbooks/troubleshooting/index.md)를 알고 있어야 합니다.

- 에 연결 하려는 장치의 이름
- 장치 OS 버전
- Android: x86을 사용 하 고 있는지 확인 에뮬레이터
- Android: 에뮬레이터 플랫폼 사용 중 입니까? Google 에뮬레이터?
  Visual Studio Android Emulator? Xamarin Android Player?
- Windows에서 iOS: Xamarin 원격 ios 시뮬레이터 버전 수행 설치한 (체크 **프로그램 추가/제거** 에 **제어판**)?
- Windows에서 iOS: 하세요 Mac 빌드 호스트에도 플랫폼 버전 정보를 제공
- 장치는 네트워크 연결 (웹 브라우저를 통해 확인)가 있습니까?

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>제거

### <a name="windows"></a>Windows

통합 문서를 구입한 방법에 따라 두 제거 절차를 수행 해야 합니다. 소프트웨어를 완전히 제거 하려면 두 가지 모두를 확인 하세요.

#### <a name="visual-studio-installer"></a>Visual Studio 설치 관리자

Visual Studio 2017을 사용 하는 경우 열 **Visual Studio 설치 관리자**, 및 찾는 위치 **개별 구성 요소** 에 대 한 **Xamarin Workbooks**합니다. 이 옵션을 선택 하는 경우 선택 취소 하 고 클릭 **수정** 를 제거 합니다.

#### <a name="system-uninstall"></a>시스템 제거

직접 설치한 경우 통합 문서를 다운로드 한 설치 관리자를 사용 하 여를 통해 제거 해야 합니다는 **앱 및 기능** 시스템 설정 페이지를 통해 Windows 10 켜거나 **프로그램 추가/제거** 컨트롤에서 이전 버전의 Windows에서 패널입니다.

> **시작 > 설정 > 시스템 > 앱 및 기능**

![](install-images/windows-remove.png "에 나열 된 Xamarin Workbooks &quot;앱 &amp; 기능&quot;")

**여전히 사용자 모르게 통합 문서를 다시 가져올지 않습니다 되도록 Visual Studio 설치 관리자를 절차를 수행 해야 합니다.**

<a name="uninstall-macos" />

### <a name="macos"></a>macOS

부터는 [1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/)를 실행 하 여 터미널에서 Xamarin Workbooks를 제거할 수 있습니다.

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

제거 프로그램에서는 파일 및 디렉터리를 제거 하 고 계속 하기 전에 확인을 자세히 설명 합니다.

전달 합니다 `-help` 인수는 `uninstall` 고급 시나리오에 대 한 스크립트입니다.

이전 버전의 경우 다음 항목을 수동으로 제거해야 합니다.

1. `"/Applications/Xamarin Workbooks.app"`에서 Workbooks 앱 삭제
2. `"Applications/Xamarin Inspector.app"`에서 Inspector 앱 삭제
2. `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` 및 `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"` 추가 기능 삭제
3. `/Library/Frameworks/Xamarin.Interactive.framework` 및 `/Library/Frameworks/Xamarin.Inspector.framework`에서 Inspector 및 지원 파일 삭제

## <a name="downgrading"></a>다운 그레이드

번들 식별자에 대 한 **응용 프로그램/Xamarin Workbooks.app** 에서 변경 `com.xamarin.Inspector` 하려면 `com.xamarin.Workbooks` 1.4 릴리스에서 Workbooks 및 Inspector로 이제 완벽 하 게 분할 됩니다.

이전 설치 관리자의 버그 때문이 아닌 경우는 1.3.2 또는 이전 설치 관리자를 사용 하 여 1.4 이상 릴리스를 다운 그레이드할 수

1.4 또는 1.3.2 최신 또는 이전 버전에서 다운 그레이드 합니다.

1. [Workbooks 및 Inspector를 수동으로 제거](#macOS)
2. 1.3.2 실행 이전 또는 `.pkg` 설치 관리자
