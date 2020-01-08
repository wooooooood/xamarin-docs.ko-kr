---
title: Xamarin Android 용 ActionBar
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: f64b57e73b69b3111087ca1352f5fb9536f855e5
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488961"
---
# <a name="actionbar-for-xamarinandroid"></a>Xamarin Android 용 ActionBar

`TabActivity`를 사용 하는 경우 Android 4.0 프레임 워크에 대해 실행 하면 탭 아이콘을 만드는 코드가 영향을 받지 않습니다. 2\.3 이전 버전의 Android에서와 같이 작동 하지만 `TabActivity` 클래스 자체는 4.0에서 더 이상 사용 되지 않습니다. 다음에 설명 하는 작업 모음 사용 하는 탭 인터페이스를 만드는 새로운 방법이 도입 되었습니다.

## <a name="action-bar-tabs"></a>작업 모음 탭

작업 모음에는 Android 4.0에서 탭 인터페이스를 추가 하는 기능이 포함 되어 있습니다.
다음 스크린샷은 이러한 인터페이스의 예를 보여 줍니다.

[에뮬레이터에서 실행 중인 앱의 ![스크린샷 두 개의 탭이 표시 됩니다.](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

작업 모음에서 탭을 만들려면 먼저 `NavigationMode` 속성을 탭 지원으로 설정 해야 합니다. Android 4에서 `ActionBar` 속성은 다음과 같이 `NavigationMode`를 설정 하는 데 사용할 수 있는 Activity 클래스에서 사용할 수 있습니다.

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

이 작업이 완료 되 면 작업 모음에서 `NewTab` 메서드를 호출 하 여 탭을 만들 수 있습니다. 이 탭 인스턴스를 사용 하 여 `SetText` 및 `SetIcon` 메서드를 호출 하 여 탭의 레이블 텍스트와 아이콘을 설정할 수 있습니다. 이러한 호출은 아래에 표시 된 코드에서 순서 대로 수행 됩니다.

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

그러나 탭을 추가 하기 전에 `TabSelected` 이벤트를 처리 해야 합니다. 이 처리기에서 탭에 대 한 콘텐츠를 만들 수 있습니다. 작업 모음 탭은 활동에서 사용자 인터페이스의 일부를 나타내는 클래스인 *조각*으로 작업 하도록 디자인 되었습니다. 이 예의 경우 조각 뷰에는 다음과 같이 `Fragment` 하위 클래스에서 확장 되는 단일 `TextView`포함 되어 있습니다.

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

`TabSelected` 이벤트에 전달 된 이벤트 인수는 `TabEventArgs`형식입니다. 여기에는 아래와 같이 조각을 추가 하는 데 사용할 수 있는 `FragmentTransaction` 속성이 포함 됩니다.

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

마지막으로 다음 코드와 같이 `AddTab` 메서드를 호출 하 여 작업 모음에 탭을 추가할 수 있습니다.

```csharp
this.ActionBar.AddTab (tab);
```

전체 예제는이 문서에 대 한 샘플 코드의 *HelloTabsICS* 프로젝트를 참조 하세요.

## <a name="shareactionprovider"></a>ShareActionProvider

`ShareActionProvider` 클래스를 사용 하면 작업 모음에서 공유 작업을 수행할 수 있습니다. 공유 의도를 처리할 수 있는 앱 목록을 사용 하 여 작업 보기를 만들고 나중에 작업 모음에서 나중에 쉽게 액세스할 수 있도록 이전에 사용한 응용 프로그램의 기록을 유지할 수 있습니다. 이를 통해 응용 프로그램은 Android 전체에서 일관 된 사용자 환경을 통해 데이터를 공유할 수 있습니다.

### <a name="image-sharing-example"></a>이미지 공유 예제

예를 들어, 아래는 이미지를 공유 하는 메뉴 항목이 있는 작업 모음의 스크린샷입니다 ( [Shareactionprovider](https://docs.microsoft.com/samples/xamarin/monodroid-samples/shareactionproviderdemo) 샘플에서 가져온 것). 사용자가 작업 모음에서 메뉴 항목을 탭 할 때 ShareActionProvider는 `ShareActionProvider`연결 된 의도를 처리 하는 응용 프로그램을 로드 합니다. 이 예제에서는 메시징 응용 프로그램이 이전에 사용 되었으므로 작업 모음에 표시 됩니다.

[작업 모음의 메시징 응용 프로그램 아이콘 스크린샷 ![](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)

사용자가 작업 모음의 항목을 클릭 하면 아래와 같이 공유 이미지가 포함 된 메시징 앱이 시작 됩니다.

[원숭이 이미지를 표시 하는 메시징 앱 ![스크린샷](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)

### <a name="specifying-the-action-provider-class"></a>동작 공급자 클래스 지정

`ShareActionProvider`를 사용 하려면 다음과 같이 작업 모음 메뉴에 대 한 XML의 메뉴 항목에서 `android:actionProviderClass` 특성을 설정 합니다.

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

메뉴를 확장 하기 위해 Activity 하위 클래스에서 `OnCreateOptionsMenu`를 재정의 합니다. 메뉴에 대 한 참조가 있으면 아래와 같이 메뉴 항목의 `ActionProvider` 속성에서 `ShareActionProvider`을 가져온 다음 SetShareIntent 메서드를 사용 하 여 `ShareActionProvider`의도를 설정할 수 있습니다.

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

`ShareActionProvider`은 위의 코드에서 `SetShareIntent` 메서드로 전달 되는 의도를 사용 하 여 적절 한 작업을 시작 합니다. 이 경우 다음 코드를 사용 하 여 이미지를 보낼 의도를 만듭니다.

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

위의 코드 예제에서 이미지는 응용 프로그램과 함께 자산으로 포함 되 고 활동이 만들어질 때 공개적으로 액세스할 수 있는 위치에 복사 되므로 메시징 앱과 같은 다른 응용 프로그램에서 액세스할 수 있습니다. 이 문서와 함께 제공 되는 샘플 코드는이 예제의 전체 소스를 포함 하 여 사용을 보여 주고 있습니다.

## <a name="related-links"></a>관련 링크

- [Hello 탭 ICS (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/hellotabsics)
- [ShareActionProvider 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/shareactionproviderdemo)
