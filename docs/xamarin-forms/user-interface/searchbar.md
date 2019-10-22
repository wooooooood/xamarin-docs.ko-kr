---
title: Xamarin.ios SearchBar
description: Xamarin.ios SearchBar는 검색을 시작 하는 데 사용 되는 사용자 입력 컨트롤입니다. SearchBar 컨트롤은 자리 표시자 텍스트, 쿼리 입력, 실행 및 취소를 지원 합니다. 이 문서에서는 XAML 및 코드에서 SearchBar를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetId: F5EFEA72-CB23-4DD6-9545-D9BB755AF3CB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/12/2019
ms.openlocfilehash: 4deeab1b2792675033372ccfe2bf343c08794955
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696425"
---
# <a name="xamarinforms-searchbar"></a>Xamarin.ios SearchBar

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)

Xamarin.ios [`SearchBar`](xref:Xamarin.Forms.SearchBar) 는 검색을 시작 하는 데 사용 되는 사용자 입력 컨트롤입니다. @No__t_0 컨트롤은 자리 표시자 텍스트, 쿼리 입력, 검색 실행 및 취소를 지원 합니다. 다음 스크린샷은 `ListView`에 결과가 표시 되는 `SearchBar` 쿼리를 보여 줍니다.

[![IOS 및 Android에 대 한 SearchBar의 스크린샷](searchbar-images/device-searchbars-cropped.png "IOS 및 Android의 SearchBar")](searchbar-images/device-searchbars.png#lightbox "IOS 및 Android의 SearchBar")

@No__t_0 클래스는 다음 속성을 정의 합니다.

* [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) 은 취소 단추의 색을 정의 하는 `Color`입니다.
* `double` 형식의 `CharacterSpacing`은 `SearchBar` 텍스트 문자 사이의 간격입니다.
* [`FontAttributes`](xref:Xamarin.Forms.SearchBar.FontAttributes) 은 `SearchBar` 글꼴이 굵게, 기울임꼴로 또는 둘 다 인지 여부를 결정 하는 `FontAttributes` 열거형 값입니다.
* [`FontFamily`](xref:Xamarin.Forms.SearchBar.FontFamily) 은 `SearchBar`에서 사용 하는 글꼴 패밀리를 결정 하는 `string`입니다.
* [`FontSize`](xref:Xamarin.Forms.SearchBar.FontSize) 은 `NamedSize` 열거형 값 이거나 플랫폼 전체의 특정 글꼴 크기를 나타내는 `double` 값일 수 있습니다.
* [`HorizontalTextAlignment`](xref:Xamarin.Forms.SearchBar.HorizontalTextAlignment) 는 쿼리 텍스트의 가로 맞춤을 정의 하는 `TextAlignment` 열거형 값입니다.
* `VerticalTextAlignment`는 쿼리 텍스트의 세로 맞춤을 정의 하는 `TextAlignment` 열거형 값입니다.
* [`Placeholder`](xref:Xamarin.Forms.SearchBar.Placeholder) 는 "Search ..."와 같은 자리 표시자 텍스트를 정의 하는 `string`입니다.
* [`PlaceholderColor`](xref:Xamarin.Forms.SearchBar.PlaceholderColor) 은 자리 표시자 텍스트의 색을 정의 하는 `Color`입니다.
* [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) 는 핑거 탭 또는 클릭과 같은 사용자 동작을 viewmodel에 정의 된 명령에 바인딩할 수 있는 `ICommand`입니다.
* [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) 는 `SearchCommand`에 전달 되어야 하는 매개 변수를 지정 하는 `object`입니다.
* [`Text`](xref:Xamarin.Forms.SearchBar.Text) 은 `SearchBar`의 쿼리 텍스트를 포함 하는 `string`입니다.
* [`TextColor`](xref:Xamarin.Forms.SearchBar.TextColor) 은 쿼리 텍스트 색을 정의 하는 `Color`입니다.

이러한 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원 됩니다. 즉, `SearchBar` 사용자 지정 하 고 데이터 바인딩의 대상이 될 수 있습니다. @No__t_0의 글꼴 속성을 지정 하는 것은 다른 [Xamarin.ios 텍스트 컨트롤](~/xamarin-forms/user-interface/text/index.md)에서 텍스트를 사용자 지정 하는 것과 일치 합니다. 자세한 내용은 [xamarin.ios의 글꼴](~/xamarin-forms/user-interface/text/fonts.md)을 참조 하세요.

## <a name="create-a-searchbar"></a>SearchBar 만들기

@No__t_0는 XAML에서 인스턴스화할 수 있습니다. 쿼리 입력 상자에서 힌트 텍스트를 정의 하기 위해 선택적 `Placeholder` 속성을 설정할 수 있습니다. @No__t_0의 기본값은 빈 문자열 이므로 설정 되지 않은 경우에는 자리 표시 자가 표시 되지 않습니다. 다음 예제에서는 선택적 `Placeholder` 속성 집합을 사용 하 여 XAML에서 `SearchBar`를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<SearchBar Placeholder="Search items..." />
```

@No__t_0 코드에서 만들 수도 있습니다.

```csharp
SearchBar searchBar = new SearchBar{ Placeholder = "Search items..." };
```

### <a name="searchbar-appearance-properties"></a>SearchBar 모양 속성

@No__t_0 컨트롤은 컨트롤의 모양을 사용자 지정 하는 다양 한 속성을 정의 합니다. 다음 예제에서는 여러 속성을 지정 하 여 XAML에서 `SearchBar`를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<SearchBar Placeholder="Search items..."
           CancelButtonColor="Orange"
           PlaceholderColor="Orange"
           TextColor="Orange"
           HorizontalTextAlignment="Center"
           FontSize="Medium"
           FontAttributes="Italic" />
```

이러한 속성은 코드에서 `SearchBar` 개체를 만들 때에도 지정할 수 있습니다.

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

다음 스크린샷에서는 결과 `SearchBar` 컨트롤을 보여 줍니다.

[![IOS 및 Android에서 사용자 지정 된 SearchBar의 스크린샷](searchbar-images/device-searchbars-styled-cropped.png "IOS 및 Android의 사용자 지정 된 SearchBar")](searchbar-images/device-searchbars-styled.png#lightbox "IOS 및 Android의 사용자 지정 된 SearchBar")

## <a name="perform-a-search-with-event-handlers"></a>이벤트 처리기를 사용 하 여 검색 수행

다음 이벤트 중 하나에 이벤트 처리기를 연결 하 여 `SearchBar` 컨트롤을 사용 하 여 검색을 실행할 수 있습니다.

* [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) 은 사용자가 검색 단추를 클릭 하거나 enter 키를 누를 때 호출 됩니다.
* [`TextChanged`](xref:Xamarin.Forms.SearchBar.TextChanged) 는 쿼리 상자의 텍스트가 변경 될 때마다 호출 됩니다.

다음 예제에서는 XAML의 `TextChanged` 이벤트에 연결 된 이벤트 처리기를 보여 주고 `ListView`를 사용 하 여 검색 결과를 표시 합니다.

```xaml
<SearchBar TextChanged="OnTextChanged" />
<ListView x:Name="searchResults" >
```

이벤트 처리기를 코드에서 만든 `SearchBar`에 연결할 수도 있습니다.

```csharp
SearchBar searchBar = new SearchBar {/*...*/};
searchBar.TextChanged += OnTextChanged;
```

@No__t_1 XAML 또는 코드를 통해 생성 되는지에 관계 없이 코드 숨김이 파일의 `TextChanged` 이벤트 처리기는 동일 합니다.

```csharp
void OnTextChanged(object sender, EventArgs e)
{
    SearchBar searchBar = (SearchBar)sender;
    searchResults.ItemsSource = DataService.GetSearchResults(searchBar.Text);
}
```

앞의 예제에서는 쿼리와 일치 하는 항목을 반환할 수 있는 `GetSearchResults` 메서드를 사용 하 여 `DataService` 클래스가 있음을 의미 합니다. @No__t_0 컨트롤의 `Text` 속성 값이 `GetSearchResults` 메서드에 전달 되 고 결과는 `ListView` 컨트롤의 `ItemsSource` 속성을 업데이트 하는 데 사용 됩니다. 전반적인 효과는 검색 결과가 `ListView` 컨트롤에 표시 되는 것입니다.

샘플 응용 프로그램은 검색 기능을 테스트 하는 데 사용할 수 있는 `DataService` 클래스 구현을 제공 합니다.

## <a name="perform-a-search-using-a-viewmodel"></a>Viewmodel을 사용 하 여 검색 수행

@No__t_0 및 `SearchCommandParameter` 속성을 `ICommand` 구현에 바인딩하여 이벤트 처리기 없이 검색을 실행할 수 있습니다. 샘플 프로젝트는 MVVM (모델-뷰-ViewModel) 패턴을 사용 하 여 이러한 구현을 보여 줍니다. MVVM를 사용한 데이터 바인딩에 대 한 자세한 내용은 [MVVM를 사용 하 여 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)을 참조 하세요.

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
> Viewmodel은 검색을 수행할 수 있는 `DataService` 클래스의 존재를 가정 합니다. 예제 데이터를 비롯 한 `DataService` 클래스는 샘플 응용 프로그램에서 사용할 수 있습니다.

다음 XAML은 검색 결과를 표시 하는 `ListView` 컨트롤을 사용 하 여 `SearchBar`를 예제 viewmodel에 바인딩하는 방법을 보여 줍니다.

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

이 예제에서는 `BindingContext`를 `SearchViewModel` 클래스의 인스턴스로 설정 합니다. 이 메서드는 viewmodel의 `PerformSearch` `ICommand`에 `SearchCommand` 속성을 바인딩하고 `SearchBar` `Text` 속성을 `SearchCommandParameter` 속성에 바인딩합니다. @No__t_0 속성은 viewmodel의 `SearchResults` 속성에 바인딩됩니다.

@No__t_0 인터페이스 및 바인딩에 대 한 자세한 내용은 [xamarin.ios 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md) 및 [ICommand 인터페이스](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

* [SearchBar 데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)
* [Xamarin.ios 텍스트 컨트롤](~/xamarin-forms/user-interface/text/index.md)
* [Xamarin.ios의 글꼴](~/xamarin-forms/user-interface/text/fonts.md)
* [Xamarin Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
