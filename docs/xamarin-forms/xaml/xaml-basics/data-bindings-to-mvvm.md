---
title: 5장. 데이터 바인딩부터 MVVM까지
description: MVVM 패턴은 세 가지 소프트웨어 계층(뷰라고 하는 XAML 사용자 인터페이스, 모델이라고 하는 기본 데이터, ViewModel이라고 하는 뷰와 모델 간의 중개자)으로 구분합니다.
ms.prod: xamarin
ms.assetid: 48B37D44-4FB1-41B2-9A5E-6D383B041F81
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: 116225165b8ee27b896d3de8598f8fbf39400f52
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767525"
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>5장. 데이터 바인딩부터 MVVM까지

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_MVVM(Model-View-ViewModel) 아키텍처 패턴은 XAML을 염두에 두고 고안되었습니다. 해당 패턴은 세 가지 소프트웨어 계층(뷰라고 하는 XAML 사용자 인터페이스, 모델이라고 하는 기본 데이터, ViewModel이라고 하는 뷰와 모델 간의 중개자)으로 구분합니다. 종종 뷰와 ViewModel은 XAML 파일에 정의된 데이터 바인딩을 통해 연결됩니다. 뷰의 BindingContext는 일반적으로 ViewModel의 인스턴스입니다._

## <a name="a-simple-viewmodel"></a>간단한 ViewModel

ViewModel에 대한 소개로 ViewModel이 없는 프로그램을 먼저 살펴 보겠습니다.
이전에 XAML 파일이 다른 어셈블리의 클래스를 참조할 수 있도록 새 XML 네임 스페이스 선언을 정의하는 방법을 살펴 보았습니다. 다음은 `System` 네임 스페이스에 대한 XML 네임 스페이스 선언을 정의하는 프로그램입니다.

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

해당 프로그램은 다음과 같이 `x:Static`을 사용하여 정적 `DateTime.Now` 속성에서 현재 날짜 및 시간을 얻고 `DateTime` 값을 `StackLayout`의 `BindingContext`로 설정합니다.

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext`는 매우 특수 한 속성입니다. 요소 `BindingContext` 에 대해를 설정 하는 경우 해당 요소의 모든 자식에 의해 상속 됩니다. 즉, `StackLayout`의 모든 자식은 해당 `BindingContext`와 동일한 것을 가지며, 여기에는 해당 개체의 속성에 대한 간단한 바인딩이 포함될 수 있습니다.

**One-Shot DateTime** 프로그램에서 두 개의 자식은 `DateTime` 값의 속성에 바인딩을 포함하고 있지만, 다른 두 자식은 바인딩 경로가 없는 것으로 보입니다. 이는 다음과 같이 `DateTime` 값 자체가 `StringFormat`에 사용됨을 의미합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.OneShotDateTimePage"
             Title="One-Shot DateTime Page">

    <StackLayout BindingContext="{x:Static sys:DateTime.Now}"
                 HorizontalOptions="Center"
                 VerticalOptions="Center">

        <Label Text="{Binding Year, StringFormat='The year is {0}'}" />
        <Label Text="{Binding StringFormat='The month is {0:MMMM}'}" />
        <Label Text="{Binding Day, StringFormat='The day is {0}'}" />
        <Label Text="{Binding StringFormat='The time is {0:T}'}" />

    </StackLayout>
</ContentPage>
```

물론, 큰 문제는 페이지가 처음 만들어지면 날짜 및 시간이 한번 설정되고 결코 변경되지 않는다는 것입니다.

