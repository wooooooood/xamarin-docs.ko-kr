---
title: C#의 네이티브 뷰
description: C#을 사용 하 여 만든 Xamarin.Forms 페이지에서 iOS, Android 및 UWP에서 네이티브 뷰를 직접 참조할 수 있습니다. 이 문서에는 C#을 사용 하 여 만든 Xamarin.Forms 레이아웃에 네이티브 뷰를 추가 하는 방법 및 해당 측정 API 사용을 수정 하려면 사용자 지정 보기 레이아웃을 재정의 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: ad633f49c1c448529fa4c2b50483ec233c1ee841
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996196"
---
# <a name="native-views-in-c"></a>C#의 네이티브 뷰

_C#을 사용 하 여 만든 Xamarin.Forms 페이지에서 iOS, Android 및 UWP에서 네이티브 뷰를 직접 참조할 수 있습니다. 이 문서에는 C#을 사용 하 여 만든 Xamarin.Forms 레이아웃에 네이티브 뷰를 추가 하는 방법 및 해당 측정 API 사용을 수정 하려면 사용자 지정 보기 레이아웃을 재정의 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

허용 하는 모든 Xamarin.Forms 컨트롤 `Content` 을 설정 하려면 않았거나는 `Children` 컬렉션 플랫폼 특정 뷰를 추가할 수 있습니다. 예를 들어, iOS `UILabel` 에 직접 추가할 수 있습니다 합니다 [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) 속성 또는 합니다 [ `StackLayout.Children` ](xref:Xamarin.Forms.Layout`1.Children) 컬렉션. 그러나이 기능을 사용 해야는 `#if` Xamarin.Forms 공유 프로젝트 솔루션에서 정의 하 고 Xamarin.Forms.NET Standard 라이브러리 솔루션에서 사용할 수 없습니다.

다음 스크린샷은 플랫폼별 뷰는 Xamarin.Forms에 추가 된 것을 보여 줍니다 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout):

[![](code-images/screenshots-sml.png "플랫폼별 뷰가 포함 된 StackLayout")](code-images/screenshots.png#lightbox "플랫폼별 뷰가 포함 된 StackLayout")

Xamarin.Forms 레이아웃에 플랫폼 전용 뷰를 추가할 수는 각 플랫폼에서 두 개의 확장 메서드로 사용 됩니다.

- `Add` – 플랫폼별 뷰를 추가 합니다 [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) 레이아웃의 컬렉션입니다.
- `ToView` – 플랫폼별 뷰를 사용 하 고는 Xamarin.Forms로 래핑되어 [ `View` ](xref:Xamarin.Forms.View) 으로 설정할 수는 `Content` 컨트롤의 속성입니다.

이러한 메서드를 사용 하 여 Xamarin.Forms 공유 프로젝트에 적절 한 플랫폼별 Xamarin.Forms 네임 스페이스 가져오기에 필요 합니다.

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **유니버설 Windows 플랫폼 (UWP)** – Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>각 플랫폼에 플랫폼별 뷰 추가

다음 섹션에서는 각 플랫폼에서 Xamarin.Forms 레이아웃에 플랫폼 전용 뷰를 추가 하는 방법을 보여 줍니다.

### <a name="ios"></a>iOS

다음 코드 예제에 추가 하는 방법을 보여 줍니다.는 `UILabel` 에 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 와 [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

가정 합니다 `stackLayout` 및 `contentView` XAML 또는 C#에서 이전에 만든 된 인스턴스.

### <a name="android"></a>Android

다음 코드 예제에 추가 하는 방법을 보여 줍니다.는 `TextView` 에 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 와 [ `ContentView` ](xref:Xamarin.Forms.ContentView):

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

가정 합니다 `stackLayout` 및 `contentView` XAML 또는 C#에서 이전에 만든 된 인스턴스.

### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

다음 코드 예제에 추가 하는 방법을 보여 줍니다.는 `TextBlock` 에 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 와 [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

가정 합니다 `stackLayout` 및 `contentView` XAML 또는 C#에서 이전에 만든 된 인스턴스.

## <a name="overriding-platform-measurements-for-custom-views"></a>사용자 지정 보기에 대 한 플랫폼 측정 값 재정의

각 플랫폼에서 사용자 지정 보기는 종종만 올바르게으로 설계 된 레이아웃 시나리오에 대 한 측정을 구현 합니다. 예를 들어, 사용자 지정 보기를만 장치의 사용 가능한 너비의 절반을 차지 하도록 설계 되었습니다 수 있습니다. 그러나 다른 사용자와 공유 되 고 후 사용자 지정 보기 장치 전체 가용 공간을 차지 하는 데 필요한 수 있습니다. 따라서 Xamarin.Forms 레이아웃으로 다시 사용할 때 사용자 지정 보기 측정 구현을 재정의 하는 데 필요한 수 있습니다. 이런 이유로 합니다 `Add` 및 `ToView` 확장 메서드는 Xamarin.Forms 레이아웃에 추가 될 때 사용자 지정 보기 레이아웃을 재정의할 수를 지정 해야 측정 대리자를 허용 하는 재정의 제공 합니다.

다음 섹션에서는 해당 측정 API 사용을 수정 하려면 사용자 지정 보기 레이아웃을 재정의 하는 방법을 보여 줍니다.

### <a name="ios"></a>iOS

다음 코드 예제는 `CustomControl` 클래스에서 상속 되는 `UILabel`:

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

이 보기의 인스턴스에 추가할를 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)다음 코드 예제에서 설명한 것 처럼:

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

그러나 때문에 `CustomControl.SizeThatFits` 150 높이 항상 반환 하는 재정의 다음 스크린샷에 표시 된 것 처럼 뷰 위와 아래에 빈 공간을 사용 하 여 표시 됩니다.

![](code-images/ios-bad-measurement.png "잘못 된 SizeThatFits 구현과 CustomControl iOS")

이 문제에 솔루션을 제공 하는 것을 `GetDesiredSizeDelegate` 다음 코드 예제에 설명 된 대로 구현:

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

이 메서드는 제공한 너비는 `CustomControl.SizeThatFits` 70 높이 대 한 150의 높이 하지만 대체 하는 메서드. 경우는 `CustomControl` 인스턴스를 추가할는 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)의 `FixSize` 으로 메서드를 지정할 수 있습니다를 `GetDesiredSizeDelegate` 제공한 잘못 된 측정을 해결 하려면는 `CustomControl` 클래스:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

따라서 위와 아래에 빈 공간 없이 올바르게 표시 되 고 사용자 지정 보기에서 다음 스크린샷에 표시 된 대로:

![](code-images/ios-good-measurement.png "iOS CustomControl GetDesiredSize 재정의 사용 하 여")

### <a name="android"></a>Android

다음 코드 예제는 `CustomControl` 클래스에서 상속 되는 `TextView`:

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

이 보기의 인스턴스에 추가할를 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)다음 코드 예제에서 설명한 것 처럼:

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

