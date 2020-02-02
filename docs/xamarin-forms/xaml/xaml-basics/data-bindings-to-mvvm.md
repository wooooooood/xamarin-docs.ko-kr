---
title: 5부. 데이터 바인딩에서 MVVM까지
description: MVVM 패턴 3 소프트웨어 계층 간의 분리를 적용-; 뷰라고 XAML 사용자 인터페이스 기본 데이터 모델 호출 고 뷰와 모델 간의 중개자로 ViewModel 이라고 합니다.
ms.prod: xamarin
ms.custom: video
ms.assetid: 48B37D44-4FB1-41B2-9A5E-6D383B041F81
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: 1a6ab1393cbcd8224411aeea2af2aca27381bba3
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940365"
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>5부. 데이터 바인딩에서 MVVM까지

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_MVVM (모델 뷰-ViewModel) 아키텍처 패턴은 XAML을 염두에 두어야 합니다. 패턴은 세 가지 소프트웨어 계층 (XAML 사용자 인터페이스 (뷰 라고 함)을 구분 합니다. 모델 이라고 하는 기본 데이터 ViewModel 이라고 하는 뷰와 모델 간의 중개자입니다. 뷰와 ViewModel은 일반적으로 XAML 파일에 정의 된 데이터 바인딩을 통해 연결 됩니다. 뷰의 BindingContext는 일반적으로 ViewModel의 인스턴스입니다._

## <a name="a-simple-viewmodel"></a>간단한 ViewModel

Viewmodel에 대 한 기본적인를 먼저 살펴보겠습니다 없는 프로그램.
이전 새 XML 네임 스페이스 선언을 XAML 파일을 다른 어셈블리에서 참조 클래스를 허용 하도록 정의 하는 방법을 살펴보았습니다. 여기는 대 한 XML 네임 스페이스 선언을 정의 하는 프로그램을 `System` 네임 스페이스:

```csharp
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

프로그램 사용할 수 `x:Static` 정적에서 현재 날짜 및 시간을 얻을 수 `DateTime.Now` 속성 설정 `DateTime` 값을 `BindingContext` 에 `StackLayout`:

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext`은 특수 한 속성입니다. 요소에 대 한 `BindingContext`를 설정 하면 해당 요소의 모든 자식에 의해 상속 됩니다. 즉, `StackLayout`의 모든 자식은 해당 `BindingContext`와 동일한 것을 가지며, 여기에는 해당 개체의 속성에 대한 간단한 바인딩이 포함될 수 있습니다.

에 **One-Shot DateTime** 자식의 두 프로그램을 포함 하는 속성에 대 한 바인딩을 `DateTime` 값과 동일 하지만 다른 두 명의 자식이 포함 바인딩 경로 없는 것으로 보이는 바인딩. 즉 합니다 `DateTime` 자체 값이 사용 되는 `StringFormat`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
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

문제는 페이지를 처음 빌드할 때 날짜와 시간을 한 번 설정 하 고 변경 하지 않는다는 것입니다.

[![](data-bindings-to-mvvm-images/oneshotdatetime.png "View Displaying Date and Time")](data-bindings-to-mvvm-images/oneshotdatetime-large.png#lightbox "View Displaying Date and Time")

XAML 파일에는 항상 현재 시간을 표시 하는 클록이 표시 될 수 있지만 도움이 되는 코드가 필요 합니다. MVVM 측면에서 생각할 때 모델 및 ViewModel은 완전히 코드로 작성 된 클래스입니다. 뷰는 데이터 바인딩을 통해 ViewModel에 정의 된 속성을 참조 하는 XAML 파일 경우가 많습니다.

적절 한 모델은 ViewModel의 무시 되며 적절 한 ViewModel 뷰의 무시 합니다. 그러나 프로그래머는 ViewModel에서 노출 하는 데이터 형식을 특정 사용자 인터페이스와 관련 된 데이터 형식으로 맞춤화 하기도 합니다. 예를 들어, 모델을 8 비트 ASCII 문자열을 포함 하는 데이터베이스에 액세스 하는 경우 ViewModel 단독 사용자 인터페이스에서 유니코드 사용에 맞게 유니코드 문자열을 해당 문자열 간에 변환 해야 합니다.

(예: 여기에 표시 된) MVVM의 간단한 예제에서는 종종 모델이 없는 전혀 패턴만 뷰를 포함 하 고 데이터 바인딩을 사용 하 여 연결 된 ViewModel입니다.

다음은 `DateTime`라는 단일 속성만 있는 clock의 ViewModel 이며,이는 1 초 마다 `DateTime` 속성을 업데이트 합니다.

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

이 ViewModel에 따라 시계는이 처럼 간단할 수 있습니다.

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

통지 하는 방법을 `ClockViewModel` 로 설정 되어 합니다 `BindingContext` 의 `Label` 속성 요소 태그를 사용 하 여 합니다. 인스턴스화할 수 있습니다는 `ClockViewModel` 에서 `Resources` 컬렉션으로 설정 합니다 `BindingContext` 통해를 `StaticResource` 태그 확장 합니다. 또는 코드 숨김 파일을 ViewModel을 인스턴스화할 수 있습니다.

`Binding` 에서 태그 확장을 `Text` 의 속성을 `Label` 형식은 `DateTime` 속성. 표시는 다음과 같습니다.

[![](data-bindings-to-mvvm-images/clock.png "View Displaying Date and Time via ViewModel")](data-bindings-to-mvvm-images/clock-large.png#lightbox "View Displaying Date and Time via ViewModel")

개별 속성에 액세스할 수 이기도 합니다 `DateTime` 마침표를 사용 하 여 속성을 구분 하 여 ViewModel의 속성:

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>대화형 MVVM

MVVM는 기본 데이터 모델을 기반으로 하는 대화형 보기에 양방향 데이터 바인딩과 함께 사용 되는 경우가 많습니다.

클래스가 다음과 같습니다 `HslViewModel` 으로 변환 하는 `Color` 값 `Hue`, `Saturation`, 및 `Luminosity` 값 또는 그 반대로:

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

로 변경 합니다 `Hue`, `Saturation`, 및 `Luminosity` 속성 원인을 `Color` 속성을 변경 하려면 및 변경을 `Color` 다른 세 가지 속성을 변경 하면 합니다. 속성이 변경 되지 않은 경우 클래스가 `PropertyChanged` 이벤트를 호출 하지 않는다는 점을 제외 하면 무한 루프 처럼 보일 수 있습니다. 이 끝을 제어할 수 없는 그렇지 않은 경우 피드백 루프를 넣습니다.

다음 XAML 파일에 포함 되어는 `BoxView` 해당 `Color` 속성이 바인딩되는 `Color` 개와 ViewModel의 속성 `Slider` 및 3 `Label` 보기에 바인딩된를 `Hue`, `Saturation`, 및 `Luminosity` 속성:

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

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "MVVM using Two-Way Data Bindings")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "MVVM using Two-Way Data Bindings")

## <a name="commanding-with-viewmodels"></a>Viewmodel을 사용 하 여 명령 실행

대부분의 경우 MVVM 패턴은 데이터 항목의 조작으로 제한됩니다. 뷰의 사용자 인터페이스 개체는 ViewModel의 데이터 개체와 병렬입니다.

하지만 뷰에는 ViewModel에서 다양한 작업을 실행하는 단추가 포함되는 경우가 있습니다. 그러나 ViewModel은 특정 사용자 인터페이스 패러다임을 ViewModel에 묶어주기 때문에 `Clicked` 처리기를 포함시키면 안됩니다.

보다는 특정 사용자 인터페이스 개체와 독립적일 수 있지만 ViewModel에 내에서 호출 될 메서드를 허용 하는 Viewmodel을 허용 하는 *명령* 인터페이스 존재 합니다. 이 명령은 인터페이스는 Xamarin.Forms에서 다음과 같은 요소에서 지원 됩니다.

- `Button`
- `MenuItem`
- `ToolbarItem`
- `SearchBar`
- `TextCell` (및 따라서도 `ImageCell`)
- `ListView`
- `TapGestureRecognizer`

제외 된 `SearchBar` 및 `ListView` 요소, 두 속성을 정의 하는 이러한 요소:

- `System.Windows.Input.ICommand` 유형의 `Command`
- `Object` 유형의 `CommandParameter`

`SearchBar` 정의 `SearchCommand` 및 `SearchCommandParameter` 속성을 하는 동안 합니다 `ListView` 정의 `RefreshCommand` 형식의 속성 `ICommand`합니다.

`ICommand` 두 메서드와 하나의 이벤트 인터페이스를 정의 합니다.

- `void Execute(object arg)`
- `bool CanExecute(object arg)`
- `event EventHandler CanExecuteChanged`

ViewModel 형식의 속성을 정의할 수 `ICommand`입니다. 이러한 속성을 바인딩할 수 있습니다 합니다 `Command` 의 각 속성 `Button` 다른 요소 또는이 인터페이스를 구현 하는 사용자 지정 보기를 가장 합니다. 선택적으로 설정할 수 있습니다 합니다 `CommandParameter` 개별 식별 하기 위해 속성 `Button` 개체 (또는 다른 요소)이 ViewModel 속성에 바인딩되어 있는 합니다. 내부적으로 `Button` 호출을 `Execute` 사용자가 누를 때마다 메서드는 `Button`에 전달 합니다 `Execute` 메서드 해당 `CommandParameter`합니다.

`CanExecute` 메서드 및 `CanExecuteChanged` 이벤트는 `Button`을 탭하는 현재의 상황이 무효가 될 수 있는 경우에 사용합니다. 이 경우 `Button`은 스스로 비활성화되어야 합니다. `Button`은 `Command` 속성이 처음 설정될 때와 `CanExecuteChanged` 이벤트가 발생할 때마다 `CanExecute`를 호출합니다. `CanExecute`가 `false`를 반환하면 `Button`은 스스로 비활성화되고 `Execute`를 호출하지 않습니다.

ViewModels 명령 추가에 대 한 도움말을 보려면 Xamarin.ios는 `ICommand`을 구현 하는 두 개의 클래스를 정의 합니다 `Command<T>` `Command`. 여기서 `T`는 `Execute` 및 `CanExecute`에 대 한 인수의 형식입니다. 이 두 클래스는 여러 생성자를 정의 및 `ChangeCanExecute` ViewModel 하도록 호출할 수 있는 메서드를 `Command` 시키려면 개체는 `CanExecuteChanged` 이벤트입니다.

전화 번호를 입력 해야 하는 간단한 키패드를 ViewModel 다음과 같습니다. 에 `Execute` 및 `CanExecute` 메서드는 생성자에서 람다 함수 오른쪽으로 정의 됩니다.

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

이 ViewModel이 있다고 가정 합니다 `AddCharCommand` 속성이 바인딩되는 `Command` 으로 식별 되는 각각 여러 단추 (또는 명령 인터페이스가 있는 다른 모든 항목)의 속성은 `CommandParameter`합니다. 이러한 단추 추가 문자를 `InputString` 속성에 대 한 전화 번호로 다음 형식이 `DisplayText` 속성입니다.

형식의 두 번째 속성 이기도 `ICommand` 라는 `DeleteCharCommand`합니다. 백 간격 단추에 바인딩되어이 있지만 삭제 하려면 문자가 없는 경우 단추가 비활성화 해야 합니다.

다음 키패드는 지 시각적으로 정교하지 없습니다. 대신, 태그 명령 인터페이스의 사용 보다 명확 하 게 보여 주기 위해 최소한으로 줄었습니다.

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

[![](data-bindings-to-mvvm-images/keypad.png "Calculator using MVVM and Commands")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "Calculator using MVVM and Commands")

### <a name="invoking-asynchronous-methods"></a>비동기 메서드를 호출합니다.

명령을 비동기 메서드를 호출할 수도 있습니다. 사용 하 여 이렇게 합니다 `async` 하 고 `await` 키워드를 지정 하는 경우를 `Execute` 메서드:

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

되었음을 표시 합니다 `DownloadAsync` 메서드는를 `Task` 대기할 수 해야 및:

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

## <a name="implementing-a-navigation-menu"></a>탐색 메뉴를 구현합니다.

합니다 [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples) 이 문서 시리즈의 모든 소스 코드를 포함 하는 프로그램은 홈 페이지에 대 한 ViewModel을 사용 합니다. 이 ViewModel은 라는 세 가지 속성을 사용 하 여 간단한 클래스의 정의 `Type`, `Title`, 및 `Description` 유형의 각 샘플 페이지, 제목 및 간단한 설명을 포함 하는 합니다. ViewModel 라는 정적 속성을 정의 하는 또한 `All` 프로그램의 모든 페이지의 컬렉션입니다.

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

XAML 파일에 대 한 `MainPage` 정의 `ListBox` 입니다 `ItemsSource` 는 속성이 설정 되어 `All` 속성을 포함 하는 `TextCell` 표시는 `Title` 및 `Description` 각 페이지의 속성:

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

페이지를 스크롤할 수 있는 목록에 표시 됩니다.

[![](data-bindings-to-mvvm-images/mainpage.png "Scrollable list of pages")](data-bindings-to-mvvm-images/mainpage-large.png#lightbox "Scrollable list of pages")

코드 숨김 파일에서 처리기는 항목을 선택할 때 트리거됩니다. 처리기 집합을 `SelectedItem` 의 속성을 `ListBox` 다시 `null` 및 그런 다음 선택한 페이지를 인스턴스화하고 여기로 이동:

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

**Xamarin.Forms 및 Prism을 사용 하 여 간단 하 게 Xamarin Evolve 2016: MVVM**

## <a name="summary"></a>요약

XAML은 Xamarin.Forms 응용 프로그램에서 데이터 바인딩된 경우에 특히 사용자 인터페이스를 정의 하기 위한 강력한 도구 및 MVVM 사용 됩니다. 결과는 코드에서 모든 배경 지원 사용 하 여 사용자 인터페이스의 깔끔하고 세련 된 잠재적으로 화할 표현입니다.

## <a name="related-links"></a>관련 링크

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [1 부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [2 부. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [3 부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [4 부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)

## <a name="related-videos"></a>관련 비디오

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-MVVM-with-XAML-6-of-11/player]

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-Navigation-with-XAML-7-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
