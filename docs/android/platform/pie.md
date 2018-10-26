---
title: Android 9 원형
description: Xamarin.Android를 사용 하 여 9 원형을 Android 용 앱 개발을 시작 하는 방법입니다.
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2018
ms.openlocfilehash: c353341af8899960b12437d55602415a02953cbc
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111677"
---
# <a name="android-pie-features"></a>Android 원형 기능

_Xamarin.Android를 사용 하 여 9 원형을 Android 용 앱 개발을 시작 하는 방법입니다._

[Android 9 원형](https://developer.android.com/about/versions/pie/) Google에서 제공 됩니다. 다양 한 새로운 기능 및 Api 제공 됩니다이 릴리스 및 그 중 대부분은 최신 Android 장치에서 새로운 하드웨어 기능을 활용 하는 데 필요한 합니다.

![Android 원형 영웅 이미지](pie-images/01-android-p-logo.png)

이 문서에서는 Android 원형 용 Xamarin.Android 앱 개발을 시작할 수 있도록 구성 되었습니다. 필요한 업데이트를 설치 하 고, SDK를 구성 하 고, 에뮬레이터 또는 테스트에 대 한 장치를 준비 하는 방법을 설명 합니다. 또한 Android 원형의 새로운 기능에 대 한 개요를 제공 하 고 주요 Android 원형 기능 중 일부를 사용 하는 방법을 보여 주는 소스 코드 예제를 제공 합니다.

![미리 보기](~/media/shared/preview.png)

Xamarin.Android 9.0에는 Android 원형에 대 한 미리 보기 지원을 제공합니다. Android 원형에 대 한 Xamarin.Android 지원에 대 한 자세한 내용은 참조는 [Android P 개발자 미리 보기 3](https://developer.xamarin.com/releases/android/xamarin.android_9/xamarin.android_9.0/#android-p-dp1) 릴리스 정보입니다.

## <a name="requirements"></a>요구 사항

다음은 Xamarin 기반 앱에서 Android 원형 기능을 사용 해야 합니다.

-   **Visual Studio** &ndash; Windows를 사용 하는 경우 Visual Studio 2017 버전 15.8 이상으로 업데이트 합니다. Mac을 사용 하는 경우 버전 7.6 이상이 Mac 용 Visual Studio 2017로 업데이트 합니다.

-   **Xamarin.Android** &ndash; Xamarin.Android 9.0.0.17 나중에 설치 해야 Visual Studio를 사용 하 여 또는 (Xamarin.Android의 일부로 자동으로 설치 됩니다 합니다 **.NET을 사용한 모바일 개발** 작업).

-   **Java Developer Kit** &ndash; Xamarin Android 9.0 개발 필요 [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (Microsoft의 분포의 미리 보기를 시도할 수 있습니다 합니다 [OpenJDK](~/android/get-started/installation/openjdk.md)). JDK8의 일부로 자동으로 설치 되는 **.NET을 사용한 모바일 개발** 워크 로드.

-   **Android SDK** &ndash; Android SDK Manager를 통해 Android SDK API 28 이상을 설치 해야 합니다.

## <a name="getting-started"></a>시작

Xamarin.Android 사용 하 여 Android 원형 앱 개발 시작 하려면 다운로드 하며 첫 번째 Android 원형 프로젝트를 만들려면 먼저 최신 도구 및 SDK 패키지를 설치 합니다.

1. 업데이트 [Visual Studio 2017 버전 15.8](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) 이상. Visual Studio for Mac를 사용 하는 경우 업데이트 [버전 7.6(visual Mac 용 Visual Studio 2017](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) 이상.

2. 설치할 **Android 원형 (API 28)** 패키지 및 SDK 관리자를 통해 도구입니다.

3. 대상으로 하는 새 Xamarin.Android 프로젝트를 만듭니다 **Android 9.0**합니다.

4. 에뮬레이터 또는 Android 원형 앱 테스트에 대 한 장치를 구성 합니다.

이러한 각 단계는 다음 섹션에서 설명 됩니다.


### <a name="update-visual-studio"></a>Visual Studio 업데이트

Visual Studio에 Android 원형 지원을 추가 하려면 Visual Studio 2017 버전 15.8 이상으로 업데이트 (지침은 [최신 릴리스로 Visual Studio 2017 업데이트](https://docs.microsoft.com/visualstudio/install/update-visual-studio)).

Android 원형에 지원을 추가 하려면 Visual Studio를 Mac 용 Visual Studio 2017 이상 Mac 7.6에 대 한 업데이트 (지침은 [설정 및 Mac 용 Visual Studio 설치](https://docs.microsoft.com/visualstudio/mac/installation)).


### <a name="install-the-android-sdk"></a>Android SDK 설치

Xamarin.Android 9.0을 사용 하 여 프로젝트를 만들려면 하면 먼저 사용 하 여 Android SDK Manager에 대 한 SDK 플랫폼을 설치 하려면 **Android 원형 (API 레벨 28)** 이상.

1. SDK Manager를 시작 합니다. Visual Studio에서 클릭 **도구 > Android > Android SDK Manager**합니다. Mac 용 Visual Studio에서 클릭 **도구 > SDK Manager**합니다.

2. 오른쪽 모서리에서 기어 아이콘을 클릭 하 고 선택 **리포지토리 > Google (지원 되지 않음)**:

    [![Google 리포지토리 설정](pie-images/vs/set-repo-sml.png)](pie-images/vs/set-repo.png#lightbox)

3. 설치를 **Android 원형** 으로 나열 된 SDK 패키지 **Android SDK 플랫폼 28** 에 **플랫폼** 탭 (SDK 관리자를 사용 하는 방법에 대 한 자세한 내용은 참조 하십시오 [ Android SDK 설치](~/android/get-started/installation/android-sdk.md)):

    [![Android 원형 패키지 설치](pie-images/vs/sdk-manager-sml.png)](pie-images/vs/sdk-manager.png#lightbox)

4. 에뮬레이터를 사용 하는 경우를 지 원하는 가상 장치를 만드는 **API 수준 28**합니다. 가상 장치를 만드는 방법에 대 한 자세한 내용은 참조 하세요. [가상 장치 관리 Android 장치 관리자를 사용 하 여](~/android/get-started/installation/android-emulator/device-manager.md)입니다.



### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android 프로젝트를 시작 합니다.

새 Xamarin.Android 프로젝트를 만듭니다. Xamarin 사용한 Android 개발을 처음 접하는 경우 참조 [Hello, Android](~/android/get-started/hello-android/index.md) Xamarin.Android 프로젝트 만들기에 대해 알아보려면 합니다.

Android 프로젝트를 만들면 대상 Android 9.0 이상의 버전 설정을 구성 해야 합니다. 예를 들어 Android 원형에 대 한 프로젝트를 대상으로 구성 해야 하는 프로젝트의 대상 Android API 레벨 **Android 9.0** (API 28). 설정 하 여 대상 프레임 워크 수준 API 28 이상 것이 좋습니다. Android API 수준을 구성 하는 방법에 대 한 자세한 내용은 참조 하세요 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다.


### <a name="configure-a-device-or-emulator"></a>장치 또는 에뮬레이터를 구성 합니다.

픽셀을 Nexus 등 물리적 장치를 사용 하는 경우 업데이트할 수 있습니다 장치를 Android 원형의 지침에 따라 [Nexus 픽셀 장치를 출하 시 이미지](https://developers.google.com/android/images)합니다.

에뮬레이터를 사용 하는 API 수준 28에 대 한 가상 장치를 만들고 x86 기반 이미지를 선택 합니다. Android 장치 관리자를 사용 하 여 만들고 가상 장치를 관리 하는 방법에 대 한 내용은 [가상 장치 관리 Android 장치 관리자를 사용 하 여](~/android/get-started/installation/android-emulator/device-manager.md)입니다.
Android 에뮬레이터를 사용 하 여 테스트 및 디버깅 하는 방법에 대 한 내용은 [Android 에뮬레이터에서 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)합니다.



## <a name="new-features"></a>새 기능

Android 원형 다양 한 새 기능을 소개합니다. 이러한 새 기능 중 일부 추가 Android 사용자 환경을 개선 하도록 설계 되어 다른 사용자는 최신 Android 장치에서 제공 하는 새 하드웨어 기능을 활용 하고자 합니다.

-   **잘라낸 지원 표시** &ndash; 찾을 위치 및 모양의 Api를 제공 합니다 _잘라낸_ 최신 Android 장치에서 화면 맨 위에 있는 합니다.

-   **알림 향상** &ndash; 이미지 및 새 알림 메시지를 표시할 수 있습니다 `Person` 클래스 대화 참가자를 간소화 하는 데 사용 됩니다.

-   **실내 위치** &ndash; 실내에서 탐색을 위한 WiFi 장치를 사용 하는 앱 수 있도록 하는 WiFi Round 왕복 시간 프로토콜에 대 한 플랫폼 지원 합니다.

-   **다중 카메라 지원** &ndash; (예: 이중-프런트 엔드 및 백 이중 카메라) 여러 물리적 카메라에서 동시에 스트림에 대 한 액세스 기능을 제공 합니다.


다음 섹션에서는 이러한 기능을 강조 하 고 앱에서 사용 하는 데 대 한 간략 한 코드 예제 시작을 제공 합니다.

### <a name="display-cutout-support"></a>잘라낸 지원 표시

Edge-에 지 화면을 사용 하 여 많은 최신 Android 장치에는 *잘라낸 표시* (또는 "등급") 카메라 및 스피커 디스플레이의 맨 위에 있는 합니다.
다음 스크린 샷에서 잘라낸 부분 에뮬레이터 예제를 제공 합니다.

[![잘라낸 부분을 시뮬레이션 하는 android 에뮬레이터](pie-images/02-example-cutout-sml.png)](pie-images/02-example-cutout.png#lightbox)

앱 창 표시 잘라낸 부분을 사용 하 여 장치에서 해당 콘텐츠를 표시 하는 방법을 관리 하려면 Android 원형 새로 추가 했습니다 [LayoutInDisplayCutoutMode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode) 창 레이아웃 특성입니다. 이 특성은 다음 값 중 하나로 설정할 수 있습니다.

-   [LayoutInDisplayCutoutModeNever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER) &ndash; 오려낸 영역을 사용 하 여 겹치는 창 허용 되지 않습니다.

-   [LayoutInDisplayCutoutModeShortEdges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES) &ndash; 창 잘라낸 부분 영역으로 하지만 짧은 화면 가장자리에만 확장할 수 있습니다. 

-   [LayoutInDisplayCutoutModeDefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT) &ndash; 창 잘라낸 시스템 막대 안에 포함 되어 있으면 잘라낸 부분 영역으로 확장할 수 있습니다.

예를 들어, 앱 창 오려낸 영역 중복을 방지 하려면 레이아웃 잘라낸 모드를 설정 *되지*: 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

다음 예제에서는 이러한 잘라낸 모드의 예제를 제공 합니다. 왼쪽의 첫 번째 스크린샷에서 비-전체 화면 모드에서 앱입니다. Center 스크린샷에서 앱 이동 전체 화면으로 `LayoutInDisplayCutoutMode` 로 `LayoutInDisplayCutoutModeShortEdges`합니다. 앱의 흰색 배경 표시 잘라낸 부분 영역에는 확장을 확인 합니다.

[![예제는 에뮬레이터에서 잘라낸 모드를 표시 합니다.](pie-images/03-cutout-modes-sml.png)](pie-images/03-cutout-modes.png#lightbox)

최종 스크린샷의 (위의 오른쪽), `LayoutInDisplayCutoutMode` 로 설정 된 `LayoutInDisplayCutoutModeShortNever` 전체 화면으로 이동 하기 전에 합니다.
앱의 흰색 배경 표시 오려낸 영역 안에 허용 되지 않음을 알 수 있습니다.

장치에서 잘라낸 부분 영역에 대 한 자세한 정보를 해야 하는 경우 사용할 수 있습니다 새 [DisplayCutout](https://developer.android.com/reference/android/view/DisplayCutout.html) 클래스입니다. `DisplayCutout` 콘텐츠를 표시 하려면 사용할 수 없는 디스플레이의 영역을 나타냅니다. 앱이 작동 하지 않는 영역에 콘텐츠를 표시 하지 않도록 잘라낸 모양의 위치를 검색 하려면이 정보를 사용할 수 있습니다.

Android P의 새로운 잘라낸 기능에 대 한 자세한 내용은 참조 하세요. [디스플레이 잘라낸 지원](https://developer.android.com/preview/features#cutout)합니다.



### <a name="notifications-enhancements"></a>알림 향상

Android 원형 메시징 환경을 개선 하기 위해 다음과 같은 향상 기능이 도입 되었습니다.

-   알림 채널 (에 도입 된 [Android Oreo](~/android/platform/oreo.md)) 이제 채널 그룹의 차단를 지원 합니다.

-   알림 시스템의 세 가지 새 Do Not 방해 범주 (경보, 시스템 소리 및 미디어 원본을 우선 순위 지정). 또한는 7 개의 새 Do Not 방해 모드가 visual 중단 (예: 배지, 알림 광원, 상태 표시줄 모양 및 전체 화면 작업의 시작)를 표시 하지 않으려면 사용할 수 있는 있습니다.

-   새 [Person](https://developer.android.com/reference/android/app/Person.html) 메시지의 보낸 사람을 나타내는 클래스가 추가 되었습니다. 이 클래스의 용도 등 해당 아바타 Uri 대화에 있는 사람을 식별 하 여 각 알림에 렌더링을 최적화 하기 위해 수 있습니다.

-   이제 알림 이미지를 표시할 수 있습니다. 

다음 예제에서는 새로운 Api를 사용 하 여 이미지를 포함 하는 알림을 생성 하는 방법을 보여 줍니다. 다음 스크린샷에서 텍스트 알림을 게시 되 고 포함 된 이미지를 사용 하 여 알림을 나옵니다. (처럼 오른쪽에) 알림을 확장 되어, 첫 번째 알림 텍스트를 표시 되 고 이미지에 포함 된 두 번째 알림을 확대 됩니다.

[![이미지를 사용 하 여 예제 알림](pie-images/04-example-notifications-sml.png)](pie-images/04-example-notifications.png#lightbox)

다음 예제 이미지는 Android 원형 알림을 포함 하는 방법 및 새 사용 방법을 보여 줍니다 `Person` 클래스:

1. 만들기는 `Person` 보낸 사람을 나타내는 개체입니다. 보낸 사람의 이름 및 아이콘에 포함 된 예를 들어 `fromPerson`:

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. 만들기는 `Notification.MessagingStyle.Message` 보낼 이미지가 포함 된 새 이미지 전달 [Notification.MessagingStyle.Message.SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri) 메서드.
   예를 들어:

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. 메시지를 추가 `Notification.MessagingStyle` 개체입니다. 예를 들어:

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. 알림 작성기에이 스타일을 연결 합니다. 예를 들어:

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. 알림을 게시 합니다. 예를 들어:

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

알림을 만드는 방법에 대 한 자세한 내용은 참조 하세요. [로컬 알림을](~/android/app-fundamentals/notifications/local-notifications.md)합니다.


### <a name="indoor-positioning"></a>실내 위치 지정

IEEE 802.11mc android 원형 지 (라고도 _WiFi Round 왕복 시간_ 또는 _WiFi RTT_), 하나는 거리를 검색 하는 앱에 대 한 수 있습니다 또는 Wi-fi 액세스 지점입니다. 이 정보를 사용 하는 것 활용 하기 위해 앱에 대 한 가능한 *실내 위치* 1-2 미터의 정확도 사용 하 여 합니다. IEEE 801.11mc에 대 한 하드웨어 지원을 제공 하는 Android 장치에서 앱 스마트 어플라이언스의 저장소를 통해 설정 하 여 설정 지침 위치 기반 컨트롤과 같은 탐색 기능을 제공할 수 있습니다.

[![WiFi RTT를 사용 하 여 실내 탐색의 예](pie-images/05-wifi-rtt-sml.png)](pie-images/05-wifi-rtt.png#lightbox)

새 [WifiRttManager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager) 클래스와 몇 가지 도우미 클래스 Wi-fi 장치로 거리를 측정 하기 위한 방법을 제공 합니다. Android P에 도입 된 실내 위치 Api에 대 한 자세한 내용은 참조 하세요. [Android.Net.Wifi.Rtt](https://developer.android.com/reference/android/net/wifi/rtt/package-summary)합니다.


### <a name="multi-camera-support"></a>다중 카메라 지원

많은 최신 Android 장치 이중 프런트 및/또는 이중 백 카메라 스테레오 시각, 향상 된 시각 효과 및 향상 된 확대/축소 기능으로 이러한 기능에 대 한 유용한 경우 Android P 새 소개 [다중 카메라](https://developer.android.com/preview/features#camera) 사용 하 여 앱에 대 한 수 있도록 하는 API를 *논리 카메라* (또는 *논리 다중 카메라*) 둘 이상에서 지 원하는 실제 카메라입니다.
장치 논리 다중 카메라를 지 원하는 경우에 장치에서 각 카메라의 기능 지원 되는지 여부를 살펴볼 수 있습니다 결정할 [RequestAvailableCapabilitiesLogicalMultiCamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA)합니다.

Android 원형 또한 새 [SessionConfiguration](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html) 초기 캡처하는 동안 지연을 줄이고를 시작 하려면 카메라 스트림을 필요성을 제거 하는 데 사용할 수 있는 클래스입니다.

다중 카메라에 대 한 자세한 내용은 Android P 지원, 참조 [다중 카메라 지원 및 카메라 업데이트](https://developer.android.com/preview/features#camera)합니다.


### <a name="other-features"></a>기타 기능

또한 Android 원형 다른 몇 가지 새로운 기능을 지원합니다.

-   새 [AnimatedImageDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html) 클래스 그리기 및 애니메이션된 이미지 표시에 사용할 수 있습니다.

-   새 [ImageDecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html) 클래스를 대체 하는 `BitmapFactory`합니다. `ImageDecoder` 디코딩하는 데 사용할 수는 `AnimatedImageDrawable`합니다.

-   HDR (높은 동적 범위) 비디오 및 HEIF (높은 효율성 Image File Format) 이미지를 지원 합니다.

-   합니다 [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) 네트워크 관련 작업을 더 지능적으로 처리 하도록 향상 되었습니다. 새 [GetNetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29) 메서드는 [JobParameters](https://developer.android.com/reference/android/app/job/JobParameters) 클래스는 지정된 된 작업에 대 한 모든 네트워크 요청을 수행 하는 것에 대 한 최상의 네트워크를 반환 합니다.

최신 Android 원형 기능에 대 한 자세한 내용은 참조 하세요. [Android 9 기능 및 Api](https://developer.android.com/about/versions/pie/android-9.0)합니다.


## <a name="behavior-changes"></a>동작 변경 내용

대상 Android 버전 28 API 수준으로 설정 되 면 위에서 설명한 새로운 기능을 구현 하지 않는 경우에 앱의 동작에 영향을 줄 수 있는 몇 가지 플랫폼 변경 내용이 있습니다. 다음 목록에 이러한 변경 내용을 간단한 요약입니다.

-  앱에는 포그라운드 서비스를 사용 하기 전에 전경 권한을 요청 이제 해야 합니다.

-  앱에 둘 이상의 프로세스, 단일을 공유할 수 없습니다 것 [WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 프로세스 간에 데이터 디렉터리입니다.

-  경로에서 다른 앱의 데이터 디렉터리에 직접 액세스는 더 이상 허용 합니다.

Android P를 대상으로 하는 앱의 동작 변경 내용에 대 한 자세한 내용은 참조 하세요. [동작 변경 내용](https://developer.android.com/preview/behavior-changes.html#p-apps)합니다.


## <a name="sample-code"></a>샘플 코드

[AndroidPMiniDemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo) 은 Xamarin.Android 샘플 앱 표시 잘라낸 모드를 설정 하는 방법을 보여 주는 원형 Android에 대 한 새 사용 방법 `Person` 클래스 및 이미지를 포함 하는 알림을 보내는 방법.


## <a name="summary"></a>요약

이 문서에서는 Android 원형을 도입 하 고 설치 및 Android 원형을 사용 하 여 최신 도구 및 Xamarin.Android 개발에 대 한 패키지를 구성 하는 방법을 설명 합니다. 이러한 기능 중 일부에 대 한 소스 코드 예제를 사용 하 여 Android 원형에서 사용할 수 있는 주요 기능의 개요를 제공 하는 것
API 설명서에 대 한 링크를 포함 하 고 원형 Android 용 앱을 만들 수 있도록 Android 개발자 항목 시작 합니다. 또한 기존 앱에 영향을 줄 수 있는 가장 중요 한 Android 원형 동작 변경 내용 강조 표시 합니다.


## <a name="related-links"></a>관련 링크

- [Android 9 원형](https://developer.android.com/about/versions/pie/)
