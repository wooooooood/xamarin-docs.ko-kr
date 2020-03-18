---
title: Nougat 기능
description: Xamarin.Android를 사용하여 Android Nougat용 앱 개발을 시작하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/02/2018
ms.openlocfilehash: 6274c75abf229268070d495ced662724f5c16627
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027085"
---
# <a name="nougat-features"></a>Nougat 기능

_Xamarin.Android를 사용하여 Android Nougat용 앱 개발을 시작하는 방법을 설명합니다._

이 문서에서는 Android Nougat에 새로 도입된 기능을 개략적으로 설명하고, Android Nougat 개발용 Xamarin.Android를 준비하는 방법을 설명하고, Xamarin.Android 앱에서 Android Nougat 기능을 사용하는 방법을 보여주는 샘플 애플리케이션의 링크를 제공합니다.

## <a name="overview"></a>개요

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html)는 Android 6.0 Marshmallow의 Google 후속 기능입니다. Xamarin.Android는 Xamarin Android 7.0 이상에서 **Android 7.x 바인딩**을 지원합니다. Android Nougat에는 아래에 설명된 Nougat 기능을 위한 여러 API가 추가되었습니다. Xamarin.Android 7.0을 사용하는 경우 Xamarin.Android 앱에서 이러한 API를 사용할 수 있습니다.

