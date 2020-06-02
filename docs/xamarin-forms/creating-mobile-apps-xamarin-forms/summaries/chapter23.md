---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 23. Triggers and behaviors''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9a0206354254f79756e29f834c85837240736eca
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136658"
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>요약 - 23장. 트리거 및 동작

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)

트리거와 동작은 서로 유사한데, 둘 모두 데이터 바인딩을 사용하는 것 외에 요소 상호 작용을 단순화하고 XAML 요소의 기능을 확장하기 위해 XAML 파일에서 사용하기 위한 것입니다. 트리거와 동작은 거의 항상 시각적 사용자 인터페이스 개체와 함께 사용됩니다.

트리거 및 동작을 지원하기 위해 `VisualElement` 및 `Style`에서는 모두 두 개의 컬렉션 속성을 지원합니다.

- [`VisualElement.Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) 및 [`Style.Triggers`](xref:Xamarin.Forms.Style.Triggers)(`IList<TriggerBase>` 형식)
- [`VisualElement.Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) 및 [`Style.Behaviors`](xref:Xamarin.Forms.Style.Behaviors)(`IList<Behavior>` 형식)

## <a name="triggers"></a>트리거

트리거는 응답(속성 변경 또는 일부 코드 실행)이 발생하는 조건(다른 속성 변경 또는 이벤트 실행)입니다. `VisualElement` 및 `Style`의 `Triggers` 속성은 `IList<TriggersBase>` 형식입니다. [`TriggerBase`](xref:Xamarin.Forms.TriggerBase)는 네 개의 봉인된 클래스가 파생되는 추상 클래스입니다.

