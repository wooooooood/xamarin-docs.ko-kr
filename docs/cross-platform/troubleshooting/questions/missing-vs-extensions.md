---
title: 설치 후 Visual Studio 확장 누락
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: asb3993
ms.author: amburns
ms.date: 03/20/2017
ms.openlocfilehash: 3e3d426e7b00725eafeba139de5bc46d416c368a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61344238"
---
# <a name="missing-visual-studio-extensions-after-installation"></a>설치 후 Visual Studio 확장 누락

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>오류 메시지: 이 프로젝트는 현재 버전의 Visual Studio와 호환 되지 않습니다.

Visual Studio 2017 (Community, Professional 또는 Enterprise)를 확인 또는 이상이 설치 되어 있습니다.

참고 항목의 [Windows 요구 사항](~/cross-platform/get-started/requirements.md#windows-requirements)합니다.

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>가능한 해결 방법 1: Visual Studio 확장 설치 되어 있는지 확인 하려면 설치 변경

특정 상황에서 Xamarin 설치 관리자 수 자동으로 선택을 취소 Visual Studio 확장에 대 한 설치 옵션입니다. 문제의 원인인 경우 설치 관리자를 사용 하 여 누락 된 Visual Studio 확장을 설치 **변경** 명령입니다. 예를 들어, Visual Studio 2013 용 확장을 설치 해야 합니다.

1. Windows를 엽니다 **프로그램 및 기능** 제어판입니다.

2. 마우스 오른쪽 단추로 클릭 합니다 **Xamarin** 항목과 선택 **변경**합니다.

3. 클릭 **다음**, 한 다음 **변경**합니다.

4. 있는지 확인 합니다 **Visual Studio 2013 용 Xamarin** 옵션 설치 설정:

    ![](missing-vs-extensions-images/installer.png "Visual Studio 2013 설치 옵션에 대 한 Xamarin을 사용 하도록 설정")

5. 설치 마법사의 나머지 과정을 진행 합니다.

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>가능한 해결 방법 2: Visual Studio 확장을 다시 설정 요청

1. Xamarin 확장은 Visual Studio extensions 폴더에 복사 된 경우를 확인 합니다.

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    확장이 제대로 설치 되었는지 (3.1.228 버전)를 폴더에 60 개 항목 수 됩니다.


    ![](missing-vs-extensions-images/folder.png "탐색기에서 'Xamarin\3.1.228.0' 폴더 내용 목록")

2. 이 폴더에 제대로 표시 되는지 확인 한 후에 Visual Studio 확장 설정을 다시 시도를 지시 합니다.

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>가능한 해결 방법 3: Xamarin의 새로 다시 설치 해 보십시오.

1.  Windows 제어판에서 표시 되는 다음 중 하나를 제거 합니다.

    *   Xamarin

    *   Windows용 Xamarin

    *   Xamarin.Android

    *   Xamarin.iOS

    *   Visual Studio용 Xamarin

2.  탐색기에서 Xamarin Visual Studio 확장 폴더에서 나머지 파일을 삭제 (모든 버전에 모두 포함 **Program Files** 하 고 **프로그램 파일 (x86)**):

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3.  "VirtualStore" 디렉터리 확장 디렉터리의 복사본을 모두 "오버레이" 않을 경우에 확인 합니다.

    `%LOCALAPPDATA%\VirtualStore`

4.  레지스트리 편집기 (regedit)를 엽니다.

5.  이 키를 찾습니다.

    _HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6.  이 패턴과 일치하는 항목을 찾아 삭제합니다.

    _C:\Program Files\*\Microsoft Visual Studio 1\*.0\Common7\IDE\Extensions\Xamarin_

7.  이 키를 찾습니다.

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8.  Xamarin과 관련된 것으로 보이는 모든 항목을 삭제합니다. 예를 들어, 다음의 하나는 하는 데 이전 버전의 Xamarin에 문제를 일으킬:

    _Mono.VisualStudio.Shell,1.0_

9.  다시 부팅합니다.

10.  현재 안정적인 버전의 Xamarin에서 다시 [visualstudio.com](https://visualstudio.com/xamarin)합니다.

## <a name="possible-fix-4-repair-visual-studio-installation"></a>가능한 해결 방법 4: Visual Studio 설치 복구

1.  Windows를 엽니다 **프로그램 및 기능** 제어판입니다.

2.  관련 Microsoft Visual Studio 항목을 마우스 오른쪽 단추로 클릭 하 고 선택 **변경**

3.  클릭 합니다 **복구** 단추 Visual Studio 대화 상자가 열립니다.
