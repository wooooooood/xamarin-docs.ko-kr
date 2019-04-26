---
title: Visual Studio용 Xamarin을 완전히 제거하려면 어떻게 할까요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 99fde9330498ee62d3cf6b5910c2cbfae39cfdeb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61159625"
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>Visual Studio용 Xamarin을 완전히 제거하려면 어떻게 할까요?


1.  Windows 제어판에서 표시 되는 다음 중 하나를 제거 합니다.

    -   Xamarin
    -   Windows용 Xamarin
    -   Xamarin.Android
    -   Xamarin.iOS
    -   Visual Studio용 Xamarin

2.  탐색기에서 Xamarin Visual Studio 확장 폴더에서 나머지 파일을 삭제 (모든 버전에 모두 포함 _Program Files_ 하 고 _프로그램 파일 (x86)_):

    _C:\\프로그램 파일\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\확장\\Xamarin_

3.  Visual Studio의 MEF 구성 요소 캐시 디렉터리도 삭제 합니다.

    _%LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*.0\\ComponentModelCache_

    사실, 자체적으로이 단계와 같은 오류를 해결 하려면 충분 한 경우가:

    -   "'XamarinShellPackage' 패키지가 올바르게 로드 되지 않았습니다"

    -   "프로젝트 파일... 열 수 없습니다. 누락 된 프로젝트 하위 형식이 있는 "

    -   "개체 참조가 개체의 인스턴스로 설정 되지 않았습니다.  at Xamarin.VisualStudio.IOS.XamarinIOSPackage.Initialize()"

    -   "패키지에 대 한 실패 SetSite" (Visual studio의 _ActivityLog.xml_)

    -   "패키지에 대 한 실패 LegacySitePackage" (Visual studio의 _ActivityLog.xml_)

    (참고 합니다 [MEF 구성 요소 캐시를 지우기](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd) Visual Studio 확장입니다.  내용과 [버그 40781, 주석 19](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19) 이러한 오류를 일으킬 수 있는 Visual Studio에서 업스트림 문제에 대 한 좀 더 컨텍스트에 대 한 합니다.)

4.  체크 인할 수도 합니다 _VirtualStore_ 하는 경우 Windows 수 저장 했는지 확인 하려면 디렉터리 오버레이 대 한 파일을 _확장\\Xamarin_ 또는 _ComponentModelCache_디렉터리에 있습니다.

    _%LOCALAPPDATA%\\VirtualStore_

5.  레지스트리 편집기를 엽니다 (`regedit`).

6.  다음 키를 찾습니다.

    _HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7.  이 패턴과 일치하는 항목을 찾아 삭제합니다.

    _C:\\프로그램 파일\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\확장\\Xamarin_

8.  이 키를 찾습니다.

    _HKEY\_CURRENT\_USER\\Software\\Microsoft\\VisualStudio\\1\*.0\\ExtensionManager\\PendingDeletions_

9.  Xamarin과 관련된 것으로 보이는 모든 항목을 삭제합니다.  예를 들어, 다음의 하나는 하는 데 이전 버전의 Xamarin에 문제를 일으킬:

    _Mono.VisualStudio.Shell,1.0_

10. 관리자를 엽니다 `cmd.exe` 명령 프롬프트를 한 다음 실행 합니다 `devenv /setup` 및 `devenv /updateconfiguration` Visual Studio의 설치 된 각 버전에 대 한 명령입니다.  예를 들어, Visual Studio 2015의 경우:

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. 다시 부팅합니다.

12. 현재 안정적인 버전의 Xamarin에서 사용 하 여 다시 [visualstudio.com](https://visualstudio.com/xamarin/)합니다.

## <a name="additional-troubleshooting-steps-for-package-did-not-load-correctly"></a>문제 해결 단계 "패키지가 올바르게 로드 되지 않았습니다"에 대 한 추가

위의 단계를 "패키지가 올바르게 로드 되지 않았습니다" 오류 해결 되지 않는 경우에는 몇 단계가 더 시도 다음과 같습니다.

1.  새 Windows 사용자 계정을 만듭니다.

2.  새 사용자에 대 한 오류 없이 Xamarin Visual Studio 확장을 로드 하는 경우를 확인 합니다.

3.  확장이 올바르게 로드 하는 경우 다음 문제 가능성이 인 원래 사용자에 대 한 저장 된 설정의 일부:

    -   **탐색기에서** – _% LOCALAPPDATA %\\Microsoft\\VisualStudio\\1\*.0_
    -   **Regedit에서** – _HKEY\_현재\_사용자\\소프트웨어\\Microsoft\\VisualStudio\\1\*.0_
    -   **Regedit에서** – _HKEY\_현재\_사용자\\소프트웨어\\Microsoft\\VisualStudio\\1\*.0\_구성_

4.  이러한 저장 된 설정을 수행 실제로 표시 되 면 문제를 백업 하 고 다음 삭제를 시도할 수 있습니다.
