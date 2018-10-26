---
title: 작업 모음
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 5bb2349f141629478eb2dce995c7cbff5a954069
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114615"
---
# <a name="actionbar"></a>작업 모음


## <a name="overview"></a>개요

사용 하는 경우 `TabActivity`에 탭 아이콘을 만드는 코드에는 Android 4.0 프레임 워크에 대해 실행 하면 영향을 주지 않습니다. 기능적으로 android 2.3의 이전 버전에서 수행한 것 처럼 작동 하지만 `TabActivity` 4.0에서 클래스 자체를 더 이상 사용 되지 되었습니다. 다음 설명의 동작 모음을 사용 하는 탭된 인터페이스를 만드는 새로운 방식으로 도입 되었습니다.


## <a name="action-bar-tabs"></a>작업 표시줄 탭

작업 모음에는 Android 4.0에서 탭된 인터페이스를 추가 하는 것에 대 한 지원이 포함 됩니다.
다음 스크린샷은 이러한 인터페이스의 예를 보여 줍니다.

[![에뮬레이터에서 실행 중인 앱의 스크린 샷 두 개의 탭이 표시 됩니다.](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

작업 모음에서 탭을 만들려면 먼저 설정 해야 해당 `NavigationMode` 속성 탭을 지원 하도록 합니다. Android 4에는 `ActionBar` 속성을 설정 하는 데 사용 수 활동 클래스에 사용할 수는 `NavigationMode` 같이:

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

호출 하 여 탭을 만들 수 있습니다이 완료 되 면를 `NewTab` 작업 모음에서 메서드. 이 탭 인스턴스를 사용 하 여 호출할 수 있습니다는 `SetText` 및 `SetIcon` 탭의 레이블에 텍스트 및 아이콘을 설정 하는 방법 이러한 호출은 아래 표시 된 코드에 순서 대로 수행 됩니다.

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

그러나 탭을 추가할 수 있습니다, 전에 처리 해야 합니다 `TabSelected` 이벤트입니다. 이 처리기에서 탭의 내용을 만들 수 있습니다. 작업 표시줄 탭 사용 하 여 작동 하도록 설계 되었습니다 *조각*, 활동의 사용자 인터페이스 부분을 나타내는 클래스는 합니다. 예를 들어 조각 뷰에 단일 `TextView`에서 확장 하는 것입니다이 `Fragment` 다음과 같은 하위 클래스입니다.

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

에 전달 되는 이벤트 인수를 `TabSelected` 이벤트 형식입니다 `TabEventArgs`를 포함 하는 `FragmentTransaction` 아래와 같이 조각을 추가 하려면 사용할 수 있는 속성:

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

마지막으로 추가할 수 있습니다 탭의 작업 모음을 호출 하 여는 `AddTab` 이 코드에 표시 된 대로 메서드:

```csharp
this.ActionBar.AddTab (tab);
```

전체 예제를 참조 하세요. 합니다 *HelloTabsICS* 이 문서에 대 한 샘플 코드는 프로젝트입니다.


## <a name="shareactionprovider"></a>ShareActionProvider

`ShareActionProvider` 클래스 작업 모음에서 수행 하는 공유 작업을 사용 하도록 설정 합니다. 공유 의도 처리할 수 있으며 작업 모음에서 나중에 쉽게 액세스할 수 있도록 이전에 사용 되는 응용 프로그램의 기록에 유지 하는 앱 목록을 사용 하 여 작업 보기를 만드는 것을 담당 합니다. 이렇게 하면 응용 프로그램이 Android 전체에서 일관 된 사용자 환경을 통해 데이터를 공유할 수 있습니다.


### <a name="image-sharing-example"></a>예제를 공유 하는 이미지

예를 들어, 다음은 이미지를 공유 하려면 작업 모음 메뉴 항목과 스크린샷입니다 (에서 가져온 합니다 [ShareActionProvider](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/) 샘플). ShareActionProvider 연관 된 의도 처리 하도록 응용 프로그램을 로드할 사용자 작업 모음에서 메뉴 항목을 누르면는 `ShareActionProvider`합니다. 이 예제에서는 메시징 응용 프로그램에 된 이전에 사용 되므로 작업 표시줄에 표시 됩니다.

[![메시징 작업 표시줄에서 응용 프로그램 아이콘의 스크린샷](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)


사용자 작업 모음에서 항목을 클릭할 때 아래와 같이 공유 이미지를 포함 하는 메시징 앱 시작가:

[![Monkey 이미지를 표시 하는 메시징 앱의 스크린 샷](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)


### <a name="specifying-the-action-provider-class"></a>동작 공급자 클래스를 지정합니다.

사용 하는 `ShareActionProvider`설정의 `android:actionProviderClass` 특성의 xml 작업 표시줄의 메뉴에 메뉴 항목을 다음과 같이:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/shareMenuItem"
      android:showAsAction="always"
      android:title="@string/sharePicture"
      android:actionProviderClass="android.widget.ShareActionProvider" />
</menu>
```


### <a name="inflating-the-menu"></a>메뉴 값

메뉴를 확장, 재정의에서는 `OnCreateOptionsMenu` 활동 서브 클래스에서. 가져올 수 있습니다. 메뉴에 대 한 참조 한 후는 `ShareActionProvider` 에서 합니다 `ActionProvider` SetShareIntent 설정할 메서드를 사용 하 여 메뉴 항목의 속성을 `ShareActionProvider`의 의도 아래와 같이:

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

`ShareActionProvider` 전달할 의도 사용 하 여를 `SetShareIntent` 메서드 위의 코드에서 적합 한 작업을 시작 합니다. 이 경우 다음 코드를 사용 하 여 이미지를 전송 하려는 의도 만듭니다.

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

위의 코드 예제의 이미지는 응용 프로그램을 사용 하 여 자산으로 포함 되어 있으며 메시징 앱과 같은 다른 응용 프로그램에 액세스할 수 있도록 작업 만들어지면 공개적으로 액세스할 수 있는 위치에 복사 합니다. 이 문서와 함께 제공 되는 샘플 코드는이 예의 용도 보여 주는 전체 소스를 포함 합니다.



## <a name="related-links"></a>관련 링크

- [Hello 탭 ICS (샘플)](https://developer.xamarin.com/samples/HelloTabsICS/)
- [ShareActionProvider 데모 (샘플)](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/)
- [Ice Cream Sandwich 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
