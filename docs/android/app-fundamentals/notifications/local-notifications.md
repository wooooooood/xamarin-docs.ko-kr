---
title: 로컬 알림
description: 이 섹션에는 로컬 알림을 Xamarin.Android에서 구현 하는 방법을 보여 줍니다. Android 알림의 다양 한 UI 요소에 설명 하 고 API에 설명의 관련 만들기 및 알림을 표시 합니다.
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 97c8372656f0cbfa5b8f7bb12d15b00feac4b5c3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773994"
---
# <a name="local-notifications"></a>로컬 알림

_이 섹션에는 로컬 알림을 Xamarin.Android에서 구현 하는 방법을 보여 줍니다. Android 알림의 다양 한 UI 요소에 설명 하 고 API에 설명의 관련 만들기 및 알림을 표시 합니다._

## <a name="local-notifications-overview"></a>로컬 알림 개요

이 항목에서는 로컬 알림을 Xamarin.Android 응용 프로그램에서 구현 하는 방법을 설명 합니다. Android 알림의 다양 한 부분에 설명 하 고 응용 프로그램 개발자에 게 사용할 수 있는 다양 한 알림 스타일에 설명 만들고 알림을 게시 하는 데 사용 되는 Api는 일부을 소개 합니다.

Android는 사용자에 게 알림 아이콘 및 알림 정보를 표시 하기 위한 두 가지 시스템 제어 영역을 제공 합니다. 에 해당 아이콘이 표시 되는 알림 처음 게시 되 면는 *알림 영역*다음 스크린 샷에 표시 된 것 처럼:

![장치에서 알림 영역 예제](local-notifications-images/01-notification-shade.png)

알림에 대 한 세부 정보를 가져오려면 사용자 (알림 콘텐츠를 표시 하기 위해 각 알림 아이콘을 확장)이 표시 되는 알림 서랍을 열 수는 알림과 관련 된 모든 작업을 수행 하 고 있습니다. 다음 스크린샷에 표시 된 *알림 서랍* 위에 표시 되는 알림 영역에 해당 하는:

