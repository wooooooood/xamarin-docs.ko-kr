---
title: Xamarin.Forms 단추
description: 단추를 탭 하거나 특정 작업을 수행 하는 응용 프로그램을 지시 하는 클릭에 응답 합니다.
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
ms.openlocfilehash: f82d590213076f349b21ebdee2832f2bf474d2f2
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305848"
---
# <a name="xamarinforms-button"></a>Xamarin.Forms 단추

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)

_단추는 응용 프로그램이 특정 작업을 수행 하도록 지시 하는 탭 하거나 클릭에 응답 합니다._

[`Button`](xref:Xamarin.Forms.Button) 은 모든 xamarin.ios에서 가장 기본적인 대화형 컨트롤입니다. 일반적으로 `Button`는 명령을 나타내는 짧은 텍스트 문자열을 표시 하지만 비트맵 이미지 또는 텍스트와 이미지의 조합을 표시할 수도 있습니다. 사용자가 손가락으로 `Button`를 누르거나 마우스로 클릭 하 여 해당 명령을 시작 합니다.

아래에서 설명 하는 대부분의 항목은 [**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) 샘플의 페이지에 해당 합니다.

## <a name="handling-button-clicks"></a>클릭할 단추 처리

`Button` 사용자가 손가락 또는 마우스 포인터로 `Button`를 누를 때 발생 하는 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트를 정의 합니다. 이 이벤트는 `Button`화면에서 손가락 또는 마우스 단추를 놓을 때 발생 합니다. `Button`은 탭에 응답 하기 위해 [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) 속성을 `true`으로 설정 해야 합니다.

[**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) 샘플의 **기본 단추 클릭** 페이지에서는 XAML로 `Button`을 인스턴스화하고 해당 `Clicked` 이벤트를 처리 하는 방법을 보여 줍니다. **Basicbutton클릭 페이지 .xaml** 파일에는 `Label` 및 `Button`를 모두 사용 하는 `StackLayout` 포함 되어 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.BasicButtonClickPage"
             Title="Basic Button Click">
    <StackLayout>

        <Label x:Name="label"
               Text="Click the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Click to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Clicked="OnButtonClicked" />

    </StackLayout>
</ContentPage>
```

`Button`는 허용 되는 모든 공간을 차지 하는 경향이 있습니다. 예를 들어 `Button`의 `HorizontalOptions` 속성을 `Fill`이외의 항목으로 설정 하지 않은 경우 `Button`는 부모의 전체 너비를 차지 합니다.

기본적으로 `Button`는 사각형 이지만 아래 섹션의 [**단추 모양**](#button-appearance)에 설명 된 대로 [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) 속성을 사용 하 여 모퉁이가 둥근 모퉁이를 제공할 수 있습니다.

[`Text`](xref:Xamarin.Forms.Button.Text) 속성은 `Button`에 나타나는 텍스트를 지정합니다. [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트는 `OnButtonClicked`라는 이벤트 처리기로 설정 됩니다. 이 처리기는 코드 **BasicButtonClickPage.xaml.cs**파일에 있습니다.

```csharp
public partial class BasicButtonClickPage : ContentPage
{
    public BasicButtonClickPage ()
    {
        InitializeComponent ();
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        await label.RelRotateTo(360, 1000);
    }
}
```

`Button` 탭 하면 `OnButtonClicked` 메서드가 실행 됩니다. `sender` 인수는이 이벤트를 담당 하는 `Button` 개체입니다. 이를 사용 하 여 `Button` 개체에 액세스 하거나 동일한 `Clicked` 이벤트를 공유 하는 여러 `Button` 개체를 구분할 수 있습니다.

이 특정 `Clicked` 처리기는 1000 밀리초 단위로 `Label` 360도를 회전 하는 애니메이션 함수를 호출 합니다. Windows 10 데스크톱 및 유니버설 Windows 플랫폼 (UWP) 응용 프로그램을 iOS 및 Android 장치에서 실행 중인 프로그램이 다음과 같습니다.

[![기본 단추 클릭](button-images/BasicButtonClick.png "기본 단추 클릭")](button-images/BasicButtonClick-Large.png#lightbox "기본 단추 클릭")

이벤트 처리기 내에서 `await`를 사용 하기 때문에 `OnButtonClicked` 메서드에 `async` 한정자가 포함 되어 있습니다. 처리기의 본문에서 `await`을 사용 하는 경우에만 `Clicked` 이벤트 처리기에 `async` 한정자가 필요 합니다.

각 플랫폼은 고유의 특정 방식으로 `Button`를 렌더링 합니다. [**단추 모양**](#button-appearance) 섹션에서 색을 설정 하 고 사용자 지정 된 모양새를 위해 `Button` 테두리를 표시 하는 방법을 알아봅니다. `Button`는 [`IFontElement`](xref:Xamarin.Forms.Internals.IFontElement) 인터페이스를 구현 하므로 [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily), [`FontSize`](xref:Xamarin.Forms.Button.FontSize)및 [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) 속성을 포함 합니다.

## <a name="creating-a-button-in-code"></a>코드에서 단추 만들기

XAML에서 `Button`를 인스턴스화하는 것이 일반적 이지만 코드에서 `Button`를 만들 수도 있습니다. 응용 프로그램에서 `foreach` 루프를 사용 하 여 열거할 수 있는 데이터를 기반으로 여러 단추를 만들어야 하는 경우에 편리 합니다.

**코드 단추 클릭** 페이지는 **기본 단추 클릭** 페이지와 기능적으로 동일한 페이지를 만드는 방법을 보여 줍니다 C#.

```csharp
public class CodeButtonClickPage : ContentPage
{
    public CodeButtonClickPage ()
    {
        Title = "Code Button Click";

        Label label = new Label
        {
            Text = "Click the Button below",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };

        Button button = new Button
        {
            Text = "Click to Rotate Text!",
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };
        button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);