[![](data-bindings-to-mvvm-images/oneshotdatetime.png "날짜 및 시간을 표시하는 뷰")](data-bindings-to-mvvm-images/oneshotdatetime-large.png#lightbox "날짜 및 시간을 표시하는 뷰")

XAML 파일은 항상 현재 시간을 나타내는 시계를 표시할 수 있지만 이를 도와주는 코드가 필요합니다. MVVM의 관점에서 보면, 모델 및 ViewModel은 완전한 코드로 작성된 클래스입니다. 뷰는 종종 데이터 바인딩을 통해 뷰모델에 정의된 속성을 참조하는 XAML 파일입니다.

적절한 모델은 ViewModel을 모르고 있으며, 적절한 ViewModel은 뷰를 모르고 있습니다. 그러나 프로그래머는 매우 빈번하게 ViewModel에 노출된 데이터 유형을 특정 사용자 인터페이스와 연관되는 데이터 유형에 맞게 조정합니다. 예를 들어, 모델이 8 비트 문자인 ASCII 문자열을 포함하는 데이터베이스에 접근하는 경우 ViewModel은 사용자 인터페이스에서 유니코드를 독점적으로 사용하기 위해 해당 문자열 사이를 유니코드 문자열로 변환해야 합니다.

MVVM의 간단한 예제(여기에 표시된 것처럼)는 종종 모델이 전혀 없는 경우가 많으며, 패턴에는 데이터 바인딩과 연결된 뷰 및 ViewModel만 포함합니다.

다음은 `DateTime`이라는 단 하나의 속성을 가진 시계에 대한 ViewModel이지만, 매초 `DateTime` 속성을 업데이트합니다.

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    class ClockViewModel : INotifyPropertyChanged
    {
        DateTime dateTime;

        public event PropertyChangedEventHandler PropertyChanged;

        public ClockViewModel()
        {
            this.DateTime = DateTime.Now;

            Device.StartTimer(TimeSpan.FromSeconds(1), () =>
                {
                    this.DateTime = DateTime.Now;
                    return true;
                });
        }

        public DateTime DateTime
        {
            set
            {
                if (dateTime != value)
                {
                    dateTime = value;

                    if (PropertyChanged != null)
                    {
                        PropertyChanged(this, new PropertyChangedEventArgs("DateTime"));
                    }
                }
            }
            get
            {
                return dateTime;
            }
        }
    }
}
```

ViewModel은 일반적으로 `INotifyPropertyChanged` 인터페이스를 구현합니다. 즉, 속성 중 하나가 변경될 때마다 클래스가 `PropertyChanged` 이벤트를 발생시킵니다. Xamarin.Forms의 데이터 바인딩 메커니즘은 해당 `PropertyChanged` 이벤트에 처리기를 붙여 속성이 변경될 때 이를 알리고 새 값으로 대상을 업데이트합니다.

해당 ViewModel에 기반한 시계는 다음과 같이 간단해질 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ClockPage"
             Title="Clock Page">

    <Label Text="{Binding DateTime, StringFormat='{0:T}'}"
           FontSize="Large"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <Label.BindingContext>
            <local:ClockViewModel />
        </Label.BindingContext>
    </Label>
</ContentPage>
```

`ClockViewModel`이 속성 요소 태그를 사용하여 `Label`의 `BindingContext`에 어떻게 설정되는지 주목하십시오. 또는 `Resources` 컬렉션에서 `ClockViewModel`을 인스턴스화하고 `StaticResource` 태그 확장을 통해 `BindingContext`로 설정할 수 있습니다. 또는 코드 숨김 파일은 ViewModel을 인스턴스화할 수 있습니다.

`Label`의 `Text` 속성에 있는 `Binding` 태그 확장은 `DateTime` 속성을 형식화합니다. 다음과 같이 표시됩니다.

