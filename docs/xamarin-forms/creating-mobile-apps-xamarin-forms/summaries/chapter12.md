---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 12. Styles''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 22f7272751c30bed350fe7aada8d5dacf0acc809
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136788"
---
# <a name="summary-of-chapter-12-styles"></a>요약 - 12장. 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)

Xamarin.Forms에서 스타일을 사용하면 여러 뷰가 속성 설정 컬렉션을 공유할 수 있습니다. 이렇게 하면 태그가 줄어들고 일관적인 시각적 테마를 유지할 수 있습니다.

스타일은 거의 항상 태그에서 정의되고 사용됩니다. [`Style`](xref:Xamarin.Forms.Style) 형식의 개체는 리소스 사전에서 인스턴스화된 후 `StaticResource` 또는 `DynamicResource` 태그 확장을 사용하여 시각적 요소의 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 속성으로 설정됩니다.

## <a name="the-basic-style"></a>기본 스타일

`Style`을 사용하려면 해당 [`TargetType`](xref:Xamarin.Forms.Style.TargetType)이 적용되는 시각적 개체 형식으로 설정되어야 합니다. `Style`이 리소스 사전에서 인스턴스화된 경우(일반적인 경우) `x:Key` 특성도 필요합니다.

`Style`에는 [`Setter`](xref:Xamarin.Forms.Setter) 개체의 컬렉션인 [`Setters`](xref:Xamarin.Forms.Style.Setters) 형식의 콘텐츠 속성이 있습니다. 각 `Setter`는 [`Property`](xref:Xamarin.Forms.Setter.Property)를 [`Value`](xref:Xamarin.Forms.Setter.Value)와 연결합니다.

XAML에서 `Property` 설정은 CLR 속성의 이름(예: `Button`의 `Text` 속성)이지만 스타일이 지정된 속성은 바인딩 가능한 속성으로 지원되어야 합니다. 또한 속성은 `TargetType` 설정으로 표시된 클래스에 정의되거나 해당 클래스에서 상속해야 합니다.

`<Setter.Value>` 속성 요소를 사용하여 `Value` 설정을 지정할 수 있습니다. 그러면 `Value`를 텍스트 문자열로 표현할 수 없는 개체 또는 `OnPlatform` 개체나 `x:Arguments` 또는 `x:FactoryMethod`를 사용하여 인스턴스화된 개체로 설정할 수 있습니다. `StaticResource` 식을 사용하여 `Value` 속성을 사전의 다른 항목으로 설정할 수도 있습니다.

[**BasicStyle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) 프로그램은 기본 구문을 보여주고 `StaticResource` 태그 확장을 사용하여 `Style`을 참조하는 방법을 보여줍니다.