        Content = new StackLayout
        {
            Children =
            {
                label,
                button
            }
        };
    }
}
```

모든 클래스의 생성자에서 수행 됩니다. `Clicked` 처리기는 문 길이는 하나 뿐 이기 때문에 매우 간단 하 게 이벤트에 연결할 수 있습니다.

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

물론 이벤트 처리기를 별도의 메서드로 정의 ( **기본 단추 클릭**의 `OnButtonClick` 메서드와 동일) 하 고 해당 메서드를 이벤트에 연결할 수도 있습니다.

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>단추를 사용 하지 않도록 설정

때로는 특정 `Button` 클릭이 올바른 작업이 아닌 특정 상태에 있는 응용 프로그램이 있을 수 있습니다. 이러한 경우 `IsEnabled` 속성을 `false`로 설정 하 여 `Button`를 사용 하지 않도록 설정 해야 합니다. 클래식 예제는 파일 열기 `Button`와 함께 파일 이름에 대 한 `Entry` 컨트롤입니다. `Button` `Entry`에 일부 텍스트를 입력 한 경우에만 사용할 수 있습니다.
[**데이터 트리거**](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) 문서에 표시 된 것 처럼이 작업에 대 한 `DataTrigger`를 사용할 수 있습니다.

## <a name="using-the-command-interface"></a>인터페이스를 사용 하 여

응용 프로그램이 `Clicked` 이벤트를 처리 하지 않고 `Button` 탭에 응답할 수 있습니다. `Button`는 _명령 또는 명령_ 인터페이스 라는 대체 알림 메커니즘을 _commanding_ 구현 합니다. 이 두 속성으로 구성 됩니다.

- [`System.Windows.Input`](xref:System.Windows.Input) 네임 스페이스에 정의 된 인터페이스인 [`ICommand`](xref:System.Windows.Input.ICommand)형식의 [`Command`](xref:Xamarin.Forms.Button.Command) 입니다.
- [`Object`](xref:System.Object)형식의 [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) 속성입니다.

이 접근 방식은 모델-뷰-ViewModel (MVVM) 아키텍처를 구현 하는 경우에 특히 데이터 바인딩 관련 하 여 및에 특히 적합 합니다. 이러한 항목에 대해서는 데이터 바인딩에서 [Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md) [데이터 바인딩에서 MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)로, [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)에 설명 되어 있습니다.

MVVM 응용 프로그램에서 viewmodel은 데이터 바인딩을 사용 하 여 XAML `Button` 요소에 연결 된 `ICommand` 형식의 속성을 정의 합니다. 또한 Xamarin은 `ICommand` 인터페이스를 구현 하 고 `ICommand`형식의 속성을 정의 하는 데 viewmodel을 지 원하는 [`Command`](xref:Xamarin.Forms.Command) 및 [`Command<T>`](xref:Xamarin.Forms.Command`1) 클래스를 정의 합니다.

[**명령 인터페이스 문서에서는 명령 인터페이스**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) 에 대해 자세히 설명 하지만 [**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) 샘플의 **기본 단추 명령** 페이지에는 기본적인 방법이 나와 있습니다.

