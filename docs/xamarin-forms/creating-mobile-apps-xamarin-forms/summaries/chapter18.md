---
title: 요약 - 18장. MVVM
description: 'Xamarin.Forms로 모바일 앱 만들기: 요약 - 18장. MVVM'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1f180173a42654c54c5686e423ba20d9586271ea
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136710"
---
# <a name="summary-of-chapter-18-mvvm"></a>요약 - 18장. MVVM

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)

애플리케이션을 설계하는 최상의 방법 중 하나는 종종 *비즈니스 논리*라고 부르는 기본 코드와 사용자 인터페이스를 분리하는 것입니다. 몇 가지 기술들이 있지만 XAML 기반 환경에 맞게 조정된 방식을 Model-View-ViewModel 또는 MVVM이라고 부릅니다.

## <a name="mvvm-interrelationships"></a>MVVM 상호 관계

MVVM 애플리케이션에는 세 가지 레이어가 있습니다.

- Model은 파일 또는 웹 액세스를 통해 기본 데이터를 제공합니다.
- View는 일반적으로 XAML로 구현되는 사용자 인터페이스 또는 프레젠테이션 레이어입니다.
- ViewModel은 Model과 View를 연결합니다.

Model은 ViewModel을 무시하고 ViewModel은 View를 무시합니다. 이러한 3개 레이어는 일반적으로 다음과 같은 메커니즘에 따라 서로 연결됩니다.

![View, ViewModel 및 View](images/ch18fg03.png "MVVM")

많은 소규모 프로그램(및 심지어 대규모 프로그램)에서는 Model이 없거나 해당 기능이 ViewModel에 통합된 경우가 많습니다.

## <a name="viewmodels-and-data-binding"></a>ViewModel 및 데이터 바인딩

데이터 바인딩에 참여하기 위해서는 ViewModel이 ViewModel의 속성 변경 시점을 View에 알릴 수 있어야 합니다. ViewModel은 `System.ComponentModel` 네임스페이스에서 [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) 인터페이스를 구현하여 이를 수행합니다. 이것은 Xamarin.Forms가 아닌 .NET의 일부입니다. (일반적으로 ViewModel은 플랫폼 독립성을 유지 관리하려고 시도합니다.)

`INotifyPropertyChanged` 인터페이스는 변경된 속성을 나타내는 [`PropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged)라는 단일 이벤트를 선언합니다.

### <a name="a-viewmodel-clock"></a>ViewModel 시계

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) 라이브러리의 [`DateTimeViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs)은 타이머를 기준으로 변경되는 `DateTime` 형식의 속성을 정의합니다. 이 클래스는 `DateTime` 속성이 변경될 때마다 `INotifyPropertyChanged`를 구현하고 `PropertyChanged` 이벤트를 발생시킵니다.

[**MvvmClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) 샘플은 이 ViewModel을 인스턴스화하고 ViewModel에 대해 데이터 바인딩을 사용하여 업데이트된 날짜 및 시간 정보를 표시합니다.

### <a name="interactive-properties-in-a-viewmodel"></a>ViewModel의 대화형 속성

ViewModel의 속성은 [**SimpleMultiplier**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) 샘플에 속하는 [`SimpleMultiplierViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) 클래스로 표시된 것처럼 더 대화형일 수 있습니다. 데이터 바인딩은 2개의 `Slider` 요소로부터 피승수 및 승수 값을 제공하고 `Label`이 포함된 제품을 표시합니다. 하지만 ViewModel 또는 코드 숨김 파일을 결과적으로 변경하지 않고도 XAML에서 이 사용자 인터페이스를 광범위하게 변경할 수 있습니다.

### <a name="a-color-viewmodel"></a>색 ViewModel

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) 라이브러리의 [`ColorViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs)은 RGB 및 HSL 색 모델을 통합합니다. 이에 대해서는 [**HslSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) 샘플을 참조하십시오.

