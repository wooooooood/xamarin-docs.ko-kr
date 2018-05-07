---
title: Visual Studio Android Emulator
description: 이 가이드에서는 Visual Studio 2015에서 Visual Studio Android Emulator를 구성 및 사용하여 Xamarin.Android 앱을 개발하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: CD128CB9-499F-4558-B49F-77248824EFDF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/30/2018
ms.openlocfilehash: 29e35d0dee614d28eed08fbe8799fc74c5ad1eba
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
---
# <a name="visual-studio-android-emulator"></a>Visual Studio Android Emulator

_이 가이드에서는 Visual Studio 2015에서 Visual Studio Android Emulator를 구성 및 사용하여 Xamarin.Android 앱을 개발하는 방법을 설명합니다._

## <a name="visual-studio-android-emulator-overview"></a>Visual Studio Android Emulator 개요

Microsoft Visual Studio 2015에는 Xamarin.Android 앱을 디버깅하기 위한 대상으로 사용할 수 있는 Android 에뮬레이터인 *Visual Studio Emulator for Android*가 들어 있습니다. 이 에뮬레이터는 개발 컴퓨터의 Hyper-V 기능을 사용하므로 Android SDK에 포함된 기본 에뮬레이터보다 시작 및 실행 시간이 더 빠릅니다. Xamarin.Android 응용 프로그램을 개발할 때 기본 Android SDK 에뮬레이터 대신 Visual Studio Emulator for Android를 사용할 수 있습니다.

> [!NOTE]
> Visual Studio Android Emulator는 Visual Studio 2015에서만 호환되고 &ndash; Visual Studio 2017에서는 작동하지 않습니다.

이 가이드에서는 Visual Studio에서 Microsoft Android 에뮬레이터를 실행하여 앱을 테스트하는 방법과, 에뮬레이터에서 제공하는 여러 기능에 대해 설명합니다. 서로 다른 유형의 Android 장치를 시뮬레이션 하기 위해 *장치 프로필*(기본 Android SDK 에뮬레이터의 장치 정의와 유사)을 선택하는 방법을 살펴볼 것입니다. 마지막으로, 문제 해결 섹션에서는 일반적인 문제 및 해결 방법을 설명합니다.

## <a name="requirements"></a>요구 사항