`CommandDemoViewModel` 클래스는 `Number``double` 형식의 속성을 정의 하는 매우 간단한 viewmodel 이며 이름이 `MultiplyBy2Command` 및 `DivideBy2Command`인 `ICommand` 형식의 두 속성을 정의 합니다.

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    double number = 1;

    public event PropertyChangedEventHandler PropertyChanged;

    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(() => Number *= 2);

        DivideBy2Command = new Command(() => Number /= 2);
    }

    public double Number
    {
        set
        {
            if (number != value)
            {
                number = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Number"));
            }
        }
        get
        {
            return number;
        }
    }

    public ICommand MultiplyBy2Command { private set; get; }

    public ICommand DivideBy2Command { private set; get; }
}
```

두 개의 `ICommand` 속성은 `Command`형식의 두 개체를 사용 하 여 클래스의 생성자에서 초기화 됩니다. `Command` 생성자에는 `Number` 속성을 두 배로 늘리거나 하는 작은 함수 (`execute` 생성자 인수 라고 함)가 포함 되어 있습니다.

**Basicbuttoncommand .xaml** 파일은 `BindingContext`를 `CommandDemoViewModel`인스턴스로 설정 합니다. `Label` 요소와 두 개의 `Button` 요소는 `CommandDemoViewModel`의 세 가지 속성에 대 한 바인딩을 포함 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.BasicButtonCommandPage"
             Title="Basic Button Command">

    <ContentPage.BindingContext>
        <local:CommandDemoViewModel />
    </ContentPage.BindingContext>

    <StackLayout>
        <Label Text="{Binding Number, StringFormat='Value is now {0}'}"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Multiply by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding MultiplyBy2Command}" />

        <Button Text="Divide by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding DivideBy2Command}" />
    </StackLayout>
</ContentPage>
```

두 개의 `Button` 요소를 탭 하면 명령이 실행 되 고 숫자 값이 변경 됩니다.

[![기본 단추 명령](button-images/BasicButtonCommand.png "기본 단추 명령")](button-images/BasicButtonCommand-Large.png#lightbox)

`Clicked` 처리기에 대 한이 방법의 장점은이 페이지의 기능과 관련 된 모든 논리가 코드 숨김이 아닌 viewmodel에 위치 하 여 비즈니스 논리에서 사용자 인터페이스를 더 잘 분리 하는 것입니다.

`Command` 개체는 `Button` 요소의 활성화 및 비활성화를 제어할 수도 있습니다. 예를 들어 숫자 값의 범위를 2<sup>10</sup> 에서 2<sup>&ndash;10</sup>으로 제한 하려는 경우를 가정해 보겠습니다. `Button`를 사용 하도록 설정 해야 하는 경우 `true`을 반환 하는 생성자 (`canExecute` 인수 라고 함)에 다른 함수를 추가할 수 있습니다. `CommandDemoViewModel` 생성자에 대 한 수정 사항은 다음과 같습니다.

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    ···
    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(
            execute: () =>
            {
                Number *= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number < Math.Pow(2, 10));

        DivideBy2Command = new Command(
            execute: () =>
            {
                Number /= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number > Math.Pow(2, -10));
    }
    ···
}
```

`Command` 메서드가 `canExecute` 메서드를 호출 하 고 `Button`를 사용 하지 않도록 설정할지 여부를 결정할 수 있도록 `Command`의 `ChangeCanExecute` 메서드에 대 한 호출이 필요 합니다. 이 코드를 변경 하면 숫자가 제한에 도달 하 고 `Button` 사용 하지 않도록 설정 됩니다.

[![기본 단추 명령-수정 됨](button-images/BasicButtonCommandModified.png "기본 단추 명령-수정 됨")](button-images/BasicButtonCommandModified-Large.png#lightbox)

둘 이상의 `Button` 요소를 동일한 `ICommand` 속성에 바인딩할 수 있습니다. `Button`의 [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) 속성을 사용 하 여 `Button` 요소를 구분할 수 있습니다. 이 경우 제네릭 [`Command<T>`](xref:Xamarin.Forms.Command`1) 클래스를 사용 하는 것이 좋습니다. 그런 다음 `CommandParameter` 개체는 `execute` 및 `canExecute` 메서드에 인수로 전달 됩니다. 이 기술은 [**명령 인터페이스**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) 문서의 [**기본 명령**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) 섹션에 자세히 나와 있습니다.

또한 [**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) 샘플은 해당 `MainPage` 클래스에서이 기법을 사용 합니다. **Mainpage** 파일에는 샘플의 각 페이지에 대 한 `Button` 포함 되어 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.MainPage"
             Title="Button Demos">
    <ScrollView>
        <FlexLayout Direction="Column"
                    JustifyContent="SpaceEvenly"
                    AlignItems="Center">

            <Button Text="Basic Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonClickPage}" />

            <Button Text="Code Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:CodeButtonClickPage}" />

            <Button Text="Basic Button Command"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonCommandPage}" />

            <Button Text="Press and Release Button"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:PressAndReleaseButtonPage}" />

            <Button Text="Button Appearance"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ButtonAppearancePage}" />

            <Button Text="Toggle Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ToggleButtonDemoPage}" />

            <Button Text="Image Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ImageButtonDemoPage}" />

        </FlexLayout>
    </ScrollView>