[![Android Nougat를 실행하는 Android 태블릿과 휴대폰의 히어로 이미지](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Android 7.x API에 대한 자세한 내용은 [개발자용 Android 7.1](https://developer.android.com/preview/api-overview.html)을 참조하세요.
알려진 Xamarin.Android 7.0 이슈 목록은 [릴리스 정보](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md)를 참조하세요.

Android Nougat는 Xamarin.Android 개발자에 게 도움이 되는 여러 가지 새 기능을 제공합니다. 이러한 기능에는 다음이 포함됩니다.

- **다중 창 지원** &ndash; 이 기능 덕분에 사용자가 화면에 두 개의 앱을 동시에 열 수 있습니다.

- **알림 기능 향상 &ndash;** Android Nougat의 새롭게 디자인된 알림 시스템에는 사용자가 알림 UI에서 바로 텍스트 메시지에 신속하게 대응할 수 있도록 *직접 회신* 기능이 포함되어 있습니다. 또한 앱에서 들어오는 메시지에 대한 알림을 만드는 경우 두 개 이상의 메시지가 수신되면 새로운 *번들 알림* 기능이 알림을 단일 그룹으로 묶을 수 있습니다.

- **데이터 보호기** &ndash; 이 기능은 앱의 셀룰러 데이터 사용량을 줄이는 데 도움이 되는 새로운 시스템 서비스이며, 사용자는 이 기능을 통해 앱에서 셀룰러 데이터를 사용하는 방식을 제어할 수 있습니다.

또한 Android Nougat는 새로운 네트워크 보안 구성 기능, 이동 중 절전 모드, 키 증명, 새로운 빠른 설정 API, 다중 로캘 지원, ICU4J API, 향상된 WebView, Java 8 언어 기능 액세스, 범위 지정 디렉터리 액세스, 사용자 지정 포인터 API, 플랫폼 VR 지원, 가상 파일, 백그라운드 처리 최적화 등 앱 개발자에게 유용한 여러 가지 향상된 기능을 제공합니다.

이 문서에서는 Android Nougat에서 앱 빌드를 시작하여 새 기능을 사용해 보고 새 Android Nougat 플랫폼을 대상으로 하는 마이그레이션 또는 기능 작업을 계획하는 방법을 설명합니다.

## <a name="requirements"></a>요구 사항

Xamarin 기반 앱에서 Android Nougat 기능을 사용하려면 다음이 필요합니다.

- **Visual Studio 또는 Mac용 Visual Studio** &ndash; Visual Studio를 사용하는 경우 Xamarin용 Visual Studio Tools 4.2.0.628 이상 버전이 필요합니다. Mac용 Visual Studio를 사용하는 경우 Mac용 Visual Studio 6.1.0 이상 버전이 필요합니다.

- **Xamarin.Android** &ndash; Visual Studio 또는 Mac용 Visual Studio에서 Xamarin.Android 7.0 이상을 설치하고 구성해야 합니다.

- **Android SDK** - Android SDK 관리자를 통해 Android SDK 7.0(API 24) 이상을 설치해야 합니다.

- **Java Developer Kit** &ndash; API 레벨 24 이상 앱을 개발하는 경우 Xamarin.Android 7.0에서 개발하려면 [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 이상이 필요합니다(JDK 8은 24 미만의 API 레벨도 지원). 사용자 지정 컨트롤 또는 Forms Previewer를 사용하는 경우 64비트 버전의 JDK 8이 필요합니다.

> [!IMPORTANT]
> Xamarin.Android는 JDK 9를 지원하지 않습니다.

앱이 Android Nougat에서 안정적으로 작동하려면 Xamarin C6SR4 이상을 사용하여 앱을 다시 빌드해야 합니다. Android Nougat는 [NDK 제공 네이티브 라이브러리](https://developer.android.com/about/versions/nougat/android-7.0-changes.html)에만 연결할 수 있으므로, **Mono.Data.Sqlite.dll** 같은 라이브러리를 사용하는 기본 앱을 적절하게 다시 빌드하지 않고 Android Nougat에서 실행하면 충돌이 발생할 수 있습니다.

## <a name="getting-started"></a>시작

Xamarin.Android에서 Android Nougat를 사용하려면 먼저 최신 도구 및 SDK 패키지를 다운로드하여 설치해야 합니다. 그러면 Android Nougat 프로젝트를 만들 수 있습니다.

1. Xamarin에서 최신 Xamarin.Android 업데이트를 설치합니다.

2. **Android 7.0(API 24)** 이상 패키지 및 도구를 설치합니다.

3. Android Nougat를 대상으로 하는 새 Xamarin.Android 프로젝트를 만듭니다.

4. Android Nougat용 에뮬레이터 또는 디바이스를 구성합니다.

각 단계는 다음 섹션에서 설명합니다.

### <a name="install-xamarin-updates"></a>Xamarin 업데이트 설치

Android Nougat에 대한 Xamarin 지원을 추가하려면 Visual Studio 또는 Mac용 Visual Studio의 업데이트 채널을 안정 채널로 변경하고 최신 업데이트를 적용합니다. 현재 알파 또는 베타 채널에서만 사용할 수 있는 기능도 필요한 경우 알파 또는 베타 채널로 전환할 수 있습니다(알파 및 베타 채널도 Android 7.x 지원). 업데이트(릴리스) 채널을 변경하는 방법에 대한 자세한 내용은 [업데이트 채널 변경](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel)을 참조하세요.

### <a name="install-the-android-sdk"></a>Android SDK 설치

Xamarin Android 7.0을 사용하여 프로젝트를 만들려면 먼저 Android SDK 관리자를 사용하여 **SDK 플랫폼 Android N(API 24)** 이상을 설치해야 합니다. 또 최신 **Android SDK Tools**를 설치해야 합니다.

1. Android SDK 관리자를 시작합니다(Mac용 Visual Studio에서는 **도구 > Android SDK 관리자 열기&hellip;** 사용, Visual Studio에서는 **도구 > Android > Android SDK 관리자**를 사용).

2. **Android 7.0(API 24)** 이상을 설치합니다.

    [![Android SDK 관리자에서 Android 7.0 패키지 선택](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3. 다음과 같이 최신 Android SDK 도구를 설치합니다.

    [![Android SDK 관리자에서 최신 Android SDK Tools 선택](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Android SDK Tools 수정 버전 25.2.2 이상, Android  SDK Platform 도구 24.0.3 이상, Android SDK Build 도구 24.0.2 이상을 설치해야 합니다.

4. **Java Development Kit 위치**가 JDK 1.8로 구성되어 있는지 확인합니다.

    [![도구 옵션에서 JDK 8 경로 구성](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Visual Studio에서 이 설정을 보려면 **도구 > 옵션 > Xamarin > Android 설정**을 클릭합니다. Mac용 Visual Studio에서 **기본 설정 > 프로젝트 > SDK 위치 > Android**를 클릭합니다.

### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android 프로젝트 시작

새 Xamarin.Android 프로젝트를 만듭니다. Xamarin을 사용한 Android 개발을 처음 접하는 경우 [Hello, Android](~/android/get-started/hello-android/index.md)를 참조하여 Xamarin.Android 프로젝트를 만드는 방법에 대해 알아보세요.

Android 프로젝트를 만들 때는 Android 7.0 이상을 대상으로 버전 설정을 구성해야 합니다. 예를 들어 Android 7.0용 프로젝트를 대상으로 하려면 프로젝트의 대상 Android API 레벨을 **Android 7.0(API 24 - Nougat)** 으로 구성해야 합니다. 대상 프레임워크 레벨을 API 24 이상으로 설정하는 것이 좋습니다. Android API 레벨을 구성하는 방법에 대한 자세한 내용은 [Android API 레벨 이해](~/android/app-fundamentals/android-api-levels.md)를 참조하세요.

> [!NOTE]
> 현재는 Android Nougat 디바이스 또는 에뮬레이터에 앱을 배포하려면 **최소 Android 버전**을 **Android 7.0(API 24 - Nougat)** 으로 설정해야 합니다.

### <a name="configure-an-emulator-or-device"></a>에뮬레이터 또는 디바이스 구성

에뮬레이터를 사용하는 경우 Android AVD 관리자를 시작하고 다음 설정을 사용하여 새 디바이스를 만듭니다.

- 디바이스: Nexus 5X, Nexus 6, Nexus 6P, Nexus Player, Nexus 9 또는 Pixel C
- 대상: Android 7.0 - API 레벨 24
- ABI: x86 또는 x86\_64

예를 들어 이 가상 디바이스는 Nexus 6를 에뮬레이트하도록 구성되었습니다.

[![Nexus 6 디바이스, Android 7.0 대상 및 Intel Atom x86 CPU/ABI를 사용하여 AVD 구성](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

Nexus 5X, 6 또는 9 같은 물리적 디바이스를 사용하는 경우 자동 OTA 업데이트를 통해 디바이스를 업데이트할 수도 있고, 시스템 이미지를 다운로드하여 디바이스를 직접 플래시할 수도 있습니다. 디바이스를 Android Nougat로 수동 업데이트하는 방법에 대한 자세한 내용은 [Nexus 디바이스용 OTA 이미지](https://developers.google.com/android/nexus/ota)를 참조하세요.

Nexus 5 디바이스는 Android Nougat에서 지원되지 않습니다.

## <a name="new-features"></a>새 기능

Android Nougat에는 다중 창 지원, 향상된 알림, 데이터 보호기 등의 다양한 새 기능이 도입되었습니다. 다음 섹션에서는 이러한 기능을 중점적으로 설명하고 앱에서 이러한 기능을 사용하는 데 도움이 되는 링크를 제공합니다.

### <a name="multi-window-mode"></a>다중 창 모드

다중 창 모드를 사용하면 사용자가 두 개의 앱을 동시에 열고 모든 멀티태스킹 지원을 사용할 수 있습니다. 이러한 앱은 분할 화면 모드에서 나란히(가로) 또는 위아래(세로)로 실행할 수 있습니다.
사용자는 앱 간에 구분선을 끌어 크기를 조정할 수 있으며, 앱 간에 콘텐츠를 잘라서 붙여넣을 수 있습니다. 두 개의 앱이 다중 창 모드로 표시되는 경우 선택한 작업은 계속 실행되고 선택되지 않은 작업은 일시 중지되지만 계속 표시됩니다. 다중 창 모드는 Android 작업 수명 주기를 수정하지 않습니다.

[![다중 창 모드에서 세로 및 가로 방향으로 실행되는 예제 앱](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Xamarin.Android 앱 작업에서 다중 창 모드를 지 원하는 방법을 구성할 수 있습니다. 예를 들어 다중 창 모드에서 앱의 최소 크기와 기본 높이 및 너비를 설정하는 특성을 구성할 수 있습니다. 새 `Activity.IsInMultiWindowMode` 속성을 사용하여 작업이 다중 창 모드인지 확인할 수 있습니다. 예를 들어:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[MultiWindowPlayground](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground) 샘플 앱에는 앱에서 다중 창 사용자 인터페이스를 활용하는 방법을 보여주는 코드가 포함되어 있습니다.

다중 창 모드에 대한 자세한 내용은 [다중 창 지원](https://developer.android.com/guide/topics/ui/multi-window.html)을 참조하세요.

### <a name="enhanced-notifications"></a>향상된 알림

Android Nougat에는 재설계된 알림 시스템이 도입되었습니다. 사용자가 알림 UI에서 바로 수신 텍스트 메시지에 대한 알림에 신속하게 응답할 수 있는 새로운 직접 회신 기능입니다. Android 7.0부터 메시지가 여러 개 수신되는 경우 알림 메시지를 단일 그룹으로 묶을 수 있습니다. 또한 개발자는 알림 보기를 사용자 지정하고, 시스템 장식을 알림에 활용하고, 알림을 생성할 때 새 알림 템플릿을 활용할 수 있습니다.

#### <a name="direct-reply"></a>직접 회신

Android Nougat에서 사용자는 들어오는 메시지에 대한 알림을 받으면 회신을 보내기 위해 메시지 앱을 여는 대신 알림 내에서 메시지에 회신할 수 있습니다.
다음과 같이 이 인라인 회신 기능을 사용하면 사용자가 알림 인터페이스 내에서 바로 SMS 또는 문자 메시지에 신속하게 응답할 수 있습니다.

[![인라인 직접 회신 필드가 있는 알림의 스크린샷](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

앱에서 이 기능을 지원하려면 사용자가 알림 UI에서 직접 텍스트를 통해 회신할 수 있도록 [RemoteInput](xref:Android.App.RemoteInput) 개체를 통해 앱에 *인라인 회신 동작*을 추가해야 합니다.
예를 들어 다음 코드는 텍스트 입력을 수신하기 위한 `RemoteInput` 코드를 작성하고, 회신 동작에 대한 보류 중인 의도를 작성하고, 원격 입력을 사용하는 동작을 만듭니다.

```csharp
// Build a RemoteInput for receiving text input:
var remoteInput = new Android.Support.V4.App.RemoteInput.Builder (EXTRA_REMOTE_REPLY)
    .SetLabel (GetString (Resource.String.reply))
    .Build ();

// Build a Pending Intent for the reply action to trigger:
PendingIntent replyIntent = PendingIntent.GetBroadcast (ApplicationContext,
                                conversation.ConversationId,
                                GetMessageReplyIntent (conversation.ConversationId),
                                PendingIntentFlags.UpdateCurrent);

// Build an Android 7.0 compatible Remote Input enabled action:
NotificationCompat.Action actionReplyByRemoteInput = new NotificationCompat.Action.Builder (
    Resource.Drawable.notification_icon,
    GetString (Resource.String.reply),
    replyIntent).AddRemoteInput (remoteInput).Build ();
```

이 동작은 알림에 추가됩니다.

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

[Messaging Service](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice) 샘플 앱에는 `RemoteInput` 개체를 사용하여 알림을 확장하는 방법을 보여주는 C# 코드가 포함되어 있습니다. Android 7.0 이상용 앱에 인라인 회신 동작을 추가하는 방법에 대한 자세한 내용은 Android [알림에 회신](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) 토픽을 참조하세요.

#### <a name="bundled-notifications"></a>번들 알림

Android Nougat는 알림 메시지를 그룹화하고(예: 메시지 토픽별로) 개별 메시지가 아닌 그룹을 표시할 수 있습니다.
이 *번들 알림* 기능을 사용하면 사용자가 한 번의 작업으로 알림 그룹을 해제하거나 보관할 수 있습니다. 사용자는 다음과 같이 아래로 밀어 알림 번들을 확장하고 각 알림을 자세히 볼 수 있습니다.

[![번들 알림 예제 스크린샷](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

번들 알림을 지원하려면 앱에서 [Builder.SetGroup](xref:Android.App.Notification.Builder.SetGroup*) 메서드를 사용하여 유사한 알림을 묶으면 됩니다. Android N의 번들 알림 그룹에 대한 자세한 내용은 Android [알림 묶음](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) 토픽을 참조하세요.

#### <a name="custom-views"></a>사용자 지정 보기

Android Nougat는 시스템 알림 헤더, 동작 및 확장 가능한 레이아웃을 사용하여 사용자 지정 알림 보기를 만들 수 있습니다. Android Nougat의 사용자 지정 알림 보기에 대한 자세한 내용은 Android [향상된 알림](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) 토픽을 참조하세요.

### <a name="data-saver"></a>데이터 보호기

Android Nougat부터 사용자는 백그라운드 데이터 사용을 차단하는 새로운 *데이터 보호기* 설정을 사용할 수 있습니다. 이 설정을 사용하면 가능하면 포그라운드에서도 데이터를 절약하라고 앱에 신호를 보냅니다. Android Nougat에서는 사용자가 데이터 보호기를 사용하도록 설정했는지 여부를 앱에서 확인하여 데이터 보호기를 사용하는 경우 앱에서 데이터 사용을 제한할 수 있도록 [ConnectivityManager](xref:Android.Net.ConnectivityManager)가 확장되었습니다.

Android Nougat의 새로운 데이터 보호기 기능에 대한 자세한 내용은 Android [네트워크 데이터 사용량 최적화](https://developer.android.com/training/basics/network-ops/data-saver.html) 토픽을 참조하세요.

### <a name="app-shortcuts"></a>앱 바로 가기

Android 7.1에는 사용자가 앱에서 일반 또는 권장 작업을 신속하게 시작할 수 있도록 *앱 바로 가기* 기능이 도입되었습니다.
바로 가기 메뉴를 활성화하려면 사용자가 앱 아이콘을 1초 이상 길게 누르면 됩니다. 그러면 빠른 진동과 함께 메뉴가 나타납니다.
누르고 있던 앱 아이콘을 놓으면 메뉴가 계속 표시됩니다.

[![메시지 앱의 앱 바로 가기 메뉴 예제 화면](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

이 기능은 API 레벨 25 이상에서만 사용할 수 있습니다.
Android 7.1의 새로운 앱 바로 가기 기능에 대한 자세한 내용은 Android [앱 바로 가기](https://developer.android.com/guide/topics/ui/shortcuts.html) 토픽을 참조하세요.

### <a name="sample-code"></a>샘플 코드

Android Nougat 기능을 활용하는 방법을 보여주는 여러 Xamarin.Android 샘플이 있습니다.

- [MultiWindowPlayground](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground)는 Android Nougat에서 제공하는 다중 창 API 사용 방법을 보여줍니다. 샘플 앱을 다중 창 모드로 전환하여 앱의 수명 주기와 동작에 미치는 영향을 확인할 수 있습니다.

- [Messaging Service](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice)는 `NotificationCompatManager`를 사용하여 알림을 보내는 간단한 서비스입니다. 또한 이 서비스는 Android Nougat 디바이스에서 앱을 열지 않고도 알림 내에서 바로 텍스트를 통해 회신할 수 있도록 `RemoteInput` 개체를 통해 알림을 확장합니다.

- [활성 알림](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-activenotifications)은 `NotificationManager` API를 사용하여 애플리케이션에 현재 표시되는 알림 수를 알려줍니다.

- [범위가 지정된 디렉터리 액세스](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-scopeddirectoryaccess)는 범위가 지정된 디렉터리 액세스 API를 사용하여 특정 디렉터리에 쉽게 액세스하는 방법을 보여줍니다. 매니페스트에서 `READ_EXTERNAL_STORAGE` 또는 `WRITE_EXTERNAL_STORAGE` 권한을 정의하는 대신 이 방법이 사용됩니다.

- [직접 부팅](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-directboot)은 디바이스 암호화 스토리지에 데이터를 저장하는 방법을 보여줍니다. 이 스토리지는 디바이스가 부팅되는 동안 사용자 자격 증명(PIN/패턴/암호)이 입력되기 전과 후에 항상 사용할 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 Android Nougat를 소개하고, Nougat Nougat에서 Xamarin.Android 개발을 위한 최신 도구 및 패키지를 설치하고 구성하는 방법을 설명했습니다. 또한 Android Nougat에서 제공하는 주요 기능을 간략하게 설명하고, Android Nougat용 앱을 만드는 데 도움이 되는 소스 코드 예제의 링크를 제공했습니다.

## <a name="related-links"></a>관련 링크

- [개발자용 Android 7.1](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Xamarin Android 7.0 릴리스 정보](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md)