그러나 때문에 `CustomControl.OnMeasure` 재정의 요청 된 너비의 절반 항목을 항상 반환, 뷰에 표시할 장치의 사용 가능한 너비를 절반만 차지 다음 스크린샷에 표시 된 대로:

![](code-images/android-bad-measurement.png "잘못 된 OnMeasure 구현과 android CustomControl")

이 문제에 솔루션을 제공 하는 것을 `GetDesiredSizeDelegate` 다음 코드 예제에 설명 된 대로 구현:

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

이 메서드는 제공한 너비는 `CustomControl.OnMeasure` 메서드 하지만 곱한 두 합니다. 경우는 `CustomControl` 인스턴스를 추가할는 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)의 `FixSize` 으로 메서드를 지정할 수 있습니다를 `GetDesiredSizeDelegate` 제공한 잘못 된 측정을 해결 하려면는 `CustomControl` 클래스:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

이 인해 되는 사용자 지정 보기에 올바르게 표시, 장치의 너비를 차지 하는 다음 스크린샷에 표시 된 대로:

![](code-images/android-good-measurement.png "사용자 지정 GetDesiredSize 대리자를 사용 하 여 android CustomControl")

### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

다음 코드 예제는 `CustomControl` 클래스에서 상속 되는 `Panel`:

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

이 보기의 인스턴스에 추가할를 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)다음 코드 예제에서 설명한 것 처럼:

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

그러나 때문에 `CustomControl.ArrangeOverride` 재정의 요청 된 너비의 절반 항목을 항상 반환, 다음 스크린샷에 표시 된 대로 장치는 사용 가능한 너비의 절반 클리핑됩니다 보기:

![](code-images/winrt-bad-measurement.png "잘못 된 ArrangeOverride 구현과 UWP CustomControl")

이 문제에 솔루션을 제공 하는 것을 `ArrangeOverrideDelegate` 뷰를 추가 하는 경우 구현 합니다 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)다음 코드 예제에서 설명한 것 처럼:

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

이 메서드는 제공한 너비는 `CustomControl.ArrangeOverride` 메서드 하지만 곱한 두 합니다. 이 인해 되는 사용자 지정 보기에 올바르게 표시, 장치의 너비를 차지 하는 다음 스크린샷에 표시 된 대로:

![](code-images/winrt-good-measurement.png "ArrangeOverride 대리자를 사용 하 여 UWP CustomControl")

## <a name="summary"></a>요약

이 문서에서는 C#을 사용 하 여 만든 Xamarin.Forms 레이아웃에 네이티브 뷰를 추가 하는 방법 및 해당 측정 API 사용을 수정 하려면 사용자 지정 보기 레이아웃을 재정의 하는 방법을 설명 합니다.


## <a name="related-links"></a>관련 링크

- [NativeEmbedding (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeEmbedding/)
- [네이티브 양식](~/xamarin-forms/platform/native-forms.md)
