---
title: 요약 - 5장. 크기 처리
description: 'Xamarin.Forms로 모바일 앱 만들기: 요약 - 5장. 크기 처리'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: c082bdb10732e42b37511cf050e50f46990a5b5b
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "70771148"
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>요약 - 5장. 크기 처리

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)

> [!NOTE]
> 이 페이지의 정보는 Xamarin.Forms가 책에 제공된 자료와 다른 영역을 표시합니다.

지금까지 Xamarin.Forms의 여러 크기를 살펴보았습니다.

- iOS 상태 표시줄의 높이는 20입니다.
- `BoxView`의 기본 너비와 높이는 40입니다.
- `Frame`의 기본 `Padding`은 20입니다.
- `StackLayout`의 기본 `Spacing`은 6입니다.
- `Device.GetNamedSize` 메서드는 숫자 글꼴 크기를 반환합니다.

이러한 크기는 픽셀이 아닙니다. 대신 각 플랫폼에서 독립적으로 인식하는 디바이스별 단위입니다.

## <a name="pixels-points-dps-dips-and-dius"></a>픽셀, 포인트, dps, DIP 및 DIU

Apple Mac 및 Microsoft Windows 초창기에는 프로그래머들이 픽셀 단위로 작업했습니다. 그러나 고해상도 디스플레이의 출현으로 화면 좌표계에 대한 보다 가상화되고 추상적인 접근법이 필요했습니다. Mac 세계에서 프로그래머는 *포인트*(일반적으로 1/72인치) 단위로 작업했고, Windows 개발자는 1/96인치를 기준으로 하는 DIU(*디바이스 독립적 단위*)를 사용했습니다.

그러나 모바일 디바이스는 일반적으로 얼굴에 훨씬 더 가까이 대고 사용하고 데스크톱 화면보다 해상도가 높기 때문에 더 많은 픽셀 밀도가 허용될 수 있습니다.

Apple iPhone 및 iPad 디바이스를 대상으로 하는 프로그래머는 *포인트* 단위로 계속 작업하지만 이러한 포인트가 인치당 160개 있습니다. 디바이스에 따라 포인트당 1, 2 또는 3개 픽셀이 있을 수 있습니다.

Android도 유사합니다. 프로그래머는 dps(*밀도 독립적 픽셀*) 단위로 작업하며 dps와 픽셀 간의 관계는 인치당 160dps를 기준으로 합니다.

Windows 전화 및 모바일 디바이스도 인치당 160diu(디바이스 독립적 단위)에 가까운 배율 인수를 설정했습니다.

> [!NOTE]
> Xamarin.Forms는 Windows 기반 전화 또는 모바일 디바이스를 더 이상 지원하지 않습니다.

요약하자면 휴대폰 및 태블릿을 대상으로 하는 Xamarin.Forms 프로그래머는 모든 측정 단위가 다음 조건을 기반으로 한다고 가정할 수 있습니다.

- 인치당 160단위
- 센티미터당 64단위

`VisualElement`에서 정의한 읽기 전용 [`Width`](xref:Xamarin.Forms.VisualElement.Width) 및 [`Height`](xref:Xamarin.Forms.VisualElement.Height) 속성의 기본 "mock" 값은 &ndash;1입니다. 요소가 레이아웃에 맞게 크기가 조정된 경우에만 이러한 속성이 디바이스 독립적인 단위로 요소의 실제 크기를 반영합니다. 이 크기에는 요소에 설정된 `Padding`은 포함되어 있지만 `Margin`은 포함되어 있지 않습니다.

시각적 요소는 `Width` 또는 `Height`가 변경된 경우에 [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged) 이벤트를 발생시킵니다. [**WhatSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) 샘플에서는 이 이벤트를 사용하여 프로그램 화면의 크기를 표시합니다.

## <a name="metrical-sizes"></a>메트릭 크기

[**MetricalBoxView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView)는 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)를 사용하여 높이 1인치, 너비 1인치의 `BoxView`를 표시합니다.

## <a name="estimated-font-sizes"></a>예상 글꼴 크기

[**FontSizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) 샘플에서는 인치당 160단위 규칙을 사용하여 글꼴 크기를 포인트 단위로 지정하는 방법을 보여줍니다. 이 기법을 사용하는 플랫폼 간의 시각적 일관성은 `Device.GetNamedSize`보다 좋습니다.

## <a name="fitting-text-to-available-size"></a>사용 가능한 크기로 텍스트 맞춤

다음 기준에 따라 `Label`의 `FontSize`를 계산하여 텍스트 블록을 특정 사각형 내에 맞출 수 있습니다.

- 줄 간격은 글꼴 크기의 120%(Windows 플랫폼의 경우 130%)입니다.
- 평균 문자 너비는 글꼴 크기의 50%입니다.

[**EstimatedFontSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) 샘플에서는 이 기법을 보여줍니다. 이 프로그램은 [`Margin`](xref:Xamarin.Forms.View.Margin) 속성을 사용할 수 있게 되기 전에 작성되었으므로 [`Padding`](xref:Xamarin.Forms.Layout.Padding) 설정이 지정된 [`ContentView`](xref:Xamarin.Forms.ContentView)를 사용하여 여백을 시뮬레이션합니다.

[![EstimatedFontSize의 삼중 스크린샷](images/ch05fg07-small.png "사용 가능한 크기로 텍스트 맞춤")](images/ch05fg07-large.png#lightbox "사용 가능한 크기로 텍스트 맞춤")

## <a name="a-fit-to-size-clock"></a>크기에 맞춤 시계

[**FitToSizeClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) 샘플에서는 [`Device.StartTimer`](xref:Xamarin.Forms.Device.StartTimer(System.TimeSpan,System.Func{System.Boolean}))를 사용하여 시계를 업데이트할 때가 되면 애플리케이션에 주기적으로 알리는 타이머를 시작하는 방법을 보여줍니다. 글꼴 크기는 디스플레이가 최대한 크게 표시되도록 페이지 너비의 1/6로 설정됩니다.

## <a name="accessibility-issues"></a>접근성 문제

**EstimatedFontSize** 프로그램과 **FitToSizeClock** 프로그램 모두에는 미묘한 결함이 있습니다. 사용자가 Android 또는 Windows 10 Mobile에서 휴대폰의 접근성 설정을 변경할 경우 프로그램이 글꼴 크기에 따라 텍스트가 렌더링되는 크기를 더 이상 예상할 수 없습니다. [**AccessibilityTest**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) 샘플에서는 이 문제를 보여줍니다.

## <a name="empirically-fitting-text"></a>경험적으로 텍스트 맞춤

텍스트를 사각형에 맞게 조정하는 또 다른 방법은 렌더링된 텍스트 크기를 경험적으로 계산하고 위아래로 조정하는 것입니다. 이 책의 프로그램에서는 시각적 요소에 대한 [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double))를 호출하여 요소의 원하는 크기를 가져옵니다. 이 메서드는 더 이상 사용되지 않으며, 프로그램은 [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))를 대신 호출해야 합니다.

`Label`의 경우 첫 번째 인수는 래핑을 허용하기 위해 컨테이너의 너비여야 하고, 두 번째 인수는 높이가 제한되지 않도록 `Double.PositiveInfinity`로 설정되어야 합니다. [**EmpiricalFontSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) 샘플에서는 이 기법을 보여줍니다.

## <a name="related-links"></a>관련 링크

- [5장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [5장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [5장 F# 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
