---
title: Xamarin 양식 선택기
description: Xamarin.ios 선택은 사용자가 항목을 선택할 수 있는 간단한 항목 목록을 표시 합니다. 이 문서에서는 선택 클래스를 사용 하 여 데이터 목록에서 텍스트 항목을 선택 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
ms.openlocfilehash: c8246f8316a2a579bc890935dc4f9beecccb8dcc
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695996"
---
# <a name="xamarinforms-picker"></a>Xamarin 양식 선택기

_선택 뷰는 데이터 목록에서 텍스트 항목을 선택 하는 컨트롤입니다._

Xamarin.ios [`Picker`](xref:Xamarin.Forms.Picker) 은 사용자가 항목을 선택할 수 있는 짧은 항목 목록을 표시 합니다. `Picker`는 다음 속성을 정의 합니다.

- `string` 형식의 [`Title`](xref:Xamarin.Forms.Picker.Title) 이며 기본값은 `null`입니다.
- [`Color`](xref:Xamarin.Forms.Color)형식의 `TitleColor` `Title` 텍스트를 표시 하는 데 사용 되는 색입니다.
- 표시 되는 항목의 원본 목록 `IList` 형식의 [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) 이며 기본값은 `null`입니다.
- `int` 형식의 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 선택 된 항목의 인덱스 이며 기본값은-1입니다.
- 선택한 항목인 `object` 형식의 [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 이며 기본값은 `null`입니다.
- [`Color`](xref:Xamarin.Forms.Color)형식의 [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) 텍스트를 표시 하는 데 사용 되는 색입니다. 기본값은 [`Color.Default`](xref:Xamarin.Forms.Color.Default)입니다.
- [`FontAttributes`](xref:Xamarin.Forms.FontAttributes)형식의 [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) 이며 기본값은 [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None)입니다.
- `string` 형식의 [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) 이며 기본값은 `null`입니다.
- `double` 형식의 [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) 이며 기본값은-1.0입니다.
- `double` 형식의 `CharacterSpacing`는 `Picker`에 표시 되는 항목의 문자 간 간격입니다.

모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에 의해 지원 됩니다. 즉, 스타일을 지정할 수 있으며 속성은 데이터 바인딩의 대상이 될 수 있습니다. [@No__t_1](xref:Xamarin.Forms.Picker.SelectedIndex) 및 [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 속성은 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)의 기본 바인딩 모드를 사용 합니다. 즉, [MVVM (모델-뷰-ViewModel)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처를 사용 하는 응용 프로그램에서 데이터 바인딩의 대상이 될 수 있습니다. 글꼴 속성을 설정 하는 방법에 대 한 자세한 내용은 [글꼴](~/xamarin-forms/user-interface/text/fonts.md)을 참조 하십시오.

[@No__t_1](xref:Xamarin.Forms.Picker) 처음 표시 될 때 데이터를 표시 하지 않습니다. 대신, 해당 [`Title`](xref:Xamarin.Forms.Picker.Title) 속성의 값은 IOS 및 Android 플랫폼에서 자리 표시자로 표시 됩니다.

[![](images/picker-initial.png "Initial Picker Display")](images/picker-initial-large.png#lightbox "Initial Picker Display")

[@No__t_1](xref:Xamarin.Forms.Picker) 포커스를 얻으면 데이터가 표시 되 고 사용자가 항목을 선택할 수 있습니다.

[![](images/picker-selection.png "Picker Selecting an Item")](images/picker-selection-large.png#lightbox "Picker Selecting an Item")

[@No__t_1](xref:Xamarin.Forms.Picker) 은 사용자가 항목을 선택할 때 [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) 이벤트를 발생 시킵니다. 선택 후 선택한 항목이 `Picker` 표시 됩니다.

![](images/picker-after-selection.png "Picker after Selection")

[@No__t_1](xref:Xamarin.Forms.Picker) 를 데이터로 채우는 방법에는 두 가지가 있습니다.

- [@No__t_1](xref:Xamarin.Forms.Picker.ItemsSource) 속성을 표시할 데이터로 설정 합니다. 이것이 권장되는 방법입니다. 자세한 내용은 [선택기의 System.windows.controls.itemscontrol.itemssource 속성 설정](populating-itemssource.md)을 참조 하세요.
- [@No__t_1](xref:Xamarin.Forms.Picker.Items) 컬렉션에 표시할 데이터를 추가 하는 중입니다. 이 기법은 [`Picker`](xref:Xamarin.Forms.Picker) 를 데이터로 채우는 원래 프로세스 였습니다. 자세한 내용은 [선택기의 항목 컬렉션에 데이터 추가](populating-items.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [선택기](xref:Xamarin.Forms.Picker)