[![](data-bindings-to-mvvm-images/clock.png "ViewModel을 통해 시간과 날짜를 표시하는 뷰")](data-bindings-to-mvvm-images/clock-large.png#lightbox "ViewModel을 통해 시간과 날짜를 표시하는 뷰")

다음과 같이 마침표로 속성을 구분하여 ViewModel의 `DateTime` 속성의 개별 속성에 접근할 수도 있습니다.

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>대화형 MVVM

MVVM은 기본 데이터 모델을 기반으로하는 대화형 뷰의 양방향 데이터 바인딩을 함께 사용하는 경우가 꽤 자주 있습니다.

다음은 `Color` 값을 `Hue`, `Saturation` 및 `Luminosity` 값으로 변환하는 `HslViewModel` 클래스입니다.

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    public class HslViewModel : INotifyPropertyChanged
    {
        double hue, saturation, luminosity;
        Color color;

        public event PropertyChangedEventHandler PropertyChanged;

        public double Hue
        {
            set
            {
                if (hue != value)
                {
                    hue = value;
                    OnPropertyChanged("Hue");
                    SetNewColor();
                }
            }
            get
            {
                return hue;
            }
        }

        public double Saturation
        {
            set
            {
                if (saturation != value)
                {
                    saturation = value;
                    OnPropertyChanged("Saturation");
                    SetNewColor();
                }
            }
            get
            {
                return saturation;
            }
        }

        public double Luminosity
        {
            set
            {
                if (luminosity != value)
                {
                    luminosity = value;
                    OnPropertyChanged("Luminosity");
                    SetNewColor();
                }
            }
            get
            {
                return luminosity;
            }
        }

        public Color Color
        {
            set
            {
                if (color != value)
                {
                    color = value;
                    OnPropertyChanged("Color");

                    Hue = value.Hue;
                    Saturation = value.Saturation;
                    Luminosity = value.Luminosity;
                }
            }
            get
            {
                return color;
            }
        }

        void SetNewColor()
        {
            Color = Color.FromHsla(Hue, Saturation, Luminosity);
        }

        protected virtual void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

`Hue`, `Saturation` 및 `Luminosity` 속성을 변경하면 `Color` 속성이 바뀌고, `Color`가 바뀌면 다른 세 가지 속성이 바뀝니다. 이는 속성이 실제로 변경되지 않는 한 클래스가 `PropertyChanged` 이벤트를 호출하지 않는다는 점을 제외하면 무한 루프처럼 보일 수 있습니다. 이로 인해 제어 불가능한 경우 없이 피드백 루프가 끝납니다.

다음 XAML 파일은 `Color` 속성이 ViewModel의 `Color` 속성에 바인딩된 `BoxView`와 `Hue`, `Saturation` 및 `Luminosity` 속성에 바인딩된 세 개의 `Slider`와 세 개의 `Label` 뷰를 포함합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.HslColorScrollPage"
             Title="HSL Color Scroll Page">
    <ContentPage.BindingContext>
        <local:HslViewModel Color="Aqua" />
    </ContentPage.BindingContext>

    <StackLayout Padding="10, 0">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Hue, Mode=TwoWay}" />

        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Saturation, Mode=TwoWay}" />

        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Luminosity, Mode=TwoWay}" />
    </StackLayout>
