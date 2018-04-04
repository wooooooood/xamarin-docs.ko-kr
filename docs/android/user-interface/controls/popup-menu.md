---
title: 팝업 메뉴
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/18/2017
ms.openlocfilehash: e7fad84133ca712c531ab0d12a67db78103c7cdd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="popup-menu"></a>팝업 메뉴

`PopupMenu` 클래스 특정 보기에 연결 된 팝업 메뉴를 표시 하는 것에 대 한 지원을 추가 합니다. 다음 그림은 두 번째 항목을 선택 하는 것 처럼 강조 표시는 단추에서 표시 하는 팝업 메뉴를 보여 줍니다.

 [![세 항목 중 세 번 PopopMenu 예제](popup-menu-images/20-popupmenu.png)](popup-menu-images/20-popupmenu.png#lightbox)

Android 4 몇 가지 새 기능을 추가 `PopupMenu` 즉 작업할 좀 더 쉽게 만들어 주는:

-   **Inflate** &ndash; The 팽창 메서드는 팝업 메뉴 클래스에서 직접 사용할 수 있는 더 합니다.
-   **DismissEvent** &ndash; 팝업 메뉴의 클래스는 DismissEvent 되었습니다.

이러한 향상 된이 기능에 살펴보겠습니다. 이 예제에는 단추를 포함 하는 단일 활동을 개가 있습니다. 사용자가 단추를 클릭 하면 아래와 같이 팝업 메뉴가 표시 됩니다.

 [![단추 및 3 개 항목 팝업 메뉴와 함께 에뮬레이터에서 실행 중인 응용 프로그램의 예](popup-menu-images/06-popupmenu.png)](popup-menu-images/06-popupmenu.png#lightbox)


## <a name="creating-a-popup-menu"></a>팝업 메뉴 만들기

인스턴스를 만들 때는 `PopupMenu`, 해당 생성자에 대 한 참조를 전달 해야는 `Context`, 메뉴 연결 되어 있는 보기 뿐만 아니라 합니다. 만들 경우에 `PopupMenu` 우리의 단추에 대 한 click 이벤트 처리기에 라는 `showPopupMenu`합니다.
이 단추는 또한 연결 수를 보기는 `PopupMenu`다음 코드에 나온 것 처럼:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
}
```

Android 3에서 XML 리소스에서 메뉴 확장할 하는 코드에 대 한 참조를 먼저 가져온 있습니다 필요한는 `MenuInflator`, 한 다음 호출 해당 `Inflate` 는 메뉴 및 메뉴 인스턴스를 확장할를 포함 하는 XML의 리소스 ID 사용 하 여 메서드. 이러한 접근 방식은 Android 4에서 프로그램과 다음 코드와 나중에 계속 적용 됩니다.

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.MenuInflater.Inflate (Resource.Menu.popup_menu, menu.Menu);
};
```

그러나 Android 4부터 이제 호출할 수 있습니다 `Inflate` 의 인스턴스에서 직접는 `PopupMenu`합니다. 이렇게 하면 코드를 다음과 같이 더욱 간결 하 게 합니다.

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```

위의 코드에서 메뉴 않아서 후를 호출 하 `menu.Show` 화면에 표시 합니다.


## <a name="handling-menu-events"></a>메뉴 이벤트 처리

사용자가 메뉴 항목을 선택할 때의 `MenuItemClick` 이벤트가 발생 하 고 메뉴가 해제 됩니다. 메뉴 바깥쪽 아무 곳 이나 탭은 단순히 해제 합니다. Android 4는 메뉴가 해제 될 때부터 두 경우 모두 해당 `DismissEvent` 발생 합니다. 다음 코드는 모두에 대 한 이벤트 처리기를 추가 합니다.는 `MenuItemClick` 및 `DismissEvent` 이벤트:

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
- [아이스크림 샌드위치 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
