---
title: XAML 표준 (미리 보기) 컨트롤
description: 이 문서에서는 Xamarin.Forms에서 사용할 수 있는 XAML 표준 컨트롤을 살펴봅니다.
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/15/2017
ms.openlocfilehash: b9bf0e1ba14f4e8584bfd8492776ac7c8668df87
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61175180"
---
# <a name="xaml-standard-preview-controls"></a>XAML 표준 (미리 보기) 컨트롤

![미리 보기](~/media/shared/preview.png)

이 페이지는 미리 보기에서 해당 하는 Xamarin.Forms 제어와 함께 사용할 수 있는 표준 XAML 컨트롤을 나열합니다.

XAML 표준에 새 속성 이름과 열거형이 있는 컨트롤의 목록은 이기도 합니다.

## <a name="controls"></a>컨트롤

|Xamarin.Forms|XAML 표준|
|--- |--- |
|프레임|테두리|
|선택기|ComboBox|
|ActivityIndicator|ProgressRing|
|StackLayout|StackPanel|
|레이블|TextBlock|
|입력|TextBox|
|전환|ToggleSwitch|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>속성 및 열거형

|업데이트 된 속성을 사용 하 여 Xamarin.Forms 컨트롤|Xamarin.Forms 속성 또는 열거형|XAML 표준에 해당|
|--- |--- |--- |
|단추, 항목, 레이블, DatePicker, 편집기, SearchBar, TimePicker|TextColor|Foreground|
|VisualElement|BackgroundColor|배경 *|
|단추 선택|BorderColor, OutlineColor|BorderBrush|
|단추|BorderWidth|BorderThickness|
|ProgressBar|진행률|값|
|단추, 항목, 레이블, 편집기, SearchBar, 범위, 글꼴|FontAttributesBold, 기울임꼴, 없음|FontStyleItalic, 일반|
|단추, 항목, 레이블, 편집기, SearchBar, 범위, 글꼴|FontAttributes|FontWeights * 굵게, 일반|
|InputView|KeyboardDefault, Url, 숫자, 전화, 텍스트, 채팅, 전자 메일|InputScopeNameValue * 기본, Url, 숫자, TelephoneNumber, 텍스트, 채팅, EmailNameOrAddress|
|StackPanel|StackOrientation|방향 *|

> [!IMPORTANT]
> 표시 된 항목 *는 현재 미리 보기에서 완전 하지 않습니다

## <a name="related-links"></a>관련 링크

- [NuGet 미리 보기](https://aka.ms/xf-xamlstandard-nuget)
