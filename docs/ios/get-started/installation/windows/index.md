---
title: Windows에 Xamarin.iOS 설치
description: 이 문서에서는 Visual Studio용 Xamarin.iOS를 설정하는 방법을 보여줍니다. Visual Studio용 Xamarin 확장의 설치 프로세스를 설명하고, Mac에 설치된 Apple SDK에 연결하는 방법을 살펴봅니다.
ms.prod: xamarin
ms.assetid: abf85d3e-a365-44a2-b1a4-6c572c7f76dd
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/29/2017
ms.openlocfilehash: 02c4b27f12382d3c3d3eed778d1bfd92ae3f1e79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="installing-xamarinios-on-windows"></a>Windows에 Xamarin.iOS 설치

_이 문서에서는 Visual Studio용 Xamarin.iOS를 설정하는 방법을 보여줍니다. Visual Studio용 Xamarin 확장의 설치 프로세스를 설명하고, Mac에 설치된 Apple SDK에 연결하는 방법을 살펴봅니다._

## <a name="overview"></a>개요

Visual Studio용 Xamarin.iOS를 사용하면 Windows 컴퓨터에서 iOS 응용 프로그램을 작성 및 테스트하고, 네트워크에 연결된 Mac으로 빌드 및 배포 서비스를 제공할 수 있습니다.

Visual Studio 내 iOS용으로 개발하여 다음과 같은 기능을 제공합니다.

- IOS, Android 및 Windows 응용 프로그램을 위한 플랫폼 간 솔루션을 만듭니다.
- iOS 소스 코드를 포함한 모든 플랫폼 간 프로젝트에 Visual Studio 도구(예: *Resharper* 및 *Team Foundation Server*)를 사용합니다.
- 익숙한 IDE를 사용하면서도 모든 Apple API의 Xamarin.iOS 바인딩을 활용합니다.

Visual Studio용 Xamarin.iOS는 Visual Studio가 Mac의 Windows 가상 머신 내에서 실행되는 구성(Parallels 또는 VMWare 사용) 또는 Mac과 동일한 네트워크에서 볼 수 있는 별도의 컴퓨터에 있는 구성을 지원합니다. 어떤 구성이 더 적합하든, Visual Studio는 SSH를 사용하여 Mac에 신속하고 안전하게 연결합니다.

이 문서에서는 개발자가 Visual Studio를 사용하여 Xamarin.iOS 응용 프로그램을 빌드, 디버그, 배포할 수 있도록 Mac 및 Windows 컴퓨터에 Xamarin.iOS 도구를 설치하고 구성하는 단계와 Mac 호스트에 연결하는 단계를 다룹니다.

아래 다이어그램은 Xamarin.iOS 개발 워크플로에 대한 간단한 개요를 보여줍니다.

