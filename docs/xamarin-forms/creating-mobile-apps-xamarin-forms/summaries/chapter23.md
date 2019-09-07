---
title: 요약 23 장입니다. 트리거 및 동작
description: 'Xamarin.ios를 사용 하 여 Mobile Apps 만들기: 요약 23 장입니다. 트리거 및 동작'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 8a1274a8447f49ce39f9c92703bbaec9e875b9e9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760591"
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>요약 23 장입니다. 트리거 및 동작

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)

트리거 및 동작을 한다는 점에서 비슷합니다는 모두 데이터 바인딩의 이상 요소 상호 작용을 단순화 하 고 XAML 요소의 기능을 확장 하려면 XAML 파일에서 사용 하기 위한 합니다. 트리거 및 동작은 모두 시각적 사용자 인터페이스 개체를 사용 하 여 거의 항상 사용 됩니다.

트리거 및 동작을 지원 하기 위해 둘 다 `VisualElement` 및 `Style` 두 컬렉션 속성을 지원 합니다.

- [`VisualElement.Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) 및 [ `Style.Triggers` ](xref:Xamarin.Forms.Style.Triggers) 형식 `IList<TriggerBase>`
- [`VisualElement.Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) 및 [ `Style.Behaviors` ](xref:Xamarin.Forms.Style.Behaviors) 형식 `IList<Behavior>`

## <a name="triggers"></a>트리거

트리거는 응답이 다른 속성 변경 등의 일부 코드를 실행 하는 조건 (속성이 변경 되거나 이벤트 발생). 합니다 `Triggers` 속성을 `VisualElement` 및 `Style` 형식의 `IList<TriggersBase>`합니다. [`TriggerBase`](xref:Xamarin.Forms.TriggerBase) 네 가지 봉인된 클래스가 파생 되는 추상 클래스가입니다.

- [`Trigger`](xref:Xamarin.Forms.Trigger) 속성 변경 내용을 기반으로 하는 응답에 대 한
- [`EventTrigger`](xref:Xamarin.Forms.EventTrigger) 이벤트 발생에 따라 응답에 대 한
- [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) 데이터 바인딩에 따라 응답에 대 한
- [`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger) 여러 트리거를 기반으로 하는 응답에 대 한

트리거는 트리거에 의해 속성이 변경 되는 요소에 대해 항상 설정 됩니다.

### <a name="the-simplest-trigger"></a>가장 간단한 트리거

합니다 [ `Trigger` ](xref:Xamarin.Forms.Trigger) 클래스 속성 값의 변경을 확인 하 고 동일한 요소의 다른 속성을 설정 하 여 응답 합니다.

`Trigger` 세 가지 속성을 정의합니다.

- [`Property`](xref:Xamarin.Forms.Trigger.Property) 형식의 `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Trigger.Value) 형식의 `Object`
- [`Setters`](xref:Xamarin.Forms.Trigger.Setters) 형식의 `IList<SetterBase>`의 content 속성 `Trigger`

또한 `Trigger` 속성에서 상속 됨 필요 `TriggerBase` 설정:

- [`TargetType`](xref:Xamarin.Forms.TriggerBase.TargetType) 요소의 형식을 나타내는 데는 `Trigger` 연결

`Property` 하 고 `Value` 조건 구성 및 `Setters` 컬렉션이 응답 합니다. 때 표시 된 `Property` 가리키는 값이 `Value`, 해당 `Setter` 개체는 `Setters` 컬렉션 적용 됩니다. 경우는 `Property` 에 다른 값이 setter 제거 됩니다. `Setter` 처음 두 속성으로 동일한 두 개의 속성을 정의 `Trigger`:

- [`Property`](xref:Xamarin.Forms.Setter.Property) 형식의 `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Setter.Value) 형식의 `Object`

[ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) 샘플을 보여 줍니다 어떻게를 `Trigger` 적용할를 `Entry` 의 크기를 늘릴 수는 `Entry` 를 통해를 `Scale` 속성 때 합니다 `IsFocused`의 속성을 `Entry` 는 `true`합니다.

일반적인 되지는 않지만 합니다 `Trigger` 로 코드에서 설정할 수 있습니다 합니다 [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) 샘플을 보여 줍니다.

[ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) 샘플과 하는 방법을 `Trigger` 에서 설정할 수 있습니다를 `Style` 여러 적용할 `Entry` 요소입니다.

### <a name="trigger-actions-and-animations"></a>트리거 동작 및 애니메이션

트리거를 기반으로 하는 작은 코드를 실행할 수 이기도 합니다. 이 코드는 속성을 대상으로 하는 애니메이션을 수 있습니다. 한 가지 일반적인 방법을 사용 하는 것을 [ `EventTrigger` ](xref:Xamarin.Forms.EventTrigger), 두 속성을 정의 하는:

- [`Event`](xref:Xamarin.Forms.EventTrigger.Event) 형식의 `string`, 이벤트의 이름
- [`Actions`](xref:Xamarin.Forms.EventTrigger.Actions) 형식의 `IList<TriggerAction>`, 목록이 응답에서 실행할 작업입니다.

파생 된 클래스를 작성 해야이 사용 하려면 [ `TriggerAction<T>` ](xref:Xamarin.Forms.TriggerAction`1)일반적으로, `TriggerAction<VisualElement>`합니다. 이 클래스의 속성을 정의할 수 있습니다. 이들은 바인딩 가능한 속성 대신 일반 CLR 속성 때문 `TriggerAction` 에서 파생 되지 `BindableObject`합니다. 재정의 해야 합니다 [ `Invoke` ](xref:Xamarin.Forms.TriggerAction`1.Invoke*) 작업이 호출 될 때 호출 되는 메서드. 인수는 대상 요소입니다.

합니다 [ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리는 예입니다. 호출 된 `ScaleTo` 속성에 애니메이션 효과를 `Scale` 요소의 속성입니다. 형식의 속성 중 하나 이므로 `Easing`는 [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) 클래스를 사용 하면 표준 `Easing` XAML의 정적 필드입니다.

[ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) 샘플을 호출 하는 방법에 설명 합니다 `ScaleAction` 에서 `EventTrigger` 모니터링 하는 개체를 `Focused` 및 `Unfocused` 이벤트.

합니다 [ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) 예제에 대 한 사용자 지정 감속/가속 함수를 정의 하는 방법을 보여 줍니다 `ScaleAction` 코드 숨김 파일에 있습니다.

사용 하 여 작업을 호출할 수도 있습니다는 `Trigger` (distinguished from `EventTrigger`). 인식 하 고이 위해서는 `TriggerBase` 두 컬렉션을 정의 합니다.

- [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions) 형식의 `IList<TriggerAction>`
- [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions) 형식의 `IList<TriggerAction>`

합니다 [ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) 샘플 이러한 컬렉션을 사용 하는 방법에 설명 합니다.

### <a name="more-event-triggers"></a>자세한 이벤트 트리거

합니다 [ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리 호출은 `ScaleTo` 확장 및 축소를 두 번입니다. 합니다 [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) 샘플에서는이 스타일이 지정 된 `EventTrigger` 시각적 피드백을 제공할 때를 `Button` 눌러져 있습니다. 이 double 애니메이션도 가능 형식의 컬렉션에서 두 가지 작업을 사용 하 여 [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

합니다 [ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) 클래스를 **Xamarin.FormsBook.Toolkit** 라이브러리 사용자 지정 가능한 우려가 동작을 정의 합니다. 합니다 [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) 샘플 이것을 보여 줍니다.

[ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) 클래스를 **Xamarin.FormsBook.Toolkit** 라이브러리는 제한 `Entry` 요소 집합과 `TextColor` 빨간색가 속성을 `Text` 속성 아닙니다는 `double`합니다. 합니다 [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) 샘플 이것을 보여 줍니다.

### <a name="data-triggers"></a>데이터 트리거

합니다 [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) 비슷합니다는 `Trigger` 제외 하 고 데이터 바인딩 값 변경에 대 한 속성을 모니터링 하는 대신 모니터링 합니다. 이 요소를 다른 요소에서 속성에 영향을 줄 하나에서 속성을 수 있습니다.

`DataTrigger` 세 가지 속성을 정의합니다.

- [`Binding`](xref:Xamarin.Forms.DataTrigger.Binding) 형식의 `BindingBase`
- [`Value`](xref:Xamarin.Forms.DataTrigger.Value) 형식의 `Object`
- [`Setters`](xref:Xamarin.Forms.DataTrigger.Setters) 형식의 `IList<SetterBase>`

합니다 [ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) 샘플에 필요 합니다 [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) 라이브러리 및 색을 파란색으로 학생 들 이름 집합 또는 분홍색 기반는 `Sex` 속성:

[![성별 색의 3 배가 스크린 샷](images/ch23fg04-small.png "성별 색")](images/ch23fg04-large.png#lightbox "성별 색")

[ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) 집합 샘플를 `IsEnabled` 속성은 `Entry` 에 `False` 경우를 `Length` 속성은 `Text` 속성을 `Entry`가 0입니다. 에 `Text` 속성을 빈 문자열로 초기화 됩니다. 즉, 기본적으로 `null`, 및 `DataTrigger` 올바르게 작동 하지 않습니다.

### <a name="combining-conditions-in-the-multitrigger"></a>에 MultiTrigger 조건 결합

합니다 [ `MultiTrigger` ](xref:Xamarin.Forms.MultiTrigger) 는 조건의 컬렉션입니다. 모든 있을 때 `true`, setter에 적용 됩니다. 클래스는 두 속성을 정의 합니다.

- [`Conditions`](xref:Xamarin.Forms.MultiTrigger.Conditions) 형식의 `IList<Condition>`
- [`Setters`](xref:Xamarin.Forms.MultiTrigger.Setters) 형식의 `IList<Setter>`

[`Condition`](xref:Xamarin.Forms.Condition) 추상 클래스 이며 두 개의 하위 클래스:

- [`PropertyCondition`](xref:Xamarin.Forms.Condition)에 있는 [ `Property` ](xref:Xamarin.Forms.PropertyCondition.Property) 하 고 [ `Value` ](xref:Xamarin.Forms.PropertyCondition.Value) 등의 속성 `Trigger`
- [`BindingCondition`](xref:Xamarin.Forms.BindingCondition)에 있는 [ `Binding` ](xref:Xamarin.Forms.BindingCondition.Binding) 하 고 [ `Value` ](xref:Xamarin.Forms.BindingCondition.Value) 등의 속성 `DataTrigger`

에 [ **AndConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) 샘플을를 `BoxView` 때 네 가지 색만입니다 `Switch` 요소는 모두 설정 됩니다.

[ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) 예제를 만드는 방법을 보여 줍니다는 `BoxView` 색을 때 *모든* 4 개 `Switch` 요소 켜 집니다. 이 De Morgan Law and 모든 논리를 반전의 응용 프로그램에 필요 합니다.

결합 및 논리는 결코 쉽지 않습니다 하며 일반적으로 보이지 않는 있고 `Switch` 중간 결과 대 한 요소입니다. [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) 샘플을 보여 줍니다 어떻게를 `Button` 경우에 사용할 수 있습니다 두 `Entry` 요소에서는 입력 한 일부 텍스트 있지만 일부 텍스트 입력 모두 있는 경우에 없습니다.

## <a name="behaviors"></a>동작

아무 것도 할 수 있는 트리거를 사용 하 여, 동작을 사용 하 여 수행할 수도 있습니다 하지만 동작에서 파생 된 클래스를 항상 필요 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) 및 다음 두 메서드를 재정의 합니다.

- [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo*)
- [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom*)

인수는 동작에 연결 된 요소입니다. 일반적으로 `OnAttachedTo` 일부 이벤트 처리기를 연결 하는 메서드 및 `OnDetachingFrom` 분리 합니다. 이러한 클래스는 일반적으로 일부 상태를 저장, 때문에 일반적으로 공유할 수 없습니다에 `Style`합니다.

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) 샘플은 비슷합니다 **TriggerEntryValidation** 동작을 사용 하 여 &mdash; 합니다 [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) 클래스는 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다.

### <a name="behaviors-with-properties"></a>속성을 사용 하 여 동작

`Behavior<T>` 파생 `Behavior`에서 파생 되는 `BindableObject`이므로 바인딩 가능한 속성 동작을 정의할 수 있습니다. 이러한 속성은 데이터 바인딩에 사용할 수 있습니다.

이 확인할를 [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) 사용 하는 프로그램을 [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) 클래스를 [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다. `ValidEmailBehavior` 속성이 읽기 전용으로 바인딩 가능 하 고 데이터 바인딩 원본으로 제공 합니다.

합니다 [ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) 샘플 이와 동일한 동작을 사용 하 여 다른 유형의 전자 메일 주소가 올바른지를 알리기 위해 표시기를 표시 합니다.

합니다 [ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) 샘플은 이전 샘플에서 변형 합니다. ButtonGlide 사용을 `DataTrigger` 함께 동작 합니다.

### <a name="toggles-and-check-boxes"></a>설정/해제 및 확인란

와 같은 클래스에 있는 토글 단추의 동작을 캡슐화 하는 것이 불가능 [ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) 에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리 모두를 정의 하 고 XAML에서 완전히 설정/해제에 대 한 시각적 개체입니다.

[ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) 샘플에서는 합니다 `ToggleBehavior` 사용 하 여를 `DataTrigger` 사용 하는 `Label` 토글에 두 개의 텍스트 문자열을 사용 하 여 합니다.

합니다 [ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) 간 전환 하 여이 개념을 확장 하는 샘플 `FormattedString` 개체입니다.

[ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) 클래스를 **Xamarin.FormsBook.Toolkit** 에서 파생 되는 라이브러리 `ContentView`, 정의 `IsToggled` 속성을 통합 및는 `ToggleBehavior` 토글에 논리입니다. 이 쉽게 설정/해제 단추, XAML에서 정의 하 여 볼 수 있듯이 합니다 [ **TraditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) 샘플입니다.

[ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) 포함는 [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) 에서 파생 된 클래스 `ToggleBase` 사용 하는 [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)Xamarin.Forms 유사한 토글 단추를 생성 하는 클래스 `Switch`합니다.

[ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) 에 **Xamarin.FormsBook.Toolkit** 에서 애니메이션된 레버를 확인 하는 데 사용 하는 애니메이션을 제공 합니다 [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)샘플.

### <a name="responding-to-taps"></a>탭에 응답

한 가지 단점은 `EventTrigger` 에 연결할 수 없습니다는 `TapGestureRecognizer` 탭에 응답할 합니다. 용도 문제를 살펴보기 [ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) 에 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

합니다 [ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) 샘플에서는 `TapBehavior` 이전을 사용 하도록 `ShiverAction` 탭에 대 한 `BoxView` 요소입니다.

합니다 [ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) 캡슐화 하 여 태그를 줄이기 위해 하는 방법을 보여 주는 예제는 `ShiverView` 클래스입니다.

### <a name="radio-buttons"></a>라디오 단추

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리 역시를 [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) 그룹화 되는 라디오 단추를 만들기 위한 클래스를 `string` 그룹 이름입니다.

합니다 [ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) 프로그램 해당 라디오 단추에 대 한 텍스트 문자열을 사용 합니다. [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) 사용 하 여 샘플을 `Style` 모양이 checked 및 unchecked 단추 사이의 차이점에 대 한 합니다. 합니다 [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) 샘플 boxed 이미지를 사용 하 여 해당 라디오 단추에 대 한 합니다.

[![라디오 이미지의 삼중 스크린 샷](images/ch23fg17-small.png "라디오 단추 이미지")](images/ch23fg17-large.png#lightbox "라디오 단추 이미지")

합니다 [ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) 샘플 원 안에 점이 있는 기존 표시 라디오 단추를 그립니다.

### <a name="fades-and-orientation"></a>페이드 및 방향

최종 샘플 [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) 라디오 단추를 사용 하 여 세 가지 서로 다른 색 선택 뷰 간에 전환할 수 있습니다. 세 가지 보기 페이드 인 및 체크 아웃 사용 하는 [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) 에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리.

프로그램에도 방향 사용 하 여 가로 세로 간에 변경 내용에 응답을 [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) 에 **Xamarin.FormsBook.Toolkit** 라이브러리입니다.

## <a name="related-links"></a>관련 링크

- [23 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [23 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [트리거 사용](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms 동작](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
