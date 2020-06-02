---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 21. Transforms''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 32393108f84ea3a57079c86b6a9a8e628ceca03a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136671"
---
# <a name="summary-of-chapter-21-transforms"></a>요약 - 21장. 변환

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)

Xamarin.Forms 보기는 일반적으로 `Layout` 또는 `Layout<View>` 파생 개체인 해당 상위 항목에 따라 결정된 위치 및 크기로 화면에 표시됩니다. *변환*은 이러한 위치, 크기 또는 방향을 수정할 수 있는 Xamarin.Forms 기능입니다.

Xamarin.Forms는 다음 세 가지 기본 변환 형식을 지원합니다.

- *좌표 이동* &mdash; 요소를 가로 또는 세로로 이동
- *크기 조정* &mdash; 요소 크기 변경
- *회전* &mdash; 점 또는 축을 기준으로 요소 회전

Xamarin.Forms에서 크기 조정은 등방성으로 수행되며 너비와 높이에 균일하게 영향을 줍니다. 회전은 화면의 2차원 표면 및 3D 공간에서 모두 지원됩니다. 기울기(또는 전단) 변환 및 일반화된 행렬 변환은 없습니다.

변환은 `VisualElement` 클래스로 정의되는 `double` 형식의 8개 속성으로 지원됩니다.

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)
- [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)
- [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)
- [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)
- [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)
- [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

이러한 모든 속성은 바인딩 가능한 속성으로 지원됩니다. 이러한 속성은 데이터 바인딩 및 스타일 지정의 대상일 수 있습니다. [**22장. 애니메이션**](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md)에서는 이러한 속성에 애니메이션 효과를 적용하는 방법을 보여주지만, 이 장의 일부 샘플은 Xamarin.Forms [timer](~/xamarin-forms/platform/device.md#devicestarttimer)를 사용하여 애니메이션 효과를 적용하는 방법을 보여 줍니다.

변환 속성은 요소의 렌더링 방법에만 영향을 주며, 레이아웃에서 요소가 인식되는 방법에는 영향을 주지 *않습니다*.

## <a name="the-translation-transform"></a>좌표 이동 변환

[`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) 및 [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) 속성의 값이 0이 아니면 요소가 가로 또는 세로로 이동합니다.

[**TranslationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) 프로그램을 사용하면 `Frame`의 `TranslationX` 및 `TranslationY` 속성을 제어하는 2개의 `Slider` 요소로 이러한 속성을 실험할 수 있습니다. 변환은 또한 해당 `Frame`의 모든 자식 요소에 영향을 줍니다.

### <a name="text-effects"></a>텍스트 효과

좌표 이동 속성의 한 가지 일반적인 용도는 텍스트 렌더링을 약간 오프셋하는 것입니다. 이에 대해서는 [**TextOffsets**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) 샘플을 참조하십시오.

[![Text 오프셋의 세 가지 스크린샷](images/ch21fg03-small.png "텍스트 오프셋")](images/ch21fg03-large.png#lightbox "텍스트 오프셋")

또 다른 효과는 [**BlockText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) 샘플에 표시된 것처럼 3D 블록과 비슷하게 `Label`의 여러 복사본을 렌더링하는 것입니다.

### <a name="jumps-and-animations"></a>점프 및 애니메이션

[**ButtonJump**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) 샘플은 좌표 이동을 이용해서 눌릴 때마다 `Button`을 이동하지만, 이 샘플의 기본 의도는 단추가 렌더링되는 위치에서 `Button`이 사용자 입력을 받는 것을 보여주기 위한 것입니다.

[**ButtonGlide**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) 샘플도 비슷하지만 타이머를 사용하여 한 포인트에서 다른 포인트로 `Button`을 애니메이션합니다.

## <a name="the-scale-transform"></a>크기 조정 변환

[`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 변환은 요소의 렌더링 크기를 늘리거나 줄일 수 있습니다. 기본값은 1입니다. 값이 0이면 요소가 표시되지 않습니다. 값이 음수면 요소가 180도 회전되어 표시됩니다. `Scale` 속성은 요소의 `Width` 또는 `Height` 속성에 영향을 주지 않습니다. 이러한 값은 동일하게 유지됩니다.

[**SimpleScaleDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) 샘플을 사용하여 `Scale` 속성을 실험할 수 있습니다

[**ButtonScaler**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) 샘플은 `Button`의 `Scale` 속성 애니메이션과 `FontSize` 속성 애니메이션 사이의 차이점을 보여줍니다. `FontSize` 속성은 레이아웃에서 `Button`이 인식되는 방법에 영향을 주고, `Scale` 속성은 영향을 주지 않습니다.

[**ScaleToSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) 샘플은 페이지를 벗어나지 않으면서 가능한 한 크게 표시될 수 있도록 `Label` 요소에 적용되는 `Scale` 속성을 계산합니다.

### <a name="anchoring-the-scale"></a>크기 조정 고정

이전 3개 샘플에서 크기가 조정된 요소는 모두 요소 중심을 기준으로 크기를 늘리거나 줄였습니다. 즉, 모든 방향으로 요소 크기가 동일하게 늘거나 줄었습니다. 크기를 조정할 때 요소 중심의 포인트 위치만 동일하게 유지됩니다.

[`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) 및 [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) 속성을 설정하면 크기 조정 시 중심을 변경할 수 있습니다. 이러한 속성은 요소 자체를 기준으로 합니다. `AnchorX`의 경우 값이 0이면 요소 왼쪽 측면을 나타내고, 1은 오른쪽 측면을 나타냅니다. `AnchorY`와 비슷하게 0은 위쪽이고 1은 아래쪽입니다. 두 속성 모두 기본값은 중심을 나타내는 0.5입니다.

[**AnchoredScaleDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) 샘플을 사용하면 `Scale` 속성 뿐만 아니라 `AnchorX` 및 `AnchorY` 속성을 실험해볼 수 있습니다.

iOS에서 기본값이 아닌 `AnchorX` 및 `AnchorY` 속성을 사용할 경우에는 일반적으로 휴대폰 방향 변경과 호환되지 않습니다.

## <a name="the-rotation-transform"></a>회전 변환

[`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성은 각도로 지정되며 `AnchorX` 및 `AnchorY`로 정의된 요소 포인트를 중심으로 한 시계방향 회전을 나타냅니다. [**PlaneRotationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo)를 사용하면 세 가지 속성을 실험할 수 있습니다.

### <a name="rotated-text-effects"></a>회전된 텍스트 효과

[**BoxViewCircle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) 샘플은 64개의 작은 회전된 `BoxView` 요소를 사용해서 원을 그리는 데 필요한 수학 계산을 보여줍니다.

[**RotatedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) 샘플은 바퀴살처럼 보이는 회전된 동일 텍스트 문자열이 포함된 여러 개의 `Label` 요소를 표시합니다.

[**CircularText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) 샘플은 원에 래핑된 것으로 보이는 텍스트 문자열을 표시합니다.

### <a name="an-analog-clock"></a>아날로그 시계

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 시계 바늘의 각도를 계산하는 [`AnalogClockViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) 클래스가 있습니다. ViewModel에 대한 플랫폼 종속성을 방지하기 위해 이 클래스는 타이머 대신 `Task.Delay`를 사용하여 새 `DateTime` 값을 찾습니다.

또한 **Xamarin.FormsBook.Toolkit**에는 `IValueConverter`를 구현하고 초침의 각도를 가장 가까운 초로 반올림하는 [`SecondTickConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) 클래스가 포함되어 있습니다.

[**MinimalBoxViewClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock)은 회전하는 3개의 `BoxView` 요소를 사용해서 아날로그 시계를 그립니다.

[**BoxViewClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock)은 시계 앞면 둘레의 자리 표시와 해당 끝점에서 약간씩의 거리를 회전하는 바늘을 포함하여 보다 포괄적인 그래픽을 위해 `BoxView`를 사용합니다.

[![BoxView Clock의 삼중 스크린샷](images/ch21fg17-small.png "아날로그 시계 앞면")](images/ch21fg17-large.png#lightbox "아날로그 시계 앞면")

또한 **Xamarin.FormsBook.Toolkit**의 [`SecondBackEaseConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) 클래스는 초침이 약간 뒤로 갔다가 앞으로 점프한 후 다시 정확한 위치로 돌아가는 모양이 되도록 만듭니다.

### <a name="vertical-sliders"></a>세로 슬라이더?

[**VerticalSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) 샘플은 `Slider` 요소가 90도 회전된 상태로도 작동할 수 있음을 보여줍니다. 하지만 레이아웃에서 이러한 요소가 계속 가로로 표시되기 때문에 이렇게 회전된 `Slider` 요소의 위치를 정하는 것이 어렵습니다.

## <a name="3d-ish-rotations"></a>3D와 비슷한 회전

[`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) 속성은 3D X축 주위로 요소를 회전하는 것으로 보이기 때문에 이 요소의 위쪽 및 아래쪽이 앞으로 이동하거나 뷰어에게서 멀어지는 것으로 보입니다. 이외 비슷하게 [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)는 Y축 주위로 요소를 회전하여 요소의 왼쪽 및 오른쪽 측면이 앞으로 이동하거나 뷰어에게서 멀어지는 것으로 보이게 만듭니다.

`AnchorX` 속성은 `RotationX`가 아닌 `RotationY`에 영향을 줍니다. `AnchorY` 속성은 `RotationY`가 아닌 `RotationX`에 영향을 줍니다. [**ThreeDeeRotationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) 샘플로 실험하여 이러한 속성의 상호 작용을 살펴볼 수 있습니다.

Xamarin.Forms에서 사용되는 3D 좌표계는 왼쪽 기준입니다. 왼쪽 손의 집게 손가락을 X 좌표를 늘리는 방향(오른쪽)으로 가리키고 가운데 손가락을 Y 좌표를 늘리는 방향(아래쪽)으로 가리키면, 엄지는 Z 좌표를 늘리는 방향(화면을 벗어나는 방향)을 가리킵니다.

또한 3개 축에 대해 왼쪽 손 엄지 손가락을 값을 늘리는 방향으로 가리키면 나머지 손가락들의 굽은 방향이 양의 회전 각도에 대한 회전 방향을 나타냅니다.

## <a name="related-links"></a>관련 링크

- [21장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [21장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
