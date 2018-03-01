---
title: "설치 및 요구 사항"
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: a587935e35882ed1dc68817fbbe1ae3e91200f29
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="installation-and-requirements"></a>설치 및 요구 사항

<script> var inspectorOnLoad = function () { var primaryTextBase = "Xamarin Workbooks & Inspector for"; var secondaryTextBase = "or download for"; var inspectorDownloadUrlMac = "https://dl.xamarin.com/interactive/XamarinInteractive.pkg"; var inspectorDownloadUrlWin = "https://dl.xamarin.com/interactive/XamarinInteractive.msi";

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = aPrimary; var aWin = aSecondary; var macTextBase = primaryTextBase; var winTextBase = secondaryTextBase;

  if (/win/i.test(navigator.platform.toLowerCase())) { aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase; }

  aMac.href = inspectorDownloadUrlMac; aMac.text = macTextBase + " Mac"; aWin.href = inspectorDownloadUrlWin; aWin.text = winTextBase + " Windows"; };

document.addEventListener("DOMContentLoaded", inspectorOnLoad);
</script>

## <a name="download-and-installation"></a>다운로드 및 설치

<ol>
  <li>다운로드 및 설치 <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">Xamarin 통합 문서 및 Mac 용 검사기</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">또는 Windows에 대 한 다운로드</a>).
  </li>
  <li><a href="~/tools/inspector/inspect.md"> 사용자 고유의 응용 프로그램을 검사 하는!</a>
    </li>
</ol>

## <a name="requirements"></a>요구 사항

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 이상
- **Windows** -Windows 7 이상 (Internet Explorer 11 이상 및.NET 4.6.1 이상)

### <a name="supported-ides"></a>지원 되는 Ide

- Xamarin Studio 6.2 이상
- Visual Studio Mac 미리 보기 4 이상
- Xamarin 사용한 visual Studio 2015 4.3.x 이상
- Visual Studio 2017 Xamarin 작업과

기업 고객에 대 한 라이브 앱 검사 ´ ù입니다.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>지원 되는 앱 플랫폼

<table>
<thead>
  <tr>
    <th>응용 프로그램 플랫폼</th>
    <th>IDE 지원</th>
    <th>노트</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Mac (통합)</td>
    <td>Mac 에서만 지원 됩니다.</td>
    <td/>
  </tr>
  <tr>
    <td>iOS (통합)</td>
    <td>XS Visual Studio에서 지원 됨</td>
    <td>Windows에서 iOS 앱 검사도 Mac 빌드 호스트에 설치 되는 관리자의 동일한 버전이 필요 합니다.</td>
  </tr>
  <tr>
    <td>Android</td>
    <td>XS Visual Studio에서 지원 됨</td>
    <td>
      <ul>
        <li>Android를 대상으로 해야 > = 4.0.3</li>
        <li>Fastdev 활성화 되어 있어야 합니다</li>
        <li>Google, Visual Studio 또는 Xamarin Android 에뮬레이터를 사용 해야 합니다. Android 에뮬레이터는 7이 이번에는 검사를 사용할 수 없습니다.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Windows에서 Visual Studio 에서만 지원 됩니다.</td>
    <td/>
  </tr>
</tbody>
</table>

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>버그를 보고

Visual Studio를 통해 직접 버그 보고 됩니다.

- **→ 송신 피드백 → 문제 보고 도움말**

다음 정보를 모두 포함 하세요.

### <a name="platform-version-information"></a>플랫폼 버전 정보

이 정보는 필수적입니다.

Mac 용 visual Studio

- **Visual Studio → 세부 정보 → 복사 정보를 표시 하는 방법에 대 한 visual Studio →**
- 버그 보고서에 붙여 넣습니다.

Xamarin Studio

- **Xamarin Studio → 표시 하는 방법에 대 한 Xamarin Studio → → 복사 정보에 설명**
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

- **도움말 → 표시 로그 파일**

Mac 용 visual Studio

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Visual Studio의 내용을 `Output` 창 하지 못할 수 정보를 제공 합니다.

### <a name="project-settings"></a>프로젝트 설정

연결할 수 있는 경우는 `.csproj` 는 검사 하려는 프로젝트에 대 한 매우 도움이 됩니다. 개별 설정에 대 한 요청 보다 쉽습니다.

또한 디버그 구성에 있는지 확인 하십시오.

### <a name="selected-devices"></a>선택한 장치

Android 및 iOS에 대 한 어떤 장치에서 디버깅 하는 검사 하려는 경우 알았으므로 필수적입니다. 알고 있어야 합니다.

- IDE에 표시 된 대로 장치의 이름
- 장치 OS 버전
- Android: x86를 사용 하 고 있는지 확인 하십시오. 에뮬레이터
- Android: 에뮬레이터 플랫폼 사용 중 인가요? Google 에뮬레이터? Visual Studio Android Emulator? Xamarin Android Player?
- 가 제대로 디버깅 하는 응용 프로그램 표시 하 고 장치에서 작동 하는지?
- 장치는 네트워크 연결 (웹 브라우저를 통해 확인)이 있습니까?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>제거

### <a name="windows"></a>Windows

어떻게 통합 문서 및 관리자를 구입한 따라 두 제거 절차를 수행 해야 할 수 있습니다. 둘 다에 소프트웨어를 완전히 제거 하려면 해당 파일을 확인 하십시오.

#### <a name="visual-studio-installer"></a>Visual Studio 설치 관리자

Visual Studio 2017를 설정한 경우 "Visual Studio 설치 관리자" 열고 "개별 구성 요소"에서 "Xamarin 통합 문서" 확인 합니다. 이 옵션을 선택 하는 경우 확인란 선택을 취소 한 다음를 제거 하려면 "수정"을 클릭 합니다.

#### <a name="system-uninstall"></a>시스템 제거

를 설치한 경우 통합 문서 및 관리자 사용자가 직접 다운로드 한 설치 관리자와을 통해 제거 해야 합니다는 **응용 프로그램 및 기능** 시스템 설정 페이지를 통해 또는 Windows 10에서 **프로그램 추가/제거**이전 버전의 Windows의 제어판에서.

> **시작 설정 → → → 시스템 응용 프로그램 및 기능**

![](install-images/windows-remove.png "Xamarin 통합 문서 및 검사기 '응용 프로그램 및 기능'에 나열 된")

**통합 문서가 있는지를 확인 하려면 Visual Studio 설치 관리자에 대 한 절차에 따라 계속 해야 & 검사기 사용자 모르게 다시 가져올 설치 되지 않습니다.**

### <a name="macos"></a>macOS

부터는 [1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/)를 실행 하 여 Xamarin 통합 문서 및 검사기 터미널에서 제거할 수 있습니다.

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

파일 및 디렉터리를 제거 하 고 계속 하기 전에 확인을 요청 합니다 제거 프로그램을 자세히 설명 합니다.

전달 된 `-help` 인수를는 `uninstall` 더 고급 시나리오에 대 한 스크립트입니다.

이전 버전의 경우 다음 항목을 수동으로 제거해야 합니다.

1. `"/Applications/Xamarin Workbooks.app"`에서 Workbooks 앱 삭제
2. `"Applications/Xamarin Inspector.app"`에서 Inspector 앱 삭제
2. `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` 및 `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"` 추가 기능 삭제
3. `/Library/Frameworks/Xamarin.Interactive.framework` 및 `/Library/Frameworks/Xamarin.Inspector.framework`에서 Inspector 및 지원 파일 삭제

