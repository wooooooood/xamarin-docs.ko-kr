---
title: Xamarin.FormsBoxView
description: 이 문서에서는 응용 프로그램에서 장식, 그래픽 및 상호 작용에 색이 지정 된 사각형을 사용 하는 방법을 설명 합니다 Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5f915955bff969ef38cdb7a89bf9cecf05401131
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136359"
---
# <a name="xamarinforms-boxview"></a>Xamarin.FormsBoxView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)

[`BoxView`](xref:Xamarin.Forms.BoxView)지정 된 너비, 높이 및 색의 단순 사각형을 렌더링 합니다. 를 사용 하 여 `BoxView` 데코레이션, 기초적인 그래픽 및 터치를 통해 사용자와 상호 작용할 수 있습니다.

에는 Xamarin.Forms 기본 제공 벡터 그래픽 시스템이 없으므로에서 보정할 수 있습니다 `BoxView` . 이 문서에서 설명 하는 일부 샘플 프로그램은를 사용 하 여 `BoxView` 그래픽을 렌더링 합니다. 는 `BoxView` 특정 너비 및 두께의 줄과 비슷하게 크기를 지정한 다음 속성을 사용 하 여 임의의 각도로 회전 합니다 `Rotation` .

`BoxView`는 간단한 그래픽을 모방 하지만 보다 정교한 그래픽 요구 사항에 대해 [SkiaSharp Xamarin.Forms 의 사용](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) 을 조사 하는 것이 좋습니다.

이 문서는 다음 항목을 설명합니다.

