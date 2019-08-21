---
title: Xamarin.ios 항목
description: 도구 모음 항목 클래스는 응용 프로그램의 탐색 모음에서 사용 되는 특수 한 형식의 단추입니다.
ms.prod: xamarin
ms.assetId: CC737D54-0280-46BD-A2BC-A0FB67DDD6A1
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/29/2019
ms.openlocfilehash: 9bb041206dbafb68c1bb60bbbb3068f0a2001b55
ms.sourcegitcommit: 9178e2e689f027212ea3e623b556b312985d79fe
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69658052"
---
# <a name="xamarinforms-toolbaritem"></a>Xamarin.ios 항목

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)

Xamarin.ios [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) 클래스는 `Page` 개체의 `ToolbarItems` 컬렉션에 추가할 수 있는 특수 한 형식의 단추입니다. 각 `ToolbarItem` 개체는 응용 프로그램의 탐색 모음에서 단추로 표시 됩니다. 인스턴스 `ToolbarItem` 는 아이콘을 포함 하 고 기본 또는 보조 메뉴 항목으로 표시 될 수 있습니다. 클래스 `ToolbarItem` 는에서 [`MenuItem`](xref:Xamarin.Forms.MenuItem)상속 됩니다.

다음 스크린샷에는 iOS `ToolbarItem` 및 Android의 탐색 모음에 있는 개체가 표시 됩니다.

![Android 및 iOS의 "도구 항목 데모 스크린샷"](toolbaritem-images/toolbaritem-device-screenshot.png "Android 및 iOS의 도구 모음의 항목 데모 스크린샷")

클래스 `ToolbarItem` 는 다음 속성을 정의 합니다.

* [`Order`](xref:Xamarin.Forms.ToolbarItem.Order)인스턴스가 기본 메뉴 또는 보조 메뉴에 표시 `ToolbarItemOrder` 되는지 여부를 결정 하는 열거형 `ToolbarItem` 값입니다.
* [`Priority`](xref:Xamarin.Forms.ToolbarItem.Priority)`Page` 개체의 컬렉션`ToolbarItems` 에 있는 항목의 표시 순서를 결정 하는 값입니다.`integer`

클래스 `ToolbarItem` 는 `MenuItem` 클래스에서 다음과 같이 일반적으로 사용 되는 속성을 상속 합니다.

* [`Command`](xref:Xamarin.Forms.MenuItem.Command)는 핑거 탭 또는 클릭과 같은 사용자 동작을 viewmodel에 정의 된 명령에 바인딩할 수 있는입니다.`ICommand`
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)는에 전달 `Command`되어야 하는 매개 변수를 지정 하는입니다.`object`
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)개체의 표시 아이콘을 결정 하는 값입니다.`ImageSource` `ToolbarItem`
* [`Text`](xref:Xamarin.Forms.MenuItem.Text)개체의 표시 텍스트를 결정 하는입니다.`string` `ToolbarItem`

이러한 속성은 개체에 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 의해 지원 `ToolbarItem` 되므로 인스턴스가 데이터 바인딩의 대상이 될 수 있습니다.

