---
title: Xamarin.Forms Shell 검색
description: Xamarin.Forms Shell 애플리케이션은 각 페이지 위쪽에 추가될 수 있는 검색 상자에 의해 제공되는 통합 검색 기능을 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: F8F9471D-6771-4D23-96C0-2B79473A06D4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2019
ms.openlocfilehash: 9bd4fe5f1a35e2a6f36540cbee13838841b36d92
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "75490066"
---
# <a name="xamarinforms-shell-search"></a>Xamarin.Forms Shell 검색

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Xamarin.Forms Shell에는 `SearchHandler` 클래스에서 제공되는 통합 검색 기능이 포함됩니다. `Shell.SearchHandler` 연결된 속성을 서브클래스 `SearchHandler` 개체로 설정하여 검색 기능을 페이지에 추가할 수 있습니다. 이를 통해 검색 상자가 페이지 위쪽에 추가됩니다.

[![iOS 및 Android의 셸 SearchHandler 스크린샷](search-images/searchhandler.png "셸 SearchHandler")](search-images/searchhandler-large.png#lightbox "셸 SearchHandler")

검색 상자에 쿼리가 입력되면 `Query` 속성이 업데이트되고 업데이트될 때마다 `OnQueryChanged` 메서드가 실행됩니다. 데이터를 사용하여 검색 제안 영역을 채우도록 이 메서드를 재정의할 수 있습니다.

[![iOS 및 Android의 셸 SearchHandler 검색 결과 스크린샷](search-images/search-suggestions.png "셸 SearchHandler 검색 결과")](search-images/search-suggestions-large.png#lightbox "셸 SearchHandler 검색 결과")

그런 다음 검색 제안 영역에서 결과가 선택되면 다음 `OnItemSelected` 메서드가 실행됩니다. 이 메서드는 세부 정보 페이지 탐색 등의 방법으로 응답이 적절히 이루어지도록 재정의할 수 있습니다.

## <a name="create-a-searchhandler"></a>SearchHandler 만들기

`SearchHandler` 클래스를 서브클래싱하고 `OnQueryChanged` 및 `OnItemSelected` 메서드를 재정의하여 검색 기능을 셸 애플리케이션에 추가할 수 있습니다.

```csharp
public class MonkeySearchHandler : SearchHandler
{
    protected override void OnQueryChanged(string oldValue, string newValue)
    {
        base.OnQueryChanged(oldValue, newValue);

        if (string.IsNullOrWhiteSpace(newValue))
        {
            ItemsSource = null;
        }
        else
        {
            ItemsSource = MonkeyData.Monkeys
                .Where(monkey => monkey.Name.ToLower().Contains(newValue.ToLower()))
                .ToList<Animal>();
        }
    }

    protected override async void OnItemSelected(object item)
    {
        base.OnItemSelected(item);

        // Note: strings will be URL encoded for navigation (e.g. "Blue Monkey" becomes "Blue%20Monkey"). Therefore, decode at the receiver.
        await (App.Current.MainPage as Xamarin.Forms.Shell).GoToAsync($"monkeydetails?name={((Animal)item).Name}");
    }
}
```

`OnQueryChanged`는 다음 두 개의 인수를 재정의합니다. `oldValue`는 이전 검색 쿼리를 포함함하고 `newValue`는 현재 검색 쿼리를 포함합니다. `SearchHandler.ItemsSource` 속성을 현재 검색 쿼리와 일치하는 항목이 포함된 `IEnumerable` 컬렉션으로 설정하여 검색 제안 영역을 업데이트할 수 있습니다.

사용자가 검색 결과를 선택하면 `OnItemSelected` 재정의가 실행되고 `SelectedItem` 속성이 설정됩니다. 이 예제에서는 메서드가 선택된 `Animal`에 대한 데이터를 표시하는 다른 페이지로 이동합니다. 탐색에 대한 자세한 내용은 [Xamarin.Forms Shell 탐색](navigation.md)을 참조하세요.

> [!NOTE]
> 검색 상자 모양을 제어하기 위해 추가적인 `SearchHandler` 속성을 설정할 수 있습니다.

## <a name="consume-a-searchhandler"></a>SearchHandler 사용

`Shell.SearchHandler` 연결된 속성을 서브클래싱된 형식의 개체로 설정하여 서브클래싱된 `SearchHandler`를 사용할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true"
                                      DisplayMemberName="Name" />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name"
});
```

이 `MonkeySearchHandler.OnQueryChanged` 메서드는 `Animal` 개체의 `List`를 반환합니다. `DisplayMemberName` 속성은 각 `Animal` 개체의 `Name` 속성으로 설정되므로 제안 영역에 표시되는 데이터는 각각 동물 이름이 됩니다.

`ShowsResults` 속성은 `true`로 설정되므로 사용자가 검색 쿼리를 입력할 때 검색 제안이 표시됩니다.

[![iOS 및 Android의 셸 SearchHandler 검색 결과 스크린샷](search-images/search-results.png "셸 SearchHandler 검색 결과")](search-images/search-results-large.png#lightbox "셸 SearchHandler 검색 결과")

검색 쿼리가 변경되면 검색 제안 영역이 업데이트됩니다.

[![iOS 및 Android의 셸 SearchHandler 검색 결과 스크린샷](search-images/search-results-change.png "셸 SearchHandler 검색 결과")](search-images/search-results-change-large.png#lightbox "셸 SearchHandler 검색 결과")

검색 결과가 선택되면 `MonkeyDetailPage`로 이동되고 선택된 원숭이에 대한 데이터가 표시됩니다.

[![iOS 및 Android의 상세한 원숭이 스크린샷](search-images/detailpage.png "원숭이 세부 정보")](search-images/detailpage-large.png#lightbox "원숭이 세부 정보")

## <a name="define-search-results-item-appearance"></a>검색 결과 항목 모양 정의

검색 결과에 `string` 데이터를 표시하는 것 외에도, `SearchHandler.ItemTemplate` 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)으로 설정하여 각 검색 결과 항목의 모양을 정의할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">    
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true">
            <controls:MonkeySearchHandler.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="0.15*" />
                            <ColumnDefinition Width="0.85*" />
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="40"
                               WidthRequest="40" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                    </Grid>
                </DataTemplate>
            </controls:MonkeySearchHandler.ItemTemplate>
       </controls:MonkeySearchHandler>
    </Shell.SearchHandler>
    ...
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name",
    ItemTemplate = new DataTemplate(() =>
    {
        Grid grid = new Grid { Padding = 10 };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.15, GridUnitType.Star) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.85, GridUnitType.Star) });

        Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 40, WidthRequest = 40 };
        image.SetBinding(Image.SourceProperty, "ImageUrl");
        Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        nameLabel.SetBinding(Label.TextProperty, "Name");

        grid.Children.Add(image);
        grid.Children.Add(nameLabel, 1, 0);
        return grid;
    })
});
```

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)에 지정된 요소는 제안 영역에 있는 각 항목의 모양을 정의합니다. 이 예제에서 `DataTemplate` 내의 레이아웃은 [`Grid`](xref:Xamarin.Forms.Grid)로 관리합니다. `Grid`는 [`Image`](xref:Xamarin.Forms.Image) 개체 및 [`Label`](xref:Xamarin.Forms.Label) 개체를 포함하고, 두 개체는 모두 각 `Monkey` 개체의 속성에 바인딩됩니다.

