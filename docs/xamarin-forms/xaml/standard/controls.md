---
title: "XAML (미리 보기) 표준 컨트롤"
description: "Xamarin.forms에서는 XAML 표준 미리 보기를 탐색을 시작 하는 방법"
ms.topic: article
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: b044cb849f9a8e591a8db5907211a55f77d6e45f
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="xaml-standard-preview-controls"></a>XAML (미리 보기) 표준 컨트롤

![미리 보기](~/media/shared/preview.png)

이 페이지는 해당 하는 Xamarin.Forms 제어와 함께 미리 보기에서 사용할 수 있는 XAML 표준 컨트롤을 나열합니다.

새 속성 및 열거형에 있는 이름이 표준 XAML 컨트롤의 목록은 이기도 합니다.

## <a name="controls"></a>컨트롤

|Xamarin.Forms|XAML 표준|
|--- |--- |
|프레임|테두리|
|선택|ComboBox|
|ActivityIndicator|ProgressRing|
|StackLayout|StackPanel|
|레이블|TextBlock|
|입력|TextBox|
|전환|ToggleSwitch|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>속성 및 열거형

|업데이트 된 속성을 가진 Xamarin.FormsControls|Xamarin.FormsProperty 또는 열거형|XAML StandardEquivalent|
|--- |--- |--- |
|Button, Entry, Label, DatePicker, Editor, SearchBar, TimePicker|TextColor|Foreground|
|VisualElement|BackgroundColor|배경 *|
|코드 조각 선택기 단추|BorderColor, OutlineColor|BorderBrush|
|단추|BorderWidth|BorderThickness|
|ProgressBar|진행률|값|
|단추, 항목, 레이블, 편집기, SearchBar, 범위, 글꼴|FontAttributesBold, Italic, None|FontStyleItalic, Normal|
|단추, 항목, 레이블, 편집기, SearchBar, 범위, 글꼴|FontAttributes|FontWeights *Bold, Normal|
|InputView|KeyboardDefault, Url, 숫자, 전화, 텍스트, 채팅을 전자 메일로 보내기|InputScopeNameValue *Default, Url, Number, TelephoneNumber, Text, Chat, EmailNameOrAddress|
|StackPanel|StackOrientation|방향 *|

> [!IMPORTANT]
> 표시 된 항목 * 현재 미리 보기에 완전 하지 않습니다

## <a name="related-links"></a>관련 링크

- [미리 보기 NuGet](https://aka.ms/xf-xamlstandard-nuget)
