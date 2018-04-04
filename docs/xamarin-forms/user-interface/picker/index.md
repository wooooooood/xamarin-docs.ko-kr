---
title: 선택
description: 선택기 보기에는 데이터의 목록에서 텍스트 항목을 선택 하기 위한 컨트롤입니다.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 9889502b635997dbb5e2b79a7654bf1ff0c99861
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="picker"></a>선택

_선택기 보기에는 데이터의 목록에서 텍스트 항목을 선택 하기 위한 컨트롤입니다._

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 항목을 선택할 수 있는 간단한 목록을 표시 합니다. 그러나 한 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 처음 표시 된 모든 데이터를 표시 하지 않습니다. 대신, 값 해당 [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) 속성 iOS 및 Android 플랫폼에 대 한 자리 표시자로 표시 됩니다.

[![](images/picker-initial.png "선택 표시를 초기")](images/picker-initial-large.png#lightbox "선택 표시를 초기 합니다.")

경우는 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 향상 포커스를 데이터가 표시 되 고 사용자는 항목을 선택할 수 있습니다.

[![](images/picker-selection.png "선택 항목을 선택 하면")](images/picker-selection-large.png#lightbox "선택 항목을 선택 하면")

선택 항목을 다음 선택된 항목으로 표시 됩니다는 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/):

![](images/picker-after-selection.png "선택한 후 선택")

채우기 위한 두 가지 방법이 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 데이터로:

- 설정의 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) 속성을 데이터를 표시 합니다. 이것이 2.3.4 Xamarin.Forms에 도입 된 권장된 방법입니다. 자세한 내용은 참조 [선택기를 ItemsSource 속성이 설정](populating-itemssource.md)합니다.
- 에 게 표시 될 데이터 추가 [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) 컬렉션입니다. 이 기술은 was를 채우기 위한 원래의 프로세스는 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 데이터를 사용 합니다. 자세한 내용은 참조 [선택기를 Items 컬렉션에 데이터 추가](populating-items.md)합니다.


## <a name="related-links"></a>관련 링크

- [선택기](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