[![예제 알림 서랍 알림 세 건을 표시 합니다.](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Android 알림을 두 종류의 레이아웃을 사용합니다.

-   ***기본 레이아웃*** &ndash; compact, 고정 표시 형식입니다.

-   ***확장 된 레이아웃*** &ndash; 자세한 정보를 표시 하기 위해 더 큰 크기를 확장할 수 있는 표시 형식입니다.

이러한 각 유형의 레이아웃 (및를 만드는 방법) 다음 섹션에 설명 되어 있습니다.


### <a name="base-layout"></a>기본 레이아웃

여기에 최소한 다음 요소를 포함 하는 기본 레이아웃 형식에 모든 Android 알림이 생성 됩니다.

1.  A *알림 아이콘*, 응용 프로그램에서 다른 유형의 알림 지 원하는 경우 원래 앱 또는 알림 유형을 나타내는입니다.

2.  알림 *제목*, 또는 개인 메시지는 경우 보낸 사람 이름입니다.

3.  알림 메시지입니다.

4.  A *타임 스탬프*합니다.

이러한 요소는 다음 그림과 같이 표시 됩니다.

[![알림 요소의 위치](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

기본 레이아웃 높이 64 밀도 독립적 픽셀 (dp)로 제한 됩니다. Android는 기본적으로이 기본 알림 스타일을 만듭니다.

필요에 따라 알림 응용 프로그램 또는 보낸 사람의 사진 나타내는 큰 아이콘을 표시할 수 있습니다. 알림을 Android 5.0 이상에서 큰 아이콘을 사용 큰 아이콘 위에 작은 알림 아이콘 배지도 표시 됩니다.

![간단한 알림 사진](local-notifications-images/04-simple-notification-photo.png)

Android 5.0 이후부터, 알림 수에 표시는 잠금 화면:

[![잠금 화면 알림 예제](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

사용자 수를 두 번 누릅니다 장치 잠금을 해제 하 고 해당 알림을 발생 시킨 응용 프로그램으로 이동 하려면 잠금 화면 알림 또는 알림을 해제할을 살짝 밉니다. 앱의 잠금 화면에 표시 되는 내용을 제어에 대 한 알림의 표시 수준을 설정할 수 및 사용자가 중요 한 내용이 잠금 화면 알림에 표시할 수 있도록 할지 여부를 선택할 수 있습니다.

Android 5.0 도입 이라고 하는 우선 순위가 높은 알림 프레젠테이션 형식 *헤드업*합니다. 화면 알림 몇 초간 화면 맨 위에서부터 내려갈 및 알림 영역에 백업 후퇴 다음:

[![예제 heads-up 알림](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

화면 알림 시스템 UI 현재 실행 중인 활동의 상태를 방해 하지 않고 중요 한 정보를 넣을 수 있도록 합니다.

Android는 알림을 정렬 및 지능적으로 표시 될 수 있도록 알림 메타 데이터에 대 한 지원을 포함 합니다. 알림 메타 데이터는 또한 헤드업 형태로 표시 하 고는 잠금 화면에서 알림 표시 되는 방식을 제어 합니다. 응용 프로그램 다음과 같은 유형의 알림 메타 데이터를 설정할 수 있습니다.

-   **우선 순위** &ndash; 우선 순위 수준은 알림이 표시 되는 방법과 시기를 결정 합니다. 예를 들어에서 Android 5.0에서 우선 순위가 높은 알림은 헤드업 알림을으로 표시 됩니다.

-   **표시 유형** &ndash; 의 잠금 화면에 알림을 표시 될 때 표시할 알림 내용의 양을 지정 합니다.

-   **범주** &ndash; 시스템 장치 중인 경우와 같은 다양 한 상황에서 알림 메시지를 처리 하는 방법을 알립니다 *방해 하지* 모드입니다.

**참고:** **가시성** 및 **범주** Android 5.0 및 이전 버전의 Android 수 없는에서 도입 되었습니다. Android 8.0부터 [알림 채널](#notif-chan) 알림을 사용자에 게 표시 되는 방식을 제어 하는 데 사용 됩니다.


### <a name="expanded-layouts"></a>확장 된 레이아웃

Android 4.1 부터는 알림 사용자가 더 많은 콘텐츠를 보려면 알림의 높이 확장 하도록 허용 하는 확장 된 레이아웃 스타일으로 구성할 수 있습니다. 예를 들어 다음 예제에서는 계약 된 모드에는 확장 된 레이아웃 알림을 보여 줍니다.

![계약 된 알림](local-notifications-images/07-contracted-notification.png)

이 알림은 확장 되 면 전체 메시지를 보여 줍니다.

![확장 된 알림](local-notifications-images/08-expanded-notification.png)

Android에서는 단일 이벤트 알림에 대 한 세 가지 확장 된 레이아웃 스타일을 지원합니다.

-   ***큰 텍스트*** &ndash; 계약 된 모드에서 두 개의 마침표 앞에 나오는 메시지의 첫 번째 줄의 일부를 표시 합니다. 확장 된 모드 (위의 예에 표시) 된 대로 전체 메시지를 표시 합니다.

-   ***받은 편지함*** &ndash; 계약 된 모드에서 새 메시지의 수를 표시 합니다. 확장 된 모드에서 첫 번째 전자 메일 메시지 또는 받은 편지함에 있는 메시지의 목록을 표시합니다.

-   ***이미지*** &ndash; 계약 된 모드에만 메시지 텍스트를 표시 합니다. 확장 된 모드에서 텍스트와 이미지를 표시합니다.

[기본 알림 외](#beyond-the-basic-notification) (이 문서의 뒷부분에)를 만드는 방법을 설명 *큰 텍스트*, *받은 편지함*, 및 *이미지* 알림입니다.


## <a name="notification-creation"></a>알림 만들기

Android 알림을 만들려면에서 사용 하는 [Notification.Builder](https://developer.xamarin.com/api/type/Android.App.Notification+Builder/) 클래스입니다. `Notification.Builder` 알림 개체 만들기를 간소화 하기 위해 Android 3.0에서 도입 되었습니다. 이전 버전 Android의 호환 되는 알림을 만들려면 사용할 수 있습니다는 [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) 클래스 대신 `Notification.Builder` (참조 [호환성](#compatibility) 를 위한이 항목의 뒷부분에 나오는 사용 하는 방법에 대 한 자세한 내용은 `NotificationCompat.Builder`).
`Notification.Builder` 알림을에서 같은 다양 한 옵션을 설정 하기 위한 메서드를 제공 합니다.

-   제목, 메시지 텍스트 및 알림 아이콘을 포함 하는 콘텐츠입니다.

-   알림, 스타일 등 *큰 텍스트*, *받은 편지함*, 또는 *이미지* 스타일입니다.

-   알림 메시지의 우선 순위: 최소, 낮은, 기본적으로 높거나 최대 합니다.

-   잠금 화면에서 알림 메시지의 표시 유형을: public, private 또는 암호입니다.

-   Android 도움이 되는 범주 메타 데이터는 분류 하 고 알림을 필터링 합니다.

-   알림을 탭이 수행 되는 경우 시작 하기 위한 작업을 나타내는 선택적 의도 합니다.

작성기에서 이러한 옵션을 설정한 후 설정을 포함 하는 알림 개체를 생성 합니다. 이 알림 개체를 전달 하면이 알림을 게시 하려면는 *알림 관리자*합니다. Android에서 제공 된 [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/) 알림을 게시 하 고 사용자에 게 표시 하는 일을 담당 하는 클래스입니다. 이 클래스에 대 한 작업 또는 서비스와 같은 모든 컨텍스트를 가져올 수 있습니다.


### <a name="how-to-generate-a-notification"></a>알림을 생성 하는 방법

Android에서 알림을 생성 하려면 다음이 단계를 따르십시오.

1.  인스턴스화하는 `Notification.Builder` 개체입니다.

2.  다양 한 메서드를 호출의 `Notification.Builder` 알림 옵션을 설정 하는 개체입니다.

3.  호출 된 [빌드](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.Build/) 의 메서드는 `Notification.Builder` 알림 개체를 인스턴스화하는 개체입니다.

4.  호출 된 [알림](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/(System.Int32%2cAndroid.App.Notification)) 알림을 게시 하는 알림 관리자의 메서드.

각 알림에 대해 다음 정보 이상 제공 해야 합니다.

-   작은 아이콘 (24 x 24 dp의 크기)

-   제목

-   알림 메시지의 텍스트

다음 코드 예제를 사용 하는 방법을 보여 줍니다 `Notification.Builder` 기본 알림을 생성 하도록 합니다. 다음에 유의 `Notification.Builder` 메서드 지원 [메서드 체인](http://en.wikipedia.org/wiki/Method_chaining);는 다음이 메서드를 호출 하는 마지막 메서드 호출의 결과 사용할 수 있도록 각 메서드가 작성기 개체를 반환 하는, 즉:

```csharp
// Instantiate the builder and set notification elements:
Notification.Builder builder = new Notification.Builder (this)
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

이 예제에서는 새 `Notification.Builder` 라는 개체 `builder` 는 제목 및 텍스트 알림을 설정 하 고 알림 아이콘에서 로드 된 인스턴스화된 **Resources/drawable/ic_notification.png**합니다. 알림 작성기에 대 한 호출 `Build` 메서드는 이러한 설정을 사용 하 여 알림 개체를 만듭니다. 다음 단계를 호출 하는 것은 `Notify` 알림 관리자의 메서드. 호출의 알림 관리자를 찾으려면 `GetSystemService`위에 표시 된 것 처럼 합니다.

`Notify` 메서드는 두 개의 매개 변수: 알림 식별자 및 알림 개체입니다. 알림 식별자는 응용 프로그램에 알림을 식별 하는 고유한 정수입니다. 이 예제에서는 알림 식별자 (0); 0으로 설정 되어 그러나 프로덕션 응용 프로그램에서는 할 각 알림에 고유한 식별자를 지정 합니다. 이전 식별자 값에 대 한 호출에서 다시 사용 `Notify` 마지막 알림을 덮어쓸 수 하 게 합니다.

이 코드에서는 Android 5.0 장치에서 실행 하는 경우 다음 예제와 같이 알리는 알림이 발생 합니다.

![샘플 코드에 대 한 알림 결과](local-notifications-images/09-hello-world.png)

알림 아이콘은 알림의 왼쪽에 표시 됩니다 &ndash; 이 이미지는 원 안의의 &ldquo;i&rdquo; Android를 근거 회색 원형 배경 그릴 수 있도록 알파 채널에 있습니다. 알파 채널 없이 아이콘을 제공할 수도 있습니다. 사진 이미지 아이콘을 표시 하려면 참조 [큰 아이콘 형식](#large-icon-format) 이 항목의 뒷부분에 나오는 합니다.

타임 스탬프는 자동으로 설정 되어 있지만 호출 하 여이 설정을 재정의할 수는 [SetWhen](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetWhen/) 알림 작성기의 메서드. 예를 들어 다음 코드 예제에서는 현재 시간에 타임 스탬프를 설정합니다.

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>사운드 및 진동을 사용 하도록 설정

또한 소리를 재생 하는 알림을 원하는 알림 작성기를 호출할 수 있습니다 [SetDefaults](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetDefaults/) 메서드와 전달은 `NotificationDefaults.Sound` 플래그:

```csharp
// Instantiate the notification builder and enable sound:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

이 호출으로 `SetDefaults` 알림이 게시 될 때 소리를 재생 하는 장치에 발생 합니다. 장치에서 소리를 재생 하는 것이 아니라 진동를 원하는 경우 전달할 수 있습니다 `NotificationDefaults.Vibrate` 를 `SetDefaults.` 장치를 소리를 재생 하 고 장치를 진동 하려는 경우 두 플래그를 전달할 수 있습니다 `SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

소리를 재생할 소리를 지정 하지 않고 사용 하도록 설정한 경우 Android 기본 시스템 알림 소리를 사용 합니다. 그러나 알림 작성기를 호출 하 여 재생 될 소리를 변경할 수 있습니다 [SetSound](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetSound/p/Android.Net.Uri/) 메서드. 예를 들어 경보를 재생할 사운드 기본 알림 소리), (대신 프로그램 알림을 사용 하 여 얻을 수 있습니다 URI 알람에서 사운드는 [RingtoneManager](https://developer.xamarin.com/api/type/Android.Media.RingtoneManager/) 시키고 `SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

또는 사용할 수 있습니다 시스템 기본 벨 소리에 알림에 대 한.

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

알림 개체에 알림 속성을 설정할 수는 알림 개체를 만든 후 (통해 미리 구성 하지 않고 `Notification.Builder` 메서드). 예를 들어 호출 하는 대신는 `SetDefaults` 메서드는 알림 진동을 사용 하도록 설정 하려면 알림의 비트 플래그를 직접 수정할 수 있습니다 [기본값](https://developer.xamarin.com/api/property/Android.App.Notification.Defaults/) 속성:

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

이 예제에는 알림을 게시 될 때를 진동 할 장치를입니다.

<a name="updating-a-notification" />

### <a name="updating-a-notification"></a>업데이트 알림

이 게시 된 후 알림의 콘텐츠를 업데이트 하려는 경우 다시 사용할 수 있습니다 기존 `Notification.Builder` 새 알림 개체를 만들고 마지막 알림의 식별자를 가진이 알림을 게시 하는 개체입니다. 예를 들어:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

이 예제에서는 기존 `Notification.Builder` 개체 다른 제목을와 메시지는 새 알림 개체를 만드는 데 사용 됩니다.
이전 알림의 식별자를 사용 하 여 새 알림 개체에 게시 하 고이 이전에 게시 알림 메시지의 콘텐츠를 업데이트 합니다.

![업데이트 된 알림](local-notifications-images/12-updated-notification.png)

이전 알림 메시지의 본문은 다시 사용 &ndash; 제목만 및의 텍스트가 알림을 알림 서랍에 표시 되는 동안 알림 변경 합니다. 제목 텍스트 "샘플 알림"에서 "업데이트 알림" 변경 하 고 메시지 텍스트 "Hello World 간에 변경! 이것은 내 첫 번째 알림! " "이이 메시지에 변경 되었습니다."를

다음 세 가지 중 하나가 발생 될 때까지 알림이 계속 표시 됩니다.

-   사용자가 알림을 해제할 (탭 또는 *모두 지우기*).

-   응용 프로그램을 호출 하 게 `NotificationManager.Cancel`, 알림의 게시할 때 할당 된 고유 알림 ID를 전달 합니다.

-   응용 프로그램 호출 `NotificationManager.CancelAll`합니다.

Android 알림을 업데이트에 대 한 자세한 내용은 [알림을 수정](http://developer.android.com/training/notify-user/managing.html#Updating)합니다.


### <a name="starting-an-activity-from-a-notification"></a>알림을에서 활동을 시작합니다.

Android에서는 연결 된에 대 한 알림을 일반적 이기는 *동작* &ndash; 사용자가 알림을 때 실행 되는 활동입니다. 이 작업은 다른 작업 또는 다른 응용 프로그램에 있을 수 있습니다. 만들면 액션에 알림을를 추가 하려면는 [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/) 개체와 연결 된 `PendingIntent` 알림에서 합니다. A `PendingIntent` 의도 보내는 응용 프로그램의 권한으로 실행 되는 미리 정의 된 코드 부분을 받는 사람의 응용 프로그램을 허용 하는 특수 한 형식입니다. 사용자가 알림을 누르면 Android 시작 하 여 지정 된 활동의 `PendingIntent`합니다.

다음 코드 조각에 관련 된 알림을 만드는 방법을 보여 주는 `PendingIntent` 원래 응용 프로그램의 작업을 시작 하는 `MainActivity`:

```csharp
// Set up an intent so that tapping the notifications returns to this app:
Intent intent = new Intent (this, typeof(MainActivity));

// Create a PendingIntent; we're only using one PendingIntent (ID = 0):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    PendingIntent.GetActivity (this, pendingIntentId, intent, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including pending intent:
Notification.Builder builder = new Notification.Builder(this)
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

이 코드는 점을 제외 하 고 이전 섹션에서 알림 코드와 매우 비슷합니다는 `PendingIntent` 알림 개체에 추가 됩니다. 이 예제는 `PendingIntent` 알림 작성기에 전달 하기 전에 원래 앱의 활동을 연관 된 [SetContentIntent](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetContentIntent/) 메서드. `PendingIntentFlags.OneShot` 플래그에 전달 되는 `PendingIntent.GetActivity` 메서드 있도록는 `PendingIntent` 한 번만 사용 됩니다. 이 코드를 실행 하는 경우 다음과 같은 알림이 표시 됩니다.

![첫 번째 작업 알림](local-notifications-images/10-first-action-notification.png)

이 알림을 탭 원래 활동으로 이동 합니다.

프로덕션 응용 프로그램에서 응용 프로그램 처리 해야 합니다는 *백 스택에* 를 누를 때는 **다시** 알림 작업 내에서 단추 (Android 작업 및 백 스택에 있는 잘 모르는 경우 참조 [ 작업 및 백 스택에](http://developer.android.com/guide/components/tasks-and-back-stack.html)).
대부분의 경우 알림 활동 뒤로 탐색 사용자를 앱 외부로 및 홈 화면으로 반환 해야 합니다. 백 스택에 앱 사용을 관리 하는 [TaskStackBuilder](https://developer.xamarin.com/api/type/Android.App.TaskStackBuilder/) 만들 클래스는 `PendingIntent` 백 스택에 있습니다.

다른 실제 고려 사항은 원래 활동 알림 작업에 데이터를 보내도록 할 수입니다. 예를 들어 알림 텍스트 메시지가 도착 하 고 사용자에 게 메시지를 표시 하는 메시지의 ID를 필요로 하는 알림 활동 (화면 보기 메시지)을 나타낼 수 있습니다. 활동을 만드는 `PendingIntent` צ ְ ײ는 [Intent.PutExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.PutExtra/p/System.String/System.String/) 메서드 (예: 문자열) 데이터를 의도이 데이터는 알림 작업에 전달 되도록 합니다.

다음 코드 샘플을 사용 하는 방법을 보여 줍니다 `TaskStackBuilder` 백 스택에 하며 관리 하 라는 알림 작업에는 단일 메시지 문자열을 보내는 방법의 예가 포함 되어 `SecondActivity`:

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
Notification.Builder builder = new Notification.Builder (this)
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

이 코드 예제에서는 두 가지 활동 앱 구성: `MainActivity` (위의 알림 코드가 포함 되어), 및 `SecondActivity`, 화면 알림을 누른 후 사용자에 게 표시 합니다. 이 코드가 실행 되 면 (이전 예제와 유사) 간단한 알림이 표시 됩니다. 알림 탭 이동 하 여 `SecondActivity` 화면:

![두 번째 활동 스크린 샷](local-notifications-images/11-second-activity.png)

문자열 메시지 (의도로 전달 된 `PutExtra` 메서드)에서 검색 `SecondActivity` 이 줄의 코드를 통해:

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

이 검색 된 메시지를 "Greetings에서 MainActivity!,"에 표시 됩니다는 `SecondActivity` 위 스크린샷에 표시 된 대로 화면입니다. 누를 때는 **다시** 동안 단추 `SecondActivity`, 탐색을 앱 외부로 및 앞에 앱의 시작 화면으로 이어집니다.

의도 보류 중인 만들기에 대 한 자세한 내용은 참조 [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)합니다.


<a name="notif-chan" />

## <a name="notification-channels"></a>알림 채널

Android (Oreo) 8.0부터 사용할 수 있습니다는 *알림 채널* 표시 하려면 알림 각 형식에 대 한 사용자 지정 가능한 채널을 만드는 기능입니다. 알림 채널을 지정할 수 있습니다에 대 한 그룹 알림 채널 우리에 게시할 모든 알림이 동일한 동작이 있습니다. 예를 들어 즉각적인 주목이 필요한 알림을 위한 알림 채널 및 정보 메시지에 사용 되는 별도 "작게" 채널을 있을 수 있습니다.

**YouTube** 두 알림 범주를 나열 하는 앱에 설치 된 Android Oreo: **다운로드 알림** 및 **일반 알림**:

[![Android Oreo에 YouTube에 대 한 알림 화면](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

이러한 각 범주는 알림 채널에 해당합니다. YouTube 앱 구현 하는 **다운로드 알림** 채널 및 **일반 알림** 채널입니다. 탭 **다운로드 알림**, 응용 프로그램의 다운로드 알림 채널에 대 한 설정 화면을 표시 하는:

[![YouTube 앱에 대 한 알림 화면을 다운로드 합니다.](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

이 화면에서 사용자의 동작을 수정할 수는 **다운로드** 다음을 수행 하 여 알림 채널:

-   중요도 수준을 설정 **긴급**, **높은**, **보통**, 또는 **낮은**, 사운드 및 시각적 중단 수준의 구성 합니다.

-   알림 점을 켜거나 끕니다.

-   깜박이 빛을 켜거나 끕니다.

-   표시 하거나 잠금 화면에서 알림 숨깁니다.

-   재정의 **방해 하지** 설정 합니다.

**일반 알림** 채널에과 비슷한 설정:

[![YouTube 앱에 대 한 일반 알림 화면](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

알림 채널 사용자 상호 작용 하는 방법을 제어할 절대 않았는지 공지 &ndash; 위의 스크린샷에서 같이 사용자가 장치에 모든 알림 채널에 대 한 설정을 수정할 수 있습니다. 그러나 (됩니다 수 아래 설명 참조)으로 기본값을 구성할 수 있습니다. 이러한 예제에서 알 수 있듯이 새 알림 채널 기능은 사용 하면 사용자가 다른 종류의 알림이 세부적으로 제어를 제공할 수 있습니다.

알림 채널에 대 한 지원을 응용 프로그램에 추가 해야 하나요? Android 8.0, 앱을 대상으로 하는 경우 *해야* 알림 채널을 구현 합니다.
알림 채널을 사용 하지 않고 사용자에 게 로컬 알림을 보내려고 시도 Oreo에 대 한 대상으로 하는 응용 프로그램 Oreo 장치에 알림을 표시 되지 것입니다. Android 8.0을 대상으로 하지 않는 경우 응용 프로그램 계속 실행 됩니다 알림 동작이 동일 하지만 Android 8.0에서 실행 또는 이전 버전의 Android 7.1 발생으로.


### <a name="creating-a-notification-channel"></a>알림 채널

알림 채널을 만들려면 다음을 수행 합니다.

1. 생성 된 [NotificationChannel](https://developer.android.com/reference/android/app/NotificationChannel.html) 다음 개체:

    - 패키지 내에서 고유한 ID 문자열입니다. 다음 예제에서는 문자열에서에서 `com.xamarin.myapp.urgent` 사용 됩니다.

    - 채널 (40 자 미만)의 사용자가 볼 수 이름입니다. 다음 예제에서는 이름에서에서 **긴급** 사용 됩니다.

    - 알림 방법을 지정을 제어 하는 채널의 중요성에 게시 되는 `NotificationChannel`합니다. 중요도 수 `Default`, `High`, `Low`, `Max`, `Min`, `None`, 또는 `Unspecified`합니다.

    이러한 값을 전달는 [생성자](https://developer.android.com/reference/android/app/NotificationChannel.html#NotificationChannel%28java.lang.String,%20java.lang.CharSequence,%20int%29) (이 예제에서는 `Resource.String.noti_chan_urgent` 로 설정 된 **긴급**):

    ```csharp
    public const string URGENT_CHANNEL = "com.xamarin.myapp.urgent";
    . . .
    string chanName = GetString (Resource.String.noti_chan_urgent);
    var importance = NotificationImportance.High;
    NotificationChannel chan =
       new NotificationChannel (URGENT_CHANNEL, chanName, importance);
    ```

2.  구성의 `NotificationChannel` 초기 설정으로는 개체입니다.
    예를 들어 다음 코드를 구성 된 `NotificationChannel` 개체를이 채널에 게시 알림 장치를 진동 되며 기본적으로 잠금 화면에 나타납니다.

    ```csharp
    chan.EnableVibration (true);
    chan.LockscreenVisibility = NotificationVisibility.Public;
    ```

3.  알림 관리자에 게 알림 채널 개체를 제출 합니다.

    ```csharp
    NotificationManager notificationManager =
        (NotificationManager) GetSystemService (NotificationService);
    notificationManager.CreateNotificationChannel (chan);
    ```


### <a name="posting-to-a-notifications-channel"></a>알림 채널에 게시

알림 채널에 대 한 알림을 게시 하려면 다음을 수행 합니다.

1.  사용 하 여 알림을 구성는 `Notification.Builder`에 채널 ID를 전달 하는 `SetChannelId` 메서드. 예를 들어:

    ```csharp
    Notification.Builder builder = new Notification.Builder (this)
        .SetContentTitle ("Attention!")
        .SetContentText ("This is an urgent notification message!")
        .SetChannelId (URGENT_CHANNEL);
    ```

2.  빌드 및 실행 알림 관리자를 사용 하 여 알림을 [알림](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/p/System.Int32/Android.App.Notification/) 메서드:

    ```csharp
    const int notificationId = 0;
    notificationManager.Notify (notificationId, builder.Build());
    ```

정보 메시지에 대 한 다른 알림 채널을 만드는 위의 단계를 반복할 수 있습니다. 이 두 번째 채널 수를 기본적으로 진동을 사용 하지 않도록 설정, 기본 잠금 화면 표시 유형을 설정 `Private`, 알림 중요도 설정 하 고 `Default`합니다.

전체 코드 예제는 Android Oreo 알림 채널의 동작에 대 한 참조는 [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) 샘플;이 샘플 응용 프로그램 채널 두 개를 관리 하 고 추가 알림 옵션을 설정 합니다.



<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>기본 알림 초과

알림 기본 Android에서 간단한 기본 레이아웃 형식으로 추가 하 여이 기본 형식은 향상 시킬 수 있습니다 하지만 `Notification.Builder` 메서드를 호출 합니다. 이 섹션에서는 알림를 대형 사진 아이콘을 추가 하는 방법을 배우게 됩니다 하 고 확장 된 레이아웃 알림을 만드는 방법의 예제를 볼 수 있습니다.

<a name="large-icon-format" />

### <a name="large-icon-format"></a>큰 아이콘 형식

Android 알림을 일반적으로 (알림 왼쪽)에 원래 응용 프로그램의 아이콘을 표시 합니다. 그러나, 이미지 또는 사진이 알림을 표시할 수 있습니다 (한 *큰 아이콘*) 표준 작은 아이콘 대신 합니다. 예를 들어 메시징 응용 프로그램 사진 앱 아이콘 보다는 보낸 사람에 게 표시할 수 있습니다.

기본 Android 5.0 알림의 예로 &ndash; 작은 응용 프로그램 아이콘만 표시 됩니다.

![예제에서는 일반 알림](local-notifications-images/13-sample-notification.png)

다음은 큰 아이콘을 표시 하려면 수정한 후 알림 스크린 샷 및 &ndash; Xamarin 코드 원숭이의 이미지에서 만들어진 아이콘을 사용 하 여:

![큰 아이콘이 알림 예제](local-notifications-images/14-large-icon-sample.png)

알림을 큰 아이콘으로 표시 되 면 큰 아이콘의 오른쪽 아래 모서리에 배지도 작은 앱 아이콘 표시 되는지 확인 합니다.

알림 작성기 알림에서 큰 아이콘으로 이미지를 사용 하려면 호출 [SetLargeIcon](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetLargeIcon/) 메서드와 비트맵 이미지의를 전달 합니다. 와 달리 `SetSmallIcon`, `SetLargeIcon` 비트맵은 허용 합니다. 이미지 파일로 비트맵으로 변환 하려면 사용 된 [BitmapFactory](https://developer.xamarin.com/api/type/Android.Graphics.BitmapFactory/) 클래스입니다. 예를 들어:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

이 예제 코드에서 이미지 파일을 열고 **Resources/drawable/monkey_icon.png**, 비트맵으로 변환 하 고 결과 비트맵을 전달 `Notification.Builder`합니다. 일반적으로 원본 이미지 해상도 작은 아이콘에 보다 큰 &ndash; 훨씬 더 않고 합니다. 너무 큰 이미지에는 알림 게시를 지연 시킬 수 있는 불필요 한 크기 조정 작업 발생할 수 있습니다.
Android에서 알림 아이콘 크기에 대 한 자세한 내용은 [알림 아이콘](http://developer.android.com/design/style/iconography.html#notification)합니다.


### <a name="big-text-style"></a>큰 텍스트 스타일

*큰 텍스트* 스타일은 알림 긴 메시지를 표시 하기 위한 사용 하는 확장 된 레이아웃 템플릿입니다. 모든 확장 된 레이아웃 알림을 처럼 큰 텍스트 알림은 처음 간결 하 게 표시할 형식으로 표시 됩니다.

![예제 큰 텍스트 알림](local-notifications-images/15-big-text-notification.png)

이 형식에 2 개의 마침표로 종료 메시지의 일부만 표시 됩니다. 사용자가 알림을 끌면, 전체 알림 메시지를 표시 하려면 확장 합니다.

![확장 된 큰 텍스트 알림](local-notifications-images/16-big-text-expanded.png)

이 확장 된 레이아웃 형식에는 알림 메시지의 맨 아래에 요약 텍스트가 포함 됩니다. 최대 높이 *큰 텍스트* 알림은 256 dp 합니다.

만들려는 *큰 텍스트* 인스턴스화할 알림을 `Notification.Builder` 개체, 이전 처럼 로컬 폴더를 인스턴스화하고 추가 [BigTextStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigTextStyle/) 개체를 `Notification.Builder` 개체입니다. 예를 들어:

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

이 예제에서는 메시지 텍스트와 요약 텍스트에 저장 됩니다는 `BigTextStyle` 개체 (`textStyle`)에 전달 되기 전에 `Notification.Builder.`


### <a name="image-style"></a>이미지 스타일

*이미지* 스타일 (라고도 *큰 그림* 스타일)은 알림의 본문에 이미지를 표시 하는 데 사용할 수 있는 확장 된 알림 형식입니다. 예를 들어 스크린 샷 앱 또는 사진 앱 צ ְ ײ는 *이미지* 알림 스타일을 사용자에 게 마지막에 대 한 알림을 제공 하는 이미지 캡처 되었습니다. 최대 높이 *이미지* 알림은 256 dp &ndash; Android이 최대 높이 제한으로 사용 가능한 메모리의 한도 내에 맞게 이미지 크기를 조정 합니다.

모든 마찬가지로 레이아웃 알림 확장 *이미지* 알림이 먼저 포함 된 메시지 텍스트의 일부를 표시 하는 간단한 형식에 표시 됩니다.

![Compact 이미지 알림 없는 이미지를 보여 줍니다.](local-notifications-images/17-image-compact.png)

끌면는 *이미지* 알림, 이미지를 표시 하기 위해 확장 합니다. 예를 들어 다음은 이전 알림 확장된 버전이입니다.

![확대 한 이미지 알림 보여 주는 이미지](local-notifications-images/18-image-expanded.png)

알림을 압축 형식으로 표시 되 면 알림 텍스트가 표시 되는 알림 (알림 작성기에 전달 되는 텍스트 `SetContentText` 메서드를 앞에서 보았듯이). 그러나 이미지를 표시 하기 위해 확장 되는 알림 이미지 위에 요약 텍스트가 표시 됩니다.

만들려는 *이미지* 인스턴스화할 알림을 `Notification.Builder` 이전 처럼 개체 로컬 폴더를 만들고 삽입는 [BigPictureStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigPictureStyle/) 개체는 `Notification.Builder` 개체입니다. 예를 들어:

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

와 같은 `SetLargeIcon` 방식의 `Notification.Builder`, [BigPicture](https://developer.xamarin.com/api/member/Android.App.Notification+BigPictureStyle.BigPicture/) 방식의 `BigPictureStyle` 알림 메시지의 본문에 표시할 이미지의 비트맵 필요 합니다. 이 예제는 [DecodeResource](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeResource/(Android.Content.Res.Resources%2cSystem.Int32)) 방식의 `BitmapFactory` 이미지 파일에 있는 읽기 **Resources/drawable/x_bldg.png** 비트맵으로 변환 합니다.

리소스로 제공 되지 않는 이미지를 표시할 수도 있습니다. 다음 샘플 코드 로컬 SD 카드에서 이미지를 로드 하 고에 표시 하는 예를 들어 한 *이미지* 알림:

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

이 예제에서는 이미지 파일에 있는 **/sdcard/Pictures/my-tshirt.jpg** 로드, 원래 크기의 절반으로 크기가 조정 되어 다음 알림 영역에서 사용할 비트맵을 변환 합니다.

![알림 예제 티셔츠 이미지](local-notifications-images/19-tshirt-notification.png)

에 대 한 호출을 래핑하는 것이 좋습니다를은 이미지 파일의 크기를 미리 알지 못하는 경우 [BitmapFactory.DecodeFile](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeFile/p/System.String/Android.Graphics.BitmapFactory+Options/) 예외 처리기에서 &ndash; 는 `OutOfMemoryError` 이미지가 너무 커서에 대 한 경우 예외를 throw 될 수 있습니다 Android 크기를 조정 합니다.

로드 하 고 디코딩 큰 비트맵 이미지에 대 한 자세한 내용은 [부하 큰 비트맵 효율적으로](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently)합니다.


### <a name="inbox-style"></a>받은 편지함 스타일

*받은 편지함* 알림 메시지의 본문에 텍스트 (예: 전자 메일 받은 편지함 요약)의 별도 줄을 표시 하기 위한는 확장 된 레이아웃 템플릿 형식이 있습니다. *받은 편지함* 형식 알림이 압축 형식으로 먼저 표시 됩니다.

![예제 compact 받은 편지함 알림](local-notifications-images/20-inbox-compact.png)

사용자가 알림을 끌면, 아래 스크린샷에 표시 되는 전자 메일 요약을 표시 하려면 확장 합니다.

![예제에서는 받은 편지함 알림 확장](local-notifications-images/21-inbox-expanded.png)

만들려면는 *받은 편지함* 인스턴스화할 알림을 `Notification.Builder` 이전 처럼 개체를 추가 [InboxStyle](https://developer.xamarin.com/api/type/Android.App.Notification+InboxStyle/) 개체는 `Notification.Builder`합니다. 예를 들어:

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

알림 본문에 새 줄의 텍스트를 추가 하려면는 [Addline](https://developer.xamarin.com/api/member/Android.App.Notification+InboxStyle.AddLine/p/System.String/) 의 메서드는 `InboxStyle` 개체 (의 최대 높이 *받은 편지함* 알림은 256 dp). 유의 달리 *큰 텍스트* 스타일의 *받은 편지함* 스타일 알림 본문의 개별 줄의 텍스트를 지원 합니다.

사용할 수도 있습니다는 *받은 편지함* 알림에 확장명된 형식에 개별 줄의 텍스트를 표시 하는 스타일입니다. 예를 들어는 *받은 편지함* 알림 스타일 요약 알림에 보류 중인 여러 알림을 결합 데 사용할 수 &ndash; 단일을 업데이트할 수 있습니다 *받은 편지함* 새 알림 스타일 지정 알림 내용의 줄 (참조 [업데이트 알림을](#updating-a-notification) 위에) 대신, 새, 주로 비슷한 알림의 연속 스트림을 생성 하는 보다 합니다. 이 방법을 사용 하는 방법에 대 한 자세한 내용은 참조 [알림에 요약](http://developer.android.com/design/patterns/notifications.html#summarize_your_notifications)합니다.


## <a name="configuring-metadata"></a>메타데이터 구성

`Notification.Builder` 우선 순위, 표시 유형 및 범주와 같이 사용자 알림에 대 한 메타 데이터를 설정 하기 위해 호출할 수 있는 방법을 제공 합니다. 이 정보를 사용 하는 android &mdash; 사용자 기본 설정 함께 &mdash; 방법과 시기를 결정 하 알림을 표시 합니다.


### <a name="priority-settings"></a>우선 순위 설정

알림이 게시 될 때 알림의 우선 순위 설정을 두 개의 결과를 결정 합니다.

-   다른 알림 기준으로 알림을 표시할 위치입니다.
    예를 들어 우선 순위가 높은 알림은 위에서 설명한 알림 서랍에서 낮은 우선 순위 알림을에 관계 없이 각 알림에 게시할 때.

-   여부 Heads-up 알림 형식 (Android 5.0 이상)에 알림이 표시 됩니다. 만 *높은* 및 *최대* 헤드업 알림으로 우선 순위 알림이 표시 됩니다.

Xamarin.Android 알림 우선 순위를 설정 하기 위한 다음과 같은 열거형을 정의 합니다.

-   `NotificationPriority.Max` &ndash; 긴급 또는 위험 상태 (예를 들어 들어오는 호출, 턴 바이 턴 방향으로 또는 응급 경고)를 사용자를 게 알립니다. Android 5.0 및 이상 장치에 최대 우선 순위 알림이 화면 형식으로 표시 됩니다.

-   `NotificationPriority.High` &ndash; 중요 한 이벤트 (예: 중요 한 전자 메일 또는 실시간 채팅 메시지의 도착)의 사용자를 게 알립니다. Android 5.0 및 이상 장치에서 우선 순위가 높은 알림이 화면 형식으로 표시 됩니다.

-   `NotificationPriority.Default` &ndash; 보통 수준의 중요도가 하는 조건 사용자를 게 알립니다.

-   `NotificationPriority.Low` &ndash; 사용자가 제공 됩니다 (예: 소프트웨어 업데이트 알림 또는 소셜 네트워크 업데이트) 해야 급하지 않은 정보에 대 한 합니다.

-   `NotificationPriority.Min` &ndash; 경우에만 정보는 사용자는 배경 정보 (예: 위치 또는 날씨 정보) 알림을 볼 수 있습니다.

알림의 우선 순위를 설정 하려면 호출는 [SetPriority](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetPriority/) 의 메서드는 `Notification.Builder` 우선 순위 수준에 전달 하는 개체입니다. 예를 들어:

```csharp
builder.SetPriority (NotificationPriority.High);
```

다음 예에서는, 우선 순위가 높은 알림, "중요 한 메시지는!"에서 알림 서랍의 맨 위에 나타납니다.

![예제에서는 우선 순위가 높은 알림](local-notifications-images/22-hi-priority-drawer.png)

우선 순위가 높은 알림 이기 때문에 사용자의 현재 활동 화면 Android 5.0에서 위에 헤드업 알림으로 표시 됩니다.

![예제에서는 화면 알림](local-notifications-images/23-heads-up-example.png)

다음 예제에서는 우선 순위가 낮은 "는 날에 대 한 생각" 알림 우선 순위가 높은 배터리 수준 알림 아래에 표시 됩니다.

![예제에서는 우선 순위가 낮은 알림](local-notifications-images/24-lo-priority-drawer.png)

"는 날에 대 한 사고" 알림 우선 순위가 낮은 알림 이기 때문에 Android에에서 표시 되지 않습니다 것 Heads-up 형식입니다.


### <a name="visibility-settings"></a>표시 유형 설정

Android 5.0 이후부터 *가시성* 설정은 보안 잠금 화면에 표시 되는 알림 내용의 양을 제어를 사용할 수 있습니다.
Xamarin.Android 알림 표시 여부를 설정 하기 위한 다음과 같은 열거형을 정의 합니다.

-   `NotificationVisibility.Public` &ndash; 알림 메시지의 전체 내용은 보안 잠금 화면에 표시 됩니다.

-   `NotificationVisibility.Private` &ndash; 중요 한 정보만 (예: 알림 아이콘 및이 게시 하는 앱의 이름), 보안 잠금 화면에 표시 되는 있지만 나머지는 알림 세부 정보는 숨겨집니다. 기본적으로 모든 알림 `NotificationVisibility.Private`합니다.

-   `NotificationVisibility.Secret` &ndash; 보안 잠금 화면 알림 아이콘도 마찬가지에 아무 내용도 표시 합니다. 알림 콘텐츠를 사용자의 장치 잠금을 해제 한 후에 사용할 수 있습니다.

알림, 앱 호출의 표시 유형을 설정 하려면는 `SetVisibility` 의 메서드는 `Notification.Builder` 표시 유형 설정에 전달 하는 개체입니다. 예를 들어이 호출으로 `SetVisibility` 알림을 사용 하면 `Private`:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

경우는 `Private` 알림이 게시 될, 이름 및 응용 프로그램의 아이콘 보안 잠금 화면에 표시 됩니다. 알림 메시지 대신 "잠금 해제 장치를이 알림이 표시" 사용자에 게 표시 합니다.

![장치 알림 메시지를 잠금 해제](local-notifications-images/25-lockscreen-private.png)

이 예제에서는 **NotificationsLab** 원래 응용 프로그램의 이름입니다. 알림 교정된이 버전은 잠금 화면에서 안전한 경우에 표시 됩니다 (즉, PIN, 패턴 또는 암호를 통해 보안) &ndash; 는 잠금 화면 보안 없는 경우에 알림 메시지의 전체 내용을 잠금 화면에서 사용할 수는 있습니다.


### <a name="category-settings"></a>범주 설정

미리 정의 된 범주는 Android 5.0 이후부터, 순위 및 알림을 필터링에 사용할 수 있습니다. Xamarin.Android 이러한 범주에 대 한 다음과 같은 열거형을 제공합니다.

-   `Notification.CategoryCall` &ndash; 들어오는 전화 통화입니다.

-   `Notification.CategoryMessage` &ndash; 들어오는 텍스트 메시지입니다.

-   `Notification.CategoryAlarm` &ndash; 경보 조건 또는 타이머 만료 합니다.

-   `Notification.CategoryEmail` &ndash; 들어오는 전자 메일 메시지입니다.

-   `Notification.CategoryEvent` &ndash; 달력 이벤트입니다.

-   `Notification.CategoryPromo` &ndash; 프로 모션 메시지 또는 알림입니다.

-   `Notification.CategoryProgress` &ndash; 백그라운드 작업의 진행 상황입니다.

-   `Notification.CategorySocial` &ndash; 소셜 네트워킹 업데이트 합니다.

-   `Notification.CategoryError` &ndash; 백그라운드 작업이 나 인증 프로세스에 오류가 있습니다.

-   `Notification.CategoryTransport` &ndash; 미디어 재생 업데이트 합니다.

-   `Notification.CategorySystem` &ndash; (시스템 또는 장치 상태) 시스템 용도로 예약 되어 있습니다.

-   `Notification.CategoryService` &ndash; 백그라운드 서비스 실행 중임을 나타냅니다.

-   `Notification.CategoryRecommendation` &ndash; 현재 실행 중인 앱 관련 권장 사항 메시지입니다.

-   `Notification.CategoryStatus` &ndash; 장치에 대 한 정보입니다.

알림을 정렬 되는 알림 우선 순위 범주 설정에 따라 보다 우선 합니다. 예를 들어 우선 순위가 높은 알림으로 표시할 화면에 속하는 경우에는 `Promo` 범주입니다. 호출 하는 알림 범주를 설정 하려면는 `SetCategory` 의 메서드는 `Notification.Builder` 범주 설정에 전달 하는 개체입니다. 예를 들어:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

*방해 하지* 범주에 따라 알림을 필터링 하는 기능 (Android 5.0의 새로운). 예를 들어는 *방해 하지* 화면에서는 **설정** 전화 통화 및 메시지에 대 한 제외 알림 수 있습니다.

![화면 스위치를 받지 않음](local-notifications-images/26-do-not-disturb.png)

사용자 구성 하는 경우 *방해 하지* (볼 수 있듯이 위의 스크린) 전화 통화를 제외한 모든 인터럽트를 차단 하기 위해 Android 하면 범주 설정 하 여 알림이 `Notification.CategoryCall` 장치 하는 동안 표시 될 수 에 *방해 하지* 모드입니다. `Notification.CategoryAlarm` 알림을 차단 되지 않으므로 *방해 하지* 모드입니다.


<a name="compatibility" />

## <a name="compatibility"></a>호환성

앱을 만드는 경우 (초기에 API 수준 4), Android의 이전 버전에서 실행할 수도 사용할 것인가 [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) 클래스 대신 `Notification.Builder`합니다. 사용 하 여 알림을 만들 때 `NotificationCompat.Builder`, Android를 사용 하면 기본 알림 콘텐츠가 오래 된 장치에서 제대로 표시 됩니다. 그러나 일부 고급 알림 기능은 이전 버전의 Android에서 사용할 수 없기 때문에 코드에 대해 확장 된 알림 스타일, 범주 및 표시 유형 수준 아래에 설명 된 대로 호환성 문제가 명시적으로 처리 해야 합니다.

사용 하도록 `NotificationCompat.Builder` 응용 프로그램에 포함 해야는 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 프로젝트에서 NuGet 합니다.

다음 코드 예제를 사용 하 여 기본 알림을 만드는 예제 `NotificationCompat.Builder`:

```csharp
// Instantiate the builder and set notification elements:
NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();
```

중요 알림 옵션에 대 한 메서드 호출의 동일이 예제에서 알 수 있듯이, `Notification.Builder`합니다. 그러나 (다음 섹션에서 설명)는 더 복잡 한 알림에 하향 호환성 문제를 처리 하는 코드에 있습니다.

[LocalNotifications](https://developer.xamarin.com/samples/monodroid/LocalNotifications) 샘플에 사용 하는 방법을 보여 줍니다 `NotificationCompat.Builder` 를 알림에서 두 번째 활동을 시작 합니다. 이 샘플 코드에 대해서는 설명의 [Xamarin.Android에서 로컬 알림을 사용 하 여](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) 연습 합니다.


### <a name="notification-styles"></a>알림 스타일

만들려는 *큰 텍스트*, *이미지*, 또는 *받은 편지함* 스타일을 사용 하 여 알림을 `NotificationCompat.Builder`, 앱이이 스타일의 호환 버전을 사용 해야 합니다. 예를 들어, 사용 하 여 *큰 텍스트* 스타일, 인스턴스화할 `NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

마찬가지로, 응용 프로그램에서 사용할 수 `NotificationCompat.InboxStyle` 및 `NotificationCompat.BigPictureStyle` 에 대 한 *받은 편지함* 및 *이미지* 각각 스타일입니다.


### <a name="notification-priority-and-category"></a>알림 우선 순위와 범주

`NotificationCompat.Builder` 지원 된 `SetPriority` 메서드 (사용 가능한 Android 4.1 부터는). 그러나는 `SetCategory` 방법은 *하지* 에서 지 원하는 `NotificationCompat.Builder` 범주 Android 5.0에 도입 된 새 알림 메타 데이터 시스템에 속하기 때문에 있습니다.

Android, where의 이전 버전을 지원 하기 위해 `SetCategory` 는 사용할 수 없는 코드 수준을 확인할 수 있습니다는 API를 호출 하는 조건에 따라 런타임에 `SetCategory` API 수준 Android 5.0 (API 수준 21) 보다 크거나 같은 경우:

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

이 예제에서는 앱의 **대상 프레임 워크** Android 5.0로 설정 된 및 **최소 Android 버전** 로 설정 된 **Android 4.1 (API 수준 16)** 합니다. 때문에 `SetCategory` 는 API 수준 21 이상에서 사용할 수 있는,이 예제 코드를 호출 합니다 `SetCategory` 경우에 사용할 수 있는 &ndash; 호출 하지 것입니다 `SetCategory` API 수준 경우 미만
21.


### <a name="lockscreen-visibility"></a>잠금 화면 표시 유형

Android는 Android 5.0 (API 수준 21) 전에 잠금 화면 알림을 지원 하지 않은 `NotificationCompat.Builder` 지원 하지 않습니다는 `SetVisibility` 메서드. 에 대해 위에서 설명한 것 처럼 `SetCategory`, 코드에서 API 수준 런타임 및 호출을 확인할 수 `SetVisiblity` 경우에 사용할 수:

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetVisibility (Notification.Public);
}
```


## <a name="summary"></a>요약

이 문서는 Android에서 로컬 알림을 만드는 방법을 설명 합니다. 알림의 구조를 설명, 사용 하는 방법을 설명 하기 `Notification.Builder` 알림을 만듭니다. 큰 아이콘에서 스타일 알림 방법 *큰 텍스트*, *이미지* 및 *받은 편지함*  형식, 우선 순위, 표시 유형 및 범주와 같은 메타 데이터 설정 알림을 설정 하는 방법 및 알림에서 활동을 시작 하는 방법입니다. 이 문서도 이러한 알림 설정을 새 화면, 잠금 화면을 사용 하는 방법을 설명 하 고 *방해 하지* Android 5.0에 도입 된 기능입니다. 마지막으로 사용 하는 방법을 배웠습니다 `NotificationCompat.Builder` 알림 Android의 이전 버전과 호환성을 유지 합니다.

Android 용 알림 디자인에 대 한 지침을 참조 하십시오. [알림](http://developer.android.com/preview/notifications.html)합니다.


## <a name="related-links"></a>관련 링크

- [NotificationsLab (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/NotificationsLab/)
- [LocalNotifications (샘플)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Android 연습에서 로컬 알림](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [알림](http://developer.android.com/design/patterns/notifications.html)
- [사용자에 게 알리는](http://developer.android.com/training/notify-user/index.html)
- [알림](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
