---
title: 'Xamarin.ios c # 태그'
description: 'C # 태그는 c #에서 선언적 Xamarin.ios 사용자 인터페이스를 빌드하는 프로세스를 간소화 하는 흐름 도우미 메서드 및 클래스의 옵트인 (opt in) 집합입니다.'
ms.prod: xamarin
ms.assetid: D41B9DCD-5C34-4C2F-B177-FC082AB2E9E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/25/2020
ms.openlocfilehash: fa758b1240570f90ebf8a723401176f6be9dd6ac
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82532876"
---
# <a name="xamarinforms-c-markup"></a>Xamarin.ios c # 태그

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-csharpmarkupdemos/)

C # 태그는 c #에서 선언적 Xamarin.ios 사용자 인터페이스를 빌드하는 프로세스를 간소화 하는 흐름 도우미 메서드 및 클래스의 옵트인 (opt in) 집합입니다. C # 태그에서 제공 되는 흐름 API는 `Xamarin.Forms.Markup` 네임 스페이스에서 사용할 수 있습니다.

XAML과 마찬가지로 c # 태그를 사용 하면 UI 태그와 UI 논리를 명확 하 게 구분할 수 있습니다. 이는 UI 태그와 UI 논리를 고유한 partial 클래스 파일로 분리 하 여 수행할 수 있습니다. 예를 들어 로그인 페이지의 경우 ui 태그는 이름이 *LoginPage.cs*인 파일에 있으며 ui 논리는 *LoginPage.logic.cs*라는 파일에 있습니다.

C # 태그는 Xamarin.ios 4.6에서 사용할 수 있습니다. 그러나 현재 실험적 이며 *App.cs* 파일에 다음 코드 줄을 추가 하 여 사용할 수 있습니다.

```csharp
Device.SetFlags(new string[]{ "Markup_Experimental" });
```

> [!NOTE]
> C # 태그는 Xamarin.ios에서 지 원하는 모든 플랫폼에서 사용할 수 있습니다.

## <a name="basic-example"></a>기본 예제

다음 예제에서는 c #에서 [`Entry`](xref:Xamarin.Forms.Entry) 개체를 만드는 방법을 보여 줍니다.

```csharp
Entry entry = new Entry
{
    Placeholder = "Enter number",
    Keyboard = Keyboard.Numeric,
    BackgroundColor = Color.AliceBlue,
    TextColor = Color.Black,
    FontSize = 15,
    HeightRequest = 44,
    Margin = fieldMargin
};
entry.SetBinding(Entry.TextProperty, new Binding("RegistrationCode", BindingMode.TwoWay));
grid.Children.Add(entry, 0, 2);
Grid.SetColumnSpan(entry, 2);
```

이 예제에서는 [`Entry`](xref:Xamarin.Forms.Entry) `TwoWay` 바인딩을 사용 하 여 데이터를 viewmodel `RegistrationCode` 의 속성에 바인딩하는 개체를 만듭니다. 의 특정 행 [`Grid`](xref:Xamarin.Forms.Grid)에 표시 되 고의 모든 열에 걸쳐 표시 되도록 설정 `Grid`됩니다. 또한의 높이는 텍스트의 글꼴 `Entry` 크기와 함께 설정 됩니다 `Margin`.

C # 태그를 사용 하면 흐름 API를 사용 하 여이 코드를 다시 작성할 수 있습니다.

```csharp
using Xamarin.Forms.Markup;
using static Xamarin.Forms.Markup.GridRowsColumns;

Entry entry = new Entry { Placeholder = "Enter number", Keyboard = Keyboard.Numeric, BackgroundColor = Color.AliceBlue, TextColor = Color.Black } .Font (15)
                         .Row (BodyRow.CodeEntry) .ColumnSpan (All<BodyCol>()) .Margin (fieldMargin) .Height (44)
                         .Bind (nameof(vm.RegistrationCode), BindingMode.TwoWay);
```

이 예제는 이전 예제와 동일 하지만 c # 태그 흐름 API는 c #에서 UI를 빌드하는 프로세스를 간소화 합니다.

> [!NOTE]
> C # 태그는 특정 뷰 속성을 설정 하는 확장 메서드를 포함 합니다. 이러한 확장 메서드는 모든 속성 setter를 대체 하기 위한 것이 아닙니다. 대신, 코드 가독성을 향상 시키기 위해 디자인 되었으며 속성 setter와 함께 사용할 수 있습니다. 속성에 대해 항상 확장 메서드를 사용 하는 것이 좋지만 기본 잔액을 선택할 수 있습니다.

