---
title: XAML(eXtensible Application Markup Language)
description: XAML은 사용자 인터페이스를 정의하는 데 사용할 수 있는 선언적 태그 언어입니다. 사용자 인터페이스를 별도 코드 숨김 파일에서 런타임 동작을 정의하는 동안 XAML 구문을 사용하여 XML 파일로 정의합니다.
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: adebd5fe7e05d6698a7d69cef56a1d4035b6d8e7
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563077"
---
# <a name="extensible-application-markup-language-xaml"></a>XAML(eXtensible Application Markup Language)

_XAML은 사용자 인터페이스를 정의하는 데 사용할 수 있는 선언적 태그 언어입니다. 사용자 인터페이스를 별도 코드 숨김 파일에서 런타임 동작을 정의하는 동안 XAML 구문을 사용하여 XML 파일로 정의합니다._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Evolve 2016: XAML 고수 되기**

> [!NOTE]
> [XAML 표준 미리 보기](standard/index.md)를 살펴보세요.

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[XAML 기본 사항](xaml-basics/index.md)

XAML은 개발자가 코드가 아닌 태그를 사용하여 Xamarin.Forms 응용 프로그램에서 사용자 인터페이스를 정의할 수 있도록 해줍니다. XAML은 Xamarin.Forms 프로그램에서 반드시 필요한 것은 아니지만, 도구화되어 있고 시각적으로 일관적이며 동일한 코드보다 더 간결합니다. XAML은 인기 있는 MVVM (Model-View-ViewModel) 응용 프로그램 아키텍처를 사용하는 데 특히 적합합니다. XAML은 XAML 기반 데이터 바인딩을 통해 ViewModel 코드에 연결된 View를 정의합니다.

## <a name="xaml-compilationxamlcmd"></a>[XAML 컴파일](xamlc.md)

XAML 컴파일러(XAMLC)를 사용하여 선택적으로 XAML을 중간 언어(IL)로 바로 컴파일할 수 있습니다. 이 문서에서는 XAMLC 사용 방법 및 장점을 설명합니다.

## <a name="xaml-previewerxaml-previewermd"></a>[XAML 미리 보기](xaml-previewer.md)

[XAML 미리 보기](~/xamarin-forms/xaml/xaml-previewer.md)는 입력하는 사용자 인터페이스를 볼 수 있도록 해주면서 XAML 태그로 페이지의 실시간 미리 보기를 단계적으로 표시합니다.

## <a name="xaml-namespacesnamespacesmd"></a>[XAML 네임스페이스](namespaces.md)

XAML은 네임 스페이스에 대한 XML 특성으로 `xmlns`를 사용합니다. 이 문서는 XAML 네임 스페이스 구문을 소개하고 형식에 접근하는 XAML 네임 스페이스를 선언하는 방법을 설명합니다.

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[XAML 태그 확장](markup-extensions/index.md)

XAML은 단순 문자열로 표현을 넘어 값 또는 개체의 설정 특성에 대한 태그 확장을 포함합니다. 참조하는 상수, 정적 속성 및 필드, 리소스 사전 및 데이터 바인딩을 포함합니다.

## <a name="field-modifiersfield-modifiersmd"></a>[필드 한정자](field-modifiers.md)

`x:FieldModifier` 네임 스페이스 특성은 명명된 XAML 요소에 대해 생성된 필드의 접근 수준을 지정합니다.

## <a name="passing-argumentspassing-argumentsmd"></a>[인수 전달](passing-arguments.md)

XAML은 팩토리 메서드 또는 비 기본 생성자에 인수를 전달하기 위해 사용할 수 있습니다. 이 문서에서는 생성자, 팩터리 메서드 호출, 제네릭 인수의 형식을 지정하는 인수를 전달할 수 있는 XAML 특성을 사용하는 방법을 보여 줍니다.

## <a name="bindable-propertiesbindable-propertiesmd"></a>[바인딩 가능한 속성](bindable-properties.md)

Xamarin.Forms에서 공용 언어 런타임(CLR) 속성의 기능은 바인딩 가능한 속성으로 인해 확장 됩니다. 바인딩 가능한 속성은 속성의 값이 Xamarin.Forms 속성 시스템에 의해 추적되는 속성의 특수 형식입니다. 이 문서에서는 바인딩 가능한 속성에 대해 소개하고 속성의 생성 및 사용 방법을 설명합니다.

## <a name="attached-propertiesattached-propertiesmd"></a>[연결된 속성](attached-properties.md)

연결된 된 속성은 다른 개체에 연결 없이 하나의 클래스로 정의되고, XAML에서 점으로 구별되는 클래스와 속성 이름을 포함하는 특성으로 인식 가능한 특수한 유형의 바인딩 가능한 속성입니다. 이 문서에서는 연결된 속성에 대해 소개하고 연결된 속성 생성및 사용 방법을 설명합니다.

## <a name="resource-dictionariesresource-dictionariesmd"></a>[리소스 사전](resource-dictionaries.md)

XAML 리소스는 한 번 이상 사용할 수 있는 개체의 정의입니다. [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)는 리소스를 한 곳에서 정의하고 Xamarin.Forms 응용 프로그램 전체에서 다시 사용할 수 있도록 해줍니다. 이 문서는 `ResourceDictionary`를 생성하고 사용하는 방법 및 하나의 `ResourceDictionary`를 다른 곳에 병합하는 방법을 설명합니다.
