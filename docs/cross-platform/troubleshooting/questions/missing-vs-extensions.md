---
title: 설치 후 누락 된 Visual Studio 확장
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: asb3993
ms.author: amburns
ms.date: 03/20/2017
ms.openlocfilehash: e47cfc4de77a6310a81867eefb07c3c1e5cc7060
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="missing-visual-studio-extensions-after-installation"></a>설치 후 누락 된 Visual Studio 확장

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>오류 메시지:이 프로젝트는 현재 버전의 Visual Studio와 호환 되지 않습니다.

호환 되는 버전의 Visual Studio가 설치 되어 있는지 확인 합니다.

-   Visual Studio 2017 (Community, Professional 또는 Enterprise)
-   Visual Studio 2015 (Community, Professional 또는 Enterprise)

참고 항목에서 [Windows 요구 사항](~/cross-platform/get-started/requirements.md#windows)합니다.

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>가능한 수정 1: Visual Studio 확장명 설치 되어 있는지 확인 하려면 설치를 변경 합니다.

특정 상황에서는 Xamarin 설치 관리자 수 자동으로 선택을 취소는 Visual Studio 확장에 대 한 설치 옵션입니다. 설치 관리자를 사용 하 여 없는 Visual Studio 확장을 설치 하 여 문제의 원인을 이면 **변경** 명령입니다. 예를 들어, Visual Studio 2013 용 확장을 설치 해야 합니다.

1. 창을 열고 **프로그램 및 기능** 패널을 제어 합니다.

2. 마우스 오른쪽 단추로 클릭는 **Xamarin** 항목과 선택 **변경**합니다.

3. 클릭 **다음**, 다음 **변경**합니다.

4. 있는지 확인는 **Visual Studio 2013 용 Xamarin** 옵션이 설치로 설정 되어 있습니다.

    ![](missing-vs-extensions-images/installer.png "Visual Studio 2013 설치 옵션에 대 한 Xamarin을 사용 하도록 설정")

5. 설치 마법사의 나머지 과정을 진행 합니다.

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>가능한 수정 2: Visual Studio에서 확장을 다시 설정 하 게

1. Xamarin 확장의 Visual Studio 확장 폴더에 복사 된 경우를 확인 합니다.

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    확장이 제대로 설치 되었는지 (3.1.228 버전), 폴더에 60 항목 있이 됩니다.


    ![](missing-vs-extensions-images/folder.png "탐색기에서 폴더 내용 'Xamarin\3.1.228.0' 목록")

2. 이 폴더에 제대로 표시 되는지 확인에 대해 Visual Studio를 다시 설정에서 확장을 시도 하십시오.

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>가능한 수정 3: 새로운 다시 Xamarin 설치를 시도 하십시오.

1.  Windows 제어판에서 제공 되는 다음 중 하나를 제거 합니다.

    *   Xamarin

    *   Windows용 Xamarin

    *   Xamarin.Android

    *   Xamarin.iOS

    *   Visual Studio용 Xamarin

2.  탐색기에서 Xamarin Visual Studio 확장명 폴더에서 나머지 파일을 삭제 (모든 버전을 둘 다를 포함 하 여 **Program Files** 및 **Program Files (x86)**):

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3.  또한 "VirtualStore" 디렉터리에 될 확장 디렉터리의 "오버레이" 복사본을 모두 확인 합니다.

    `%LOCALAPPDATA%\VirtualStore`

4.  레지스트리 편집기 (regedit)를 엽니다.

5.  이 키를 찾습니다.

    _HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6.  이 패턴과 일치하는 항목을 찾아 삭제합니다.

    _C:\Program Files\*\Microsoft Visual Studio 1\*.0\Common7\IDE\Extensions\Xamarin_

7.  이 키를 찾습니다.

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8.  Xamarin과 관련된 것으로 보이는 모든 항목을 삭제합니다. 예를 들어 여기의 하나 사용는 이전 버전의 Xamarin에 문제가 발생할 수 있습니다:

    _Mono.VisualStudio.Shell,1.0_

9.  다시 부팅합니다.

10.  Xamarin에서의 최신 안정화 버전 다시 설치 [visualstudio.com](https://visualstudio.com/xamarin)합니다.

## <a name="possible-fix-4-repair-visual-studio-installation"></a>가능한 수정 4: Visual Studio를 복구 설치

1.  창을 열고 **프로그램 및 기능** 패널을 제어 합니다.

2.  관련 Microsoft Visual Studio 항목을 마우스 오른쪽 단추로 클릭 하 고 선택 **변경**

3.  클릭는 **복구** 열린 Visual Studio 대화 상자에서 단추입니다.
