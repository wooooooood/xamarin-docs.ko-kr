---
title: "요약 장 11입니다. 바인딩 가능한 인프라"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 6e0f1abf04695dfb5348b631a9fbdbd2c81bc431
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>요약 장 11입니다. 바인딩 가능한 인프라

모든 C# 프로그래머는 C#에 대해 잘 알고 *속성*합니다. 속성에 포함 된 *설정* 접근자 및/또는 *가져오기* 접근자입니다. 통칭 되 *CLR 속성* 공용 언어 런타임에 대 한 합니다.

라고 하는 향상 된 속성 정의 정의 하는 Xamarin.Forms는 *바인딩 가능 속성* 에 의해 캡슐화는 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 클래스 및에서 지원 되는 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)클래스입니다. 이러한 클래스는 관련 있지만 매우 별도:는 `BindableProperty` 자체; 속성을 정의 하는 데 사용 `BindableObject` 비슷합니다 `object` 이라는 점에서 바인딩 가능 속성을 정의 하는 클래스에 대 한 기본 클래스입니다.

## <a name="the-xamarinforms-class-hierarchy"></a>Xamarin.Forms 클래스 계층 구조

[ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) 샘플 Xamarin.Forms 클래스 계층 구조를 표시 하 고 실행 하는 중요 한 역할을 시연 하기 위해 `BindableObject` 이 계층 구조에서. `BindableObject` 파생 `Object` 의 부모 클래스를 [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) 올 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) 파생 됩니다. 이 클래스는 부모 클래스를 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 및 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/),이를 부모 클래스 [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/):

[![공유 하는 클래스 계층 구조의 세 스크린 샷](images/ch11fg01-small.png "클래스 계층 구조 공유")](images/ch11fg01-large.png#lightbox "클래스 계층 구조 공유")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>BindableObject 및 BindableProperty 피킹

파생 된 클래스에서 `BindableObject` 많은 CLR 속성이 "를 통해 지원" 바인딩 가능한 속성으로 간주 됩니다. 예를 들어는 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) 의 속성은 `Label` 클래스는 CLR 속성 이지만 `Label` 클래스에는 또한 라는 공용 정적 읽기 전용 필드 정의 [ `TextProperty` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextProperty/) 의 형식 `BindableProperty`합니다.

응용 프로그램 설정 하거나 가져올 수는 `Text` 속성 `Label` 일반적으로 하거나 응용 프로그램을 설정할 수는 `Text` 호출 하 여는 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) 정의한 메서드 `BindableObject` 와 `Label.TextProperty` 인수입니다. 응용 프로그램의 값을 가져오려면 먼저 마찬가지로,는 `Text` 호출 하 여 속성의 [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) 메서드를 사용 하 여 다시는 `Label.TextProperty` 인수입니다. 이 확인할는 [ **PropertySettings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) 샘플.

실제로 `Text` CLR 속성은 완전히 구현 하 여 사용 하는 `SetValue` 및 `GetValue` 정의한 메서드 `BindableObject` 와 함께에서 `Label.TextProperty` 정적 속성입니다.

`BindableObject` 및 `BindableProperty` 에 대 한 지원을 제공 합니다.

- 속성의 기본값을 제공합니다.
- 현재 값을 저장합니다.
- 속성 값 유효성 검사에 대 한 메커니즘을 제공 하
- 단일 클래스에 관련 된 속성의 일관성을 유지 관리
- 속성 변경 내용에 응답
- 속성을 변경 하려고 추가 되었거나 변경 된 경우 알림 트리거
- 데이터 바인딩 지원
- 스타일을 지 원하는
- 동적 리소스 지원

때마다 바인딩 가능한 속성 변경에서 지 원하는 속성 `BindableObject` 발생 한 [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) 변경 된 속성을 식별 하는 이벤트입니다. 속성이 동일한 값으로 설정 된 경우이 이벤트가 발생 하지 않습니다.

일부 속성 바인딩 가능한 속성 및 일부 인 Xamarin.Forms 클래스 & #x 2014; 백업 하지 않습니다. 와 같은 `Span` & #x 2014;에서 파생 되지 않는 `BindableObject`합니다. 파생 된 클래스만 있는 `BindableObject` 때문에 바인딩 가능한 속성을 지원할 수 `BindableObject` 정의 `SetValue` 및 `GetValue` 메서드.

