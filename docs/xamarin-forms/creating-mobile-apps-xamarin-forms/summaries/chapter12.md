---
title: "요약 장 12입니다. 스타일"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 168ad0bc2f59f0afd5b457dba38ae39cfa148c89
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-12-styles"></a>요약 장 12입니다. 스타일

Xamarin.Forms에 스타일 속성 설정의 컬렉션을 공유 하는 여러 보기를 허용 합니다. 태그 줄어들고 일관 된 시각적 테마를 유지 관리를 사용 하도록 설정 합니다.

스타일은 거의 항상 정의 이며 태그에서 사용 됩니다. 형식의 개체 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 리소스 사전에서 인스턴스화되고 다음으로 설정 된 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) 속성을 사용 하 여 시각적 요소는 `StaticResource` 또는 `DyanamicResource` 태그 확장입니다.

## <a name="the-basic-style"></a>기본 스타일

A `Style` 을 사용 하려면 해당 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) 에 적용 되는 시각적 개체의 형식으로 설정 합니다. 경우는 `Style` 인스턴스화될 (일반적인 경우와 마찬가지로) 리소스 사전에도 필요는 `x:Key` 특성입니다.

`Style` 형식의 콘텐츠 속성이 [ `Setters` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Setters/)의 컬렉션인 [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) 개체입니다. 각 `Setter` 연결는 [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) 와 [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/)합니다.

XAML에서는 `Property` 설정은 CLR 속성의 이름입니다 (같은 `Text` 속성의 `Button`) 있지만 스타일이 지정 된 속성을 바인딩할 수 있는 속성에 의해 백업 해야 합니다. 또한 가리키는 클래스에서 속성 정의 해야 합니다는 `TargetType` 설정값, 또는 해당 클래스에서 상속 합니다.

지정할 수는 `Value` 속성 요소를 사용 하 여 설정 `<Setter.Value>`합니다. 이렇게 하면 설정 `Value` 또는 텍스트 문자열에서 표현할 수 없는 개체에는 `OnPlatform` 개체 또는 개체를 사용 하 여 인스턴스화될 `x:Arguments` 또는 `x:FactoryMethod`합니다. `Value` 속성으로 설정할 수도 있습니다는 `StaticResource` 사전에 있는 다른 항목에는 식입니다.

[ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) 프로그램의 기본 구문을 보여 주는 및 참조 하는 방법을 보여 줍니다.는 `Style` 와 `StaticResource` 태그 확장:

[![기본 스타일의 삼중 스크린 샷](images/ch12fg01-small.png "기본 스타일이")](images/ch12fg01-large.png "기본 스타일")

`Style` 개체 및에서 만든 모든 개체는 `Style` 개체로 `Value` 설정 하는 참조 하는 모든 뷰 간에 공유 되 `Style`합니다. `Style` 와 같은 공유할 수 없는 모든 항목을 포함할 수 없습니다는 `View` 파생 클래스입니다.

이벤트 처리기에서 설정할 수 없습니다는 `Style`합니다. `GestureRecognizers` 속성 설정할 수 없습니다는 `Style` 바인딩 가능한 속성에서 지원 하지 않는 때문에 있습니다.

## <a name="styles-in-code"></a>코드에서 스타일

일반적인 경우에 있습니다 수 인스턴스화하고 초기화 합니다. `Style` 코드의 개체입니다. 이 확인할는 [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) 샘플.

## <a name="style-inheritance"></a>스타일 상속

