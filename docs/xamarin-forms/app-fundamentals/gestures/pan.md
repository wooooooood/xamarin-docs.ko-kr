---
title: 이동 제스처 인식기 추가
description: 이 문서에서는 이미지 콘텐츠가 이미지 차원보다 작게 뷰포트에 표시되는 경우 모든 이미지 콘텐츠를 볼 수 있도록 이동 제스처를 사용하여 이미지를 가로 및 세로로 이동하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 73e312a1af56091a7e579d3fcbcea810ee0efb1e
ms.sourcegitcommit: 266e75fa6893d3732e4e2c0c8e79c62be2804468
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820974"
---
# <a name="adding-a-pan-gesture-recognizer"></a>이동 제스처 인식기 추가

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pangesture)

_이동 제스처는 화면에서 손가락의 움직임을 감지하고 이 움직임을 콘텐츠에 적용하는 데 사용되며 `PanGestureRecognizer` 클래스로 구현됩니다. 이동 제스처에 대한 일반적인 시나리오는 이미지 콘텐츠가 이미지 차원보다 작게 뷰포트에 표시되는 경우 모든 이미지 콘텐츠를 볼 수 있도록 이미지를 가로 및 세로로 이동하는 것입니다. 이는 뷰포트 내의 이미지를 이동하여 수행되며 이 문서에서 설명됩니다._

팬 제스처를 사용하여 사용자 인터페이스 요소를 이동 가능하도록 만들려면 [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) 인스턴스를 만들고, [`PanUpdated`](xref:Xamarin.Forms.PanGestureRecognizer.PanUpdated) 이벤트를 처리하고, 사용자 인터페이스 요소의 [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) 컬렉션에 새 제스처 인식기를 추가합니다. 다음 코드 예제에서는 [`Image`](xref:Xamarin.Forms.Image) 요소에 연결된 `PanGestureRecognizer`를 보여줍니다.

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

이 작업은 다음 코드 예제에 표시된 대로 XAML에서 수행할 수도 있습니다.

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

`OnPanUpdated` 이벤트 처리기의 코드가 코드 숨김 파일에 추가됩니다.

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

## <a name="creating-a-pan-container"></a>이동 컨테이너 만들기

이 섹션에서는 일반적으로 이미지 또는 맵 내에서 탐색에 적합한 자유 형식 이동을 수행하는 일반화된 도우미 클래스를 포함합니다. 이 작업을 수행하기 위해 이동 제스처를 처리하려면 사용자 인터페이스를 변환하는 일부 수학이 필요합니다. 이 수학은 래핑된 사용자 인터페이스 요소의 경계 내에서만 이동하는 데 사용됩니다. 다음 코드 예제는 `PanContainer` 클래스를 보여줍니다.

```csharp
public class PanContainer : ContentView
{
  double x, y;

  public PanContainer ()
  {
    // Set PanGestureRecognizer.TouchPoints to control the
    // number of touch points needed to pan
    var panGesture = new PanGestureRecognizer ();
    panGesture.PanUpdated += OnPanUpdated;
    GestureRecognizers.Add (panGesture);
  }

  void OnPanUpdated (object sender, PanUpdatedEventArgs e)
  {
    ...
  }
}
```

제스처가 래핑된 사용자 인터페이스 요소를 이동할 수 있도록 사용자 인터페이스 요소에서 이 클래스를 래핑할 수 있습니다. 다음 XAML 코드 예제에서는 [`Image`](xref:Xamarin.Forms.Image) 요소 `PanContainer` 래핑을 보여줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PanGesture"
             x:Class="PanGesture.HomePage">
    <ContentPage.Content>
        <AbsoluteLayout>
            <local:PanContainer>
                <Image Source="MonoMonkey.jpg" WidthRequest="1024" HeightRequest="768" />
            </local:PanContainer>
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

다음 코드 예제에서는 `PanContainer`가 C# 페이지에서 [`Image`](xref:Xamarin.Forms.Image) 요소를 래핑하는 방법을 보여줍니다.

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new AbsoluteLayout {
      Padding = new Thickness (20),
      Children = {
        new PanContainer {
          Content = new Image {
            Source = ImageSource.FromFile ("MonoMonkey.jpg"),
            WidthRequest = 1024,
            HeightRequest = 768
          }
        }
      }
    };
  }
}
```

두 예제에서 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성은 표시되는 이미지의 너비 및 높이 값으로 설정됩니다.

[`Image`](xref:Xamarin.Forms.Image) 요소가 이동 제스처를 수신하는 경우 표시된 이미지가 이동됩니다. 이동은 `PanContainer.OnPanUpdated` 메서드로 수행됩니다. 이는 다음 코드 예제에 나와 있습니다.

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  switch (e.StatusType) {
  case GestureStatus.Running:
    // Translate and ensure we don't pan beyond the wrapped user interface element bounds.
    Content.TranslationX =
      Math.Max (Math.Min (0, x + e.TotalX), -Math.Abs (Content.Width - App.ScreenWidth));
    Content.TranslationY =
      Math.Max (Math.Min (0, y + e.TotalY), -Math.Abs (Content.Height - App.ScreenHeight));
    break;

  case GestureStatus.Completed:
    // Store the translation applied during the pan
    x = Content.TranslationX;
    y = Content.TranslationY;
    break;
  }
}
```

이 메서드는 사용자의 이동 제스처에 따라 래핑된 사용자 인터페이스 요소의 볼 수 있는 콘텐츠를 업데이트합니다. 이동의 방향 및 거리를 계산하는 [`PanUpdatedEventArgs`](xref:Xamarin.Forms.PanUpdatedEventArgs) 인스턴스의 [`TotalX`](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalX) 및 [`TotalY`](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalY) 속성의 값을 사용하여 수행됩니다. `App.ScreenWidth` 및 `App.ScreenHeight` 속성은 뷰포트의 높이 및 너비를 제공하며, 해당 플랫폼별 프로젝트에 의해 디바이스의 화면 너비 및 화면 높이 값으로 설정됩니다. 그런 다음, 래핑된 사용자 요소는 계산된 값으로 해당 [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) 및 [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) 속성을 설정하여 이동됩니다.

전체 화면을 차지하지 않는 요소에서 콘텐츠를 이동하는 경우 뷰포트의 높이 및 너비는 요소의 [`Height`](xref:Xamarin.Forms.VisualElement.Height) 및 [`Width`](xref:Xamarin.Forms.VisualElement.Width) 속성에서 가져올 수 있습니다.

> [!NOTE]
> 고해상도 이미지를 표시하면 앱의 메모리 공간이 크게 증가할 수 있습니다. 따라서 필요한 경우에만 만들어야 하며, 앱에 더 이상 필요하지 않을 경우 즉시 해제되어야 합니다. 자세한 내용은 [이미지 리소스 최적화](~/xamarin-forms/deploy-test/performance.md#optimize-image-resources)를 참조하세요.

## <a name="related-links"></a>관련 링크

- [PanGesture(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pangesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PanGestureRecognizer](xref:Xamarin.Forms.PanGestureRecognizer)