## <a name="data-binding"></a>데이터 바인딩

C # 태그에 `Bind` 는 바인딩 가능한 속성 및 지정 된 속성 간에 데이터 바인딩을 만드는 오버 로드와 함께 확장 메서드가 포함 되어 있습니다. 메서드 `Bind` 는 xamarin.ios에 포함 된 대부분의 컨트롤에 대 한 기본 바인딩 가능 속성을 알고 있습니다. 따라서 일반적으로이 메서드를 사용 하는 경우 대상 속성을 지정할 필요가 없습니다. 그러나 다음과 같은 추가 컨트롤에 대 한 기본 바인딩 가능 속성을 등록할 수도 있습니다.

```csharp
using Xamarin.Forms.Markup;
//...

DefaultBindableProperties.Register(HoverButton.CommandProperty, RadialGauge.ValueProperty);
```

`Bind` 메서드를 사용 하 여 바인딩 가능한 속성에 바인딩할 수 있습니다.

```csharp
using Xamarin.Forms.Markup;
// ...

new Label { Text = "No data available" }
           .Bind (Label.IsVisibleProperty, nameof(vm.Empty))
```

또한 확장 메서드는 `BindCommand` 단일 메서드 호출에서 컨트롤의 기본 `Command` 및 `CommandParameter` 속성에 바인딩할 수 있습니다.

```csharp
using Xamarin.Forms.Markup;
// ...

new TextCell { Text = "Tap me" }
              .BindCommand (nameof(vm.TapCommand))
```

기본적으로는 `CommandParameter` 바인딩 컨텍스트에 바인딩됩니다. `Command` 및 `CommandParameter` 바인딩에 대해 바인딩 경로와 소스를 지정할 수도 있습니다.

```csharp
using Xamarin.Forms.Markup;
// ...

new TextCell { Text = "Tap Me" }
              .BindCommand (nameof(vm.TapCommand), vm, nameof(Item.Id))
```

이 예제에서 바인딩 컨텍스트는 `Item` 인스턴스이며 `Id` `CommandParameter` 바인딩에 대 한 소스를 지정할 필요가 없습니다.

에 `Command`바인딩하려는 경우에는 `null` `parameterPath` `BindCommand` 메서드의 인수에를 전달할 수 있습니다. 또는 `Bind` 메서드를 사용 합니다.

추가 컨트롤의 기본 `Command` 및 `CommandParameter` 속성을 등록할 수도 있습니다.

```csharp
using Xamarin.Forms.Markup;
//...

DefaultBindableProperties.RegisterCommand(
    (CustomViewA.CommandProperty, CustomViewA.CommandParameterProperty),
    (CustomViewB.CommandProperty, CustomViewB.CommandParameterProperty)
);
```

인라인 변환기 코드는 `Bind` `convert` 및 `convertBack` 매개 변수를 사용 하 여 메서드에 전달할 수 있습니다.

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tree" }
           .Bind (Label.MarginProperty, nameof(TreeNode.TreeDepth),
                  convert: (int depth) => new Thickness(depth * 20, 0, 0, 0))
```

또한 형식 안전 변환기 매개 변수도 지원 됩니다.

```csharp
using Xamarin.Forms.Markup;
//...

new Label { }
           .Bind (nameof(viewModel.Text),
                  convert: (string text, int repeat) => string.Concat(Enumerable.Repeat(text, repeat)))
```

또한 다음과 같이 변환기 코드 및 인스턴스를 `FuncConverter` 클래스와 함께 다시 사용할 수 있습니다.

```csharp
using Xamarin.Forms.Markup;
//...

FuncConverter<int, Thickness> treeMarginConverter = new FuncConverter<int, Thickness>(depth => new Thickness(depth * 20, 0, 0, 0));
new Label { Text = "Tree" }
           .Bind (Label.MarginProperty, nameof(TreeNode.TreeDepth), converter: treeMarginConverter),
```

클래스 `FuncConverter` 는 개체도 `CultureInfo` 지원 합니다.

```csharp
using Xamarin.Forms.Markup;
//...

