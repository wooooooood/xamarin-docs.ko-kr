---
title: "18 장의 요약입니다. MVVM"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: fa62ff952a4a8916a0c9603157d14948119d243d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-18-mvvm"></a>18 장의 요약입니다. MVVM

응용 프로그램을 설계 하는 가장 좋은 방법 중 하나이 라고도 하는 기본 코드에서 사용자 인터페이스를 분리 하는 것은 *비즈니스 논리*합니다. 다양 한 기술이 있기 있지만 XAML 기반 환경에 맞게 작성 된 것 이라고 모델-뷰-ViewModel 또는 MVVM 합니다.

## <a name="mvvm-interrelationships"></a>MVVM 상호 관계

MVVM 응용 프로그램에 있는 3 계층:

- 이 모델에서는 기본 데이터에서 경우에 따라 파일을 통해 또는 웹 액세스
- 보기는 사용자 인터페이스 또는 프레젠테이션 계층, 일반적으로 XAML에서 구현 되는
- ViewModel 연결 모델 및 뷰

모델은 ViewModel의 무시 되며 ViewModel 보기에 알지 못합니다. 일반적으로이 세 가지 계층에는 다음과 같은 메커니즘을 사용 하 여 서로 연결:

![ViewModel, 뷰와 뷰](images/ch18fg03.png "MVVM")

대부분의 더 작은 프로그램 (및 훨씬 더 큰 데이터베이스)에 종종 모델 absent 되었거나 해당 기능은 ViewModel에 통합 되어 있습니다.

## <a name="viewmodels-and-data-binding"></a>Viewmodel 및 데이터 바인딩

데이터 바인딩, 관여 하기는 ViewModel ViewModel의 속성이 변경 되 면 보기에 알리는 수 있어야 합니다. ViewModel이 작업을 구현 하 여 수행 된 [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) 인터페이스는 `System.ComponentModel` 네임 스페이스입니다. Xamarin.Forms를 사용 하지 않고.NET 부분입니다. (일반적으로 Viewmodel 플랫폼 독립성을 유지 하려고 합니다.)

