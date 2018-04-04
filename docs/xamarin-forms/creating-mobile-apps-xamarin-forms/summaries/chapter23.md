---
title: 요약 장 23입니다. 트리거 및 동작
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: f83a93fb5d101a7acd0a67903813dba1ba2bf8b4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>요약 장 23입니다. 트리거 및 동작

트리거 및 동작을 한다는 점에서 비슷합니다 둘 다 하는 것은 데이터 바인딩에 사용 초과 요소 상호 작용을 단순화 하 고 XAML 요소의 기능을 확장 하려면 XAML 파일에서 사용할 수 있습니다. 트리거 및 동작 모두 거의 항상 그래픽 사용자 인터페이스 개체와 함께 사용 됩니다.

트리거 및 동작을 지원 하기 위해 둘 다 `VisualElement` 및 `Style` 두 개의 컬렉션 속성을 지원 합니다.

- [`VisualElement.Triggers`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) 및 [ `Style.Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Triggers/) 형식의 `IList<TriggerBase>`
- [`VisualElement.Behaviors`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) 및 [ `Style.Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Behaviors/) 형식의 `IList<Behavior>`

## <a name="triggers"></a>트리거

트리거는 다른 속성 변경 등의 일부 코드를 실행 하는 응답에 발생 하는 조건 (속성 변경 또는 이벤트의 발생). `Triggers` 속성 `VisualElement` 및 `Style` 유형의 `IList<TriggersBase>`합니다. [`TriggerBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerBase/) 4 개의 봉인 된 클래스에서 파생 되는 추상 클래스:

- [`Trigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) 속성 변경 내용을 기반으로 하는 응답에 대 한
- [`EventTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/) 이벤트 발생에 따라 응답에 대 한
- [`DataTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) 데이터 바인딩에 따라 응답에 대 한
- [`MultiTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) 여러 트리거를 기반으로 하는 응답에 대 한

트리거는 트리거에 의해 속성이 변경 되는 요소에 대해 항상 설정 됩니다.

### <a name="the-simplest-trigger"></a>가장 간단한 트리거

[ `Trigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) 클래스 속성 값의 변경 되었는지 확인 하 고 동일한 요소의 다른 속성을 설정 하 여 응답 합니다.

`Trigger` 세 가지 속성을 정의합니다.

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Property/) 형식의 `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Value/) 형식의 `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Setters/) 형식의 `IList<SetterBase>`의 content 속성 `Trigger`

또한 `Trigger` 가 다음과 같은 속성에서 상속 해야 `TriggerBase` 설정할 수 있습니다.

- [`TargetType`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.TargetType/) 에 있는 요소의 형식을 지정 하기 위해는 `Trigger` 연결

`Property` 및 `Value` 조건을 구성 하 고 `Setters` 컬렉션은 응답입니다. 때 표시 된 `Property` 가리키는 값이 `Value`, 하면 `Setter` 개체에 `Setters` 컬렉션 적용 됩니다. 경우는 `Property` 에 다른 값이 setter 제거 됩니다. `Setter` 처음 두 속성으로 동일 하 게 두 개의 속성을 정의 합니다 `Trigger`:

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) 형식의 `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/) 형식의 `Object`

[ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) 샘플 방법을 `Trigger` 에 적용는 `Entry` 의 크기를 늘릴 수는 `Entry` 통해는 `Scale` 속성 때는 `IsFocused`의 속성은 `Entry` 은 `true`합니다.

일반적인 하지는 않지만 `Trigger` 으로 코드에서 설정할 수는 [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) 샘플을 보여 줍니다.

[ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) 샘플 방법을 `Trigger` 에서 설정할 수 있습니다는 `Style` 여러에 적용할 `Entry` 요소입니다.

### <a name="trigger-actions-and-animations"></a>트리거 동작 및 애니메이션

트리거를 기반으로 하는 작은 코드를 실행할 수 이기도 합니다. 이 코드는 속성을 대상으로 하는 애니메이션 될 수 있습니다. 사용 하는 일반적인 방법 중 하나는 [ `EventTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/), 두 개의 속성을 정의 하는:

- [`Event`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Event/) 형식의 `string`, 이벤트의 이름
- [`Actions`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Actions/) 형식의 `IList<TriggerAction>`, 목록이 응답에서 실행할 작업입니다.