cultureAwareConverter = new FuncConverter<DateTimeOffset, string, int>(
    (date, daysToAdd, culture) => date.AddDays(daysToAdd).ToString(culture)
);
```

또한 `FormattedText` 속성을 사용 하 여 지정 된 `Span` 개체에 데이터 바인딩할 수 있습니다.

```csharp
using Xamarin.Forms.Markup;
//...

new Label { } .FormattedText (
    new Span { Text = "Built with " },
    new Span { TextColor = Color.Blue, TextDecorations = TextDecorations.Underline }
              .BindTapGesture (nameof(vm.ContinueToCSharpForMarkupCommand))
              .Bind (nameof(vm.Title))
)
```

### <a name="gesture-recognizers"></a>제스처 인식기

`Command`및 `CommandParameter` 속성은 `BindClickGesture`, `BindSwipeGesture`및 `BindTapGesture` 확장 메서드 `GestureElement` 를 사용 하 여 및 `View` 형식에 바인딩될 수 있습니다.

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tap Me" }
           .BindTapGesture (nameof(vm.TapCommand))
```

이 예제에서는 지정 된 형식의 제스처 인식기를 만들어에 추가 [`Label`](xref:Xamarin.Forms.Label)합니다. `Bind*Gesture` 확장 메서드는 `BindCommand` 확장 메서드와 동일한 매개 변수를 제공 합니다. 그러나는 기본적 `Bind*Gesture` 으로 바인딩하지 `CommandParameter`않지만는 `BindCommand` 그렇지 않습니다.

매개 변수를 사용 하 여 제스처 인식기를 초기화 `ClickGesture`하려면 `PanGesture`, `PinchGesture`, `SwipeGesture`, 및 `TapGesture` 확장 메서드를 사용 합니다.

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tap Me" }
           .TapGesture (g => g.Bind(nameof(vm.DoubleTapCommand)).NumberOfTapsRequired = 2)
```

제스처 인식기 `BindableObject`는 이기 때문에 초기화할 때 `Bind` 및 `BindCommand` 확장 메서드를 사용할 수 있습니다. 또한 `Gesture<TGestureElement, TGestureRecognizer>` 확장 메서드를 사용 하 여 사용자 지정 제스처 인식기 형식을 초기화할 수 있습니다.

## <a name="layout"></a>레이아웃

C # 태그에는 레이아웃의 위치 지정 및 뷰의 콘텐츠를 지 원하는 일련의 레이아웃 확장 메서드가 포함 되어 있습니다.

| Type | 확장 메서드 |
|---|---|
| `FlexLayout` | `AlignSelf`, `Basis`, `Grow`, `Menu`, `Order`, `Shrink` |
| `Grid` | `Row`, `Column`, `RowSpan`, `ColumnSpan` |
| `Label` | `TextLeft`, `TextCenterHorizontal`, `TextRight` <br/> `TextTop`, `TextCenterVertical`, `TextBottom` <br/> `TextCenter` |
| `Layout` | `Padding`, `Paddings` |
| `LayoutOptions` | `Left`, `CenterHorizontal`, `FillHorizontal`, `Right` <br/> `LeftExpand`, `CenterExpandHorizontal`, `FillExpandHorizontal`, `RightExpand` <br /> `Top`, `Bottom`, `CenterVertical`, `FillVertical` <br /> `TopExpand`, `BottomExpand`, `CenterExpandVertical`, `FillExpandVertical` <br /> `Center`, `Fill`, `CenterExpand`, `FillExpand` |
| `View` | `Margin`, `Margins` |
| `VisualElement` | `Height`, `Width`, `MinHeight`, `MinWidth`, `Size`, `MinSize` |

### <a name="left-to-right-and-right-to-left-support"></a>왼쪽에서 오른쪽 및 오른쪽에서 왼쪽으로의 지원

왼쪽에서 오른쪽 (LTR) 또는 오른쪽에서 왼쪽 (RTL) 흐름 방향을 지원 하도록 설계 된 c # 태그의 경우 위에 나열 된 확장 메서드는 가장 직관적인 `Left`이름 집합을 `Right`제공 합니다., `Top` 및. `Bottom`

왼쪽 및 오른쪽 확장 메서드의 올바른 집합을 사용할 수 있도록 하 고 프로세스에서 태그가 디자인 되는 흐름 방향을 명시적으로 지정 하려면 다음 두 `using` 지시문 중 하나를 포함 합니다. `using Xamarin.Forms.Markup.LeftToRight;`또는. `using Xamarin.Forms.Markup.RightToLeft;`

왼쪽에서 오른쪽 및 오른쪽에서 왼쪽 흐름 방향을 모두 지원 하도록 설계 된 c # 태그의 경우 위의 네임 스페이스 중 하나가 아니라 다음 표에 있는 확장 메서드를 사용 하는 것이 좋습니다.

| Type | 확장 메서드 |
|---|---|
| `Label` | `TextStart`, `TextEnd` |
| `LayoutOptions` | `Start`, `End` <br/> `StartExpand`, `EndExpand` |

### <a name="layout-line-convention"></a>레이아웃 선 규칙

보기에 대 한 모든 레이아웃 확장 메서드를 다음 순서로 한 줄에 배치 하는 것이 좋습니다.

1. 뷰를 포함 하는 행과 열입니다.
1. 행과 열 내의 맞춤입니다.
1. 보기 주위의 여백입니다.
1. 보기 크기입니다.
1. 뷰 내의 안쪽 여백입니다.
1. 안쪽 여백 내의 내용 맞춤입니다.

다음 코드에서는이 규칙의 예를 보여 줍니다.

```csharp
new Label { }
           .Row (BodyRow.Prompt) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .CenterVertical () .Margin (fieldNameMargin) .TextCenterHorizontal () // Layout line
