---
제목: " Xamarin.Forms 단추" 설명: "단추는 응용 프로그램이 특정 작업을 수행 하도록 지시 하는 탭 하거나 클릭에 응답 합니다."
assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5: xamarin-forms author: davidbritch: dabritch:: 12/04/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-button"></a>Xamarin.Forms 단추

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)

_단추는 응용 프로그램이 특정 작업을 수행 하도록 지시 하는 탭 하거나 클릭에 응답 합니다._

는 [`Button`](xref:Xamarin.Forms.Button) 모든에서 가장 기본적인 대화형 컨트롤입니다 Xamarin.Forms . `Button`일반적으로는 명령을 나타내는 짧은 텍스트 문자열을 표시 하지만 비트맵 이미지 또는 텍스트와 이미지의 조합을 표시할 수도 있습니다. 사용자가 `Button` 손가락으로를 누르거나 마우스로 클릭 하 여 해당 명령을 시작 합니다.

아래에서 설명 하는 대부분의 항목은 [**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) 샘플의 페이지에 해당 합니다.

## <a name="handling-button-clicks"></a>단추 클릭 처리

`Button`[`Clicked`](xref:Xamarin.Forms.Button.Clicked)사용자가 `Button` 손가락 또는 마우스 포인터로를 누를 때 발생 하는 이벤트를 정의 합니다. 이 이벤트는 화면에서 손가락 또는 마우스 단추를 놓을 때 발생 `Button` 합니다. 는 `Button` [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) `true` 탭에 응답 하려면 속성이로 설정 되어 있어야 합니다.

[**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) 샘플의 **기본 단추 클릭** 페이지에서는 XAML로를 인스턴스화하고 해당 이벤트를 처리 하는 방법을 보여 줍니다 `Button` `Clicked` . **Basicbutton클릭 페이지 .xaml** 파일에는 `StackLayout` 와를 모두 포함 하는가 포함 되어 있습니다. `Label` `Button`

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

는 `Button` 허용 되는 모든 공간을 차지 하는 경향이 있습니다. 예를 들어, `HorizontalOptions` 의 속성을 `Button` 이외의 다른 항목으로 설정 하지 않으면 `Fill` `Button` 이 해당 부모의 전체 너비를 차지 합니다.

