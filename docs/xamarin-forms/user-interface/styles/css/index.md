---
title: (CSS 스타일 시트)를 사용 하 여 Xamarin.Forms 앱 스타일 지정
description: Xamarin.Forms는 스타일 (CSS 스타일 시트)를 사용 하는 시각적 요소를 지원 합니다.
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: 25410dc24d3cd0f9e31a72844052f8450177e9b4
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67512958"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>(CSS 스타일 시트)를 사용 하 여 Xamarin.Forms 앱 스타일 지정

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)

_Xamarin.Forms는 스타일 (CSS 스타일 시트)를 사용 하는 시각적 요소를 지원 합니다._

CSS를 사용 하 여 스타일 Xamarin.Forms 응용 프로그램을 지정할 수 있습니다. 스타일 시트를 하나 이상의 선택기와 선언이 블록으로 구성 된 각 규칙을 사용 하 여 규칙의 목록을 이루어져 있습니다. 선언 블록을 중괄호로 속성, 콜론 및 값으로 이루어진 각 선언과 선언의 목록을 이루어져 있습니다. 블록의 여러 선언이 있을 경우 세미콜론으로 구분 기호로 삽입 됩니다. 다음 코드 예제에서는 일부 Xamarin.Forms 규격 CSS를 보여 줍니다.

```css
navigationpage {
    -xf-bar-background-color: lightgray;
}

^contentpage {
    background-color: lightgray;
}

#listView {
    background-color: lightgray;
}

stacklayout {
    margin: 20;
}

.mainPageTitle {
    font-style: bold;
    font-size: medium;
}

.mainPageSubtitle {
    margin-top: 15;
}

.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}

listview image {
    height: 60;
    width: 60;
}

stacklayout>image {
    height: 200;
    width: 200;
}
```

Xamarin.Forms에서 CSS 스타일 시트를 구문 분석 되 고 컴파일 시간 대신 런타임에 평가 및 스타일 시트에서 사용 하 여 다시 구문 분석 됩니다.

> [!NOTE]
> 현재, 모든 XAML 스타일을 사용 하 여 사용할 수 있는 스타일의 CSS를 사용 하 여 수행할 수 없습니다. 그러나 XAML 스타일 Xamarin.Forms에서 현재 지원 되지 않는 속성에 대 한 CSS를 보완 하기 위해 사용할 수 있습니다. XAML 스타일에 대 한 자세한 내용은 참조 하세요. [XAML 스타일을 사용 하 여 Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/xaml/index.md)합니다.

합니다 [MonkeyAppCSS](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/) 샘플에서는 CSS를 사용 하 여 간단한 앱 스타일을 지정 하 고 다음 스크린샷과에서 같습니다.

