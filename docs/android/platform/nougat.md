---
title: Nougat 기능
description: Xamarin.Android를 사용 하 여 Android Nougat 용 앱 개발을 시작 하는 방법.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: 2e15de944f05fede802dbf52987d80a46fb890ef
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242155"
---
# <a name="nougat-features"></a>Nougat 기능

_Xamarin.Android를 사용 하 여 Android Nougat 용 앱 개발을 시작 하는 방법._

이 문서에서는 Android Nougat에서 도입 된 기능의 개요 Xamarin.Android Android Nougat 개발, 준비 하는 방법에 설명 및 Android Nougat 기능을 사용 하는 방법을 보여 주는 샘플 응용 프로그램에 대 한 링크를 제공 합니다. 제공 Xamarin.Android 앱입니다.


## <a name="overview"></a>개요

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) Android 6.0 Marshmallow에 Google의 후속 됩니다. Xamarin.Android에 대 한 지원 제공 **Android 7.x 바인딩** Xamarin Android 7.0 이상. Android Nougat; 아래에 설명 된 Nougat 기능에 대 한 많은 새 Api 추가 Xamarin.Android 7.0을 사용 하는 경우 이러한 Api는 Xamarin.Android 앱에 사용할 수 있습니다.

[![Android 태블릿 및 휴대폰 Android Nougat 실행의 대표 이미지](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Android 7.x Api에 대 한 자세한 내용은 참조 하세요. [개발자를 위한 Android 7.1](http://developer.android.com/preview/api-overview.html)합니다.
Xamarin.Android 7.0의 알려진된 문제 목록을 참조 하십시오 합니다 [릴리스](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)합니다.

Android Nougat Xamarin.Android 개발자에 게 관심 있는 여러 가지 새로운 기능을 제공합니다. 이러한 기능에는 다음이 포함됩니다.

-   **다중 창 지원** &ndash; 이 향상 된이 기능을 사용 하면 사용자가 화면에서 두 개의 앱을 한 번에 열 수 있습니다.

-   **알림 향상** &ndash; Android Nougat에서 다시 디자인 된 알림 시스템에는 *직접 회신* 알림에서 직접 문자 메시지에 빠르게 대응할 수 있도록 하는 기능 UI입니다. 앱에 대 한 알림을 만드는 경우 수신 하는 또한 메시지, 새 *알림을 번들* 기능 번들로 알림을 함께 단일 그룹으로 둘 이상의 메시지를 받을 때.

-   **데이터 보호기** &ndash; 이 기능은 앱에서 셀룰러 데이터 사용을 줄이는 데 도움이 되는 새 시스템 서비스는 사용자가 앱에서 셀룰러 데이터를 사용 하는 방법에 대 한 제어를 제공 합니다.

Android Nougat 다른 많은 향상 된 기능을 제공 하는 또한 새 네트워크 보안 구성 기능 등 앱 개발자에 게 관심 이동 Doze, 증명, 새로운 빠른 설정 Api를 다중 로캘 지원을 ICU4J Api WebView 향상 된 기능 키 Java 8 언어 기능에 대 한 액세스, 범위가 지정 된 디렉터리 액세스, 포인터 사용자 지정 API, 플랫폼 VR 지원, 가상 파일 및 백그라운드 최적화를 처리 합니다.

이 문서에서는 새 기능을 사용해 새 Android Nougat 플랫폼을 대상으로 마이그레이션 또는 기능 작업을 계획 하는 Android Nougat를 사용 하 여 앱 빌드를 시작 하는 방법에 설명 합니다.


## <a name="requirements"></a>요구 사항

다음은 Xamarin 기반 앱에서 새 Android Nougat 기능을 사용 하려면 필요 합니다.

-   **Visual Studio 또는 Mac 용 Visual Studio** &ndash; 4.2.0.628 버전 Visual Studio를 사용 하는 경우 또는 Visual Studio Tools for Xamarin의 뒷부분에 나오는 필요 합니다. 사용 중인 경우 Visual Studio 6.1.0 버전, Mac 또는 Visual Studio의 나중에 Mac이 필요 합니다.

-   **Xamarin.Android** &ndash; 7.0 또는 이후 버전의 Xamarin.Android를 설치 하 고 mac 용 Visual Studio 또는 Visual Studio를 사용 하 여 구성 해야

-   **Android SDK** -Android SDK (API 24) 7.0 또는 나중에 설치 해야 Android SDK Manager를 통해.

-   **Java Developer Kit** &ndash; Xamarin Android 7.0 개발 필요 [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) API 수준 24 개발 하는 경우 이후 또는 큰 (JDK 8도 지원 API 수준 24 미만도). JDK 8 64 비트 버전이 필요한 경우 사용자 지정 컨트롤 또는 폼 미리 보기를 사용 하는입니다.

> [!IMPORTANT]
> Xamarin.Android는 JDK 9를 지원하지 않습니다.

Note는 앱 빌드해야 Xamarin C6SR4 이상 Android Nougat를 사용 하 여 안정적으로 작동 합니다. Android Nougat에만 연결할 수 있으므로 [NDK 제공한 네이티브 라이브러리](https://developer.android.com/about/versions/nougat/android-7.0-changes.html)와 같은 라이브러리를 사용 하 여 기존 응용 프로그램 **Mono.Data.Sqlite.dll** 하지 않은 경우 제대로 Android Nougat에서 실행 하는 경우 충돌이 발생할 수 있습니다 다시 작성 합니다.



## <a name="getting-started"></a>시작

시작 하려면 Android Nougat Xamarin.Android와 함께 사용 하 여 다운로드 하며 Android Nougat 프로젝트를 만들려면 먼저 최신 도구 및 SDK 패키지를 설치 합니다.

1.  Xamarin에서 최신 Xamarin.Android 업데이트를 설치 합니다.

2.  설치 합니다 **Android 7.0 (API 24)** 패키지 및 도구 이상입니다.

3.  Android Nougat를 대상으로 하는 새 Xamarin.Android 프로젝트를 만듭니다.

4.  Android Nougat에 대해 에뮬레이터 또는 장치를 구성 합니다.

이러한 각 단계는 다음 섹션에서 설명 됩니다.


### <a name="install-xamarin-updates"></a>Xamarin 업데이트를 설치 합니다.

Xamarin Android Nougat 지원을 추가 하 고 안정 채널을 Mac 용 Visual Studio 또는 Visual Studio의 업데이트 채널 변경 및 최신 업데이트를 적용 합니다. 또한 현재 알파 또는 베타 채널 에서만에서 사용할 수 있는 기능이 필요한 경우에 알파 또는 베타 채널 (알파 및 베타 채널을 위한 지원도 제공 Android 7.x) 전환할 수 있습니다. (릴리스) 업데이트 채널 변경 하는 방법에 대 한 정보를 참조 하세요 [업데이트 채널 변경](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel)합니다.



### <a name="install-the-android-sdk"></a>Android SDK 설치

Xamarin Android 7.0을 사용 하 여 프로젝트를 만들려면 하면 먼저 사용 하 여 Android SDK Manager를 설치 하려면 **SDK 플랫폼 Android N (API 24)** 이상. 최신도 설치 해야 합니다 **Android SDK Tools**:

1.  Android SDK Manager 시작 (Mac 용 Visual Studio에서 사용 하 여 **도구 > Android SDK 관리자를 열고&hellip;**; Visual Studio를 사용 하 여 **도구 > Android > Android SDK Manager**).

2.  설치할 **Android 7.0 (API 24)** 이상:

    [![Android SDK Manager에서 Android 7.0 패키지 선택](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  최신 Android SDK 도구를 설치 합니다.

    [![Android SDK Manager의 최신 Android SDK 도구 선택](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Android SDK Tools 버전 25.2.2 또는 이상, Android SDK 플랫폼 도구 24.0.3 이상 및 Android SDK 빌드 도구 24.0.2 설치 해야 이상.

4.  있는지 확인 합니다 **Java Development Kit 위치** JDK 1.8에 대 한 구성 됩니다.

    [![도구 옵션에서 JDK 8 경로 구성](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    이 설정은 Visual Studio에서 보려면 클릭 **도구 > 옵션 > Xamarin > Android 설정**합니다. Mac 용 Visual Studio에서 클릭 **기본 설정 > 프로젝트 > SDK 위치 > Android**합니다.



### <a name="start-a-xamarinandroid-project"></a>Xamarin.Android 프로젝트를 시작 합니다.

새 Xamarin.Android 프로젝트를 만듭니다. Xamarin 사용한 Android 개발을 처음 접하는 경우 참조 [Hello, Android](~/android/get-started/hello-android/index.md) Xamarin.Android 프로젝트 만들기에 대해 알아보려면 합니다.

Android 프로젝트를 만들면 대상 Android 7.0 이상 버전 설정을 구성 해야 합니다. 예를 들어 Android 7.0에 대 한 프로젝트를 대상으로 구성 해야 하는 프로젝트의 대상 Android API 레벨 **Android 7.0 (API 24-Nougat)** 합니다. 대상 프레임 워크 수준 API 24 개 이상 설정 하는 것이 좋습니다. Android API 수준 수준을 구성 하는 방법에 대 한 자세한 내용은 참조 하세요 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다.


> [!NOTE]
> 현재 설정 해야 합니다 **최소 Android 버전** 에 **Android 7.0 (API 24-Nougat)** Android Nougat 장치 또는 에뮬레이터에 앱을 배포 하려면.



### <a name="configure-an-emulator-or-device"></a>에뮬레이터 또는 장치를 구성 합니다.

에뮬레이터를 사용 하는 경우 Android AVD Manager를 시작 하 고 다음 설정을 사용 하 여 새 장치를 만듭니다.

-   장치: Nexus 5 Nexus 6, Nexus 6 P, Nexus 플레이어 Nexus 9, 또는 3. 픽셀 X
-   대상: Android 7.0-API 수준 24
-   ABI: x86 또는 x86\_64

예를 들어,이 가상 장치를 구성 하는 데 Nexus 6을 에뮬레이트 해야 합니다.

[![Nexus 6 장치, Android 7.0 대상 및 Intel x86 Atom CPU/ABI를 사용 하 여 AVD를 구성 합니다.](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

X 5, 6 또는 9 Nexus 같은 물리적 장치를 사용 하는 경우에 무선 OTA 업데이트를 통해 자동을 통해 장치를 업데이트 하거나 시스템 이미지를 다운로드 하 고 장치를 직접 플래시 수 있습니다. Android Nougat에 장치를 수동으로 업데이트 하는 방법에 대 한 자세한 내용은 참조 하세요. [Nexus 장치에 대 한 OTA 이미지](https://developers.google.com/android/nexus/ota)합니다.

참고 Nexus 5 장치가 Android Nougat에서 지원 되지 않습니다.



## <a name="new-features"></a>새 기능

Android Nougat 다양 한 새로운 기능과 다중 창을 지원, 알림 향상 된 기능 및 데이터 보호기와 같은 기능을 소개합니다. 다음 섹션에서는 이러한 기능을 강조 하 고 앱에서 사용 하는 데 대 한 링크 시작을 제공 합니다.



### <a name="multi-window-mode"></a>다중 창 모드

다중 창 모드를 통해 사용자가 전체 멀티태스킹 지원을 통해 한 번에 두 개의 앱을 열 수 있습니다. 이러한 앱 화면 분할 모드로-side-by-side (가로) 또는 1-위에서-the-다른 (세로)을 실행할 수 있습니다.
사용자 조정 하거나, 앱 간의 구분선을 끌어 놓을 수 있으며 잘라내기 및 콘텐츠를 붙여 넣을 수 있습니다는 앱 간에 합니다. 두 개의 앱 다중 창 모드에서 표시 되 면 선택한 작업 계속 선택 되지 않은 작업이 일시 중지 하지만 여전히 표시 하는 동안 실행 됩니다. 다중 창 모드에서 Android 작업 수명 주기를 수정 하지 않습니다.

[![가로 및 세로 다중 창 모드에서 실행 되는 예제 앱](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Xamarin android 앱의 활동 다중 창 모드를 지원 하는 방법을 구성할 수 있습니다. 예를 들어 다중 창 모드에서 기본 높이 최소 크기 및 앱의 너비를 설정 하는 특성을 구성할 수 있습니다. 새 따르면 `Activity.IsInMultiWindowMode` 다중 창 모드에서 작업 인지 확인 하는 속성입니다. 예를 들어:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

합니다 [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) 다중 창 사용자 인터페이스와 앱을 활용 하는 방법을 보여 주는 C# 코드를 포함 하는 샘플 앱입니다.

다중 창 모드에 대 한 자세한 내용은 참조는 [다중 창을 지원](https://developer.android.com/guide/topics/ui/multi-window.html)합니다.



### <a name="enhanced-notifications"></a>향상 된 알림

Android Nougat 새로 디자인 된 알림 시스템을 소개합니다. 들어오는 텍스트 메시지 알림 UI에서 직접에 대 한 알림을 회신 신속 하 게 할 수 있도록 하는 새로운 직접 회신 기능을 특징으로 합니다. Android 7.0, 알림 메시지 수로 묶을 수 단일 그룹으로 둘 이상의 메시지를 받을 때부터 시작 합니다. 또한 개발자 알림 뷰 알림의 시스템 장식을 활용 및 알림을 생성할 때 새 알림 템플릿을 활용을 사용자 지정할 수 있습니다.


#### <a name="direct-reply"></a>직접 회신

들어오는 메시지에 대 한 알림을 받은 사용자를 Android Nougat 있도록 알림 내에서 메시지에 회신 대신 회신을 보낼 메시징 앱을 열 수 있습니다.
이 인라인 응답 기능을 사용 하면 사용자가 SMS 또는 텍스트 메시지 알림 인터페이스 내에서 직접 신속 하 게 응답할 수 있습니다:

[![인라인 직접 회신 필드를 사용 하 여 알림의 스크린 샷](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

추가 앱에서이 기능을 지원 해야 합니다 *인라인 회신 동작* 를 통해 앱에는 [RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/) 개체 사용자가 알림을 UI에서 직접 텍스트를 통해 회신할 수 있도록 합니다.
예를 들어, 다음 코드 빌드를 `RemoteInput` 텍스트 입력을 받고에 대 한 회신 동작에 대 한 보류 중인 의도 빌드하고 원격 입력된 설정 된 작업을 만듭니다.

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

[메시징 서비스가](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) 알림을 사용 하 여 확장 하는 방법을 보여 주는 C# 코드를 포함 하는 샘플 앱을 `RemoteInput` 개체입니다. 인라인 회신을 추가 하는 방법에 대 한 자세한 내용은 작업 앱을 Android 7.0 이상에 참조 Android [알림에 회신](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) 항목입니다.


#### <a name="bundled-notifications"></a>번들된 알림

Android Nougat (예를 들어 메시지 항목)에서 알림 메시지를 함께 그룹화 하 고 각 메시지를 사용 하지 않고 그룹을 표시할 수 있습니다.
이 *알림을 번들* 기능을 사용 하면 사용자를 해제 하거나 하나의 작업의 알림 그룹을 보관할 수 있습니다. 사용자는 각 알림 세부 정보에서 보려는 알림의 번들 확장 아래로 슬라이드 할 수 있습니다.

[![번들 알림의 스크린샷 예제](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

번들된 알림을 지원 하려면 앱이 사용할 수는 [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/) 번들 유사한 알림 방법입니다. Android N에서 번들된 알림 그룹에 대 한 자세한 내용은 참조는 Android [묶음 알림](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) 항목입니다.


#### <a name="custom-views"></a>사용자 지정 보기

Android Nougat 시스템 알림 헤더, 동작 및 확장 가능한 레이아웃을 사용 하 여 사용자 지정 알림 뷰를 만들 수 있습니다. Android Nougat에서 사용자 지정 알림 보기에 대 한 자세한 내용은 참조는 Android [알림 향상](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) 항목입니다.



### <a name="data-saver"></a>데이터 보호기

Android Nougat 부터는 사용자가 설정할 수 있습니다. 새 *데이터 보호기* 블록 백그라운드 데이터 사용을 설정 합니다. 이 설정에는 가능한 포그라운드에 적은 양의 데이터를 사용 하도록 앱을 신호로 알립니다. 합니다 [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/) 앱은 앱 데이터 보호기가 활성화 하는 경우 해당 데이터 사용량을 제한 하기 위해 할 수 있도록 사용자 데이터 보호기가 활성화 여부를 확인할 수 있도록 Android Nougat에서 확장 되었습니다.

Android Nougat에서 새로운 데이터 보호기 기능에 대 한 자세한 내용은 참조는 Android [네트워크 데이터 사용량 최적화](https://developer.android.com/training/basics/network-ops/data-saver.html) 항목입니다.



### <a name="app-shortcuts"></a>앱 바로 가기

Android 7.1 도입을 *앱 바로 가기* 신속 하 게 시작 일반 또는 권장 되는 태스크 응용 프로그램을 사용 하 여 할 수 있도록 하는 기능입니다.
바로 가기 메뉴를 활성화 하려면 사용자 장기-누름 앱 아이콘을 두 번째 이상 &ndash; 빠른 진동을 사용 하 여 표시 합니다.
눌러 해제 메뉴를 유지를 발생 시킵니다.

[![앱 바로 가기 메뉴를 사용 하 여 메시징 앱을의 예제에서는 화면](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

이 기능은 사용할 수 있는 유일한 API 레벨 25 이상으로 구성 되어 있습니다.
Android 7.1의 새로운 앱 바로 가기 기능에 대 한 자세한 내용은 참조는 Android [앱 바로 가기](https://developer.android.com/guide/topics/ui/shortcuts.html) 항목입니다.


### <a name="sample-code"></a>샘플 코드

여러 Xamarin.Android 샘플은 Android Nougat 기능을 활용 하는 방법을 보여 줍니다.

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) Android Nougat에서 사용할 수 있는 다중 창 API의 사용법을 보여 줍니다. 샘플 앱에 미치는 영향 앱의 수명 주기와 동작을 확인 하려면 여러 windows 모드로 전환할 수 있습니다.

-   [메시징 서비스](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) 알림을 전송 하는 간단한 서비스를 사용 하 여 `NotificationCompatManager`입니다. 또한 알림을 사용 하 여 확장을 `RemoteInput` Android Nougat 장치 앱을 열지 않고도 알림에서 직접 텍스트를 통해 회신을 허용 하는 개체입니다.

-   [활성 알림](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/) 사용 하는 방법을 보여 줍니다는 `NotificationManager` API에 얼마나 많은 알림을 현재 응용 프로그램에 표시 합니다.

-   [범위가 지정 된 디렉터리 액세스](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/) 범위가 지정 된 디렉터리 액세스 API를 사용 하 여 특정 디렉터리에 쉽게 액세스 하는 방법에 설명 합니다. 이 정의 하는 대신 역할도 `READ_EXTERNAL_STORAGE` 또는 `WRITE_EXTERNAL_STORAGE` 매니페스트에 사용 권한.

-   [부팅 직접](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/) 장치가 부팅 되는 둘 다 전과 후 모든 사용자 credentials(PIN/Pattern/Password) 입력 하는 동안 항상 사용할 수 있는 장치에서 암호화 된 저장소에 데이터를 저장 하는 방법을 보여 줍니다.


## <a name="summary"></a>요약

이 문서에서는 Android Nougat를 도입 하 고 설치 및 Android Nougat에서 최신 도구 및 Xamarin.Android 개발에 대 한 패키지를 구성 하는 방법을 설명 합니다. 또한 Android Nougat에 대해 앱을 만들기 시작할 수 있도록 하는 예제 소스 코드에 대 한 링크를 사용 하 여 Android Nougat에서 사용할 수 있는 주요 기능의 개요를 제공 합니다.


## <a name="related-links"></a>관련 링크

- [개발자를 위한 android 7.1](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Xamarin Android 7.0 릴리스 정보](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