> [!NOTE]
> 개체에서 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) 도구 모음을 만드는 대신 여러 뷰가 포함 된 레이아웃 [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) 클래스로 연결 된 속성을 설정 합니다. 자세한 내용은 [탐색 모음에서 보기 표시](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md#displaying-views-in-the-navigation-bar)를 참조 하세요.

## <a name="create-a-toolbaritem"></a>단일 항목 만들기

개체 `ToolbarItem` 는 XAML에서 인스턴스화할 수 있습니다. `Text` 및`IconImageSource` 속성을 설정 하 여 탐색 모음에 단추를 표시 하는 방법을 결정할 수 있습니다. 다음 예제에서는 몇 가지 공용 속성 집합 `ToolbarItem` 을 사용 하 여를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<ToolbarItem Text="Example Item"
             IconImageSource="example_icon.png"
             Order="Primary"
             Priority="0" />
```

이 예제 `ToolbarItem` 에서는 텍스트를 포함 하는 개체와 아이콘을 기본 탐색 모음 영역에 먼저 표시 합니다. 코드 `ToolbarItem` 에서를 만들어 `ToolbarItems` 컬렉션에 추가할 수도 있습니다.

```csharp
ToolbarItem item = new ToolbarItem
{
    Text = "Example Item",
    IconImageSource = ImageSource.FromFile("example_icon.png"),
    Order = ToolbarItemOrder.Primary,
    Priority = 0
};

// "this" refers to a Page object
this.ToolbarItems.Add(item);
```

속성으로 제공 되는 `string`이 나타내는 파일은 각 플랫폼 프로젝트에 존재 해야 합니다. `IconImageSource`

> [!NOTE]
> 이미지 자산은 각 플랫폼에서 다르게 처리 됩니다. 는 `ImageSource` 로컬 파일이 나 포함 된 리소스, URI 또는 스트림을 포함 하 여 소스에서 가져올 수 있습니다. Xamarin.ios에서 속성과 이미지를 설정 `IconImageSource` 하는 방법에 대 한 자세한 내용은 [xamarin.ios의 이미지](~/xamarin-forms/user-interface/images.md)를 참조 하세요.

## <a name="define-button-behavior"></a>단추 동작 정의

클래스 `ToolbarItem` `Clicked` 는 클래스`MenuItem` 에서 이벤트를 상속 합니다. 이벤트 처리기를 `Clicked` 이벤트에 연결 하 여 XAML에서 인스턴스에 대 `ToolbarItem` 한 탭 또는 클릭에 반응할 수 있습니다.

```xaml
<ToolbarItem ...
             Clicked="OnItemClicked" />
```

이벤트 처리기를 코드에 연결할 수도 있습니다.

```csharp
ToolbarItem item = new ToolbarItem { ... }
item.Clicked += OnItemClicked;
```

이전 예제에서는 이벤트 `OnItemClicked` 처리기를 참조 했습니다. 다음 코드에서는 구현 예를 보여 줍니다.

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    ToolbarItem item = (ToolbarItem)sender;
    messageLabel.Text = $"You clicked the \"{item.Text}\" toolbar item.";
}
```

`ToolbarItem`또한 개체는 `Command` 및 `CommandParameter` 속성을 사용 하 여 이벤트 처리기 없이 사용자 입력에 반응할 수 있습니다. `ICommand` 인터페이스 및 MVVM 데이터 바인딩에 대 한 자세한 내용은 [xamarin.ios MenuItem MVVM Behavior](~/xamarin-forms/user-interface/menuitem.md#define-menuitem-behavior-with-mvvm)를 참조 하세요.

## <a name="primary-and-secondary-menus"></a>기본 및 보조 메뉴

열거형에는 `Default`, `Primary` 및`Secondary`값이있습니다. `ToolbarItemOrder`

속성이로 `Primary`설정 되 면 개체는 `ToolbarItem` 모든 플랫폼의 기본 탐색 모음에 표시 됩니다. `Order` `ToolbarItem`개체는 페이지 제목 보다 우선 순위가 지정 되며, 항목의 공간을 확보 하기 위해 잘립니다. 다음 스크린샷에서는 iOS `ToolbarItem` 및 Android의 기본 메뉴에 있는 개체를 보여 줍니다.

!["화면 캡처 항목 주 메뉴 스크린샷 Android 및 iOS"](toolbaritem-images/toolbaritem-primary-menu.png "도구 모음의 항목 기본 메뉴 스크린샷 (Android 및 iOS") )

`Order` 속성이 로`Secondary`설정 된 경우 동작은 플랫폼 마다 다릅니다. UWP 및 Android에서 항목 메뉴 `Secondary` 는 탭 하거나 클릭 하 여 세로 목록에 항목을 표시할 수 있는 세 개의 점으로 표시 됩니다. IOS에서 항목 메뉴 `Secondary` 는 탐색 모음 아래에 가로 목록으로 표시 됩니다. 다음 스크린샷에는 iOS 및 Android의 보조 메뉴가 표시 됩니다.

!["화면 캡처 항목 보조 메뉴 스크린샷 Android 및 iOS"](toolbaritem-images/toolbaritem-secondary-menu.png "도구 모음의 항목 보조 메뉴 스크린샷 (Android 및 iOS") )

> [!WARNING]
> `Order` 속성이 `ToolbarItem` 로`Secondary` 설정 된 개체의 아이콘 동작은 플랫폼 간에 일관 되지 않습니다. 보조 메뉴에 `IconImageSource` 표시 되는 항목에 대해서는 속성을 설정 하지 마십시오.

## <a name="related-links"></a>관련 링크

* [나이 항목 데모](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)
* [Xamarin.ios의 이미지](~/xamarin-forms/user-interface/images.md)
* [Xamarin.ios MenuItem](~/xamarin-forms/user-interface/menuitem.md)