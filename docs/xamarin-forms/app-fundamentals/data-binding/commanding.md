---
title: Xamarin.Forms 명령 인터페이스
description: 이 문서에서는 Xamarin.Forms 데이터 바인딩을 사용 하는 명령 속성을 구현 하는 방법을 설명 합니다. 명령 인터페이스는 다른 접근 방식은 명령을 구현 하는 데 훨씬 더 잘 적합 한 MVVM 아키텍처를 제공 합니다.
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 37fe5bbcfa3dbc6aa5483c89b49c1698a00ecbb6
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241314"
---
# <a name="the-xamarinforms-command-interface"></a>Xamarin.Forms 명령 인터페이스

파생 된 클래스는 일반적으로 ViewModel에서 속성 간의 데이터 바인딩이 정의 되어 모델-뷰-MVVM () 아키텍처에서 `INotifyPropertyChanged`, 및 XAML 파일은 일반적으로 보기에서 속성입니다. 경우에 따라 응용 프로그램에 ViewModel에 영향을 주는 명령을 시작 하려면 사용자를 요구 하 여 이러한 속성 바인딩을 이외에 필요 합니다. 이러한 명령은 단추를 클릭 하 여 일반적으로 신호를 받기 또는 손가락으로 탭, 및에 대 한 처리기에서 코드 숨김 파일에서 처리 되는 일반적으로 `Clicked` 의 이벤트는 `Button` 또는 `Tapped` 의 이벤트는 `TapGestureRecognizer`합니다.

명령 인터페이스는 다른 접근 방식은 명령을 구현 하는 데 훨씬 더 잘 적합 한 MVVM 아키텍처를 제공 합니다. ViewModel 자체와 같은 보기에서 특정 작업에 대 한 응답에서 실행 되는 메서드인 명령이 포함 되어는 `Button` 클릭 합니다. 이러한 명령 사이 데이터 바인딩이 정의 되어 및 `Button`합니다.

간의 데이터 바인딩을 허용 하는 `Button` 및 ViewModel는 `Button` 두 속성을 정의 합니다.

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) 형식의 <xref:System.Windows.Input.ICommand>
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) 형식의 `Object`

대상으로 하는 데이터 바인딩을 명령 인터페이스를 사용 하려면 정의 `Command` 속성은 `Button` 소스는 형식의 ViewModel에서 속성 `ICommand`합니다. 와 연결 된 코드를 포함 하는 ViewModel `ICommand` 단추를 클릭할 때 실행 되는 속성입니다. 설정할 수 있습니다 `CommandParameter` 단추를 여러 개 있더라도 모든 구분 하기 위해 임의의 데이터에 바인딩된 `ICommand` ViewModel 속성입니다.

`Command` 및 `CommandParameter` 속성은 다음 클래스에서 정의 됩니다.

