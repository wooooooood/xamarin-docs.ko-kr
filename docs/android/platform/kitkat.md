---
title: KitKat 기능
description: Android 4.4 (KitKat)은 사용자 및 개발자를 위한 cornucopia 기능과 함께 로드 됩니다. 이 가이드에서는 이러한 기능 중 몇 가지를 중점적으로 설명 하 고, KitKat을 최대한 활용 하는 데 도움이 되는 코드 예제 및 구현 세부 정보를 제공 합니다.
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 6f3df1c7c4664f4138e0f399419ac95e15231916
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757524"
---
# <a name="kitkat-features"></a>KitKat 기능

_Android 4.4 (KitKat)은 사용자 및 개발자를 위한 cornucopia 기능과 함께 로드 됩니다. 이 가이드에서는 이러한 기능 중 몇 가지를 중점적으로 설명 하 고, KitKat을 최대한 활용 하는 데 도움이 되는 코드 예제 및 구현 세부 정보를 제공 합니다._

## <a name="overview"></a>개요

Android 4.4 (API 레벨 19) ("KitKat" 라고도 함)는 2013에 릴리스 되었습니다. KitKat는 다음과 같은 다양 한 새로운 기능 및 향상 된 기능을 제공 합니다.

- [사용자 환경](#user_experience) &ndash; 전환 프레임 워크, 반투명 상태 및 탐색 모음, 전체 화면 몰입 형 모드를 사용 하는 간단한 애니메이션을 통해 사용자에 게 더 나은 환경을 만들 수 있습니다.

- [사용자 콘텐츠](#user_content) &ndash; 향상 된 인쇄 api를 통해 저장소 액세스 프레임 워크를 사용 하 여 간소화 된 사용자 파일 관리, 그림, 웹 사이트 및 기타 콘텐츠 인쇄를 더 쉽게 수행할 수 있습니다.

- [하드웨어](#hardware) Nfc 호스트 기반 카드 에뮬레이션을 사용 하 여 앱을 nfc 카드로 전환 하 고를 `SensorManager` 사용 하 여 저전력 센서를 실행 합니다. &ndash;

- [개발자 도구](#developer_tools) &ndash; Android Debug Bridge 클라이언트를 사용 하 여 동영상 가이드 응용 프로그램을 Android SDK 일부로 사용할 수 있습니다.

이 가이드에서는 기존 Xamarin.ios 응용 프로그램을 KitKat로 마이그레이션하기 위한 지침 뿐만 아니라 Xamarin.ios 개발자를 위한 KitKat에 대 한 개략적인 개요를 제공 합니다.

## <a name="requirements"></a>요구 사항

KitKat를 사용 하 여 Xamarin Android 응용 프로그램을 개발 하려면 다음 스크린샷에 표시 된 것 처럼 Android SDK Manager를 통해 *xamarin android 4.11.0* 이상 및 Android 4.4 (API 수준 19)를 설치 해야 합니다.

[![Android SDK Manager에서 Android 4.4 선택](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>앱을 KitKat로 마이그레이션

이 섹션에서는 기존 응용 프로그램을 Android 4.4로 전환 하는 데 도움이 되는 일부 첫 번째 응답 항목을 제공 합니다.

### <a name="check-system-version"></a>시스템 버전 확인

응용 프로그램이 이전 버전의 Android와 호환 되어야 하는 경우 아래 코드 샘플에 나와 있는 것 처럼 시스템 버전 검사에서 KitKat 특정 코드를 래핑해야 합니다.

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>알람 일괄 처리

Android는 지정 된 시간에 백그라운드에서 응용 프로그램의 절전 모드를 해제 하는 데 경보 서비스를 사용 합니다. KitKat는 성능을 유지 하기 위해 경보를 일괄 처리 하 여이 단계를 더 수행 합니다. 즉, 각 응용 프로그램을 정확히 시간에 절전 모드에서 해제 하는 대신, KitKat는 동일한 시간 간격 동안 절전 모드 해제에 등록 된 여러 응용 프로그램을 그룹화 하 고 동시에 절전 모드를 해제 합니다.
Android에서 지정 된 시간 간격 동안 앱의 절전 모드를 해제 하도록 `SetWindow` 지시 하려면 [`AlarmManager`](xref:Android.App.AlarmManager)에 대해를 호출 하 여 앱이 해제 될 때까지 경과할 수 있는 최소 및 최대 시간 (밀리초)을 전달 하 고 절전 모드 해제 시 수행할 작업을 지정 합니다.
다음 코드는 기간이 30 분에서 1 시간 사이에 해제 해야 하는 응용 프로그램의 예를 제공 합니다.

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

앱을 정확한 시간에 계속 해 서 절전 모드에서 `SetExact`해제 하려면를 사용 하 여 앱을 해제 하는 정확한 시간 및 수행할 작업을 전달 합니다.

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

KitKat를 사용 하면 정확 하 게 반복 되는 경보를 설정할 수 없습니다. 을 사용 하는 응용 프로그램[`SetRepeating`](xref:Android.App.AlarmManager.SetRepeating*)
그리고 정확한 경보를 사용 하려면 이제 각 경보를 수동으로 트리거해야 합니다.

### <a name="external-storage"></a>외부 스토리지

외부 저장소는 이제 응용 프로그램에 고유한 저장소와 여러 응용 프로그램에서 공유 되는 데이터의 두 가지 유형으로 나뉩니다. 외부 저장소에서 앱의 특정 위치를 읽고 쓰려면 특별 한 권한이 필요 하지 않습니다. 이제 공유 저장소의 데이터와 상호 작용 하려면 `READ_EXTERNAL_STORAGE` 또는 `WRITE_EXTERNAL_STORAGE` 권한이 필요 합니다. 이러한 두 가지 형식은 다음과 같이 분류할 수 있습니다.

- 에서 `Context` 메서드를 호출 하 여 파일 또는 디렉터리 경로를 가져오는 경우 (예:[`GetExternalFilesDir`](xref:Android.Content.Context.GetExternalFilesDir*)
  디스크나[`GetExternalCacheDirs`](xref:Android.Content.Context.GetExternalCacheDirs)
  - 앱에 추가 권한이 필요 하지 않습니다.

- 속성에 액세스 하거나에서 `Environment` 메서드를 호출 하 여 파일 또는 디렉터리 경로를 가져오는 경우[`GetExternalStorageDirectory`](xref:Android.OS.Environment.ExternalStorageDirectory)
  디스크나[`GetExternalStoragePublicDirectory`](xref:Android.OS.Environment.GetExternalStoragePublicDirectory*)
  앱에는 `READ_EXTERNAL_STORAGE` 또는 `WRITE_EXTERNAL_STORAGE` 권한이 필요 합니다.

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE`는 `READ_EXTERNAL_STORAGE` 사용 권한을 암시 하므로 하나의 사용 권한만 설정할 필요가 있습니다.

### <a name="sms-consolidation"></a>SMS 통합

KitKat 사용자가 선택한 하나의 기본 응용 프로그램에서 모든 SMS 콘텐츠를 집계 하 여 사용자에 대 한 메시징을 간소화 합니다. 개발자는 앱을 기본 메시징 응용 프로그램으로 선택할 수 있도록 하 고, 응용 프로그램을 선택 하지 않은 경우 코드에서 적절 하 게 작동 합니다. SMS 앱을 KitKat로 전환 하는 방법에 대 한 자세한 내용은 Google의 [KitKat에 대 한 Sms 앱 준비](http://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) 하기 가이드를 참조 하세요.

### <a name="webview-apps"></a>웹 보기 앱

[웹 보기](xref:Android.Webkit.WebView) 는 KitKat에서 개조을 얻었습니다. 가장 큰 변화는에 콘텐츠 `WebView`를 로드 하는 보안을 추가 하는 것입니다. 이전 API 버전을 대상으로 하는 대부분의 응용 프로그램은 정상적으로 작동 하지만 `WebView` 클래스를 사용 하는 응용 프로그램을 테스트 하는 것이 좋습니다. 영향을 받는 웹 보기 Api에 대 한 자세한 내용은 android [4.4 설명서의 android에서 웹 보기로 마이그레이션](https://developer.android.com/guide/webapps/migrating.html) 을 참조 하세요.

<a name="user_experience" />

## <a name="user-experience"></a>사용자 환경

KitKat에는 속성 애니메이션 처리를 위한 새로운 전환 프레임 워크 및 테마를 위한 반투명 UI 옵션을 포함 하 여 사용자 환경을 향상 시키기 위한 몇 가지 새로운 Api가 제공 됩니다. 이러한 변경 내용은 아래에 설명 되어 있습니다.

### <a name="transition-framework"></a>전환 프레임 워크

전환 프레임 워크를 사용 하면 애니메이션을 보다 쉽게 구현할 수 있습니다. KitKat를 사용 하면 코드를 한 줄만 사용 하 여 간단한 속성 애니메이션을 수행 하거나 *장면을*사용 하 여 전환을 사용자 지정할 수 있습니다.

#### <a name="simple-property-animation"></a>Simple 속성 애니메이션

새 Android 전환 라이브러리는 코드 뒤에 속성 애니메이션을 단순화 합니다. 프레임 워크를 사용 하면 최소한의 코드로 간단한 애니메이션을 수행할 수 있습니다. 예를 들어 다음 코드 샘플에서는를 사용 합니다.[`TransitionManager.BeginDelayedTransition`](xref:Android.Transitions.TransitionManager.BeginDelayedTransition*)
표시 및 숨기기 `TextView`에 애니메이션 효과를 주려면:

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

위의 예제에서는 전환 프레임 워크를 사용 하 여 변경 된 속성 값 간에 자동 기본 전환을 만듭니다. 애니메이션은 한 줄의 코드로 처리 되기 때문에 시스템 버전 검사에서 `BeginDelayedTransition` 호출을 래핑하여 이전 버전의 Android와 호환 되도록 손쉽게 설정할 수 있습니다. 자세한 내용은 [앱을 KitKat으로 마이그레이션](#Migrating_Your_App_to_KitKat) 섹션을 참조 하세요.

아래 스크린샷은 애니메이션 전의 앱을 보여 줍니다.

[![애니메이션을 시작 하기 전의 앱 스크린샷](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

아래 스크린샷은 애니메이션 후의 앱을 보여 줍니다.

[![애니메이션이 완료 된 후의 앱 스크린샷](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

다음 섹션에서 설명 하는 장면 전환에 대 한 더 많은 제어를 얻을 수 있습니다.

#### <a name="android-scenes"></a>Android 장면

[백그라운드](xref:Android.Transitions.Scene) 는 개발자가 애니메이션을 더 효과적으로 제어할 수 있도록 전환 프레임 워크의 일부로 도입 되었습니다. UI에서 동적 영역 만들기: 컨테이너 내의 XML 콘텐츠에 대해 컨테이너와 여러 버전 또는 "장면"을 지정 하 고, Android는 백그라운드 간 전환에 애니메이션 효과를 주기 위해 나머지 작업을 수행 합니다. Android 장면을 사용 하면 개발 측에서 최소한의 작업으로 복잡 한 애니메이션을 빌드할 수 있습니다.

동적 콘텐츠를 보유 하는 정적 UI 요소는 *컨테이너* 또는 *장면 기반*이라고 합니다. 아래 예제에서는 Android Designer를 사용 하 여 호출 `RelativeLayout` `container`된를 만듭니다.

[![Android Designer를 사용 하 여 RelativeLayout 컨테이너 만들기](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

샘플 레이아웃에는 아래에 `sceneButton` `container`라는 단추도 정의 되어 있습니다. 이 단추를 클릭 하면 전환이 트리거됩니다.

컨테이너 내의 동적 콘텐츠에는 새로운 두 개의 Android 레이아웃이 필요 합니다. 이러한 레이아웃은 컨테이너 *내의* 코드만 지정 합니다.
아래 예제 코드는 각각 "Kit" 및 "Kat"를 읽는 두 개의 텍스트 필드가 포함 된 *Scene1* 라는 레이아웃을 정의 하 고 같은 텍스트 필드가 반전 된 *Scene2* 라는 두 번째 레이아웃을 정의 합니다. XML은 다음과 같습니다.

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

위의 예에서는를 `merge` 사용 하 여 뷰 코드를 더 짧게 만들고 뷰 계층 구조를 단순화 합니다. 레이아웃에 대 한 자세한 `merge` 내용은 [여기](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html)를 참조 하세요.

아래 코드 예제에 나와 있는 [`Scene.GetSceneForLayout`](xref:Android.Transitions.Scene.GetSceneForLayout*)것 처럼를 호출 하 고 컨테이너 개체, 장면 레이아웃 파일의 리소스 ID 및 현재 `Context`를 전달 하 여 장면을 만듭니다.

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

단추를 클릭 하면 두 장면 사이에서 대칭 이동 되며, Android에서 기본 전환 값을 사용 하 여 애니메이션 효과를 적용 합니다.

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

아래 스크린샷은 애니메이션 전의 장면을 보여줍니다.

[![애니메이션을 시작 하기 전의 앱 스크린샷](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

아래 스크린샷은 애니메이션 후의 장면을 보여줍니다.

[![애니메이션이 완료 된 후의 앱 스크린샷](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)

> [!NOTE]
> Android 전환 라이브러리에는 사용자가 두 번째로 활동을 탐색할 때를 사용 `GetSceneForLayout` 하 여 만든 장면을 중단 시키는 [알려진 버그가](https://code.google.com/p/android/issues/detail?id=62450) 있습니다. Java 해결 방법은 [여기](http://www.doubleencore.com/2013/11/new-transitions-framework/)에 설명 되어 있습니다.

##### <a name="custom-transitions-in-scenes"></a>사용자 지정 전환 장면

아래 스크린샷에 표시 된 것 처럼 아래 `transition` `Resources`에 있는 디렉터리의 xml 리소스 파일에서 사용자 지정 전환을 정의할 수 있습니다.

[![리소스/전환 디렉터리 아래의 전환 xml 파일 위치](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

다음 코드 샘플에서는 5 초 동안 애니메이션 효과를 주는 전환을 정의 하 고 [오버 보간 기](https://developer.android.com/reference/android/views/animation/OvershootInterpolator.html)를 사용 합니다.

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

아래 코드에 나와 있는 것 처럼 [TransitionInflater](xref:Android.Transitions.TransitionInflater)를 사용 하 여 활동에서 전환이 만들어집니다.

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

그런 다음 애니메이션을 시작 하는 `Go` 호출에 새 전환이 추가 됩니다.

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>반투명 UI

KitKat를 사용 하면 선택적 반투명 상태 및 탐색 모음으로 앱에 테마를 지정 하는 기능을 더 효과적으로 제어할 수 있습니다. Android 테마를 정의 하는 데 사용 하는 것과 동일한 XML 파일에서 시스템 UI 요소의 반투명도를 변경할 수 있습니다. KitKat에서는 다음 속성을 소개 합니다.

- `windowTranslucentStatus`-True로 설정 하면 위쪽 상태 표시줄을 반투명 하 게 만듭니다.

- `windowTranslucentNavigation`-True로 설정 하면 아래쪽 탐색 막대가 반투명 하 게 표시 됩니다.

- `fitsSystemWindows`-위쪽 또는 아래쪽 막대를 transcluent로 설정 하면 기본적으로 투명 UI 요소에서 콘텐츠가 이동 합니다. 이 속성을로 `true` 설정 하는 것은 콘텐츠가 반투명 시스템 UI 요소와 겹치지 않도록 하는 간단한 방법입니다.

다음 코드에서는 반투명 상태와 탐색 모음이 있는 테마를 정의 합니다.

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

아래 스크린샷은 반투명 상태 및 탐색 모음이 있는 위의 테마를 보여줍니다.

[![반투명 상태 및 탐색 모음이 있는 앱의 예제 스크린샷](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content" />

## <a name="user-content"></a>사용자 콘텐츠

### <a name="storage-access-framework"></a>저장소 액세스 프레임 워크

SAF (저장소 액세스 프레임 워크)는 사용자가 이미지, 비디오 및 문서와 같은 저장 된 콘텐츠와 상호 작용 하는 새로운 방법입니다. 사용자에 게 콘텐츠를 처리 하는 응용 프로그램을 선택할 수 있는 대화 상자를 제공 하는 대신 KitKat는 사용자가 하나의 집계 위치에서 데이터에 액세스할 수 있도록 하는 새 UI를 엽니다. 콘텐츠를 선택 하면 사용자가 콘텐츠를 요청한 응용 프로그램으로 돌아가면 앱 환경이 정상적으로 계속 됩니다.

이렇게 변경 하려면 개발자 쪽에 두 가지 작업이 필요 합니다. 먼저 공급자의 콘텐츠를 필요로 하는 앱을 콘텐츠를 요청 하는 새로운 방법으로 업데이트 해야 합니다. 둘째,에 데이터 `ContentProvider` 를 쓰는 응용 프로그램은 새 프레임 워크를 사용 하도록 수정 해야 합니다. 두 시나리오 모두 새로운[`DocumentsProvider`](xref:Android.Provider.DocumentsProvider)
API.

#### <a name="documentsprovider"></a>DocumentsProvider

KitKat에서는 `DocumentsProvider` 클래스를 사용 `ContentProviders` 하 여와의 상호 작용을 추상화 합니다. 즉, saf는 API를 `DocumentsProvider` 통해 액세스할 수 있는 한 데이터의 물리적 위치를 고려 하지 않습니다. 로컬 공급자, 클라우드 서비스 및 외부 저장 장치는 모두 동일한 인터페이스를 사용 하며 동일한 방식으로 처리 되므로 사용자와 개발자에 게 사용자의 콘텐츠와 상호 작용할 수 있는 한 곳을 제공 합니다.

이 섹션에서는 저장소 액세스 프레임 워크를 사용 하 여 콘텐츠를 로드 하 고 저장 하는 방법을 설명 합니다.

#### <a name="request-content-from-a-provider"></a>공급자의 콘텐츠 요청

장치에서 사용할 수 있는 모든 콘텐츠 공급자에 연결 하려는 것을 의미 하는 `ActionOpenDocument` 의도를 사용 하 여 saf UI를 사용 하 여 콘텐츠를 선택 하도록 KitKat에 지시할 수 있습니다. 을 지정 `CategoryOpenable`하 여 이러한 의도에 일부 필터링을 추가할 수 있습니다. 즉, 열 수 있는 콘텐츠 (즉, 액세스 가능한 콘텐츠)만 반환 됩니다. 또한 KitKat를 사용 하면를 `MimeType`사용 하 여 콘텐츠를 필터링 할 수 있습니다. 예를 들어 아래 코드는 이미지 `MimeType`를 지정 하 여 이미지 결과를 필터링 합니다.

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

를 `StartActivityForResult` 호출 하면 사용자가 이미지를 선택 하 여 선택할 수 있는 saf UI가 시작 됩니다.

[![이미지를 검색 하기 위한 저장소 액세스 프레임 워크를 사용 하는 앱의 예제 스크린샷](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

사용자가 이미지를 선택한 후에 `OnActivityResult` 는 선택한 파일 `Android.Net.Uri` 의를 반환 합니다. 아래 코드 샘플은 사용자의 이미지 선택 항목을 표시 합니다.

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

#### <a name="write-content-to-a-provider"></a>공급자에 콘텐츠 쓰기

KitKat는 saf UI에서 콘텐츠를 로드 하는 것 외에도 `ContentProvider` `DocumentProvider` API를 구현 하는에 콘텐츠를 저장할 수 있습니다. 콘텐츠를 저장 하려면 `Intent` 다음 `ActionCreateDocument`을 사용 합니다.

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

위의 코드 샘플에서는 SAF UI를 로드 하 여 사용자가 파일 이름을 변경 하 고 새 파일을 저장할 디렉터리를 선택 합니다.

[![다운로드 디렉터리에서 파일 이름을 NewDoc로 변경 하는 사용자의 스크린샷](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

사용자가 **저장**을 누를 때 `OnActivityResult` 를 사용 `data.Data`하 `Android.Net.Uri` 여 액세스할 수 있는 새로 만든 파일의를 전달 합니다. Uri는 새 파일에 데이터를 스트리밍하는 데 사용할 수 있습니다.

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

다음 사항에 유의 하십시오.[`ContentResolver.OpenOutputStream(Android.Net.Uri)`](xref:Android.Content.ContentResolver.OpenOutputStream*)
는 `System.IO.Stream`를 반환 하므로 전체 스트리밍 프로세스를 .net으로 작성할 수 있습니다.

저장소 액세스 프레임 워크를 사용 하 여 콘텐츠를 로드, 작성 및 편집 하는 방법에 대 한 자세한 내용은 [Android 설명서에서 저장소 액세스 프레임 워크](https://developer.android.com/guide/topics/providers/document-provider.html)를 참조 하세요.

### <a name="printing"></a>인쇄

인쇄 콘텐츠는 [인쇄 서비스](xref:Android.PrintServices) 및 `PrintManager`를 도입 하 여 KitKat에서 간소화 됩니다. KitKat는 [Google Cloud print 응용 프로그램](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint)을 사용 하 여 [Google의 클라우드 인쇄 서비스 api](https://developers.google.com/cloud-print/) 를 완전히 활용 하는 첫 번째 API 버전 이기도 합니다.
KitKat와 함께 제공 되는 대부분의 장치는 Google Cloud 인쇄 앱 및 [HP 인쇄 서비스 플러그 인](https://play.google.com/store/apps/details?id=com.hp.android.printservice)을 WiFi에 처음 연결할 때 자동으로 다운로드 합니다. 사용자는 **설정 > 시스템 > 인쇄**로 이동 하 여 장치의 인쇄 설정을 확인할 수 있습니다.

[![인쇄 설정 화면의 예제 스크린샷](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)

> [!NOTE]
> 인쇄 Api는 기본적으로 Google Cloud Print와 함께 작동 하도록 설정 되어 있지만 개발자가 새 Api를 사용 하 여 인쇄 콘텐츠를 준비 하 고 다른 응용 프로그램에 보내 인쇄를 처리할 수 있습니다.

#### <a name="printing-html-content"></a>HTML 콘텐츠 인쇄

KitKat는를 사용 [`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter) 하 여 `WebView.CreatePrintDocumentAdapter`웹 뷰에 대해를 자동으로 만듭니다. 웹 콘텐츠를 인쇄 하는 작업은 HTML [`WebViewClient`](xref:Android.Webkit.WebViewClient) 콘텐츠가 로드 될 때까지 대기 하는와 작업에서 옵션 메뉴의 인쇄 옵션을 사용할 수 있도록 하는 것과 사용자가 인쇄 옵션을 선택 하 고를 호출할 때까지 대기 하는 작업을 알 수 있도록 하는 것입니다. `Print`에서 .`PrintManager` 이 섹션에서는 화면에 HTML 콘텐츠를 인쇄 하는 데 필요한 기본 설정에 대해 설명 합니다.

웹 콘텐츠를 로드 하 고 인쇄 하려면 인터넷 권한이 필요 합니다.

[![앱 옵션에서 인터넷 사용 권한 설정](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>인쇄 메뉴 항목

일반적으로 인쇄 옵션은 활동의 [옵션 메뉴](https://developer.android.com/guide/topics/ui/menus.html#options-menu)에 표시 됩니다.
사용자는 옵션 메뉴를 사용 하 여 활동에 대 한 작업을 수행할 수 있습니다. 화면의 오른쪽 위 모서리에 있으며 다음과 같이 표시 됩니다.

[![화면의 오른쪽 위 모서리에 표시 되는 인쇄 메뉴 항목의 예제 스크린샷](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)

*리소스*의 *메뉴*디렉터리에서 추가 메뉴 항목을 정의할 수 있습니다. 아래 코드는 [Print](xref:Android.Print.PrintManager)라는 샘플 메뉴 항목을 정의 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

활동의 옵션 메뉴와의 상호 작용은 및 `OnCreateOptionsMenu` `OnOptionsItemSelected` 메서드를 통해 수행 됩니다.
`OnCreateOptionsMenu`*메뉴* 리소스 디렉터리에서 인쇄 옵션과 같은 새 메뉴 항목을 추가 하는 장소입니다.
`OnOptionsItemSelected`메뉴에서 인쇄 옵션을 선택 하 고 인쇄를 시작 하는 사용자를 수신 합니다.

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

위의 코드는 HTML 콘텐츠의 상태를 추적 `dataLoaded` 하기 위해 라는 변수도 정의 합니다. 모든 `WebViewClient` 콘텐츠가 로드 될 때에서이 변수를 true로 설정 하므로 작업에서 옵션 메뉴에 인쇄 메뉴 항목을 추가 하는 것을 알 수 있습니다.

##### <a name="webviewclient"></a>WebViewClient

의 `WebViewClient` 작업은 인쇄 옵션이 `OnPageFinished` 메서드와 함께 사용 되는 `WebView` 메뉴에 표시 되기 전에의 데이터가 완전히 로드 되도록 하는 것입니다. `OnPageFinished`는 웹 콘텐츠가 로드를 완료할 때까지 수신 하 고 작업에서 해당 옵션 메뉴 `InvalidateOptionsMenu`를 다시 만들도록 합니다.

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

`OnPageFinished`는 `dataLoaded` 값도로 `true`설정 하므로 `OnCreateOptionsMenu` 인쇄 옵션이 제자리에 있는 메뉴를 다시 만들 수 있습니다.

##### <a name="printmanager"></a>PrintManager

다음 코드 예제에서는의 `WebView`내용을 인쇄 합니다.

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print`인수로 사용: 인쇄 작업 이름 (이 예제에서는 "MyWebPage 페이지"),[`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter)
그러면 콘텐츠에서 인쇄 문서를 생성 합니다.[`PrintAttributes`](xref:Android.Print.PrintAttributes)
(`null` 위 예제에서). 기본 특성이 대부분 `PrintAttributes` 의 시나리오를 처리 해야 하는 경우에도 인쇄 된 페이지에서 콘텐츠를 레이아웃 하는 데 도움이 되도록 지정할 수 있습니다.

를 `Print` 호출 하면 인쇄 작업에 대 한 옵션이 나열 된 인쇄 UI가 로드 됩니다. UI는 아래 스크린샷에 나와 있는 것 처럼 사용자에 게 HTML 콘텐츠를 PDF에 인쇄 하거나 저장할 수 있는 옵션을 제공 합니다.

[![인쇄 메뉴를 표시 하는 PrintHtmlActivity의 스크린샷](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![PDF로 저장 메뉴를 표시 하는 PrintHtmlActivity의 스크린샷](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware" />

## <a name="hardware"></a>하드웨어

KitKat는 새 장치 기능을 수용 하기 위해 몇 가지 Api를 추가 합니다. 가장 주목할 만한 것은 호스트 기반 카드 에뮬레이션 및 새 `SensorManager`입니다.

### <a name="host-based-card-emulation-in-nfc"></a>NFC의 호스트 기반 카드 에뮬레이션

HCE (호스트 기반 카드 에뮬레이션)를 통해 응용 프로그램은 통신 회사의 독점 보안 요소에 의존 하지 않고 NFC 카드 또는 NFC 카드 판독기 처럼 작동할 수 있습니다. HCE를 설정 하기 전에 다음을 사용 하 여 `PackageManager.HasSystemFeature`장치에서 hce를 사용할 수 있는지 확인 합니다.

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

Hce를 사용 하려면 `Nfc` `AndroidManifest.xml`다음과 같이 hce 기능과 사용 권한을 모두 응용 프로그램에 등록 해야 합니다.

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![앱 옵션에서 NFC 권한 설정](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

작업 하려면 HCE를 백그라운드에서 실행할 수 있어야 하 고, 사용자가 NFC 트랜잭션을 수행할 때 HCE를 사용 하는 응용 프로그램이 실행 되 고 있지 않은 경우에도이를 시작 해야 합니다. HCE 코드를로 `Service`작성 하 여이를 수행할 수 있습니다. Hce 서비스는 다음 메서드 `HostApduService` 를 구현 하는 인터페이스를 구현 합니다.

- *Processcommandapdu* -APDU (응용 프로그램 프로토콜 데이터 단위)는 NFC 판독기와 Hce 서비스 간에 전송 되는 항목입니다. 이 메서드는 판독기에서 ADPU를 사용 하 고 응답으로 데이터 단위를 반환 합니다.

- *Ondeactivated* `HostAdpuService` 됨-hce 서비스가 더 이상 NFC 판독기와 통신 하지 않는 경우 비활성화 됩니다.

또한 HCE 서비스는 응용 프로그램의 매니페스트에 등록 하 고 적절 한 권한, 의도 필터 및 메타 데이터로 데코 레이트 되어야 합니다. 다음 코드는 `Service` 특성을 사용 하 여 `HostApduService` android Manifest에 등록 된의 예제입니다. 특성에 대 한 자세한 내용은 [android 매니페스트](~/android/platform/android-manifest.md) 를 사용 하 여 Xamarin 작업 가이드를 참조 하세요.

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

위의 서비스는 NFC 판독기가 응용 프로그램과 상호 작용 하는 방법을 제공 하지만, NFC 판독기는이 서비스에서 검사 해야 하는 NFC 카드를 에뮬레이션 하 고 있는지 여부를 여전히 알 수 없습니다. NFC 판독기가 서비스를 식별 하는 데 도움이 되도록 서비스에 고유한 *응용 프로그램 ID (지원)* 를 할당할 수 있습니다. `MetaData` 특성에 등록 된 xml 리소스 파일에서 hce 서비스에 대 한 다른 메타 데이터와 함께 지원 정보를 지정 합니다 (위의 코드 예제 참조). 이 리소스 파일에 하나 이상의 지원 필터-하나 이상의 NFC 판독기 장치 지원에 해당 하는 16 진수 형식의 고유 식별자 문자열을 지정 합니다.

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

Xml 리소스 파일은 지원 필터 외에도 HCE 서비스에 대 한 사용자 관련 설명을 제공 하 고, 지원 그룹 (지불 응용 프로그램 및 "기타")을 지정 하며, 지불 응용 프로그램의 경우 사용자에 게 표시 되는 260x96 dp 배너를 제공 합니다.

위에 설명 된 설정은 NFC 카드를 에뮬레이트하는 응용 프로그램에 대 한 기본 구성 요소를 제공 합니다. NFC 자체를 사용 하려면 몇 가지 추가 단계와 추가 테스트를 구성 해야 합니다. 호스트 기반 카드 에뮬레이션에 대 한 자세한 내용은 [Android 설명서 포털](https://developer.android.com/guide/topics/connectivity/nfc/hce.html)을 참조 하세요.
Xamarin과 함께 NFC를 사용 하는 방법에 대 한 자세한 내용은 [XAMARIN NFC 샘플](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample)을 확인 하세요.

### <a name="sensors"></a>센서

KitKat는를 [`SensorManager`](xref:Android.Hardware.SensorManager)통해 장치의 센서에 대 한 액세스를 제공 합니다.
에서 `SensorManager` OS는 응용 프로그램에 센서 정보를 배달 하도록 예약 하 여 배터리 수명을 보존할 수 있습니다.

KitKat에는 사용자의 단계를 추적 하기 위한 두 가지 새 센서 유형도 함께 제공 됩니다. 이러한 작업은가 속도계를 기반으로 하며 다음을 포함 합니다.

- 단계 *탐지기* -사용자가 단계를 수행 하면 앱이 알림/해제 되며, 탐지기는 단계가 발생 했을 때의 시간 값을 제공 합니다.

- *Stcounter* - *다음 장치를 다시 부팅할 때까지*센서가 등록 된 이후에 사용자가 수행한 단계 수를 추적 합니다.

아래 스크린샷에서는 작업의 단계 카운터를 보여 줍니다.

[![단계 카운터를 표시 하는 SensorsActivity 앱의 스크린샷](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

를 호출 `SensorManager` `GetSystemService(SensorService)` 하 고 결과를로 `SensorManager`캐스팅 하 여를 만들 수 있습니다. 단계 카운터를 사용 하려면 `GetDefaultSensor` `SensorManager`에서을 호출 합니다. 센서를 등록 하 고 다음의 도움말을 사용 하 여 단계 수의 변경 내용을 수신할 수 있습니다.[`ISensorEventListener`](xref:Android.Hardware.ISensorEventListener)
아래 코드 샘플에 나와 있는 것 처럼 인터페이스.

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

`OnSensorChanged`응용 프로그램이 전경에 있는 동안 단계 수가 업데이트 되 면가 호출 됩니다. 응용 프로그램이 백그라운드에 들어가거나 장치가 절전 모드인 `OnSensorChanged` 경우는 호출 되지 않습니다. 그러나가 호출 될 때까지 `UnregisterListener` 단계가 계속 계산 됩니다.

*단계 수 값은 센서를 등록 하는 모든 응용 프로그램에서 누적*된다는 점에 유의 하세요. 즉, 응용 프로그램을 제거 하 고 다시 설치 하 고 응용 프로그램 시작 `count` 시 0에서 변수를 초기화 하는 경우에도 센서에서 보고 하는 값은 센서가 등록 된 동안 수행 된 총 단계 수를 유지 합니다. 응용 프로그램 또는 다른. 아래 코드에 나와 있는 것 처럼에서를 호출 `UnregisterListener` `SensorManager`하 여 응용 프로그램이 단계 카운터에 추가 되지 않도록 할 수 있습니다.

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

장치를 다시 부팅 하면 단계 수가 0으로 다시 설정 됩니다. 센서 또는 장치의 상태를 사용 하는 다른 응용 프로그램에 관계 없이 응용 프로그램에 대 한 정확한 카운트를 보고 하도록 앱에 추가 코드가 필요 합니다.

> [!NOTE]
> 단계 검색 및 계산에 대 한 API는 KitKat와 함께 제공 되지만 모든 전화가 센서와 outfitted 않습니다. 을 실행 `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`하 여 센서를 사용할 수 있는지 확인 하거나, `null`의 `GetDefaultSensor` 반환 된 값이이 아닌지 확인 합니다.

<a name="developer_tools" />

## <a name="developer-tools"></a>개발자 도구

### <a name="screen-recording"></a>화면 녹화

KitKat에는 개발자가 응용 프로그램을 실행할 수 있도록 새로운 화면 녹화 기능이 포함 되어 있습니다. 화면 기록은 Android SDK의 일부로 다운로드할 수 있는 [ADB (Android Debug Bridge](https://developer.android.com/tools/help/adb.html) ) 클라이언트를 통해 사용할 수 있습니다.

화면을 기록 하려면 장치를 연결 합니다. 그런 다음 Android SDK 설치를 찾고 **플랫폼 도구** 디렉터리로 이동한 후 **adb** 클라이언트를 실행 합니다.

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

위의 명령은 기본 해상도 (4Mbps로 기본 3 분 비디오를 기록 합니다. 길이를 편집 하려면 *--시간 제한* 플래그를 추가 합니다.
해상도를 변경 하려면 *--비트 전송률* 플래그를 추가 합니다. 다음 명령은 8Mbps에서 분 길이의 비디오를 기록 합니다.

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

장치에서 비디오를 찾을 수 있습니다. 기록이 완료 되 면 갤러리에 표시 됩니다.

## <a name="other-kitkat-additions"></a>기타 KitKat 추가

위에서 설명한 변경 내용 외에도 KitKat를 사용 하면 다음을 수행할 수 있습니다.

- *전체 화면을 사용* 하 여 콘텐츠 검색, 게임 재생, 전체 화면 환경에서 혜택을 받을 수 있는 다른 응용 프로그램 실행을 위한 새로운 [몰입 형 모드](https://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int)) 를 KitKat.

- *알림 사용자 지정* -를 사용 하 여 시스템 알림에 대 한 추가 세부 정보를 가져옵니다.[`NotificationListenerService`](xref:Android.Service.Notification.NotificationListenerService)
  . 이를 통해 앱 내에서 다른 방식으로 정보를 제공할 수 있습니다.

- *그릴 수 있는 리소스 미러링* -그릴 수 있는 리소스에 새로운[`autoMirrored`](https://developer.android.com/reference/android/R.attr.html#autoMirrored)
  시스템에서 왼쪽에서 오른쪽 레이아웃으로의 대칭 이동이 필요한 이미지에 대해 미러된 버전을 만들도록 지시 하는 특성입니다.

- *애니메이션 일시 중지* -를 사용 하 여 만든 애니메이션을 일시 중지 하 고 다시 시작 합니다.[`Animator`](xref:Android.Animation.Animator)
  포함됩니다.

- *동적으로 변경 되는 텍스트 읽기* -새 텍스트를 사용 하 여 동적으로 업데이트 되는 UI 파트를 새 텍스트와 함께 "라이브 영역"으로 표시 합니다.[`accessibilityLiveRegion`](https://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion)
  새 텍스트를 접근성 모드에서 자동으로 읽도록 특성입니다.

- *오디오 환경 개선* -를 사용 하 여 트랙을 크게 만듭니다.[`LoudnessEnhancer`](xref:Android.Media.Audiofx.LoudnessEnhancer)
  에서 다음을 사용 하 여 오디오 스트림의 최고 및 RMS를 찾습니다.[`Visualizer`](xref:Android.Media.Audiofx.Visualizer.MeasurementModePeakRms)
  클래스를 사용 하 고 오디오 [타임 스탬프](xref:Android.Media.AudioTimestamp) 에서 정보를 가져와 오디오-비디오 동기화에 도움을 줍니다.

- *사용자 지정 간격에 따라 ContentResolver 동기화* -KitKat은 동기화 요청이 수행 되는 시간에 일부 가변성을 추가 합니다. 를 호출 `ContentResolver` `ContentResolver.RequestSync` 하 고를 `SyncRequest`전달 하 여 사용자 지정 시간 또는 간격으로를 동기화 합니다.

- *컨트롤러를 구분* 합니다. KitKat 컨트롤러에는 장치의 `ControllerNumber` 속성을 통해 액세스할 수 있는 고유한 정수 식별자가 할당 됩니다. 이렇게 하면 게임에서 플레이어를 쉽게 구분할 수 있습니다.

- *원격 제어* -하드웨어 및 소프트웨어 쪽에서 몇 가지 사항이 변경 되 면 KitKat를 사용 하 여 IR 전송기를 사용 하는 장치 outfitted을를 사용 하 여 원격 `ConsumerIrService`제어로 전환 하 고, 새로운 기능을 사용 하 여 주변 장치를 조작할 수 있습니다.[`RemoteController`](xref:Android.Media.RemoteController)
  Api.

위의 API 변경 내용에 대 한 자세한 내용은 Google [Android 4.4 api](https://developer.android.com/about/versions/android-4.4.html) 개요를 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 Android 4.4 (API 수준 19)에서 사용할 수 있는 몇 가지 새로운 Api 및 응용 프로그램을 KitKat로 전환할 때 제공 되는 모범 사례를 소개 했습니다. *전환 프레임 워크* 및 *테마*에 대 한 새 옵션을 포함 하 여 사용자 환경에 영향을 주는 api에 대 한 변경 내용이 요약 되어 있습니다. 그런 다음 *저장소 액세스 프레임 워크* 및 `DocumentsProvider` 클래스와 새 *인쇄 api*를 도입 했습니다. *NFC 호스트 기반 카드 에뮬레이션* 및 사용자의 단계를 추적 하기 위한 두 개의 새 센서를 비롯 한 *저전력 센서*를 사용 하는 방법을 살펴보았습니다. 마지막으로, *화면 녹화*를 사용 하 여 응용 프로그램의 실시간 데모를 캡처하고 자세한 KitKat API 변경 내용 및 추가 목록을 제공 했습니다.

## <a name="related-links"></a>관련 링크

- [KitKat 샘플](https://docs.microsoft.com/samples/xamarin/monodroid-samples/kitkat)
- [Android 4.4 Api](https://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](https://developer.android.com/about/versions/kitkat.html)