[![Xamarin.iOS 개발 워크플로](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
> Visual Studio는 프로젝트를 빌드하는 별도의 MSBuild 프로세스를 시작합니다. 이 프로세스에서는 Mac에 대한 새 연결을 설정합니다. 즉, Visual Studio가 빌드할 때 Windows에서 Mac으로 이어지는 두 개의 SSH 연결이 있습니다. [명령줄](~/ios/get-started/installation/windows/connecting-to-mac/index.md)에서 빌드하면 MSBuild 프로세스가 하나만 생성됩니다. 다이어그램을 단순화하기 위해 모든 연결을 화살표로 표시했습니다.

## <a name="requirements"></a>요구 사항

Visual Studio용 Xamarin.iOS는 놀라운 결과를 가져다 줍니다. 개발자가 Visual Studio IDE를 사용하여 Windows 컴퓨터에서 iOS 응용 프로그램을 만들고 빌드하고 디버그할 수 있습니다. 단독으로는 불가능합니다. Apple의 컴파일러가 없으면 iOS 응용 프로그램을 만들 수 없으며, Apple의 인증서 및 코드 서명 도구 없이는 배포할 수 없습니다. 다시 말해서, Visual Studio용 Xamarin.iOS를 설치하려면 이러한 작업을 수행할 수 있도록 네트워크로 연결된 Mac OS X 컴퓨터에 연결해야 합니다. 구성이 완료되면 Xamarin 도구가 프로세스를 최대한 원활하게 만들어줍니다.


<a name="system-requirements"/>

### <a name="system-requirements"></a>시스템 요구 사항

시스템 요구 사항은 다음과 같습니다.

### <a name="windows"></a>Windows

1. Windows 7 이상.

2. Visual Studio 2015 Professional 이상

    a. 엔터프라이즈 라이선스가 있는 경우 Visual Studio Enterprise를 설치해야 합니다.

3. Visual Studio용 Xamarin.

Xamarin 도구는 플러그 인이 지원되지 않아 Visual Studio Express 버전에 사용할 수 없습니다. Xamarin은 Visual Studio Community에서 지원됩니다.

### <a name="mac"></a>Mac

1. macOS Sierra(10.12) 이상을 실행하는 Mac(안정적인 최신 버전을 권장함).

2. Xamarin iOS SDK. Mac용 Visual Studio를 다운로드할 때 설치됩니다.

3. Apple의 Xcode IDE 및 iOS SDK(Mac 앱 스토어에서 제공하는 안정적인 최신 버전을 권장함).

**Windows 컴퓨터가 네트워크를 통해 Mac에 연결할 수 있어야 합니다.**

### <a name="apple-developer-account"></a>Apple 개발자 계정

장치에 응용 프로그램을 배포하거나 앱 스토어에 제출하려면 Apple 개발자 계정이 필요합니다. 관련 개발자 인증서 및 프로비전 프로필을 만들어서 네트워크로 연결된 Mac에 설치해야만 Visual Studio용 Xamarin.iOS가 작동합니다. [장치 프로비전](~/ios/get-started/installation/device-provisioning/index.md) 문서에서 개발 인증서를 얻고 장치를 프로비전하는 단계를 참조하세요.

## <a name="features"></a>기능 

Visual Studio용 Xamarin.iOS를 사용하면 Windows에서 Xamarin.iOS 프로젝트를 만들고, 편집하고, 빌드하고, 배포할 수 있습니다. 여기에는 다음과 같은 기능이 포함됩니다.

- 새 iOS 프로젝트를 만듭니다.

- Xamarin.Android 및 UWP 프로젝트까지 포함하는 iOS 프로젝트 및 플랫폼 간 솔루션을 편집합니다.

- Xamarin.Android 및 UWP 프로젝트까지 포함하는 iOS 프로젝트 및 플랫폼 간 솔루션을 컴파일합니다.

- iOS 디자이너를 사용하여 스토리보드 및 .xib를 지원합니다.

- iOS 응용 프로그램을 배포하고 디버그합니다. 그러면 앱 자체가 시뮬레이터에서 또는 Mac에 연결된 장치에서 실행됩니다.

- Windows의 iOS 시뮬레이터. Windows에서 iOS 시뮬레이터를 사용하는 방법에 대한 자세한 내용은 [이 가이드](~/tools/ios-simulator.md)를 참조하세요.

<a name="configuring" />

## <a name="configuring-your-mac"></a>Mac 구성

<a name="installation"/>

### <a name="installation"></a>설치

mac 호스트에 Xamarin.iOS 도구를 설치하려면 [Mac용 Visual Studio를 설치](https://docs.microsoft.com/visualstudio/mac/installation)해야 합니다.

소프트웨어가 설치되면 다음 섹션의 단계에 따라 Visual Studio용 Xamarin이 연결할 수 있도록 macOS에서 Xamarin.iOS를 구성합니다.

> [!IMPORTANT]
> Windows 컴퓨터는 Mac이 연결되는 것과 동일한 Xamarin.iOS 버전을 사용해야 합니다. 다음과 같은 방법으로 이를 확인할 수 있습니다.
>
> - **Visual Studio 2015 이하**: Mac용 Visual Studio와 동일한 [업데이트 채널](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/)에 있는지 확인합니다.
>
> - **Visual Studio 2017, 릴리스 버전**: Mac용 Visual Studio의 **안정적인** 채널에 있는지 확인합니다.
>
> - **Visual Studio 2017, 미리 보기 버전**: Mac용 Visual Studio **알파** 채널에 있는지 확인합니다.

<a name="configuration" />


### <a name="configuration"></a>구성

Visual Studio용 Xamarin 확장과 Mac 간의 통신에 액세스하려면 Mac에서 **원격 로그인**을 허용해야 합니다. 원격 로그인을 설정하려면 아래 단계를 수행합니다.

1. *스포트라이트*를 열고(**Cmd-Space**) **원격 로그인**을 검색한 후 **공유** 결과를 선택합니다. 그러면 **공유** 패널에 **시스템 환경 설정**이 열립니다.

![원격 로그인에 대한 스포트라이트 검색](images/spotlight.png)

2. 왼쪽에 있는 **서비스** 목록에서 **원격 로그인**을 선택하여 Visual Studio용 Xamarin이 Mac에 연결할 수 있도록 허용합니다.

![서비스 목록에서 원격 로그인 옵션 선택](images/sharing.png)

3. **모든 사용자**에 대한 액세스를 허용하도록 또는 Mac 사용자/그룹이 오른쪽에 있는 허용되는 사용자 목록에 포함되도록 **원격 로그인**을 설정합니다.

이제 Mac이 동일한 네트워크에 있으면 Visual Studio에서 Mac을 검색할 수 있습니다.

> [!NOTE]
> 기본적으로 서명된 응용 프로그램을 차단하도록 설정된 macOS 방화벽이 있는 경우 `mono-sgen`이 들어오는 연결을 받을 수 있도록 허용해야 합니다. 이 경우 들어오는 연결을 받을 수 있도록 허용하라는 경고 대화가 표시됩니다.

<a name="developersetup"/>

### <a name="ios-developer-setup"></a>iOS 개발자 설정

iOS 개발의 경우 관련 서명 ID를 사용하여 Mac 컴퓨터를 구성하는 것이 중요합니다. 이렇게 하면 앱이 올바르게 서명되므로 앱 스토어를 통해 또는 임시로 앱을 배포할 수 있습니다. Xamarin 사용한 iOS 개발에 적합하도록 Mac을 설정하는 지침을 보려면 아래 링크를 따라 이동하세요.

- [장치 프로비전](~/ios/get-started/installation/device-provisioning/index.md?ide=vs)

Mac을 구성한 후에는 Windows 컴퓨터를 설정해야 합니다.

<a name="windowsinstallation"/>

## <a name="windows-installation"></a>Windows 설치

Xamarin은 Visual Studio 2017 또는 2015 설치의 일부로 설치할 수 있습니다. Xamarin용 Visual Studio Tools를 설치하려면 [Windows 설치](~/cross-platform/get-started/installation/windows.md) 가이드를 참조하세요.

## <a name="installation-complete"></a>설치 완료

설치 프로세스를 완료한 후에도 모든 기능을 사용하려면 몇 가지 단계를 더 수행해야 합니다.

- [Visual Studio를 Mac에 연결](#connectingtomac) – Visual Studio를 Mac 빌드 호스트에 연결해야만 Xamarin.iOS 프로젝트를 빌드할 수 있습니다.
- [Visual Studio 도구 모음 구성](#toolbar) - 그러면 Visual Studio에서 Xamarin.iOS 기능에 손쉽게 액세스할 수 있습니다.

<a name="connectingtomac" /> 

### <a name="connecting-to-the-mac"></a>Mac에 연결

연결은 컴퓨터 간 SSH 연결을 통해 Visual Studio용 Xamarin.iOS에서 Mac 빌드 호스트로 설정됩니다. 연결에 대한 자세한 내용은 [Mac에 연결](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 가이드를 참조하세요.

Mac에 연결하려면 아래 단계를 수행합니다.

- **도구 > 옵션**으로 이동한 후 **Xamarin** 아래에서 **iOS 설정**을 선택합니다.

  [![iOS 설정 화면](images/image2.png)](images/image2.png#lightbox)

- **원격 로그인**을 허용하도록 Mac을 올바르게 [구성](#configuration)하면 목록에서 Mac을 선택할 수 있습니다.

  [![원격 호스트 대화 상자](images/xma3.png)](images/xma3.png#lightbox)

- Mac 호스트의 관리 자격 증명을 요구합니다.

  [![로그인 대화 상자](images/xma4.png)](images/xma4.png#lightbox)

- 연결되면 컴퓨터 이름 옆에 '연결 성공' 아이콘이 표시됩니다.

  [![컴퓨터 이름 옆에 연결 성공 아이콘을 표시하는 원격 대화 상자](images/image6.png)](images/image6.png#lightbox)

Visual Studio를 시작할 때마다 다시 연결됩니다.

<a name="toolbar" />

## <a name="visual-studio-toolbar-configuration"></a>Visual Studio Toolbar Configuration

iOS 프로젝트를 열면 iOS 도구 모음이 기본적으로 표시되며, 별도로 구성할 필요는 없습니다.

iOS 도구 모음이 표시되지 않으면 아래 단계를 사용합니다.

도구 모음을 구성하려면 먼저 **보기 > 도구 모음** 메뉴를 열고 **iOS** 항목을 선택합니다. 이 스크린샷에 보이는 것처럼 메뉴 항목을 선택하면 도구 모음을 표시하도록 선택됩니다.

[![도구 모음 > iOS 선택](images/image31.png)](images/image31.png#lightbox)

### <a name="visual-studio-2015"></a>Visual Studio 2015

Visual Studio 2017 이전 버전에서는 **솔루션 플랫폼** 단추를 표준 도구 모음에 추가해야 하는 경우가 있습니다. 이렇게 하면 디버그할 때 iOS 장치 또는 iOS 시뮬레이터를 선택할 수 있습니다. 이렇게 설정하려면 아래 지침을 따릅니다.

표준 도구 모음 오른쪽에서 메뉴 단추를 클릭합니다.

- **단추 추가 또는 제거** 선택
- **솔루션 플랫폼** 선택

[![솔루션 플랫폼 선택](images/image35.png)](images/image35.png#lightbox)

이제 **표준** 및 **iOS** 도구 모음이 이 스크린샷과 비슷하게 보일 것입니다.

[![이제 표준 및 iOS 도구 모음이 이 스크린샷과 비슷하게 표시됨](images/image36.png)](images/image36.png#lightbox)

도구 모음 구성이 완료되면 Visual Studio용 Xamarin iOS를 사용할 준비가 완료된 것입니다.

## <a name="summary"></a>요약

이 문서에서는 Visual Studio용 Xamarin iOS를 설치, 구성 및 사용하는 단계별 가이드를 제공했습니다.

Windows 및 Mac OS X에 필수 구성 요소 도구를 설치하고 구성하는 방법을 다루었습니다.


## <a name="related-links"></a>관련 링크

- [설치](~/cross-platform/get-started/installation/windows.md)
- [장치 프로비전](~/ios/get-started/installation/device-provisioning/index.md)
- [Visual Studio용 Xamarin.iOS 소개](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)
- [XMA를 사용하여 Mac을 Visual Studio 환경에 연결(비디오)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
