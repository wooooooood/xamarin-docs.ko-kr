---
title: Xamarin 제거
description: 이 문서에서는 Windows의 Visual Studio에서 Xamarin을 제거하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
author: conceptdev
ms.author: crdun
ms.date: 01/22/2020
ms.openlocfilehash: 4c9096edddeb00070aaabc3e93b283f2d55c1bfa
ms.sourcegitcommit: a3b7e016fb25584dbf57bae89b64a9f98031e7c9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76550009"
---
# <a name="uninstall-xamarin-from-visual-studio"></a>Visual Studio에서 Xamarin 제거

이 가이드에서는 Windows의 Visual Studio에서 Xamarin을 제거하는 방법을 설명합니다.

<a name="uninstallvs2017" />

## <a name="visual-studio-2019-and-visual-studio-2017"></a>Visual Studio 2019 및 Visual Studio 2017

설치 관리자 앱을 사용하여 Visual Studio 2019 및 Visual Studio 2017에서 Xamarin을 제거합니다.

1. **시작 메뉴**를 사용하여 **Visual Studio 설치 관리자**를 엽니다.

2. 변경하려는 인스턴스의 **수정** 단추를 누릅니다.

    [![](uninstalling-xamarin-images/vs2017-02-sml.png "Press the modify button")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

3. **워크로드** 탭에서 **.NET을 사용하는 모바일 개발** 옵션의 선택을 취소합니다(**모바일 및 게임** 섹션에서).

    [![](uninstalling-xamarin-images/vs2017-03-sml.png "Uncheck the Mobile Development workload")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

4. 창의 오른쪽 아래에 있는 **수정** 단추를 클릭합니다.

5. 설치 관리자는 선택이 취소된 구성 요소를 제거합니다(설치 관리자가 변경하기 전에 Visual Studio 2017이 닫힘).

    [![](uninstalling-xamarin-images/vs2017-04-sml.png "Press the Modify button")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

3단계에서 **개별 구성 요소** 탭을 전환하고 특정 구성 요소의 선택을 취소하여 개별 Xamarin 구성 요소(예: 프로파일러 또는 Workbooks)를 제거할 수 있습니다.

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Uninstall individual components")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Visual Studio 2017을 완전히 제거하려면 **시작** 단추 옆에 있는 3개의 표시줄 메뉴에서 **제거**를 선택합니다.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Uninstall Visual Studio completely")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> 릴리스 및 미리 보기 버전 등 Visual Studio의 두 개(이상) 인스턴스를 나란히(SxS) 설치한 경우 하나의 인스턴스를 제거하면 다음을 비롯한 다른 Visual Studio 인스턴스에서 일부 Xamarin 기능이 제거될 수 있습니다.
>
> - Xamarin Profiler
> - Xamarin Workbooks/검사기
> - Xamarin 원격 iOS 시뮬레이터
> - Apple Bonjour SDK
>
> 특정 조건에서 SxS 인스턴스 중 하나를 제거하면 이러한 기능을 잘못 제거할 수 있습니다. SxS 인스턴스를 제거한 후에 시스템에 남아 있는 Visual Studio 인스턴스에서 Xamarin 플랫폼의 성능이 저하될 수 있습니다.
>
>Visual Studio 설치 관리자에서 **복구** 옵션을 실행하여 해결합니다. 그러면 누락된 구성 요소가 다시 설치됩니다.

<a name="uninstallvs2015"></a>

## <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 및 이전 버전

Visual Studio 2015를 완전히 제거하려면 [visualstudio.com에서 Support Answer](https://visualstudio.microsoft.com/vs/support/vs2015/uninstall-visual-studio-2015/)를 사용합니다.

**제어판**을 통해 Windows 컴퓨터에서 Xamarin을 제거할 수 있습니다. 아래 그림과 같이 **프로그램 및 기능** 또는 **프로그램 > 프로그램 제거**로 이동합니다.

 [![](uninstalling-xamarin-images/image3.png "Navigate to Programs and Features or Programs  Uninstall a Program as illustrated here")](uninstalling-xamarin-images/image3.png#lightbox)

[제어판]에서 표시되는 다음 중 하나를 제거합니다.

- Xamarin
- Windows용 Xamarin
- Xamarin.Android
- Xamarin.iOS
- Visual Studio용 Xamarin

탐색기에서 Xamarin Visual Studio 확장 폴더(프로그램 파일과 프로그램 파일(x86)을 포함한 모든 버전)에서 나머지 파일을 모두 삭제합니다.

```
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

다음 위치에 있어야 하는 Visual Studio의 MEF 구성 요소 캐시 디렉터리를 삭제합니다.

```
%LOCALAPPDATA%\Microsoft\VisualStudio\1*.0\ComponentModelCache
```

Windows가 **Extensions\Xamarin** 또는 **ComponentModelCache** 디렉터리에 대한 오버레이 파일을 저장했는지 확인하려면 **VirtualStore** 디렉터리를 체크 인합니다.

```
%LOCALAPPDATA%\VirtualStore
```

레지스트리 편집기(regedit)를 열고 다음 키를 찾습니다.

```
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls
```

이 패턴과 일치하는 항목을 찾아 삭제합니다.

```
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

이 키를 찾습니다.

```
HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\1*.0\ExtensionManager\PendingDeletions
```

Xamarin과 관련된 것으로 보이는 모든 항목을 삭제합니다. 예를 들어, `mono` 또는 `xamarin` 용어를 포함하는 모든 항목이 해당됩니다.

관리자 cmd.exe 명령 프롬프트를 연 다음, Visual Studio의 설치된 각 버전에 대해 `devenv /setup` 및 `devenv /updateconfiguration` 명령을 실행합니다. 예를 들어, Visual Studio 2015의 경우:

```cmd
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
```
