---
title: Android 9 Pie
description: Xamarin Android를 사용 하 여 Android 9 용 앱 개발을 시작 하는 방법입니다.
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2018
ms.openlocfilehash: d4d7379e1d4d2dd605331b30d692df299f5f5c13
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69523669"
---
# <a name="android-pie-features"></a>Android 원형 기능

_Xamarin Android를 사용 하 여 Android 9 용 앱 개발을 시작 하는 방법입니다._

[Android 9 원형](https://developer.android.com/about/versions/pie/) 은 이제 Google에서 제공 됩니다. 이 릴리스에서는 여러 가지 새로운 기능과 Api를 사용할 수 있으며,이 중 상당수는 최신 Android 장치에서 새로운 하드웨어 기능을 활용 하는 데 필요 합니다.

![Android 원형 주인공 이미지](pie-images/01-android-p-logo.png)

이 문서는 Android 용 Xamarin Android 앱 개발을 시작 하는 데 도움이 되는 구조화 된 문서입니다. 필요한 업데이트를 설치 하 고, SDK를 구성 하 고, 테스트할 에뮬레이터 또는 장치를 준비 하는 방법을 설명 합니다. 또한 Android 원형의 새로운 기능에 대 한 개요를 제공 하 고 몇 가지 주요 Android 원형 기능을 사용 하는 방법을 보여 주는 예제 소스 코드를 제공 합니다.

Xamarin Android 9.0는 Android 원형을 지원 합니다. Android 원형의 Xamarin Android 지원에 대 한 자세한 내용은 [Android P Developer Preview 3](https://docs.microsoft.com/xamarin/android/release-notes/9/9.0/#android-p-dp1) 릴리스 정보를 참조 하세요.

## <a name="requirements"></a>요구 사항

다음 목록은 Xamarin 기반 앱에서 Android 원형 기능을 사용 하는 데 필요 합니다.

- **Visual Studio** &ndash; Visual Studio 2019을 권장 합니다.
    Visual Studio 2017을 사용 하는 경우 Windows 업데이트에서 Visual Studio 2017 버전 15.8 이상으로 설정 합니다. MacOS에서 Mac 용 Visual Studio 2017 버전 7.6 이상으로 업데이트 합니다.

- Visual Studio를 사용 하 여 xamarin.ios 9.0.0.17 이상 버전을 설치 해야 합니다. xamarin은 .net 워크 로드 **를 사용한 모바일 개발** 의 일부로 자동으로 설치 됩니다. &ndash;

- **Java 개발자 키트** Xamarin Android 9.0 개발에는 [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 이 필요 합니다. 또는 Microsoft의 [openjdk](~/android/get-started/installation/openjdk.md)배포 미리 보기를 사용해 볼 수 있습니다. &ndash; JDK8는 .NET 워크 로드 **를 사용 하 여 모바일 개발** 의 일부로 자동으로 설치 됩니다.

- **Android SDK** &ndash; Android SDK API 28 이상은 Android SDK 관리자를 통해 설치 해야 합니다.

## <a name="getting-started"></a>시작

Xamarin.ios를 사용 하 여 Android 원형 앱 개발을 시작 하려면 먼저 최신 도구 및 SDK 패키지를 다운로드 하 여 설치 해야 합니다.

1. Visual Studio 2019를 사용하는 것이 좋습니다. Visual Studio 2017을 사용 하는 경우 [Visual studio 2017 버전 15.8](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) 이상으로 업데이트 합니다. Mac용 Visual Studio를 사용 하는 경우 [Mac 용 Visual Studio 2017 버전 7.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) 이상으로 업데이트 합니다.

2. SDK Manager를 통해 **Android 원형 (API 28)** 패키지 및 도구를 설치 합니다.

3. **Android 9.0**를 대상으로 하는 새 Xamarin android 프로젝트를 만듭니다.

4. Android 원형 앱을 테스트 하기 위한 에뮬레이터 또는 장치를 구성 합니다.

이러한 각 단계는 다음 섹션에 설명 되어 있습니다.

### <a name="update-visual-studio"></a>Visual Studio 업데이트

Xamarin을 사용 하 여 Android 원형 앱을 빌드하려면 Visual Studio 2019을 사용 하는 것이 좋습니다.

Visual Studio 2017을 사용 하는 경우 Visual studio 2017 버전 15.8 이상으로 업데이트 합니다 (자세한 내용은 [Visual studio 2017을 최신 릴리스로 업데이트](https://docs.microsoft.com/visualstudio/install/update-visual-studio)). MacOS에서 Mac 7.6 이상에 대해 Visual Studio 2017로 업데이트 합니다 (자세한 내용은 [Mac용 Visual Studio 설정 및 설치](https://docs.microsoft.com/visualstudio/mac/installation)).

### <a name="install-the-android-sdk"></a>Android SDK 설치

Xamarin. Android 9.0를 사용 하 여 프로젝트를 만들려면 먼저 Android SDK Manager를 사용 하 여 Android 용 SDK 플랫폼 **(API 레벨 28)** 이상을 설치 해야 합니다.

1. SDK Manager를 시작 합니다. Visual Studio에서 **도구 > Android > Android SDK Manager**를 클릭 합니다. Mac용 Visual Studio에서 **도구 > SDK Manager**를 클릭 합니다.

2. 오른쪽 아래 모서리에서 기어 아이콘을 클릭 하 고 **저장소 > Google (지원 되지 않음)** 을 선택 합니다.

    [![리포지토리를 Google으로 설정](pie-images/vs/set-repo-sml.png)](pie-images/vs/set-repo.png#lightbox)

3. 플랫폼 탭에 **Android SDK Platform 28** 로 나열 된 **Android 원형** SDK 패키지를 설치 합니다 (SDK Manager를 사용 하는 방법에 대 한 자세한 내용은 [Android SDK 설치](~/android/get-started/installation/android-sdk.md)참조).

    [![Android 원형 패키지 설치](pie-images/vs/sdk-manager-sml.png)](pie-images/vs/sdk-manager.png#lightbox)

4. 에뮬레이터를 사용 하는 경우 **API 레벨 28**을 지 원하는 가상 장치를 만듭니다. 가상 장치를 만드는 방법에 대 한 자세한 내용은 [Android Device Manager 사용 하 여 가상 장치 관리](~/android/get-started/installation/android-emulator/device-manager.md)를 참조 하세요.

### <a name="start-a-xamarinandroid-project"></a>Xamarin Android 프로젝트 시작

새 Xamarin Android 프로젝트를 만듭니다. Xamarin을 사용 하 여 Android 개발을 처음 접하는 경우에는 [Hello, android](~/android/get-started/hello-android/index.md) 를 참조 하 여 xamarin.ios 프로젝트를 만드는 방법에 대해 알아보세요.

Android 프로젝트를 만들 때 Android 9.0 이상 버전을 대상으로 하도록 버전 설정을 구성 해야 합니다. 예를 들어 Android 원형 용 프로젝트를 대상으로 하려면 프로젝트의 대상 Android API 수준을 **android 9.0** (API 28)로 구성 해야 합니다. 대상 프레임 워크 수준을 API 28 이상으로 설정 하는 것이 좋습니다. Android API 수준 구성에 대 한 자세한 내용은 [ANDROID Api 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조 하세요.


### <a name="configure-a-device-or-emulator"></a>장치 또는 에뮬레이터 구성

Nexus 또는 픽셀과 같은 물리적 장치를 사용 하는 경우 [nexus 및 픽셀 장치용 팩터리 이미지](https://developers.google.com/android/images)의 지침에 따라 Android 원형으로 장치를 업데이트할 수 있습니다.

에뮬레이터를 사용 하는 경우 API 레벨 28 용 가상 장치를 만들고 x86 기반 이미지를 선택 합니다. Android Device Manager를 사용 하 여 가상 장치를 만들고 관리 하는 방법에 대 한 자세한 내용은 Android Device Manager를 사용 하 여 [가상 장치 관리](~/android/get-started/installation/android-emulator/device-manager.md)를 참조 하세요.
테스트 및 디버깅에 Android 에뮬레이터를 사용 하는 방법에 대 한 자세한 내용은 [Android Emulator의 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)을 참조 하세요.



## <a name="new-features"></a>새 기능

Android 원형에는 다양 한 새로운 기능이 도입 되었습니다. 이러한 새로운 기능 중 일부는 최신 Android 장치에서 제공 하는 새로운 하드웨어 기능을 활용 하기 위한 것이 고, 다른 일부는 Android 사용자 환경을 더욱 향상 시키기 위해 설계 되었습니다.

- **컷아웃 지원 표시** 최신 Android 장치에서 화면 위쪽에 있는 잘라낸 위치와 모양을 찾는 api를 제공 합니다. &ndash;

- **알림 기능 향상** 이제 알림 메시지에서 이미지를 표시 하 고 대화 `Person` 참가자를 간소화 하는 데 새 클래스를 사용할 수 있습니다. &ndash;

- **실내 위치 지정** &ndash; Wifi 왕복 시간 프로토콜에 대 한 플랫폼 지원으로 앱에서 실내 설정 탐색에 wifi 장치를 사용할 수 있습니다.

- **다중 카메라 지원** &ndash; 에서는 여러 실제 카메라 (예: 이중 전면 카메라 및 이중 후면 카메라)에서 스트림에 동시에 액세스할 수 있는 기능을 제공 합니다.


다음 섹션에서는 이러한 기능을 강조 하 고 응용 프로그램에서 사용을 시작 하는 데 도움이 되는 간단한 코드 예제를 제공 합니다.

### <a name="display-cutout-support"></a>컷아웃 지원 표시

Edge to edge 화면을 사용 하는 최신 Android 장치에는 카메라 및 스피커의 디스플레이 위쪽에 *표시 컷아웃* (또는 "노치")가 있습니다.
다음 스크린샷은 절단의 에뮬레이터 예제를 제공 합니다.

[![컷아웃 시뮬레이션을 시뮬레이트하는 Android 에뮬레이터](pie-images/02-example-cutout-sml.png)](pie-images/02-example-cutout.png#lightbox)

응용 프로그램 창에 표시 되는 장치에서 콘텐츠를 표시 하는 방법을 관리 하려면 Android 원형에서 새로운 [LayoutInDisplayCutoutMode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode) 창 레이아웃 특성을 추가 했습니다. 이 특성은 다음 값 중 하나로 설정할 수 있습니다.

- [LayoutInDisplayCutoutModeNever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER) &ndash; 창은 잘라낸 영역과 겹칠 수 없습니다.

- [LayoutInDisplayCutoutModeShortEdges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES) &ndash; 창은 잘라낸 영역으로 확장 될 수 있지만 화면의 짧은 가장자리 에서만 가능 합니다. 

- [LayoutInDisplayCutoutModeDefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT) &ndash; 잘라낸 부분을 시스템 표시줄 안에 포함 하는 경우 창을 잘라낸 영역으로 확장할 수 있습니다.

예를 들어 응용 프로그램 창이 잘라낸 영역과 겹치지 않도록 하려면 레이아웃 오려내기 모드를 사용 *안 함*으로 설정 합니다. 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

다음 예에서는 이러한 컷아웃 모드의 예제를 제공 합니다. 왼쪽의 첫 번째 스크린샷은 비 전체 화면 모드의 앱입니다. 가운데 스크린샷에는가로 `LayoutInDisplayCutoutMode` `LayoutInDisplayCutoutModeShortEdges`설정 된 상태에서 앱이 전체 화면으로 이동 합니다. 앱의 흰색 배경은 표시 컷아웃 영역으로 확장 됩니다.

[![에뮬레이터에서 잘라낸 모드를 표시 하는 예제](pie-images/03-cutout-modes-sml.png)](pie-images/03-cutout-modes.png#lightbox)

마지막 스크린샷 (오른쪽 위의) `LayoutInDisplayCutoutMode` 에서는 전체 화면으로 이동 하기 전에로 `LayoutInDisplayCutoutModeShortNever` 설정 됩니다.
앱의 흰색 배경은 표시 컷아웃 영역으로 확장할 수 없습니다.

장치의 오려내기 영역에 대 한 자세한 정보가 필요한 경우 새 [Displaycutout](https://developer.android.com/reference/android/view/DisplayCutout.html) 클래스를 사용할 수 있습니다. `DisplayCutout`콘텐츠를 표시 하는 데 사용할 수 없는 디스플레이 영역을 나타냅니다. 이 정보를 사용 하 여 앱이 기능을 사용 하지 않는 영역에 콘텐츠를 표시 하지 않도록 잘라낸 위치와 셰이프를 검색할 수 있습니다.

Android P의 새로운 컷아웃 기능에 대 한 자세한 내용은 [잘라낸 부분 표시 지원](https://developer.android.com/about/versions/pie/android-9.0#cutout)을 참조 하세요.



### <a name="notifications-enhancements"></a>알림 기능 향상

Android 원형은 다음과 같은 향상 된 기능을 제공 하 여 메시징 환경을 향상 시킵니다.

- 알림 채널 ( [Android Oreo](~/android/platform/oreo.md)에서 도입 됨)은 이제 채널 그룹의 차단을 지원 합니다.

- 알림 시스템에는 경보, 시스템 소리 및 미디어 원본에 우선 순위를 정하는 세 가지 새로운 비-방해 범주가 있습니다. 또한 시각적 중단을 방지 하는 데 사용할 수 있는 7 가지 새로운 비-방해 모드 (예: 배지, 알림 광원, 상태 표시줄 모양, 전체 화면 작업 시작)가 있습니다.

- 메시지의 보낸 사람을 나타내기 위해 새 [Person](https://developer.android.com/reference/android/app/Person.html) 클래스가 추가 되었습니다. 이 클래스를 사용 하면 대화에 포함 된 사용자 (아바타 및 Uri 포함)를 식별 하 여 각 알림의 렌더링을 최적화할 수 있습니다.

- 이제 알림이 이미지를 표시할 수 있습니다. 

다음 예제에서는 새 Api를 사용 하 여 이미지를 포함 하는 알림을 생성 하는 방법을 보여 줍니다. 다음 스크린샷에는 텍스트 알림이 게시 되 고 다음에 포함 된 이미지를 사용 하는 알림이 발생 합니다. 오른쪽에 표시 된 대로 알림이 확장 되 면 첫 번째 알림의 텍스트가 표시 되 고 두 번째 알림에 포함 된 이미지는 확대 됩니다.

[![이미지를 사용 하는 예제 알림](pie-images/04-example-notifications-sml.png)](pie-images/04-example-notifications.png#lightbox)

다음 예제에서는 Android 원형 알림에 이미지를 포함 하는 방법을 보여 줍니다 .이 예제에서는 새 `Person` 클래스를 사용 하는 방법을 보여 줍니다.

1. 보낸 사람 `Person` 을 나타내는 개체를 만듭니다. 예를 들어 발신자의 이름과 아이콘은에 `fromPerson`포함 되어 있습니다.

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. 보낼 이미지 `Notification.MessagingStyle.Message` 를 포함 하는을 만들어 이미지를 새 [Notification. messagingstyle. Message. SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri) 메서드로 전달 합니다.
   예를 들어:

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. `Notification.MessagingStyle` 개체에 메시지를 추가 합니다. 예:

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. 이 스타일을 알림 작성기에 연결 합니다. 예:

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. 알림을 게시 합니다. 예:

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

알림을 만드는 방법에 대 한 자세한 내용은 [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)을 참조 하세요.


### <a name="indoor-positioning"></a>실내 위치 지정

Android 원형은 IEEE 802.11 mc ( _wifi 왕복 시간_ 또는 _wifi RTT_라고도 함)에 대 한 지원을 제공 하 여 앱이 하나 이상의 wi-fi 액세스 지점과의 거리를 검색할 수 있도록 합니다. 이 정보를 사용 하 여 앱이 1 ~ 2 미터의 정확도로 *실내 배치* 를 활용할 수 있습니다. IEEE 801.11 mc에 대 한 하드웨어 지원을 제공 하는 Android 장치에서 앱은 스마트 어플라이언스의 위치 기반 제어와 같은 탐색 기능을 제공 하거나 스토어를 통해 단계별 지침을 수행할 수 있습니다.

[![WiFi RTT를 사용 하는 실내 탐색의 예](pie-images/05-wifi-rtt-sml.png)](pie-images/05-wifi-rtt.png#lightbox)

새 [WifiRttManager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager) 클래스와 여러 도우미 클래스는 wi-fi 장치에 대 한 거리를 측정 하는 방법을 제공 합니다. Android P에서 도입 된 실내 위치 지정 Api에 대 한 자세한 내용은 [android .net.](https://developer.android.com/reference/android/net/wifi/rtt/package-summary)


### <a name="multi-camera-support"></a>다중 카메라 지원

최신 Android 장치에는 스테레오 비전과 향상 된 시각적 효과 및 향상 된 확대/축소 기능과 같은 기능에 유용한 이중 전면 및/또는 이중 카메라를 제공 합니다. Android P는 응용 프로그램에서 둘 이상의 실제 카메라에 의해 지원 되는 *논리 카메라* (또는 *논리적 다중 카메라*)를 사용할 수 있도록 하는 새로운 [다중 카메라](https://developer.android.com/about/versions/pie/android-9.0#camera) API를 도입 했습니다.
장치가 논리적 다중 카메라를 지원 하는지 확인 하기 위해 장치에서 각 카메라의 기능을 확인 하 여 [RequestAvailableCapabilitiesLogicalMultiCamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA)을 지원 하는지 확인할 수 있습니다.

Android 원형에는 초기 캡처 시 지연을 줄이고 카메라 스트림을 시작 하 고 시작할 필요가 없도록 하는 데 사용할 수 있는 새로운 [SessionConfiguration](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html) 클래스도 포함 되어 있습니다.

Android P의 다중 카메라 지원에 대 한 자세한 내용은 [다중 카메라 지원 및 카메라 업데이트](https://developer.android.com/about/versions/pie/android-9.0#camera)를 참조 하세요.


### <a name="other-features"></a>기타 기능

또한 Android 원형은 다음과 같은 여러 가지 새로운 기능을 지원 합니다.

- 애니메이션 이미지를 그리거나 표시 하는 데 사용할 수 있는 새 [AnimatedImageDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html) 클래스입니다.

- `BitmapFactory`을 대체 하는 새 [ImageDecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html) 클래스입니다. `ImageDecoder`를 디코딩하 `AnimatedImageDrawable`는 데 사용할 수 있습니다.

- HDR (높은 동적 범위) 비디오 및 뛰어난 경우 (높은 효율성 이미지 파일 형식) 이미지에 대 한 지원.

- [Jobscheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) 는 네트워크 관련 작업을 보다 지능적으로 처리 하도록 개선 되었습니다. [Jobparameters](https://developer.android.com/reference/android/app/job/JobParameters) 클래스의 새 [getnetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29) 메서드는 지정 된 작업에 대 한 네트워크 요청을 수행 하기 위한 최상의 네트워크를 반환 합니다.

최신 Android 원형 기능에 대 한 자세한 내용은 [android 9의 기능 및 api](https://developer.android.com/about/versions/pie/android-9.0)를 참조 하세요.


## <a name="behavior-changes"></a>동작 변경

대상 Android 버전이 API 수준 28로 설정 된 경우 위에 설명 된 새로운 기능을 구현 하지 않는 경우에도 앱의 동작에 영향을 줄 수 있는 몇 가지 플랫폼 변경이 있습니다. 다음 목록은 이러한 변경 내용에 대 한 간략 한 요약입니다.

- 이제 응용 프로그램은 포그라운드 서비스를 사용 하기 전에 포그라운드 권한을 요청 해야 합니다.

- 앱에 프로세스가 두 개 이상 있는 경우 여러 프로세스에서 단일 [웹 보기](xref:Android.Webkit.WebView) 데이터 디렉터리를 공유할 수 없습니다.

- 경로를 기준으로 다른 응용 프로그램의 데이터 디렉터리에 직접 액세스 하는 것은 더 이상 허용 되지 않습니다.

Android P를 대상으로 하는 앱의 동작 변경에 대 한 자세한 내용은 [동작 변경](https://developer.android.com/about/versions/pie/android-9.0-changes-all#p-apps)을 참조 하세요.


## <a name="sample-code"></a>샘플 코드

[AndroidPMiniDemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo) 는 디스플레이 컷아웃 모드를 설정 하는 방법, 새 `Person` 클래스를 사용 하는 방법 및 이미지를 포함 하는 알림을 보내는 방법을 보여 주는 android 용 Xamarin 샘플 앱입니다.


## <a name="summary"></a>요약

이 문서에서는 Android 원형을 소개 하 고 Android 원형을 사용 하 여 Xamarin Android 개발용 최신 도구 및 패키지를 설치 하 고 구성 하는 방법을 설명 했습니다. Android 원형에서 사용할 수 있는 주요 기능에 대 한 개요를 제공 했으며 이러한 기능 중 몇 가지에 대 한 소스 코드를 예로 들 수 있습니다.
Android 용 앱 만들기를 시작 하는 데 도움이 되는 API 설명서 및 Android 개발자 항목에 대 한 링크가 포함 되어 있습니다. 또한 기존 앱에 영향을 줄 수 있는 가장 중요 한 Android 원형 동작 변경 내용도 강조 표시 되어 있습니다.


## <a name="related-links"></a>관련 링크

- [Android 9 원형](https://developer.android.com/about/versions/pie/)