파생 되는 클래스를 작성 해야이 사용 하려면 [ `TriggerAction<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/), 일반적으로 `TriggerAction<VisualElement>`합니다. 이 클래스에서 속성을 정의할 수 있습니다. 이들은 바인딩 가능 속성 보다는 일반 CLR 속성 때문에 `TriggerAction` 에서 파생 되 지도 `BindableObject`합니다. 재정의 해야 합니다는 [ `Invoke` ](https://developer.xamarin.com/api/member/Xamarin.Forms.TriggerAction%3CT%3E.Invoke/p/T/) 동작이 호출 될 때 호출 되는 메서드. 인수는 대상 요소입니다.

[ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리는 예입니다. 호출 하는 `ScaleTo` 애니메이션 효과를 줄 속성의 `Scale` 요소의 속성입니다. 해당 속성 중 하나가 형식 이므로 `Easing`, [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) 클래스를 사용 하면 표준 사용할 `Easing` XAML의 정적 필드입니다.

[ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) 샘플에서는 호출 하는 방법을 보여 줍니다.는 `ScaleAction` 에서 `EventTrigger` 모니터링 하는 개체는 `Focused` 및 `Unfocused` 이벤트입니다.

[ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) 에 대 한 사용자 지정 감속/가속 함수를 정의 하는 방법을 보여 주는 예제 `ScaleAction` 코드 숨김 파일에 있습니다.

사용 하 여 작업을 호출할 수도 있습니다는 `Trigger` (distinguished에서 `EventTrigger`). 인식 하 고이 위해서는 `TriggerBase` 두 컬렉션을 정의 합니다.

- [`EnterActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.EnterActions/) 형식의 `IList<TriggerAction>`
- [`ExitActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.ExitActions/) 형식의 `IList<TriggerAction>`

[ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) 샘플에서는 이러한 컬렉션을 사용 하는 방법을 보여 줍니다.

### <a name="more-event-triggers"></a>자세한 이벤트 트리거

[ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리 호출 `ScaleTo` 확장 및 축소를 두 번입니다. [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) 에 사용 하 여이 한 스타일이 적용 된 `EventTrigger` 시각적 피드백을 제공 하는 경우는 `Button` 를 누를 합니다. Double이 애니메이션도 가능 형식의 컬렉션에 두 가지 동작을 사용 하 여 [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

[ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) 클래스에 **Xamarin.FormsBook.Toolkit** 라이브러리 사용자 지정 가능한 우려가 동작을 정의 합니다. [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) 샘플 것을 보여 줍니다.

[ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) 클래스에 **Xamarin.FormsBook.Toolkit** 라이브러리는 제한 `Entry` 요소 및 설정의 `TextColor` 빨간색 이면 속성의 `Text` 속성 않습니다는 `double`합니다. [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) 샘플 것을 보여 줍니다.

### <a name="data-triggers"></a>데이터 트리거

[ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) 는 비슷합니다는 `Trigger` 제외 하 고 값이 변경에 대 한 속성을 모니터링 하는 대신 한 데이터 바인딩을 모니터링 합니다. 따라서 속성을 하나 요소의 다른 요소에서 속성에 영향을 줄 수 있습니다.

`DataTrigger` 세 가지 속성을 정의합니다.

- [`Binding`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Binding/) 형식의 `BindingBase`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Value/) 형식의 `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Setters/) 형식의 `IList<SetterBase>`

[ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) 샘플을 사용 하려면는 [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) 라이브러리 및 색을 파랑 학생 들의 이름을 설정 또는 분홍색 기반는 `Sex` 속성:

