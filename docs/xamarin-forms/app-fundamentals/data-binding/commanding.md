---
title: Xamarin.Forms 명령 인터페이스
description: 이 문서에서는 Xamarin.Forms 데이터 바인딩을 사용하여 Command 속성을 구현하는 방법을 설명합니다. 명령 인터페이스는 MVVM 아키텍처에 훨씬 더 적합한 명령을 구현하는 또 다른 방법을 제공합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 253255f08cec6f08e03df94798c8572f7cf10f30
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139726"
---
# <a name="the-xamarinforms-command-interface"></a>Xamarin.Forms 명령 인터페이스

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

MVVM(Model-View-ViewModel) 아키텍처에서 데이터 바인딩은 일반적으로 `INotifyPropertyChanged`에서 파생되는 클래스인 ViewModel의 속성과 일반적으로 XAML 파일인 View의 속성 간에 정의됩니다. 경우에 따라 애플리케이션에서 사용자에게 ViewModel의 어떤 항목에 영향을 주는 명령을 시작하도록 요구하여 이러한 속성 바인딩을 뛰어넘어야 합니다. 이러한 명령은 일반적으로 단추를 클릭하거나 손가락으로 탭하여 신호를 받으며, 대개 `Button`의 `Clicked` 이벤트 또는 `TapGestureRecognizer`의 `Tapped` 이벤트에 대한 처리기의 코드 숨김 파일에서 처리됩니다.

명령 인터페이스는 MVVM 아키텍처에 훨씬 더 적합한 명령을 구현하는 또 다른 방법을 제공합니다. ViewModel 자체는 `Button` 클릭과 같은 View(보기)의 특정 활동에 응답하여 실행되는 메서드인 명령을 포함할 수 있습니다. 데이터 바인딩은 이러한 명령과 `Button` 간에 정의됩니다.

`Button`과 ViewModel 간의 데이터 바인딩을 허용하려면 `Button`에서 다음 두 가지 속성을 정의합니다.

- [`Command`](xref:Xamarin.Forms.Button.Command)([`System.Windows.Input.ICommand`](xref:System.Windows.Input.ICommand) 형식)
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)(`Object` 형식)

명령 인터페이스를 사용하려면 `Button`의 `Command` 속성을 대상으로 하는 데이터 바인딩을 정의합니다. 여기서 원본은 ViewModel의 속성(`ICommand` 형식)입니다. ViewModel에는 단추가 클릭되면 실행되는 `ICommand` 속성과 연결되는 코드가 포함됩니다. 여러 단추가 모두 ViewModel의 동일한 `ICommand` 속성에 바인딩되는 경우 `CommandParameter`를 임의 데이터로 설정하여 여러 단추를 구분합니다.

또한 `Command` 및 `CommandParameter` 속성도 다음 클래스에서 정의됩니다.

- [`MenuItem`](xref:Xamarin.Forms.MenuItem) 및 이에 따라 `MenuItem`에서 파생된 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)
- [`TextCell`](xref:Xamarin.Forms.TextCell) 및 이에 따라 `TextCell`에서 파생된 [`ImageCell`](xref:Xamarin.Forms.ImageCell)
- [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)

[`SearchBar`](xref:Xamarin.Forms.SearchBar)는 [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) 속성(`ICommand` 형식) 및 [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) 속성을 정의합니다. [`ListView`](xref:Xamarin.Forms.ListView)의 [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) 속성도 `ICommand` 형식입니다.

이러한 모든 명령은 View의 특정 사용자 인터페이스 개체에 종속되지 않는 방식으로 ViewModel 내에서 처리할 수 있습니다.

## <a name="the-icommand-interface"></a>ICommand 인터페이스

[`System.Windows.Input.ICommand`](xref:System.Windows.Input.ICommand) 인터페이스는 Xamarin.Forms의 일부가 아닙니다. 대신 [System.Windows.Input](xref:System.Windows.Input) 네임스페이스에서 정의되며, 다음과 같이 두 개의 메서드와 하나의 이벤트로 구성됩니다.

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

