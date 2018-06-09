---
title: 5 장의 요약입니다. 크기와 처리
description: 'Xamarin.Forms를 사용 하 여 모바일 응용 프로그램 만들기: 5 장 요약 합니다. 크기와 처리'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0b93942d6623106a5e507d6eef3e7140f9d409bd
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241649"
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>5 장의 요약입니다. 크기와 처리

Xamarin.Forms에 몇 가지 크기로 지금까지 발생 했습니다.

- IOS 상태 표시줄의 높이 20
- `BoxView` 기본 너비와 높이가 40
- 기본 `Padding` 에 `Frame` 은 20
- 기본 `Spacing` 에 `StackLayout` 은 6
- `Device.GetNamedSize` 숫자 글꼴 크기를 반환 하는 메서드

이러한 크기 (픽셀) 않습니다. 대신 장치 독립적 단위는 각 플랫폼에서 독립적으로 인식 됩니다.

## <a name="pixels-points-dps-dips-and-dius"></a>픽셀, 점, dp, Dip 및 DIUs

Apple Mac 및 Microsoft Windows의 기록, 초기에 프로그래머는 픽셀 단위로 작동 합니다. 그러나 등장 고해상도 디스플레이의 화면 좌표를 가상화 하 고 추상 접근 방식이 필요합니다. Mac 세계에서 프로그래머의 단위로 작동 *포인트*, 일반적으로 1/72 인치, Windows 개발자가 사용 하는 동안 *장치 독립적 단위* (DIUs) 1/96 인치의 기반 합니다.

그러나 모바일 장치에는 글꼴에 더 가깝습니다 유지 일반적으로 하 고 데스크톱 보다 더 높은 해상도 화면 큰 픽셀 밀도 허용 될 수를 의미 합니다.

Apple iPhone 및 iPad 장치를 대상으로 하는 프로그래머의 단위에서 작업을 계속 *포인트*만 160 인치에 이러한 사항이 있습니다. 장치에 따라 1, 2 또는 3 픽셀 지점 수. 있습니다

Android가 비슷합니다. 프로그래머의 단위로 작동 *밀도 독립적 픽셀* (dp) 인치당 160 dp dp 사이의 픽셀 상관 관계를 기반 합니다.

Windows 런타임 인치당 160 장치 독립적 단위에 가까운 내재 된 배율 설정도 했습니다.

요약 하자면, 휴대폰 및 태블릿을 대상으로 하는 Xamarin.Forms 프로그래머 모든 측정 단위는 다음 조건에 따라 결정 되도록 가정할 수 있습니다.

- 해당 하는 인치 160 단위
- 64는 센티미터 단위

읽기 전용 [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) 및 [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) 정의한 속성 `VisualElement` "모의" 값의 기본 &ndash;1입니다. 요소 크기가 조정 되 고 레이아웃에 적용 하는 경우에 이러한 속성 반영 됩니다 장치 독립적 단위에 있는 요소의 실제 크기입니다. 이 크기를 포함 하는 `Padding` 요소에 설정 하지 않고는 `Margin`합니다.

시각적 요소 발생는 [ `SizeChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/) 이벤트 때 해당 `Width` 또는 `Height` 변경 되었습니다. [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) 샘플이이 이벤트를 사용 하 여 프로그램의 화면 크기를 표시 합니다.

## <a name="metrical-sizes"></a>Metrical 크기

[ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) 사용 하 여 [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) 및 [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) 표시 하는 `BoxView` 1 인치 높이 및 하나 센티미터 너비입니다.

## <a name="estimated-font-sizes"></a>예상된 글꼴 크기

[ **FontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) 샘플 포인트 단위로에서 글꼴 크기를 지정 하려면 160 인치 단위를-the-인치 규칙을 사용 하는 방법을 보여 줍니다. 이 기술을 사용 하 여 플랫폼 마다 시각적 일관성 더 좋은 것은 `Device.GetNamedSize`합니다.

## <a name="fitting-text-to-available-size"></a>사용 가능한 크기에 대 한 텍스트 맞춤

수를 계산 하 여 특정 사각형 내에서 텍스트 블록에 맞게는 `FontSize` 의 `Label` 다음과 같은 조건을 사용 하 여:

- 줄 간격은 120% 글꼴 크기 (Windows 플랫폼에서 130%)입니다.
- 평균 문자 너비는 글꼴 크기의 50%입니다.

[ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) 샘플에는이 기술을 보여 줍니다. 하기 전에 작성 된이 [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) 속성은 사용할 수 있으므로 사용는 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) 와 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) 설정을 시뮬레이션 하는 여백입니다.

[![예상된 글꼴 크기의 세 스크린 샷](images/ch05fg07-small.png "텍스트 사용 가능한 크기에 맞추기")](images/ch05fg07-large.png#lightbox "텍스트 사용 가능한 크기에 맞추기")

## <a name="a-fit-to-size-clock"></a>크기에 맞게 시계

[ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) 샘플 사용을 보여 줍니다. [ `Device.StartTimer` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.StartTimer/p/System.TimeSpan/System.Func%7BSystem.Boolean%7D/) 를 주기적으로 시계를 업데이트 하는 응용 프로그램에 알립니다 하는 타이머를 시작 합니다. 글꼴 크기는 디스플레이 최대한 크게 되도록 페이지 너비의 1/6에 설정 됩니다.

## <a name="accessibility-issues"></a>내게 필요한 옵션 문제

**EstimatedFontSize** 프로그램 및 **FitToSizeClock** 프로그램 모두에 감지 하기 어려운 결함 들어: 사용자가 Android 또는 Windows 10 Mobile, 프로그램을 더 이상 휴대폰의 내게 필요한 옵션 설정을 변경 하는 경우 예측할 수 크기 텍스트가 렌더링 되는 글꼴 크기에 따라. [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) 샘플에서는이 문제를 보여 줍니다.

## <a name="empirically-fitting-text"></a>실험적으로 텍스트

텍스트 사각형에 맞게 다른 방법은 실험적으로 렌더링 된 텍스트 크기를 계산 하 여 늘리거나 줄여서 조정 하는 것입니다. 책 호출에서 프로그램 [ `GetSizeRequest` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/) 에서 시각적 요소는 요소의 원하는 크기를 구합니다. 메서드는 더 이상 사용 되지 않습니다, 및 프로그램 대신 호출 해야 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)합니다.

에 대 한는 `Label`, 첫 번째 인수는 줄 바꿈을 허용) (컨테이너의 너비 수, 두 번째 인수는 설정 해야를 `Double.PositiveInfinity` 제약 없이 높이 있도록 합니다. [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) 샘플에는이 기술을 보여 줍니다.



## <a name="related-links"></a>관련 링크

- [5 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [5 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [5 장 F # 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