[![성별 색의 삼중 스크린 샷](images/ch23fg04-small.png "성별 색")](images/ch23fg04-large.png#lightbox "성별 색")

[ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) 집합 예제는 `IsEnabled` 속성은 `Entry` 를 `False` 경우는 `Length` 속성의는 `Text` 는 의속성`Entry`0입니다. 에 `Text` 속성은 빈 문자열로 초기화 됩니다. 즉, 기본적으로 `null`, 및 `DataTrigger` 방법이 올바르게 작동 하지 않습니다.

### <a name="combining-conditions-in-the-multitrigger"></a>MultiTrigger에 조건 결합

[ `MultiTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) 는 조건 컬렉션입니다. 모든 때 `true`, setter 적용 됩니다. 클래스는 두 개의 속성을 정의합니다.

- [`Conditions`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Conditions/) 형식의 `IList<Condition>`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Setters/) 형식의 `IList<Setter>`

[`Condition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/) 추상 클래스 이며 두 개의 하위 클래스:

- [`PropertyCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/)를 가진 [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Property/) 및 [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Value/) 등의 속성 `Trigger`
- [`BindingCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingCondition/)를 가진 [ `Binding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Binding/) 및 [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Value/) 등의 속성 `DataTrigger`

에 [ **AndConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) 샘플에서는 `BoxView` 때 4 개의 색만입니다 `Switch` 요소가 모두 설정 합니다.

[ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) 샘플을 만드는 방법을 보여 줍니다.는 `BoxView` 색 때 *모든* 4의 `Switch` 요소가 설정 됩니다. 이렇게 하려면 응용 프로그램 De Morgan 법률 및 모든 논리를 반대로 합니다.

결합 AND 또는 논리 쉽지 이며 일반적으로 보이지 않는 필요 하 고 `Switch` 중간 결과 대 한 요소입니다. [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) 샘플 방법을 `Button` 경우 사용할 수 있습니다 2의 `Entry` 요소에를 입력 한 텍스트를 갖지만 모두 경우 하지에 일부 텍스트를 입력 합니다.

## <a name="behaviors"></a>동작

아무 것도 할 수 있는 트리거, 동작을 수행할 수도 있지만 동작에서 파생 되는 클래스를 항상 필요 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) 다음 두 가지 메서드를 재정의 합니다.

- [`OnAttachedTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/T/)
- [`OnDetachingFrom`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/T/)

인수는 요소에 연결 된 동작입니다. 일반적으로 `OnAttachedTo` 메서드 일부 이벤트 처리기 연결 및 `OnDetachingFrom` 을 분리 합니다. 이러한 클래스는 일반적으로 몇 가지 상태를 저장 하기 때문에 일반적으로 공유할 수 없습니다에 `Style`합니다.

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) 샘플은 유사한 **TriggerEntryValidation** 동작을 사용 하 여 &mdash; 는 [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다.

### <a name="behaviors-with-properties"></a>속성을 가진 동작

`Behavior<T>` 파생 `Behavior`에서 파생 되는 `BindableObject`이므로 하는 동작에 바인딩 가능한 속성을 정의할 수 있습니다. 이러한 속성을 데이터 바인딩에 사용할 수 있습니다.

이 확인할는 [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) 사용 프로그램의 사용은 [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) 클래스에 [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다. `ValidEmailBehavior` 에 읽기 전용 바인딩 가능한 속성 및 데이터 바인딩이 소스 역할을 합니다.

[ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) 샘플 이와 동일한 동작을 사용 하 여 다른 유형의 전자 메일 주소가 올바른지 신호를 보내 표시기를 표시 합니다.

[ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) 샘플은 이전 샘플에서 변형입니다. 사용 하 여 ButtonGlide는 `DataTrigger` 함께 해당 동작 합니다.

### <a name="toggles-and-check-boxes"></a>설정/해제 및 확인란

와 같은 클래스에 있는 토글 단추의 동작을 캡슐화 하는 것이 불가능 [ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) 에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리를 모두를 정의 하 고 XAML에서 설정/해제에 대 한 표시 합니다.

[ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) 샘플에서는 `ToggleBehavior` 와 `DataTrigger` 사용 하는 `Label` 토글에 대 한 두 개의 텍스트 문자열을 사용 합니다.

[ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) 두 사이 전환 하 여이 개념을 확장 하는 샘플 `FormattedString` 개체입니다.

[ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) 클래스에 **Xamarin.FormsBook.Toolkit** 라이브러리에서 파생 `ContentView`, 정의 `IsToggled` 속성을 포함 하는 `ToggleBehavior` 토글에 논리입니다. 이렇게 하면 쉽게 xaml에서는 토글 단추를 정의 하 여 볼 수 있듯이 [ **TranditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) 샘플.

[ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) 포함 한 [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) 에서 파생 된 클래스 `ToggleBase` 사용 하 여 한 [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)Xamarin.Forms는 유사한 토글 단추를 생성 하는 클래스 `Switch`합니다.

A [ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) 에 **Xamarin.FormsBook.Toolkit** 제공는 애니메이션된 수준에서 확인 하는 데 사용 되는 애니메이션의 [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)샘플.

### <a name="responding-to-taps"></a>탭에 응답

한 가지 단점은 `EventTrigger` 은 연결할 수 없습니다는 `TapGestureRecognizer` 탭에 응답할 수 있습니다. 용도 해당 문제를 살펴보기 [ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) 에 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

[ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) 샘플에서는 `TapBehavior` 이전 사용할 `ShiverAction` 탭에 대 한 `BoxView` 요소입니다.

[ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) 캡슐화 하 여 태그에 따라 줄일 방법을 보여 주는 예제는 `ShiverView` 클래스입니다.

### <a name="radio-buttons"></a>라디오 단추

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) library에는 한 [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) 로 그룹화 하는 라디오 단추를 만들기 위한 클래스를 `string` 그룹 이름입니다.

[ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) 프로그램 해당 라디오 단추에 대 한 텍스트 문자열을 사용 합니다. [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) 샘플에서는 `Style` 모양이 checked 및 unchecked 단추 사이의 차이점에 대 한 합니다. [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) 샘플 boxed 이미지를 사용 하 여 해당 라디오 단추에 대 한:

[![라디오 이미지의 삼중 스크린 샷](images/ch23fg17-small.png "라디오 단추 이미지")](images/ch23fg17-large.png#lightbox "라디오 단추 이미지")

[ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) 샘플 원 안에 점이 있는 기존의 표시 라디오 단추를 그립니다.

### <a name="fades-and-orientation"></a>페이드 및 방향

마지막 샘플 [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) 라디오 단추를 사용 하 여 세 가지 서로 다른 색 선택을 뷰 간에 전환할 수 있습니다. 세 가지 뷰 및 축소를 사용 하 여 페이드는 [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) 에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다.

프로그램은 또한 방향 사용 하 여 가로 세로 간에 변경에 응답 한 [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) 에 **Xamarin.FormsBook.Toolkit** 라이브러리입니다.



## <a name="related-links"></a>관련 링크

- [장 23 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [23 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [트리거 사용](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms 동작](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
