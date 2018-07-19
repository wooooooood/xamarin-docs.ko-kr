---
title: Xamarin.Forms 선택
description: Xamarin.Forms 선택 항목을 사용자 항목을 선택할 수 있는 간단한 목록을 표시 합니다. 이 문서 선택기 클래스를 사용 하 여 데이터의 목록에서 텍스트 항목을 선택 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: c852cd29197b000ed1ff53853d64cfa25fb699e7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996599"
---
# <a name="xamarinforms-picker"></a>Xamarin.Forms 선택

_선택기 뷰는 데이터의 목록에서 텍스트 항목을 선택 하는 컨트롤입니다._

Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker) 사용자 항목을 선택할 수 있는 항목의 간단한 목록을 표시 합니다. `Picker` 8 가지 속성을 정의합니다.

- [`Title`](xref:Xamarin.Forms.Picker.Title) 형식의 `string`, 기본값은 `null`합니다.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) 형식의 `IList`, 기본값은 표시할 항목의 원본 목록 `null`합니다.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 형식의 `int`,-1 기본값은 선택한 항목의 인덱스입니다.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 형식의 `object`, 기본값은 선택한 항목을 `null`입니다.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) 형식의 [ `Color` ](xref:Xamarin.Forms.Color), 기본값은 텍스트를 표시 하는데 사용 된 색 [ `Color.Default` ](xref:Xamarin.Forms.Color.Default)합니다.
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) 형식의 [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), 기본값은 [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None)합니다.
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) 형식의 `string`, 기본값은 `null`합니다.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) 형식의 `double`,-1.0 기본값은입니다.

8 가지 속성 모두를 지 원하는 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 스타일 지정할 수 있습니다 하 고 속성에는 데이터 바인딩 대상이 될 수 있음을 의미 하는 개체입니다. 합니다 [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) 및 [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) 속성의 기본 바인딩 모드를 가지 [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), 즉, 데이터 바인딩의 대상 수 응용 프로그램에서 사용 하는 [-Model-view-viewmodel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처입니다. 글꼴 속성을 설정 하는 방법에 대 한 내용은 [글꼴](~/xamarin-forms/user-interface/text/fonts.md)합니다.

A [ `Picker` ](xref:Xamarin.Forms.Picker) 를 처음 표시할 때 데이터를 표시 하지 않습니다. 대신 값을 해당 [ `Title` ](xref:Xamarin.Forms.Picker.Title) 속성 iOS 및 Android 플랫폼에서 자리 표시자로 표시 됩니다.

[![](images/picker-initial.png "선택 표시를 초기")](images/picker-initial-large.png#lightbox "초기 선택기 표시")

경우는 [ `Picker` ](xref:Xamarin.Forms.Picker) 향상 포커스를 해당 데이터가 표시 되 고 항목을 선택할 수 있습니다.

[![](images/picker-selection.png "선택 항목을 선택 하면")](images/picker-selection-large.png#lightbox "선택 항목을 선택 하면")

합니다 [ `Picker` ](xref:Xamarin.Forms.Picker) 발생을 [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) 이벤트는 사용자가 항목을 선택 합니다. 선택 항목을 다음 선택한 항목으로 표시 되는 `Picker`:

![](images/picker-after-selection.png "선택한 후 선택")

두 가지 기술을 채우는 [ `Picker` ](xref:Xamarin.Forms.Picker) 데이터를 사용 하 여:

- 설정 된 [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) 속성을 표시할 데이터입니다. 이것이 2.3.4 Xamarin.Forms에 도입 된 권장된 방법을입니다. 자세한 내용은 [선택기의 ItemsSource 속성을 설정할](populating-itemssource.md)합니다.
- 에 표시할 데이터를 추가 합니다 [ `Items` ](xref:Xamarin.Forms.Picker.Items) 컬렉션입니다. 이 기술은 채우기 위한 원래 프로세스는 [ `Picker` ](xref:Xamarin.Forms.Picker) 데이터를 사용 하 여 합니다. 자세한 내용은 [선택기 항목 컬렉션에 추가 데이터](populating-items.md)입니다.

## <a name="related-links"></a>관련 링크

- [선택기](xref:Xamarin.Forms.Picker)
