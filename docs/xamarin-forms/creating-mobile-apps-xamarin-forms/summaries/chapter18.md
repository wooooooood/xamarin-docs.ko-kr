---
title: 요약 18 장입니다. MVVM
description: Xamarin.Forms를 사용 하 여 모바일 앱을 만듭니다. 요약 18 장입니다. MVVM
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 6379bafb8c879237171951756441d1227f65b825
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61334637"
---
# <a name="summary-of-chapter-18-mvvm"></a>요약 18 장입니다. MVVM

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)

이 라고도 하는 기본 코드의 사용자 인터페이스를 분리 하 여는 응용 프로그램을 설계 하는 가장 좋은 방법 중 하나는 *비즈니스 논리*합니다. 다양 한 기술을 존재 하지만 XAML 기반 환경에 맞게 작성 된 것은 모델-뷰-ViewModel 또는 MVVM 이라고 합니다.

## <a name="mvvm-interrelationships"></a>MVVM 상호 관계

MVVM 응용 프로그램은 세 가지 계층:

- 이 모델에서는 기본 데이터를 경우에 따라 파일을 통해 또는 웹에 액세스
- 뷰는 사용자 인터페이스 또는 프레젠테이션 계층, 일반적으로 XAML에서 구현 되는
- ViewModel 연결 모델 및 뷰

모델은 ViewModel의 무시 되며 ViewModel 뷰의 무시 합니다. 이 세 가지 계층은 일반적으로 다음과 같은 메커니즘을 사용 하 여 서로 연결 합니다.

![뷰와 ViewModel에 뷰](images/ch18fg03.png "MVVM")

많은 작은 프로그램 (및 훨씬 더 큰 데이터베이스)에 종종 모델 absent 되었거나 기능 ViewModel에 통합 됩니다.

## <a name="viewmodels-and-data-binding"></a>Viewmodel 및 데이터 바인딩

데이터 바인딩에 관여 하기 ViewModel ViewModel의 속성이 변경 되 면 뷰를 알리는 수 있어야 합니다. ViewModel이 작업을 구현 하 여 수행 합니다 [ `INotifyPropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) 인터페이스는 `System.ComponentModel` 네임 스페이스입니다. Xamarin.Forms를 사용 하지 않고.NET 부분입니다. (일반적으로 Viewmodel 플랫폼 독립성을 유지 하려고 합니다.)

합니다 `INotifyPropertyChanged` 인터페이스 라는 단일 이벤트를 선언 [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) 변경 된 속성을 나타내는입니다.

### <a name="a-viewmodel-clock"></a>ViewModel 시계

합니다 [ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) 에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) 형식의 속성을 정의 하는 라이브러리 `DateTime` 타이머에 따라 변경 합니다. 클래스 구현 `INotifyPropertyChanged` 발생 하 고는 `PropertyChanged` 이벤트 때마다는 `DateTime` 속성 변경 합니다.

합니다 [ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) 샘플이이 ViewModel을 인스턴스화하고 ViewModel에 대 한 데이터 바인딩을 사용 하 여 업데이트 된 날짜 및 시간 정보를 표시 합니다.

### <a name="interactive-properties-in-a-viewmodel"></a>ViewModel의 대화형 속성

나타난 것 처럼 ViewModel의 속성을 좀 더 대화형 수는 [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) 일부인 클래스의 합니다 [ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) 샘플입니다. 2 multiplicand 및 승수 값을 제공 하는 데이터 바인딩을 `Slider` 요소 인 제품을 표시 하 고는 `Label`합니다. 그러나 ViewModel 또는 코드 숨김 파일에 그에 따른 변경 하지 않고 XAML에서이 사용자 인터페이스에 광범위 하 게 변경 가능 합니다.

### <a name="a-color-viewmodel"></a>색 ViewModel

합니다 [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) 에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) RGB 및 HSL 색 모델을 통합 하는 라이브러리입니다. 에 설명 되어는 [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) 샘플:

[![삼중 스크린샷 TK](images/ch18fg08-small.png "HSL 색 모델")](images/ch18fg08-large.png#lightbox "HSL 색 모델")

### <a name="streamlining-the-viewmodel"></a>ViewModel을 간소화

Viewmodel의 코드를 정의 하 여 간소화 될 수는 `OnPropertyChanged` 메서드를 사용 하는 [ `CallerMemberName` ](xref:System.Runtime.CompilerServices.CallerMemberNameAttribute) 특성을 자동으로 호출 속성 이름을 가져옵니다. 합니다 [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) 라이브러리는이 작업을 수행 하 고 Viewmodel에 대 한 기본 클래스를 제공 합니다.

## <a name="the-command-interface"></a>명령 인터페이스

MVVM 데이터 바인딩을 사용 하 여 작동 및 MVVM 처리에 있어서 불완전 한 것 같습니다. 데이터 바인딩 속성을 함께 사용을 `Clicked` 의 이벤트를 `Button` 또는 `Tapped` 의 이벤트를 `TapGestureRecognizer`입니다. Xamarin.Forms는 이러한 이벤트를 처리 하는 Viewmodel을 허용 하려면 다음을 지원 합니다.는 *명령 인터페이스*합니다.

자체 인터페이스를 매니페스트는 `Button` 두 개의 공용 속성을 사용 하 여:

- [`Command`](xref:Xamarin.Forms.Button.Command) 형식의 [ `ICommand` ](xref:System.Windows.Input.ICommand) (에 정의 된 `System.Windows.Input` 네임 스페이스)
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) 형식의 `Object`

명령 인터페이스를 지원 하기 위해 ViewModel 형식의 속성을 정의 해야 합니다 `ICommand` 에 바인딩된 후 데이터는 합니다 `Command` 의 속성을 `Button`. `ICommand` 인터페이스는 두 가지 메서드와 하나의 이벤트를 선언 합니다.

- [ `Execute` ](xref:System.Windows.Input.ICommand.Execute(System.Object)) 형식의 인수를 사용 하 여 메서드 `object`
- A [ `CanExecute` ](xref:System.Windows.Input.ICommand.CanExecute(System.Object)) 형식의 인수를 사용 하 여 메서드 `object` 반환 하는 `bool`
- A [ `CanExecuteChanged` ](xref:System.Windows.Input.ICommand.CanExecuteChanged) 이벤트

ViewModel 형식의 각 속성을 설정 하는 내부적으로 `ICommand` 구현 하는 클래스의 인스턴스에 `ICommand` 인터페이스입니다. 데이터 바인딩을 통해 합니다 `Button` 처음으로 호출 합니다 `CanExecute` 메서드를 고 메서드가 반환 하면 자체적으로 비활성화 `false`합니다. 또한에 대 한 처리기를 설정 합니다 `CanExecuteChanged` 이벤트 및 호출 `CanExecute` 때마다 이벤트가 발생 합니다. 경우는 `Button` 는 호출을 사용 합니다 `Execute` 메서드 때마다는 `Button` 클릭할.

일부 Viewmodel은 Xamarin.Forms는 해야 할 수 있습니다 및 인터페이스를 이미 지원 될 수 있습니다. Xamarin.Forms를 제공 하는 Xamarin.Forms와만 함께 사용할 새 Viewmodel 의도 한 것에 대 한는 [ `Command` ](xref:Xamarin.Forms.Command) 클래스와 [ `Command<T>` ](xref:Xamarin.Forms.Command`1) 구현 하는 클래스는 `ICommand` 인터페이스입니다. 제네릭 형식 인수의 형식이 합니다 `Execute` 고 `CanExecute` 메서드.

