---
title: XAML 스타일을 사용 하 여 Xamarin.Forms 앱 스타일 지정
description: 이 가이드에는 XAML 스타일을 사용 하 여 Xamarin.Forms 응용 프로그램의 모양을 사용자 지정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 2607298bdc0842f60a1d1a3299bed61bbea925a1
ms.sourcegitcommit: 2f6a5c1abf90fbdb0475fd8a3ce6de3cd7c7d575
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52459865"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>XAML 스타일을 사용 하 여 Xamarin.Forms 앱 스타일 지정

## <a name="introductionintroductionmd"></a>[소개](introduction.md)

Xamarin.Forms 응용 프로그램은 종종 동일한 모양이 있는 여러 컨트롤을 포함 합니다. 반복 될 수 있습니다 각 개별 컨트롤의 모양을 설정 하 고 오류가 발생 하기 쉽습니다. 대신, 스타일 만들 수 있습니다 그룹화 하 고 컨트롤 형식에 사용할 수 있는 속성을 설정 하 여 컨트롤 모양을 사용자 지정 하는 합니다.

## <a name="explicit-stylesexplicitmd"></a>[명시적 스타일](explicit.md)

*명시적* 스타일을 설정 하 여 선택적으로 컨트롤에 적용 된 해당 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) 속성입니다.

## <a name="implicit-stylesimplicitmd"></a>[암시적 스타일](implicit.md)

*암시적* 스타일은 동일한 모든 컨트롤에서 사용 되는 하나 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType), 각 컨트롤에 스타일을 참조 하지 않고도 합니다.

## <a name="global-stylesapplicationmd"></a>[글로벌 스타일](application.md)

스타일 사용할 수 있습니다 전체적으로 응용 프로그램을 추가 하 여 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)합니다. 이를 페이지 또는 컨트롤에서 스타일의 중복을 방지할 수 있습니다.

## <a name="style-inheritanceinheritancemd"></a>[스타일 상속](inheritance.md)

스타일을 중복을 줄이고 다시 사용할 수 있도록 다른 스타일에서 상속할 수 있습니다.

## <a name="dynamic-stylesdynamicmd"></a>[동적 스타일](dynamic.md)

스타일 속성 변경에 응답 하지 않으며 응용 프로그램의 기간 동안 변경 되지 않습니다. 그러나 응용 프로그램 동적 리소스를 사용 하 여 런타임에 동적으로 스타일 변경 내용에 응답할 수 있습니다.

## <a name="device-stylesdevicemd"></a>[장치 스타일](device.md)

Xamarin.Forms는 6 개 포함 되어 있습니다 *동적* 스타일 이라고 *장치* 스타일에 [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) 클래스입니다. 에 모든 6 가지 스타일을 적용할 수 있습니다 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스에만 있습니다.
