---
title: Xamarin.Forms BoxView
description: 이 문서에 장식, 그래픽 및 Xamarin.Forms 응용 프로그램에서 상호 작용에 대 한 색이 지정 된 사각형을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2017
ms.openlocfilehash: edb2785362f2cc7377d9adb0c1a89a6fa2b08111
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244317"
---
# <a name="xamarinforms-boxview"></a>Xamarin.Forms BoxView

[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) 지정 된 너비, 높이 및 색상의 간단한 사각형을 렌더링합니다. 사용할 수 있습니다 `BoxView` 데코레이션에 기초적인 그래픽 및 터치를 통해 사용자와 상호 작용 합니다.

Xamarin.Forms에 기본 제공 벡터 그래픽 시스템 없기 때문에 `BoxView` 보정 하는 데 도움이 됩니다. 이 문서 사용에 설명 된 샘플 프로그램의 일부 `BoxView` 그래픽 렌더링에 대 한 합니다. `BoxView` 한 줄의 사용자 지정 및 두께 유사 하 게 크기가 조정 하 고 사용 하 여 모든 각도 회전할 수는 `Rotation` 속성입니다.

하지만 `BoxView` 간단한 그래픽을 모방 하면, 조사 하는 것이 좋습니다 [xamarin.forms에서를 사용 하 여 SkiaSharp](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) 보다 정교한 그래픽 요구 사항에 대 한 합니다.

이 문서에서는 다음 내용을 다룹니다.

