---
title: Xamarin.ios MenuItem
description: MenuItem 클래스는 ListView 항목 상황에 맞는 메뉴 및 셸 응용 프로그램 플라이 아웃 메뉴와 같은 메뉴에 대 한 메뉴 항목을 만드는 데 사용 됩니다.
ms.prod: xamarin
ms.assetId: 62655C21-6053-466D-A7F4-DE2BE36538F5
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/01/2019
ms.openlocfilehash: cbc39ee38ce623ce446d50494829119058fc88dc
ms.sourcegitcommit: 1341f2950b775a4daa7d0548a51fdef759afd6e3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69976468"
---
# <a name="xamarinforms-menuitem"></a>Xamarin.ios MenuItem

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitem/)

Xamarin.ios [`MenuItem`](xref:Xamarin.Forms.MenuItem) 클래스는 항목 컨텍스트 메뉴 및 셸 응용 프로그램 플라이 아웃 `ListView` 메뉴와 같은 메뉴에 대 한 메뉴 항목을 정의 합니다.

다음 스크린샷에서는 iOS `MenuItem` 및 Android의 `ListView` 상황에 맞는 메뉴에 있는 개체를 보여 줍니다.

[(menuitem-images/menuitem-demo-cropped.png "Ios 및 android의") !["Ios 및 android의 menuitem"]menuitem] (menuitem-images/menuitem-demo-full.png#lightbox "IOS 및 Android의 menuitemfull image")

클래스 `MenuItem` 는 다음 속성을 정의 합니다.

* [`Command`](xref:Xamarin.Forms.MenuItem.Command)는 핑거 탭 또는 클릭과 같은 사용자 동작을 viewmodel에 정의 된 명령에 바인딩할 수 있는입니다.`ICommand`
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)는에 전달 `Command`되어야 하는 매개 변수를 지정 하는입니다.`object`
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)표시 아이콘을 정의 하는 값입니다.`ImageSource`
* [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive)가 목록에서 연결 된 UI 요소 `MenuItem` 를 제거 하는지 여부를 나타내는 값입니다.`bool`
* [`IsEnabled`](xref:Xamarin.Forms.MenuItem.IsEnabled)이 개체가 사용자 입력에 응답 하는지 여부를 결정 하는 값입니다.`bool`
* [`Text`](xref:Xamarin.Forms.MenuItem.Text)표시 텍스트를 지정 하는 값입니다.`string`

이러한 속성은 개체에 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 의해 지원 `MenuItem` 되므로 인스턴스가 데이터 바인딩의 대상이 될 수 있습니다.

## <a name="create-a-menuitem"></a>MenuItem 만들기

`MenuItem`개체는 `ListView` 개체의 항목에 대 한 상황에 맞는 메뉴 내에서 사용할 수 있습니다. 가장 일반적인 `MenuItem` 패턴은 `ViewCell` 인스턴스 내에서 개체를 만드는 것입니다 `ItemTemplate`.이 개체는의 `DataTemplate` `ListView`개체로 사용 됩니다. 개체를 채울 때 항목에 대 한 상황에 맞는 메뉴가 `DataTemplate`활성화 되 면 `MenuItem` 선택 항목을 표시 하 여를 사용 하 여 각 항목을 만듭니다. `ListView`

다음 예제에서는 `ListView` 개체 `MenuItem` 의 컨텍스트 내에서 인스턴스화를 보여 줍니다.

```xaml
<ListView>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <ViewCell.ContextActions>
                    <MenuItem Text="Context Menu Option" />
                </ViewCell.ContextActions>
                <Label Text="{Binding .}" />
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

코드 `MenuItem` 에서를 만들 수도 있습니다.

```csharp
// A function returns a ViewCell instance that
// is used as the template for each list item
DataTemplate dataTemplate = new DataTemplate(() =>
{
    // A Label displays the list item text
    Label label = new Label();
    label.SetBinding(Label.TextProperty, ".");

    // A ViewCell serves as the DataTemplate
    ViewCell viewCell = new ViewCell
    {
        View = label
    };

    // Add a MenuItem instance to the ContextActions
    MenuItem menuItem = new MenuItem
    {
        Text = "Context Menu Option"
    };
    viewCell.ContextActions.Add(menuItem);

    // The function returns the custom ViewCell
    // to the DataTemplate constructor
    return viewCell;
});

// Finally, the dataTemplate is provided to
// the ListView object
ListView listView = new ListView
{
    ...
    ItemTemplate = dataTemplate
};
```

## <a name="define-menuitem-behavior-with-events"></a>이벤트를 사용 하 여 MenuItem 동작 정의

`MenuItem` 클래스는 `Clicked` 이벤트를 노출합니다. 이 이벤트에 이벤트 처리기를 연결 하 여 XAML의 `MenuItem` 인스턴스에서 탭 하거나 클릭에 반응할 수 있습니다.

```xaml
<MenuItem ...
          Clicked="OnItemClicked" />
```

이벤트 처리기를 코드에 연결할 수도 있습니다.

```csharp
MenuItem item = new MenuItem { ... }
item.Clicked += OnItemClicked;
```

이전 예제에서는 이벤트 `OnItemClicked` 처리기를 참조 했습니다. 다음 코드에서는 구현 예를 보여 줍니다.

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    // The sender is the menuItem
    MenuItem menuItem = sender as MenuItem;

    // Access the list item through the BindingContext
    var contextItem = menuItem.BindingContext;

    // Do something with the contextItem here
}
```