### <a name="simple-method-executions"></a>간단한 메서드 실행

합니다 [ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) 샘플 ViewModel의 인터페이스를 사용 하는 방법에 설명 합니다. 합니다 [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) 형식의 두 가지 속성을 정의 하는 클래스 `ICommand` 도 가장 간단한에 전달 하는 두 개의 private 속성을 정의 하 고 [ `Command` 생성자](xref:Xamarin.Forms.Command.%23ctor(System.Action))합니다. 프로그램에이 ViewModel에서 데이터 바인딩을 포함 합니다 `Command` 의 두 속성 `Button` 요소입니다.

합니다 `Button` 사용 하 여 요소를 쉽게 바꿀 수 `TapGestureRecognizer` 코드 변경 없이 XAML의 개체입니다.

### <a name="a-calculator-almost"></a>계산기를 거의

[ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) 는 샘플을 모두 사용 합니다 `Execute` 및 `CanExecute` 의 메서드 `ICommand`합니다. 사용 하 여는 [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) 라이브러리입니다. 형식의 6 개의 속성을 포함 하는 ViewModel `ICommand`합니다. 초기화 되는 이러한 합니다 [ `Command` 생성자](xref:Xamarin.Forms.Command.%23ctor(System.Action)) 하 고 [ `Command` 생성자](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) 의 `Command` 하며 [ `Command<T>` 생성자](https://docs.microsoft.com/dotnet/api/xamarin.forms.command.-ctor?view=xamarin-forms#Xamarin_Forms_Command__ctor_System_Action_System_Object__System_Func_System_Object_System_Boolean__) `Command<T>`합니다. 추가 컴퓨터의 숫자 키를 모두 사용 하 여 초기화 된 속성에 바인딩된 `Command<T>`, 및 `string` 인수 `Execute` 및 `CanExecute` 특정 키를 식별 합니다.

## <a name="viewmodels-and-the-application-lifecycle"></a>Viewmodel 및 응용 프로그램 수명 주기

`AdderViewModel` 에 사용 되는 합니다 **AddingMachine** 샘플에는 또한 라는 두 개의 메서드도 정의 `SaveState` 및 `RestoreState`합니다. 이러한 메서드는 절전 모드로 전환 하 고 다시 시작할 때 응용 프로그램에서 호출 됩니다.



## <a name="related-links"></a>관련 링크

- [18 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [18 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
- [Xamarin.Forms 전자책을 사용 하 여 엔터프라이즈 응용 프로그램 패턴](~/xamarin-forms/enterprise-application-patterns/index.md)
