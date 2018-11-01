---
title: Xamarin.Forms 명령 인터페이스
description: 이 문서에서는 Xamarin.Forms 데이터 바인딩을 사용 하 여 명령 속성을 구현 하는 방법에 설명 합니다. 명령 인터페이스는 MVVM 아키텍처에 훨씬 더 적합 명령을 구현 하는 데 또 다른 방법을 제공 합니다.
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 68c7869254ae861cef8307431d925368082be921
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675258"
---
# <a name="the-xamarinforms-command-interface"></a>Xamarin.Forms 명령 인터페이스

파생 된 클래스는 일반적으로 ViewModel의 속성 간에 정의 된 데이터 바인딩 모델-뷰-ViewModel (MVVM) 아키텍처에서는 `INotifyPropertyChanged`, 및 XAML 파일이 일반적으로 뷰에서 속성입니다. 응용 프로그램에서 ViewModel에 영향을 주는 명령을 시작 하려면 사용자를 요구 하 여 이러한 속성 바인딩 보다 우수한 요구 하는 경우가 있습니다. 및 코드 숨김 파일에 대 한 처리기에서 처리 되는 일반적으로 이러한 명령 단추를 클릭 하 여 일반적으로 신호 또는 누르기, 손가락으로 `Clicked` 의 이벤트를 `Button` 또는 `Tapped` 이벤트를 `TapGestureRecognizer`합니다.

명령 인터페이스는 MVVM 아키텍처에 훨씬 더 적합 명령을 구현 하는 데 또 다른 방법을 제공 합니다. ViewModel 자체와 같은 보기에서 특정 활동에 대 한 응답에서 실행 되는 메서드는 명령이 포함 되어는 `Button` 클릭 합니다. 이러한 명령 사이 데이터 바인딩이 정의 되어 및 `Button`합니다.

간의 데이터 바인딩을 허용 하는 `Button` 및 ViewModel을 `Button` 두 속성을 정의:

- [`Command`](xref:Xamarin.Forms.Button.Command) 형식의 [`System.Windows.Input.ICommand`](xref:System.Windows.Input.ICommand)
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) 형식의 `Object`

명령 인터페이스를 사용 하려면 대상으로 하는 데이터 바인딩을 정의 `Command` 의 속성을 `Button` 여기서 소스는 형식의 ViewModel의 속성 `ICommand`합니다. 연결 된 코드를 포함 하는 ViewModel `ICommand` 단추를 클릭할 때 실행 되는 속성입니다. 설정할 수 있습니다 `CommandParameter` 동일 하 게 바인딩된 모든 경우에 여러 단추를 구분 하기 위해 임의의 데이터 `ICommand` ViewModel의 속성입니다.

합니다 `Command` 및 `CommandParameter` 속성 다음 클래스에 의해 정의 됩니다.

- [`MenuItem`](xref:Xamarin.Forms.MenuItem) 및 따라서 [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem)에서 파생 되는 `MenuItem`
- [`TextCell`](xref:Xamarin.Forms.TextCell) 및 따라서 [ `ImageCell` ](xref:Xamarin.Forms.ImageCell)에서 파생 되는 `TextCell`
- [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)

[`SearchBar`](xref:Xamarin.Forms.SearchBar) 정의 된 [ `SearchCommand` ](xref:Xamarin.Forms.SearchBar.SearchCommand) 형식의 속성 `ICommand` 와 [ `SearchCommandParameter` ](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) 속성입니다. 합니다 [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand) 속성을 [ `ListView` ](xref:Xamarin.Forms.ListView) 형식의 이기도 `ICommand`합니다.

이러한 명령은 모든 보기에서 특정 사용자 인터페이스 개체에 의존 하지 않는 방식으로 ViewModel 내에서 처리할 수 있습니다.

## <a name="the-icommand-interface"></a>ICommand 인터페이스

합니다 [ `System.Windows.Input.ICommand` ](xref:System.Windows.Input.ICommand) 인터페이스 Xamarin.Forms의 일부가 아닙니다. 대신에 정의 되어는 [System.Windows.Input](xref:System.Windows.Input) 네임 스페이스 고 두 메서드 및 이벤트 구성:

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

