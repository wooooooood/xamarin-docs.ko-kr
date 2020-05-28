---
title: UrhoSharp 사용Xamarin.Forms
description: 이 문서에서는 UrhoSharp를 사용 하 여 고급 시각화를 위해 3D 그래픽을 응용 프로그램에 추가 하는 방법을 설명 Xamarin.Forms 합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c0eed1a451d62025562ac5fff4f12be96f0bf53
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137659"
---
# <a name="using-urhosharp-in-xamarinforms"></a>UrhoSharp 사용Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/urho-samples/tree/master/FormsSample)

## <a name="what-is-urhosharp"></a>UrhoSharp 이란?

[Urhosharp](~/graphics-games/urhosharp/index.md) 는 Xamarin 및 .net 개발자를 위한 강력한 3d 엔진입니다. [소개](~/graphics-games/urhosharp/introduction.md) 에서는 UrhoSharp 라이브러리에 대해 자세히 설명 하 고, [이러한 메모](~/graphics-games/urhosharp/using.md) 는 장면 및 작업을 프로그래밍 하는 방법을 설명 합니다.

UrhoSharp는 응용 프로그램에서 그래픽을 렌더링 하는 데 사용할 수 있습니다 Xamarin.Forms .
이 [샘플](https://github.com/xamarin/urho-samples/tree/master/FormsSample) 에서는 UrhoSharp를 사용 하 여 대화형 3d 차트를 생성 하는 방법을 보여 줍니다.

![](urhosharp-images/ios-animation.gif "UrhoSharp 3D Interactive Chart on iOS")
![](urhosharp-images/android-animation.gif "UrhoSharp 3D Interactive Chart on Android")

## <a name="adding-the-urhosharp-nuget-packages"></a>UrhoSharp NuGet 패키지 추가

UrhoSharp를 사용 하기 전에 개발자는 솔루션에 UrhoSharp NuGet 패키지를 추가 해야 합니다. 이 가이드에서는 Xamarin.Forms 프로젝트가 iOS, Android 및 .NET Standard library 프로젝트를 사용 한다고 가정 합니다. 모든 코드는 .NET Standard library 프로젝트에 작성 됩니다. 하지만 UrhoSharp NuGet은 iOS 및 Android 프로젝트에도 추가 해야 합니다.

UrhoSharp. Forms NuGet 패키지에는 UrhoSharp 개체를 만드는 데 필요한 모든 개체가 포함 되어 있습니다. UrhoSharp. Forms NuGet 패키지에는 `UrhoSurface` UrhoSharp를 호스트 하는 데 사용 되는 클래스가 포함 되어 있습니다 Xamarin.Forms .
시작 하려면 .NET Standard library 프로젝트에서 **패키지** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **패키지 추가**...를 선택 합니다. 검색 **단어를 입력 하 고**, ** Xamarin.Forms urhosharp **를 선택 하 고, **패키지 추가**를 클릭 합니다.

[![](urhosharp-images/add-package-sml.png "Add Packages Dialog")](urhosharp-images/add-package.png#lightbox "Add Packages Dialog")

UrhoSharp. Forms NuGet 패키지는 프로젝트에 추가 됩니다.

![](urhosharp-images/packages.png "Packages Folder")

플랫폼별 프로젝트 (예: iOS 및 Android)에 대해 위의 단계를 반복 합니다.

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>연습: 앱에 UrhoSharp 추가 Xamarin.Forms

다음 단계는 urhosharp 샘플의 코드를 설명 합니다 Xamarin.Forms .

1. [Xamarin Forms 페이지 만들기](#1)
2. [UrhoSurface 추가](#2)
3. [Urho 응용 프로그램 빌드](#3)
4. [UrhoSurface에 차트 클래스 추가](#4)
5. [UrhoSharp와 상호 작용](#5)

이 샘플에서는 c # 6 기능을 사용 하며 이전 버전의 Visual Studio에서 컴파일되지 않을 수 있습니다.

<a name="1"/>

### <a name="1-create-a-xamarin-forms-page"></a>1. Xamarin Forms 페이지 만들기

다음 코드는 Xamarin.Forms `UrhoPage` urho 관련 코드가 추가 되기 전의 페이지를 보여 줍니다.

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

UrhoSharp는 다른 컨트롤 처럼에서 호스팅될 수 있습니다 `ContentPage` Xamarin.Forms .
아래 코드 조각은 페이지에 추가 된를 보여 줍니다 `UrhoSurface` Xamarin.Forms .

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

`Charts`이 샘플에서 사용 되는 Urho 3d 그래픽의 구현에 대 한 클래스를 참조 하세요. 기본 코드 개요는 아래와 같습니다. 클래스는 `Urho.Application` `Xamarin.Forms.Application` **App.cs**에서 구현 된 클래스와 다른을 구현 합니다.

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

[Urhosharp 설명서](~/graphics-games/urhosharp/index.md) 에는 3d 장면 및 작업을 빌드하는 방법에 대 한 자세한 내용이 포함 되어 있습니다.

<a name="4"/>

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. UrhoSurface에 차트 클래스를 추가 합니다.

`UrhoSurface.Show<T>`제네릭 메서드를 사용 하 여 Urho 응용 프로그램을 페이지에 추가 Xamarin.Forms 합니다. 아래 코드 조각에서는 클래스를 만드는 데 필요한 추가 코드를 보여 줍니다 `Charts` .

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

참고: `Show<T>` 메서드는 비동기 이며 키워드를 사용 하 여 호출 해야 합니다 `await` .

<a name="5"/>

### <a name="5-interacting-with-urhosharp"></a>5. UrhoSharp와 상호 작용

이 예에서는 차트 막대를 선택 하 고 수정할 수 있습니다. `Charts`클래스는 및를 노출 하 여 `Bars` `SelectedBar` 이 상호 작용을 가능 하 게 합니다.

각 "bar"에는 `Charts` 노출 된 컬렉션을 반복 하 여 클래스를 렌더링 한 후에 추가 된 선택 이벤트 처리기가 있습니다 `Bars` .

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

이벤트 처리기는 컨트롤의 값을 사용 하 여 Xamarin.Forms `Slider` 지정 된 가로 막대의 높이를 조정 합니다.

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

마지막으로, 두 `Slider` 컨트롤 값이 변경 될 때 UrhoSharp 캔버스가 영향을 받을 수 있도록 두 컨트롤을 연결 합니다. 첫 번째 슬라이더는 3D 차트 이미지를 회전 하 고 두 번째 슬라이더는 선택한 막대의 높이를 조정 합니다.

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

[페이지 맨 위에](#what-is-urhosharp) 있는 애니메이션에는 실행 중인 샘플이 표시 됩니다.

## <a name="summary"></a>요약

이 페이지에서는 UrhoSharp를 사용 하 여 3D 데이터 시각화를에 추가 하는 방법을 보여 줍니다 Xamarin.Forms . 위에 표시 된 방법을 사용 하 여 앱에 포함할 수 있는 Ur호 장면을 빌드하는 방법에 대 한 자세한 내용은 [Urhosharp 설명서](~/graphics-games/urhosharp/index.md) 를 참조 Xamarin.Forms 하세요.

## <a name="related-links"></a>관련 링크

- [차트 샘플 (c # 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