[![CSS 스타일을 사용 하 여 주 페이지 MonkeyApp](css-images/MonkeyAppMainPage.png "CSS 스타일을 사용 하 여 주 페이지 MonkeyApp")](css-images/MonkeyAppMainPage-Large.png#lightbox "CSS 스타일을 사용 하 여 MonkeyApp 주 페이지")

[![CSS 스타일을 사용 하 여 세부 정보 페이지 MonkeyApp](css-images/MonkeyAppDetailPage.png "CSS 스타일을 사용 하 여 세부 정보 페이지 MonkeyApp")](css-images/MonkeyAppDetailPage-Large.png#lightbox "CSS 스타일을 사용 하 여 MonkeyApp 세부 정보 페이지")

## <a name="consuming-a-style-sheet"></a>스타일 시트를 사용합니다.

솔루션에 스타일 시트를 추가 하기 위한 프로세스는 다음과 같습니다.

1. .NET Standard 라이브러리 프로젝트에 빈 CSS 파일을 추가 합니다.
1. CSS 파일의 빌드 동작을 설정 **EmbeddedResource**합니다.

### <a name="loading-a-style-sheet"></a>스타일 시트를 로드합니다.

스타일 시트를 로드 하는 방법의 여러 가지가 있습니다.

### <a name="xaml"></a>XAML

스타일 시트를 로드 하 고 사용 하 여 구문 분석할 수는 [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) 클래스에 추가 되기 전에 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Application ...>
    <Application.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </Application.Resources>
</Application>
```

[ `StyleSheet.Source` ](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) 속성 URI로 시작 하는 경우 스타일 시트 바깥쪽 XAML 파일의 위치를 기준으로 또는 프로젝트 루트를 기준으로 URI로 지정 된 `/`합니다.

> [!WARNING]
> CSS 파일 빌드 작업 설정 하지 않으면 로드 되지 것입니다 **EmbeddedResource**합니다.

또는 스타일 시트를 로드 하 고 수로 구문 분석 합니다 [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) 클래스에 추가 되기 전에 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), 인라인에 `CDATA` 섹션:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet>
            <![CDATA[
            ^contentpage {
                background-color: lightgray;
            }
            ]]>
        </StyleSheet>
    </ContentPage.Resources>
    ...
</ContentPage>
```

리소스 사전에 대 한 자세한 내용은 참조 하세요. [리소스가](~/xamarin-forms/xaml/resource-dictionaries.md)합니다.

### <a name="c"></a>C#

C#, 스타일 시트의 포함 리소스로 로드 하 고 추가할 수는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        this.Resources.Add(StyleSheet.FromAssemblyResource(
            IntrospectionExtensions.GetTypeInfo(typeof(MyPage)).Assembly,
            "MyProject.Assets.styles.css"));
    }
}
```

첫 번째 인수는 `StyleSheet.FromAssemblyResource` 메서드는 스타일 시트를 포함 하는 어셈블리는 두 번째 인수는 `string` 리소스 식별자를 나타내는입니다. 리소스 식별자에서 가져올 수 있습니다 합니다 **속성** 창 CSS 파일을 선택 합니다.

또는에서 스타일 시트를 로드할 수 있습니다는 `StringReader` 에 추가 하 고는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        using (var reader = new StringReader("^contentpage { background-color: lightgray; }"))
        {
            this.Resources.Add(StyleSheet.FromReader(reader));
        }
    }
}
```

인수는 `StyleSheet.FromReader` 메서드는는 `TextReader` 스타일 시트는 읽기입니다.

## <a name="selecting-elements-and-applying-properties"></a>요소를 선택 하 고 속성 적용

