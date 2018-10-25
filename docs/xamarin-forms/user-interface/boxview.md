---
title: Xamarin.Forms BoxView
description: 이 문서에 장식, 그래픽 및 Xamarin.Forms 응용 프로그램에서 상호 작용에 대 한 색이 지정 된 사각형을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/26/2018
ms.openlocfilehash: 3ae2fb8110b7e0a5c6c85c489897acc1a03be8d8
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "38997054"
---
# <a name="xamarinforms-boxview"></a>Xamarin.Forms BoxView

[`BoxView`](xref:Xamarin.Forms.BoxView) 지정 된 너비, 높이 및 색상의 간단한 사각형을 렌더링합니다. 사용할 수 있습니다 `BoxView` 장식, 기본적인 그래픽 및 터치를 통해 사용자와의 상호 작용 합니다.

Xamarin.Forms에 기본 제공 벡터 그래픽 시스템에 없기 때문에 `BoxView` 보정 하는 데 도움이 됩니다. 이 문서 사용에 설명 된 샘플 프로그램 중 일부 `BoxView` 그래픽 렌더링에 대 한 합니다. `BoxView` 특정 폭과 두께의 줄을 유사 하 게 크기가 정 해지고 사용 하 여 모든 각도 회전할 수는 `Rotation` 속성입니다.

하지만 `BoxView` 간단한 그래픽을 모방 하면, 조사 하는 것이 좋습니다 [Xamarin.Forms에서 SkiaSharp 사용 하 여](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) 보다 정교한 그래픽 요구 사항에 대 한 합니다.

이 문서에서는 다음 내용을 다룹니다.

