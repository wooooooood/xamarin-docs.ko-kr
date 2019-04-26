---
title: XAML 태그 확장
description: 이 문서에서는 Xamarin.Forms XAML 태그 확장을 사용하여 일반적인 텍스트 문자열 이외의 소스에서 요소 특성을 설정할 수 있도록 하여 XAML의 기능과 유연성을 확장하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: cfdd639672f7fa624c7c8e30f17fbfc9dad403af
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075483"
---
# <a name="xaml-markup-extensions"></a>XAML 태그 확장

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)

XAML 태그 확장은 일반적인 텍스트 문자열 이외의 소스에서 요소 특성을 설정할 수 있도록 하여 XAML의 기능과 유연성 확장을 돕습니다.

예를 들어, 다음과 같이 `BoxView`의 `Color` 속성을 설정합니다.

```xaml
<BoxView Color="Blue" />
```

또는 다음과 같이 16 진수 RGB 색상 값으로 설정할 수 있습니다.

```xaml
<BoxView Color="#FF0080" />
```

두 경우 모두, `Color` 특성으로 설정된 텍스트 문자열은 [ `ColorTypeConverter` ](xref:Xamarin.Forms.ColorTypeConverter) 클래스에 의해 `Color` 값으로 변환됩니다.

`Color` 특성을 리소스 사전에 저장 된 값, 또는 사용자가 생성한 클래스의 정적 속성의 값, 또는 페이지에 있는 다른 요소의 `Color` 유형, 또는 색조, 채도 및 명도 값으로 구분하여 설정하는 것을 선호할 수 있습니다.

이러한 모든 옵션은 XAML 태그 확장을 사용하여 수행할 수 있습니다.  그러나 구 "태그 확장" 하다 고 걱정할: XAML 태그 확장은 *되지* XML에 대 한 확장입니다. XAML 태그 확장을 사용하더라도 XAML은 항상 유효한 XML입니다.

태그 확장은 실제로 요소 특성을 표현하는 다른 방식입니다. XAML 태그 확장은 일반적으로 다음과 같이 중괄호로 묶인 특성 설정으로 식별할 수 있습니다.

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

중괄호 안에 있는 모든 특성 설정은 *항상* XAML 태그 확장입니다. 그러나 XAML 태그 확장은 중괄호를 사용하지 않고도 참조할 수 있습니다.

이 문서는 다음 두 부분으로 구분됩니다.

## <a name="consuming-xaml-markup-extensionsconsumingmd"></a>[XAML 태그 확장 사용](consuming.md)  

Xamarin.Forms에 정의된 XAML 태그 확장을 사용합니다.

## <a name="creating-xaml-markup-extensionscreatingmd"></a>[XAML 태그 확장 만들기](creating.md)

사용자 고유의 사용자 지정 XAML 태그 확장을 작성합니다.



## <a name="related-links"></a>관련 링크

- [태그 확장 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Xamarin.Forms 책에서 XAML 태그 확장 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [동적 스타일](~/xamarin-forms/user-interface/styles/dynamic.md)
- [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
