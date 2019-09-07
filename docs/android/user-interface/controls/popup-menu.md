---
title: 팝업 메뉴
description: 특정 뷰에 고정 된 팝업 메뉴를 추가 하는 방법입니다.
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: 8a808e6ea49338de5b6bd1618fc2227e6cf5f0b1
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764892"
---
# <a name="xamarinandroid-popup-menu"></a>Xamarin Android 팝업 메뉴

[PopupMenu](xref:Android.Widget.PopupMenu) ( _바로 가기 메뉴_라고도 함)는 특정 뷰에 고정 된 메뉴입니다. 다음 예제에서 단일 활동에는 단추가 포함 되어 있습니다. 사용자가 단추를 누르면 세 개의 항목 팝업 메뉴가 표시 됩니다.

[![단추 및 3 항목 팝업 메뉴를 사용 하는 앱의 예](popup-menu-images/01-app-example-sml.png)](popup-menu-images/01-app-example.png#lightbox)

## <a name="creating-a-popup-menu"></a>팝업 메뉴 만들기

첫 번째 단계는 메뉴에 대 한 메뉴 리소스 파일을 만들고 **리소스/메뉴**에 추가 하는 것입니다. 예를 들어 다음 XML은 이전 스크린샷 **Resources/menu/popup_menu**에 표시 된 세 항목 메뉴에 대 한 코드입니다.

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

그런 다음의 `PopupMenu` 인스턴스를 만들고 해당 뷰에 앵커를 고정 합니다. 의 `PopupMenu`인스턴스를 만들 때 해당 생성자에에 `Context` 대 한 참조와 메뉴가 연결 될 뷰를 전달 합니다. 결과적으로 팝업 메뉴는 생성 중에이 뷰에 고정 됩니다.

다음 예제 `PopupMenu` 에서는 단추에 대 한 click 이벤트 처리기에가 생성 됩니다 (이름이 지정 `showPopupMenu`됨). 이 단추는 다음 코드 예제와 같이이 `PopupMenu` 앵커 되는 뷰입니다.

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
};
```

마지막으로 팝업 메뉴는 앞에서 만든 메뉴 리소스와 *팽창* 되어야 합니다. 다음 예제에서는 메뉴의 [팽창](xref:Android.Views.LayoutInflater.Inflate*) 메서드에 대 한 호출을 추가 하 고 표시 메서드를 호출 하 [여 표시 합니다](xref:Android.Widget.PopupMenu.Show) .

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```

## <a name="handling-menu-events"></a>메뉴 이벤트 처리

사용자가 메뉴 항목을 선택 하면 [MenuItemClick](xref:Android.Widget.PopupMenu.MenuItemClick) click 이벤트가 발생 하 고 메뉴가 해제 됩니다. 메뉴 외부의 아무 곳 이나 누르면 간단히 해제 됩니다. 두 경우 모두 메뉴를 해제 하면 해당 [DismissEvent](xref:Android.Widget.PopupMenu.Dismiss) 이 발생 합니다. 다음 코드는 `MenuItemClick` 및 `DismissEvent` 이벤트에 대 한 이벤트 처리기를 추가 합니다.

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

- [PopupMenuDemo (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/popupmenudemo)
