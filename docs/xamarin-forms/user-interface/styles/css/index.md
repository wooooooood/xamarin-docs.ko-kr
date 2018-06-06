---
title: 스타일 (CSS 스타일 시트)를 사용 하 여 Xamarin.Forms 앱을 지정 합니다.
description: Xamarin.Forms는 스타일 (CSS 스타일 시트)를 사용 하는 시각적 요소를 지원 합니다.
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 76ca67f7ac8a8e27e5f502455d48874c775fc172
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794087"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>스타일 (CSS 스타일 시트)를 사용 하 여 Xamarin.Forms 앱을 지정 합니다.

_Xamarin.Forms는 스타일 (CSS 스타일 시트)를 사용 하는 시각적 요소를 지원 합니다._

Xamarin.Forms 3.0에 CSS를 사용 하 여 응용 프로그램 스타일을 지정 하는 기능이 추가 되었습니다. 스타일 시트의 각 규칙을 하나 이상의 선택기 및 선언 블록으로 구성 된 규칙 목록으로 구성 됩니다. 중괄호에는 속성, 콜론 및 값의 각 선언으로 선언 목록을 선언 블록으로 구성 됩니다. 블록에 여러 번 선언 많을 때는 세미콜론으로 구분 기호로 삽입 됩니다. 다음 코드 예제에서는 일부 Xamarin.Forms 규격 CSS를 보여 줍니다.

```css
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

Xamarin.forms에 CSS 스타일 시트를 구문 분석 되 고 컴파일 시간 보다는 런타임, 평가 및 사용 시 다시 구문 분석 된 스타일 시트는 합니다.

> [!NOTE]
> 현재, 모든 XAML 스타일 지정 발생할 수 있는 스타일 지정 CSS로 수행할 수 없습니다. 그러나 XAML 스타일 Xamarin.Forms에서 현재 지원 되지 않는 속성에 대 한 CSS를 보완 하기 위해 사용할 수 있습니다. XAML 스타일에 대 한 자세한 내용은 참조 [XAML 스타일을 사용 하는 스타일 지정 Xamarin.Forms 앱](~/xamarin-forms/user-interface/styles/xaml/index.md)합니다.

[MonkeyAppCSS](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/) 샘플 CSS를 사용 하 여 간단한 응용 프로그램을 스타일을 지정 하 고 다음 스크린샷에서에 표시 됩니다.

[![CSS 스타일이 있는 MonkeyApp Main 페이지](css-images/MonkeyAppMainPage.png "CSS 스타일이 있는 MonkeyApp Main 페이지")](css-images/MonkeyAppMainPage-Large.png#lightbox "CSS 스타일이 있는 MonkeyApp 주 페이지")

[![CSS 스타일 지정 된 세부 정보 페이지 MonkeyApp](css-images/MonkeyAppDetailPage.png "CSS 스타일 지정 된 세부 정보 페이지 MonkeyApp")](css-images/MonkeyAppDetailPage-Large.png#lightbox "CSS 스타일이 있는 MonkeyApp 세부 정보 페이지")

> [!NOTE]
> 불가능 현재 스타일의 배경색을 지정 하는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 스타일 시트를 사용 합니다. 따라서 샘플 응용 프로그램에에서는 [ `NavigationPage.BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) 코드에서 속성을 설정 합니다.

## <a name="consuming-a-style-sheet"></a>스타일 시트를 사용합니다.

솔루션에는 스타일 시트를 추가 하기 위한 프로세스는 다음과 같습니다.

1. .NET 표준 라이브러리 프로젝트에 빈 CSS 파일을 추가 합니다.
1. CSS 파일의 빌드 작업 설정 **포함 리소스**합니다.

### <a name="loading-a-style-sheet"></a>스타일 시트를 로드합니다.

스타일 시트를 로드 하는 데 사용할 수 있는 방법의 여러 가지가 있습니다.

### <a name="xaml"></a>XAML

