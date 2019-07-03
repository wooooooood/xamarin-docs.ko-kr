---
title: 요약 21 장입니다. 변환
description: Xamarin.Forms를 사용 하 여 모바일 앱을 만듭니다. 요약 21 장입니다. 변환
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 313348952b87d94db63d1682f8e1b9413d56714d
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67513023"
---
# <a name="summary-of-chapter-21-transforms"></a>요약 21 장입니다. 변형

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)

Xamarin.Forms 뷰 화면 위치 및 크기는 일반적으로 부모 기준에서 표시 되는 `Layout` 또는 `Layout<View>` 파생 합니다. 합니다 *변환* 해당 위치, 크기 또는 심지어 방향을 수정할 수 있는 Xamarin.Forms 기능입니다.

Xamarin.Forms는 세 가지 기본 유형의 변환 지원합니다.

- *번역* &mdash; 가로나 세로 방향으로 요소 이동
- *크기 조정* &mdash; 요소의 크기를 변경 합니다.
- *회전* &mdash; 지점 또는 축 주위 요소를 설정 합니다.

Xamarin.forms에 크기 조정은 등방성; 영향을 너비와 높이 균일 하 게 합니다. 회전은 둘 다에서 화면 및 2 차원 표면 3D 공간에서 지원 됩니다. 없는 오차 (또는 순수) 변환 및 일반화 된 매트릭스 변환이 있습니다.

