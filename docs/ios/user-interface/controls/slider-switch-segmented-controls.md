---
title: 슬라이더, 스위치 및 Xamarin.iOS에서 분할 된 컨트롤
description: 이 문서에서는 슬라이드, 스위치 및 Xamarin.iOS 프로그래밍 방식으로 iOS 디자이너에서에서 작업 하는 방법을 설명에서 분할 된 컨트롤을 설명 합니다.
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 7b66005ff64763d68dc6101985e514fa56cf896d
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827369"
---
# <a name="sliders-switches-and-segmented-controls-in-xamarinios"></a>슬라이더, 스위치 및 Xamarin.iOS에서 분할 된 컨트롤

<a name="Sliders" />

## <a name="sliders"></a>슬라이더

슬라이더 컨트롤 범위 내에서 숫자 값의 간단한 선택할 수 있습니다. 컨트롤 0과 1 사이의 값을 기본적으로 사용 되지만 이러한 한도 사용자 지정할 수 있습니다.

 [![](slider-switch-segmented-controls-images/image25a.png "슬라이더")](slider-switch-segmented-controls-images/image25a.png#lightbox)

다음 스크린 샷에서 디자이너에서 편집할 수 있는 속성을 보여 줍니다.

 [![](slider-switch-segmented-controls-images/image26a.png "슬라이더 속성")](slider-switch-segmented-controls-images/image25a.png#lightbox)

아래와 같이 연결에서 현재 선택 된 값을 표시 하는 처리기를 포함 하 여 코드에서 이러한 값을 설정할 수는 `UILabel` 제어 합니다.

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

설정으로 슬라이더의 시각적 모양을 사용자 지정할 수도 있습니다.

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

사용자 지정된 슬라이더는 다음과 같습니다.

 [![](slider-switch-segmented-controls-images/image27a.png "사용자 지정 슬라이더")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> 현재는 [버그](https://stackoverflow.com/a/19496179) 일으키는 `ThumbTint` 예상 대로 런타임에 렌더링 되지 합니다. 다음 코드 줄을 추가할 수 있습니다 **하기 전에** 위의 코드는이 문제를 해결 합니다. [[Source](https://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> 재정의할 수 있지만 배치 되 고 있는지 확인 하는 대로 모든 이미지를 사용할 수 있습니다 _에서_ 리소스 디렉터리 코드에서 호출 됩니다.

<a name="Switch" />

## <a name="switch"></a>전환

iOS를 사용 하는 `UISwitch` 부울 값을 입력 하는 다른 플랫폼에서 라디오 단추에 표시 될 수 있습니다. 이동 하 여 컨트롤을 조작할 수는 *thumb* 간에 **켜고** 위치 합니다.

 [![](slider-switch-segmented-controls-images/image28a.png "Switch")](slider-switch-segmented-controls-images/image28a.png#lightbox)

스위치의 모양을 사용자 지정할 수 있습니다 합니다 **Properties Pad** 하면 기본 상태를 제어, 디자이너 **켜고 tint** 색 및 **on/off 이미지**. 이 아래 이미지에 나와 있습니다.

 [![](slider-switch-segmented-controls-images/image29a.png "스위치 속성")](slider-switch-segmented-controls-images/image29a.png#lightbox)

코드에서 스위치의 속성을 설정할 수도 있습니다, 예를 들어 아래 코드의 기본값을 사용 하 여 스위치를 알아보겠습니다 `On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>분할된 컨트롤

분할 된 컨트롤은 체계적인된 방식으로 사용자가 적은 수의 옵션을 사용 하 여 상호 작용할 수 있도록 합니다. 가로로 배치 하 고 각 세그먼트 별도 단추로 작동 합니다. 분할 된 컨트롤 디자이너를 사용 하는 경우 아래에서 확인할 수 있습니다 **도구 상자 > 컨트롤**, 다음 이미지와 같이 표시 됩니다.

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "분할 된 제어")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

디자이너의 고유한 기능 아래 그림과 같이 각 세그먼트는 디자인 화면에서 개별적으로 선택할 수 있습니다.

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "분할 된 제어")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

따라서 Properties Pad 보다 정확 하 게 각 세그먼트의 속성을 제어 하는 데 사용할 수 있습니다. 아래 스크린샷에 편집 가능한 속성을 확인할 수 있습니다.

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "분할 된 제어")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

iOS7, 분할 된 컨트롤 스타일에 되지 않으며 따라서 iOS7 응용 프로그램에서이 대 한 옵션 조정 해도 아무런 영향이 유의 해야 합니다.

## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/monotouch/Controls/)
- [경고 컨트롤러](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
