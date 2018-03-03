---
title: "Nougat 기능"
description: "Xamarin.Android를 사용 하 여 Nougat Android 용 앱 개발을 시작 하는 방법입니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 60879273b5a736d4834bd6ba1685d5685fd05e67
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="nougat-features"></a>Nougat 기능

_Xamarin.Android를 사용 하 여 Nougat Android 용 앱 개발을 시작 하는 방법입니다._

이 문서에서는 Android Nougat에 도입 된 기능의 개요 Xamarin.Android Android Nougat 개발을 위해 준비 하는 방법에 설명 하 고 Android Nougat 기능을 사용 하는 방법을 보여 주는 예제 응용 프로그램에 대 한 링크를 제공 합니다. Xamarin.Android 앱입니다.

<a name="overview" />

## <a name="overview"></a>개요

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) Android 6.0 Marshmallow에 Google의 후속 됩니다. Xamarin.Android에 대 한 지원을 제공 **Android 7.x 바인딩** Xamarin Android 7.0 및 이후 버전입니다. Android Nougat; 아래에 설명 된 Nougat 기능에 대 한 많은 새 Api가 추가 이러한 Api는 Xamarin.Android 7.0을 사용 하는 경우 Xamarin.Android 앱에 사용할 수 있습니다.

[![Android 태블릿 및 휴대폰 Android Nougat 실행의 이미지 Hero](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png)

Android 7.x Api에 대 한 자세한 내용은 참조 [개발자를 위한 Android 7.1](http://developer.android.com/preview/api-overview.html)합니다.
Xamarin.Android 7.0의 알려진된 문제 목록은 참조 하십시오는 [릴리스 정보](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)합니다.

Android Nougat Xamarin.Android 개발자에 게 관심 있는 여러 가지 새로운 기능을 제공합니다. 이러한 기능에는 다음이 포함됩니다.

-   **다중 창 지원** &ndash; 이 향상 된이 기능을 사용 하면 사용자가을 한 번에 두 개 앱이 화면에서 열에 대 한 수입니다.

-   **알림 향상 된 기능** &ndash; Android Nougat에서 다시 디자인 된 알림 시스템에는 *직접 회신* 알림을에서 직접 문자 메시지에 빠르게 대응할 수 있도록 하는 기능 UI입니다. 또한 응용 프로그램에 대 한 알림을 만드는 경우이 수신 메시지, 새로운 *알림 bundled* 기능 번들로 알림을 함께 단일 그룹으로 둘 이상의 메시지를 받을 때.

-   **데이터 보호기** &ndash; 이 기능은 앱 셀룰러 데이터 사용을 줄일 수 있는 새 시스템 서비스; 앱 셀룰러 데이터를 사용 하는 방법에 대 한 제어 사용자에 게 제공 합니다.

Android Nougat 다른 많은 향상 된 기능을 제공 하는 또한 이동 중에 Doze 관심 있는 응용 프로그램 개발자는 새 네트워크 보안 구성 기능 등을, 키 증명, 새로운 빠른 설정, Api 다중 로캘 지원을, ICU4J Api WebView 향상 된 기능 Java 8 언어 기능에 대 한 액세스, 범위가 지정 된 디렉터리 액세스, 사용자 정의 포인터 API, VR 지원 되는 플랫폼, 가상 파일 및 백그라운드 최적화 처리

이 문서는 새로운 기능을 사용해 새 Nougat Android 플랫폼을 대상으로 마이그레이션 또는 기능 작업을 계획 하는 Android Nougat를 사용 하 여 앱을 작성 하는 방법에 설명 합니다.


<a name="requirements" />

## <a name="requirements"></a>요구 사항

다음은 Xamarin 기반 앱에서 새 Android Nougat 기능을 사용 하려면 필요 합니다.

-   **Visual Studio 또는 Mac 용 Visual Studio** &ndash; 4.2.0.628 버전 Visual Studio를 사용 하는 경우 또는 Xamarin for Visual Studio의 이후이 필요 합니다. 사용 중인 경우 Visual Studio 버전 6.1.0, Mac 용 또는 나중에 Visual Studio의 Mac가 필요 합니다.

-   **Xamarin.Android** &ndash; 7.0 이상 Xamarin.Android를 설치 하 고 Mac.에 대 한 Visual Studio 또는 Visual Studio를 사용 하 여 구성 해야

-   **Android SDK** -Android SDK (API 24) 7.0 나중에 설치 되어 있어야 Android SDK Manager를 통해 합니다.

-   **Java 개발자 키트** &ndash; Xamarin Android 7.0 개발 시 필요한 [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 또는 24 API 수준에 대 한 개발 하는 경우 이후 이상 (JDK 8 API에서는 수준을 지원 24 보다 이전 버전). JDK 8의 64 비트 버전은 사용자 지정 컨트롤 또는 양식 미리 보기를 사용 하는 경우에 필요 합니다.

> [!IMPORTANT]
> **참고:** Xamarin.Android JDK 9를 지원 하지 않습니다.

응용 프로그램 다시 작성 해야 Xamarin C6SR4 이상 안정적으로 Android Nougat 작업할 note 합니다. Android Nougat에만 연결할 수 있으므로 [NDK 제공 하는 네이티브 라이브러리](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), 같은 라이브러리를 사용 하 여 기존 앱 **Mono.Data.Sqlite.dll** 그렇지 않은 경우 제대로 Nougat Android에서 실행 중인 경우 충돌이 발생할 수 있음 다시 작성합니다.


<a name="gettingstarted" />

## <a name="getting-started"></a>시작

Android Nougat Xamarin.Android를 사용 하 여 시작 하려면 다운로드 하 고 Android Nougat 프로젝트를 만들려면 먼저 최신 도구와 SDK 패키지를 설치 해야 합니다.

1.  Xamarin에서 최신 Xamarin.Android 업데이트를 설치 합니다.

2.  설치는 **Android 7.0 (API 24)** 패키지 및 도구 이상.

3.  Android Nougat를 대상으로 하는 새 Xamarin.Android 프로젝트를 만듭니다.

4.  Android Nougat에 대해 에뮬레이터 또는 장치를 구성 합니다.

이러한 각 단계는 다음 섹션에서 설명:

<a name="updates" />

### <a name="install-xamarin-updates"></a>Xamarin 업데이트를 설치 합니다.

Xamarin Android Nougat 지원을 추가 하 고 안정적인 채널에 Mac 용 Visual Studio 또는 Visual Studio에서 업데이트 채널을 변경 하며 최신 업데이트를 적용 합니다. 현재 베타 또는 알파 채널에만 사용할 수 있는 기능을 필요할 경우 (베타 및 알파 채널 위한 지원도 제공 Android 7.x) 베타 또는 알파 채널을 전환할 수 있습니다. 업데이트 (버전)로 변경 하는 방법에 대 한 정보를 참조 하십시오. [업데이트 채널 변경](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/)합니다.


<a name="sdk" />

### <a name="install-the-android-sdk"></a>Android SDK 설치

Xamarin Android 7.0을 사용 하 여 프로젝트를 만들려면 먼저 사용 해야 Android SDK Manager를 설치 하려면 **SDK 플랫폼 Android N (API 24)** 이상. 최신도 설치 해야 **Android SDK 도구**:

1.  Android SDK Manager 시작 (Mac 용 Visual Studio에서 사용 하 여 **도구 > 열기 Android SDK Manager&hellip;**; Visual Studio를 사용 하 여 **도구 > Android > Android SDK Manager**).

2.  설치 **Android 7.0 (API 24)** 이상:

    [![Android SDK Manager에서 Android 7.0 패키지 선택](nougat-images/preview-packages.png)](nougat-images/preview-packages.png)

3.  최신 Android SDK 도구를 설치 합니다.

    [![Android SDK Manager의 최신 Android SDK 도구 선택](nougat-images/preview-tools.png)](nougat-images/preview-tools.png)

    Android SDK 도구 개정 25.2.2 또는 이상, Android SDK 플랫폼 도구 24.0.3 또는 나중에 및 Android SDK 빌드 도구 24.0.2 설치 해야 이상.

4.  확인은 **Java 개발 키트 위치가** JDK 1.8에 대해 구성 된:

    [![도구 옵션에서 JDK 8 경로 구성](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png)

    Visual Studio에서이 설정을 보려면 클릭 **도구 > 옵션 > Xamarin > Android 설정**합니다. Mac 용 Visual Studio에서 클릭 **기본 설정 > 프로젝트 > SDK 위치 > Android**합니다.


<a name="xaproject" />

### <a name="start-a-xamarinandroid-project"></a>Start a Xamarin.Android Project

새 Xamarin.Android 프로젝트를 만듭니다. Xamarin 사용한 Android 개발을 처음 접하는 경우 참조 [Hello, Android](~/android/get-started/hello-android/index.md) Xamarin.Android 프로젝트를 만드는 방법에 대 한 자세한 내용은 합니다.

Android 프로젝트를 만들 때 대상 Android 7.0 이상 버전 설정을 구성 해야 합니다. 예를 들어 Android 7.0에 대 한 프로젝트를 대상으로 구성 해야 하는 프로젝트 대상 Android API 수준 **Android 7.0 (API 24-Nougat)**합니다. 대상 프레임 워크 수준 이상으로 API 24를 설정 하는 것이 좋습니다. Android API 수준 수준을 구성 하는 방법에 대 한 자세한 내용은 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다.


> [!NOTE]
> **참고:** 현재 설정 해야 합니다는 **최소 Android 버전** 를 **Android 7.0 (API 24-Nougat)** Nougat Android 장치 또는 에뮬레이터에 앱을 배포 하려면.


<a name="emudev" />

### <a name="configure-an-emulator-or-device"></a>에뮬레이터 또는 장치를 구성 합니다.

에뮬레이터를 사용 하는 경우 Android AVD Manager를 시작 하 고 다음 설정을 사용 하 여 새 장치를 만듭니다.

-   장치: Nexus 5, 6, 9, Nexus 6 P, Nexus 플레이어 Nexus 또는 픽셀 C. Nexus X
-   대상: Android 7.0-24 API 레벨
-   ABI: x86 또는 x86\_64

예를 들어이 가상 장치를 Nexus 6을 에뮬레이션 하기 위해 구성 합니다.

[![AVD Nexus 6 장치, Android 7.0 대상 및 Intel x86 Atom CPU/ABI를 사용 하 여 구성](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png)

X 5, 6, 또는 9 Nexus 같은 물리적 장치를 사용 하는 경우에 무선 OTA 업데이트를 통해 자동을 통해 장치를 업데이트 하거나 시스템 이미지를 다운로드 하 고 장치를 직접 업데이트 수 합니다. Android Nougat에 장치를 수동으로 업데이트 하는 방법에 대 한 자세한 내용은 참조 [Nexus 장치용 OTA 이미지](https://developers.google.com/android/nexus/ota)합니다.

참고 Nexus 5 장치에서 Android Nougat 지원 되지 않습니다.


<a name="newfeatures" />

## <a name="new-features"></a>새 기능

Android Nougat 다양 한 새로운 기능과 다중 창을 지원, 알림 향상 기능 및 데이터 보호기 같은 기능을 소개합니다. 다음 섹션에서는 이러한 기능을 강조 표시 하 고 응용 프로그램에서 사용 하 여 시작할 수 있도록 링크를 제공 합니다.


<a name="multiwindow" />

### <a name="multi-window-mode"></a>다중 창 모드

다중 창 모드로 사용 하면 사용자가 전체 멀티태스킹 지원을 통해 한 번에 두 개의 앱을 열 수 있습니다. 이러한 응용 프로그램 화면 분할 모드로-side-by-side (가로) 또는 1-위에-the-기타 (세로)을 실행할 수 있습니다.
사용자, 크기를 조정 하려면 두 앱 간에 구분선을 끌 수 있으며 잘라내기 및 콘텐츠를 붙여 넣을 수 있습니다는 앱 간에 합니다. 두 응용 프로그램이 다중 창 모드로 표시 하는 경우 선택된 된 활동이 계속 선택 되지 않은 작업은 표시 되지만 일시 중지 된 동안 실행 됩니다. 다중 창 모드는 Android 활동 수명 주기를 수정 하지 않습니다.

[![세로 및 가로의 다중 창 모드에서 실행 되는 예제 앱](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png)

Xamarin.Android 앱의 활동을 다중 창 모드를 지원 하는 방법을 구성할 수 있습니다. 예를 들어 다중 창 모드로 최소 크기 및 기본 높이 응용 프로그램의 너비를 설정 하는 특성을 구성할 수 있습니다. 새 사용할 수 있습니다 `Activity.IsInMultiWindowMode` 다중 창 모드로 작업 인지 확인 하는 속성입니다. 예:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) 다중 창 사용자 인터페이스 응용 프로그램과 함께 활용 하는 방법을 보여 주는 C# 코드를 포함 하는 샘플 응용 프로그램입니다.

다중 창 모드에 대 한 자세한 내용은 참조는 [다중 창을 지원](https://developer.android.com/guide/topics/ui/multi-window.html)합니다.


<a name="enhanced_notifications" />

### <a name="enhanced-notifications"></a>향상 된 알림

Android Nougat 다시 디자인 된 알림 시스템을 소개합니다. 사용자가 들어오는 텍스트 메시지 알림 UI에서 직접에 대 한 알림 신속 하 게 회신을 가능 하 게 하는 새 직접 회신 기능을 제공 합니다. Android 7.0에서는 알림 메시지 수 함께 제공 되는 단일 그룹으로 둘 이상의 메시지를 받을 때 시작 합니다. 또한 개발자 알림 뷰 알림에서 시스템 장식 활용 및 알림을 생성할 때 새 알림 템플릿 활용 하기 위해 사용자 지정할 수 있습니다.

<a name="direct_reply" />

#### <a name="direct-reply"></a>직접 회신

들어오는 메시지에 대 한 알림을 수신 하는 사용자를 Android Nougat를 통해 알림 내에서 메시지에 회신 하 (보다는 대 한 회신을 보낼 메시징 응용 프로그램을 엽니다).
이 인라인 회신 기능을 사용 하면 사용자가 SMS 또는 텍스트 메시지 알림 인터페이스 내에서 직접에 신속 하 게 응답할 수 있습니다.

[![인라인 직접 회신 필드와 알림 스크린 샷](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png)

응용 프로그램에서이 기능을 지원 하려면 추가 해야 *인라인 회신 동작* 를 통해 응용 프로그램에 [RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/) 개체 알림 UI에서 직접 텍스트를 통해 사용자가 응답할 수 있도록 합니다.
예를 들어 다음 코드 빌드는 `RemoteInput` 텍스트 입력을 받고에 대 한 회신 동작에 대 한 보류 중인 의도 작성 하 고 원격 입력된 활성화 된 작업을 만듭니다.

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

[메시징 서비스](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) 샘플 응용 프로그램에 알림을 확장 하는 방법을 보여 주는 C# 코드를 포함 한 `RemoteInput` 개체입니다. 인라인 회신을 추가 하는 방법에 대 한 자세한 내용은 작업 앱을 Android 7.0 또는 이상 버전에서는 참조은 Android [알림 회신](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) 항목입니다.

<a name="bundled_notifications" />

#### <a name="bundled-notifications"></a>번들로 묶은 알림

Android Nougat (예를 들어 메시지 항목)에서 알림 메시지를 함께 그룹화 하 고 각 메시지 아닌 그룹을 표시할 수 있습니다.
이 *알림 bundled* 기능을 사용 하면 사용자가을 해제 하거나 보관을 한 번에 알림 그룹에 대 한 합니다. 사용자를 각 알림 세부 정보에서 볼에 알림 번들 확장 내려갈 수 있습니다.:

[![번들로 묶은 알림 스크린 샷 예제](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png)

알림을 지원 하기 위해 번들을 응용 프로그램에서 사용할 수는 [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/) 유사한 알림을 번들로 묶는 방법입니다. Android n에서 번들로 묶은 알림 그룹에 대 한 자세한 내용은 참조는 Android [번들 알림](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) 항목입니다.

<a name="custom_views" />

#### <a name="custom-views"></a>사용자 지정 보기

Android Nougat 시스템 알림 헤더, 동작 및 확장 가능한 레이아웃으로 사용자 지정 알림 보기를 만들 수 있습니다. Android Nougat에 사용자 지정 알림 보기에 대 한 자세한 내용은 참조는 Android [알림 향상 된 기능](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) 항목입니다.


<a name="datasaver" />

### <a name="data-saver"></a>데이터 보호기

Android Nougat 부터는 사용자가 사용할 수는 새 *데이터 보호기* 블록 배경 데이터 사용을 설정 합니다. 또한이 설정은 가능 포그라운드에서 적은 양의 데이터를 사용 하 여 앱을 알립니다. [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/) 앱 응용 프로그램 데이터 보호기가 활성화 된 경우 해당 데이터 사용을 제한 하기 위해 만들 수 있도록 사용자 데이터 보호기가 활성화 여부를 확인할 수 있도록 Android Nougat에서 확장 되었습니다.

Android Nougat의 새로운 데이터 보호기 기능에 대 한 자세한 내용은 참조는 Android [네트워크 데이터 사용량 최적화](https://developer.android.com/training/basics/network-ops/data-saver.html) 항목입니다.


<a name="app_shortcuts" />

### <a name="app-shortcuts"></a>앱 바로 가기 키

Android 7.1 도입는 *응용 프로그램 바로 가기를* 사용자가 신속 하 게 시작 공통 또는 권장 작업을 앱과을 가능 하 게 하는 기능입니다.
바로 가기 메뉴를 활성화 하려면 사용자 장기 누름 앱 아이콘 초 이상의 대 한 &ndash; 빠른 진동 함께 메뉴가 나타납니다.
유지 하기 위한 메뉴를 프로그램이 언론을 해제 합니다.

[![앱 바로 가기 메뉴를 사용 하 여 메시징 응용 프로그램에의 예제에서는 화면](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png)

이 기능은 사용할 수 있는 유일한 API 수준 25 이상으로 구성 되어 있습니다.
Android 7.1의 새로운 응용 프로그램 바로 가기를 기능에 대 한 자세한 내용은 참조는 Android [응용 프로그램 바로 가기를](https://developer.android.com/guide/topics/ui/shortcuts.html) 항목입니다.

<a name="sample_code" />

### <a name="sample-code"></a>샘플 코드

여러 가지 Xamarin.Android 샘플이 Android Nougat 기능을 활용 하는 방법을 보여 주는 사용할 수 있습니다.

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) Android Nougat에서 사용할 수 있는 다중 창 API의 사용법을 보여줍니다. 샘플 응용 프로그램을 어떻게 응용 프로그램의 수명 주기 및 동작 적용 되었는지 확인 다중 windows 모드로 전환할 수 있습니다.

-   [메시징 서비스](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) 알림을 전송 하는 간단한 서비스를 사용 하 여 `NotificationCompatManager`합니다. 또한 포함 된 알림 확장는 `RemoteInput` Android Nougat 장치 앱을 열 필요 없이 텍스트를 통해 알림을에서 직접 회신 수 있도록 하는 개체입니다.

-   [활성 알림](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/) 사용 하는 방법을 보여 줍니다.는 `NotificationManager` 현재 응용 프로그램에 표시 하는 알림 개수를 파악 하는 API입니다.

-   [디렉터리 액세스 범위](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/) 특정 디렉터리에 쉽게 액세스 범위 지정 된 디렉터리 액세스 API를 사용 하는 방법을 보여 줍니다. 이 정의 하려면 지정 하는 대신 갖습니다 `READ_EXTERNAL_STORAGE` 또는 `WRITE_EXTERNAL_STORAGE` 매니페스트에 사용 권한입니다.

-   [부팅 직접](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/) 장치 부팅 되는 둘 다 전과 후 모든 사용자 credentials(PIN/Pattern/Password)을 입력 하는 동안 항상 사용할 수 있는 장치 암호화 저장소에 데이터를 저장 하는 방법을 보여 줍니다.

<a name="summary" />

## <a name="summary"></a>요약

이 문서는 Android Nougat 도입 하 고 설치 하 고 Android Nougat에 있는 최신 도구와 Xamarin.Android 개발에 대 한 패키지를 구성 하는 방법을 설명 합니다. 또한 Nougat Android 용 앱을 만드는에서 시작할 수 있도록 예제에서는 소스 코드에 대 한 링크가 있는 Android Nougat에서 사용할 수 있는 주요 기능에 대해 간략하게를 제공 합니다.


## <a name="related-links"></a>관련 링크

- [개발자를 위한 android 7.1](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Xamarin Android 7.0 릴리스 정보](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
