---
title: Xamarin.Forms 데이터 템플릿
description: DataTemplate 지원 되는 컨트롤에 대해 데이터의 모양을 지정 하는 데 사용 됩니다 및 일반적으로 표시 될 데이터에 바인딩합니다.
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 771ae22c3e28a4fce758bbfd6a3bd63bafb75e53
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994973"
---
# <a name="xamarinforms-data-templates"></a>Xamarin.Forms 데이터 템플릿

_DataTemplate 지원 되는 컨트롤에 대해 데이터의 모양을 지정 하는 데 사용 됩니다 및 일반적으로 표시 될 데이터에 바인딩합니다._

## <a name="introductionintroductionmd"></a>[소개](introduction.md)

Xamarin.Forms 데이터 템플릿은 지원 되는 컨트롤에 데이터를 정의 하는 기능을 제공 합니다. 이 문서에서는 데이터 템플릿이 필요한 이유를 검사를 소개 합니다.

## <a name="creating-a-datatemplatecreatingmd"></a>[DataTemplate 만들기](creating.md)

데이터 템플릿은에서 인라인으로 만들 수 있습니다는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), 사용자 지정 형식 또는 적절 한 Xamarin.Forms 셀 형식입니다. 인라인 템플릿을 사용할 경우 데이터 템플릿을 다른 곳에서 다시 사용할 필요가 없습니다. 또는 데이터 템플릿 사용자 지정 형식으로 또는 제어 수준, 페이지 수준 또는 응용 프로그램 수준 리소스로 정의 하 여 재사용할 수 있습니다.

## <a name="creating-a-datatemplateselectorselectormd"></a>[DataTemplateSelector 만들기](selector.md)

A [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) 선택에 사용할 수는 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 데이터 바인딩된 속성의 값을 기반으로 하는 런타임 시. 이렇게 하면 여러 `DataTemplate` 인스턴스가 동일한 유형의 개체를 특정 개체의 모양을 사용자 지정을 적용할 수 있습니다. 이 문서를 만들고 사용 하는 방법을 보여 줍니다는 `DataTemplateSelector`합니다.


## <a name="related-links"></a>관련 링크

- [데이터 템플릿 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
