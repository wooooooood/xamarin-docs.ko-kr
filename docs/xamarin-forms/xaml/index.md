---
title: XAML(eXtensible Application Markup Language)
description: XAML은 사용자 인터페이스를 정의하는 데 사용하는 선언적 마크업 언어입니다. XAML 구문을 사용한 XML 파일에서 사용자 인터페이스를 정의하고 런타임 동작은 비하인드 코드 파일에 정의합니다.
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

_XAML은 사용자 인터페이스를 정의 하는 데 사용할 수 있는 선언적 태그 언어입니다. 사용자 인터페이스를 별도 코드 숨김 파일에서 런타임 동작이 정의 되는 동안 XAML 구문을 사용 하 여 XML 파일에 정의 됩니다._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Evolve 2016: XAML 마스터 되기**

> [!NOTE]
> [XAML Standard 미리보기](standard/index.md) 사용해보기

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[XAML 기본 사항](xaml-basics/index.md)

XAML은 코드가 아닌 태그를 사용 하 여 Xamarin.Forms 응용 프로그램에서 사용자 인터페이스를 정의 하는 개발자를 수 있습니다. XAML Xamarin.Forms 프로그램에서 필요 하지 않습니다 하지만 화할, 되 고이 시각적으로 일관적이 고 해당 하는 코드 보다 더 간결 합니다. XAML은 인기 있는 모델-뷰-ViewModel (MVVM) 응용 프로그램 아키텍처를 사용 하는 데 특히 적합 합니다: XAML XAML 기반 데이터 바인딩을 통해 ViewModel 코드에 연결 된 뷰를 정의 합니다.

## <a name="xaml-compilationxamlcmd"></a>[XAML 컴파일](xamlc.md)

필요한 경우 XAML 컴파일러(XAMLC)를 사용하여 XAML을 중간 언어(IL)로 바로 컴파일할 수 있습니다. 이 문서에서는 XAMLC와 그 혜택을 사용하는 방법을 설명합니다.

## <a name="xaml-previewerxaml-previewermd"></a>[XAML 미리 보기](xaml-previewer.md)

[XAML 미리 보기](~/xamarin-forms/xaml/xaml-previewer.md)를 통해 XAML 마크업과 페이지를 나란히 미리 볼 수 있어 입력된 코드가 사용자 인터페이스에 어떻게 렌더링되는지 보여줍니다.

## <a name="xaml-namespacesnamespacesmd"></a>[XAML 네임스페이스](namespaces.md)

XAML에서 `xmlns`이라는 XML 특성은 네임 스페이스를 선언하는 데 사용합니다. 이 문서는 XAML 네임 스페이스 구문을 소개하고 형식을 사용하기 위해 어떻게 XAML 네임스페이스를 선언하는지 보여줍니다.

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[XAML 마크업 확장](markup-extensions/index.md)

XAML은 마크업 확장을 통해 간단한 문자열 이외에 값 또는 개체를 특성(Attribute)에 설정할 수 있습니다. 여기에는 상수, 정적 속성 및 필드, 리소스 사전 및 데이터 바인딩을 포함합니다.

## <a name="field-modifiersfield-modifiersmd"></a>[필드 한정자](field-modifiers.md)

`x:FieldModifier` 네임 스페이스 특성을 통해 명명된 XAML 요소에서 생성 된 필드에 대한 액세스 수준을 지정할 수 있습니다.

## <a name="passing-argumentspassing-argumentsmd"></a>[인수 전달](passing-arguments.md)

XAML은 기본이 아닌 생성자 혹은 팩토리 메서드로 인수를 전달할 수 있습니다. 이 문서에서는 XAML 특성을 통해 생성자에 인수를 전달하고, 팩터리 메서드를 호출하며, 제너릭 인수의 타입을 정하는 방법을 알아봅니다.

## <a name="bindable-propertiesbindable-propertiesmd"></a>[Bindable Properties](bindable-properties.md)

Xamarin.Forms에서는 공용 언어 런타임 (CLR) 속성의 기능을 Bindable Property를 통해 확장할 수 있습니다. Bindable Property는 Xamarin.Forms의 속성 체계에 속성의 값이 추적되는 특수 형식입니다. 이 문서에서는 Bindable Properties에 대한 기본 소개와 어떻게 만들고 사용하는지에 대해 알아봅니다.

## <a name="attached-propertiesattached-propertiesmd"></a>[Attached Properties](attached-properties.md)

연결된 된 속성은 다른 개체에 연결 없이 하나의 클래스로 정의되고, XAML에서 점으로 구별되는 클래스와 속성 이름을 포함하는 특성으로 인식 가능한 특수한 유형의 바인딩 가능한 속성입니다. 이 문서에서는 연결된 속성에 대해 소개하고 연결된 속성 생성및 사용 방법을 설명합니다.

## <a name="resource-dictionariesresource-dictionariesmd"></a>[Resource Dictionaries](resource-dictionaries.md)

XAML 리소스는 한 번 이상 사용할 수 있는 개체 정의의 모음입니다. [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)는 리소스를 한 곳에서 정의하고 Xamarin.Forms 응용 프로그램 전체에서 다시 사용할 수 있도록 해줍니다. 이 문서에서는 `ResourceDictionary`를 어떻게 만들고 사용하는 지와 `ResourceDictionary`를 다른 것과 합치는 방법을 알아봅니다.
