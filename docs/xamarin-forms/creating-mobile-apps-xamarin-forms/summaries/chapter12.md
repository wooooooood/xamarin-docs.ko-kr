---
title: 요약 12 장입니다. 스타일
description: Xamarin.Forms를 사용 하 여 모바일 앱을 만듭니다. 요약 12 장입니다. 스타일
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 408f171a3c7c690b700f7be21a3dcaff503467d9
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926915"
---
# <a name="summary-of-chapter-12-styles"></a>요약 12 장입니다. 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)

Xamarin.Forms 스타일 속성 설정의 컬렉션을 공유할 수 있는 여러 보기를 허용 합니다. 태그 줄이고 일관 된 시각적 테마를 유지 관리를 사용 하도록 설정 합니다.

스타일은 거의 항상 정의 이며 태그에 사용 됩니다. 형식의 개체 [ `Style` ](xref:Xamarin.Forms.Style) 리소스 사전에서 인스턴스화되고로 설정 합니다 [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) 사용 하 여 시각적 요소 속성을 `StaticResource` 또는 `DynamicResource` 태그 확장입니다.

## <a name="the-basic-style"></a>기본 스타일

A `Style` 는 해당 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) 적용할 시각적 개체의 형식으로 설정 합니다. 경우는 `Style` 인스턴스화됩니다 (일반적 임)으로 리소스 사전에도 필요는 `x:Key` 특성입니다.

합니다 `Style` 형식의 콘텐츠 속성이 [ `Setters` ](xref:Xamarin.Forms.Style.Setters)의 컬렉션인 [ `Setter` ](xref:Xamarin.Forms.Setter) 개체입니다. 각 `Setter` 연결 된 [ `Property` ](xref:Xamarin.Forms.Setter.Property) 사용 하 여는 [ `Value` ](xref:Xamarin.Forms.Setter.Value)합니다.

XAML에는 `Property` 설정은 CLR 속성의 이름입니다 (같은 `Text` 의 속성 `Button`) 스타일의 속성 바인딩 가능한 속성으로 백업 해야 하지만 합니다. 또한에서 표시 한 클래스에서 속성을 정의 해야 합니다는 `TargetType` 설정을 또는 해당 클래스에서 상속 합니다.

지정할 수 있습니다 합니다 `Value` property 요소를 사용 하 여 설정 `<Setter.Value>`합니다. 이렇게 하면 설정할 수 있습니다 `Value` 또는 텍스트 문자열에서 표현할 수 없는 개체에는 `OnPlatform` 개체 또는 개체를 사용 하 여 인스턴스화된 `x:Arguments` 또는 `x:FactoryMethod`합니다. 합니다 `Value` 속성을 사용 하 여 설정할 수도 있습니다는 `StaticResource` 사전에 다른 항목에는 식입니다.

합니다 [ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) 프로그램의 기본 구문을 보여 주는 및 참조 하는 방법을 보여 줍니다 합니다 `Style` 사용 하 여는 `StaticResource` 태그 확장:

