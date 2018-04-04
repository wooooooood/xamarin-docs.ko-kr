---
title: XAML 태그 확장
description: 원본 특성이 설정 되는 XAML에서 범위 확장
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: b81bc4b31edd1d8b8f5f43f97885c38e889dd32c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-markup-extensions"></a>XAML 태그 확장

XAML 태그 확장 리터럴 텍스트 문자열 이외의 원본에서 요소 특성을 허용 하 여 작업 효율성과 XAML의 유연성 증대를 확장할 수 있습니다.

예를 들어 일반적으로 설정 하면는 `Color` 속성 `BoxView` 다음과 같이 합니다.

```xaml
<BoxView Color="Blue" />
```

또는 16 진 RGB 색 값으로 설정할 수 있습니다.

```xaml
<BoxView Color="#FF0080" />
```

두 경우 모두에서 텍스트 문자열을로 설정 된 `Color` 특성으로 변환 됩니다는 `Color` 값을 [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/) 클래스입니다.

하려고 할 때 대신 설정는 `Color` 리소스 사전에 저장 된 값 또는 만든 하는 클래스의 정적 속성의 값 또는 형식의 속성을 특성 `Color` 페이지에서 다른 요소에서 생성 된 또는 색상, 채도 및 명도 값을 구분 합니다.

이러한 모든 옵션은 XAML 태그 확장을 사용 하 여 합니다. 구를 잊지 마십시오 "태그 확장" 두려워: XAML 태그 확장은 *하지* XML에 대 한 확장입니다. XAML 태그 확장 된 경우에이 XAML은 항상 xml이 유효한 XML입니다. 

태그 확장은 실제로 다른 방식으로 요소의 특성을 표현할 수 있습니다. XAML 태그 확장은 중괄호로 묶인 특성 설정에 의해 일반적으로 식별 됩니다.

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

중괄호 안에 있는 모든 특성 설정은 *항상* XAML 태그 확장 합니다. 그러나 볼 수 있듯이 XAML 태그 확장 중괄호를 사용 하지 않고 참조할 수도 있습니다.

이 문서는 두 부분으로 이루어져 있습니다.

## <a name="consuming-xaml-markup-extensionsconsumingmd"></a>[XAML 태그 확장 사용](consuming.md)  

Xamarin.Forms에 정의 된 XAML 태그 확장을 사용 합니다.

## <a name="creating-xaml-markup-extensionscreatingmd"></a>[XAML 태그 확장 만들기](creating.md) 

사용자 고유의 사용자 지정 XAML 태그 확장을 씁니다.



## <a name="related-links"></a>관련 링크

- [태그 확장 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Xamarin.Forms 책에서 XAML 태그 확장 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [동적 스타일](~/xamarin-forms/user-interface/styles/dynamic.md)
- [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