다음 스크린샷은 제안 영역에서 각 항목을 템플릿화한 결과를 보여 줍니다.

[![iOS 및 Android의 템플릿화된 셸 SearchHandler 검색 결과 스크린샷](search-images/search-results-template.png "템플릿화된 셸 SearchHandler 검색 결과")](search-images/search-results-template-large.png#lightbox "템플릿화된 셸 SearchHandler 검색 결과")

데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

## <a name="search-box-visibility"></a>검색 상자 표시 여부

`SearchHandler`가 페이지 위쪽에 추가되면 기본적으로 검색 상자는 표시되고 완전히 확장됩니다. 그러나 이 동작은 `SearchHandler.SearchBoxVisibility` 속성을 `SearchBoxVisibility` 열거형 멤버 중 하나로 설정하여 변경할 수 있습니다.

- `Hidden` – 검색 상자가 표시되지 않거나 액세스할 수 없습니다.
- `Collapsible` – 사용자가 검색 상자를 표시하는 작업을 수행할 때까지 검색 상자가 숨겨집니다. iOS에서 검색 상자는 페이지 콘텐츠를 세로로 튕기면 표시되고 Android에서는 물음표 아이콘을 누르면 검색 상자가 표시됩니다.
- `Expanded` – 검색 상자가 표시되고 완전히 확장됩니다. 이 값은 `SearchHandler.SearchBoxVisibility` 속성의 기본값입니다.

> [!IMPORTANT]
> iOS에서 축소 가능한 검색 상자 기능을 사용하려면 iOS 11 이상 버전이 필요합니다.

다음 예제에서는 검색 상자를 숨기는 방법을 보여 줍니다.

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:AnimalSearchHandler SearchBoxVisibility="Hidden"
                                      ... />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

## <a name="search-box-focus"></a>검색 상자 포커스

검색 상자를 탭하면 화상 키보드가 호출되고 검색 상자에 입력 포커스가 나타납니다. 검색 상자에서 입력 포커스를 설정하고 성공적인 경우 `true`를 반환하는 `Focus` 메서드를 호출하여 프로그래밍 방식으로 수행할 수도 있습니다. 검색 상자에 포커스가 나타나면 `Focus` 이벤트가 발생하고 재정의 가능한 `OnFocused` 메서드가 호출됩니다.

검색 상자에 입력 포커스가 있는 경우 화면에서 다른 부분을 탭하면 화상 키보드가 해제되고 검색 상자의 입력 포커스가 사라집니다. `Unfocus` 메서드를 호출하여 프로그래밍 방식으로 수행할 수도 있습니다. 검색 상자에서 포커스가 사라지면 `Unfocused` 이벤트가 해제되고 재정의 가능한 `OnUnfocus` 메서드가 호출됩니다.

검색 상자의 포커스 상태는 `SearchHandler`에 현재 입력 포커스가 있는 경우 `true`를 반환하는 `IsFocused` 속성을 통해 검색할 수 있습니다.

## <a name="searchhandler-appearance"></a>SearchHandler 모양

`SearchHandler` 클래스는 그 모양에 영향을 주는 다음 속성을 정의합니다.

- `Color` 형식의 `BackgroundColor`는 검색 상자 텍스트의 배경색입니다.
- `Color` 형식의 `CancelButtonColor`는 취소 단추의 색입니다.
- `double` 형식의 `CharacterSpacing`은 `SearchHandler` 텍스트를 구성하는 문자 사이의 간격입니다.
- `FontAttributes` 형식의 `FontAttributes`는 검색 상자 텍스트가 기울임꼴인지 볼드인지를 나타냅니다.
- `string` 형식의 `FontFamily`는 검색 상자 텍스트에 사용되는 글꼴 패밀리입니다.
- `double` 형식의 `FontSize`는 검색 상자 텍스트의 크기입니다.
- `TextAlignment` 형식의 `HorizontalTextAlignment`는 검색 상자 텍스트의 가로 맞춤입니다.
- `Color` 형식의 `PlaceholderColor`는 자리 표시자 검색 상자 텍스트의 색입니다.
- `Color` 형식의 `TextColor`는 검색 상자 텍스트의 색입니다.
- `TextAlignment` 형식의 `VerticalTextAlignment`는 검색 상자 텍스트의 세로 맞춤입니다.

## <a name="searchhandler-keyboard"></a>SearchHandler 키보드

사용자가 `SearchHandler`와 상호 작용할 때 나타나는 키보드는 `Keyboard` 속성을 이용하여 [`Keyboard`](xref:Xamarin.Forms.Keyboard) 클래스의 다음 속성 중 하나로 프로그래밍 방식으로 설정할 수 있습니다.

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) – 텍스트 작성 및 이모티콘이 유용한 경우에 사용합니다.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) – 기본 키보드입니다.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) – 전자 메일 주소를 입력할 때 사용합니다.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) – 숫자를 입력할 때 사용합니다.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) – 지정된 [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) 없이 텍스트를 입력할 때 사용합니다.
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) – 전화 번호를 입력할 때 사용합니다.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) – 텍스트를 입력할 때 사용합니다.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) – 파일 경로 및 웹 주소를 입력하는 데 사용합니다.