명령 인터페이스를 사용하기 위해 ViewModel에 `ICommand` 형식의 속성이 포함됩니다.

```csharp
public ICommand MyCommand { private set; get; }
```

또한 ViewModel은 `ICommand` 인터페이스를 구현하는 클래스도 참조해야 합니다. 이 클래스는 곧 설명됩니다. View에서 `Button`의 `Command` 속성이 해당 속성에 바인딩됩니다.

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

사용자가 `Button`을 누르면 `Button`이 해당 `Command` 속성에 바인딩된 `ICommand` 개체에서 `Execute` 메서드를 호출합니다. 이는 명령 인터페이스의 가장 간단한 부분입니다.

`CanExecute` 메서드는 더 복잡합니다. 바인딩이 `Button`의 `Command` 속성에 처음 정의되고 데이터 바인딩이 어떤 방식으로든 변경되면 `Button`이 `ICommand` 개체의 `CanExecute` 메서드를 호출합니다. `CanExecute`에서 `false`가 반환되면 `Button` 자체를 사용하지 않도록 자동으로 설정됩니다. 이는 특정 명령이 현재 사용될 수 없거나 유효하지 않음을 나타냅니다.

또한 `Button`은 `ICommand`의 `CanExecuteChanged` 이벤트에 대한 처리기를 연결합니다. 이벤트는 ViewModel 내에서 발생합니다. 이 이벤트가 발생하면 `Button`에서 `CanExecute`를 다시 호출합니다. `CanExecute`에서 `true`가 반환되면 `Button` 자체를 사용하도록 설정되고, `CanExecute`에서 `false`가 반환되면 자체를 사용하지 않도록 설정됩니다.

> [!IMPORTANT]
> 명령 인터페이스를 사용하는 경우 `Button`의 `IsEnabled` 속성은 사용하지 마세요.  

## <a name="the-command-class"></a>Command 클래스

ViewModel에서 `ICommand` 형식의 속성을 정의하는 경우 ViewModel에서 `ICommand` 인터페이스를 구현하는 클래스도 포함하거나 참조해야 합니다. 이 클래스는 `Execute` 및 `CanExecute` 메서드를 포함하거나 참조해야 하며, `CanExecute` 메서드에서 다른 값을 반환할 수 있을 때마다 `CanExecuteChanged` 이벤트를 발생시킵니다.

이러한 클래스를 직접 작성할 수도 있고, 다른 사용자가 작성한 클래스를 사용할 수도 있습니다. `ICommand`는 Microsoft Windows의 일부이므로 Windows MVVM 애플리케이션에서 수년간 사용되어 왔습니다. `ICommand`를 구현하는 Windows 클래스를 사용하면 Windows 애플리케이션과 Xamarin.Forms 애플리케이션 간에 ViewModel을 공유할 수 있습니다.

