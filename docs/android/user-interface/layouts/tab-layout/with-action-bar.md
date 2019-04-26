---
title: 작업 모음이 있는 탭된 레이아웃
description: 이 가이드 소개 하 고 작업 모음이 Api를 사용 하 여 Xamarin.Android 응용 프로그램에서 탭된 사용자 인터페이스를 만드는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 6ce8099aa4230a11a12f4fe8aeffe850f9ef2ce9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61304511"
---
# <a name="tabbed-layouts-with-the-actionbar"></a>작업 모음이 있는 탭된 레이아웃

_이 가이드 소개 하 고 작업 모음이 Api를 사용 하 여 Xamarin.Android 응용 프로그램에서 탭된 사용자 인터페이스를 만드는 방법에 설명 합니다._


## <a name="overview"></a>개요

작업 표시줄은 탭, 응용 프로그램 id, 메뉴 및 검색과 같은 주요 기능에 대 한 일관 된 사용자 인터페이스를 제공 하는 데 사용 되는 Android UI 패턴입니다. Android 3.0 (API 레벨 11), Google Android 플랫폼에 작업 모음이 Api를 도입 했습니다. 작업 모음이 Api는 일관 된 모양과 느낌 및 탭된 사용자 인터페이스에 대 한 허용 하는 클래스를 제공 하는 UI 테마를 소개 합니다. 이 가이드에서는 Xamarin.Android 응용 프로그램에 작업 표시줄 탭을 추가 하는 방법을 설명 합니다. 또한 Android 2.1 Android 2.3을 대상으로 하는 Xamarin.Android 응용 프로그램을 백 포팅 작업 모음이 탭 Android 지원 라이브러리 v7을 사용 하는 방법을 설명 합니다. 

사실은 `Toolbar` 대신 사용 해야 하는 새롭고 보다 일반화 된 작업 표시줄 구성 요소 `ActionBar` (`Toolbar` 대체 하도록 고안 된 `ActionBar`). 자세한 내용은 [도구 모음](~/android/user-interface/controls/tool-bar/index.md)합니다. 



## <a name="requirements"></a>요구 사항

모든 Xamarin.Android 응용 프로그램입니다 11 (Android 3.0) API 레벨을 대상으로 하거나 더 높은 네이티브 Android Api의 일부로 작업 모음이 Api에 액세스할 수 있습니다. 