이렇게 하면 다음과 같이 XAML로 수행할 수 있습니다.

```xaml
<SearchHandler Keyboard="Email" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
SearchHandler searchHandler = new SearchHandler { Keyboard = Keyboard.Email };
```

또한 [`Keyboard`](xref:Xamarin.Forms.Keyboard) 클래스에는 대소문자, 맞춤법 검사 및 제안 동작을 지정하여 키보드를 사용자 지정하는 데 사용할 수 있는 [`Create`](xref:Xamarin.Forms.Keyboard.Create*) 팩터리 메서드가 있습니다. [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) 열거형 값은 사용자 지정된 `Keyboard`가 반환되는 메서드에 대한 인수로 지정됩니다. `KeyboardFlags` 열거는 다음 값을 포함합니다.

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) – 키보드에 추가되는 기능이 없습니다.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) – 입력된 각 문장의 첫 번째 단어의 첫 문자가 자동으로 대문자로 시작함을 나타냅니다.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) - 입력한 텍스트에 대해 맞춤법 검사를 수행할지를 나타냅니다.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) - 입력한 텍스트에 대해 완성된 단어를 제안할지를 나타냅니다.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) – 각 단어의 첫 문자가 자동으로 대문자로 시작함을 나타냅니다.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) – 모든 문자가 자동으로 대문자로 시작함을 나타냅니다.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) – 자동 대소문자 표시가 이루어지지 않음을 나타냅니다.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) – 입력한 텍스트에 대해 맞춤법 검사, 단어 완성 및 문장 첫 글자 대문자 입력이 이루어질 것임을 나타냅니다.

