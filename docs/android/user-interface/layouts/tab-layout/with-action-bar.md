---
title: ActionBar를 사용 하는 탭 레이아웃
description: 이 가이드에서는 ActionBar Api를 사용 하 여 Xamarin Android 응용 프로그램에서 탭 사용자 인터페이스를 만드는 방법을 소개 하 고 설명 합니다.
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 4673b178512a886e5fdb154c57c8d659276bb392
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522334"
---
# <a name="tabbed-layouts-with-the-actionbar"></a>ActionBar를 사용 하는 탭 레이아웃

_이 가이드에서는 ActionBar Api를 사용 하 여 Xamarin Android 응용 프로그램에서 탭 사용자 인터페이스를 만드는 방법을 소개 하 고 설명 합니다._


## <a name="overview"></a>개요

작업 모음은 탭, 응용 프로그램 id, 메뉴 및 검색과 같은 주요 기능에 대해 일관 된 사용자 인터페이스를 제공 하는 데 사용 되는 Android UI 패턴입니다. Android 3.0 (API 수준 11)에서 Google은 Android 플랫폼에 ActionBar Api를 도입 했습니다. ActionBar Api는 탭 사용자 인터페이스에 사용할 수 있는 일관 된 모양과 느낌 및 클래스를 제공 하는 UI 테마를 도입 합니다. 이 가이드에서는 Xamarin Android 응용 프로그램에 작업 모음 탭을 추가 하는 방법에 대해 설명 합니다. Android 지원 라이브러리 v7를 사용 하 여 android 2.1를 대상으로 하는 Xamarin Android 응용 프로그램에 android 2.3를 사용 하는 방법에 대해서도 설명 합니다. 

는 대신를 사용 해야 하는 보다 최신의 `ActionBar` 일반화 된 작업 모음 구성 요소입니다 (`Toolbar` 를 대체 `ActionBar`하도록 설계 됨). `Toolbar` 자세한 내용은 [도구 모음](~/android/user-interface/controls/tool-bar/index.md)을 참조 하세요. 



## <a name="requirements"></a>요구 사항

API 수준 11 (Android 3.0) 이상을 대상으로 하는 Xamarin Android 응용 프로그램은 네이티브 Android Api의 일부로 ActionBar Api에 액세스할 수 있습니다. 

