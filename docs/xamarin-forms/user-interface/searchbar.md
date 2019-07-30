---
title: Xamarin.ios SearchBar
description: Xamarin.ios SearchBar는 검색을 시작 하는 데 사용 되는 사용자 입력 컨트롤입니다. SearchBar 컨트롤은 자리 표시자 텍스트, 쿼리 입력, 실행 및 취소를 지원 합니다. 이 문서에서는 XAML 및 코드에서 SearchBar를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetId: F5EFEA72-CB23-4DD6-9545-D9BB755AF3CB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/12/2019
ms.openlocfilehash: 66d947c8b80546e68c68915b960587a48bd2448d
ms.sourcegitcommit: 25be5acf979f6b18b6d0e64392c9ab307259c032
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68610493"
---
# <a name="xamarinforms-searchbar"></a>Xamarin.ios SearchBar

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SearchBar)

Xamarin.ios는 검색을 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 시작 하는 데 사용 되는 사용자 입력 컨트롤입니다. 컨트롤 `SearchBar` 은 자리 표시자 텍스트, 쿼리 입력, 검색 실행 및 취소를 지원 합니다. 다음 스크린샷에서는 `ListView`에 결과가 `SearchBar` 표시 된 쿼리를 보여 줍니다.

Ios 및 Android의 [ ![Ios 및 android searchbar에 대 한 Searchbar의 스크린샷](searchbar-images/device-searchbars-cropped.png "") ] (searchbar-images/device-searchbars.png#lightbox "IOS 및 Android의 Searchbar")

는 `SearchBar` 다음 속성을 정의 합니다.

* [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor)는 취소 단추의 색을 정의 하는입니다.`Color`
* [`FontAttributes`](xref:Xamarin.Forms.SearchBar.FontAttributes)글꼴이 굵게, 기울임꼴로 또는 둘 다 `FontAttributes` 인지 여부를 결정 하는 열거형 `SearchBar` 값입니다.
* [`FontFamily`](xref:Xamarin.Forms.SearchBar.FontFamily)는에서 사용 `SearchBar`하는 글꼴 패밀리를 결정 하는입니다.`string`
* [`FontSize`](xref:Xamarin.Forms.SearchBar.FontSize)는 `NamedSize` 열거형 값 `double` 이거나 여러 플랫폼에서 특정 글꼴 크기를 나타내는 값일 수 있습니다.
* [`HorizontalTextAlignment`](xref:Xamarin.Forms.SearchBar.HorizontalTextAlignment)쿼리 텍스트의 가로 맞춤을 정의 하는 열거형값입니다.`TextAlignment`
* [`Placeholder`](xref:Xamarin.Forms.SearchBar.Placeholder)는 "Search ..."와 같은 자리 표시자 텍스트를 정의 하는입니다.`string`
* [`PlaceholderColor`](xref:Xamarin.Forms.SearchBar.PlaceholderColor)자리 표시자 텍스트의 색을 정의 하는입니다.`Color`
* [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand)는 핑거 탭 또는 클릭과 같은 사용자 동작을 viewmodel에 정의 된 명령에 바인딩할 수 있는입니다.`ICommand`
* [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter)는에 전달 `SearchCommand`되어야 하는 매개 변수를 지정 하는입니다.`object`
* [`Text`](xref:Xamarin.Forms.SearchBar.Text)는의 쿼리 텍스트 `SearchBar`를 포함하는입니다.`string`
* [`TextColor`](xref:Xamarin.Forms.SearchBar.TextColor)는 쿼리 텍스트 색을 정의 하는입니다.`Color`

이러한 속성은 개체에 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 의해 지원 됩니다. 즉 `SearchBar` ,를 사용자 지정 하 고 데이터 바인딩의 대상으로 지정할 수 있습니다. 에서 `SearchBar` 글꼴 속성을 지정 하는 것은 다른 [xamarin.ios 텍스트 컨트롤](~/xamarin-forms/user-interface/text/index.md)에서 텍스트를 사용자 지정 하는 것과 일치 합니다. 자세한 내용은 [xamarin.ios의 글꼴](~/xamarin-forms/user-interface/text/fonts.md)을 참조 하세요.

## <a name="create-a-searchbar"></a>SearchBar 만들기

는 `SearchBar` XAML에서 인스턴스화될 수 있습니다. 쿼리 입력 `Placeholder` 상자에 힌트 텍스트를 정의 하기 위해 선택적 속성을 설정할 수 있습니다. 의 기본값은 빈 문자열 `Placeholder` 이므로 설정 되지 않은 경우에는 자리 표시 자가 표시 되지 않습니다. 다음 예제에서는 선택적 `SearchBar` `Placeholder` 속성 집합을 사용 하 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<SearchBar Placeholder="Search items..." />
```

코드 `SearchBar` 에서를 만들 수도 있습니다.

```csharp
SearchBar searchBar = new SearchBar{ Placeholder = "Search items..." };
```

### <a name="searchbar-appearance-properties"></a>SearchBar 모양 속성

컨트롤 `SearchBar` 은 컨트롤의 모양을 사용자 지정 하는 다양 한 속성을 정의 합니다. 다음 예제에서는 여러 속성을 지정 하 `SearchBar` 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<SearchBar Placeholder="Search items..."
           CancelButtonColor="Orange"
           PlaceholderColor="Orange"
           TextColor="Orange"
           HorizontalTextAlignment="Center"
           FontSize="Medium"
           FontAttributes="Italic" />
```

코드에서을 `SearchBar` 만들 때도 이러한 속성을 지정할 수 있습니다.

```csharp
SearchBar searchBar = new SearchBar
{
    Placeholder = "Search items...",
    PlaceholderColor = Color.Orange,
    TextColor = Color.Orange,
    HorizontalTextAlignment = TextAlignment.Center,
    FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(SearchBar)),
    FontAttributes = FontAttributes.Italic
};
```

다음 스크린샷은 결과 `SearchBar`를 보여 줍니다.

Ios 및 android에서 사용자 지정 [ ![된 Searchbar의 스크린샷 및 android](searchbar-images/device-searchbars-styled-cropped.png "사용자 지정 searchbar") ] (searchbar-images/device-searchbars-styled.png#lightbox "IOS 및 Android의 사용자 지정 된 SearchBar")

## <a name="perform-a-search-with-event-handlers"></a>이벤트 처리기를 사용 하 여 검색 수행

다음 이벤트 중 하나에 이벤트 처리기 `SearchBar` 를 연결 하 여 컨트롤을 사용 하 여 검색을 실행할 수 있습니다.

* [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)사용자가 검색 단추를 클릭 하거나 enter 키를 누를 때 호출 됩니다.
* [`TextChanged`](xref:Xamarin.Forms.SearchBar.TextChanged)쿼리 상자의 텍스트가 변경 될 때마다이 호출 됩니다.

다음 예제에서는 XAML의 `TextChanged` 이벤트에 연결 된 이벤트 처리기를 보여 주고를 `ListView` 사용 하 여 검색 결과를 표시 합니다.

```xaml
<SearchBar TextChanged="OnTextChanged" />
<ListView x:Name="searchResults" >
```

이벤트 처리기를 코드에서 `SearchBar` 만든에 연결할 수도 있습니다.

```csharp
SearchBar searchBar = new SearchBar {/*...*/};
searchBar.TextChanged += OnTextChanged;
```

코드 숨김이 XAML 또는 코드를 통해 생성 `SearchBar` 되었는지 여부에 관계 없이 코드 숨김이 파일의 이벤트처리기는동일합니다.`TextChanged`

```csharp
void OnTextChanged(object sender, EventArgs e)
{
    SearchBar searchBar = (SearchBar)sender;
    searchResults.ItemsSource = DataService.GetSearchResults(searchBar.Text);
}
```

이전 예제에서는 쿼리와 일치 하는 항목 `DataService` 을 반환할 수 `GetSearchResults` 있는 메서드가 포함 된 클래스가 있음을 의미 합니다. `ItemsSource` `ListView` 컨트롤의 `Text` 속성`GetSearchResults` 값이 메서드에 전달 되 고 결과는 컨트롤의 속성을 업데이트 하는 데 사용 됩니다. `SearchBar` 전반적인 효과는 검색 결과가 `ListView` 컨트롤에 표시 되는 것입니다.

샘플 응용 프로그램은 검색 `DataService` 기능을 테스트 하는 데 사용할 수 있는 클래스 구현을 제공 합니다.

## <a name="perform-a-search-using-a-viewmodel"></a>Viewmodel을 사용 하 여 검색 수행

`SearchCommand` `ICommand` 및 속성을구현에바인딩하여이벤트처리기없이검색을실행할수있습니다.`SearchCommandParameter` 샘플 프로젝트는 MVVM (모델-뷰-ViewModel) 패턴을 사용 하 여 이러한 구현을 보여 줍니다. MVVM를 사용한 데이터 바인딩에 대 한 자세한 내용은 [MVVM를 사용 하 여 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)을 참조 하세요.

샘플 응용 프로그램의 viewmodel에는 다음 코드가 포함 되어 있습니다.

```csharp
public class SearchViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void NotifyPropertyChanged([CallerMemberName] string propertyName = "")
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

    public ICommand PerformSearch => new Command<string>((string query) =>
    {
        SearchResults = DataService.GetSearchResults(query);
    });

    private List<string> searchResults = DataService.Fruits;
    public List<string> SearchResults
    {
        get
        {
            return searchResults;
        }
        set
        {
            searchResults = value;
            NotifyPropertyChanged();
        }
    }
}
```

> [!NOTE]
> Viewmodel은 검색을 수행할 수 있는 `DataService` 클래스의 존재를 가정 합니다. 예제 데이터를 포함 하는 클래스는샘플응용프로그램에서사용할수있습니다.`DataService`

다음 XAML은 검색 결과를 표시 하 `SearchBar` 는 `ListView` 컨트롤을 사용 하 여를 예제 viewmodel에 바인딩하는 방법을 보여 줍니다.

```xaml
<ContentPage ...>
    <ContentPage.BindingContext>
        <viewmodels:SearchViewModel />
    </ContentPage.BindingContext>
    <StackLayout ...>
        <SearchBar x:Name="searchBar"
                   ...
                   SearchCommand="{Binding PerformSearch}"
                   SearchCommandParameter="{Binding Text, Source={x:Reference searchBar}}"/>
        <ListView x:Name="searchResults"
                  ...
                  ItemsSource="{Binding SearchResults} />
    </StackLayout>
</ContentPage>
```

이 예제에서는을 `BindingContext` `SearchViewModel` 클래스의 인스턴스로 설정 합니다. `SearchCommand` `Text` `SearchBar` Viewmodel의에 속성 `PerformSearch` 을바인딩하고속성을속성에바인딩합니다.`SearchCommandParameter` `ICommand` 속성은 viewmodel의 `SearchResults` 속성에 바인딩됩니다. `ListView.ItemsSource`

`ICommand` 인터페이스 및 바인딩에 대 한 자세한 내용은 [xamarin.ios 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md) 및 [ICommand 인터페이스](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

* [SearchBar 데모](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SearchBar)
* [Xamarin.ios 텍스트 컨트롤](~/xamarin-forms/user-interface/text/index.md)
* [Xamarin.ios의 글꼴](~/xamarin-forms/user-interface/text/fonts.md)
* [Xamarin Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
