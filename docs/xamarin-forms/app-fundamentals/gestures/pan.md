---
title: 팬 제스처 인식기를 추가합니다.
description: 이 문서에서는 이미지 크기 보다 작은 뷰포트에서 표시 되는 경우 모든 이미지 콘텐츠를 볼 수 있습니다 가로로 이동 제스처를 사용 하 여 이미지를 세로로 이동 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 59e9f4c61bda86faa5a55d70ef91411adb14da6d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2018
ms.locfileid: "38996808"
---
# <a name="adding-a-pan-gesture-recognizer"></a>팬 제스처 인식기를 추가합니다.

_팬 제스처 화면에서 이리저리 손가락의 움직임을 감지 하 고 콘텐츠에 대 한 이동을 적용 되 고 사용 하 여 구현 됩니다는 `PanGestureRecognizer` 클래스입니다. 팬 제스처에 대 한 일반적인 시나리오는를 가로 및 세로로 이동 이미지 하므로 이미지 크기 보다 작은 뷰포트에서 표시 되는 경우 모든 이미지 콘텐츠를 볼 수 있습니다. 이 지정 된 뷰포트 내에 있는 이미지를 이동 하 여 수행 됩니다 및이 문서에서 설명 됩니다._

팬 제스처를 사용 하 여 이동 가능한 사용자 인터페이스 요소를 확인, 만들려면를 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) 인스턴스를 처리 합니다 [ `PanUpdated` ](xref:Xamarin.Forms.PanGestureRecognizer.PanUpdated) 이벤트를 새 제스처 인식기를 추가 하 고는 [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) 사용자 인터페이스 요소에는 컬렉션입니다. 다음 코드 예제는 `PanGestureRecognizer` 에 연결을 [ `Image` ](xref:Xamarin.Forms.Image) 요소:

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

이 작업은 또한 다음 코드 예제에 표시 된 대로 XAML에도 수행할 수 있습니다:

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

에 대 한 코드는 `OnPanUpdated` 이벤트 처리기가 코드 숨김 파일에 추가 합니다.

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

> [!NOTE]
> Android에서 올바른 이동 해야 합니다 [Xamarin.Forms 2.1.0-pre1 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1) 최소한 합니다.

## <a name="creating-a-pan-container"></a>팬 컨테이너를 만듭니다.

이 섹션에서는 일반적으로 이미지 또는 맵 내에서 탐색에 적합 한 자유 형식 이동을 수행 하는 일반화 된 도우미 클래스를 포함 합니다. 이 작업을 수행 하려면 팬 제스처 처리 사용자 인터페이스를 변환 하기 위한 일부 계산에 필요 합니다. 이 수치는 래핑된 사용자 인터페이스 요소의 경계 내 에서만 이동 하는 데 사용 됩니다. 다음 코드 예제는 `PanContainer` 클래스를 보여줍니다.

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

제스처는 래핑된 사용자 인터페이스 요소를 이동할 수 있도록 사용자 인터페이스 요소 주위의이 클래스를 래핑할 수 있습니다. 다음 XAML 코드 예제에서는 합니다 `PanContainer` 래핑하는 [ `Image` ](xref:Xamarin.Forms.Image) 요소:

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

다음 코드 예제에서는 하는 방법을 `PanContainer` 래핑하는 [ `Image` ](xref:Xamarin.Forms.Image) C# 페이지의 요소:

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

두 예제에서는 합니다 [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) 하 고 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성이 표시 되는 이미지의 너비 및 높이 값으로 설정 됩니다.

경우는 [ `Image` ](xref:Xamarin.Forms.Image) 팬 제스처를 수신 하는 요소, 표시 된 이미지를 이동 합니다. 이동 하 여 수행 됩니다는 `PanContainer.OnPanUpdated` 메서드를 다음 코드 예제에 나와 있는:

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

이 메서드는 사용자의 팬 제스처에 따라 래핑된 사용자 인터페이스 요소의 볼 수 있는 콘텐츠를 업데이트 합니다. 값을 사용 하 여 이렇게 합니다 [ `TotalX` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalX) 및 [ `TotalY` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalY) 의 속성을 [ `PanUpdatedEventArgs` ](xref:Xamarin.Forms.PanUpdatedEventArgs) 방향을 계산 하는 인스턴스 및 pan의 거리입니다. 합니다 `App.ScreenWidth` 고 `App.ScreenHeight` 속성 뷰포트의 너비와 높이 제공 하 고 해당 플랫폼 특정 프로젝트에서 화면 너비 및 장치의 화면 높이 값으로 설정 됩니다. 래핑된 사용자 정의 요소는 다음 설정으로 이동 하는 [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) 하 고 [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) 계산된 된 값으로 속성입니다.

뷰포트의 너비와 높이가 전체 화면을 차지 하지 않습니다 하는 요소에서는 콘텐츠를 이동 하는 경우 요소에서 가져올 수 있습니다 [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) 하 고 [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) 속성입니다.

> [!NOTE]
> 고해상도 이미지를 표시 합니다. 앱의 메모리 사용 공간이 크게 증가할 수 있습니다. 따라서 이러한만 만들어야 필요 하 고 앱에서 더 이상 필요 하는 즉시 해제 되어야 합니다. 자세한 내용은 [이미지 리소스 최적화](~/xamarin-forms/deploy-test/performance.md#optimizeimages)를 참조하세요.

## <a name="related-links"></a>관련 링크

- [PanGesture (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PanGestureRecognizer](xref:Xamarin.Forms.PanGestureRecognizer)