일부 ActionBar Api는 API 수준 7 (Android 2.1)으로 다시 이식 되었으며 [V7 AppCompat 라이브러리](https://developer.android.com/tools/support-library/features.html#v7-appcompat)를 통해 사용할 수 있습니다 .이는 [Xamarin android 지원 라이브러리-V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) 패키지를 통해 xamarin android 앱에서 사용할 수 있습니다.



## <a name="introducing-tabs-in-the-actionbar"></a>ActionBar 탭 소개

작업 모음은 모든 탭을 동시에 표시 하 고 가장 넓은 탭 레이블의 너비를 기준으로 모든 탭의 크기를 동일 하 게 만듭니다. 이는 다음 스크린샷에 설명 되어 있습니다. 

![모든 동일한 크기의 탭이 표시 된 ActionBar의 예제 스크린샷](with-action-bar-images/image1.png)

ActionBar는 모든 탭을 표시할 수 없는 경우 가로로 스크롤할 수 있는 보기에서 탭을 설정 합니다. 사용자는 왼쪽 또는 오른쪽으로 살짝 밀어 나머지 탭을 볼 수 있습니다. Google Play의이 스크린샷에서는이에 대 한 예를 보여 줍니다. 

![가로로 스크롤 가능한 뷰에서 탭의 예제 스크린샷](with-action-bar-images/image2.png)

작업 모음의 각 탭은 [*조각*](~/android/platform/fragments/index.md)에 연결 되어야 합니다. 사용자가 탭을 선택 하면 응용 프로그램은 탭과 연결 된 조각을 표시 합니다. ActionBar는 사용자에 게 적절 한 조각을 표시 하는 일을 담당 하지 않습니다. 대신 ActionBar는 ActionBar 인터페이스를 구현 하는 클래스를 통해 탭의 상태 변경 내용을 응용 프로그램에 알립니다. 이 인터페이스는 탭의 상태가 변경 될 때 Android에서 호출 하는 세 가지 콜백 메서드를 제공 합니다. 

- **Ontabselected** -이 메서드는 사용자가 탭을 선택할 때 호출 됩니다. 조각이 표시 되어야 합니다.

- **OnTabReselected** -이 메서드는 탭이 이미 선택 되어 있지만 사용자가 다시 선택 하는 경우 호출 됩니다. 이 콜백은 일반적으로 표시 되는 조각을 새로 고치거 나 업데이트 하는 데 사용 됩니다.

- **Ontabunselected 취소** -이 메서드는 사용자가 다른 탭을 선택할 때 호출 됩니다. 이 콜백은 표시 된 조각에 상태를 저장 하는 데 사용 됩니다.

Xamarin.ios는 `ActionBar.Tab` 클래스의 이벤트 `ActionBar.ITabListener` 를 사용 하 여를 래핑합니다. 응용 프로그램은 이러한 이벤트 중 하나 이상에 이벤트 처리기를 할당할 수 있습니다. 작업 모음 탭에서 발생 하는 세 가지 이벤트 `ActionBar.ITabListener`(의 각 메서드에 하나씩)가 있습니다. 

- TabSelected
- TabReselected
- TabUnselected 취소



### <a name="adding-tabs-to-the-actionbar"></a>ActionBar에 탭 추가

ActionBar는 Android 3.0 (API 수준 11) 이상으로 기본 제공 되며이 API를 대상으로 하는 모든 Xamarin Android 응용 프로그램에서 사용할 수 있습니다. 

다음 단계에서는 Android 활동에 ActionBar 탭을 추가 하는 방법을 보여 줍니다. 

1. &ndash; `ActionBar.NavigationModeTabs` `ActionBar` `OnCreate` UI 위젯을&ndash;초기화 하기 전의 활동 메서드에서 응용 프로그램은 다음 코드 조각에 표시 된 대로의를로 설정 해야합니다.`NavigationMode`

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. 을 사용 하 여 `ActionBar.NewTab()`새 탭을 만듭니다.

3. 이벤트 처리기를 할당 하거나 사용자가 `ActionBar.ITabListener` ActionBar 탭과 상호 작용할 때 발생 하는 이벤트에 응답 하는 사용자 지정 구현을 제공 합니다.

4. 이전 단계 `ActionBar`에서 만든 탭을에 추가 합니다.


다음 코드는 이러한 단계를 사용 하 여 상태 변경에 응답 하는 이벤트 처리기를 사용 하는 응용 프로그램에 탭을 추가 하는 한 가지 예입니다. 

```csharp
protected override void OnCreate(Bundle bundle)
{
    ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
    SetContentView(Resource.Layout.Main);

    ActionBar.Tab tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab1_text));
    tab.SetIcon(Resource.Drawable.tab1_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);

    tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab2_text));
    tab.SetIcon(Resource.Drawable.tab2_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);
}
```


#### <a name="event-handlers-vs-actionbaritablistener"></a>이벤트 처리기 vs ActionBar

응용 프로그램은 다양 한 시나리오 `ActionBar.ITabListener` 에서 이벤트 처리기 및를 사용 해야 합니다. 이벤트 처리기는 특정 양의 구문을 편리 하 게 제공 합니다. 클래스를 만들고를 구현 `ActionBar.ITabListener`하지 않아도 됩니다. 이 편의를 위해 Xamarin은 무료로 &ndash; 제공 됩니다. Android는이 변환을 수행 하 여 클래스를 만들고 구현 `ActionBar.ITabListener` 합니다. 응용 프로그램에 제한 된 수의 탭이 있으면 괜찮습니다. 

많은 탭을 처리 하거나 ActionBar 탭 간의 일반적인 기능을 공유 하는 경우,를 구현 `ActionBar.ITabListener`하 고 클래스의 단일 인스턴스를 공유 하는 사용자 지정 클래스를 만드는 것이 메모리와 성능 측면에서 더 효율적일 수 있습니다. 이렇게 하면 Xamarin Android 응용 프로그램에서 사용 하는 GREF의 수가 줄어듭니다. 



### <a name="backwards-compatibility-for-older-devices"></a>이전 장치에 대 한 이전 버전과의 호환성

[Android 지원 라이브러리 V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) 백 ports ActionBar 탭에서 android 2.1 (API 수준 7)로 이동 합니다. 이 구성 요소가 프로젝트에 추가 되 면 Xamarin Android 응용 프로그램에서 탭에 액세스할 수 있습니다.

ActionBar를 사용 하려면 다음 코드 조각과 같이 작업 `ActionBarActivity` 에서 AppCompat 테마를 하위 클래스 하 고 사용 해야 합니다.

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

활동은 `ActionBarActivity.SupportingActionBar` 속성에서 ActionBar에 대 한 참조를 가져올 수 있습니다. 다음 코드 조각은 활동에서 ActionBar을 설정 하는 예를 보여 줍니다.

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity : ActionBarActivity, ActionBar.ITabListener
{
    static readonly string Tag = "ActionBarTabsSupport";

    public void OnTabReselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Optionally refresh/update the displayed tab.
        Log.Debug(Tag, "The tab {0} was re-selected.", tab.Text);
    }

    public void OnTabSelected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Display the fragment the user should see
        Log.Debug(Tag, "The tab {0} has been selected.", tab.Text);
    }

    public void OnTabUnselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Save any state in the displayed fragment.
        Log.Debug(Tag, "The tab {0} as been unselected.", tab.Text);
    }

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SupportActionBar.NavigationMode = ActionBar.NavigationModeTabs;
        SetContentView(Resource.Layout.Main);
    }

    void AddTabToActionBar(int labelResourceId, int iconResourceId)
    {
        ActionBar.Tab tab = SupportActionBar.NewTab()
                                            .SetText(labelResourceId)
                                            .SetIcon(iconResourceId)
                                            .SetTabListener(this);
        SupportActionBar.AddTab(tab);
    }
}
```


## <a name="summary"></a>요약

이 가이드에서는 ActionBar를 사용 하 여 Xamarin Android에서 탭 사용자 인터페이스를 만드는 방법에 대해 설명 했습니다. ActionBar에 탭을 추가 하는 방법과 활동에서 인터페이스를 `ActionBar.ITabListener` 통해 탭 이벤트와 상호 작용 하는 방법에 대해 살펴보았습니다. Android 지원 라이브러리 v7 AppCompat 패키지에서 ActionBar 탭을 이전 버전의 Android로 포트 하는 방법도 살펴보았습니다. 


## <a name="related-links"></a>관련 링크

- [Action바 탭 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-actionbartabs)
- [도구 모음](~/android/user-interface/controls/tool-bar/index.md)
- [조각](~/android/platform/fragments/index.md)
- [ActionBar](https://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](https://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [작업 모음 패턴](https://developer.android.com/design/patterns/actionbar.html)
- [Android v7 AppCompat](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Xamarin Android 지원 라이브러리 v7 AppCompat NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