</ContentPage>
```

각 `Label`의 바인딩은 기본값이 `OneWay`이므로. 값만 표시하면 됩니다. 하지만 각 `Slider`의 바인딩은 `TwoWay`이므로. `Slider`는 ViewModel에서 초기화할 수 있습니다. ViewModel이 인스턴스화될 때, `Color` 속성은 `Aqua`로 설정됩니다. 하지만 `Slider`를 변경하면 뷰모델의 속성 값을 새로 설정해야 새로운 색상이 계산됩니다.

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "양방향 데이터 바인딩을 사용하는 MVVM")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "양방향 데이터 바인딩을 사용하는 MVVM")

## <a name="commanding-with-viewmodels"></a>ViewModel을 사용하여 명령 실행

대부분의 경우 MVVM 패턴은 데이터 항목을 조작 하는 것으로 제한 됩니다. 의 사용자 인터페이스 개체는 ViewModel의 병렬 데이터 개체를 표시 합니다.

하지만 뷰에는 ViewModel에서 다양한 작업을 실행하는 단추가 포함되는 경우가 있습니다. 그러나 ViewModel은 특정 사용자 인터페이스 패러다임을 ViewModel에 묶어주기 때문에 `Clicked` 처리기를 포함시키면 안됩니다.

ViewModel이 특정 사용자 인터페이스 개체와 좀 더 독립되도록 해주지만, ViewModel 내에서 메서드를 호출할 수 있도록 하려면 *command* 인터페이스가 있어야 합니다. 이 명령의 인터페이스는 Xamarin.Forms의 다음과 같은 요소에서 지원됩니다.

- `Button`
- `MenuItem`
- `ToolbarItem`
- `SearchBar`
- `TextCell`(그리고 `ImageCell`도 따름)
- `ListView`
- `TapGestureRecognizer`

`SearchBar`와 `ListView` 요소를 제외하고, 해당 요소들은 다음과 같이 두 개의 속성을 정의합니다.

- `System.Windows.Input.ICommand` 유형의 `Command`
- `Object` 유형의 `CommandParameter`

`SearchBar`는 `SearchCommand` 및  `SearchCommandParameter` 속성을 정의하는 반면 `ListView`는 `ICommand` 유형의 `RefreshCommand`를 정의합니다.

`ICommand` 인터페이스는 다음과 같이 두 개의 메서드와 하나의 이벤트를 정의합니다.

- `void Execute(object arg)`
- `bool CanExecute(object arg)`
- `event EventHandler CanExecuteChanged`

ViewModel은 `ICommand` 유형의 속성을 정의할 수 있습니다. 그런 다음 해당 속성들을 `Button`이나 다른 요소의 `Command` 속성 또는 해당 인터페이스를 구현하는 사용자 지정 뷰에 바인딩할 수 있습니다. 선택적으로 `CommandParameter` 속성을 설정하여 해당 ViewModel 속성에 바인딩된 개별 `Button` 개체(또는 다른 요소)를 식별할 수 있습니다. 내부적으로 `Button`은 사용자가 `Button`을 누를 때마다 `Execute` 메서드를 호출하고 `Execute` 메서드에 자신의 `CommandParameter`를 전달합니다.

`CanExecute` 메서드 및 `CanExecuteChanged` 이벤트는 `Button`을 탭하는 현재의 상황이 무효가 될 수 있는 경우에 사용합니다. 이 경우 `Button`은 스스로 비활성화되어야 합니다. `Button`은 `Command` 속성이 처음 설정될 때와 `CanExecuteChanged` 이벤트가 발생할 때마다 `CanExecute`를 호출합니다. `CanExecute`가 `false`를 반환하면 `Button`은 스스로 비활성화되고 `Execute`를 호출하지 않습니다.

ViewModel에 명령 추가를 돕기 위하여 Xamarin.Forms는 `ICommand`를 구현하는 두 클래스를 정의하는 데, 이는 `Command`와 `Execute` 및 `CanExecute`에 대한 인수의 유형이 `T`인 `Command<T>`입니다. 해당 두 클래스는 여러 생성자와 ViewModel이 `CanExecuteChanged` 이벤트를 발생시키도록 `Command` 개체를 강제로 호출하는 `ChangeCanExecute` 메서드의 연결을 정의합니다.

다음은 전화 번호를 입력하기 위한 간단한 키패드 용 ViewModel입니다. 다음과 같이 `Execute` 및 `CanExecute` 메서드는 생성자에서 람다 함수로 오른쪽에 정의됩니다.

```csharp
using System;
using System.ComponentModel;
using System.Windows.Input;
using Xamarin.Forms;

namespace XamlSamples
{
    class KeypadViewModel : INotifyPropertyChanged
    {
        string inputString = "";
        string displayText = "";
        char[] specialChars = { '*', '#' };

        public event PropertyChangedEventHandler PropertyChanged;