기본적으로는 `Button` 사각형 이지만 [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) 아래 섹션의 [**단추 모양**](#button-appearance)에서 설명한 대로 속성을 사용 하 여 모퉁이가 둥근 모퉁이를 제공할 수 있습니다.

[`Text`](xref:Xamarin.Forms.Button.Text)속성은에 표시 되는 텍스트를 지정 합니다 `Button` . [`Clicked`](xref:Xamarin.Forms.Button.Clicked)이벤트는 이라는 이벤트 처리기로 설정 됩니다 `OnButtonClicked` . 이 처리기는 코드 **BasicButtonClickPage.xaml.cs**파일에 있습니다.

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

`Button`을 탭하면 `OnButtonClicked` 메서드가 실행됩니다. `sender`인수는 `Button` 이 이벤트를 담당 하는 개체입니다. 이를 사용 하 여 개체에 액세스 `Button` 하거나 동일한 이벤트를 공유 하는 여러 개체를 구분할 수 있습니다 `Button` `Clicked` .

이 특정 `Clicked` 처리기는 `Label` 360도를 1000 밀리초로 회전 하는 애니메이션 함수를 호출 합니다. IOS 및 Android 장치에서 실행 되는 프로그램과 Windows 10 desktop의 UWP (유니버설 Windows 플랫폼) 응용 프로그램은 다음과 같습니다.

[![기본 단추 클릭](button-images/BasicButtonClick.png "기본 단추 클릭")](button-images/BasicButtonClick-Large.png#lightbox "기본 단추 클릭")

`OnButtonClicked` `async` `await` 는 이벤트 처리기 내에서 사용 되므로 메서드는 한정자를 포함 합니다. `Clicked`이벤트 처리기는 `async` 처리기의 본문이를 사용 하는 경우에만 한정자를 요구 합니다 `await` .

각 플랫폼은 `Button` 고유의 특정 방식으로를 렌더링 합니다. [**단추 모양**](#button-appearance) 섹션에서 색을 설정 하 고 사용자 지정 된 모양에 맞게 테두리를 표시 하는 방법을 볼 수 있습니다 `Button` . `Button`는 인터페이스를 구현 [`IFontElement`](xref:Xamarin.Forms.Internals.IFontElement) 하므로 [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) , [`FontSize`](xref:Xamarin.Forms.Button.FontSize) 및 속성을 포함 [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) 합니다.

## <a name="creating-a-button-in-code"></a>코드에서 단추 만들기

XAML에서를 인스턴스화하는 것이 일반적 `Button` 이지만 코드에서를 만들 수도 있습니다 `Button` . 응용 프로그램에서 루프를 사용 하 여 열거할 수 있는 데이터를 기반으로 여러 단추를 만들어야 하는 경우에 편리 `foreach` 합니다.

**코드 단추 클릭** 페이지에서는 **기본 단추 클릭** 페이지와 기능적으로 동일한 페이지를 만드는 방법을 보여 주지만 c #에서 전체적으로 다음을 수행 합니다.

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

모든 항목은 클래스의 생성자에서 수행 됩니다. 처리기는 `Clicked` 문 길이는 하나 뿐 이기 때문에 매우 간단 하 게 이벤트에 연결할 수 있습니다.

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

물론 이벤트 처리기를 별도의 메서드로 정의 ( `OnButtonClick` **기본 단추 클릭**의 메서드와 동일) 하 고 해당 메서드를 이벤트에 연결할 수도 있습니다.

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>단추 사용 안 함

때로는 특정 `Button` 클릭이 올바른 작업이 아닌 특정 상태에 있는 응용 프로그램이 있을 수 있습니다. 이러한 경우에는 `Button` 속성을로 설정 하 여을 사용 하지 않도록 설정 해야 합니다 `IsEnabled` `false` . 일반적인 예제는 `Entry` 파일 열기와 함께 파일 이름에 대 한 컨트롤입니다 .는에 `Button` `Button` 일부 텍스트가 입력 된 경우에만 사용할 수 있습니다 `Entry` .
`DataTrigger` [**데이터 트리거**](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) 문서에 표시 된 것 처럼이 작업에를 사용할 수 있습니다.

## <a name="using-the-command-interface"></a>명령 인터페이스 사용

응용 프로그램이 `Button` 이벤트를 처리 하지 않고 탭에 응답할 수 있습니다 `Clicked` . 는 `Button` _명령 또는 명령_ 인터페이스 라는 대체 알림 메커니즘을 _commanding_ 구현 합니다. 이는 두 가지 속성으로 구성 됩니다.

- [`Command`](xref:Xamarin.Forms.Button.Command)[`ICommand`](xref:System.Windows.Input.ICommand)네임 스페이스에 정의 된 인터페이스인 형식의입니다 [`System.Windows.Input`](xref:System.Windows.Input) .
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)형식의 속성 [`Object`](xref:System.Object) 입니다.

이 방법은 특히 MVVM (모델-뷰-ViewModel) 아키텍처를 구현할 때 데이터 바인딩과 연결 하는 데 특히 적합 합니다. 이러한 항목에 대해서는 데이터 바인딩에서 [Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md) [데이터 바인딩에서 MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)로, [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)에 설명 되어 있습니다.

MVVM 응용 프로그램에서 viewmodel은 `ICommand` 데이터 바인딩을 사용 하 여 XAML 요소에 연결 되는 형식의 속성을 정의 합니다 `Button` . Xamarin.Forms는 [`Command`](xref:Xamarin.Forms.Command) [`Command<T>`](xref:Xamarin.Forms.Command`1) 인터페이스를 구현 하 `ICommand` 고 형식의 속성을 정의 하는 데 viewmodel을 지 원하는 및 클래스도 정의 합니다 `ICommand` .

[**명령 인터페이스 문서에서는 명령 인터페이스**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) 에 대해 자세히 설명 하지만 [**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) 샘플의 **기본 단추 명령** 페이지에는 기본적인 방법이 나와 있습니다.

`CommandDemoViewModel`클래스는 라는 형식의 속성을 정의 하 `double` `Number` 고 및 라는 형식의 속성 두 개를 정의 하는 매우 간단한 viewmodel입니다 `ICommand` `MultiplyBy2Command` `DivideBy2Command` .

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

두 `ICommand` 속성은 형식의 두 개체를 사용 하 여 클래스의 생성자에서 초기화 됩니다 `Command` . 생성자에는 `Command` `execute` 속성을 두 배로 늘리거나 절반 하는 작은 함수 (생성자 인수 라고 함)가 포함 됩니다 `Number` .

**Basicbuttoncommand .xaml** 파일은를 인스턴스로 설정 `BindingContext` `CommandDemoViewModel` 합니다. `Label`요소와 두 요소에는 `Button` 의 세 가지 속성에 대 한 바인딩이 포함 되어 `CommandDemoViewModel` 있습니다.

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

두 요소를 `Button` 탭 하면 명령이 실행 되 고 숫자 값이 변경 됩니다.

[![기본 단추 명령](button-images/BasicButtonCommand.png "기본 단추 명령")](button-images/BasicButtonCommand-Large.png#lightbox)

처리기에 대 한이 방법의 장점은 `Clicked` 이 페이지의 기능과 관련 된 모든 논리가 코드 숨김이 아닌 viewmodel에 위치 하 여 비즈니스 논리와 사용자 인터페이스를 더 잘 분리 하는 것입니다.

또한 `Command` 개체는 요소의 활성화 및 비활성화를 제어할 수 있습니다 `Button` . 예를 들어 2<sup>10</sup> 에서 2<sup> &ndash; 10</sup>사이의 숫자 값 범위를 제한 하려고 한다고 가정 합니다. `canExecute` `true` 를 `Button` 사용 하도록 설정 해야 하는 경우를 반환 하는 생성자 (인수 라고 함)에 다른 함수를 추가할 수 있습니다. 생성자에 대 한 수정 사항은 `CommandDemoViewModel` 다음과 같습니다.

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

메서드가 `ChangeCanExecute` 메서드를 호출 하 고를 사용 하지 `Command` `Command` `canExecute` 않도록 설정할지 여부를 결정할 수 있도록의 메서드에 대 한 호출이 필요 합니다 `Button` . 이 코드를 변경 하면 숫자가 한도에 도달 하면 `Button` 가 비활성화 됩니다.

[![기본 단추 명령-수정 됨](button-images/BasicButtonCommandModified.png "기본 단추 명령-수정 됨")](button-images/BasicButtonCommandModified-Large.png#lightbox)

둘 이상의 `Button` 요소를 동일한 속성에 바인딩할 수 있습니다 `ICommand` . `Button`요소는의 속성을 사용 하 여 구분할 수 있습니다 [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) `Button` . 이 경우 제네릭 클래스를 사용 하는 것이 좋습니다 [`Command<T>`](xref:Xamarin.Forms.Command`1) . `CommandParameter`그런 다음 개체는 및 메서드에 인수로 전달 됩니다 `execute` `canExecute` . 이 기술은 [**명령 인터페이스**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) 문서의 [**기본 명령**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) 섹션에 자세히 나와 있습니다.

또한 [**Buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) 샘플은 해당 클래스에서이 기법을 사용 `MainPage` 합니다. **Mainpage** 파일에는 `Button` 샘플의 각 페이지에 대 한가 포함 되어 있습니다.

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

각에는 `Button` `Command` 라는 속성에 바인딩된 속성이 `NavigateCommand` 있으며,는 프로젝트의 `CommandParameter` [`Type`](xref:System.Type) 페이지 클래스 중 하나에 해당 하는 개체로 설정 됩니다.

해당 `NavigateCommand` 속성은 형식이 `ICommand` 며 코드 숨김이 파일에 정의 됩니다.

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

`NavigateCommand` `Command<Type>` `Type` 는 `CommandParameter` XAML 파일에 설정 된 개체의 형식이 기 때문에 생성자는 속성을 개체로 초기화 합니다. 이는 `execute` 메서드에 `Type` 이 개체에 해당 하는 형식의 인수가 있음을 의미 합니다 `CommandParameter` . 함수는 페이지를 인스턴스화한 다음 탐색 합니다.

생성자는를로 설정 하 여 마칩니다 `BindingContext` . 이는 XAML 파일의 속성을 속성에 바인딩하기 위해 필요 `NavigateCommand` 합니다.

## <a name="pressing-and-releasing-the-button"></a>단추 누르기 및 해제

이벤트 외 `Clicked` 에 `Button` 도 [`Pressed`](xref:Xamarin.Forms.Button.Pressed) 및 이벤트를 정의 [`Released`](xref:Xamarin.Forms.Button.Released) 합니다. `Pressed`이 이벤트는 손가락에서을 누를 때 발생 `Button` 하거나 포인터가 위에 배치 된 상태에서 마우스 단추를 누르면 발생 합니다 `Button` . `Released`이 이벤트는 손가락 또는 마우스 단추를 놓을 때 발생 합니다. 일반적으로 이벤트는 `Clicked` 이벤트와 동시에 발생 `Released` 하지만 손가락 또는 마우스 포인터가의 표면에서 벗어나면 `Button` `Clicked` 이벤트가 발생 하지 않을 수 있습니다.

`Pressed`및 `Released` 이벤트는 자주 사용 되지 않지만, **누르기 및 릴리스 단추** 페이지에 나와 있는 것 처럼 특수 한 용도로 사용할 수 있습니다. XAML 파일에는 및 `Label` `Button` 이벤트에 대 한 처리기가 연결 된 및가 포함 되어 있습니다 `Pressed` `Released` .

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

코드를 `Label` 실행 하면 이벤트가 발생할 때에 애니메이션이 적용 `Pressed` 되지만 이벤트가 발생 하는 경우에는 회전이 일시 중단 됩니다 `Released` .

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

그 결과는 `Label` 손가락이와 접촉 하는 동안에만 회전 하 `Button` 고 손가락이 해제 되 면 중지 됩니다.

[![눌렀다가 놓기 단추](button-images/PressAndReleaseButton.png "눌렀다가 놓기 단추")](button-images/PressAndReleaseButton-Large.png)

이러한 종류의 동작에는 게임을 위한 응용 프로그램이 있습니다 .에서 손가락을 사용 하면 화면에 있는 `Button` 개체가 특정 방향으로 이동 하 게 만들 수 있습니다.

## <a name="button-appearance"></a>단추 모양

는 `Button` 모양에 영향을 주는 몇 가지 속성을 상속 하거나 정의 합니다.

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)텍스트의 색입니다. `Button`
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)해당 텍스트에 대 한 배경의 색입니다.
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor)는을 둘러싼 영역의 색입니다.`Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)텍스트에 사용 되는 글꼴 패밀리입니다.
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)텍스트 크기입니다.
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)텍스트가 기울임꼴 또는 굵은 글꼴 인지 여부를 나타냅니다.
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth)테두리의 너비입니다.
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius)는의 모퉁이 반경입니다.`Button`
- `CharacterSpacing`텍스트 문자 사이의 간격입니다. `Button`

> [!NOTE]
> `Button`또한 클래스에 [`Margin`](xref:Xamarin.Forms.View.Margin) [`Padding`](xref:Xamarin.Forms.Button.Padding) 는의 레이아웃 동작을 제어 하는 및 속성이 있습니다 `Button` . 자세한 내용은 [여백 및 패딩](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)을 참조하세요.

이러한 속성 중 6 개 (및 제외)의 효과는 `FontFamily` `FontAttributes` **단추 모양** 페이지에서 보여 줍니다. 다른 속성인를 [`Image`](xref:Xamarin.Forms.Button.ImageSource) 사용 하는 방법에 대 한 자세한 내용은 [**단추를 사용 하 여 비트맵을 사용**](#using-bitmaps-with-buttons)합니다.

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

`Button`페이지 맨 위에는 `Color` 페이지 아래쪽의 요소에 바인딩된 3 개의 속성이 있습니다 `Picker` . 요소의 항목은 `Picker` `NamedColor` 프로젝트에 포함 된 클래스의 색입니다. 세 `Slider` 요소에 `FontSize` 는의, 및 속성에 대 한 양방향 바인딩이 포함 되어 `BorderWidth` `CornerRadius` `Button` 있습니다.

이 프로그램을 사용 하면 다음 모든 속성의 조합을 시험해 볼 수 있습니다.

[![단추 모양](button-images/ButtonAppearance.png "단추 모양")](button-images/ButtonAppearance-Large.png)

테두리를 보려면를 `Button` `BorderColor` 이외의 다른 값으로 설정 하 고를 `Default` 양수 값으로 설정 해야 합니다 `BorderWidth` .

IOS에서 많은 테두리 너비가의 내부에 방해가 텍스트 표시를 방해 함을 알 수 있습니다 `Button` . IOS에서 테두리를 사용 하도록 선택 하는 경우 `Button` `Text` 표시 유형을 유지 하기 위해 공백을 사용 하 여 속성을 시작 하 고 종료 하는 것이 좋습니다.

UWP에서 `CornerRadius` 높이의 절반을 초과 하는를 선택 하면 `Button` 예외가 발생 합니다.

## <a name="button-visual-states"></a>단추 시각적 상태

[`Button`](xref:Xamarin.Forms.Button)에는 `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) `Button` 사용자가 사용 하도록 설정 된 경우 사용자가 누를 때의 시각적 변경을 시작 하는 데 사용할 수 있는가 있습니다.

