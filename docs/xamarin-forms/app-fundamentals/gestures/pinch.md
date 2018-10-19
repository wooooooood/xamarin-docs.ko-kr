---
title: 축소 제스처 인식기를 추가합니다.
description: 이 문서에서는 축소 위치에서 이미지의 대화형 확대/축소를 수행 하려면 축소 제스처를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: f67cbb136c42a4bc476c1715ea6fd15255d71dc7
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2018
ms.locfileid: "38998704"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>축소 제스처 인식기를 추가합니다.

_축소 제스처 대화형 확대/축소를 수행 하는 데 사용 되 고 PinchGestureRecognizer 클래스를 사용 하 여 구현 됩니다. 축소 제스처의 일반적인 시나리오는 축소 위치에서 이미지의 대화형 확대/축소를 수행 하는 것입니다. 뷰포트의 콘텐츠를 확장 하 여 수행 됩니다 하 고이 문서에서 설명 됩니다._

축소 제스처를 사용 하 여 가능한 사용자 인터페이스 요소를 확인, 만들려면를 [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer) 인스턴스를 처리 합니다 [ `PinchUpdated` ](xref:Xamarin.Forms.PinchGestureRecognizer.PinchUpdated) 이벤트를 새 제스처 인식기를 추가 하 고는 [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) 사용자 인터페이스 요소에는 컬렉션입니다. 다음 코드 예제는 `PinchGestureRecognizer` 에 연결을 [ `Image` ](xref:Xamarin.Forms.Image) 요소:

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

이 작업은 또한 다음 코드 예제에 표시 된 대로 XAML에도 수행할 수 있습니다:

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

에 대 한 코드는 `OnPinchUpdated` 이벤트 처리기가 코드 숨김 파일에 추가 합니다.

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>PinchToZoom 컨테이너를 만듭니다.

확대/축소 작업을 수행 하려면 축소 제스처 처리 사용자 인터페이스를 변환 하기 위한 일부 계산에 필요 합니다. 이 섹션에서는 대화형 사용자 인터페이스 요소를 확대/축소를 사용할 수 있는 계산을 수행 하는 일반화 된 도우미 클래스를 포함 합니다. 다음 코드 예제는 `PinchToZoomContainer` 클래스를 보여줍니다.

```csharp
public class PinchToZoomContainer : ContentView
{
  ...

  public PinchToZoomContainer ()
  {
    var pinchGesture = new PinchGestureRecognizer ();
    pinchGesture.PinchUpdated += OnPinchUpdated;
    GestureRecognizers.Add (pinchGesture);
  }

  void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
  {
    ...
  }
}
```

축소 제스처는 래핑된 사용자 인터페이스 요소를 확대/축소 되도록 관련 사용자 인터페이스 요소를이 클래스를 래핑할 수 있습니다. 다음 XAML 코드 예제에서는 합니다 `PinchToZoomContainer` 래핑하는 [ `Image` ](xref:Xamarin.Forms.Image) 요소:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PinchGesture;assembly=PinchGesture"
             x:Class="PinchGesture.HomePage">
    <ContentPage.Content>
        <Grid Padding="20">
            <local:PinchToZoomContainer>
                <local:PinchToZoomContainer.Content>
                    <Image Source="waterfront.jpg" />
                </local:PinchToZoomContainer.Content>
            </local:PinchToZoomContainer>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

다음 코드 예제에서는 하는 방법을 `PinchToZoomContainer` 래핑하는 [ `Image` ](xref:Xamarin.Forms.Image) C# 페이지의 요소:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new Grid {
      Padding = new Thickness (20),
      Children = {
        new PinchToZoomContainer {
          Content = new Image { Source = ImageSource.FromFile ("waterfront.jpg") }
        }
      }
    };
  }
}
```

경우는 [ `Image` ](xref:Xamarin.Forms.Image) 요소 축소 제스처에서 받을, 표시 된 이미지는 사용할 확대/축소 됨 또는 참여 합니다. 확대/축소에 의해 수행 됩니다는 `PinchZoomContainer.OnPinchUpdated` 메서드를 다음 코드 예제에 표시 됩니다.

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  if (e.Status == GestureStatus.Started) {
    // Store the current scale factor applied to the wrapped user interface element,
    // and zero the components for the center point of the translate transform.
    startScale = Content.Scale;
    Content.AnchorX = 0;
    Content.AnchorY = 0;
  }
  if (e.Status == GestureStatus.Running) {
    // Calculate the scale factor to be applied.
    currentScale += (e.Scale - 1) * startScale;
    currentScale = Math.Max (1, currentScale);

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the X pixel coordinate.
    double renderedX = Content.X + xOffset;
    double deltaX = renderedX / Width;
    double deltaWidth = Width / (Content.Width * startScale);
    double originX = (e.ScaleOrigin.X - deltaX) * deltaWidth;

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the Y pixel coordinate.
    double renderedY = Content.Y + yOffset;
    double deltaY = renderedY / Height;
    double deltaHeight = Height / (Content.Height * startScale);
    double originY = (e.ScaleOrigin.Y - deltaY) * deltaHeight;

    // Calculate the transformed element pixel coordinates.
    double targetX = xOffset - (originX * Content.Width) * (currentScale - startScale);
    double targetY = yOffset - (originY * Content.Height) * (currentScale - startScale);

    // Apply translation based on the change in origin.
    Content.TranslationX = targetX.Clamp (-Content.Width * (currentScale - 1), 0);
    Content.TranslationY = targetY.Clamp (-Content.Height * (currentScale - 1), 0);

    // Apply scale factor.
    Content.Scale = currentScale;
  }
  if (e.Status == GestureStatus.Completed) {
    // Store the translation delta's of the wrapped user interface element.
    xOffset = Content.TranslationX;
    yOffset = Content.TranslationY;
  }
}
```

이 메서드는 사용자의 축소 제스처에 따라 래핑된 사용자 인터페이스 요소의 확대/축소 수준을 업데이트 합니다. 값을 사용 하 여 이렇게 합니다 [ `Scale` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale), [ `ScaleOrigin` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin) 및 [ `Status` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Status) 속성을는 [ `PinchGestureUpdatedEventArgs` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs) 인스턴스 축소 제스처의 원점에 적용할 크기 조정 비율을 계산 합니다. 래핑된 사용자 정의 요소 값이 축소 제스처의 원점을 설정 하 여 확대 한 다음 해당 [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX)하십시오 [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY), 및 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 계산된 된 값으로 속성입니다.

## <a name="related-links"></a>관련 링크

- [PinchGesture (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PinchGestureRecognizer](xref:Xamarin.Forms.PinchGestureRecognizer)