Windows와 Xamarin.Forms 간에 ViewModel을 공유하는 것이 문제가 되지 않으면 Xamarin.Forms에 포함된 [`Command`](xref:Xamarin.Forms.Command) 또는 [`Command<T>`](xref:Xamarin.Forms.Command`1) 클래스를 사용하여 `ICommand` 인터페이스를 구현할 수 있습니다. 이러한 클래스를 사용하면 클래스 생성자에서 `Execute` 및 `CanExecute` 메서드의 본문을 지정할 수 있습니다. `CommandParameter` 속성을 사용하여 동일한 `ICommand` 속성에 바인딩된 여러 보기를 구분하는 경우 `Command<T>`를 사용하고, 이처럼 구분할 필요가 없는 경우 더 간단한 `Command` 클래스를 사용합니다.

## <a name="basic-commanding"></a>기본 명령

[**데이터 바인딩 데모**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos) 프로그램의 **Person Entry**(사용자 항목) 페이지는 ViewModel에서 구현된 몇 가지 간단한 명령을 보여 줍니다.

`PersonViewModel`은 사용자를 정의하는 `Name`, `Age` 및 `Skills`라는 세 가지 속성을 정의합니다. 이 클래스에는 `ICommand` 속성이 *포함되지 않습니다*.

```csharp
public class PersonViewModel : INotifyPropertyChanged
{
    string name;
    double age;
    string skills;

    public event PropertyChangedEventHandler PropertyChanged;

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public double Age
    {
        set { SetProperty(ref age, value); }
        get { return age; }
    }

    public string Skills
    {
        set { SetProperty(ref skills, value); }
        get { return skills; }
    }

    public override string ToString()
    {
        return Name + ", age " + Age;
    }

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

아래와 같은 `PersonCollectionViewModel`은 `PersonViewModel` 형식의 새 개체를 만들고 사용자가 데이터를 채울 수 있도록 합니다. 이를 위해 클래스는 `bool` 형식의 `IsEditing` 속성과 `PersonViewModel` 형식의 `PersonEdit` 속성을 정의합니다. 또한 이 클래스는 `ICommand` 형식의 세 가지 속성과 `IList<PersonViewModel>` 형식의 `Persons` 속성을 정의합니다.

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{
    PersonViewModel personEdit;
    bool isEditing;

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public bool IsEditing
    {
        private set { SetProperty(ref isEditing, value); }
        get { return isEditing; }
    }

    public PersonViewModel PersonEdit
    {
        set { SetProperty(ref personEdit, value); }
        get { return personEdit; }
    }

    public ICommand NewCommand { private set; get; }

    public ICommand SubmitCommand { private set; get; }

    public ICommand CancelCommand { private set; get; }

    public IList<PersonViewModel> Persons { get; } = new ObservableCollection<PersonViewModel>();

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

이 약식 목록에는 클래스의 생성자가 포함되지 않으며, 여기에는 `ICommand` 형식의 세 가지 속성이 정의되며 이러한 속성은 곧 표시됩니다. `ICommand` 형식의 세 가지 속성 및 `Persons` 속성이 변경되면 `PropertyChanged` 이벤트가 발생하지 않습니다. 클래스가 처음 만들어질 때 이러한 속성이 설정되며 그 이후에는 변경되지 않습니다.

`PersonCollectionViewModel` 클래스의 생성자를 검사하기 전에 **Person Entry** 프로그램의 XAML 파일을 살펴보겠습니다. 여기에는 해당 `BindingContext` 속성이 `PersonCollectionViewModel`로 설정된 `Grid`가 포함되어 있습니다. `Grid`에는 해당 `Command` 속성이 ViewModel의 `NewCommand` 속성에 바인딩되는 **New**(새로 만들기) 텍스트가 있는 `Button`, `IsEditing` 속성에 바인딩되는 속성과 `PersonViewModel`의 속성이 있는 입력 양식 및 ViewModel의 `SubmitCommand` 및 `CancelCommand`속성에 바인딩되는 두 개의 단추가 포함되어 있습니다. 마지막의 `ListView`에서 이미 입력된 사용자의 컬렉션을 표시합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.PersonEntryPage"
             Title="Person Entry">
    <Grid Margin="10">
        <Grid.BindingContext>
            <local:PersonCollectionViewModel />
        </Grid.BindingContext>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <!-- New Button -->
        <Button Text="New"
                Grid.Row="0"
                Command="{Binding NewCommand}"
                HorizontalOptions="Start" />

        <!-- Entry Form -->
        <Grid Grid.Row="1"
              IsEnabled="{Binding IsEditing}">

            <Grid BindingContext="{Binding PersonEdit}">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Label Text="Name: " Grid.Row="0" Grid.Column="0" />
                <Entry Text="{Binding Name}"
                       Grid.Row="0" Grid.Column="1" />

                <Label Text="Age: " Grid.Row="1" Grid.Column="0" />
                <StackLayout Orientation="Horizontal"
                             Grid.Row="1" Grid.Column="1">
                    <Stepper Value="{Binding Age}"
                             Maximum="100" />
                    <Label Text="{Binding Age, StringFormat='{0} years old'}"
                           VerticalOptions="Center" />
                </StackLayout>

                <Label Text="Skills: " Grid.Row="2" Grid.Column="0" />
                <Entry Text="{Binding Skills}"
                       Grid.Row="2" Grid.Column="1" />

            </Grid>
        </Grid>

        <!-- Submit and Cancel Buttons -->
        <Grid Grid.Row="2">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>

            <Button Text="Submit"
                    Grid.Column="0"
                    Command="{Binding SubmitCommand}"
                    VerticalOptions="CenterAndExpand" />

            <Button Text="Cancel"
                    Grid.Column="1"
                    Command="{Binding CancelCommand}"
                    VerticalOptions="CenterAndExpand" />
        </Grid>

        <!-- List of Persons -->
        <ListView Grid.Row="3"
                  ItemsSource="{Binding Persons}" />
    </Grid>
</ContentPage>
```

작동하는 방법을 다음과 같습니다. 사용자가 먼저 **새로 만들기** 단추를 누릅니다. 이렇게 하면 입력 양식은 사용할 수 있지만 **새로 만들기** 단추가 비활성화됩니다. 그러면 사용자가 이름, 나이 및 기술을 입력합니다. 사용자는 편집하는 중에 언제라도 **취소** 단추를 눌러 다시 시작할 수 있습니다. 이름과 유효 기간이 입력된 경우에만 **제출** 단추가 활성화됩니다. 이 **제출** 단추를 누르면 사용자를 `ListView`에서 표시하는 컬렉션으로 전송합니다. **취소** 또는 **제출** 단추를 누르면 입력 양식이 지워지고 **새로 만들기** 단추가 활성화됩니다.

왼쪽의 iOS 화면에서는 유효한 나이를 입력하기 전의 레이아웃을 보여 줍니다. Android 화면에는 나이를 설정하면 활성화되는 **제출** 단추가 표시됩니다.

[![개인 항목](commanding-images/personentry-small.png "개인 항목")](commanding-images/personentry-large.png#lightbox "개인 항목")

프로그램에는 기존 항목을 편집할 수 있는 기능이 없으며, 페이지에서 이동할 때 항목을 저장하지 않습니다.

**새로 만들기**, **제출** 및 **취소** 단추에 대한 모든 논리는 `NewCommand`, `SubmitCommand` 및 `CancelCommand` 속성의 정의를 통해 `PersonCollectionViewModel`에서 처리됩니다. `PersonCollectionViewModel`의 생성자는 이러한 세 가지 속성을 `Command` 형식의 개체로 설정합니다.  

`Command` 클래스의 [생성자](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean}))를 사용하면 `Execute` 및 `CanExecute` 메서드에 해당하는 `Action` 및 `Func<bool>` 형식의 인수를 전달할 수 있습니다. 이러한 작업과 함수를 `Command` 생성자에서 직접 람다 함수로 정의하는 것이 가장 쉽습니다. `NewCommand` 속성에 대한 `Command` 개체의 정의는 다음과 같습니다.

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {
        NewCommand = new Command(
            execute: () =>
            {
                PersonEdit = new PersonViewModel();
                PersonEdit.PropertyChanged += OnPersonEditPropertyChanged;
                IsEditing = true;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return !IsEditing;
            });

        ···

    }

    void OnPersonEditPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        (SubmitCommand as Command).ChangeCanExecute();
    }

    void RefreshCanExecutes()
    {
        (NewCommand as Command).ChangeCanExecute();
        (SubmitCommand as Command).ChangeCanExecute();
        (CancelCommand as Command).ChangeCanExecute();
    }

    ···

}
```

사용자가 **새로 만들기** 단추를 클릭하면 `Command` 생성자에 전달된 `execute` 함수가 실행됩니다. 이렇게 하면 새 `PersonViewModel` 개체가 만들어지고, 해당 개체의 `PropertyChanged` 이벤트에 대한 처리기가 설정되고, `IsEditing`이 `true`로 설정되고, 생성자 뒤에 정의된 `RefreshCanExecutes` 메서드가 호출됩니다.

또한 `Command` 클래스는 `ICommand` 인터페이스를 구현하는 것 외에도 `ChangeCanExecute`라는 메서드를 정의합니다. `CanExecute` 메서드의 반환 값이 변경될 수 있는 상황이 발생할 때마다 ViewModel에서 `ICommand` 속성에 대한 `ChangeCanExecute`를 호출해야 합니다. `ChangeCanExecute`에 대한 호출로 인해 `Command` 클래스에서 `CanExecuteChanged` 메서드를 실행합니다. `Button`은 해당 이벤트에 대한 처리기를 연결하고, `CanExecute`를 다시 호출한 후 해당 메서드의 반환 값에 따라 자체를 활성화하여 응답합니다.

`NewCommand`의 `execute` 메서드에서 `RefreshCanExecutes`를 호출하면 `NewCommand` 속성이 `ChangeCanExecute`에 대한 호출을 가져오고, `Button`이 `canExecute` 메서드를 호출하고, `IsEditing` 속성이 현재 `true`이므로 `false`를 반환합니다.

새 `PersonViewModel` 개체에 대한 `PropertyChanged` 처리기에서 `SubmitCommand`의 `ChangeCanExecute` 메서드를 호출합니다. 명령 속성이 구현되는 방법은 다음과 같습니다.

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        SubmitCommand = new Command(
            execute: () =>
            {
                Persons.Add(PersonEdit);
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return PersonEdit != null &&
                       PersonEdit.Name != null &&
                       PersonEdit.Name.Length > 1 &&
                       PersonEdit.Age > 0;
            });

        ···
    }

    ···

}
```

`SubmitCommand`에 대한 `canExecute` 함수는 편집하는 `PersonViewModel` 개체에서 변경되는 속성이 있을 때마다 호출됩니다. `Name` 속성이 하나 이상의 문자이고 `Age`가 0보다 큰 경우에만 `true`를 반환합니다. 이때 **제출** 단추가 활성화됩니다.

**제출**에 대한 `execute` 함수는 `PersonViewModel`에서 속성이 변경된 처리기를 제거하고, 개체를 `Persons` 컬렉션에 추가한 다음, 모든 항목을 초기 조건으로 반환합니다.

**취소** 단추에 대한 `execute` 함수는 **제출** 단추에서 개체를 컬렉션에 추가하는 것을 제외하고 수행하는 모든 작업을 수행합니다.

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        CancelCommand = new Command(
            execute: () =>
            {
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return IsEditing;
            });
    }

    ···

}
```

`PersonViewModel`이 편집될 때마다 `canExecute` 메서드에서 `true`를 반환합니다.

이러한 기술은 좀 더 복잡한 시나리오에 적용할 수 있습니다. 기존 항목 편집을 위해 `PersonCollectionViewModel`의 속성을 `ListView`의 `SelectedItem` 속성에 바인딩할 수 있고, 이러한 항목을 삭제하기 위해 **삭제** 단추를 추가할 수 있습니다.

`execute` 및 `canExecute` 메서드를 람다 함수로 정의할 필요는 없습니다. 이러한 메서드를 ViewModel의 일반 개인 메서드로 작성하고 `Command` 생성자에서 이를 참조할 수 있습니다. 그러나 이 방법은 ViewModel에서 한 번만 참조되는 메서드가 많이 발생하는 경향이 있습니다.

## <a name="using-command-parameters"></a>Command 매개 변수 사용

하나 이상의 단추(또는 다른 사용자 인터페이스 개체)가 ViewModel에서 동일한 `ICommand` 속성을 공유하는 것이 편리한 경우가 있습니다. 이 경우 `CommandParameter` 속성을 사용하여 단추를 구분합니다.

`Command` 클래스는 이러한 공유 `ICommand` 속성에 대해 계속 사용할 수 있습니다. 이 클래스는 `Object` 형식의 매개 변수가 있는 `execute` 및 `canExecute` 메서드를 허용하는 [대체 생성자](xref:Xamarin.Forms.Command.%23ctor(System.Action{System.Object},System.Func{System.Object,System.Boolean}))를 정의합니다. 이는 `CommandParameter`가 이러한 메서드에 전달되는 방법입니다.

그러나 `CommandParameter`를 사용하는 경우 제네릭 [`Command<T>`](xref:Xamarin.Forms.Command`1) 클래스를 사용하여 `CommandParameter`로 설정된 개체 형식을 지정하는 것이 가장 쉽습니다. 지정한 `execute` 및 `canExecute` 메서드에는 해당 형식의 매개 변수가 있습니다.

**10진수 키보드** 페이지에서는 10진수를 입력할 수 있는 키패드를 구현하는 방법을 보여줌으로써 이 기술을 보여 줍니다. `Grid`에 대한 `BindingContext`는 `DecimalKeypadViewModel`입니다. 이 ViewModel의 `Entry` 속성은 `Label`의 `Text` 속성에 바인딩됩니다. 모든 `Button` 개체는 ViewModel의 다양한 명령(`ClearCommand`, `BackspaceCommand` 및 `DigitCommand`)에 바인딩됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.DecimalKeypadPage"
             Title="Decimal Keyboard">

    <Grid WidthRequest="240"
          HeightRequest="480"
          ColumnSpacing="2"
          RowSpacing="2"
          HorizontalOptions="Center"
          VerticalOptions="Center">

        <Grid.BindingContext>
            <local:DecimalKeypadViewModel />
        </Grid.BindingContext>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Button">
                    <Setter Property="FontSize" Value="32" />
                    <Setter Property="BorderWidth" Value="1" />
                    <Setter Property="BorderColor" Value="Black" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Label Text="{Binding Entry}"
               Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3"
               FontSize="32"
               LineBreakMode="HeadTruncation"
               VerticalTextAlignment="Center"
               HorizontalTextAlignment="End" />

        <Button Text="CLEAR"
                Grid.Row="1" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding ClearCommand}" />

        <Button Text="&#x21E6;"
                Grid.Row="1" Grid.Column="2"
                Command="{Binding BackspaceCommand}" />

        <Button Text="7"
                Grid.Row="2" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="7" />

        <Button Text="8"
                Grid.Row="2" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="8" />

        <Button Text="9"
                Grid.Row="2" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="9" />

        <Button Text="4"
                Grid.Row="3" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="4" />

        <Button Text="5"
                Grid.Row="3" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="5" />

        <Button Text="6"
                Grid.Row="3" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="6" />

        <Button Text="1"
                Grid.Row="4" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="1" />

        <Button Text="2"
                Grid.Row="4" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="2" />

        <Button Text="3"
                Grid.Row="4" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="3" />

        <Button Text="0"
                Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding DigitCommand}"
                CommandParameter="0" />

        <Button Text="&#x00B7;"
                Grid.Row="5" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="." />
    </Grid>
</ContentPage>
```

10개 숫자 및 소수점에 대한 11개 단추는 `DigitCommand`에 대한 바인딩을 공유합니다. `CommandParameter`는 이러한 단추를 구분합니다. `CommandParameter`로 설정된 값은 일반적으로 소수점을 제외하고는 단추에서 표시하는 텍스트와 동일하며, 명확히 하기 위해 중간 점 문자로 표시됩니다.

실행 중인 프로그램은 다음과 같습니다.

[![음악 키보드](commanding-images/decimalkeyboard-small.png "음악 키보드")](commanding-images/decimalkeyboard-large.png#lightbox "음악 키보드")

입력된 숫자에 이미 소수점이 포함되어 있으므로 세 개의 스크린샷 모두에서 소수점 단추가 비활성화되어 있습니다.

`DecimalKeypadViewModel`은 `string` 형식의 `Entry` 속성(`PropertyChanged` 이벤트를 트리거하는 유일한 속성)과 `ICommand` 형식의 세 가지 속성을 정의합니다.

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{
    string entry = "0";

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public string Entry
    {
        private set
        {
            if (entry != value)
            {
                entry = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Entry"));
            }
        }
        get
        {
            return entry;
        }
    }

    public ICommand ClearCommand { private set; get; }

    public ICommand BackspaceCommand { private set; get; }

    public ICommand DigitCommand { private set; get; }
}
```

`ClearCommand`에 해당하는 단추는 항상 활성화되며, 단순히 항목을 "0"으로 다시 설정합니다.

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {
        ClearCommand = new Command(
            execute: () =>
            {
                Entry = "0";
                RefreshCanExecutes();
            });

        ···

    }

    void RefreshCanExecutes()
    {
        ((Command)BackspaceCommand).ChangeCanExecute();
        ((Command)DigitCommand).ChangeCanExecute();
    }

    ···

}
```

단추가 항상 활성화되므로 `Command` 생성자에서 `canExecute` 인수를 지정할 필요가 없습니다.

숫자가 입력되지 않으면 `Entry` 속성이 "0" 문자열이 되므로 숫자와 백스페이스를 입력하는 논리는 약간 까다롭습니다. 사용자가 0을 더 많이 입력하면 `Entry`에 하나의 0만 계속 포함됩니다. 사용자가 다른 숫자를 입력하면 해당 숫자가 0을 대체합니다. 그러나 사용자가 다른 숫자 앞에 소수점을 입력하면 `Entry`가 "0" 문자열이 됩니다.

항목의 길이가 1보다 크거나 `Entry`가 "0" 문자열과 같지 않은 경우에만 **백스페이스** 단추가 활성화됩니다.

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        BackspaceCommand = new Command(
            execute: () =>
            {
                Entry = Entry.Substring(0, Entry.Length - 1);
                if (Entry == "")
                {
                    Entry = "0";
                }
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return Entry.Length > 1 || Entry != "0";
            });

        ···

    }

    ···

}
```

**백스페이스** 단추에 대한 `execute` 함수의 논리는 `Entry`가 적어도 "0" 문자열임을 보장합니다.

`DigitCommand` 속성은 각각 `CommandParameter` 속성으로 식별되는 11개의 단추에 바인딩됩니다. `DigitCommand`는 일반 `Command` 클래스의 인스턴스로 설정할 수 있지만, `Command<T>` 제네릭 클래스를 사용하는 것이 더 쉽습니다. XAML에서 명령 인터페이스를 사용하는 경우 `CommandParameter` 속성은 일반적으로 문자열이며, 제네릭 인수 형식입니다. 그러면 `execute` 및 `canExecute` 함수에 `string` 형식의 인수가 있습니다.

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        DigitCommand = new Command<string>(
            execute: (string arg) =>
            {
                Entry += arg;
                if (Entry.StartsWith("0") && !Entry.StartsWith("0."))
                {
                    Entry = Entry.Substring(1);
                }
                RefreshCanExecutes();
            },
            canExecute: (string arg) =>
            {
                return !(arg == "." && Entry.Contains("."));
            });
    }

    ···

}
```

`execute` 메서드는 문자열 인수를 `Entry` 속성에 추가합니다. 그러나 결과가 0으로 시작하는 경우(0 및 소수점이 아님) `Substring` 함수를 사용하여 초기 0을 제거해야 합니다.

인수가 소수점이고(소수점을 눌렀는지 나타냄) `Entry`에 이미 소수점이 포함되어 있는 경우에만 `canExecute` 메서드에서 `false`를 반환합니다.

모든 `execute` 메서드는 `RefreshCanExecutes`를 호출한 다음, `DigitCommand` 및 `ClearCommand` 모두에 대해 `ChangeCanExecute`를 호출합니다. 이렇게 하면 입력된 숫자의 현재 순서에 따라 소수점 및 백스페이스 단추가 활성화 또는 비활성화되도록 보장합니다.

## <a name="adding-commands-to-existing-views"></a>기존 보기에 명령 추가

명령 인터페이스를 지원하지 않는 보기에서 명령 인터페이스를 사용하려면 이벤트를 명령으로 변환하는 Xamarin.Forms 동작을 사용하면 됩니다. 이에 대해 [**재사용 가능한 EventToCommandBehavior**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md) 문서에서 설명하고 있습니다.

## <a name="asynchronous-commanding-for-navigation-menus"></a>비동기 탐색 메뉴 명령

명령은 [**데이터 바인딩 데모**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos) 프로그램 자체에서와 같은 탐색 메뉴를 구현하는 데 유용합니다. **MainPage.xaml**의 일부는 다음과 같습니다.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.MainPage"
             Title="Data Binding Demos"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection Title="Basic Bindings">

                <TextCell Text="Basic Code Binding"
                          Detail="Define a data-binding in code"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicCodeBindingPage}" />

                <TextCell Text="Basic XAML Binding"
                          Detail="Define a data-binding in XAML"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicXamlBindingPage}" />

                <TextCell Text="Alternative Code Binding"
                          Detail="Define a data-binding in code without a BindingContext"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:AlternativeCodeBindingPage}" />

                ···

            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

