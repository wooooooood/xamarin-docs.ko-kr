---
title: 설치 후 Visual Studio 확장 누락
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: conceptdev
ms.author: crdun
ms.date: 03/20/2017
ms.openlocfilehash: 180db6789ab9cc665ad815b943013b117a562709
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757062"
---
# <a name="missing-visual-studio-extensions-after-installation"></a>설치 후 Visual Studio 확장 누락

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>오류 메시지: 이 프로젝트는 현재 버전의 Visual Studio와 호환 되지 않습니다.

Visual Studio 2017 (Community, Professional 또는 Enterprise) 이상이 설치 되어 있는지 확인 합니다.

[Windows 요구 사항](~/cross-platform/get-started/requirements.md#windows-requirements)도 참조 하세요.

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>가능한 해결 방법 1: 설치를 변경 하 여 Visual Studio 확장이 설치 되어 있는지 확인 합니다.

특정 상황에서 Xamarin 설치 관리자는 Visual Studio 확장에 대 한 설치 옵션을 자동으로 선택 취소 합니다. 문제의 원인이 되는 경우 설치 관리자의 **변경** 명령을 사용 하 여 누락 된 Visual Studio 확장을 설치 합니다. 예를 들어 Visual Studio 2013에 대 한 확장을 설치 하려면 다음을 수행 합니다.

1. Windows **프로그램 및 기능** 제어판을 엽니다.

2. **Xamarin** 항목을 마우스 오른쪽 단추로 클릭 하 고 **변경**을 선택 합니다.

3. **다음**, **변경**을 차례로 클릭 합니다.

4. **Xamarin for Visual Studio 2013** 옵션이 설치로 설정 되어 있는지 확인 합니다.

    ![](missing-vs-extensions-images/installer.png "Visual Studio 2013 용 Xamarin 사용 설치 옵션")

5. 설치 관리자 마법사의 나머지 단계를 진행 합니다.

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>가능한 수정 2: Visual Studio에서 확장을 다시 설정 하도록 요청

1. Xamarin 확장이 Visual Studio extensions 폴더에 복사 되었는지 확인 합니다.

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    3\.1.228 버전에 대 한 확장이 올바르게 설치 된 경우 폴더에는 60 개의 항목이 있습니다.

    ![](missing-vs-extensions-images/folder.png "탐색기에서 ' Xamarin\3.1.228.0 ' 폴더 내용 목록")

2. 이 폴더가 올바른지 확인 한 후에는 Visual Studio에 확장을 다시 설정 하도록 지시 합니다.

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>가능한 수정 3: Xamarin을 새로 다시 설치 해 보세요.

1. Windows 제어판에서 존재 하는 다음 중 하나를 제거 합니다.

    * Xamarin

    * Windows용 Xamarin

    * Xamarin.Android

    * Xamarin.iOS

    * Visual Studio용 Xamarin

2. 탐색기에서 Xamarin Visual Studio 확장 폴더 ( **Program files** 및 **program files (x86)** 를 포함 한 모든 버전)에서 나머지 파일을 삭제 합니다.

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3. 또한 "VirtualStore" 디렉터리를 확인 하 여 확장 디렉터리의 "오버레이" 복사본이 있는지 확인 합니다.

    `%LOCALAPPDATA%\VirtualStore`

4. 레지스트리 편집기 (regedit)를 엽니다.

5. 이 키를 찾습니다.

    _HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6. 이 패턴과 일치하는 항목을 찾아 삭제합니다.

    _C:\Program Files\*\Microsoft Visual Studio 1\*.0\Common7\IDE\Extensions\Xamarin_

7. 이 키를 찾습니다.

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8. Xamarin과 관련된 것으로 보이는 모든 항목을 삭제합니다. 예를 들어 다음은 이전 버전의 Xamarin에서 문제를 발생 시키는 데 사용 되는 것입니다.

    _Mono.VisualStudio.Shell,1.0_

9. 다시 부팅합니다.

10. [Visualstudio.com](https://visualstudio.com/xamarin)에서 현재 안정적인 버전의 Xamarin을 다시 설치 합니다.

## <a name="possible-fix-4-repair-visual-studio-installation"></a>가능한 수정 4: Visual Studio 설치 복구

1. Windows **프로그램 및 기능** 제어판을 엽니다.

2. 관련 Microsoft Visual Studio 항목을 마우스 오른쪽 단추로 클릭 하 고 **변경** 을 선택 합니다.

3. 열리는 Visual Studio 대화 상자에서 **복구** 단추를 클릭 합니다.
