---
title: 로컬 알림
description: 이 섹션에는 Xamarin.Android에서 로컬 알림을 구현 하는 방법을 보여 줍니다. Android 알림 다양 한 UI 요소에 설명 하 고 API에 설명의 만들기 및 알림을 표시를 사용 하 여 관련 됩니다.
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/16/2018
ms.openlocfilehash: 362041efc5a19dfb70430054f3e4636d4fdfbd7e
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672752"
---
<a name="compatibility"></a>

# <a name="local-notifications"></a>로컬 알림

_이 섹션에는 Xamarin.Android에서 로컬 알림을 구현 하는 방법을 보여 줍니다. Android 알림 다양 한 UI 요소에 설명 하 고 API에 설명의 만들기 및 알림을 표시를 사용 하 여 관련 됩니다._

## <a name="local-notifications-overview"></a>로컬 알림 개요

Android는 사용자에 게 알림 아이콘 및 알림 정보를 표시 하는 것에 대 한 시스템 제어 방식의 두 가지 영역을 제공 합니다. 먼저 알림을 게시, 해당 아이콘에 표시 됩니다는 *알림 영역*다음 스크린샷에 표시 된 것 처럼:

![장치에서 알림 영역 예제](local-notifications-images/01-notification-shade.png)

알림에 대 한 세부 정보를 가져오려면 사용자 (각 알림 아이콘 표시 알림 콘텐츠 확장)이 표시 되는 알림 서랍을 열 수는 알림과 관련 된 모든 작업을 수행 하 고 있습니다. 다음 스크린 샷에 표시 된 *알림 서랍* 위에 표시 알림 영역에 해당 하는:

[![세 가지 알림을 표시 하는 예제 알림 서랍](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Android 알림을 두 종류의 레이아웃을 사용합니다.

-   ***기본 레이아웃*** &ndash; compact, 고정 표시 형식입니다.

-   ***확장 된 레이아웃*** &ndash; 자세한 정보를 표시 하기 위해 더 큰 크기로 확장할 수 있는 프레젠테이션 형식입니다.

이러한 각 유형의 레이아웃 (및 만드는 방법) 다음 섹션에 설명 되어 있습니다.

> [!NOTE]
> 이 가이드에 중점을 두고 합니다 [NotificationCompat Api](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) 에서 합니다 [Android 지원 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)합니다. 이러한 Api는 역호환성 최대 Android 4.0 (API 수준 14).


### <a name="base-layout"></a>기본 레이아웃

모든 Android 알림 최소한 다음과 같은 요소를 포함 하는 기본 레이아웃 형식에 빌드됩니다.

1.  A *알림 아이콘*, 앱에 다양 한 유형의 알림 지 원하는 경우 원래 앱 또는 알림 유형을 나타내는입니다.

2.  알림을 *title*, 또는 경우 개인 메시지를 보낸 사람의 이름입니다.

3.  알림 메시지입니다.

4.  A *타임 스탬프*합니다.

이러한 요소는 다음 다이어그램과 같이 표시 됩니다.

[![알림 요소의 위치](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

기본 레이아웃 높이 64 밀도 독립적 픽셀 (dp)으로 제한 됩니다. Android는 기본적으로이 기본 알림 스타일을 만듭니다.

필요에 따라 알림 응용 프로그램 또는 보낸 사람의 사진을 나타내는 큰 아이콘을 표시할 수 있습니다. 큰 아이콘이 알림을 Android 5.0 이상에서 사용할 작은 알림 아이콘을 큰 아이콘 배지로 표시 됩니다.

![간단한 알림 사진](local-notifications-images/04-simple-notification-photo.png)

Android 5.0부터, 알림 나타날 수도 있습니다 잠금 화면에서:

[![잠금 화면 알림 예제](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

사용자 수 두 번 눌러서 잠금 화면 알림 장치를 잠금 해제 하 고 해당 알림을 발생 하는 앱으로 이동 하거나 안쪽으로 살짝 밀어 알림을 해제 합니다. 앱 잠금 화면에 보이는 것을 제어 하는 알림의 표시 수준을 설정할 수 있습니다 하 고 사용자의 화면 잠금 알림을 표시할 중요 한 콘텐츠를 허용할 것인지를 선택할 수 있습니다.

Android 5.0 호출 우선 순위가 높은 알림 표시 형식을 도입 *헤 즈 업*합니다. 헤 즈 업 알림 몇 초간 화면 맨 위에서 아래로 슬라이드 및 알림 영역에 백업 후퇴 후:

[![예제 heads-up 알림](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

헤 즈 업 알림 수 있도록 시스템 현재 실행 중인 활동의 상태를 중단 하지 않고 사용자 앞에 중요 한 정보를 배치 하는 UI에 대 한 합니다.

Android는 알림을 정렬 하 고 지능적으로 표시 될 수 있도록 알림 메타 데이터에 대 한 지원을 포함 합니다. 알림 메타 데이터는 또한 헤드업 형식 및 잠금 화면에서 알림 표시 되는 방식을 제어 합니다. 응용 프로그램 다음과 같은 유형의 알림 메타 데이터를 설정할 수 있습니다.

-   **우선 순위** &ndash; 우선 순위 수준을 알림은 제공 된 방법 및 시기를 결정 합니다. 예를 들어에 Android 5.0에서 우선 순위가 높은 알림 헤드업 알림으로 표시 됩니다.

-   **표시 여부** &ndash; 알림이 잠금 화면에 나타날 때 표시 될 알림 콘텐츠의 양을 지정 합니다.

-   **범주** &ndash; 시스템에 장치가 표시 되는 경우와 같은 다양 한 상황에서 알림 메시지를 처리 하는 방법에 알립니다 *방해 금지* 모드입니다.

**참고:** **표시 유형** 하 고 **범주** 이전 버전의 Android에서 사용할 수 없는 되며 Android 5.0에서 도입 되었습니다. Android 8.0부터 [알림 채널](#notif-chan) 알림을 사용자에 게 표시 되는 방식을 제어 하는 데 사용 됩니다.


### <a name="expanded-layouts"></a>확장 된 레이아웃

Android 4.1 부터는 알림은 자세한 내용을 보려면 알림의 높이 확장 하는 데 사용할 수 있는 확장 된 레이아웃 스타일을 사용 하 여 구성할 수 있습니다. 예를 들어, 다음 예제에서는 계약 모드로 확장 된 레이아웃 알림을 보여 줍니다.

![계약 된 알림](local-notifications-images/07-contracted-notification.png)

이 알림은 확장 되 면 전체 메시지가 표시 됩니다.

![확장 된 알림](local-notifications-images/08-expanded-notification.png)

Android에서는 단일 이벤트 알림에 대 한 세 가지 확장된 레이아웃 스타일을 지원합니다.

-   ***큰 텍스트*** &ndash; 계약 모드로 뒤에 마침표 두 개 메시지의 첫 번째 줄의 일부를 표시 합니다. 확장 된 모드에 (처럼 위의 예에서) 전체 메시지를 표시 합니다.

-   ***받은 편지함*** &ndash; 계약된 모드에서 새 메시지의 수를 표시 합니다. 확장 된 모드에서 첫 번째 전자 메일 메시지 또는 받은 편지함에서 메시지 목록을 표시합니다.

-   ***이미지*** &ndash; 계약 모드로 메시지 텍스트를 표시 합니다. 확장 된 모드에서 텍스트 및 이미지를 표시합니다.

[기본 알림 넘어](#beyond-the-basic-notification) (이 문서의 뒷부분에서)를 만드는 방법을 설명 *큰 텍스트*를 *수신함*, 및 *이미지* 알림.

<a name="notif-chan"></a>
<a name="notification-channels"></a>
## <a name="notification-channels"></a>알림 채널

Android 8.0 oreo (용)부터 사용할 수는 *알림 채널* 알림 표시 하려는 각 형식에 대 한 사용자 지정 가능한 채널을 만드는 기능입니다. 알림 채널 수 있도록 하기 그룹 알림을 동작은 채널 별첨에 모든 알림 게시 되도록 합니다. 예를 들어 해야 즉각적인 주의가 필요한 알림을 위한 알림 채널 및 정보 메시지에 사용 되는 별도 "작게" 채널을 합니다.

합니다 **YouTube** Android Oreo를 사용 하 여 설치 된 앱에는 두 알림 범주를 나열 합니다. **다운로드 알림** 하 고 **일반 알림**:

[![Android Oreo에서 YouTube에 대 한 알림 화면](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

이러한 각 범주는 알림 채널에 해당합니다. YouTube 앱을 구현 하는 한 **다운로드 알림을** 채널 및 **일반 알림** 채널입니다. 사용자가 탭 **알림을 다운로드**, 알림 채널을 다운로드 하는 앱에 대 한 설정 화면을 표시 하는:

[![YouTube 앱에 대 한 알림 화면 다운로드](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

이 화면에서 사용자의 동작을 수정할 수는 **다운로드** 다음을 수행 하 여 알림 채널:

-   중요도 수준을 설정 **Urgent**, **높은**를 **보통**, 또는 **낮은**, 사운드 및 시각적 중단 수준을 구성 하는 합니다.

-   알림 점을 켜거나 끕니다.

-   깜박이 표시등을 켜거나 끕니다.

-   잠금 화면에 알림을 표시할지 설정 합니다.

-   재정의 된 **방해 금지** 설정 합니다.

합니다 **일반 알림** 채널에 유사한 설정:

[![YouTube 앱에 대 한 일반 알림 화면](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

공지 알림 채널 사용자 상호 작용 하는 방법을 제어할 절대 없습니다 &ndash; 위의 스크린샷에서 볼 수 있듯이 사용자가 장치에서 모든 알림 채널에 대 한 설정을 수정할 수 있습니다. 그러나 (아래와 같이 수) 기본값을 구성할 수 있습니다. 이 예제에서 알 수 있듯이 새 알림 채널 기능을 사용 하면 세부적으로 제어할 다른 종류의 알림이 사용자에 게 제공할 수 있습니다.


## <a name="notification-creation"></a>알림 만들기

Android에서 알림을 만들려면를 사용 합니다 [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder) 에서 클래스를 [Xamarin.Android.Support.v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet 패키지. 이 클래스를 사용 하면 이전 버전의 Android에 대 한 알림을 게시할 수 있습니다. 사용에 대 한 자세한 내용은 `NotificationCompat.Builder`를 참조 하세요 [호환성](#compatibility) 이 항목의 뒷부분에 나오는.

`NotificationCompat.Builder` 알림을에서 같은 다양 한 옵션을 설정 하기 위한 메서드를 제공 합니다.

-   콘텐츠를 제목, 메시지 텍스트 및 알림 아이콘을 포함 합니다.

-   알림의 스타일과 같은 *큰 텍스트*를 *수신함*, 또는 *이미지* 스타일입니다.

-   알림의 우선 순위: 최소 낮은 기본적으로 높거나 최대입니다. 우선 순위를 통해 설정 됩니다 Android 8.0 이상에 [ _알림 채널_](#notification-channels)합니다.

-   잠금 화면에서 알림 표시 유형: 공용, 개인 또는 암호입니다.

-   Android는 데 도움이 되는 범주 메타 데이터 분류 및 알림을 필터링 합니다.

-   알림을 탭 할 때 시작 하기 위한 작업을 나타내는 선택적 의도 합니다.

-   (Android 8.0 이상)에서 알림을 게시 한다는 알림 채널의 ID입니다.

작성기에서 이러한 옵션을 설정한 후 설정을 포함 하는 알림 개체를 생성 합니다. 공지를 게시 하려면이 알림 개체를 전달 합니다 *알림 관리자*합니다. Android가 제공 합니다 [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/) 알림을 게시 하 고 사용자에 게 표시 하는 일을 담당 하는 클래스입니다. 작업 또는 서비스와 같은 모든 컨텍스트를이 클래스에 대 한 참조를 가져올 수 있습니다.


### <a name="creating-a-notification-channel"></a>알림 채널

Android 8.0에서 실행 되는 앱의 알림에 대 한 알림 채널을 만들어야 합니다. 알림 채널에는 다음 세 가지를 정보가 필요합니다.

* 채널을 식별 하는 패키지에 고유한 ID 문자열입니다.
* 사용자에 게 표시 되는 채널의 이름입니다.  이름은 1에서 40 사이 해야 문자입니다.
* 채널의 중요도입니다.

앱을 실행 중인 Android의 버전을 확인 해야 합니다.
Android 8.0 보다 오래 된 버전을 실행 하는 장치에 알림 채널을 만들지 마십시오. 다음 메서드는 작업의 알림 채널을 만드는 방법의 예로:

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

활동이 만들어질 때마다 알림 채널을 만들어야 합니다. 에 대 한 합니다 `CreateNotificationChannel` 메서드를 호출 해야는 `OnCreate` 활동의 메서드.

### <a name="creating-and-publishing-a-notification"></a>만들기 및 게시는 알림

Android에서 알림을 생성 하려면 다음이 단계를 수행 합니다.

1.  인스턴스화하는 `NotificationCompat.Builder` 개체입니다.

2.  다양 한 메서드를 호출 합니다 `NotificationCompat.Builder` 알림 옵션을 설정할 개체입니다.

3.  호출 된 [빌드](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.Build/) 메서드의 `NotificationCompat.Builder` 알림 개체를 인스턴스화하는 개체입니다.

4.  호출 된 [알릴](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/(System.Int32%2cAndroid.App.Notification)) 알림을 게시할 알림 관리자의 메서드.

최소한 각 알림에 대해 다음 정보를 제공 해야 합니다.

-   작은 아이콘 (24 x 24 크기가 dp)

-   간단한 타이틀

-   알림의 텍스트

다음 코드 예제를 사용 하는 방법을 보여 줍니다 `NotificationCompat.Builder` 기본 알림을 생성 하도록 합니다. 있음을 `NotificationCompat.Builder` 메서드를 지원 [메서드 체인](https://en.wikipedia.org/wiki/Method_chaining); 메서드 호출 다음에 대 한 마지막 메서드 호출의 결과 사용할 수 있도록 각 메서드는 작성기 개체를 반환 하는:

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

이 예제에서는 새 `NotificationCompat.Builder` 라는 개체 `builder` 사용할 알림 채널의 ID와 함께 인스턴스화됩니다. 제목 및 텍스트 알림의 설정 하 고 알림 아이콘에서 로드 되 **Resources/drawable/ic_notification.png**합니다. 알림 작성기에 대 한 호출 `Build` 메서드는 이러한 설정을 사용 하 여 알림 개체를 만듭니다. 다음 단계를 호출 하는 것은 `Notify` 알림 관리자의 메서드. 호출 알림 관리자를 찾으려면 `GetSystemService`위와 같이 합니다.

`Notify` 메서드는 두 매개 변수: 알림 식별자 및 알림 개체입니다. 알림 식별자에는 응용 프로그램에 알림을 식별 하는 고유 정수입니다. 이 예제에서는 알림 식별자 (0); 0으로 설정 됩니다. 그러나 프로덕션 응용 프로그램에서는 하려는 각 알림 고유한 식별자를 지정 합니다. 이전 식별자 값에 대 한 호출에서 다시 사용 `Notify` 마지막 알림을 덮어쓸 수 하 게 합니다.

Android 5.0 장치에서이 코드를 실행 하는 경우 다음 예제와 같이 알림을 생성 합니다.

![샘플 코드에 대 한 알림 결과](local-notifications-images/09-hello-world.png)

알림 아이콘이 알림 왼쪽에 표시 됩니다 &ndash; 이 이미지를 원 안의 &ldquo;있나요&rdquo; 알파 채널은 Android 뒤 순환 배경이 회색을 그릴 수 있도록 합니다. 알파 채널이 없는 아이콘을 제공할 수도 있습니다. 사진 이미지 아이콘으로 표시할 참조 [큰 아이콘 형식을](#large-icon-format) 이 항목의에서 뒷부분에 있습니다.

타임 스탬프를 자동으로 설정 되어 있지만 호출 하 여이 설정을 재정의할 수 있습니다 합니다 [SetWhen](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetWhen/) 알림 작성기의 메서드. 예를 들어, 다음 코드 예제에서는 현재 시간 타임 스탬프를 설정 합니다.

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>사운드 및 진동을 사용 하도록 설정

또한 소리를 재생 하 여 알림을 원한다 면 알림 작성기를 호출할 수 있습니다 [SetDefaults](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetDefaults/) 메서드를 전달 합니다 `NotificationDefaults.Sound` 플래그:

```csharp
// Instantiate the notification builder and enable sound:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

이 호출으로 `SetDefaults` 알림이 게시 될 때 소리를 재생 하는 장치에 발생 합니다. 소리를 재생 하는 것이 아니라 진동 하도록 하려면 장치를 원하는 경우 전달할 수 있습니다 `NotificationDefaults.Vibrate` 하 `SetDefaults.` 소리와 장치를 진동 장치를 원한다 면 두 플래그를 전달할 수 있습니다 `SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

소리를 재생할 소리를 지정 하지 않고 사용 하도록 설정한 경우 Android 기본 시스템 알림 소리를 사용 합니다. 그러나 알림 작성기를 호출 하 여 재생할 사운드를 변경할 수 있습니다 [SetSound](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetSound/p/Android.Net.Uri/) 메서드. 예를 들어, 경보 재생 (대신 기본 알림 소리), 여 알림과 함께 사운드 얻을 수 있습니다 URI 알람에서 사운드를 [RingtoneManager](https://developer.xamarin.com/api/type/Android.Media.RingtoneManager/) 에 전달 `SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

또는 사용할 수는 시스템 기본 벨소리 사운드 알림에:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

알림 개체에 알림 속성을 설정할 수는 알림 개체를 만든 후 (통해 미리 구성 하지 않고 `NotificationCompat.Builder` 메서드). 예를 들어, 호출 하는 대신 합니다 `SetDefaults` 알림 한 진동을 사용 하도록 설정 하는 방법 알림의 비트 플래그를 직접 수정할 수 있습니다 [기본적으로](https://developer.xamarin.com/api/property/Android.App.Notification.Defaults/) 속성:

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

장치를에 알림이 게시 되 면 진동 하는이 예제가입니다.

<a name="updating-a-notification" />

### <a name="updating-a-notification"></a>알림을 업데이트합니다.

이 게시 된 후에 알림의 콘텐츠를 업데이트 하려는 경우 다시 사용할 수 있습니다 기존 `NotificationCompat.Builder` 새 알림 개체를 만들고 마지막 알림 식별자를 사용 하 여이 알림을 게시 하는 개체입니다. 예를 들어:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

이 예제에서는 기존 `NotificationCompat.Builder` 개체는 다양 한 제목 및 메시지를 사용 하 여 새 알림 개체를 만드는 데 사용 됩니다.
이전 알림 식별자를 사용 하 여 새 알림 개체를 게시 하 고 이전에 게시 알림 콘텐츠 업데이트:

![업데이트 알림](local-notifications-images/12-updated-notification.png)

이전 알림의 본문은 다시 &ndash; 만 제목 및 텍스트 알림을 알림 서랍에 표시 되는 동안 알림이 변경입니다. "샘플 알림"에서 "업데이트 알림" 제목 텍스트를 변경 하 고 "Hello World에서 메시지 텍스트를 변경! 내 첫 번째 알림입니다. " "이이 메시지를 변경 합니다."를

다음 중 하나의 해당할 때까지 알림이 계속 표시 됩니다.

-   알림을 해제 하는 사용자 (또는 탭 *모두 지우기*).

-   응용 프로그램에 대 한 호출을 통해 `NotificationManager.Cancel`알림을 게시 된 경우 할당 된 고유 알림 ID에 전달 합니다.

-   응용 프로그램이 호출 `NotificationManager.CancelAll`합니다.

Android 알림을 업데이트 하는 방법에 대 한 자세한 내용은 참조 하세요 [알림을 수정](https://developer.android.com/training/notify-user/managing.html#Updating)합니다.


### <a name="starting-an-activity-from-a-notification"></a>알림을에서 작업 시작

Android에서 일반적으로 사용 하 여 연결에 대 한 알림을는 *동작* &ndash; 사용자가 알림을 탭 하면 시작 되는 활동입니다. 이 작업은 다른 작업 또는 다른 응용 프로그램에 있을 수 있습니다. 만든 작업 알림을 추가할를 [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/) 개체와 연결 합니다 `PendingIntent` 알림을 사용 하 여 합니다. `PendingIntent` 받는 사람 응용 프로그램 코드의 미리 정의 된 일부 보내는 응용 프로그램의 권한으로 실행을 허용 하는 의도 특수 한 형식입니다. Android 시작 하 여 지정 된 활동 사용자가 알림을 누르면는 `PendingIntent`합니다.

다음 코드 조각에 포함 된 알림을 만드는 방법을 보여 주는 `PendingIntent` 원래 앱의 활동 시작 됩니다 `MainActivity`:

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

이 코드는 점을 제외 하면 이전 섹션에서 알림 코드와 매우 비슷합니다는 `PendingIntent` 알림 개체에 추가 됩니다. 이 예제는 `PendingIntent` 알림 작성기에 전달 되기 전에 원래 앱의 활동을 사용 하 여 연결 된 [SetContentIntent](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetContentIntent/) 메서드. 합니다 `PendingIntentFlags.OneShot` 플래그에 전달 되는 `PendingIntent.GetActivity` 메서드 있도록는 `PendingIntent` 한 번만 사용 됩니다. 이 코드를 실행 하는 경우 다음 알림이 표시 됩니다.

![첫 번째 작업 알림](local-notifications-images/10-first-action-notification.png)

원래 활동으로 사용자는이 알림을 탭 합니다.

앱을 프로덕션 앱에서 처리 해야 합니다는 *백 스택에* 를 누를 때 합니다 **다시** 알림 작업 내에서 단추 (Android 작업과 백 스택에 잘 모르는 경우 참조 [ 작업 및 뒤로 스택](https://developer.android.com/guide/components/tasks-and-back-stack.html)).
대부분의 경우에서 알림 작업에서 뒤로 이동 하면 사용자를 앱 외부로 및 홈 화면으로 반환 해야 합니다. 앱이 사용 하는 백 스택에 관리 하는 [TaskStackBuilder](https://developer.xamarin.com/api/type/Android.App.TaskStackBuilder/) 만들 클래스를 `PendingIntent` 백 스택을 사용 하 여 합니다.

다른 실제 고려 사항은 원래 활동 알림 작업에 데이터를 전송 해야 할 수입니다. 예를 들어, 알림 텍스트 메시지가 도착 했는지를 사용자에 게 메시지를 표시 하는 메시지의 ID를 필요로 하는 알림 작업 (보기 화면 메시지)을 나타낼 수 있습니다. 만드는 작업을 `PendingIntent` 사용할 수는 [Intent.PutExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.PutExtra/p/System.String/System.String/) 이 데이터는 알림 작업에 전달 되도록 의도 (예를 들어, 문자열) 데이터를 추가 하는 방법입니다.

다음 코드 샘플을 사용 하는 방법을 보여 줍니다 `TaskStackBuilder` 백 스택에 하며 관리 하 라는 알림 작업에는 단일 메시지 문자열을 보내는 방법의 예가 포함 되어 있습니다. `SecondActivity`:

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

이 코드 예제에서는 두 가지 활동으로 구성 앱: `MainActivity` (위의 알림 코드 포함), 및 `SecondActivity`, 화면 알림을 탭 한 후 사용자에 게 표시 합니다. 이 코드를 실행 하는 경우 (이전 예제와 유사) 간단한 알림이 표시 됩니다. 알림 탭에 사용자를 안내 합니다 `SecondActivity` 화면:

![두 번째 활동 스크린 샷](local-notifications-images/11-second-activity.png)

문자열 메시지 (의도 전달할 `PutExtra` 메서드)에서 검색 됩니다 `SecondActivity` 코드이 줄을 통해:

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

이 검색된 메시지를 "Greetings에서 MainActivity!,"에 표시 됩니다는 `SecondActivity` 위의 스크린샷에 나와 있는 것 처럼 화면. 누를 때를 **다시** 하는 동안 단추 `SecondActivity`, 탐색을 앱 외부로 및 앱의 출시 이전 화면으로 이어집니다.

인 텐트 보류 중인 만들기에 대 한 자세한 내용은 참조 하세요. [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)합니다.


<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>기본 알림 초과

Android에서 간단한 기본 레이아웃 형식으로 알림을 기본적 이지만이 기본 형식으로 추가 하 여 향상 시킬 수 있습니다 `NotificationCompat.Builder` 메서드 호출 합니다. 이 섹션에서는 큰 사진 아이콘에 알림 추가 하는 방법을 알아봅니다 및 확장 된 레이아웃 알림을 만드는 방법의 예제를 볼 수 있습니다.

<a name="large-icon-format" />

### <a name="large-icon-format"></a>큰 아이콘 형식

일반적으로 android 알림 (알림 왼쪽)에 원래 앱의 아이콘을 표시 합니다. 이미지 또는 사진의 알림을 표시할 수 있지만 (한 *큰 아이콘*) 표준 작은 아이콘 대신 합니다. 예를 들어, 메시징 앱을 사진 앱 아이콘 대신 보낸 사람에 게 표시할 수 있습니다.

기본 Android 5.0 알림의 예로 &ndash; 작은 앱 아이콘에만 표시 됩니다.

![예제에서는 일반 알림](local-notifications-images/13-sample-notification.png)

다음은 스크린샷입니다 큰 아이콘을 표시 하는 수정 후 알림 및 &ndash; Xamarin 코드 monkey의 이미지에서 만들어진 아이콘 사용:

![큰 아이콘 알림 예제](local-notifications-images/14-large-icon-sample.png)

알림을 큰 아이콘으로 표시 되 면 작은 앱 아이콘 배지를 큰 아이콘의 오른쪽 아래 모서리에서으로 표시 되는지 확인 합니다.

알림 작성기의 알림의 큰 아이콘으로 이미지를 사용 하려면 호출 [SetLargeIcon](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetLargeIcon/) 메서드 및 비트맵 이미지의 전달 합니다. 와 달리 `SetSmallIcon`, `SetLargeIcon` 비트맵을 허용 합니다. 비트맵으로 이미지 파일로 변환 하려면 사용 합니다 [BitmapFactory](https://developer.xamarin.com/api/type/Android.Graphics.BitmapFactory/) 클래스입니다. 예를 들어:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

이 예제 코드에서 이미지 파일을 엽니다 **Resources/drawable/monkey_icon.png**을 비트맵으로 변환 하 고 결과 비트맵을 전달 `NotificationCompat.Builder`합니다. 일반적으로 원본 이미지 해상도 작은 아이콘 보다 큰 &ndash; 훨씬 더 않고 합니다. 너무 큰 이미지에는 알림 게시를 지연 시킬 수 있는 불필요 한 크기 조정 작업이 발생할 수 있습니다.


### <a name="big-text-style"></a>큰 텍스트 스타일

합니다 *큰 텍스트* 스타일은 긴 메시지 알림에 표시에 사용 하는 확장 된 레이아웃 템플릿입니다. 모든 확장 된 레이아웃 알림과 같은 큰 텍스트 알림 compact 표시 형식에 처음에 표시 됩니다.

![예제 큰 텍스트 알림](local-notifications-images/15-big-text-notification.png)

이 형식으로 두 마침표로 종료 메시지의 발췌만 표시 됩니다. 사용자가 알림을 끌면, 전체 알림 메시지를 표시 하려면 확장 합니다.

![확장 된 큰 텍스트 알림](local-notifications-images/16-big-text-expanded.png)

이 확장 된 레이아웃 형식에는 요약 텍스트 알림의 맨 아래에 포함 됩니다. 최대 높이 *큰 텍스트* 알림은 256 dp 합니다.

만들려면를 *큰 텍스트* 인스턴스화할 알림을 `NotificationCompat.Builder` 개체를 이전 처럼 로컬 폴더를 인스턴스화하고 추가 [BigTextStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigTextStyle/) 개체는 `NotificationCompat.Builder` 개체. 예를 들면 다음과 같습니다.

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

이 예제에서는 요약 텍스트와 메시지 텍스트에 저장 됩니다는 `BigTextStyle` 개체 (`textStyle`)에 전달 되기 전에 `NotificationCompat.Builder.`


### <a name="image-style"></a>이미지 스타일

*이미지* 스타일 (라고도 합니다 *전체적인* 스타일) 알림의 본문에 이미지를 표시 하는 데 사용할 수 있는 확장 된 알림 형식. 예를 들어, 스크린 샷 앱 또는 사진 앱을 사용할 수는 *이미지* 알림은 마지막에 대 한 알림을 사용 하 여 사용자를 제공 하는 스타일 이미지를 캡처한 합니다. 최대 높이 *이미지* 알림은 256 dp &ndash; Android이 최대 높이 제한은 사용 가능한 메모리 한도 내에 맞게 이미지 크기를 조정 합니다.

모든 같은 레이아웃 알림 확장 *이미지* 알림은 함께 제공 되는 메시지 텍스트의 일부를 표시 하는 압축 된 형식으로 먼저 표시 됩니다.

![Compact 이미지 알림 없는 이미지를 보여 줍니다.](local-notifications-images/17-image-compact.png)

끌면는 *이미지* 알림, 이미지를 표시 하려면 확장 합니다. 예를 들어, 다음은 이전 알림 확장 된 버전:

![확장 된 이미지 알림 보여 주는 이미지](local-notifications-images/18-image-expanded.png)

알림을 압축 형식으로 표시 되 면 알림 텍스트를 표시 하는 알림 (알림 작성기에 전달 되는 텍스트 `SetContentText` 메서드, 앞서 설명한 대로). 그러나 이미지를 표시 하려면 알림 확장 되어 이미지 위에 요약 텍스트가 표시 됩니다.

만들려는 *이미지* 인스턴스화할 알림을 `NotificationCompat.Builder` 이전과 마찬가지로 개체 로컬 폴더를 만들고 삽입을 [BigPictureStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigPictureStyle/) 개체를 `NotificationCompat.Builder` 개체. 예를 들어:

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

같은 합니다 `SetLargeIcon` 메서드의 `NotificationCompat.Builder`의 [BigPicture](https://developer.xamarin.com/api/member/Android.App.Notification+BigPictureStyle.BigPicture/) 메서드의 `BigPictureStyle` 알림의 본문에 표시 하려는 이미지의 비트맵을 필요 합니다. 이 예제는 [DecodeResource](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeResource/(Android.Content.Res.Resources%2cSystem.Int32)) 메서드의 `BitmapFactory` 이미지 파일에 있는 읽기 **Resources/drawable/x_bldg.png** 비트맵으로 변환 합니다.

또한 리소스로 제공 되지 않는 이미지를 표시할 수 있습니다. 다음 샘플 코드를 로컬 SD 카드에서 이미지를 로드 하 고에 표시 하는 예를 들어 있는 *이미지* 알림:

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

이 예제에서는 이미지 파일에 있는 **/sdcard/Pictures/my-tshirt.jpg** 로드, 해당 원래 크기의 절반으로 조정 되어 알림에 사용할 비트맵을 변환 합니다.

![알림의 예제 티셔츠 이미지](local-notifications-images/19-tshirt-notification.png)

이미지 파일 크기인을 사전에 모르는 경우에 대 한 호출을 래핑할는 것이 좋습니다 [BitmapFactory.DecodeFile](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeFile/p/System.String/Android.Graphics.BitmapFactory+Options/) 예외 핸들러가 &ndash; 는 `OutOfMemoryError` 이미지에 대 한 너무 크면 예외가 throw 될 수 있습니다 Android의 크기를 조정 합니다.

로드 하 고 큰 비트맵 이미지 디코딩 대 한 자세한 내용은 참조 하세요 [부하 효율적으로 큰 비트맵](https://github.com/xamarin/recipes/tree/master/Recipes/android/resources/general/load_large_bitmaps_efficiently)합니다.


### <a name="inbox-style"></a>받은 편지함 스타일

*수신함* 형식은 알림 메시지의 본문에 별도 줄 (예: 전자 메일 받은 편지함 요약)는 텍스트의 표시를 위한 확장 된 레이아웃 템플릿입니다. 합니다 *수신함* 형식 알림이 압축 된 형식으로 먼저 표시 됩니다.

![예제 compact 수신함 알림](local-notifications-images/20-inbox-compact.png)

사용자가 알림을 끌면, 아래 스크린샷에 표시 된 것 처럼 전자 메일 요약을 표시 하려면 확장 합니다.

![확장 예제 수신함 알림](local-notifications-images/21-inbox-expanded.png)

만들려면를 *수신함* 인스턴스화할 알림을 `NotificationCompat.Builder` 이전 처럼 개체를 추가 [InboxStyle](https://developer.xamarin.com/api/type/Android.App.Notification+InboxStyle/) 개체는 `NotificationCompat.Builder`. 예를 들면 다음과 같습니다.

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

알림 본문에 새 텍스트 줄을 추가 하려면 호출를 [Addline](https://developer.xamarin.com/api/member/Android.App.Notification+InboxStyle.AddLine/p/System.String/) 메서드를 `InboxStyle` 개체 (의 최대 높이 *수신함* 알림은 256 dp). 이때 달리 *큰 텍스트* 스타일의 *수신함* 스타일이 알림 본문에 개별 줄의 텍스트를 지원 합니다.

사용할 수도 있습니다는 *수신함* 확장 된 형식으로 개별 줄의 텍스트를 표시 해야 하는 알림의 스타일입니다. 예를 들어 합니다 *수신함* 알림 스타일을 사용 하 여 요약 알림에 보류 중인 여러 알림을 결합할 수 있습니다 &ndash; 단일을 업데이트할 수 있습니다 *수신함* new를 사용 하 여 알림 스타일 알림 콘텐츠 줄 (참조 [알림을 업데이트](#updating-a-notification) 위에) 아닌 대부분 마찬가지로 새 알림의 연속 스트림을 생성 하는 것입니다.


## <a name="configuring-metadata"></a>메타 데이터 구성

`NotificationCompat.Builder` 우선 순위, 표시 유형 및 범주와 같은 사용자 알림에 대 한 메타 데이터를 설정 하려면 호출할 수 있는 메서드를 포함 합니다. 이 정보를 사용 하는 android &mdash; 사용자 기본 설정과 함께 &mdash; 방법과 시기를 확인 하려면 알림을 표시 합니다.


### <a name="priority-settings"></a>우선 순위 설정

앱에서 실행 중인 Android 7.1 낮은 우선 순위 알림 자체에서 직접 설정 해야 합니다. 알림의 우선 순위 설정을 알림이 게시 될 때 두 개의 결과를 결정 합니다.

-   여기서 알림을 다른 알림 기준으로 표시 됩니다.
    예를 들어 우선 순위가 높은 알림 표시 됩니다 알림 서랍에서 낮은 우선 순위 알림 위에 관계 없이 각 알림은 게시 된 경우.

-   알림은 Heads-up 알림 형식 (Android 5.0 이상)의 표시 여부입니다. 만 *높은* 하 고 *최대* 우선 순위 알림은 헤드업 알림으로 표시 됩니다.

Xamarin.Android 알림 우선 순위를 설정 하기 위한 다음과 같은 열거형을 정의 합니다.

-   `NotificationPriority.Max` &ndash; 경고는 긴급 또는 위험 조건 (예: 들어오는 호출, 설정 하 여 설정 지침 또는 응급 경고)에 메시지를 표시 합니다. Android 5.0 및 이상 장치에서는 최대 우선 순위 알림은 헤드업 형식으로 표시 됩니다.

-   `NotificationPriority.High` &ndash; 중요 한 이벤트 (예: 중요 한 전자 메일 또는 실시간 채팅 메시지 도착)의 사용자를 게 알립니다. Android 5.0 및 이후 장치에서 우선 순위가 높은 알림 헤드업 형식으로 표시 됩니다.

-   `NotificationPriority.Default` &ndash; 중요도 수준이 보통 조건의 사용자를 게 알립니다.

-   `NotificationPriority.Low` &ndash; 사용자 (예: 소프트웨어 업데이트 알림 또는 소셜 네트워크 업데이트)의 새로운 소식 해야 한다고 급하지 않은 정보에 대 한 합니다.

-   `NotificationPriority.Min` &ndash; 사용자의 경우에만 통지는 배경 정보 (예: 위치 또는 날씨 정보) 알림을 볼 수 있습니다.

알림의 우선 순위를 설정 하려면 호출을 [SetPriority](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetPriority/) 메서드는 `NotificationCompat.Builder` 우선 순위 수준에 전달 하 여 개체를 합니다. 예를 들어:

```csharp
builder.SetPriority (NotificationPriority.High);
```

다음 예제에서는, 우선 순위가 높은 알림, "중요 메시지!" 알림 서랍의 맨 위에 나타납니다.

![예제에서는 우선 순위가 높은 알림](local-notifications-images/22-hi-priority-drawer.png)

우선 순위가 높은 알림 이기 때문에 Android 5.0에서 사용자의 현재 활동 화면 위 헤 즈 업 알림으로 표시 됩니다.

![예제 헤드업 알림](local-notifications-images/23-heads-up-example.png)

다음 예제에서는 우선 순위가 높은 배터리 수준 알림에서 우선 순위가 낮은 "당일의 생각" 알림이 표시 됩니다.

![예제에서는 우선 순위가 낮은 알림](local-notifications-images/24-lo-priority-drawer.png)

"당일의 사고" 알림 우선 순위가 낮은 알림을 이기 때문에 Android에에서 표시 되지 않습니다 것 Heads-up 형식입니다.

> [!NOTE]
> Android 8.0 이상에서는 알림 메시지의 우선 순위 알림 채널 및 사용자 설정의 우선 순위에 따라 결정 됩니다.

### <a name="visibility-settings"></a>표시 유형 설정

Android 5.0부터 합니다 *가시성* 설정은 보안 잠금 화면에 표시 되는 알림 콘텐츠의 양을 제어 수입니다.
Xamarin.Android 알림 표시 유형 설정에 대 한 다음과 같은 열거형을 정의 합니다.

-   `NotificationVisibility.Public` &ndash; 알림의 전체 콘텐츠를 보안 잠금 화면에 표시 됩니다.

-   `NotificationVisibility.Private` &ndash; 보안 잠금 화면 (예: 알림 아이콘 및 게시 하는 앱의 이름)에 중요 한 정보만 표시 됩니다 있지만 알림의 세부 정보 나머지는 숨겨집니다. 기본적으로 모든 알림 `NotificationVisibility.Private`합니다.

-   `NotificationVisibility.Secret` &ndash; 아무 것도 보안 잠금 화면에서 알림 아이콘도 표시 됩니다. 알림 콘텐츠는 사용자는 장치의 잠금을 해제 한 후에 사용할 수 있습니다.

앱 호출 알림의 표시 유형을 설정 하려면 합니다 `SetVisibility` 메서드를 `NotificationCompat.Builder` 개체, 표시 유형 설정을 전달 합니다. 예를 들어,이 호출은 `SetVisibility` 알림을 통해 `Private`:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

경우는 `Private` 알림이 게시 될, 이름 및 앱의 아이콘은 보안 잠금 화면에 표시 됩니다. 알림 메시지를 대신 "잠금 해제 장치의이 알림을 확인 하려면" 사용자에 게 표시 합니다.

![장치 알림 메시지를 잠금 해제](local-notifications-images/25-lockscreen-private.png)

이 예에서 **NotificationsLab** 원래 앱의 이름입니다. 알림 모드이 버전은 잠금 화면 안전한 경우에 표시 됩니다 (즉, PIN, 패턴 또는 암호를 통해 보안 됨) &ndash; 잠금 화면에 보안이 설정 되지 않으면 알림의 전체 콘텐츠는 잠금 화면에서 사용할 수 있습니다.


### <a name="category-settings"></a>범주 설정

Android 5.0부터, 미리 정의 된 범주는 순위 및 알림을 필터링에 사용할 수 있습니다. Xamarin.Android는 이러한 범주에 대 한 다음과 같은 열거형을 제공합니다.

-   `Notification.CategoryCall` &ndash; 들어오는 전화 통화입니다.

-   `Notification.CategoryMessage` &ndash; 들어오는 텍스트 메시지입니다.

-   `Notification.CategoryAlarm` &ndash; 경보 조건 또는 타이머 만료 합니다.

-   `Notification.CategoryEmail` &ndash; 들어오는 전자 메일 메시지입니다.

-   `Notification.CategoryEvent` &ndash; 달력 이벤트입니다.

-   `Notification.CategoryPromo` &ndash; 프로 모션 메시지 또는 알림입니다.

-   `Notification.CategoryProgress` &ndash; 백그라운드 작업의 진행률입니다.

-   `Notification.CategorySocial` &ndash; 소셜 네트워킹 업데이트 합니다.

-   `Notification.CategoryError` &ndash; 백그라운드 작업 또는 인증 프로세스 중 오류가 발생 했습니다.

-   `Notification.CategoryTransport` &ndash; 미디어 재생 업데이트 합니다.

-   `Notification.CategorySystem` &ndash; (시스템 또는 장치 상태) 시스템용으로 예약 되어 있습니다.

-   `Notification.CategoryService` &ndash; 백그라운드 서비스 실행 중임을 나타냅니다.

-   `Notification.CategoryRecommendation` &ndash; 현재 실행 중인 앱에 관련 된 권장 사항 메시지입니다.

-   `Notification.CategoryStatus` &ndash; 장치에 대 한 정보를 제공 합니다.

알림 정렬 되는 알림의 우선 순위 범주 설정에 따라 보다 우선 합니다. 예를 들어 우선 순위가 높은 알림으로 표시할 헤 즈 업에 속하는 경우에는 `Promo` 범주입니다. 알림의 범주를 설정 하려면 호출을 `SetCategory` 메서드의 `NotificationCompat.Builder` 범주 설정에 전달 하 여 개체를 합니다. 예를 들어:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

합니다 *방해 금지* 범주를 기반으로 하는 알림을 필터링 하는 기능 (Android 5.0의 새로운). 예를 들어 합니다 *방해 금지* 화면의 **설정** 전화 통화 및 메시지에 대 한 예외 알림을 사용 하면:

![화면 스위치 방해 금지](local-notifications-images/26-do-not-disturb.png)

사용자 구성 하는 경우 *방해 금지* Android 설정 범주를 사용 하 여 알림 사용 하면 전화 통화 (처럼 위의 스크린샷에) 제외 하 고 모든 인터럽트를 차단 하려면 `Notification.CategoryCall` 장치 하는 동안 표시 될 수 상태인 *방해 금지* 모드입니다. 사실은 `Notification.CategoryAlarm` 알림을 차단 되지 않으므로 *방해 금지* 모드입니다.

합니다 [LocalNotifications](https://developer.xamarin.com/samples/monodroid/LocalNotifications) 샘플에 사용 하는 방법을 보여 줍니다. `NotificationCompat.Builder` 알림에서 두 번째 작업을 시작 합니다. 이 샘플 코드에 설명 되어는 [Xamarin.Android에서 로컬 알림을 사용 하 여](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) 연습 합니다.


### <a name="notification-styles"></a>알림 스타일

만들려는 *큰 텍스트*, *이미지*, 또는 *수신함* 알림과 스타일 `NotificationCompat.Builder`, 앱 이러한 스타일의 호환 버전을 사용 해야 합니다. 예를 들어 사용 하 여 *큰 텍스트* 스타일, 인스턴스화할 `NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

마찬가지로, 앱을 사용할 수 `NotificationCompat.InboxStyle` 하 고 `NotificationCompat.BigPictureStyle` 에 대 한 *수신함* 하 고 *이미지* 스타일 각각.


### <a name="notification-priority-and-category"></a>알림 우선 순위와 범주

`NotificationCompat.Builder` 지원 된 `SetPriority` 메서드 (사용 가능한 Android 4.1 부터는). 그러나 합니다 `SetCategory` 메서드는 *없습니다* 지 `NotificationCompat.Builder` 범주 Android 5.0에 도입 된 새 알림 메타 데이터 시스템의 일부 이므로 합니다.

Android의 이전 버전을 지원 하도록 `SetCategory` 는 사용할 수 없는 코드 확인해 조건부로 호출 하는 런타임 시 API 수준 `SetCategory` API 수준 Android 5.0 (API 수준 21) 보다 크거나 같은 경우:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

이 예제에서는 앱의 **대상 프레임 워크** Android 5.0으로 설정 되어 하며 **최소 Android 버전** 로 설정 되어 **Android 4.1 (API 레벨 16)** 합니다. 때문에 `SetCategory` 는 API 수준 21 이상에서 사용할 수 있는,이 예제 코드를 호출 합니다 `SetCategory` 경우에 사용할 수 있습니다 &ndash; 호출 하지 `SetCategory` API 수준 21 보다 작은 경우.


### <a name="lock-screen-visibility"></a>잠금 화면 표시

Android (API 수준 21), Android 5.0 이전 화면 잠금 알림을 지원 하지 않았기 때문에 `NotificationCompat.Builder` 지원 하지 않습니다는 `SetVisibility` 메서드. 위에서 설명한 것 처럼 `SetCategory`, 코드는 런타임 및 호출 API 수준을 확인할 수 있습니다 `SetVisiblity` 경우에 사용할 수 있습니다.

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop) {
    builder.SetVisibility (Notification.Public);
}
```


## <a name="summary"></a>요약

이 문서에는 Android에서 로컬 알림을 만드는 방법을 설명 합니다. 알림의 분석 설명, 사용 하는 방법을 설명 했습니다 `NotificationCompat.Builder` 알림을 만들려면 큰 아이콘의 스타일 알림 방법 *큰 텍스트*를 *이미지* 고 *수신함*  형식, 알림 우선 순위, 표시 유형 및 범주와 같은 메타 데이터 설정을 설정 하는 방법 및 알림에서 활동을 시작 하는 방법입니다. 이 문서에서는 또한 이러한 알림 설정을 새 헤드업, 잠금 화면을 사용 하는 방법을 설명 하 고 *방해 금지* Android 5.0에 도입 된 기능입니다. 마지막으로 사용 하는 방법을 알아보았습니다 `NotificationCompat.Builder` 이전 버전의 Android 사용 하 여 알림 호환성을 유지 합니다.

Android에 대 한 알림 디자인에 대 한 지침을 참조 하세요 [알림을](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)합니다.


## <a name="related-links"></a>관련 링크

- [NotificationsLab (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/NotificationsLab/)
- [LocalNotifications (샘플)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Android 연습에서 로컬 알림](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [사용자에 게 알리지](https://developer.android.com/training/notify-user/index.html)
- [알림](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