```

규칙을 지속적으로 따라 신속 하 게 c # 태그를 읽고 보기 콘텐츠가 UI에 있는 멘 탈 지도를 빌드할 수 있습니다.

## <a name="grid-rows-and-columns"></a>Grid 행 및 열

숫자를 사용 하는 대신 [`Grid`](xref:Xamarin.Forms.Grid) 열거형을 사용 하 여 행과 열을 정의할 수 있습니다. 이를 통해 행 또는 열을 추가 하거나 제거할 때 번호를 다시 매길 필요가 없다는 장점이 있습니다.

> [!IMPORTANT]
> 열거형 [`Grid`](xref:Xamarin.Forms.Grid) 을 사용 하 여 행과 열을 `using` 정의 하려면 다음 지시문이 필요 합니다.`using static Xamarin.Forms.Markup.GridRowsColumns;`

다음 코드는 열거형을 사용 하 여 [`Grid`](xref:Xamarin.Forms.Grid) 행과 열을 정의 하 고 사용 하는 방법의 예를 보여 줍니다.

```csharp
using Xamarin.Forms.Markup;
using Xamarin.Forms.Markup.LeftToRight;
using static Xamarin.Forms.Markup.GridRowsColumns;
// ...

enum BodyRow
{
    Prompt,
    CodeHeader,
    CodeEntry,
    Button
}

enum BodyCol
{
    FieldLabel,
    FieldValidation
}

View Build() => new Grid
{
    RowDefinitions = Rows.Define(
        (BodyRow.Prompt    , 170 ),
        (BodyRow.CodeHeader, 75  ),
        (BodyRow.CodeEntry , Auto),
        (BodyRow.Button    , Auto)
    ),

    ColumnDefinitions = Columns.Define(
        (BodyCol.FieldLabel     , 160 ),
        (BodyCol.FieldValidation, Star)
    ),

    Children =
    {
        new Label { LineBreakMode = LineBreakMode.WordWrap } .Font (15) .Bold ()
                   .Row (BodyRow.Prompt) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .CenterVertical () .Margin (fieldNameMargin) .TextCenterHorizontal ()
                   .Bind (nameof(vm.RegistrationPrompt)),

        new Label { Text = "Registration code" } .Bold ()
                   .Row (BodyRow.CodeHeader) .Column(BodyCol.FieldLabel) .Bottom () .Margin (fieldNameMargin),

        new Label { } .Italic ()
                   .Row (BodyRow.CodeHeader) .Column (BodyCol.FieldValidation) .Right () .Bottom () .Margin (fieldNameMargin)
                   .Bind (nameof(vm.RegistrationCodeValidationMessage)),

        new Entry { Placeholder = "E.g. 123456", Keyboard = Keyboard.Numeric, BackgroundColor = Color.AliceBlue, TextColor = Color.Black } .Font (15)
                   .Row (BodyRow.CodeEntry) .ColumnSpan (All<BodyCol>()) .Margin (fieldMargin) .Height (44)
                   .Bind (nameof(vm.RegistrationCode), BindingMode.TwoWay),

        new Button { Text = "Verify" } .Style (FilledButton)
                    .Row (BodyRow.Button) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .Margin (PageMarginSize)
                    .Bind (Button.IsVisibleProperty, nameof(vm.CanVerifyRegistrationCode))
                    .Bind (nameof(vm.VerifyRegistrationCodeCommand)),
    }
};
```

또한 열거 하지 않고 행과 열을 간결 하 게 정의할 수 있습니다.

```csharp
new Grid
{
    RowDefinitions = Rows.Define (Auto, Star, 20),
    ColumnDefinitions = Columns.Define (Auto, Star, 20, 40)
    // ...
}
```

## <a name="fonts"></a>글꼴

다음 목록의 컨트롤 `FontSize`은, `Bold` `Italic`, 및 `Font` 확장 메서드를 호출 하 여 컨트롤에 의해 표시 되는 텍스트의 모양을 설정할 수 있습니다.

- `Button`
- `DatePicker`
- `Editor`
- `Entry`
- `Label`
- `Picker`
- `SearchBar`
- `Span`
- `TimePicker`

## <a name="effects"></a>효과

`Effect` 확장 메서드를 사용 하 여 컨트롤에 효과를 연결할 수 있습니다.

```csharp
using Xamarin.Forms.Markup;
// ...

