---
title: 손가락 모으기 제스처 인식기 추가
description: 이 문서에서는 손가락 모으기 제스처를 사용하여 손가락 모으기 위치에서 이미지의 대화형 확대/축소를 수행하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: da4a8bc66a7986efd3683de6dce1f6af618b85cc
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137854"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>손가락 모으기 제스처 인식기 추가

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pinchgesture)

_손가락 모으기 제스처는 대화형 확대/축소 작업을 수행하는 데 사용되며 PinchGestureRecognizer 클래스로 구현됩니다. 손가락 모으기 제스처에 대한 일반적인 시나리오는 손가락 모으기 위치에서 이미지의 대화형 확대/축소를 수행하는 것입니다. 이는 뷰포트의 콘텐츠를 크기 조정하여 수행되며, 이 문서에서 설명됩니다._

손가락 모으기 제스처를 사용하여 사용자 인터페이스 요소를 확대/축소 가능하도록 만들려면 [`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer) 인스턴스를 만들고, [`PinchUpdated`](xref:Xamarin.Forms.PinchGestureRecognizer.PinchUpdated) 이벤트를 처리하고, 사용자 인터페이스 요소의 [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) 컬렉션에 새 제스처 인식기를 추가합니다. 다음 코드 예제에서는 [`Image`](xref:Xamarin.Forms.Image) 요소에 연결된 `PinchGestureRecognizer`를 보여줍니다.

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

이 작업은 다음 코드 예제에 표시된 대로 XAML에서 수행할 수도 있습니다.

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

`OnPinchUpdated` 이벤트 처리기의 코드가 코드 숨김 파일에 추가됩니다.

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>PinchToZoom 컨테이너 만들기

확대/축소 작업을 수행하기 위해 손가락 모으기 제스처를 처리하려면 사용자 인터페이스를 변환하는 일부 수학이 필요합니다. 이 섹션에서는 모든 사용자 인터페이스 요소를 대화형으로 확대/축소하는 데 사용할 수 있는 수학을 수행하는 일반화된 도우미 클래스를 포함합니다. 다음 코드 예제는 `PinchToZoomContainer` 클래스를 보여줍니다.

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

손가락 모으기 제스처가 래핑된 사용자 인터페이스 요소를 확대/축소할 수 있도록 사용자 인터페이스 요소에서 이 클래스를 래핑할 수 있습니다. 다음 XAML 코드 예제에서는 [`Image`](xref:Xamarin.Forms.Image) 요소 `PinchToZoomContainer` 래핑을 보여줍니다.

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

다음 코드 예제에서는 `PinchToZoomContainer`가 C# 페이지에서 [`Image`](xref:Xamarin.Forms.Image) 요소를 래핑하는 방법을 보여줍니다.

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

[`Image`](xref:Xamarin.Forms.Image) 요소가 손가락 모으기 제스처를 수신하는 경우 표시된 이미지가 확대 또는 축소됩니다. 확대/축소는 `PinchZoomContainer.OnPinchUpdated` 메서드로 수행됩니다. 이는 다음 코드 예제에 나와 있습니다.

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

이 메서드는 사용자의 손가락 모으기 제스처에 따라 래핑된 사용자 인터페이스 요소의 확대/축소 수준을 업데이트합니다. 손가락 모으기 제스처의 방향에서 적용되는 배율을 계산하도록 [`PinchGestureUpdatedEventArgs`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs) 인스턴스의 [`Scale`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale), [`ScaleOrigin`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin) 및 [`Status`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Status) 속성의 값을 사용하여 수행됩니다. 그런 다음, 래핑된 사용자 요소는 손가락 모으기 제스처의 방향에서 해당 [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX), [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) 및 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성을 설정하여 계산된 값으로 확대/축소됩니다.

## <a name="related-links"></a>관련 링크

- [PinchGesture(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pinchgesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PinchGestureRecognizer](xref:Xamarin.Forms.PinchGestureRecognizer)
