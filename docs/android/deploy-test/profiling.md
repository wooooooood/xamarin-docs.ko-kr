---
title: Android 앱 프로파일링
description: 이 가이드에서는 프로파일러 도구를 사용하여 Android 앱의 성능 및 메모리 사용을 검사하는 방법을 설명합니다.
ms.topic: article
ms.prod: xamarin
ms.assetid: 8C823FEE-A6F6-4C31-9EB6-E51407A2FD8E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 400075a1cbd2303f2ecddb9b1cc9465bbcbde32d
ms.sourcegitcommit: f255aa286bd52e8a80ffa620c2e93c97f069f8ec
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68680259"
---
# <a name="profiling-android-apps"></a>Android 앱 프로파일링

앱 스토어에 앱을 배포하기 전에 모든 성능 병목 지점, 과도한 메모리 사용 문제 또는 네트워크 리소스의 비효율적인 사용을 확인하고 해결하는 것이 중요합니다. 이 목적을 위해 두 가지 프로파일러 도구를 사용할 수 있습니다.

-  Xamarin Profiler 
-  Android Studio에서 Android Profiler

이 가이드는 Xamarin Profiler를 소개하고 Android Profiler의 사용을 시작하기 위한 자세한 정보를 제공합니다.

 
## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin Profiler는 IDE 내에서 Xamarin 앱을 프로파일링하기 위해 Visual Studio 및 Mac용 Visual Studio와 통합되는 독립 실행형 애플리케이션입니다. Xamarin Profiler 사용에 대한 자세한 내용은 [Xamarin Profiler](~/tools/profiler/index.md)를 참조하세요.

> [!NOTE]
> Windows의 Visual Studio Enterprise 또는 Mac용 Visual Studio에서 Xamarin Profiler 기능을 잠금 해제하려면 [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/) 구독자여야 합니다.
 
## <a name="android-studio-profiler"></a>Android Studio Profiler

Android Studio 3.0 이상은 Android Profiler 도구를 포함합니다. Android Profiler를 사용하여 Visual Studio Enterprise 라이선스의 필요 없이 Visual Studio로 빌드된 Xamarin Android 앱의 성능을 측정할 수 있습니다. 그러나 Xamarin Profiler와 달리 Android Profiler는 Visual Studio와 통합되지 않으며 미리 빌드되고 Android Profiler로 가져온 APK(Android 애플리케이션 패키지)를 프로파일링하는 데만 사용할 수 있습니다.

### <a name="launching-a-xamarin-android-app-in-android-profiler"></a>Android Profiler에서 Xamarin Android 앱 시작

