---
title: Xamarin을 사용한 Visual Studio 2017 릴리스 후보를 사용할 수 있습니까?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8E752F36-F73A-4EFC-9F82-4E18FDE1C9E2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 18329d226df5d129bd648c82b01e1ea045d131f4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Xamarin을 사용한 Visual Studio 2017 릴리스 후보를 사용할 수 있습니까?

## <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Xamarin을 사용한 Visual Studio 2017 릴리스 후보를 사용할 수 있습니까?

예. Visual Studio 2017 릴리스 후보를 통해 Xamarin을 활용할 수 있습니다. 그러나 유의 사항: 현재, Visual Studio 2017 RC에 Xamarin을 설치 하면 이전 버전의 Visual Studio 2015/2013에 설치 하는 Xamarin 제거 됩니다. Visual Studio 2017 용 Xamarin 없습니다 설치할 수 있습니다 Xamarin으로 동시에 대 한 Visual Studio 2015/2013 MSI 패키지는 Visual Studio 설치 관리자 시스템의 사용률을 향해 이동 하는 Visual Studio 2017 결과로.

현재에 예상 되는이 동작을 우회할 수 있는 방법으로 팀을 보고 하는 동안 사용자가을 요구에 따라 개발 환경을 선택할 수행 하시기 바랍니다. 

> [!IMPORTANT]
> Xamarin.iOS 프로젝트를 작성 하는 경우 Visual Studio 쌍으로 연결 된 Mac 컴퓨터에 Mac 용 Xamarin.iOS 채널의 인스턴스와 동일한 버전 인지 확인 합니다.

## <a name="how-do-i-install-xamarin-to-visual-studio-2017-release-candidate"></a>Visual Studio 2017 릴리스 후보를 Xamarin을 설치 하려면 어떻게 해야 합니까?

### <a name="installing-during-the-visual-studio-2017-rc-installer"></a>Visual Studio 2017 RC 설치 관리자는 동안 설치

* 선택 된 **Xamarin** 새의 일부로 구성 요소 **Visual Studio 설치 관리자**

  [![](visualstudio-2017-rc-images/install1-sml.png "Visual Studio 2017 RC Installer Screen")](visualstudio-2017-rc-images/install1-orig.png#lightbox)

그러면 Xamarin.iOS 및 Xamarin.Android 개발을 위한 Visual Studio 확장명을 설치 됩니다.

### <a name="installing-or-reinstalling-xamarin-in-an-existing-installation-of-visual-studio-2017-rc"></a>Visual Studio 2017 RC의 기존 설치에서 Xamarin을 설치 또는 다시 설치

#### <a name="using-the-visual-studio-installer"></a>Visual Studio 설치 관리자를 사용 하 여:

1. Visual Studio 설치 관리자 응용 프로그램 검색

  [![](visualstudio-2017-rc-images/reinstall1-sml.png "Visual Studio 설치 관리자 응용 프로그램에 대 한 검색 결과")](visualstudio-2017-rc-images/reinstall1-orig.png#lightbox)

2. 선택:는 합니다. **.NET (미리 보기)을 사용한 모바일 개발** 작업 탭에서 또는

  [![](visualstudio-2017-rc-images/reinstall2-sml.png "VS 설치 관리자 작업 탭") ](visualstudio-2017-rc-images/reinstall2-orig.png#lightbox) b 합니다. **Xamarin** 에 **개별 구성 요소** 탭

  [![](visualstudio-2017-rc-images/reinstall3-sml.png "VS 설치 관리자 구성 요소 탭")](visualstudio-2017-rc-images/reinstall3-orig.png#lightbox)

#### <a name="using-the-visual-studio-installer-within-visual-studio"></a>Visual Studio 내에서 Visual Studio 설치 관리자를 사용 하 여:
1. Visual Studio 2017 시작 페이지로 이동
2. 클릭 **더 프로젝트 템플릿** 아래는 **새 프로젝트** 섹션

    [![](visualstudio-2017-rc-images/reinstall4-sml.png "Visual Studio 시작 페이지")](visualstudio-2017-rc-images/reinstall4-orig.png#lightbox)
3. 클릭 `Open Visual Studio Installer` 왼쪽된 창에서

    [![](visualstudio-2017-rc-images/reinstall5-sml.png "새 프로젝트 화면")](visualstudio-2017-rc-images/reinstall5-orig.png#lightbox)
4. 선택:
    
    a. **.NET (미리 보기)을 사용한 모바일 개발** 작업 탭에서 또는

    [![](visualstudio-2017-rc-images/reinstall2-sml.png "VS 설치 관리자 작업 탭") ](visualstudio-2017-rc-images/reinstall2-orig.png#lightbox) b 합니다. **Xamarin** 에 **개별 구성 요소** 탭

    [![](visualstudio-2017-rc-images/reinstall3-sml.png "VS 설치 관리자 구성 요소 탭")](visualstudio-2017-rc-images/reinstall3-orig.png#lightbox)