new Button { Text = "Tap Me" }
            .Effects (new ButtonMixedCaps())
```

## <a name="logic-integration"></a>논리 통합

`Invoke` 확장 메서드를 사용 하 여 c # 태그에서 코드 인라인을 실행할 수 있습니다.

```csharp
using Xamarin.Forms.Markup;
// ...

new ListView { } .Invoke (l => l.ItemTapped += OnListViewItemTapped)
```

또한 `Assign` 확장 메서드를 사용 하 여 ui 태그 외부 (ui 논리 파일)에서 컨트롤에 액세스할 수 있습니다.

```csharp
using Xamarin.Forms.Markup;
// ...

new ListView { } .Assign (out MyListView)
```

## <a name="styles"></a>스타일

다음 예제에서는 c # 태그를 사용 하 여 암시적 및 명시적 스타일을 만드는 방법을 보여 줍니다.

```csharp
using Xamarin.Forms.Markup;
using Xamarin.Forms;

namespace CSharpForMarkupDemos
{
    public static class Styles
    {
        static Style<Button> buttons, filledButton;
        static Style<Label> labels;
        static Style<Span> link;

        #region Implicit styles

        public static ResourceDictionary Implicit => new ResourceDictionary { Buttons, Labels };

        public static Style<Button> Buttons => buttons ?? (buttons = new Style<Button>(
            (Button.HeightRequestProperty, 44),
            (Button.FontSizeProperty, 13),
            (Button.HorizontalOptionsProperty, LayoutOptions.Center),
            (Button.VerticalOptionsProperty, LayoutOptions.Center)
        ));

        public static Style<Label> Labels => labels ?? (labels = new Style<Label>(
            (Label.FontSizeProperty, 13),
            (Label.TextColorProperty, Color.Black)
        ));

        #endregion Implicit styles

        #region Explicit styles

        public static Style<Button> FilledButton => filledButton ?? (filledButton = new Style<Button>(
            (Button.TextColorProperty, Color.White),
            (Button.BackgroundColorProperty, Color.FromHex("#1976D2")),
            (Button.CornerRadiusProperty, 5)
        )).BasedOn(Buttons);

        public static Style<Span> Link => link ?? (link = new Style<Span>(
            (Span.TextColorProperty, Color.Blue),
            (Span.TextDecorationsProperty, TextDecorations.Underline)
        ));

        #endregion Explicit styles
    }
}
```

암시적 스타일은 응용 프로그램 리소스 사전에 로드 하 여 사용 될 수 있습니다.

```csharp
public App()
{
    Resources = Styles.Implicit;
    // ...
}
```

명시적 스타일은 `Style` 확장 메서드와 함께 사용 될 수 있습니다.

```csharp
using static CSharpForMarkupExample.Styles;
// ...

