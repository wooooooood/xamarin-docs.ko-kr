---
title: Xamarin.Forms 단추
description: 단추를 탭 하거나 특정 작업을 수행 하는 응용 프로그램을 지시 하는 클릭에 응답 합니다.
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/19/2018
ms.openlocfilehash: cdf89b55c30b0a4e7ab247c396a870e0bad24886
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287709"
---
# <a name="xamarinforms-button"></a>Xamarin.Forms 단추

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)

_단추를 탭 하거나 특정 작업을 수행 하는 응용 프로그램을 지시 하는 클릭에 응답 합니다._

합니다 [ `Button` ](xref:Xamarin.Forms.Button) 모든 Xamarin.Forms의 가장 기본적인 대화형 컨트롤입니다. `Button` 명령 하지만 나타내는 짧은 텍스트 문자열 수도 표시 비트맵 이미지를 또는 텍스트의 조합 및 이미지를 표시 하는 일반적으로 합니다. 사용자가 `Button` 손가락을 사용 하 여 명령을 시작 하려면 마우스로 클릭 하거나 탭 할 합니다.

아래 설명 된 항목의 대부분의 페이지에 해당 합니다 [ **ButtonDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) 샘플입니다.

## <a name="handling-button-clicks"></a>클릭할 단추 처리

`Button` 정의 된 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) 사용자가 누를 때 실행 되는 이벤트를 `Button` 손가락이 나 마우스 포인터를 사용 하 여 합니다. 이벤트의 화면에서 손가락이 나 마우스 단추를 놓으면 발생 합니다 `Button`합니다. `Button` 있어야 해당 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) 속성이로 설정 `true` 탭에 응답 해야 합니다.

**기본 단추 클릭** 페이지를 [ **ButtonDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) 샘플을 인스턴스화하는 방법을 보여 줍니다를 `Button` XAML 핸들에 해당 `Clicked` 이벤트. **BasicButtonClickPage.xaml** 파일에는 `StackLayout` 둘 다를 사용 하 여를 `Label` 및 `Button`:

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

`Button` 에 대 한 허용 되는 모든 공간을 차지 하는 경향이 있습니다. 예를 들어, 설정 하지 않으면 합니다 `HorizontalOptions` 속성을 `Button` 이외의 값으로 `Fill`, `Button` 부모의 전체 너비를 차지 합니다.

기본적으로 `Button` 직사각형은 사용 하 여 it 둥근 모퉁이 제공할 수 있지만 합니다 [ `CornerRadius` ](xref:Xamarin.Forms.Button.CornerRadius) 속성 섹션에서 설명한 대로 [ **모양 단추** ](#button-appearance).

합니다 [ `Text` ](xref:Xamarin.Forms.Button.Text) 속성에 표시 되는 텍스트를 지정 합니다 `Button`합니다. 합니다 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) 이벤트를 명명 된 이벤트 처리기로 `OnButtonClicked`합니다. 이 처리기를 코드 숨김 파일에 위치한 **BasicButtonClickPage.xaml.cs**:

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

경우는 `Button` 를 탭 할는 `OnButtonClicked` 메서드를 실행 합니다. 합니다 `sender` 인수가 `Button` 이 이벤트에 대 한 개체입니다. 에 액세스 하는 데 사용할 수 있습니다는 `Button` 개체 또는 여러 구별할 수 `Button` 공유 하는 동일한 개체 `Clicked` 이벤트입니다.

이 특정 `Clicked` 회전 하는 애니메이션 함수를 호출 하는 처리기는 `Label` 1000 밀리초의 360도 합니다. Windows 10 데스크톱 및 유니버설 Windows 플랫폼 (UWP) 응용 프로그램을 iOS 및 Android 장치에서 실행 중인 프로그램이 다음과 같습니다.

