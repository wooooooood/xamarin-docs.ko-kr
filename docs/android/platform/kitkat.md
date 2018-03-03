---
title: "KitKat 기능"
description: "Android 4.4 (KitKat)의 기능에 대해 사용자와 개발자가 cornucopia 로드 되어 있습니다. 이 가이드는 이러한 기능 중 몇 가지 강조 표시 하 고 코드 예제 및 KitKat을 최대한 활용 하기 위해 구현 세부 정보를 제공 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: ae6b89e48005ca028db5d13f1a55f237888ae08b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="kitkat-features"></a>KitKat 기능

_Android 4.4 (KitKat)의 기능에 대해 사용자와 개발자가 cornucopia 로드 되어 있습니다. 이 가이드는 이러한 기능 중 몇 가지 강조 표시 하 고 코드 예제 및 KitKat을 최대한 활용 하기 위해 구현 세부 정보를 제공 합니다._

## <a name="overview"></a>개요

Android 4.4 (API 수준 19) 라고도 "KitKat"가, 런타임에 2013에서 릴리스 되었습니다. KitKat는 다양 한 새로운 기능과 향상 된 기능을 포함 하 여 제공 합니다.

-  [사용자 경험](#user_experience) &ndash; 쉬운 애니메이션 전환 프레임 워크, 반투명 상태와 탐색 모음 몰입 전체 화면 모드와 사용자에 대 한 향상 된 환경을 만들 수 있습니다.

-  [사용자가 콘텐츠](#user_content) &ndash; 저장소 액세스 프레임 워크와 사용자 파일 관리 간소화; 인쇄 그림, 웹 사이트 및 기타 콘텐츠 향상 된 인쇄 Api와 더 쉽습니다.

-  [하드웨어](#hardware) &ndash; NFC 카드 에뮬레이션 호스트 기반으로 하는 NFC 카드에는 모든 앱; 절전 센서와 실행의 `SensorManager` 합니다.

-  [개발자 도구](#developer_tools) &ndash; 동영상 가이드 응용 프로그램에서 사용할 수 있는 Android 디버그 브리지 클라이언트 동작과 Android SDK의 일부분으로 합니다.


이 가이드에서는 Xamarin.Android 개발자를 위한 상위 수준 개요를 KitKat 뿐만 아니라 KitKat, 기존 Xamarin.Android 응용 프로그램을 마이그레이션하기 위한 지침을 제공 합니다.

## <a name="requirements"></a>요구 사항

KitKat를 사용 하 여 Xamarin.Android 응용 프로그램을 개발 하려면 *Xamarin.Android 4.11.0* 또는 상위 수준과 Android 4.4 (API 수준 19) 다음 스크린샷에 표시 된 것 처럼 Android SDK Manager를 통해를 설치 합니다.

[![Android SDK Manager에서 Android 4.4를 선택합니다.](kitkat-images/api19.png)](kitkat-images/api19.png)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>KitKat로 앱 마이그레이션

이 섹션에서는 일부 첫 번째 응답 항목 Android 4.4를 기존 응용 프로그램의 전환 하 합니다.

### <a name="check-system-version"></a>시스템 버전 확인

응용 프로그램을 이전 버전의 Android와 호환 되어야 하는 경우에 다음 코드 예제에 표시 된 것 처럼 모든 KitKat 관련 코드 시스템 버전 검사를 래핑하면 야 합니다.

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>경보 일괄 처리

Android 앱의 백그라운드에서 지정된 된 시간에 절전 모드를 경보 서비스를 사용 합니다. KitKat는 전원 유지 하는 경보를 일괄 처리 하 여 한 단계를 추가 합니다. 이 그룹은 같은 시간 간격 동안 절전 모드 및 절전 모드를 동시에 해제를 위해 등록 하는 여러 응용 프로그램을 선호 KitKat 정확한 시간에 각 응용 프로그램을 다시 시작, 대신이 대화 상자, 것을 의미 합니다.
Android 앱 지정된 된 시간 간격 동안 절전 모드를 판단할 수 호출 `SetWindow` 에 [ `AlarmManager` ](https://developer.xamarin.com/api/type/Android.App.AlarmManager/)에 최소 및 최대 시간을 밀리초 수 앱 절전 모드 해제 하기 전까지 걸리는, 단위로 수행할 작업에 전달 절전 모드 해제 합니다.
다음 코드에서는 창에 설정 된 시간에서 1 시간 30 분 사이의 해제 해야 하는 응용 프로그램의 예를 제공 합니다.

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

정확한 시간에 응용 프로그램을 다시 시작을 계속 하려면 사용 하 여 `SetExact`응용 프로그램을 해제 해야 하는 정확한 시간과 수행할 작업에 전달 합니다.

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

더 이상 KitKat에서는 정확한 반복 경보를 설정할 수 없습니다. 사용 하는 응용 프로그램 [ `SetRepeating` ](https://developer.xamarin.com/api/member/Android.App.AlarmManager.SetRepeating/p/Android.App.AlarmType/System.Int64/System.Int64/Android.App.PendingIntent/) 정확한 경보를 수동으로 각 경보를 트리거하도록 지금 필요한에서 실행 되도록 해야 합니다.

### <a name="external-storage"></a>외부 저장소

외부 저장소는 이제 두 가지 유형-응용 프로그램 및 데이터를 여러 응용 프로그램에서 공유에 고유한 저장소 나뉩니다. 읽기 및 쓰기 응용 프로그램의 특정 위치를 외부 저장소에 없는 특별 한 권한이 필요 합니다. 이제 공유 저장소에는 데이터와 상호 작용 필요는 `READ_EXTERNAL_STORAGE` 또는 `WRITE_EXTERNAL_STORAGE` 권한. 따라서 두 가지를 분류할 수 있습니다.

-  메서드를 호출 하 여 파일 또는 디렉터리 경로 액세스 하는 경우 `Context` -예를 들어 [ `GetExternalFilesDir` ](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalFilesDir/p/System.String/) 또는 [`GetExternalCacheDirs`](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalCacheDirs%28%29/)
   - 앱 추가 권한이 필요합니다.

-  표시 되는 경우 파일 또는 디렉터리 경로 속성에 액세스 하거나에서 메서드를 호출 하 여 `Environment` 와 같은 [ `GetExternalStorageDirectory` ](https://developer.xamarin.com/api/property/Android.OS.Environment.ExternalStorageDirectory/) 또는 [ `GetExternalStoragePublicDirectory` ](https://developer.xamarin.com/api/member/Android.OS.Environment.GetExternalStoragePublicDirectory/p/System.String/) , 앱에 필요한는 `READ_EXTERNAL_STORAGE` 또는 `WRITE_EXTERNAL_STORAGE` 권한.

> [!NOTE]
> **참고:** `WRITE_EXTERNAL_STORAGE` 의미는 `READ_EXTERNAL_STORAGE` 권한, 해야만 해야 하나 사용 권한을 설정 합니다.

### <a name="sms-consolidation"></a>SMS 통합

사용자가 선택한 하나의 기본 응용 프로그램에서 모든 SMS 콘텐츠를 집계 하 여 사용자에 대 한 메시징 KitKat를 간소화 합니다. 개발자는 응용 프로그램의 기본 메시징 응용 프로그램과 선택 가능한을 만들고이 작동 적절 하 게 수명 및 코드에서 응용 프로그램을 선택 하지 않으면 하는 일을 담당 합니다. SMS 앱 KitKat 전환에 대 한 자세한 내용은 참조는 [Your SMS 앱 준비 KitKat](http://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) Google에서 가이드입니다.

### <a name="webview-apps"></a>WebView 응용 프로그램

[WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) 모양 바꾸기 KitKat에서 가져왔습니다. 가장 크게 변화 보안 콘텐츠를 로드 하기 위한 추가 되는 `WebView`합니다. 이전 API 버전을 대상으로 대부분의 응용 프로그램에 예상 대로 작동 해야 하는 동안 사용 하는 응용 프로그램 테스트는 `WebView` 클래스 것이 좋습니다. 영향을 받는 WebView Api에 대 한 자세한 내용은 Android 참조 [Android 4.4에서 WebView로 마이그레이션](http://developer.android.com/guide/webapps/migrating.html) 설명서입니다.

<a name="user_experience" />

## <a name="user-experience"></a>사용자 경험

KitKat 속성 애니메이션 및 테마 설정에 대 한 반투명 UI 옵션을 처리 하기 위한 새로운 전환 프레임 워크를 비롯 하 여 사용자 환경을 향상 시키기 위해 여러 가지 새로운 Api 함께 제공 됩니다. 이러한 변경 내용은 아래에서 다룹니다.

### <a name="transition-framework"></a>전환 프레임 워크

전환 프레임 워크를 사용 하면 애니메이션 쉽게 구현할 수 있습니다. KitKat를 사용 하면 코드의 줄만 간단한 속성 애니메이션을 수행 하거나 전환을 사용 하 여 사용자 지정 *장면*합니다.

#### <a name="simple-property-animation"></a>단순 속성 애니메이션

새 Android 전환 라이브러리 속성 애니메이션 뒤에 있는 코드를 간소화 합니다. 프레임 워크를 사용 하면 최소한의 코드로 간단한 애니메이션을 수행할 수 있습니다. 예를 들어 다음 코드 샘플에서는 [ `TransitionManager.BeginDelayedTransition` ](https://developer.xamarin.com/api/member/Android.Transitions.TransitionManager.BeginDelayedTransition/p/Android.Views.ViewGroup/Android.Transitions.Transition/) 표시 및 숨기기 애니메이션 효과를 주는 `TextView`:

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

위의 예제에서는 자동, 기본 속성 값 변경을 전환 만들 전환 프레임 워크를 사용 합니다. 애니메이션에서는 코드의 한 줄을 통해 처리 하기 때문에 쉽게 수행할 수 있습니다이 이전 버전의 Android 호환 래핑하여는 `BeginDelayedTransition` 시스템 버전 검사를 호출 합니다. 참조는 [Your 응용 프로그램에 KitKat에 마이그레이션](#Migrating_Your_App_to_KitKat) 더 섹션입니다.

아래 스크린샷에서 애니메이션 하기 전에 앱을 보여 줍니다.

[![애니메이션을 시작 하기 전에 응용 프로그램 스크린 샷](kitkat-images/trans-before.png)](kitkat-images/trans-before.png)

아래 스크린샷에서 애니메이션 후 앱을 보여 줍니다.

[![애니메이션이 완료 된 후 응용 프로그램 스크린 샷](kitkat-images/trans-after.png)](kitkat-images/trans-after.png)

다음 섹션에서 다루는 장면 가진 전환 보다 자세히 제어를 얻을 수 있습니다.

#### <a name="android-scenes"></a>Android 장면

[장면](https://developer.xamarin.com/api/type/Android.Transitions.Scene/) 제어할 수 있도록 개발자 자세한 애니메이션 전환 프레임 워크의 일부로 도입 되었습니다. 장면 UI에 동적 영역을 만들: 컨테이너와 여러 가지 버전, 또는 "상태", 컨테이너 내부 XML 콘텐츠에 대 한을 지정 하 고 Android 수행 하는 작업 간의 전환 애니메이션을 적용 하는 작업의 나머지 작업을 수행 합니다. Android 장면 개발 측면에서 복잡 한 애니메이션을 최소한의 노력으로 빌드할 수 있습니다.

동적 콘텐츠를 보유 하는 정적 UI 요소 라고는 *컨테이너* 또는 *장면 기본*합니다. 다음 예제에서는 Android 디자이너를 사용 하 여 만듭니다는 `RelativeLayout` 호출 `container`:

[![Android 디자이너를 사용 하 여 RelativeLayout 컨테이너를 만들려면](kitkat-images/container.png)](kitkat-images/container.png)

샘플 레이아웃 이라는 단추가 정의 `sceneButton` 아래에서 `container`합니다. 이 단추는 전환이 트리거됩니다.

컨테이너 내에서 동적 콘텐츠는 두 개의 새 Android 레이아웃 필요합니다. 이러한 레이아웃 지정 코드만 *내* 컨테이너입니다.
아래 예제에서는 코드 라는 레이아웃을 정의 *Scene1* 두 개의 텍스트 필드를 읽기 "키트" 및 "캐 탈" 각각 포함 하 고 두 번째 레이아웃 호출 *Scene2* 되돌릴 동일한 텍스트 필드를 포함 하는 합니다. XML은 다음과 같습니다.

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

사용 하 여 위의 예에서는 `merge` 짧은 코드 보기를 만들고 보기 계층 구조를 단순화 합니다. 에 대 한 자세한 내용은 `merge` 레이아웃 [여기](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html)합니다.

장면이 호출 하 여 만든 [ `Scene.GetSceneForLayout` ](https://developer.xamarin.com/api/member/Android.Transitions.Scene.GetSceneForLayout/p/Android.Views.ViewGroup/System.Int32/Android.Content.Context/)컨테이너 개체를 장면의 레이아웃 파일 및 현재 리소스 ID를 전달, `Context`와 다음 코드 예제에서와 같이:

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

단추를 클릭 하는 Android 기본 전환 값으로 애니메이션 효과 주는 두 장면 사이의 대칭 이동:

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

아래 스크린샷에서 전에 애니메이션 장면의 보여 줍니다.

[![애니메이션을 시작 하기 전에 응용 프로그램의 스크린 샷](kitkat-images/trans-after.png)](kitkat-images/trans-after.png)

아래 스크린샷에서 후 애니메이션 장면의 보여 줍니다.

[![애니메이션이 완료 된 후의 응용 프로그램의 스크린 샷](kitkat-images/scene.png)](kitkat-images/scene.png)


> [!NOTE]
> **참고:** 는 [알려진된 버그](https://code.google.com/p/android/issues/detail?id=62450) Android 전환에 백그라운드에서 발생 하는 라이브러리를 사용 하 여 만든 `GetSceneForLayout` 두 번째 활동을 통해는 사용자가을 중단 하도록 합니다. Java 해결 방법이 설명 되어 [여기](http://www.doubleencore.com/2013/11/new-transitions-framework/)합니다.


##### <a name="custom-transitions-in-scenes"></a>장면에 사용자 지정 전환

사용자 지정 전환의 xml 리소스 파일에 정의할 수 있습니다는 `transition` 아래의 디렉터리 `Resources`아래 스크린샷에서 예와 같이:

[![리소스/전환 디렉터리 transition.xml 파일의 위치](kitkat-images/resources.png)](kitkat-images/resources.png)

다음 코드 샘플 5 초간 애니메이션 효과 적용 하 고 사용 하는 전환을 정의 [보간 지나친](http://developer.android.com/reference/android/views/animation/OvershootInterpolator.html):

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

활동에 전환이 만들어집니다를 사용 하는 [TransitionInflater](https://developer.xamarin.com/api/type/Android.Transitions.TransitionInflater/)아래 코드 예와 같이:

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

새 전환에 추가 되는 `Go` 애니메이션을 시작 하는 호출:

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>반투명 UI

KitKat 사용 하면 더 제어할 테마 설정 선택적 transclucent 상태와 탐색 모음을 사용 하 여 앱. Android 테마를 정의 하는 데 사용 하는 동일한 XML 파일에서 시스템 UI 요소의 투명도 변경할 수 있습니다. KitKat는 다음과 같은 속성이 도입 되었습니다.

-  `windowTranslucentStatus` -로 설정 된 경우 true를 사용 하면 상위 상태 표시줄 반투명 합니다.

-  `windowTranslucentNavigation` -로 설정 된 경우 true를 사용 하면 아래쪽 탐색 모음 반투명 합니다.

-  `fitsSystemWindows` -Transcluent를 위쪽 또는 아래쪽 모음을 설정 합니다. 기본적으로 투명 한 UI 요소 아래에서 콘텐츠를 이동 합니다. 이 속성을 설정 `true` 콘텐츠가 반투명 시스템 UI 요소와 일치 하는 것을 방지 하는 간단한 방법입니다.


다음 코드와 반투명 상태와 탐색 모음 테마를 정의합니다.

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

아래 스크린샷에서 위에 반투명 상태 및 탐색 모음과 테마를 보여 줍니다.

[![반투명 상태와 탐색 모음으로 응용 프로그램의 예제 스크린 샷](kitkat-images/theme.png)](kitkat-images/theme.png)

<a name="user_content" />

## <a name="user-content"></a>사용자 콘텐츠

### <a name="storage-access-framework"></a>저장소 액세스 프레임 워크

저장소 액세스 프레임 워크 (SAF)는 사용자가 이미지, 비디오, 문서와 같은 저장 된 콘텐츠에 사용에 대 한 새로운 방법입니다. 사용자가 콘텐츠를 처리 하는 응용 프로그램을 선택 하는 대화 상자를 제시 하는 대신 KitKat 새 UI는 사용자가 집계 한 위치에서 데이터에 액세스할 수 있도록 열립니다. 콘텐츠를 선택한 후 사용자가 콘텐츠를 요청한 응용 프로그램에 반환 하 고 앱 환경을 정상적으로 계속 됩니다.

이렇게이 변경 하려면 개발자 쪽에서 두 가지 동작: 첫 번째 공급자 콘텐츠를 필요한 응용 프로그램의 reqesting 콘텐츠를 새로운 방식으로 업데이트 필요 합니다. 둘째, 응용 프로그램 데이터를 기록 하는 `ContentProvider` 새 프레임 워크를 사용 하도록 수정 해야 합니다. 두 시나리오 모두 새에 따라 달라 집니다 [ `DocumentsProvider` ](https://developer.xamarin.com/api/type/Android.Provider.DocumentsProvider/) API입니다.

#### <a name="documentsprovider"></a>DocumentsProvider

KitKat와 상호 작용에서에서 `ContentProviders` 와 추상화는 `DocumentsProvider` 클래스입니다. 즉,는 SAF 신경 쓰지 않습니다 데이터 물리적으로를 통해 액세스할 수 있는 상태로 `DocumentsProvider` API입니다. 로컬 공급자 클라우드 서비스 및 외부 저장 장치 모두 사용 하 여 인터페이스를 동일 하 고 동일한 방식으로 처리 됩니다 한 곳은 사용자의 콘텐츠로와 상호 작용할 수 있는 사용자와 개발자를 제공 합니다.

이 섹션에서는 로드 하 고 저장소 액세스 프레임 워크와 콘텐츠를 저장 하는 방법에 설명 합니다.

#### <a name="request-content-from-a-provider"></a>공급자에서 콘텐츠를 요청 합니다.

말씀 KitKat SAF UI를 사용 하 여 콘텐츠를 선택 하 고 `ActionOpenDocument` 의도 장치에 사용할 수 있는 모든 콘텐츠 공급자에 연결 하려고 한다는 것을 의미 하는 합니다. 몇 가지 필터링을 이러한 의도 지정 하 여 추가할 수 있습니다 `CategoryOpenable`합니다 (즉, 액세스할 수 있는, 사용 가능한 콘텐츠) 열 수 있는 전용 콘텐츠를 의미 하는 반환 됩니다. 필터링 된 콘텐츠의 KitKat 수도 있습니다는 `MimeType`합니다. 이미지를 지정 하 여 결과 이미지에 대 한 필터 아래 코드 예를 들어 `MimeType`:

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

호출 `StartActivityForResult` 사용자 이미지를 선택 하 여 탐색할 수 있는 SAF UI를 시작 합니다.

[![저장소 액세스 프레임 워크를 사용 하 여 browing 이미지에 대 한 응용 프로그램의 예제 스크린 샷](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png)

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

#### <a name="write-content-to-a-provider"></a>콘텐츠 공급자에 쓰기

SAF UI에서 콘텐츠를 로드 하는 것 외에도 KitKat 수도 있습니다를 하나로 콘텐츠를 저장할 `ContentProvider` 구현 하는 `DocumentProvider` API입니다. 콘텐츠 사용 하 여 저장 한 `Intent` 와 `ActionCreateDocument`:

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

위 예제 코드는 파일 이름을 바꾸고 새 파일을 저장할 디렉터리를 선택 하 여 사용자 SAF UI를 로드 합니다.

[![NewDoc 다운로드 디렉터리에 파일 이름을 변경 하는 사용자의 스크린샷](kitkat-images/saf-save.png)](kitkat-images/saf-save.png)

사용자가 누르면 **저장**, `OnActivityResult` 전달 되는 `Android.Net.Uri` 으로 액세스할 수 있는 새로 생성된 된 파일의 `data.Data`합니다. 새 파일에 데이터를 스트리밍하는 uri를 사용할 수 있습니다.

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

[ `ContentResolver.OpenOutputStream(Android.Net.Uri)` ](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.OpenOutputStream/(Android.Net.Uri)) 반환는 `System.IO.Stream`이므로.NET 전체 스트리밍 프로세스를 작성할 수 있습니다.

로드에 대 한 자세한 내용은 만들기 및 편집 저장소 액세스 프레임 워크와 콘텐츠를 참조는 [저장소 액세스 프레임 워크에 대 한 Android 설명서](http://developer.android.com/guide/topics/providers/document-provider.html)합니다.

### <a name="printing"></a>인쇄

인쇄 콘텐츠의 도입으로 KitKat 간소화 되어는 [인쇄 서비스](https://developer.xamarin.com/api/namespace/Android.PrintServices/) 및 `PrintManager`합니다. KitKat 완전히 활용 하 여 첫 번째 API 버전 이기도 [Google의 클라우드 인쇄 서비스 Api](https://developers.google.com/cloud-print/) 를 사용 하는 [Google 클라우드 인쇄 응용 프로그램](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint)합니다.
Google 클라우드 인쇄 앱을 다운로드 하는 자동으로 KitKat와 함께 제공 되는 대부분 장치 및 [HP 인쇄 서비스 플러그인](https://play.google.com/store/apps/details?id=com.hp.android.printservice)처음에 연결할 때 WiFi 합니다. 로 이동 하 여 사용자가 자신의 장치 인쇄 설정을 확인할 수 **설정 > 시스템 > 인쇄**:

[![인쇄 설정 화면의 예제 스크린 샷](kitkat-images/printing.png)](kitkat-images/printing.png)


> [!NOTE]
> **참고:** Android 여전히 개발자는 새로운 Api를 사용 하 여 인쇄 내용을 준비 하 고 인쇄를 처리 하도록 다른 응용 프로그램에 보낼 인쇄 Api는 기본적으로 Google 클라우드 인쇄와 작동 하도록 설정 된, 있지만 수 있습니다.



#### <a name="printing-html-content"></a>HTML 콘텐츠 인쇄

KitKat 자동으로 만듭니다는 [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) 와 웹 보기에 대 한 `WebView.CreatePrintDocumentAdapter`합니다. 콘텐츠 인쇄 웹은 간의 협조를 [ `WebViewClient` ](https://developer.xamarin.com/api/type/Android.Webkit.WebViewClient/) HTML 콘텐츠를 로드 될 때까지 대기 하 고 인쇄 옵션을 옵션 메뉴에서에서 사용할 수 있도록 알고 활동 및 사용자를 때까지 대기 하는 Actvity 있습니다 인쇄 옵션 및 호출 선택 `Print`에 `PrintManager`합니다. 이 단원에서는 화면의 인쇄 하는 데 필요한 기본 설치 HTML 콘텐츠입니다.

Note를 로드 하 고 웹 콘텐츠를 인쇄 하려면 인터넷 권한이 필요 합니다.

[![응용 프로그램 옵션에 대 한 인터넷 권한 설정](kitkat-images/internet.png)](kitkat-images/internet.png)

##### <a name="print-menu-item"></a>인쇄 메뉴 항목

인쇄 옵션은 활동의에 일반적으로 표시 됩니다 [옵션 메뉴](http://developer.android.com/guide/topics/ui/menus.html#options-menu)합니다.
옵션 메뉴에는 사용자가 활동에 작업을 수행할 수 있습니다. 화면의 오른쪽 위에 이며 다음과 같습니다.

[![화면 오른쪽 위 모퉁이에 인쇄 메뉴 항목 표시의 예제 스크린 샷](kitkat-images/menu.png)](kitkat-images/menu.png)


추가 메뉴 항목에 정의할 수는 *메뉴*아래의 디렉터리 *리소스*합니다. 아래 코드 샘플 실행을 정의 [인쇄](https://developer.xamarin.com/api/type/Android.Print.PrintManager/):

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

통해 수행 된 작업에서 [옵션] 메뉴와의 상호 작용은 `OnCreateOptionsMenu` 및 `OnOptionsItemSelected` 메서드.
`OnCreateOptionsMenu` 가에서 인쇄 옵션을 같은 새 메뉴 항목을 추가 하는 위치는 *메뉴* resources 디렉터리입니다.
`OnOptionsItemSelected` 메뉴에서 인쇄 옵션을 선택 하면 사용자에 대 한 수신 대기 하 고 인쇄를 시작 합니다.

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

위의 코드는 또한 라는 변수 정의 `dataLoaded` HTML 콘텐츠의 상태를 추적할 수 있습니다. `WebViewClient` 이 변수를 옵션 메뉴에 인쇄 메뉴 항목을 추가 하는 작업에서 알 수 있도록 모든 콘텐츠를 로드 하는 경우 true로 설정 합니다.

##### <a name="webviewclient"></a>WebViewClient

작업을는 `WebViewClient` 는 데이터를 확인 하는 `WebView` 늘어나도 메뉴에서 인쇄 옵션 표시 될 때까지 완전히 로드 되는 `OnPageFinished` 메서드. `OnPageFinished` 웹 콘텐츠를 완전히 로드, 수신 대기 하 고 해당 옵션 메뉴를 다시 만드는 작업을 지시 `InvalidateOptionsMenu`:

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

`OnPageFinished` 또한 설정는 `dataLoaded` 값을 `true`하므로 `OnCreateOptionsMenu` 위치에서 인쇄 옵션을 사용 하 여 메뉴를 다시 만들 수 있습니다.

##### <a name="printmanager"></a>PrintManager

다음 코드 예제에서는의 내용을 인쇄는 `WebView`:

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print` 인수로 사용: ("MyWebPage"이 예에서), 인쇄 작업에 대 한 이름을 [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) 콘텐츠에서 인쇄 문서를 생성 하 고 [ `PrintAttributes` ](https://developer.xamarin.com/api/type/Android.Print.PrintAttributes/) (`null` 에 위 예제)입니다. 지정할 수 있습니다 `PrintAttributes` 에 해결 되는 인쇄 된 페이지의 콘텐츠를 레이아웃할 기본 특성에는 대부분 scenarions 처리 해야 합니다.

호출 `Print` 인쇄 작업에 대 한 옵션을 나열 하는 인쇄 UI를 로드 합니다. UI 아래 스크린샷과 표시 된 것 처럼 사용자에 게를 pdf로 HTML 콘텐츠를 저장 하거나 인쇄 옵션을 제공 합니다.

[![스크린샷의 PrintHtmlActivity 인쇄 메뉴 표시](kitkat-images/print1.png)](kitkat-images/print1.png)

[![스크린샷의 PrintHtmlActivity 저장 PDF 메뉴로 표시](kitkat-images/print2.png)](kitkat-images/print2.png)

<a name="hardware" />

## <a name="hardware"></a>하드웨어

KitKat 새 장치 기능을 수용 하기 위해 여러 Api를 추가 합니다. 이러한 가장 주목할 만한은 카드 에뮬레이션 호스트 기반 및 새 `SensorManager`합니다.

### <a name="host-based-card-emulation-in-nfc"></a>NFC에서 호스트 기반 카드 에뮬레이션

호스트 기반 카드 에뮬레이션 (HCE)에 운송 업체의 고유 보안 요소에 의존 하지 않고 NFC 카드 또는 NFC 카드 판독기 처럼 동작 하도록 응용 프로그램이 있습니다. HCE를 설정 하기 전에 HCE를 사용 하 여 장치에서 사용할 수 있는지 확인 `PackageManager.HasSystemFeature`:

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

사용 하려면 HCE HCE 기능 및 `Nfc` 권한이 응용 프로그램의 등록 `AndroidManifest.xml`:

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![응용 프로그램 옵션에서 NFC 사용 권한 설정](kitkat-images/nfc.png)](kitkat-images/nfc.png)

작동 하려면 HCE 백그라운드에서 실행할 수 및 HCE를 사용 하 여 응용 프로그램 실행 되지 않는 경우에 사용자가 NFC 트랜잭션을 만들 때 시작 해야 합니다. HCE 코드를 작성 하 여이 작업을 수행할 수 있습니다는 `Service`합니다. HCE 서비스에서 구현 하는 `HostApduService` 다음 메서드를 구현 하는 인터페이스:

-  *ProcessCommandApdu* -는 응용 프로그램 프로토콜 데이터 단위 (APDU)은 NFC 판독기와 HCE 서비스 간에 보내집니다. 이 메서드는 판독기에서 ADPU 소비 하 고 응답에 데이터 단위를 반환 합니다.

-  *OnDeactivated* - `HostAdpuService` HCE 서비스 NFC 판독기와 통신 하는 더 이상 때 비활성화 됩니다.


또한 HCE 서비스 응용 프로그램의 매니페스트를 등록 하 고 적절 한 사용 권한, 의도 필터 및 메타 데이터로 데코레이팅된를 해야 합니다. 다음 코드의 예는는 `HostApduService` Android 매니페스트를 사용 하 여 등록 된 `Service` 특성 (특성에 대 한 자세한 내용은 참조는 xamarin [Android 매니페스트 작업](~/android/platform/android-manifest.md) 가이드):

```csharp
[Service(Exported=true, Permission="android.permissions.BIND_NFC_SERVICE"),
    IntentFilter(new[] {"android.nfc.cardemulation.HOST_APDU_SERVICE"}),
    MetaData("andorid.nfc.cardemulation.host.apdu_service",
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

위의 서비스 NFC 판독기 응용 프로그램을 상호 작용할 수 있는 방법을 제공 하지만 NFC 판독기에는 아직이 서비스를 검색 하는 데 필요한 NFC 카드를 에뮬레이션 하는 경우 알 수 없습니다. 서비스를 식별 하는 NFC 판독기를 하려면 म 할당할 수는 서비스는 고유한 *응용 프로그램 ID (보조)*합니다. म 지정 HCE 서비스에 대 한 다른 메타 데이터와 함께 보다 쉽게 xml 리소스 파일에 등록 된 `MetaData` 특성 (위의 코드 예제 참조). 이 리소스 파일 하나 이상의 보조 필터를-하나 이상의 NFC 판독기 장치의 보조 기능에 해당 하는 16 진수 형식의 고유 식별자 문자열을 지정 합니다.

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

보조 필터 외에 보조 기능 그룹 ("기타"과 지불 응용 프로그램)를 지정 xml 리소스 파일에서는 HCE 서비스의 사용자 용 설명 및 사용자에 게 표시할 260 x 96 dp 배너 지불 응용 프로그램의 경우.

위에 설명 된 설치 NFC 카드 에뮬레이션 하는 응용 프로그램에 대 한 기본 구성 요소를 제공 합니다. 자체 NFC 추가 테스트를 구성 하 고 몇 가지 더 많은 단계가 필요 합니다. 호스트 기반 카드 에뮬레이션에 대 한 자세한 내용은 참조는 [Android 설명서 포털](https://developer.android.com/guide/topics/connectivity/nfc/hce.html)합니다.
NFC를 사용 하 여 xamarin에 대 한 자세한 내용은 체크 아웃 된 [Xamarin NFC 샘플](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample)합니다.

### <a name="sensors"></a>센서

KitKat 통해 장치의 센서에 액세스할 수는 [ `SensorManager` ](https://developer.xamarin.com/api/type/Android.Hardware.SensorManager/)합니다.
`SensorManager` 유지 하면서 배터리 수명 센서 정보 일괄 작업으로 응용 프로그램에 배달 되도록 예약 하는 OS를 허용 합니다.

또한 KitKat 사용자의 단계를 추적 하기 위한 두 개의 새 센서 유형이 포함 되어 있습니다. 이러한가 속도계 기반 및를 포함 합니다.

-  *StepDetector* -앱 알림을/절전 모드 해제는 탐지기 단계는 발생 했을 때 시간 값을 제공 하 고 사용자가을 한 단계를 수행 합니다.

-  *StepCounter* -센서도 등록 된 후 사용자가 수행 하는 단계 수는 추적 *는 다음 장치를 다시 부팅 될 때까지*합니다.

아래 스크린샷에서 단계 카운터를의 동작을 보여 줍니다.

[![단계 카운터 표시 SensorsActivity 앱의 스크린 샷](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png)

만들 수는 `SensorManager` 호출 하 여 `GetSystemService(SensorService)` 로 결과 캐스팅 하는 `SensorManager`합니다. 단계 카운터를 사용 하려면 호출 `GetDeafultSensor` 에 `SensorManager`합니다. 센서를 등록 하 고 사용 하 여 단계 수의 변화에 따라 수신 된 [ `ISensorEventListener` ](https://developer.xamarin.com/api/type/Android.Hardware.ISensorEventListener/) 인터페이스, 다음 코드 예제에 표시 된 것 처럼:

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

`OnSensorChanged` 응용 프로그램이 있는 동안 포그라운드에서 단계 수를 업데이트 하는 경우 호출 됩니다. 응용 프로그램은 백그라운드 있거나 장치를 절전 경우 `OnSensorChanged` 호출 되지 것입니다; 그러나, 단계 개수를 셀 때까지 계속 됩니다 `UnregisterListener` 호출 됩니다.

*센서를 등록 하는 모든 응용 프로그램 단계 개수 값이 누적*합니다. 즉, 제거 하 고 응용 프로그램을 다시 설치 하 고 초기화 하는 경우에는 `count` 응용 프로그램 시작 시 0에서 변수, 센서에서 보고 되는 값 남습니다 센서 등록 된 동안 수행 하는 단계의 총 수에서 여부 프로그램 응용 프로그램 또는 다른 합니다. 응용 프로그램에서 호출 하 여 단계 카운터를 추가 하지 못할 수 있습니다 `UnregisterListener` 에 `SensorManager`아래 코드 예와 같이:

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

장치를 다시 부팅 하면 단계 수를 0으로 다시 설정 합니다. 앱 추가 코드가 센서 또는 장치의 상태를 사용 하 여 다른 응용 프로그램에 관계 없이 응용 프로그램에 대 한 정확한 개수를 보고 하는 것을 확인 해야 합니다.


> [!NOTE]
> **참고:** 단계 검색과 KitKat과 함께 제공 되며 계산에 대 한 API를 하는 동안 모든 휴대폰 센서를 갖출 됩니다. 센서가 실행 하 여 사용할 수 있는 경우를 확인할 수 있습니다 `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`, 또는 반환된 된 값의 되도록 확인 `GetDefaultSensor` 되지 않습니다 `null`합니다.


 <a name="developer_tools" />


## <a name="developer-tools"></a>개발자 도구

### <a name="screen-recording"></a>화면 녹화

KitKat 녹음 기능을 개발자가 작업에서 응용 프로그램을 기록할 수 있도록 하는 새 화면이 포함 되어 있습니다. 화면 녹화를 통해 사용할 수는 [Android 디버그 브리지 (ADB)](http://developer.android.com/tools/help/adb.html) Android SDK의 일부로 다운로드할 수 있는 클라이언트입니다.

사용자의 화면을 기록 하려면 장치에 연결 합니다. 그런 다음 Android SDK 설치를 찾아에서 이동 하는 **플랫폼 도구** 디렉터리 및 실행 된 **adb** 클라이언트:

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

위의 명령은 기본 3 분으로 4Mbps의 기본 해상도로 비디오를 기록 합니다. 길이 편집 하려면 추가 *-제한 시간* 플래그입니다.
해상도 변경 하려면 추가 *-비트 전송률* 플래그입니다. 다음 명령을 8Mbps에서 분 장기 비디오를 기록 됩니다.

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

장치에서 비디오를 찾을 수 있습니다-녹음/녹화 완료 되 면 갤러리에 표시 됩니다.

<a name="other_kitkat_additions" />

## <a name="other-kitkat-additions"></a>기타 KitKat 추가 기능

위에서 설명한 변경 KitKat 수 있습니다.

-  *전체 화면을 사용 하 여* -KitKat 탑재 되었습니다 [몰입 모드](http://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int)) 콘텐츠를 탐색, 게임, 및 전체 화면 경험을 활용할 수 있는 다른 응용 프로그램을 실행 합니다.

-  *알림을 사용자 지정할* -사용 하 여 시스템 알림에 대 한 추가 정보는 [ `NotificationListenerService` ](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) 합니다. 이렇게 하면 응용 프로그램 내의 다른 방식으로 정보를 제공할 수 있습니다.

-  *그릴 수 있는 리소스를 미러링할* -그릴 수 있는 리소스에는 새 [ `autoMirrored` ](http://developer.android.com/reference/android/R.attr.html#autoMirrored) 왼쪽에서 오른쪽 레이아웃 대칭 이동 해야 하는 이미지에 대 한 미러된 버전을 작성 하는 시스템에 알리는 특성이 필요 합니다.

-  *애니메이션을 일시 중지* -일시 중지 및 다시 시작 애니메이션을 사용 하 여 만든는 [ `Animator` ](https://developer.xamarin.com/api/type/Android.Animation.Animator/) 클래스입니다.

-  *동적으로 변경 텍스트 읽기* -업데이트 하는 동적으로 새 텍스트로 "라이브 영역"으로 새 UI의 부분을 나타내는 [ `accesibilityLiveRegion` ](http://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion) 액세스 모드 자동으로 새 텍스트를 읽을 수는 특성입니다.

-  *오디오 환경을 향상* -확인 된 크게 트랙의 [ `LoudnessEnhancer` ](https://developer.xamarin.com/api/type/Android.Media.Audiofx.LoudnessEnhancer/) , 최고 및 오디오 스트림의 RMS를 찾습니다는 [ `Visualizer` ](https://developer.xamarin.com/api/field/Android.Media.Audiofx.Visualizer.MeasurementModePeakRms/) 클래스 하 고는 에서정보를가져올[오디오 타임 스탬프](https://developer.xamarin.com/api/type/Android.Media.AudioTimestamp/) 오디오 / 비디오 동기화에 도움이 되도록 합니다.

-  *사용자 지정 간격으로 ContentResolver 동기화* -KitKat 동기화 요청이 수행 되는 시간에 일부 가변성을 추가 합니다. 동기화는 `ContentResolver` 사용자 지정 시간이 나 간격 호출 하 여 `ContentResolver.RequestSync` 를 전달 하 고는 `SyncRequest`합니다.

-  *컨트롤러 간의 구별* -KitKat에서 컨트롤러는 장치를 통해 액세스할 수 있는 고유 정수 식별자를 할당 된 `ControllerNumber` 속성입니다. 이렇게 하면 쉽게 더 많이 떨어져 있는 플레이어가 게임에 알 수 있습니다.

-  *원격 제어* -하드웨어와 소프트웨어 측면에 몇 가지 변경 내용과 KitKat를 사용 하 여 원격 제어에 IR 전송기를 갖출 장치를 끌 수 있습니다는 `ConsumerIrService`, 주변 장치 새 와상호작용하고[ `RemoteController` ](https://developer.xamarin.com/api/type/Android.Media.RemoteController/) Api입니다.

위의 API 변경 내용에 대 한 자세한 내용은 참조 하십시오는 Google [Android 4.4 Api](http://developer.android.com/about/versions/android-4.4.html) 개요입니다.


## <a name="summary"></a>요약

이 문서는 몇 가지 Android 4.4 (API 수준 19)에서 사용할 수 있는 새로운 Api를 도입 하 고 KitKat에 응용 프로그램을 전환할 때 유용한 정보를 다룹니다. 사용자에 영향을 주는 Api의 변경 사항 개략적으로 설명 된 환경을 포함 하 여 it는 *전환 프레임 워크* 및에 대 한 새 옵션 *테마 설정*합니다. 도입 된 다음에 *저장소 액세스 프레임 워크* 및 `DocumentsProvider` 클래스 뿐만 아니라 새 *Api 인쇄*합니다. 탐색 것 *NFC 호스트 기반 카드 에뮬레이션* 및 사용 방법을 *절전 센서*, 사용자의 단계를 추적 하기 위한 두 개의 새 센서 등입니다. 응용 프로그램의 실시간 데모 캡처와 마지막으로, *화면 녹화*, KitKat API 변경 및 추가의 자세한 목록을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [KitKat 샘플](https://developer.xamarin.com/samples/KitKat/)
- [Android 4.4 Api](http://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](http://developer.android.com/about/versions/kitkat.html)
