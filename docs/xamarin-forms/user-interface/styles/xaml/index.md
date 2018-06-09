---
title: XAML 스타일을 사용 하 여 Xamarin.Forms 앱 스타일 지정
description: 이 가이드에는 XAML 스타일을 사용 하 여 Xamarin.Forms 응용 프로그램의 모양을 사용자 지정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: f439e3ba888b67ac1752eae95149adcf55055943
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245874"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>XAML 스타일을 사용 하 여 Xamarin.Forms 앱 스타일 지정

## <a name="introductionintroductionmd"></a>[소개](introduction.md)

Xamarin.Forms 응용 프로그램에 동일한 모양을 갖도록 하는 여러 컨트롤 포함 되는 경우가 많습니다. 반복 될 수 있습니다 각 개별 컨트롤의 모양을 설정 하 고 오류가 발생 하기 쉽습니다. 대신, 스타일 여 만들 수 있습니다 컨트롤 모양을 사용자 지정 하는 그룹화 및 설정 속성 컨트롤 형식에 사용할 수 있습니다.

## <a name="explicit-stylesexplicitmd"></a>[명시적 스타일](explicit.md)

*명시적* 선택적으로 설정 하 여 컨트롤에 적용 된 스타일은 해당 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성입니다.

## <a name="implicit-stylesimplicitmd"></a>[암시적 스타일](implicit.md)

*암시적* 하나는 동일한 모든 컨트롤에서 사용 되는 스타일은 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), 각 컨트롤에 스타일을 참조 하지 않고도 합니다.

## <a name="global-stylesapplicationmd"></a>[글로벌 스타일](application.md)

스타일 사용할 수 있도록 설정 전체적으로 응용 프로그램을 추가 하 여 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)합니다. 페이지나 컨트롤 스타일의 중복을 방지할 수 있습니다.

## <a name="style-inheritanceinheritancemd"></a>[스타일 상속](inheritance.md)

스타일을 중복을 줄이고 다시 사용할 수 있도록 다른 스타일에서 상속할 수 있습니다.

## <a name="dynamic-stylesdynamicmd"></a>[동적 스타일](dynamic.md)

스타일 속성 변경에 응답 하지 않으며 응용 프로그램의 기간 동안 변경 되지 않습니다. 그러나 응용 프로그램 동적 리소스를 사용 하 여 런타임에 동적으로 스타일 변경 내용에 응답할 수 있습니다.

## <a name="device-stylesdevicemd"></a>[장치 스타일](device.md)

Xamarin.Forms 포함 6 *동적* 라고 스타일 *장치* 스타일에 [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) 클래스입니다. 6 가지 모든 스타일을 적용할 수 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 인스턴스만 있습니다.
