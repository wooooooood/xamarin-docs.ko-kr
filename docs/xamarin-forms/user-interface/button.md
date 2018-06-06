---
title: Xamarin.Forms 단추
description: 단추 탭 하거나 특정 태스크를 수행 하는 응용 프로그램에 지시 하는 클릭에 응답 합니다.
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/01/2018
ms.openlocfilehash: 095736e77b2f502261f9b85ab73c45dce74309b9
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734181"
---
# <a name="xamarinforms-button"></a>Xamarin.Forms 단추

_단추 탭 하거나 특정 태스크를 수행 하는 응용 프로그램에 지시 하는 클릭에 응답 합니다._ 

[ `Button` ](xref:Xamarin.Forms.Button) 모든 Xamarin.Forms의 가장 기본적인 대화형 컨트롤입니다. `Button` 표시 하지만 명령을 나타내는 짧은 텍스트 문자열 수도 비트맵 이미지 또는 텍스트의 조합을 이미지를 표시 하는 일반적으로 합니다. 사용자가 `Button` 손가락으로 또는 해당 명령을 시작 하는 마우스 클릭 합니다.

아래에 설명 된 항목의 대부분의 페이지에 해당 하는 [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) 샘플.

## <a name="handling-button-clicks"></a>클릭할 단추 처리

`Button` 정의 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) 사용자가 누를 때 발생 하는 이벤트는 `Button` 손가락 또는 마우스 포인터를 가진 합니다. 화면에서 손가락 또는 마우스 단추를 놓을 때 이벤트가 `Button`합니다. `Button` 있어야 해당 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) 속성이로 설정 `true` 탭에 응답 하기에 대 한 합니다. 

**기본 단추 클릭** 페이지에 [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) 샘플에서는 인스턴스화하는 방법을 보여 줍니다.는 `Button` XAML 핸들에 해당 `Clicked` 이벤트입니다. **BasicButtonClickPage.xaml** 파일에 포함 되어는 `StackLayout` 둘 다와 함께 한 `Label` 및 `Button`:

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

`Button` 에 허용 되는 모든 공간을 차지 하는 경향이 있습니다. 예를 들어, 설정 하지 않으면는 `HorizontalOptions` 속성 `Button` 이외의 다른 값으로 `Fill`, `Button` 부모의 전체 너비를 차지 합니다.

