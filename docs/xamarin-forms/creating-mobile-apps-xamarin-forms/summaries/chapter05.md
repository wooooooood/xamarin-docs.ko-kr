---
title: 요약 5 장입니다. 크기를 사용 하 여 처리
description: 'Xamarin.ios를 사용 하 여 Mobile Apps 만들기: 요약 5 장입니다. 크기를 사용 하 여 처리'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: c082bdb10732e42b37511cf050e50f46990a5b5b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771148"
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>요약 5 장입니다. 크기를 사용 하 여 처리

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)

> [!NOTE]
> 이 페이지에 대 한 참고 사항 Xamarin.Forms 책의 내용을에서 달라졌는지를 위치 하는 영역을 나타냅니다.

Xamarin.Forms의 몇 가지 크기로 지금 발생 했습니다.

- IOS 상태 표시줄의 높이 20
- `BoxView` 기본 너비 및 높이가 40
- 기본값 `Padding` 에 `Frame` 은 20
- 기본값 `Spacing` 에 `StackLayout` 6
- `Device.GetNamedSize` 메서드는 숫자 글꼴 크기를 반환 합니다.

이러한 크기 (픽셀) 않습니다. 대신 장치 독립적 단위 각 플랫폼에서 독립적으로 인식 됩니다.

## <a name="pixels-points-dps-dips-and-dius"></a>픽셀, 지점, dps, Dip 및 DIUs

Apple Mac 및 Microsoft Windows의 기록, 초기에 프로그래머는 픽셀 단위로 작동합니다. 그러나 출현 고해상도 디스플레이의 화면 좌표를 가상화 하 고 추상 방법이 필요합니다. Mac 환경에서 프로그래머의 단위로 작동 *포인트*, 일반적으로 1/72 인치, Windows 개발자가 사용 하는 동안 *장치 독립적 단위* 1/96 인치를 기반으로 (DIUs).

그러나 모바일 장치에는 일반적으로 얼굴에 더 가깝습니다 유지 하 고 데스크톱 보다 더 높은 해상도 화면 보다 높은 픽셀 밀도 허용할 수 있는지를 의미 합니다.

Apple iPhone 및 iPad 장치를 대상으로 하는 프로그래머의 단위에서 작업을 계속할 *지점*, 1/96 인치 사항은 160 되지만 합니다. 장치에 따라 1, 2 또는 3 점 픽셀 수입니다.

Android는 유사 합니다. 프로그래머의 단위로 작동 *밀도 독립적 픽셀* (dps) dps 픽셀 사이의 관계는 1/96 인치 160 dps 기반으로 합니다.

Windows 휴대폰 및 모바일 장치 160 장치 독립적인 1/96 인치 단위 가까운 내재 된 배율 설정도 있습니다.

> [!NOTE]
> Xamarin.Forms는 모든 Windows 기반 전화 또는 모바일 장치에 더 이상 지원합니다.

요약 하자면, 휴대폰 및 태블릿을 대상으로 하는 Xamarin.Forms 프로그래머 모든 측정 단위는 다음 조건을 기반으로 한다는 가정할 수 있습니다.

- 에 해당 하는 1/96 인치 160 단위
- 64는 센티미터 단위

읽기 전용 [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) 하 고 [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) 정의한 속성 `VisualElement` 기본값이 "모의"의 값 &ndash;1입니다. 요소 크기 조정 및 레이아웃에 적용 된 경우에 이러한 속성 반영 됩니다 장치 독립적 단위에 있는 요소의 실제 크기입니다. 이 크기에 `Padding` 요소에 설정 하지 않고는 `Margin`합니다.

시각적 요소 발생 합니다 [ `SizeChanged` ](xref:Xamarin.Forms.VisualElement.SizeChanged) 이벤트 때 해당 `Width` 또는 `Height` 변경 되었습니다. 합니다 [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) 샘플 프로그램의 화면 크기를 표시 하려면이 이벤트를 사용 합니다.

## <a name="metrical-sizes"></a>Metrical 크기

[ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) 사용 하 여 [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) 표시할를 `BoxView` 1 인치 높이 개와 센티미터 다양 합니다.

## <a name="estimated-font-sizes"></a>예상된 글꼴 크기

합니다 [ **FontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) 샘플은 160 인치 단위에-the-인치 규칙을 사용 하 여 글꼴 크기를 포인트 단위로 지정 하는 방법을 보여 줍니다. 이 기술을 사용 하 여 플랫폼 간 visual 일관성 보다는 낫습니다 `Device.GetNamedSize`합니다.

## <a name="fitting-text-to-available-size"></a>사용 가능한 크기에 텍스트 맞춤

계산 하 여 특정 사각형 내에서 텍스트 블록에 맞게 수를 `FontSize` 의 `Label` 다음 조건을 사용 하 여:

- 줄 간격은 120% 글꼴 크기 (Windows 플랫폼에서 130%)입니다.
- 평균 문자 너비는 글꼴 크기의 50%입니다.

합니다 [ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) 샘플에는이 기술을 보여 줍니다. 되기 전에 작성 된이 프로그램을 [ `Margin` ](xref:Xamarin.Forms.View.Margin) 속성은 사용 하기를 사용할 수는 [ `ContentView` ](xref:Xamarin.Forms.ContentView) 사용 하 여를 [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) 시뮬레이션 하려면 설정을 여백입니다.

[![예상된 글꼴 크기의 3 배가 스크린 샷](images/ch05fg07-small.png "텍스트가 사용 가능한 크기에 맞지")](images/ch05fg07-large.png#lightbox "텍스트 사용 가능한 크기에 맞추기")

## <a name="a-fit-to-size-clock"></a>크기에 맞게 시계

합니다 [ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) 샘플을 사용 하 여 보여 줍니다 [ `Device.StartTimer` ](xref:Xamarin.Forms.Device.StartTimer(System.TimeSpan,System.Func{System.Boolean})) 주기적으로 클록을 업데이트할 때 응용 프로그램에 알리는 하는 타이머를 시작 하려면. 글꼴 크기를 최대한 크게 표시할 수 있도록 페이지 너비의 1/6으로 설정 됩니다.

## <a name="accessibility-issues"></a>액세스 가능성 문제

**EstimatedFontSize** 프로그램과 **FitToSizeClock** 프로그램에는 모두 미묘한 결함이 포함 되어 있습니다. 사용자가 Android 또는 Windows 10 Mobile에서 휴대폰의 접근성 설정을 변경 하는 경우 프로그램에서 글꼴 크기에 따라 텍스트가 렌더링 되는 크기를 더 이상 예상할 수 없습니다. 합니다 [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) 샘플에서는이 문제를 보여 줍니다.

## <a name="empirically-fitting-text"></a>경험적으로 맞춤 텍스트

텍스트 사각형에 맞게 다른 방법은 알려지고 실험적으로 렌더링 된 텍스트 크기를 계산 하 고 위나 아래로 조정 하는 것입니다. 책 호출에서 프로그램 [ `GetSizeRequest` ](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double)) 원하는 크기를 요소의 시각적 요소에 있습니다. 메서드는 더 이상 사용 되지 않습니다을 프로그램 대신 호출 해야 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))합니다.

에 대 한는 `Label`인수를 설정 해야 하지만 두 번째, 첫 번째 인수에 줄 바꿈을 허용) (컨테이너의 너비를 해야 합니다.를 `Double.PositiveInfinity` 비제한 높이 확인 합니다. 합니다 [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) 샘플에는이 기술을 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [5 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [5 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [5 장 F# 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