다음 XAML 코드 예제에서는 완성 단어를 제안하고 입력한 모든 글자를 대문자로 시작하도록 기본 [`Keyboard`](xref:Xamarin.Forms.Keyboard)를 사용자 지정하는 방법을 보여 줍니다.

```xaml
<SearchHandler Placeholder="Enter search terms">
    <SearchHandler.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </SearchHandler.Keyboard>
</SearchHandler>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
SearchHandler searchHandler = new SearchHandler { Placeholder = "Enter search terms" };
searchHandler.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

## <a name="searchhandler-reference"></a>SearchHandler 참조

`SearchHandler` 클래스는 해당 모양 및 동작을 제어하는 다음 속성을 정의합니다.

- `Color` 형식의 `BackgroundColor`는 검색 상자 텍스트의 배경색입니다.
- `Color` 형식의 `CancelButtonColor`는 취소 단추의 색입니다.
- [`ImageSource`](xref:Xamarin.Forms.ImageSource) 형식의 `ClearIcon` - 검색 상자의 콘텐츠를 지우기 위해 표시되는 아이콘입니다.
- `string` 형식의 `ClearIconHelpText` - 지우기 아이콘의 액세스할 수 있는 도움말 텍스트입니다.
- `string` 형식의 `ClearIconName` - 화면 읽기 프로그램과 함께 사용할 지우기 아이콘의 이름입니다.
- `ICommand` 형식의 `ClearPlaceholderCommand` - `ClearPlaceholderIcon`을 탭할 때 실행됩니다.
- `object` 형식의 `ClearPlaceholderCommandParameter` - `ClearPlaceholderCommand`에 전달되는 매개 변수입니다.
- `bool` 형식의 `ClearPlaceholderEnabled` - `ClearPlaceholderCommand`를 실행할지 여부를 결정합니다. 기본값은 `true`입니다.
- `string` 형식의 `ClearPlaceholderHelpText` - 지우기 자리 표시자 아이콘의 액세스할 수 있는 도움말 텍스트입니다.
- [`ImageSource`](xref:Xamarin.Forms.ImageSource) 형식의 `ClearPlaceholderIcon` - 검색 상자가 비어 있는 경우 표시되는 지우기 자리 표시자 아이콘입니다.
- `string` 형식의 `ClearPlaceholderName` - 화면 읽기 프로그램과 함께 사용할 지우기 자리 표시자 아이콘의 이름입니다.
- `ICommand` 형식의 `Command` - 검색 쿼리를 확인할 때 실행됩니다.
- `object` 형식의 `CommandParameter` - `Command`에 전달되는 매개 변수입니다.
- `string` 형식의 `DisplayMemberName` - `ItemsSource` 컬렉션의 각 데이터 항목에 대해 표시되는 속성의 이름 또는 경로입니다.
- `FontAttributes` 형식의 `FontAttributes`는 검색 상자 텍스트가 기울임꼴인지 볼드인지를 나타냅니다.
- `string` 형식의 `FontFamily`는 검색 상자 텍스트에 사용되는 글꼴 패밀리입니다.
- `double` 형식의 `FontSize`는 검색 상자 텍스트의 크기입니다.
- `TextAlignment` 형식의 `HorizontalTextAlignment`는 검색 상자 텍스트의 가로 맞춤입니다.
- `SearchHandler`에 현재 입력 포커스가 있는지 여부를 나타내는 `bool` 형식의 `IsFocused`.
- `bool` 형식의 `IsSearchEnabled` - 검색 상자의 사용 가능 상태를 나타냅니다. 기본값은 `true`입니다.
- `IEnumerable` 형식의 `ItemsSource` - 제안 영역에 표시할 항목의 컬렉션을 지정하고 `null`의 기본값을 포함합니다.
- [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 형식의 `ItemTemplate` - 제안 영역에 표시할 항목의 컬렉션에서 각 항목에 적용할 템플릿을 지정합니다.
- `Keyboard` 형식의 `Keyboard`는 `SearchHandler`용 키보드입니다.
- `string` 형식의 `Placeholder` - 검색 상자가 비어 있을 때 표시할 텍스트입니다.
- `Color` 형식의 `PlaceholderColor`는 자리 표시자 검색 상자 텍스트의 색입니다.
- `string` 형식의 `Query` - 검색 상자에 사용자가 입력한 텍스트입니다.
- [`ImageSource`](xref:Xamarin.Forms.ImageSource) 형식의 `QueryIcon` - 사용자에게 검색을 사용할 수 있음을 나타내는 데 사용되는 아이콘입니다.
- `string` 형식의 `QueryIconHelpText` - 쿼리 아이콘의 액세스할 수 있는 도움말 텍스트입니다.
- `string` 형식의 `QueryIconName` - 화면 읽기 프로그램과 함께 사용할 쿼리 아이콘의 이름입니다.
- `SearchBoxVisibility` 형식의 `SearchBoxVisibility` - 검색 상자의 표시 여부입니다. 기본적으로 검색 상자는 표시되고 완전히 확장됩니다.
- `object` 형식의 `SelectedItem` - 검색 결과의 선택된 항목입니다. 이 속성은 읽기 전용이고 `null`의 기본값을 포함합니다.
- `bool` 형식의 `ShowsResults` - 텍스트 입력 시 제안 영역에서 검색 결과를 예상할지 여부를 나타냅니다. 기본값은 `false`입니다.
- `Color` 형식의 `TextColor`는 검색 상자 텍스트의 색입니다.

이 모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되며, 이는 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다.

또한 `SearchHandler` 클래스는 다음 재정의 가능한 메서드를 제공합니다.

- `OnClearPlaceholderClicked` - `ClearPlaceholderIcon`이 탭될 때마다 호출됩니다.
- `OnItemSelected` - 사용자가 검색 결과를 선택할 때마다 호출됩니다.
- `OnFocused` - `SearchHandler`가 입력 포커스를 갖게 되면 호출됩니다.
- `OnQueryChanged` - `Query` 속성이 변경될 때 호출됩니다.
- `OnQueryConfirmed` - 사용자가 검색 상자에서 쿼리를 입력하거나 확인할 때마다 호출됩니다.
- `OnUnfocus` - `SearchHandler`에서 입력 포커스가 사라지면 호출됩니다.

## <a name="related-links"></a>관련 링크

- [Xaminals(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Xamarin.Forms Shell 탐색](navigation.md)
