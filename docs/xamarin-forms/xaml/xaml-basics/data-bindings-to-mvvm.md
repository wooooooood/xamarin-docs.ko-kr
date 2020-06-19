---
title: 5부. 데이터 바인딩에서 MVVM까지
description: MVVM 패턴은 세 가지 소프트웨어 계층 (XAML 사용자 인터페이스 (뷰 라고 함)을 구분 합니다. 모델 이라고 하는 기본 데이터 ViewModel 이라고 하는 뷰와 모델 간의 중개자입니다.
ms.prod: xamarin
ms.custom: video
ms.assetid: 48B37D44-4FB1-41B2-9A5E-6D383B041F81
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 047cf963394325e8f88759ffe9da7dcf2ca3ad12
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127532"
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>5부. 데이터 바인딩에서 MVVM까지

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_MVVM (모델 뷰-ViewModel) 아키텍처 패턴은 XAML을 염두에 두어야 합니다. 패턴은 세 가지 소프트웨어 계층 (XAML 사용자 인터페이스 (뷰 라고 함)을 구분 합니다. 모델 이라고 하는 기본 데이터 ViewModel 이라고 하는 뷰와 모델 간의 중개자입니다. 뷰와 ViewModel은 일반적으로 XAML 파일에 정의 된 데이터 바인딩을 통해 연결 됩니다. 뷰의 BindingContext는 일반적으로 ViewModel의 인스턴스입니다._

## <a name="a-simple-viewmodel"></a>간단한 ViewModel

ViewModels을 소개 하 고, 먼저 프로그램이 없는 프로그램을 살펴보겠습니다.
이전에는 XAML 파일이 다른 어셈블리의 클래스를 참조할 수 있도록 새 XML 네임 스페이스 선언을 정의 하는 방법을 살펴보았습니다. 네임 스페이스에 대 한 XML 네임 스페이스 선언을 정의 하는 프로그램은 `System` 다음과 같습니다.

```csharp
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

프로그램은를 사용 `x:Static` 하 여 정적 속성에서 현재 날짜 및 시간을 가져오고 `DateTime.Now` 해당 값을의로 설정할 수 있습니다 `DateTime` `BindingContext` `StackLayout` .

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext`는 특별 한 속성입니다. 요소에 대해를 설정 하면 해당 `BindingContext` 요소의 모든 자식에 의해 상속 됩니다. 즉,의 모든 자식 항목이 동일 하 `StackLayout` `BindingContext` 고 해당 개체의 속성에 대 한 단순 바인딩을 포함할 수 있습니다.

**일회성 DateTime** 프로그램에서 두 개의 자식에는 해당 값의 속성에 대 한 바인딩이 포함 되어 `DateTime` 있지만 두 개의 다른 자식은 바인딩 경로가 누락 된 것으로 보이는 바인딩을 포함 합니다. 즉, `DateTime` 값 자체가에 사용 됩니다 `StringFormat` .

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

XAML 파일에는 항상 현재 시간을 표시 하는 클록이 표시 될 수 있지만 도움이 되는 코드가 필요 합니다. MVVM 측면에서 생각할 때 모델 및 ViewModel은 완전히 코드로 작성 된 클래스입니다. 뷰는 대개 데이터 바인딩을 통해 ViewModel에 정의 된 속성을 참조 하는 XAML 파일입니다.

적절 한 모델은 ViewModel을 무시 하 고 적절 한 ViewModel은 뷰를 무시 합니다. 그러나 프로그래머는 ViewModel에서 노출 하는 데이터 형식을 특정 사용자 인터페이스와 관련 된 데이터 형식으로 맞춤화 하기도 합니다. 예를 들어 모델에서 8 비트 문자 ASCII 문자열을 포함 하는 데이터베이스에 액세스 하는 경우 ViewModel은 사용자 인터페이스에서 유니코드의 단독 사용을 수용 하기 위해 이러한 문자열을 유니코드 문자열로 변환 해야 합니다.

MVVM의 간단한 예 (예: 여기에 표시 된 것)에서는 모델이 전혀 없는 경우가 많으며 패턴에는 데이터 바인딩과 연결 된 뷰 및 ViewModel만 포함 됩니다.

다음은 두 번째 속성을 업데이트 하는 라는 단일 속성만 있는 clock의 ViewModel입니다 `DateTime` `DateTime` .

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

ViewModels은 일반적으로 `INotifyPropertyChanged` 인터페이스를 구현 합니다. 즉, `PropertyChanged` 해당 속성 중 하나가 변경 될 때마다 클래스가 이벤트를 발생 시킵니다. 의 데이터 바인딩 메커니즘은 Xamarin.Forms 이 이벤트에 처리기를 연결 하 여 `PropertyChanged` 속성이 변경 되 고 대상이 새 값으로 업데이트 된 상태로 유지 될 때 알림이 표시 될 수 있도록 합니다.

이 ViewModel을 기반으로 하는 clock은 다음과 같이 간단할 수 있습니다.

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

를 `ClockViewModel` `BindingContext` `Label` using 속성 요소 태그의로 설정 하는 방법을 확인 합니다. 또는 `ClockViewModel` 컬렉션에서를 인스턴스화하고 `Resources` `BindingContext` 태그 확장을 통해로 설정할 수 있습니다 `StaticResource` . 또는 코드 숨겨진 파일이 ViewModel을 인스턴스화할 수 있습니다.

`Binding`의 속성에 대 한 태그 확장은 `Text` `Label` 속성의 형식을 지정 합니다 `DateTime` . 표시 되는는 다음과 같습니다.

[![](data-bindings-to-mvvm-images/clock.png "View Displaying Date and Time via ViewModel")](data-bindings-to-mvvm-images/clock-large.png#lightbox "View Displaying Date and Time via ViewModel")

`DateTime`속성을 마침표로 구분 하 여 ViewModel의 속성에 대 한 개별 속성에 액세스할 수도 있습니다.

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>대화형 MVVM

MVVM는 기본 데이터 모델을 기반으로 하는 대화형 보기에 양방향 데이터 바인딩과 함께 사용 되는 경우가 많습니다.

다음은 `HslViewModel` 값을, 및 값으로 변환 하는 라는 클래스 `Color` `Hue` `Saturation` `Luminosity` 이며 그 반대의 경우도 마찬가지입니다.

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

`Hue`, `Saturation` 및 속성을 변경 하면 `Luminosity` `Color` 속성이 변경 되 고에 대 한 변경 내용으로 인해 `Color` 다른 세 가지 속성이 변경 됩니다. 속성이 변경 되지 않은 경우 클래스가 이벤트를 호출 하지 않는다는 점을 제외 하면 무한 루프 처럼 보일 수 있습니다 `PropertyChanged` . 이렇게 하면 제어할 수 없는 피드백 루프에 끝이 배치 됩니다.

다음 XAML 파일에는 `BoxView` `Color` 속성이 `Color` ViewModel의 속성에 바인딩되어 있고 `Slider` `Label` `Hue` , `Saturation` 및 속성에 바인딩된 3 개 및 3 개의 뷰가 `Luminosity` 포함 되어 있습니다.

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

각각에 대 한 바인딩이 `Label` 기본값입니다 `OneWay` . 값을 표시 하기만 하면 됩니다. 그러나 각에 대 한 `Slider` 바인딩은 `TwoWay` 입니다. 이를 통해를 `Slider` ViewModel에서 초기화할 수 있습니다. `Color`ViewModel이 인스턴스화될 때 속성이로 설정 되어 있는지 확인 `Aqua` 합니다. 그러나의 변경 내용으로는 `Slider` ViewModel의 속성에 새 값을 설정 해야 합니다. 그런 다음 새 색을 계산 합니다.

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "MVVM using Two-Way Data Bindings")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "MVVM using Two-Way Data Bindings")

## <a name="commanding-with-viewmodels"></a>ViewModels 사용 하 여 명령

대부분의 경우 MVVM 패턴은 데이터 항목을 조작 하는 것으로 제한 됩니다. 사용자 인터페이스 개체는 ViewModel의 병렬 데이터 개체 보기에 있습니다.

그러나 뷰에서 여러 작업을 트리거하는 단추를 뷰에 포함 해야 하는 경우도 있습니다. 그러나 viewmodel은 `Clicked` viewmodel을 특정 사용자 인터페이스 패러다임에 연결 하기 때문에 단추에 대 한 처리기를 포함 해서는 안 됩니다.

ViewModels을 특정 사용자 인터페이스 개체와 독립적으로 사용할 수 있지만 ViewModel 내에서 메서드를 호출할 수 있도록 허용 하려면 *명령* 인터페이스가 있습니다. 이 명령 인터페이스는의 다음 요소에서 지원 됩니다 Xamarin.Forms .

- `Button`
- `MenuItem`
- `ToolbarItem`
- `SearchBar`
- `TextCell`(및 `ImageCell` )
- `ListView`
- `TapGestureRecognizer`

`SearchBar`및 요소를 제외 하 고 `ListView` , 이러한 요소는 두 가지 속성을 정의 합니다.

- `Command`유형`System.Windows.Input.ICommand`
- `CommandParameter`유형`Object`

는 `SearchBar` `SearchCommand` 및 `SearchCommandParameter` 속성을 정의 하는 반면는 `ListView` 형식의 속성을 정의 합니다 `RefreshCommand` `ICommand` .

`ICommand`인터페이스는 다음과 같은 두 개의 메서드와 하나의 이벤트를 정의 합니다.

- `void Execute(object arg)`
- `bool CanExecute(object arg)`
- `event EventHandler CanExecuteChanged`

ViewModel은 형식의 속성을 정의할 수 있습니다 `ICommand` . 그런 다음 이러한 속성을 `Command` 각 `Button` 또는 다른 요소의 속성 또는이 인터페이스를 구현 하는 사용자 지정 보기에 바인딩할 수 있습니다. 필요에 따라 속성을 설정 `CommandParameter` 하 여 `Button` 이 ViewModel 속성에 바인딩된 개별 개체 (또는 다른 요소)를 식별할 수 있습니다. 내부적으로은 `Button` `Execute` 사용자가를 탭 할 때마다 메서드를 호출 하 여 `Button` `Execute` 메서드를 메서드에 전달 `CommandParameter` 합니다.

`CanExecute`메서드와 이벤트는 `CanExecuteChanged` 탭이 현재 유효 하지 않을 수 있는 경우에 사용 됩니다 .이 경우 `Button` 는 `Button` 자신을 사용 하지 않도록 설정 해야 합니다. `Button` `CanExecute` `Command` 속성이 처음으로 설정 되 고 이벤트가 발생 될 때마다를 호출 합니다 `CanExecuteChanged` . 가 `CanExecute` 를 반환 하면 `false` 가 `Button` 자신을 사용 하지 않도록 설정 하 고 호출을 생성 하지 않습니다 `Execute` .

ViewModels 명령 추가에 대 한 도움말을 보려면에서를 Xamarin.Forms 구현 하는 두 개의 클래스를 정의 합니다. `ICommand` `Command` `Command<T>` 여기서 `T` 은 및에 대 한 인수의 형식입니다 `Execute` `CanExecute` . 이러한 두 클래스는 여러 생성자와 `ChangeCanExecute` ViewModel이 이벤트를 발생 시키는 개체를 강제로 호출할 수 있는 메서드를 정의 `Command` `CanExecuteChanged` 합니다.

전화 번호를 입력 하기 위한 간단한 키패드의 ViewModel은 다음과 같습니다. `Execute`및 `CanExecute` 메서드는 생성자에서 바로 람다 함수로 정의 됩니다.

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

이 ViewModel은 `AddCharCommand` 속성이 `Command` 여러 단추의 속성 (또는 명령 인터페이스가 있는 기타 모든 항목)에 바인딩되어 있다고 가정 합니다. 이러한 속성은 각각로 식별 됩니다 `CommandParameter` . 이러한 단추는 속성에 문자 `InputString` 를 추가한 다음 속성의 전화 번호로 형식이 지정 됩니다 `DisplayText` .

또한 이라는 형식의 두 번째 속성이 있습니다 `ICommand` `DeleteCharCommand` . 이는 배경 간격 단추에 바인딩되므로 삭제할 문자가 없으면 단추를 사용할 수 없습니다.

다음 키패드는 다음과 같이 시각적으로 복잡 하지 않습니다. 대신 명령 인터페이스 사용을 보다 명확 하 게 보여 주기 위해 태그가 최소한으로 줄었습니다.

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

`Command`이 태그에 표시 되는 첫 번째의 속성은에 `Button` 바인딩되고 `DeleteCharCommand` 나머지는 `AddCharCommand` 면에 표시 되는 문자와 동일한를 사용 하 여에 바인딩됩니다 `CommandParameter` `Button` . 작동 중인 프로그램은 다음과 같습니다.

[![](data-bindings-to-mvvm-images/keypad.png "Calculator using MVVM and Commands")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "Calculator using MVVM and Commands")

### <a name="invoking-asynchronous-methods"></a>비동기 메서드 호출

명령에서 비동기 메서드를 호출할 수도 있습니다. 이는 `async` `await` 메서드를 지정할 때 및 키워드를 사용 하 여 `Execute` 수행 됩니다.

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

이는 메서드가이 `DownloadAsync` `Task` 고 대기 해야 함을 나타냅니다.

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

이 문서 시리즈의 모든 소스 코드를 포함 하는 [Xamlsamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples) 프로그램은 홈 페이지에 ViewModel을 사용 합니다. 이 ViewModel은 `Type` `Title` `Description` 각 샘플 페이지의 형식, 제목 및 간단한 설명을 포함 하는, 및 라는 세 개의 속성을 가진 short 클래스의 정의입니다. 또한 ViewModel은 `All` 프로그램에 있는 모든 페이지의 컬렉션인 이라는 정적 속성을 정의 합니다.

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

에 대 한 XAML 파일은 `MainPage` `ListBox` `ItemsSource` 속성이 해당 속성으로 설정 되 `All` 고 `TextCell` `Title` `Description` 각 페이지의 및 속성을 표시 하기 위한를 포함 하는를 정의 합니다.

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

페이지는 스크롤 가능한 목록으로 표시 됩니다.

[![](data-bindings-to-mvvm-images/mainpage.png "Scrollable list of pages")](data-bindings-to-mvvm-images/mainpage-large.png#lightbox "Scrollable list of pages")

사용자가 항목을 선택 하면 코드 숨김이 파일의 처리기가 트리거됩니다. 처리기는 `SelectedItem` 의 속성을로 설정 하 `ListBox` `null` 고 선택 된 페이지를 인스턴스화한 다음 해당 페이지를 탐색 합니다.

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

**Xamarin 진화 2016: MVVM를 사용 하 여 간단 하 게 만든 Xamarin.Forms 프리즘**

## <a name="summary"></a>요약

XAML은 응용 프로그램에서 사용자 인터페이스를 정의 하기 위한 강력한 도구 이며 Xamarin.Forms 특히 데이터 바인딩 및 MVVM를 사용 하는 경우에 유용 합니다. 결과적으로 코드의 모든 백그라운드 지원이 포함 된 사용자 인터페이스를 정리 하 고, 세련 되 고, 잠재적으로 표시할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [1 부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [2 부. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [3 부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [4 부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)

## <a name="related-videos"></a>관련 동영상

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-MVVM-with-XAML-6-of-11/player]

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-Navigation-with-XAML-7-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