명령 인터페이스를 사용 하 여 ViewModel 형식의 속성이 포함 된 `ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

ViewModel 클래스를 구현 하는 참조 해야 합니다 `ICommand` 인터페이스입니다. 이 클래스는 곧 설명 합니다. 뷰에서 `Command` 의 속성을 `Button` 해당 속성에 바인딩된:

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

누를 때를 `Button`, `Button` 호출을 `Execute` 에서 메서드를 `ICommand` 개체에 바인딩된 해당 `Command` 속성입니다. 명령 인터페이스의 가장 간단한 부분입니다.

`CanExecute` 메서드는 더 복잡 합니다. 바인딩이에서 처음 정의 된 경우는 `Command` 의 속성을 `Button`, 어떤 방식으로든에서 데이터 바인딩이 변경 되 면 및를 `Button` 호출을 `CanExecute` 에서 메서드를 `ICommand` 개체입니다. 하는 경우 `CanExecute` 반환 `false`, 그런 다음 `Button` 자체를 사용 하지 않도록 설정 합니다. 이 특정 명령을 사용할 수 없거나 잘못 되었습니다. 현재 임을 나타냅니다.

`Button` 에 처리기를 연결 합니다 `CanExecuteChanged` 이벤트를 `ICommand`입니다. ViewModel 내에서 이벤트가 발생 합니다. 해당 이벤트가 발생 하는 경우는 `Button` 호출 `CanExecute` 다시 합니다. 합니다 `Button` 경우 자체적으로 활성화 `CanExecute` 반환 `true` 경우 자체적으로 비활성화 하 고 `CanExecute` 반환 `false`합니다.

> [!IMPORTANT]
> 사용 하지 않는 합니다 `IsEnabled` 속성의 `Button` 명령 인터페이스를 사용 하는 경우.  

## <a name="the-command-class"></a>명령 클래스

프로그램 ViewModel 형식의 속성을 정의 하는 경우 `ICommand`, ViewModel 포함 하거나 구현 하는 클래스를 참조 해야 합니다도 합니다 `ICommand` 인터페이스입니다. 이 클래스를 포함 하거나 참조 해야 합니다는 `Execute` 하 고 `CanExecute` 메서드 및 화재를 `CanExecuteChanged` 이벤트 때마다는 `CanExecute` 메서드는 다른 값을 반환할 수 있습니다.

이러한 클래스를 직접 작성할 수 있습니다 또는 다른 사용자가 기록 하는 클래스를 사용할 수 있습니다. 때문에 `ICommand` 일부인 Windows MVVM 응용 프로그램을 사용 하 여 년 동안 사용 되 고 Microsoft Windows입니다. 구현 하는 Windows 클래스를 사용 하 여 `ICommand` Viewmodel Windows 응용 프로그램 및 Xamarin.Forms 응용 프로그램 간에 공유할 수 있습니다.

Windows 및 Xamarin.Forms 간의 Viewmodel을 공유 하지 않아도 되는, 경우 사용할 수는 [ `Command` ](xref:Xamarin.Forms.Command) 또는 [ `Command<T>` ](xref:Xamarin.Forms.Command`1) 를구현하는Xamarin.Forms에포함된클래스`ICommand`인터페이스입니다. 이러한 클래스를 사용 하면 본문을 지정 하는 `Execute` 및 `CanExecute` 클래스 생성자에 메서드. 사용 하 여 `Command<T>` 사용 하는 경우는 `CommandParameter` 속성 여러 뷰 사이 구분을 동일 하 게 바인딩된 `ICommand` 속성을 보다 단순한 `Command` 요구 되지 않는 클래스입니다.

## <a name="basic-commanding"></a>기본 명령

**Person 항목** 페이지에 [ **데이터 바인딩 데모** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) 프로그램 ViewModel에 구현 된 몇 가지 간단한 명령을 보여 줍니다.

합니다 `PersonViewModel` 라는 세 가지 속성을 정의 `Name`를 `Age`, 및 `Skills` 사람을 정의 하는 합니다. 이 클래스는 *되지* 포함할 `ICommand` 속성:

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