스타일 시트를 로드 하 고 구문 분석할 수는 [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) 클래스에 추가 되기 전에 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 페이지:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    ...
</ContentPage>
```

[ `StyleSheet.Source` ](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) 속성 URI로 시작 하는 경우 스타일 시트 바깥쪽 XAML 파일의 위치를 기준으로 또는 프로젝트 루트에 상대적인 URI로 지정 된 `/`합니다.

> [!WARNING]
> CSS 파일 경우 로드 되지 않는 빌드 작업이로 설정 하지는 **포함 리소스**합니다.

또는 스타일 시트 로드 하 고 수으로 구문 분석은 [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) 클래스 인라인 처리에 `CDATA` 섹션:

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

### <a name="c"></a>C#

C#에서는 스타일 시트 로드 포함 리소스로 하 고 수에 추가 된 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 페이지:

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

첫 번째 인수는 `StyleSheet.FromAssemblyResource` 메서드는 스타일 시트를 포함 하는 어셈블리, 두 번째 인수는는 `string` 리소스 식별자를 표시 합니다. 리소스 식별자를 가져올 수는 **속성** 창 CSS 파일을 선택 합니다.

또는에서 스타일 시트를 로드할 수 있습니다는 `StringReader` 에 추가 하 고는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 페이지:

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

에 대 한 인수는 `StyleSheet.FromReader` 메서드는는 `TextReader` 스타일 시트 읽기입니다.

## <a name="selecting-elements-and-applying-properties"></a>요소를 선택 하 고 속성 적용

CSS 선택기를 사용 하 여 요소를 대상으로 결정 합니다. 스타일 선택기를 일치 하는 정의 순서로 연속적으로 적용 됩니다. 특정 항목에 정의 된 스타일 항상 마지막에 적용 됩니다. 지원 되는 선택기에 대 한 자세한 내용은 참조 [선택기 참조](#selector-reference)합니다.

CSS 선택한 요소의 스타일 속성을 사용 합니다. 각 속성에 가능한 값 집합이 있으며 일부 속성에 영향 모든 형식의 요소를 반면 요소 그룹에 적용 합니다. 지원 되는 속성에 대 한 자세한 내용은 참조 [속성 참조](#property-reference)합니다.

### <a name="selecting-elements-by-type"></a>형식에서 요소 선택

대/소문자 구분 된 형식에서 시각적 트리의 요소를 선택할 수 `element` 선택기:

```css
stacklayout {
    margin: 20;
}
```

이 선택기는 식별 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 스타일 시트를 사용 하 고 해당 여백 20의 균일 두께를 설정 하는 페이지의 요소입니다.

> [!NOTE]
> `element` 선택기는 지정 된 형식의 하위 클래스를 식별 하지 않습니다.

### <a name="selecting-elements-by-base-class"></a>기본 클래스에 의해 요소 선택

대/소문자 구분 된 기본 클래스에 의해 시각적 트리의 요소를 선택할 수 `^base` 선택기:

```css
^contentpage {
    background-color: lightgray;
}
```

이 선택기는 식별 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 스타일 시트를 사용 하 고 배경과 설정 하는 요소 색상을 `lightgray`합니다.

> [!NOTE]
> `^base` 선택기 xamarin.forms, 특정 이며 CSS 사양에 포함 되지 않습니다.

### <a name="selecting-an-element-by-name"></a>이름으로 요소를 선택합니다.

대/소문자 구분 된 시각적 트리의 개별 요소를 선택할 수 `#id` 선택기:

```css
#listView {
    background-color: lightgray;
}
```

이 선택기는 요소를 식별 합니다. 해당 [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) 속성이 `listView`합니다. 그러나 경우는 `StyleId` 속성이 설정 되지 않은, 선택기는 사용 하도록 대체는 `x:Name` 요소입니다. 따라서 다음 XAML 예제에에서는 `#listView` 선택기는 식별는 [ `ListView` ](xref:Xamarin.Forms.ListView) 인 `x:Name` 특성이로 설정 된 `listView`의 배경색을 설정 하 고 `lightgray`합니다.

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

대/소문자 구분 된 특정 클래스 특성을 가진 요소를 선택할 수 `.class` 선택기:

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

CSS 클래스를 설정 하 여 XAML 요소에 할당할 수는 [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) CSS 클래스 이름에는 요소의 속성입니다. 따라서 다음 XAML 예제에서 스타일에 정의 된는 `.detailPageTitle` 클래스 첫 번째에 할당 된 [ `Label` ](xref:Xamarin.Forms.Label), 반면에 정의 된 스타일의 `.detailPageSubtitle` 클래스는 두 번째 인스턴스에 할당 된 `Label`합니다.

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

대/소문자 구분 된 시각적 트리의 자식 요소를 선택할 수 있습니다 `element element` 선택기:

```css
listview image {
    height: 60;
    width: 60;
}
```