- 속성 변경에 기반한 응답인 경우 [`Trigger`](xref:Xamarin.Forms.Trigger)
- 이벤트 실행에 기반한 응답인 경우 [`EventTrigger`](xref:Xamarin.Forms.EventTrigger)
- 데이터 바인딩에 기반한 응답인 경우 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)
- 다중 트리거에 기반한 응답인 경우 [`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger)

트리거는 해당 트리거에 의해 속성이 변경되는 요소에 대해 항상 설정됩니다.

### <a name="the-simplest-trigger"></a>가장 간단한 트리거

[`Trigger`](xref:Xamarin.Forms.Trigger) 클래스는 속성 값의 변경 여부를 확인하고 같은 요소의 다른 속성을 설정하여 응답합니다.

`Trigger`는 다음 세 가지 속성을 정의합니다.

- [`Property`](xref:Xamarin.Forms.Trigger.Property)(`BindableProperty` 형식)
- [`Value`](xref:Xamarin.Forms.Trigger.Value)(`Object` 형식)
- [`Setters`](xref:Xamarin.Forms.Trigger.Setters)(`IList<SetterBase>` 형식), `Trigger`의 콘텐츠 속성

또한 `Trigger`는 `TriggerBase`에서 상속된 다음 속성을 설정해야 합니다.

- `Trigger`가 연결된 요소의 형식을 나타내는 [`TargetType`](xref:Xamarin.Forms.TriggerBase.TargetType)

`Property` 및 `Value`는 조건을 구성하고 `Setters` 컬렉션은 응답입니다. 표시된 `Property`에 `Value`로 표시된 값이 있으면 `Setters` 컬렉션의 `Setter` 개체가 적용됩니다. `Property`에 다른 값이 있으면 setter가 제거됩니다. `Setter`는 `Trigger`의 처음 두 속성과 동일한 두 개의 속성을 정의합니다.

- [`Property`](xref:Xamarin.Forms.Setter.Property)(`BindableProperty` 형식)
- [`Value`](xref:Xamarin.Forms.Setter.Value)(`Object` 형식)

[**EntryPop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) 샘플에서는 `Entry`에 적용된 `Trigger`가 `Entry`의 `IsFocused` 속성이 `true`일 때 `Scale` 속성을 통해 `Entry`의 크기를 증가시키는 방법을 보여 줍니다.

일반적이지는 않지만 [**EntryPopCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) 샘플에서 보여주는 것처럼 `Trigger`를 코드에서 설정할 수 있습니다.

[**StyledTriggers**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) 샘플에서는 `Trigger`을 `Style`에서 설정하여 여러 `Entry` 요소에 적용하는 방법을 보여 줍니다.

### <a name="trigger-actions-and-animations"></a>트리거 작업 및 애니메이션

트리거에 기반한 작은 코드를 실행할 수도 있습니다. 이 코드는 속성을 대상으로 하는 애니메이션일 수 있습니다. 일반적인 방법 중 하나는 다음 두 속성을 정의하는 [`EventTrigger`](xref:Xamarin.Forms.EventTrigger)를 사용하는 것입니다.

- [`Event`](xref:Xamarin.Forms.EventTrigger.Event)(`string` 형식), 이벤트의 이름
- [`Actions`](xref:Xamarin.Forms.EventTrigger.Actions)(`IList<TriggerAction>` 형식), 응답에서 실행할 작업 목록

이를 사용하려면 [`TriggerAction<T>`](xref:Xamarin.Forms.TriggerAction`1)에서 파생되는 클래스를 작성해야 합니다(일반적으로 `TriggerAction<VisualElement>`). 이 클래스의 속성을 정의할 수 있습니다. 이들 속성은 `TriggerAction`이 `BindableObject`에서 파생되지 않으므로 바인딩 가능한 속성이 아닌 일반 CLR 속성입니다. 작업이 호출될 때 호출되는 [`Invoke`](xref:Xamarin.Forms.TriggerAction`1.Invoke*) 메서드를 재정의해야 합니다. 인수는 대상 요소입니다.

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`ScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) 클래스는 예제입니다. `ScaleTo` 속성을 호출하여 요소의 `Scale` 속성에 애니메이션 효과를 적용합니다. 속성 중 하나가 `Easing` 형식이므로 [`EasingConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) 클래스를 사용하면 XAML에서 표준 `Easing` 정적 필드를 사용할 수 있습니다.

[**EntrySwell**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) 샘플에서는 `Focused` 및 `Unfocused` 이벤트를 모니터링하는 `EventTrigger` 개체에서 `ScaleAction`을 호출하는 방법을 보여 줍니다.

[**CustomEasingSwell**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) 샘플에서는 코드 숨김 파일에서 `ScaleAction`에 대한 사용자 지정 감속/가속 함수를 정의하는 방법을 보여 줍니다.

`Trigger`를 사용하여 작업을 호출할 수도 있습니다(`EventTrigger`와 구별). 이렇게 하려면 `TriggerBase`에서 정의하는 다음 두 컬렉션을 알아야 합니다.

- [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions)(`IList<TriggerAction>` 형식)
- [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions)(`IList<TriggerAction>` 형식)

[**EnterExitSwell**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) 샘플에서는 이러한 컬렉션을 사용하는 방법을 보여 줍니다.

### <a name="more-event-triggers"></a>추가 이벤트 트리거

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`ScaleUpAndDownAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) 클래스에서 `ScaleTo`를 두 번 호출하여 확장 및 축소합니다. [**ButtonGrowth**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) 샘플에서는 이를 스타일이 적용된 `EventTrigger`에서 사용하여 `Button`을 누를 때 시각적 피드백을 제공합니다. 또한 이 double 애니메이션은 [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs) 형식의 컬렉션에서 두 작업을 사용할 수도 있습니다.

**Xamarin.FormsBook.Toolkit** 라이브러리의 [`ShiverAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) 클래스는 사용자 지정 가능한 shiver 작업을 정의합니다. [**ShiverButtonDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) 샘플에서는 이를 보여 줍니다.

**Xamarin.FormsBook.Toolkit** 라이브러리의 [`NumericValidationAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) 클래스는 `Entry` 요소로 제한되며 `Text` 속성이 `double`이 아니면 `TextColor` 속성을 빨간색으로 설정합니다. [**TriggerEntryValidation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) 샘플에서는 이를 보여 줍니다.

### <a name="data-triggers"></a>데이터 트리거

[`DataTrigger`](xref:Xamarin.Forms.DataTrigger)는 값 변경에 대한 속성을 모니터링하는 대신 데이터 바인딩을 모니터링한다는 점을 제외하면 `Trigger`와 비슷합니다. 따라서 한 요소의 속성이 다른 요소의 속성에 영향을 줄 수 있습니다.

`DataTrigger`는 다음 세 가지 속성을 정의합니다.

- [`Binding`](xref:Xamarin.Forms.DataTrigger.Binding)(`BindingBase` 형식)
- [`Value`](xref:Xamarin.Forms.DataTrigger.Value)(`Object` 형식)
- [`Setters`](xref:Xamarin.Forms.DataTrigger.Setters)(`IList<SetterBase>` 형식)