[![기본 단추를 클릭](button-images/BasicButtonClick.png "기본 단추 클릭")](button-images/BasicButtonClick-Large.png#lightbox "기본 단추 클릭")

`OnButtonClicked` 메서드를 포함 합니다 `async` 한정자 때문에 `await` 이벤트 처리기 내에서 사용 됩니다. `Clicked` 이벤트 처리기에 필요 합니다 `async` 처리기의 본문을 사용 하는 경우에 한정자 `await`합니다.

각 플랫폼 렌더링을 `Button` 자체 특정 방식에서. 에 [ **모양 단추** ](#button-appearance) 섹션인 색을 설정 하 고 확인 하는 방법을 배웁니다를 `Button` 테두리 모양을 사용자 지정된에 대 한 표시 합니다. `Button` 구현 된 [ `IFontElement` ](xref:Xamarin.Forms.Internals.IFontElement) 인터페이스를 포함 하도록 [ `FontFamily` ](xref:Xamarin.Forms.Button.FontFamily)를 [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize), 및 [ `FontAttributes` ](xref:Xamarin.Forms.Button.FontAttributes)속성입니다.

## <a name="creating-a-button-in-code"></a>코드에서 단추 만들기

일반적으로 인스턴스화하는 `Button` 에 XAML, 있지만 만들 수 있습니다를 `Button` 코드에서. 응용 프로그램을 사용 하 여 열거할 수 있는 데이터를 기반으로 하는 여러 단추를 만들 때 편리할 수 있습니다는 `foreach` 루프입니다.

합니다 **코드 단추 클릭** 페이지에는 기능적으로 하는 페이지를 만드는 방법을 보여 줍니다 합니다 **기본 단추 클릭** 완전히 이지만 페이지 C#:

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

모든 클래스의 생성자에서 수행 됩니다. 때문에 `Clicked` 처리기가 긴 문을 하나만, 매우 간단 하 게 이벤트에 연결할 수 있습니다.

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

물론, 이벤트 처리기를 별도 메서드로 정의할 수도 있습니다 (마찬가지로 합니다 `OnButtonClick` 의 메서드 **기본 단추 클릭**) 및 이벤트에 해당 메서드를 연결:

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>단추를 사용 하지 않도록 설정

경우에 따라 응용 프로그램은 특정 상태의 경우 특정 `Button` 클릭은 올바른 작업이 아닙니다. 이러한 경우에는 `Button` 해야 사용할 수 없게 설정 하 여 해당 `IsEnabled` 속성을 `false`입니다. 클래식 예제는 파일 열기 `Entry` `Button`와 함께 파일 이름에 대 한 컨트롤입니다. 는 일부 텍스트를에 `Entry`입력 한 경우에만 사용할 수 있습니다. `Button`
사용할 수는 `DataTrigger` 에 표시 된 대로이 태스크에 대 한는 [ **데이터 트리거** ](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) 문서.

## <a name="using-the-command-interface"></a>인터페이스를 사용 하 여

응용 프로그램에 응답할 가능성이 `Button` 처리 없이 탭은 `Clicked` 이벤트입니다. 합니다 `Button` 이라는 다른 알림 메커니즘을 구현 합니다 _명령_ 또는 _명령_ 인터페이스입니다. 이 두 속성으로 구성 됩니다.

- [`Command`](xref:Xamarin.Forms.Button.Command) 형식의 [ `ICommand` ](xref:System.Windows.Input.ICommand)에 정의 된 인터페이스는 [ `System.Windows.Input` ](xref:System.Windows.Input) 네임 스페이스입니다.
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) 형식의 속성 [ `Object` ](xref:System.Object)합니다.

이 접근 방식은 모델-뷰-ViewModel (MVVM) 아키텍처를 구현 하는 경우에 특히 데이터 바인딩 관련 하 여 및에 특히 적합 합니다. 문서에서 설명 하는 이러한 [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)를 [에서 데이터에 대 한 바인딩을 MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), 및 [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)합니다.

MVVM 응용 프로그램에서는 ViewModel 형식의 속성을 정의 `ICommand` 는 XAML에 연결 되어 있는 `Button` 데이터 바인딩 사용 하 여 요소입니다. Xamarin.Forms 정의 [ `Command` ](xref:Xamarin.Forms.Command) 하 고 [ `Command<T>` ](xref:Xamarin.Forms.Command`1) 구현 하는 클래스는 `ICommand` 인터페이스 및 형식의 속성정의ViewModel을지원하는`ICommand`.

문서에 자세히 설명 되어 명령 [ **The 명령 인터페이스** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) 되지만 **기본 단추 명령** 페이지에 [  **ButtonDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) 샘플에는 기본적인 방법을 보여 줍니다.

합니다 `CommandDemoViewModel` 클래스는 형식의 속성을 정의 하는 매우 간단한 ViewModel `double` 라는 `Number`, 및 형식의 두 가지 속성 `ICommand` 라는 `MultiplyBy2Command` 및 `DivideBy2Command`:

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

두 개의 `ICommand` 속성 형식의 두 개체를 사용 하 여 클래스의 생성자에서 초기화 됩니다 `Command`합니다. `Command` 작은 함수를 포함 하는 생성자 (호출 합니다 `execute` 생성자 인수) 두 배로 증가 또는는 `Number` 속성입니다.

합니다 **BasicButtonCommand.xaml** 파일 집합 해당 `BindingContext` 인스턴스에 `CommandDemoViewModel`합니다. 합니다 `Label` 요소와 두 개의 `Button` 요소에 포함 하는 세 가지 속성에 바인딩 `CommandDemoViewModel`:

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

두도 `Button` 요소는 탭, 명령이 실행 되는 숫자 값 변경 및:

[![기본 단추 명령](button-images/BasicButtonCommand.png "기본 단추 명령")](button-images/BasicButtonCommand-Large.png#lightbox)

통해이 방법의 장점은 `Clicked` 처리기는이 페이지의 기능을 포함 하는 모든 논리는 있는 코드 숨김 파일을 대신 ViewModel에 비즈니스 논리에서 더 나은 사용자 인터페이스를 분리를 달성 합니다.

수 이기도 합니다 `Command` 활성화 및 비활성화를 제어 하는 개체는 `Button` 요소입니다. 예를 들어, 2 사이의 숫자 값의 범위를 제한 하려는<sup>10</sup> 2<sup>&ndash;10</sup>합니다. 생성자에 다른 함수를 추가할 수 있습니다 (호출을 `canExecute` 인수) 반환 하는 `true` 경우는 `Button` 활성화 해야 합니다. 같습니다. 수정 된 `CommandDemoViewModel` 생성자:

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

에 대 한 호출을 `ChangeCanExecute` 메서드의 `Command` 필요한 있도록를 `Command` 메서드를 호출할 수는 `canExecute` 메서드 확인 하 고 있는지 여부를 `Button` 여부 수 없게 됩니다. 이 코드 변경으로 개수로 제한에 도달 합니다 `Button` 을 사용할 수 없습니다.

[![기본 단추 명령-수정할](button-images/BasicButtonCommandModified.png "수정 하는 기본 단추 명령-")](button-images/BasicButtonCommandModified-Large.png#lightbox)

두 개 이상에 대 한 있기 `Button` 요소를 동일 하 게 바인딩할 수 `ICommand` 속성입니다. 합니다 `Button` 요소를 사용 하 여 구분할 수는 [ `CommandParameter` ](xref:Xamarin.Forms.Button.CommandParameter) 속성의 `Button`합니다. 이 예에서는 사용 하려는 제네릭 [ `Command<T>` ](xref:Xamarin.Forms.Command`1) 클래스입니다. 합니다 `CommandParameter` 개체를 인수로 전달 되는 `execute` 및 `canExecute` 메서드. 이 기술은에 자세히 표시 됩니다는 [ **기본 명령** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) 섹션의 [ **명령 인터페이스** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) 문서.

합니다 [ **ButtonDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) 샘플에서이 기술을 사용 하 여 해당 `MainPage` 클래스입니다. **MainPage.xaml** 파일 포함을 `Button` 샘플의 각 페이지에 대 한 합니다.

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

각 `Button` 에 해당 `Command` 속성 이름이 속성에 바인딩할 `NavigateCommand`, 및 `CommandParameter` 로 설정 됩니다는 [ `Type` ](xref:System.Type) 프로젝트 페이지 클래스 중 하나에 해당 하는 개체입니다.

`NavigateCommand` 형식의 속성이 `ICommand` 코드 숨김 파일에 정의 됩니다.

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

생성자가 초기화를 `NavigateCommand` 속성을를 `Command<Type>` 때문에 개체 `Type` 유형의 `CommandParameter` XAML 파일에서 설정 하는 개체입니다. 즉 합니다 `execute` 메서드는 형식의 인수 `Type` 이에 해당 하는 `CommandParameter` 개체입니다. 함수 페이지를 인스턴스화하고 여기로 이동 합니다.

설정 하 여 마지막 생성자는 해당 `BindingContext` 자신에 게 합니다. 이 속성에 바인딩할 XAML 파일에 대 한 필요를 `NavigateCommand` 속성입니다.

## <a name="pressing-and-releasing-the-button"></a>누른 단추에서 손을 떼기

외에 합니다 `Clicked` 이벤트 `Button` 도 정의 [ `Pressed` ](xref:Xamarin.Forms.Button.Pressed) 하 고 [ `Released` ](xref:Xamarin.Forms.Button.Released) 이벤트입니다. 합니다 `Pressed` 이벤트 손가락을 누를 때 발생을 `Button`, 또는 위에 포인터를 사용 하 여 마우스 단추를 누를 `Button`합니다. `Released` 이벤트 손가락이 나 마우스 단추를 놓을 때 발생 합니다. 일반적으로 `Clicked` 이벤트와 동일한 시간에도 발생 합니다 `Released` 손가락이 나 마우스 포인터의 화면에서 슬라이드 경우 하지만 이벤트를 `Button` 반환 되기 전에 `Clicked` 이벤트가 발생 하지 않을 수 있습니다.

`Pressed` 하 고 `Released` 이벤트는 자주 사용 되지 않지만에서 설명한 것 처럼 특별 한 용도로 사용할 수 있습니다는 **누르고 분리 단추** 페이지. XAML 파일에 포함 되어는 `Label` 와 `Button` 처리기에 대 한 연결을 사용 하 여 합니다 `Pressed` 및 `Released` 이벤트:

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

코드 숨김 파일에 애니메이션을 적용 합니다 `Label` 때를 `Pressed` 이벤트 발생 하지만 회전을 일시 중단 때는 `Released` 이벤트 발생:

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

결과 `Label` contact에 손가락을 움직일 때만 회전을 `Button`, 손가락 해제 되 면 청구가 중지 됩니다:

[![단추를 눌렀다](button-images/PressAndReleaseButton.png "누른 단추")](button-images/PressAndReleaseButton-Large.png)

이러한 종류의 동작에는 게임 응용 프로그램이 있습니다. 에 있는 손가락이 `Button` 화면 개체를 특정 방향으로 이동할 수 있습니다.

<a name="button-appearance" />

## <a name="button-appearance"></a>단추 모양

`Button` 상속 되거나 해당 모양에 영향을 주는 몇 가지 속성을 정의 합니다.

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) 색인을 `Button` 텍스트
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 그 텍스트에 배경의 색
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) 주위의 영역 색인을 `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) 텍스트에 대 한 글꼴 패밀리를 사용 하 시겠습니까
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) 텍스트의 크기
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) 기울임꼴 또는 굵게 표시 된 텍스트 인지 여부를 나타냅니다.
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) 테두리의 너비
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) 모서리 반지름을 `Button`

> [!NOTE]
> `Button` 클래스에 [ `Margin` ](xref:Xamarin.Forms.View.Margin) 하 고 [ `Padding` ](xref:Xamarin.Forms.Button.Padding) 레이아웃 동작을 제어 하는 속성을 `Button`합니다. 자세한 내용은 [여백 및 안쪽 여백](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)합니다.

이러한 속성 중 6 개 미치는 (제외 `FontFamily` 및 `FontAttributes`)에서 설명 됩니다 합니다 **단추의 모양을** 페이지. 다른 속성인 [ `Image` ](xref:Xamarin.Forms.Button.ImageSource), 섹션에 설명 되어 [ **단추를 사용 하 여 비트맵을 사용 하 여**](#image-button)합니다.

뷰 및 데이터 바인딩에 모든 합니다 **단추의 모양을** XAML 파일에 정의 된 페이지:

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

`Button` 페이지의 맨 위에 있는 세 `Color` 속성에 바인딩된 `Picker` 페이지의 맨 아래에 있는 요소입니다. 항목의 `Picker` 요소가에서 색은 `NamedColor` 프로젝트에 포함 하는 클래스입니다. 세 `Slider` 요소에 대 한 양방향 바인딩을 포함 합니다 `FontSize`, `BorderWidth`, 및 `CornerRadius` 속성의는 `Button`.

이 프로그램을 사용 하면 이러한 모든 속성의 조합을 테스트할 수 있습니다.

[![단추 모양](button-images/ButtonAppearance.png "모양 단추")](button-images/ButtonAppearance-Large.png)

참조 하는 `Button` 설정 해야 테두리를 `BorderColor` 이외의 값으로 `Default`, 및 `BorderWidth` 양수 값으로.

IOS에서 보면 큰 테두리 너비의 내부를 저해 하는 `Button` 텍스트 표시를 방해 합니다. IOS를 사용 하 여 테두리를 사용 하기로 `Button`, 시작 및 종료 하 고 싶을 `Text` 보이도록 유지 하려면 공백 사용 하 여 속성입니다.

UWP에서 선택 하는 `CornerRadius` 높이의 절반을 초과 하는 `Button` 예외를 발생 시킵니다.

## <a name="button-visual-states"></a>단추 시각적 상태

[`Button`](xref:Xamarin.Forms.Button) 에 `Pressed` [ `VisualState` ](xref:Xamarin.Forms.VisualState) 시각적으로 변화를 시작 하려면 사용할 수 있는 `Button` 설정 된 사용자가 누를 때.

다음 XAML 예제에 대 한 시각적 상태를 정의 하는 방법을 보여 줍니다는 `Pressed` 상태:

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

`Pressed` [ `VisualState` ](xref:Xamarin.Forms.VisualState) 되도록 지정 합니다 [ `Button` ](xref:Xamarin.Forms.Button) 를 누르면 해당 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 속성에서 변경할 수는 해당 1 0.8은 사용자의 기본 값입니다. 합니다 `Normal` `VisualState` 되도록 지정 합니다 `Button` 정상 상태의 해당 `Scale` 속성이 1로 설정 됩니다. 따라서 전체적인 효과 때를 `Button` 는 약간 더 작은 및 시기를 재조정는 누른는 `Button` 는 기본 크기를 재조정은 해제 합니다.

시각적 상태에 대 한 자세한 내용은 참조 하세요. [은 Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)합니다.

## <a name="creating-a-toggle-button"></a>설정/해제 단추 만들기

온-오프 스위치와 `Button` 같이 작동 하도록 서브 클래스를 사용할 수 있습니다. 단추를 한 번 누르고 단추를 다시 탭 하 여 설정/해제 합니다.

다음 `ToggleButton` 클래스에서 파생 되며 `Button` 라는 새 이벤트를 정의 하 고 `Toggled` 및 라는 부울 속성이 `IsToggled`합니다. 다음은 Xamarin.Forms를 정의한 동일한 두 개의 속성 [ `Switch` ](xref:Xamarin.Forms.Switch):

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

`ToggleButton` 생성자에 처리기를 연결 합니다 `Clicked` 한다는의 값을 변경할 수 있도록 이벤트를 `IsToggled` 속성입니다. 합니다 `OnIsToggledChanged` 메서드 실행을 `Toggled` 이벤트입니다.

마지막 줄을 `OnIsToggledChanged` 정적 호출 `VisualStateManager.GoToState` 메서드 두 텍스트 문자열 "ToggledOn" 및 "ToggledOff"입니다. 읽을 수 있습니다이 메서드 및 응용 프로그램 응답 하는 방법을 문서의 시각적 상태에 대 한 [ **The Xamarin.Forms Visual State Manager**](~/xamarin-forms/user-interface/visual-state-manager.md)합니다.

때문에 `ToggleButton` 를 호출할 `VisualStateManager.GoToState`, 클래스 자체를 기반으로 단추의 모양을 변경 하려면 추가 기능 포함 될 필요가 없습니다 해당 `IsToggled` 상태입니다. 즉 호스트 하는 XAML의 책임을 `ToggleButton`입니다.

**토글 단추 데모** 의 두 인스턴스를 포함 하는 페이지 `ToggleButton`를 설정 하는 Visual State Manager 태그를 포함 하 여를 `Text`, `BackgroundColor`, 및 `TextColor` 단추의 표시 상태에 따라:

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

`Toggled` 이벤트 처리기의 코드 숨김 파일에 있습니다. 설정에 대 한 책임이 `FontAttributes` 의 속성을 `Label` 단추의 상태를 기반으로:

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

[![단추 데모를 설정/해제](button-images/ToggleButtonDemo.png "단추 데모를 설정/해제")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>단추를 사용 하 여 비트맵을 사용 하 여

`Button` 클래스 정의 [ `ImageSource` ](xref:Xamarin.Forms.Button.Image) 에서 비트맵 이미지를 표시할 수 있는 속성을 `Button`, 단독으로 또는 텍스트와 함께에서 합니다. 텍스트 및 이미지 정렬 방식을 지정할 수 있습니다.

속성은 형식 [`ImageSource`](xref:Xamarin.Forms.ImageSource)입니다. 즉, 파일, 포함 된 리소스, URI 또는 스트림에서 비트맵을 로드할 수 있습니다. `ImageSource`

Xamarin.Forms에서 지 원하는 각 플랫폼 이미지를 응용 프로그램에서 실행 될 수 있는 다양 한 장치의 다른 픽셀 해상도 대해 다양 한 크기에 저장할 수 있습니다. 여러 비트맵은 명명 된 또는 저장 장치의 비디오에 대 한 운영 체제 가장 일치 하는 중 선택할 수는 방식으로 디스플레이 해상도입니다.

비트맵을 `Button`최적의 크기는 일반적으로 32 비트 및 64 장치 독립적 단위 간에, 크기에 따라 원하는 되도록 합니다. 이 예에서 사용 된 이미지 48 장치 독립적 단위 크기를 기반으로 합니다.

IOS 프로젝트에는 **리소스** 이 이미지의 세 가지 크기를 포함 하는 폴더:

- 48 픽셀 사각형 비트맵으로 저장 **/Resources/MonkeyFace.png**
- 96 픽셀 사각형 비트맵으로 저장 **/Resource/MonkeyFace@2x.png**
- 144 픽셀 사각형 비트맵으로 저장 **/Resource/MonkeyFace@3x.png**

제공 된 모든 세 가지 비트맵을 **빌드 작업** 의 **BundleResource**합니다.

Android 프로젝트에 대 한 모든 비트맵 동일한 이름을 갖지만의 다른 하위 폴더에 저장 됩니다는 **리소스** 폴더:

- 72 픽셀 사각형 비트맵으로 저장 **/Resources/drawable-hdpi/MonkeyFace.png**
- 96 픽셀 사각형 비트맵으로 저장 **/Resources/drawable-xhdpi/MonkeyFace.png**
- 144 픽셀 사각형 비트맵으로 저장 **/Resources/drawable-xxhdpi/MonkeyFace.png**
- 192 픽셀 사각형 비트맵으로 저장 **/Resources/drawable-xxxhdpi/MonkeyFace.png**

제공 된 이러한를 **빌드 작업** 의 **AndroidResource**합니다.

UWP 프로젝트에서 비트맵에에서 저장할 수 어디서 나 프로젝트에 있지만 일반적으로 사용자 지정 폴더에 저장 됩니다 또는 **자산** 기존 폴더입니다. UWP 프로젝트에 이러한 비트맵을 포함 되어 있습니다.

- 48 픽셀 사각형 비트맵으로 저장 **/Assets/MonkeyFace.scale-100.png**
- 96 픽셀 사각형 비트맵으로 저장 **/Assets/MonkeyFace.scale-200.png**
- 192 픽셀 사각형 비트맵으로 저장 **/Assets/MonkeyFace.scale-400.png**

모든 제공 된를 **빌드 작업** 의 **콘텐츠**합니다.

지정할 수 있습니다 하는 방법을 `Text` 및 `ImageSource` 속성에 정렬 되는 `Button` 를 사용 하 여를 [ `ContentLayout` ](xref:Xamarin.Forms.Button.ContentLayout) 속성 `Button`. 이 속성은 형식의 [ `ButtonContentLayout` ](xref:Xamarin.Forms.Button.ButtonContentLayout)에 포함된 된 클래스에는 `Button`합니다. 합니다 [생성자](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double)) 두 인수를 포함 합니다.

- 멤버는 [ `ImagePosition` ](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) 열거형: `Left`, `Top`를 `Right`, 또는 `Bottom` 비트맵 텍스트를 기준으로 표시 되는 방식을 나타내는입니다.
- `double` 비트맵와 텍스트 사이의 간격에 대 한 값입니다.

기본값은 `Left` 및 10 단위입니다. 두 가지 읽기 전용 속성 `ButtonContentLayout` 라는 [ `Position` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) 하 고 [ `Spacing` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) 이러한 속성의 값을 제공 합니다.

코드에서 만들 수 있습니다는 `Button` 설정의 `ContentLayout` 이와 같이 속성:

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

합니다 **이미지 단추 데모** 사용 하 여 페이지 `OnPlatform` iOS, Android 및 UWP 비트맵 파일에 대 한 다른 파일 이름을 지정 합니다. 각 플랫폼에 대해 동일한 파일 이름을 사용 하 고 사용 하지 않도록 하려는 경우 `OnPlatform`, UWP 비트맵을 프로젝트의 루트 디렉터리에 저장 해야 합니다.

첫 번째 `Button` 에 **이미지 단추 데모** 집합 페이지를 `Image` 속성 아닌는 `Text` 속성:

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

태그를 반복적으로 많이 방지 하려면 합니다 **ImageButtonDemo.xaml** 파일을 암시적 `Style` 설정도 정의 되어를 `ImageSource` 속성입니다. 이렇게 `Style` 5 다른 자동으로 적용 됩니다 `Button` 요소입니다. 다음은 전체 XAML 파일이입니다.

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

마지막 네 `Button` 요소를 사용 합니다 `ContentLayout` 위치 및 텍스트와 비트맵의 간격을 지정 하는 속성:

[![이미지 단추 데모](button-images/ImageButtonDemo.png "단추 데모 이미지")](button-images/ImageButtonDemo-Large.png#lightbox)

처리할 수 있는 다양 한 방법을 확인 하 셨 `Button` 이벤트 및 변경 된 `Button` 모양입니다.

## <a name="related-links"></a>관련 링크

- [ButtonDemos 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)
- [단추 API](xref:Xamarin.Forms.Button)