변환 형식 8 가지 속성을 사용 하 여 지원 됩니다 `double` 정의한는 `VisualElement` 클래스:

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)
- [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)
- [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)
- [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)
- [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)
- [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

이러한 모든 속성은 바인딩 가능한 속성으로 지원 됩니다. 데이터 바인딩의 대상 수 있으며 스타일입니다. [**22 장입니다. 애니메이션** ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) 어떻게 이러한 속성 애니메이션을 적용할 수를 보여 줍니다.이 챕터에 몇 가지 샘플 Xamarin.Forms를 사용 하 여 애니메이트할 수 있습니다 하는 방법을 보여 줍니다. 그러나 [타이머](~/xamarin-forms/platform/device.md#devicestarttimer)합니다.

요소를 렌더링 하 고 수행 하는 방법에 속성에 영향을 변환 *되지* 레이아웃에서 요소는 인식 하는 방법에 영향을 줍니다.

## <a name="the-translation-transform"></a>번역 변환

0이 아닌 값을 [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) 및 [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) 속성 가로나 세로 방향으로 요소를 이동 합니다.

합니다 [ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) 프로그램을 사용 하면 두 개를 사용 하 여 이러한 속성을 사용 하 여 실험 `Slider` 제어 하는 요소는 `TranslationX` 및 `TranslationY` 속성을 `Frame`. 변환 하는 모든 자식에도 영향을 줍니다 `Frame`합니다.

### <a name="text-effects"></a>텍스트 효과

변환 속성을 일반적으로 사용 오프셋 약간 텍스트 렌더링 하는 것입니다. 에 설명 되어이 [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) 샘플:

[![텍스트를 오프셋 하는 triple 스크린샷](images/ch21fg03-small.png "텍스트 오프셋")](images/ch21fg03-large.png#lightbox "텍스트 오프셋")

다른 효과의 여러 복사본을 렌더링 하는 것을 `Label` 에서 설명한 것 처럼 같은 3D 블록을 유사 하 게 합니다 [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) 샘플입니다.

### <a name="jumps-and-animations"></a>점프 및 애니메이션

[ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) 샘플 변환을 사용 하 여 이동를 `Button` 를 탭 할 때마다 기본 의도 있다는 `Button` 위치에서 사용자 입력을 받는 위치 단추가 렌더링 됩니다.

합니다 [ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) 샘플 유사 하지만 타이머를 사용 하 여 애니메이션을 적용 하는 `Button` 다른 한 지점에서.

## <a name="the-scale-transform"></a>배율 변환

합니다 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 변환 늘이거나 요소의 렌더링 된 크기를 줄일 수 있습니다. 기본값은 1입니다. 값 0 사용 하면 표시 되지 않도록 하려면 요소가 있습니다. 음수 값에 해당 요소가 나타날 180도 회전된 하도록 발생 합니다. 합니다 `Scale` 속성에 영향을 주지 않습니다 합니다 `Width` 또는 `Height` 요소의 속성입니다. 이러한 값이 그대로 유지 합니다.

실험해 볼 수 있습니다 합니다 `Scale` 사용 하 여 속성을 [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) 샘플입니다.

[**ButtonScaler**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) 샘플은 `Scale` 속성의 `Button` 애니메이션 효과와 `FontSize` 속성 애니메이션의 차이점을 보여줍니다. `FontSize` 속성은 `Button`가 레이아웃에서 어떻게 인식되는지에 영향을줍니다. `Scale` 속성은 그렇지 않습니다.

[**ScaleToSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) 샘플은 `Scale` 요소에 적용되는 `Label` 속성을 계산하여 가능한 한 큰 값으로 만듭니다.

### <a name="anchoring-the-scale"></a>소수 자릿수를 고정합니다.

이전 세 가지 샘플 규모 감축 하는 요소가 있는 모든 증가 하거나 감소 요소의 가운데를 기준으로 크기입니다. 즉, 요소 늘리거나 모든 방향에서 동일한 크기의 줄입니다. 크기 조정 하는 동안 동일한 위치에 요소 중심이 지점만 유지 됩니다.

[`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) 및 [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) 속성을 설정하여 크기 조정의 중심을 변경할 수 있습니다. 이러한 속성은 요소 자체 기준으로 하는 것입니다. 에 대 한 `AnchorX`, 요소의 왼쪽에 값이 0 참조 하 고 왼쪽에서 오른쪽으로 1 참조 합니다. 마찬가지로 `AnchorY`, 0은 맨 하 고 1은 아래쪽 합니다. 두 속성 모두를 센터인 0.5의 기본 값을 갖습니다.

[**AnchoredScaleDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) 샘플을 사용하면 `AnchorX` 및 `AnchorY` 속성뿐만 아니라 `Scale` 속성을 실험 할 수 있습니다.

iOS에서 기본값이 아닌 `AnchorX` 및 `AnchorY` 속성을 사용하면 일반적으로 휴대 전화 방향 변경과 호환되지 않습니다.

## <a name="the-rotation-transform"></a>회전 변환

[`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성은도 단위로 지정되며 `AnchorX` 및 `AnchorY`로 정의 된 요소의 점을 중심으로 시계 방향으로 회전을 나타냅니다. 합니다 [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) 이러한 세 가지 속성을 사용 하 여 실험할 수 있습니다.

### <a name="rotated-text-effects"></a>회전 된 텍스트 효과

합니다 [ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) 샘플과 64 작은 회전을 사용 하 여 원을 그리려면 필요한 수학 `BoxView` 요소입니다.

합니다 [ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) 샘플 여러 개 표시 `Label` 스포크와 같은 표시할 요소는 동일한 텍스트 문자열로 회전 합니다.

합니다 [ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) 샘플 원 안에 래핑할 표시 되는 텍스트 문자열을 표시 합니다.

### <a name="an-analog-clock"></a>아날로그 시계

합니다 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) 시계 바늘에 대 한 각도 계산 하는 클래스입니다. ViewModel 클래스 사용의에서 플랫폼 종속성을 방지 하려면 `Task.Delay` 찾는 새 타이머 대신 `DateTime` 값입니다.

에 포함 **Xamarin.FormsBook.Toolkit** 되는 [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) 클래스를 구현 하는 `IValueConverter` 가장 가까운 두 번째는 두 번째 각도 반올림 하는 데 사용 되 고 합니다.

합니다 [ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) 3 회전을 사용 하 여 `BoxView` 아날로그 시계의 그릴 요소입니다.

합니다 [ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) 사용 하 여 `BoxView` 더 광범위 한 그래픽에 대 한 눈금 클록의 글꼴 관련 표시 등 회전는 거의 거리의 끝에서 전달 합니다.

[![삼중 스크린샷 BoxView 클록](images/ch21fg17-small.png "아날로그 시계 앞면")](images/ch21fg17-large.png#lightbox "아날로그 시계 앞면")

또한를 [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) 클래스 **Xamarin.FormsBook.Toolkit** 를 진행 하기 전에 잠시 달릴 표시할 초침 하면 올바른 위치로 다시 이동 합니다.

### <a name="vertical-sliders"></a>세로 슬라이더?

합니다 [ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) 샘플을 보여 줍니다 `Slider` 요소 90도 회전 된 수와 작동 합니다. 그러나 어렵습니다 위치로 회전 이러한 `Slider` 가로 것으로 표시 해야 하며 레이아웃에 있으므로 요소입니다.

## <a name="3d-ish-rotations"></a>문제 3D 회전

합니다 [ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX) 속성 요소의 아래쪽과 위쪽 방향으로 또는 뷰어에서 떨어져 이동 보일를 3D 축 주위 요소를 회전 하 여 표시 됩니다. 마찬가지로, 합니다 [ `RotationY` ](xref:Xamarin.Forms.VisualElement.RotationY) 왼쪽과 오른쪽 요소를 향해 또는 뷰어에서 떨어져 이동할 것 처럼 보일 수 있도록 y 축을 요소를 회전 하는 것 같습니다.

합니다 `AnchorX` 속성에 영향을 줍니다 `RotationY` 있지만 `RotationX`합니다. 합니다 `AnchorY` 속성에 영향을 줍니다 `RotationX` 있지만 `RotationY`합니다. 실험해 볼 수 있습니다 합니다 [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) 이러한 속성의 상호 작용을 탐색 하는 샘플입니다.

Xamarin.Forms 사용 권한에 포함 된 3D 좌표 시스템의 왼쪽입니다. 가리키는 경우 증가 x 방향의 왼쪽 손 엄지와 (오른쪽)를 조정 하 고 손가락은 증가 y 방향에서 (아래쪽), 다음에 thumb 포인트 Z 좌표 (화면)에서 증가 하는 방향으로 조정 합니다.

또한 세 개의 축에 대해 왼쪽 엄지 손가락 값 증가 방향을 가리키면 다음 손가락의 곡선의 방향을 나타냅니다 양의 회전 각도 회전 합니다.



## <a name="related-links"></a>관련 링크

- [21 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [21 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
