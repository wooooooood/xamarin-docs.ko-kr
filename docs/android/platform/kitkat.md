---
title: KitKat 기능
description: Android 4.4 (KitKat) 제공을 사용자 및 개발자를 위한 기능을 사용 하 여 로드 됩니다. 이 가이드는 이러한 기능 중 일부를 강조 표시 하 고 코드 예제 및 KitKat 최대한를 내릴 수 있도록 구현 세부 정보를 제공 합니다.
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: b3981572e4d2629fd88d1e255fc7459bfe8912f1
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669013"
---
# <a name="kitkat-features"></a>KitKat 기능

_Android 4.4 (KitKat) 제공을 사용자 및 개발자를 위한 기능을 사용 하 여 로드 됩니다. 이 가이드는 이러한 기능 중 일부를 강조 표시 하 고 코드 예제 및 KitKat 최대한를 내릴 수 있도록 구현 세부 정보를 제공 합니다._

## <a name="overview"></a>개요

Android 4.4 (API 수준 19) 라고도 "KitKat"가, 런타임에 2013에서 릴리스 되었습니다. KitKat은 다양 한 새로운 기능과 향상 된 기능을 포함 하 여 제공 합니다.

-  [사용자 환경](#user_experience) &ndash; 전환 프레임 워크, 반투명 상태 및 탐색 모음 및 몰입 형 전체 화면 모드를 사용 하 여 쉽게 애니메이션 사용자에 대 한 더 나은 환경을 만드는 데 도움이 됩니다.

-  [사용자가 콘텐츠](#user_content) &ndash; 저장소 액세스 프레임 워크를 사용 하 여 사용자 파일 관리 간소화;는 향상 된 인쇄 Api 사용 하 여 사진을 인쇄, 웹 사이트 및 기타 콘텐츠 쉽습니다.

-  [하드웨어](#hardware) &ndash; NFC 카드를 카드 에뮬레이션 호스트 기반 NFC 사용 하 여에 앱을 전환; 저전력 센서와 실행을 `SensorManager` 입니다.

-  [개발자 도구](#developer_tools) &ndash; 스크린 캐스트 응용 프로그램 사용 가능한 Android Debug Bridge 클라이언트를 사용 하 여 실행 중인 Android SDK의 일부분으로 합니다.


이 가이드에서는 Xamarin.Android 개발자를 위한 KitKat, 뿐만 아니라 KitKat의 대략적인 개요를 기존 Xamarin.Android 응용 프로그램을 마이그레이션하기 위한 지침을 제공 합니다.

## <a name="requirements"></a>요구 사항

KitKat를 사용 하 여 Xamarin.Android 응용 프로그램을 개발 하려면 *Xamarin.Android 4.11.0* 또는 더 높은 및 Android 4.4 (API 수준 19) 다음 스크린샷에 표시 된 대로 Android SDK Manager를 통해를 설치 합니다.

[![Android SDK Manager에서 Android 4.4 선택](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>KitKat로 앱 마이그레이션

이 섹션에서는 Android 4.4를 기존 응용 프로그램 전환 하는 데 첫 번째 응답 항목 일부를 제공 합니다.

### <a name="check-system-version"></a>시스템 버전 확인

응용 프로그램을 이전 버전의 Android와 호환 되도록 해야 하는 경우에 다음 코드 예제에 나온 것 처럼 KitKat 별 코드 시스템 버전 검사를 래핑할 야 합니다.

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>경보 일괄 처리

Android 경보 services를 사용 하 여 지정된 된 시간에 백그라운드에서 앱을 다시 시작 합니다. KitKat은 전원 유지 하는 경보를 일괄 처리 하 여 한 단계 더 발전 합니다. 이 정확한 시간에 각 앱을 깨우, 대신 KitKat 하 길 원하는 동일한 시간 간격 동안 절전 모드를 해제 하는 동시에 절전 모드 해제를 등록 하는 응용 프로그램 그룹을 의미 합니다.
Android 앱 지정된 된 시간 간격 동안 절전 모드를 이야기, 호출 `SetWindow` 에 [ `AlarmManager` ](https://developer.xamarin.com/api/type/Android.App.AlarmManager/)수행할 작업을 최소 및 최대 시간 (밀리초)를 하기 전에 경과할 수 절전 모드에서 앱에 전달 절전 모드 해제 합니다.
다음 코드는 30 분 사이의 시간 창 설정 된 시간 해제 해야 하는 응용 프로그램의 예제를 제공 합니다.

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

정확한 시간에 앱을 깨우를 계속 하려면 사용 하 여 `SetExact`앱을 해제 해야 하는 정확한 시간에 수행할 작업을 전달 합니다.

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

더 이상 KitKat 정확한 반복 경보를 설정할 수 없습니다. 사용 하는 응용 프로그램 [`SetRepeating`](https://developer.xamarin.com/api/member/Android.App.AlarmManager.SetRepeating/p/Android.App.AlarmType/System.Int64/System.Int64/Android.App.PendingIntent/)
및 정확한 경보를 수동으로 각 경보를 트리거하는 지금 필요한 작업에 필요 합니다.

### <a name="external-storage"></a>외부 스토리지

외부 저장소는 이제 두 형식을-응용 프로그램 및 여러 응용 프로그램에서 공유 데이터에 고유한 저장소 나뉩니다. 읽기 및 쓰기 앱의 특정 위치를 외부 저장소에 없는 특별 한 권한이 필요 합니다. 이제 공유 저장소에 데이터와 상호 작용 해야 합니다 `READ_EXTERNAL_STORAGE` 또는 `WRITE_EXTERNAL_STORAGE` 권한. 두 유형으로 분류할 수 있습니다.

-  표시 되는 경우 파일 또는 디렉터리 경로에서 메서드를 호출 하 여 `Context` -예를 들어, [`GetExternalFilesDir`](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalFilesDir/p/System.String/)
   또는 [`GetExternalCacheDirs`](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalCacheDirs%28%29/)
   - 앱 추가 권한이 필요합니다.

-  표시 되는 경우 파일 또는 디렉터리 경로 속성에 액세스 하거나에서 메서드를 호출 하 여 `Environment` 와 같은 [`GetExternalStorageDirectory`](https://developer.xamarin.com/api/property/Android.OS.Environment.ExternalStorageDirectory/)
   또는 [`GetExternalStoragePublicDirectory`](https://developer.xamarin.com/api/member/Android.OS.Environment.GetExternalStoragePublicDirectory/p/System.String/)
   를 앱에 필요한 합니다 `READ_EXTERNAL_STORAGE` 또는 `WRITE_EXTERNAL_STORAGE` 권한.

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE` 의미를 `READ_EXTERNAL_STORAGE` 권한이 있으므로 하나의 사용 권한을 설정 해야 합니다.

### <a name="sms-consolidation"></a>SMS 통합

KitKat은 사용자가 선택한 기본 응용 프로그램에서 모든 SMS 콘텐츠를 집계 하 여 사용자에 대 한 메시징 간소화 합니다. 개발자는 앱의 기본 메시징 응용 프로그램으로 선택할 수 있는 만들고 작동 적절 하 게 수명 및 코드에서 응용 프로그램을 선택 하지 않으면 하는 일을 담당 합니다. KitKat SMS 앱의 전환에 대 한 자세한 내용은 참조는 [Your SMS 앱 준비 KitKat](http://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) Google에서 가이드입니다.

### <a name="webview-apps"></a>WebView 앱

[WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) KitKat에는 변경 되었습니다. 가장 크게 변화 된 보안 콘텐츠를 로드 하기 위한 추가 되는 `WebView`합니다. 이전 API 버전을 대상으로 하는 대부분의 응용 프로그램 정상적으로 작동 해야 하는 동안 테스트를 사용 하는 응용 프로그램을 `WebView` 클래스 것이 좋습니다. 영향을 받는 WebView Api에 대 한 자세한 내용은 Android 참조 [Android 4.4에서 WebView로](https://developer.android.com/guide/webapps/migrating.html) 설명서.

<a name="user_experience" />

## <a name="user-experience"></a>사용자 경험

KitKat 속성 애니메이션 및 테마에 대 한 반투명 UI 옵션을 처리 하기 위한 새로운 전환 프레임 워크를 비롯 하 여 사용자 환경을 향상 시키기 위해 몇 가지 새로운 Api를 사용 하 여 제공 됩니다. 이러한 변경 내용은 아래에서 다룹니다.

### <a name="transition-framework"></a>전환 프레임 워크

전환 프레임 워크를 사용 하면 애니메이션 보다 쉽게 구현할 수 있습니다. KitKat를 사용 하면 한 줄의 코드를 사용 하 여 간단한 속성 애니메이션을 수행 하거나 전환을 사용 하 여 사용자 지정 *장면*합니다.

#### <a name="simple-property-animation"></a>단순 속성 애니메이션

새 Android 전환 라이브러리 코드 숨김 속성 애니메이션을 간소화합니다. 프레임 워크를 사용 하면 최소한의 코드를 사용 하 여 간단한 애니메이션을 수행할 수 있습니다. 예를 들어 다음 코드 예제에서는 [`TransitionManager.BeginDelayedTransition`](https://developer.xamarin.com/api/member/Android.Transitions.TransitionManager.BeginDelayedTransition/p/Android.Views.ViewGroup/Android.Transitions.Transition/)
표시 및 숨기기 애니메이션 효과 `TextView`:

```csharp
using Android.Transitions;

public class MainActivity : Activity
{
    LinearLayout linear;
    Button button;
    TextView text;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        linear = FindViewById<LinearLayout> (Resource.Id.linearLayout);
        button = FindViewById<Button> (Resource.Id.button);
        text = FindViewById<TextView> (Resource.Id.textView);

        button.Click += (o, e) => {

            TransitionManager.BeginDelayedTransition (linear);

            if(text.Visibility != ViewStates.Visible)
            {
                text.Visibility = ViewStates.Visible;
            }
            else
            {
                text.Visibility = ViewStates.Invisible;
            }
        };
    }
}
```

위의 예제에서는 전환 프레임을 사용 하 여 기본 전환 변경 속성 값을 자동을 만듭니다. 애니메이션에서는 단일 코드 줄을 통해 처리 하기 때문에 쉽게 수행할 수 있습니다이 이전 버전의 Android 호환 래핑하여는 `BeginDelayedTransition` 시스템 버전 검사를 호출 합니다. 참조 된 [Your App을 KitKat에 마이그레이션](#Migrating_Your_App_to_KitKat) 자세한 섹션입니다.

아래 스크린샷에서 애니메이션 전의 앱을 보여 줍니다.

[![애니메이션 시작 하기 전에 앱 스크린 샷](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

아래 스크린샷에서 애니메이션 후 앱을 보여 줍니다.

[![애니메이션이 완료 된 후 앱 스크린 샷](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

다음 섹션에 나와 있는 작업을 사용 하 여 전환 보다 자세히 제어를 얻을 수 있습니다.

#### <a name="android-scenes"></a>Android 장면

[장면](https://developer.xamarin.com/api/type/Android.Transitions.Scene/) 개발자 애니메이션을 더 많이 제어할 수 있도록 전환 프레임 워크의 일부로 도입 되었습니다. 백그라운드에서 UI에 동적 영역을 만들: 컨테이너 및 몇 가지 버전, 또는 "작업" 컨테이너 내 XML 콘텐츠에 대 한을 지정 하 고 Android의 작업을 수행 하는 작업 간의 전환에 애니메이션 효과 주기 나머지 작업을 수행 합니다. Android 장면 개발 측면에서 최소한의 작업을 사용 하 여 복잡 한 애니메이션을 빌드할 수 있습니다.

동적 콘텐츠를 보유할 수 있도록 정적 UI 요소 라고 한 *컨테이너* 또는 *장면 기본*입니다. 아래 예제에서는 Android Designer를 사용 하 여 만듭니다는 `RelativeLayout` 호출 `container`:

[![Android Designer를 사용 하 여 RelativeLayout 컨테이너를 만들려면](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

샘플 레이아웃 정의 이라는 단추가 `sceneButton` 아래는 `container`합니다. 이 단추는 전환이 트리거됩니다.

컨테이너 내에서 동적 콘텐츠를 두 개의 새 Android 레이아웃에 필요합니다. 이러한 레이아웃 지정 코드만 *내부* 컨테이너입니다.
호출 하는 레이아웃을 정의 하는 아래 예제 코드 *Scene1* 및 포함 하는 두 텍스트 필드 "키트" 읽기 "캐 탈" 각각 및 레이아웃을 두 번째 호출 *Scene2* 되돌릴 동일한 텍스트 필드가 들어 있는입니다. XML은 다음과 같습니다.

 **Scene1.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kit"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textA"
        android:text="Kat"
        android:textSize="35sp" />
</merge>
```

 **Scene2.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kat"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textB"
        android:text="Kit"
        android:textSize="35sp" />
</merge>
```

사용 하 여 위의 예제 `merge` 짧은 코드 보기를 확인 하 여 뷰 계층 구조를 단순화 합니다. 에 대 한 자세한 내용은 `merge` 레이아웃 [여기](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html)합니다.

호출 하 여 장면 만들어집니다 [ `Scene.GetSceneForLayout` ](https://developer.xamarin.com/api/member/Android.Transitions.Scene.GetSceneForLayout/p/Android.Views.ViewGroup/System.Int32/Android.Content.Context/)컨테이너 개체는 장면 레이아웃 파일 및 현재 리소스 ID에 전달 `Context`아래 코드 예제에 나온 것 처럼,:

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

단추를 클릭 하는 Android 기본값으로 전환 애니메이션을 적용 하는 두 장면 사이의 대칭 이동:

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

아래 스크린샷에서 애니메이션 전의 장면을 보여 줍니다.

[![애니메이션을 시작 하기 전에 앱의 스크린 샷](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

아래 스크린샷에서 후 애니메이션 장면을 보여 줍니다.

[![애니메이션이 완료 된 후 앱의 스크린 샷](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)


> [!NOTE]
> [알려진된 버그가](https://code.google.com/p/android/issues/detail?id=62450) Android 전환의 백그라운드에서 발생 시키는 라이브러리를 사용 하 여 만든 `GetSceneForLayout` 사용자 두 번째 활동을 통해 이동 하면 중단 합니다. Java 해결 방법이 설명 되어 [여기](http://www.doubleencore.com/2013/11/new-transitions-framework/)합니다.


##### <a name="custom-transitions-in-scenes"></a>장면에 사용자 지정 전환

사용자 지정 전환의 xml 리소스 파일에서 정의할 수 있습니다 합니다 `transition` 디렉터리 아래 `Resources`아래 스크린샷에 나온 것 처럼,:

[![리소스/전환 디렉터리 transition.xml 파일의 위치](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

다음 코드 샘플 5 초간 애니메이션을 적용 하며 사용 하 여 전환을 정의 합니다 [보간 복원 지점을 지나친](https://developer.android.com/reference/android/views/animation/OvershootInterpolator.html):

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

작업의 전환이 만들어집니다를 사용 하는 [TransitionInflater](https://developer.xamarin.com/api/type/Android.Transitions.TransitionInflater/)아래 코드에 나온 것 처럼,:

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

새 전환에 추가 되는 `Go` 애니메이션을 시작 하는 호출:

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>반투명 UI

KitKat 제어할 수 더 테마 선택적 반투명 상태 및 탐색 막대를 사용 하 여 앱. Android 테마를 정의 하는 데 동일한 XML 파일에서 시스템 UI 요소의 합니다 반투명도 변경할 수 있습니다. KitKat 다음 속성을 제공합니다.

-  `windowTranslucentStatus` -로 설정 하면 true를 사용 하면 상위 상태 표시줄 반투명 합니다.

-  `windowTranslucentNavigation` -로 설정 하면 true를 사용 하면 아래쪽 탐색 모음 반투명 합니다.

-  `fitsSystemWindows` -Transcluent를 위쪽 이나 아래쪽 막대를 설정 합니다. 기본적으로 투명 하 게 UI 요소 아래에서 콘텐츠를 이동 합니다. 이 속성을 설정 `true` 겹침을 방지 하기 콘텐츠 반투명 시스템 UI 요소를 사용 하 여 간단한 방법이 있습니다.


다음 코드는 반투명 상태 및 탐색 막대를 사용 하 여 테마를 정의 합니다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <style name="KitKatTheme" parent="android:Theme.Holo.Light">
        <item name="android:windowBackground">@color/xamgray</item>
        <item name="android:windowTranslucentStatus">true</item>
        <item name="android:windowTranslucentNavigation">true</item>
        <item name="android:fitsSystemWindows">true</item>
        <item name="android:actionBarStyle">@style/ActionBar.Solid.KitKat</item>
    </style>

    <style name="ActionBar.Solid.KitKat" parent="@android:style/Widget.Holo.Light.ActionBar.Solid">
        <item name="android:background">@color/xampurple</item>
    </style>
</resources>
```

아래 스크린샷에서 위에 반투명 상태 및 탐색 모음을 사용 하 여 테마를 보여 줍니다.

[![반투명 상태 및 탐색 막대를 사용 하 여 앱의 스크린샷 예제](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content" />

## <a name="user-content"></a>사용자 콘텐츠

### <a name="storage-access-framework"></a>저장소 액세스 프레임 워크

저장소 액세스 프레임 워크 (SAF)는 이미지, 비디오, 문서 등 저장 된 콘텐츠가와 상호 작용 하는 사용자는 새로운 방법입니다. KitKat은 콘텐츠를 처리 하도록 응용 프로그램을 선택 하는 대화 상자를 사용 하 여 사용자를 표시 하는 대신 하나의 집계 위치에서 해당 데이터에 액세스할 수 있도록 새 UI를 엽니다. 콘텐츠를 선택한 후 사용자가 콘텐츠를 요청한 응용 프로그램에 반환 하 고 앱 환경을 정상적으로 계속 됩니다.

이 변경 개발자 쪽에서 두 작업을 해야: 먼저 앱 공급자의 콘텐츠를 필요로 하는 요청 콘텐츠는 새로운 방법이 업데이트 해야 합니다. 데이터를 기록 하는 두 번째 응용 프로그램을 `ContentProvider` 새 프레임 워크를 사용 하도록 수정 해야 합니다. 두 시나리오 모두 새 달라 집니다. [`DocumentsProvider`](https://developer.xamarin.com/api/type/Android.Provider.DocumentsProvider/)
API.

#### <a name="documentsprovider"></a>DocumentsProvider

KitKat와 상호 작용에에서 `ContentProviders` 는 사용 하 여 추상화는 `DocumentsProvider` 클래스입니다. 즉,는 SAF 중요 데이터를 실제로 통해 액세스할 수 있는 것으로 `DocumentsProvider` API. 로컬 공급자 클라우드 서비스 및 외부 저장 장치 인터페이스를 동일 하 고 동일 하 게 처리 됩니다 모든 사용 사용자의 콘텐츠와 상호 작용 하 여 한 곳을 사용 하 여 사용자 및 개발자를 제공 합니다.

이 섹션에서는 로드 하 고 저장소 액세스 프레임 워크를 사용 하 여 콘텐츠를 저장 하는 방법에 설명 합니다.

#### <a name="request-content-from-a-provider"></a>공급자에서 콘텐츠를 요청 합니다.

말씀 KitKat SAF UI를 사용 하 여 콘텐츠를 선택 하려고 한다는 것을 `ActionOpenDocument` 의도 장치에 사용할 수 있는 모든 콘텐츠 공급자에 연결 하려고 한다는 것을 의미 하는 합니다. 지정 하 여이 의도 필터링을 추가할 수 있습니다 `CategoryOpenable`, 즉, (즉, 액세스할 수 있는, 사용 가능한 콘텐츠) 열 수 있는 내용만 반환 됩니다. KitKat도 필터링 할 수 있습니다 사용 하 여 콘텐츠를 `MimeType`입니다. 이미지를 지정 하 여 이미지가 결과 대 한 필터 아래 코드 예를 들어 `MimeType`:

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

호출 `StartActivityForResult` 사용자 이미지를 선택 하 여 탐색할 수 있는 SAF UI를 시작 합니다.

[![이미지 검색에 대 한 저장소 액세스 프레임 워크를 사용 하는 앱의 스크린샷 예제](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

사용자가 이미지를 선택한 후 `OnActivityResult` 반환 된 `Android.Net.Uri` 선택한 파일의 합니다. 다음 코드 예제에는 사용자의 이미지 선택을 표시 됩니다.

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == save_request_code) {
        imageView = FindViewById<ImageView> (Resource.Id.imageView);
        imageView.SetImageURI (data.Data);
    }
}
```

#### <a name="write-content-to-a-provider"></a>공급자에 콘텐츠를 작성 합니다.

SAF UI에서 콘텐츠를 로드 하는 것 외에도 KitKat 있습니다에 콘텐츠를 저장할 `ContentProvider` 를 구현 하는 `DocumentProvider` API. 사용 하 여 콘텐츠를 저장 하는 `Intent` 사용 하 여 `ActionCreateDocument`:

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

위의 코드 샘플 파일 이름을 바꾸고 새 파일을 저장할 디렉터리를 선택 합니다. 사용자에 게 알리는 SAF UI를 로드 합니다.

[![NewDoc 다운로드 디렉터리에 파일 이름을 변경 하는 사용자의 스크린샷](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

누를 때 **저장할**, `OnActivityResult` 전달 되는 `Android.Net.Uri` 사용 하 여 액세스할 수 있는 새로 만든된 파일의 `data.Data`합니다. 새 파일에 데이터를 스트리밍하려면 uri를 사용할 수 있습니다.

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == write_request_code) {
        using (Stream stream = ContentResolver.OpenOutputStream(data.Data)) {
            Encoding u8 = Encoding.UTF8;
            string content = "Hello, world!";
            stream.Write (u8.GetBytes(content), 0, content.Length);
        }
    }
}
```

Note는 [`ContentResolver.OpenOutputStream(Android.Net.Uri)`](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.OpenOutputStream/(Android.Net.Uri))
반환 된 `System.IO.Stream`이므로.NET 전체 스트리밍 프로세스를 작성할 수 있습니다.

로드에 대 한 자세한 내용은 만들기 및 편집 저장소 액세스 프레임 워크를 사용 하 여 콘텐츠를 참조 합니다 [저장소 액세스 프레임 워크에 대 한 Android 설명서](https://developer.android.com/guide/topics/providers/document-provider.html)합니다.

### <a name="printing"></a>인쇄

인쇄 컨텐트의 도입으로 KitKat 간소화 되어는 [인쇄 서비스](https://developer.xamarin.com/api/namespace/Android.PrintServices/) 고 `PrintManager`입니다. KitKat 완벽 하 게 활용 하 여 첫 번째 API 버전 이기도 합니다 [Google 클라우드 인쇄 서비스 Api](https://developers.google.com/cloud-print/) 사용 하 여는 [Google 클라우드 인쇄 응용 프로그램](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint)합니다.
Google 클라우드 인쇄 앱을 다운로드 하는 자동으로 KitKat 함께 제공 되는 대부분의 장치 및 [HP 인쇄 서비스 플러그 인](https://play.google.com/store/apps/details?id=com.hp.android.printservice)처음 연결할 때 WiFi 합니다. 사용자로 이동 하 여 자신의 장치 인쇄 설정을 확인할 수 있습니다 **설정 > 시스템 > 인쇄**:

[![인쇄 설정 화면 스크린샷 예제](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)


> [!NOTE]
> 인쇄 Api는 기본적으로 Google 클라우드 인쇄와 작동 하도록 설정 됩니다, 있지만 Android에 여전히 개발자를 새로운 Api를 사용 하 여 인쇄 콘텐츠를 준비 하 고 인쇄를 처리 하도록 다른 응용 프로그램에 보낼 수 있습니다.



#### <a name="printing-html-content"></a>HTML 콘텐츠 인쇄

KitKat 자동으로 만듭니다는 [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) 포함 된 웹 보기에 대 한 `WebView.CreatePrintDocumentAdapter`합니다. 인쇄 웹 콘텐츠는 간에 조정 된 작업을 [ `WebViewClient` ](https://developer.xamarin.com/api/type/Android.Webkit.WebViewClient/) HTML 콘텐츠를 로드 될 때까지 대기 하 고 옵션 메뉴에서 인쇄 옵션을 사용 가능 알고 활동 및 사용자가 될 때까지 대기 하는 활동을 수 있습니다. 선택 하 고 인쇄 옵션 호출 `Print`에 `PrintManager`합니다. 이 섹션에서는 화면에 인쇄 하는 데 필요한 기본 설정을 설명 HTML 콘텐츠입니다.

Note 로드 및 웹 콘텐츠를 인쇄 하려면 인터넷 권한이 필요 합니다.

[![응용 프로그램 옵션의 인터넷 권한 설정](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>인쇄 메뉴 항목

인쇄 옵션을 작업에 일반적으로 표시 됩니다 [[옵션] 메뉴](https://developer.android.com/guide/topics/ui/menus.html#options-menu)합니다.
[옵션] 메뉴 작업에서 작업을 수행할 수 있도록 합니다. 화면 오른쪽 상단에서 이며 다음과 같이 표시 됩니다.

[![화면의 오른쪽 위 모서리에 표시 된 인쇄 메뉴 항목의 예제 스크린샷](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)


추가 메뉴 항목을 정의할 수 있습니다 합니다 *메뉴*아래에 있는 디렉터리 *리소스*합니다. 아래 코드 정의 항목 이라는 샘플 메뉴가 [인쇄](https://developer.xamarin.com/api/type/Android.Print.PrintManager/):

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

통해 수행 된 작업의 옵션 메뉴를 사용 하 여 상호 작용 합니다 `OnCreateOptionsMenu` 고 `OnOptionsItemSelected` 메서드.
`OnCreateOptionsMenu` 가에서 인쇄 옵션 등 새 메뉴 항목을 추가 하는 위치를 *메뉴* 리소스 디렉터리입니다.
`OnOptionsItemSelected` 메뉴에서 인쇄 옵션을 선택 하면 사용자에 대해 수신 하 고 인쇄를 시작 합니다.

```csharp
bool dataLoaded;

public override bool OnCreateOptionsMenu (IMenu menu)
{
    base.OnCreateOptionsMenu (menu);
    if (dataLoaded) {
        MenuInflater.Inflate (Resource.Menu.print, menu);
    }
    return true;
}

public override bool OnOptionsItemSelected (IMenuItem item)
{
    if (item.ItemId == Resource.Id.menu_print) {
        PrintPage ();
        return true;
    }
    return base.OnOptionsItemSelected (item);
}
```

위의 코드 라는 변수도 정의 `dataLoaded` HTML 콘텐츠의 상태를 추적할 수 있습니다. `WebViewClient` 이 변수를 인쇄 메뉴 항목 [옵션] 메뉴에 추가할 작업에서 알 수 있도록 모든 콘텐츠를 로드 하는 경우 true로 설정 합니다.

##### <a name="webviewclient"></a>WebViewClient

작업을 `WebViewClient` 에서 데이터를 확인 하는 것을 `WebView` 인쇄 옵션을 사용 하 여 수행 되는 메뉴에 표시 되기 전에 완전히 로드 되는 `OnPageFinished` 메서드. `OnPageFinished` 로드를 완료할 수 있는 웹 콘텐츠 수신 하 고이를 사용 하 여 해당 옵션 메뉴를 다시 만드는 작업을 지시 `InvalidateOptionsMenu`:

```csharp
class MyWebViewClient : WebViewClient
{
    PrintHtmlActivity caller;

    public MyWebViewClient (PrintHtmlActivity caller)
    {
        this.caller = caller;
    }

    public override void OnPageFinished (WebView view, string url)
    {
        caller.dataLoaded = true;
        caller.InvalidateOptionsMenu ();
    }
}
```

`OnPageFinished` 또한 설정 합니다 `dataLoaded` 값을 `true`이므로 `OnCreateOptionsMenu` 곳에서 인쇄 옵션을 사용 하 여 메뉴를 다시 만들 수 있습니다.

##### <a name="printmanager"></a>PrintManager

내용을 인쇄 하는 다음 코드 예제는 `WebView`:

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print` 인수로: ("MyWebPage"이 예제의), 인쇄 작업의 이름을 [`PrintDocumentAdapter`](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/)
콘텐츠에서 인쇄 문서를 생성 하 고 [`PrintAttributes`](https://developer.xamarin.com/api/type/Android.Print.PrintAttributes/)
(`null` 위의 예제에서). 지정할 수 있습니다 `PrintAttributes` 문제가 해결 되는 콘텐츠 페이지의 인쇄 레이아웃 대부분의 시나리오를 처리 하는 기본 특성을 합니다.

호출 `Print` 인쇄 작업에 대 한 옵션을 나열 하는 인쇄 UI를 로드 합니다. UI 아래 스크린샷에서 나온 것 처럼 사용자의 인쇄 하거나 HTML 콘텐츠를 pdf로 저장 옵션을 제공 합니다.

[![스크린 샷의 PrintHtmlActivity 인쇄 메뉴 표시](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![스크린 샷의 PrintHtmlActivity 저장 PDF 메뉴 표시](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware" />

## <a name="hardware"></a>하드웨어

KitKat 새 장치 기능을 수용 하기 위해 여러 Api를 추가 합니다. 이 중 가장 주목할 만한은 카드 에뮬레이션 호스트 기반 및 새 `SensorManager`합니다.

### <a name="host-based-card-emulation-in-nfc"></a>NFC에서 호스트 기반 카드 에뮬레이션

호스트 기반 카드 에뮬레이션 (HCE) 운송 업체의 독점 보안 요소에 의존 하지 않고 NFC 카드나 NFC 카드 판독기 처럼 동작 하도록 응용 프로그램을 허용 합니다. HCE를 설정 하기 전에 확인 HCE 사용 하 여 장치에서 사용할 수 있는 `PackageManager.HasSystemFeature`:

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

HCE 해야 HCE 기능 및 `Nfc` 권한을 응용 프로그램을 사용 하 여 등록할 `AndroidManifest.xml`:

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![응용 프로그램 옵션에서 NFC 사용 권한 설정](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

하려면 HCE를 백그라운드에서 실행할 수 있고 HCE를 사용 하 여 응용 프로그램 실행 하지 않는 경우에 사용자는 NFC 트랜잭션 하면 시작 해야 합니다. HCE 코드를 작성 하 여이 작업을 수행할 수 있습니다 것을 `Service`입니다. HCE 서비스를 구현 하는 `HostApduService` 다음 메서드를 구현 하는 인터페이스:

-  *ProcessCommandApdu* -NFC 판독기와 HCE 서비스 간에 어떤 정보가 전송 되는지는 응용 프로그램 프로토콜 데이터 단위 (APDU) 됩니다. 이 메서드는 ADPU 판독기에서 사용 하 고 응답에서 데이터 단위를 반환 합니다.

-  *OnDeactivated* - `HostAdpuService` HCE 서비스 NFC 판독기와 통신 하는 더 이상 경우 비활성화 됩니다.


HCE 서비스를 응용 프로그램의 매니페스트를 사용 하 여 등록 되 고 적절 한 사용 권한, 의도 필터 및 메타 데이터를 사용 하 여 데코레이팅 해야 합니다. 다음 코드는 예가 `HostApduService` Android 매니페스트를 사용 하 여 등록 합니다 `Service` 특성 (특성에 대 한 자세한 내용은 Xamarin 참조 [Android 매니페스트 사용](~/android/platform/android-manifest.md) 가이드):

```csharp
[Service(Exported=true, Permission="android.permissions.BIND_NFC_SERVICE"),
    IntentFilter(new[] {"android.nfc.cardemulation.HOST_APDU_SERVICE"}),
    MetaData("android.nfc.cardemulation.host.apdu_service",
    Resource="@xml/hceservice")]

class HceService : HostApduService
{
    public override byte[] ProcessCommandApdu(byte[] apdu, Bundle extras)
    {
        ...
    }

    public override void OnDeactivated (DeactivationReason reason)
    {
        ...
    }
}
```

위의 서비스 NFC 판독기 응용 프로그램과 상호 작용 하는 방법을 제공 하지만 NFC 판독기에는 아직이 서비스를 검색 해야 하는 NFC 카드를 에뮬레이션 하는 경우 알 수 없습니다. 서비스를 식별 하는 NFC 판독기를 위해 할당할 수 있습니다 서비스는 고유한 *응용 프로그램 ID (보조)* 합니다. HCE 서비스에 대 한 다른 메타 데이터와 함께 지 원하는 지정 된 xml 리소스 파일에서 사용 하 여 등록 된 `MetaData` 특성 (위의 코드 예제 참조). 이 리소스 파일 하나 이상의 보조 필터를-하나 이상의 NFC 판독기 장치의 보조 기능에 해당 하는 16 진수 형식의 고유 식별자 문자열을 지정 합니다.

```xml
<host-apdu-service xmlns:android="http://schemas.android.com/apk/res/android"
    android:description="@string/hce_service_description"
    android:requireDeviceUnlock="false"
    android:apduServiceBanner="@drawable/service_banner">
    <aid-group android:description="@string/aid_group_description"
                android:category="payment">
        <aid-filter android:name="1111111111111111"/>
        <aid-filter android:name="0123456789012345"/>
    </aid-group>
</host-apdu-service>
```

지원 그룹 (및 "기타" 지불 응용 프로그램)를 지정 xml 리소스 파일에 설명 사용자 측 HCE 서비스를 지 원하는 필터 외에도 및 사용자에 게 표시할 260 x 96 dp 배너가 지불 응용 프로그램의 경우.

위에 설명 된 설치 프로그램에서 NFC 카드를 에뮬레이션 하는 응용 프로그램에 대 한 기본 구성 요소를 제공 합니다. NFC 자체는 몇 가지 자세한 단계 및 구성에 추가적인 테스트가 필요 합니다. 호스트 기반 카드 에뮬레이션에 대 한 자세한 내용은 참조는 [Android 설명서 포털](https://developer.android.com/guide/topics/connectivity/nfc/hce.html)합니다.
NFC를 사용 하 여 Xamarin을 사용 하 여에 대 한 자세한 내용은 체크 아웃 합니다 [Xamarin NFC 샘플](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample)합니다.

### <a name="sensors"></a>센서

KitKat 통해 장치 센서에 액세스할 수는 [ `SensorManager` ](https://developer.xamarin.com/api/type/Android.Hardware.SensorManager/)합니다.
`SensorManager` 배터리 유지 OS의 응용 프로그램 일괄 처리에 센서 정보 배달을 예약할 수 있습니다.

KitKat은 사용자의 단계를 추적 하기 위한 두 개의 새 센서 형식으로도 제공 됩니다. 이러한가 속도계를 기반으로 하 고 포함 합니다.

-  *StepDetector* -앱 알림을/절전 모드에서 사용자가 단계를 수행 하 고 검색 기가 단계 발생 했을 때 시간 값을 제공 합니다.

-  *StepCounter* -센서를 등록 하는 사용자가 수행 하는 단계 수는 추적 *장치 다시 부팅 될 때까지*입니다.

아래 스크린샷에서 작업에서 단계 카운터를 보여 줍니다.

[![단계 카운터 표시 SensorsActivity 앱의 스크린 샷](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

만들 수 있습니다는 `SensorManager` 를 호출 하 여 `GetSystemService(SensorService)` 결과 캐스팅 하는 `SensorManager`합니다. 단계 카운터를 사용 하려면 호출 `GetDefaultSensor` 에 `SensorManager`합니다. 센서를 등록 하 고 활용 하 여 단계 수의 변경 내용을 수신 하는 [`ISensorEventListener`](https://developer.xamarin.com/api/type/Android.Hardware.ISensorEventListener/)
다음 코드 예제에 나온 것 처럼 인터페이스:

```csharp
public class MainActivity : Activity, ISensorEventListener
{
    float count = 0;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        SensorManager senMgr = (SensorManager) GetSystemService (SensorService);
        Sensor counter = senMgr.GetDefaultSensor (SensorType.StepCounter);
        if (counter != null) {
            senMgr.RegisterListener(this, counter, SensorDelay.Normal);
        }
    }

    public void OnAccuracyChanged (Sensor sensor, SensorStatus accuracy)
    {
        Log.Info ("SensorManager", "Sensor accuracy changed");
    }

    public void OnSensorChanged (SensorEvent e)
    {
        count = e.Values [0];
    }
}
```

`OnSensorChanged` 응용 프로그램이 전경에 있는 동안 단계 수를 업데이트 하는 경우 호출 됩니다. 그러나 응용 프로그램이 백그라운드에 들어가거나 장치를 절전 모드, 하는 경우 `OnSensorChanged` 호출 되지 것입니다. 단계를 계속 될 때까지 계산; `UnregisterListener` 라고 합니다.

에 유의 *센서를 등록 하는 모든 응용 프로그램에서 단계 개수 값이 누적*합니다. 즉, 제거 하 고 응용 프로그램을 다시 설치 하 고 초기화 하는 경우에는 `count` 응용 프로그램 시작 시 0에서 변수, 센서에서 보고 된 값 남아 총 센서를 등록 하는 동안 수행 하는 단계 하 든 사용자 응용 프로그램 또는 다른 합니다. 응용 프로그램을 호출 하 여 단계 카운터를 추가 하지 않으려면 `UnregisterListener` 에 `SensorManager`아래 코드에 나온 것 처럼,:

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

장치 다시 부팅 단계 수를 0으로 다시 설정 합니다. 앱에 대 한 보고 센서 또는 장치의 상태를 사용 하 여 다른 응용 프로그램에 관계 없이 응용 프로그램에 대 한 정확한 개수를 확인 하려면 추가 코드가 필요 합니다.


> [!NOTE]
> 단계 검색과 KitKat과 함께 제공 되며 계산에 대 한 API를 하는 동안 일부 휴대폰 센서를 갖출 됩니다. 센서를 실행 하 여 사용할 수 있는 경우를 확인할 수 있습니다 `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`, 또는 반환 된 값을 확인 하십시오 `GetDefaultSensor` 되지 `null`합니다.


<a name="developer_tools" />

## <a name="developer-tools"></a>개발자 도구

### <a name="screen-recording"></a>화면 녹화

KitKat은 개발자가 실행 중인 응용 프로그램을 기록할 수 있도록 기능을 기록 하는 새 화면을 포함 합니다. 화면 녹화를 통해 제공 되는 [브리지 ADB (Android Debug)](https://developer.android.com/tools/help/adb.html) 클라이언트 Android SDK의 일부로 다운로드할 수 있습니다.

장치 연결 화면을 기록 하려면 그런 다음 Android SDK 설치를 찾을로 이동 합니다 **플랫폼 도구** 디렉터리 및 실행 합니다 **adb** 클라이언트:

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

위의 명령은 4Mbps의 기본 해상도 기본 3 분짜리 비디오를 기록 합니다. 길이 편집 하려면 다음을 추가 합니다 *-제한 시간* 플래그입니다.
해상도 변경 하려면 추가 합니다 *-비트 전송률* 플래그입니다. 다음 명령을 8Mbps 분 길이의 비디오를 기록 됩니다.

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

장치에서 비디오를 찾을 수 있습니다-기록이 완료 되 면 갤러리에 표시 됩니다.


## <a name="other-kitkat-additions"></a>다른 KitKat 추가

위에서 설명한 변경 하는 것 외에도 KitKat 수 있습니다.

-  *전체 화면을 사용 하 여* -새 KitKat 소개 [몰입 형 모드](https://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int)) 내용 찾아보기, 게임 및 전체 화면 환경을 활용할 수 있는 다른 응용 프로그램을 실행 합니다.

-  *알림을 사용자 지정할* -시스템 알림 사용 하 여에 대 한 추가 정보는 [`NotificationListenerService`](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/)
   . 이렇게 하면 앱 내에서 다른 방식으로 정보를 제공할 수 있습니다.

-  *드로어 블 리소스 미러* -드로어 블 리소스에 새 [`autoMirrored`](https://developer.android.com/reference/android/R.attr.html#autoMirrored)
   시스템에 알려주는 특성에는 왼쪽에서 오른쪽 레이아웃 대칭 이동 해야 하는 이미지에 대 한 미러된 버전을 만듭니다.

-  *애니메이션을 일시 중지* -일시 중지 및 다시 시작 애니메이션을 사용 하 여 만든 합니다 [`Animator`](https://developer.xamarin.com/api/type/Android.Animation.Animator/)
   포함됩니다.

-  *동적으로 변경 텍스트 읽기* -새 "라이브 영역"으로 새 텍스트를 사용 하 여 동적으로 업데이트 하는 UI 부분을 나타내는 [ `accessibilityLiveRegion`](https://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion)
   새 텍스트를 자동으로 읽을 액세스 모드에는 특성입니다.

-  *오디오 환경을 개선할* -확인 된 크게 트랙 합니다 [`LoudnessEnhancer`](https://developer.xamarin.com/api/type/Android.Media.Audiofx.LoudnessEnhancer/)
   최대 및 오디오 스트림 사용 하 여 RMS를 찾기는 [`Visualizer`](https://developer.xamarin.com/api/field/Android.Media.Audiofx.Visualizer.MeasurementModePeakRms/)
   클래스 및에서 정보를 가져올는 [오디오 타임 스탬프](https://developer.xamarin.com/api/type/Android.Media.AudioTimestamp/) 오디오-비디오 동기화에 도움이 되도록 합니다.

-  *사용자 지정 간격으로 ContentResolver 동기화* -KitKat 동기화 요청이 수행 되는 시간에 일부 가변성을 추가 합니다. 동기화를 `ContentResolver` 사용자 지정 시간이 나 호출 하 여 간격 `ContentResolver.RequestSync` 전달 하 고는 `SyncRequest`합니다.

-  *컨트롤러 간에 구별* -KitKat에서 컨트롤러는 할당 된 장치를 통해 액세스할 수 있는 고유 정수 식별자 `ControllerNumber` 속성입니다. 그러면 떨어져 플레이어가 게임에서 하기가 더 쉽습니다.

-  *원격 제어* -하드웨어 및 소프트웨어를 둘 다 쪽에 몇 가지 변경 내용으로 KitKat를 사용 하 여 원격 제어는 IR 전송기를 갖출 장치를 끌 수 있습니다는 `ConsumerIrService`, 새 주변 장치와 상호 작용 하 고 [`RemoteController`](https://developer.xamarin.com/api/type/Android.Media.RemoteController/)
   Api입니다.

위의 API 변경 내용에 대 한 자세한 내용은 참조 하십시오 Google [Android 4.4 Api](https://developer.android.com/about/versions/android-4.4.html) 개요.


## <a name="summary"></a>요약

이 문서에서는 일부 Android 4.4 (API 수준 19)에서 사용할 수 있는 새 Api를 도입 하 고 KitKat 응용 프로그램을 전환 하는 경우 모범 사례를 설명 합니다. 사용자에 영향을 주는 Api에 설명 된 변경 내용의 환경을 포함 하 여 it 합니다 *전환 프레임 워크* 및에 대 한 새 옵션 *테마*합니다. 도입 된 다음에 *저장소 액세스 프레임 워크* 하 고 `DocumentsProvider` 클래스 뿐만 아니라 새 *인쇄 Api*합니다. 탐색 것 *NFC 호스트 기반 카드 에뮬레이션* 및 사용 하는 방법 *저전력 센서*, 사용자의 단계를 추적 하기 위한 두 개의 새 센서를 포함 하 여 합니다. 사용 하 여 응용 프로그램의 실시간 데모 캡처 설명 마지막으로, *화면 녹화*, KitKat API 변경 내용 및 추가의 자세한 목록을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [KitKat 샘플](https://developer.xamarin.com/samples/KitKat/)
- [Android 4.4 Api](https://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](https://developer.android.com/about/versions/kitkat.html)
