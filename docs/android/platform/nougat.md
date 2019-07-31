---
title: Nougat 기능
description: Xamarin.ios를 사용 하 여 Android Nougat 앱을 개발 하는 방법을 알아봅니다.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/02/2018
ms.openlocfilehash: a28368e0fa4574fbb92a43dbd650a127008f5d06
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643459"
---
# <a name="nougat-features"></a>Nougat 기능

_Xamarin.ios를 사용 하 여 Android Nougat 앱을 개발 하는 방법을 알아봅니다._

이 문서에서는 android Nougat에 도입 된 기능에 대 한 개요를 제공 하 고 Android Nougat 개발용 Xamarin을 준비 하는 방법을 설명 하며,에서 Android Nougat 기능을 사용 하는 방법을 설명 하는 샘플 응용 프로그램에 대 한 링크를 제공 합니다. Xamarin Android 앱.


## <a name="overview"></a>개요

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) 는 Android 6.0 Marshmallow에 대 한 Google의 후속 기능입니다. Xamarin android는 Xamarin Android 7.0 이상에서 **android 4.X 바인딩을** 지원 합니다. Android Nougat는 아래에 설명 된 Nougat 기능에 대해 많은 새 Api를 추가 합니다. 이러한 Api는 Xamarin android 7.0를 사용할 때 Xamarin Android 앱에서 사용할 수 있습니다.