</ContentPage>
```

각 `Button`에는 `NavigateCommand`라는 속성에 바인딩된 `Command` 속성이 있고 `CommandParameter`는 프로젝트의 페이지 클래스 중 하나에 해당 하는 [`Type`](xref:System.Type) 개체로 설정 됩니다.

해당 `NavigateCommand` 속성은 `ICommand` 형식이 며 코드 숨김이 파일에 정의 됩니다.

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

`Type`는 XAML 파일에 설정 된 `CommandParameter` 개체의 형식이 기 때문에 생성자는 `NavigateCommand` 속성을 `Command<Type>` 개체로 초기화 합니다. 이는 `execute` 메서드에이 `CommandParameter` 개체에 해당 하는 `Type` 형식의 인수가 있음을 의미 합니다. 함수 페이지를 인스턴스화하고 여기로 이동 합니다.

생성자는 해당 `BindingContext` 자체에 설정 하 여 마칩니다. XAML 파일의 속성을 `NavigateCommand` 속성에 바인딩하려면이 작업이 필요 합니다.

## <a name="pressing-and-releasing-the-button"></a>누른 단추에서 손을 떼기

`Clicked` 이벤트 외에도 `Button`은 [`Pressed`](xref:Xamarin.Forms.Button.Pressed) 및 [`Released`](xref:Xamarin.Forms.Button.Released) 이벤트도 정의합니다. `Pressed` 이벤트는 `Button`에서 손가락을 누를 때 발생 하거나 포인터가 `Button`위에 배치 된 상태에서 마우스 단추를 누르면 발생 합니다. `Released` 이벤트는 손가락 또는 마우스 단추를 놓을 때 발생 합니다. 일반적으로 `Clicked` 이벤트는 `Released` 이벤트와 동시에 발생 하지만 손가락 또는 마우스 포인터를 해제 하기 전에 `Button` 화면에서 벗어나면 `Clicked` 이벤트가 발생 하지 않을 수 있습니다.

`Pressed` 및 `Released` 이벤트는 자주 사용 되지 않지만, **누름 및 릴리스 단추** 페이지에 나와 있는 것 처럼 특수 한 용도로 사용할 수 있습니다. XAML 파일에는 `Pressed` 및 `Released` 이벤트에 대해 연결 된 처리기가 있는 `Label` 및 `Button` 포함 되어 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.PressAndReleaseButtonPage"
             Title="Press and Release Button">
    <StackLayout>

        <Label x:Name="label"
               Text="Press and hold the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Press to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Pressed="OnButtonPressed"
                Released="OnButtonReleased" />

    </StackLayout>
</ContentPage>
```

코드를 `Pressed` 이벤트가 발생할 때 `Label`에 애니메이션 효과를 주는 코드 숨김이 있지만 `Released` 이벤트가 발생 하는 경우에는 회전이 일시 중단 됩니다.

```csharp
public partial class PressAndReleaseButtonPage : ContentPage
{
    bool animationInProgress = false;
    Stopwatch stopwatch = new Stopwatch();

    public PressAndReleaseButtonPage ()
    {
        InitializeComponent ();
    }

    void OnButtonPressed(object sender, EventArgs args)
    {
        stopwatch.Start();
        animationInProgress = true;

        Device.StartTimer(TimeSpan.FromMilliseconds(16), () =>
        {
            label.Rotation = 360 * (stopwatch.Elapsed.TotalSeconds % 1);

            return animationInProgress;
        });
    }

    void OnButtonReleased(object sender, EventArgs args)
    {
        animationInProgress = false;
        stopwatch.Stop();
    }
}
```

그 결과 `Label` 손가락이 `Button`와 접촉 하는 동안에만 회전 하 고 손가락을 놓을 때 중지 됩니다.

[![눌렀다가 놓기 단추](button-images/PressAndReleaseButton.png "눌렀다가 놓기 단추")](button-images/PressAndReleaseButton-Large.png)

이러한 종류의 동작에는 게임에 대 한 응용 프로그램이 있습니다. `Button`에 있는 손가락은 특정 방향으로 화면에 개체를 이동 하 게 만들 수 있습니다.

<a name="button-appearance" />

## <a name="button-appearance"></a>단추 모양