합니다 `PersonCollectionViewModel` 표시 다음 형식의 새 개체를 만듭니다 `PersonViewModel` 데이터를 입력 하 고 사용자 수 있도록 합니다. 이 위해 클래스 속성을 정의 `IsEditing` 형식의 `bool` 하 고 `PersonEdit` 형식의 `PersonViewModel`합니다. 클래스 형식의 세 가지 속성을 정의 하는 또한 `ICommand` 속성 및 이름이 `Persons` 형식의 `IList<PersonViewModel>`:

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

약식된이 목록에는 클래스의 생성자를 포함 되지 않습니다 유형의 세 가지 속성 `ICommand` 정의 되는 곧 표시 됩니다. 세 가지 유형의 속성을 변경 `ICommand` 하며 `Persons` 속성에서 발생 하지 `PropertyChanged` 발생 하는 이벤트입니다. 이러한 속성은 모든 집합 클래스를 처음 만들 때 및 이후 변경 되지 않습니다.

생성자를 살펴보기 전에 `PersonCollectionViewModel` 클래스를 XAML 파일을 살펴보겠습니다 합니다 **Person 항목** 프로그램. 여기에 포함 됩니다는 `Grid` 사용 하 여 해당 `BindingContext` 속성이로 설정 합니다 `PersonCollectionViewModel`합니다. `Grid` 포함를 `Button` 텍스트를 사용 하 여 **새로 만들기** 사용 하 여 해당 `Command` 속성에 바인딩된를 `NewCommand` ViewModel의 속성에 바인딩된 속성을 사용 하 여 입력 폼을 `IsEditing` 속성으로 속성으로 잘 `PersonViewModel`에 바인딩된 두 개의 추가 단추 및 합니다 `SubmitCommand` 및 `CancelCommand` ViewModel의 속성입니다. 최종 `ListView` 이미 입력 한 사용자의 컬렉션을 표시 합니다.

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

작동 하는 방법을 다음과 같습니다: 첫 번째 누르면 합니다 **새로 만들기** 단추입니다. 이 입력 폼을 사용 하도록 설정 되지만 사용 하지 않도록 설정 합니다 **새로 만들기** 단추입니다. 사용자 이름, 나가 및 기술을 입력 합니다. 사용자를 편집 하는 동안 언제 든 누를 수는 **취소** 단추를 다시 시작 합니다. 이름 및 유효한 시간 입력 된만 합니다 **제출** 단추를 사용할 수 있습니다. 이 키를 눌러 **제출** 단추를 표시 하 여 컬렉션에 사용자를 전송 합니다 `ListView`합니다. 후 합니다 **취소** 또는 **제출** 단추가 눌러져, 입력 양식 선택이 취소 되어 및 **새로 만들기** 단추가 다시 활성화 됩니다.

유효한 age를 시작 하기 전까지 왼쪽에 있는 iOS 화면 레이아웃을 보여 줍니다. Android 및 UWP 화면 표시를 **제출** 나가 설정 된 후 사용 하도록 설정 하는 단추:

