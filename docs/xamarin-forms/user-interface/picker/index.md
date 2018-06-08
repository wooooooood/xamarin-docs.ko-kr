---
title: 선택
description: 선택기 보기에는 데이터의 목록에서 텍스트 항목을 선택 하기 위한 컨트롤입니다.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 7f0050351ca28d7f8afeb82a85e82e51d399824b
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847500"
---
# <a name="picker"></a>선택

_선택기 보기에는 데이터의 목록에서 텍스트 항목을 선택 하기 위한 컨트롤입니다._

Xamarin.Forms는 [ `Picker` ](xref:Xamarin.Forms.Picker) 사용자 항목을 선택할 수 있는 항목의 간단한 목록을 표시 합니다. `Picker` 8 가지 속성을 정의합니다.

- [`Title`](xref:Xamarin.Forms.Picker.Title) 형식의 `string`, 데이터베이스가 기본 인 `null`합니다.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) 형식의 `IList`, 기본 설정 된 항목을 표시 하려면 소스 목록 `null`합니다.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 형식의 `int`, 기본값-1은 선택한 항목의 인덱스입니다.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 형식의 `object`, 기본적으로 선택한 항목을 `null`합니다.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) 형식의 [ `Color` ](xref:Xamarin.Forms.Color), 기본 설정 된 텍스트를 표시 하는 데 사용할 색 [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/)합니다.
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) 형식의 [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), 데이터베이스가 기본 인 [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None)합니다.
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) 형식의 `string`, 데이터베이스가 기본 인 `null`합니다.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) 형식의 `double`,-1.0 데이터베이스가 기본 인 합니다.

모든 8 가지 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 개체 즉, 이러한 스타일을 지정할 수 속성에는 데이터 바인딩의 대상이 될 수 있습니다. [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) 및 [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) 속성의 기본 바인딩 모드에는 [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), 즉, 데이터 바인딩의 대상이 될 수 사용 하는 응용 프로그램에는 [모델-뷰-MVVM ()](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처. 글꼴 속성을 설정 하는 방법에 대 한 정보를 참조 하십시오. [글꼴](~/xamarin-forms/user-interface/text/fonts.md)합니다.

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 처음 표시 된 모든 데이터를 표시 하지 않습니다. 대신, 값 해당 [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) 속성 iOS 및 Android 플랫폼에 대 한 자리 표시자로 표시 됩니다.

[![](images/picker-initial.png "선택 표시를 초기")](images/picker-initial-large.png#lightbox "선택 표시를 초기 합니다.")

경우는 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 향상 포커스를 데이터가 표시 되 고 사용자는 항목을 선택할 수 있습니다.

[![](images/picker-selection.png "선택 항목을 선택 하면")](images/picker-selection-large.png#lightbox "선택 항목을 선택 하면")

[ `Picker` ](xref:Xamarin.Forms.Picker) 발생 한 [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) 이벤트는 사용자가 항목을 선택 합니다. 선택 항목을 다음 선택된 항목으로 표시 됩니다는 `Picker`:

![](images/picker-after-selection.png "선택한 후 선택")

채우기 위한 두 가지 방법이 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 데이터로:

- 설정의 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) 속성을 데이터를 표시 합니다. 이것이 2.3.4 Xamarin.Forms에 도입 된 권장된 방법입니다. 자세한 내용은 참조 [선택기를 ItemsSource 속성이 설정](populating-itemssource.md)합니다.
- 에 게 표시 될 데이터 추가 [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) 컬렉션입니다. 이 기술은 was를 채우기 위한 원래의 프로세스는 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 데이터를 사용 합니다. 자세한 내용은 참조 [선택기를 Items 컬렉션에 데이터 추가](populating-items.md)합니다.

## <a name="related-links"></a>관련 링크

- [선택기](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