이 선택기는 식별 [ `Image` ](xref:Xamarin.Forms.Image) 의 자식 요소를 [ `ListView` ](xref:Xamarin.Forms.ListView) 요소,-60의 높이 너비를 가져오거나 설정 합니다. 따라서 다음 XAML 예제에에서는 `listview image` 선택기 식별는 [ `Image` ](xref:Xamarin.Forms.Image) 의 자식이 [ `ListView` ](xref:Xamarin.Forms.ListView), 60 높이 너비를 가져오거나 설정 합니다.

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
> `element element` 선택기 자식 요소가 필요 하지 않습니다는 _직접_ 자식 요소가 부모-자식에 다른 부모 릴리스 레코드가 있습니다. 선택 영역에 있는 상위 항목은 지정된 된 첫 번째 요소 발생 합니다.

### <a name="selecting-direct-child-elements"></a>직접 자식 요소 선택

대/소문자 구분 된 시각적 트리의 자식 요소를 선택할 수 있습니다 직접 `element>element` 선택기:

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

이 선택기는 식별 [ `Image` ](xref:Xamarin.Forms.Image) 요소를의 직계 자식 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 요소를 200의 높이 너비를 가져오거나 설정 합니다. 따라서 다음 XAML 예제에에서는 `stacklayout>image` 선택기는 식별는 [ `Image` ](xref:Xamarin.Forms.Image) 의 직접 자식이 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), 200 높이 너비를 가져오거나 설정 합니다.

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
> `element>element` 선택기를 실행 하려면 자식 요소는 _직접_ 부모의 자식입니다.

## <a name="selector-reference"></a>선택기 참조

다음 CSS 선택기 Xamarin.Forms 지원 합니다.

|선택기|예|설명|
|---|---|---|
|`.class`|`.header`|와 모든 요소를 선택는 `StyleClass` '머리글'을 포함 하는 속성입니다. 이 선택기는 대/소문자 구분을 참고 합니다.|
|`#id`|`#email`|요소를 모두 선택 `StyleId` 로 설정 `email`합니다. 경우 `StyleId` 설정 되지 않은 경우 대체 인증 방법 `x:Name`합니다. XAML을 사용 하는 경우 `x:Name` 보다 선호 `StyleId`합니다. 이 선택기는 대/소문자 구분을 참고 합니다.|
|`*`|`*`|모든 요소를 선택 합니다.|
|`element`|`label`|형식의 요소를 모두 선택 `Label`, 하위 클래스가 아니라 하지만 합니다. 이 선택기는 대/소문자 구분을 참고 합니다.|
|`^base`|`^contentpage`|요소를 모두 선택 `ContentPage` 기본 클래스로 포함 하 여 `ContentPage` 자체입니다. 참고가이 선택기는 대/소문자를 구분 하 고 CSS 사양에 포함 되지 않습니다.|
|`element,element`|`label,button`|모든 선택 `Button` 요소와 모든 `Label` 요소입니다. 이 선택기는 대/소문자 구분을 참고 합니다.|
|`element element`|`stacklayout label`|모든 선택 `Label` 내부 요소는 `StackLayout`합니다. 이 선택기는 대/소문자 구분을 참고 합니다.|
|`element>element`|`stacklayout>label`|모든 선택 `Label` 요소 `StackLayout` 직접 부모로 합니다. 이 선택기는 대/소문자 구분을 참고 합니다.|
|`element+element`|`label+entry`|모든 선택 `Entry` 뒤 직접 요소는 `Label`합니다. 이 선택기는 대/소문자 구분을 참고 합니다.|
|`element~element`|`label~entry`|모든 선택 `Entry` 요소 앞에 `Label`합니다. 이 선택기는 대/소문자 구분을 참고 합니다.|

스타일 선택기를 일치 하는 정의 순서로 연속적으로 적용 됩니다. 특정 항목에 정의 된 스타일 항상 마지막에 적용 됩니다.

> [!TIP]
> 와 같은 제한 되지 않고 결합할 수 선택기 `StackLayout>ContentView>label.email`합니다.

다음 선택기 현재 지원 되지 않습니다.

- `[attribute]`
- `@media` 및 `@supports`
- `:` 및 `::`

> [!NOTE]
> 구체적 정보 및 특이성 재정의가 지원 되지 않습니다.

## <a name="property-reference"></a>속성 참조

다음과 같은 CSS 속성 Xamarin.Forms에서 지원 됩니다 (에 **값** 열 유형은 _기울임꼴_문자열 리터럴은 반면, `gray`):

