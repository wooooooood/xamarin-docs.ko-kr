---
title: "C#에서 기본 뷰"
description: "C#을 사용 하 여 만든 Xamarin.Forms 페이지에서 네이티브 iOS, Android 및 UWP 뷰를 직접 참조할 수 있습니다. 이 문서에서는 C#을 사용 하 여 만든 Xamarin.Forms 레이아웃에 기본 뷰를 추가 하는 방법과 해당 측정 API 사용을 해결 하려면 사용자 지정 보기의 레이아웃을 재정의 하는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 55864073aecb48176d650da6edefad24c3248767
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="native-views-in-c"></a>C#에서 기본 뷰

_C#을 사용 하 여 만든 Xamarin.Forms 페이지에서 네이티브 iOS, Android 및 UWP 뷰를 직접 참조할 수 있습니다. 이 문서에서는 C#을 사용 하 여 만든 Xamarin.Forms 레이아웃에 기본 뷰를 추가 하는 방법과 해당 측정 API 사용을 해결 하려면 사용자 지정 보기의 레이아웃을 재정의 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

수 있는 모든 Xamarin.Forms 컨트롤 `Content` 를 설정할 수에 또는 끝점이 `Children` 컬렉션 플랫폼 특정 보기를 추가할 수 있습니다. 예를 들어 iOS `UILabel` 에 직접 추가할 수는 [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) 속성 또는 [ `StackLayout.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) 컬렉션입니다. 그러나이 기능을 사용 해야는 메모 `#if` Xamarin.Forms 공유 프로젝트 솔루션에서 정의 하 고 Xamarin.Forms 이식 가능한 클래스 라이브러리 (PCL) 솔루션에서 사용할 수 없습니다.

다음 스크린샷에서 보여 플랫폼 관련 뷰는 xamarin.forms 추가 되어 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/):

[![](code-images/screenshots-sml.png "플랫폼 특정 보기를 포함 하는 StackLayout")](code-images/screenshots.png "플랫폼 특정 보기를 포함 하는 StackLayout")

각 플랫폼에서 두 개의 확장 메서드로 Xamarin.Forms 레이아웃에 플랫폼 관련 뷰를 추가 하는 기능 사용 됩니다.

- `Add` – 플랫폼별 뷰를 추가 하는 [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) 레이아웃의 컬렉션입니다.
- `ToView` – 플랫폼 특정 보기를 사용 하 고 Xamarin.Forms는으로 래핑합니다 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 로 설정할 수 있습니다는 `Content` 컨트롤의 속성입니다.

이러한 메서드를 사용 하 여 Xamarin.Forms 공유 프로젝트에 적절 한 플랫폼별 Xamarin.Forms 네임 스페이스 가져오기 필요 합니다.

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Windows Runtime** – Xamarin.Forms.Platform.WinRT
- **유니버설 Windows 플랫폼 (UWP)** – Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>각 플랫폼에서 플랫폼 관련 뷰 추가

다음 섹션에서는 플랫폼 관련 뷰는 각 플랫폼에서 Xamarin.Forms 레이아웃에 추가 하는 방법을 보여 줍니다.

### <a name="ios"></a>iOS

다음 코드 예제에서는 추가 하는 방법을 보여 줍니다.는 `UILabel` 에 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 및 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

이 예에서는 가정 하는 `stackLayout` 및 `contentView` XAML 또는 C#에서 이전에 만든 한 인스턴스.

### <a name="android"></a>Android

다음 코드 예제에서는 추가 하는 방법을 보여 줍니다.는 `TextView` 에 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 및 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
var textView = new TextView (Forms.Context) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

이 예에서는 가정 하는 `stackLayout` 및 `contentView` XAML 또는 C#에서 이전에 만든 한 인스턴스.

### <a name="windows-runtime-and-universal-windows-platform"></a>Windows 런타임 및 유니버설 Windows 플랫폼

다음 코드 예제에서는 추가 하는 방법을 보여 줍니다.는 `TextBlock` 에 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 및 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

이 예에서는 가정 하는 `stackLayout` 및 `contentView` XAML 또는 C#에서 이전에 만든 한 인스턴스.

## <a name="overriding-platform-measurements-for-custom-views"></a>사용자 지정 보기에 대 한 플랫폼 측정 재정의

사용자 지정 보기는 각 플랫폼에는 종종만 올바르게으로 설계 된 레이아웃 시나리오에 대 한 측정을 구현 합니다. 예를 들어 사용자 지정 보기만 장치의 사용 가능한 너비의 절반을 차지 하도록 설계 되었습니다 수 있습니다. 그러나 후 다른 사용자와 공유 되 고, 사용자 지정 보기는 장치의 사용 가능한 전체 너비를 차지 필요할 수 있습니다. 따라서 Xamarin.Forms 레이아웃에서 다시 사용 하는 경우 사용자 지정 보기 측정 구현을 재정의 해야 할 수 있습니다. 이런 이유로 `Add` 및 `ToView` 확장 메서드는 Xamarin.Forms 레이아웃에 추가 될 때 사용자 지정 보기 레이아웃을 재정의할 수를 지정 해야 측정 대리자를 허용 하는 재정의 제공 합니다.