`Button`는 모양에 영향을 주는 몇 가지 속성을 상속 하거나 정의 합니다.

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) `Button` 텍스트의 색입니다.
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 은 해당 텍스트에 대 한 배경 색입니다.
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) 은 `Button` 주변 영역의 색입니다.
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) 는 텍스트에 사용 되는 글꼴 패밀리입니다.
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) 는 텍스트 크기입니다.
- 텍스트를 기울임꼴 또는 굵게 표시 [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) 나타냅니다.
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) 테두리의 너비입니다.
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) 은 `Button`의 모퉁이 반경입니다.
- `CharacterSpacing`은 `Button` 텍스트 문자 사이의 간격입니다.

> [!NOTE]
> `Button` 클래스에는 `Button`의 레이아웃 동작을 제어 하는 [`Margin`](xref:Xamarin.Forms.View.Margin) 및 [`Padding`](xref:Xamarin.Forms.Button.Padding) 속성도 있습니다. 자세한 내용은 [여백 및 패딩](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)을 참조하세요.

이러한 속성 (`FontFamily` 및 `FontAttributes`제외)의 6 가지 효과는 **단추 모양** 페이지에서 보여 줍니다. 다른 속성인 [`Image`](xref:Xamarin.Forms.Button.ImageSource)은 [**비트맵 사용 단추를 사용 하**](#image-button)여 섹션에서 설명 합니다.

**단추 모양** 페이지의 모든 뷰 및 데이터 바인딩은 XAML 파일에 정의 됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ButtonAppearancePage"
             Title="Button Appearance">
    <StackLayout>
        <Button x:Name="button"
                Text="Button"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                TextColor="{Binding Source={x:Reference textColorPicker},
                                    Path=SelectedItem.Color}"
                BackgroundColor="{Binding Source={x:Reference backgroundColorPicker},
                                          Path=SelectedItem.Color}"
                BorderColor="{Binding Source={x:Reference borderColorPicker},
                                      Path=SelectedItem.Color}" />

        <StackLayout BindingContext="{x:Reference button}"
                     Padding="10">

            <Slider x:Name="fontSizeSlider"
                    Maximum="48"
                    Minimum="1"
                    Value="{Binding FontSize}" />

            <Label Text="{Binding Source={x:Reference fontSizeSlider},
                                  Path=Value,
                                  StringFormat='FontSize = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="borderWidthSlider"
                    Minimum="-1"
                    Maximum="12"
                    Value="{Binding BorderWidth}" />

            <Label Text="{Binding Source={x:Reference borderWidthSlider},
                                  Path=Value,
                                  StringFormat='BorderWidth = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="cornerRadiusSlider"
                    Minimum="-1"
                    Maximum="24"
                    Value="{Binding CornerRadius}" />

            <Label Text="{Binding Source={x:Reference cornerRadiusSlider},
                                  Path=Value,
                                  StringFormat='CornerRadius = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.Resources>
                    <Style TargetType="Label">
                        <Setter Property="VerticalOptions" Value="Center" />
                    </Style>
                </Grid.Resources>

                <Label Text="Text Color:"
                       Grid.Row="0" Grid.Column="0" />

                <Picker x:Name="textColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="0" Grid.Column="1" />

                <Label Text="Background Color:"
                       Grid.Row="1" Grid.Column="0" />

                <Picker x:Name="backgroundColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="1" Grid.Column="1" />

                <Label Text="Border Color:"
                       Grid.Row="2" Grid.Column="0" />

                <Picker x:Name="borderColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="2" Grid.Column="1" />
            </Grid>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

페이지 맨 위에 있는 `Button`에는 페이지 아래쪽의 `Picker` 요소에 바인딩된 3 개의 `Color` 속성이 있습니다. `Picker` 요소의 항목은 프로젝트에 포함 된 `NamedColor` 클래스의 색입니다. 3 개의 `Slider` 요소에는 `Button`의 `FontSize`, `BorderWidth`및 `CornerRadius` 속성에 대 한 양방향 바인딩이 포함 되어 있습니다.

이 프로그램을 사용 하면 이러한 모든 속성의 조합을 테스트할 수 있습니다.

[![단추 모양](button-images/ButtonAppearance.png "단추 모양")](button-images/ButtonAppearance-Large.png)

`Button` 테두리를 보려면 `BorderColor`를 `Default`이외의 값으로 설정 하 고 `BorderWidth`를 양수 값으로 설정 해야 합니다.

IOS에서 많은 테두리 너비가 `Button` 내부에 방해가 텍스트 표시를 방해 한다는 것을 알 수 있습니다. IOS `Button`와 함께 테두리를 사용 하도록 선택 하는 경우 표시 유형을 유지 하기 위해 공백을 사용 하 여 `Text` 속성을 시작 하 고 종료 하는 것이 좋습니다.