- **[BoxView 색 및 크기 설정](#colorandsize)**  &ndash; 설정는 `BoxView` 속성입니다.
- **[렌더링 텍스트 장식](#textdecorations)**  &ndash; 사용 하 여 한 `BoxView` 렌더링 줄에 대 한 합니다.
- **[BoxView 된 색을 나열 하는](#listingcolors)**  &ndash; 시스템에 색이 모두 표시 한 `ListView`합니다.
- **[서브클래싱 BoxView 하 여 게임의 수명 재생](#subclassing)**  &ndash; 유명한 셀룰러 자동화를 구현 합니다.
- **[디지털 클록 만들기](#digitalclock)**  &ndash; 도트 디스플레이 시뮬레이션 합니다.
- **[아날로그 클록 만들기](#analogclock)**  &ndash; 변환 및 애니메이션 `BoxView` 요소입니다.

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>설정 BoxView 색 및 크기

다음 세 가지 속성을 설정 합니다 경우가 매우 자주 `BoxView`:

- [`Color`](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) 색을 설정 합니다.
- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) 너비를 설정 하는 `BoxView` 장치 독립적 단위에서입니다.
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) 높이 설정 하는 `BoxView`합니다.

`Color` 형식의 속성은 `Color`; 속성에 설정할 수 있습니다 `Color` 값을 사전순으로 까지의 색 라는 141 읽기 전용의 정적 필드를 포함 하 여 `AliceBlue` 를 `YellowGreen`합니다.

`WidthRequest` 및 `HeightRequest` 속성만 역할을 하는 경우는 `BoxView` 은 *무제한* 레이아웃에서 합니다. 레이아웃 컨테이너를 자식의, 예를 들어 시 크기를 알고 있어야 하는 경우이 대/소문자는 `BoxView` 의 자동 크기 셀 자식인는 `Grid` 레이아웃 합니다. A `BoxView` 하지 않으면 제약 조건이 지정 된 경우 해당 `HorizontalOptions` 및 `VerticalOptions` 속성이 아닌 다른 값으로 설정 된 `LayoutOptions.Fill`합니다. 경우는 `BoxView` , 제약을 받지 않는 있지만 `WidthRequest` 및 `HeightRequest` 속성이 설정 되지 않은 다음 너비 또는 높이가 40 단위 또는 약 1/4 인치 모바일 장치에서의 기본 값으로 설정 됩니다.

`WidthRequest` 및 `HeightRequest` 속성은 무시 됩니다는 `BoxView` 은 *제한* 경우 레이아웃 컨테이너 자체 크기에 부과 레이아웃에는 `BoxView`합니다.

A `BoxView` 에서 하나 이상의 차원을 제한 하 고 다른 제약 받지 않을 수 있습니다. 예를 들어 경우는 `BoxView` 세로의 자식인 `StackLayout`의 세로 크기는 `BoxView` 은 제약 없이 및 가로 크기는 일반적으로 제한 됩니다. 되지만 가로 해당 차원에 대 한 예외가: 경우는 `BoxView` 가 해당 `HorizontalOptions` 아닌 다른 값으로 설정 하는 속성 `LayoutOptions.Fill`, 가로 방향도 제한 되지 않습니다. 에 대 한도 가능는 `StackLayout` 자체는 쿼리에서 제한 되지 않은 가로 차원에는 `BoxView` 됩니다 가로 방향으로 제한 합니다.

[ **BasicBoxView** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView) 샘플 제약 없이 1-인치-사각형이 표시 `BoxView` 해당 페이지의 가운데에:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BasicBoxView"
             x:Class="BasicBoxView.MainPage">

    <BoxView Color="CornflowerBlue"
             WidthRequest="160"
             HeightRequest="160"
             VerticalOptions="Center"
             HorizontalOptions="Center" />

</ContentPage>
```

다음은 결과가입니다.

[![기본 BoxView](boxview-images/basicboxview-small.png "기본 BoxView")](boxview-images/basicboxview-large.png#lightbox "BasicBoxView")

경우는 `VerticalOptions` 및 `HorizontalOptions` 속성에서 제거 됩니다는 `BoxView` 태그 또는로 설정 `Fill`, 하면 `BoxView` 페이지의 크기에 의해 제한 되 고 페이지 크기에 맞게 확장 합니다.

A `BoxView` 의 자식이 될 수도 있습니다는 `AbsoluteLayout`합니다. 위치 및 크기의 경우는 `BoxView` 사용 하 여 설정 되는 `LayoutBounds` 연결 된 바인딩 가능한 속성입니다. `AbsoluteLayout` 문서에 설명 [ **AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md)합니다.

다음에 나오는 예제 프로그램에서는 이러한 모든 경우의 예에 표시 됩니다.

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>텍스트 장식 렌더링

사용할 수 있습니다는 `BoxView` 몇 가지 간단한 장식에서 페이지의 가로 및 세로줄이 표시 형식에 추가 합니다. [ **TextDecoration** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration) 이 샘플을 보여 줍니다. 에 정의 된 모든 프로그램의 시각적 개체는 **MainPage.xaml** 여러 포함 된 파일을 `Label` 및 `BoxView` 의 요소는 `StackLayout` 여기에 표시 된:

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

자식인 모든 태그 뒤에 `StackLayout`합니다. 이 태그 몇 가지 유형의 장식 이루어져 `BoxView` 와 사용 되는 요소는 `Label` 요소:

[![텍스트 장식](boxview-images/textdecoration-small.png "텍스트 장식")](boxview-images/textdecoration-large.png#lightbox "적용 되는 텍스트")

페이지 맨 위에 있는 세련 된 헤더를 통해는 `AbsoluteLayout` 자식을 네 가지 `BoxView` 요소와 `Label`모든를의 특정 위치 및 크기를 할당:

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

XAML 파일에는 `AbsoluteLayout` 나옵니다는 `Label` 설명 하는 텍스트 형식의로 `AbsoluteLayout`합니다.

텍스트 문자열 모두를 포함 하 여 밑줄을 넣을 수는 `Label` 및 `BoxView` 에 `StackLayout` 있는 해당 `HorizontalOptions` 값 이외의 값으로 설정 `Fill`합니다. 너비는 `StackLayout` 다음의 너비에 의해 관리는 `Label`, 그러면 해당 너비 설정는 `BoxView`합니다. `BoxView` 명시적 높이만 할당 됩니다.

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

텍스트 문자열이 긴 또는 단락 내에서 개별 단어를 밑줄을이 기법을 사용할 수 없습니다.

사용 하 여 이기도 한 `BoxView` HTML 유사 하 게 `hr` (단락 구분선) 요소입니다. 너비를 사용 하면는 `BoxView` 이 경우에 해당 부모 컨테이너에 의해 결정 된 `StackLayout`:

```xaml
<BoxView HeightRequest="3" />
```

둘 다 포함 하 여 텍스트 단락을의 한 쪽에 세로 선이 그릴 수는 마지막으로 `BoxView` 및 `Label` 를 수평 `StackLayout`합니다. 이 경우의 높이 `BoxView` 의 높이와 같습니다 `StackLayout`의 높이 의해 관리 하는 `Label`:

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
/StackLayout>
```
<a name="listingcolors" />

## <a name="listing-colors-with-boxview"></a>BoxView 된 색을 나열합니다.

`BoxView` 색을 표시 하기 위한 편리 합니다. 이 프로그램에서 사용 하는 `ListView` 모든 공개 읽기 전용의 정적 필드는 Xamarin.Forms 나열 `Color` 구조:

[![ListView 색](boxview-images/listviewcolors-small.png "ListView 색")](boxview-images/listviewcolors-large.png#lightbox "ListView 색")

[ **ListViewColors** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ListViewColors/) 라는 클래스를 포함 하는 프로그램 `NamedColor`합니다. 정적 생성자의 모든 필드에 액세스 하기 위해는 `Color` 만들고 구조는 `NamedColor` 각각에 대 한 개체입니다. 이러한 정적에 저장 됩니다 `All` 속성:

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

프로그램 시각적 개체는 XAML 파일에 설명 되어 있습니다. `ItemsSource` 속성의는 `ListView` static로 설정 되어 `NamedColor.All` 속성 즉는 `ListView` 모든 개별 표시 `NamedColor` 개체:

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

`NamedColor` 하 여 서식이 지정 된 개체는 `ViewCell` 의 데이터 템플릿으로 설정 된 개체는 `ListView`합니다. 이 템플릿에는 `BoxView` 인 `Color` 속성이 바인딩되는 `Color` 속성은 `NamedColor` 개체입니다.

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>수명 서브클래싱 BoxView 하 여 게임

수명 게임은 수학적 John Conway가 및의 페이지에 알려진 셀룰러 자동화 *과학 American* 는 1970 년대에 있습니다. 대 한 Wikipedia 문서에서 제공 [Conway 수명 게임](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)합니다.

Xamarin.Forms는 [ **GameOfLife** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/) 라는 클래스를 정의 하는 프로그램 `LifeCell` 에서 파생 된 `BoxView`합니다. 이 클래스 수명 게임의에서 개별 셀의 논리를 캡슐화합니다.

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

`LifeCell` 3 개 더 많은 속성을 추가 하는 `BoxView`:는 `Col` 및 `Row` 모눈에 있는 셀의 위치를 저장 하는 속성 및 `IsAlive` 속성의 상태를 나타냅니다. `IsAlive` 속성 설정도 `Color` 의 속성은 `BoxView` 셀 활성 상태가 아닌 경우 활성화 및 흰색 셀이 검정색으로 합니다.

`LifeCell` 또한 설치는 `TapGestureRecognizer` 을 탭 하 여 셀의 상태를 전환할 수 있도록 합니다. 변환 하는 클래스는 `Tapped` 이벤트 자체에 제스처 인식기에서 `Tapped` 이벤트입니다.

**GameOfLife** 프로그램도 포함 되어는 `LifeGrid` 의 많은 게임의 논리를 캡슐화 하는 클래스 및 `MainPage` 프로그램의 시각적 개체를 처리 하는 클래스입니다. 여기에 게임의 규칙을 설명 하는 오버레이 포함 됩니다. 다음 몇 백을 보여 주는 실행 프로그램은 `LifeCell` 페이지에 있는 개체:

[![수명 게임](boxview-images/gameoflife-small.png "수명 게임")](boxview-images/gameoflife-large.png#lightbox "수명 게임")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>디지털 클록 만들기

[ **DotMatrixClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock/) 프로그램 만듭니다 210 `BoxView` 요소를 사용 하는 구식 5-7 도트 디스플레이의 점 시뮬레이션 합니다. 세로 또는 가로 모드의 시간을 읽을 수는 있지만 가로 크기:

[![도트 클록](boxview-images/dotmatrixclock-small.png "도트 클록")](boxview-images/dotmatrixclock-large.png#lightbox "도트 클록")

XAML 파일을 인스턴스화할 수 약는 `AbsoluteLayout` 클록에 사용:

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

다른 모든 항목 코드 숨김 파일에서 발생합니다. 도트 디스플레이 논리의 10 자리 숫자 및 콜론 각에 해당 하는 점을 설명 하는 여러 배열 정의 의해 상당히 단순화 되었지만:

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

이러한 필드는 3 차원 배열을 사용 하 여 결론을 내릴 `BoxView` 자릿수 6 개에 대 한 점 패턴을 저장 하기 위한 요소입니다.

생성자는 모든 만듭니다는 `BoxView` 는 숫자 및 콜론과 초기화에 대 한 요소는 `Color` 의 속성은 `BoxView` 콜론에 대 한 요소:

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

이 프로그램에 상대 위치 및 크기 조정 기능을 사용 하 여 `AbsoluteLayout`합니다. 너비와 높이는 각 `BoxView` 소수 자릿수 값, 특히 85 %1의 가로 및 세로 점 수로 나눈 값으로 설정 됩니다. 위치 소수 값으로 설정 됩니다.

모든 위치 및 크기의 전체 크기를 기준으로 때문에 `AbsoluteLayout`, `SizeChanged` 페이지에 대 한 처리기만 설정할 필요는 `HeightRequest` 의 `AbsoluteLayout`:

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

너비는 `AbsoluteLayout` 페이지의 전체 너비로 확장 때문에 자동으로 설정 됩니다.

마지막 코드는 `MainPage` 클래스 타이머 콜백을 처리 하 고 각 숫자의 점 색을 지정 합니다. 코드 숨김 파일의 시작 부분에서 다차원 배열의 정의이 논리는 간단 프로그램을 확인 하는 데 도움이 됩니다.

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

## <a name="creating-an-analog-clock"></a>아날로그 시계의 만들기

도트 클록의 응용 프로그램 명백한 것 처럼 보일 수 `BoxView`, 하지만 `BoxView` 요소 아날로그 시계의 인식 수도 있습니다.

[![BoxView 클록](boxview-images/boxviewclock-small.png "BoxView 클록")](boxview-images/boxviewclock-large.png#lightbox "BoxView 클록")

모든 시각적 개체는 [ **BoxViewClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/) 프로그램 자식 요소인는 `AbsoluteLayout`합니다. 사용 하 여 이러한 요소는 크기는 `LayoutBounds` 연결 된 속성을 사용 하 여 회전 및는 `Rotation` 속성입니다.

세 개의 `BoxView` 시계 바늘에 대 한 요소는 XAML 파일에서 인스턴스화될 배치 또는 안 크기:

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

코드 숨김 파일 생성자는 60 `BoxView` 클록의 둘레의 눈금 표시에 대 한 요소:

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

모든 배치 및 크기 조정은 `BoxView` 요소에서 발생 된 `SizeChanged` 에 대 한 처리기는 `AbsoluteLayout`합니다. 클래스의 내부 약간 구조 호출 `HandParams` 각 클록의 총 크기를 기준으로 세 포인터의 크기를 설명 합니다.

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

`SizeChanged` 중심과의 반지름을 결정 하는 처리기는 `AbsoluteLayout`, 다음의 크기를 조정 하 고는 60 배치 `BoxView` 눈금 표시로 사용 되는 요소입니다. `for` 설정 하 여 루프 종료는 `Rotation` 속성 각각의 `BoxView` 요소입니다. 끝에는 `SizeChanged` 처리기는 `LayoutHand` 메서드는 크기와 위치는 세 가지 시계 바늘을:

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

`LayoutHand` 메서드는 크기를 조정 하 고 12시 위치까지 바로 가리키도록 각 포인터를 놓습니다. 메서드의 끝에는 `AnchorY` 속성을 해당 하는 클록의 가운데 위치로 설정 합니다. 회전 중심을 나타냅니다.

바늘은 타이머 콜백 함수에서 회전 됩니다.

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

두 번째 손 모양 아이콘이 약간 다르게 처리 됩니다: 감속/가속 함수 애니메이션 울 이동 기계적 보다는 만들기 부드러운에 적용 됩니다. 각 단위를 두 번째 손 모양 아이콘이 약간 가져오며 다시 다음 짧게 대상 표시 되 면 합니다. 이 약간의 코드 이동 현실감을 많이 추가합니다.

## <a name="conclusion"></a>결론

`BoxView` 것 처럼 보일 수에 간단한 먼저 하지만 때 다양 한 용도로 수 있으며 수 거의 재현 시각적 효과 정상적으로 에서만 가능 벡터 그래픽 보았으며 합니다. 보다 정교한 그래픽을 참조 하십시오. [xamarin.forms에서를 사용 하 여 SkiaSharp](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)합니다.


## <a name="related-links"></a>관련 링크

- [기본 BoxView (샘플)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)
- [텍스트 장식 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)
- [색 ListBox (샘플)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox)
- [게임 수명 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)
- [도트 클록 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock)
- [BoxView 클록 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock)
- [BoxView](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)
