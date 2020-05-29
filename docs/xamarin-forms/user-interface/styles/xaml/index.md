---
title: Xamarin.FormsXAML 스타일을 사용 하 여 앱 스타일 지정
description: 이 가이드에서는 Xamarin.Forms XAML 스타일을 사용 하 여 응용 프로그램의 모양을 사용자 지정 하는 방법을 설명 합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 72effe15d3456b5a48cbf5d09e889600134ac686
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138803"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>Xamarin.FormsXAML 스타일을 사용 하 여 앱 스타일 지정

## <a name="introduction"></a>[소개](introduction.md)

Xamarin.Forms응용 프로그램에는 동일한 모양의 여러 컨트롤이 포함 되어 있는 경우가 많습니다. 각 개별 컨트롤의 모양을 반복적으로 설정 하면 오류가 발생할 수 있습니다. 대신 컨트롤 형식에 사용할 수 있는 속성을 그룹화 하 고 설정 하 여 컨트롤 모양을 사용자 지정 하는 스타일을 만들 수 있습니다.

## <a name="explicit-styles"></a>[명시적 스타일](explicit.md)

*명시적* 스타일은 해당 속성을 설정 하 여 컨트롤에 선택적으로 적용 되는 스타일입니다 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) .

## <a name="implicit-styles"></a>[암시적 스타일](implicit.md)

*암시적* 스타일은 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 각 컨트롤이 스타일을 참조할 필요 없이 동일한의 모든 컨트롤에서 사용 하는 스타일입니다.

## <a name="global-styles"></a>[글로벌 스타일](application.md)

스타일은 응용 프로그램의에 추가 하 여 전역적으로 사용할 수 있습니다 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . 이렇게 하면 여러 페이지나 컨트롤에서 스타일의 중복을 방지할 수 있습니다.

## <a name="style-inheritance"></a>[스타일 상속](inheritance.md)

스타일은 다른 스타일에서 상속 하 여 중복을 줄이고 재사용을 가능 하 게 합니다.

## <a name="dynamic-styles"></a>[동적 스타일](dynamic.md)

스타일은 속성 변경에 응답 하지 않으며 응용 프로그램 기간 동안 변경 되지 않은 상태로 유지 됩니다. 그러나 응용 프로그램은 동적 리소스를 사용 하 여 런타임에 동적으로 스타일 변경에 응답할 수 있습니다.

## <a name="device-styles"></a>[디바이스 스타일](device.md)

Xamarin.Forms클래스에는 *장치* 스타일 이라고 하는 여섯 가지 *동적* 스타일을 포함 [`Devices.Styles`](xref:Xamarin.Forms.Device.Styles) 합니다. 6 가지 스타일은 모두 인스턴스에만 적용할 수 있습니다 [`Label`](xref:Xamarin.Forms.Label) .

## <a name="style-classes"></a>[스타일 클래스](style-class.md)

Xamarin.Forms스타일 클래스를 사용 하면 스타일 상속을 사용 하지 않고 컨트롤에 여러 스타일을 적용할 수 있습니다.
