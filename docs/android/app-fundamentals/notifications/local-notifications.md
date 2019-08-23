---
title: Android의 로컬 알림
description: 이 섹션에서는 Xamarin.ios에서 로컬 알림을 구현 하는 방법을 보여 줍니다. Android 알림의 다양 한 UI 요소에 대해 설명 하 고 알림을 만들고 표시 하는 것과 관련 된 API에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/16/2018
ms.openlocfilehash: 19998f685955ce118ffe37e7624fd43b082ab994
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644427"
---
# <a name="local-notifications-on-android"></a>Android의 로컬 알림

_이 섹션에서는 Xamarin.ios에서 로컬 알림을 구현 하는 방법을 보여 줍니다. Android 알림의 다양 한 UI 요소에 대해 설명 하 고 알림을 만들고 표시 하는 것과 관련 된 API에 대해 설명 합니다._

## <a name="local-notifications-overview"></a>로컬 알림 개요

Android는 알림 아이콘과 알림 정보를 사용자에 게 표시 하는 두 가지 시스템 제어 영역을 제공 합니다. 알림이 처음 게시 되 면 다음 스크린샷에 표시 된 것 처럼 해당 아이콘이 *알림 영역*에 표시 됩니다.

![장치의 예제 알림 영역](local-notifications-images/01-notification-shade.png)

알림에 대 한 세부 정보를 얻기 위해 사용자는 알림 서랍 (각 알림 아이콘을 확장 하 여 알림 콘텐츠 표시)을 열고 알림과 관련 된 모든 작업을 수행할 수 있습니다. 다음 스크린샷은 위에 표시 된 알림 영역에 해당 하는 *알림 서랍* 을 보여 줍니다.

