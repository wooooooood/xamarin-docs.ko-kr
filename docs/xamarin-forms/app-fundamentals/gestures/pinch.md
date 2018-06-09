---
title: 축소 제스처 인식기를 추가합니다.
description: 이 문서에서는 축소 위치에서 이미지의 대화형 확대/축소를 수행 하는 축소 제스처를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 3600a8bf059bf29429cce35a233cc6618daa4d79
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241779"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>축소 제스처 인식기를 추가합니다.

_축소 제스처 대화형 확대/축소를 수행 하는 데 사용 되 고 PinchGestureRecognizer 클래스를 사용 하 여 구현 됩니다. 축소 제스처에 대 한 일반적인 시나리오는 축소 위치에서 이미지의 대화형 확대/축소를 수행 하는 것입니다. 이 뷰포트의 콘텐츠를 조정 하 여 수행 됩니다 하며이 문서에 나와 있습니다._

## <a name="overview"></a>개요

축소 제스처와 가능한 사용자 인터페이스 요소 하 게 하려면 만듭니다는 [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) 인스턴스를 처리는 [ `PinchUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PinchGestureRecognizer.PinchUpdated/) 이벤트를 새 제스처 인식기를 추가 하 고는 [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) 사용자 인터페이스 요소의 컬렉션입니다. 다음 코드 예제는 `PinchGestureRecognizer` 에 연결 된 프로그램 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 요소:

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

또한 이렇게 할 수 있습니다, XAML에서 다음 코드 예제에 나와 있는 것 처럼:

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

에 대 한 코드는 `OnPinchUpdated` 이벤트 처리기 코드 숨김 파일에 추가 됩니다.

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>PinchToZoom 컨테이너를 만듭니다.

확대/축소 작업을 수행 하려면 축소 제스처 처리 사용자 인터페이스를 변환 하는 일부 수학이 필요 합니다. 이 섹션 대화형 사용자 인터페이스 요소를 확대/축소를 사용할 수 있는 계산을 수행 하는 일반화 된 도우미 클래스를 포함 합니다. 다음 코드 예제는 `PinchToZoomContainer` 클래스를 보여줍니다.

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

축소 제스처 래핑된 사용자 인터페이스 요소 확대/축소 됩니다 되도록이 클래스는 사용자 인터페이스 요소 주위 줄 바꿈할 수 있습니다. 다음 XAML 코드 예제는 `PinchToZoomContainer` 래핑는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 요소:

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

다음 코드 예제는 방법을 `PinchToZoomContainer` 래핑하는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) C# 페이지에서 요소:

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

경우는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 요소가 축소 제스처를 받을, 표시 된 이미지는 수 확대/축소 됨 또는 합니다. 확대/축소에 의해 수행 됩니다는 `PinchZoomContainer.OnPinchUpdated` 메서드를 다음 코드 예제에 표시 됩니다.

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

이 메서드는 확대/축소 수준을 기반으로 사용자의 축소 제스처 래핑된 사용자 인터페이스 요소를 업데이트 합니다. 이 값을 사용 하 여 수행 된 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale/), [ `ScaleOrigin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin/) 및 [ `Status` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Status/) 의 속성은 [ `PinchGestureUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureUpdatedEventArgs/) 축소 제스처의 원점에 적용할 배율 인수를 계산 하는 인스턴스. 래핑된 사용자 요소 값이 축소 제스처의 원점을 설정 하 여 확대 한 다음 해당 [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/), 및 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 계산된 된 값으로 속성입니다.

## <a name="summary"></a>요약

축소 제스처 대화형 확대/축소를 수행 하는 데 사용 되 고 사용 하 여 구현 된 [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) 클래스입니다.


## <a name="related-links"></a>관련 링크

- [PinchGesture (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PinchGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/)
