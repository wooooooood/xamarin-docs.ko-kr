---
title: Visual Studio 2017에 Xamarin 설치
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: 0f1c014f316ff4f3eb7341fa9815475175d11937
ms.sourcegitcommit: dc6ccf87223942088ca926c0dadd5b5478c683cb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2018
---
# <a name="installing-xamarin-in-visual-studio-2017"></a>Visual Studio 2017에 Xamarin 설치

<a name="requirements" />

## <a name="requirements"></a>요구 사항

Visual Studio 2017에 Xamarin을 설치하려면 다음이 필요합니다.

1. Windows 7 이상.

2. Visual Studio 2017(Community, Professional 또는 Enterprise).

3. Visual Studio용 Xamarin.

Xamarin을 설치하고 사용하기 위한 필수 구성 요소에 대한 자세한 내용은 [시스템 요구 사항](~/cross-platform/get-started/requirements.md)을 참조하세요.

<a name="installation" />

## <a name="installation"></a>설치

Xamarin은 새로운 Visual Studio 2017 설치의 일부로 설치할 수 있습니다.
이렇게 하려면 다음 단계를 따릅니다.

1. [Visual Studio](https://www.visualstudio.com/vs/) 페이지에서 Visual Studio 2017 Community, Visual Studio Professional 또는 Visual Studio Enterprise를 다운로드합니다(아래쪽에 다운로드 링크가 제공됨).

2. 다운로드한 패키지를 두 번 클릭하여 설치를 시작합니다.

3. 설치 화면에서 **.NET을 사용한 모바일 개발** 워크로드를 선택합니다. 

    [![워크로드 화면에서 .NET을 사용한 모바일 개발 선택](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png#lightbox)

4. **.NET을 사용한 모바일 개발**이 선택된 상태에서 오른쪽에 있는 **요약** 패널을 봅니다. 여기에서는 설치하지 않으려는 모바일 개발 옵션의 선택을 취소할 수 있습니다. 기본적으로 다음 스크린샷에 표시된 모든 옵션이 설치됩니다(**Xamarin Workbooks**, **Xamarin Profiler**, **Xamarin Remoted Simulator**, **Android NDK**, **Android SDK**, **Java SE Development Kit**, **Google Android Emulator**, **F# 지원** 및 **Intel HAXM**).

    ![설치되는 Xamarin 옵션을 나열하는 요약 패널](windows-images/02-summary.png)

5. Visual Studio 2017 설치를 시작할 준비가 되면 오른쪽 아래 모서리에서 **설치** 단추를 클릭합니다.

    ![설치 단추 위치](windows-images/03-click-install.png)

   설치하는 Visual Studio 2017의 버전에 따라 설치 프로세스를 완료하는 데 시간이 오래 걸릴 수 있습니다. 진행률 표시줄을 통해 설치를 모니터링할 수 있습니다.

    ![설치 중 진행률 표시줄의 스크린샷 예제](windows-images/04-progress-bars.png)

6. Visual Studio 2017 설치가 완료되면 **시작** 단추를 클릭하여 Visual Studio를 시작합니다.

    ![시작 단추의 위치](windows-images/05-launch.png)

<a name="vs2017" />

### <a name="adding-xamarin-to-visual-studio-2017"></a>Visual Studio 2017에 Xamarin 추가

Visual Studio 2017이 이미 설치되어 있는 경우 Visual Studio 2017 설치 관리자를 다시 실행하여 워크로드를 수정함으로써 Xamarin을 추가할 수 있습니다(자세한 내용은 [Visual Studio 수정](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) 참조). 그런 다음, 위에 나열된 단계를 따라 Xamarin을 설치합니다.

Visual Studio 2017을 다운로드하고 설치하는 방법에 대한 자세한 내용은 [Visual Studio 2017 설치](https://docs.microsoft.com/visualstudio/install/install-visual-studio)를 참조하세요.


### <a name="verifying-installation"></a>설치 확인

Visual Studio 2017에서 **도움말** 메뉴를 클릭하여 Xamarin이 설치되었는지 확인할 수 있습니다. Xamarin이 설치된 경우 이 스크린샷에 표시된 것처럼 **Xamarin** 메뉴 항목이 표시됩니다.

![도움말 메뉴에 표시되는 Xamarin 메뉴 항목](windows-images/12-xamarin-menu-item.png)

이전 버전의 Visual Studio를 사용 중인 경우 **도움말 > Microsoft Visual Studio 정보**를 클릭하고 설치된 제품 목록을 스크롤하여 Xamarin이 설치되었는지 확인할 수 있습니다.

![Visual Studio의 설치된 제품 화면](windows-images/13-xamarin-is-installed.png)

버전 정보를 찾는 방법에 대한 자세한 내용은 [내 버전 정보와 로그를 어디에서 찾을 수 있습니까?](~/cross-platform/troubleshooting/questions/version-logs.md)를 참조하세요.

<a name="nextsteps" />

## <a name="next-steps"></a>다음 단계

Visual Studio 2017에 Xamarin을 설치하면 앱에 대한 코드 작성을 시작할 수 있지만, 시뮬레이터, 에뮬레이터 및 장치에 앱을 빌드하고 배포하기 위한 추가 설정은 필요하지 않습니다. 다음 가이드를 참조하여 설치를 완료하고 플랫폼 간 앱 빌드를 시작하세요.

### <a name="ios"></a>iOS

자세한 내용은 [Windows에 Xamarin.iOS 설치](~/ios/get-started/installation/windows/index.md) 가이드를 참조하세요. 

1. [Mac용 Visual Studio 설치](https://docs.microsoft.com/visualstudio/mac/installation)
2. [Mac 빌드 호스트에 Visual Studio 연결](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
3. [iOS 개발자 설정](~/ios/get-started/installation/device-provisioning/index.md) - 장치에서 응용 프로그램을 실행하는 데 필요합니다.
5. [원격 iOS 시뮬레이터](~/tools/ios-simulator.md)
6. [Visual Studio용 Xamarin.iOS 소개](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

### <a name="android"></a>Android

자세한 내용은 [Windows에 Xamarin.Android 설치](~/android/get-started/installation/windows.md) 가이드를 참조하세요.

1. [Xamarin.Android Configuration](~/android/get-started/installation/windows.md#configuration)
2. [Xamarin Android SDK Manager 사용](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Android SDK 에뮬레이터](~/android/get-started/installation/android-emulator/index.md)
4. [개발용 장치 설정](~/android/get-started/installation/set-up-device-for-development.md)