에뮬레이터를 실행하려면 컴퓨터가 Hyper-V 실행을 위한 요구 사항을 충족해야 합니다. Hyper-V에는 64비트 버전 Windows 8 Pro Edition, Windows 8.1 및 Windows 10 이상이 필요합니다. 요구 사항에 대한 자세한 내용은 [Visual Studio Emulator for Android에 대한 시스템 요구 사항](https://msdn.microsoft.com/en-us/library/mt228280.aspx)을 참조하세요.

> [!NOTE]
> Hyper-V를 사용할 때는 HAXM(Android SDK 에뮬레이터에서 사용)을 사용할 수 없습니다. HAXM의 제한 사항과 가능한 문제에 대한 자세한 내용은 [HAXM 가상화 충돌](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#virt-conflicts)을 참조하세요.


## <a name="running-the-emulator"></a>에뮬레이터 실행

Visual Studio는 **디버그 대상** 드롭다운 메뉴에서 미리 구성된 몇 가지 대상-장치 프로필을 제공합니다(다음 스크린 샷에 표시). Microsoft Android Emulator 대상의 앞에는 **VS Emulator**:가 붙습니다.

[![미리 구성된 대상 장치 프로필](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs-sml.png)](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs.png#lightbox)

Visual Studio가 Xamarin.Android 응용 프로그램을 시작할 때 에뮬레이터가 선택된 장치 대상으로 시작되며 앱이 에뮬레이터에 배포됩니다. Visual Studio 왼쪽 아래 모서리에 에뮬레이터가 시작 중임을 나타내는 메시지가 표시됩니다.

[![VS 에뮬레이터 시작](visual-studio-android-emulator-images/02-emulator-starting-vs-sml.png)](visual-studio-android-emulator-images/02-emulator-starting-vs.png#lightbox)

시작이 지연된 후 에뮬레이터 화면이 왼쪽 아래처럼 표시됩니다. 화면 위쪽으로 잠금 아이콘을 끌어가면 장치 잠금이 해제됩니다.
그런 다음 Xamarin.Android 앱이 오른쪽에 보이는 것처럼 에뮬레이터에서 실행될 것입니다.

[![에뮬레이터 스크린샷](visual-studio-android-emulator-images/03-first-screen-vs-sml.png)](visual-studio-android-emulator-images/03-first-screen-vs.png#lightbox)

기본 Android SDK 에뮬레이터에서와 마찬가지로 코드에서 중단점을 설정하고, 변수를 점검하며, 호출 스택을 확인할 수 있습니다. 에뮬레이터 오른쪽의 세로 도구 모음은 에뮬레이터 기능에 대한 액세스를 제공합니다.

[![세로 도구 모음의 단추](visual-studio-android-emulator-images/04-vertical-toolbar-vs-sml.png)](visual-studio-android-emulator-images/04-vertical-toolbar-vs.png#lightbox)

다음 목록에는 세로 도구 모음의 각 단추 기능이 요약되어 있습니다.

-   **닫기** &ndash; 에뮬레이터 응용 프로그램을 종료합니다. 이 단추는 자주 사용되지 않습니다. &ndash; 일반적으로 에뮬레이터는 (다시 시작 지연을 방지하기 위해) 최초 시작 후 실행되는 상태로 유지되며 더 이상 필요하지 않은 경우에만 종료하기 때문입니다.

-   **최소화** &ndash; 에뮬레이터를 실행 상태로 두지만 작업 표시줄의 크기는 최소화합니다.

-   **전원** &ndash; 장치 켜기 및 끄기를 시뮬레이션합니다. 에뮬레이터는 실행 상태로 유지됩니다.

-   **멀티 터치** &ndash; 장치 디스플레이에 여러 점을 중첩하여 집기 및 확대/축소의 터치 지점 역할을 수행합니다.
    한 점을 끌면 다른 점이 반대 방향으로 이동하면서 두 손 동작을 시뮬레이션합니다.

-   **단일 지점 마우스 입력** &ndash;(멀티 터치 입력을 사용한 후) 장치를 단일 지점 입력으로 되돌립니다.

-   **왼쪽으로 회전/오른쪽으로 회전** &ndash; 방향 변경에 앱이 반응하는 방식을 테스트하는 데 도움이 됩니다. 예를 들어 *왼쪽으로 회전* 단추를 처음 클릭하면 에뮬레이터가 가로 모드로 전환됩니다. *오른쪽으로 회전* 단추를 누르면 에뮬레이터가 세로 모드로 되돌아갑니다.

-   **화면에 맞추기** &ndash; 바탕 화면에 맞게 에뮬레이터 화면의 크기를 확대/축소합니다.

-   **확대/축소** &ndash; 에뮬레이터 화면을 33%, 50%, 66%, 100% 또는 사용자 지정 백분율로 조정합니다.


*추가 도구* 단추는 에뮬레이터의 추가 기능을 표시하는 대화 상자를 엽니다.

[![추가 도구 대화 상자](visual-studio-android-emulator-images/05-additional-tools-vs-sml.png)](visual-studio-android-emulator-images/05-additional-tools-vs.png#lightbox)


각 추가 기능은 대화 상자 맨 위에 있는 탭의 행에서 제공됩니다.

-   **가속도계** &ndash; 3D 공간에 장치 움직임을 시뮬레이션합니다.

-   **위치** &ndash; GPS 위치 선택 및 시뮬레이션에 사용할 수 있는 지도를 표시합니다. 이 지도에서 위치 간 이동을 시뮬레이션하기 위한 *지도 점*을 만들 수 있습니다.

-   **배터리** &ndash; 배터리에 남은 충전량을 시뮬레이션하기 위한 슬라이더를 제공합니다.

-   **스크린 샷** &ndash; 이 탭에는 스크린샷을 만들고 인스턴트 미리 보기를 표시하는 **캡처** 단추가 있습니다. *저장* 단추는 스크린 샷을 저장합니다.

-   **카메라** &ndash; 고정 애니메이션 이미지, 파일의 사진 또는 호스트 컴퓨터의 연결된 웹캠을 통한 사진 찍기를 시뮬레이션합니다. 전면 또는 후면 카메라 중 하나를 선택할 수 있습니다.

-   **SD 카드** &ndash; 에뮬레이터에서는 호스트 컴퓨터의 폴더를 장치에 SD 카드처럼 제공할 수 있습니다.
    앱이 시뮬레이션된 SD 카드에 파일을 읽고 쓸 때는 `adb` 명령을 사용하지 않고 바탕 화면에서 직접 액세스할 수 있습니다.

-   **네트워크** &ndash; 에뮬레이터의 네트워크 설정 요약을 표시합니다(에뮬레이터는 호스트 컴퓨터의 네트워크 연결을 다시 사용).

이러한 기능을 사용하는 방법에 대한 자세한 내용은 [Visual Studio Emulator for Android 소개](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/)를 참조하세요.



## <a name="configuring-device-profiles"></a>장치 프로필 구성

Microsoft Android 에뮬레이터에는 출시된 Android 장치의 가장 인기 있는 Android 버전, 화면 크기, 하드웨어 속성을 나타내는 장치 프로필 집합이 포함되어 있습니다. 또한 이러한 장치 프로필은 이미 KitKat, Lollipop 및 Marshmallow 등과 같은 여러 Android 버전용으로 구성되어 있습니다.

*Emulator Manager*는 장치 프로필을 설치, 제거, 시작하는 데 사용됩니다. 이 스크린 샷에 표시된 것처럼 **도구** 메뉴에서 **Visual Studio Emulator for Android...** 를 선택합니다.

[![도구 메뉴에서 에뮬레이터 시작](visual-studio-android-emulator-images/06-launch-emulator-manager-vs-sml.png)](visual-studio-android-emulator-images/06-launch-emulator-manager-vs.png#lightbox)

그러면 **장치 프로필** 대화 상자가 열립니다. 설치된 프로필은 장치 프로필 목록의 맨 위서 강조 표시됩니다. 설치되지 않았으나 설치할 수 있는 프로필은 흐리게 표시됩니다.

[![장치 프로필 아이콘](visual-studio-android-emulator-images/07-device-profiles-vs-sml.png)](visual-studio-android-emulator-images/07-device-profiles-vs.png#lightbox)

새 프로필을 설치하려면 프로필 설치 아이콘을 클릭합니다(위 스크린 샷에서 표시된 아래쪽 방향 화살표). 예를 들어 **5.7" Marshmallow (6.0.0) XHDPI Phone** 설치 아이콘을 클릭하면 Emulator Manager가 아래와 같이 프로필을 다운로드합니다.

[![프로필 다운로드의 예제](visual-studio-android-emulator-images/08-downloading-profile-vs-sml.png)](visual-studio-android-emulator-images/08-downloading-profile-vs.png#lightbox)

장치 프로필을 다운로드한 후에는 프로필이 설치되었음을 표시하기 위해 강조 표시됩니다. *세부 정보 표시* 아이콘을 클릭하면 플랫폼 유형, CPU 아키텍처, 화면 크기/해상도, 장치에 사용 가능한 메모리가 표시됩니다.

[![장치 프로필 세부 정보 표시](visual-studio-android-emulator-images/09-show-details-vs-sml.png)](visual-studio-android-emulator-images/09-show-details-vs.png#lightbox)

Visual Studio **디버그 대상** 드롭다운 메뉴가 열리면 새로 설치된 장치 프로필을 대상으로 사용할 수 있습니다.

[![대상 드롭다운 메뉴의 새 프로필](visual-studio-android-emulator-images/10-debug-target-vs-sml.png)](visual-studio-android-emulator-images/10-debug-target-vs.png#lightbox)

*Emulator Manager*에서 **이 프로필 제거**를 클릭하여 사용하지 않는 장치 프로필을 제거하면 이 목록을 줄일 수 있습니다. 현재는 이 에뮬레이터에서 사용자 지정 장치 프로필을 만들 수 없습니다.


## <a name="troubleshooting"></a>문제 해결

이 섹션에서는 Xamarin.Android에서 *Visual Studio Emulator for Android*를 사용할 때의 몇 가지 일반적인 오류와 해결 방법을 설명합니다.

<a name="cant_connect" />

### <a name="emulator-will-not-start"></a>에뮬레이터가 시작되지 않음

경우에 따라 호스트 프로세서와 Hyper-V 가상 머신 간의 호환성 문제가 있으면 에뮬레이터가 시작되지 않습니다. 이 문제를 해결하려면 가상 머신이 가질 수 있는 프로세서 기능을 제한하도록 Hyper-V를 구성합니다. &ndash; 이렇게 하면 서로 다른 호스트 프로세서 버전과 가상 머신의 호환성이 향상될 수 있습니다.
다음 단계에 따라 이 변경을 수행합니다.

1.  **시작** 단추를 클릭하고 **MMC**를 입력한 다음 **Enter**를 누릅니다. 다음 그림처럼 **Hyper-V Manager**를 클릭합니다.

    [![Hyper-V 관리자](visual-studio-android-emulator-images/15-launch-hyperv-manager.png)](visual-studio-android-emulator-images/15-launch-hyperv-manager.png#lightbox)

2.  Hyper-V Manager **가상 머신** 창에서 편집에 사용할 에뮬레이터를 마우스 오른쪽 단추로 클릭하고 **설정...** 을 클릭합니다.

    [![가상 머신 설정 메뉴 항목](visual-studio-android-emulator-images/16-vm-settings.png)](visual-studio-android-emulator-images/16-vm-settings.png#lightbox)

3.  설정 창에서 **호환성** 섹션(**하드웨어 > 프로세서** 아래)을 찾고 **다른 프로세서 버전을 사용하는 물리적 컴퓨터로 마이그레이션**을 사용하도록 설정합니다.

    [![마이그레이션 옵션 선택](visual-studio-android-emulator-images/17-set-compatibility-vs-sml.png)](visual-studio-android-emulator-images/17-set-compatibility-vs.png#lightbox)

4.  **확인**을 클릭하고 Hyper-V Manager 창을 닫습니다.



### <a name="app-deploys-and-starts-but-fails-immediately"></a>앱 배포가 시작되나 즉시 실패

이 상황에서는 에뮬레이터가 시작되고 앱이 에뮬레이터에 성공적으로 배포되며 앱이 시작됩니다. 그러나 즉시 앱이 실패합니다.
많은 경우 호스트 프로세서와 Hyper-V 가상 머신 간의 비호환성으로 인해서도 이 문제가 발생합니다. 이 오류를 해결하려면 [에뮬레이터가 시작되지 않음](#cant_connect)(위)의 지침을 따릅니다.


### <a name="emulator-stops-with-the-diagnostic-message-libaot-mscorlibdllso-not-found"></a>진단 메시지 **libaot-mscorlib.dll.so not found**와 함께 에뮬레이터 멈춤

이 오류를 해결하려면 다음 단계를 통해 빠른 배포를 사용하지 않게 설정합니다.

1.  [에뮬레이터가 시작되지 않음](#cant_connect)(위)의 지침을 따릅니다.

2.  프로젝트 **속성**을 두 번 클릭합니다.

3.  **Android 옵션**을 클릭하고 **빠른 배포 사용(디버그 모드에만 해당)** 을 선택 취소합니다.

    [![빠른 배포 옵션 사용 선택 취소](visual-studio-android-emulator-images/18-fast-deployment-vs-sml.png)](visual-studio-android-emulator-images/18-fast-deployment-vs.png#lightbox)



### <a name="drag-and-drop-does-not-work"></a>끌어서 놓기가 작동하지 않음

관리자로 *Visual Studio Emulator for Android*를 시작한 경우(또는 Visual Studio가 관리자 권한으로 실행 중인 동안 Visual Studio에서 시작한 경우) .APK 또는 .ZIP 파일의 끌어서 놓기가 작동하지 않을 수 있습니다. 이 문제를 해결하려면 권한 상승 없이 *Visual Studio Emulator for Android*를 실행합니다(즉 관리자가 아니게).


### <a name="other-errors"></a>기타 오류

위의 문제 해결 팁은 Xamarin.Android에서 Visual Studio Android Emulator를 사용할 때 발생하는 대부분의 일반적인 문제에 적용될 수 있습니다.  Visual Studio Android Emulator 문제 해결에 대한 더 자세한 가이드는 [Visual Studio Emulator for Android 문제 해결](https://msdn.microsoft.com/en-us/library/mt228282.aspx)을 참조하세요.



## <a name="summary"></a>요약

이 문서에서는 *Visual Studio Emulator for Android*를 소개했습니다. 에뮬레이터를 사용하여 Visual Studio에서 Xamarin.Android 앱을 디버그하는 것과, 세로 도구 모음의 단추 기능을 설명하고 **추가 도구** 대화 상자에서 사용할 수 있는 기능을 간략히 살펴보았습니다. *Emulator Manager*를 사용하여 장치 프로필을 설치, 제거, 시작하는 방법을 설명했습니다. *문제 해결* 섹션에서는 에뮬레이터를 사용할 때의 일반적인 문제와 해결 방법을 설명했습니다.


## <a name="related-links"></a>관련 링크

- [Visual Studio Emulator for Android](https://www.visualstudio.com/vs/msft-android-emulator/)
- [Visual Studio Emulator for Android 소개](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/)