- [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) 따라서 및 [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)에서 파생 되는 `MenuItem`
- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) 따라서 및 [ `ImageCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/)에서 파생 되는 `TextCell`
- [`TapGestureRecognizer`](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)

[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) 정의 [ `SearchCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) 형식의 속성이 `ICommand` 및 [ `SearchCommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) 속성입니다. [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) 속성 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 형식의 이기도 `ICommand`합니다.

이 명령은 모든 보기에서 특정 사용자 인터페이스 개체에 의존 하지 않는 방식으로 ViewModel 내에서 처리할 수 있습니다.

## <a name="the-icommand-interface"></a>ICommand 인터페이스

<xref:System.Windows.Input.ICommand> 인터페이스 Xamarin.Forms의 일부가 아닙니다. 대신에 정의 되어는 [System.Windows.Input](xref:System.Windows.Input) 네임 스페이스를 두 가지 메서드와 하나의 이벤트로 구성 됩니다.

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

명령 인터페이스를 사용 하려면 프로그램 ViewModel 포함 형식의 속성 `ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

ViewModel 구현 하는 클래스 참조 해야는 `ICommand` 인터페이스입니다. 잠시 후이 클래스에 설명 합니다. 뷰에서 `Command` 의 속성은 `Button` 는 해당 속성에 바인딩됩니다.

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

누를 때는 `Button`, `Button` 호출은 `Execute` 에서 메서드는 `ICommand` 에 바인딩된 개체의 `Command` 속성입니다. 명령 인터페이스의 가장 간단한 부분입니다.

`CanExecute` 메서드는 더 복잡 합니다. 바인딩이에 처음 정의 된 경우는 `Command` 속성의는 `Button`, 데이터 바인딩 어떤 식으로든에서 변경 되 면 및는 `Button` 호출은 `CanExecute` 에서 메서드는 `ICommand` 개체입니다. 경우 `CanExecute` 반환 `false`, 하면 `Button` 자체를 사용 하지 않도록 설정 합니다. 특정 명령을 사용할 수 없거나 잘못 되었습니다. 현재 임을 나타냅니다.

`Button` 도 처리기에 연결 된 `CanExecuteChanged` 이벤트의 `ICommand`합니다. ViewModel 내에서 이벤트가 발생 합니다. 해당 이벤트가 발생 하는 경우는 `Button` 호출 `CanExecute` 다시 합니다. `Button` 를 사용 하면 자체 `CanExecute` 반환 `true` 고 자체적으로 비활성화 하는 경우 `CanExecute` 반환 `false`합니다.

> [!IMPORTANT]
> 사용 하지 마십시오는 `IsEnabled` 속성 `Button` 명령 인터페이스를 사용 하는 경우.  

## <a name="the-command-class"></a>명령 클래스

프로그램 ViewModel 형식의 속성을 정의 하는 경우 `ICommand`, ViewModel 해야도 포함 하거나 참조 구현 하는 클래스는 `ICommand` 인터페이스입니다. 이 클래스를 포함 하거나 참조 해야는 `Execute` 및 `CanExecute` 메서드와 화재는 `CanExecuteChanged` 이벤트 때마다는 `CanExecute` 메서드는 다른 값을 반환할 수 있습니다.

이러한 클래스를 직접 작성할 수 있습니다 또는 다른 사용자가 기록 하는 클래스를 사용할 수 있습니다. 때문에 `ICommand` 일부인의 Microsoft Windows에서 사용 된 Windows MVVM 응용 프로그램과 함께 년입니다. 구현 하는 Windows 클래스를 사용 하 여 `ICommand` 프로그램 Viewmodel Windows 응용 프로그램 및 Xamarin.Forms 응용 프로그램 간에 공유할 수 있습니다.

Viewmodel 창과 Xamarin.Forms 사이의 공유은 문제가 되지 경우 사용할 수 있습니다는 [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) 또는 [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) 는 를구현하는Xamarin.Forms에포함된클래스`ICommand`인터페이스입니다. 이러한 클래스를 사용 하면의 본문을 지정할 수는 `Execute` 및 `CanExecute` 클래스 생성자의 메서드. 사용 하 여 `Command<T>` 사용 하는 경우는 `CommandParameter` 여러 뷰 간을 서로 구별 하는 속성을 동일한 바인딩할 `ICommand` 속성 및 보다 단순한 `Command` 요구 사항을 반드시 그럴 경우 클래스입니다.

## <a name="basic-commanding"></a>기본 명령 실행

**개인 항목** 페이지에 [ **데이터 바인딩 데모** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) 프로그램은 ViewModel에서 구현 된 몇 가지 간단한 명령을 보여 줍니다.

`PersonViewModel` 라는 세 가지 속성을 정의 `Name`, `Age`, 및 `Skills` 사람을 정의 하는 합니다. 이 클래스는 *하지* 포함할 `ICommand` 속성:

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

`PersonCollectionViewModel` 표시 된 다음 형식의 새 개체를 만듭니다 `PersonViewModel` 사용자 데이터를 입력을 수 있습니다. 이 위해에 클래스 속성을 정의 `IsEditing` 형식의 `bool` 및 `PersonEdit` 형식의 `PersonViewModel`합니다. 클래스 형식의 세 가지 속성을 정의 하는 또한 `ICommand` 속성 및 이름이 `Persons` 형식의 `IList<PersonViewModel>`:

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

약식된이 목록에는 클래스의 생성자에는 세 가지 유형의 속성을 `ICommand` 정의 된 곧 표시 됩니다. 유형의 세 가지 속성으로 변경 하는 알림 `ICommand` 및 `Persons` 속성으로 나타나지 `PropertyChanged` 이벤트 발생 합니다. 이러한 속성 모두 설정 클래스를 처음 만들 때 되며 그 이후 변경 되지 않습니다.

생성자를 검토 하기 전에 `PersonCollectionViewModel` 클래스에 대 한 XAML 파일에는 **개인 항목** 프로그램. 여기에 포함 된 `Grid` 와 해당 `BindingContext` 속성이로 설정는 `PersonCollectionViewModel`합니다. `Grid` 포함는 `Button` 텍스트와 함께 **새로 만들기** 와 해당 `Command` 속성에 바인딩된는 `NewCommand` ViewModel에서 속성, 속성을 가진 입력 폼에 바인딩된는 `IsEditing` 속성으로 속성에 따라 적절히 `PersonViewModel`, 두 개 더 많은 단추에 바인딩되고는 `SubmitCommand` 및 `CancelCommand` ViewModel의 속성입니다. 최종 `ListView` 이미 입력 한 사용자의 컬렉션을 표시 합니다.

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

함수 방법은 다음과 같습니다: 누르면 첫 번째는 **새로** 단추입니다. 이 입력 폼을 사용 하도록 설정 되지만 사용 하지 않도록 설정 된 **새로** 단추입니다. 사용자 이름, 연령 및 기술을 입력합니다. 사용자를 편집 하는 동안 언제 든 누를 수는 **취소** 단추를 처음부터 다시 시작 합니다. 이름 및 유효한 시간 입력 된 파일인는 **전송** 단추를 사용할 수 있습니다. 이 키를 누르면 **전송** 여 표시 되는 컬렉션에 사용자를 전송 하는 단추는 `ListView`합니다. 중는 **취소** 또는 **전송** 단추가 눌려질, 입력 양식 선택이 취소 되어 및 **새로** 단추를 다시 사용할 수 있습니다.

IOS 화면 왼쪽에 유효한 시간을 입력 하기 전에 레이아웃을 보여 줍니다. Android 및 UWP 화면 표시는 **전송** age이 설정 된 후 활성화 단추:

[![개인 항목](commanding-images/personentry-small.png "개인 항목")](commanding-images/personentry-large.png#lightbox "개인 항목")

프로그램에 기존 항목을 편집 하기 위한 모든 기능 없고 페이지에서 벗어나면 항목을 저장 하지 않습니다.

에 대 한 모든 논리는 **새로**, **전송**, 및 **취소** 의 처리 하는 단추 `PersonCollectionViewModel` 의 정의 통해는 `NewCommand`, `SubmitCommand`, 및 `CancelCommand` 속성입니다. 생성자는 `PersonCollectionViewModel` 이 세 가지 속성 형식의 개체에 설정 `Command`합니다.  

A [생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/) 의 `Command` 클래스 형식의 인수를 전달할 수 있도록 `Action` 및 `Func<bool>` 에 해당 하는 `Execute` 및 `CanExecute` 메서드. 람다 함수 right에으로 이러한 작업 및 함수를 정의 하는 것이 더 편리는 `Command` 생성자입니다. 여기에 정의 `Command` 개체에 대 한는 `NewCommand` 속성:

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

사용자가 클릭할 때는 **새로** 단추를는 `execute` 함수에 전달 된는 `Command` 생성자는 실행 합니다. 새로 만듭니다.이 `PersonViewModel` 개체, 해당 개체에 대 한 처리기를 설정 `PropertyChanged` 설정 하는 이벤트를 `IsEditing` 를 `true`, 호출는 `RefreshCanExecutes` 메서드를 생성자 정의 합니다.

구현 외에도 `ICommand` 인터페이스는 `Command` 클래스 라는 메서드도 정의 `ChangeCanExecute`합니다. 프로그램 ViewModel 호출 해야 `ChangeCanExecute` 에 대 한 프로그램 `ICommand` 발생 한 경우 아무 것도 때마다 속성의 반환 값을 변경 될 수 있습니다는 `CanExecute` 메서드. 에 대 한 호출 `ChangeCanExecute` 하면는 `Command` 를 발생 시키는 클래스는 `CanExecuteChanged` 메서드. `Button` 해당 이벤트에 대 한 처리기가 연결 하 고 호출 하 여 응답 `CanExecute` 다시 후 해당 메서드의 반환 값에 따라 자체를 사용 하도록 설정 합니다.

경우는 `execute` 방식의 `NewCommand` 호출 `RefreshCanExecutes`, `NewCommand` 속성에 대 한 호출을 가져옵니다 `ChangeCanExecute`, 및 `Button` 호출은 `canExecute` 이제 반환 하는 메서드 `false` 때문에 `IsEditing`속성은 이제 `true`합니다.

`PropertyChanged` 새 처리기 `PersonViewModel` 호출 개체는 `ChangeCanExecute` 방식의 `SubmitCommand`합니다. 해당 명령 속성을 구현 하는 방법을 다음과 같습니다.


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

`canExecute` 에 대 한 함수 `SubmitCommand` 속성이 변경 될 때마다 호출 됩니다는 `PersonViewModel` 편집 중인 개체입니다. 반환 `true` 경우에만 `Name` 속성은 문자 하나 이상 및 `Age` 0 보다 큽니다. 그 당시는 **전송** 단추가 활성화 됩니다.

`execute` 에 대 한 함수 **전송** 에서 속성 변경 처리기를 제거는 `PersonViewModel`, 개체를 추가 `Persons` 컬렉션을 초기 조건에 모든 항목을 반환 합니다.

`execute` 에 대 한 함수는 **취소** 단추 모든 작업을 수행 하는 **전송** 단추는 execept 개체를 컬렉션에 추가:

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

`canExecute` 메서드 반환 `true` 언제 든 지는 `PersonViewModel` 편집 하 고 있습니다.

이러한 기술을 보다 복잡 한 시나리오에 적용할 수: 속성에 `PersonCollectionViewModel` 에 바인딩할 수 없습니다는 `SelectedItem` 의 속성은 `ListView` 기존 항목을 편집 하기 위해 및 **삭제** 삭제를 단추를 추가할 수 없습니다 이러한 항목입니다.

정의할 필요가 없습니다는 `execute` 및 `canExecute` 람다 함수로 메서드. ViewModel에서 일반 전용 메서드를 작성 하 고에서 참조할 수는 `Command` 생성자입니다. 그러나이 방법은 않습니다 ViewModel에서 한 번만 참조 되는 메서드의 너무 경향이 있습니다.

## <a name="using-command-parameters"></a>명령 매개 변수를 사용 하 여

하나 이상의 단추 (또는 기타 사용자 인터페이스 개체)를 동일한 공유에 대 한 편리 하 게 될 `ICommand` ViewModel 속성입니다. 사용 하는 경우에 `CommandParameter` 단추 간을 서로 구별 하는 속성입니다.

계속 사용할 수는 `Command` 이러한 공유에 대 한 클래스 `ICommand` 속성입니다. 클래스 정의 [대체 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action%7BSystem.Object%7D/System.Func%7BSystem.Object,System.Boolean%7D/) 를 받아들이는 `execute` 및 `canExecute` 메서드 형식 매개 변수가 있는 `Object`합니다. 이것은 방법을 `CommandParameter` 를 이러한 메서드에 전달 됩니다.

그러나 사용 하는 경우 `CommandParameter`, 제네릭을 사용 하는 것이 더 편리 [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) 로 설정 하는 개체의 유형을 지정 하는 클래스 `CommandParameter`합니다. `execute` 및 `canExecute` 지정 하는 메서드는 해당 형식의 매개 변수가 있습니다.

**10 진수 키보드** 10 진수 숫자를 입력 하는 것에 대 한 키패드를 구현 하는 방법을 표시 하 여이 기술을 보여 줍니다. `BindingContext` 에 대 한는 `Grid` 는 `DecimalKeypadViewModel`합니다. `Entry` 이 ViewModel의 속성이 바인딩되는 `Text` 속성은 `Label`합니다. 모든는 `Button` 개체 ViewModel에서 다양 한 명령에 바인딩된: `ClearCommand`, `BackspaceCommand`, 및 `DigitCommand`:

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

숫자 10 가지 및 소수점에 대 한 11 단추 공유에 대 한 바인딩을 `DigitCommand`합니다. `CommandParameter` 이러한 단추를 구분 합니다. 값으로 설정 `CommandParameter` 는 일반적으로 소수점 명확히 전달 하기 위해 표시 되는 중간 점 문자를 제외 하 고 단추에 표시 되는 텍스트와 동일 합니다.

다음은 실행에서 프로그램이입니다.

[![10 진수 키보드](commanding-images/decimalkeyboard-small.png "10 진수 키보드")](commanding-images/decimalkeyboard-large.png#lightbox "10 진수 키보드")

입력 한 수에 이미 소수점이 포함 되어 있으므로 모든 세 스크린샷에서 소수점에 대 한 단추는 비활성화를 확인 합니다.

`DecimalKeypadViewModel` 정의 `Entry` 형식의 속성이 `string` (트리거하는 유일한 속성 변수인는 `PropertyChanged` 이벤트) 및 유형의 세 가지 속성 `ICommand`:

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

에 해당 하는 단추는 `ClearCommand` 항상 활성화 되어 있으며을 "0"으로 다시 설정 합니다.

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

지정할 필요가 없다는 항상 단추가 활성화 되므로 `canExecute` 인수에는 `Command` 생성자입니다.

때문에 숫자를 입력 하 고 백 스 페이싱에 대 한 논리는 약간 까다로울 없는 숫자를 입력 하면 `Entry` 속성은 문자열 "0"입니다. 사용자가 더 많은 0으로 하면 `Entry` 여전히 하나만 포함 되어 0입니다. 다른 모든 숫자를 입력 하는 경우 해당 숫자가 0을 대체 합니다. 그러나 충분하기 전에 다른 모든 숫자를 소수점 `Entry` 문자열 "0"입니다.

**백스페이스** 단추를 사용할 수 있는 항목의 길이 1 보다 큰 경우에 또는 경우 `Entry` 문자열 "0"과 같지 않은:

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

에 대 한 논리는 `execute` 에 대 한 함수는 **백스페이스** 되도록 단추는 `Entry` 이상는 문자열 "0"입니다.

`DigitCommand` 11 단추, 자체를 식별 하며 각 속성이 바인딩되는 `CommandParameter` 속성입니다. `DigitCommand` 일반의 인스턴스로 설정할 수 있습니다 `Command` 클래스와 보다 쉽게 사용할 수는 `Command<T>` 제네릭 클래스입니다. Xaml을 명령 인터페이스를 사용 하는 경우는 `CommandParameter` 속성은 문자열, 일반적으로 및 제네릭 인수 형식입니다. `execute` 및 `canExecute` 함수는 다음 형식의 인수를 갖고 `string`:

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

`execute` 메서드 추가에 대 한 문자열 인수는 `Entry` 속성입니다. 그러나 결과 0 (하지만 하지 0과 소수점)로 시작 하는 경우 다음 해당 초기 0 제거 해야 합니다를 사용 하 여 `Substring` 함수입니다.

`canExecute` 메서드 반환 `false` (소수점을 눌렀는지를 나타내는) 소수점 인수가 있는 경우에 및 `Entry` 이미 소수점이 포함 되어 있습니다.

모든는 `execute` 메서드 호출 `RefreshCanExecutes`, 호출 `ChangeCanExecute` 둘 다에 대해 `DigitCommand` 및 `ClearCommand`합니다. 이렇게 하면 소수점 및 백스페이스 단추를 사용할 수 또는 입력 한 숫자의 현재 시퀀스에 따라 사용할 수 없습니다.

## <a name="adding-commands-to-existing-views"></a>기존 보기에 명령 추가

지원 되지 않는 뷰가 포함 된 명령 인터페이스를 사용 하려는 경우 이벤트를 명령으로 변환 하는 Xamarin.Forms 동작을 사용 하 여입니다. 이 문서에 설명 되어 [ **다시 사용할 수 있는 EventToCommandBehavior**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md)합니다.

## <a name="asynchronous-commanding-for-navigation-menus"></a>비동기 탐색 메뉴에 대 한 명령 실행

명령 실행 하는 것이 같은 탐색 메뉴를 구현 하기 위한 편리는 [ **데이터 바인딩 데모** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) 자체를 프로그래밍 합니다. 여기의 일부인 **MainPage.xaml**:


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

Xaml을 명령 실행을 사용 하는 경우 `CommandParameter` 일반적으로 속성을 문자열로 설정 합니다. 하지만 XAML 태그 확장 사용이 경우 있도록는 `CommandParameter` 유형의 `System.Type`합니다.

각 `Command` 속성이 라는 속성이에 바인딩될 `NavigateCommand`합니다. 속성이 코드 숨김 파일에 정의 된 **MainPage.xaml.cs**:

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

생성자는 `NavigateCommand` 속성을는 `execute` 메서드를 인스턴스화하는 `System.Type` 매개 변수를 이동 합니다. 때문에 `PushAsync` 호출 해야는 `await` 연산자는 `execute` 메서드 같이 비동기 플래그 설정 해야 합니다. 이 사용 하 여 수행 되는 `async` 매개 변수 목록 앞에 키워드입니다.

생성자도 설정 하는 `BindingContext` 자체 페이지의 바인딩을 참조는 `NavigateCommand` 이 클래스의 합니다.

이 생성자의 코드의 순서가 달라 집니다:는 `InitializeComponent` 호출 하면 XAML을 구문 분석할 수 있지만 그 당시 속성에 바인딩한 라는 `NavigateCommand` 해결할 수 없습니다 `BindingContext` 로 설정 된 `null`합니다. 경우는 `BindingContext` 생성자에서 설정 *전에* `NavigateCommand` 이 설정 될 때 바인딩을 확인할 수 `BindingContext` 설정 되어 있지만 그 당시 `NavigateCommand` 여전히 `null`합니다. 설정 `NavigateCommand` 후 `BindingContext` 효과가 없습니다. 바인딩에 때문에에 대 한 변경 `NavigateCommand` 발생 하지 않습니다는 `PropertyChanged` 바인딩 및 이벤트를 하지 않는 것을 알고 `NavigateCommand` 이제 올바릅니다.

모두 설정 `NavigateCommand` 및 `BindingContext` (어떤 순서로 든) 호출 하기 전에 `InitializeComponent` 두 구성 요소는 바인딩 XAML 파서가 바인딩 정의 발견할 때 설정 되어 있으므로 작동 합니다.

데이터 바인딩, 작업은 복잡할 수 있지만이 일련의 문서에서 지금까지 살펴본 대로 속도가 강력 하 고 용도가 넓은 함수로, 크게 사용자 인터페이스에서 기본 논리를 분리 하 여 코드를 구성 하는 데 도움이 됩니다.



## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
