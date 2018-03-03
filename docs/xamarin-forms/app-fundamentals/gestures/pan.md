---
title: "팬 제스처 인식기를 추가합니다."
description: "팬 제스처 끌어 감지 하는 데 사용 되 고 PanGestureRecognizer 클래스를 사용 하 여 구현 됩니다. 팬 제스처에 대 한 일반적인 시나리오는 가로 또는 세로로 끌어 이미지 되므로 이미지 크기 보다 작은 뷰포트에 표시 되는 때 모든 이미지 콘텐츠를 볼 수 있습니다. 이 뷰포트 내 이미지를 이동 하 여 수행 됩니다 하며이 문서에 나와 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 4da42a92c83dcc1ec0b0ba2528de672e3fcf3be3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="adding-a-pan-gesture-recognizer"></a>팬 제스처 인식기를 추가합니다.

_팬 제스처 끌어 감지 하는 데 사용 되 고 PanGestureRecognizer 클래스를 사용 하 여 구현 됩니다. 팬 제스처에 대 한 일반적인 시나리오는 가로 또는 세로로 끌어 이미지 되므로 이미지 크기 보다 작은 뷰포트에 표시 되는 때 모든 이미지 콘텐츠를 볼 수 있습니다. 이 뷰포트 내 이미지를 이동 하 여 수행 됩니다 하며이 문서에 나와 있습니다._

## <a name="overview"></a>개요

사용자 인터페이스 요소를 팬 제스처와 draggable 있도록 만들는 [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) 인스턴스를 처리는 [ `PanUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PanGestureRecognizer.PanUpdated/) 이벤트를 새 제스처 인식기를 추가 하 고는 [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) 사용자 인터페이스 요소의 컬렉션입니다. 다음 코드 예제는 `PanGestureRecognizer` 에 연결 된 프로그램 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 요소:

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

또한 이렇게 할 수 있습니다, XAML에서 다음 코드 예제에 나와 있는 것 처럼:

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

에 대 한 코드는 `OnPanUpdated` 이벤트 처리기 코드 숨김 파일에 추가 됩니다.

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

> [!NOTE]
> Android에 대 한 올바른 패닝이 필요는 [Xamarin.Forms 2.1.0-pre1 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1) 최소한 합니다.

## <a name="creating-a-pan-container"></a>팬 컨테이너를 만듭니다.

이 섹션은 일반적으로 적합 이미지 또는 맵 내에서 탐색 하는 자유형 이동을 수행 하는 일반화 된 도우미 클래스를 포함 합니다. 끌기 작업을 수행 하려면 팬 제스처 처리 사용자 인터페이스를 변환 하는 일부 수학이 필요 합니다. 이 수치를 끌기 래핑된 사용자 인터페이스 요소의 경계 내 에서만 사용 됩니다. 다음 코드 예제는 `PanContainer` 클래스를 보여줍니다.

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

이 클래스 팬 제스처를 래핑된 사용자 인터페이스 요소를 끌어 있도록 사용자 인터페이스 요소 주위에 배치할 수 있습니다. 다음 XAML 코드 예제는 `PanContainer` 래핑는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 요소:

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

다음 코드 예제는 방법을 `PanContainer` 래핑하는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) C# 페이지에서 요소:

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

두 예에서 모두는 [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) 및 [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) 속성이 표시 되는 이미지의 너비 및 높이 값으로 설정 됩니다.

경우는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 요소가 팬 제스처를 받을, 표시 된 이미지 드래그 됩니다. 끌기 수행한는 `PanContainer.OnPanUpdated` 다음 코드 예제에 표시 된 메서드:

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

이 메서드는 사용자의 팬 제스처에 따라 래핑된 사용자 인터페이스 요소를 볼 수 있는 콘텐츠를 업데이트 합니다. 이 값을 사용 하 여 수행 된 [ `TotalX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalX/) 및 [ `TotalY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalY/) 의 속성은 [ `PanUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanUpdatedEventArgs/) 방향을 계산 하는 인스턴스 및 팬의 거리입니다. `App.ScreenWidth` 및 `App.ScreenHeight` 속성 뷰포트의 너비와 높이 제공 하 고 해당 플랫폼별 프로젝트에서 화면 너비 및 장치의 화면 높이 값으로 설정 됩니다. 래핑된 사용자 요소를 설정 하 여 끌 다음 해당 [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) 및 [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) 계산된 된 값으로 속성입니다.

뷰포트 너비와 높이가 요소에서 가져올 수 전체 화면을 차지 하지 않는 프로그램 요소에 콘텐츠를 이동 하는 경우 [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) 및 [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) 속성입니다.

> [!NOTE]
> 고해상도 이미지 표시는 응용 프로그램의 메모리 사용 공간을 크게 증가할 수 있습니다. 따라서 이러한만 만들지 필요 하 고 응용 프로그램에서 더 이상 요구 되는 즉시 해제 되어야 합니다. 자세한 내용은 [이미지 리소스 최적화](~/xamarin-forms/deploy-test/performance.md#optimizeimages)를 참조하세요.

## <a name="summary"></a>요약

팬 제스처 끌어 감지 하는 데 사용 되 고 사용 하 여 구현 된 [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) 클래스입니다.



## <a name="related-links"></a>관련 링크

- [PanGesture (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PanGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/)