[![기본 스타일의 삼중 스크린 샷](images/ch12fg01-small.png "기본 스타일")](images/ch12fg01-large.png#lightbox "기본 스타일")

합니다 `Style` 에서 만든 모든 개체 및 개체를 `Style` 개체를 `Value` 설정 하는 참조 하는 모든 뷰 간에 공유 되 `Style`합니다. 합니다 `Style` 와 같은 공유 될 수 없는 모든 항목을 포함할 수 없습니다는 `View` 파생 합니다.

이벤트 처리기에서 설정할 수 없습니다는 `Style`합니다. 합니다 `GestureRecognizers` 속성에 설정할 수 없습니다는 `Style` 하므로 바인딩 가능한 속성에서 지원 되지 않습니다.

## <a name="styles-in-code"></a>코드 스타일

인스턴스화 및 초기화 수 것이 일반적 이지만 `Style` 코드의 개체입니다. 이 확인할 합니다 [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) 샘플입니다.

## <a name="style-inheritance"></a>스타일 상속

`Style` 에 [ `BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) 속성을 설정할 수 있는 `StaticResource` 다른 스타일을 참조 하는 태그 확장 합니다. 이렇게 하면 이전 스타일에서 상속 하 고 추가, 속성 설정을 대체 하는 스타일입니다. 합니다 [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) 샘플에서는이 방법을 보여 줍니다.

하는 경우 `Style2` 기반으로 `Style1`의 `TargetType` 의 `Style2` 과 같아야 `Style1` 에서 파생 된 또는 `Style1`합니다. 리소스 사전을 `Style1` 저장 된으로 동일한 리소스 사전을 해야 `Style2` 또는 시각적 트리의 더 높은 리소스 사전.

## <a name="implicit-styles"></a>암시적 스타일

경우는 `Style` 리소스에서 사전에 없는 `x:Key` 특성 설정을, 사전 키를 자동으로 할당 됩니다 및 `Style` 개체를 *암시적 스타일*. 없이 뷰를 `Style` 설정과 형식이 일치 하는 `TargetType` 정확 하 게 나타나는 해당 스타일으로를 [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) 샘플을 보여 줍니다.

암시적 스타일에서 파생할 수는 `Style` 사용 하 여는 `x:Key` 설정 하지만 되지에 반대 방향의 합니다. 암시적 스타일을 명시적으로 참조할 수 없습니다.

세 가지 유형의 스타일을 사용 하 여 계층을 구현할 수 있습니다 및 `BasedOn`:

- 에 정의 된 스타일에서을 `Application` 및 `Page` 시각적 트리의 아래쪽 레이아웃에 정의 된 스타일까지 합니다.
- 와 같은 기본 클래스에 정의 된 스타일에서 `VisualElement` 및 `View` 특정 클래스에 대해 정의 된 스타일입니다.
- 암시적 스타일 명시적 사전 키를 사용 하 여 스타일입니다.

이러한 계층에서 보여 합니다 [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) 샘플입니다.

## <a name="dynamic-styles"></a>동적 스타일

리소스 사전에 스타일을 참조할 수 있습니다 `DynamicResource` 대신 `StaticResource`합니다. 이렇게 하면 스타일을 *동적 스타일*합니다. 해당 스타일 바뀌면 리소스 사전에 다른 스타일에 의해 동일한 키를 사용 하 여 해당 스타일을 참조 하는 뷰 `DynamicResource` 자동으로 변경 합니다. 지정된 된 키를 사용 하 여 사전 항목의 부재 하면 뿐만 `StaticResource` 예외를 발생 시키려면 아닌 `DynamicResource`합니다.

동적 스타일 또는으로 테마를 변경 하려면이 기법을 사용할 수는 [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) 샘플을 보여 줍니다.

그러나 설정할 수 없습니다 합니다 `BasedOn` 속성을를 `DynamicResource` 구성 확장 하기 때문에 `BasedOn` 바인딩 가능한 속성에 의해 지원 되지 않습니다. 스타일을 동적으로 파생을 설정 하지 않으면 `BasedOn`합니다. 대신 설정 합니다 [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) 사전 키를 파생할 원하는 스타일의 속성입니다. 합니다 [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) 샘플에는이 기술을 보여 줍니다.

## <a name="device-styles"></a>장치 스타일

합니다 [ `Device.Styles` ](xref:Xamarin.Forms.Device.Styles) 사용 하 여 6 개의 스타일에 대 한 12 개의 정적 읽기 전용 필드를 정의 하는 중첩된 클래스를 `TargetType` 의 `Label` 텍스트 사용의 일반적인 유형에 사용할 수 있는 합니다.

이러한 필드 중 6 개는 형식이 `Style` 에 직접 설정할 수 있습니다는 `Style` 코드에서 속성:

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)

다른 6 개 필드는 형식의 `string` 동적 스타일에 대 한 사전 키로 사용할 수 있습니다.

- [`BodyStyleKey`](xref:Xamarin.Forms.Device.Styles.BodyStyleKey) "BodyStyle" 같음
- [`TitleStyleKey`](xref:Xamarin.Forms.Device.Styles.TitleStyleKey) "TitleStyle" 같음
- [`SubtitleStyleKey`](xref:Xamarin.Forms.Device.Styles.SubtitleStyleKey) "SubtitleStyle" 같음
- [`CaptionStyleKey`](xref:Xamarin.Forms.Device.Styles.CaptionStyleKey) "CaptionStyle" 같음
- [`ListItemTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyleKey) "ListItemTextStyle" 같음
- [`ListItemDetailTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyleKey) "ListItemDetailTextStyle" 같음

이러한 스타일에 설명 되어 있습니다 합니다 [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) 샘플입니다.

## <a name="related-links"></a>관련 링크

- [12 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [12 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [스타일](~/xamarin-forms/user-interface/styles/index.md)