CSS 선택기를 사용 하 여 대상 요소 결정 합니다. 스타일 선택기를 일치 하는 정의 순서로 연속적으로 적용 됩니다. 특정 항목에 정의 된 스타일은 항상 마지막으로 적용 됩니다. 지원 되는 선택기에 대 한 자세한 내용은 참조 하세요. [선택기 참조](#selector-reference)합니다.

CSS 속성을 사용 하 여 선택한 요소의 스타일. 각 속성에 가능한 값의 집합 및 요소 그룹에 적용 하는 동안 일부 속성이 모든 형식의 요소에 영향을 줄 수 있습니다. 지원 되는 속성에 대 한 자세한 내용은 참조 하세요. [속성 참조](#property-reference)합니다.

### <a name="selecting-elements-by-type"></a>형식에서 요소 선택

대/소문자 구분 형식으로 시각적 트리의 요소를 선택할 수 `element` 선택기:

```css
stacklayout {
    margin: 20;
}
```

이 선택기는 모든 식별 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 스타일 시트를 사용 하 고 해당 여백 20의 균일 두께를 설정 하는 페이지의 요소입니다.

> [!NOTE]
> `element` 선택기는 지정 된 형식의 하위 클래스를 식별 하지 않습니다.

### <a name="selecting-elements-by-base-class"></a>기본 클래스에서 요소를 선택합니다.

대/소문자 구분을 사용 하 여 기본 클래스에 의해 시각적 트리의 요소를 선택할 수 `^base` 선택기:

```css
^contentpage {
    background-color: lightgray;
}
```

이 선택기는 모든 식별 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 스타일 시트를 사용 하 고 배경과 설정 하는 요소에 색 `lightgray`합니다.

> [!NOTE]
> `^base` 선택기 Xamarin.Forms 관련이 및 CSS 사양에 포함 되지 않습니다.

### <a name="selecting-an-element-by-name"></a>이름으로 요소를 선택합니다.

대/소문자를 사용 하 여 시각적 트리의 개별 요소를 선택할 수 `#id` 선택기:

```css
#listView {
    background-color: lightgray;
}
```

요소를 식별 하는이 선택기입니다 [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) 속성이 `listView`합니다. 그러나 경우 합니다 `StyleId` 속성이 설정 되지 않은, 선택기를 사용 하도록 대체를 `x:Name` 요소입니다. 다음 XAML 예제에 따라서 합니다 `#listView` 선택기 식별 하는 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 입니다 `x:Name` 특성이로 설정 된 `listView`의 배경색을 설정 하 고 `lightgray`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView x:Name="listView" ...>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

### <a name="selecting-elements-with-a-specific-class-attribute"></a>특정 클래스 특성으로 요소를 선택합니다.

대/소문자를 사용 하 여 특정 클래스 특성으로 요소를 선택할 수 있습니다 `.class` 선택기:

```css
.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}
```

CSS 클래스를 설정 하 여 XAML 요소에 할당할 수 있습니다 합니다 [ `StyleClass` ](xref:Xamarin.Forms.NavigableElement.StyleClass) CSS 클래스 이름 요소의 속성입니다. 따라서 다음 XAML 예제에서는 스타일을 정의 하 여는 `.detailPageTitle` 클래스는 첫 번째에 할당 된 [ `Label` ](xref:Xamarin.Forms.Label), 정의한 스타일 동안 합니다 `.detailPageSubtitle` 클래스는 두 번째 할당 된 `Label`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            <Label ... StyleClass="detailPageTitle" />
            <Label ... StyleClass="detailPageSubtitle"/>
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

### <a name="selecting-child-elements"></a>자식 요소를 선택합니다.

대/소문자 구분을 사용 하 여 시각적 트리의 자식 요소를 선택할 수 `element element` 선택기:

```css
listview image {
    height: 60;
    width: 60;
}
```

이 선택기는 식별 된 [ `Image` ](xref:Xamarin.Forms.Image) 자식 요소 [ `ListView` ](xref:Xamarin.Forms.ListView) 요소인 60 해당 높이 너비를 가져오거나 설정 합니다. 다음 XAML 예제에 따라서를 `listview image` 선택기 식별 하는 [ `Image` ](xref:Xamarin.Forms.Image) 의 자식인를 [ `ListView` ](xref:Xamarin.Forms.ListView), 60 해당 높이 너비를 가져오거나 설정 합니다.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView ...>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid>
                            ...
                            <Image ... />
                            ...
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> 합니다 `element element` 선택기는 자식 요소가 필요 하지 않습니다는 _직접_ 부모-자식 요소는 자식에 다른 부모가 있을 수 있습니다. 상위 항목은 지정된 된 첫 번째 요소는 선택이 발생 합니다.

### <a name="selecting-direct-child-elements"></a>직접 자식 요소를 선택합니다.

대/소문자 구분을 사용 하 여 시각적 트리의 자식 요소를 선택할 수 직접 `element>element` 선택기:

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

이 선택기는 식별 된 [ `Image` ](xref:Xamarin.Forms.Image) 요소를 직접 자식의 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 요소를 200으로 해당 높이 너비를 가져오거나 설정 합니다. 다음 XAML 예제에 따라서를 `stacklayout>image` 선택기 식별 하는 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 의 직접 자식이 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), 200으로 해당 높이 너비를 가져오거나 설정 합니다.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            ...
            <Image ... />
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

> [!NOTE]
> 합니다 `element>element` 선택기에서는 자식 요소를 _직접_ 부모의 자식입니다.

## <a name="selector-reference"></a>선택기 참조

다음 CSS 선택기는 Xamarin.Forms에서 지원 됩니다.

|선택기|예제|설명|
|---|---|---|
|`.class`|`.header`|모든 요소를 선택 합니다 `StyleClass` 'header'를 포함 하는 속성입니다. 이 선택기는 대/소문자 구분 임을 유의 합니다.|
|`#id`|`#email`|사용 하 여 요소를 모두 선택 `StyleId` 로 `email`합니다. 하는 경우 `StyleId` 을 설정 하지 않으면 대체 `x:Name`합니다. XAML을 사용 하는 경우 `x:Name` 보다 선호 됩니다 `StyleId`합니다. 이 선택기는 대/소문자 구분 임을 유의 합니다.|
|`*`|`*`|모든 요소를 선택합니다.|
|`element`|`label`|형식의 요소를 모두 선택 `Label`, 하지만 하위 클래스가 없습니다. 이 선택기는 대/소문자 구분 임을 유의 합니다.|
|`^base`|`^contentpage`|사용 하 여 요소를 모두 선택 `ContentPage` 기본 클래스를 포함 하 여 `ContentPage` 자체. 참고가이 선택기는 대/소문자를 구분 하 고 CSS 사양에 포함 되지 않습니다.|
|`element,element`|`label,button`|모두 선택 `Button` 요소와 모든 `Label` 요소입니다. 이 선택기는 대/소문자 구분 임을 유의 합니다.|
|`element element`|`stacklayout label`|모두 선택 `Label` 내부 요소는 `StackLayout`합니다. 이 선택기는 대/소문자 구분 임을 유의 합니다.|
|`element>element`|`stacklayout>label`|모두 선택 `Label` 요소가 `StackLayout` 직접 부모입니다. 이 선택기는 대/소문자 구분 임을 유의 합니다.|
|`element+element`|`label+entry`|모두 선택 `Entry` 뒤에 직접 요소를 `Label`입니다. 이 선택기는 대/소문자 구분 임을 유의 합니다.|
|`element~element`|`label~entry`|모두 선택 `Entry` 요소 앞에 `Label`합니다. 이 선택기는 대/소문자 구분 임을 유의 합니다.|

스타일 선택기를 일치 하는 정의 순서로 연속적으로 적용 됩니다. 특정 항목에 정의 된 스타일은 항상 마지막으로 적용 됩니다.

> [!TIP]
> 선택기 제한 없이 같은 결합할 수 있습니다 `StackLayout>ContentView>label.email`합니다.

다음 선택기를 현재 지원 되지 않습니다.

- `[attribute]`
- `@media` 및 `@supports`
- `:` 및 `::`

> [!NOTE]
> 특수성 및 특이성 재정의가 지원 되지 않습니다.

## <a name="property-reference"></a>속성 참조

Xamarin.Forms를 다음 CSS 속성만 지원 됩니다 (에 **값** 열 유형은 _기울임꼴_반면 문자열 리터럴은 `gray`):

|속성|적용 대상|값|예제|
|---|---|---|---|
|`align-content`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial` |`align-content: space-between;`|
|`align-items`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial` |`align-items: flex-start;`|
|`align-self`|`VisualElement`| `auto` \| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial`|`align-self: flex-end;`|
|`background-color`|`VisualElement`|_색_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_문자열_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`, `ImageButton`|_색_ \| `initial`|`border-color: #9acd32;`|
|`border-radius`|`BoxView`, `Button`, `Frame`, `ImageButton`|_Double_ \| `initial` |`border-radius: 10;`|
|`border-width`|`Button`, `ImageButton`|_Double_ \| `initial` |`border-width: .5;`|
|`color`|`ActivityIndicator`, `BoxView`, `Button`, `CheckBox`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `ProgressBar`, `SearchBar`, `Switch`, `TimePicker`|_색_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`column-gap`|`Grid`|_Double_ \| `initial`|`column-gap: 9;`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`flex-direction`|`FlexLayout`| `column` \| `columnreverse` \| `row` \| `rowreverse` \| `row-reverse` \| `column-reverse` \| `initial`|`flex-direction: column-reverse;`|
|`flex-basis`|`VisualElement`|_부동 소수점_ \| `auto` \| `initial`합니다. 또한 사용 하 여 백분율에는 범위 0%에서 100%를 지정할 수 있습니다는 `%` 로그인 합니다.|`flex-basis: 25%;`|
|`flex-grow`|`VisualElement`|_Float_ \| `initial`|`flex-grow: 1.5;`|
|`flex-shrink`|`VisualElement`|_Float_ \| `initial`|`flex-shrink: 1;`|
|`flex-wrap`|`VisualElement`| `nowrap` \| `wrap` \| `reverse` \| `wrap-reverse` \| `initial`|`flex-wrap: wrap-reverse;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_문자열_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_이중_ \| _namedsize_ \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_Double_ \| `initial` |`min-height: 250;`|
|`justify-content`|`FlexLayout`| `start` \| `center` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial`|`justify-content: flex-end;`|
|`line-height`|`Label`, `Span`|_Double_ \| `initial` |`line-height: 1.8;`|
|`margin`|`View`|_두께_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_두께_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_두께_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_두께_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_두께_ \| `initial` |`margin-bottom: 6;`|
|`max-lines`|`Label`|_Int_ \| `initial`|`max-lines: 2;`|
|`min-height`|`VisualElement`|_Double_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_Double_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_Double_ \| `initial` |`opacity: .3;`|
|`order`|`VisualElement`|_Int_ \| `initial`|`order: -1;`|
|`padding`|`Button`, `ImageButton`, `Layout`, `Page`|_두께_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Button`, `ImageButton`, `Layout`, `Page`|_Double_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Button`, `ImageButton`, `Layout`, `Page`| _Double_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Button`, `ImageButton`, `Layout`, `Page`| _Double_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Button`, `ImageButton`, `Layout`, `Page`| _Double_ \| `initial` |`padding-bottom: 6;`|
|`position`|`FlexLayout`| `relative` \| `absolute` \| `initial`|`position: absolute;`|
|`row-gap`|`Grid`| _Double_ \| `initial`|`row-gap: 12;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`. `left` 및 `right` 오른쪽에서 왼쪽 환경에서 피해 야 합니다.| `text-align: right;`|
|`text-decoration`|`Label`, `Span`|`none` \| `underline` \| `strikethrough` \| `line-through` \| `initial`|`text-decoration: underline, line-through;`|
|`transform`|`VisualElement`| `none`, `rotate`, `rotateX`, `rotateY`, `scale`, `scaleX`, `scaleY`, `translate`, `translateX`, `translateY`, `initial` |`transform: rotate(180), scaleX(2.5);`|
|`transform-origin`|`VisualElement`| _이중_, _double_ \| `initial` |`transform-origin: 7.5, 12.5;`|
|`vertical-align`|`Label`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`|`vertical-align: bottom;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial `|`visibility: hidden;`|
|`width`|`VisualElement`|_Double_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial` 모든 속성에 대 한 유효한 값이입니다. 다른 스타일에서 설정 된 값 (기본값으로 다시 설정)을 지웁니다.

다음 속성을 현재 지원 되지 않습니다.

- `all: initial`.
- 레이아웃 속성 (상자 또는 그리드)입니다.
- 줄임 속성 같은 `font`, 및 `border`합니다.

또한 방법이 없는 `inherit` 값 등 상속은 지원 되지 않습니다. 예를 들어, 설정할 수 없습니다, 따라서 합니다 `font-size` 레이아웃의 속성 모두 예상 합니다 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스 값을 상속할 수에서. 한 가지 예외는 합니다 `direction` 속성의 기본값은의 `inherit`합니다.

### <a name="xamarinforms-specific-properties"></a>Xamarin.Forms 특정 속성

다음 Xamarin.Forms 특정 CSS 속성 에서도 지원 됩니다 (에 **값** 열 유형은 _기울임꼴_반면 문자열 리터럴은 `gray`):

|속성|적용 대상|값|예제|
|---|---|---|---|
|`-xf-placeholder`|`Entry`, `Editor`, `SearchBar`|_따옴표로 묶인된 텍스트_ \| `initial` |`-xf-placeholder: Enter name;`|
|`-xf-placeholder-color`|`Entry`, `Editor`, `SearchBar`|_색_ \| `initial` |`-xf-placeholder-color: green;`|
|`-xf-max-length`|`Entry`, `Editor`|_Int_ \| `initial` |`-xf-max-length: 20;`|
|`-xf-bar-background-color`|`NavigationPage`, `TabbedPage`|_색_ \| `initial` |`-xf-bar-background-color: teal;`|
|`-xf-bar-text-color`|`NavigationPage`, `TabbedPage`|_색_ \| `initial` |`-xf-bar-text-color: gray`|
|`-xf-orientation`|`ScrollView`, `StackLayout`| `horizontal` \| `vertical` \| `both` \| `initial`. `both` 에서만 지원 되는 `ScrollView`합니다. |`-xf-orientation: horizontal;`|
|`-xf-horizontal-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-horizontal-scroll-bar-visibility: never;`|
|`-xf-vertical-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-vertical-scroll-bar-visibility: always;`|
|`-xf-min-track-color`|`Slider`|_색_ \| `initial` |`-xf-min-track-color: yellow;`|
|`-xf-max-track-color`|`Slider`|_색_ \| `initial` |`-xf-max-track-color: red;`|
|`-xf-thumb-color`|`Slider`|_색_ \| `initial` |`-xf-thumb-color: limegreen;`|
|`-xf-spacing`|`StackLayout`|_Double_ \| `initial` |`-xf-spacing: 8;`|

### <a name="xamarinforms-shell-specific-properties"></a>Xamarin.Forms 셸 특정 속성

다음 Xamarin.Forms 셸 특정 CSS 속성 에서도 지원 됩니다 (에 **값** 열 유형은 _기울임꼴_반면 문자열 리터럴은 `gray`):

|속성|적용 대상|값|예제|
|---|---|---|---|
|`-xf-flyout-background`|`Shell`|_색_ \| `initial` |`-xf-flyout-background: red;`|
|`-xf-shell-background`|`Element`|_색_ \| `initial` |`-xf-shell-background: green;`|
|`-xf-shell-disabled`|`Element`|_색_ \| `initial` |`-xf-shell-disabled: blue;`|
|`-xf-shell-foreground`|`Element`|_색_ \| `initial` |`-xf-shell-foreground: yellow;`|
|`-xf-shell-tabbar-background`|`Element`|_색_ \| `initial` |`-xf-shell-tabbar-background: white;`|
|`-xf-shell-tabbar-disabled`|`Element`|_색_ \| `initial` |`-xf-shell-tabbar-disabled: black;`|
|`-xf-shell-tabbar-foreground`|`Element`|_색_ \| `initial` |`-xf-shell-tabbar-foreground: gray;`|
|`-xf-shell-tabbar-title`|`Element`|_색_ \| `initial` |`-xf-shell-tabbar-title: lightgray;`|
|`-xf-shell-tabbar-unselected`|`Element`|_색_ \| `initial` |`-xf-shell-tabbar-unselected: cyan;`|
|`-xf-shell-title`|`Element`|_색_ \| `initial` |`-xf-shell-title: teal;`|
|`-xf-shell-unselected`|`Element`|_색_ \| `initial` |`-xf-shell-unselected: limegreen;`|

### <a name="color"></a>색

다음 `color` 값이 지원 됩니다.

- `X11` [색](https://en.wikipedia.org/wiki/X11_color_names/), CSS 색, 미리 정의 된 색 UWP 및 Xamarin.Forms 색 일치입니다. 이러한 색 값은 대/소문자 구분을 참고 합니다.
- 색의 16 진수: `#rgb`하십시오 `#argb`, `#rrggbb`, `#aarrggbb`
- rgb 색: `rgb(255,0,0)`, `rgb(100%,0%,0%)`합니다. 값은 범위 0-255 또는 0% ~ 100%입니다.
- rgba 색: `rgba(255, 0, 0, 0.8)`, `rgba(100%, 0%, 0%, 0.8)`합니다. 불투명도 값은 0.0-1.0 범위입니다.
- hsl 색: `hsl(120, 100%, 50%)`합니다. H 값은 범위 0-360 s 및 l은에 범위 0-100%입니다.
- hsla 색: `hsla(120, 100%, 50%, .8)`합니다. 불투명도 값은 0.0-1.0 범위입니다.

### <a name="thickness"></a>Thickness

1, 2, 3 또는 4 `thickness` 값에 지원 되는 경우 공백으로 구분 합니다.

- 단일 값을 균일 두께 나타냅니다.
- 두 값을 세로 가로 두께를 나타냅니다.
- 세 가지 값 위쪽 가로 (왼쪽 및 오른쪽)를 차례로 아래쪽 두께 나타냅니다.
- 4 개의 값 맨 위에 다음 오른쪽 차례로 아래쪽, 왼쪽된 두께 나타냅니다.

> [!NOTE]
> CSS `thickness` XAML에서 값이 다르면 [ `Thickness` ](xref:Xamarin.Forms.Thickness) 값입니다. XAML을 두 값의 예를 들어 `Thickness` 4 값을 하는 동안 가로 다음 세로 두께 나타내는 `Thickness` 아래쪽 두께 다음 왼쪽에서 차례로 위쪽, 오른쪽을 나타냅니다. 또한 XAML `Thickness` 값은 쉼표로 구분 합니다.

### <a name="namedsize"></a>NamedSize

대/소문자 구분 다음 `namedsize` 값이 지원 됩니다.

- `default`
- `micro`
- `small`
- `medium`
- `large`

각각의 정확한 의미 `namedsize` 값은 플랫폼에 종속 된 보기에 따라 다릅니다.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>CSS Xamarin.University 사용 하 여 Xamarin.Forms에

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin.Forms 3.0 CSS 비디오**

## <a name="related-links"></a>관련 링크

- [MonkeyAppCSS (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [XAML 스타일을 사용하여 Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/xaml/index.md)
