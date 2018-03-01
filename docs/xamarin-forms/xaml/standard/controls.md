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
ms.openlocfilehash: 3f30a77975a9f42380ecf7efd73426763ec83ef0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-standard-preview-controls"></a>XAML (미리 보기) 표준 컨트롤

![미리 보기](~/media/shared/preview.png)

이 페이지는 해당 하는 Xamarin.Forms 제어와 함께 미리 보기에서 사용할 수 있는 XAML 표준 컨트롤을 나열합니다.

새 속성 및 열거형에 있는 이름이 표준 XAML 컨트롤의 목록은 이기도 합니다.

## <a name="controls"></a>컨트롤

<table style="width:300px">
  <tr><th>Xamarin.Forms</th><th>XAML 표준</th></tr>
  <tr><td>프레임</td><td>테두리</td></tr>
  <tr><td>선택</td><td>ComboBox</td></tr>
  <tr><td>ActivityIndicator</td><td>ProgressRing</td></tr>
  <tr><td>StackLayout</td><td>StackPanel</td></tr>
  <tr><td>레이블</td><td>TextBlock</td></tr>
  <tr><td>입력</td><td>TextBox</td></tr>
  <tr><td>전환</td><td>ToggleSwitch</td></tr>
  <tr><td>ContentView</td><td>UserControl</td></tr>
</table>

## <a name="properties-and-enumerations"></a>속성 및 열거형

<table>
  <tr><th>Xamarin.Forms<br/>업데이트 된 속성을 사용 하 여 컨트롤</th><th>Xamarin.Forms<br/>속성 또는 열거형</th><th>XAML 표준<br/>해당 항목</th></tr>
  <tr><td>Button, Entry, Label, DatePicker, Editor, SearchBar, TimePicker</td><td>TextColor</td><td>Foreground</td></tr>
  <tr><td>VisualElement</td><td>BackgroundColor</td><td><i>배경 *</i></td></tr>
  <tr><td>코드 조각 선택기 단추</td><td>BorderColor, OutlineColor</td><td>BorderBrush</td></tr>
  <tr><td>단추</td><td>BorderWidth</td><td>BorderThickness</td></tr>
  <tr><td>ProgressBar</td><td>진행률</td><td>값</td></tr>
  <tr><td>단추, 항목, 레이블, 편집기, SearchBar, 범위, 글꼴</td><td>FontAttributes<br/>굵게, 기울임꼴, 없음</td><td>FontStyle<br/>기울임꼴, 보통</td></tr>
  <tr><td>단추, 항목, 레이블, 편집기, SearchBar, 범위, 글꼴</td><td>FontAttributes</td><td><i>FontWeights *</i><br/>굵게, 일반</td></tr>
  <tr><td>InputView</td><td>키보드<br/>기본적으로 Url, 숫자, 전화, 텍스트, 채팅, 전자 메일</td><td><i>InputScopeNameValue *</i><br/>Default, Url, Number, TelephoneNumber, Text, Chat, EmailNameOrAddress</td></tr>
  <tr><td>StackPanel</td><td>StackOrientation</td><td><i>방향 *</i></td></tr>
</table>

> [!IMPORTANT]
> 표시 된 항목 * 현재 미리 보기에 완전 하지 않습니다


## <a name="related-links"></a>관련 링크

- [미리 보기 NuGet](https://aka.ms/xf-xamlstandard-nuget)