- **[BoxView 색 및 크기 설정](#colorandsize)** &ndash; 속성을 설정 `BoxView` 합니다.
- **[텍스트 장식 렌더링](#textdecorations)** &ndash; 선을 렌더링 하려면를 사용 `BoxView` 합니다.
- BoxView를 사용 **[하 여 색 나열](#listingcolors)** &ndash; 에 모든 시스템 색을 표시 `ListView` 합니다.
- BoxView를 서브클래싱 **[하 여 게임의 생명 재생](#subclassing)** &ndash; 유명한 셀룰러 automaton을 구현 합니다.
- **[디지털 시계 만들기](#digitalclock)** &ndash; 점 행렬 디스플레이를 시뮬레이션 합니다.
- **[아날로그 클록 만들기](#analogclock)** &ndash; 요소를 변환 하 고 애니메이션 효과를 적용 `BoxView` 합니다.

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>BoxView 색 및 크기 설정

일반적으로의 다음 속성을 설정 합니다 `BoxView` .

- [`Color`](xref:Xamarin.Forms.BoxView.Color)색을 설정 합니다.
- [`CornerRadius`](xref:Xamarin.Forms.BoxView.CornerRadius)모퉁이 반경을 설정 합니다.
- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)장치 독립적 단위로의 너비를 설정 합니다 `BoxView` .
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)의 높이를 설정 `BoxView` 합니다.

`Color`속성은 형식입니다 .이 `Color` 속성은 `Color` 에서로의 사전순으로 범위가 지정 된 명명 된 색의 정적 읽기 전용 필드인 141을 포함 하 여 모든 값으로 설정할 수 있습니다 `AliceBlue` `YellowGreen` .

`CornerRadius`속성은 형식입니다 [`CornerRadius`](xref:Xamarin.Forms.CornerRadius) . 속성은 단일 단일 `double` 모퉁이 반지름 값으로 설정 하거나 `CornerRadius` `double` 의 왼쪽 위, 오른쪽 위, 왼쪽 아래 및 오른쪽 아래에 적용 되는 네 가지 값으로 정의 된 구조로 설정할 수 있습니다 `BoxView` .

`WidthRequest`및 `HeightRequest` 속성은 `BoxView` 가 레이아웃에서 *제한* 되지 않는 경우에만 역할을 수행 합니다. 이는가 `BoxView` 레이아웃에서 자동 크기 셀의 자식인 경우와 같이 레이아웃 컨테이너가 자식의 크기를 알고 있어야 하는 경우입니다 `Grid` . `BoxView` `HorizontalOptions` 및 `VerticalOptions` 속성이 이외의 값으로 설정 된 경우에도이 제한 되지 않습니다 `LayoutOptions.Fill` . `BoxView`이 제한 되지 않지만 `WidthRequest` 및 `HeightRequest` 속성이 설정 되지 않은 경우 너비 또는 높이는 기본값 40 단위 또는 모바일 장치의 약 1/4 인치로 설정 됩니다.

`WidthRequest` `HeightRequest` 가 레이아웃으로 제한 되는 경우 및 속성은 무시 됩니다 `BoxView` .이 경우 레이아웃 컨테이너는에 자체 크기 *constrained* 를 `BoxView` 적용 합니다.

은 `BoxView` 한 차원에서 제약을 받을 수 있으며 다른 차원에서는 제한 되지 않습니다. 예를 들어 `BoxView` 가 세로의 자식인 경우 `StackLayout` 의 세로 차원은 `BoxView` 제한 되지 않으며 가로 차원은 일반적으로 제한 됩니다. 그러나이 가로 차원에 대 한 예외가 있습니다 .의 `BoxView` `HorizontalOptions` 속성이 이외의 다른로 설정 된 경우에는 `LayoutOptions.Fill` 가로 차원만 제한 되지 않습니다. 또한 `StackLayout` 자체에 제한 되지 않은 가로 차원이 있을 수 있으며,이 경우에는 행이 `BoxView` 제한 되지 않습니다.

[**Basicboxview**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview) 샘플은 `BoxView` 해당 페이지의 가운데에서 1 인치의 제한 없는 1 인치의 사각형을 표시 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BasicBoxView"
             x:Class="BasicBoxView.MainPage">

    <BoxView Color="CornflowerBlue"
             CornerRadius="10"
             WidthRequest="160"
             HeightRequest="160"
             VerticalOptions="Center"
             HorizontalOptions="Center" />

</ContentPage>
```

결과:

[![기본 BoxView](boxview-images/basicboxview-small.png "기본 BoxView")](boxview-images/basicboxview-large.png#lightbox "BasicBoxView")

`VerticalOptions` `HorizontalOptions` 태그에서 및 속성을 제거 `BoxView` 하거나로 설정 하면가 `Fill` `BoxView` 페이지 크기에 따라 제약 되 고 페이지를 채우도록 확장 됩니다.

는 `BoxView` 의 자식일 수도 있습니다 `AbsoluteLayout` . 이 경우의 위치와 크기는 모두 `BoxView` 연결 된 바인딩 가능 속성을 사용 하 여 설정 됩니다 `LayoutBounds` . 는 `AbsoluteLayout` [**AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md)문서에서 설명 합니다.

다음 샘플 프로그램에서 이러한 모든 사례에 대 한 예제를 확인할 수 있습니다.

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>텍스트 장식 렌더링

를 사용 하 여 `BoxView` 가로 및 세로 선 형식으로 페이지에 몇 가지 간단한 장식을 추가할 수 있습니다. [**Textdecoration**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration) 샘플에서는이를 보여 줍니다. 모든 프로그램의 시각적 개체는 **MainPage.xaml** `Label` `BoxView` `StackLayout` 다음에 표시 된의 여러 및 요소를 포함 하는 mainpage 파일에 정의 됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:TextDecoration"
             x:Class="TextDecoration.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Black" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ScrollView Margin="15">
        <StackLayout>

            ···

        </StackLayout>
    </ScrollView>
</ContentPage>
```

뒤에 오는 모든 태그는의 자식입니다 `StackLayout` . 이 태그는 요소에 사용 되는 여러 형식의 장식용 요소로 구성 됩니다 `BoxView` `Label` .

[![텍스트 장식](boxview-images/textdecoration-small.png "텍스트 장식")](boxview-images/textdecoration-large.png#lightbox "텍스트 장식")

페이지 위쪽의 세련 된 머리글은 해당 자식이 4 개의 요소와 인을 사용 하 여 달성 됩니다. 여기에는 `AbsoluteLayout` `BoxView` `Label` 특정 위치와 크기가 할당 됩니다.

```xaml
<AbsoluteLayout>
    <BoxView AbsoluteLayout.LayoutBounds="0, 10, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="0, 20, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="10, 0, 5, 65" />
    <BoxView AbsoluteLayout.LayoutBounds="20, 0, 5, 65" />
    <Label Text="Stylish Header"
           FontSize="24"
           AbsoluteLayout.LayoutBounds="30, 25, AutoSize, AutoSize"/>
</AbsoluteLayout>
```

XAML 파일에서에는를 `AbsoluteLayout` `Label` 설명 하는 형식이 지정 된 텍스트가 `AbsoluteLayout` 있습니다.

`Label` `BoxView` `StackLayout` `HorizontalOptions` 값이 이외의 값으로 설정 `Fill` 된에 및를 포함 하 여 텍스트 문자열에 밑줄을 지정할 수 있습니다. 그런 다음의 너비는의 너비에 의해 제어 되 고이에 따라에 해당 `StackLayout` `Label` 너비가 적용 됩니다 `BoxView` . 에는 `BoxView` 명시적 높이도 할당 됩니다.

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

이 기법을 사용 하 여 긴 텍스트 문자열이 나 단락 내의 개별 단어에 밑줄을 표시할 수 없습니다.

또한를 사용 하 여 `BoxView` HTML `hr` (수평선 규칙) 요소와 유사할 수 있습니다. 해당 `BoxView` 부모 컨테이너 (이 경우)에 의해 너비가 결정 되도록 하기만 하면 됩니다 `StackLayout` .

```xaml
<BoxView HeightRequest="3" />
```

마지막으로 및를 모두 가로로 묶으면 텍스트 단락의 한쪽 면에 세로줄을 그릴 수 있습니다 `BoxView` `Label` `StackLayout` . 이 경우의 높이는의 높이와 동일 합니다 .이 높이는의 높이에 `BoxView` `StackLayout` 의해 결정 됩니다 `Label` .

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
</StackLayout>
```

<a name="listingcolors" />

## <a name="listing-colors-with-boxview"></a>BoxView를 사용 하 여 색 나열

는 `BoxView` 색을 표시 하는 데 편리 합니다. 이 프로그램은를 사용 하 여 `ListView` 구조체의 모든 public static 읽기 전용 필드를 나열 합니다 Xamarin.Forms `Color` .

[![ListView 색](boxview-images/listviewcolors-small.png "ListView 색")](boxview-images/listviewcolors-large.png#lightbox "ListView 색")

[**ListViewColors**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors) 프로그램에는 라는 클래스가 포함 되어 있습니다 `NamedColor` . 정적 생성자는 리플렉션을 사용 하 여 구조체의 모든 필드에 액세스 하 `Color` 고 `NamedColor` 각각에 대 한 개체를 만듭니다. 이러한 속성은 정적 속성에 저장 됩니다 `All` .

```csharp
public class NamedColor
{
    // Instance members.
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    // Static members.
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields ())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof (Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                               (int)(255 * color.R),
                                               (int)(255 * color.G),
                                               (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }
}
```

프로그램 시각적 개체는 XAML 파일에 설명 되어 있습니다. `ItemsSource`의 속성은 `ListView` 정적 속성으로 설정 됩니다 `NamedColor.All` . 즉,에서 `ListView` 모든 개별 개체를 표시 합니다 `NamedColor` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ListViewColors"
             x:Class="ListViewColors.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="10, 20, 10, 0" />
            <On Platform="Android, UWP" Value="10, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ListView SeparatorVisibility="None"
              ItemsSource="{x:Static local:NamedColor.All}">
        <ListView.RowHeight>
            <OnPlatform x:TypeArguments="x:Int32">
                <On Platform="iOS, Android" Value="80" />
                <On Platform="UWP" Value="90" />
            </OnPlatform>
        </ListView.RowHeight>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ContentView Padding="5">
                        <Frame OutlineColor="Accent"
                               Padding="10">
                            <StackLayout Orientation="Horizontal">
                                <BoxView Color="{Binding Color}"
                                         WidthRequest="50"
                                         HeightRequest="50" />
                                <StackLayout>
                                    <Label Text="{Binding FriendlyName}"
                                           FontSize="22"
                                           VerticalOptions="StartAndExpand" />
                                    <Label Text="{Binding RgbDisplay, StringFormat='RGB = {0}'}"
                                           FontSize="16"
                                           VerticalOptions="CenterAndExpand" />
                                </StackLayout>
                            </StackLayout>
                        </Frame>
                    </ContentView>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

`NamedColor`개체의 형식은 `ViewCell` 의 데이터 템플릿으로 설정 된 개체로 지정 됩니다 `ListView` . 이 템플릿에는 `BoxView` `Color` 속성이 `Color` 개체의 속성에 바인딩된가 포함 됩니다 `NamedColor` .

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>BoxView를 서브클래싱 하 여 게임의 생명 재생

생활의 게임은 *1970 년대의 mvvm* 페이지에서 Mathematician John conway가 만든 셀룰러 automaton입니다. 가장 좋은 소개는 위키백과 문서와 생활의 [생활](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)에서 제공 됩니다.

Xamarin.Forms [**GameOfLife**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife) 프로그램은 `LifeCell` 에서 파생 되는 이라는 클래스를 정의 합니다 `BoxView` . 이 클래스는 수명 게임의 개별 셀에 대 한 논리를 캡슐화 합니다.

```csharp
class LifeCell : BoxView
{
    bool isAlive;

    public event EventHandler Tapped;

    public LifeCell()
    {
        BackgroundColor = Color.White;

        TapGestureRecognizer tapGesture = new TapGestureRecognizer();
        tapGesture.Tapped += (sender, args) =>
        {
            Tapped?.Invoke(this, EventArgs.Empty);
        };
        GestureRecognizers.Add(tapGesture);
    }

    public int Col { set; get; }

    public int Row { set; get; }

    public bool IsAlive
    {
        set
        {
            if (isAlive != value)
            {
                isAlive = value;
                BackgroundColor = isAlive ? Color.Black : Color.White;
            }
        }
        get
        {
            return isAlive;
        }
    }
}
```

`LifeCell`에 세 가지 추가 속성을 추가 `BoxView` 합니다. `Col` 및 `Row` 속성은 표 안에 셀의 위치를 저장 하 고 `IsAlive` 속성은 해당 상태를 나타냅니다. `IsAlive`또한이 속성은 `Color` `BoxView` 셀이 활성 상태 이면의 속성을 검은색으로 설정 하 고, 셀이 활성 상태가 아닌 경우에는 흰색으로 설정 합니다.

`LifeCell`또한 사용자가 셀을 탭 하 여 상태를 토글할 수 있도록를 설치 합니다 `TapGestureRecognizer` . 클래스는 `Tapped` 제스처 인식기에서 이벤트를 자체 이벤트로 변환 합니다 `Tapped` .

**GameOfLife** 프로그램에는 `LifeGrid` 게임의 많은 논리를 캡슐화 하는 클래스와 `MainPage` 프로그램의 시각적 개체를 처리 하는 클래스가 포함 되어 있습니다. 여기에는 게임의 규칙을 설명 하는 오버레이가 포함 됩니다. 페이지에 수백 개의 개체를 표시 하는 작업의 프로그램은 `LifeCell` 다음과 같습니다.

[![생명 게임](boxview-images/gameoflife-small.png "생명 게임")](boxview-images/gameoflife-large.png#lightbox "생명 게임")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>디지털 시계 만들기

[**DotMatrixClock**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock) 프로그램은 210 `BoxView` 요소를 만들어 이전의 5-7 점 행렬 디스플레이의 점을 시뮬레이션 합니다. 세로 또는 가로 모드에서 시간을 읽을 수 있지만 가로 크기는 다음과 같습니다.

[![점 행렬 클록](boxview-images/dotmatrixclock-small.png "점 행렬 클록")](boxview-images/dotmatrixclock-large.png#lightbox "점 행렬 클록")

XAML 파일은 클록에 사용 된를 인스턴스화하는 것 보다 약간 더 적습니다 `AbsoluteLayout` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DotMatrixClock"
             x:Class="DotMatrixClock.MainPage"
             Padding="10"
             SizeChanged="OnPageSizeChanged">

    <AbsoluteLayout x:Name="absoluteLayout"
                    VerticalOptions="Center" />
</ContentPage>
```

모든 항목은 코드 숨김이 파일에 발생 합니다. 점 행렬 표시 논리는 각각 10 자리 및 콜론에 해당 하는 점을 설명 하는 여러 배열의 정의에 의해 크게 간소화 됩니다.

```csharp
public partial class MainPage : ContentPage
{
    // Total dots horizontally and vertically.
    const int horzDots = 41;
    const int vertDots = 7;

    // 5 x 7 dot matrix patterns for 0 through 9.
    static readonly int[, ,] numberPatterns = new int[10, 7, 5]
    {
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 1, 1}, { 1, 0, 1, 0, 1},
            { 1, 1, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 0, 0}, { 0, 1, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0},
            { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0},
            { 0, 0, 1, 0, 0}, { 0, 1, 0, 0, 0}, { 1, 1, 1, 1, 1}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 0, 1, 0},
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 0, 1, 0}, { 0, 0, 1, 1, 0}, { 0, 1, 0, 1, 0}, { 1, 0, 0, 1, 0},
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 0, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0}, { 0, 0, 0, 0, 1},
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 1, 0}, { 0, 1, 0, 0, 0}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0},
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0},
            { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0},
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 1},
            { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 1, 1, 0, 0}
        },
    };

    // Dot matrix pattern for a colon.
    static readonly int[,] colonPattern = new int[7, 2]
    {
        { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }
    };

    // BoxView colors for on and off.
    static readonly Color colorOn = Color.Red;
    static readonly Color colorOff = new Color(0.5, 0.5, 0.5, 0.25);

    // Box views for 6 digits, 7 rows, 5 columns.
    BoxView[, ,] digitBoxViews = new BoxView[6, 7, 5];

    ···

}
```

이러한 필드는 `BoxView` 6 자리 숫자에 대 한 점 패턴을 저장 하는 3 차원 요소의 배열로 끝납니다.

생성자는 `BoxView` 숫자와 콜론의 모든 요소를 만들고 `Color` `BoxView` 콜론에 대 한 요소의 속성도 초기화 합니다.

```csharp
public partial class MainPage : ContentPage
{

    ···

    public MainPage()
    {
        InitializeComponent();

        // BoxView dot dimensions.
        double height = 0.85 / vertDots;
        double width = 0.85 / horzDots;

        // Create and assemble the BoxViews.
        double xIncrement = 1.0 / (horzDots - 1);
        double yIncrement = 1.0 / (vertDots - 1);
        double x = 0;

        for (int digit = 0; digit < 6; digit++)
        {
            for (int col = 0; col < 5; col++)
            {
                double y = 0;

                for (int row = 0; row < 7; row++)
                {
                    // Create the digit BoxView and add to layout.
                    BoxView boxView = new BoxView();
                    digitBoxViews[digit, row, col] = boxView;
                    absoluteLayout.Children.Add(boxView,
                                                new Rectangle(x, y, width, height),
                                                AbsoluteLayoutFlags.All);
                    y += yIncrement;
                }
                x += xIncrement;
            }
            x += xIncrement;

            // Colons between the hours, minutes, and seconds.
            if (digit == 1 || digit == 3)
            {
                int colon = digit / 2;

                for (int col = 0; col < 2; col++)
                {
                    double y = 0;

                    for (int row = 0; row < 7; row++)
                    {
                        // Create the BoxView and set the color.
                        BoxView boxView = new BoxView
                            {
                                Color = colonPattern[row, col] == 1 ?
                                            colorOn : colorOff
                            };
                        absoluteLayout.Children.Add(boxView,
                                                    new Rectangle(x, y, width, height),
                                                    AbsoluteLayoutFlags.All);
                        y += yIncrement;
                    }
                    x += xIncrement;
                }
                x += xIncrement;
            }
        }

        // Set the timer and initialize with a manual call.
        Device.StartTimer(TimeSpan.FromSeconds(1), OnTimer);
        OnTimer();
    }

    ···

}
```

이 프로그램에서는의 상대 위치 지정 및 크기 조정 기능을 사용 `AbsoluteLayout` 합니다. 각의 너비와 높이는 `BoxView` 소수 값으로 설정 됩니다. 특히 85% 1은 가로 및 세로 점 수로 나눈 값입니다. 위치도 소수 값으로 설정 됩니다.

모든 위치와 크기는의 전체 크기를 기준으로 하기 때문에 `AbsoluteLayout` `SizeChanged` 페이지 처리기는의만 설정 하면 됩니다 `HeightRequest` `AbsoluteLayout` .

```csharp
public partial class MainPage : ContentPage
{

    ···

    void OnPageSizeChanged(object sender, EventArgs args)
    {
        // No chance a display will have an aspect ratio > 41:7
        absoluteLayout.HeightRequest = vertDots * Width / horzDots;
    }

    ···

}
```

의 너비는 `AbsoluteLayout` 페이지의 전체 너비로 확장 되기 때문에 자동으로 설정 됩니다.

클래스의 최종 코드는 `MainPage` 타이머 콜백을 처리 하 고 각 자릿수의 점을 색으로 처리 합니다. 코드를 작성 하는 파일의 시작 부분에 있는 다차원 배열 정의는이 논리를 프로그램의 가장 간단한 부분으로 만드는 데 도움이 됩니다.

```csharp
public partial class MainPage : ContentPage
{

    ···

    bool OnTimer()
    {
        DateTime dateTime = DateTime.Now;

        // Convert 24-hour clock to 12-hour clock.
        int hour = (dateTime.Hour + 11) % 12 + 1;

        // Set the dot colors for each digit separately.
        SetDotMatrix(0, hour / 10);
        SetDotMatrix(1, hour % 10);
        SetDotMatrix(2, dateTime.Minute / 10);
        SetDotMatrix(3, dateTime.Minute % 10);
        SetDotMatrix(4, dateTime.Second / 10);
        SetDotMatrix(5, dateTime.Second % 10);
        return true;
    }

    void SetDotMatrix(int index, int digit)
    {
        for (int row = 0; row < 7; row++)
            for (int col = 0; col < 5; col++)
            {
                bool isOn = numberPatterns[digit, row, col] == 1;
                Color color = isOn ? colorOn : colorOff;
                digitBoxViews[index, row, col].Color = color;
            }
    }
}
```

<a name="analogclock" />

## <a name="creating-an-analog-clock"></a>아날로그 클록 만들기

점 행렬 clock은의 명확한 응용 프로그램으로 보일 수 `BoxView` 있지만 `BoxView` 요소는 아날로그 클록을 인식 하는 데에도 사용할 수 있습니다.

[![BoxView 클록](boxview-images/boxviewclock-small.png "BoxView 클록")](boxview-images/boxviewclock-large.png#lightbox "BoxView 클록")

[**Boxviewclock**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) 프로그램의 모든 시각적 개체는의 자식 `AbsoluteLayout` 입니다. 이러한 요소는 연결 된 속성을 사용 하 여 크기가 조정 되 `LayoutBounds` 고 속성을 사용 하 여 회전 됩니다 `Rotation` .

`BoxView`Clock의 손을 위한 세 요소는 XAML 파일에서 인스턴스화되고 위치 지정 또는 크기가 지정 되지 않습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BoxViewClock"
             x:Class="BoxViewClock.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <AbsoluteLayout x:Name="absoluteLayout"
                    SizeChanged="OnAbsoluteLayoutSizeChanged">

        <BoxView x:Name="hourHand"
                 Color="Black" />

        <BoxView x:Name="minuteHand"
                 Color="Black" />

        <BoxView x:Name="secondHand"
                 Color="Black" />
    </AbsoluteLayout>
</ContentPage>
```

코드를 표시 하는 파일의 생성자는 `BoxView` 클록 둘레의 눈금 표시에 대 한 60 요소를 인스턴스화합니다.

```csharp
public partial class MainPage : ContentPage
{

    ···

    BoxView[] tickMarks = new BoxView[60];

    public MainPage()
    {
        InitializeComponent();

        // Create the tick marks (to be sized and positioned later).
        for (int i = 0; i < tickMarks.Length; i++)
        {
            tickMarks[i] = new BoxView { Color = Color.Black };
            absoluteLayout.Children.Add(tickMarks[i]);
        }

        Device.StartTimer(TimeSpan.FromSeconds(1.0 / 60), OnTimerTick);
    }

    ···

}
```

모든 요소의 크기 조정 및 위치 지정은 `BoxView` `SizeChanged` 에 대 한 처리기에서 발생 합니다 `AbsoluteLayout` . 클래스의 내부 구조 중 작은 구조는 `HandParams` clock의 총 크기를 기준으로 세 가지 바늘의 크기를 설명 합니다.

```csharp
public partial class MainPage : ContentPage
{
    // Structure for storing information about the three hands.
    struct HandParams
    {
        public HandParams(double width, double height, double offset) : this()
        {
            Width = width;
            Height = height;
            Offset = offset;
        }

        public double Width { private set; get; }   // fraction of radius
        public double Height { private set; get; }  // ditto
        public double Offset { private set; get; }  // relative to center pivot
    }

    static readonly HandParams secondParams = new HandParams(0.02, 1.1, 0.85);
    static readonly HandParams minuteParams = new HandParams(0.05, 0.8, 0.9);
    static readonly HandParams hourParams = new HandParams(0.125, 0.65, 0.9);

    ···

 }
```

`SizeChanged`처리기는의 중심 및 반경을 결정 한 `AbsoluteLayout` 다음 `BoxView` 눈금으로 사용 되는 60 요소의 크기와 위치를 조정 합니다. `for`루프는 `Rotation` 이러한 각 요소의 속성을 설정 하 여 마칩니다 `BoxView` . 처리기의 끝에서 `SizeChanged` `LayoutHand` 메서드를 호출 하 여 세 개의 시계 바늘을 크기 조정 하 고 위치를 조정 합니다.

```csharp
public partial class MainPage : ContentPage
{

    ···

    void OnAbsoluteLayoutSizeChanged(object sender, EventArgs args)
    {
        // Get the center and radius of the AbsoluteLayout.
        Point center = new Point(absoluteLayout.Width / 2, absoluteLayout.Height / 2);
        double radius = 0.45 * Math.Min(absoluteLayout.Width, absoluteLayout.Height);

        // Position, size, and rotate the 60 tick marks.
        for (int index = 0; index < tickMarks.Length; index++)
        {
            double size = radius / (index % 5 == 0 ? 15 : 30);
            double radians = index * 2 * Math.PI / tickMarks.Length;
            double x = center.X + radius * Math.Sin(radians) - size / 2;
            double y = center.Y - radius * Math.Cos(radians) - size / 2;
            AbsoluteLayout.SetLayoutBounds(tickMarks[index], new Rectangle(x, y, size, size));
            tickMarks[index].Rotation = 180 * radians / Math.PI;
        }

        // Position and size the three hands.
        LayoutHand(secondHand, secondParams, center, radius);
        LayoutHand(minuteHand, minuteParams, center, radius);
        LayoutHand(hourHand, hourParams, center, radius);
    }

    void LayoutHand(BoxView boxView, HandParams handParams, Point center, double radius)
    {
        double width = handParams.Width * radius;
        double height = handParams.Height * radius;
        double offset = handParams.Offset;

        AbsoluteLayout.SetLayoutBounds(boxView,
            new Rectangle(center.X - 0.5 * width,
                          center.Y - offset * height,
                          width, height));

        // Set the AnchorY property for rotations.
        boxView.AnchorY = handParams.Offset;
    }

    ···

}
```

메서드는 크기를 조정 하 `LayoutHand` 고 각 손 위치를 지정 하 여 12:00 위치를 가리키도록 합니다. 메서드의 끝에서 `AnchorY` 속성은 clock의 중심에 해당 하는 위치로 설정 됩니다. 회전 중심을 나타냅니다.

이 손을 타이머 콜백 함수에서 회전 합니다.

```csharp
public partial class MainPage : ContentPage
{

    ···

    bool OnTimerTick()
    {
        // Set rotation angles for hour and minute hands.
        DateTime dateTime = DateTime.Now;
        hourHand.Rotation = 30 * (dateTime.Hour % 12) + 0.5 * dateTime.Minute;
        minuteHand.Rotation = 6 * dateTime.Minute + 0.1 * dateTime.Second;

        // Do an animation for the second hand.
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        secondHand.Rotation = 6 * (dateTime.Second + t);
        return true;
    }
}
```

두 번째 손은 약간 다르게 처리 됩니다. 애니메이션 감속/가속 함수를 적용 하 여 움직임이 부드러운이 지 않고 기계적으로 보이도록 합니다. 각 틱에서 두 번째 손은 약간 뒤로 초과 다음 해당 대상을 다시 가져옵니다. 이 약간 약간의 코드는 이동의 현실감에 많은을 추가 합니다.

## <a name="conclusion"></a>결론

는 `BoxView` 처음에는 간단 하 게 보일 수 있지만 앞서 살펴본 것 처럼 매우 다양 하 고, 일반적으로 벡터 그래픽 에서만 가능한 시각적 개체를 거의 재현할 수 있습니다. 보다 정교한 그래픽은 [ Xamarin.Forms 에서 SkiaSharp 사용 ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [기본 BoxView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)
- [텍스트 장식 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration)
- [ListView 색 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/)
- [생명 Game (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)
- [도트 매트릭스 클록 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)
- [BoxView 클록 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock)
- [BoxView](xref:Xamarin.Forms.BoxView)