때문에 `Span` 에서 파생 되지 않은 `BindableObject`, 해당 속성 & #x 2014;와 같은 없음 `Text` & #x 2014; 바인딩 가능한 속성에 의해 지원 됩니다. 이 인해는 `DynamicResource` 에 설정 된 `Text` 의 속성 `Span` 에서 예외를 발생 시킵니다는 [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) 이전 장에서 샘플. [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) 샘플에서는 동적 리소스를 사용 하 여 코드에서 설정 하는 방법을 보여 줍니다.는 [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/p/Xamarin.Forms.BindableProperty/System.String/) 정의한 메서드 `Element`합니다. 첫 번째 인수는 형식의 개체로 `BindableProperty`합니다.

마찬가지로,는 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) 정의한 메서드 `BindableObject` 형식의 첫 번째 인수가 `BindableProperty`합니다.

## <a name="defining-bindable-properties"></a>바인딩 가능한 속성 정의

정적을 사용 하 여 직접 바인딩할 수 있는 속성을 정의할 수 있습니다 [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) 메서드나 약간 더 짧은 [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) 형식의 정적 읽기 전용 필드를 만들려면 오버 로드 `BindableProperty`합니다.

이 확인할는 [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다. 클래스에서 파생 `Label` 고 글꼴 크기를 포인트 단위로 지정할 수 있습니다. 에 설명 된 [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) 샘플.

4 개 인수는 `BindableProperty.Create` 메서드는 필요 합니다.

- `propertyName`: 속성 (동일 CLR 속성 이름)의 텍스트 이름
- `returnType`: CLR 속성의 형식
- `declaringType`: 속성을 선언 하는 클래스의 유형
- `defaultValue`: 속성의 기본값

때문에 `defaultValue` 유형의 `object`, 컴파일러는 기본 값의 형식을 확인할 수 있어야 합니다. 예를 들어 경우는 `returnType` 은 `double`, `defaultValue` 0.0 아니라 0과 같은 값으로 설정 되어야 합니다 또는 형식이 일치 하지 않습니다 런타임 시 예외를 트리거합니다.

포함 하도록 바인딩 가능한 속성에 대 한 매우 일반적인 이기도 합니다.

- `propertyChanged`: 정적 메서드는 속성 값이 변경 될 때 호출 합니다. 첫 번째 인수가 속성이 변경 된 클래스의 인스턴스를 보여 줍니다.

나머지 두 인수 `BindableProperty.Create` 일반적이 지:

- `defaultBindingMode`:와 관련 된 데이터 바인딩 사용 (에 설명 된 대로 [ **장 16입니다. 데이터 바인딩**](chapter16.md))
- `validateValue`: 유효한 값을 확인 하는 콜백을
- `propertyChanging`: 속성이 변경 될 때를 나타내는 콜백
- `coerceValue`: coerce 다른 값으로 설정 된 값에 대 한 콜백을
- `defaultValueCreate`: 클래스 (예를 들어 컬렉션)의 인스턴스 간에 공유할 수 없는 기본값을 만들 콜백

### <a name="the-read-only-bindable-property"></a>읽기 전용 바인딩 가능한 속성

바인딩 가능한 속성은 읽기 전용 될 수 있습니다. 정적 메서드를 호출 해야 읽기 전용 바인딩 가능 속성을 만드는 [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) 크기는 [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) 전용 정적 읽기 전용 필드 형식의 정의를 [ `BindablePropertyKey` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindablePropertyKey/).

그런 다음 CLR 속성을 정의 `set` 으로 접근자에서는 `private` 호출 하는 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindablePropertyKey/System.Object/) 오버 로드는 `BindablePropertyKey` 개체입니다. 이렇게 하면 속성을 클래스 외부에서 설정 되지 않습니다.

이 확인할는 [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) 에 사용 되는 클래스는 [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) 샘플.



## <a name="related-links"></a>관련 링크

- [11 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [11 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
