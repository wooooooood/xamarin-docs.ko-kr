---
title: 요약 - 11장. 바인딩할 수 있는 인프라
description: 'Xamarin.Forms로 모바일 앱 만들기: 요약 - 11장. 바인딩할 수 있는 인프라'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: f9e3326c0f55469cfa84a019a674679d82dfc007
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "61334364"
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>요약 - 11장. 바인딩할 수 있는 인프라

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)

모든 C# 프로그래머는 C# *속성*에 익숙합니다. 속성에는 *set* 접근자 및/또는 *get* 접근자가 포함됩니다. 종종 이를 공용 언어 런타임에 대한 *CLR 속성*이라고도 부릅니다.

Xamarin.Forms는 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 클래스로 캡슐화되고 [`BindableObject`](xref:Xamarin.Forms.BindableObject) 클래스로 지원되는 *바인딩할 수 있는 속성*이라고 부르는 향상된 속성 정의를 정의합니다. 이러한 클래스는 서로 연관되어 있더라도 서로 다른 점이 많습니다. `BindableProperty`는 속성 자체를 정의하기 위해 사용됩니다. `BindableObject`는 바인딩할 수 있는 속성을 정의하는 클래스의 기본 클래스라는 점에서 `object`와 비슷합니다.

## <a name="the-xamarinforms-class-hierarchy"></a>Xamarin.Forms 클래스 계층 구조

[**ClassHierarchy**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) 샘플은 리플렉션을 사용해서 Xamarin.Forms의 클래스 계층 구조를 표시하고 이 계층 구조에서 `BindableObject`로 수행되는 중요한 역할을 보여줍니다. `BindableObject`는 `Object`에서 파생되며 [`VisualElement`](xref:Xamarin.Forms.VisualElement)가 파생되는 [`Element`](xref:Xamarin.Forms.Element)에 대한 부모 클래스입니다. 이 클래스는 [`Layout`](xref:Xamarin.Forms.Layout)의 부모 클래스인 [`Page`](xref:Xamarin.Forms.Page) 및 [`View`](xref:Xamarin.Forms.View)의 부모 클래스입니다.