UWP에서 `Button` 높이의 절반을 초과 하는 `CornerRadius`를 선택 하면 예외가 발생 합니다.

## <a name="button-visual-states"></a>단추 시각적 상태

[`Button`](xref:Xamarin.Forms.Button) 에는 사용 하도록 설정 된 경우 사용자가 이동할 때 `Button`에 대 한 시각적 변경을 시작 하는 데 사용할 수 있는 `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) 있습니다.

다음 XAML 예제에서는 `Pressed` 상태에 대 한 시각적 상태를 정의 하는 방법을 보여 줍니다.

```xaml
<Button Text="Click me!"
        ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="1" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="0.8" />
                </VisualState.Setters>
            </VisualState>

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Button>
```

`Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) [`Button`](xref:Xamarin.Forms.Button) 를 누를 때 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성이 기본값 1에서 0.8로 변경 되도록 지정 합니다. `Normal` `VisualState`는 `Button` 정상 상태 이면 해당 `Scale` 속성이 1로 설정 되도록 지정 합니다. 따라서 전반적인 효과는 `Button`을 누를 때 약간 더 작은 것으로 재조정, `Button` 해제 될 때 기본 크기로 재조정 됩니다.

시각적 상태에 대 한 자세한 내용은 [Xamarin.ios 시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)를 참조 하세요.

## <a name="creating-a-toggle-button"></a>설정/해제 단추 만들기

On off 스위치와 같이 작동 하도록 `Button` 서브 클래스를 사용할 수 있습니다. 단추를 한 번 탭 하 여 단추를 설정/해제 하 고 다시 탭 하 여 설정/해제 합니다.

다음 `ToggleButton` 클래스는 `Button`에서 파생 되며 `Toggled` 이라는 새 이벤트와 `IsToggled`라는 부울 속성을 정의 합니다. 이러한 속성은 Xamarin.ios [`Switch`](xref:Xamarin.Forms.Switch)에서 정의한 두 가지 속성입니다.

```csharp
class ToggleButton : Button
{
    public event EventHandler<ToggledEventArgs> Toggled;

    public static BindableProperty IsToggledProperty =
        BindableProperty.Create("IsToggled", typeof(bool), typeof(ToggleButton), false,
                                propertyChanged: OnIsToggledChanged);

    public ToggleButton()
    {
        Clicked += (sender, args) => IsToggled ^= true;
    }

    public bool IsToggled
    {
        set { SetValue(IsToggledProperty, value); }
        get { return (bool)GetValue(IsToggledProperty); }
    }

    protected override void OnParentSet()
    {
        base.OnParentSet();
        VisualStateManager.GoToState(this, "ToggledOff");
    }

    static void OnIsToggledChanged(BindableObject bindable, object oldValue, object newValue)
    {
        ToggleButton toggleButton = (ToggleButton)bindable;
        bool isToggled = (bool)newValue;

        // Fire event
        toggleButton.Toggled?.Invoke(toggleButton, new ToggledEventArgs(isToggled));

        // Set the visual state
        VisualStateManager.GoToState(toggleButton, isToggled ? "ToggledOn" : "ToggledOff");
    }
}
```

`ToggleButton` 생성자는 `IsToggled` 속성의 값을 변경할 수 있도록 `Clicked` 이벤트에 처리기를 연결 합니다. `OnIsToggledChanged` 메서드는 `Toggled` 이벤트를 발생 시킵니다.

`OnIsToggledChanged` 메서드의 마지막 줄은 두 텍스트 문자열 "ToggledOn" 및 "ToggledOff"를 사용 하 여 정적 `VisualStateManager.GoToState` 메서드를 호출 합니다. 이 메서드와 응용 프로그램이 [**Xamarin.ios 시각적 상태 관리자**](~/xamarin-forms/user-interface/visual-state-manager.md)문서에서 시각적 상태에 응답 하는 방법에 대해 알아볼 수 있습니다.

`ToggleButton` `VisualStateManager.GoToState`에 대 한 호출을 수행 하기 때문에 클래스 자체에는 `IsToggled` 상태에 따라 단추의 모양을 변경 하는 추가 기능이 포함 되지 않아도 됩니다. 이것은 `ToggleButton`를 호스팅하는 XAML의 책임입니다.

**설정/해제 단추 데모** 페이지에는 시각적 상태를 기준으로 단추의 `Text`, `BackgroundColor`및 `TextColor`을 설정 하는 시각적 상태 관리자 태그를 포함 하 여 `ToggleButton`의 두 인스턴스가 포함 됩니다.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ToggleButtonDemoPage"
             Title="Toggle Button Demo">

    <ContentPage.Resources>
        <Style TargetType="local:ToggleButton">
            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            <Setter Property="HorizontalOptions" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <local:ToggleButton Toggled="OnItalicButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Italic Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Italic On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <local:ToggleButton Toggled="OnBoldButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Bold Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Bold On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <Label x:Name="label"
               Text="Just a little passage of some sample text that can be formatted in italic or boldface by toggling the two buttons."
               FontSize="Large"
               HorizontalTextAlignment="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