[![3 개 알림을 표시 하는 알림 서랍 예](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Android 알림은 다음과 같은 두 가지 유형의 레이아웃을 사용 합니다.

-   ***기본 레이아웃*** &ndash; 간결한 고정 프레젠테이션 형식입니다.

-   ***확장 된 레이아웃*** &ndash; 추가 정보를 표시 하기 위해 더 큰 크기로 확장할 수 있는 프레젠테이션 형식입니다.

다음 섹션에서는 이러한 각 레이아웃 유형과 이러한 레이아웃을 만드는 방법에 대해 설명 합니다.

> [!NOTE]
> 이 가이드에서는 [Android 지원 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)의 [notificationcompat api](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) 에 대해 집중적으로 설명 합니다. 이러한 Api는 Android 4.0 (API 수준 14)에 대 한 최대 이전 버전과의 호환성을 보장 합니다.

### <a name="base-layout"></a>기본 레이아웃

모든 Android 알림은 기본 레이아웃 형식으로 빌드됩니다. 여기에는 최소한 다음 요소가 포함 됩니다.

1.  원본 앱을 나타내는 *알림 아이콘*또는 앱에서 다양 한 유형의 알림을 지 원하는 경우 알림 유형입니다.

2.  알림 *제목*또는 알림이 개인 메시지 인 경우 보낸 사람의 이름입니다.

3.  알림 메시지입니다.

4.  *타임 스탬프*입니다.

이러한 요소는 다음 다이어그램에 나와 있는 것 처럼 표시 됩니다.

[![알림 요소의 위치](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

기본 레이아웃은 높이의 64 dp (밀도 독립적 픽셀)로 제한 됩니다. Android는 기본적으로이 기본 알림 스타일을 만듭니다.

필요에 따라 알림은 응용 프로그램 또는 보낸 사람의 사진을 나타내는 커다란 아이콘을 표시할 수 있습니다. Android 5.0 이상에서 알림에 작은 아이콘을 사용 하는 경우 작은 알림 아이콘이 크게 아이콘 위에 배지로 표시 됩니다.

![간단한 알림 사진](local-notifications-images/04-simple-notification-photo.png)

Android 5.0부터 알림은 잠금 화면에도 표시 될 수 있습니다.

[![잠금 화면 알림 예제](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

사용자는 잠금 화면 알림을 두 번 탭 하 여 장치의 잠금을 해제 하 고 해당 알림을 시작한 앱으로 이동 하거나, 살짝 밀어 알림을 해제할 수 있습니다. 앱은 알림 표시 수준을 설정 하 여 잠금 화면에 표시 되는 항목을 제어 하 고, 사용자는 중요 한 콘텐츠를 화면 잠금 알림에 표시 하도록 허용할지 여부를 선택할 수 있습니다.

Android 5.0에는 우선 순위가 높은 알림 프레젠테이션 형식이 도입 *되었습니다.* 준비 알림은 몇 초 동안 화면 맨 위에서 아래로 이동 하 고 알림 영역에 다시 돌아갑니다.

[![예제 헤드 알림](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

알림 메시지를 통해 시스템 UI는 현재 실행 중인 작업의 상태를 방해 하지 않고 사용자 앞에 중요 한 정보를 넣을 수 있습니다.

Android에는 알림을 지능적으로 정렬 하 고 표시할 수 있도록 알림 메타 데이터에 대 한 지원이 포함 되어 있습니다. 알림 메타 데이터는 또한 잠금 화면 및 준비 형식에 알림이 표시 되는 방식을 제어 합니다. 응용 프로그램은 다음과 같은 유형의 알림 메타 데이터를 설정할 수 있습니다.

-   **우선 순위** &ndash; 우선 순위 수준은 알림이 표시 되는 방법 및 시기를 결정 합니다. 예를 들어 Android 5.0에서는 우선 순위가 높은 알림이 준비 알림으로 표시 됩니다.

-   **표시 유형** &ndash; 잠금 화면에 알림이 표시 될 때 표시 되는 알림 콘텐츠 크기를 지정 합니다.

-   **범주** 장치가 방해 금지 모드에 있을 때와 같이 다양 한 상황에서 알림을 처리 하는 방법을 시스템에 알립니다. &ndash;

> [!NOTE]
> **표시 유형** 및 **범주** 는 android 5.0에서 도입 되었으며 이전 버전의 android에서 사용할 수 없습니다. Android 8.0부터 [알림 채널](#notif-chan) 은 사용자에 게 알림이 표시 되는 방식을 제어 하는 데 사용 됩니다.


### <a name="expanded-layouts"></a>확장 된 레이아웃

Android 4.1 부터는 사용자가 알림의 높이를 확장 하 여 더 많은 콘텐츠를 볼 수 있도록 하는 확장 된 레이아웃 스타일로 알림을 구성할 수 있습니다. 예를 들어 다음 예제에서는 계약 된 모드에서 확장 된 레이아웃 알림을 보여 줍니다.

![계약 알림](local-notifications-images/07-contracted-notification.png)

이 알림이 확장 되 면 전체 메시지를 표시 합니다.

![확장 된 알림](local-notifications-images/08-expanded-notification.png)

Android는 단일 이벤트 알림에 대해 3 가지 확장 된 레이아웃 스타일을 지원 합니다.

-   ***큰 텍스트*** &ndash; 계약 된 모드에서는 메시지의 첫 번째 줄에 두 개의 마침표가 있는 발췌를 표시 합니다. 확장 모드에서는 전체 메시지를 표시 합니다 (위 예제에 표시 된 대로).

-   ***수신함*** &ndash; 계약 된 모드에서 새 메시지 수를 표시 합니다. 확장 된 모드에서 첫 번째 전자 메일 메시지 또는 수신함의 메시지 목록을 표시 합니다.

-   ***이미지*** &ndash; 계약 된 모드에서는 메시지 텍스트만 표시 합니다. 확장 모드에서 텍스트와 이미지를 표시 합니다.

[기본 알림 이상](#beyond-the-basic-notification) (이 문서 뒷부분에서는) *큰 텍스트*, *수신함*및 *이미지* 알림을 만드는 방법을 설명 합니다.

<a name="notif-chan"></a>
<a name="notification-channels"></a>
## <a name="notification-channels"></a>알림 채널

Android 8.0 (Oreo)부터 *알림 채널* 기능을 사용 하 여 표시 하려는 각 알림 유형에 대 한 사용자 지정 가능한 채널을 만들 수 있습니다. 알림 채널을 사용 하면 채널에 게시 된 모든 알림이 동일한 동작을 나타내도록 알림을 그룹화 할 수 있습니다. 예를 들어 즉각적인 주의가 필요한 알림과 정보 메시지에 사용 되는 별도의 "조용한" 채널을 위한 알림 채널이 있을 수 있습니다.

Android Oreo와 함께 설치 되는 **YouTube** 앱은 다음과 같은 두 가지 알림 범주를 나열 합니다. **알림** 및 **일반 알림**다운로드:

[![Android Oreo의 YouTube에 대 한 알림 화면](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

이러한 각 범주는 알림 채널에 해당 합니다. YouTube 앱은 **다운로드 알림** 채널 및 **일반 알림** 채널을 구현 합니다. 사용자는 앱의 다운로드 알림 채널에 대 한 설정 화면을 표시 하는 **다운로드 알림**을 탭 할 수 있습니다.

[![YouTube 앱에 대 한 알림 다운로드 화면](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

이 화면에서 사용자는 다음을 수행 하 여 **다운로드** 알림 채널의 동작을 수정할 수 있습니다.

-   중요도 수준을 **긴급**, **높음**, **중간**또는 **낮음**으로 설정 하 여 사운드 및 시각적 중단의 수준을 구성 합니다.

-   알림 점을 설정 하거나 해제 합니다.

-   깜박이는 조명을 설정 하거나 해제 합니다.

-   잠금 화면에 알림을 표시 하거나 숨깁니다.

-   **방해 금지** 설정을 재정의 합니다.

**일반 알림** 채널의 설정은 다음과 유사 합니다.

[![YouTube 앱에 대 한 일반 알림 화면](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

알림 채널이 사용자 &ndash; 와 상호 작용 하는 방식을 절대적으로 제어 하지는 않습니다. 사용자는 위의 스크린샷에 표시 된 대로 장치에서 알림 채널에 대 한 설정을 수정할 수 있습니다. 그러나 아래에 설명 된 대로 기본값을 구성할 수 있습니다. 이 예제에서 설명 하는 것 처럼 새 알림 채널 기능을 사용 하면 다양 한 종류의 알림을 사용자에 게 세분화 하 여 제어할 수 있습니다.

## <a name="notification-creation"></a>알림 만들기

Android에서 알림을 만들려면 [Xamarin.Android.Support.v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) Nuget.exe NuGet 패키지에서 [Notificationcompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder) 클래스를 사용 합니다. 이 클래스를 사용 하면 이전 버전의 Android에서 알림을 만들고 게시할 수 있습니다.
`NotificationCompat.Builder`도 설명 합니다.

`NotificationCompat.Builder`알림에서 다음과 같은 다양 한 옵션을 설정 하는 메서드를 제공 합니다.

-   제목, 메시지 텍스트 및 알림 아이콘이 포함 된 콘텐츠입니다.

-   *큰 텍스트*, *수신함*또는 *이미지* 스타일과 같은 알림 스타일입니다.

-   알림의 우선 순위는 minimum, low, default, high 또는 maximum입니다. Android 8.0 이상에서 우선 순위는 [_알림 채널_](#notification-channels)을 통해 설정 됩니다.

-   잠금 화면에 표시 되는 알림 (공개, 개인 또는 암호)입니다.

-   Android에서 알림을 분류 하 고 필터링 하는 데 도움이 되는 범주 메타 데이터입니다.

-   알림을 누를 때 시작할 활동을 나타내는 선택적 의도입니다.

-   알림이 게시 될 알림 채널의 ID입니다 (Android 8.0 이상).

작성기에서 이러한 옵션을 설정한 후에는 설정을 포함 하는 알림 개체를 생성 합니다. 알림을 게시 하려면이 알림 개체를 *알림 관리자*에 게 전달 합니다. Android는 알림 게시 및 사용자에 게 표시를 담당 하는 [Notificationmanager](xref:Android.App.NotificationManager) 클래스를 제공 합니다. 활동 또는 서비스와 같은 모든 컨텍스트에서이 클래스에 대 한 참조를 가져올 수 있습니다.

### <a name="creating-a-notification-channel"></a>알림 채널 만들기

Android 8.0에서 실행 되는 앱은 알림에 대 한 알림 채널을 만들어야 합니다. 알림 채널에는 다음과 같은 세 가지 정보가 필요 합니다.

- 채널을 식별 하는 패키지에 고유한 ID 문자열입니다.
- 사용자에 게 표시 되는 채널의 이름입니다.  이름은 1 ~ 007e; 40 자 사이 여야 합니다.
- 채널의 중요도입니다.

앱은 실행 중인 Android 버전을 확인 해야 합니다.
Android 8.0 이전 버전을 실행 하는 장치는 알림 채널을 만들 수 없습니다. 다음 메서드는 활동에서 알림 채널을 만드는 방법에 대 한 한 가지 예입니다.

```csharp
void CreateNotificationChannel()
{
    if (Build.VERSION.SdkInt < BuildVersionCodes.O)
    {
        // Notification channels are new in API 26 (and not a part of the
        // support library). There is no need to create a notification
        // channel on older versions of Android.
        return;
    }

    var channelName = Resources.GetString(Resource.String.channel_name);
    var channelDescription = GetString(Resource.String.channel_description);
    var channel = new NotificationChannel(CHANNEL_ID, channelName, NotificationImportance.Default)
                  {
                      Description = channelDescription
                  };

    var notificationManager = (NotificationManager) GetSystemService(NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

활동을 만들 때마다 알림 채널을 만들어야 합니다. 메서드의 경우 활동의 `OnCreate` 메서드에서 호출 해야 합니다. `CreateNotificationChannel`

### <a name="creating-and-publishing-a-notification"></a>알림 만들기 및 게시

Android에서 알림을 생성 하려면 다음 단계를 수행 합니다.

1.  개체를 `NotificationCompat.Builder` 인스턴스화합니다.

2.  `NotificationCompat.Builder` 개체에 대 한 다양 한 메서드를 호출 하 여 알림 옵션을 설정 합니다.

3.  `NotificationCompat.Builder` 개체의 [빌드](xref:Android.App.Notification.Builder.Build) 메서드를 호출 하 여 알림 개체를 인스턴스화합니다.

4.  알림 관리자의 [알림 메서드를](xref:Android.App.NotificationManager.Notify*) 호출 하 여 알림을 게시 합니다.

각 알림에 대해 최소한 다음 정보를 제공 해야 합니다.

-   작은 아이콘 (24x24 dp 크기)

-   짧은 제목

-   알림 텍스트

다음 코드 예제에서는를 사용 `NotificationCompat.Builder` 하 여 기본 알림을 생성 하는 방법을 보여 줍니다. 메서드는 `NotificationCompat.Builder` [메서드 체인](https://en.wikipedia.org/wiki/Method_chaining)을 지원 합니다. 즉, 각 메서드는 빌더 개체를 반환 하므로 마지막 메서드 호출의 결과를 사용 하 여 다음 메서드 호출을 호출할 수 있습니다.

```csharp
// Instantiate the builder and set notification elements:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

이 예제에서는 사용할 알림 채널 `NotificationCompat.Builder` 의 ID `builder` 와 함께 라는 새 개체가 인스턴스화됩니다. 알림의 제목과 텍스트가 설정 되 고, 알림 아이콘이 **Resources/그릴 수 있는/ic_notification**에서 로드 됩니다. 알림 작성기의 `Build` 메서드에 대 한 호출은 이러한 설정을 사용 하 여 알림 개체를 만듭니다. 다음 단계는 알림 관리자의 `Notify` 메서드를 호출 하는 것입니다. 알림 관리자를 찾으려면 위와 같이를 호출 `GetSystemService`합니다.

메서드 `Notify` 는 알림 식별자와 알림 개체의 두 가지 매개 변수를 허용 합니다. 알림 id는 응용 프로그램에 대 한 알림을 식별 하는 고유한 정수입니다. 이 예제에서 알림 식별자는 0으로 설정 됩니다. 그러나 프로덕션 응용 프로그램에서는 각 알림에 고유한 식별자를 지정 하는 것이 좋습니다. 호출에서 이전 식별자 값을 다시 사용 `Notify` 하면 마지막 알림이 덮어쓰여집니다.

이 코드는 Android 5.0 장치에서 실행 되는 경우 다음 예제와 같은 알림을 생성 합니다.

![샘플 코드에 대 한 알림 결과](local-notifications-images/09-hello-world.png)

알림 아이콘은 알림의 &ndash; 왼쪽에 표시 됩니다 .이 이미지는&rdquo; 원으로 &ldquo;표시 된 알파 채널이 있으므로 Android에서 회색 원형 배경을 그릴 수 있습니다. 알파 채널 없이 아이콘을 제공할 수도 있습니다. 사진 이미지를 아이콘으로 표시 하려면이 항목의 뒷부분에 나오는 [크게 아이콘 형식](#large-icon-format) 을 참조 하세요.

타임 스탬프는 자동으로 설정 되지만 알림 작성기의 [Setwhen](xref:Android.App.Notification.Builder.SetWhen*) 메서드를 호출 하 여이 설정을 재정의할 수 있습니다. 예를 들어 다음 코드 예제에서는 타임 스탬프를 현재 시간으로 설정 합니다.

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>음향 및 진동 사용

알림이 소리를 재생 하도록 하려면 알림 작성기의 [setdefaults](xref:Android.App.Notification.Builder.SetDefaults*) 메서드를 호출 하 고 `NotificationDefaults.Sound` 플래그를 전달 하면 됩니다.

```csharp
// Instantiate the notification builder and enable sound:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

이 호출 `SetDefaults` 을 수행 하면 알림이 게시 될 때 장치가 소리를 재생 합니다. 장치가 소리를 재생 하는 대신 진동 하도록 하려면를에 전달 하 여 장치 `NotificationDefaults.Vibrate` 에서 `SetDefaults.` 소리를 재생 하 고 장치를 진동 하는 경우에를 전달 하면 두 플래그를 모두에 `SetDefaults`전달할 수 있습니다.

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

재생할 소리를 지정 하지 않고 소리를 사용 하도록 설정 하면 Android에서 기본 시스템 알림 소리를 사용 합니다. 그러나 알림 작성기의 [setsound](xref:Android.App.Notification.Builder.SetSound*) 메서드를 호출 하 여 재생 되는 소리를 변경할 수 있습니다. 예를 들어 알림을 사용 하 여 알람 소리를 재생 하려면 (기본 알림 사운드 대신) [RingtoneManager](xref:Android.Media.RingtoneManager) 에서 알람 소리에 대 한 URI를 가져온 다음에 `SetSound`전달할 수 있습니다.

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

또는 알림에 대해 시스템 기본 벨 소리 소리를 사용할 수 있습니다.

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

알림 개체를 만든 후에는 메서드를 통해 `NotificationCompat.Builder` 미리 구성 하는 대신 알림 개체에 알림 속성을 설정할 수 있습니다. 예를 들어 알림에 대해 진동을 `SetDefaults` 사용 하도록 메서드를 호출 하는 대신 알림의 [기본값](xref:Android.App.Notification.Defaults) 속성의 비트 플래그를 직접 수정할 수 있습니다.

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

이 예제에서는 알림이 게시 될 때 장치를 진동 합니다.

### <a name="updating-a-notification"></a>알림 업데이트

게시 된 알림의 콘텐츠를 업데이트 하려는 경우 기존 `NotificationCompat.Builder` 개체를 다시 사용 하 여 새 알림 개체를 만들고이 알림을 마지막 알림의 식별자로 게시할 수 있습니다. 예:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

이 예제에서 기존 `NotificationCompat.Builder` 개체는 다른 제목 및 메시지를 사용 하 여 새 알림 개체를 만드는 데 사용 됩니다.
새 notification 개체는 이전 알림의 식별자를 사용 하 여 게시 되 고 이전에 게시 된 알림의 콘텐츠를 업데이트 합니다.

![업데이트 된 알림](local-notifications-images/12-updated-notification.png)

알림이 알림 서랍에 표시 되는 동안 &ndash; 이전 알림의 본문은 제목만 다시 사용 하 고 알림의 텍스트를 변경 합니다. 제목 텍스트가 "Sample Notification"에서 "업데이트 된 알림"으로 바뀌고 메시지 텍스트는 "Hello World! 이것은 첫 번째 알림입니다. " "이 메시지를 변경 했습니다."

다음 세 가지 중 하나가 발생할 때까지 알림이 계속 표시 됩니다.

-   사용자가 알림을 해제 하거나 [ *모두 지우기*]를 탭 합니다.

-   응용 프로그램은를 `NotificationManager.Cancel`호출 하 여 알림이 게시 될 때 할당 된 고유한 알림 ID를 전달 합니다.

-   응용 프로그램에서 `NotificationManager.CancelAll`를 호출 합니다.

Android 알림 업데이트에 대 한 자세한 내용은 [알림 수정](https://developer.android.com/training/notify-user/managing.html#Updating)을 참조 하세요.


### <a name="starting-an-activity-from-a-notification"></a>알림에서 작업 시작

Android에서 사용자가 알림을 누를 때 실행 되는 *작업* &ndash; 에 대 한 알림이 일반적으로 발생 합니다. 이 활동은 다른 응용 프로그램에 있거나 다른 작업 에서도 존재할 수 있습니다. 알림에 작업을 추가 하려면 [pendingintent](xref:Android.App.PendingIntent) 개체를 만들고이 알림과 연결 `PendingIntent` 합니다. 는 `PendingIntent` 받는 사람 응용 프로그램에서 보내는 응용 프로그램의 사용 권한으로 미리 정의 된 코드 조각을 실행할 수 있도록 하는 특별 한 유형의 의도입니다. 사용자가 알림을 탭 하면 Android가에 `PendingIntent`지정 된 작업을 시작 합니다.

다음 코드 조각에서는 원래 앱의 작업을 `PendingIntent` `MainActivity`시작 하는를 사용 하 여 알림을 만드는 방법을 보여 줍니다.

```csharp
// Set up an intent so that tapping the notifications returns to this app:
Intent intent = new Intent (this, typeof(MainActivity));

// Create a PendingIntent; we're only using one PendingIntent (ID = 0):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    PendingIntent.GetActivity (this, pendingIntentId, intent, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including pending intent:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

이 코드는가 알림 개체에 추가 된다는 `PendingIntent` 점을 제외 하 고 이전 섹션의 알림 코드와 매우 비슷합니다. 이 예제에서는 `PendingIntent` 알림 작성기의 [setcontentintent](xref:Android.App.Notification.Builder.SetContentIntent*) 메서드에 전달 되기 전에 원래 앱의 활동과 연결 됩니다. 플래그 `PendingIntentFlags.OneShot` 는 `PendingIntent` 가 한 번만 `PendingIntent.GetActivity` 사용 되도록 메서드에 전달 됩니다. 이 코드가 실행 되 면 다음과 같은 알림이 표시 됩니다.

![첫 번째 작업 알림](local-notifications-images/10-first-action-notification.png)

이 알림을 누르면 사용자가 원래 활동으로 돌아갑니다.

프로덕션 앱에서는 사용자가 알림 작업 내에서 **뒤로** 단추를 누를 때 앱이 *뒤로 스택을* 처리 해야 합니다 (Android 작업 및 뒤로 스택에 익숙하지 않은 경우 [작업 및 뒤로 스택](https://developer.android.com/guide/components/tasks-and-back-stack.html)참조).
대부분의 경우 알림 작업에서 뒤로 이동 하면 앱에서 사용자를 반환 하 고 홈 화면으로 다시 이동 해야 합니다. 백 스택을 관리 하기 위해 앱은 [taskstackbuilder](xref:Android.App.TaskStackBuilder) 클래스를 사용 하 여 백 스택으로 `PendingIntent` 을 만듭니다.

또 다른 실제적인 고려 사항은 원래 활동에서 알림 활동에 데이터를 보내야 할 수도 있다는 것입니다. 예를 들어 알림에는 문자 메시지가 도착 했음을 나타내고 알림 작업 (메시지 보기 화면)에는 메시지 ID를 사용 하 여 사용자에 게 메시지를 표시할 수 있습니다. 을 `PendingIntent` 만드는 활동은이 데이터가 알림 활동에 전달 되도록 [의도. putextra](xref:Android.Content.Intent.PutExtra*) 메서드를 사용 하 여 데이터 (예: 문자열)를 의도에 추가할 수 있습니다.

다음 코드 샘플에서는를 사용 `TaskStackBuilder` 하 여 백 스택을 관리 하는 방법을 보여 줍니다. 여기에는 라는 `SecondActivity`알림 활동에 단일 메시지 문자열을 보내는 방법에 대 한 예제가 포함 되어 있습니다.

```csharp
// Setup an intent for SecondActivity:
Intent secondIntent = new Intent (this, typeof(SecondActivity));

// Pass some information to SecondActivity:
secondIntent.PutExtra ("message", "Greetings from MainActivity!");

// Create a task stack builder to manage the back stack:
TaskStackBuilder stackBuilder = TaskStackBuilder.Create(this);

// Add all parents of SecondActivity to the stack:
stackBuilder.AddParentStack (Java.Lang.Class.FromType (typeof (SecondActivity)));

// Push the intent that starts SecondActivity onto the stack:
stackBuilder.AddNextIntent (secondIntent);

// Obtain the PendingIntent for launching the task constructed by
// stackbuilder. The pending intent can be used only once (one shot):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    stackBuilder.GetPendingIntent (pendingIntentId, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including
// the pending intent:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my second action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

이 코드 예제에서 앱은 `MainActivity` (위의 알림 코드를 포함 하는) 두 가지 작업으로 구성 되며 `SecondActivity`, 알림을 탭핑 한 후 사용자에 게 표시 되는 화면입니다. 이 코드가 실행 되 면 간단한 알림 (이전 예제와 유사)이 표시 됩니다. 알림을 누르면 사용자가 `SecondActivity` 화면으로 이동 합니다.

![두 번째 활동 스크린샷](local-notifications-images/11-second-activity.png)

문자열 메시지 (의도의 `PutExtra` 메서드로 전달 됨)는 다음 코드 줄을 통해에서 `SecondActivity` 검색 됩니다.

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

위의 스크린샷에 표시 된 것 처럼이 검색 된 메시지 "mainactivity!"의 `SecondActivity` 인사말과 화면에 표시 됩니다. 사용자가 로그인 `SecondActivity`하는 동안 **뒤로** 단추를 누르면 앱이 시작 되 고 앱이 시작 된 후에 다시 화면으로 이동 합니다.

보류 중인 의도를 만드는 방법에 대 한 자세한 내용은 [Pendingintent](xref:Android.App.PendingIntent)를 참조 하세요.

<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>기본 알림 이상

알림은 Android에서 간단한 기본 레이아웃 형식으로 기본 설정 되지만 추가 `NotificationCompat.Builder` 메서드 호출을 통해이 기본 형식을 향상 시킬 수 있습니다. 이 섹션에서는 알림에 긴 사진 아이콘을 추가 하는 방법에 대해 알아보고 확장 된 레이아웃 알림을 만드는 방법에 대 한 예제를 살펴봅니다.

<a name="large-icon-format" />

### <a name="large-icon-format"></a>크게 아이콘 형식

Android 알림은 일반적으로 알림의 왼쪽에 있는 원래 앱의 아이콘을 표시 합니다. 그러나 알림은 표준 작은 아이콘 대신 이미지나 사진 ( *크게 아이콘*)을 표시할 수 있습니다. 예를 들어 메시징 앱은 앱 아이콘이 아닌 보낸 사람의 사진을 표시할 수 있습니다.

다음은 작은 앱 아이콘만 표시 하는 기본 Android &ndash; 5.0 알림 예제입니다.

![예제 일반 알림](local-notifications-images/13-sample-notification.png)

여기에는 커다란 아이콘이 &ndash; 표시 되도록 수정한 후에는 Xamarin 코드 원숭이 이미지에서 만들어진 아이콘이 사용 됩니다.

![예 크게 아이콘 알림](local-notifications-images/14-large-icon-sample.png)

알림이 크게 아이콘 형식으로 표시 되는 경우 작은 앱 아이콘은 크게 아이콘의 오른쪽 아래 모서리에 배지로 표시 됩니다.

알림에서 이미지를 큼 아이콘으로 사용 하려면 알림 작성기의 [SetLargeIcon](xref:Android.App.Notification.Builder.SetLargeIcon*) 메서드를 호출 하 고 이미지의 비트맵을 전달 합니다. 와 달리 `SetSmallIcon`는 비트맵만 허용 합니다. `SetLargeIcon` 비트맵으로 이미지 파일을 변환 하려면 [BitmapFactory](xref:Android.Graphics.BitmapFactory) 클래스를 사용 합니다. 예:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

이 예제 코드는 **Resources/그릴 수 있는/monkey_icon**에서 이미지 파일을 열고, 비트맵으로 변환 하 고, 결과 비트맵을에 `NotificationCompat.Builder`전달 합니다. 일반적으로 원본 이미지 해상도는 작은 아이콘 &ndash; 보다 크지만 크게 크지 않습니다. 이미지가 너무 크면 알림 게시를 지연 시킬 수 있는 불필요 한 크기 조정 작업이 발생할 수 있습니다.

### <a name="big-text-style"></a>큰 텍스트 스타일

*큰 텍스트* 스타일은 알림에서 긴 메시지를 표시 하는 데 사용 하는 확장 된 레이아웃 템플릿입니다. 모든 확장 된 레이아웃 알림과 마찬가지로, 빅 텍스트 알림이 처음에는 컴팩트 프레젠테이션 형식으로 표시 됩니다.

![큰 텍스트 알림 예제](local-notifications-images/15-big-text-notification.png)

이 형식에서는 메시지의 발췌만 표시 되 고 두 개의 마침표로 종료 됩니다. 사용자가 알림을 아래로 끌면 전체 알림 메시지를 표시 하도록 확장 됩니다.

![확장 된 빅 텍스트 알림](local-notifications-images/16-big-text-expanded.png)

이 확장 된 레이아웃 형식에는 알림 하단의 요약 텍스트도 포함 되어 있습니다. *빅 텍스트* 알림의 최대 높이는 256 dp입니다.

*큰 텍스트* 알림을 만들려면 이전과 같은 `NotificationCompat.Builder` 개체를 인스턴스화한 다음, `NotificationCompat.Builder` 개체에 대 한 추가 [textstyle](xref:Android.App.Notification.BigTextStyle) 개체를 인스턴스화하고 추가 합니다. 다음 예를 참조하세요.

```csharp
// Instantiate the Big Text style:
Notification.BigTextStyle textStyle = new Notification.BigTextStyle();

// Fill it with text:
string longTextMessage = "I went up one pair of stairs.";
longTextMessage += " / Just like me. ";
//...
textStyle.BigText (longTextMessage);

// Set the summary text:
textStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (textStyle);

// Create the notification and publish it ...
```

이 예제에서 메시지 텍스트와 요약 텍스트는에 전달 되기 전에 `BigTextStyle` 개체 (`textStyle`)에 저장 됩니다.`NotificationCompat.Builder.`

### <a name="image-style"></a>이미지 스타일

*이미지* 스타일 ( *큰 그림* 스타일이 라고도 함)은 알림 본문에 이미지를 표시 하는 데 사용할 수 있는 확장 된 알림 형식입니다. 예를 들어 스크린샷 앱 또는 사진 앱은 *이미지* 알림 스타일을 사용 하 여 캡처된 마지막 이미지에 대 한 알림을 사용자에 게 제공할 수 있습니다. *이미지* 알림의 최대 높이는 256 dp &ndash; Android는 사용 가능한 메모리 제한 내에서이 최대 높이 제한에 맞게 이미지 크기를 조정 합니다.

모든 확장 된 레이아웃 알림과 마찬가지로 *이미지* 알림은 해당 메시지 텍스트의 발췌를 표시 하는 압축 형식으로 먼저 표시 됩니다.

![압축 이미지 알림에 이미지가 표시 되지 않음](local-notifications-images/17-image-compact.png)

사용자가 *이미지* 알림을 아래로 끌면 이미지를 표시 하도록 확장 됩니다. 예를 들어 다음은 이전 알림의 확장 된 버전입니다.

![확장 된 이미지 알림이 이미지를 표시 합니다.](local-notifications-images/18-image-expanded.png)

알림이 압축 형식으로 표시 되 면 알림 텍스트 (이전에 표시 된 것 처럼 알림 작성기의 `SetContentText` 메서드에 전달 되는 텍스트)가 표시 됩니다. 그러나 이미지를 표시 하도록 알림을 확장 하면 이미지 위에 요약 텍스트가 표시 됩니다.

*이미지* 알림을 만들려면 이전 처럼 `NotificationCompat.Builder` 개체를 인스턴스화한 다음 `NotificationCompat.Builder`개체에 개체에 대 한 [BigPictureStyle](xref:Android.App.Notification.BigPictureStyle)개체를 만들어 삽입합니다. 예를 들어:

```csharp
// Instantiate the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Convert the image to a bitmap before passing it into the style:
picStyle.BigPicture (BitmapFactory.DecodeResource (Resources, Resource.Drawable.x_bldg));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create the notification and publish it ...
```

`NotificationCompat.Builder`의 `SetLargeIcon`메서드와 마찬가지로 `BigPictureStyle`의 [BigPicture](xref:Android.App.Notification.BigPictureStyle.BigPicture*) 메서드는알림의본문에표시하려는이미지의비트맵이필요 합니다. 이 예제에서의 `BitmapFactory` [DecodeResource](xref:Android.Graphics.BitmapFactory.DecodeResource*) 메서드는 **Resources/그릴 수 있는/x_bldg** 에 있는 이미지 파일을 읽고 비트맵으로 변환 합니다.

리소스로 패키지 되지 않은 이미지를 표시할 수도 있습니다. 예를 들어 다음 샘플 코드는 로컬 SD 카드에서 이미지를 로드 하 고 *이미지* 알림에 표시 합니다.

```csharp
// Using the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Read an image from the SD card, subsample to half size:
BitmapFactory.Options options = new BitmapFactory.Options();
options.InSampleSize = 2;
string imagePath = "/sdcard/Pictures/my-tshirt.jpg";
picStyle.BigPicture (BitmapFactory.DecodeFile (imagePath, options));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("Check out my new T-Shirt!");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create notification and publish it ...
```

이 예제에서는 **/sdcard/Pictures/my-tshirt.jpg** 에 있는 이미지 파일이 로드 되 고 원래 크기의 절반으로 크기가 조정 된 다음 알림에 사용할 수 있도록 비트맵으로 변환 됩니다.

![알림에서 티셔츠 이미지 예](local-notifications-images/19-tshirt-notification.png)

미리 이미지 파일의 크기를 모르는 경우에는 예외 처리기 &ndash; `OutOfMemoryError` 에서 [BitmapFactory](xref:Android.Graphics.BitmapFactory.DecodeFile*) 에 대 한 호출을 래핑하는 것이 좋습니다. 이미지가 너무 커서 크기를 조정할 수 없는 경우 예외가 throw 될 수 있습니다.

대량 비트맵 이미지를 로드 하 고 디코딩하는 방법에 대 한 자세한 내용은 [효율적인 대량 비트맵 로드](https://github.com/xamarin/recipes/tree/master/Recipes/android/resources/general/load_large_bitmaps_efficiently)를 참조 하세요.

### <a name="inbox-style"></a>수신함 스타일

*수신함* 형식은 알림 본문에 전자 메일 받은 편지함 요약 등의 별도의 텍스트 줄을 표시 하기 위한 확장 된 레이아웃 템플릿입니다. *받은 편지함* 형식 알림은 먼저 압축 형식으로 표시 됩니다.

![예제 compact 수신함 알림](local-notifications-images/20-inbox-compact.png)

사용자가 알림을 아래로 끌면 아래 스크린샷에 표시 된 대로 확장 되어 전자 메일 요약을 표시 합니다.

![확장 된 예제 수신함 알림](local-notifications-images/21-inbox-expanded.png)

*받은 편지함* 알림을 만들려면 이전과 같은 `NotificationCompat.Builder` 개체를 인스턴스화하고 `NotificationCompat.Builder`에 [InboxStyle](xref:Android.App.Notification.InboxStyle) 개체를 추가 합니다. 다음 예를 참조하세요.

```csharp
// Instantiate the Inbox style:
Notification.InboxStyle inboxStyle = new Notification.InboxStyle();

// Set the title and text of the notification:
builder.SetContentTitle ("5 new messages");
builder.SetContentText ("chimchim@xamarin.com");

// Generate a message summary for the body of the notification:
inboxStyle.AddLine ("Cheeta: Bananas on sale");
inboxStyle.AddLine ("George: Curious about your blog post");
inboxStyle.AddLine ("Nikko: Need a ride to Evolve?");
inboxStyle.SetSummaryText ("+2 more");

// Plug this style into the builder:
builder.SetStyle (inboxStyle);
```

알림 본문에 새 텍스트 줄을 추가 하려면 `InboxStyle` 개체의 [addline](xref:Android.App.Notification.InboxStyle.AddLine*) 메서드를 호출 합니다 ( *수신함* 알림의 최대 높이는 256 dp). *큰 텍스트* 스타일과 달리 *수신함* 스타일은 알림 본문에서 개별 텍스트 줄을 지원 합니다.

또한 개별 텍스트 줄을 확장 된 형식으로 표시 해야 하는 모든 알림에 대해 *수신함* 스타일을 사용할 수 있습니다. 예를 들어, *수신함* 알림 스타일을 사용 하 여 여러 보류 중인 알림을 요약 알림으로 &ndash; 결합할 수 있습니다. 그러면 알림 콘텐츠가 새 줄로 표시 [된 단일 수신함 스타일 알림을 위의 알림을 업데이트](#updating-a-notification) 하는 것은 일반적으로 유사한 새로운 알림의 연속 스트림을 생성 하지 않습니다.

## <a name="configuring-metadata"></a>메타 데이터 구성

`NotificationCompat.Builder`에는 우선 순위, 표시 유형, 범주 등 알림에 대 한 메타 데이터를 설정 하기 위해 호출할 수 있는 메서드가 포함 되어 있습니다. Android에서는 사용자 기본 &mdash; 설정과 &mdash; 함께이 정보를 사용 하 여 알림을 표시 하는 방법과 시기를 결정 합니다.

### <a name="priority-settings"></a>우선 순위 설정

Android 7.1에서 실행 되는 앱은 알림 자체에서 직접 우선 순위를 설정 해야 합니다. 알림의 우선 순위 설정은 알림이 게시 될 때 두 가지 결과를 결정 합니다.

-   다른 알림과 관련 하 여 알림이 표시 되는 위치입니다.
    예를 들어 우선 순위가 높은 알림은 각 알림이 게시 된 시기에 관계 없이 알림 서랍의 낮은 우선 순위 알림 위에 표시 됩니다.

-   알림이 준비 알림 형식 (Android 5.0 이상)에 표시 되는지 여부입니다. *높음* 및 *최대* 우선 순위 알림만 준비 알림으로 표시 됩니다.

Xamarin.ios는 알림 우선 순위를 설정 하기 위해 다음 열거형을 정의 합니다.

-   `NotificationPriority.Max`&ndash; 사용자에 게 긴급 또는 위험 조건 (예: 들어오는 전화, 턴 턴 방향 또는 비상 경고)을 알립니다. Android 5.0 이상 장치에서는 최대 우선 순위 알림이 준비 형식으로 표시 됩니다.

-   `NotificationPriority.High`&ndash; 중요 한 이벤트 (예: 중요 한 전자 메일 또는 실시간 채팅 메시지 도착)를 사용자에 게 알립니다. Android 5.0 이상 장치에서는 높은 우선 순위 알림이 준비 형식으로 표시 됩니다.

-   `NotificationPriority.Default`&ndash; 보통 중요도의 조건을 사용자에 게 알립니다.

-   `NotificationPriority.Low`&ndash; 사용자에 게 알림을 받아야 하는 긴급 하지 않은 정보 (예: 소프트웨어 업데이트 미리 알림 또는 소셜 네트워크 업데이트)

-   `NotificationPriority.Min`&ndash; 사용자가 알림 (예: 위치 또는 날씨 정보)을 볼 때만 확인 하는 배경 정보

알림의 우선 순위를 설정 하려면 `NotificationCompat.Builder` 개체의 [setpriority](xref:Android.App.Notification.Builder.SetPriority*) 메서드를 호출 하 여 우선 순위 수준으로 전달 합니다. 예를 들어:

```csharp
builder.SetPriority (NotificationPriority.High);
```

다음 예에서는 높은 우선 순위 알림 인 "중요 한 메시지!" 알림 서랍의 맨 위에 나타납니다.

![우선 순위가 높은 알림 예제](local-notifications-images/22-hi-priority-drawer.png)

이는 우선 순위가 높은 알림 이므로 Android 5.0에서 사용자의 현재 작업 화면 위에 있는 요약 알림으로도 표시 됩니다.

![예제 헤드 알림](local-notifications-images/23-heads-up-example.png)

다음 예제에서 낮은 우선 순위의 "일"은 우선 순위가 높은 배터리 수준 알림에서 표시 됩니다.

![낮은 우선 순위 알림 예제](local-notifications-images/24-lo-priority-drawer.png)

"일에 대 한 생각" 알림은 낮은 우선 순위의 알림 이므로 Android는이를 준비 형식으로 표시 하지 않습니다.

> [!NOTE]
> Android 8.0 이상에서 알림 채널 및 사용자 설정의 우선 순위에 따라 알림의 우선 순위가 결정 됩니다.

### <a name="visibility-settings"></a>표시 유형 설정

Android 5.0부터 *표시 유형* 설정을 사용 하 여 보안 잠금 화면에 표시 되는 알림 콘텐츠의 크기를 제어할 수 있습니다.
Xamarin Android는 알림 표시 여부를 설정 하기 위해 다음 열거형을 정의 합니다.

-   `NotificationVisibility.Public`&ndash; 알림의 전체 콘텐츠가 보안 잠금 화면에 표시 됩니다.

-   `NotificationVisibility.Private`&ndash; 중요 한 정보만 보안 잠금 화면에 표시 됩니다 (예: 알림 아이콘 및 게시 된 앱의 이름). 그러나 나머지 알림의 세부 정보는 숨겨집니다. 모든 알림은 기본적으로 `NotificationVisibility.Private`입니다.

-   `NotificationVisibility.Secret`&ndash; 알림 아이콘이 아니라 보안 잠금 화면에 아무 것도 표시 되지 않습니다. 알림 콘텐츠는 사용자가 장치를 잠금 해제 한 후에만 사용할 수 있습니다.

알림 표시 여부를 설정 하기 위해 앱은 `SetVisibility` `NotificationCompat.Builder` 개체의 메서드를 호출 하 여 표시 유형 설정을 전달 합니다. 예를 들어를 `SetVisibility` 호출 하면 알림이 `Private`수행 됩니다.

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

`Private` 알림이 게시 되 면 앱의 이름과 아이콘만 보안 잠금 화면에 표시 됩니다. 사용자는 알림 메시지 대신 "이 알림을 볼 수 있도록 장치 잠금 해제"를 확인 합니다.

![장치 알림 메시지 잠금 해제](local-notifications-images/25-lockscreen-private.png)

이 예제에서 **Notificationslab 래 브** 는 원래 앱의 이름입니다. 이 교정 된 버전의 알림은 잠금 화면이 안전 하 게 보호 되는 경우 (즉, PIN, 패턴 또는 암호를 통해 보안 됨) &ndash; 잠금 화면에 안전 하지 않은 경우 잠금 화면에서 전체 알림 콘텐츠를 사용할 수 있는 경우에만 표시 됩니다.

### <a name="category-settings"></a>범주 설정

Android 5.0 부터는 미리 정의 된 범주를 순위 및 필터링 알림에 사용할 수 있습니다. Xamarin Android는 이러한 범주에 대해 다음과 같은 열거형을 제공 합니다.

-   `Notification.CategoryCall`&ndash; 들어오는 전화 통화입니다.

-   `Notification.CategoryMessage`&ndash; 들어오는 문자 메시지입니다.

-   `Notification.CategoryAlarm`&ndash; 경보 조건 또는 타이머 만료입니다.

-   `Notification.CategoryEmail`&ndash; 받는 전자 메일 메시지입니다.

-   `Notification.CategoryEvent`&ndash; 일정 이벤트입니다.

-   `Notification.CategoryPromo`&ndash; 프로 모션 메시지 또는 광고.

-   `Notification.CategoryProgress`&ndash; 백그라운드 작업의 진행률입니다.

-   `Notification.CategorySocial`&ndash; 소셜 네트워킹 업데이트.

-   `Notification.CategoryError`&ndash; 백그라운드 작업이 나 인증 프로세스가 실패 했습니다.

-   `Notification.CategoryTransport`&ndash; 미디어 재생 업데이트.

-   `Notification.CategorySystem`&ndash; 시스템에서 사용 하도록 예약 되었습니다 (시스템 또는 장치 상태).

-   `Notification.CategoryService`&ndash; 백그라운드 서비스가 실행 중임을 나타냅니다.

-   `Notification.CategoryRecommendation`&ndash; 현재 실행 중인 앱과 관련 된 권장 사항 메시지입니다.

-   `Notification.CategoryStatus`&ndash; 장치에 대 한 정보입니다.

알림이 정렬 되 면 알림 우선 순위가 해당 범주 설정 보다 우선 적용 됩니다. 예를 들어 우선 순위가 높은 알림은 `Promo` 범주에 속한 경우에도 준비로 표시 됩니다. 알림의 범주를 설정 하려면 `SetCategory` `NotificationCompat.Builder` 개체의 메서드를 호출 하 여 범주 설정을 전달 합니다. 예를 들어:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

사용 *안 함* 기능 (Android 5.0의 새로운 기능)은 범주를 기반으로 알림을 필터링 합니다. 예를 들어 **설정** 의 *방해* 금지 화면에서 사용자는 전화 통화 및 메시지에 대 한 알림을 제외할 수 있습니다.

![화면 스위치를 방해 하지 않음](local-notifications-images/26-do-not-disturb.png)

위의 스크린샷에 나와 있는 것 처럼 사용자가 전화 통화를 제외 하 고 모든 인터럽트를 차단 하도록 *방해* 금지를 구성 하는 경우 Android에서 범주가로 설정 `Notification.CategoryCall` 된 알림이 표시 되도록 허용 합니다.  *방해* 모드입니다. 알림은 방해 금지 모드에서 차단 되지 않습니다. `Notification.CategoryAlarm`

[Localnotifications](https://docs.microsoft.com/samples/xamarin/monodroid-samples/localnotifications) 샘플에서는를 사용 `NotificationCompat.Builder` 하 여 알림에서 두 번째 활동을 시작 하는 방법을 보여 줍니다. 이 샘플 코드는 [xamarin.ios 연습의 로컬 알림 사용](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) 에서 설명 합니다.

### <a name="notification-styles"></a>알림 스타일

를 사용 `NotificationCompat.Builder`하 여 *빅 텍스트*, *이미지*또는 *수신함* 스타일 알림을 만들려면 앱에서 이러한 스타일의 호환성 버전을 사용 해야 합니다. 예를 들어, *큰 텍스트* 스타일을 사용 하려면 다음 `NotificationCompat.BigTextstyle`을 인스턴스화합니다.

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

마찬가지로 `NotificationCompat.InboxStyle` 앱은 각각 및 `NotificationCompat.BigPictureStyle` 에 *수신함* 및 *이미지* 스타일을 사용할 수 있습니다.

### <a name="notification-priority-and-category"></a>알림 우선 순위 및 범주

`NotificationCompat.Builder`메서드 ( `SetPriority` Android 4.1부터 사용 가능)를 지원 합니다. 그러나 범주는 Android 5.0에 도입 된 `NotificationCompat.Builder` 새 알림 메타 데이터 시스템에 포함 되기 때문에에서는 메서드가지원되지않습니다.`SetCategory`

을 사용할 수 없는 이전 버전의 Android `SetCategory` 를 지원 하려면 api 수준이 Android 5.0 (api 레벨 21) 보다 크거나 같은 경우 `SetCategory` 코드에서 런타임에 api 수준을 검사 하 여 조건적으로 호출할 수 있습니다.

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

이 예제에서는 앱의 **대상 프레임 워크가** android 5.0로 설정 되 고 **최소 android 버전이** **android 4.1 (API 수준 16)** 로 설정 됩니다. 는 `SetCategory` api 수준 21 이상에서 사용할 수 있기 때문에이 예제 코드는 `SetCategory` `SetCategory` 사용 가능한 &ndash; 경우에만를 호출 하 고 api 수준이 21 보다 작은 경우에는를 호출 하지 않습니다.

### <a name="lock-screen-visibility"></a>잠금 화면 표시 유형

Android 5.0 (API 레벨 21) 이전에는 android에서 화면 잠금 알림을 지원 하지 `NotificationCompat.Builder` 않으므로에서이 메서드 `SetVisibility` 를 지원 하지 않습니다. 에 대해 `SetCategory`위에서 설명한 것 처럼 코드는 런타임에 API 수준을 확인 하 고 사용 가능한 `SetVisiblity` 경우에만 호출할 수 있습니다.

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop) {
    builder.SetVisibility (Notification.Public);
}
```

## <a name="summary"></a>요약

이 문서에서는 Android에서 로컬 알림을 만드는 방법을 설명 했습니다. 알림 분석에 대해 설명 하 고 `NotificationCompat.Builder` ,을 사용 하 여 알림을 만드는 방법, 큰 아이콘의 알림 스타일 지정 방법, *빅 텍스트*, *이미지* 및 *수신함* 형식, 등의 알림 메타 데이터 설정을 설정 하는 방법에 대해 설명 했습니다. 우선 순위, 표시 유형 및 범주와 알림에서 작업을 시작 하는 방법을 설명 합니다. 또한이 문서에서는 Android 5.0에 도입 된 새로운 준비, 잠금 화면 및 *방해 금지* 기능에서 이러한 알림 설정이 작동 하는 방식을 설명 했습니다. 마지막으로를 사용 `NotificationCompat.Builder` 하 여 이전 버전의 Android와의 호환성을 유지 하는 방법을 배웠습니다.

Android에 대 한 알림을 디자인 하는 방법에 대 한 지침은 [알림](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [NotificationsLab 래 브 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-notificationslab)
- [LocalNotifications (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/localnotifications)
- [Android의 로컬 알림 연습](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [사용자에 게 알림](https://developer.android.com/training/notify-user/index.html)
- [알림](xref:Android.App.Notification)
- [NotificationManager](xref:Android.App.NotificationManager)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](xref:Android.App.PendingIntent)
