---
title: 팝업 메뉴
description: 특정 보기에 고정 되는 팝업 메뉴를 추가 하는 방법.
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/31/2018
ms.openlocfilehash: d7cadde88e9ae7ee30815ee9323785038dbb1a39
ms.sourcegitcommit: ecdc031e9e26bbbf9572885531ee1f2e623203f5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39393661"
---
# <a name="popup-menu"></a>팝업 메뉴

합니다 [팝업 메뉴](https://developer.xamarin.com/api/type/Android.Widget.PopupMenu/) (라고도 _바로 가기 메뉴_) 특정 보기 고정 되는 메뉴입니다. 다음 예제에서는 단일 작업 단추를 포함합니다. 사용자가 단추를 누르면 항목이 세 팝업 메뉴가 표시 됩니다.

[![단추 및 팝업 메뉴 항목이 세 개를 사용 하 여 앱의 예](popup-menu-images/01-app-example-sml.png)](popup-menu-images/01-app-example.png#lightbox)


## <a name="creating-a-popup-menu"></a>팝업 메뉴 만들기

첫 번째 단계는 메뉴의 메뉴 리소스 파일을 만들고 그 안에 배치 **리소스/메뉴**합니다. 예를 들어, 다음 XML은 이전 스크린샷에서 표시 되는 세 가지 항목 메뉴에 대 한 코드 **Resources/menu/popup_menu.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/item1"
          android:title="item 1" />
    <item android:id="@+id/item1"
          android:title="item 2" />
    <item android:id="@+id/item1"
          android:title="item 3" />
</menu>
```

그런 다음의 인스턴스를 만듭니다 `PopupMenu` 및 해당 보기에 고정 합니다. 인스턴스를 만들면 `PopupMenu`, 생성자에 대 한 참조를 전달 합니다 `Context` 메뉴 연결 보기 및 합니다. 결과적으로, 팝업 메뉴는 생성 하는 동안이 보기에 앵커 되어 있습니다.

다음 예제에서는 `PopupMenu` 단추의 클릭 이벤트 처리기에 만들어집니다 (이라는 `showPopupMenu`). 이 단추는 또한 뷰를는 `PopupMenu` 다음 코드 예제와 같이 고정 됩니다.

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
};
```

마지막으로 팝업 메뉴 해야 *배포용* 앞서 만든 메뉴 리소스를 사용 하 여 합니다. 다음 예제에서는 메뉴의 호출에서에서 [Inflate](https://developer.xamarin.com/api/member/Android.Views.LayoutInflater.Inflate/p/System.Int32/Android.Views.ViewGroup/) 메서드가 추가 되 고 [표시](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Show%28%29/) 메서드를 호출 하 고 표시:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```


## <a name="handling-menu-events"></a>메뉴 이벤트를 처리합니다.

사용자가 메뉴 항목을 선택 합니다 [MenuItemClick](https://developer.xamarin.com/api/event/Android.Widget.PopupMenu.MenuItemClick/) 클릭 이벤트가 발생 하 고 메뉴가 해제 됩니다. 메뉴 밖 아무 곳 이나 눌러서는 단순히 알림을 해제 합니다. 두 경우 모두 메뉴가 닫힐 때 해당 [DismissEvent](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Dismiss%28%29/) 발생 합니다. 다음 코드는 모두에 대 한 이벤트 처리기를 추가 합니다 `MenuItemClick` 고 `DismissEvent` 이벤트:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);

    menu.MenuItemClick += (s1, arg1) => {
        Console.WriteLine ("{0} selected", arg1.Item.TitleFormatted);
    };

    menu.DismissEvent += (s2, arg2) => {
        Console.WriteLine ("menu dismissed");
    };
    menu.Show ();
};
```



## <a name="related-links"></a>관련 링크

- [PopupMenuDemo (샘플)](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/)