`Toggled` 이벤트 처리기는 코드 숨김이 파일에 있습니다. 단추의 상태를 기준으로 `Label`의 `FontAttributes` 속성을 설정 해야 합니다.

```csharp
public partial class ToggleButtonDemoPage : ContentPage
{
    public ToggleButtonDemoPage ()
    {
        InitializeComponent ();
    }

    void OnItalicButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Italic;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Italic;
        }
    }

    void OnBoldButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Bold;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Bold;
        }
    }
}
```

IOS, Android 및 UWP에서 실행 중인 프로그램이 다음과 같습니다.

[![설정/해제 단추 데모](button-images/ToggleButtonDemo.png "설정/해제 단추 데모")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>단추를 사용 하 여 비트맵을 사용 하 여

`Button` 클래스는 `Button`에 단독으로 또는 텍스트와 함께 비트맵 이미지를 표시 하는 데 사용할 수 있는 [`ImageSource`](xref:Xamarin.Forms.Button.Image) 속성을 정의 합니다. 텍스트 및 이미지 정렬 방식을 지정할 수 있습니다.

`ImageSource` 속성은 [`ImageSource`](xref:Xamarin.Forms.ImageSource)형식입니다. 즉, 파일, 포함 된 리소스, URI 또는 스트림에서 비트맵을 로드할 수 있습니다.

> [!NOTE]
> `Button`는 애니메이션 GIF를 로드할 수 있지만 GIF의 첫 번째 프레임만 표시 됩니다.

Xamarin.Forms에서 지 원하는 각 플랫폼 이미지를 응용 프로그램에서 실행 될 수 있는 다양 한 장치의 다른 픽셀 해상도 대해 다양 한 크기에 저장할 수 있습니다. 여러 비트맵은 명명 된 또는 저장 장치의 비디오에 대 한 운영 체제 가장 일치 하는 중 선택할 수는 방식으로 디스플레이 해상도입니다.

`Button`비트맵의 경우 가장 좋은 크기는 일반적으로 원하는 크기에 따라 32과 64 장치 독립적 단위 사이에 있습니다. 이 예에서 사용 된 이미지 48 장치 독립적 단위 크기를 기반으로 합니다.

IOS 프로젝트의 **Resources** 폴더에는이 이미지의 세 가지 크기가 포함 되어 있습니다.

- **/Resources/MonkeyFace.png** 로 저장 된 48 픽셀 사각형 비트맵
- **/Resource/MonkeyFace@2x.png** 로 저장 된 96 픽셀 사각형 비트맵
- **/Resource/MonkeyFace@3x.png** 로 저장 된 144 픽셀 사각형 비트맵

세 개의 비트맵 모두 **BundleResource**의 **빌드 작업** 을 제공 했습니다.

Android 프로젝트의 경우 비트맵은 모두 동일한 이름을 갖지만 **Resources** 폴더의 다른 하위 폴더에 저장 됩니다.

- **/Resources/drawable-hdpi/MonkeyFace.png** 로 저장 된 72 픽셀 사각형 비트맵
- **/Resources/drawable-xhdpi/MonkeyFace.png** 로 저장 된 96 픽셀 사각형 비트맵
- **/Resources/drawable-xxhdpi/MonkeyFace.png** 로 저장 된 144 픽셀 사각형 비트맵
- **/Resources/drawable-xxxhdpi/MonkeyFace.png** 로 저장 된 192 픽셀 사각형 비트맵

여기에는 **Androidresource**의 **빌드 작업이** 제공 되었습니다.

UWP 프로젝트에서 비트맵은 프로젝트의 어디에 나 저장 될 수 있지만 일반적으로 사용자 지정 폴더 또는 **자산** 기존 폴더에 저장 됩니다. UWP 프로젝트에 이러한 비트맵을 포함 되어 있습니다.

- **/Assets/MonkeyFace.scale-100.png** 로 저장 된 48 픽셀 사각형 비트맵
- **/Assets/MonkeyFace.scale-200.png** 로 저장 된 96 픽셀 사각형 비트맵
- **/Assets/MonkeyFace.scale-400.png** 로 저장 된 192 픽셀 사각형 비트맵

**콘텐츠의** **빌드 작업이** 모두 제공 되었습니다.

`Button`의 [`ContentLayout`](xref:Xamarin.Forms.Button.ContentLayout) 속성을 사용 하 여 `Button`에서 `Text` 및 `ImageSource` 속성을 정렬 하는 방법을 지정할 수 있습니다. 이 속성은 `Button`의 포함 클래스인 [`ButtonContentLayout`](xref:Xamarin.Forms.Button.ButtonContentLayout)형식입니다. [생성자](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double)) 에는 두 개의 인수가 있습니다.