|속성|적용 대상|값|예|
|---|---|---|---|
|`background-color`|`VisualElement`|_색_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_문자열_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`|_색_ \| `initial`|`border-color: #9acd32;`|
|`border-width`|`Button`|_double_ \| `initial` |`border-width: .5;`|
|`color`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`|_색_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_문자열_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_이중_ \| _namedsize_ \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_double_ \| `initial` |`min-height: 250;`|
|`margin`|`View`|_두께_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_두께_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_두께_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_두께_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_두께_ \| `initial` |`margin-bottom: 6;`|
|`min-height`|`VisualElement`|_double_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_double_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_double_ \| `initial` |`opacity: .3;`|
|`padding`|`Layout`, `Page`|_두께_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Layout`, `Page`|_double_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Layout`, `Page`| _double_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Layout`, `Page`| _double_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Layout`, `Page`| _double_ \| `initial` |`padding-bottom: 6;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `right` \| `center` \| `start` \| `end` \| `initial`. `left` 및 `right` 오른쪽에서 왼쪽으로 환경에서 사용 하지 않아야 합니다.| `text-align: right;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial `|`visibility: hidden;`|
|`width`|`VisualElement`|_double_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial` 모든 속성에 대 한 유효한 값이입니다. 다른 스타일에서 설정 된 값 (기본값으로 다시 설정)을 지웁니다.

다음과 같은 속성이 현재 지원 되지 않습니다.

- `all: initial`.
- 레이아웃 속성 (상자 또는 그리드)입니다.
- 줄임 속성 같은 `font`, 및 `border`합니다.

또한, 없습니다 `inherit` 값과 상속 지원 되지 않습니다. 예를 들어 설정할 수 없습니다, 따라서는 `font-size` 레이아웃 속성 모든 기대는 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스 값을 상속 하도록 레이아웃에서 합니다. 한 가지 예외는는 `direction` 기본 값이 속성의 `inherit`합니다.

### <a name="color"></a>색

다음 `color` 값이 지원 됩니다.

- `X11` [색](https://en.wikipedia.org/wiki/X11_color_names/)을 CSS 색, 미리 정의 된 색 UWP 및 Xamarin.Forms 색 일치입니다. 이러한 색 값은 대/소문자 구분을 참고 합니다.
- 색의 16 진수: `#rgb`, `#argb`, `#rrggbb`, `#aarrggbb`
- rgb 색: `rgb(255,0,0)`, `rgb(100%,0%,0%)`합니다. 값은 0%-100% 또는 0-255 범위입니다.
- rgba 색: `rgba(255, 0, 0, 0.8)`, `rgba(100%, 0%, 0%, 0.8)`합니다. 불투명도 값은 0.0-1.0 범위입니다.
- hsl 색: `hsl(120, 100%, 50%)`합니다. H 값은 범위 0-360 s 및 l는에 범위 0 ~ 100%입니다.
- hsla 색: `hsla(120, 100%, 50%, .8)`합니다. 불투명도 값은 0.0-1.0 범위입니다.

### <a name="thickness"></a>Thickness

1, 2, 3 또는 4 `thickness` 공백으로 구분 된 각, 값이 지원 됩니다.

- 단일 값 균일 두께 나타냅니다.
- 두 값은 세로 다음 가로 두께 나타냅니다.
- 3 개의 값 (왼쪽 및 오른쪽) 수평, 위쪽, 차례로 아래쪽 두께 나타냅니다.
- 4 개의 값 위쪽, 오른쪽, 한 다음 아래쪽으로 차례로 왼쪽된 두께 나타냅니다.

> [!NOTE]
> CSS `thickness` XAML에서 값이 다른 [ `Thickness` ](/api/type/Xamarin.Forms.Thickness/) 값입니다. XAML 두 값의 예를 들어 `Thickness` 4 값 가로 다음 세로 두께 나타내는 `Thickness` 아래쪽 두께 다음 왼쪽 차례로 위쪽, 오른쪽으로 나타냅니다. 또한 XAML `Thickness` 값은 쉼표로 구분 합니다.

### <a name="namedsize"></a>NamedSize

대/소문자 구분 다음 `namedsize` 값이 지원 됩니다.

- `default`
- `micro`
- `small`
- `medium`
- `large`

각각의 정확한 의미 `namedsize` 값은 플랫폼별 및 보기에 따라 다릅니다.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>CSS Xamarin.University와 Xamarin.Forms에

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin.Forms 3.0 CSS를 위한 것 이며 [Xamarin 대학](https://university.xamarin.com/)**

## <a name="related-links"></a>관련 링크

- [MonkeyAppCSS (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)
- [XAML 스타일을 사용하여 Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/xaml/index.md)
