---
title: KitKat 기능
description: Android 4.4(KitKat)에는 사용자와 개발자를 위한 기능이 가득 제공됩니다. 이 가이드에서는 이러한 기능 중 몇 가지를 중점적으로 설명하고 KitKat을 최대한 활용하는 데 도움이 되는 코드 예제 및 구현 세부 사항을 제공합니다.
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 43061272f3d3486926f38af792ee3b9df0c53670
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027235"
---
# <a name="kitkat-features"></a>KitKat 기능

_Android 4.4(KitKat)에는 사용자와 개발자를 위한 기능이 가득 제공됩니다. 이 가이드에서는 이러한 기능 중 몇 가지를 중점적으로 설명하고 KitKat을 최대한 활용하는 데 도움이 되는 코드 예제 및 구현 세부 사항을 제공합니다._

## <a name="overview"></a>개요

"KitKat"이라고도 하는 Android 4.4(API 레벨 19)는 2013년 말에 릴리스되었습니다. KitKat은 다음을 비롯한 다양하고 새로운 기능과 향상 기능을 제공합니다.

- [사용자 환경](#user_experience) &ndash; 전환 프레임워크, 반투명 상태 및 탐색 모음, 전체 화면 몰입형 모드를 통해 손쉬운 애니메이션이 가능하기 때문에 더 나은 사용자 환경을 만들 수 있습니다.

- [사용자 콘텐츠](#user_content) &ndash; 스토리지 액세스 프레임워크로 간소화된 사용자 파일 관리; 향상된 인쇄 API를 통해 사진, 웹 사이트 및 기타 콘텐츠를 더 쉽게 인쇄할 수 있습니다.

- [하드웨어](#hardware) &ndash; NFC 호스트 기반 카드 에뮬레이션으로 모든 앱을 NFC 카드로 전환하고, `SensorManager`로 저전력 센서를 실행합니다.

- [개발자 도구](#developer_tools) &ndash; Android SDK의 일부로 제공되는 Android Debug Bridge 클라이언트에서 스크린캐스트 애플리케이션이 작동합니다.

이 가이드는 기존 Xamarin.Android 애플리케이션을 KitKat으로 마이그레이션하는 지침과 Xamarin.Android 개발자를 위한 KitKat에 대한 개략적인 개요를 제공합니다.

## <a name="requirements"></a>요구 사항

KitKat을 사용하여 Xamarin.Android 애플리케이션을 개발하려면 다음 스크린샷과 같이 Android SDK 관리자를 통해 *Xamarin.Android 4.11.0* 이상 및 Android 4.4(API Level 19)를 설치해야 합니다.

[![Android SDK 관리자에서 Android 4.4 선택](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>KitKat으로 앱 마이그레이션

이 섹션에서는 기존 애플리케이션을 Android 4.4로 전환할 수 있는 첫 반응 항목을 몇 가지 제공합니다.

### <a name="check-system-version"></a>시스템 버전 확인

애플리케이션이 이전 Android 버전과 호환되어야 하는 경우에는 아래 코드 샘플에 나와있는 것처럼 시스템 버전 확인에서 KitKat 관련 코드를 래핑해야 합니다.

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>알람 일괄 처리

Android는 알람 서비스를 사용하여 지정된 시간에 백그라운드에서 앱의 절전 모드를 해제합니다. KitKat은 전력을 보존하기 위해 알람을 일괄 처리하여 이 단계를 수행합니다. 즉, KitKat은 각 앱의 절전 모드를 정확한 시간에 해제하는 대신 동일한 시간 간격을 두고 절전 모드를 해제하도록 등록된 여러 애플리케이션을 그룹화하여 동시에 절전 모드를 해제하는 것을 선호합니다.
지정된 시간 간격 동안 앱의 절전 모드를 해제하도록 Android에 알리려면, [`AlarmManager`](xref:Android.App.AlarmManager)에서 `SetWindow`를 호출하여 앱의 절전 모드를 해제하기 전에 경과할 수 있는 최소 및 최대 시간(밀리초 단우)과 절전 모드 해제 시 수행할 작업을 전달합니다.
다음 코드는 시간이 설정된 시간으로부터 30분에서 1시간 사이에 절전 모드를 해제해야 하는 애플리케이션의 예를 제공합니다.

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

정확한 시간에 앱의 절전 모드를 계속 해제하려면 `SetExact`를 사용하여 앱의 절전 모드를 해제할 정확한 시간과 수행할 작업을 전달합니다.

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

정확하게 반복되는 알람을 KitKat에서 더 이상 설정할 수 없습니다. [`SetRepeating`](xref:Android.App.AlarmManager.SetRepeating*)을 사용하고
정확한 알람이 작동해야 하는 애플리케이션은 이제 각 알람을 수동으로 트리거해야 합니다.

### <a name="external-storage"></a>외부 스토리지

외부 스토리지는 이제 애플리케이션 전용 스토리지와 여러 애플리케이션이 데이터를 공유하는 두 가지 유형으로 나뉩니다. 외부 스토리지에서 앱의 특정 위치에 쓰고 읽는 데는 특별한 권한이 필요하지 않습니다. 이제 공유 스토리지의 데이터와 상호 작용하려면 `READ_EXTERNAL_STORAGE` 또는 `WRITE_EXTERNAL_STORAGE` 권한이 필요합니다. 두 가지 유형은 다음과 같이 분류할 수 있습니다.

- `Context`에서 메서드를 호출하여 파일 또는 디렉터리 경로를 가져오는 경우(예: [`GetExternalFilesDir`](xref:Android.Content.Context.GetExternalFilesDir*)
  또는 [`GetExternalCacheDirs`](xref:Android.Content.Context.GetExternalCacheDirs))
  - 앱에 추가 권한이 필요하지 않습니다.

- 속성에 액세스하거나 `Environment`에서 메서드를 호출하여 파일 또는 디렉터리 경로를 가져오는 경우(예: [`GetExternalStorageDirectory`](xref:Android.OS.Environment.ExternalStorageDirectory)
  또는 [`GetExternalStoragePublicDirectory`](xref:Android.OS.Environment.GetExternalStoragePublicDirectory*))
  앱에 `READ_EXTERNAL_STORAGE` 또는 `WRITE_EXTERNAL_STORAGE` 권한이 필요합니다.

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE`는 `READ_EXTERNAL_STORAGE` 권한을 암시하므로 하나의 권한만 설정하면 됩니다.

### <a name="sms-consolidation"></a>SMS 통합

KitKat은 사용자가 선택한 하나의 기본 애플리케이션에 모든 SMS 콘텐츠를 모으는 방식으로 사용자의 메시징 작업을 간소화합니다. 개발자는 앱을 기본 메시징 애플리케이션으로 선택할 수 있도록 해야 하고 애플리케이션이 선택되지 않을 경우 코드에서 그리고 실제로 앱이 적절하게 작동하도록 해야 합니다. SMS 앱을 KitKat으로 전환하는 방법에 대한 자세한 내용은 Google의 [Getting Your SMS Apps Ready for KitKat](https://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html)(KitKat에 맞게 SMS 앱 준비) 가이드를 참조하세요.

### <a name="webview-apps"></a>WebView 앱

[WebView](xref:Android.Webkit.WebView)는 KitKat에서 변모되었습니다. 가장 큰 변화는 `WebView`로 콘텐츠를 로드하기 위해 보안이 추가된 것입니다. 이전 API 버전을 대상으로 하는 대부분의 애플리케이션이 예상대로 작동하지만 `WebView` 클래스를 사용하는 애플리케이션을 테스트하는 것이 좋습니다. 영향을 받는 WebView API에 대한 자세한 정보는 Android [Migrating to WebView in Android 4.4](https://developer.android.com/guide/webapps/migrating.html)(Android 4.4에서 WebView로 마이그레이션) 문서를 참조하세요.

<a name="user_experience" />

## <a name="user-experience"></a>설치 환경

KitKat에는 사용자 환경을 향상시키는 몇 가지 새로운 API가 제공되며, 여기에는 속성 애니메이션 처리를 위한 새로운 전환 프레임워크와 테마 지정을 위한 반투명 UI 옵션이 포함됩니다. 이러한 변화는 아래에 설명되어 있습니다.

### <a name="transition-framework"></a>전환 프레임워크

전환 프레임워크를 사용하면 애니메이션을 보다 쉽게 구현할 수 있습니다. KitKat에서는 코드 한 줄로 간단한 속성 애니메이션을 수행하거나 *Scene*을 사용하여 전환을 사용자 지정할 수 있습니다.

#### <a name="simple-property-animation"></a>간단한 속성 애니메이션

새로운 Android Transitions 라이브러리는 속성 애니메이션의 코드를 간소화합니다. 프레임워크를 사용하면 최소한의 코드로 간단한 애니메이션을 수행할 수 있습니다. 예를 들어, 다음 코드 샘플은 [`TransitionManager.BeginDelayedTransition`](xref:Android.Transitions.TransitionManager.BeginDelayedTransition*)을 사용하여
`TextView` 표시 및 숨기기에 애니메이션 효과를 줍니다.

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

위의 예제는 전환 프레임워크를 사용하여 변하는 속성 값 사이에 자동 기본 전환을 만듭니다. 애니메이션이 코드 한 줄로 처리되기 때문에 `BeginDelayedTransition` 호출을 시스템 버전 확인에 래핑하여 이전 Android 버전과 호환되도록 쉽게 만들 수 있습니다. 자세한 내용은 [KitKat으로 앱 마이그레이션](#Migrating_Your_App_to_KitKat) 섹션을 참조하세요.

아래 스크린샷은 애니메이션 전의 앱을 보여줍니다.

[![애니메이션 시작 전 앱 스크린샷](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

아래 스크린샷은 애니메이션 후의 앱을 보여줍니다.

[![애니메이션 완료 후 앱 스크린샷](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Scene을 통해 전환을 보다 효과적으로 제어할 수 있으며, 이 내용은 다음 섹션에 설명되어 있습니다.

#### <a name="android-scenes"></a>Android Scene

[Scene](xref:Android.Transitions.Scene)은 개발자가 애니메이션을 보다 효과적으로 제어할 수 있도록 전환 프레임워크의 일부로 도입되었습니다. Scene은 UI에서 동적 영역을 만듭니다. 컨테이너 내부의 XML 콘텐츠에 대해 컨테이너 및 몇 가지 버전 또는 "scene"을 지정하면 나머지 작업은 Android가 수행하여 장면 간의 전환에 애니메이션 효과를 줍니다. Android Scene을 사용하면 개발 측에서 최소한의 작업으로 복잡한 애니메이션을 만들 수 있습니다.

동적 컨텐츠를 수용하는 정적 UI 요소를 *컨테이너* 또는 *장면 베이스*라고 합니다. 아래 예제는 Android Designer를 사용하여 `container`라는 `RelativeLayout`을 만듭니다.

[![Android Designer를 사용하여 RelativeLayout 컨테이너 만들기](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

샘플 레이아웃은 `container` 아래에 `sceneButton`이라는 단추도 정의합니다. 이 단추를 누르면 전환이 트리거됩니다.

컨테이너 내의 동적 콘텐츠에는 두 가지 새로운 Android 레이아웃이 필요합니다. 이 레이아웃은 컨테이너 *내부*의 코드만 지정합니다.
아래 예제 코드는 각각 "Kit"과 "Kat"을 읽는 텍스트 필드 두 개를 포함하는 *Scene1*이라는 레이아웃과 동일한 텍스트 필드의 순서가 바뀐 *Scene2*라는 두 번째 레이아웃을 정의합니다. XML은 다음과 같습니다.

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

위의 예에서는 `merge`를 사용하여 뷰 코드를 더 짧게 만들고 뷰 계층 구조를 간소화합니다. `merge` 레이아웃에 대한 자세한 내용은 [여기](https://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html)에서 참조하세요.

아래 코드 예제에 나와있는 것처럼 [`Scene.GetSceneForLayout`](xref:Android.Transitions.Scene.GetSceneForLayout*)을 호출하고, 컨테이너 개체, Scene 레이아웃 파일의 리소스 ID 및 현재 `Context`를 전달하면 Scene이 만들어집니다.

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

단추를 클릭하면 두 Scene이 번갈아 표시되고, Android가 기본 전환 값으로 애니메이션 효과를 적용합니다.

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

아래 스크린샷은 애니메이션 전 장면을 보여줍니다.

[![애니메이션이 시작되기 전의 앱 스크린샷](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

아래 스크린샷은 애니메이션 후 장면을 보여줍니다.

[![애니메이션이 완료된 후의 앱 스크린샷](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)

> [!NOTE]
> Android 전환 라이브러리에는 `GetSceneForLayout`을 사용하여 만든 Scene에 사용자가 두 번째로 Activity를 탐색할 때 깨지는 [알려진 버그](https://code.google.com/p/android/issues/detail?id=62450)가 있습니다. Java 해결 방법이 [여기](http://www.doubleencore.com/2013/11/new-transitions-framework/)에 설명되어 있습니다.

##### <a name="custom-transitions-in-scenes"></a>Scene에서 사용자 지정 전환

사용자 지정 전환은 아래 스크린샷에 표시되어 있는 `Resources` 아래 `transition` 디렉터리에 있는 xml 리소스 파일에서 정의할 수 있습니다.

[![Resources/transition 디렉터리 아래 transition.xml 파일의 위치 ](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

다음 코드 샘플은 5초 동안 애니메이션 효과를 주고 [overshoot interpolator](https://developer.android.com/reference/android/views/animation/OvershootInterpolator.html)를 사용하는 전환을 정의합니다.

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

아래 코드와 같이 [TransitionInflater](xref:Android.Transitions.TransitionInflater)를 사용하여 Activity에 전환이 만들어집니다.

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

그러면 새로운 전환이 애니메이션을 시작하는 `Go` 호출에 추가됩니다.

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>반투명 UI

KitKat에서는 반투명 상태 및 탐색 모음 옵션을 사용하여 앱 테마 지정을 보다 효과적으로 제어할 수 있습니다. Android 테마를 정의하는 데 사용하는 것과 동일한 XML 파일에서 시스템 UI 요소의 반투명도를 변경할 수 있습니다. KitKat에는 다음 속성이 도입되었습니다.

- `windowTranslucentStatus` - true로 설정하면 맨 위의 상태 표시줄을 반투명하게 만듭니다.

- `windowTranslucentNavigation` - true로 설정하면 맨 아래 탐색 모음을 반투명하게 만듭니다.

- `fitsSystemWindows` - 맨 위의 상태 표시줄이나 맨 아래 탐색 모음을 반투명으로 설정하면, 기본적으로 투명 UI 요소 아래 콘텐츠가 이동합니다. 이 속성을 `true`로 설정하는 것은 반투명 시스템 UI 요소와 콘텐츠가 겹치지 않도록 하는 간단한 방법입니다.

다음 코드는 반투명 상태와 탐색 모음이 있는 테마를 정의합니다.

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

아래 스크린샷은 반투명 상태와 탐색 모음이 있는 위의 테마를 보여줍니다.

[![반투명 상태와 탐색 모음이 있는 앱 예시 스크린샷](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content" />

## <a name="user-content"></a>사용자 콘텐츠

### <a name="storage-access-framework"></a>스토리지 액세스 프레임워크

SAF(스토리지 액세스 프레임워크)는 사용자가 이미지, 비디오 및 문서와 같은 저장된 콘텐츠와 상호 작용하는 새로운 방법입니다. KitKat은 콘텐츠를 처리할 애플리케이션을 선택하는 대화 상자를 사용자에게 제공하는 대신, 사용자가 집계 위치한 곳에서 데이터에 액세스할 수 있는 새로운 UI를 엽니다. 콘텐츠를 선택하면 사용자는 콘텐츠를 요청한 애플리케이션으로 돌아가고 앱 환경은 정상적으로 계속됩니다

이렇게 변경하려면 개발자 쪽에서 두 가지 작업이 필요합니다. 첫째, 공급자의 콘텐츠가 필요한 앱을 콘텐츠를 요청하는 새로운 방식으로 업데이트해야 합니다. 둘째 `ContentProvider`에 데이터를 쓰는 애플리케이션이 새 프레임워크를 사용하도록 수정해야 합니다. 두 시나리오 모두 새로운 [`DocumentsProvider`](xref:Android.Provider.DocumentsProvider)에 의존합니다.
API.

#### <a name="documentsprovider"></a>DocumentsProvider

KitKat에서 `ContentProviders`와의 상호 작용은 `DocumentsProvider` 클래스로 추상화됩니다. 즉, `DocumentsProvider`를 통해 데이터에 액세스할 수만 있으면 SAF는 데이터의 물리적 위치를 신경 쓰지 않습니다. 로컬 공급자, 클라우드 서비스 및 외부 스토리지는 모두 동일한 인터페이스를 사용하며, 동일한 방식으로 처리되기 때문에 사용자와 개발자에게 사용자 콘텐츠와 상호 작용할 수 있는 한 곳을 제공합니다.

이 섹션에서는 스토리지 액세스 프레임워크를 사용하여 콘텐츠를 로드하고 저장하는 방법을 설명합니다.

#### <a name="request-content-from-a-provider"></a>공급자의 콘텐츠 요청

`ActionOpenDocument` Intent와 함께 SAF UI를 사용하여 콘텐츠를 선택하려고 한다는 것을 KitKat에 알릴 수 있으며, 이것은 디바이스에서 사용 가능한 모든 콘텐츠 공급자에 연결하려고 한다는 것을 의미합니다. `CategoryOpenable`을 지정하여 Intent에 몇 가지 필터링을 추가할 수 있으며, 이것은 열 수 있는 콘텐츠(즉, 액세스 가능하고 사용 가능한 콘텐츠)만 반환된다는 의미입니다. KitKat을 사용하면 컨텐츠를 `MimeType`으로 필터링할 수도 있습니다. 예를 들어 아래 코드는 이미지 `MimeType`을 지정하여 이미지 결과를 필터링합니다.

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

`StartActivityForResult`를 호출하면 SAF UI가 시작되고 그러면 사용자가 이미지를 찾아서 선택할 수 있습니다.

[![스토리지 액세스 프레임워크를 사용하여 이미지를 찾아보는 앱 예시 스크린샷](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

사용자가 이미지를 선택하면 `OnActivityResult`는 선택한 파일의 `Android.Net.Uri`를 반환합니다. 아래 코드 샘플은 사용자의 이미지 선택을 표시합니다.

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

KitKat을 사용하면 SAF UI에서 콘텐츠를 로드하는 것 외에도 `DocumentProvider` API를 구현하는 `ContentProvider`에 콘텐츠를 저장할 수 있습니다. 콘텐츠를 저장하면 `ActionCreateDocument`와 `Intent`가 사용됩니다.

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

위의 코드 샘플은 사용자가 파일 이름을 변경하고 새 파일을 저장할 디렉터리를 선택할 수 있는 SAF UI를 로드합니다.

[![사용자가 Downloads 디렉터리에서 파일 이름을 NewDoc로 변경하는 스크린샷](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

사용자가 **저장**을 누르면 `data.Data`를 통해 액세스할 수 있는 새로 만든 파일의 `Android.Net.Uri`가 `OnActivityResult`에 전달됩니다. 이 URI는 새 파일에 데이터를 스트리밍하는 데 사용할 수 있습니다.

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

[`ContentResolver.OpenOutputStream(Android.Net.Uri)`](xref:Android.Content.ContentResolver.OpenOutputStream*)은
`System.IO.Stream`을 반환하기 때문에 전체 스트리밍 프로세스를 .NET 형식으로 작성할 수 있습니다.

스토리지 액세스 프레임워크를 사용하여 콘텐츠를 로드하고 만들고 편집하는 방법에 대한 자세한 정보는 [스토리지 액세스 프레임워크에 대한 Android 설명서](https://developer.android.com/guide/topics/providers/document-provider.html)를 참조하세요.

### <a name="printing"></a>인쇄

[인쇄 서비스](xref:Android.PrintServices)와 `PrintManager`가 도입되면서 KitKat에서 콘텐츠 인쇄가 간소화되었습니다. KitKat은 [Google 클라우드 프린트 애플리케이션](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint)을 사용하여 [Google의 클라우드 프린트 서비스 API](https://developers.google.com/cloud-print/)를 완전히 활용하는 첫 번째 API 버전이기도 합니다.
KitKat과 함께 제공되는 대부분의 디바이스는 Wi-Fi에 처음 연결될 때 Google 클라우드 프린트 앱과 [HP 인쇄 서비스 플러그 인](https://play.google.com/store/apps/details?id=com.hp.android.printservice)을 자동으로 다운로드합니다. 사용자는 **설정> 시스템> 인쇄**로 이동하여 디바이스의 인쇄 설정을 확인할 수 있습니다.

[![인쇄 설정 화면 예시 스크린샷](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)

> [!NOTE]
> 인쇄 API가 Google 클라우드 프린트와 작동하도록 기본적으로 설정되어 있더라도, Android 개발자는 새로운 API를 사용하여 인쇄 콘텐츠를 준비하고 다른 애플리케이션으로 보내서 인쇄를 처리할 수 있습니다.

#### <a name="printing-html-content"></a>HTML 콘텐츠 인쇄

KitKat은 `WebView.CreatePrintDocumentAdapter`로 웹 보기가 가능하도록 [`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter)를 자동으로 만듭니다. 웹 콘텐츠 인쇄는 HTML 콘텐츠가 로드될 때까지 기다렸다가 Activity에 알려서 옵션 메뉴에서 인쇄 옵션을 사용 가능하게 만드는 [`WebViewClient`](xref:Android.Webkit.WebViewClient)와, 사용자가 인쇄 옵션을 선택할 때까지 기다렸다가 `PrintManager`에서 `Print`을 호출하는 Activity 사이의 협력 작업입니다. 이 섹션에서는 화면 HTML 콘텐츠를 인쇄하는 데 필요한 기본 설정에 대해 설명합니다.

웹 콘텐츠를 로드하고 인쇄하려면 인터넷 권한이 필요합니다.

[![앱 옵션에서 인터넷 권한 설정](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>인쇄 메뉴 항목

인쇄 옵션은 일반적으로 Activity의 [옵션 메뉴](https://developer.android.com/guide/topics/ui/menus.html#options-menu)에 표시됩니다.
사용자는 옵션 메뉴를 사용하여 Activity에 대한 작업을 수행할 수 있습니다. 화면의 오른쪽 위 모서리에 있으며 다음과 같은 모양입니다.

[![화면의 오른쪽 위 모서리에 표시되는 인쇄 메뉴 항목 예시 스크린샷](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)

추가 메뉴 항목은 *리소스* 아래 *menu* 디렉토리에서 정의할 수 있습니다. 아래 코드는 [Print](xref:Android.Print.PrintManager)라는 샘플 메뉴 항목을 정의합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

Activity에서 옵션 메뉴와의 상호 작용은 `OnCreateOptionsMenu`와 `OnOptionsItemSelected` 메서드를 통해 발생합니다.
`OnCreateOptionsMenu`는 *menu* 리소스 디렉터리에서 인쇄 옵션과 같은 새 메뉴 항목을 추가하는 곳입니다.
`OnOptionsItemSelected`는 메뉴에서 인쇄 옵션을 선택하는 사용자의 명령을 수신하고 인쇄를 시작합니다.

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

위의 코드는 HTML 콘텐츠의 상태를 추적하기 위해 `dataLoaded`라는 변수도 정의합니다. 모든 콘텐츠가 로드되면 `WebViewClient`가 이 변수를 true로 설정하기 때문에 Activity가 알게 되고 옵션 메뉴에 인쇄 메뉴 항목을 추가합니다.

##### <a name="webviewclient"></a>WebViewClient

`WebViewClient`의 작업은 메뉴에 인쇄 옵션이 나타나기 전에 `WebView`의 데이터가 완전히 로드되도록 하는 것이며, `OnPageFinished` 메서드를 통해 수행됩니다. `OnPageFinished`는 웹 콘텐츠가 로딩을 마칠 때까지 대기했다가 Activity에 알려서 `InvalidateOptionsMenu`를 사용하여 옵션 메뉴를 다시 만들도록 지시합니다.

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

`OnPageFinished`는 `dataLoaded` 값도 `true`로 설정하기 때문에 `OnCreateOptionsMenu`가 인쇄 옵션을 사용하여 메뉴를 다시 만들 수 있습니다.

##### <a name="printmanager"></a>PrintManager

다음 코드 예제는 `WebView`의 콘텐츠를 인쇄합니다.

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print`는 인쇄 작업의 이름(이 예에서, "MyWebPage"), 콘텐츠에서 인쇄 문서를 생성하는 [`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter)
그리고 [`PrintAttributes`](xref:Android.Print.PrintAttributes)
(위의 예제에서 `null`)를 인수로 사용합니다. 기본 특성이 대부분의 시나리오를 처리해야 하지만 인쇄된 페이지에 콘텐츠를 배치하는 데 도움이 되도록 `PrintAttributes`를 지정할 수 있습니다.

`Print`를 호출하면 인쇄 UI가 로드되고, 여기에 인쇄 작업에 대한 옵션이 나열됩니다. 이 UI는 아래 스크린샷에 나와 있듯이 사용자에게 HTML 콘텐츠를 인쇄하거나 PDF로 저장하는 옵션을 제공합니다.

[![인쇄 메뉴를 표시하는 PrintHtmlActivity 스크린샷](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![PDF로 저장 메뉴를 표시하는 PrintHtmlActivity 스크린샷](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware" />

## <a name="hardware"></a>하드웨어

KitKat은 새로운 디바이스 기능을 수용하기 위해 몇 가지 API를 추가합니다. 그 중 가장 주목할만한 것은 호스트 기반 카드 에뮬레이션과 새로운 `SensorManager`입니다.

### <a name="host-based-card-emulation-in-nfc"></a>NFC의 호스트 기반 카드 에뮬레이션

HCE(호스트 기반 카드 에뮬레이션)를 사용하면 애플리케이션이 통신사 소유의 보안 요소에 의존하지 않고 NFC 카드 또는 NFC 카드 판독기처럼 작동할 수 있습니다. HCE를 설정하기 전에 `PackageManager.HasSystemFeature`를 사용하여 디바이스에서 HCE를 사용할 수 있는지 확인합니다.

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

HCE를 사용하려면 HCE 기능과 `Nfc` 권한을 모두 애플리케이션의 `AndroidManifest.xml`에 등록해야 합니다.

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![앱 옵션에서 NFC 권한 설정](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

작동하려면 HCE를 백그라운드에서 실행할 수 있어야 하며 HCE를 사용하는 애플리케이션이 실행되고 있지 않아도 사용자가 NFC 트랜잭션을 수행하면 시작되어야 합니다. 이러한 기능은 HCE 코드를 `Service`로 작성하여 구현할 수 있습니다. HCE 서비스는 다음과 같은 메서드를 구현하는 `HostApduService` 인터페이스를 구현합니다.

- *ProcessCommandApdu* - APDU(Application Protocol Data Unit)는 NFC 판독기와 HCE 서비스 사이에 전송되는 사항입니다. 이 메서드는 판독기에서 ADPU를 사용하고 이에 대한 응답으로 데이터 단위를 반환합니다.

- *OnDeactivated* - HCE 서비스가 NFC 판독기와 더 이상 통신하지 않으면 `HostAdpuService`가 비활성화됩니다.

HCE 서비스도 애플리케이션의 매니페스트에 등록하고 적절한 권한, Intent 필터 및 메타데이터로 데코레이트해야 합니다. 다음 코드는 `Service` 특성을 사용하여 Android Manifest에 등록된 `HostApduService`의 예입니다. (특성에 대한 자세한 내용은 Xamarin [Android Manifest 작업](~/android/platform/android-manifest.md) 가이드를 참조하세요.)

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

위의 서비스는 NFC 판독기가 애플리케이션과 상호 작용할 수 있는 방법을 제공하지만, 스캔이 필요한 NFC 카드를 이 서비스가 에뮬레이트하고 있는지를 NFC 판독기가 알 수 있는 방법이 아직 없습니다. NFC 판독기가 서비스를 식별할 수 있도록 서비스에 고유한 *AID(Application ID)* 를 할당할 수 있습니다. `MetaData` 특성에 등록된 xml 리소스 파일에 HCE 서비스에 대한 다른 메타데이터와 함께 AID를 지정합니다(위의 코드 예제 참조). 이 리소스 파일은 하나 이상의 AID 필터(하나 이상의 NFC 판독기 디바이스의 AID에 해당하는 16진수 형식의 고유 식별자 문자열)를 지정합니다.

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

AID 필터 외에도, xml 리소스 파일은 HCE 서비스에 대한 사용자 관련 설명을 제공하고, AID 그룹(지불 애플리케이션 및 "other")을 지정하고, 결제 애플리케이션의 경우 사용자에게 표시할 260x96dp 배너를 지정합니다.

위에 설명된 설정은 NFC 카드를 에뮬레이트하는 애플리케이션의 기본 구성 요소를 제공합니다. NFC 자체에는 구성을 위한 몇 가지 단계와 테스트가 더 필요합니다. 호스트 기반 카드 에뮬레이션에 대한 자세한 내용은 [Android Documentation 포털](https://developer.android.com/guide/topics/connectivity/nfc/hce.html)을 참조하세요.
Xamarin과 함께 NFC를 사용하는 방법에 대한 자세한 내용은 [Xamarin NFC 샘플](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample)을 참조하세요.

### <a name="sensors"></a>센서

KitKat은 [`SensorManager`](xref:Android.Hardware.SensorManager)를 통해 디바이스의 센서에 대한 액세스를 제공합니다.
`SensorManager`를 사용하면 OS가 센서 정보를 애플리케이션에 일괄 전달하도록 예약하여 배터리 수명을 보존할 수 있습니다.

KitKat에는 사용자의 단계를 추적하기 위한 두 가지 새로운 센서 유형이 제공되며, 가속도계를 기반으로 하고 다음을 포함합니다.

- *StepDetector* - 사용자가 단계를 수행하면 앱에 알림이 전송되고/절전 모드가 해제되고 탐지기는 단계가 발생한 시간 값을 제공합니다.

- *StepCounter* - 다음 디바이스 재부팅 때까지, 센서가 등록된 이후에 사용자가 수행한 단계의 카운트를 추적합니다. 

아래 스크린샷은 작동 중인 단계 카운터를 보여줍니다.

[![단계 카운터가 표시된 SensorsActivity 앱 스크린샷](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

`GetSystemService(SensorService)`를 호출하고 결과를 `SensorManager`로 캐스팅하여 `SensorManager`를 만들 수 있습니다. 단계 카운터를 사용하려면 `SensorManager`에서 `GetDefaultSensor`를 호출합니다. [`ISensorEventListener`](xref:Android.Hardware.ISensorEventListener)
인터페이스를 사용하여 센서를 등록하고 단계 카운터의 변화를 수신 대기할 수 있습니다. 아래 코드 샘플을 참조하세요.

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

애플리케이션이 포그라운드에 있는 동안 단계 카운트가 업데이트되면 `OnSensorChanged`가 호출됩니다. 애플리케이션 백그라운드로 전환되거나 디바이스가 절전 모드이면 `OnSensorChanged`가 호출되지 않지만, `UnregisterListener`가 호출될 때까지 단계는 계속 계산됩니다.

단계 카운드 값은 센서를 등록하는 모든 애플리케이션에서 누적됩니다.  즉, 애플리케이션을 제거했다가 다시 설치하고 애플리케이션 시작 시 `count` 변수를 0으로 초기화하더라도, 센서가 보고한 값은 센서가 등록되어 있는 동안 사용한 애플리케이션(또는 다른 애플리케이션)을 통해 수행된 총 단계 수로 유지됩니다. 아래 코드와 같이 `SensorManager`에서 `UnregisterListener`를 호출하면 애플리케이션이 단계 카운터에 추가되지 않도록 할 수 있습니다.

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

디바이스를 재부팅하면 단계 카운트가 0으로 재설정됩니다. 센서를 사용하는 다른 애플리케이션이나 디바이스 상태에 관계없이 애플리케이션의 정확한 카운트를 보고하려면 앱에 추가 코드가 필요합니다.

> [!NOTE]
> 단계 감지 및 계산을 위한 API가 KitKat에 함께 제공되지만 일부 전화에는 센서가 장착되어 있지 않습니다. `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`를 실행하여 센서를 사용할 수 있는지 확인하거나, `GetDefaultSensor`의 반환 값이 `null`이 아닌지 확인할 수 있습니다.

<a name="developer_tools" />

## <a name="developer-tools"></a>개발자 도구

### <a name="screen-recording"></a>화면 녹화

KitKat에는 새로운 화면 녹화 기능이 포함되어 있기 때문에 개발자가 작동 중인 애플리케이션을 녹화할 수 있습니다. 화면 녹화는 Android SDK의 일부로 다운로드할 수 있는 [ADB(Android Debug Bridge)](https://developer.android.com/tools/help/adb.html) 클라이언트를 통해 가능합니다.

화면을 녹화하려면 디바이스를 연결한 다음, Android SDK 설치를 찾고 **platform-tools** 디렉터리로 이동하여 **adb** 클라이언트를 실행합니다.

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

위 명령은 기본 해상도 4Mbps로 기본 3분 비디오를 녹화합니다. 길이를 편집하려면 *--time-limit* 플래그를 추가합니다.
해상도를 변경하려면 *--bit-rate* 플래그를 추가합니다. 다음 명령은 8Mbps로 1분 길이의 비디오를 녹화합니다.

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

디바이스에서 비디오를 찾을 수 있습니다. 녹화가 완료되면 갤러리에 나타납니다.

## <a name="other-kitkat-additions"></a>기타 KitKat 추가

위에서 설명한 변경 사항 외에도 KitKat을 사용하여 다음을 수행할 수 있습니다.

- 전체 화면 사용  - 콘텐츠를 탐색하고 게임을 즐기는 등 전체 화면 환경의 이점을 활용할 수 있는 기타 애플리케이션을 실행할 수 있도록 새로운 [몰입형 모드](https://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int))가 KitKat에 도입되었습니다.

- 알림 사용자 지정  - [`NotificationListenerService`](xref:Android.Service.Notification.NotificationListenerService)를 사용하여 시스템 알림에 대한 추가 정보를 가져옵니다
  을 선택합니다. 이렇게 하면 앱 내부에서 다른 방식으로 정보를 제공할 수 있습니다.

- 미러 드로어블 리소스  - 드로어블 리소스에는 이미지의 미러링(왼쪽에서 오른쪽 레이아웃으로 대칭 이동)된 버전을 생성하도록 시스템에 지시하는 새로운 [`autoMirrored`](https://developer.android.com/reference/android/R.attr.html#autoMirrored)
  특성이 있습니다.

- 애니메이션 일시 중지  - [`Animator`](xref:Android.Animation.Animator)
  클래스로 만든 애니메이션을 일시 중지했다가 다시 시작합니다.

- 동적으로 변하는 텍스트 읽기  - 새로운 [`accessibilityLiveRegion`](https://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion) 특성을 사용하여 새 텍스트를 "라이브 영역"이라고 동적으로 업데이트하여
  접근성 모드에서 새 텍스트를 자동으로 읽는 UI 부분을 나타냅니다.

- 오디오 환경 개선  - [`LoudnessEnhancer`](xref:Android.Media.Audiofx.LoudnessEnhancer)를 사용하여 트랙의 소리를 키우고,
  [`Visualizer`](xref:Android.Media.Audiofx.Visualizer.MeasurementModePeakRms) 클래스를 사용하여 오디오 스트림의 RMS 및 피크를 찾고,
  오디오-비디오 동기화를 지원할 수 있도록 [오디오 타임스탬프](xref:Android.Media.AudioTimestamp)에서 정보를 가져옵니다.

- 사용자 지정 간격으로 ContentResolver 동기화  - KitKat은 동기화 요청이 수행되는 시간에 약간의 변동성을 추가합니다. `ContentResolver.RequestSync`를 호출하고 `SyncRequest`를 전달하여 사용자 지정 시간 또는 간격으로 `ContentResolver`를 동기화합니다.

- 컨트롤러 구별  - KitKat에서는 디바이스의 `ControllerNumber` 속성을 통해 액세스할 수 있는 고유한 정수 식별자가 컨트롤러에 할당됩니다. 이렇게 하면 게임에서 플레이어를 쉽게 구별할 수 있습니다.

- *원격 제어* - 하드웨어와 소프트웨어 측면에서 약간 변경된 KitKat을 사용하면 `ConsumerIrService`를 사용하여 IR 송신기가 장착된 디바이스를 리모컨으로 전환하고 새로운 [`RemoteController`](xref:Android.Media.RemoteController)
  API 액세스를 간소화합니다.

위의 API 변경에 대한 자세한 내용은 Google [Android 4.4 API](https://developer.android.com/about/versions/android-4.4.html) 개요를 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 Android 4.4(API 수준 19)에서 사용할 수 있는 몇 가지 새로운 API를 소개하고 애플리케이션을 KitKat으로 전환하는 모범 사례를 설명했습니다. *테마 지정*에 대한 새로운 옵션과 *전환 프레임워크*를 비롯하여 사용자 경험에 영향을 주는 API의 변경 사항에 대해서도 설명했습니다. 다음으로 *스토리지 액세스 프레임워크* 및 `DocumentsProvider` 클래스와 새로운 *인쇄 API*를 소개했습니다. *NFC 호스트 기반 카드 에뮬레이션* 및 사용자의 단계를 추적하는 두 가지 새로운 센서를 비롯한 *저전력 센서*를 사용하는 방법도 살펴보았습니다. 마지막으로, *화면 녹화*를 통해 애플리케이션의 실시간 데모를 캡처하는 방법을 보여주고 KitKat API 변경 및 추가에 대한 자세한 목록을 제공했습니다.

## <a name="related-links"></a>관련 링크

- [KitKat 샘플](https://docs.microsoft.com/samples/xamarin/monodroid-samples/kitkat)
- [Android 4.4 API](https://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](https://developer.android.com/about/versions/kitkat.html)
