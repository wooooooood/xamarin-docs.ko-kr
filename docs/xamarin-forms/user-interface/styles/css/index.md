---
title: CSS(CSS 스타일시트)를 사용하여 Xamarin.Forms 앱 스타일 지정
description: Xamarin.ios는 CSS (CSS 스타일시트)를 사용 하 여 시각적 요소 스타일 지정을 지원 합니다.
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 09/19/2019
ms.openlocfilehash: fdee070021b22f82cb69571f0fa2f396831b14e6
ms.sourcegitcommit: 6781967baeed4fe2c58f070476e7c21d01c25c30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74052798"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>CSS 스타일시트를 사용 하 여 Xamarin Forms 앱 스타일 지정 (CSS)

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)

_Xamarin.ios는 CSS (CSS 스타일시트)를 사용 하 여 시각적 요소 스타일 지정을 지원 합니다._

CSS를 사용 하 여 Xamarin Forms 응용 프로그램에 스타일을 지정할 수 있습니다. 스타일 시트는 하나 이상의 선택기와 선언 블록으로 구성 된 규칙의 목록으로 구성 됩니다. 선언 블록은 중괄호의 선언 목록으로 구성 되며 각 선언에는 속성, 콜론 및 값으로 구성 됩니다. 블록에 선언이 여러 개 있으면 세미콜론 (;)이 구분 기호로 삽입 됩니다. 다음 코드 예제에서는 몇 가지 Xamarin.ios 규격 CSS를 보여 줍니다.

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

Xamarin.ios에서 CSS 스타일 시트는 컴파일 시간이 아니라 런타임에 구문 분석 되 고 계산 되며 스타일 시트는 사용 시 다시 구문 분석 됩니다.

> [!NOTE]
> 현재 XAML 스타일을 사용 하 여 가능한 모든 스타일은 CSS로 수행할 수 없습니다. 그러나 XAML 스타일을 사용 하 여 현재 Xamarin.ios에서 지원 하지 않는 속성에 대해 CSS를 보완할 수 있습니다. XAML 스타일에 대한 자세한 내용은 [XAML 스타일을 사용하여 Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/xaml/index.md)을 참조하세요.

[Monkeyappcss](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss) 샘플에서는 CSS를 사용 하 여 간단한 응용 프로그램의 스타일을 보여 주고 다음 스크린샷에 표시 됩니다.