다음 XAML 예제에서는 상태에 대 한 시각적 상태를 정의 하는 방법을 보여 줍니다 `Pressed` .

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

는를 `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) [`Button`](xref:Xamarin.Forms.Button) 누르면 해당 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성이 기본값 1에서 0.8로 변경 되도록 지정 합니다. 는 `Normal` `VisualState` `Button` 가 정상 상태 이면 해당 `Scale` 속성은 1로 설정 되도록 지정 합니다. 따라서 전반적인 효과는를 `Button` 누를 때 약간 더 재조정, `Button` 가 릴리스되면이는 기본 크기로 재조정는 것입니다.

시각적 상태에 대 한 자세한 내용은 [ Xamarin.Forms 시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)를 참조 하세요.

## <a name="creating-a-toggle-button"></a>설정/해제 단추 만들기

On off 스위치와 같이 작동 하도록 서브 클래스를 사용할 수 있습니다. 단추를 한 번 탭 하 여 단추를 설정/해제 하 `Button` 고 다시 탭 하 여 설정/해제 합니다.

다음 `ToggleButton` 클래스는에서 파생 `Button` 되며 이라는 새 이벤트 `Toggled` 와 라는 부울 속성을 정의 합니다 `IsToggled` . 이러한 속성은 다음과 같은 두 가지 속성으로 정의 됩니다 Xamarin.Forms [`Switch`](xref:Xamarin.Forms.Switch) .

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

`ToggleButton`생성자는 `Clicked` 속성 값을 변경할 수 있도록 이벤트에 처리기를 연결 합니다 `IsToggled` . `OnIsToggledChanged`메서드는 이벤트를 발생 시킵니다 `Toggled` .

메서드의 마지막 줄에서는 `OnIsToggledChanged` `VisualStateManager.GoToState` 두 개의 텍스트 문자열 "ToggledOn" 및 "ToggledOff"를 사용 하 여 정적 메서드를 호출 합니다. 이 메서드와 응용 프로그램이 [** Xamarin.Forms 시각적 상태 관리자**](~/xamarin-forms/user-interface/visual-state-manager.md)문서에서 시각적 상태에 응답 하는 방법에 대해 알아볼 수 있습니다.

`ToggleButton`는에 대 한 호출을 수행 하기 때문에 `VisualStateManager.GoToState` 클래스 자체에는 해당 상태에 따라 단추의 모양을 변경 하는 추가 기능을 포함할 필요가 없습니다 `IsToggled` . 이는를 호스팅하는 XAML의 책임입니다 `ToggleButton` .

**설정/해제 단추 데모** 페이지에는 `ToggleButton` `Text` `BackgroundColor` `TextColor` 시각적 상태를 기준으로 단추의, 및를 설정 하는 시각적 상태 관리자 태그를 포함 하 여의 두 인스턴스가 포함 되어 있습니다.

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

`Toggled`이벤트 처리기는 코드 숨김이 파일에 있습니다. `FontAttributes`단추의 상태에 따라의 속성을 설정 해야 합니다 `Label` .

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

IOS, Android 및 UWP에서 실행 되는 프로그램은 다음과 같습니다.

[![설정/해제 단추 데모](button-images/ToggleButtonDemo.png "설정/해제 단추 데모")](button-images/ToggleButtonDemo-Large.png#lightbox)

## <a name="using-bitmaps-with-buttons"></a>단추에 비트맵 사용

클래스는에서 `Button` [`ImageSource`](xref:Xamarin.Forms.Button.Image) 비트맵 이미지를 단독으로 또는 텍스트와 함께 표시할 수 있는 속성을 정의 합니다 `Button` . 텍스트와 이미지를 정렬 하는 방법을 지정할 수도 있습니다.

`ImageSource`속성은 형식입니다 [`ImageSource`](xref:Xamarin.Forms.ImageSource) . 즉, 파일, 포함 된 리소스, URI 또는 스트림에서 비트맵을 로드할 수 있습니다.

> [!NOTE]
> 는 `Button` 애니메이션 gif를 로드할 수 있지만 gif의 첫 번째 프레임만 표시 합니다.

에서 지 원하는 각 플랫폼 Xamarin.Forms 을 사용 하면 응용 프로그램을 실행할 수 있는 다양 한 장치에 대 한 다양 한 픽셀 해상도에 대해 이미지를 여러 크기로 저장할 수 있습니다. 이러한 여러 비트맵은 운영 체제에서 장치의 비디오 디스플레이 해상도에 가장 일치 하는 항목을 선택할 수 있는 방식으로 명명 되거나 저장 됩니다.

에서 비트맵의 경우 `Button` 가장 좋은 크기는 원하는 크기에 따라 일반적으로 32과 64 장치 독립적 단위 사이입니다. 이 예제에 사용 된 이미지는 48 장치 독립적 단위 크기를 기반으로 합니다.

IOS 프로젝트의 **Resources** 폴더에는이 이미지의 세 가지 크기가 포함 되어 있습니다.

- **/Resources/MonkeyFace.png** 로 저장 된 48 픽셀 사각형 비트맵
- 로 저장 된 96 픽셀 사각형 비트맵**/Resource/MonkeyFace@2x.png**
- 로 저장 된 144 픽셀 사각형 비트맵**/Resource/MonkeyFace@3x.png**

세 개의 비트맵 모두 **BundleResource**의 **빌드 작업** 을 제공 했습니다.

Android 프로젝트의 경우 비트맵은 모두 동일한 이름을 갖지만 **Resources** 폴더의 다른 하위 폴더에 저장 됩니다.

- /Resources/drawable-hdpi/로 저장 된 72 픽셀 사각형 비트맵 **MonkeyFace.png**
- /Resources/drawable-xhdpi/로 저장 된 96 픽셀 사각형 비트맵 **MonkeyFace.png**
- /Resources/drawable-xxhdpi/로 저장 된 144 픽셀 사각형 비트맵 **MonkeyFace.png**
- /Resources/drawable-xxxhdpi/로 저장 된 192 픽셀 사각형 비트맵 **MonkeyFace.png**

여기에는 **Androidresource**의 **빌드 작업이** 제공 되었습니다.

UWP 프로젝트에서 비트맵은 프로젝트의 어디에 나 저장 될 수 있지만 일반적으로 사용자 지정 폴더 또는 **자산** 기존 폴더에 저장 됩니다. UWP 프로젝트는 다음 비트맵을 포함 합니다.

- /Assets/로 저장 된 48 픽셀 사각형 비트맵 **MonkeyFace.scale-100.png**
- /Assets/로 저장 된 96 픽셀 사각형 비트맵 **MonkeyFace.scale-200.png**
- /Assets/로 저장 된 192 픽셀 사각형 비트맵 **MonkeyFace.scale-400.png**

**콘텐츠의** **빌드 작업이** 모두 제공 되었습니다.

`Text` `ImageSource` `Button` 의 속성을 사용 하 여에서 및 속성을 정렬 하는 방법을 지정할 수 있습니다 [`ContentLayout`](xref:Xamarin.Forms.Button.ContentLayout) `Button` . 이 속성은의 [`ButtonContentLayout`](xref:Xamarin.Forms.Button.ButtonContentLayout) 포함 클래스인 형식입니다 `Button` . [생성자] (f: Xamarin.Forms 입니다. ButtonContentLayout .% 23ctor ( Xamarin.Forms . ButtonContentLayout. ImagePosition, system.string)에는 두 개의 인수가 있습니다.

- 열거형의 멤버 [`ImagePosition`](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) :,, `Left` `Top` `Right` 또는 `Bottom` 텍스트를 기준으로 비트맵이 표시 되는 방법을 나타내는입니다.
- `double`비트맵과 텍스트 사이의 간격 값입니다.

기본값은 `Left` 10 단위입니다. 라는의 두 읽기 전용 속성 `ButtonContentLayout` [`Position`](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) [`Spacing`](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) 은 해당 속성의 값을 제공 합니다.

코드에서을 만들고 `Button` 다음과 같이 속성을 설정할 수 있습니다 `ContentLayout` .

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

XAML에서 열거형 멤버 또는 간격만 지정 하거나 쉼표로 구분 된 모든 순서로 지정 해야 합니다.

```xaml
<Button Text="button text"
        ImageSource="image filename"
        ContentLayout="Right, 20" />