[![사용자 항목](commanding-images/personentry-small.png "Person 항목")](commanding-images/personentry-large.png#lightbox "사용자 항목")

프로그램에 기존 항목을 편집 하기 위한 모든 기능 없고 페이지 외부로 이동할 때 항목을 저장 하지 않습니다.

에 대 한 모든 논리는 **새로 만들기**, **제출**, 및 **취소** 단추에서 처리 됩니다 `PersonCollectionViewModel` 의 정의 통해 합니다 `NewCommand`, `SubmitCommand`, 및 `CancelCommand` 속성입니다. 생성자는 `PersonCollectionViewModel` 형식의 개체에 이러한 세 가지 속성을 설정 `Command`합니다.  

[생성자](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) 의 `Command` 클래스 형식의 인수를 전달할 수 있습니다 `Action` 하 고 `Func<bool>` 해당 하는 `Execute` 및 `CanExecute` 메서드. 람다에 바로 함수를 이러한 작업 및 함수를 정의 하는 것이 쉽습니다는 `Command` 생성자입니다. 정의 같습니다. 합니다 `Command` 개체는 `NewCommand` 속성:

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

클릭할 때를 **새로 만들기** 단추를 `execute` 함수에 전달 된를 `Command` 생성자가 실행 되 합니다. 이 만듭니다 `PersonViewModel` 개체, 해당 개체에 대해 처리기를 설정 합니다 `PropertyChanged` 이벤트를 설정 `IsEditing` 에 `true`, 호출 및를 `RefreshCanExecutes` 생성자 뒤에 정의 된 메서드.

구현 하는 것 외에도 합니다 `ICommand` 인터페이스를 `Command` 클래스 라는 메서드도 정의 `ChangeCanExecute`합니다. ViewModel에 호출 해야 `ChangeCanExecute` 에 대 한는 `ICommand` 아무 것도 발생 하는 속성의 반환 값을 변경 될 수 있습니다는 `CanExecute` 메서드. 에 대 한 호출 `ChangeCanExecute` 하면 합니다 `Command` 시키는 클래스를 `CanExecuteChanged` 메서드. 합니다 `Button` 가 해당 이벤트에 대 한 처리기를 연결 하 고 호출 하 여 응답 `CanExecute` 다시 해당 메서드의 반환 값에 따라 사용 하도록 설정 합니다.

경우는 `execute` 메서드의 `NewCommand` 호출 `RefreshCanExecutes`의 `NewCommand` 속성에 대 한 호출을 가져옵니다 `ChangeCanExecute`, 및 `Button` 호출 합니다 `canExecute` 이제 반환 하는 메서드 `false` 때문에 합니다 `IsEditing`속성은 이제 `true`합니다.

합니다 `PropertyChanged` 새 처리기 `PersonViewModel` 호출 개체의 `ChangeCanExecute` 메서드의 `SubmitCommand`합니다. 해당 명령 속성은 구현 하는 방법을 다음과 같습니다.


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

`canExecute` 함수에 대 한 `SubmitCommand` 속성이 변경 될 때마다 호출 되는 `PersonViewModel` 편집 중인 개체입니다. 반환 `true` 경우에만 합니다 `Name` 속성은 최소 1 자 및 `Age` 0 보다 큽니다. 이때 합니다 **제출** 단추가 활성화 됩니다.

`execute` 함수에 대 한 **제출** 에서 속성 변경 처리기를 제거 합니다 `PersonViewModel`에 개체를 추가 `Persons` 컬렉션 초기 조건에 모두를 반환 합니다.

`execute` 함수는 **취소** 단추 모든 작업을 수행 하는 합니다 **제출** 단추를 사용 하는 제외 하 고 개체 컬렉션에 추가:

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

`canExecute` 메서드가 반환 되는 `true` 언제 든 지는 `PersonViewModel` 있기 때문입니다.

이러한 기술을 보다 복잡 한 시나리오에 적용할 수 있습니다:에서 속성 `PersonCollectionViewModel` 에 바인딩할 수 없습니다를 `SelectedItem` 의 속성을 `ListView` 기존 항목을 편집 하는 것에 대 한 및 **삭제** 삭제에 단추를 추가할 수 있습니다 이러한 항목입니다.

정의할 필요가 없습니다 합니다 `execute` 및 `canExecute` 람다 함수로 메서드. ViewModel에서 일반 개인 메서드를 작성 하 고에 참조할 수 있습니다는 `Command` 생성자입니다. 그러나이 이렇게 않습니다 ViewModel에서 한 번만 참조 하는 메서드가 많이 발생 하는 경향이 있습니다.

## <a name="using-command-parameters"></a>명령 매개 변수를 사용 하 여

동일한 공유를 하나 이상의 단추 (또는 다른 사용자 인터페이스 개체)에 대 한 편리한 경우가 `ICommand` ViewModel의 속성입니다. 사용 하는 경우에 `CommandParameter` 단추를 구분 하는 속성입니다.

계속 사용할 수 있습니다 합니다 `Command` 이러한 공유에 대 한 클래스 `ICommand` 속성입니다. 클래스 정의 [대체 생성자](xref:Xamarin.Forms.Command.%23ctor(System.Action{System.Object},System.Func{System.Object,System.Boolean})) 받아들이는 `execute` 하 고 `canExecute` 형식의 매개 변수를 사용 하 여 메서드 `Object`. 이 방법을 `CommandParameter` 이러한 메서드에 전달 됩니다.

그러나 사용 하는 경우 `CommandParameter`, 제네릭을 사용 하는 것이 쉽습니다 [ `Command<T>` ](xref:Xamarin.Forms.Command`1) 로 설정 하는 개체의 유형을 지정 하는 클래스 `CommandParameter`합니다. 합니다 `execute` 및 `canExecute` 지정 하는 방법에는 해당 형식의 매개 변수가 있습니다.

합니다 **10 진수 키보드** 페이지 입력 10 진수 숫자 키패드를 구현 하는 방법을 표시 하 여이 기법을 보여 줍니다. 합니다 `BindingContext` 에 대 한는 `Grid` 는 `DecimalKeypadViewModel`합니다. `Entry` 이 ViewModel의 속성에 바인딩되어 합니다 `Text` 의 속성을 `Label`. 모든 합니다 `Button` ViewModel에 다양 한 명령에 바인딩된 개체에는: `ClearCommand`를 `BackspaceCommand`, 및 `DigitCommand`:

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

10 개의 숫자와 소수점 11 단추가 공유에 대 한 바인딩을 `DigitCommand`합니다. `CommandParameter` 이러한 단추를 구분 합니다. 에 설정 된 값 `CommandParameter` 는 일반적으로 소수점 명확히 전달 하기 위해 중간 점 문자를 사용 하 여 표시 되는 제외 하 고 단추에 표시 되는 텍스트와 동일 합니다.

작업에서 프로그램이 다음과 같습니다.

[![10 진수 키보드](commanding-images/decimalkeyboard-small.png "소수 키보드")](commanding-images/decimalkeyboard-large.png#lightbox "10 진수 키보드")

소수점을 입력 한 수에 이미 포함 되어 있으므로 모든 스크린샷 세 개에서 소수점에 대 한 단추는 비활성화를 확인 합니다.

`DecimalKeypadViewModel` 정의 `Entry` 형식의 속성 `string` (트리거하는 유일한 속성 되는 `PropertyChanged` 이벤트) 및 유형의 세 가지 속성 `ICommand`:

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

에 해당 하는 단추는 `ClearCommand` 항상 활성화 되어 있으며 다시 "0"으로 항목을 설정 합니다.

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

지정할 필요가 없는 단추를 항상 사용 하기 때문에 `canExecute` 인수에는 `Command` 생성자입니다.

때문에 숫자를 입력 하 고 백 스 페이싱에 대 한 논리는 약간 까다로울 자릿수 없이 입력 한 경우 해당 `Entry` 속성이 문자열 "0"입니다. 사용자가 자세한 0 하면 `Entry` 여전히 하나만 포함 되어 0입니다. 다른 모든 숫자를 입력 하는 경우 해당 숫자가 0을 대체 합니다. 하지만 사용자가 전에 다른 모든 숫자를 소수점 다음 `Entry` 문자열 "0."입니다.

**백스페이스** 활성화 하는 경우 또는 항목의 길이 1 보다 큰 경우에 `Entry` 문자열 "0"와 같지 않습니다.

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

에 대 한 논리를 `execute` 함수를 **백스페이스** 되도록 단추를 `Entry` 은 적어도 문자열 "0"입니다.

합니다 `DigitCommand` 속성이 11 단추를 사용 하 여 자신을 식별 하는 각 바인딩되는 `CommandParameter` 속성입니다. `DigitCommand` 일반적인 인스턴스로 설정할 수 없습니다 `Command` 하지만 쉽게 사용할 수의 `Command<T>` 제네릭 클래스입니다. XAML을 사용 하 여 명령 인터페이스를 사용 하는 경우는 `CommandParameter` 속성은 일반적으로 문자열 되며 제네릭 인수 형식입니다. 합니다 `execute` 하 고 `canExecute` 함수에는 다음 형식의 인수에 `string`:

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

합니다 `execute` 메서드는 문자열 인수를 추가 합니다 `Entry` 속성입니다. 그러나 결과 0 (하지만 하지 0과 소수점)을 사용 하 여 시작 하는 경우 다음 해당 초기 0 제거 해야 합니다를 사용 하 여 `Substring` 함수입니다.

합니다 `canExecute` 메서드가 반환 `false` 인수가 소수점 (소수점을 눌렀는지 나타내는) 경우에 및 `Entry` 이미 소수점을 포함 합니다.

모든 합니다 `execute` 메서드를 호출 `RefreshCanExecutes`를 호출 `ChangeCanExecute` 둘 다에 대해 `DigitCommand` 및 `ClearCommand`합니다. 이렇게 하면 소수점 및 백스페이스 단추를 사용 하도록 설정 되거나 현재 입력 한 숫자 시퀀스를 기반으로 사용 하지 않도록 설정 합니다.

## <a name="adding-commands-to-existing-views"></a>기존 보기에 명령 추가

명령 인터페이스를 지원 하지 않는 뷰를 사용 하 여 사용 하려는 경우 이벤트를 명령으로 변환 하는 Xamarin.Forms 동작을 사용 하 합니다. 이 문서에 설명 되어 [ **재사용 가능한 EventToCommandBehavior**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md)합니다.

## <a name="asynchronous-commanding-for-navigation-menus"></a>비동기 탐색 메뉴 명령

명령 같은 탐색 메뉴를 구현 하는 데 유용 합니다 [ **데이터 바인딩 데모** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) 프로그램 자체. 여기의 일부인 **MainPage.xaml**:

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

XAML을 사용 하 여 명령 실행을 사용 하는 경우 `CommandParameter` 속성은 일반적으로 문자열로 설정 합니다. 하지만 XAML 태그 확장을 사용이 경우에 있도록 합니다 `CommandParameter` 유형의 `System.Type`합니다.

각 `Command` 속성은 명명 된 속성에 바인딩할 `NavigateCommand`합니다. 속성 코드 숨김 파일에 정의 되어 있는지 **MainPage.xaml.cs**:

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

생성자는 `NavigateCommand` 속성을는 `execute` 인스턴스화하는 메서드는 `System.Type` 매개 변수 다음 여기로 이동 하 고 합니다. 때문에 `PushAsync` 호출 필요를 `await` 연산자는 `execute` 메서드 비동기 플래그가 지정 해야 합니다. 사용 하 여 이렇게는 `async` 키워드 매개 변수 목록입니다.

생성자도 설정 합니다 `BindingContext` 자체 페이지의 바인딩을 참조를 `NavigateCommand` 이 클래스에서.

이 생성자의 코드 순서 차이가: 합니다 `InitializeComponent` 호출은 구문 분석할 XAML 하지만 이때 속성에 바인딩한 라는 `NavigateCommand` 확인할 수 없습니다 `BindingContext` 로 설정 되어 `null`합니다. 경우는 `BindingContext` 생성자에서 설정 됩니다 *하기 전에* `NavigateCommand` 은 설정 될 때 바인딩을 확인할 수 `BindingContext` 설정 되어 있지만 이때 `NavigateCommand` 여전히 `null`합니다. 설정 `NavigateCommand` 후 `BindingContext` 아무런 효과가 없습니다 바인딩의 때문에 변경 `NavigateCommand` 발생 하지 않습니다는 `PropertyChanged` 바인딩 및 이벤트에이 사실을 모른다면 `NavigateCommand` 잘못 되었습니다.

모두 설정 `NavigateCommand` 하 고 `BindingContext` (임의 순서로) 호출 하기 전에 `InitializeComponent` XAML 파서가 바인딩 정의 발견 하면 두 구성 요소 바인딩 설정 되어 있으므로 작동 합니다.

데이터 바인딩, 작업은 복잡할 수 있지만이 문서 시리즈에서는 지금까지 살펴본 대로 속도가 강력 하 고 다양 한 사용자 인터페이스에서 기본 논리를 분리 하 여 코드를 구성 하는 데 크게 도움이 됩니다.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
