---
title: "작업 모음으로 탭된 레이아웃"
description: "이 가이드 소개 하 고 Xamarin.Android 응용 프로그램에서 탭된 사용자 인터페이스를 만들 작업 모음 Api를 사용 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: afaa02168dcac54115e8fca53683725926e4baed
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="tabbed-layouts-with-the-actionbar"></a>작업 모음으로 탭된 레이아웃

_이 가이드 소개 하 고 Xamarin.Android 응용 프로그램에서 탭된 사용자 인터페이스를 만들 작업 모음 Api를 사용 하는 방법을 설명 합니다._


## <a name="overview"></a>개요

작업 모음은 탭, 응용 프로그램 id, 메뉴 및 검색 등의 주요 기능에 대 한 일관 된 사용자 인터페이스를 제공 하는 데 사용 하는 Android UI 패턴입니다. Android 3.0 (API 수준 11)에서는 Google Android 플랫폼에 작업 모음 Api를 도입 합니다. 작업 모음 Api 탭된 사용자 인터페이스에 대 한 허용 하는 클래스 및 일관 된 모양과 느낌을 제공 하기 위해 UI 테마를 소개 합니다. 이 가이드에서는 작업 표시줄 탭 Xamarin.Android 응용 프로그램을 추가 하는 방법을 설명 합니다. 또한 지원 라이브러리 Android v7 backport 작업 모음 탭 Android 2.1 Android 2.3을 대상으로 하는 Xamarin.Android 응용 프로그램을 사용 하는 방법을 설명 합니다. 

`Toolbar` 의 대신 사용 해야 하는 새롭고 보다 일반화 작업 모음 구성 요소 `ActionBar` (`Toolbar` 대체 하도록 설계 된 `ActionBar`). 자세한 내용은 참조 [도구 모음](~/android/user-interface/controls/tool-bar/index.md)합니다. 



## <a name="requirements"></a>요구 사항

API 수준 11 (Android 3.0)을 대상으로 나 높을수록 네이티브 Android Api의 일부로 작업 모음 Api에 액세스할 수 있는 모든 Xamarin.Android 응용 프로그램입니다. 

