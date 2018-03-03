---
title: XAML eXtensible Application Markup Language)
description: "XAML은 사용자 인터페이스를 정의 하는 데 사용할 수 있는 선언적 태그 언어입니다. 사용자 인터페이스는 별도 코드 숨김 파일에서 런타임 동작이 정의 되는 동안 XAML 구문을 사용 하 여 XML 파일에 정의 됩니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/24/2016
ms.openlocfilehash: a6e31fa9da7a5764d9a7fd04aa73d7d246143384
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="extensible-application-markup-language-xaml"></a>XAML eXtensible Application Markup Language)

_XAML은 사용자 인터페이스를 정의 하는 데 사용할 수 있는 선언적 태그 언어입니다. 사용자 인터페이스는 별도 코드 숨김 파일에서 런타임 동작이 정의 되는 동안 XAML 구문을 사용 하 여 XML 파일에 정의 됩니다._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**XAML 마스터 되 고 2016 개선:**

> [!NOTE]
> 사용을 [XAML 표준 미리 보기](standard/index.md)

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[XAML 기본 사항](xaml-basics/index.md)

XAML 코드 보다는 태그를 사용 하 여 Xamarin.Forms 응용 프로그램에서 사용자 인터페이스를 정의 하는 개발자를 있습니다. XAML Xamarin.Forms 프로그램에 필요 하지 않지만 높은, 되어는 대개 더 시각적으로 일관적이 고 해당 하는 코드 보다 더 간결 합니다. XAML은 인기 있는 모델-뷰-MVVM () 응용 프로그램 아키텍처와 함께 사용 하는 데 특히 적합: XAML XAML 기반 데이터 바인딩을 통해 ViewModel 코드에 연결 된 뷰를 정의 합니다.

## <a name="xaml-compilationxamlcmd"></a>[XAML 컴파일](xamlc.md)

필요한 경우 XAML 컴파일러(XAMLC)를 사용하여 XAML을 중간 언어(IL)로 바로 컴파일할 수 있습니다. 이 문서에서는 XAMLC, 및 이점을 사용 하는 방법에 설명 합니다.

## <a name="xaml-previewerxaml-previewermd"></a>[XAML 미리 보기](xaml-previewer.md)

[XAML 미리 보기](~/xamarin-forms/xaml/xaml-previewer.md) 에서 발표 되었습니다 Xamarin 발전 2016은 알파 채널에서 테스트할 수 있습니다.

## <a name="xaml-namespacesnamespacesmd"></a>[XAML 네임스페이스](namespaces.md)

XAML에서 사용 하 여 `xmlns` 네임 스페이스 선언에 대 한 XML 특성입니다. 이 문서는 XAML 네임 스페이스 구문을 소개 하 고 형식 액세스 하는 XAML 네임 스페이스를 선언 하는 방법을 보여 줍니다.

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[XAML 태그 확장](markup-extensions/index.md)

XAML 특성 값 이나 간단한 문자열로 표현 될 수 있는 다음 개체를 설정 하기 위한 태그 확장에 포함 되어 있습니다. 여기에 참조 하는 상수, 정적 속성 및 필드, 리소스 및 데이터 바인딩을 포함 합니다.

## <a name="passing-argumentspassing-argumentsmd"></a>[인수 전달](passing-arguments.md)

XAML은 기본값이 아닌 생성자 또는 팩터리 메서드에 인수를 전달 데 사용할 수 있습니다. 이 문서는 팩터리 메서드를 호출 하 고 제네릭 인수의 형식을 지정 하는 생성자에 인수를 전달 하는 데 사용할 수 있는 XAML 특성 사용 하 여 보여줍니다.

## <a name="bindable-propertiesbindable-propertiesmd"></a>[바인딩 가능한 속성](bindable-properties.md)

Xamarin.forms에 공용 언어 런타임 (CLR) 속성의 기능을 바인딩할 수 있는 속성에 의해 확장 됩니다. 바인딩 가능한 속성은 속성의 값 Xamarin.Forms 속성 시스템에 의해 추적 되는 위치는 특수 한 유형의 속성. 이 문서에서는 바인딩 가능 속성을 소개 하 고 만들고 하 게 사용 하는 방법을 보여 줍니다.

## <a name="attached-propertiesattached-propertiesmd"></a>[연결 된 속성](attached-properties.md)

연결된 된 속성은 특수 한 유형의 바인딩 가능한 속성을 하나의 클래스에 정의 되었지만 다른 개체에 연결 하는 클래스를 포함 하는 특성으로 XAML에서 인식할 수 있는 및 마침표로 구분 되는 속성 이름입니다. 이 문서에서는 연결 된 속성을 소개 하 고 만들고 하 게 사용 하는 방법을 보여 줍니다.

## <a name="resource-dictionariesresource-dictionariesmd"></a>[리소스 사전](resource-dictionaries.md)

XAML 리소스는 한 번만 사용할 수 있는 개체의 정의입니다. A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 하면 리소스를 단일 위치에서 정의 하 고 Xamarin.Forms 응용 프로그램에 걸쳐 다시 사용할 수 있습니다. 이 문서에서는 만들고 사용 하는 방법을 `ResourceDictionary`을 하나로 병합 하는 방법과 `ResourceDictionary` 다른 파티션으로 합니다.