        // Constructor
        public KeypadViewModel()
        {
            AddCharCommand = new Command<string>((key) =>
                {
                    // Add the key to the input string.
                    InputString += key;
                });

            DeleteCharCommand = new Command(() =>
                {
                    // Strip a character from the input string.
                    InputString = InputString.Substring(0, InputString.Length - 1);
                },
                () =>
                {
                    // Return true if there's something to delete.
                    return InputString.Length > 0;
                });
        }

        // Public properties
        public string InputString
        {
            protected set
            {
                if (inputString != value)
                {
                    inputString = value;
                    OnPropertyChanged("InputString");
                    DisplayText = FormatText(inputString);

                    // Perhaps the delete button must be enabled/disabled.
                    ((Command)DeleteCharCommand).ChangeCanExecute();
                }
            }

            get { return inputString; }
        }

        public string DisplayText
        {
            protected set
            {
                if (displayText != value)
                {
                    displayText = value;
                    OnPropertyChanged("DisplayText");
                }
            }
            get { return displayText; }
        }

        // ICommand implementations
        public ICommand AddCharCommand { protected set; get; }

        public ICommand DeleteCharCommand { protected set; get; }

        string FormatText(string str)
        {
            bool hasNonNumbers = str.IndexOfAny(specialChars) != -1;
            string formatted = str;

            if (hasNonNumbers || str.Length < 4 || str.Length > 10)
            {
            }
            else if (str.Length < 8)
            {
                formatted = String.Format("{0}-{1}",
                                          str.Substring(0, 3),
                                          str.Substring(3));
            }
            else
            {
                formatted = String.Format("({0}) {1}-{2}",
                                          str.Substring(0, 3),
                                          str.Substring(3, 3),
                                          str.Substring(6));
            }
            return formatted;
        }

        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

해당 ViewModel은 `AddCharCommand` 속성이 `CommandParameter`에 의해 구별되는 여러 단추(또는 명령 인터페이스가 있는 다른 항목)의 `Command` 속성에 바인딩되어 있다고 가정합니다. 해당 단추는 `InputString` 속성에 `DisplayText` 속성의 전화번호 형식 문자를 추가합니다.

또한 `DeleteCharCommand`라는 `ICommand` 유형의 두 번째 속성이 있습니다. 해당 단추는 백 스페이스 단추에 바인딩되어 있지만 삭제하려는 문자가 없는 경우 단추를 비활성화해야 합니다.

다음 키패드는 시각적으로 정교하지 않습니다. 대신, 명령 인터페이스 사용을 보다 명확하게 보여주기 위해 태그를 최소한으로 줄였습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.KeypadPage"
             Title="Keypad Page">

    <Grid HorizontalOptions="Center"
          VerticalOptions="Center">
        <Grid.BindingContext>
            <local:KeypadViewModel />
        </Grid.BindingContext>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
        </Grid.ColumnDefinitions>

        <!-- Internal Grid for top row of items -->
        <Grid Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="Auto" />
            </Grid.ColumnDefinitions>

            <Frame Grid.Column="0"
                   OutlineColor="Accent">
                <Label Text="{Binding DisplayText}" />
            </Frame>

            <Button Text="&#x21E6;"
                    Command="{Binding DeleteCharCommand}"
                    Grid.Column="1"
                    BorderWidth="0" />
        </Grid>

        <Button Text="1"
                Command="{Binding AddCharCommand}"
                CommandParameter="1"
                Grid.Row="1" Grid.Column="0" />

        <Button Text="2"
                Command="{Binding AddCharCommand}"
                CommandParameter="2"
                Grid.Row="1" Grid.Column="1" />

        <Button Text="3"
                Command="{Binding AddCharCommand}"
                CommandParameter="3"
                Grid.Row="1" Grid.Column="2" />

        <Button Text="4"
                Command="{Binding AddCharCommand}"
                CommandParameter="4"
                Grid.Row="2" Grid.Column="0" />

