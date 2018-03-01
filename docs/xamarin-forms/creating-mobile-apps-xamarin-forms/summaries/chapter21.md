---
title: "요약 장 21입니다. 변형"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 378ce3fb39cfb5c42d5ec7611415f5420146a9cc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-21-transforms"></a>요약 장 21입니다. 변형

위치 및 크기는 일반적으로 해당 부모에 의해 결정 화면에 Xamarin.Forms 보기 표시는 `Layout` 또는 `Layout<View>` 파생 클래스입니다. *변환* 해당 위치, 크기 또는 심지어 방향을 수정할 수 있는 Xamarin.Forms 기능입니다.

Xamarin.Forms는 세 가지 기본 유형은 변환를 지원합니다.

- *번역* & #x 2014; 가로 또는 세로로 요소 이동
- *배율* & #x 2014; 요소 크기 변경
- *회전* & #x 2014; 또는 축 데이터 요소에는 요소를 설정 합니다.

Xamarin.Forms에는 확장은 등방성; 적용 너비와 높이 균일 하 게 됩니다. 회전은 모두에서 화면 및 2 차원 표면 3D 공간에서 사용할 수 있습니다. 변환이 없는 시간차 (또는 순수) 및 일반화 매트릭스 변환 되지 있습니다.

변환은 8 가지 속성 형식의 지원 `double` 정의한는 `VisualElement` 클래스:

- [`TranslationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)
- [`TranslationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)
- [`Scale`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)
- [`Rotation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)
- [`RotationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/)
- [`RotationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/)
- [`AnchorX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/)
- [`AnchorY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/)

이러한 모든 속성은 바인딩 가능한 속성에 의해 지원 됩니다. 데이터 바인딩 대상 수 있으며 스타일이 지정 합니다. [**장 22입니다. 애니메이션** ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) 어떻게 이러한 속성이 애니메이션을 적용할 수를 보여 줍니다. 하지만이 장의 일부 샘플 표시 방법을 애니메이션을 적용할 수는 Xamarin.Forms를 사용 하 여 [타이머](~/xamarin-forms/platform/device.md#Device_StartTimer)합니다.

요소를 렌더링 및 수행 방법에 속성에 영향을 변형 *하지* 레이아웃에서 요소는 인식 하는 방법에 영향을 줍니다.

## <a name="the-translation-transform"></a>번역 변환

0이 아닌 값의는 [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) 및 [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) 가로 또는 세로로 속성 요소를 이동 합니다.

[ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) 프로그램을 통해 두 개의로 이러한 속성을 확인할 수 있습니다 `Slider` 제어 하는 요소는 `TranslationX` 및 `TranslationY` 는 의속성`Frame`. 변환 하는의 모든 자식 항목에도 영향을 줍니다 `Frame`합니다.

### <a name="text-effects"></a>텍스트 효과

변환 속성의 한 일반적인 용도를 오프셋할 약간 텍스트의 렌더링입니다. 이 확인할는 [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) 샘플:

[![텍스트 만큼 오프셋의 삼중 스크린 샷](images/ch21fg03-small.png "텍스트 오프셋")](images/ch21fg03-large.png "텍스트 오프셋")

여러 복사본을 렌더링 하는 다른 효과 `Label` 에서 같은 3D 블록 유사 하 게는 [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) 샘플.

### <a name="jumps-and-animations"></a>점프 및 애니메이션

[ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) 샘플 번역을 사용 하 여 이동 하는 `Button` 는 탭 있지만 주로 하 때마다는 `Button` 위치에서 사용자 입력을 받는 위치 단추 렌더링 됩니다.

[ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) 샘플 유사 하지만 타이머를 사용 하 여 애니메이션 효과를 줄의 `Button` 한 지점에서 다른 합니다.

## <a name="the-scale-transform"></a>크기 조정 변환

[ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 변환 늘리거나 요소의 렌더링 된 크기를 줄일 수 있습니다. 기본값은 1입니다. 값이 0 사용 하면 요소가 보이지 않아야 합니다. 음수 값 인해 요소를 180도 회전 된 것 같습니다. `Scale` 속성에 영향을 주지 않습니다는 `Width` 또는 `Height` 요소의 속성입니다. 이러한 값이 그대로 유지 합니다.

실험할 수는 `Scale` 사용 하 여 속성의 [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) 샘플.

[ **ButtonScaler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) 샘플 애니메이션 차이점을 보여 줍니다.는 `Scale` 속성은 `Button` 애니메이션을 적용 하 고는 `FontSize` 속성입니다. `FontSize` 속성에 영향을 줍니다 방법을 `Button` 레이아웃;에서 인식 된 `Scale` 속성은 그렇지 않습니다.

[ **ScaleToSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) 샘플 계산는 `Scale` 속성에 적용 되는 `Label` 동안 페이지 내에서 여전히 맞춤을 가능한 한 크게 만들 요소입니다.

### <a name="anchoring-the-scale"></a>눈금을 고정

이전 세 개의 샘플에서 확장 요소는 모든 증가 또는 감소의 크기는 요소 중심을 기준으로 합니다. 즉, 요소 늘리거나 크기에 모든 방향에서 동일 하 게 줄입니다. 크기 조정을 하는 동안 동일한 위치에 요소 중심에 지점만 유지 됩니다.

설정 하 여 크기 조정의 중심을 변경할 수는 [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) 및 [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) 속성입니다. 이러한 속성은 요소 자체를 기준으로 합니다. 에 대 한 `AnchorX`, 값이 0 요소의 왼쪽으로 참조 하 고 왼쪽에서 오른쪽으로 1 참조 합니다. 마찬가지로 `AnchorY`, 0은 맨 위에 고 1은 맨 아래입니다. 두 속성 모두의 센터인 0.5, 기본 값을 갖습니다.

[ **AnchoredScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) 샘플에서는 시험해 볼 수는 `AnchorX` 및 `AnchorY` 속성으로 `Scale` 속성입니다.

Ios에서의 기본값이 아닌 값을 사용 하 여 `AnchorX` 및 `AnchorY` 속성 전화 방향 변경을 일반적으로 호환 되지 않습니다.

## <a name="the-rotation-transform"></a>회전 변환

[ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) 속성도 단위로 지정 하며 범위에 정의 된 요소 점을 시계 방향 회전을 나타내는 `AnchorX` 및 `AnchorY`합니다. [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) 이 세 가지 속성을 시험해 볼 수 있습니다.

### <a name="rotated-text-effects"></a>회전 된 텍스트 효과

[ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) 샘플 아주 작은 회전 64를 사용 하 여 원을 그리려면 하는 데 필요한 수학 `BoxView` 요소입니다.

[ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) 샘플에는 여러 개 표시 되며 `Label` 스포크 없는 동일한 텍스트 문자열로 요소 회전 합니다.

[ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) 샘플 원에서 표시 되는 텍스트 문자열을 표시 합니다.

### <a name="an-analog-clock"></a>아날로그 시계

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에 포함 되어 있는 [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) 시계 바늘에 대 한 각도 계산 하는 클래스입니다. ViewModel 클래스 사용에에서 대 한 플랫폼 종속성을 방지 하기 위해 `Task.Delay` 새 찾기에 대 한 타이머 대신 `DateTime` 값입니다.

에 포함 **Xamarin.FormsBook.Toolkit** 는 [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) 구현 하는 클래스 `IValueConverter` 에 가장 가까운 초에 두 번째 각도 반올림을 제공 합니다.

[ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) 세 회전을 사용 하 여 `BoxView` 아날로그 시계의 그릴 요소입니다.

[ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) 사용 하 여 `BoxView` 클록의 글꼴 관련 표시 되며 거의 거리를 회전 하의 끝에서 전달 눈금을 포함 하 여 하 더 광범위 한 그래픽:

[![BoxView 클록의 삼중 스크린 샷](images/ch21fg17-small.png "아날로그 시계")](images/ch21fg17-large.png "아날로그 시계 모양")

또한는 [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) 클래스 **Xamarin.FormsBook.Toolkit** 약간, 미리 시작 하기 전에 달릴 나타내려면 두 번째 손 모양 아이콘이 사용 하면 다음 올바른 위치로 다시 이동 하 합니다.

### <a name="vertical-sliders"></a>세로 슬라이더?

[ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) 샘플을 보여 줍니다 `Slider` 요소 90도 회전 가능와 작동 합니다. 그러나 어렵습니다 회전 이러한 위치를 지정할 `Slider` 가로 것으로 표시 하므로 계속 레이아웃에 요소입니다.

## <a name="3d-ish-rotations"></a>3D 문제 회전

[ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) 속성이 위쪽과 아래쪽 요소를 향해 또는 뷰어에서 이동 것 같습니다를 3D x 축 중심 요소를 회전 하 여 표시 합니다. 마찬가지로,는 [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) 요소는 왼쪽과 오른쪽 요소를 향해 또는 뷰어에서 이동 것 같습니다. 있도록 y 축 중심으로 회전 하는 것 같습니다.

`AnchorX` 속성에 영향을 미칩니다 `RotationY` 아닌 `RotationX`합니다. `AnchorY` 속성에 영향을 미칩니다 `RotationX` 아닌 `RotationY`합니다. 실험할 수는 [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) 이러한 속성의 상호 작용을 탐색 하는 샘플입니다.

Xamarin.Forms 사용 권한에 포함 된 3 차원 좌표계가 왼쪽 합니다. 가리키면 증가 X 방향으로 왼쪽 손 집게 좌표 (오른쪽)를 하 고 중간 손가락 증가 Y 방향에서 (아래쪽), 다음 프로그램 thumb 포인트의 Z 좌표 (화면) 중 오름차순 방향으로 조정 합니다.

또한 모든 세 개의 축에 대해 값이 늘어나는 방향을에서 프로그램 왼쪽 엄지 단추를 가리킬 경우 다음 손가락으로 곡선의 방향을 나타냅니다 회전 양수 회전 각도입니다.



## <a name="related-links"></a>관련 링크

- [장 21 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [21 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
