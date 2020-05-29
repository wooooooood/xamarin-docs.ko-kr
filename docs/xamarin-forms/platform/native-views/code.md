---
title: ''
description: 'IOS, Android 및 UWP의 기본 뷰는 Xamarin.Forms c #을 사용 하 여 만든 페이지에서 직접 참조할 수 있습니다. 이 문서에서는 Xamarin.Forms c #을 사용 하 여 만든 레이아웃에 네이티브 뷰를 추가 하는 방법과 사용자 지정 뷰의 레이아웃을 재정의 하 여 해당 측정 API 사용법을 수정 하는 방법을 보여 줍니다.'
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 18cdeccbdff86a6b20aab4b33db259f1f06ee096
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139596"
---
# <a name="native-views-in-c"></a>C의 네이티브 뷰\#

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeembedding)

_IOS, Android 및 UWP의 기본 뷰는 Xamarin.Forms c #을 사용 하 여 만든 페이지에서 직접 참조할 수 있습니다. 이 문서에서는 Xamarin.Forms c #을 사용 하 여 만든 레이아웃에 네이티브 뷰를 추가 하는 방법과 사용자 지정 뷰의 레이아웃을 재정의 하 여 해당 측정 API 사용법을 수정 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

을 Xamarin.Forms 설정 하거나 컬렉션을 포함 하는 컨트롤은 `Content` `Children` 플랫폼별 뷰를 추가할 수 있습니다. 예를 들어 iOS는 `UILabel` 속성에 직접 추가 [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) 하거나 컬렉션에 추가할 수 있습니다 [`StackLayout.Children`](xref:Xamarin.Forms.Layout`1.Children) . 그러나이 기능을 사용 하려면 `#if` 공유 프로젝트 솔루션에 정의를 사용 해야 Xamarin.Forms 하며 Xamarin.Forms .NET Standard 라이브러리 솔루션에서 사용할 수 없습니다.

다음 스크린샷에는에 추가 된 플랫폼별 뷰가 설명 되어 Xamarin.Forms [`StackLayout`](xref:Xamarin.Forms.StackLayout) 있습니다.

[![](code-images/screenshots-sml.png "StackLayout Containing Platform-Specific Views")](code-images/screenshots.png#lightbox "StackLayout Containing Platform-Specific Views")

플랫폼 특정 뷰를 레이아웃에 추가 하는 기능은 Xamarin.Forms 각 플랫폼에서 두 가지 확장 메서드를 사용 하 여 사용할 수 있습니다.

- `Add`– 레이아웃의 컬렉션에 플랫폼별 뷰를 추가 [`Children`](xref:Xamarin.Forms.Layout`1.Children) 합니다.
- `ToView`– 플랫폼별 뷰를 사용 하 여 Xamarin.Forms [`View`](xref:Xamarin.Forms.View) 컨트롤의 속성으로 설정할 수 있는으로 래핑합니다 `Content` .

공유 프로젝트에서 이러한 메서드를 사용 Xamarin.Forms 하려면 적절 한 플랫폼별 네임 스페이스를 가져와야 합니다 Xamarin.Forms .

- **iOS** – Xamarin.Forms . Platform.object
- **Android** – Xamarin.Forms . 플랫폼 Android
- **유니버설 Windows 플랫폼 (UWP)** – Xamarin.Forms . Platform UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>플랫폼 특정 뷰를 각 플랫폼에 추가

다음 섹션에서는 플랫폼 특정 뷰를 Xamarin.Forms 각 플랫폼의 레이아웃에 추가 하는 방법을 보여 줍니다.

### <a name="ios"></a>iOS

다음 코드 예제에서는 및에를 추가 하는 방법을 보여 줍니다 `UILabel` [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`ContentView`](xref:Xamarin.Forms.ContentView) .

```csharp
var uiLabel = new UILabel {
  MinimumFontSize = 14f,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = originalText,
};
stackLayout.Children.Add (uiLabel);
contentView.Content = uiLabel.ToView();
```

이 예제에서는 `stackLayout` 및 `contentView` 인스턴스가 XAML 또는 c #에서 이전에 생성 된 것으로 가정 합니다.

### <a name="android"></a>Android

다음 코드 예제에서는 및에를 추가 하는 방법을 보여 줍니다 `TextView` [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`ContentView`](xref:Xamarin.Forms.ContentView) .

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

이 예제에서는 `stackLayout` 및 `contentView` 인스턴스가 XAML 또는 c #에서 이전에 생성 된 것으로 가정 합니다.

### <a name="universal-windows-platform"></a>범용 Windows 플랫폼

다음 코드 예제에서는 및에를 추가 하는 방법을 보여 줍니다 `TextBlock` [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`ContentView`](xref:Xamarin.Forms.ContentView) .

```csharp
var textBlock = new TextBlock
{
    Text = originalText,
    FontSize = 14,
    FontFamily = new FontFamily("HelveticaNeue"),
    TextWrapping = TextWrapping.Wrap
};
stackLayout.Children.Add(textBlock);
contentView.Content = textBlock.ToView();
```

이 예제에서는 `stackLayout` 및 `contentView` 인스턴스가 XAML 또는 c #에서 이전에 생성 된 것으로 가정 합니다.

## <a name="overriding-platform-measurements-for-custom-views"></a>사용자 지정 보기에 대 한 플랫폼 측정 재정의

각 플랫폼에 대 한 사용자 지정 보기는 디자인 된 레이아웃 시나리오에 대 한 측정만 올바르게 구현 하는 경우가 많습니다. 예를 들어 사용자 지정 보기는 사용 가능한 장치 너비의 절반 까지만 차지 하도록 설계 되었을 수 있습니다. 그러나 다른 사용자와 공유 하 고 나면 사용자 지정 보기를 사용 하 여 사용 가능한 전체 장치 너비를 차지할 수 있습니다. 따라서 레이아웃에서 재사용 될 때 사용자 지정 뷰 측정 구현을 재정의 해야 할 수 있습니다 Xamarin.Forms . `Add`이러한 이유로 및 `ToView` 확장 메서드는 측정 대리자를 지정할 수 있는 재정의를 제공 하며,이는 레이아웃에 추가 될 때 사용자 지정 뷰 레이아웃을 재정의할 수 있습니다 Xamarin.Forms .

다음 섹션에서는 사용자 지정 보기의 레이아웃을 재정의 하 여 해당 측정 API 사용법을 수정 하는 방법을 보여 줍니다.

### <a name="ios"></a>iOS

다음 코드 예제에서는 `CustomControl` 에서 상속 되는 클래스를 보여 줍니다 `UILabel` .

```csharp
public class CustomControl : UILabel
{
  public override string Text {
    get { return base.Text; }
    set { base.Text = value.ToUpper (); }
  }

  public override CGSize SizeThatFits (CGSize size)
  {
    return new CGSize (size.Width, 150);
  }
}
```

이 뷰의 인스턴스는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 다음 코드 예제에서 보여 주는 것 처럼에 추가 됩니다.

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

그러나 재정의는 항상 높이를 150으로 반환 하기 때문에 `CustomControl.SizeThatFits` 다음 스크린샷에 표시 된 것 처럼 뷰가 표시 되 고 그 아래에 빈 공간이 표시 됩니다.

![](code-images/ios-bad-measurement.png "iOS CustomControl with Bad SizeThatFits Implementation")

이 문제에 대 한 해결 방법은 `GetDesiredSizeDelegate` 다음 코드 예제에서 보여 주는 것 처럼 구현을 제공 하는 것입니다.

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, double width, double height)
{
  var uiView = renderer.Control;

  if (uiView == null) {
    return null;
  }

  var constraint = new CGSize (width, height);

  // Let the CustomControl determine its size (which will be wrong)
  var badRect = uiView.SizeThatFits (constraint);

  // Use the width and substitute the height
  return new SizeRequest (new Size (badRect.Width, 70));
}
```

이 메서드는 메서드에서 제공 하는 너비를 사용 `CustomControl.SizeThatFits` 하지만 높이 70의 높이를 150로 대체 합니다. 인스턴스가에 `CustomControl` 추가 되 면 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 메서드를로 `FixSize` 지정 `GetDesiredSizeDelegate` 하 여 클래스에서 제공 하는 잘못 된 측정을 수정할 수 있습니다 `CustomControl` .

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

그러면 다음 스크린샷에 표시 된 것 처럼 사용자 지정 보기가 제대로 표시 되 고 그 아래에 빈 공백이 표시 되지 않습니다.

![](code-images/ios-good-measurement.png "iOS CustomControl with GetDesiredSize Override")

### <a name="android"></a>Android

다음 코드 예제에서는 `CustomControl` 에서 상속 되는 클래스를 보여 줍니다 `TextView` .

```csharp
public class CustomControl : TextView
{
  public CustomControl (Context context) : base (context)
  {
  }

  protected override void OnMeasure (int widthMeasureSpec, int heightMeasureSpec)
  {
    int width = MeasureSpec.GetSize (widthMeasureSpec);

    // Force the width to half of what's been requested.
    // This is deliberately wrong to demonstrate providing an override to fix it with.
    int widthSpec = MeasureSpec.MakeMeasureSpec (width / 2, MeasureSpec.GetMode (widthMeasureSpec));

    base.OnMeasure (widthSpec, heightMeasureSpec);
  }
}
```

이 뷰의 인스턴스는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 다음 코드 예제에서 보여 주는 것 처럼에 추가 됩니다.

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

그러나 `CustomControl.OnMeasure` 재정의는 항상 요청 된 너비의 절반을 반환 하기 때문에 다음 스크린샷에 표시 된 것 처럼 보기는 사용 가능한 장치 너비의 절반만 표시 됩니다.

![](code-images/android-bad-measurement.png "Android CustomControl with Bad OnMeasure Implementation")

이 문제에 대 한 해결 방법은 `GetDesiredSizeDelegate` 다음 코드 예제에서 보여 주는 것 처럼 구현을 제공 하는 것입니다.

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, int widthConstraint, int heightConstraint)
{
  var nativeView = renderer.Control;

  if ((widthConstraint == 0 && heightConstraint == 0) || nativeView == null) {
    return null;
  }

  int width = Android.Views.View.MeasureSpec.GetSize (widthConstraint);
  int widthSpec = Android.Views.View.MeasureSpec.MakeMeasureSpec (
    width * 2, Android.Views.View.MeasureSpec.GetMode (widthConstraint));
  nativeView.Measure (widthSpec, heightConstraint);
  return new SizeRequest (new Size (nativeView.MeasuredWidth, nativeView.MeasuredHeight));
}
```

이 메서드는 메서드에서 제공 하는 너비를 사용 `CustomControl.OnMeasure` 하지만 2를 곱합니다. 인스턴스가에 `CustomControl` 추가 되 면 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 메서드를로 `FixSize` 지정 `GetDesiredSizeDelegate` 하 여 클래스에서 제공 하는 잘못 된 측정을 수정할 수 있습니다 `CustomControl` .

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

그러면 다음 스크린샷에 표시 된 것 처럼 장치 너비를 차지 하는 사용자 지정 보기가 올바르게 표시 됩니다.

![](code-images/android-good-measurement.png "Android CustomControl with Custom GetDesiredSize Delegate")

### <a name="universal-windows-platform"></a>범용 Windows 플랫폼

다음 코드 예제에서는 `CustomControl` 에서 상속 되는 클래스를 보여 줍니다 `Panel` .

```csharp
public class CustomControl : Panel
{
  public static readonly DependencyProperty TextProperty =
    DependencyProperty.Register(
      "Text", typeof(string), typeof(CustomControl), new PropertyMetadata(default(string), OnTextPropertyChanged));

  public string Text
  {
    get { return (string)GetValue(TextProperty); }
    set { SetValue(TextProperty, value.ToUpper()); }
  }

  readonly TextBlock textBlock;

  public CustomControl()
  {
    textBlock = new TextBlock
    {
      MinHeight = 0,
      MaxHeight = double.PositiveInfinity,
      MinWidth = 0,
      MaxWidth = double.PositiveInfinity,
      FontSize = 14,
      TextWrapping = TextWrapping.Wrap,
      VerticalAlignment = VerticalAlignment.Center
    };

    Children.Add(textBlock);
  }

  static void OnTextPropertyChanged(DependencyObject dependencyObject, DependencyPropertyChangedEventArgs args)
  {
    ((CustomControl)dependencyObject).textBlock.Text = (string)args.NewValue;
  }

  protected override Size ArrangeOverride(Size finalSize)
  {
      // This is deliberately wrong to demonstrate providing an override to fix it with.
      textBlock.Arrange(new Rect(0, 0, finalSize.Width/2, finalSize.Height));
      return finalSize;
  }

  protected override Size MeasureOverride(Size availableSize)
  {
      textBlock.Measure(availableSize);
      return new Size(textBlock.DesiredSize.Width, textBlock.DesiredSize.Height);
  }
}
```

이 뷰의 인스턴스는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 다음 코드 예제에서 보여 주는 것 처럼에 추가 됩니다.

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

그러나 `CustomControl.ArrangeOverride` 재정의는 항상 요청 된 너비의 절반을 반환 하기 때문에 다음 스크린샷에 표시 된 것 처럼 뷰가 사용 가능한 장치 너비의 1/2로 잘립니다.

![](code-images/winrt-bad-measurement.png "UWP CustomControl with Bad ArrangeOverride Implementation")

이 문제에 대 한 해결 방법은 `ArrangeOverrideDelegate` [`StackLayout`](xref:Xamarin.Forms.StackLayout) 다음 코드 예제에서 보여 주는 것 처럼에 뷰를 추가할 때 구현을 제공 하는 것입니다.

```csharp
stackLayout.Children.Add(fixedControl, arrangeOverrideDelegate: (renderer, finalSize) =>
{
    if (finalSize.Width <= 0 || double.IsInfinity(finalSize.Width))
    {
        return null;
    }
    var frameworkElement = renderer.Control;
    frameworkElement.Arrange(new Rect(0, 0, finalSize.Width * 2, finalSize.Height));
    return finalSize;
});
```

이 메서드는 메서드에서 제공 하는 너비를 사용 `CustomControl.ArrangeOverride` 하지만 2를 곱합니다. 그러면 다음 스크린샷에 표시 된 것 처럼 장치 너비를 차지 하는 사용자 지정 보기가 올바르게 표시 됩니다.

![](code-images/winrt-good-measurement.png "UWP CustomControl with ArrangeOverride Delegate")

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms c #을 사용 하 여 만든 레이아웃에 네이티브 뷰를 추가 하는 방법 및 사용자 지정 보기의 레이아웃을 재정의 하 여 해당 측정 API 사용법을 수정 하는 방법을 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [NativeEmbedding (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeembedding)
- [네이티브 양식](~/xamarin-forms/platform/native-forms.md)
