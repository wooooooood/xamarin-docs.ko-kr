---
title: Visual Studio용 Xamarin.iOS 소개
description: 이 문서에서는 Visual Studio를 사용하여 Xamarin iOS 응용 프로그램을 빌드하고 테스트하는 방법을 설명합니다. 프로젝트 만들기, 앱 실행 및 디버깅 및 Windows에서 Mac 빌드 호스트에 연결을 설명합니다.
ms.prod: xamarin
ms.assetid: bf3c779f-959f-428d-babb-428f363f7e4e
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2018
ms.openlocfilehash: e07119bee6478a503ca6c586fa3348206ccd16f7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786202"
---
# <a name="introduction-to-xamarinios-for-visual-studio"></a>Visual Studio용 Xamarin.iOS 소개

Windows용 Xamarin을 사용하면 Visual Studio 내에서 iOS 응용 프로그램을 작성 및 테스트하고, 네트워크에 연결된 Mac으로 빌드 및 배포 서비스를 제공할 수 있습니다.

이 문서에서는 Visual Studio를 사용하여 iOS 응용 프로그램을 빌드하도록 각 컴퓨터에 Xamarin.iOS 도구를 설치 및 구성하는 단계를 설명합니다.

Visual Studio 내부에서 iOS를 개발하면 다음과 같은 여러 가지 이점이 있습니다.

-  iOS, Android 및 Windows 응용 프로그램을 위한 플랫폼 간 솔루션을 만듭니다.
-  iOS 소스 코드를 포함한 모든 플랫폼 간 프로젝트에 자신이 선호하는 Visual Studio 도구(예: **Resharper** 및 **Team Foundation Server**)를 사용합니다.
-  익숙한 IDE를 사용하면서도 모든 Apple API의 Xamarin.iOS 바인딩을 활용합니다.

<a name="Requirements_and_Installation" />

## <a name="requirements-and-installation"></a>요구 사항 및 설치

Visual Studio에서 iOS를 개발할 때 준수해야 하는 몇 가지 요구 사항이 있습니다. 개요에서 간략하게 언급했듯이, IPA 파일을 컴파일하려면 Mac이 필요하고, Apple의 인증서와 코드 서명 도구 없이는 장치에 응용 프로그램을 배포할 수 없습니다.

사용 가능한 몇 가지 구성 옵션이 있으므로 각자 자신의 개발 요구 사항에 가장 적합한 구성을 선택하면 됩니다. 구성은 다음과 같습니다.