- **[BoxView 색과 크기를 설정](#colorandsize)**  &ndash; 설정의 `BoxView` 속성입니다.
- **[렌더링 텍스트 장식을](#textdecorations)**  &ndash; 사용 하 여는 `BoxView` 렌더링 줄에 대 한 합니다.
- **[BoxView 색 나열](#listingcolors)**  &ndash; 의 시스템 색이 모두 표시는 `ListView`합니다.
- **[게임의 수명 동안 서브클래싱 BoxView에서 재생](#subclassing)**  &ndash; 유명한 셀룰러 automaton를 구현 합니다.
- **[디지털 시계를 만드는](#digitalclock)**  &ndash; 도트 표시를 시뮬레이션 합니다.
- **[만들기는 아날로그 클록](#analogclock)**  &ndash; 변환한에 애니메이션 효과 주기 `BoxView` 요소입니다.

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>설정 BoxView 색 및 크기

다음 속성을 설정 하면 일반적으로 `BoxView`:

- [`Color`](xref:Xamarin.Forms.BoxView.Color) 색을 설정 합니다.
- [`CornerRadius`](xref:Xamarin.Forms.BoxView.CornerRadius) 모퉁이 반지름을 설정 합니다.
- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 너비를 설정 하 여 `BoxView` 장치 독립적 단위에서입니다.
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 높이 설정 하 여 `BoxView`입니다.

`Color` 형식의 속성은 `Color`; 속성에 설정할 수 있습니다 `Color` 값 141 정적 읽기 전용 필드를 포함 하 여 명명 된 색에서 사전순으로 이르기까지 `AliceBlue` 에 `YellowGreen`입니다.

합니다 `CornerRadius` 형식의 속성은 [ `CornerRadius` ](xref:Xamarin.Forms.CornerRadius); 단일 속성을 설정할 수 있습니다 `double` 모퉁이 반지름 값, uniform 또는 `CornerRadius` 4 정의한 구조 `double` 에 적용 되는 값 왼쪽, 오른쪽 위 위쪽, 아래쪽, 왼쪽 및 오른쪽 아래를 `BoxView`입니다.

`WidthRequest` 및 `HeightRequest` 속성만 역할을 하는 경우를 `BoxView` 됩니다 *비제한* 레이아웃에서. 레이아웃 컨테이너 자식의 크기, 예를 들어 때 알아야 할 때이 경우는 `BoxView` 자식인의 자동 크기 셀을 `Grid` 레이아웃. A `BoxView` 도 제한 된 경우 해당 `HorizontalOptions` 하 고 `VerticalOptions` 속성 이외의 값으로 설정 됩니다 `LayoutOptions.Fill`합니다. 경우는 `BoxView` 에 제한 되지 않지만 `WidthRequest` 및 `HeightRequest` 속성을 설정 하지는 않으면 너비 또는 높이가 40 개 단위 또는 모바일 장치에서 약 1/4 인치의 기본 값으로 설정 됩니다.

`WidthRequest` 및 `HeightRequest` 경우 속성은 무시 됩니다는 `BoxView` 는 *제한* 레이아웃으로 경우 레이아웃 컨테이너 자체 크기에 적용 합니다 `BoxView`합니다.

`BoxView` 하나 이상의 차원을 제한 되며 다른 비제한 수 있습니다. 예를 들어 경우는 `BoxView` 세로의 자식인 `StackLayout`의 수직 크기는 `BoxView` 은 비제한 및 해당 가로 방향 일반적으로 제한 됩니다. 가로 해당 차원에 대 한 예외 되지만: 경우 합니다 `BoxView` 에 해당 `HorizontalOptions` 이외의 값으로 설정 하는 속성 `LayoutOptions.Fill`를 가로 방향도 제한 되지 않습니다. 수 이기도 합니다 `StackLayout` 자체는 경우에 제한 되지 않은 가로 방향에는 `BoxView` 됩니다 가로 방향으로 제한 합니다.

합니다 [ **BasicBoxView** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView) 비제한 하나-인치-사각형을 표시 하는 샘플 `BoxView` 해당 페이지의 가운데에:

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

결과 다음과 같습니다.

[![기본 BoxView](boxview-images/basicboxview-small.png "기본 BoxView")](boxview-images/basicboxview-large.png#lightbox "BasicBoxView")

경우는 `VerticalOptions` 및 `HorizontalOptions` 속성에서 제거 됩니다 합니다 `BoxView` 태그 또는로 설정 됩니다 `Fill`, 해당 `BoxView` 페이지의 크기에 따라 제한 됩니다 및 페이지 크기에 맞게 확장 합니다.

A `BoxView` 자식의 수도 있습니다는 `AbsoluteLayout`합니다. 위치와 크기의 경우에서는 `BoxView` 을 설정 하는 `LayoutBounds` 바인딩 가능 속성을 연결 합니다. 합니다 `AbsoluteLayout` 문서에서 설명 됩니다 [ **AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md)합니다.

다음 예제 프로그램에서는 이러한 모든 경우의 예를 볼 수 있습니다.

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>텍스트 장식을 렌더링

사용할 수는 `BoxView` 가로선과 세로선의 형태로 페이지에 몇 가지 간단한 장식을 추가 합니다. 합니다 [ **TextDecoration** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration) 샘플에서는이 방법을 보여 줍니다. 에 정의 된 모든 프로그램의 시각적 개체를 **MainPage.xaml** 몇 가지를 포함 하는 파일 `Label` 및 `BoxView` 요소에는 `StackLayout` 다음과 같습니다:

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

자식인 모든 뒤에 오는 태그의 `StackLayout`합니다. 이 태그는 여러 유형의 장식 구성 되어 있습니다 `BoxView` 사용 하는 요소는 `Label` 요소:

[![텍스트 장식](boxview-images/textdecoration-small.png "텍스트 장식")](boxview-images/textdecoration-large.png#lightbox "텍스트 장식")

페이지의 맨 위에 있는 세련 된 헤더를 통해는 `AbsoluteLayout` 자식을 네 가지 `BoxView` 요소 및 `Label`모든를의 특정 위치 및 크기를 할당 합니다.

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

XAML 파일에는 `AbsoluteLayout` 뒤에 `Label` 여 설명 하는 텍스트 형식으로 `AbsoluteLayout`합니다.

텍스트 문자열을 모두 묶어 밑줄을 넣을 수는 `Label` 및 `BoxView` 에 `StackLayout` 있는 해당 `HorizontalOptions` 이외의 값으로 설정 `Fill`합니다. 너비를 `StackLayout` 의 너비를 따릅니다를 `Label`, 그러면에 너비를 적용 합니다 `BoxView`합니다. `BoxView` 는 명시적 높이만 할당 됩니다.

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

더 긴 텍스트 문자열 또는 단락 내에서 개별 단어를 밑줄로이 기술을 사용할 수 없습니다.

것도 사용할 수는 `BoxView` HTML와 유사 하 게 `hr` (가로줄) 요소입니다. 단순히의 너비를 사용 합니다 `BoxView` 이 경우에 해당 부모 컨테이너에서 확인할 수는 `StackLayout`:

```xaml
<BoxView HeightRequest="3" />
```

마지막으로 그릴 수 있습니다 세로 선이 한쪽 텍스트 단락을 모두 묶어 합니다 `BoxView` 및 `Label` 를 수평 `StackLayout`. 이 예에서 높이 `BoxView` 의 높이와 같습니다 `StackLayout`의 높이 의해 관리 됩니다는 `Label`:

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

## <a name="listing-colors-with-boxview"></a>BoxView 색 나열

`BoxView` 색을 표시 하는 데 유용 합니다. 이 프로그램이 사용 하는 `ListView` 모든 public static 읽기 전용 필드 Xamarin.Forms의 목록에 `Color` 구조:

[![ListView 색](boxview-images/listviewcolors-small.png "ListView 색")](boxview-images/listviewcolors-large.png#lightbox "ListView 색")

합니다 [ **ListViewColors** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ListViewColors/) 라는 클래스를 포함 하는 프로그램 `NamedColor`합니다. 정적 생성자의 모든 필드에 액세스 하기 위해 합니다 `Color` 만들어 구조체는 `NamedColor` 각각에 대 한 개체입니다. 정적 저장 됩니다 `All` 속성:

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

프로그램 시각적 개체는 XAML 파일에 설명 되어 있습니다. `ItemsSource` 의 속성을 `ListView` 정적으로 설정 됩니다 `NamedColor.All` 즉 속성을를 `ListView` 모든 개별 표시 `NamedColor` 개체:

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

`NamedColor` 에서 개체는 서식이 지정 합니다 `ViewCell` 의 데이터 템플릿으로 설정 된 개체는 `ListView`. 이 템플릿에를 `BoxView` 해당 `Color` 속성이 바인딩되는 `Color` 의 속성은 `NamedColor` 개체.

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>서브클래싱 BoxView 여 수명의 게임 실행

수명 게임은 수학적 John Conway 프랑스인이 발명 한 페이지에서 잘 알려진를 셀룰러 automaton *과학 American* 1970 년대에 있습니다. Wikipedia 문서에서 제공 하는 데 유용한 정보 [Conway's 게임의 수명](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)합니다.

Xamarin.Forms [ **GameOfLife** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/) 라는 클래스를 정의 하는 프로그램 `LifeCell` 에서 파생 되는 `BoxView`합니다. 이 클래스의 게임의 개별 셀의 논리를 캡슐화합니다.

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

`LifeCell` 세 개의 자세한 속성을 추가 `BoxView`: 합니다 `Col` 및 `Row` 그리드 내에 있는 셀의 위치를 저장 하는 속성 및 `IsAlive` 속성 상태를 나타냅니다. `IsAlive` 속성 설정 합니다 `Color` 의 속성은 `BoxView` 검정색 셀이 활성 상태로 유지 하 고 흰색 셀 활성 상태가 아닌 경우.

`LifeCell` 또한 설치를 `TapGestureRecognizer` 을 탭 하 여 셀의 상태를 전환할 수 있도록 합니다. 변환 하는 클래스를 `Tapped` 자체에 제스처 인식기에서 이벤트 `Tapped` 이벤트입니다.

**GameOfLife** 프로그램 또한를 `LifeGrid` 대부분의 게임 논리를 캡슐화 하는 클래스 및 `MainPage` 프로그램의 시각적 개체를 처리 하는 클래스입니다. 여기에 게임의 규칙을 설명 하는 오버레이 포함 됩니다. 여기 몇 백을 보여 주는 실행 중인 프로그램이 `LifeCell` 페이지에 있는 개체:

[![게임 수명의](boxview-images/gameoflife-small.png "수명의 게임")](boxview-images/gameoflife-large.png#lightbox "수명의 게임")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>디지털 시계가 만들기

[ **DotMatrixClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock/) 프로그램 만듭니다 210 `BoxView` 요소를 사용 하는 구식 5-7 도트 표시의 점을 시뮬레이션 합니다. 에 세로 또는 가로 모드에서 읽을 수는 있지만 환경에서 더 큰 것:

[![도트 클록](boxview-images/dotmatrixclock-small.png "도트 클록")](boxview-images/dotmatrixclock-large.png#lightbox "도트 시계")

XAML 파일을 약간 넘는 인스턴스화하는 `AbsoluteLayout` 시계를 사용 합니다.

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

나머지 코드 숨김 파일에서 발생합니다. 도트 표시 논리의 10 자리 숫자 및 콜론 각각에 해당 하는 점을 설명 하는 여러 배열 정의에서 간단 합니다.

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

이러한 필드는 3 차원 배열을 사용 하 여 종료 `BoxView` 자릿수 6 개에 대 한 점 패턴 저장에 대 한 요소입니다.

생성자는 모든는 `BoxView` 숫자 및 콜론, 고도 초기화에 대 한 요소를 `Color` 의 속성을 `BoxView` 콜론 요소:

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

이 프로그램에서는 상대 위치 및 크기 조정 기능의 `AbsoluteLayout`합니다. 각각의 높이 너비 `BoxView` 가로 및 세로 점선의 수로 나눈 값 1의 85% 특히 소수 값으로 설정 됩니다. 위치를 소수 값으로 설정 됩니다.

모든 위치 및 크기의 총 크기를 기준으로 되므로 합니다 `AbsoluteLayout`는 `SizeChanged` 페이지에 대 한 처리기만 설정 해야를 `HeightRequest` 의 `AbsoluteLayout`:

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

너비는 `AbsoluteLayout` 페이지의 전체 너비까지 확장 되 면 자동으로 설정 됩니다.

최종 코드는 `MainPage` 클래스 타이머 콜백이 처리 하 고 각 숫자의 점 색입니다. 코드 숨김 파일의 시작 부분에서 다차원 배열의 정의 간단 하지만 프로그램의이 논리를 확인 하는 데 도움이 됩니다.

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

도트 클록의 확실 한 응용 프로그램 보일 수 있습니다 `BoxView`, 하지만 `BoxView` 요소 아날로그 시계의 인식 수도 있습니다.

[![BoxView 클록](boxview-images/boxviewclock-small.png "BoxView 클록")](boxview-images/boxviewclock-large.png#lightbox "BoxView 시계")

모든 시각적 개체를 [ **BoxViewClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/) 프로그램의 자식인는 `AbsoluteLayout`합니다. 이러한 요소를 사용 하 여 크기가 조정 되는 `LayoutBounds` 연결 된 속성 및 사용 하 여 회전 된 `Rotation` 속성입니다.

세 가지 `BoxView` 시계 바늘 요소는 XAML 파일에서 인스턴스화된 있지만 배치 또는 크기 조정:

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

코드 숨김 파일의 생성자를 인스턴스화하는 60 `BoxView` 시계의 둘레 눈금에 대 한 요소:

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

모든 배치 및 크기 조정 합니다 `BoxView` 요소에서 발생 합니다 `SizeChanged` 에 대 한 처리기를 `AbsoluteLayout`입니다. 구조 클래스의 내부 호출 `HandParams` 각 클록의 총 크기를 기준으로 세 가지 실습의 크기를 설명 합니다.

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

합니다 `SizeChanged` center 및 둥근 모퉁이의 반지름을 결정 하는 처리기는 `AbsoluteLayout`, 다음 크기 및 배치를 60 `BoxView` 눈금 표시로 사용 되는 요소입니다. `for` 루프를 설정 하 여 완료 합니다 `Rotation` 이러한 각 속성 `BoxView` 요소입니다. 끝에 `SizeChanged` 처리기는 `LayoutHand` 크기와 위치는 세 가지 시계 바늘 메서드는:

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

`LayoutHand` 메서드 크기 및 12:00 위치까지 바로 가리키도록 각 포인터를 배치 합니다. 메서드의 끝에는 `AnchorY` 시계의 센터로 해당 위치로 설정 합니다. 회전 중심을 나타냅니다.

바늘은 타이머 콜백 함수에서 회전 합니다.

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

초침 약간 다르게 처리 됩니다: 부드러운 대신 기계적 보일 이동 하도록 애니메이션 감속/가속 함수 적용 됩니다. 각 틱에를 초침 약간 다시 가져온 다음 대상 짧게 표시 되 면 합니다. 이 약간의 코드 이동 현실감을 많이 추가합니다.

## <a name="conclusion"></a>결론

`BoxView` 첫째, 하지만 때에 간단한 보일 수 다양 한 용도로 수 및 수 거의 재현 하는 시각적 개체 에서만 일반적으로 가능한 벡터 그래픽 살펴봤습니다. 고급 그래픽에 대 한 참조 [Xamarin.Forms에서 SkiaSharp 사용 하 여](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)입니다.


## <a name="related-links"></a>관련 링크

- [기본 BoxView (샘플)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)
- [텍스트 장식 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)
- [색 ListBox (샘플)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox)
- [게임 수명 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)
- [도트 클럭 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock)
- [BoxView 클록 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock)
- [BoxView](xref:Xamarin.Forms.BoxView)
