---
title: Visual Studio용 Xamarin을 완전히 제거하려면 어떻게 할까요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
ms.openlocfilehash: ed0171c2b6bd98e5b29ec100d0235131d36acb05
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013335"
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>Visual Studio용 Xamarin을 완전히 제거하려면 어떻게 할까요?

1. Windows 제어판에서 존재 하는 다음 중 하나를 제거 합니다.

    - Xamarin
    - Windows용 Xamarin
    - Xamarin.Android
    - Xamarin.iOS
    - Visual Studio용 Xamarin

2. 탐색기에서 Xamarin Visual Studio 확장 폴더 ( _Program files_ 및 _program files (x86)_ 를 포함 한 모든 버전)에서 나머지 파일을 삭제 합니다.

    _C:\\Program Files\*\\Microsoft Visual Studio 1\*.0\\Common7\\Xamarin\\Xamarin_

3. Visual Studio의 MEF 구성 요소 캐시 디렉터리도 삭제 합니다.

    _% LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*\\ComponentModelCache_

    실제로이 단계 자체는 다음과 같은 오류를 해결 하기에 충분 한 경우가 많습니다.

    - "' XamarinShellPackage ' 패키지가 제대로 로드 되지 않았습니다."

    - "프로젝트 파일 ... 을 열 수 없습니다. 프로젝트 하위 형식이 누락 되었습니다. "

    - "개체 참조가 개체의 인스턴스로 설정 되지 않았습니다.  VisualStudio. XamarinIOSPackage () "

    - "SetSite failed for package" (Visual Studio의 _Activitylog .xml_)

    - "LegacySitePackage failed for package" (Visual Studio의 _Activitylog .xml_)

    ( [CLEAR MEF 구성 요소 캐시](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd) Visual Studio 확장도 참조 하세요.  이러한 오류를 일으킬 수 있는 Visual Studio의 업스트림 문제에 대 한 자세한 컨텍스트는 [버그 40781, 주석 19를](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19) 참조 하세요.

4. 또한 _Virtualstore_ 디렉터리를 확인 하 여 Windows에서 Xamarin 또는 _componentmodelcache_ 디렉터리 _\\확장_ 에 대 한 오버레이 파일을 저장 했는지 확인 합니다.

    _% LOCALAPPDATA%\\VirtualStore_

5. 레지스트리 편집기 (`regedit`)를 엽니다.

6. 다음 키를 찾습니다.

    _HKEY\_로컬\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7. 이 패턴과 일치하는 항목을 찾아 삭제합니다.

    _C:\\Program Files\*\\Microsoft Visual Studio 1\*.0\\Common7\\Xamarin\\Xamarin_

8. 이 키를 찾습니다.

    _HKEY\_현재\_사용자\\소프트웨어\\Microsoft\\VisualStudio\\1\*.0\\ExtensionManager\\PendingDeletions_

9. Xamarin과 관련된 것으로 보이는 모든 항목을 삭제합니다.  예를 들어 다음은 이전 버전의 Xamarin에서 문제를 발생 시키는 데 사용 되는 것입니다.

    _VisualStudio, 1.0_

10. 관리자 `cmd.exe` 명령 프롬프트를 열고 설치 된 각 버전의 Visual Studio에 대 한 `devenv /setup` 및 `devenv /updateconfiguration` 명령을 실행 합니다.  예를 들어, Visual Studio 2015의 경우:

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. 다시 부팅합니다.

12. [Visualstudio.com](https://visualstudio.com/xamarin/)에서를 사용 하 여 현재 안정적인 버전의 Xamarin을 다시 설치 합니다.

## <a name="additional-troubleshooting-steps-for-package-did-not-load-correctly"></a>"패키지가 제대로 로드 되지 않았습니다."에 대 한 추가 문제 해결 단계

위의 단계를 수행 하 여 "패키지가 제대로 로드 되지 않았습니다." 라는 오류를 해결 하지 못하는 경우 다음 몇 가지 단계를 수행 합니다.

1. 새 Windows 사용자 계정을 만듭니다.

2. 새 사용자에 대 한 오류 없이 Xamarin Visual Studio 확장이 로드 되는지 확인 합니다.

3. 확장이 올바르게 로드 되 면 원래 사용자에 대해 저장 된 설정 중 일부에 의해 문제가 발생 한 것일 수 있습니다.

    - **탐색기** - _% LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*. 0_
    - **Regedit** – _HKEY\_현재\_사용자\\소프트웨어\\Microsoft\\VisualStudio\\1\*. 0_
    - **Regedit** – _HKEY\_현재\_사용자\\소프트웨어\\Microsoft\\VisualStudio\\1\*. 0\_구성_

4. 이러한 저장 된 설정으로 인해 문제가 발생 하는 것으로 나타나는 경우 해당 설정의 백업 및 삭제를 시도할 수 있습니다.