-  Mac을 주 개발 컴퓨터로 사용하고 Visual Studio가 설치된 Windows 가상 머신을 실행합니다. [Parallels](http://www.parallels.com/products/desktop/) 또는 [VMWare](http://www.vmware.com/products/fusion/) 같은 VM 소프트웨어를 사용하는 것이 좋습니다.
-  Mac을 빌드 호스트로만 사용합니다. 이 시나리오에서는 [필요한](~/cross-platform/get-started/installation/windows.md#installation) 도구가 설치된 Windows 컴퓨터와 동일한 네트워크에 연결됩니다.

두 경우 모두 다음 단계를 수행해야 합니다.

- [Mac용 Visual Studio 설치](https://docs.microsoft.com/visualstudio/mac/installation)
- [Windows에 Xamarin 도구 설치](~/cross-platform/get-started/installation/windows.md)

## <a name="connecting-to-the-mac"></a>Mac에 연결

Visual Studio를 Mac 빌드 호스트에 연결하려면 [Mac에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 지침을 따릅니다.

## <a name="visual-studio-toolbar-overview"></a>Visual Studio 도구 모음 개요

Visual Studio용 Xamarin iOS는 표준 도구 모음 및 새 iOS 도구 모음에 항목을 추가합니다.
이러한 도구 모음의 기능은 아래에 설명되어 있습니다.

### <a name="standard-toolbar"></a>표준 도구 모음

Xamarin iOS 개발과 관련된 컨트롤은 빨간색 원으로 표시되어 있습니다.

 [![](introduction-to-xamarin-ios-for-visual-studio-images/03.png "Xamarin iOS 개발과 관련된 컨트롤은 빨간색 원으로 표시")](introduction-to-xamarin-ios-for-visual-studio-images/03.png#lightbox "Xamarin iOS 개발과 관련된 컨트롤은 빨간색 원으로 표시")

-  **시작** - 선택한 플랫폼에서 응용 프로그램 디버그 또는 실행을 시작합니다. 연결된 Mac이 있어야 합니다(iOS 도구 모음의 상태 표시기 참조).
-  **솔루션 구성** – 사용할 구성을 선택할 수 있습니다(예: 디버그, 릴리스).
-  **솔루션 플랫폼** - iPhone 또는 iPhoneSimulator를 배포하기로 선택할 수 있습니다.

### <a name="ios-toolbar"></a>iOS 도구 모음

Visual Studio의 iOS 도구 모음은 각 Visual Studio 버전에서 비슷하게 생겼으며 아래에 전부 나열되어 있습니다.

[![](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png "iOS 도구 모음")](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png#lightbox)

이 항목은 아래에 설명되어 있습니다.

-  **Mac Agent/연결 관리자** – Xamarin Mac Agent 대화 상자를 표시합니다. 이 아이콘은 연결 시도 중에는 *주황색*, 연결되면 *녹색*으로 표시됩니다.
-  **iOS 시뮬레이터 표시** – iOS 시뮬레이터 창을 Mac 앞으로 불러옵니다.
-  **빌드 서버에 IPA 파일 표시** – Mac에서 Finder를 응용 프로그램의 IPA 출력 파일 위치로 엽니다.

## <a name="ios-output-options"></a>iOS 출력 옵션

### <a name="output-window"></a>출력 창

*출력* 창에는 빌드, 배포, 연결 메시지 및 오류를 검색하는 데 사용할 수 있는 옵션이 있습니다.

아래 스크린 샷은 사용 가능한 출력 창을 보여주며, 사용 가능한 출력 창은 프로젝트 형식에 따라 달라질 수 있습니다.

[![](introduction-to-xamarin-ios-for-visual-studio-images/output-sml.png "사용 가능한 출력 창")](introduction-to-xamarin-ios-for-visual-studio-images/output-large.png#lightbox)

- **Xamarin** – 여기에는 Mac과의 연결이나 활성화 상태처럼 오직 Xamarin에만 관련된 정보가 포함되어 있습니다.

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output3-sml.png "Mac과의 연결이나 활성화 상태처럼 오직 Xamarin에만 관련된 정보")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

- **Xamarin 진단** – Android와의 상호 작용처럼 Xamarin 프로젝트에 대한 자세한 정보를 표시합니다.

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output4-sml.png "Xamarin 프로젝트에 대한 자세한 정보")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

디버그 및 빌드 같은 다른 기본 Visual Studio 출력 창은 출력 보기 내에서 여전히 사용할 수 있으며 출력 및 MSBuild 출력을 디버그하는 데 사용됩니다.

-  **디버그**

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output2-sml.png "출력 디버그")](introduction-to-xamarin-ios-for-visual-studio-images/output2-large.png#lightbox)

- **빌드** & **빌드 순서**

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output1-sml.png "MSBuild 출력")](introduction-to-xamarin-ios-for-visual-studio-images/output1-large.png#lightbox)

## <a name="ios-project-properties"></a>iOS 프로젝트 속성

Visual Studio의 프로젝트 속성은 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 상황에 맞는 메뉴에서 *속성*을 클릭하여 액세스할 수 있습니다. 그러면 아래 스크린샷처럼 iOS 응용 프로그램을 구성할 수 있습니다.

 ![](introduction-to-xamarin-ios-for-visual-studio-images/iosproperties.png "iOS 응용 프로그램 구성")

-  *iOS 번들 서명* – Mac에 연결하여 코드 서명 ID 및 프로비전 프로필을 채웁니다.

 ![](introduction-to-xamarin-ios-for-visual-studio-images/bundlesigning.png "코드 서명 ID 및 프로비전 프로필 채우기")

-  *iOS IPA 옵션* – IPA 파일은 Mac의 파일 시스템에 저장됩니다.

 ![](introduction-to-xamarin-ios-for-visual-studio-images/ipaoptions.png "iOS IPA 옵션")

-  *iOS 실행 옵션* – 추가 매개 변수를 구성합니다.

 ![](introduction-to-xamarin-ios-for-visual-studio-images/iosrunoptions.png "iOS 실행 옵션")

## <a name="creating-a-new-project-for-ios-applications"></a>iOS 응용 프로그램에 대한 새 프로젝트 만들기

Visual Studio 내에서 새 iOS 프로젝트를 만드는 작업은 다른 프로젝트 형식과 마찬가지 방법으로 수행됩니다. **파일 > 새 프로젝트**를 선택하면 아래 대화 상자가 열리고, 새 iOS 프로젝트를 만드는 데 사용할 수 있는 몇 가지 프로젝트 형식이 표시됩니다.

![새 프로젝트 만들기](introduction-to-xamarin-ios-for-visual-studio-images/newproject.w157.png)

**iOS 앱(Xamarin)** 을 선택하면 새 Xamarin.iOS 응용 프로그램을 만들기 위해 다음 템플릿이 표시됩니다.

![iOS 앱의 템플릿 선택](introduction-to-xamarin-ios-for-visual-studio-images/newproject-2.w157.png)

Visual Studio에서 iOS 디자이너를 사용하여 스토리보드 및 .xib 파일을 편집할 수 있습니다. 스토리보드를 만들려면 스토리보드 템플릿 중 하나를 선택합니다. 그러면 아래 스크린샷처럼 **솔루션 탐색기**에 **Main.storyboard** 파일이 생성됩니다.

![솔루션 탐색기의 Main.storyboard 파일](introduction-to-xamarin-ios-for-visual-studio-images/solution-explorer-new.w157.png)

스토리보드 만들기 또는 편집을 시작하려면 `Main.storyboard`를 두 번 클릭하여 iOS 디자이너에서 엽니다.

![](introduction-to-xamarin-ios-for-visual-studio-images/iosdesigner.png "iOS 디자이너에 있는 Main.storyboard")

보기에 개체를 추가하려면 **도구 상자** 창을 사용하여 디자인 화면으로 항목을 끌어다 놓습니다. 아직 도구 상자를 추가하지 않은 경우 **보기 > 도구 상자**를 선택하여 도구 상자를 추가할 수 있습니다. 아래 그림과 같이 **속성** 창을 사용하여 개체 속성을 수정하고, 레이아웃을 조정하고, 이벤트를 만들 수 있습니다.

![](introduction-to-xamarin-ios-for-visual-studio-images/properties.png "속성 창")

 iOS 디자이너 사용에 대한 자세한 내용은 [디자이너](~/ios/user-interface/designer/index.md) 지침을 참조하세요.

## <a name="running--debugging-ios-applications"></a>iOS 응용 프로그램 실행 및 디버그

### <a name="device-logging"></a>장치 로깅

Visual Studio 2017에서는 Android 및 iOS 로그 패드가 통합되었습니다.

새로운 Visual Studio용 장치 로그 도구 창은 Android 및 iOS 장치에 대한 로그를 표시할 수 있습니다. 다음 명령 중 하나를 실행하여 표시할 수 있습니다.

- **보기 > 다른 창 > 장치 로그**
- **도구 > iOS > 장치 로그**
- **iOS 도구 모음 > 장치 로그**

도구 창이 표시되면 사용자가 장치 드롭다운에서 물리적 장치를 선택할 수 있습니다. 장치를 선택하면 로그가 테이블에 자동으로 추가됩니다. 장치 간에 전환하면 장치 로깅이 중지되었다가 다시 시작됩니다.

장치가 콤보 상자에 표시되려면 iOS 프로젝트가 로드되어야 합니다. 또한 iOS의 경우 Visual Studio가 [Mac 서버에 연결](~/ios/get-started/installation/windows/connecting-to-mac/index.md)되어야만 Mac에 연결된 iOS 장치를 검색할 수 있습니다.

이 도구 창은 로그 항목 테이블, 장치를 선택할 수 있는 드롭다운, 로그 항목을 지우는 방법, 검색 상자 및 재생/중지/일시 중지 단추를 제공합니다.

### <a name="set-debugging-stops"></a>디버깅 중지 설정

응용 프로그램의 어느 위치에나 프로그램 실행을 일시적으로 중지하라고 디버거에 신호를 보내는 중단점을 설정할 수 있습니다. Visual Studio에서 중단점을 설정하려면 편집기의 여백 영역에서 중단하려는 코드의 줄 번호 옆을 클릭합니다.

![](introduction-to-xamarin-ios-for-visual-studio-images/image18.png "디버그 지점 설정")

디버깅을 시작하고, 시뮬레이터 또는 장치를 사용하여 응용 프로그램을 중단점으로 이동합니다. 중단점에 도착하면 줄이 강조 표시되고 Visual Studio의 일반적인 디버깅 동작이 사용됩니다. 코드를 한 단계씩 실행하거나 프로시저 단위로 실행하거나 코드에서 나갈 수 있고, 지역 변수를 검사할 수 있고, 직접 실행 창을 사용할 수 있습니다.

다음 스크린샷은 OS X에서 Parallels를 사용하여 Visual Studio 옆에서 실행되는 iOS 시뮬레이터를 보여줍니다.

![](introduction-to-xamarin-ios-for-visual-studio-images/image19.png "OS X에서 Parallels를 사용하여 Visual Studio 옆에서 실행되는 iOS 시뮬레이터를 보여주는 스크린샷")

### <a name="examine-local-variables"></a>지역 변수 검사

![](introduction-to-xamarin-ios-for-visual-studio-images/image20.png "디버깅을 통해 지역 변수 검사")

## <a name="summary"></a>요약

이 문서에서는 Visual Studio용 Xamarin.iOS를 사용하는 방법을 보여드렸습니다. Visual Studio 내에서 iOS 앱을 만들고 빌드하고 테스트할 수 있는 다양한 기능을 나열하고 간단한 iOS 응용 프로그램을 빌드하고 디버깅하는 방법을 연습했습니다.

## <a name="related-links"></a>관련 링크

- [Xamarin.iOS 설치](~/ios/get-started/installation/windows/index.md)
- [장치 프로비전](~/ios/get-started/installation/device-provisioning/index.md)
- [코드로 iOS UI 만들기](~/ios/app-fundamentals/ios-code-only.md)
- [XMA를 사용하여 Mac을 Visual Studio 환경에 연결(비디오)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