        <Button Text="5"
                Command="{Binding AddCharCommand}"
                CommandParameter="5"
                Grid.Row="2" Grid.Column="1" />

        <Button Text="6"
                Command="{Binding AddCharCommand}"
                CommandParameter="6"
                Grid.Row="2" Grid.Column="2" />

        <Button Text="7"
                Command="{Binding AddCharCommand}"
                CommandParameter="7"
                Grid.Row="3" Grid.Column="0" />

        <Button Text="8"
                Command="{Binding AddCharCommand}"
                CommandParameter="8"
                Grid.Row="3" Grid.Column="1" />

        <Button Text="9"
                Command="{Binding AddCharCommand}"
                CommandParameter="9"
                Grid.Row="3" Grid.Column="2" />

        <Button Text="*"
                Command="{Binding AddCharCommand}"
                CommandParameter="*"
                Grid.Row="4" Grid.Column="0" />

        <Button Text="0"
                Command="{Binding AddCharCommand}"
                CommandParameter="0"
                Grid.Row="4" Grid.Column="1" />

        <Button Text="#"
                Command="{Binding AddCharCommand}"
                CommandParameter="#"
                Grid.Row="4" Grid.Column="2" />
    </Grid>
</ContentPage>
```

해당 태그에 나타나는 첫 번째 `Button`의 `Command` 속성은 `DeleteCharCommand`에 바인딩되어 있습니다. 나머지는 `AddCharCommand`와 `Button` 표면에 표시되는 문자와 동일한 `CommandParameter`를 가지고 있습니다. 동작 중인 프로그램은 다음과 같습니다.

[![](data-bindings-to-mvvm-images/keypad.png "MVVM 및 명령을 사용하는 계산기")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "MVVM 및 명령을 사용하는 계산기")

### <a name="invoking-asynchronous-methods"></a>비동기 메서드 호출

명령은 비동기 메서드 또한 호출할 수 있습니다. 이를 위해 다음과 같이 `Execute` 메서드를 지정할 때 `async`와 `await` 키워드를 사용하면 됩니다.

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

이는 다음과 같이 `DownloadAsync` 메서드가 `Task`이고 기다려야(await) 한다는 것을 의미합니다.

```csharp
async Task DownloadAsync ()
{
    await Task.Run (() => Download ());
}

void Download ()
{
    ...
}
```

## <a name="implementing-a-navigation-menu"></a>탐색 메뉴 구현

해당 시리즈 글의 모든 소스 코드가 들어있는 [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples) 프로그램은 해당 홈 페이지에 대한 ViewModel을 사용합니다. 해당 ViewModel은 샘플 페이지, 제목 및 간단한 설명의 유형을 포함하는 `Type`, `Title` 및 `Description`이라는 세 가지 속성을 가진 짧은 클래스 정의입니다. 또한 ViewModel은 프로그램의 모든 페이지 컬렉션인 `All`이라는 정적 속성을 정의합니다.

```csharp
public class PageDataViewModel
{
    public PageDataViewModel(Type type, string title, string description)
    {
        Type = type;
        Title = title;
        Description = description;
    }

    public Type Type { private set; get; }

    public string Title { private set; get; }

    public string Description { private set; get; }