- [`ImagePosition`](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) 열거형의 멤버: `Left`, `Top`, `Right`또는 텍스트를 기준으로 비트맵이 표시 되는 방법을 나타내는 `Bottom`입니다.
- 비트맵과 텍스트 사이의 간격에 대 한 `double` 값입니다.

기본값은 `Left` 및 10 단위입니다. [`Position`](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) 이라는 `ButtonContentLayout`의 두 가지 읽기 전용 속성은 해당 속성의 값을 제공 [`Spacing`](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) .

코드에서 `Button` 만들고 다음과 같이 `ContentLayout` 속성을 설정할 수 있습니다.

```csharp
Button button = new Button
{
    Text = "button text",
    ImageSource = new FileImageSource
    {
        File = "image filename"
    },
    ContentLayout = new Button.ButtonContentLayout(Button.ButtonContentLayout.ImagePosition.Right, 20)
};
```

XAML, 열거형 멤버에만 또는 간격을 지정 해야 또는 쉼표로 구분 된 순서에 관계 없이 모두:

```xaml
<Button Text="button text"
        ImageSource="image filename"
        ContentLayout="Right, 20" />
```

**이미지 단추 데모** 페이지는 `OnPlatform`을 사용 하 여 IOS, ANDROID 및 UWP 비트맵 파일의 다른 파일 이름을 지정 합니다. 각 플랫폼에 대해 동일한 파일 이름을 사용 하 고 `OnPlatform`사용 하지 않으려면 UWP 비트맵을 프로젝트의 루트 디렉터리에 저장 해야 합니다.

**이미지 단추 데모** 페이지의 첫 번째 `Button`는 `Image` 속성을 설정 하지만 `Text` 속성은 설정 하지 않습니다.

```xaml
<Button>
    <Button.ImageSource>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="MonkeyFace.png" />
            <On Platform="UWP" Value="Assets/MonkeyFace.png" />
        </OnPlatform>
    </Button.ImageSource>
</Button>
```

UWP 비트맵은 프로젝트의 루트 디렉터리에 저장 되 면이 태그는 상당히 간소화할 수 있습니다.

```xaml
<Button ImageSource="MonkeyFace.png" />
```

**Imagebuttondemo .xaml** 파일에서 많은 반복적인 태그를 방지 하기 위해 `ImageSource` 속성을 설정 하도록 암시적 `Style`도 정의 됩니다. 이 `Style`는 다른 5 개의 `Button` 요소에 자동으로 적용 됩니다. 다음은 전체 XAML 파일이입니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.ImageButtonDemoPage">

    <FlexLayout Direction="Column"
                JustifyContent="SpaceEvenly"
                AlignItems="Center">

        <FlexLayout.Resources>
            <Style TargetType="Button">
                <Setter Property="ImageSource">
                    <OnPlatform x:TypeArguments="ImageSource">
                        <On Platform="iOS, Android" Value="MonkeyFace.png" />
                        <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                    </OnPlatform>
                </Setter>
            </Style>
        </FlexLayout.Resources>

        <Button>
            <Button.ImageSource>
                <OnPlatform x:TypeArguments="ImageSource">
                    <On Platform="iOS, Android" Value="MonkeyFace.png" />
                    <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                </OnPlatform>
            </Button.ImageSource>
        </Button>

        <Button Text="Default" />

        <Button Text="Left - 10"
                ContentLayout="Left, 10" />

        <Button Text="Top - 10"
                ContentLayout="Top, 10" />

        <Button Text="Right - 20"
                ContentLayout="Right, 20" />

        <Button Text="Bottom - 20"
                ContentLayout="Bottom, 20" />
    </FlexLayout>
</ContentPage>
```

최종 4 개의 `Button` 요소는 `ContentLayout` 속성을 사용 하 여 텍스트와 비트맵의 위치와 간격을 지정 합니다.

[![이미지 단추 데모](button-images/ImageButtonDemo.png "이미지 단추 데모")](button-images/ImageButtonDemo-Large.png#lightbox)

이제 `Button` 이벤트를 처리 하 고 `Button` 모양을 변경할 수 있는 다양 한 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [ButtonDemos 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)
- [단추 API](xref:Xamarin.Forms.Button)