다음 섹션에서는 재정의 자신의 측정 API 사용을 해결 하려면 사용자 지정 보기를 레이아웃 하는 방법을 보여 줍니다.

### <a name="ios"></a>iOS

다음 코드 예제는 `CustomControl` 클래스에서 상속 하는 `UILabel`:

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

이 보기의 인스턴스를 추가할는 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)다음 코드 예제에서와 같이,:

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

그러나 때문에 `CustomControl.SizeThatFits` 150의 높이 항상 반환 하는 재정의 보기 다음 스크린샷에 표시 된 것 처럼 위와 아래의 빈 공간으로 표시 됩니다.

![](code-images/ios-bad-measurement.png "잘못 된 SizeThatFits 구현으로 CustomControl iOS")

제공 하는 것이 문제를 해결할 수는 `GetDesiredSizeDelegate` 다음 코드 예제에서와 같이 구현 합니다.

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

이 방법은 사용 하 여 제공 된 너비는 `CustomControl.SizeThatFits` 메서드, 하지만 70의 높이 대 한 150의 높이 대체 합니다. 경우는 `CustomControl` 인스턴스가에 추가 되는 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), `FixSize` 로 메서드를 지정할 수 있습니다는 `GetDesiredSizeDelegate` 문제를 해결 하 여 제공 된 잘못 된 측정 하는 `CustomControl` 클래스:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

이 인해 위와 아래의 빈 공간 없이 올바르게 표시 되 고 사용자 지정 보기 다음 스크린샷에 표시 된 것 처럼:

![](code-images/ios-good-measurement.png "iOS CustomControl GetDesiredSize 재정의")

### <a name="android"></a>Android

다음 코드 예제는 `CustomControl` 클래스에서 상속 하는 `TextView`:

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

이 보기의 인스턴스를 추가할는 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)다음 코드 예제에서와 같이,:

```csharp
var customControl = new CustomControl (Forms.Context) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

그러나 때문에 `CustomControl.OnMeasure` 재정의 요청 된 너비의 절반 항목을 항상 반환는 보기 표시 됩니다. 장치의 사용 가능한 너비 절반을 차지 다음 스크린샷에 표시 된 것 처럼:

![](code-images/android-bad-measurement.png "잘못 된 OnMeasure 구현으로 android CustomControl")

제공 하는 것이 문제를 해결할 수는 `GetDesiredSizeDelegate` 다음 코드 예제에서와 같이 구현 합니다.

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

이 방법은 사용 하 여 제공 된 너비는 `CustomControl.OnMeasure` 메서드, 하지만 곱한 두 합니다. 경우는 `CustomControl` 인스턴스가에 추가 되는 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), `FixSize` 로 메서드를 지정할 수 있습니다는 `GetDesiredSizeDelegate` 문제를 해결 하 여 제공 된 잘못 된 측정 하는 `CustomControl` 클래스:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

이 인해 되는 사용자 지정 보기에 올바르게 표시, 장치의 너비를 차지 다음 스크린샷에 표시 된 것 처럼:

![](code-images/android-good-measurement.png "사용자 지정 GetDesiredSize 대리자와 android CustomControl")

### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

다음 코드 예제는 `CustomControl` 클래스에서 상속 하는 `Panel`:

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

이 보기의 인스턴스를 추가할는 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)다음 코드 예제에서와 같이,:

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

그러나 때문에 `CustomControl.ArrangeOverride` 보기에 클리핑됩니다 사용 가능한 너비의 절반 장치를 다음 스크린샷에 표시 된 것 처럼, 재정의 요청 된 너비의 절반 항목을 항상 반환 합니다.

![](code-images/winrt-bad-measurement.png "잘못 된 ArrangeOverride 구현으로 UWP CustomControl")

제공 하는 것이 문제를 해결할 수는 `ArrangeOverrideDelegate` 구현에서 뷰를 추가 하는 경우는 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)다음 코드 예제에서와 같이,:

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

이 방법은 사용 하 여 제공 된 너비는 `CustomControl.ArrangeOverride` 메서드, 하지만 곱한 두 합니다. 이 인해 되는 사용자 지정 보기에 올바르게 표시, 장치의 너비를 차지 다음 스크린샷에 표시 된 것 처럼:

![](code-images/winrt-good-measurement.png "UWP CustomControl ArrangeOverride 대리자와")

## <a name="summary"></a>요약

이 문서에는 C#을 사용 하 여 만든 Xamarin.Forms 레이아웃에 기본 뷰를 추가 하는 방법과 해당 측정 API 사용을 해결 하려면 사용자 지정 보기의 레이아웃을 재정의 하는 방법을 설명 합니다.


## <a name="related-links"></a>관련 링크

- [NativeEmbedding (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeEmbedding/)
- [기본 형식](~/xamarin-forms/platform/native-forms.md)