API 수준 (Android 2.1) 7로 다시 이식 되었습니다 및 통해 사용할 수 있는 몇 가지 작업 모음 Api는 [V7 AppCompat 라이브러리](http://developer.android.com/tools/support-library/features.html#v7-appcompat)를 통해 Xamarin.Android 앱을 사용할 수 있는 [Xamarin Android 지원 라이브러리-v 7 ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) 패키지 합니다.



## <a name="introducing-tabs-in-the-actionbar"></a>탭에는 작업 모음 소개

작업 모음은 탭의 모든 작업을 동시에 표시 하 고 있는 모든 탭 크기에 따라 가장 넓은 탭 레이블의 너비를 같게 하려고 합니다. 다음 스크린 샷에서에서이 확인할 수 있습니다. 

![표시 된 모든 크기가 같은 탭이 포함 된 작업 모음의 예제 스크린 샷](with-action-bar-images/image1.png)

작업 모음에서 모든 탭을 표시할 수 없습니다, 수평으로 스크롤 가능한 보기에서 탭을 설정 합니다. 왼쪽 또는 오른쪽 나머지 탭을 보려면 사용자 살짝 수 있습니다. Google Play에서이 스크린샷에서 이러한 예를 보여 줍니다. 

![탭에 수평으로 스크롤 가능한 보기의 예에서는 스크린 샷](with-action-bar-images/image2.png)

각 탭 작업 모음에 연결 해야는 [ *조각*](~/android/platform/fragments/index.md)합니다. 사용자가 탭을 선택 하면 응용 프로그램 탭 연관 된 조각을 표시 됩니다. 작업 모음은 사용자에 게 적절 한 조각 표시 처리 하지 않습니다. 대신,는 작업 모음 ActionBar.ITabListener 인터페이스를 구현 하는 클래스를 통해 탭의 상태 변경에 대 한 응용 프로그램에 게 합니다. 이 인터페이스를 Android 탭의 상태가 변경 될 때 호출할 콜백 메서드를 세 가지를 제공 합니다. 

-  **OnTabSelected** -이 메서드는 사용자가 탭을 선택 합니다. 조각을 표시할지 합니다.

-  **OnTabReselected** -이 메서드는 탭은 이미 선택 되어 있지만 사용자가 다시 선택할 때 호출 됩니다. 이 콜백은 표시 된 조각 새로 고침/업데이트 일반적으로 사용 됩니다.

-  **OnTabUnselected** -이 메서드는 사용자가 다른 탭을 선택 합니다. 이 콜백은 사라지기 전에 표시 된 조각에 상태를 저장 하는 데 사용 됩니다.

Xamarin.Android 래핑하는 `ActionBar.ITabListener` 의 이벤트에 `ActionBar.Tab` 클래스입니다. 응용 프로그램은 이러한 이벤트 중 하나 이상의 이벤트 처리기를 할당할 수 있습니다. 세 개의 이벤트 (에서 각 방법에 대해 하나씩 `ActionBar.ITabListener`) 작업 표시줄 탭을 발생 시키는: 

-  TabSelected
-  TabReselected
-  TabUnselected



### <a name="adding-tabs-to-the-actionbar"></a>작업 모음에 탭 추가

작업 모음 네이티브 Android 3.0 (API 수준 11) 이상 이며 모든 Xamarin.Android 대상 응용 프로그램을 최소한이 API를 사용할 수 있습니다. 

다음 단계에는 Android 활동에 작업 모음 탭을 추가 하는 방법을 보여 줍니다. 

1. 에 `OnCreate` 메서드는 활동의 &ndash; *모든 UI 위젯 초기화 하기 전에* &ndash; 응용 프로그램 설정 해야 합니다는 `NavigationMode` 에 `ActionBar` 를 `ActionBar.NavigationModeTabs` 이 코드에 나와 있는 것 처럼 코드 조각:

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. 사용 하 여 새 탭 만들기 `ActionBar.NewTab()`합니다.

3. 이벤트 처리기를 할당 하거나 사용자 지정 제공 `ActionBar.ITabListener` 구현 하는 작업 모음 탭와 상호 작용할 때 발생 하는 이벤트에 응답 합니다.

4. 이전 단계에서 만든 탭 추가 `ActionBar`합니다.


다음 코드는 다음이 단계를 사용 하 여 이벤트 처리기를 사용 하 여 상태 변경에 응답 하는 응용 프로그램에 탭을 추가 하는 한 가지 예: 

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
                       }
    ActionBar.AddTab(tab);

    tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab2_text));
    tab.SetIcon(Resource.Drawable.tab2_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       }
    ActionBar.AddTab(tab);
}
```


#### <a name="event-handlers-vs-actionbaritablistener"></a>이벤트 처리기 vs ActionBar.ITabListener

응용 프로그램에서 이벤트 처리기를 사용 해야 하 고 `ActionBar.ITabListener` 다양 한 시나리오에 대 한 합니다. 이벤트 처리기 일정 정도 발생 한 구문 편의; 제공 되는 것 클래스를 만들고 구현 하는 것과 절약 됩니다 `ActionBar.ITabListener`합니다. 이 편리 하이 게를 비용 가져옵니까 &ndash; Xamarin.Android 있습니다 하나의 클래스를 만들고 구현에 대 한이 변환을 수행 하며, `ActionBar.ITabListener` 드립니다. 이 응용 프로그램은 제한 된 수의 탭 세밀 하 게 합니다. 

많은 탭을 처리할 때 작업 모음 탭 간에 공통 기능을 공유 하는 것이 메모리 및 구현 하는 사용자 지정 클래스를 만드는 성능 측면에서 더 효율적일 또는 `ActionBar.ITabListener`, 및 클래스의 단일 인스턴스를 공유 합니다. 이렇게 하면 GREF의 Xamarin.Android 응용 프로그램을 사용 하는 수가 줄어듭니다. 



### <a name="backwards-compatibility-for-older-devices"></a>이전 버전과 이전 버전의 장치에 대 한 호환성

[지원 라이브러리 Android v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) 백 포트 작업 모음 탭 Android 2.1 (API 수준 7). 이 구성 요소 프로젝트에 추가 되 면 탭 Xamarin.Android 응용 프로그램에서 액세스할 수 있습니다.

작업 모음을 사용 하려면 활동 해야 하위 클래스 `ActionBarActivity` 다음 코드 조각에 나와 있는 것 처럼 AppCompat 테마를 사용 하 여:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

활동에서 작업의 모음에 대 한 참조를 얻을 수 있습니다는 `ActionBarActivity.SupportingActionBar` 속성입니다. 다음 코드 조각에서는 활동에는 작업 모음을 설정 하는 예를 보여 줍니다.

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

이 가이드에는 작업 모음을 사용 하 여 Xamarin.Android에 탭된 사용자 인터페이스를 만드는 방법에 설명 했습니다. 탭에서 작업 모음을 추가 하는 방법과 활동 탭 이벤트를 통해 상호 작용할 수는 어떻게 다룬는 `ActionBar.ITabListener` 인터페이스입니다. 또한 지원 라이브러리 Android v7 AppCompat 패키지 backports 작업 모음에서 이전 버전의 Android 탭 하는 방법에 대해 살펴보았습니다. 


## <a name="related-links"></a>관련 링크

- [ActionBarTabs (샘플)](https://developer.xamarin.com/samples/monodroid/UserInterface/ActionBarTabs/)
- [도구 모음](~/android/user-interface/controls/tool-bar/index.md)
- [조각](~/android/platform/fragments/index.md)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](http://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [작업 모음 패턴](http://developer.android.com/design/patterns/actionbar.html)
- [Android v7 AppCompat](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Xamarin.Android 지원 라이브러리 v7 AppCompat NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
