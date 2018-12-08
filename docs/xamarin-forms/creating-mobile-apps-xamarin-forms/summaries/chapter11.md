---
title: 요약 11 장입니다. 바인딩할 수 있는 인프라
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 11 장 요약 합니다. 바인딩할 수 있는 인프라'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: f9e3326c0f55469cfa84a019a674679d82dfc007
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53054238"
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>요약 11 장입니다. 바인딩할 수 있는 인프라

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)

모든 C# 프로그래머는 C#을 사용 하 여 익숙한 *속성*합니다. 속성에 포함 된 *설정* 접근자 및/또는 *가져오기* 접근자입니다. 이러한 이라고 *CLR 속성* 공용 언어 런타임에 대 한 합니다.

호출 하는 향상 된 속성 정의 정의 하는 Xamarin.Forms를 *바인딩 가능 속성* 캡슐화를 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 클래스 및에서 지 원하는 합니다 [ `BindableObject` ](xref:Xamarin.Forms.BindableObject)클래스입니다. 이러한 클래스는 관련 하지만 매우 고유한:는 `BindableProperty` 자체; 속성을 정의 하는 `BindableObject` 비슷합니다 `object` 바인딩 가능한 속성을 정의 하는 클래스에 대 한 기본 클래스입니다.

## <a name="the-xamarinforms-class-hierarchy"></a>Xamarin.Forms 클래스 계층 구조

합니다 [ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) 샘플 리플렉션을 사용 하 여 Xamarin.Forms의 클래스 계층 구조를 표시 하 고 중요 한 역할을 보여 줍니다 `BindableObject` 이 계층 구조에서. `BindableObject` 파생 `Object` 부모 클래스 이며 [ `Element` ](xref:Xamarin.Forms.Element) 올 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 파생 됩니다. 이 부모 클래스 [ `Page` ](xref:Xamarin.Forms.Page) 하 고 [ `View` ](xref:Xamarin.Forms.View)를 부모 클래스인 [ `Layout` ](xref:Xamarin.Forms.Layout):

