---
title: Xamarin.forms에서 UrhoSharp를 사용 하 여
description: UrhoSharp는 사용 하 여 3D 그래픽 고급 시각화에 대 한 응용 프로그램에 추가할 수 있습니다.
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/11/2016
ms.openlocfilehash: 1a982078f7a3fb2ba462cd7d6f1420b1d27618f7
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/10/2018
---
# <a name="using-urhosharp-in-xamarinforms"></a>Xamarin.forms에서 UrhoSharp를 사용 하 여

## <a name="what-is-urhosharp"></a>UrhoSharp 란?

[UrhoSharp](~/graphics-games/urhosharp/index.md) Xamarin 및.NET 개발자 강력한 3D 엔진입니다. [소개](~/graphics-games/urhosharp/introduction.md) 추가로 UrhoSharp 라이브러리에 대 한 설명 및 [이러한 참고 사항은](~/graphics-games/urhosharp/using.md) 장면 및 동작을 프로그래밍 하는 방법에 설명 합니다.

UrhoSharp는 Xamarin.Forms 응용 프로그램에서 그래픽 렌더링 데 사용할 수 있습니다.
이 [샘플](https://github.com/xamarin/urho-samples/tree/master/FormsSample) 대화형 3D 차트를 생성 하 UrhoSharp 수 사용 하는 방법을 보여 줍니다.

![](urhosharp-images/ios-animation.gif "IOS에서 3D 대화형 차트 UrhoSharp")
![](urhosharp-images/android-animation.gif "UrhoSharp Android에서 3D 대화형 차트")

## <a name="adding-the-urhosharp-nuget-packages"></a>UrhoSharp Nuget 패키지를 추가합니다.

UrhoSharp를 사용 하기 전에 개발자는 솔루션에 UrhoSharp Nuget 패키지를 추가 해야 합니다. 이 가이드에서는 iOS, Android, 및 표준.NET을 사용 하 여 Xamarin.Forms 프로젝트 가정 라이브러리 프로젝트. .NET 표준 라이브러리 프로젝트; 기록 되어 모든 코드 하지만 너무 UrhoSharp Nuget iOS 및 Android 프로젝트에 추가 해야 합니다.

UrhoSharp.Forms Nuget 패키지는 모든 UrhoSharp 개체를 만드는 데 필요한 개체를 포함 합니다. UrhoSharp.Forms nuget 패키지에 포함 된 `UrhoSurface` xamarin.forms에서 UrhoSharp 호스트 하는 데 사용 되는 클래스입니다.
를 시작 하려면 마우스 오른쪽 단추로 클릭는 **패키지** 고 표준.NET 라이브러리 프로젝트에서 폴더 **패키지 추가 중...** . 검색어를 입력 **UrhoSharp.Forms**선택, **UrhoSharp Xamarin.Forms에 대해**, 클릭 **패키지 추가**합니다.

[![](urhosharp-images/add-package-sml.png "추가 패키지 대화 상자")](urhosharp-images/add-package.png#lightbox "패키지 대화 상자를 추가 합니다.")

UrhoSharp.Forms NuGet 패키지를 프로젝트에 추가 됩니다.

![](urhosharp-images/packages.png "패키지 폴더")

플랫폼별 프로젝트 (예: iOS 및 Android)에 대 한 위의 단계를 반복 합니다.

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>연습: UrhoSharp Xamarin.Forms 앱 추가

이러한 단계는 Xamarin.Forms UrhoSharp 샘플의 코드를 설명합니다.

1. [Xamarin Forms 페이지 만들기](#1)
2. [UrhoSurface 추가](#2)
3. [Urho 응용 프로그램 빌드](#3)
4. [에 UrhoSurface 차트 클래스 추가](#4)
5. [UrhoSharp와 상호 작용](#5)

참고 샘플에서는 C# 6 기능 및 이전 버전의 Visual Studio에서 컴파일되지 않을 수 있습니다.

<a name="1"/>

### <a name="1-create-a-xamarin-forms-page"></a>1. Xamarin Forms 페이지 만들기

아래 코드 Xamarin.Forms 페이지를 보여 줍니다. `UrhoPage` Urho 관련 코드 추가 되기 전에:

```csharp
public class UrhoPage : ContentPage
{
  Slider selectedBarSlider, rotationSlider;

  public UrhoPage()
  {
    // we'll add Urho later

    rotationSlider = new Slider(0, 500, 250);

    selectedBarSlider = new Slider(0, 5, 2.5);

    Title = " UrhoSharp + Xamarin.Forms";
    Content = new StackLayout {
      Padding = new Thickness (12, 12, 12, 40),
      VerticalOptions = LayoutOptions.FillAndExpand,
      Children = {
        rotationSlider,
        new Label { Text = "SELECTED VALUE:" },
        selectedBarSlider,
      }
    };
  }
```

<a name="2"/>

### <a name="2-add-the-urhosurface"></a>2. UrhoSurface 추가

UrhoSharp에서 호스팅할 수 있습니다는 `ContentPage` 다른 Xamarin.Forms 컨트롤과 같이 합니다.
다음 코드 조각은 `UrhoSurface` Xamarin.Forms 페이지에 추가 합니다.

```csharp
using Urho;
using Urho.Forms;
...
public class UrhoPage : ContentPage
{
  UrhoSurface urhoSurface;

  public UrhoPage()
  {
    urhoSurface = new UrhoSurface();
    urhoSurface.VerticalOptions = LayoutOptions.FillAndExpand;
...
    Content = new StackLayout {
    Padding = new Thickness (12, 12, 12, 40),
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = {
      urhoSurface,  // added
      new Label { Text = "ROTATION:" },
      rotationSlider,
      new Label { Text = "SELECTED VALUE:" },
      selectedBarSlider,
    }
  };
```

<a name="3"/>

### <a name="3-build-a-urho-application"></a>3. Urho 응용 프로그램 빌드

참조는 `Charts` 이 샘플에 사용 된 Urho 3D 그래픽의 구현에 대 한 클래스입니다. 기본 코드 개요 표시 된 아래-구현 하는 클래스를 참고 `Urho.Application` 에 다른는 `Xamarin.Forms.Application` 클래스에서 구현 되는 **App.cs**합니다.

```csharp
using Urho;
using Urho.Actions;
using Urho.Gui;
using Urho.Shapes;

namespace FormsSample
{
    public class Charts : Urho.Application
    {
    public Charts (ApplicationOptions options = null) : base(options) { }
    protected override void Start ()
    {
      ...
    }
    protected override void OnUpdate(float timeStep)
    {
      ...
    }
```

[UrhoSharp 설명서](~/graphics-games/urhosharp/index.md) 3D 장면 및 동작을 작성 하는 방법에 자세한 정보를 포함 합니다.

<a name="4"/>

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. 에 UrhoSurface 차트 클래스 추가

사용 된 `UrhoSurface.Show<T>` 제네릭 메서드에서 Xamarin.Forms 페이지로 Urho 응용 프로그램을 추가 합니다. 아래 코드 조각을 만드는 데 필요한 추가 코드를 보여 줍니다.는 `Charts` 클래스:

```csharp
public class UrhoPage : ContentPage
{
  Charts urhoApp;
  ...
  protected override async void OnAppearing ()
  {
    urhoApp = await urhoSurface.Show<Charts> (new ApplicationOptions(assetsFolder: null)
      { Orientation = ApplicationOptions.OrientationType.Portrait });
  }
```

참고:는 `Show<T>` 메서드는 비동기적 이며으로 호출할 수는 `await` 키워드입니다.

<a name="5"/>

### <a name="5-interacting-with-urhosharp"></a>5. UrhoSharp와 상호 작용

이 예제에서 차트 막대를를 선택 하 고 수정할 수 있습니다. `Charts` 클래스가 노출 된 `Bars` 및 `SelectedBar` 이 상호 작용할 수 있도록 합니다.

각 "막대" 뒤에 추가 선택 영역 이벤트 처리기에는 `Charts` 클래스 렌더링 된, 노출 된 반복 하 여 `Bars` 컬렉션:

```csharp
protected override async void OnAppearing ()
{
  urhoApp = await urhoSurface.Show<Charts>(new ApplicationOptions(assetsFolder: null) { Orientation = ApplicationOptions.OrientationType.Portrait });
  foreach (var bar in urhoApp.Bars)
  {
    bar.Selected += OnBarSelection;
  }
}
```

Xamarin.Forms는의 값을 사용 하는 이벤트 처리기 `Slider` 컨트롤을 지정 된 막대의 높이 조정 합니다.

```csharp
private void OnBarSelection(Bar bar)
{
  //reset value
  selectedBarSlider.ValueChanged -= OnValuesSliderValueChanged;
  selectedBarSlider.Value = bar.Value;
  selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
}

void OnValuesSliderValueChanged(object sender, ValueChangedEventArgs e)
{  // C# 6
  if (urhoApp?.SelectedBar != null)
  {
    urhoApp.SelectedBar.Value = (float)e.NewValue;
  }
}
```

두를 연결 하는 마지막으로, `Slider` 해당 값이 변경 될 때 UrhoSharp 캔버스 미치는 영향을 제어 합니다. 첫 번째 슬라이더 3D 차트 이미지를 회전 하 고 두 번째 슬라이더 선택한 막대의 높이 조정 합니다.

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

애니메이션에는 [페이지의 위쪽](#) 실행 되는 예제를 보여 줍니다.

## <a name="summary"></a>요약

이 페이지에서는 xamarin.forms 3D 데이터 시각화를 추가 하려면 UrhoSharp를 사용할 수 있는 방법을 보여 줍니다. 읽기는 [UrhoSharp 설명서](~/graphics-games/urhosharp/index.md) 위에 표시 된 메서드를 사용 하 여 Xamarin.Forms 앱에 포함 될 수 있는 Urho 장면 빌드하는 방법에 대 한 자세한 내용은 합니다.


## <a name="related-links"></a>관련 링크

- [차트 샘플 (C# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