[**GenderColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) 샘플을 사용하려면 [**SchoolOfFineArt**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) 라이브러리가 필요하며, `Sex` 속성에 기반하여 학생 이름의 색을 blue 또는 pink로 설정합니다.

[![성별 색의 삼중 스크린샷](images/ch23fg04-small.png "성별 색")](images/ch23fg04-large.png#lightbox "성별 색")

[**ButtonEnabler**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) 샘플에서는 `Entry`의 `Text` 속성의 `Length` 속성이 0인 경우 `Entry`의 `IsEnabled` 속성을 `False`로 설정합니다. `Text` 속성이 빈 문자열로 초기화되었는지 확인합니다. 기본적으로 `null`이며 `DataTrigger`는 제대로 작동하지 않습니다.

### <a name="combining-conditions-in-the-multitrigger"></a>MultiTrigger에서 조건 조합

[`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger)는 조건의 컬렉션입니다. 모두 `true`이면 setter가 적용됩니다. 클래스는 다음 두 가지 속성을 정의합니다.

- [`Conditions`](xref:Xamarin.Forms.MultiTrigger.Conditions)(`IList<Condition>` 형식)
- [`Setters`](xref:Xamarin.Forms.MultiTrigger.Setters)(`IList<Setter>` 형식)

[`Condition`](xref:Xamarin.Forms.Condition)은 추상 클래스이며 다음 두 하위 클래스가 있습니다.

- `Trigger`와 같은 [`Property`](xref:Xamarin.Forms.PropertyCondition.Property) 및 [`Value`](xref:Xamarin.Forms.PropertyCondition.Value) 속성이 있는 [`PropertyCondition`](xref:Xamarin.Forms.Condition)
- `DataTrigger`와 같은 [`Binding`](xref:Xamarin.Forms.BindingCondition.Binding) 및 [`Value`](xref:Xamarin.Forms.BindingCondition.Value) 속성이 있는 [`BindingCondition`](xref:Xamarin.Forms.BindingCondition)

[**AndConditions**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) 샘플에서 `BoxView`는 4개의 `Switch` 요소가 모두 설정된 경우에만 색이 지정됩니다.

[**OrConditions**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) 샘플에서는 4개의 `Switch` 요소 중 *하나*가 설정된 경우 `BoxView`에 색을 지정하는 방법을 보여 줍니다. 이를 위해서는 De Morgan 법칙을 적용하고 모든 논리를 반대로 해야 합니다.

AND와 OR 논리를 결합하는 것은 쉽지 않으며 일반적으로 중간 결과에 대해 보이지 않는 `Switch` 요소가 필요합니다. [**XorConditions**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) 샘플에서는 두 개의 `Entry` 요소 중 하나에 입력된 텍스트가 있으면 `Button`을 사용하지만 두 요소에 모두 입력된 텍스트가 있는 경우에는 사용하지 않도록 설정하는 방법을 보여 줍니다.

## <a name="behaviors"></a>동작

트리거를 사용하여 수행할 수 있는 모든 작업을 하나의 동작으로 수행할 수도 있지만 동작은 항상 [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1)에서 파생되는 클래스가 필요하며 다음 두 메서드를 재정의합니다.

- [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo*)
- [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom*)

인수는 동작이 연결된 요소입니다. 일반적으로 `OnAttachedTo` 메서드는 일부 이벤트 처리기를 연결하고 `OnDetachingFrom` 메서드는 분리합니다. 이러한 클래스는 대개 일부 상태를 저장하기 때문에 일반적으로 `Style`에서 공유할 수 없습니다.

[**BehaviorEntryValidation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) 샘플은 동작([**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`NumericValidationBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) 클래스)을 사용한다는 점을 제외하고 **TriggerEntryValidation**과 비슷합니다.

### <a name="behaviors-with-properties"></a>속성이 있는 동작

`Behavior<T>`는 `BindableObject`에서 파생되는 `Behavior`로부터 파생되므로 바인딩 가능한 속성을 동작에 정의할 수 있습니다. 이러한 속성은 데이터 바인딩에서 활성화될 수 있습니다.

