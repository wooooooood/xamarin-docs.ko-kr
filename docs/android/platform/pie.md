---
title: Android 9 Pie
description: Xamarin.Android를 사용하여 Android 9 Pie용 앱 개발을 시작하는 방법입니다.
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2018
ms.openlocfilehash: 0105b43116df697bc6688becb77298c236dfa601
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019881"
---
# <a name="android-pie-features"></a>Android Pie 기능

_Xamarin.Android를 사용하여 Android 9 Pie용 앱 개발을 시작하는 방법입니다._

이제 Google에서 [Android 9 Pie](https://developer.android.com/about/versions/pie/)를 사용할 수 있습니다. 이 릴리스에서는 여러 가지 새로운 기능과 API를 사용할 수 있으며 이 중 상당수는 최신 Android 디바이스에서 새로운 하드웨어 기능을 활용하는 데 필요합니다.

![Android Pie 영웅 이미지](pie-images/01-android-p-logo.png)

이 문서는 Android Pie용 Xamarin.Android 앱 개발을 시작하는 데 도움이 되도록 구성되었습니다. 필요한 업데이트를 설치하고, SDK를 구성하고, 테스트할 에뮬레이터 또는 디바이스를 준비하는 방법을 설명합니다. 또한 Android Pie의 새로운 기능에 대한 개요를 제공하고 주요 Android Pie 기능 중 일부를 사용하는 방법을 보여주는 예제 소스 코드를 제공합니다.

Xamarin.Android 9.0은 Android Pie를 지원합니다. Android Pie에 대한 Xamarin.Android 지원에 대한 자세한 내용은 [Android P 개발자 미리 보기 3](https://docs.microsoft.com/xamarin/android/release-notes/9/9.0/#android-p-dp1)릴리즈 정보를 참조하세요.

## <a name="requirements"></a>요구 사항

Xamarin 기반 앱에서 Android Pie 기능을 사용하려면 다음 목록이 필요합니다.

- **Visual Studio** &ndash; Visual Studio 2019를 사용하는 것이 좋습니다.
    Visual Studio 2017을 사용하는 경우 Windows에서 Visual Studio 2017 버전 15.8 이상으로 업데이트합니다. macOS에서 Mac용 Visual Studio 2017 버전 7.6 이상으로 업데이트합니다.

- **Xamarin.Android** &ndash; Xamarin.Android 9.0.0.17 이상은 Visual Studio와 함께 설치되어야 합니다(Xamarin.Android는 **.NET을 이용한 모바일 개발** 워크로드의 일부로 자동 설치됨).

- **Java 개발자 키트** &ndash; Xamarin Android 9.0 개발에는 [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)이 필요합니다. Microsoft의 [OpenJDK](~/android/get-started/installation/openjdk.md)에 대한 미리 보기를 사용해 볼 수도 있습니다. JDK8은 **.NET을 사용한 모바일 개발** 워크로드의 일부로 자동으로 설치됩니다.

- **Android SDK** &ndash; Android SDK 관리자를 통해 Android SDK API 28 이상을 설치해야 합니다.

## <a name="getting-started"></a>시작

Xamarin.Android를 사용하여 Android Pie 앱 개발을 시작하려면 첫 번째 Android Pie 프로젝트를 만들기 전에 최신 도구 및 SDK 패키지를 다운로드하여 설치해야 합니다.

1. Visual Studio 2019를 사용하는 것이 좋습니다. Visual Studio 2017을 사용하는 경우 [Visual Studio 2017 버전 15.8](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) 이상으로 업데이트합니다. Mac용 Visual Studio를 사용하는 경우 [Mac용 Visual Studio 2017 버전 7.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) 이상으로 업데이트합니다.

2. SDK 관리자를 통해 **Android Pie(API 28)** 패키지 및 도구를 설치합니다.

3. **Android 9.0**을 대상으로 하는 새 Xamarin.Android 프로젝트를 만듭니다.

4. Android Pie 앱을 테스트하기 위한 에뮬레이터 또는 디바이스를 구성합니다.

각 단계는 다음 섹션에서 설명합니다.

### <a name="update-visual-studio"></a>Visual Studio 업데이트

Xamarin을 사용하여 Android Pie 앱을 빌드하려면 Visual Studio 2019를 사용하는 것이 좋습니다.

Visual Studio 2017을 사용하는 경우 Visual Studio 2017 버전 15.8 이상으로 업데이트합니다. 자세한 내용은 [Visual Studio 2017을 최신 릴리스로 업데이트](https://docs.microsoft.com/visualstudio/install/update-visual-studio)를 참조하세요. macOS에서 Mac용 Visual Studio 2017 7.6 이상으로 업데이트합니다. 자세한 내용은 [Mac용 Visual Studio 설정 및 설치](https://docs.microsoft.com/visualstudio/mac/installation)를 참조하세요.

### <a name="install-the-android-sdk"></a>Android SDK 설치

Xamarin.Android 9.0을 통해 프로젝트를 만들려면 먼저 Android SDK 관리자를 사용하여 **Android Pie(API 레벨 28)** 이상에 대한 SDK 플랫폼을 설치해야 합니다.

1. SDK 관리자를 시작합니다. Visual Studio에서 **도구 > Android > Android SDK 관리자**를 클릭합니다. Mac용 Visual Studio에서 **도구 > SDK 관리자**를 클릭합니다.

2. 오른쪽 아래 모서리에서 기어 아이콘을 클릭하고 **리포지토리 > Google(지원되지 않음)** 을 선택합니다.

    [![리포지토리를 Google로 설정](pie-images/vs/set-repo-sml.png)](pie-images/vs/set-repo.png#lightbox)

3. **플랫폼** 탭에서 **Android SDK 플랫폼 28**로 나열된 **Android Pie** SDK 패키지를 설치합니다(SDK 관리자 사용에 대한 자세한 내용은 [Android SDK 설치](~/android/get-started/installation/android-sdk.md) 참조).

    [![Android Pie 패키지 설치](pie-images/vs/sdk-manager-sml.png)](pie-images/vs/sdk-manager.png#lightbox)

4. 에뮬레이터를 사용하는 경우 **API 레벨 28**을 지원하는 가상 디바이스를 만듭니다. 가상 디바이스 만들기에 대한 자세한 내용은 [Android 디바이스 관리자를 사용하여 가상 디바이스 관리](~/android/get-started/installation/android-emulator/device-manager.md)를 참조하세요.

### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android 프로젝트 시작

새 Xamarin.Android 프로젝트를 만듭니다. Xamarin을 사용한 Android 개발을 처음 접하는 경우 [Hello, Android](~/android/get-started/hello-android/index.md)를 참조하여 Xamarin.Android 프로젝트를 만드는 방법에 대해 알아보세요.

Android 프로젝트를 만들 때는 Android 9.0 이상을 대상으로 버전 설정을 구성해야 합니다. 예를 들어 Android Pie용 프로젝트를 대상으로 하려면 프로젝트의 대상 Android API 레벨을 **Android 9.0**(API 28)으로 구성해야 합니다. 또한 대상 프레임워크 수준을 API 28 이상으로 설정하는 것이 좋습니다. Android API 레벨을 구성하는 방법에 대한 자세한 내용은 [Android API 레벨 이해](~/android/app-fundamentals/android-api-levels.md)를 참조하세요.

### <a name="configure-a-device-or-emulator"></a>디바이스 또는 에뮬레이터 구성

Nexus 또는 Pixel과 같은 물리적 디바이스를 사용하는 경우 [Nexus 및 Pixel 디바이스에 대한 팩터리 이미지](https://developers.google.com/android/images)의 지침에 따라 Android Pie로 디바이스를 업데이트할 수 있습니다.

에뮬레이터를 사용하는 경우 API 레벨 28용 가상 디바이스를 만들고 x86 기반 이미지를 선택합니다. Android 디바이스 관리자를 사용하여 가상 디바이스를 만들고 관리하는 방법에 대한 자세한 내용은 [Android 디바이스 관리자를 사용하여 가상 디바이스 관리](~/android/get-started/installation/android-emulator/device-manager.md)를 참조하세요.
테스트 및 디버깅을 위해 Android 에뮬레이터를 사용하는 방법에 대한 자세한 내용은 [Android 에뮬레이터에서 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)을 참조하세요.

## <a name="new-features"></a>새 기능

Android Pie에는 다양한 새로운 기능이 도입되었습니다. 해당 새로운 기능 중 일부는 최신 Android 디바이스에서 제공하는 새로운 하드웨어 기능을 활용하기 위한 것이며 다른 일부는 Android 사용자 환경을 더욱 향상하기 위해 설계되었습니다.

- **디스플레이 컷아웃 지원** &ndash; 최신 Android 디바이스의 화면 맨 위에 표시되는 _컷아웃_의 위치와 모양을 찾기 위한 API를 제공합니다.

- **알림 기능 향상** &ndash; 알림 메시지에서 이미지를 표시할 수 있으며, 대화 참가자를 간소화하는 데 새로운 `Person` 클래스가 사용됩니다.

- **실내 위치 지정** &ndash; WiFi Round-Trip-Time 프로토콜에 대한 플랫폼이 지원되므로 실내 설정에서 앱이 탐색을 위해 WiFi 디바이스를 사용할 수 있도록 합니다.

- **다중 카메라 지원** &ndash; 여러 실제 카메라(예: 이중 전면 카메라 및 이중 후면 카메라)에서 동시에 스트림에 액세스하는 기능을 제공합니다.

다음 섹션에서는 해당 기능을 중점적으로 설명하고 앱에서 해당 기능을 사용하는 데 도움이 되는 간단한 코드 예제를 제공합니다.

### <a name="display-cutout-support"></a>디스플레이 컷아웃 지원

더 넓은 화면이 채택된 많은 최신 Android 디바이스의 디스플레이 맨 위에는 카메라 및 스피커를 위한 ‘디스플레이 컷아웃’(또는 “노치”)이 있습니다. 
다음 스크린샷은 에뮬레이터의 컷아웃 예입니다.

[![컷아웃을 시뮬레이션하는 Android Emulator](pie-images/02-example-cutout-sml.png)](pie-images/02-example-cutout.png#lightbox)

디스플레이 컷아웃이 있는 디바이스에서 앱 창에 콘텐츠가 표시되는 방법을 관리하기 위해 Android Pie는 새 [LayoutInDisplayCutoutMode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode) 창 레이아웃 특성을 추가했습니다. 이 특성은 다음 값 중 하나로 설정할 수 있습니다.

- [LayoutInDisplayCutoutModeNever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER) &ndash; 창은 컷아웃 영역과 겹칠 수 없습니다.

- [LayoutInDisplayCutoutModeShortEdges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES) &ndash; 창은 컷아웃 영역으로 확장될 수 있지만 화면의 짧은 가장자리에서만 확장됩니다. 

- [LayoutInDisplayCutoutModeDefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT) &ndash; 창은 컷아웃이 시스템 표시줄 내에 포함된 경우에만 컷아웃 영역으로 확장될 수 있습니다.

예를 들어, 앱 창이 컷아웃 영역과 겹치지 않도록 하려면 레이아웃 컷아웃 모드를 ‘never’로 설정합니다.  

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

다음 예제에서는 해당 컷아웃 모드의 예를 제공합니다. 왼쪽의 첫 번째 스크린샷은 전체 화면이 아닌 모드로 표시된 앱입니다. 가운데 스크린샷에서 앱은 `LayoutInDisplayCutoutMode`가 `LayoutInDisplayCutoutModeShortEdges`로 설정된 전체 화면 모드가 됩니다. 앱의 흰색 배경은 디스플레이 컷아웃 영역으로 확장됩니다.

[![에뮬레이터의 예제 디스플레이 컷아웃 모드](pie-images/03-cutout-modes-sml.png)](pie-images/03-cutout-modes.png#lightbox)

마지막 스크린샷(오른쪽 위)에서 `LayoutInDisplayCutoutMode`는 전체 화면이 되기 전에 `LayoutInDisplayCutoutModeShortNever`로 설정됩니다.
앱의 흰색 배경은 디스플레이 컷아웃 영역으로 확장될 수 없습니다.

디바이스에서 컷아웃 영역에 대한 자세한 정보가 필요한 경우 새 [DisplayCutout](https://developer.android.com/reference/android/view/DisplayCutout.html) 클래스를 사용할 수 있습니다. `DisplayCutout`은 콘텐츠를 표시하는 데 사용할 수 없는 디스플레이 영역을 나타냅니다. 앱이 해당 비기능 영역에 콘텐츠를 표시하려고 하지 않도록 이 정보를 사용하여 컷아웃의 위치와 모양을 검색할 수 있습니다.

Android P의 새로운 컷아웃 기능에 대한 자세한 내용은 [디스플레이 컷아웃 지원](https://developer.android.com/about/versions/pie/android-9.0#cutout)을 참조하세요.

### <a name="notifications-enhancements"></a>알림 기능 향상

Android Pie는 다음과 같은 향상된 기능을 제공하여 메시징 환경을 향상시킵니다.

- 알림 채널([Android Oreo](~/android/platform/oreo.md)에 도입됨)은 이제 채널 그룹을 차단하도록 지원합니다.

- 알림 시스템에는 경보, 시스템 소리 및 미디어 원본에 우선 순위를 지정하는 세 가지 새로운 방해 금지 범주가 있습니다. 또한 시각적 중단을 방지하는 데 사용할 수 있는 7가지 새로운 방해 금지 모드(예: 배지, 알림 표시등, 상태 표시줄 모양, 전체 화면 작업 시작)가 있습니다.

- 메시지 보낸 사람을 나타내기 위해 새 [Person](https://developer.android.com/reference/android/app/Person.html) 클래스가 추가되었습니다. 이 클래스를 사용하면 대화에 포함된 사용자(아바타 및 URI 포함)를 식별하여 각 알림의 렌더링을 최적화할 수 있습니다.

- 이제 알림에 이미지를 표시할 수 있습니다. 

다음 예제에서는 새 API를 사용하여 이미지를 포함하는 알림을 생성하는 방법을 보여 줍니다. 다음 스크린샷에는 텍스트 알림이 게시되고 그 뒤에 포함된 이미지가 있는 알림이 표시됩니다. 오른쪽에 표시된 대로 알림이 확장되면 첫 번째 알림의 텍스트가 표시되고 두 번째 알림에 포함된 이미지는 확대됩니다.

[![이미지를 포함하는 예제 알림](pie-images/04-example-notifications-sml.png)](pie-images/04-example-notifications.png#lightbox)

다음 예제에서는 Android Pie 알림에 이미지를 포함하는 방법과 새 `Person` 클래스를 사용하는 방법을 보여 줍니다.

1. 보낸 사람을 나타내는 `Person` 개체를 만듭니다. 예를 들어, 보낸 사람의 이름과 아이콘이 `fromPerson`에 포함됩니다.

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. 보낼 이미지를 포함하는 `Notification.MessagingStyle.Message`를 만들고 이미지를 새 [Notification.MessagingStyle.Message.SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri) 메서드에 전달합니다.
   예를 들어:

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. `Notification.MessagingStyle` 개체에 메시지를 추가합니다. 예를 들어:

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. 이 스타일을 알림 작성기에 연결합니다. 예를 들어:

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. 알림을 게시합니다. 예를 들어:

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

알림을 만드는 방법에 대한 자세한 내용은 [로컬 알림](~/android/app-fundamentals/notifications/local-notifications.md)을 참조하세요.

### <a name="indoor-positioning"></a>실내 위치 지정

Android Pie는 IEEE 802.11mc(_WiFi Round-Trip-Time_ 또는 _WiFi RTT_라고도 함)를 지원하여 앱이 하나 이상의 Wi-Fi 액세스 지점까지의 거리를 검색할 수 있도록 합니다. 이 정보를 사용하여 앱이 1~2m의 정확도로 ‘실내 위치 지정’을 활용할 수 있도록 합니다.  IEEE 801.11mc에 대한 하드웨어 지원을 제공하는 Android 디바이스에서 앱은 스토어를 통해 스마트 어플라이언스의 위치 기반 제어 또는 turn-by-turn 지침과 같은 탐색 기능을 제공할 수 있습니다.

[![WiFi RTT를 사용하는 실내 탐색 예제](pie-images/05-wifi-rtt-sml.png)](pie-images/05-wifi-rtt.png#lightbox)

새 [WifiRttManager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager) 클래스와 여러 도우미 클래스를 통해 Wi-Fi 디바이스가 얼마나 떨어져 있는지 측정할 수 있습니다. Android P에 도입된 실내 위치 지정 API에 대한 자세한 내용은 [Android.Net.Wifi.Rtt](https://developer.android.com/reference/android/net/wifi/rtt/package-summary)를 참조하세요.

### <a name="multi-camera-support"></a>다중 카메라 지원

많은 최신 Android 디바이스에는 스테레오 비전, 향상된 시각적 효과 및 향상된 확대/축소 기능 등의 기능에 유용한 이중 전면 및/또는 이중 후면 카메라가 있습니다. Android P는 앱이 둘 이상의 실제 카메라에서 지원되는 ‘논리적 카메라’(또는 ‘논리적 다중 카메라’)를 사용할 수 있도록 하는 새로운 [다중 카메라](https://developer.android.com/about/versions/pie/android-9.0#camera) API를 도입했습니다.  
디바이스에서 논리적 다중 카메라를 지원하는지 확인하려면 디바이스에서 각 카메라의 기능을 확인하여 [RequestAvailableCapabilitiesLogicalMultiCamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA)를 지원하는지 알아볼 수 있습니다.

Android Pie에는 초기 캡처 시 지연을 줄이고 카메라 스트림을 시작 및 중지할 필요가 없도록 하기 위해 사용할 수 있는 새로운 [SessionConfiguration](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html) 클래스도 포함되어 있습니다.

Android P의 다중 카메라 지원에 대한 자세한 내용은 [다중 카메라 지원 및 카메라 업데이트](https://developer.android.com/about/versions/pie/android-9.0#camera)를 참조하세요.

### <a name="other-features"></a>기타 기능

또한 Android Pie는 다음과 같은 여러 가지 새로운 기능을 지원합니다.

- 애니메이션 이미지를 그리거나 표시하는 데 사용할 수 있는 새로운 [AnimatedImageDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html) 클래스

- `BitmapFactory`을 대체하는 새로운 [ImageDecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html) 클래스. `ImageDecoder`는 `AnimatedImageDrawable`을 디코딩하는 데 사용할 수 있습니다.

- HDR(High Dynamic Range) 비디오 및 HEIF(High Efficiency Image File Format) 이미지에 대한 지원

- [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)는 네트워크 관련 작업을 보다 지능적으로 처리하도록 개선되었습니다. [JobParameters](https://developer.android.com/reference/android/app/job/JobParameters) 클래스의 새 [GetNetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29) 메서드는 지정된 작업에 대한 네트워크 요청을 수행하기 위한 최상의 네트워크를 반환합니다.

최신 Android Pie 기능에 대한 자세한 내용은 [Android 9 기능 및 API](https://developer.android.com/about/versions/pie/android-9.0)를 참조하세요.

## <a name="behavior-changes"></a>동작 변경

대상 Android 버전이 API 레벨 28로 설정되면 위에서 설명된 새로운 기능을 구현하지 않더라도 앱의 동작에 영향을 주는 몇 가지 플랫폼 변경 내용이 있습니다. 다음 목록은 해당 변경 내용을 간략하게 요약한 것입니다.

- 이제 앱은 포그라운드 서비스를 사용하기 전에 포그라운드 권한을 요청해야 합니다.

- 앱에 둘 이상의 프로세스가 있는 경우 여러 프로세스에서 단일 [WebView](xref:Android.Webkit.WebView) 데이터 디렉터리를 공유할 수 없습니다.

- 경로에 따라 다른 앱 데이터 디렉터리에 직접 액세스하는 것은 더 이상 허용되지 않습니다.

Android P를 대상으로 하는 앱의 동작 변경에 대한 자세한 내용은 [동작 변경](https://developer.android.com/about/versions/pie/android-9.0-changes-all#p-apps)을 참조하세요.

## <a name="sample-code"></a>샘플 코드

[AndroidPMiniDemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo)는 디스플레이 컷아웃 모드를 설정하는 방법, 새 `Person` 클래스를 사용하는 방법 및 이미지를 포함하는 알림을 보내는 방법을 보여 주는 Android Pie용 Xamarin.Android 샘플 앱입니다.

## <a name="summary"></a>요약

이 문서에서는 Android Pie를 소개하고 Android Pie를 사용하여 Xamarin.Android 개발을 위한 최신 도구 및 패키지를 설치 및 구성하는 방법을 설명했습니다. Android Pie에서 사용할 수 있는 주요 기능에 대한 개요를 제공하고 해당 기능 중 몇 가지에 대한 소스 코드를 예로 제공했습니다.
여기에는 Android Pie용 앱을 만드는 과정을 시작하는 데 도움이 되는 API 설명서 및 Android 개발자 항목에 대한 링크가 포함되어 있습니다. 기존 앱에 영향을 줄 수 있는 가장 중요한 Android Pie 동작 변경 내용도 강조 표시되어 있습니다.

## <a name="related-links"></a>관련 링크

- [Android 9 Pie](https://developer.android.com/about/versions/pie/)