[![클래스 계층 구조 공유에 대한 세 가지 스크린샷](images/ch11fg01-small.png "클래스 계층 구조 공유")](images/ch11fg01-large.png#lightbox "클래스 계층 구조 공유")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>BindableObject 및 BindableProperty 살펴보기

`BindableObject`에서 파생되는 클래스에서 많은 CLR 속성은 바인딩할 수 있는 속성으로 "지원된다"고 말합니다. 예를 들어 `Label` 클래스의 [`Text`](xref:Xamarin.Forms.Label.Text) 속성은 CLR 속성이지만 `Label` 클래스도 `BindableProperty` 형식의 [`TextProperty`](xref:Xamarin.Forms.Label.TextProperty)라는 공용 정적 읽기 전용 필드를 정의합니다.

애플리케이션은 일반적으로 `Label`의 `Text` 속성을 설정하거나 가져올 수 있습니다. 또는 애플리케이션이 `Label.TextProperty` 인수로 `BindableObject`에 의해 정의된 [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) 메서드를 호출하여 `Text`를 설정할 수 있습니다. 마찬가지로 애플리케이션은 다시 `Label.TextProperty` 인수로 [`GetValue`](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) 메서드를 호출하여 `Text` 속성의 값을 가져올 수 있습니다. 이에 대해서는 [**PropertySettings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) 샘플을 참조하십시오.

실제로 `Text` CLR 속성은 전적으로 `Label.TextProperty` 정적 속성과 함께 `BindableObject`에 의해 정의된 `SetValue` 및 `GetValue` 메서드를 사용하여 구현됩니다.

`BindableObject` 및 `BindableProperty`는 다음에 대한 지원을 제공합니다.

- 속성 기본값 제공
- 현재 값 저장
- 속성 값 유효성 검사를 위한 메커니즘 제공
- 단일 클래스에서 관련된 속성 간 일관성 유지 관리
- 속성 변경에 응답
- 속성이 변경될 예정이거나 변경된 경우 알림 트리거
- 데이터 바인딩 지원
- 스타일 지원
- 동적 리소스 지원

바인딩할 수 있는 속성으로 지원되는 속성이 변경될 때마다 `BindableObject`는 변경된 속성을 식별하는 [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) 이벤트를 발생시킵니다. 속성이 동일한 값으로 설정되었을 때는 이 이벤트가 발생되지 않습니다.

일부 속성은 바인딩할 수 있는 속성으로 지원되지 않으며, `Span`과 같은 일부 Xamarin.Forms 클래스는 `BindableObject`에서 파생되지 않습니다. `BindableObject`가 `SetValue` 및 `GetValue` 메서드를 정의하기 때문에 `BindableObject`에서 파생되는 클래스만 바인딩할 수 있는 속성을 지원할 수 있습니다.

`Span`이 `BindableObject`에서 파생되지 않기 때문에 `Text`와 같은 해당 속성은 바인딩할 수 있는 속성으로 지원되지 않습니다. 따라서 `Span`의 `Text` 속성에 `DynamicResource`를 설정하면 이전 장의 [**DynamicVsStatic**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) 샘플에서 예외가 발생합니다. [**DynamicVsStaticCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) 샘플은 `Element`로 정의된 [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource(Xamarin.Forms.BindableProperty,System.String)) 메서드를 사용하여 코드에서 동적 리소스를 설정하는 방법을 보여줍니다. 첫 번째 인수는 `BindableProperty` 형식의 객체입니다.

마찬가지로 `BindableObject`로 정의된 [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 메서드에는 `BindableProperty` 형식의 첫 번째 인수가 포함됩니다.

## <a name="defining-bindable-properties"></a>바인딩할 수 있는 속성 정의

`BindableProperty` 형식의 정적 읽기 전용 필드를 만들도록 정적 [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) 메서드를 사용하여 사용자 고유의 바인딩할 수 있는 속성을 정의할 수 있습니다.

이에 대해서는 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`AltLabel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) 클래스를 참조하십시오. 클래스가 `Label`에서 파생되며, 이를 이용해서 글꼴 크기(포인트)를 지정할 수 있습니다. 이에 대해서는 [**PointSizedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) 샘플을 참조하십시오.

`BindableProperty.Create` 메서드의 네 가지 인수가 필요합니다.

- `propertyName`: 속성의 텍스트 이름(CLR 속성 이름과 동일)
- `returnType`: CLR 속성의 형식
- `declaringType`: 속성을 선언하는 클래스의 형식
- `defaultValue`: 속성의 기본값

`defaultValue`가 `object` 형식이기 때문에 컴파일러가 기본값의 형식을 확인할 수 있어야 합니다. 예를 들어 `returnType`이 `double`이면 `defaultValue`를 단지 0이 아닌 0.0과 같은 값으로 설정해야 합니다. 그렇지 않으면 형식 불일치로 인해 런타임에 예외가 트리거됩니다.

또한 바인딩할 수 있는 속성에 대해 다음을 포함하는 것이 매우 일반적입니다.

- `propertyChanged`: 속성이 값을 변경할 때 호출되는 정적 메서드입니다. 첫 번째 인수는 해당 속성이 변경된 클래스의 인스턴스입니다.

`BindableProperty.Create`의 다른 인수는 그렇게 일반적이지 않습니다.

- `defaultBindingMode`: 데이터 바인딩과 함께 사용됩니다([**16장. 데이터 바인딩**](chapter16.md) 참조).
- `validateValue`: 유효한 값을 확인하기 위한 콜백입니다.
- `propertyChanging`: 속성이 변경될 시간을 나타내기 위한 콜백입니다.
- `coerceValue`: 설정 값을 다른 값으로 강제 변환하기 위한 콜백입니다.
- `defaultValueCreate`: 클래스의 인스턴스 간에 공유될 수 없는 기본값을 만들기 위한 콜백입니다(예: 컬렉션).

### <a name="the-read-only-bindable-property"></a>읽기 전용 바인딩할 수 있는 속성

바인딩할 수 있는 속성은 읽기 전용일 수 있습니다. 읽기 전용 바인딩할 수 있는 속성을 만들려면 정적 메서드 [`BindableProperty.CreateReadOnly`](xref:Xamarin.Forms.BindableProperty.CreateReadOnly(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate))를 호출하여 [`BindablePropertyKey`](xref:Xamarin.Forms.BindablePropertyKey) 형식의 프라이빗 정적 읽기 전용 필드를 정의해야 합니다.

그런 다음, CLR 속성 `set` 접근자를 `private`로 정의하여 `BindablePropertyKey` 개체로 [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindablePropertyKey,System.Object)) 오버로드를 호출합니다. 그러면 속성이 클래스 외부에 설정되지 않도록 방지됩니다.

이에 대해서는 [**BaskervillesCount**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) 샘플에 사용되는 [`CountedLabel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) 클래스를 참조하십시오.

## <a name="related-links"></a>관련 링크

- [11장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [11장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
- [바인딩 가능한 속성](~/xamarin-forms/xaml/bindable-properties.md)