[![Android Nougat을 실행 하는 Android 태블릿 및 휴대폰의 주인공 이미지](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Android 4.x Api에 대 한 자세한 내용은 [개발자를 위한 android 7.1](https://developer.android.com/preview/api-overview.html)을 참조 하세요.
알려진 Xamarin Android 7.0 문제 목록은 [릴리스 정보](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md)를 참조 하세요.

Android Nougat는 Xamarin Android 개발자에 게 유용한 여러 가지 새로운 기능을 제공 합니다. 이러한 기능은 다음과 같습니다.

-   **다중 창 지원** &ndash; 이 기능을 사용 하면 사용자가 화면에서 한 번에 두 개의 앱을 열 수 있습니다.

-   **알림 기능 향상** Android Nougat의 다시 디자인 된 알림 시스템에는 사용자가 알림 UI에서 직접 텍스트 메시지에 신속 하 게 대응할 수 있도록 하는 *직접 회신* 기능이 포함 되어 있습니다. &ndash; 또한 앱에서 수신 된 메시지에 대 한 알림을 만드는 경우 두 개 이상의 메시지를 받을 때 새 *번들 알림* 기능에서 알림을 단일 그룹으로 묶을 수 있습니다.

-   **데이터 보호기** &ndash; 이 기능은 앱에서 셀룰러 데이터 사용을 줄이는 데 도움이 되는 새로운 시스템 서비스입니다. 사용자가 앱에서 셀룰러 데이터를 사용 하는 방식을 제어할 수 있습니다.

또한 Android Nougat는 새 네트워크 보안 구성 기능과 같은 앱 개발자에 게 다양 한 향상 된 기능을 제공 하 고, 이동, 키 증명, 새 빠른 설정 Api, 다중 로캘 지원, ICU4J Api, 웹 보기 향상 등의 기능을 제공 합니다. Java 8 언어 기능, 범위 지정 디렉터리 액세스, 사용자 지정 포인터 API, 플랫폼 VR 지원, 가상 파일 및 백그라운드 처리 최적화에 액세스할 수 있습니다.

이 문서에서는 Android Nougat를 사용 하 여 앱 빌드를 시작 하 고 새로운 기능을 사용해 보고 새 Android Nougat 플랫폼을 대상으로 하는 마이그레이션 또는 기능 작업을 계획 하는 방법을 설명 합니다.


## <a name="requirements"></a>요구 사항

Xamarin 기반 앱에서 새로운 Android Nougat 기능을 사용 하려면 다음이 필요 합니다.

-   **Visual Studio 또는 Mac용 Visual Studio** &ndash; Visual Studio를 사용 하는 경우 Xamarin에 대 한 Visual Studio Tools 버전 4.2.0.628 이상이 필요 합니다. Mac용 Visual Studio 사용 하는 경우 Mac용 Visual Studio 버전 6.1.0 이상이 필요 합니다.

-   Visual Studio 또는 Mac용 Visual Studio를 사용 하 여 **xamarin android** &ndash; xamarin android 7.0 이상 버전을 설치 하 고 구성 해야 합니다.

-   Android SDK Manager를 통해 **Android SDK** Android SDK 7.0 (API 24) 이상을 설치 해야 합니다.

-   **Java 개발자 키트** Api 레벨 24 이상에서 개발 하는 경우 Xamarin Android 7.0 개발에는 [jdk 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 이상이 필요 합니다 (jdk 8은 24 이전 api 수준도 지원). &ndash; 사용자 지정 컨트롤 또는 폼 미리 보기를 사용 하는 경우 64 비트 버전의 JDK 8이 필요 합니다.

> [!IMPORTANT]
> Xamarin.Android는 JDK 9를 지원하지 않습니다.

Android Nougat로 안정적으로 작업 하려면 앱을 Xamarin C6SR4 이상으로 다시 빌드해야 합니다. Android Nougat는 NDK에서 제공 하는 [네이티브 라이브러리](https://developer.android.com/about/versions/nougat/android-7.0-changes.html)에만 연결할 수 있으므로 **Mono** 와 같은 라이브러리를 사용 하는 기존 앱이 제대로 다시 빌드되지 않으면 android Nougat에서 실행 될 때 충돌할 수 있습니다.



## <a name="getting-started"></a>시작하기

Android Nougat에서 android 사용을 시작 하려면 Android Nougat 프로젝트를 만들기 전에 최신 도구 및 SDK 패키지를 다운로드 하 여 설치 해야 합니다.

1.  Xamarin에서 최신 Xamarin Android 업데이트를 설치 합니다.

2.  **Android 7.0 (API 24)** 패키지 및 도구 이상을 설치 합니다.

3.  Android Nougat를 대상으로 하는 새 Xamarin Android 프로젝트를 만듭니다.

4.  Android Nougat 에뮬레이터 또는 장치를 구성 합니다.

이러한 각 단계는 다음 섹션에 설명 되어 있습니다.


### <a name="install-xamarin-updates"></a>Xamarin 업데이트 설치

Android Nougat에 대 한 Xamarin 지원을 추가 하려면 Visual Studio 또는 Mac용 Visual Studio의 업데이트 채널을 안정적인 채널로 변경 하 고 최신 업데이트를 적용 합니다. 알파 또는 베타 채널 에서만 현재 사용할 수 있는 기능도 필요한 경우 알파 또는 베타 채널 (알파 및 베타 채널은 Android 4.x에 대 한 지원도 제공)으로 전환할 수 있습니다. 업데이트 (릴리스) 채널을 변경 하는 방법에 대 한 자세한 내용은 [업데이트 채널 변경](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel)을 참조 하십시오.



### <a name="install-the-android-sdk"></a>Android SDK 설치

Xamarin Android 7.0를 사용 하 여 프로젝트를 만들려면 먼저 Android SDK Manager를 사용 하 여 **SDK Platform Android N (API 24)** 이상을 설치 해야 합니다. 또한 최신 **Android SDK Tools**를 설치 해야 합니다.

1.  Android SDK manager를 시작 합니다 (Mac용 Visual Studio에서 도구를 사용 하 여 **Android SDK&hellip;관리자 > 엽니다**. Visual Studio에서 **도구 > Android > Android SDK Manager**)를 사용 합니다.

2.  **Android 7.0 (API 24)** 이상을 설치 합니다.

    [![Android SDK Manager에서 Android 7.0 패키지 선택](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  최신 Android SDK 도구를 설치 합니다.

    [![Android SDK Manager에서 최신 Android SDK 도구 선택](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Android SDK Tools 수정 버전 25.2.2 이상, Android SDK Platform tools v24.0.3 이상 Android SDK 및 빌드 도구 24.0.2 이상을 설치 해야 합니다.

4.  **Java Development Kit 위치가** JDK 1.8에 대해 구성 되어 있는지 확인 합니다.

    [![도구 옵션 아래에서 JDK 8 경로 구성](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Visual Studio에서이 설정을 보려면 **도구 > 옵션 > Xamarin > Android 설정**을 클릭 합니다. Mac용 Visual Studio에서 **기본 설정 > 프로젝트 > SDK 위치 > Android**를 클릭 합니다.



### <a name="start-a-xamarinandroid-project"></a>Xamarin Android 프로젝트 시작

새 Xamarin Android 프로젝트를 만듭니다. Xamarin을 사용 하 여 Android 개발을 처음 접하는 경우에는 [Hello, android](~/android/get-started/hello-android/index.md) 를 참조 하 여 xamarin.ios 프로젝트를 만드는 방법에 대해 알아보세요.

Android 프로젝트를 만들 때 Android 7.0 이상 버전을 대상으로 하도록 버전 설정을 구성 해야 합니다. 예를 들어 Android 7.0에 대 한 프로젝트를 대상으로 하려면 프로젝트의 대상 Android API 수준을 **android 7.0 (API 24-Nougat)** 로 구성 해야 합니다. 대상 프레임 워크 수준을 API 24 이상으로 설정 하는 것이 좋습니다. Android API 수준 수준을 구성 하는 방법에 대 한 자세한 내용은 [ANDROID Api 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조 하세요.


> [!NOTE]
> 현재 android Nougat 장치 또는 에뮬레이터에 앱을 배포 하려면 **최소 android 버전** 을 **ANDROID 7.0 (API 24-Nougat)** 로 설정 해야 합니다.



### <a name="configure-an-emulator-or-device"></a>에뮬레이터 또는 장치 구성

에뮬레이터를 사용 하는 경우 Android AVD 관리자를 시작 하 고 다음 설정을 사용 하 여 새 장치를 만듭니다.

-   장치: Nexus 5, Nexus 6, Nexus 6P, Nexus 플레이어, Nexus 9 또는 픽셀 C입니다.
-   대상: Android 7.0-API 수준 24
-   ABI: x86 또는 x86\_64

예를 들어이 가상 장치는 Nexus 6을 에뮬레이트하는 것으로 구성 됩니다.

[![Nexus 6 장치, Android 7.0 대상 및 Intel Atom x86 CPU/ABI를 사용 하 여 AVD 구성](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

Nexus 5, 6 또는 9와 같은 물리적 장치를 사용 하는 경우 OTA (air) 업데이트에서 자동으로 장치를 업데이트 하거나 시스템 이미지를 다운로드 하 여 장치를 직접 플래시 할 수 있습니다. Android Nougat 장치를 수동으로 업데이트 하는 방법에 대 한 자세한 내용은 [Nexus 장치용 OTA 이미지](https://developers.google.com/android/nexus/ota)를 참조 하세요.

Nexus 5 장치는 Android Nougat에서 지원 되지 않습니다.



## <a name="new-features"></a>새 기능

Android Nougat에는 다중 창 지원, 알림 기능 향상 및 데이터 보호기와 같은 다양 한 새로운 기능 및 기능이 도입 되었습니다. 다음 섹션에서는 이러한 기능을 강조 하 고 앱에서 사용을 시작 하는 데 도움이 되는 링크를 제공 합니다.



### <a name="multi-window-mode"></a>다중 창 모드

다중 창 모드를 사용 하면 사용자가 모든 멀티태스킹 지원을 통해 한 번에 두 개의 앱을 열 수 있습니다. 이러한 앱은 분할 화면 모드에서 나란히 (가로) 또는 위쪽 (세로)으로 실행할 수 있습니다.
사용자는 앱 간에 구분선을 끌어 크기를 조정할 수 있으며 앱 간 콘텐츠를 잘라내어 붙여 넣을 수 있습니다. 두 개의 앱이 다중 창 모드로 표시 되는 경우 선택 되지 않은 작업이 일시 중지 되었지만 계속 표시 되는 동안 선택한 작업은 계속 실행 됩니다. 다중 창 모드는 Android 작업 수명 주기를 수정 하지 않습니다.

[![세로 및 가로에서 다중 창 모드로 실행 되는 예제 앱](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Xamarin Android 앱의 작업에서 다중 창 모드를 지 원하는 방법을 구성할 수 있습니다. 예를 들어, 앱의 최소 크기와 기본 높이 및 너비를 여러 창 모드로 설정 하는 특성을 구성할 수 있습니다. 새 `Activity.IsInMultiWindowMode` 속성을 사용 하 여 활동이 다중 창 모드 인지 여부를 확인할 수 있습니다. 예를 들어:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[Multiwindowwindowsample](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground) 앱에는 C# 앱과 함께 여러 창 사용자 인터페이스를 활용 하는 방법을 보여 주는 코드가 포함 되어 있습니다.

다중 창 모드에 대 한 자세한 내용은 [다중 창 지원](https://developer.android.com/guide/topics/ui/multi-window.html)을 참조 하세요.



### <a name="enhanced-notifications"></a>향상 된 알림

Android Nougat는 재설계 된 알림 시스템을 도입 했습니다. 사용자가 알림 UI에서 직접 들어오는 텍스트 메시지에 대 한 알림에 빠르게 회신할 수 있도록 하는 새로운 직접 회신 기능을 갖추고 있습니다. Android 7.0부터 둘 이상의 메시지를 수신 하는 경우 알림 메시지를 단일 그룹으로 묶을 수 있습니다. 또한 개발자는 알림 보기를 사용자 지정 하 고, 알림의 시스템 장식을 활용 하 고, 알림을 생성할 때 새 알림 템플릿을 활용할 수 있습니다.


#### <a name="direct-reply"></a>직접 회신

사용자가 들어오는 메시지에 대 한 알림을 받으면 Android Nougat는 회신을 보내기 위해 메시징 앱을 여는 대신 알림 내에서 메시지에 회신할 수 있습니다.
이 인라인 회신 기능을 사용 하면 사용자가 알림 인터페이스 내에서 직접 SMS 또는 문자 메시지에 신속 하 게 응답할 수 있습니다.

[![인라인 직접 회신 필드가 있는 알림의 스크린샷](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

앱에서이 기능을 지원 하려면 사용자가 알림 UI에서 직접 텍스트를 통해 회신할 수 있도록 [Remoteinput](xref:Android.App.RemoteInput) 개체를 통해 앱에 *인라인 회신 동작* 을 추가 해야 합니다.
예를 들어 다음 코드는 텍스트 입력 `RemoteInput` 을 수신 하기 위한를 빌드하고, 회신 동작에 대 한 보류 중인 의도를 빌드하고, 원격 입력 사용 작업을 만듭니다.

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

이 작업은 알림에 추가 됩니다.

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

[메시징 서비스](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice) 샘플 앱은 C# `RemoteInput` 개체를 사용 하 여 알림을 확장 하는 방법을 보여 주는 코드를 포함 합니다. Android 7.0 이상용 앱에 인라인 회신 작업을 추가 하는 방법에 대 한 자세한 내용은 Android [알림에 회신](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) 항목을 참조 하세요.


#### <a name="bundled-notifications"></a>번들로 제공 되는 알림

Android Nougat는 메시지 항목을 통해 알림 메시지를 함께 그룹화 하 고 각각의 개별 메시지가 아니라 그룹을 표시할 수 있습니다.
이러한 *번들* 로 구성 된 알림 기능을 사용 하면 사용자가 한 번의 작업으로 알림 그룹을 해제 하거나 보관할 수 있습니다. 사용자는 다음으로 이동 하 여 알림 번들을 확장 하 여 각 알림을 자세히 볼 수 있습니다.

[![함께 제공 된 알림의 스크린샷 예](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

번들로 구성 된 알림을 지원 하기 위해 앱은 [빌더. SetGroup](xref:Android.App.Notification.Builder.SetGroup*) 메서드를 사용 하 여 유사한 알림을 묶을 수 있습니다. Android N의 번들 알림 그룹에 대 한 자세한 내용은 Android [번들 알림](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) 항목을 참조 하세요.


#### <a name="custom-views"></a>사용자 지정 보기

Android Nougat를 사용 하면 시스템 알림 헤더, 작업 및 확장 가능한 레이아웃을 사용 하 여 사용자 지정 알림 보기를 만들 수 있습니다. Android Nougat의 사용자 지정 알림 보기에 대 한 자세한 내용은 Android [알림 고급 기능](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) 항목을 참조 하세요.



### <a name="data-saver"></a>데이터 보호기

Android Nougat 부터는 사용자가 백그라운드 데이터 사용을 차단 하는 새 *데이터 보호기* 설정을 사용 하도록 설정할 수 있습니다. 이 설정을 사용 하면 가능한 경우에도 응용 프로그램이 포그라운드에서 더 작은 데이터를 사용 하도록 신호를 보냅니다. [ConnectivityManager](xref:Android.Net.ConnectivityManager) 는 Android Nougat에서 확장 되었으므로 앱에서 데이터 보호기를 사용 하도록 설정 했는지 여부를 확인 하 여 데이터 보호기를 사용 하는 경우 앱에서 데이터 사용을 제한 하는 작업을 수행할 수 있습니다.

Android Nougat의 새로운 데이터 보호기 기능에 대 한 자세한 내용은 Android [네트워크 데이터 사용 현황 최적화](https://developer.android.com/training/basics/network-ops/data-saver.html) 항목을 참조 하세요.



### <a name="app-shortcuts"></a>앱 바로 가기

Android 7.1에는 사용자가 앱을 사용 하 여 일반 또는 권장 작업을 신속 하 게 시작할 수 있도록 하는 *앱 바로 가기* 기능이 도입 되었습니다.
바로 가기 메뉴를 활성화 하기 위해 두 번째 또는 그 이상의 &ndash; 메뉴에 대 한 앱 아이콘을 길게 누르면 빠른 진동이 표시 됩니다.
키를 놓으면 메뉴가 계속 유지 됩니다.

[![메시징 앱에 대 한 앱 바로 가기 메뉴의 예제 화면](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

이 기능은 API 수준 25 이상 에서만 사용할 수 있습니다.
Android 7.1의 새로운 앱 바로 가기 기능에 대 한 자세한 내용은 Android [앱 바로 가기](https://developer.android.com/guide/topics/ui/shortcuts.html) 항목을 참조 하세요.


### <a name="sample-code"></a>예제 코드

Android Nougat 기능을 활용 하는 방법을 보여 주는 몇 가지 Xamarin Android 샘플을 사용할 수 있습니다.

-   [Multiwindowplayground](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-multiwindowplayground) 는 Android에서 사용할 수 있는 다중 창 API를 사용 하는 방법을 보여 줍니다. 샘플 앱을 다중 windows 모드로 전환 하 여 앱의 수명 주기와 동작에 미치는 영향을 확인할 수 있습니다.

-   [메시징 서비스](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-messagingservice) 는를 `NotificationCompatManager`사용 하 여 알림을 보내는 간단한 서비스입니다. 또한 `RemoteInput` 개체를 사용 하 여 알림을 확장 하 여 Android Nougat 장치에서 앱을 열지 않고도 알림에서 직접 텍스트를 통해 회신할 수 있습니다.

-   [활성 알림은](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-activenotifications) `NotificationManager` API를 사용 하 여 응용 프로그램에 현재 표시 되는 알림 수를 알려 주는 방법을 보여 줍니다.

-   [범위 디렉터리 액세스](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-scopeddirectoryaccess) 범위가 지정 된 디렉터리 액세스 API를 사용 하 여 특정 디렉터리에 쉽게 액세스 하는 방법을 보여 줍니다. 이는 매니페스트에서 또는 `READ_EXTERNAL_STORAGE` `WRITE_EXTERNAL_STORAGE` 사용 권한을 정의 하는 대신 사용 됩니다.

-   [직접 부팅](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-n-directboot) 장치 암호화 저장소에 데이터를 저장 하는 방법을 보여 줍니다 .이 저장소는 장치를 부팅 하는 동안 항상 사용할 수 있으며 사용자 자격 증명 (PIN/패턴/암호)을 입력 합니다.


## <a name="summary"></a>요약

이 문서에서는 android Nougat을 소개 하 고 Android Nougat에 대 한 최신 도구 및 패키지를 설치 하 고 구성 하는 방법을 설명 했습니다. 또한 android Nougat에서 사용할 수 있는 주요 기능에 대 한 개요를 제공 했으며, Android Nougat 용 앱을 만들기 시작 하는 데 도움이 되는 소스 코드 예제에 대 한 링크도 제공 합니다.


## <a name="related-links"></a>관련 링크

- [개발자를 위한 Android 7.1](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Xamarin Android 7.0 릴리스 정보](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_7/xamarin.android_7.0/index.md)
