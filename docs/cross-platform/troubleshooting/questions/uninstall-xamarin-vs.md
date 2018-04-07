---
title: 수행 하는 방법 한 철저 한 Visual Studio 용 Xamarin에 대 한 제거?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 49577961026d9895912d2848975e71a9f7eebbd8
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/06/2018
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>수행 하는 방법 한 철저 한 Visual Studio 용 Xamarin에 대 한 제거?


1.  Windows 제어판에서 제공 되는 다음 중 하나를 제거 합니다.

    -   Xamarin
    -   Windows 용 Xamarin
    -   Xamarin.Android
    -   Xamarin.iOS
    -   Visual Studio용 Xamarin

2.  탐색기에서 Xamarin Visual Studio 확장명 폴더에서 나머지 파일을 삭제 (모든 버전을 둘 다를 포함 하 여 _Program Files_ 및 _Program Files (x86)_):

    _C:\\프로그램 파일\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\확장\\Xamarin_

3.  Visual Studio의 MEF 구성 요소 캐시 디렉터리도 삭제 합니다.

    _%LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*.0\\ComponentModelCache_

    사실, 자체적으로이 단계는와 같은 오류를 해결 하는 데 충분 경우가 많습니다.

    -   "'XamarinShellPackage' 패키지가 올바르게 로드 되지 않았습니다"

    -   "... 프로젝트 파일을 열 수 없습니다. 누락 된 프로젝트 하위 형식 "있습니다.

    -   "개체 참조가 개체의 인스턴스로 설정 되지 않았습니다.  at Xamarin.VisualStudio.IOS.XamarinIOSPackage.Initialize()"

    -   "패키지에 대 한 실패 SetSite" (Visual Studio에서 _ActivityLog.xml_)

    -   "패키지에 대 한 실패 LegacySitePackage" (Visual Studio에서 _ActivityLog.xml_)

    (참고 항목에서 [MEF 구성 요소 캐시 지우기](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd) Visual Studio 확장 합니다.  참조 및 [버그 40781, 주석 19](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19) 이러한 오류를 일으킬 수 있는 Visual Studio에서 업스트림 문제에 대 한 좀 더 컨텍스트에 대 한.)

4.  체크 인할 수도 _VirtualStore_ 디렉터리 경우 Windows 수 저장 한 모든 참조를 오버레이 대 한 파일은 _확장\\Xamarin_ 또는 _ComponentModelCache_디렉터리에 있습니다.

    _%LOCALAPPDATA%\\VirtualStore_

5.  레지스트리 편집기를 엽니다 (`regedit`).

6.  다음 키를 찾습니다.

    _HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7.  찾아이 패턴과 일치 하는 모든 항목을 삭제 합니다.

    _C:\\프로그램 파일\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\확장\\Xamarin_

8.  이 키를 찾습니다.

    _HKEY\_CURRENT\_USER\\Software\\Microsoft\\VisualStudio\\1\*.0\\ExtensionManager\\PendingDeletions_

9.  Xamarin 관련 될 수 있습니다 처럼 보이는 항목을 삭제 합니다.  예를 들어 여기의 하나 사용는 이전 버전의 Xamarin에 문제가 발생할 수 있습니다:

    _Mono.VisualStudio.Shell,1.0_

10. 관리자를 열고 `cmd.exe` 명령 프롬프트를 한 다음 실행에서 `devenv /setup` 및 `devenv /updateconfiguration` Visual Studio의 설치 된 각 버전에 대 한 명령입니다.  Visual Studio 2015에 대 한 예를 들어:

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. 다시 부팅합니다.

12. 사용 하 여 Xamarin의 최신 안정화 버전 다시 설치 [visualstudio.com](https://visualstudio.com/xamarin/)합니다.

## <a name="additional-troubleshooting-steps-for-package-did-not-load-correctly"></a>추가 문제 해결 단계 "패키지가 올바르게 로드 되지 않았습니다"에 대 한

위의 단계 "패키지가 올바르게 로드 되지 않았습니다" 오류를 해결 하지 않으면 여기서의 경우에서 시도 하려면 몇 가지 더 많은 단계는 다음과 같습니다.

1.  새 Windows 사용자 계정을 만듭니다.

2.  새 사용자에 대 한 오류 없이 Xamarin Visual Studio 확장을 로드 하는 경우를 확인 합니다.

3.  확장에 올바르게 로드 하는 경우 다음 문제는으로 인 한 가능성이 원래 사용자에 대 한 저장 된 설정 중 일부를:

    -   **탐색기에서** – _% LOCALAPPDATA %\\Microsoft\\VisualStudio\\1\*.0_
    -   **Regedit에서** – _HKEY\_현재\_사용자\\소프트웨어\\Microsoft\\VisualStudio\\1\*.0_
    -   **Regedit에서** – _HKEY\_현재\_사용자\\소프트웨어\\Microsoft\\VisualStudio\\1\*.0\_구성_

4.  이러한 저장 된 설정은 않는 경우 실제로 문제 수, 백업 및 삭제를 시도할 수 있습니다.
