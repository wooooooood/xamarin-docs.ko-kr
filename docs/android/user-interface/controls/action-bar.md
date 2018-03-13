---
title: ActionBar
ms.topic: article
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 64a5ac7e0c448205da66f9790a506ca34a944140
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="actionbar"></a>ActionBar


## <a name="overview"></a>개요

사용 하는 경우 `TabActivity`, 탭 아이콘을 만들려면 코드에는 Android 4.0 프레임 워크에 대해 실행 하면 영향을 주지 않습니다. Android 2.3, 이전 버전에서 작동 기능 하지만 `TabActivity` 클래스 자체는 4.0에서 사용 되지 합니다. 그런 다음 아래에서는 작업 모음을 사용 하는 탭된 인터페이스를 만들 수 있는 새로운 방법 도입 되었습니다.


## <a name="action-bar-tabs"></a>작업 표시줄 탭

작업 표시줄 탭된 인터페이스를 Android 4.0에 추가 하는 것에 대 한 지원이 포함 되어 있습니다.
다음 스크린 샷에서 이러한 인터페이스의 예를 보여 줍니다.

[![에뮬레이터;에서 실행 중인 앱의 스크린 샷 두 개의 탭이 표시 됩니다.](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

작업 표시줄에 탭을 만들려면 원하므로 먼저 설정 해야 해당 `NavigationMode` 탭을 지 원하는 속성을 합니다. Android 4에서는 `ActionBar` 속성을 설정 하는 데 사용할 수 있는 म 활동 클래스에 사용할 수는 `NavigationMode` 다음과 같이 합니다.

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

이 작업이 완료 되 면 호출 하 여 탭을 만들 수 있습니다는 `NewTab` 작업 모음에서 메서드. 이 탭 인스턴스와 호출할 수 있습니다는 `SetText` 및 `SetIcon` 탭의 레이블에 텍스트 및 아이콘;를 설정 하는 메서드를 이러한 호출은 다음 코드에 순서 대로 수행 됩니다.

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

그러나 탭을 추가할 수 있습니다, 전에 처리 해야는 `TabSelected` 이벤트입니다. 이 처리기에서 탭에 대 한 콘텐츠를 만들 수 있습니다. 작업 모음 탭은 함께 사용 하도록 *조각*은 활동에서 사용자 인터페이스의 일부분을 나타내는 클래스입니다. 예를 들어 조각에서 보기 포함 하는 하나의 `TextView`에서 확장할 우리는 우리의 `Fragment` 다음과 같은 하위 클래스:

```csharp
class SampleTabFragment: Fragment
{           
    public override View OnCreateView (LayoutInflater inflater,
        ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreateView (inflater, container, savedInstanceState);
       
        var view = inflater.Inflate (
            Resource.Layout.Tab, container, false);

        var sampleTextView =
            view.FindViewById<TextView> (Resource.Id.sampleTextView);            
        sampleTextView.Text = "sample fragment text";


        return view;
    }
}
```

에 전달 되는 이벤트 인수는 `TabSelected` 형식의 이벤트는 `TabEventArgs`를 포함 하는 `FragmentTransaction` 다음과 같이 조각에서 추가를 사용할 수 있는 속성:

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

마지막으로 추가할 수 있습니다 탭 작업 모음을 호출 하 여는 `AddTab` 이 코드에 표시 된 메서드:

```csharp
this.ActionBar.AddTab (tab);
```

전체 예제를 참조 하십시오.는 *HelloTabsICS* 이 문서에 대 한 샘플 코드에 대 한 프로젝트입니다.


## <a name="shareactionprovider"></a>ShareActionProvider

`ShareActionProvider` 클래스를 사용 하면 작업 모음에서 수행할 동작을 공유 합니다. 공유 의도 처리할 수 있는 작업 모음에서 나중에 쉽게 액세스할 수 이전에 사용 되는 응용 프로그램 기록을를 유지 하는 응용 프로그램 목록은 사용 하 여 작업 보기를 만드는 것이 담당 합니다. 따라서 응용 프로그램이 Android 전체에서 일치 하는 사용자 경험을 통해 데이터를 공유할 수 있습니다.


### <a name="image-sharing-example"></a>이미지 공유 예제

예를 들어 다음은 이미지를 공유 하는 작업 모음 메뉴 항목과 스크린샷입니다 (에서 가져온는 [ShareActionProvider](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/) 샘플). ShareActionProvider 연관 된 의도 처리 하도록 응용 프로그램을 로드할 사용자 작업 모음에서 메뉴 항목을 누르면는 `ShareActionProvider`합니다. 이 예제에서는 메시징 응용 프로그램에 이전에 사용 되었으므로, 작업 모음에 표시 됩니다.

[![메시징 작업 모음에서 응용 프로그램 아이콘의 스크린 샷](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)


작업 모음에서 항목에 대해 사용자가 아래와 같이 공유 이미지가 포함 된 메시징 앱 시작 됩니다.

[![원숭이 이미지를 표시 하는 메시징 응용 프로그램의 스크린 샷](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)


### <a name="specifying-the-action-provider-class"></a>작업 공급자 클래스를 지정합니다.

사용 하는 `ShareActionProvider`로 설정 된 `android:actionProviderClass` 특성을 다음과 같이 작업 표시줄의 메뉴에 대 한 XML에는 메뉴 항목에서:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/shareMenuItem"
      android:showAsAction="always"
      android:title="@string/sharePicture"
      android:actionProviderClass="android.widget.ShareActionProvider" />
</menu>
```


### <a name="inflating-the-menu"></a>메뉴 않아서

재정의을 메뉴를 확장할 `OnCreateOptionsMenu` 는 활동 하위 클래스입니다. 메뉴에 대 한 참조, 상태를 가져올 수 있습니다는 `ShareActionProvider` 에서 `ActionProvider` 의 속성 메뉴 항목 및 다음 사용 하 여 설정 하려면 SetShareIntent 메서드는 `ShareActionProvider`의 의도 아래와 같이:

```csharp
public override bool OnCreateOptionsMenu (IMenu menu)
{
    MenuInflater.Inflate (Resource.Menu.ActionBarMenu, menu);       
           
    var shareMenuItem = menu.FindItem (Resource.Id.shareMenuItem);           
    var shareActionProvider =
       (ShareActionProvider)shareMenuItem.ActionProvider;
    shareActionProvider.SetShareIntent (CreateIntent ());
}
```


### <a name="creating-the-intent"></a>의도 만들기

`ShareActionProvider` ´ ֲ에 전달 된 의도 `SetShareIntent` 적합 한 작업을 시작 하려면 위 코드에서 메서드. 이 경우 다음 코드를 사용 하 여 이미지를 보내도록 의도 만듭니다.

```csharp
Intent CreateIntent ()
{  
    var sendPictureIntent = new Intent (Intent.ActionSend);
    sendPictureIntent.SetType ("image/*");
    var uri = Android.Net.Uri.FromFile (GetFileStreamPath ("monkey.png"));          
    sendPictureIntent.PutExtra (Intent.ExtraStream, uri);
    return sendPictureIntent;
}
```

위의 코드 예제에서 응용 프로그램과 함께 자산으로 포함 된, 이미지 메시징 응용 프로그램 같은 다른 응용 프로그램에 액세스할 수 있도록는 활동이 생성 될 때 공개적으로 액세스할 수 있는 위치에 복사 합니다. 이 문서와 함께 제공 되는 샘플 코드는이 예제에서는 사용을 보여주는 전체 소스를 포함 합니다.



## <a name="related-links"></a>관련 링크

- [Hello 탭 ICS (샘플)](https://developer.xamarin.com/samples/HelloTabsICS/)
- [ShareActionProvider 데모 (샘플)](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/)
- [아이스크림 샌드위치 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