[![TK의 세 가지 스크린샷](images/ch18fg08-small.png "HSL 색 모델")](images/ch18fg08-large.png#lightbox "HSL 색 모델")

### <a name="streamlining-the-viewmodel"></a>ViewModel 스트리밍

ViewModel의 코드는 호출 속성 이름을 자동으로 가져오는 [`CallerMemberName`](xref:System.Runtime.CompilerServices.CallerMemberNameAttribute) 특성을 사용해서 `OnPropertyChanged` 메서드를 정의하여 스트리밍될 수 있습니다. [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) 라이브러리의 [`ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) 클래스가 이를 수행하고 ViewModel에 대한 기본 클래스를 제공합니다.

## <a name="the-command-interface"></a>명령 인터페이스

MVVM은 데이터 바인딩으로 작동하고, 데이터 바인딩은 속성으로 작동합니다. 따라서 MVVM은 `Button`의 `Clicked` 이벤트 또는 `TapGestureRecognizer`의 `Tapped` 이벤트를 처리할 때 결함이 있는 것으로 보입니다. ViewModel이 이러한 이벤트를 처리할 수 있도록 Xamarin.Forms에서 *명령 인터페이스*가 지원됩니다.

명령 인터페이스는 다음 두 가지 공용 속성을 사용해서 `Button`에서 스스로 매니페스트합니다.

- [`ICommand`](xref:System.Windows.Input.ICommand) 형식의 [`Command`](xref:Xamarin.Forms.Button.Command)(`System.Windows.Input` 네임스페이스에 정의됨)
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)(`Object` 형식)

명령 인터페이스를 지원하기 위해서는 ViewModel이 `Button`의 `Command` 속성에 바인딩된 데이터인 `ICommand` 형식의 속성을 정의해야 합니다. `ICommand` 인터페이스는 두 가지 메서드와 하나의 이벤트를 선언합니다.

- `object` 형식의 인수가 포함된 [`Execute`](xref:System.Windows.Input.ICommand.Execute(System.Object)) 메서드
- `bool`를 반환하는 `object` 형식의 인수가 포함된 [`CanExecute`](xref:System.Windows.Input.ICommand.CanExecute(System.Object)) 메서드
- [`CanExecuteChanged`](xref:System.Windows.Input.ICommand.CanExecuteChanged) 이벤트

내부적으로 ViewModel은 `ICommand` 형식의 각 속성을 `ICommand` 인터페이스를 구현하는 클래스의 인스턴스로 설정합니다. 데이터 바인딩을 통해 `Button`은 초기에 `CanExecute` 메서드를 호출하고, 메서드가 `false`를 반환하면 자신을 사용하지 않도록 설정합니다. 또한 `CanExecuteChanged` 이벤트에 대해 처리기를 설정하고 이 이벤트가 발생할 때마다 `CanExecute`를 호출합니다. `Button`이 사용하도록 설정되었으면 `Button`이 클릭될 때마다 `Execute` 메서드를 호출합니다.

Xamarin.Forms보다 앞선 ViewModel이 있을 수 있으며, 명령 인터페이스를 이미 지원할 수 있습니다. Xamarin.Forms에서만 사용하도록 의도된 새 ViewModel에 대해 Xamarin.Forms는 `ICommand` 인터페이스를 구현하는 [`Command`](xref:Xamarin.Forms.Command) 클래스 및 [`Command<T>`](xref:Xamarin.Forms.Command`1) 클래스를 제공합니다. 제네릭 형식은 `Execute` 및 `CanExecute` 메서드에 대한 인수 형식입니다.

### <a name="simple-method-executions"></a>간단한 메서드 실행

[**PowersOfThree**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) 샘플은 ViewModel에서 명령 인터페이스를 사용하는 방법을 보여줍니다. [`PowersViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) 클래스는 `ICommand` 형식의 두 속성을 정의하고 가장 간단한 [`Command` 생성자](xref:Xamarin.Forms.Command.%23ctor(System.Action))로 전달하는 2개의 프라이빗 속성도 정의합니다. 이 프로그램에는 이 ViewModel에서 두 `Button` 요소의 `Command` 속성으로의 데이터 바인딩이 포함됩니다.

`Button` 요소는 코드 변경 없이 XAML의 `TapGestureRecognizer` 개체로 쉽게 교체될 수 있습니다.

### <a name="a-calculator-almost"></a>거의 계산기

[**AddingMachine**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) 샘플은 `ICommand`의 `Execute` 및 `CanExecute` 메서드를 모두 사용합니다. 이 샘플은 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) 라이브러리의 [`AdderViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) 클래스를 사용합니다. ViewModel에는 `ICommand` 형식의 6개 속성이 포함됩니다. 이것들은 [`Command` 생성자](xref:Xamarin.Forms.Command.%23ctor(System.Action)), `Command`의 [`Command` 생성자](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) 및 `Command<T>`의 [`Command<T>` 생성자](https://docs.microsoft.com/dotnet/api/xamarin.forms.command.-ctor?view=xamarin-forms#Xamarin_Forms_Command__ctor_System_Action_System_Object__System_Func_System_Object_System_Boolean__)로부터 초기화됩니다. 더하기 머신의 숫자 키는 모두 `Command<T>`로 초기화되는 속성에 바인딩되며, `Execute` 및 `CanExecute`에 대한 `string` 인수는 특정 키를 식별합니다.

## <a name="viewmodels-and-the-application-lifecycle"></a>ViewModels 및 애플리케이션 수명 주기

**AddingMachine** 샘플에 사용되는 `AdderViewModel`은 또한 `SaveState` 및 `RestoreState`라는 두 메서드를 정의합니다. 이러한 메서드는 절전 모드로 전환될 때 그리고 다시 시작될 때 애플리케이션에서 호출됩니다.

## <a name="related-links"></a>관련 링크

- [18장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [18장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
- [Xamarin.Forms 전자책을 사용한 엔터프라이즈 애플리케이션 패턴](~/xamarin-forms/enterprise-application-patterns/index.md)
