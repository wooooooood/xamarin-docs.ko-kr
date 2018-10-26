---
title: eXtensible Application Markup Language (XAML)
description: XAML은 사용자 인터페이스를 정의 하는 데 사용할 수 있는 선언적 태그 언어입니다. 사용자 인터페이스를 별도 코드 숨김 파일에서 런타임 동작이 정의 되는 동안 XAML 구문을 사용 하 여 XML 파일에 정의 됩니다.
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/18/2018
ms.openlocfilehash: 4045f3a1d31b1c0c8b69e840d3943b6ce258b894
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113588"
---
# <a name="extensible-application-markup-language-xaml"></a>eXtensible Application Markup Language (XAML)

_XAML은 사용자 인터페이스를 정의 하는 데 사용할 수 있는 선언적 태그 언어입니다. 사용자 인터페이스를 별도 코드 숨김 파일에서 런타임 동작이 정의 되는 동안 XAML 구문을 사용 하 여 XML 파일에 정의 됩니다._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Evolve 2016: 쌓 XAML**

> [!NOTE]
> 사용해는 [XAML 표준 미리 보기](standard/index.md)

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[XAML 기본 사항](xaml-basics/index.md)

XAML은 코드가 아닌 태그를 사용 하 여 Xamarin.Forms 응용 프로그램에서 사용자 인터페이스를 정의 하는 개발자를 수 있습니다. XAML Xamarin.Forms 프로그램에서 필요 하지 않습니다 하지만 화할, 되 고이 시각적으로 일관적이 고 해당 하는 코드 보다 더 간결 합니다. XAML은 인기 있는 모델-뷰-ViewModel (MVVM) 응용 프로그램 아키텍처를 사용 하는 데 특히 적합 합니다: XAML XAML 기반 데이터 바인딩을 통해 ViewModel 코드에 연결 된 뷰를 정의 합니다.

## <a name="xaml-compilationxamlcmd"></a>[XAML 컴파일](xamlc.md)

필요한 경우 XAML 컴파일러(XAMLC)를 사용하여 XAML을 중간 언어(IL)로 바로 컴파일할 수 있습니다. 이 문서에서는 XAMLC, 및 그 혜택을 사용 하는 방법을 설명 합니다.

## <a name="xaml-previewerxaml-previewermd"></a>[XAML 미리 보기](xaml-previewer.md)

합니다 [XAML 미리 보기](~/xamarin-forms/xaml/xaml-previewer.md) 를 페이지-side-by-side 입력할 때 렌더링 하 여 사용자 인터페이스를 볼 수 있도록 하 고 XAML 태그로 실시간 미리 보기를 렌더링 합니다.

## <a name="xaml-namespacesnamespacesmd"></a>[XAML 네임스페이스](namespaces.md)

XAML을 사용 하 여 `xmlns` 네임 스페이스 선언에 대 한 XML 특성입니다. 이 문서는 XAML 네임 스페이스 구문을 소개 하 고 형식에 액세스 하는 XAML 네임 스페이스를 선언 하는 방법을 보여 줍니다.

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[XAML 태그 확장](markup-extensions/index.md)

XAML 특성 값 또는 간단한 문자열을 사용 하 여 표현 될 수 있습니다 이상의 개체를 설정 하는 것에 대 한 태그 확장을 포함 합니다. 참조 하는 상수, 정적 속성 및 필드, 리소스 사전 및 데이터 바인딩을 포함 됩니다.

## <a name="field-modifiersfield-modifiersmd"></a>[필드 한정자](field-modifiers.md)

`x:FieldModifier` 네임 스페이스 특성 명명 된 XAML 요소에 대해 생성 된 필드에 대 한 액세스 수준을 지정 합니다.

## <a name="passing-argumentspassing-argumentsmd"></a>[인수 전달](passing-arguments.md)

팩터리 메서드 또는 기본이 아닌 생성자에 인수를 전달 하려면 XAML은 사용할 수 있습니다. 이 문서에서는 생성자, 팩터리 메서드를 호출 하 고 제네릭 인수의 형식을 지정 하 여 인수를 전달할 수 있는 XAML 특성을 사용 하는 방법을 보여 줍니다.

## <a name="bindable-propertiesbindable-propertiesmd"></a>[바인딩 가능한 속성](bindable-properties.md)

Xamarin.Forms에서 공용 언어 런타임 (CLR) 속성의 기능 바인딩 가능한 속성으로 확장 됩니다. 바인딩 가능한 속성을 속성의 값 Xamarin.Forms 속성 시스템에서 추적 되는 속성의 특수 형식입니다. 이 문서에서는 바인딩 가능한 속성에 대해 소개 하 고 만들고이 사용 하는 방법을 보여 줍니다.

## <a name="attached-propertiesattached-propertiesmd"></a>[연결된 속성](attached-properties.md)

연결된 된 속성은 특수 한 유형의 바인딩 가능한 속성을 하나의 클래스에 정의 되었지만 다른 개체에 연결 하 고 클래스를 포함 하는 특성으로 XAML에서 인식할 수 있는 속성 이름은 마침표로 구분 합니다. 이 문서에서는 연결 된 속성에 대해 소개 하 고 만들고이 사용 하는 방법을 보여 줍니다.

## <a name="resource-dictionariesresource-dictionariesmd"></a>[리소스 사전](resource-dictionaries.md)

XAML 리소스는 두 번 이상 사용할 수 있는 개체의 정의입니다. A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 리소스를 한 곳에서 정의 하 고 Xamarin.Forms 응용 프로그램 전체에서 다시 사용할 수 있습니다. 이 문서에 만들고 사용 하는 방법을 보여 줍니다.는 `ResourceDictionary`를 하나로 병합 하는 방법과 `ResourceDictionary` 다른 합니다.