XAML에서 명령을 사용하는 경우 `CommandParameter` 속성은 일반적으로 문자열로 설정됩니다. 그러나 이 경우 XAML 태그 확장을 사용하여 `System.Type` 형식의 `CommandParameter`가 되도록 합니다.

각 `Command` 속성은 `NavigateCommand`이라는 속성에 바인딩됩니다. 이 속성은 코드 숨김 파일인 **MainPage.xaml.cs**에 정의되어 있습니다.

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(
            async (Type pageType) =>
            {
                Page page = (Page)Activator.CreateInstance(pageType);
                await Navigation.PushAsync(page);
            });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

생성자는 `NavigateCommand` 속성을 `execute` 메서드로 설정하여 `System.Type` 매개 변수를 인스턴스화한 다음, 해당 항목으로 이동합니다. `PushAsync` 호출에 `await` 연산자가 필요하므로 `execute` 메서드를 비동기 플래그로 지정해야 합니다. 이는 매개 변수 목록 앞에 있는 `async` 키워드를 사용하여 완성됩니다.

또한 생성자는 바인딩에서 이 클래스의 `NavigateCommand`를 참조하도록 페이지의 `BindingContext`를 자체로 설정합니다.

이 생성자의 코드 순서가 중요합니다. `InitializeComponent` 호출로 인해 XAML이 구문 분석되지만 `BindingContext`가 `null`로 설정되어 있으므로 이때에는 `NavigateCommand`라는 속성에 대한 바인딩을 확인할 수 없습니다. `NavigateCommand`이(가) 설정되기 *전에* `BindingContext`이(가) 생성자에서 설정되면 `BindingContext`이(가) 설정될 때 바인딩을 확인할 수 있지만, 이 시점에서 `NavigateCommand`은(는) 여전히 `null`입니다. `NavigateCommand`를 변경하면 `PropertyChanged` 이벤트가 발생하지 않고 바인딩에서 `NavigateCommand`가 현재 유효한지 인식할 수 없으므로 `BindingContext` 뒤에 `NavigateCommand`를 설정해도 바인딩에 아무 영향을 주지 않습니다.

XAML 파서에서 바인딩 정의를 발견하면 바인딩의 두 구성 요소가 모두 설정되므로 `InitializeComponent`를 호출하기 전에 `NavigateCommand` 및 `BindingContext`를 모두 임의의 순서로 설정할 수 있습니다.

데이터 바인딩은 까다로운 경우도 있지만, 이 시리즈 문서에서 살펴본 대로 강력하고 다양하며, 사용자 인터페이스와 기본 논리를 분리하여 코드를 구성하는 데 크게 도움이 됩니다.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Xamarin.Forms 책의 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