기본적으로는 `Button` 사각형,이 사용 하 여 it 둥근 모서리를 제공할 수 있지만 [ `CornerRadius` ](xref:Xamarin.Forms.Button.CornerRadius) 속성을 아래 섹션에 설명 된 대로 [ **단추 모양** ](#button-appearance).

[ `Text` ](xref:Xamarin.Forms.Button.Text) 속성 지정에 나타나는 텍스트는 `Button`합니다. [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) 이벤트는 이벤트 처리기를 설정 `OnButtonClicked`합니다. 이 처리기 코드 숨김 파일에 있는 **BasicButtonClickPage.xaml.cs**:

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

경우는 `Button` 탭이 수행 되는 `OnButtonClicked` 메서드가 실행 합니다. `sender` 인수가 `Button` 이 이벤트에 대 한 개체입니다. 에 액세스 하는 데 사용할 수 있습니다는 `Button` 개체 또는 여러 구분 하기 위해 `Button` 공유 하는 동일한 개체 `Clicked` 이벤트입니다.

이 특정 `Clicked` 으로 회전 하는 애니메이션 함수를 호출 하는 처리기는 `Label` 1000 밀리초에서 360도 합니다. 다음은 Windows 10 desktop에 iOS 및 Android 장치에서 및 유니버설 Windows 플랫폼 (UWP) 응용 프로그램으로 실행 프로그램입니다.

[![기본 단추 클릭](button-images/BasicButtonClick.png "기본 단추 클릭")](button-images/BasicButtonClick-Large.png#lightbox "기본 단추 클릭")

다음에 유의 `OnButtonClicked` 메서드에 포함 됩니다는 `async` 한정자 때문에 `await` 이벤트 처리기 내에서 사용 됩니다. A `Clicked` 이벤트 처리기에 필요한는 `async` 처리기의 본문을 사용 하는 경우에 한정자 `await`합니다.

각 플랫폼 렌더링는 `Button` 특정 한 방식으로 고유 합니다. 에 [ **단추 모양** ](#button-appearance) 섹션에서 색을 설정 하 고 확인 하는 방법을 확인할 수는 `Button` 테두리 모양을 사용자 지정된에 대 한 표시 합니다. `Button` 구현 하는 [ `IFontElement` ](xref:Xamarin.Forms.Internals.IFontElement) 인터페이스를 포함 하도록 [ `FontFamily` ](xref:Xamarin.Forms.Button.FontFamily), [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize), 및 [ `FontAttributes` ](xref:Xamarin.Forms.Button.FontAttributes)속성입니다.

## <a name="creating-a-button-in-code"></a>코드에서 단추 만들기

일반적으로 인스턴스화하는 `Button` XAML에 있지만 만들 수 있습니다는 `Button` 코드에서. 응용 프로그램으로 열거 가능 데이터를 기반으로 하는 여러 개의 단추를 만들어야 할 때 편리할 수 있습니다는 `foreach` 루프입니다.

**코드 단추 클릭** 페이지에는 기능적으로 하는 페이지를 만드는 방법을 보여 줍니다는 **기본 단추 클릭** 완전히 하지만 C# 페이지:

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

별도 방법으로 이벤트 처리기도 정의할 수 물론, (마찬가지로 `OnButtonClick` 에서 메서드 **기본 단추 클릭**) 및 해당 메서드를 이벤트에 연결:

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>단추 비활성화

경우에 따라 응용 프로그램은 특정 상태에서에 특정 `Button` 클릭은 올바른 작업이 아닙니다. 이러한 경우에는 `Button` 않도록 설정 하 여 해당 `IsEnabled` 속성을 `false`합니다. 기본적인 예로 `Entry` 파일 열기와 함께 파일 이름에 대 한 제어 `Button`:는 `Button` 에 일부 텍스트를 입력 하는 경우에 사용 하도록 설정할 수는 `Entry`합니다. 사용할 수는 `DataTrigger` 와 같이이 작업에 대 한는 [ **데이터 트리거** ](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) 문서.

## <a name="using-the-command-interface"></a>인터페이스 사용

응용 프로그램에 응답 하는 `Button` 처리 없이 탭에서 `Clicked` 이벤트입니다. `Button` 라고 하는 대체 알림 메커니즘을 구현 하는 _명령_ 또는 _명령 실행_ 인터페이스입니다. 이 두 가지 속성으로 구성 됩니다.

- [`Command`](xref:Xamarin.Forms.Button.Command) 형식의 [ `ICommand` ](xref:System.Windows.Input.ICommand)에 정의 된 인터페이스는 [ `System.Windows.Input` ](xref:System.Windows.Input) 네임 스페이스입니다.
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) 형식의 속성이 [ `Object` ](xref:System.Object)합니다.

이 방법은 특히 모델-뷰-MVVM () 아키텍처를 구현 하는 경우와 관련 된 데이터 바인딩 및에 특히 적합 합니다. 문서에서 설명 하는 이러한 [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md), [에서 데이터에 대 한 바인딩을 MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), 및 [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)합니다.

MVVM 응용 프로그램에서 ViewModel 정의 형식의 속성 `ICommand` XAML에 연결 되어 있는 `Button` 데이터 바인딩 사용 하는 요소입니다. Xamarin.Forms도 정의 [ `Command` ]((xref:Xamarin.Forms.Command`1)) 및 [ `Command<T>` ](xref:Xamarin.Forms.Command`1) 구현 하는 클래스는 `ICommand` 인터페이스를 지 원하는 ViewModel 속성을정의`ICommand`.

문서에 자세히 설명 되어 명령 실행 [ **The 명령 인터페이스** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) 있지만 **기본 단추 명령** 페이지에 [  **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) 샘플에는 기본적인 방법을 보여 줍니다.

`CommandDemoViewModel` 클래스는 형식의 속성을 정의 하는 매우 간단한 ViewModel `double` 라는 `Number`와 두 가지 속성 형식의 `ICommand` 라는 `MultiplyBy2Command` 및 `DivideBy2Command`:

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

두 `ICommand` 속성 형식의 두 개체와 해당 클래스의 생성자에서 초기화 된 `Command`합니다. `Command` 작은 함수를 포함 하는 생성자 (호출는 `execute` 생성자 인수) 2 배로 증가 또는 분모를 하는 `Number` 속성입니다.

**BasicButtonCommand.xaml** 파일 집합의 `BindingContext` 인스턴스에 `CommandDemoViewModel`합니다. `Label` 요소와 두 `Button` 요소에서 세 가지 속성에 대 한 바인딩만 포함할 `CommandDemoViewModel`:

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

두도 `Button` 요소 함, 명령이 실행 되는 숫자 값을 변경 하 고:

[![기본 단추 명령](button-images/BasicButtonCommand.png "기본 단추 명령")](button-images/BasicButtonCommand-Large.png#lightbox)

에 비해이 방식의 장점은 `Clicked` 처리기는이 페이지의 기능 관련 된 모든 논리에 위치 하는 코드 숨김 파일이 아니라 ViewModel 비즈니스 논리에서 사용자 인터페이스의 더 잘 분리를 달성 합니다.

에 대 한도 가능는 `Command` 설정 및 해제를 제어 하는 개체는 `Button` 요소입니다. 예를 들어 2 사이의 숫자 값의 범위를 제한 하려고 한다고 가정할 경우<sup>10</sup> 2<sup>&ndash;10</sup>합니다. 생성자에 다른 함수를 추가할 수 있습니다 (라는 `canExecute` 인수)을 반환 하 `true` 경우는 `Button` 활성화 해야 합니다. 다음은 수정 된 `CommandDemoViewModel` 생성자:

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

에 대 한 호출은 `ChangeCanExecute` 메서드 `Command` 필요 있도록는 `Command` 메서드를 호출할 수는 `canExecute` 메서드 확인 여부는 `Button` 또는 사용 하지 않도록 합니다. 이러한 코드 변경으로 인해 수로 제한에 도달는 `Button` 을 사용할 수 없습니다.

[![기본 단추 명령-수정](button-images/BasicButtonCommandModified.png "기본 단추 명령-수정")](button-images/BasicButtonCommandModified-Large.png#lightbox)

두 개 이상의 `Button` 요소를 동일 하 게 바인딩할 수 `ICommand` 속성입니다. `Button` 요소를 사용 하 여 구분할 수는 [ `CommandParameter` ](xref:Xamarin.Forms.Button.CommandParameter) 속성 `Button`합니다. 제네릭 사용 해야 하는 경우에 [ `Command<T>` ](xref:Xamarin.Forms.Command`1) 클래스입니다. `CommandParameter` 개체는 인수를 변수로 전달 되어는 `execute` 및 `canExecute` 메서드. 이 기술이에서 자세히는 [ **기본 명령** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) 의 섹션은 [ **명령 인터페이스** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) 문서.

[ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) 또한 샘플의이 기술에서는 해당 `MainPage` 클래스입니다. **MainPage.xaml** 파일에 포함 되어는 `Button` 샘플의 각 페이지에 대해:

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

각 `Button` 에 해당 `Command` 속성이 바인딩된 속성 `NavigateCommand`, 및 `CommandParameter` 로 설정 되는 [ `Type` ](xref:System.Type) 프로젝트의 페이지 클래스 중 하나에 해당 하는 개체입니다.

`NavigateCommand` 속성은 형식이 `ICommand` 와 코드 숨김 파일에 정의 됩니다.

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

생성자가 초기화 하는 `NavigateCommand` 속성을는 `Command<Type>` 때문에 개체 `Type` 유형의 `CommandParameter` XAML 파일에서 설정 개체입니다. 즉는 `execute` 메서드는 형식의 인수 `Type` 에 해당 하는 `CommandParameter` 개체입니다. 함수는 페이지를 인스턴스화하고 이동 합니다.

설정 하 여 결론을 내립니다 생성자는 해당 `BindingContext` 자신에 게 있습니다. 이 작업은 속성에 바인딩할 XAML 파일에 대 한 필요는 `NavigateCommand` 속성입니다.

## <a name="pressing-and-releasing-the-button"></a>누른 단추에서 손을 떼기

외에는 `Clicked` 이벤트 `Button` 도 정의 [ `Pressed` ](xref:Xamarin.Forms.Button.Pressed) 및 [ `Released` ](xref:Xamarin.Forms.Button.Released) 이벤트입니다. `Pressed` 이벤트 손가락 누를 때 발생 한 `Button`, 또는 위에 포인터를 놓고 마우스 단추를 누를 `Button`합니다. `Released` 이벤트 손가락 또는 마우스 단추를 놓을 때 발생 합니다. 일반적으로 `Clicked` 도 이벤트가 같은 시간에는 `Released` 손가락 또는 마우스 포인터의 화면에서 슬라이드 경우 하지만 이벤트는 `Button` 반환 되기 전에 `Clicked` 이벤트 발생 하지 않을 수 있습니다.

`Pressed` 및 `Released` 이벤트는 자주 사용 되지 않지만에서 같이 특별 한 용도로 사용할 수 있습니다는 **누르고 분리 단추** 페이지. XAML 파일에 포함 되어는 `Label` 및 `Button` 와 연결 된 처리기는 `Pressed` 및 `Released` 이벤트:

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

코드 숨김 파일에 애니메이션 효과 `Label` 때는 `Pressed` 이벤트 발생 한 응응 회전을 일시 중단 때는 `Released` 이벤트 발생:

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

결과 `Label` 만 손가락 contact 하는 동안 회전의 `Button`, 손가락을 놓을 때 중지:

[![단추를 눌렀다](button-images/PressAndReleaseButton.png "단추를 눌렀다")](button-images/PressAndReleaseButton-Large.png)

이러한 종류의 동작에는 게임을 위한 응용 프로그램이: 보유 하 고 손가락은 `Button` on 화면 개체가 특정 방향으로 이동 확인 될 수 있습니다. 

<a name="button-appearance" />

## <a name="button-appearance"></a>단추 모양

`Button` 상속 하거나 해당 모양에 영향을 주는 몇 가지 속성을 정의 합니다.

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) 색의 `Button` 텍스트
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 그 텍스트에 배경 색은
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) 색의 한 영역 주위의 `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) 텍스트에 글꼴 패밀리를 사용 하 시겠습니까
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) 텍스트의 크기
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) 기울임꼴 또는 굵은 텍스트 인지 여부를 나타냅니다.
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) 테두리의 너비 
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) 둥글게 합니다.

이러한 속성 중 여섯 개의 효과 (제외 `FontFamily` 및 `FontAttributes`)에서 보여지는 **단추 모양을** 페이지. 다른 속성 [ `Image` ](xref:Xamarin.Forms.Button.Image), 섹션에서 설명한 [ **비트맵을 사용 하 여 단추와**](#image-button)합니다.

모든의 뷰 및 데이터 바인딩에 **단추 모양을** 페이지 XAML 파일에 정의 된:

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

`Button` 페이지의 위쪽에는 해당 3 `Color` 속성에 바인딩된 `Picker` 페이지 아래쪽에 있는 요소입니다. 항목에는 `Picker` 요소가에서 색은 `NamedColor` 프로젝트에 포함 된 클래스입니다. 세 가지 `Slider` 요소에 대 한 양방향 바인딩을 포함는 `FontSize`, `BorderWidth`, 및 `CornerRadius` 의 속성은 `Button`합니다.

이 프로그램을 사용 하면 이러한 모든 속성의 조합을 테스트할 수 있습니다.

[![단추 모양](button-images/ButtonAppearance.png "단추 모양")](button-images/ButtonAppearance-Large.png)

참조 하는 `Button` 테두리를 설정 하려면 필요 합니다는 `BorderColor` 이외의 다른 값으로 `Default`, 및 `BorderWidth` 양수 값으로.

Ios에서 보면의 내부에 큰 테두리 두께 방해 하는 `Button` 텍스트 표시를 방해 합니다. IOS 사용 하 여 테두리를 사용 하기로 `Button`, 시작 및 종료 좋을 것은 `Text` 보이도록 유지 하려면 공백 사용 하 여 속성입니다.

UWP에서 선택 하는 `CornerRadius` 높이의 절반을 초과 하는 `Button` 예외를 발생 시킵니다.

## <a name="creating-a-toggle-button"></a>설정/해제 단추 만들기

하위 클래스 수 `Button` 설정 / 해제 스위치와 같은 작동 되도록: 하에 있는 단추를 설정/해제 하 고 탭을 다시 해제 단추를 한 번 누릅니다.

다음 `ToggleButton` 클래스에서 파생 `Button` 라는 새 이벤트를 정의 하 고 `Toggled` 및 라는 부울 속성 `IsToggled`합니다. 다음은 Xamarin.Forms에 정의 된 동일한 두 개의 속성 [ `Switch` ](xref:Xamarin.Forms.Switch):

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

`ToggleButton` 에 처리기를 연결 하는 생성자는 `Clicked` 한다는의 값을 변경할 수 있도록 이벤트는 `IsToggled` 속성입니다. `OnIsToggledChanged` 발생 합니다. 메서드는 `Toggled` 이벤트입니다. 

마지막 줄은 `OnIsToggledChanged` 정적 메서드를 호출 `VisualStateManager.GoToState` "ToggledOn" 및 "ToggledOff" 메서드를 두 개의 텍스트 문자열입니다. 읽을 수 있습니다이 방법과 응용 프로그램 응답 하는 방법을 시각적 상태 문서에 대 한 [ **The Xamarin.Forms Visual State Manager**](~/xamarin-forms/user-interface/visual-state-manager.md)합니다. 

때문에 `ToggleButton` 를 호출할 `VisualStateManager.GoToState`, 클래스 자체를 기반으로 하는 단추의 모양을 변경 하는 모든 추가 기능이 포함 되어 필요가 해당 `IsToggled` 상태입니다. 호스트 하는 XAML의 책임 즉는 `ToggleButton`합니다. 

**토글 단추 데모** 의 두 인스턴스를 포함 하는 페이지 `ToggleButton`, 포함 하 여 Visual State Manager를 설정 하는 `Text`, `BackgroundColor`, 및 `TextColor` 시각적 상태에 따라 단추: 

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

`Toggled` 이벤트 처리기 코드 숨김 파일에 있습니다. 설정에 대 한 책임이 `FontAttributes` 의 속성은 `Label` 단추의 상태에 따라:

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

다음은 iOS, Android 및 UWP에서 실행 중인 프로그램입니다.

[![단추 데모를 설정/해제](button-images/ToggleButtonDemo.png "단추 데모를 설정/해제")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>비트맵 단추와 함께 사용 하 여

`Button` 클래스 정의 [ `Image` ](xref:Xamarin.Forms.Button.Image) 속성에 비트맵 이미지를 표시할 수 있도록는 `Button`를 단독으로 또는 텍스트와 함께 사용 합니다. 텍스트 및 이미지 정렬 되는 방식을 지정할 수 있습니다.

`Image` 형식의 속성은 [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource), 즉, 비트맵 개별 플랫폼 프로젝트에서 하 고 표준.NET 라이브러리 프로젝트에 리소스 그룹으로 저장 되어야 합니다. 

Xamarin.Forms에서 지 원하는 각 플랫폼에는 이미지를의 다양 한 장치에 응용 프로그램이 실행 될 수 있는 다른 픽셀 해상도 대해 여러 크기에 저장 될 수 있습니다. 이러한 여러 비트맵의 비디오 장치에 대 한 운영 체제 가장 일치 하는 중 선택할 수는 방식에 저장 되거나 라는 되 디스플레이 해상도입니다. 

비트맵에 대 한는 `Button`, 최적의 크기는 일반적으로 장치 독립적 단위 32에서 64 사이, 크기에 따라 원하는 되도록 합니다. 이 예에서 사용 되는 이미지 48 장치 독립적 단위의 크기를 기반으로 합니다.

IOS 프로젝트에는 **리소스** 이 이미지의 세 가지 크기를 포함 하는 폴더:

- 48 픽셀 정사각형 비트맵으로 저장 **/Resources/MonkeyFace.png**
- 96 픽셀 정사각형 비트맵으로 저장 **/Resource/MonkeyFace@2x.png**
- 144 픽셀 정사각형 비트맵으로 저장 **/Resource/MonkeyFace@3x.png**

모든 세 비트맵 제공는 **빌드 작업** 의 **BundleResource**합니다.

Android 프로젝트에 대 한 모든 비트맵 같은 이름을 갖지만의 서로 다른 하위 폴더에 저장 됩니다는 **리소스** 폴더:

- 72 픽셀 정사각형 비트맵으로 저장 **/Resources/drawable-hdpi/MonkeyFace.png**
- 96 픽셀 정사각형 비트맵으로 저장 **/Resources/drawable-xhdpi/MonkeyFace.png**
- 144 픽셀 정사각형 비트맵으로 저장 **/Resources/drawable-xxhdpi/MonkeyFace.png**
- 192 픽셀 정사각형 비트맵으로 저장 **/Resources/drawable-xxxhdpi/MonkeyFace.png**

제공 된 이러한는 **빌드 작업** 의 **AndroidResource**합니다.

에 UWP 프로젝트에는 프로젝트에 비트맵 아무 곳 이나 저장할 수 있지만 일반적으로 사용자 지정 폴더에 저장 되기 또는 **자산** 기존 폴더입니다. UWP 프로젝트에 이러한 비트맵을 포함 되어 있습니다.

- 48 픽셀 정사각형 비트맵으로 저장 **/Assets/MonkeyFace.scale-100.png**
- 96 픽셀 정사각형 비트맵으로 저장 **/Assets/MonkeyFace.scale-200.png**
- 192 픽셀 정사각형 비트맵으로 저장 **/Assets/MonkeyFace.scale-400.png**

모든 제공 된는 **빌드 작업** 의 **콘텐츠**합니다.

지정할 수 있습니다 방법을 `Text` 및 `Image` 에 정렬 되는 속성의 `Button` 를 사용 하 여는 [ `ContentLayout` ](xref:Xamarin.Forms.Button.ContentLayout) 속성 `Button`합니다. 이 속성은 형식의 [ `ButtonContentLayout` ](xref:Xamarin.Forms.Button.ButtonContentLayout), 포함 된 클래스는 변수인 `Button`합니다. [생성자](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double)) 인수가 두 개:

- 멤버는 [ `ImagePosition` ](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) 열거형: `Left`, `Top`, `Right`, 또는 `Bottom` 비트맵 텍스트를 기준으로 표시 되는 방식을 나타내는입니다.
- A `double` 비트맵와 텍스트 사이의 간격에 대 한 값입니다.

기본값은 `Left` 및 10 단위입니다. 두 읽기 전용 속성 `ButtonContentLayout` 라는 [ `Position` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) 및 [ `Spacing` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) 이러한 속성의 값을 제공 합니다.

코드에서 만들 수 있습니다는 `Button` 설정 하 고는 `ContentLayout` 다음과 같이 속성:

```csharp
Button button = new Button
{
    Text = "button text",
    Image = new FileImageSource
    {
        File = "image filename"
    },
    ContentLayout = new Button.ButtonContentLayout(Button.ButtonContentLayout.ImagePosition.Right, 20)
};
```

XAML은 열거형 멤버 또는, 간격 지정 필요 또는 둘 다에서 순서에 관계 없이 쉼표로 구분 하 여:

```xaml
<Button Text="button text"
        Image="image filename"
        ContentLayout="Right, 20" />
```

**이미지 단추 데모** 사용 하 여 페이지 `OnPlatform` iOS, Android 및 UWP 비트맵 파일에 대 한 서로 다른 파일 이름을 지정할 수 있습니다. 세 플랫폼 모두에 같은 파일 이름을 사용 하 고 사용 하지 않도록 하려는 경우 `OnPlatform`, UWP 비트맵 프로젝트의 루트 디렉터리에 저장 해야 합니다.

첫 번째 `Button` 에 **이미지 단추 데모** 설정 페이지는 `Image` 속성 하지 않고는 `Text` 속성:

```xaml
<Button>
    <Button.Image>
        <OnPlatform x:TypeArguments="FileImageSource">
            <On Platform="iOS, Android" Value="MonkeyFace.png" />
            <On Platform="UWP" Value="Assets/MonkeyFace.png" />
        </OnPlatform>
    </Button.Image>
</Button>
```

UWP 비트맵은 프로젝트의 루트 디렉터리에 저장 되 면이 태그는 상당히 단순화할 수 있습니다.

```xaml
<Button Image="MonkeyFace.png" />
```

반복적인 태그에 많은 방지 하기 위해는 **ImageButtonDemo.xaml** 파일, 암시적 `Style` 설정도 정의 되어는 `Image` 속성입니다. 이 `Style` 다른 5 자동으로 적용 되 `Button` 요소입니다. 다음은 전체 XAML 파일이입니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.ImageButtonDemoPage">

    <FlexLayout Direction="Column"
                JustifyContent="SpaceEvenly"
                AlignItems="Center">
        
        <FlexLayout.Resources>
            <Style TargetType="Button">
                <Setter Property="Image">
                    <OnPlatform x:TypeArguments="FileImageSource">
                        <On Platform="iOS, Android" Value="MonkeyFace.png" />
                        <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                    </OnPlatform>
                </Setter>
            </Style>
        </FlexLayout.Resources>

        <Button>
            <Button.Image>
                <OnPlatform x:TypeArguments="FileImageSource">
                    <On Platform="iOS, Android" Value="MonkeyFace.png" />
                    <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                </OnPlatform>
            </Button.Image>
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

마지막 4 `Button` 요소 확인을 사용 하 여는 `ContentLayout` 위치 및 텍스트와 비트맵의 간격을 지정 하는 속성:

[![이미지 단추 데모](button-images/ImageButtonDemo.png "단추 데모 이미지")](button-images/ImageButtonDemo-Large.png#lightbox)

처리할 수 있는 다양 한 방법으로 이제 보았으며 `Button` 이벤트 및 변경의 `Button` 모양입니다.

## <a name="related-links"></a>관련 링크

- [ButtonDemos 샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)
- [API 단추](xref:Xamarin.Forms.Button)