`INotifyPropertyChanged` 명명 된 단일 이벤트를 선언 하는 인터페이스 [ `PropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) 변경 된 속성을 나타내는입니다.

### <a name="a-viewmodel-clock"></a>ViewModel 클록

[ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) 에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) 형식의 속성을 정의 하는 라이브러리 `DateTime` 타이머에 따라 변경 합니다. 클래스 구현 `INotifyPropertyChanged` 를 발생 시키는 `PropertyChanged` 이벤트 때마다는 `DateTime` 속성 변경 합니다.

[ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) 샘플이 ViewModel이 인스턴스화하고 ViewModel에 대 한 데이터 바인딩을 사용 하 여 업데이트 된 날짜 및 시간 정보를 표시 합니다.

### <a name="interactive-properties-in-a-viewmodel"></a>ViewModel에서 대화형 속성

ViewModel 속성에 나타난 것 처럼 좀 더 대화형 일 수는 [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) 참가 하는 클래스의는 [ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) 샘플. 두 개에서 승수 및 승수 값을 제공 하는 데이터 바인딩 `Slider` 요소 인 제품을 표시 하 고는 `Label`합니다. 그러나 ViewModel 또는 코드 숨김 파일에 따른 변경 하지 않고 XAML에서이 사용자 인터페이스에 광범위 하 게 변경 가능 합니다.

### <a name="a-color-viewmodel"></a>색 ViewModel

[ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) 에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) RGB 및 HSL 색 모델을 통합 하는 라이브러리입니다. 에 설명 된 [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) 샘플:

[![TK의 삼중 스크린 샷](images/ch18fg08-small.png "HSL 색 모델")](images/ch18fg08-large.png "HSL 색 모델")

### <a name="streamlining-the-viewmodel"></a>ViewModel 간소화

Viewmodel의 코드를 정의 하 여 간소화 될 수는 `OnPropertyChanged` 메서드를 사용 하는 [ `CallerMemberName` ](https://developer.xamarin.com/api/type/System.Runtime.CompilerServices.CallerMemberNameAttribute/) 특성을 자동으로 호출 하는 속성 이름을 가져옵니다. [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) 라이브러리는이 작업을 수행 하 고 Viewmodel에 대 한 기본 클래스를 제공 합니다.

## <a name="the-command-interface"></a>명령 인터페이스

데이터 바인딩을 사용 하는 작동 하는 MVVM 및 MVVM 처리에 관한 불완전 한 것 같습니다 데이터 바인딩 속성을 함께 사용는 `Clicked` 의 이벤트는 `Button` 또는 `Tapped` 의 이벤트는 `TapGestureRecognizer`합니다. 이러한 이벤트를 처리 하는 Viewmodel 있도록 Xamarin.Forms 지원는 *명령 인터페이스*합니다.

자체 명령 인터페이스 매니페스트는 `Button` 두 개의 공용 속성이 있는:

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) 형식의 [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) (에 정의 된 `System.Windows.Input` 네임 스페이스)
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) 형식의 `Object`

명령 인터페이스를 지원 하기 위해 한 ViewModel 형식의 속성을 정의 해야 `ICommand` 에 바인딩된 다음 데이터 즉는 `Command` 의 속성은 `Button`합니다. `ICommand` 인터페이스 하나의 이벤트와 두 개의 메서드를 선언 합니다.

- [ `Execute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.Execute/p/System.Object/) 형식의 인수를 사용 하 여 메서드 `object`
- A [ `CanExecute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.CanExecute/p/System.Object/) 형식의 인수를 사용 하 여 메서드 `object` 반환 하는 `bool`
- A [ `CanExecuteChanged` ](https://developer.xamarin.com/api/event/System.Windows.Input.ICommand.CanExecuteChanged/) 이벤트

ViewModel 형식의 각 속성을 설정 하는 내부적으로 `ICommand` 구현 하는 클래스의 인스턴스에 `ICommand` 인터페이스입니다. 데이터 바인딩을 통해는 `Button` 처음으로 호출 된 `CanExecute` 메서드를 고 메서드가 반환 하면 자체적으로 비활성화 `false`합니다. 또한 설정에 대 한 처리기는 `CanExecuteChanged` 이벤트 및 호출 `CanExecute` 때마다 이벤트가 발생 합니다. 경우는 `Button` 는 호출을 사용 하도록 설정는 `Execute` 메서드 때마다는 `Button` 를 클릭 합니다.

Xamarin.Forms를 이전의 일부 Viewmodel 있을 수 있습니다 및 명령 인터페이스도 이미 지원 될 수 있습니다. Xamarin.Forms를 제공 하는 새 Viewmodel Xamarin.Forms로만 사용 하도록에 대 한 한 [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) 클래스 및 [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) 구현 하는 클래스는 `ICommand` 인터페이스입니다. 제네릭 형식에 대 한 인수 형식이 고 `Execute` 및 `CanExecute` 메서드.

### <a name="simple-method-executions"></a>간편 하 게 실행

[ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) 샘플에는 ViewModel 인터페이스를 사용 하는 방법을 보여 줍니다. [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) 클래스 형식의 두 가지 속성을 정의 `ICommand` 로 전달 하는 가장 간단한 두 개인 속성 정의 [ `Command` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/)합니다. 프로그램에이 ViewModel에서 데이터 바인딩을 포함는 `Command` 속성 두 `Button` 요소입니다.

`Button` 으로 요소를 쉽게 바꿀 수 `TapGestureRecognizer` 코드 변경 없이 XAML에서 개체입니다.

### <a name="a-calculator-almost"></a>계산기를 거의

[ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) 샘플을 사용 하면 둘 다의 사용은 `Execute` 및 `CanExecute` 의 메서드 `ICommand`합니다. 사용 하 여 프로그램 [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) 라이브러리입니다. ViewModel 형식의 속성 6 개가 포함 `ICommand`합니다. 여기에서 초기화 되는 [ `Command` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/) 및 [ `Command` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/) 의 `Command` 및 [ `Command<T>` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command%3CT%3E.Command%3CT%3E/p/System.Action%7BT%7D/System.Func%7BT,System.Boolean%7D/) `Command<T>`합니다. 추가 컴퓨터의 숫자 키 모두 사용 하 여 초기화 속성에 바인딩된 `Command<T>`, 및 `string` 인수를 `Execute` 및 `CanExecute` 특정 키를 식별 합니다.

## <a name="viewmodels-and-the-application-lifecycle"></a>Viewmodel 및 응용 프로그램 수명 주기

`AdderViewModel` 에 사용 되는 **AddingMachine** 샘플에는 또한 라는 두 개의 메서드를 정의 `SaveState` 및 `RestoreState`합니다. 이러한 메서드는 대기 상태로 전환 및 다시 시작할 때 응용 프로그램에서 호출 됩니다.



## <a name="related-links"></a>관련 링크

- [18 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [18 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