new Button { Text = "Tap Me" } .Style (FilledButton),
```

> [!NOTE]
> `Style` `ApplyToDerivedTypes`확장 메서드 외에도 `BasedOn` `Add`,, 및 `CanCascade` 확장 메서드가 있습니다.

또는 고유한 스타일 확장 메서드를 만들 수 있습니다.

```csharp
public static TButton Filled<TButton>(this TButton button) where TButton : Button
{
    button.Buttons(); // Equivalent to Style .BasedOn (Buttons)
    button.TextColor = Color.White;
    button.BackgroundColor = Color.Red;
    return button;
}
```

확장 `Filled` 메서드는 다음과 같이 사용 될 수 있습니다.

```csharp
new Button { Text = "Tap Me" } .Filled ()
```

## <a name="platform-specifics"></a>플랫폼 사양

확장 `Invoke` 메서드는 플랫폼별를 적용 하는 데 사용할 수 있습니다. 그러나 모호성 오류를 방지 하기 위해 `using` `Xamarin.Forms.PlatformConfiguration.*Specific` 네임 스페이스에 대 한 지시문을 직접 포함 하지 마십시오. 대신 네임 스페이스 별칭을 만들고 별칭을 통해 플랫폼별를 사용 합니다.

```csharp
using Xamarin.Forms.Markup;
using PciOS = Xamarin.Forms.PlatformConfiguration.iOSSpecific;
// ...

new ListView { } .Invoke (l => PciOS.ListView.SetGroupHeaderStyle(l, PciOS.GroupHeaderStyle.Grouped))
```

또한 특정 플랫폼을 사용 하는 경우 사용자 고유의 확장 클래스에서 흐름 확장 메서드를 만들 수 있습니다.

```csharp
public static T iOSGroupHeaderStyle<T>(this T listView, PciOS.GroupHeaderStyle style) where T : Forms.ListView
{
  PciOS.ListView.SetGroupHeaderStyle(listView, style);
  return listView;
}
```

확장 메서드는 다음과 같이 사용 될 수 있습니다.

```csharp
new ListView { } .iOSGroupHeaderStyle(PciOS.GroupHeaderStyle.Grouped)
```

플랫폼 세부 정보에 대 한 자세한 내용은 [Android 플랫폼 기능](~/xamarin-forms/platform/android/index.md), [iOS 플랫폼 기능](~/xamarin-forms/platform/ios/index.md)및 [Windows 플랫폼 기능](~/xamarin-forms/platform/windows/index.md)을 참조 하세요.

## <a name="recommended-convention"></a>권장 규칙

권장 순서 및 속성 및 도우미 메서드 그룹은 다음과 같습니다.

- **목적**: 컨트롤의 용도 (예: `Text`, `Placeholder`,`Assign`)를 식별 하는 값을 갖는 모든 속성 또는 도우미 메서드입니다.
- **기타**: 레이아웃이 나 바인딩이 아닌 모든 속성 또는 도우미 메서드가 같은 줄 또는 여러 줄에 있습니다.
- **레이아웃**: 레이아웃은 안쪽 정렬 됩니다. 행 및 열, 레이아웃 옵션, 여백, 크기, 안쪽 여백 및 내용 맞춤입니다.
- **바인딩**: 데이터 바인딩은 줄 마다 바인딩된 속성 하나를 사용 하 여 메서드 체인의 끝에서 수행 됩니다. 바인딩 가능한 *기본* 속성이 바인딩되면 메서드 체인의 끝에 있어야 합니다.

다음 코드에서는이 규칙을 준수 하는 예를 보여 줍니다.

```csharp
new Button { Text = "Verify" /* purpose */ } .Style (FilledButton) // other
            .Row (BodyRow.Button) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .Margin (10) // layout
            .Bind (Button.IsVisibleProperty, nameof(vm.CanVerifyRegistrationCode)) // bind
            .Bind (nameof(vm.VerifyRegistrationCodeCommand)), // bind default

new Label { }
           .Assign (out animatedMessageLabel) // purpose
           .Invoke (label => label.SizeChanged += MessageLabel_SizeChanged) // other
           .Row (BodyRow.Message) .ColumnSpan (All<BodyCol>()) // layout
           .Bind (nameof(vm.Message)), // bind default
```

이 규칙을 일관 되 게 적용 하면 c # 태그를 빠르게 검색 하 고 UI 레이아웃의 멘 탈 이미지를 빌드할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [CSharpForMarkupDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-csharpmarkupdemos/)
- [Android 플랫폼 기능](~/xamarin-forms/platform/android/index.md)
- [iOS 플랫폼 기능](~/xamarin-forms/platform/ios/index.md)
- [Windows 플랫폼 기능](~/xamarin-forms/platform/windows/index.md)