## <a name="define-menuitem-behavior-with-mvvm"></a>MVVM를 사용 하 여 MenuItem 동작 정의

클래스 `MenuItem` 는 개체 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 와`ICommand` 인터페이스를 통해 MVVM (모델-뷰-ViewModel) 패턴을 지원 합니다. 다음 XAML은 viewmodel `MenuItem` 에 정의 된 명령에 바인딩되는 인스턴스를 보여 줍니다.

```xaml
<ContentPage.BindingContext>
    <viewmodels:ListPageViewModel />
</ContentPage.BindingContext>

<StackLayout>
    <Label Text="{Binding Message}" ... />
    <ListView ItemsSource="{Binding Items}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ViewCell.ContextActions>
                        <MenuItem Text="Edit"
                                    IconImageSource="icon.png"
                                    Command="{Binding Source={x:Reference contentPage}, Path=BindingContext.EditCommand}"
                                    CommandParameter="{Binding .}"/>
                        <MenuItem Text="Delete"
                                    Command="{Binding Source={x:Reference contentPage}, Path=BindingContext.DeleteCommand}"
                                    CommandParameter="{Binding .}"/>
                    </ViewCell.ContextActions>
                    <Label Text="{Binding .}" />
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackLayout>
```

이전 예제에서 두 `MenuItem` 개체는 viewmodel의 명령에 바인딩된 `Command` 및 `CommandParameter` 속성을 사용 하 여 정의 됩니다. Viewmodel에는 XAML에서 참조 되는 명령이 포함 되어 있습니다.

```csharp
public class ListPageViewModel : INotifyPropertyChanged
{
    ...

    public ICommand EditCommand => new Command<string>((string item) =>
    {
        Message = $"Edit command was called on: {item}";
    });

    public ICommand DeleteCommand => new Command<string>((string item) =>
    {
        Message = $"Delete command was called on: {item}";
    });
}
```

예제 응용 프로그램에는 `DataService` `ListView` 개체를 채우는 데 사용 되는 항목의 목록을 가져오는 데 사용 되는 클래스가 포함 되어 있습니다. Viewmodel은 `DataService` 클래스의 항목을 사용 하 여 인스턴스화되고 코드 숨김으로로 설정 `BindingContext` 됩니다.

```csharp
public MenuItemXamlMvvmPage()
{
    InitializeComponent();
    BindingContext = new ListPageViewModel(DataService.GetListItems());
}
```

## <a name="menuitem-icons"></a>MenuItem 아이콘

> [!WARNING]
> `MenuItem`개체는 Android 에서만 아이콘을 표시 합니다. 다른 플랫폼에서는 `Text` 속성으로 지정 된 텍스트만 표시 됩니다.

 아이콘은 속성을 `IconImageSource` 사용 하 여 지정 됩니다. 아이콘을 지정 하면 `Text` 속성에 지정 된 텍스트가 표시 되지 않습니다. 다음 스크린샷은 Android에서 아이콘이 `MenuItem` 있는를 보여 줍니다.

!["Android의 MenuItem 아이콘 스크린샷"](menuitem-images/menuitem-android-icon.png "Android의 MenuItem 아이콘 스크린샷")

Xamarin.ios에서 이미지를 사용 하는 방법에 대 한 자세한 내용은 [xamarin.ios의 이미지](~/xamarin-forms/user-interface/images.md)를 참조 하세요.

## <a name="cross-platform-context-menu-behavior"></a>플랫폼 간 상황에 맞는 메뉴 동작

상황에 맞는 메뉴는 각 플랫폼 마다 다르게 액세스 되 고 표시 됩니다.

Android에서 상황에 맞는 메뉴는 목록 항목을 길게 누르면 활성화 됩니다. 상황에 맞는 메뉴는 제목 및 탐색 모음 영역을 `MenuItem` 대체 하 고 옵션은 가로 단추로 표시 됩니다.

!["Android의 상황에 맞는 메뉴 스크린샷"](menuitem-images/menuitem-android-icon.png "Android의 상황에 맞는 메뉴 스크린샷")

IOS에서 상황에 맞는 메뉴는 목록 항목을 살짝 밀어 활성화 됩니다. 상황에 맞는 메뉴는 목록 항목 `MenuItems` 에 표시 되며 가로 단추로 표시 됩니다.

!["IOS의 상황에 맞는 메뉴 스크린샷"](menuitem-images/menuitem-ios-contextmenu.png "IOS에 대 한 상황에 맞는 메뉴의 스크린샷")

UWP에서는 목록 항목을 마우스 오른쪽 단추로 클릭 하 여 상황에 맞는 메뉴를 활성화 합니다. 상황에 맞는 메뉴는 커서 근처에 세로 목록으로 표시 됩니다.

!["UWP의 상황에 맞는 메뉴 스크린샷"](menuitem-images/menuitem-uwp.png "UWP의 상황에 맞는 메뉴 스크린샷")

## <a name="related-links"></a>관련 링크

* [MenuItem 데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-menuitem/)
* [Xamarin.ios의 이미지](~/xamarin-forms/user-interface/images.md)