`Style` 에 [ `BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) 속성을 설정할 수 있는 한 `StaticResource` 다른 스타일을 참조 하는 태그 확장 합니다. 따라서 이전 스타일에서 상속 하는 추가 하거나 속성 설정을 교체 하기 위해 스타일을 수 있습니다. [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) 이 샘플을 보여 줍니다.

경우 `Style2` 기반 `Style1`, `TargetType` 의 `Style2` 과 같아야 `Style1` 또는에서 파생 된 `Style1`합니다. 있는 리소스 사전 `Style1` 저장 된으로 동일한 리소스 사전 여야 `Style2` 또는 시각적 트리의 상위 리소스 사전입니다.

## <a name="implicit-styles"></a>암시적 스타일

경우는 `Style` 리소스 사전 필요 하지 않습니다는 `x:Key` 설정, 특성 사전 키를 자동으로 할당 된 및 `Style` 개체 됩니다는 *암시적 스타일*합니다. 없이 뷰는 `Style` 설정 및 형식이 일치는 `TargetType` 정확히 경우가 해당 스타일으로는 [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) 샘플을 보여 줍니다.

암시적 스타일에서 파생 될 수는 `Style` 와 `x:Key` 설정이 있지만 하지 반대의 경우도 있습니다. 암시적 스타일을 명시적으로 참조할 수 없습니다.

세 가지 유형의 계층 구조 스타일으로 구현할 수 있습니다 및 `BasedOn`:

- 에 정의 된 스타일에서는 `Application` 및 `Page` 아래로 레이아웃 시각적 트리의 아래쪽에 정의 된 스타일입니다.
- 와 같은 기본 클래스에 대해 정의 된 스타일에서 `VisualElement` 및 `View` 특정 클래스에 대해 정의 된 스타일입니다.
- 암시적 스타일으로 명시적 사전 키를 가진 스타일입니다.

이러한 계층에 나타난는 [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) 샘플.

## <a name="dynamic-styles"></a>동적 스타일

리소스 사전에 스타일을 참조할 수 있습니다 `DynamicResource` 대신 `StaticResource`합니다. 이렇게 하면 스타일은 *동적 스타일*합니다. 해당 스타일 바뀌면 리소스 사전에 다른 스타일으로 동일한 키와 해당 스타일을 참조 하는 뷰 `DynamicResource` 자동으로 변경 합니다. 지정된 된 키와 사전 항목의 부재 하면 또한 `StaticResource` 에서 예외가 발생 하 아닌 `DynamicResource`합니다.

이 기술을 사용 하 여으로 테마 또는 스타일을 동적으로 변경 하는 [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) 샘플을 보여 줍니다.

그러나 설정할 수 없습니다는 `BasedOn` 속성을 한 `DynamicResource` 구성 확장 하기 때문에 `BasedOn` 바인딩 가능한 속성에 지원 되지 않습니다. 스타일을 동적으로 파생을 설정 하지 않으면 `BasedOn`합니다. 대신, 설정는 [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) 사전 키를 파생할 원하는 스타일의 속성입니다. [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) 샘플에는이 기술을 보여 줍니다.

## <a name="device-styles"></a>장치 스타일

[ `Device.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) 가진 6 개의 스타일에 대 한 12 개의 정적 읽기 전용 필드를 정의 하는 중첩 된 클래스는 `TargetType` 의 `Label` 텍스트의 일반 형식에 대해 사용할 수 있는 합니다.

이러한 필드 중 6 개는 형식이 `Style` 에 직접 설정할 수 있는 한 `Style` 코드에서 속성:

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)

다른 6 개의 필드는 형식의 `string` 동적 스타일에 대 한 사전 키로 사용할 수 있습니다.

- [`BodyStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyleKey/) "BodyStyle" 같음
- [`TitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyleKey/) "TitleStyle" 같음
- [`SubtitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyleKey/) "SubtitleStyle" 같음
- [`CaptionStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyleKey/) "CaptionStyle" 같음
- [`ListItemTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyleKey/) "ListItemTextStyle" 같음
- [`ListItemDetailTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyleKey/) "ListItemDetailTextStyle" 같음

이러한 스타일을 보여 줍니다는 [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) 샘플.



## <a name="related-links"></a>관련 링크

- [12 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [12 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [스타일](~/xamarin-forms/user-interface/styles/index.md)