API 수준 (Android 2.1) 7로 다시 이식 되었으며 일부 작업 모음이 Api 및을 통해 사용할 수는 [V7 AppCompat 라이브러리](https://developer.android.com/tools/support-library/features.html#v7-appcompat)를 통해 Xamarin.Android 앱에 사용할 수 있는 [Xamarin Android 지원 라이브러리-V7 ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) 패키지 있습니다.



## <a name="introducing-tabs-in-the-actionbar"></a>작업 모음이 소개 탭

작업 표시줄 탭 모두 동시에 표시 하 고 크기가 가장 넓은 탭 레이블의 너비를 기준으로 모든 탭을 확인 하려고 합니다. 다음 스크린샷에 설명 되어 있습니다. 

![표시 된 모든 크기가 같은 탭을 사용 하 여 작업 모음이의 예제 스크린샷](with-action-bar-images/image1.png)

작업 모음이 없습니다 모든 탭을 표시 하는 경우 가로로 스크롤할 수 있는 보기를 탭 위로 설정 됩니다. 사용자 왼쪽 또는 오른쪽 나머지 탭을 보려면 살짝 밉니다 수 있습니다. Google Play에서이 스크린샷에서이 예를 보여줍니다. 

![가로 방향으로 스크롤할 수 있는 보기에는 탭의 스크린샷 예제](with-action-bar-images/image2.png)

작업 모음에서 각 탭은 연결 된 [ *조각*](~/android/platform/fragments/index.md)합니다. 사용자가 탭을 선택 하면 응용 프로그램 탭을 사용 하 여 연결 된 조각이 표시 됩니다. 작업 모음이 담당 하지 사용자에 게 적절 한 조각을 표시 합니다. 대신, 작업 모음이 ActionBar.ITabListener 인터페이스를 구현 하는 클래스를 통해 탭에서 상태 변경에 대 한 응용 프로그램을 알립니다. 이 인터페이스는 탭의 상태가 변경 되 면 Android 호출 하는 세 가지 콜백 메서드를 제공 합니다. 

-  **OnTabSelected** -이 메서드는 사용자가 탭을 선택 합니다. 조각이 표시 됩니다.

-  **OnTabReselected** -이 메서드는 탭 이미 선택 되어 있지만 사용자가 다시 선택 합니다. 이 콜백은 일반적으로 표시 된 조각에 대 한 새로 고침/업데이트가 됩니다.

-  **OnTabUnselected** -이 메서드는 사용자가 다른 탭을 선택 합니다. 이 콜백은 사라지기 전에 표시 된 조각의 상태를 저장 하는 데 사용 됩니다.

Xamarin.Android를 래핑하는 `ActionBar.ITabListener` 이벤트를 사용 하 여는 `ActionBar.Tab` 클래스입니다. 응용 프로그램은 이러한 이벤트 중 하나 이상의 이벤트 처리기를 할당할 수 있습니다. 세 가지 이벤트가 (의 각 메서드에 하나 `ActionBar.ITabListener`)을 작업 표시줄 탭을 발생 시키는: 

-  TabSelected
-  TabReselected
-  TabUnselected



### <a name="adding-tabs-to-the-actionbar"></a>작업 모음이에 탭 추가

작업 모음이 네이티브 Android 3.0 (API 레벨 11) 이상 이며 최소한이 API를 대상으로 하는 모든 Xamarin.Android 응용 프로그램에 사용할 수 있습니다. 

다음 단계를 작업 모음이 탭 Android 활동을 추가 하는 방법을 보여 줍니다. 

1. 에 `OnCreate` 활동의 메서드 &ndash; *UI 위젯을 초기화 하기 전에* &ndash; 응용 프로그램 설정 해야 합니다는 `NavigationMode` 에 `ActionBar` 에 `ActionBar.NavigationModeTabs` 이 코드에 표시 된 대로 조각:

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. 사용 하 여 새 탭 `ActionBar.NewTab()`합니다.

3. 이벤트 처리기를 할당 하거나 사용자 지정 제공 `ActionBar.ITabListener` 작업 모음이 탭와 상호 작용할 때 발생 하는 이벤트에 응답 하는 구현 합니다.

4. 이전 단계에서 만든 탭을 추가 합니다 `ActionBar`합니다.


다음 코드는 이벤트 처리기를 사용 하 여 상태 변경에 대응 하는 응용 프로그램 탭을 추가 하려면 다음이 단계를 사용 하는 한 가지 예: 

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


#### <a name="event-handlers-vs-actionbaritablistener"></a>이벤트 처리기 vs ActionBar.ITabListener

응용 프로그램에서 이벤트 처리기를 사용 해야 하 고 `ActionBar.ITabListener` 다양 한 시나리오에 대 한 합니다. 이벤트 처리기는 일정량의 구문상의 편의일; 제공 클래스를 작성 하 고 구현 하는 것과 절약 됩니다 `ActionBar.ITabListener`합니다. 이 편의 않습니다 대가가 &ndash; Xamarin.Android를 하나의 클래스를 만들기 및 구현에 대 한이 변환을 수행 `ActionBar.ITabListener` 있습니다. 이 응용 프로그램에 제한 된 수의 탭 해도 괜찮습니다. 

여러 탭을 사용 하 여 처리할 때 작업 모음이 탭 간에 공통 기능을 공유 될 수도 있습니다 메모리 및 구현 하는 사용자 지정 클래스를 만들려면 성능 측면에서 효율적 `ActionBar.ITabListener`, 및 클래스의 단일 인스턴스를 공유 합니다. 이렇게 하면 GREF의 Xamarin.Android 응용 프로그램을 사용 하는 수가 줄어듭니다. 



### <a name="backwards-compatibility-for-older-devices"></a>이전 버전과 이전 버전의 장치에 대 한 호환성

합니다 [Android 지원 라이브러리 v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) 백 포트 작업 모음이 탭 Android 2.1 (API 수준 7). 이 구성 요소 프로젝트에 추가 되 면 탭 Xamarin.Android 응용 프로그램에서 액세스할 수 있습니다.

작업 모음이 사용 하려면 작업 해야 하위 클래스입니다 `ActionBarActivity` 하 고 다음 코드 조각과에서 같이 AppCompat 테마를 사용 합니다.

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

활동에서 해당 작업 모음이에 대 한 참조를 얻을 수는 `ActionBarActivity.SupportingActionBar` 속성입니다. 다음 코드 조각에서는 작업에서 작업 모음이 설정의 예를 보여 줍니다.

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

이 가이드의 작업 모음이 사용 하 여 Xamarin.Android에서 탭된 사용자 인터페이스를 만드는 방법에 설명 했습니다. 작업 모음이에 탭을 추가 하는 방법 및 작업을 통해 탭 이벤트를 사용 하 여 조작할 수 있는 방법을 설명 하는 것은 `ActionBar.ITabListener` 인터페이스입니다. 또한 Android 지원 라이브러리 v7 AppCompat 패키지 backports 작업 모음이 이전 버전의 Android 탭 하는 방법에 대해 살펴보았습니다. 


## <a name="related-links"></a>관련 링크

- [ActionBarTabs (샘플)](https://developer.xamarin.com/samples/monodroid/UserInterface/ActionBarTabs/)
- [도구 모음](~/android/user-interface/controls/tool-bar/index.md)
- [조각](~/android/platform/fragments/index.md)
- [ActionBar](https://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](https://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [작업 표시줄 패턴](https://developer.android.com/design/patterns/actionbar.html)
- [Android v7 AppCompat](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Xamarin.Android 지원 라이브러리 v7 AppCompat NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
