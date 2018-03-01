---
title: "Windows에서 Visual Studio에 Xamarin 설치"
ms.topic: article
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: b68e03251b83192bdc5836af6ea54446ddaad24a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="installing-xamarin-in-visual-studio-on-windows"></a>Windows에서 Visual Studio에 Xamarin 설치

Xamarin은 이제 모든 버전의 Visual Studio에 무료로 포함되며 별도의 라이선스를 요구하지 않으므로 Visual Studio 설치 관리자를 사용하여 Xamarin 도구를 다운로드하고 설치할 수 있습니다.

-   [Requirements](#requirements)
-   [설치](#installation)
-   [Visual Studio 2017에 Xamarin 추가](#vs2017)
-   [Visual Studio 2015에 Xamarin 추가](#vs2015)
-   [설치 확인](#verifying)
-   [다음 단계](#nextsteps)


<a name="requirements" />

# <a name="requirements"></a>요구 사항

Xamarin용 Visual Studio 도구를 설치하려면 다음이 필요합니다.

1. Windows 7 이상.

2. Visual Studio 2015 또는 2017(Community, Professional 또는 Enterprise).

3. Visual Studio용 Xamarin.

Xamarin은 플러그 인이 지원되지 않아 Visual Studio Express 버전에 사용할 수 없습니다.

Xamarin을 설치하고 사용하기 위한 필수 구성 요소에 대한 자세한 내용은 [시스템 요구 사항](~/cross-platform/get-started/requirements.md)을 참조하세요.


<a name="installation" />

# <a name="installation"></a>설치

Xamarin은 새로운 Visual Studio 설치의 일부로 설치할 수 있습니다.
이렇게 하려면 다음 단계를 따릅니다.

1. [Visual Studio](https://www.visualstudio.com/vs/) 페이지에서 Visual Studio Community, Visual Studio Professional 또는 Visual Studio Enterprise를 다운로드합니다(아래쪽에 다운로드 링크가 제공됨).

2. 다운로드한 패키지를 두 번 클릭하여 설치를 시작합니다.

3. 설치 화면에서 **.NET을 사용한 모바일 개발** 워크로드를 선택합니다. 

    [![워크로드 화면에서 .NET을 사용한 모바일 개발 선택](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png)

4. **.NET을 사용한 모바일 개발**이 선택된 상태에서 오른쪽에 있는 **요약** 패널을 봅니다. 여기에서는 설치하지 않으려는 모바일 개발 옵션의 선택을 취소할 수 있습니다. 기본적으로 다음 스크린샷에 표시된 모든 옵션이 설치됩니다(**Xamarin Workbooks**, **Xamarin Profiler**, **Xamarin Remoted Simulator**, **Android NDK**, **Android SDK**, **Java SE Development Kit**, **Google Android Emulator**, **F# 지원** 및 **Intel HAXM**).

    ![설치되는 Xamarin 옵션을 나열하는 요약 패널](windows-images/02-summary.png)

5. Visual Studio 설치를 시작할 준비가 되면 오른쪽 아래 모서리에서 **설치** 단추를 클릭합니다.

    ![설치 단추 위치](windows-images/03-click-install.png)

   설치하는 Visual Studio의 버전에 따라 설치 프로세스를 완료하는 데 오랜 시간이 걸릴 수 있습니다. 진행률 표시줄을 통해 설치를 모니터링할 수 있습니다.

    ![설치 중 진행률 표시줄의 스크린샷 예제](windows-images/04-progress-bars.png)

6. Visual Studio 설치가 완료되면 **시작** 단추를 클릭하여 Visual Studio를 시작합니다.

    ![시작 단추의 위치](windows-images/05-launch.png)


<a name="vs2017" />

## <a name="adding-xamarin-to-visual-studio-2017"></a>Visual Studio 2017에 Xamarin 추가

Visual Studio 2017이 이미 설치된 경우 워크로드를 수정하는 Visual Studio 설치 관리자를 다시 실행하여 Xamarin을 추가할 수 있습니다(자세한 내용은 [Visual Studio 수정](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) 참조). 그런 다음, 위에 나열된 단계를 따라 Xamarin을 설치합니다.

Visual Studio 2017을 다운로드하고 설치하는 방법에 대한 자세한 내용은 [Visual Studio 2017 설치](https://docs.microsoft.com/visualstudio/install/install-visual-studio)를 참조하세요.


<a name="vs2015" />

## <a name="adding-xamarin-to-visual-studio-2015"></a>Visual Studio 2015에 Xamarin 추가

Xamarin.Android를 Visual Studio 2015의 기존 설치에 추가하려면 다음 단계를 따릅니다.

1. Windows **시작** 단추를 마우스 오른쪽 단추로 클릭하고 **프로그램 및 기능**을 선택합니다.

2. **Microsoft Visual Studio**를 마우스 오른쪽 단추로 클릭하고 **변경**을 클릭합니다.

3. Visual Studio 설치 관리자 대화 상자가 나타나면 **수정** 단추를 클릭합니다.

4. **기능** 탭에서 **플랫폼 간 모바일 개발**까지 아래로 스크롤합니다. **C#/.NET(Xamarin)** 옆에 있는 확인란을 클릭합니다.

    ![Visual Studio 2015에 C#/.NET Xamarin 추가](windows-images/06-add-xamarin.png)

5. **업데이트** 단추를 클릭하여 Visual Studio에 Xamarin을 추가합니다.


<a name="verifying" />

## <a name="verifying-installation"></a>설치 확인

Visual Studio 2017에서 **도움말** 메뉴를 클릭하여 Xamarin이 설치되었는지 확인할 수 있습니다. Xamarin이 설치된 경우 이 스크린샷에 표시된 것처럼 **Xamarin** 메뉴 항목이 표시됩니다.

![도움말 메뉴에 표시되는 Xamarin 메뉴 항목](windows-images/12-xamarin-menu-item.png)

이전 버전의 Visual Studio를 사용 중인 경우 **도움말 > Microsoft Visual Studio 정보**를 클릭하고 설치된 제품 목록을 스크롤하여 Xamarin이 설치되었는지 확인할 수 있습니다.

![Visual Studio의 설치된 제품 화면](windows-images/13-xamarin-is-installed.png)

버전 정보를 찾는 방법에 대한 자세한 내용은 [내 버전 정보와 로그를 어디에서 찾을 수 있습니까?](~/cross-platform/troubleshooting/questions/version-logs.md)를 참조하세요.

<a name="nextsteps" />

# <a name="next-steps"></a>다음 단계

Xamarin용 Visual Studio Tools를 설치하면 앱용 코드 작성을 시작할 수 있지만 앱을 빌드하여 시뮬레이터, 에뮬레이터 및 장치에 배포하기 위한 추가 설정이 필요 없습니다. 다음 가이드를 참조하여 설치를 완료하고 플랫폼 간 앱 빌드를 시작하세요.

## <a name="ios"></a>iOS

자세한 내용은 [Windows에 Xamarin.iOS 설치](~/ios/get-started/installation/windows/index.md) 가이드를 참조하세요. 

1. [Mac에서 Xamarin.iOS 도구 설치](~/ios/get-started/installation/windows/index.md#installation)
2. [Mac 구성](~/ios/get-started/installation/windows/index.md#configuration)
3. [iOS 개발자 설정](~/ios/get-started/installation/windows/index.md#developersetup)(장치에서 응용 프로그램을 실행하려면).
4. [Mac 빌드 호스트에 Visual Studio 연결](~/ios/get-started/installation/windows/index.md#connectingtomac)
5. [원격 iOS 시뮬레이터](~/tools/ios-simulator.md)
6. [Visual Studio용 Xamarin.iOS 소개](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

## <a name="android"></a>Android

자세한 내용은 [Windows에 Xamarin.Android 설치](~/android/get-started/installation/windows.md) 가이드를 참조하세요.

1. [Xamarin.Android Configuration](~/android/get-started/installation/windows.md#configuration)
2. [Xamarin Android SDK Manager 사용](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Android SDK 에뮬레이터](~/android/get-started/installation/android-emulator/index.md)
4. [개발용 장치 설정](~/android/get-started/installation/set-up-device-for-development.md)
