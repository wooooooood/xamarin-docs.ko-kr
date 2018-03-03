---
title: "데이터 템플릿"
description: "DataTemplate 지원 되는 컨트롤에 데이터의 모양을 지정 하는 데 사용 되 고 일반적으로 표시 되는 데이터에 바인딩할 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 99a00c98471ae85af2a8cba2e1e52444370a9332
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="data-templates"></a>데이터 템플릿

_DataTemplate 지원 되는 컨트롤에 데이터의 모양을 지정 하는 데 사용 되 고 일반적으로 표시 되는 데이터에 바인딩할 합니다._

## <a name="introductionintroductionmd"></a>[소개](introduction.md)

Xamarin.Forms 데이터 템플릿은 지원 되는 컨트롤에 데이터의 표현을 정의 하는 기능을 제공 합니다. 이 문서는 필요한 이유를 검사 하는 데이터 서식 파일에 대 한 소개를 제공 합니다.

## <a name="creating-a-datatemplatecreatingmd"></a>[DataTemplate 만들기](creating.md)

데이터 서식 파일에서 인라인으로 만들 수 있습니다는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), 또는 사용자 정의 형식 또는 적절 한 Xamarin.Forms 셀 유형입니다. 데이터 서식 파일을 다른 곳에서 다시 사용할 필요가 없는 경우 인라인 템플릿은 사용 해야 합니다. 또는 데이터 서식 파일은 컨트롤 수준 페이지 수준 또는 응용 프로그램 수준 리소스 또는 사용자 지정 형식으로 정의 하 여 재사용할 수 있습니다.

## <a name="creating-a-datatemplateselectorselectormd"></a>[DataTemplateSelector 만들기](selector.md)

A [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) 선택에 사용할 수는 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 데이터 바인딩된 속성의 값에 따라 런타임에 합니다. 이렇게 하면 여러 `DataTemplate` 인스턴스는 동일한 유형의 특정 개체의 모양을 사용자 지정할 개체에 적용 될 수 있습니다. 이 문서에서는 만들고 사용 하는 방법을 `DataTemplateSelector`합니다.


## <a name="related-links"></a>관련 링크

- [데이터 템플릿 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
