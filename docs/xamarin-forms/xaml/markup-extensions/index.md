---
title: ''
description: 이 문서에서는 Xamarin.Forms xaml 태그 확장을 사용 하 여 리터럴 텍스트 문자열이 아닌 소스에서 요소 특성을 설정할 수 있도록 하 여 xaml의 기능과 유연성을 확장 하는 방법을 설명 합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 568cffc335f28b1a47f3278ad061d851ebef84b6
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130392"
---
# <a name="xaml-markup-extensions"></a>XAML 태그 확장

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML 태그 확장은 리터럴 텍스트 문자열이 아닌 소스에서 요소 특성을 설정할 수 있도록 하 여 XAML의 기능과 유연성을 확장 하는 데 도움이 됩니다.

예를 들어, 일반적으로 `Color` 다음과 같이의 속성을 설정 합니다 `BoxView` .

```xaml
<BoxView Color="Blue" />
```

또는 16 진수 RGB 색 값으로 설정할 수 있습니다.

```xaml
<BoxView Color="#FF0080" />
```

두 경우 모두 특성으로 설정 된 텍스트 문자열은 `Color` `Color` 클래스에 의해 값으로 변환 됩니다 [`ColorTypeConverter`](xref:Xamarin.Forms.ColorTypeConverter) .

대신 `Color` 리소스 사전에 저장 된 값 이나 사용자가 만든 클래스의 정적 속성 값 또는 페이지의 다른 요소 형식 속성에서 특성을 설정 `Color` 하거나 별도의 색상, 채도 및 명도 값으로 생성 된 특성을 설정 하는 것이 좋습니다.

이러한 모든 옵션은 XAML 태그 확장을 사용 하 여 가능 합니다. 그러나 "태그 확장" 이라는 문구를 사용 하지 않는 것이 좋습니다. XAML 태그 확장은 XML로 확장 *되지 않습니다* . Xaml 태그 확장을 사용 하는 경우에도 XAML은 항상 올바른 XML입니다.

태그 확장은 단순히 요소의 특성을 표현 하는 다른 방법입니다. XAML 태그 확장은 일반적으로 중괄호 안에 있는 특성 설정으로 식별할 수 있습니다.

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

중괄호 안에 있는 모든 특성 설정은 *항상* XAML 태그 확장입니다. 그러나 표시 된 것 처럼 중괄호를 사용 하지 않고도 XAML 태그 확장을 참조할 수도 있습니다.

이 문서는 다음 두 부분으로 구분 됩니다.

## <a name="consuming-xaml-markup-extensions"></a>[XAML 태그 확장 사용](consuming.md)  

에 정의 된 XAML 태그 확장을 사용 Xamarin.Forms 합니다.

## <a name="creating-xaml-markup-extensions"></a>[XAML 태그 확장 만들기](creating.md)

사용자 고유의 사용자 지정 XAML 태그 확장을 작성 합니다.

## <a name="related-links"></a>관련 링크

- [태그 확장 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [XAML 태그 확장 책의 장 Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [동적 스타일](~/xamarin-forms/user-interface/styles/dynamic.md)
- [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
