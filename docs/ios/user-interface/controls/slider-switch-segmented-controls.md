---
title: Xamarin.ios의 슬라이더, 스위치 및 분할 컨트롤
description: 이 문서에서는 Xamarin.ios의 슬라이드, 스위치 및 분할 된 컨트롤을 설명 하 고 프로그래밍 방식으로 iOS 디자이너에서 작업 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 763165f1e09f847745b820987f8dbbae8f834fd7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021958"
---
# <a name="sliders-switches-and-segmented-controls-in-xamarinios"></a>Xamarin.ios의 슬라이더, 스위치 및 분할 컨트롤

<a name="Sliders" />

## <a name="sliders"></a>슬라이더

슬라이더 컨트롤을 사용 하면 범위 내에서 숫자 값을 간단히 선택할 수 있습니다. 컨트롤의 기본값은 0에서 1 사이의 값 이지만 이러한 한도를 사용자 지정할 수 있습니다.

 [![](slider-switch-segmented-controls-images/image25a.png "Slider")](slider-switch-segmented-controls-images/image25a.png#lightbox)

다음 스크린샷은 디자이너에서 편집할 수 있는 속성을 보여 줍니다.

 [![](slider-switch-segmented-controls-images/image26a.png "Slider Properties")](slider-switch-segmented-controls-images/image25a.png#lightbox)

`UILabel` 컨트롤에서 현재 선택 된 값을 표시 하는 처리기를 연결 하는 것을 포함 하 여 아래와 같이 코드에서 이러한 값을 설정할 수 있습니다.

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

또한를 설정 하 여 슬라이더의 시각적 모양을 사용자 지정할 수 있습니다.

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

사용자 지정 된 슬라이더는 다음과 같습니다.

 [![](slider-switch-segmented-controls-images/image27a.png "Custom Slider")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> 현재 `ThumbTint`가 런타임에 예상 대로 렌더링 되지 않도록 하는 [버그가](https://stackoverflow.com/a/19496179) 있습니다. 위의 **코드 줄에 다음 코드 줄** 을 추가 하 여 해결 방법을 사용할 수 있습니다. [[원본](https://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> 모든 이미지는 재정의 되므로 사용할 수 있지만, 리소스 디렉터리 _에_ 배치 되 고 코드에서 호출 되는지 확인 합니다.

<a name="Switch" />

## <a name="switch"></a>전환

iOS는 다른 플랫폼에서 라디오 단추로 표현할 수 있는 부울 입력으로 `UISwitch`를 사용 합니다. 사용자는 **설정/해제** 위치 사이의 *엄지 단추* 를 이동 하 여 컨트롤을 조작할 수 있습니다.

 [![](slider-switch-segmented-controls-images/image28a.png "Switch")](slider-switch-segmented-controls-images/image28a.png#lightbox)

스위치의 모양은 디자이너의 **Properties Pad** 에서 사용자 지정할 수 있으며,이를 통해 기본 상태, **설정/해제 색조** 색 및 **설정/해제 이미지**를 제어할 수 있습니다. 이는 아래 이미지에 나와 있습니다.

 [![](slider-switch-segmented-controls-images/image29a.png "Switch Properties")](slider-switch-segmented-controls-images/image29a.png#lightbox)

코드에서 스위치의 속성을 설정할 수도 있습니다. 예를 들어 아래 코드는 `On`의 기본값을 사용 하는 스위치를 표시 합니다.

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />

## <a name="segmented-controls"></a>분할된 컨트롤

분할 된 컨트롤은 사용자가 적은 수의 옵션을 조작할 수 있도록 구성 된 방법입니다. 가로로 배치 되며 각 세그먼트는 별도의 단추로 작동 합니다. 디자이너를 사용할 때 분할 된 컨트롤은 **도구 상자 > 컨트롤**에서 찾을 수 있으며 다음 이미지와 같이 표시 됩니다.

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "Segmented Control")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

디자이너의 고유한 기능을 사용 하면 아래 그림과 같이 디자인 화면에서 각 세그먼트를 개별적으로 선택할 수 있습니다.

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Segmented Control")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

이렇게 하면 Properties Pad를 사용 하 여 각 세그먼트의 속성을 보다 정확 하 게 제어할 수 있습니다. 아래 스크린샷에서 편집 가능한 속성을 볼 수 있습니다.

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Segmented Control")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

분할 된 컨트롤 스타일은 iOS7에서 더 이상 사용 되지 않으므로 iOS7 응용 프로그램에서이에 대 한 옵션을 조정 해도 아무런 영향을 주지 않습니다.

## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
- [경고 컨트롤러](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