    static PageDataViewModel()
    {
        All = new List<PageDataViewModel>
        {
            // Part 1. Getting Started with XAML
            new PageDataViewModel(typeof(HelloXamlPage), "Hello, XAML",
                                  "Display a Label with many properties set"),

            new PageDataViewModel(typeof(XamlPlusCodePage), "XAML + Code",
                                  "Interact with a Slider and Button"),

            // Part 2. Essential XAML Syntax
            new PageDataViewModel(typeof(GridDemoPage), "Grid Demo",
                                  "Explore XAML syntax with the Grid"),

            new PageDataViewModel(typeof(AbsoluteDemoPage), "Absolute Demo",
                                  "Explore XAML syntax with AbsoluteLayout"),

            // Part 3. XAML Markup Extensions
            new PageDataViewModel(typeof(SharedResourcesPage), "Shared Resources",
                                  "Using resource dictionaries to share resources"),

            new PageDataViewModel(typeof(StaticConstantsPage), "Static Constants",
                                  "Using the x:Static markup extensions"),

            new PageDataViewModel(typeof(RelativeLayoutPage), "Relative Layout",
                                  "Explore XAML markup extensions"),

            // Part 4. Data Binding Basics
            new PageDataViewModel(typeof(SliderBindingsPage), "Slider Bindings",
                                  "Bind properties of two views on the page"),

            new PageDataViewModel(typeof(SliderTransformsPage), "Slider Transforms",
                                  "Use Sliders with reverse bindings"),

            new PageDataViewModel(typeof(ListViewDemoPage), "ListView Demo",
                                  "Use a ListView with data bindings"),

            // Part 5. From Data Bindings to MVVM
            new PageDataViewModel(typeof(OneShotDateTimePage), "One-Shot DateTime",
                                  "Obtain the current DateTime and display it"),

            new PageDataViewModel(typeof(ClockPage), "Clock",
                                  "Dynamically display the current time"),

            new PageDataViewModel(typeof(HslColorScrollPage), "HSL Color Scroll",
                                  "Use a view model to select HSL colors"),

            new PageDataViewModel(typeof(KeypadPage), "Keypad",
                                  "Use a view model for numeric keypad logic")
        };
    }

    public static IList<PageDataViewModel> All { private set; get; }
}
```

`MainPage`에 대한 XAML 파일은 다음과 같이 `ItemsSource` 속성이 `All` 속성으로 설정되고 각 페이지의 `Title` 및 `Description` 속성을 표시하는 `TextCell`을 포함하는 `ListBox`입니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage"
             Padding="5, 0"
             Title="XAML Samples">

    <ListView ItemsSource="{x:Static local:PageDataViewModel.All}"
              ItemSelected="OnListViewItemSelected">
        <ListView.ItemTemplate>
            <DataTemplate>
                <TextCell Text="{Binding Title}"
                          Detail="{Binding Description}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

다음과 같이 페이지는 스크롤 가능한 목록으로 표시됩니다.

[![](data-bindings-to-mvvm-images/mainpage.png "페이지의 스크롤 가능한 목록")](data-bindings-to-mvvm-images/mainpage-large.png#lightbox "페이지의 스크롤 가능한 목록")

코드 숨김 파일에서 처리기(handler)는 사용자가 항목을 선택할 때 실행됩니다. 처리기는 다음과 같이 `ListBox`의 `SelectedItem` 속성을 `null`로 설정한 다음 선택한 페이지를 인스턴스화하고 해당 페이지로 이동합니다.

```csharp
private async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
{
    (sender as ListView).SelectedItem = null;

    if (args.SelectedItem != null)
    {
        PageDataViewModel pageData = args.SelectedItem as PageDataViewModel;
        Page page = (Page)Activator.CreateInstance(pageData.Type);
        await Navigation.PushAsync(page);
    }
}
```

## <a name="video"></a>비디오

> [!VIDEO https://youtube.com/embed/DYRLcqG2BAY]

**Xamarin 진화 2016: MVVM 및 프리즘를 사용 하 여 간단한 구성**

## <a name="summary"></a>요약

XAML은 특히 데이터 바인딩 및 MVVM을 사용할 때 Xamarin.Forms 응용 프로그램에서 사용자 인터페이스를 정의하는 강력한 도구입니다. 결과는 코드에서 모든 배경을 지원하는 사용자 인터페이스를 깔끔하고 세련되며, 잠재적으로 도구화하여 표현할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Xaml 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [1부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [2부. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [3부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [4부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