[![CSS 스타일을 사용 하는 MonkeyApp 기본 페이지](css-images/MonkeyAppMainPage.png "CSS 스타일을 사용 하는 MonkeyApp 기본 페이지")](css-images/MonkeyAppMainPage-Large.png#lightbox "CSS 스타일을 사용 하는 MonkeyApp 기본 페이지")

[![CSS 스타일을 사용 하는 MonkeyApp 세부 정보 페이지](css-images/MonkeyAppDetailPage.png "CSS 스타일을 사용 하는 MonkeyApp 세부 정보 페이지")](css-images/MonkeyAppDetailPage-Large.png#lightbox "CSS 스타일을 사용 하는 MonkeyApp 세부 정보 페이지")

## <a name="consuming-a-style-sheet"></a>스타일 시트 사용

스타일 시트를 솔루션에 추가 하는 프로세스는 다음과 같습니다.

1. .NET Standard library 프로젝트에 빈 CSS 파일을 추가 합니다.
1. CSS 파일의 빌드 작업을 **EmbeddedResource**로 설정 합니다.

### <a name="loading-a-style-sheet"></a>스타일 시트 로드

스타일 시트를 로드 하는 데 사용할 수 있는 여러 가지 방법이 있습니다.

### <a name="xaml"></a>XAML

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 추가 되기 전에 [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet) 클래스를 사용 하 여 스타일 시트를 로드 하 고 구문 분석할 수 있습니다.

```xaml
<Application ...>
    <Application.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </Application.Resources>
</Application>
```

[`StyleSheet.Source`](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) 속성은 바깥쪽 XAML 파일의 위치에 상대적인 uri로 스타일 시트를 지정 하거나 uri가 `/`시작 하는 경우 프로젝트 루트에 상대적인 uri로 지정 합니다.

> [!WARNING]
> 빌드 작업이 **EmbeddedResource**으로 설정 되지 않은 경우 CSS 파일을 로드 하지 못합니다.

또는 [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet) 클래스를 사용 하 여 스타일 시트를 로드 하 고 구문 분석 하 여 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 추가 하기 전에 `CDATA` 섹션에서 인라인 만들 수 있습니다.

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

리소스 사전에 대 한 자세한 내용은 [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)을 참조 하세요.

### <a name="c"></a>C\#

에서는 C#`StringReader`에서 스타일 시트를 로드 하 여 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 추가할 수 있습니다.

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

`StyleSheet.FromReader` 메서드에 대 한 인수는 스타일 시트를 읽은 `TextReader`입니다.

## <a name="selecting-elements-and-applying-properties"></a>요소 선택 및 속성 적용

CSS는 선택기를 사용 하 여 대상으로 지정할 요소를 결정 합니다. 일치 하는 선택기를 사용 하는 스타일은 정의 순서 대로 연속적으로 적용 됩니다. 특정 항목에 대해 정의 된 스타일은 항상 마지막에 적용 됩니다. 지원 되는 선택기에 대 한 자세한 내용은 [선택기 참조](#selector-reference)를 참조 하세요.

CSS는 속성을 사용 하 여 선택한 요소의 스타일을 만듭니다. 각 속성에는 가능한 값 집합이 있고 일부 속성은 모든 요소 형식에 영향을 줄 수 있으며, 다른 속성은 요소 그룹에 적용 됩니다. 지원 되는 속성에 대 한 자세한 내용은 [속성 참조](#property-reference)를 참조 하세요.

### <a name="selecting-elements-by-type"></a>유형별 요소 선택

시각적 트리의 요소는 대/소문자를 구분 하지 않는 `element` 선택기를 사용 하 여 형식으로 선택할 수 있습니다.

```css
stacklayout {
    margin: 20;
}
```

이 선택기는 스타일 시트를 사용 하는 페이지의 모든 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 요소를 식별 하 고 해당 여백을 균일 두께 20으로 설정 합니다.

> [!NOTE]
> `element` 선택기는 지정 된 형식의 서브 클래스를 식별 하지 않습니다.

### <a name="selecting-elements-by-base-class"></a>기본 클래스에서 요소 선택

시각적 트리의 요소는 대/소문자를 구분 하지 않는 `^base` 선택기를 사용 하 여 기본 클래스에서 선택할 수 있습니다.

```css
^contentpage {
    background-color: lightgray;
}
```

이 선택기는 스타일 시트를 사용 하는 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 요소를 식별 하 고 배경색을 `lightgray`로 설정 합니다.

> [!NOTE]
> `^base` 선택기는 Xamarin.ios와 관련 되며 CSS 사양의 일부가 아닙니다.

### <a name="selecting-an-element-by-name"></a>이름으로 요소 선택

시각적 트리의 개별 요소는 대/소문자 구분 `#id` 선택기를 사용 하 여 선택할 수 있습니다.

```css
#listView {
    background-color: lightgray;
}
```

이 선택기는 [`StyleId`](xref:Xamarin.Forms.Element.StyleId) 속성이 `listView`로 설정 된 요소를 식별 합니다. 그러나 `StyleId` 속성이 설정 되지 않은 경우 선택기는 요소의 `x:Name`를 사용 하 여 대체 됩니다. 따라서 다음 XAML 예제에서 `#listView` 선택기는 `x:Name` 특성이 `listView`로 설정 된 [`ListView`](xref:Xamarin.Forms.ListView) 를 식별 하 고 배경색을 `lightgray`로 설정 합니다.

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

### <a name="selecting-elements-with-a-specific-class-attribute"></a>특정 클래스 특성을 가진 요소 선택

대/소문자를 구분 하는 `.class` 선택기를 사용 하 여 특정 클래스 특성을 가진 요소를 선택할 수 있습니다.

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

요소의 [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) 속성을 css 클래스 이름으로 설정 하 여 css 클래스를 XAML 요소에 할당할 수 있습니다. 따라서 다음 XAML 예제에서는 `.detailPageTitle` 클래스에 의해 정의 된 스타일이 첫 번째 [`Label`](xref:Xamarin.Forms.Label)에 할당 되는 반면 `.detailPageSubtitle` 클래스에서 정의한 스타일은 두 번째 `Label`에 할당 됩니다.

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

### <a name="selecting-child-elements"></a>자식 요소 선택

대/소문자를 구분 하지 않는 `element element` 선택기를 사용 하 여 시각적 트리의 자식 요소를 선택할 수 있습니다.

```css
listview image {
    height: 60;
    width: 60;
}
```

이 선택기는 [`ListView`](xref:Xamarin.Forms.ListView) 요소의 자식인 [`Image`](xref:Xamarin.Forms.Image) 요소를 식별 하 고 높이와 너비를 60으로 설정 합니다. 따라서 다음 XAML 예제에서 `listview image` 선택기는 [`ListView`](xref:Xamarin.Forms.ListView)의 자식인 [`Image`](xref:Xamarin.Forms.Image) 를 식별 하 고 높이와 너비를 60으로 설정 합니다.

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
> `element element` 선택기에는 자식 요소가 부모의 _직계_ 자식일 필요가 없습니다. 자식 요소가 다른 부모를 가질 수 있습니다. 상위 항목이 지정 된 첫 번째 요소인 경우 선택이 발생 합니다.

### <a name="selecting-direct-child-elements"></a>직계 자식 요소 선택

시각적 트리의 직접 자식 요소는 대/소문자를 구분 하지 않는 `element>element` 선택기에서 선택할 수 있습니다.

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

이 선택기는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 요소의 직계 자식인 [`Image`](xref:Xamarin.Forms.Image) 요소를 식별 하 고 높이와 너비를 200으로 설정 합니다. 따라서 다음 XAML 예제에서 `stacklayout>image` 선택기는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)의 직계 자식인 [`Image`](xref:Xamarin.Forms.Image) 를 식별 하 고 높이와 너비를 200으로 설정 합니다.

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
> `element>element` 선택기를 사용 하려면 자식 요소가 부모의 _직계_ 자식 이어야 합니다.

## <a name="selector-reference"></a>선택기 참조

다음 CSS 선택기는 Xamarin.ios에서 지원 됩니다.

|선택기|예제|설명|
|---|---|---|
|`.class`|`.header`|' Header '를 포함 하는 `StyleClass` 속성을 사용 하 여 모든 요소를 선택 합니다. 이 선택기는 대/소문자를 구분 합니다.|
|`#id`|`#email`|`email``StyleId` 설정 된 모든 요소를 선택 합니다. `StyleId` 설정 되지 않은 경우 `x:Name`로 대체 합니다. XAML을 사용 하는 경우 `x:Name` `StyleId` 보다 우선적으로 사용 됩니다. 이 선택기는 대/소문자를 구분 합니다.|
|`*`|`*`|모든 요소를 선택 합니다.|
|`element`|`label`|`Label`형식의 모든 요소를 선택 하지만 서브 클래스는 선택 하지 않습니다. 이 선택기는 대/소문자를 구분 하지 않습니다.|
|`^base`|`^contentpage`|`ContentPage`를 포함 하 여 `ContentPage`를 기본 클래스로 사용 하는 모든 요소를 선택 합니다. 이 선택기는 대/소문자를 구분 하지 않으며 CSS 사양의 일부가 아닙니다.|
|`element,element`|`label,button`|모든 `Button` 요소 및 `Label` 요소를 모두 선택 합니다. 이 선택기는 대/소문자를 구분 하지 않습니다.|
|`element element`|`stacklayout label`|`StackLayout`내에서 `Label` 요소를 모두 선택 합니다. 이 선택기는 대/소문자를 구분 하지 않습니다.|
|`element>element`|`stacklayout>label`|`StackLayout`가 있는 모든 `Label` 요소를 직접 부모로 선택 합니다. 이 선택기는 대/소문자를 구분 하지 않습니다.|
|`element+element`|`label+entry`|`Label`바로 뒤의 모든 `Entry` 요소를 선택 합니다. 이 선택기는 대/소문자를 구분 하지 않습니다.|
|`element~element`|`label~entry`|`Label`앞에 있는 모든 `Entry` 요소를 선택 합니다. 이 선택기는 대/소문자를 구분 하지 않습니다.|

일치 하는 선택기를 사용 하는 스타일은 정의 순서 대로 연속적으로 적용 됩니다. 특정 항목에 대해 정의 된 스타일은 항상 마지막에 적용 됩니다.

> [!TIP]
> 선택기는 `StackLayout>ContentView>label.email`와 같은 제한 없이 결합 될 수 있습니다.

현재 지원 되지 않는 선택기는 다음과 같습니다.

- `[attribute]`
- `@media` 및 `@supports`
- `:` 및 `::`

> [!NOTE]
> 특이성 및 특이성 재정의는 지원 되지 않습니다.

## <a name="property-reference"></a>속성 참조

다음 CSS 속성은 Xamarin.ios에서 지원 됩니다 ( **값** 열에서 형식은 _기울임꼴_, 문자열 리터럴은 `gray`).

|속성|적용 대상|값|예제|
|---|---|---|---|
|`align-content`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| |`align-content: space-between;`|
|`align-items`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| 0 1 2 |`align-items: flex-start;`|
|`align-self`|`VisualElement`| `auto` \| `stretch` \| `center` \| `start` \| `end` \| 0 1 2 3 4|`align-self: flex-end;`|
|`background-color`|`VisualElement`|_색_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_문자열_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`에서 `Frame`에서 `ImageButton`|_색_ \| `initial`|`border-color: #9acd32;`|
|`border-radius`|`BoxView`, `Button`, `Frame`, `ImageButton`|_double_ \| `initial` |`border-radius: 10;`|
|`border-width`|`Button`, `ImageButton`|_double_ \| `initial` |`border-width: .5;`|
|`color`|`ActivityIndicator`, `BoxView`, `Button`, `CheckBox`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `ProgressBar`, `SearchBar`, `Switch`, `TimePicker`|_색_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`column-gap`|`Grid`|_double_ \| `initial`|`column-gap: 9;`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`flex-direction`|`FlexLayout`| `column` \| `columnreverse` \| `row` \| `rowreverse` \| `row-reverse` \| 0 1 2|`flex-direction: column-reverse;`|
|`flex-basis`|`VisualElement`|_float_ \| `auto` \| `initial`. 또한 `%` 부호를 사용 하 여 0%에서 100% 범위의 백분율을 지정할 수 있습니다.|`flex-basis: 25%;`|
|`flex-grow`|`VisualElement`|_float_ \| `initial`|`flex-grow: 1.5;`|
|`flex-shrink`|`VisualElement`|_float_ \| `initial`|`flex-shrink: 1;`|
|`flex-wrap`|`VisualElement`| `nowrap` \| `wrap` \| `reverse` \| `wrap-reverse` \| `initial`|`flex-wrap: wrap-reverse;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_문자열_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_double_ \| _namedsize_ \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_double_ \| `initial` |`min-height: 250;`|
|`justify-content`|`FlexLayout`| `start` \| `center` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \||`justify-content: flex-end;`|
|`line-height`|`Label`, `Span`|_double_ \| `initial` |`line-height: 1.8;`|
|`margin`|`View`|_두께_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_두께_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_두께_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_두께_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_두께_ \| `initial` |`margin-bottom: 6;`|
|`max-lines`|`Label`|_int_ \| `initial`|`max-lines: 2;`|
|`min-height`|`VisualElement`|_double_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_double_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_double_ \| `initial` |`opacity: .3;`|
|`order`|`VisualElement`|_int_ \| `initial`|`order: -1;`|
|`padding`|`Button`, `ImageButton`, `Layout`, `Page`|_두께_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Button`, `ImageButton`, `Layout`, `Page`|_double_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \| `initial` |`padding-bottom: 6;`|
|`position`|`FlexLayout`| `relative` \| `absolute` \| `initial`|`position: absolute;`|
|`row-gap`|`Grid`| _double_ \| `initial`|`row-gap: 12;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `top` \| `right` \| `bottom` \| `start` \| 0 1 2 3 4 5 6 합니다. `left` 및 `right`은 오른쪽에서 왼쪽으로 진행 되는 환경에서 피해 야 합니다.| `text-align: right;`|
|`text-decoration`|`Label`, `Span`|`none` \| `underline` \| `strikethrough` \| `line-through` \| `initial`|`text-decoration: underline, line-through;`|
|`transform`|`VisualElement`| `none`, `rotate`, `rotateX`, `rotateY`, `scale`, `scaleX`, `scaleY`, `translate`, `translateX`, `translateY`, `initial` |`transform: rotate(180), scaleX(2.5);`|
|`transform-origin`|`VisualElement`| _double_, _double_ \| `initial` |`transform-origin: 7.5, 12.5;`|
|`vertical-align`|`Label`|`left` \| `top` \| `right` \| `bottom` \| `start` \| 0 1 2 3 4 5 6|`vertical-align: bottom;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| 0|`visibility: hidden;`|
|`width`|`VisualElement`|_double_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial`은 모든 속성에 대해 유효한 값입니다. 다른 스타일에서 설정 된 값 (기본값으로 다시 설정)을 지웁니다.

현재 지원 되지 않는 속성은 다음과 같습니다.

- `all: initial`
- 레이아웃 속성 (상자 또는 그리드).
- `font`, `border`등의 줄임 속성입니다.

또한 `inherit` 값이 없으므로 상속이 지원 되지 않습니다. 따라서 예를 들어 레이아웃에 `font-size` 속성을 설정 하 고 레이아웃의 모든 [`Label`](xref:Xamarin.Forms.Label) 인스턴스가 값을 상속 하도록 할 수는 없습니다. 한 가지 예외는 `direction` 속성 이며 기본값은 `inherit`입니다.

`Span` 요소를 대상으로 지정 하는 것은 요소와 이름 (`#` 기호 사용)에 의해 CSS 스타일의 대상이 될 수 없도록 하는 알려진 문제가 있습니다. `Span` 요소는 `StyleClass` 속성이 없는 `GestureElement`에서 파생 되므로 범위는 CSS 클래스 대상을 지원 하지 않습니다. 자세한 내용은 [범위 제어에 CSS 스타일을 적용할 수 없음](https://github.com/xamarin/Xamarin.Forms/issues/5979)을 참조 하세요.

### <a name="xamarinforms-specific-properties"></a>Xamarin.ios 관련 속성

다음 Xamarin.ios 관련 CSS 속성도 지원 됩니다 ( **값** 열에서 형식은 _기울임꼴_, 문자열 리터럴은 `gray`).

|속성|적용 대상|값|예제|
|---|---|---|---|
|`-xf-bar-background-color`|`NavigationPage`, `TabbedPage`|_색_ \| `initial` |`-xf-bar-background-color: teal;`|
|`-xf-bar-text-color`|`NavigationPage`, `TabbedPage`|_색_ \| `initial` |`-xf-bar-text-color: gray`|
|`-xf-horizontal-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-horizontal-scroll-bar-visibility: never;`|
|`-xf-max-length`|`Entry`, `Editor`|_int_ \| `initial` |`-xf-max-length: 20;`|
|`-xf-max-track-color`|`Slider`|_색_ \| `initial` |`-xf-max-track-color: red;`|
|`-xf-min-track-color`|`Slider`|_색_ \| `initial` |`-xf-min-track-color: yellow;`|
|`-xf-orientation`|`ScrollView`, `StackLayout`| `horizontal` \| `vertical` \| `both` \| `initial`. `both`은 `ScrollView` 에서만 지원 됩니다. |`-xf-orientation: horizontal;`|
|`-xf-placeholder`|`Entry`에서 `Editor`에서 `SearchBar`|_따옴표로 묶인 텍스트_ \| `initial` |`-xf-placeholder: Enter name;`|
|`-xf-placeholder-color`|`Entry`에서 `Editor`에서 `SearchBar`|_색_ \| `initial` |`-xf-placeholder-color: green;`|
|`-xf-spacing`|`StackLayout`|_double_ \| `initial` |`-xf-spacing: 8;`|
|`-xf-thumb-color`|`Slider`, `Switch`|_색_ \| `initial` |`-xf-thumb-color: limegreen;`|
|`-xf-vertical-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-vertical-scroll-bar-visibility: always;`|
|`-xf-vertical-text-alignment`|`Label`| `start` \| `center` \| `end` \| `initial`|`-xf-vertical-text-alignment: end;`|
|`-xf-visual`|`VisualElement`|_문자열_ \| `initial` |`-xf-visual: material;`|

### <a name="xamarinforms-shell-specific-properties"></a>Xamarin.ios 셸 관련 속성

다음 Xamarin. Forms 셸 특정 CSS 속성 ( **값** 열에서 형식은 _기울임꼴_, 문자열 리터럴은 `gray`)도 지원 됩니다.

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

### <a name="color"></a>Color

지원 되는 `color` 값은 다음과 같습니다.

- CSS 색, UWP 미리 정의 된 색 및 Xamarin.ios 색과 일치 하는 `X11` [색](https://en.wikipedia.org/wiki/X11_color_names/)입니다. 이러한 색 값은 대/소문자를 구분 하지 않습니다.
- 16 진수 색: `#rgb`, `#argb`, `#rrggbb`, `#aarrggbb`
- rgb 색: `rgb(255,0,0)`, `rgb(100%,0%,0%)`. 값의 범위는 0-255 또는 0%-100%입니다.
- rgba 색: `rgba(255, 0, 0, 0.8)`, `rgba(100%, 0%, 0%, 0.8)`. 불투명도 값은 0.0-1.0 범위에 있습니다.
- hsl 색: `hsl(120, 100%, 50%)`. H 값은 0-360 범위에 있고 s와 l은 0%-100% 범위에 있습니다.
- hsla 색: `hsla(120, 100%, 50%, .8)`. 불투명도 값은 0.0-1.0 범위에 있습니다.

### <a name="thickness"></a>Thickness

각각 공백으로 구분 되는 1, 2, 3 또는 4 개의 `thickness` 값이 지원 됩니다.

- 단일 값은 균일 두께를 나타냅니다.
- 두 값은 세로와 가로 두께를 나타냄
- 위쪽, 가로 (왼쪽 및 오른쪽), 아래쪽 두께를 나타내는 세 개의 값이 있습니다.
- 네 개의 값은 위쪽, 오른쪽, 아래쪽, 왼쪽 두께를 표시 합니다.

> [!NOTE]
> CSS `thickness` 값은 XAML [`Thickness`](xref:Xamarin.Forms.Thickness) 값과 다릅니다. 예를 들어 XAML에서 두 값 `Thickness`는 가로, 세로 두께를 나타냅니다. 반면 네 값 `Thickness`은 왼쪽, 위쪽, 오른쪽, 아래쪽 두께를 나타냅니다. 또한 XAML `Thickness` 값은 쉼표로 구분 됩니다.

### <a name="namedsize"></a>NamedSize

다음과 같은 대/소문자를 구분 하지 않는 `namedsize` 값이 지원 됩니다.

- `default`
- `micro`
- `small`
- `medium`
- `large`

각 `namedsize` 값의 정확한 의미는 플랫폼에 따라 다르며 뷰에 따라 다릅니다.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>Xamarin.ios를 사용 하는 Xamarin.ios의 CSS

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin.ios 3.0 CSS 비디오**

## <a name="related-links"></a>관련 링크

- [MonkeyAppCSS (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [XAML 스타일을 사용하여 Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/xaml/index.md)
