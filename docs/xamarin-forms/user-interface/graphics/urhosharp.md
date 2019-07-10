---
title: Xamarin.Forms에서 UrhoSharp 사용
description: 이 문서는 고급 시각화에 대 한 Xamarin.Forms 응용 프로그램에 3D 그래픽을 추가할 UrhoSharp를 사용할 수 있는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/11/2016
ms.openlocfilehash: fc316a9e6ab4261eaa956a987b47aeaf546344a2
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675268"
---
# <a name="using-urhosharp-in-xamarinforms"></a>Xamarin.Forms에서 UrhoSharp 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/urho-samples/tree/master/FormsSample)

## <a name="what-is-urhosharp"></a>UrhoSharp 란?

[UrhoSharp](~/graphics-games/urhosharp/index.md) 는 Xamarin 및.NET 개발자를 위한 강력한 3D 엔진입니다. 합니다 [소개](~/graphics-games/urhosharp/introduction.md) UrhoSharp 라이브러리에 대 한 더 설명 및 [이러한 정보](~/graphics-games/urhosharp/using.md) 장면 및 동작을 프로그래밍 하는 방법에 설명 합니다.

UrhoSharp는 Xamarin.Forms 응용 프로그램에서 그래픽을 렌더링에 사용할 수 있습니다.
이렇게 [샘플](https://github.com/xamarin/urho-samples/tree/master/FormsSample) UrhoSharp 대화형 3D 차트를 생성 하는 방법을 사용할 수 있습니다 하는 방법을 보여 줍니다.

![](urhosharp-images/ios-animation.gif "IOS에서 3D 대화형 차트 UrhoSharp")
![](urhosharp-images/android-animation.gif "Android에서 UrhoSharp 3D 대화형 차트")

## <a name="adding-the-urhosharp-nuget-packages"></a>UrhoSharp Nuget 패키지 추가

UrhoSharp 사용 하기 전에 개발자가 솔루션에 UrhoSharp Nuget 패키지를 추가 해야 합니다. 이 가이드에서는 iOS, Android 및.NET Standard를 사용 하 여 Xamarin.Forms 프로젝트 가정 라이브러리 프로젝트. 코드의 모든.NET Standard 라이브러리 프로젝트에 기록 됩니다. 하지만 너무 UrhoSharp Nuget iOS 및 Android 프로젝트에 추가 해야 합니다.

UrhoSharp.Forms Nuget 패키지는 모든 UrhoSharp 개체를 만드는 데 필요한 개체를 포함 합니다. UrhoSharp.Forms nuget 패키지에 포함 된 `UrhoSurface` Xamarin.Forms에서 UrhoSharp 호스팅하는 데 사용 하는 클래스입니다.
시작 하려면 마우스 오른쪽 단추로 클릭 합니다 **패키지** 선택한.NET Standard 라이브러리 프로젝트 폴더 **패키지 추가...** . 검색 용어를 입력 **UrhoSharp.Forms**를 선택 **Xamarin.Forms 용 UrhoSharp**, 클릭 **패키지 추가**합니다.

[![](urhosharp-images/add-package-sml.png "추가 패키지 대화 상자")](urhosharp-images/add-package.png#lightbox "패키지 대화 상자를 추가 합니다.")

UrhoSharp.Forms NuGet 패키지를 프로젝트에 추가 됩니다.

![](urhosharp-images/packages.png "패키지 폴더")

플랫폼별 프로젝트 (예: iOS 및 Android)에 대해 위의 단계를 반복 합니다.

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>연습: UrhoSharp는 Xamarin.Forms 앱에 추가

이러한 단계는 Xamarin.Forms UrhoSharp 샘플의 코드를 설명합니다.

1. [Xamarin Forms 페이지 만들기](#1)
2. [UrhoSurface 추가](#2)
3. [Urho 응용 프로그램 빌드](#3)
4. [에 UrhoSurface 차트 클래스 추가](#4)
5. [UrhoSharp 상호 작용](#5)

샘플을 사용 하는 C# 6 기능 및 이전 버전의 Visual Studio에서 컴파일할 수 없습니다.

<a name="1"/>

### <a name="1-create-a-xamarin-forms-page"></a>1. Xamarin Forms 페이지 만들기

아래 코드는 Xamarin.Forms 페이지를 보여 줍니다. `UrhoPage` Urho 관련 코드 추가 되었습니다.

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

UrhoSharp에서 호스팅될 수는 `ContentPage` 다른 Xamarin.Forms 컨트롤과 같이 합니다.
아래 코드는 `UrhoSurface` Xamarin.Forms 페이지에 추가 합니다.

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

참조 된 `Charts` Urho 3D 그래픽이이 샘플에 사용 되는 구현의 클래스입니다. 기본 코드 개요 표시 됩니다 아래-구현 하는 클래스를 확인 `Urho.Application` 에 다른를 `Xamarin.Forms.Application` 클래스에서 구현 되 **App.cs**.

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

합니다 [UrhoSharp 설명서](~/graphics-games/urhosharp/index.md) 3D 장면 및 작업을 작성 하는 방법에 대 한 자세한 정보를 포함 합니다.

<a name="4"/>

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. 에 UrhoSurface 차트 클래스 추가

사용 하 여는 `UrhoSurface.Show<T>` 제네릭 메서드 Xamarin.Forms 페이지 Urho 응용 프로그램을 추가 합니다. 아래 코드 조각을 만드는 데 필요한 추가 코드를 보여 줍니다.는 `Charts` 클래스:

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

참고: 합니다 `Show<T>` 메서드는 비동기적 이며 호출 해야 합니다 `await` 키워드입니다.

<a name="5"/>

### <a name="5-interacting-with-urhosharp"></a>5. UrhoSharp 상호 작용

예제 차트 막대를를 선택 하 고 수정할 수 있습니다. 합니다 `Charts` 노출 클래스는 `Bars` 및 `SelectedBar` 이 상호 작용할 수 있도록 합니다.

각 "bar"가 뒤에 추가 이벤트 처리기를 선택 합니다 `Charts` 클래스 렌더링 된, 노출 된 반복 하 여 `Bars` 컬렉션:

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

Xamarin.Forms의 값을 사용 하는 이벤트 처리기 `Slider` 지정 된 막대의 높이 조정 하도록 제어 합니다.

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

마지막으로 두 개의 연결 `Slider` UrhoSharp 캔버스 영향을 받는 경우 해당 값이 변경 되도록 제어 합니다. 3D 차트 이미지를 회전 하는 첫 번째 슬라이더 및 두 번째 슬라이더 선택한 막대의 높이 조정 합니다.

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

애니메이션 합니다 [페이지의 위쪽](#what-is-urhosharp) 샘플 실행을 표시 합니다.

## <a name="summary"></a>요약

이 페이지는 Xamarin.Forms에 3D 데이터 시각화를 추가할 UrhoSharp를 사용할 수 있는 방법을 보여 줍니다. 읽기를 [UrhoSharp 설명서](~/graphics-games/urhosharp/index.md) Urho 장면 위에 표시 된 메서드를 사용 하 여 Xamarin.Forms 앱에 포함 될 수 있는 빌드하는 방법에 대 한 자세한 내용은 합니다.


## <a name="related-links"></a>관련 링크

- [샘플 차트 (C# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
