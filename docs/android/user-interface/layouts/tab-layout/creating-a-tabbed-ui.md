---
title: "연습-TabHost를 사용 하 여 탭된 UI 만들기"
description: "이 문서에서는 Xamarin.Android TabHost API를 사용 하 여 탭된 UI를 만드는 과정을 안내 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: AD6E2173-974E-477C-940F-0CAB5E53326D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 2dd397e824ce7735be4421c3f258852de3f77ecb
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="walkthrough---creating-a-tabbed-ui-with-tabhost"></a>연습-TabHost를 사용 하 여 탭된 UI 만들기

_이 문서에서는 Xamarin.Android TabHost API를 사용 하 여 탭된 UI를 만드는 과정을 안내 합니다._

> [!NOTE]
> `TabHost` Google에서 사용 되지 않은 오래 된 API입니다. 개발자는를 사용 하 여 탭된 응용 프로그램을 빌드하는 [작업 모음](~/android/user-interface/controls/action-bar.md)합니다. `ActionBar` Android의 모든 버전에서 사용할 수 있습니다. Android (API 수준 11) 3.0에서 처음 도입 하 고 Android 2.2 (API 수준 8) 및 Android 2.3 (API 수준 10)에서 다시 포팅된는 [V7 AppCompat 라이브러리](http://developer.android.com/tools/support-library/features.html#v7-appcompat), Xamarin.Android를 통해 제공 되는 [Xamarin Android 지원 라이브러리-V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) 패키지 합니다.

이 문서에서는 Xamarin.Android를 사용 하 여 탭된 UI를 만드는 과정을 안내는 `TabHost` API입니다. Android의 모든 버전에서 사용할 수 있는 이전 API입니다. 이 예제는 활동에 캡슐화 되 고 각 탭에 대 한 논리와 세 개의 탭이 포함 된 응용 프로그램을 만듭니다.
다음 스크린 샷에서 만들 응용 프로그램의 예제.

![여러 탭이 포함 된 응용 프로그램의 예제 스크린 샷](creating-a-tabbed-ui-images/image02.png)


## <a name="creating-the-application"></a>응용 프로그램 작성

다운로드 하 고 압축을 풀면 [TabHostWalkthrough](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/)합니다.
이 프로젝트는 응용 프로그램에 대 한 시작 지점으로 사용 되 고 일부 이미지를 포함 합니다. 이 프로젝트를 검토 하면 이미 탭 아이콘에 대 한 그릴 수 있는 리소스는 만들었습니다 표시 됩니다.

첫 번째 레이아웃 파일 정보를 업데이트 **Resources/Layout/Main.axml** 호스팅하는 탭 합니다. 편집 **Resources/Layout/Main.axml** 하 고 다음과 같은 XML을 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<TabHost xmlns:android="http://schemas.android.com/apk/res/android"
         android:id="@android:id/tabhost"
         android:layout_width="fill_parent"
         android:layout_height="fill_parent">
    <LinearLayout
            android:orientation="vertical"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent"
            android:padding="5dp">
        <TabWidget
                android:id="@android:id/tabs"
                android:layout_width="fill_parent"
                android:layout_height="wrap_content" />
        <FrameLayout
                android:id="@android:id/tabcontent"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:padding="5dp" />
    </LinearLayout>
</TabHost>
```

다음 스크린 샷에서 Xamarin 디자이너에서 레이아웃을 보여 줍니다.

[![Xamarin 디자이너에서 TabHost 레이아웃의 스크린 샷](creating-a-tabbed-ui-images/image04-sml.png)](creating-a-tabbed-ui-images/image04.png#lightbox)

TabHost 그 안에 두 개의 자식 뷰 있어야:는 `TabWidget` 및 `FrameLayout`합니다. 위치는 `TabWidget` 및 `FrameLayout` 세로로 내부는 `TabHost`, `LinearLayout` 사용 됩니다. 각 탭에 대 한 콘텐츠 흐름 방향은, 비어 있는 FrameLayout은 때문에 `TabHost` 런타임에 각 활동을 자동으로 포함 됩니다. 탭된 사용자 인터페이스에 대 한 레이아웃을 만들 때 따라야 하는 몇 가지 규칙이 있습니다.

-   `TabHost` id는 `@android:id/tabhost`합니다.

-   `TabWidget` id는 `@android:id/tabs`합니다.

-   `FrameLayout` id는 `@android:id/tabcontent`합니다.

-   `TabHost` 상속 하는 관리 하는 모든 활동 필요 `TabActivity`합니다. 따라서 반드시 하위 클래스 `TabActivity` 여기 &ndash; 정기 활동 작동 하지 것입니다.

파일을 편집 **MainActivity.cs** 있도록 클래스 `MainActivity` 하위 클래스 `TabActivity` 다음 코드 조각에 나와 있는 것 처럼:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/ic_launcher")]
public class MainActivity : TabActivity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
    }
}
```

프로젝트에서 별도 네 가지 활동 클래스를 만듭니다: `MyScheduleActivity`, `SessionsActivity`, `SpeakersActivity`, 및 `WhatsOnActivity`합니다. 각 작업 탭의 UI를 구성 하면 있습니다. 이제 이러한 활동을 표시 하는 스텁 됩니다는 `TextView` 간단한 메시지를 사용 합니다. 다음을 포함 하 여 각 활동에서 코드를 편집 `OnCreate` 구현:

```csharp
[Activity]
public class MyScheduleActivity : Activity
{
    protected override void OnCreate (Bundle savedInstanceState)
    {
        base.OnCreate (savedInstanceState);
        TextView textview = new TextView (this);
        textview.Text = "This is the My Schedule tab";
        SetContentView (textview);
    }
}
```

위의 코드에서는 레이아웃 파일을 사용 하지 않는 것을 확인 합니다. 만들기만 `TextView` 일부 텍스트를 포함 하는 집합도 `TextView` 콘텐츠 뷰로 합니다. 각 나머지 세 가지 작업에 대 한이 중복 됩니다.

그런 다음 각 탭에 아이콘을 할당 합니다 했습니다. 각 탭에는 두 개의 아이콘 필요 &ndash; 선택 된 상태 및 선택 되지 않은 상태에 대 한 합니다. 이러한 두 개의 서로 다른 아이콘의 예 (이 응용 프로그램에 대 한 필요한 아이콘 이미 추가한 샘플 프로젝트에) 다음 두 이미지에서 볼 수 있습니다.

![선택 된 상태 및 선택 되지 않은 상태 아이콘의 스크린 샷](creating-a-tabbed-ui-images/tab-icons.png)


아이콘 탭으로 그릴 수 있는 리소스를 정의 하 여 할당 됩니다는 *상태 목록 Drawable*합니다. 상태 목록 drawables는 XML에 정의 되어 있으며 해당 항목의 상태에만 적용 되는 서로 다른 이미지를 지정할 수 있도록 하는 특별 한 그릴 수 있는 리소스입니다. 이 예제는 탭을 선택 하는 경우 사용 되는 하나의 이미지 및 다른 탭을 선택 하지 않으면 사용 되는 합니다. 에 시간을 절약할 필요한 상태 목록 drawables에 추가한 프로젝트입니다. 다음 목록의 파일의 XML이 포함 되어 있습니다.

-   `ic_tab_my_schedule.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

-   `ic_tab_sessions.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_sessions_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_sessions_unselected"/>
    </selector>
    ```

-   `ic_tab_speakers.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_speakers_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_speakers_unselected"/>
    </selector>
    ```

-   `ic_tab_whats_on.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

탭에 추가 되는 `TabHost` 매우 반복적인 태스크는 프로그래밍 방식으로 합니다. 이 해결 하려면 클래스에 다음 메서드를 추가 `MainActivity`:

```csharp
private void CreateTab(Type activityType, string tag, string label, int drawableId )
{
    var intent = new Intent(this, activityType);
    intent.AddFlags(ActivityFlags.NewTask);

    var spec = TabHost.NewTabSpec(tag);
    var drawableIcon = Resources.GetDrawable(drawableId);
    spec.SetIndicator(label, drawableIcon);
    spec.SetContent(intent);

    TabHost.AddTab(spec);
}
```

각 탭에는 `TabHost` 의 인스턴스에서 나타내는의 `TabHost.TabSpec` 클래스입니다. 이 인스턴스는 메타 데이터를 포함 합니다. 특히 탭을 렌더링 하는 데 필요한:

-   **텍스트 및 아이콘** &ndash; 에 표시 되는 `TabWidget`합니다.

-   **콘텐츠 탭** &ndash; 일 수 있습니다이 `Activity` 또는 `View` 에서 탭을 선택 하는 경우 표시 됩니다.

-   **고유 태그** &ndash; 각 탭에 할당 된 고유 태그에 있어야 합니다.

추가 해야는 `TabHost.TabSpec` 응용 프로그램의 각 탭에 대 한 인스턴스. 다음 번에 수행할 수 있습니다.

메서드를 업데이트 `OnCreate` 에 `MainActivity` 다음 코드와 같이:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateTab(typeof(WhatsOnActivity), "whats_on", "What's On", Resource.Drawable.ic_tab_whats_on);
    CreateTab(typeof(SpeakersActivity), "speakers", "Speakers", Resource.Drawable.ic_tab_speakers);
    CreateTab(typeof(SessionsActivity), "sessions", "Sessions", Resource.Drawable.ic_tab_sessions);
    CreateTab(typeof(MyScheduleActivity), "my_schedule", "My Schedule", Resource.Drawable.ic_tab_my_schedule);
}
```

응용 프로그램을 실행합니다. 응용 프로그램은이 연습의 시작 부분에 표시 된 스크린 샷과 비슷해야 합니다.

정말 간단하죠. 응용 프로그램의 다른 부분에 쉽게 이동 하는 사용자를 제공 하는 탭된 응용 프로그램을 작성 했습니다.



## <a name="summary"></a>요약

이 장에서 탭된 레이아웃을 설명 하 고 탭된 응용 프로그램을 만드는 과정을 안내 하 합니다. 이 연습에 사용 하는 방법을 보여 주는 `TabActivity` 확장할 레이아웃 파일 하려면 해당 호스팅는 `TabHost` 및 `TabWidget`합니다. `TabHost` 컬렉션으로 채워진 후 `TabHost.TabSpec` 에서 사용 되는 개체는 `TabHost` 각 탭에서 사용할 수 있도록 하는 활동을 인스턴스화하는 런타임 시.



## <a name="related-links"></a>관련 링크

- [TabHostWalkthrough (샘플)](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android 지원 라이브러리 v7 AppCompat NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 appcompat 라이브러리](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
