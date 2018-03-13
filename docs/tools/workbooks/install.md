---
title: "설치 및 요구 사항"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9D4E10E8-A288-4C6C-9475-02969198C119
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: abc9f9402b55a11e313b9938f07f37e5329b55b6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="installation-and-requirements"></a>설치 및 요구 사항

<script> var inspectorOnLoad = function () { var primaryTextBase = "Xamarin Workbooks for"; var secondaryTextBase = "or download for"; var inspectorDownloadUrlMac = "https://dl.xamarin.com/interactive/XamarinInteractive.pkg"; var inspectorDownloadUrlWin = "https://dl.xamarin.com/interactive/XamarinInteractive.msi";

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = aPrimary; var aWin = aSecondary; var macTextBase = primaryTextBase; var winTextBase = secondaryTextBase;

  if (/win/i.test(navigator.platform.toLowerCase())) { aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase; }

  aMac.href = inspectorDownloadUrlMac; aMac.text = macTextBase + " Mac"; aWin.href = inspectorDownloadUrlWin; aWin.text = winTextBase + " Windows"; };

document.addEventListener("DOMContentLoaded", inspectorOnLoad);
</script>

<a name="install" />

## <a name="download-and-install"></a>다운로드 및 설치

<ol>
  <li>확인 된 <a href="#Requirements"> 요구 사항</a> 아래 합니다.</li>
  <li>다운로드 및 설치 <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">Mac 용 Xamarin 통합 문서</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">또는 Windows에 대 한 다운로드</a>).
  </li>
  <li>시작 <a href="~/tools/workbooks/workbook.md"> 리스트</a> 통합 문서 또는 out 시도로 <a href="https://developer.xamarin.com/workbooks/">샘플</a>합니다.
    </li>
</ol>

## <a name="requirements"></a>요구 사항

#### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 이상
- **Windows** -Windows 7 이상 (Internet Explorer 11 이상 및.NET 4.6.1 이상)

#### <a name="supported-app-platforms"></a>지원 되는 앱 플랫폼

<table>
<thead>
  <tr>
    <th>응용 프로그램 플랫폼</th>
    <th>운영 체제 지원</th>
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
    <td>Mac 및 Windows에서 지원</td>
    <td>
      <ul>
        <li>Xamarin.iOS 11.0 및 Xcode 9.0 이상 mac 설치 되어야 합니다.</li>
        <li>Windows에서 iOS 통합 문서를 실행 하려면 위의 모든 실행 하는 Mac 빌드 호스트 및 <a href="~/tools/ios-simulator.md">원격 iOS 시뮬레이터</a> Windows에 설치 합니다.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Mac 및 Windows에서 지원</td>
    <td>가상 장치에 Google, Visual Studio 또는 Xamarin Android 에뮬레이터를 사용 해야 > = 5.0 인 경우</td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Windows 에서만 지원 됩니다.</td>
    <td/>
  </tr>
  <tr>
    <td>콘솔 (.NET Framework)</td>
    <td>Mac 및 Windows에서 지원</td>
    <td/>
  </tr>
  <tr>
    <td>콘솔 (.NET 코어)</td>
    <td>Mac 및 Windows에서 지원</td>
    <td/>
  </tr>
</tbody>
</table>

## <a name="reporting-bugs"></a>버그를 보고

하십시오 [GitHub에서 문제를 보고][bugs], 다음 정보를 모두 포함 합니다.

### <a name="log-files"></a>로그 파일

항상 통합 문서 클라이언트 로그 파일을 연결 합니다.

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

또한 1.4.x 주 메뉴에서 Finder (macOS) 또는 직접 탐색기 (Windows) 로그 파일을 선택할 수 있는 기능을 제공 합니다.

- **도움말 → 표시 로그 파일**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>통합 문서 1.3 및 이전 버전의 로그 경로:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>플랫폼 버전 정보

운영 체제에 대 한 세부 정보를 파악 하는 데 유용 하 게 사용 되며 Xamarin 제품을 설치 합니다.

통합 문서에서 주 메뉴:

* **도움말 복사본 버전 정보 →**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>통합 문서 1.3 및 이전 버전의 지침:

Mac 용 visual Studio

- **Visual Studio → 세부 정보 → 복사 정보를 표시 하는 방법에 대 한 visual Studio →**
- 버그 보고서에 붙여 넣습니다.

Visual Studio

- **→ Visual Studio → 정보 복사에 대 한 도움말**
- 운영 체제 버전과 32 비트 또는 64 비트 Windows를 실행 하는지 여부를 알려 주십시오.

### <a name="samples"></a>샘플

연결 하거나 연결할 수 있는 경우는 `.workbooks` 보다 신속 하 게 버그를 해결 하는 데 도움은 문제가 있는, 파일입니다.

### <a name="devices"></a>장치

IOS 또는 Android 통합 문서 연결에 문제가 이미 체크 아웃 하는 경우 [품질 문제 해결 페이지](~/tools/workbooks/troubleshooting/index.md)를 알고 있어야 합니다.

- 장치에 연결 하는 이름
- 장치 OS 버전
- Android: x86를 사용 하 고 있는지 확인 하십시오. 에뮬레이터
- Android: 에뮬레이터 플랫폼 사용 중 인가요? Google 에뮬레이터?
  Visual Studio Android Emulator? Xamarin Android Player?
- Windows에서 iOS: 어떤 버전의 Xamarin 원격 iOS 시뮬레이터의 수행을 설치한 상태 (확인 `Add/Remove Programs` 에 `Control Panel`)?
- Windows에서 iOS: 하십시오 Mac 빌드 호스트에도 플랫폼 버전 정보를 제공
- 장치는 네트워크 연결 (웹 브라우저를 통해 확인)이 있습니까?

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>제거

### <a name="windows"></a>Windows

어떻게 통합 문서 및 관리자를 구입한 따라 두 제거 절차를 수행 해야 할 수 있습니다. 둘 다에 소프트웨어를 완전히 제거 하려면 해당 파일을 확인 하십시오.

#### <a name="visual-studio-installer"></a>Visual Studio 설치 관리자

Visual Studio 2017 있으면 열 **Visual Studio 설치 관리자**, 찾는 위치 및 **개별 구성 요소** 에 대 한 **Xamarin 통합 문서**합니다. 이 옵션을 선택 하는 확인란 선택을 취소 한 다음 클릭 **수정** 를 제거 합니다.

#### <a name="system-uninstall"></a>시스템 제거

를 설치한 경우 통합 문서 및 관리자 사용자가 직접 다운로드 한 설치 관리자와을 통해 제거 해야 합니다는 **응용 프로그램 및 기능** 시스템 설정 페이지를 통해 또는 Windows 10에서 **프로그램 추가/제거**이전 버전의 Windows의 제어판에서.

> **시작 설정 → → → 시스템 응용 프로그램 및 기능**

![](install-images/windows-remove.png "Xamarin 통합 문서 및 관리자에 나열 된 &quot;앱 &amp; 기능&quot;")

**통합 문서가 있는지를 확인 하려면 Visual Studio 설치 관리자에 대 한 절차에 따라 계속 해야 & 검사기 사용자 모르게 다시 가져올 설치 되지 않습니다.**

<a name="uninstall-macos" />

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

## <a name="downgrading"></a>다운 그레이드

에 대 한 번들 식별자 `/Applications/Xamarin Workbooks.app` 에서 변경 `com.xamarin.Inspector` 를 `com.xamarin.Workbooks` 1.4 릴리스에서 Xamarin 통합 문서 및 검사기 설치 관리자의 향후 분할을 용이 하 게 합니다.

이전 설치 관리자의 버그 때문에는 1.3.2 또는 이전 설치 관리자를 사용 하 여 1.4 이상 릴리스를 다운 그레이드할 수 없습니다.

1.4 또는 1.3.2 최신 또는 이전 버전 다운 그레이드 합니다 하:

1. [통합 문서 및 검사기를 수동으로 제거](#macOS)
2. 1.3.2 실행 이전 또는 `.pkg` 설치 관리자