```

**이미지 단추 데모** 페이지는를 사용 하 여 `OnPlatform` iOS, Android 및 UWP 비트맵 파일의 다른 파일 이름을 지정 합니다. 각 플랫폼에 대해 동일한 파일 이름을 사용 하 고를 사용 하지 않으려면 `OnPlatform` UWP 비트맵을 프로젝트의 루트 디렉터리에 저장 해야 합니다.

`Button` **이미지 단추 데모** 페이지의 첫 번째는 속성을 설정 `Image` 하지만 속성은 설정 하지 않습니다 `Text` .

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

UWP 비트맵이 프로젝트의 루트 디렉터리에 저장 된 경우이 태그를 크게 간소화할 수 있습니다.

```xaml
<Button ImageSource="MonkeyFace.png" />
```

**Imagebuttondemo .xaml** 파일에서 많은 반복적인 태그를 방지 하기 위해 `Style` 속성을 설정 하기 위해 암시적인도 정의 됩니다 `ImageSource` . 이 `Style` 는 다른 5 개 요소에 자동으로 적용 됩니다 `Button` . 전체 XAML 파일은 다음과 같습니다.

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

최종 4 개 `Button` 요소는 속성을 사용 `ContentLayout` 하 여 텍스트와 비트맵의 위치와 간격을 지정 합니다.

[![이미지 단추 데모](button-images/ImageButtonDemo.png "이미지 단추 데모")](button-images/ImageButtonDemo-Large.png#lightbox)

이제 이벤트를 처리 하 고 모양을 변경할 수 있는 다양 한 방법을 살펴보았습니다 `Button` `Button` .

## <a name="related-links"></a>관련 링크

- [ButtonDemos 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)
- [단추 API](xref:Xamarin.Forms.Button)