이는 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`ValidEmailBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) 클래스를 사용하는 [**EmailValidationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) 프로그램에 설명되어 있습니다. `ValidEmailBehavior`는 바인딩 가능한 읽기 전용 속성을 가지며 데이터 바인딩에서 소스로 사용됩니다.

[**EmailValidationConv**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) 샘플에서는 이 동일한 동작을 사용하여 이메일 주소가 유효하다는 신호를 표시하는 다른 유형의 표시기를 표시합니다.

[**EmailValidationTrigger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) 샘플은 이전 샘플의 변형입니다. ButtonGlide는 이 동작과 함께 `DataTrigger`를 사용합니다.

### <a name="toggles-and-check-boxes"></a>토글 및 확인란

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`ToggleBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs)와 같은 클래스에서 토글 단추의 동작을 캡슐화하고 XAML에서 전체 토글에 대한 모든 시각적 개체를 정의할 수 있습니다.

[**ToggleLabel**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) 샘플에서는 토글에 대한 두 개의 텍스트 문자열이 있는 `Label`을 사용하기 위해 `ToggleBehavior`를 `DataTrigger`와 함께 사용합니다.

[**FormattedTextToggle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) 샘플에서는 두 개의 `FormattedString` 개체 사이를 전환하여 이 개념을 확장합니다.

**Xamarin.FormsBook.Toolkit** 라이브러리의 [`ToggleBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) 클래스는 `ContentView`에서 파생되고, `IsToggled` 속성을 정의하며, 토글 논리에 대한 `ToggleBehavior`를 통합합니다. 이렇게 하면 [**TraditionalCheckBox**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) 샘플에 설명된 대로 XAML에서 토글 단추를 보다 쉽게 정의할 수 있습니다.

[**SwitchCloneDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo)에는 `ToggleBase`에서 파생되고 [`TranslateAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs) 클래스를 사용하여 Xamarin.Forms `Switch`와 유사한 토글 단추를 생성하는 [`SwitchClone`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) 클래스가 포함됩니다.

**Xamarin.FormsBook.Toolkit**의 [`RotateAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs)에서는 [**LeverToggle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle) 샘플에서 애니메이션 레버를 만드는 데 사용되는 애니메이션을 제공합니다.

### <a name="responding-to-taps"></a>탭에 응답

`EventTrigger`의 단점 중 하나는 `TapGestureRecognizer`에 연결하여 탭에 응답할 수 없다는 것입니다. 이 문제를 해결하는 것이 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)에서 [`TapBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs)의 목적입니다.

[**BoxViewTapShiver**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) 샘플에서는 탭형 `BoxView` 요소에 대해 이전 `ShiverAction`을 사용할 수 있도록 `TapBehavior`를 사용합니다.

[**ShiverViews**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) 샘플에서는 `ShiverView` 클래스를 캡슐화하여 표시를 잘라내는 방법을 보여 줍니다.

### <a name="radio-buttons"></a>라디오 단추

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 `string` 그룹 이름으로 그룹화된 라디오 단추를 만들기 위한 [`RadioBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) 클래스도 있습니다.

[**RadioLabels**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) 프로그램은 해당 라디오 단추에 대해 텍스트 문자열을 사용합니다. [**RadioStyle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) 샘플에서는 선택된 단추와 선택되지 않은 단추 간의 모양 차이에 `Style`을 사용합니다. [**RadioImages**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) 샘플에서는 라디오 단추에 대해 boxed 이미지를 사용합니다.

[![라디오 이미지의 삼중 스크린샷](images/ch23fg17-small.png "라디오 단추 이미지")](images/ch23fg17-large.png#lightbox "라디오 단추 이미지")

[**TraditionalRadios**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) 샘플은 원 안에 점이 있는 기존에 표시되는 라디오 단추를 그립니다.

### <a name="fades-and-orientation"></a>페이드 및 방향

마지막 [**MultiColorSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) 샘플을 사용하면 라디오 단추를 사용하는 세 가지 색 선택 뷰 사이에 전환할 수 있습니다. 이 세 가지 뷰는 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`FadeEnableAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs)을 사용하여 페이드 인 및 페이드 아웃을 수행합니다.

또한 이 프로그램은 **Xamarin.FormsBook.Toolkit** 라이브러리의 [`GridOrientationBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs)를 사용하여 세로와 가로 간의 방향 변경에 응답합니다.

## <a name="related-links"></a>관련 링크

- [23장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [23장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [트리거 작업](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms 동작](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