[![공유 하는 클래스 계층의 삼중 스크린 샷](images/ch11fg01-small.png "클래스 계층 구조 공유")](images/ch11fg01-large.png#lightbox "클래스 계층 구조 공유")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>BindableObject BindableProperty를 피킹

파생 된 클래스에 `BindableObject` 여러 CLR 속성 "지원 되도록" 바인딩 가능한 속성 이라고 합니다. 예를 들어를 [ `Text` ](xref:Xamarin.Forms.Label.Text) 의 속성을 `Label` 클래스는 CLR 속성 하지만 `Label` 클래스도 라는 공용 정적 읽기 전용 필드를 정의 [ `TextProperty` ](xref:Xamarin.Forms.Label.TextProperty) 의 형식 `BindableProperty`합니다.

응용 프로그램 설정 하거나 가져올 수 있습니다는 `Text` 속성을 `Label` 일반적으로 응용 프로그램을 설정할 수는 `Text` 호출 하 여를 [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) 정의한 메서드 `BindableObject` 사용 하 여를 `Label.TextProperty` 인수입니다. 마찬가지로, 응용 프로그램의 값을 가져올 수 있습니다는 `Text` 를 호출 하 여 속성을 [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) 메서드를 사용 하 여 다시는 `Label.TextProperty` 인수. 이 확인할 합니다 [ **PropertySettings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) 샘플입니다.

실제로 `Text` CLR 속성은 완전히 구현 하 여 사용 하 여 합니다 `SetValue` 및 `GetValue` 정의한 메서드 `BindableObject` 와 함께에서 `Label.TextProperty` 정적 속성입니다.

`BindableObject` 및 `BindableProperty` 에 대 한 지원을 제공 합니다.

- 속성의 기본값을 제공합니다.
- 현재 값을 저장합니다.
- 속성 값의 유효성을 검사 하기 위한 메커니즘을 제공 합니다.
- 단일 클래스에 관련 된 속성의 일관성을 유지 관리
- 속성 변경에 응답
- 속성 변경 되었거나 변경 된 경우 알림 트리거
- 데이터 바인딩 지원
- 스타일을 지 원하는
- 동적 리소스를 지원합니다.

때마다 바인딩 가능한 속성 변경, 지 속성 `BindableObject` 발생을 [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) 변경 된 속성을 식별 하는 이벤트입니다. 이 이벤트는 속성은 동일한 값으로 설정 된 경우 발생 되지 않습니다.

일부 속성은 바인딩 가능한 속성 및 일부 Xamarin.Forms 클래스에서 지원 되지 않는 &mdash; 와 같은 `Span` &mdash; 에서 파생 되지 않은 `BindableObject`합니다. 파생 된 클래스만 `BindableObject` 때문에 바인딩 가능한 속성을 지원할 수 있습니다 `BindableObject` 정의 된 `SetValue` 및 `GetValue` 메서드.

때문에 `Span` 에서 파생 되지 않습니다 `BindableObject`, 해당 속성이 없는 &mdash; 와 같은 `Text` &mdash; 바인딩 가능한 속성에 의해 지원 됩니다. 이유는를 `DynamicResource` 에 설정 합니다 `Text` 속성을 `Span` 에서 예외를 발생 시킵니다를 [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) 이전 장에서 샘플. [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) 샘플 코드를 사용 하 여 동적 리소스를 설정 하는 방법에 설명 합니다 [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource(Xamarin.Forms.BindableProperty,System.String)) 정의한 메서드 `Element`합니다. 첫 번째 인수는 형식의 개체 `BindableProperty`합니다.

마찬가지로, 합니다 [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 정의한 메서드 `BindableObject` 형식의 첫 번째 인수가 `BindableProperty`합니다.

## <a name="defining-bindable-properties"></a>바인딩 가능한 속성 정의

정적을 사용 하 여 사용자 고유의 바인딩 가능한 속성을 정의할 수 있습니다 [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) 형식의 정적 읽기 전용 필드를 만들려면 메서드 `BindableProperty`합니다.

에 설명 되어이 [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다. 클래스에서 파생 `Label` 고 글꼴 크기를 포인트 단위로 지정할 수 있습니다. 에 설명 되어는 [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) 샘플입니다.

네 개의 인수를 `BindableProperty.Create` 메서드는 필요 합니다.

- `propertyName`: (동일한 CLR 속성 이름으로) 속성의 텍스트 이름
- `returnType`: CLR 속성의 형식
- `declaringType`: 속성을 선언 하는 클래스의 형식
- `defaultValue`: 속성의 기본값

때문에 `defaultValue` 유형의 `object`, 컴파일러는 기본 값의 형식을 확인할 수 있어야 합니다. 예를 들어 경우는 `returnType` 됩니다 `double`는 `defaultValue` 0.0 아니라 0과 같은 값으로 설정 해야 하거나 형식 불일치 런타임 시 예외를 트리거합니다.

바인딩 가능한 속성을 포함 한 매우 일반적인 이기도 합니다.

- `propertyChanged`: 속성 값을 변경 하는 때 정적 메서드를 호출 합니다. 첫 번째 인수는 속성이 변경 된 클래스의 인스턴스입니다.

다른 인수는 `BindableProperty.Create` 일반적이 지:

- `defaultBindingMode`: 데이터 바인딩와 관련 된 사용 (에서 설명 했 듯이 [ **16 장입니다. 데이터 바인딩**](chapter16.md))
- `validateValue`: 유효한 값을 확인 하는 콜백
- `propertyChanging`: 임을 때 속성 변경에 대 한 콜백
- `coerceValue`: 다른 값으로 설정 된 값을 강제 변환에 대 한 콜백
- `defaultValueCreate`: 클래스 (예를 들어, 컬렉션)의 인스턴스 간에 공유할 수 없는 기본 값에 대 한 콜백

### <a name="the-read-only-bindable-property"></a>읽기 전용으로 바인딩할 수 있는 속성

바인딩 가능한 속성은 읽기 전용 수 있습니다. 읽기 전용으로 바인딩할 수 있는 속성을 만드는 정적 메서드를 호출 해야 [ `BindableProperty.CreateReadOnly` ](xref:Xamarin.Forms.BindableProperty.CreateReadOnly(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) 필드를 정의 하려면 개인 정적 읽기 전용 형식의 [ `BindablePropertyKey` ](xref:Xamarin.Forms.BindablePropertyKey)합니다.

그런 다음 CLR 속성을 정의 `set` 으로 접근자에서는 `private` 를 호출 하는 [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindablePropertyKey,System.Object)) 오버 로드는 `BindablePropertyKey` 개체입니다. 이렇게 하면 속성을 클래스 외부에 설정 되지 않습니다.

에 설명 되어이 [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) 에서 사용 되는 클래스를 [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) 샘플입니다.

## <a name="related-links"></a>관련 링크

- [11 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [11 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
- [바인딩 가능한 속성](~/xamarin-forms/xaml/bindable-properties.md)