다음 단계에서는 Android Studio의 Android Profiler 도구에서 Xamarin Android 애플리케이션을 시작하는 방법을 설명합니다. 아래 예제 스크린샷에서 Xamarin Forms [XamagonXuzzle](https://docs.microsoft.com/samples/xamarin/mobile-samples/liveplayer-xamagonxuzzlelp/) 앱이 빌드되고 Android Profiler를 사용하여 프로파일링됩니다.

1.  Android 프로젝트에서 빌드 옵션에서 **공유 런타임 사용**을 비활성화합니다. 이렇게 하면 공유 개발 시 Mono 런타임에서 종속성 없이 APK(Android 애플리케이션 패키지)가 빌드됩니다.

    ![공유 런타임 사용 비활성화](profiling-images/vswin/01-turn-off-shared-runtime.png)

2.  **디버그**용 앱을 빌드하고 물리적 디바이스 또는 에뮬레이터에 배포합니다. 이로 인해 APK의 서명된 **디버그** 버전이 빌드됩니다.
    **XamagonXuzzle** 예제의 경우 결과 APK는 **com.companyname.XamagonXuzzle-Signed.apk**로 이름이 지정됩니다.

3.  프로젝트 폴더를 열고 **bin/Debug**로 이동합니다. 이 폴더에서 앱의 **Signed.apk** 버전을 찾고 편리하게 액세스할 수 있는 위치로 복사합니다(예: 바탕 화면). 다음 스크린샷에서 APK **com.companyname.XamagonXuzzle-Signed.apk**가 배치되고 바탕 화면으로 복사됩니다.

    [![디버그 서명된 APK 파일의 위치](profiling-images/vswin/02-locating-the-debug-apk-sml.png)](profiling-images/vswin/02-locating-the-debug-apk.png#lightbox)

4.  Android Studio를 시작하고 **APK 프로파일링 또는 디버그**를 선택합니다.

    ![Android Studio 시작 화면에서 프로파일러 시작](profiling-images/vswin/03-android-studio.png)

5.  **APK 파일 선택** 대화 상자에서 앞에서 빌드하고 복사한 APK로 이동합니다. APK를 선택하고 **확인**을 클릭합니다. 
    
    ![APK 파일 선택 대화 상자에서 APK 선택](profiling-images/vswin/04-select-apk-dialog.png)

6.  Android Studio는 APK를 로드하고 **classes.dex**를 디스어셈블합니다.

    ![APK 설정](profiling-images/vswin/05-setting-up-the-apk.png)

7.  APK가 로드되면 Android Studio는 APK에 대한 다음 프로젝트 화면을 표시합니다. 왼쪽의 트리 보기에서 앱 이름을 마우스 오른쪽 단추로 클릭하고 **모듈 설정 열기**를 선택합니다.

    [![모듈 설정 열기 메뉴 항목의 위치](profiling-images/vswin/06-open-module-settings-sml.png)](profiling-images/vswin/06-open-module-settings.png#lightbox)

8.  **프로젝트 설정 > 모듈**로 이동하고, 앱의 **서명된** 노드를 선택한 다음, **&lt;No SDK&gt;** 를 클릭합니다.

    [![SDK 설정으로 이동](profiling-images/vswin/07-project-settings-modules-sml.png)](profiling-images/vswin/07-project-settings-modules.png#lightbox)

9.  **모듈 SDK** 풀다운 메뉴에서 앱을 빌드하는 데 사용된 Android SDK 수준을 선택합니다(이 예제에서는 API 수준 26이 **XamagonXuzzle**을 빌드하는 데 사용됨).

    [![프로젝트 SDK 수준 설정](profiling-images/vswin/08-project-sdk-level-sml.png)](profiling-images/vswin/08-project-sdk-level.png#lightbox)

    **적용** 및 **확인**을 클릭하여 이 설정을 저장합니다.

10. 도구 모음 아이콘에서 프로파일러를 시작합니다.

    [![프로파일러 도구 모음 아이콘의 위치](profiling-images/vswin/09-launch-profiler-sml.png)](profiling-images/vswin/09-launch-profiler.png#lightbox)

11. 앱을 실행/프로파일링하기 위해 배포 대상을 선택하고 **확인**을 클릭합니다. 배포 대상은 에뮬레이터에서 실행되는 물리적 디바이스 또는 가상 디바이스일 수 있습니다. 이 예제에서는 Nexus 5X 디바이스가 사용됩니다.

    ![배포 대상 선택](profiling-images/vswin/10-select-deployment-target.png)

12. 프로파일러가 시작된 후 배포 디바이스 및 앱 프로세스에 연결되는 데 몇 초가 걸립니다. APK를 설치하는 동안 Android Profiler는 **연결된 디바이스 없음** 및 **디버깅할 수 있는 프로세스 없음**을 보고합니다.

    [![프로파일러에서 APK 설치](profiling-images/vswin/11-no-connected-devices-sml.png)](profiling-images/vswin/11-no-connected-devices.png#lightbox)

13. 몇 초 후 Android Profiler는 APK 설치를 완료하고 디바이스 이름 및 프로파일링되는 앱 프로세스의 이름을 보고하는 APK를 시작합니다(이 예제에서는 각각 **LGE Nexus 5X** 및 **com.companyname.XamagonXuzzle**).

    [![시작 후 프로파일러 창](profiling-images/vswin/12-profiler-starts-sml.png)](profiling-images/vswin/12-profiler-starts.png#lightbox)

14. 디바이스 및 디버깅할 수 있는 프로세스를 식별한 후 Android Profiler는 앱 프로파일링을 시작합니다.

    [![프로파일러에서 실행 중인 앱에 대해 표시](profiling-images/vswin/13-profiler-running-sml.png)](profiling-images/vswin/13-profiler-running.png#lightbox)

15. **XamagonXuzzle**에서 **RANDOMIZE** 단추를 탭하는 경우(타일을 이동하고, 임의로 지정하도록 함) 앱의 임의 간격 동안 CPU 사용량이 표시됩니다.

    [![RANDOMIZE 단추를 탭한 경우 CPU 사용량](profiling-images/vswin/14-tap-randomize-sml.png)](profiling-images/vswin/14-tap-randomize.png#lightbox)


### <a name="using-the-android-profiler"></a>Android Profiler 사용

Android Profiler 사용에 대한 자세한 정보는 [Android Studio 설명서](https://developer.android.com/studio/profile/android-profiler.html)에 포함되어 있습니다.
다음 항목은 Xamarin Android 개발자의 관심 항목입니다.

-   [CPU 프로파일러](https://developer.android.com/studio/profile/cpu-profiler.html)&ndash; 실시간으로 앱의 CPU 사용량 및 스레드 작업을 검사하는 방법을 설명합니다.

-   [메모리 프로파일러](https://developer.android.com/studio/profile/memory-profiler.html)&ndash; 앱의 메모리 사용의 실시간 그래프를 표시하고, 분석의 레코드 메모리 할당에 대한 단추를 포함합니다.

-   [네트워크 프로파일러](https://developer.android.com/studio/profile/network-profiler.html)&ndash; 앱에서 보내고 받는 데이터의 실시간 네트워크 작업을 표시합니다.
