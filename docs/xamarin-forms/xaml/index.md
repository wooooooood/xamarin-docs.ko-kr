---
title: XAML(eXtensible Application Markup Language)
description: XAML은 사용자 인터페이스를 정의하는 데 사용하는 선언적 태그 언어입니다. XAML 구문을 사용한 XML 파일에서 사용자 인터페이스를 정의하고 런타임 동작은 코드 숨김 파일에 정의합니다.
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2019
ms.openlocfilehash: fa93cb86867cb8539fb7ce4db45ad4751bfe6e04
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58854226"
---
# <a name="extensible-application-markup-language-xaml"></a>XAML(eXtensible Application Markup Language)

_XAML은 사용자 인터페이스를 정의하는 데 사용하는 선언적 태그 언어입니다. XAML 구문을 사용한 XML 파일에서 사용자 인터페이스를 정의하고 런타임 동작은 코드 숨김 파일에 정의합니다._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Evolve 2016: XAML 마스터 되 고**

> [!NOTE]
> [XAML Standard 미리 보기](standard/index.md) 사용해 보기

<a name="xaml" />

## [<a name="xaml-basics"></a>XAML 기본 사항](xaml-basics/index.md)

XAML을 통해 개발자는 Xamarin.Forms 응용 프로그램에서 사용자 인터페이스를 코드보다 태그를 이용해 정의할 수 있습니다. XAML은 Xamarin.Forms 프로그램에서 반드시 필요한 요소는 아니지만 도구를 사용할 수 있게 해주고, 같은 기능을 하는 코드보다 시각적으로 더욱 일관되고 간결합니다. XAML은 많이 사용되는 Model-View-ViewModel (MVVM) 아키텍처를 사용하는 데 특히 적합합니다.  XAML은 XAML 기반의 데이터 바인딩을 통해 ViewModel 코드에 연결된 뷰를 정의할 수 있습니다.

## [<a name="xaml-compilation"></a>XAML 컴파일](xamlc.md)

필요한 경우 XAML 컴파일러(XAMLC)를 사용하여 XAML을 중간 언어(IL)로 바로 컴파일할 수 있습니다. 이 문서에서는 XAMLC와 그 혜택을 사용하는 방법을 설명합니다.

## [<a name="xaml-previewer"></a>XAML 미리 보기](xaml-previewer/index.md)

[XAML 미리 보기](~/xamarin-forms/xaml/xaml-previewer/index.md)는 입력하는 사용자 인터페이스를 볼 수 있도록 해주면서 XAML 태그로 페이지의 실시간 미리 보기를 단계적으로 표시합니다.

## [<a name="xaml-namespaces"></a>XAML 네임스페이스](namespaces.md)

XAML에서 `xmlns`라는 XML 특성은 네임스페이스를 선언하는 데 사용합니다. 이 문서는 XAML 네임스페이스 구문을 소개하고 형식을 사용하기 위해 XAML 네임스페이스를 선언하는 방법을 보여줍니다.

## [<a name="xaml-custom-namespace-schemas"></a>XAML Namespace 사용자 지정 스키마](custom-namespace-schemas.md)

XAML 네임 스페이스 사용자 지정 스키마를 정의할 수 있습니다는 `XmlnsDefinitionAttribute` 사용자 지정 URL 및 하나 이상의 CLR 네임 스페이스 간의 매핑을 지정 하는 클래스입니다. 사용자 지정 네임 스페이스 스키마 XAML 네임 스페이스 선언에서 사용할 수 있습니다.

## [<a name="xaml-namespace-recommended-prefixes"></a>XAML Namespace 권장 접두사](custom-prefix.md)

`XmlnsPrefixAttribute` XAML 사용에 대 한 XAML 네임 스페이스와 연결할 권장된 접두사를 지정 하려면 컨트롤 작성자 클래스를 사용할 수 있습니다.

## [<a name="xaml-markup-extensions"></a>XAML 태그 확장](markup-extensions/index.md)

XAML은 단순 문자열로 표현을 넘어 값 또는 개체의 설정 특성에 대한 태그 확장을 포함합니다. 참조하는 상수, 정적 속성 및 필드, 리소스 사전 및 데이터 바인딩을 포함합니다.

## [<a name="field-modifiers"></a>필드 한정자](field-modifiers.md)

`x:FieldModifier` 네임스페이스 특성을 통해 명명된 XAML 요소에서 생성된 필드에 대한 액세스 수준을 지정할 수 있습니다.

## [<a name="passing-arguments"></a>인수 전달](passing-arguments.md)

XAML을 사용하여 기본이 아닌 생성자 혹은 팩토리 메서드에 인수를 전달할 수 있습니다. 이 문서에서는 XAML 특성을 통해 생성자에 인수를 전달하고, 팩터리 메서드를 호출하며, 제네릭 인수의 형식을 지정하는 방법을 알아봅니다.

## [<a name="bindable-properties"></a>바인딩 가능한 속성](bindable-properties.md)

Xamarin.Forms에서는 공용 언어 런타임(CLR) 속성의 기능을 Bindable Property를 통해 확장할 수 있습니다. Bindable Property는 Xamarin.Forms의 속성 체계에 의해 속성의 값이 추적되는 특수 형식입니다. 이 문서에서는 Bindable Properties에 대한 기본 소개와 어떻게 만들고 사용하는지에 대해 알아봅니다.

## [<a name="attached-properties"></a>연결된 속성](attached-properties.md)

XAML은 태그 확장을 통해 간단한 문자열 이외에 값 또는 개체를 특성(Attribute)에 설정할 수 있습니다. 여기에는 상수, 정적 속성 및 필드, 리소스 사전 및 데이터 바인딩이 포함됩니다. 이 문서에서는 Attached Properties에 대한 소개와 어떻게 만들고 사용하는지에 대해 알아봅니다.

## [<a name="resource-dictionaries"></a>리소스 사전](resource-dictionaries.md)

AML 리소스는 두 번 이상 사용할 수 있는 개체의 정의들의 모음입니다. [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)는 리소스를 한 곳에서 정의하고 Xamarin.Forms 응용 프로그램 전체에서 다시 사용할 수 있도록 해줍니다. 이 문서에서는 `ResourceDictionary`를 어떻게 만들고 사용하는지와 두 `ResourceDictionary`를 병합하는 방법을 알아봅니다.

## [<a name="loading-xaml-at-runtime"></a>런타임 시 XAML 로드](runtime-load.md)

XAML 로드 되 고 사용 하 여 런타임에 구문 분석 합니다 [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 확장 메서드.
