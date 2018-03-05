---
title: "슬라이더, 스위치 및 세그먼트 컨트롤"
ms.topic: article
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 1c24b1faf7b108466d6e93ffae8112d0dea6d844
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="sliders-switches-and-segmented-controls"></a>슬라이더, 스위치 및 세그먼트 컨트롤

<a name="Sliders" />


## <a name="sliders"></a>슬라이더

범위 내에서 숫자 값의 간단한 선택에 대 한 슬라이더 컨트롤 수 있습니다. 컨트롤 0과 1 사이의 값을 기본적으로 사용 되지만 이러한 한도 사용자 지정할 수 있습니다.

 [ ![](slider-switch-segmented-controls-images/image25a.png "슬라이더")](slider-switch-segmented-controls-images/image25a.png)

다음 스크린 샷에서 디자이너에서 편집할 수 있는 속성을 보여 줍니다.

 [ ![](slider-switch-segmented-controls-images/image26a.png "슬라이더 속성")](slider-switch-segmented-controls-images/image25a.png)

아래와 같이 연결에서 현재 선택 된 값을 표시 하는 처리기를 포함 하 여 코드에서 이러한 값을 설정할 수는 `UILabel` 제어:

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

설정에 따라 슬라이더의 시각적 모양을 사용자 지정할 수도 있습니다.

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

사용자 지정 된 슬라이더는 다음과 같습니다.

 [ ![](slider-switch-segmented-controls-images/image27a.png "사용자 지정 슬라이더")](slider-switch-segmented-controls-images/image28a.png)

> [!IMPORTANT]
> ˇ ľ ř ˝는 [버그](http://stackoverflow.com/a/19496179) 일으키는 `ThumbTint` 예상 대로 실행 시 렌더링 되지 합니다. 다음 코드 줄을 추가할 수 있습니다 **전에** 위의 코드 문제를 해결 합니다. [[Source](http://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> 재정의할 수 있지만 배치 되 고 있는지 확인 모든 이미지를 사용할 수 있습니다 _에_ Resources 디렉터리 코드에서 호출 됩니다.

<a name="Switch" />

## <a name="switch"></a>전환

iOS를 사용 하 여는 `UISwitch` 부울 입력 다른 플랫폼에서 라디오 단추에 표시 될 수 있습니다. 사용자를 이동 하 여 컨트롤을 조작할 수는 *thumb* 간에 **켜기/끄기** 위치입니다.

 [ ![](slider-switch-segmented-controls-images/image28a.png "스위치")](slider-switch-segmented-controls-images/image28a.png)

스위치의 모양을 사용자 지정할 수 있습니다는 **속성 패드** 디자이너는 기본 상태를 제어할 수 있도록, **켜기/끄기 tint** 색과   **/사용 안 함 이미지**. 아래 그림에서는 이러한 보여 줍니다.

 [ ![](slider-switch-segmented-controls-images/image29a.png "스위치 속성")](slider-switch-segmented-controls-images/image29a.png)

코드에서 스위치의 속성을 설정, 아래 코드는 스위치의 기본값을 표시 하는 예를 들어 `On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>분할 된 컨트롤

분할 된 컨트롤은 사용자가 적은 수의 옵션 상호 작용할 수 있도록 하는 구성 된 방법. 가로 방향으로 배치 된 및 각 세그먼트는 별도 단추로 작동 합니다. 조각화 된 컨트롤을 찾을 수는 디자이너를 사용할 때는 **도구 상자 > 컨트롤**, 다음 이미지와 같이 표시 해야 합니다.

 [ ![](slider-switch-segmented-controls-images/segmentedcontrol.png "분할 된 컨트롤")](slider-switch-segmented-controls-images/segmentedcontrol.png)

디자이너의 고유한 기능 아래 그림과 같이 각 세그먼트 디자인 화면에서 개별적으로 선택 하려면 허용 됩니다.

 [ ![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "분할 된 컨트롤")](slider-switch-segmented-controls-images/segmentedcontrolselection.png)

따라서 속성 패드 보다 정확 하 게 각 세그먼트의 속성을 제어 하는 데 사용할 수 있습니다. 아래 스크린샷에서 편집 가능한 속성을 확인할 수 있습니다.

 [ ![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "분할 된 컨트롤")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png)

용도로 iOS7에서 조각화 된 컨트롤 스타일은 사용 되지 하 고 따라서 iOS7 응용 프로그램에서이 대 한 옵션을 조정 아무런 효과가 없습니다.

## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/Controls/)
- [경고 컨트롤러](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