[![BasicStyle의 삼중 스크린샷](images/ch12fg01-small.png "기본 스타일")](images/ch12fg01-large.png#lightbox "기본 스타일")

`Style` 개체 및 `Style` 개체에서 `Value` 설정으로 만든 모든 개체는 해당 `Style`을 참조하는 모든 뷰 간에 공유됩니다. `Style`은 공유할 수 없는 항목(예: `View` 파생 항목)을 포함할 수 없습니다.

`Style`에 이벤트 처리기를 설정할 수 없습니다. `GestureRecognizers` 속성은 바인딩 가능한 속성에서 지원하지 않기 때문에 `Style`에 설정할 수 없습니다.

## <a name="styles-in-code"></a>코드의 Style

일반적이지는 않지만 코드에서 `Style` 개체를 인스턴스화하고 초기화할 수 있습니다. 이는 [**BasicStyleCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) 샘플에서 설명합니다.

## <a name="style-inheritance"></a>Style 상속

`Style`에는 다른 스타일을 참조하는 `StaticResource` 태그 확장으로 설정할 수 있는 [`BasedOn`](xref:Xamarin.Forms.Style.BasedOn) 속성이 있습니다. 이를 통해 이전 스타일에서 스타일을 상속하고 속성 설정을 추가하거나 바꿀 수 있습니다. [**StyleInheritance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) 샘플에서는 이를 보여줍니다.

`Style2`가 `Style1`을 기반으로 하는 경우 `Style2`의 `TargetType`은 `Style1`과 동일하거나 `Style1`에서 파생되어야 합니다. `Style1`이 저장되는 리소스 사전은 시각적 트리에서 `Style2`와 동일한 리소스 사전 또는 그보다 상위 리소스 사전에 있어야 합니다.

## <a name="implicit-styles"></a>암시적 스타일

리소스 사전의 `Style`에 `x:Key` 특성 설정이 없는 경우 사전 키가 자동으로 할당되고 `Style` 개체가 *암시적 스타일*이 됩니다. [**ImplicitStyle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) 샘플에 나와 있는 것처럼 `Style` 설정이 없고 해당 형식이 `TargetType`과 정확히 일치하는 뷰가 해당 스타일을 찾습니다.

암시적 스타일은 `x:Key` 설정을 사용하여 `Style`에서 파생될 수 있지만 그 밖의 다른 방법은 안 됩니다. 암시적 스타일은 명시적으로 참조할 수 없습니다.

스타일과 `BasedOn`을 사용하여 세 가지 유형의 계층 구조를 구현할 수 있습니다.

- `Application` 및 `Page`에 정의된 스타일에서 시각적 트리의 하위 레이아웃에 정의된 스타일까지
- `VisualElement` 및 `View` 같은 기본 클래스에 대해 정의된 스타일에서 특정 클래스에 대해 정의된 스타일까지
- 명시적 사전 키가 있는 스타일에서 암시적 스타일까지

이러한 계층 구조는 [**StyleHierarchy**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) 샘플에서 설명합니다.

## <a name="dynamic-styles"></a>동적 스타일

리소스 사전의 스타일은 `StaticResource`가 아닌 `DynamicResource`에서 참조할 수 있습니다. 이 경우 스타일은 *동적 스타일*이 됩니다. 리소스 사전에서 해당 스타일을 동일한 키를 사용하는 다른 스타일로 바꾸면 `DynamicResource`를 사용하여 해당 스타일을 참조하는 뷰가 자동으로 변경됩니다. 또한 지정된 키를 가진 사전 항목이 없으면 `StaticResource`는 예외를 유발하지만 `DynamicResource`는 그렇지 않습니다.

이 기법을 사용하여 [**DynamicStyles**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) 샘플에 나와 있는 것처럼 스타일이나 테마를 동적으로 변경할 수 있습니다.

그러나 `BasedOn`은 바인딩 가능한 속성에서 지원하지 않으므로 `BasedOn` 속성을 `DynamicResource` 태그 확장으로 설정할 수 없습니다. 스타일을 동적으로 파생시키려면 `BasedOn`을 설정하지 마세요. 대신 [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) 속성을 파생하려는 스타일의 사전 키로 설정하세요. [**DynamicStylesInheritance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) 샘플에서 이 기법을 보여줍니다.

## <a name="device-styles"></a>디바이스 스타일

[`Device.Styles`](xref:Xamarin.Forms.Device.Styles) 중첩 클래스는 일반 텍스트 형식에 사용할 수 있는 `Label`의 `TargetType`을 사용하여 6개 스타일에 12개의 정적 읽기 전용 필드를 정의합니다.

이러한 필드 중 6개는 코드에서 `Style` 속성으로 직접 설정할 수 있는 `Style` 형식입니다.

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)

다른 6개 필드는 `string` 형식이며 동적 스타일의 사전 키로 사용할 수 있습니다.

- [`BodyStyleKey`](xref:Xamarin.Forms.Device.Styles.BodyStyleKey) = "BodyStyle"
- [`TitleStyleKey`](xref:Xamarin.Forms.Device.Styles.TitleStyleKey) = "TitleStyle"
- [`SubtitleStyleKey`](xref:Xamarin.Forms.Device.Styles.SubtitleStyleKey) = "SubtitleStyle"
- [`CaptionStyleKey`](xref:Xamarin.Forms.Device.Styles.CaptionStyleKey) = "CaptionStyle"
- [`ListItemTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyleKey) = "ListItemTextStyle"
- [`ListItemDetailTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyleKey) = "ListItemDetailTextStyle"

이러한 스타일은 [**DeviceStylesList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) 샘플에서 설명합니다.

## <a name="related-links"></a>관련 링크

- [12장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [12장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [스타일](~/xamarin-forms/user-interface/styles/index.md)
