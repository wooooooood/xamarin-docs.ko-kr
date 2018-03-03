---
title: "5 단계입니다. MVVM에 대 한 데이터 바인딩을에서"
description: "모델-뷰-MVVM () 아키텍처 패턴 염두에서 XAML과 고안 된 합니다. 세 가지 소프트웨어 계층을 분리를 적용 하는 패턴-; 뷰라고 XAML 사용자 인터페이스 내부 데이터, 모델 고 보기와 모델 간에 중간자 ViewModel 라고 합니다. 뷰와 ViewModel 종종 XAML 파일에 정의 된 데이터 바인딩을 통해 연결 됩니다. 뷰에 대 한 BindingContext ViewModel의 인스턴스는 일반적으로 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: b16aa2456cdae7a08f8f9ee8adbc32c124e78e18
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>5 단계입니다. MVVM에 대 한 데이터 바인딩을에서

_모델-뷰-MVVM () 아키텍처 패턴 염두에서 XAML과 고안 된 합니다. 세 가지 소프트웨어 계층을 분리를 적용 하는 패턴-; 뷰라고 XAML 사용자 인터페이스 내부 데이터, 모델 고 보기와 모델 간에 중간자 ViewModel 라고 합니다. 뷰와 ViewModel 종종 XAML 파일에 정의 된 데이터 바인딩을 통해 연결 됩니다. 뷰에 대 한 BindingContext ViewModel의 인스턴스는 일반적으로 합니다._

## <a name="a-simple-viewmodel"></a>간단한 ViewModel

Viewmodel에 대 한 기본적인를 먼저 살펴보겠습니다 하나 하지 않고 프로그램입니다.
이전 버전에서 다른 어셈블리의 참조 클래스에 XAML 파일을 허용 하려면 새 XML 네임 스페이스 선언을 정의 하는 방법을 알아보았습니다. 다음에 대 한 XML 네임 스페이스 선언을 정의 하는 프로그램은는 `System` 네임 스페이스:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

프로그램 צ ְ ײ `x:Static` 정적 현재 날짜 및 시간을 얻으려고 `DateTime.Now` 속성을 설정 하 고 `DateTime` 값을 `BindingContext` 에 `StackLayout`:

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext` 매우 특수 속성: 설정 하는 경우는 `BindingContext` 요소에 해당 요소의 모든 자식을에서 상속 됩니다. 즉, 모든 자식을 `StackLayout` 이 동일한 `BindingContext`, 고 해당 개체의 속성에 대 한 간단한 바인딩을 포함할 수 있습니다.

에 **One-Shot DateTime** 프로그램 자식이 두 개 포함 된의 속성에 대 한 바인딩을 `DateTime` 값과 동일 하지만 다른 두 명의 자식이 포함 하는 바인딩이 없거나 바인딩 경로입니다. 즉는 `DateTime` 값 자체에 사용 되는 `StringFormat`:

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

물론, 큰 문제가 날짜와 시간 표시 되는 페이지 처음 빌드할 때 면 집합과 되지 변경.

[ ![](data-bindings-to-mvvm-images/oneshotdatetime.png "날짜 및 시간을 표시 하는 보기")](data-bindings-to-mvvm-images/oneshotdatetime-large.png "날짜 및 시간을 표시 하는 보기")

XAML 파일에는 항상 현재 시간을 보여 주는 클록 표시할 수 있지만 필요한 일부 코드를 도움을 줍니다. MVVM, 모델 및 ViewModel 고안 된 경우 전체를 코드로 작성 된 클래스 됩니다. 뷰는 보통 데이터 바인딩을 통해 ViewModel에 정의 된 속성을 참조 하는 XAML 파일입니다.

적절 한 모델은 ViewModel의 무시 되며 적절 한 ViewModel 보기에 알지 못합니다. 그러나 매우 자주 프로그래머가 조정 ViewModel 특정 사용자 인터페이스와 연결 된 데이터 형식에 의해 노출 된 데이터 형식을 합니다. 예를 들어 모델에서 8 비트 ASCII 문자열을 포함 하는 데이터베이스를 액세스할 경우 ViewModel 이러한 문자열을 유니코드 문자열로 사용자 인터페이스에서 유니코드를 단독으로 사용을 수용 하기 위해 사이 변환할 해야 합니다.

(예: 여기에 표시 된) MVVM의 간단한 예제에서 종종 모델은 없으므로, 패턴에서는 보기에만 하 고 데이터 바인딩을 사용 하는 연결 된 ViewModel 합니다.

라는 하나의 속성만 사용 하 여 표시에 대 한 ViewModel 같습니다 `DateTime`, 업데이트에 있지만 `DateTime` 1 초 마다 속성:

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

Viewmodel 일반적으로 구현는 `INotifyPropertyChanged` 인터페이스는 클래스가 발생 시킨 즉는 `PropertyChanged` 속성 중 하나가 변경 될 때마다 이벤트입니다. Xamarin.Forms에 데이터 바인딩 메커니즘인이 처리기를 연결 `PropertyChanged` 이벤트 속성이 변경 될 때 알림을 받을 수 있도록 고 새 값으로 업데이트 대상을 유지 합니다.

이 ViewModel이에 따라 클록이 처럼 간단 해질 수 있습니다.

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

알림 방법을 `ClockViewModel` 로 설정 되는 `BindingContext` 의 `Label` 속성 요소 태그를 사용 하 여 합니다. 인스턴스화할 수 있습니다는 `ClockViewModel` 에 `Resources` 컬렉션으로 설정 하는 `BindingContext` 통해는 `StaticResource` 태그 확장 합니다. 또는 코드 숨김 파일 ViewModel 인스턴스화할 수 있습니다.

`Binding` 태그 확장에는 `Text` 의 속성은 `Label` 형식은 `DateTime` 속성입니다. 디스플레이 다음과 같습니다.

[ ![](data-bindings-to-mvvm-images/clock.png "ViewModel 통해 시간과 날짜를 표시 하는 보기")](data-bindings-to-mvvm-images/clock-large.png "ViewModel 통해 시간과 날짜를 표시 하는 보기")

개별 속성에 액세스할 수 이기도 `DateTime` 마침표로 구분 하 여 ViewModel 속성:

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>대화형 MVVM

MVVM는 기본 데이터 모델에 따라 상호작용 형 보기에 대 한 양방향 데이터 바인딩을 사용 하는 경우가 매우 자주 사용 됩니다.

다음은 라는 클래스 `HslViewModel` 변환 하는 `Color` 값에 `Hue`, `Saturation`, 및 `Luminosity` 값을 그 반대의:

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

로 변경 된 `Hue`, `Saturation`, 및 `Luminosity` 속성 원인은 `Color` 속성을 변경 하려면 및 변경을 `Color` 는 다른 세 속성이 변경 합니다. 이 보일 수 있습니다 무한 루프를 제외 하 고 클래스를 호출 하지 않습니다는 `PropertyChanged` 이벤트에서 속성이 실제로 변경 하지 않는 한 합니다. 그렇지 않으면 제어할 수 없는 피드백 루프에 end를 추가합니다.

다음 XAML 파일에 포함 되어는 `BoxView` 인 `Color` 속성이 바인딩되는 `Color` ViewModel과 3의 속성 `Slider` 및 세 개의 `Label` 에 바인딩된 뷰는 `Hue`, `Saturation`, 및 `Luminosity` 속성:

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

각 바인딩 `Label` 기본값이 `OneWay`합니다. 만 값을 표시 해야 합니다. 하지만 각 바인딩 `Slider` 은 `TwoWay`합니다. 이 통해는 `Slider` ViewModel에서 초기화 합니다. 다음에 유의 `Color` 속성이 `Blue` ViewModel 인스턴스화될 때. 하지만 변경에는 `Slider` 도 다음 새 색을 계산 하는 ViewModel에서 속성에 대 한 새 값을 설정 해야 합니다.

[ ![](data-bindings-to-mvvm-images/hslcolorscroll.png "양방향 데이터 바인딩을 사용 하 여 MVVM")](data-bindings-to-mvvm-images/hslcolorscroll-large.png "양방향 데이터 바인딩을 사용 하 여 MVVM")

## <a name="commanding-with-viewmodels"></a>Viewmodel와 명령 실행

MVVM 패턴은 제한 된 데이터 항목의 조작에 대부분의 경우에서: 보기에서 사용자 인터페이스 개체 병렬 ViewModel에서 데이터 개체입니다.

그러나 때로는 보기 포함 되어야 ViewModel에서 다양 한 작업을 트리거하는 단추입니다. 하지만 디스크 ViewModel `Clicked` 단추에 대 한 처리기는 특정 사용자 인터페이스 패러다임을 ViewModel을 연결 하는 때문에 있습니다.

Viewmodel 더 특정 사용자 인터페이스 개체에 대해 독립적일 수 있지만 계속 ViewModel, 내에서 호출 될 메서드를 사용할 수 있도록 한 *명령* 인터페이스 존재 합니다. 이 명령은 인터페이스 Xamarin.Forms의 다음 요소에 의해 사용할 수 있습니다.

-  `Button`
-  `MenuItem`
-  `ToolbarItem`
-  `SearchBar`
-  `TextCell` (및 따라서도 `ImageCell`)
-  `ListView`
-  `TapGestureRecognizer`

제외는 `SearchBar` 및 `ListView` 요소를 이러한 요소는 두 개의 속성을 정의 합니다.

-  `Command` 형식의  `System.Windows.Input.ICommand`
-  `CommandParameter` 형식의  `Object`

`SearchBar` 정의 `SearchCommand` 및 `SearchCommandParameter` 속성, 동안는 `ListView` 정의 `RefreshCommand` 형식의 속성이 `ICommand`합니다.

`ICommand` 두 가지 메서드와 하나의 이벤트 인터페이스를 정의 합니다.

-  `void Execute(object arg)`
-  `bool CanExecute(object arg)`
-  `event EventHandler CanExecuteChanged`

ViewModel 형식의 속성을 정의할 수 `ICommand`합니다. 이러한 속성을 바인딩할 수 있습니다는 `Command` 각 속성 `Button` 또는 기타 요소 또는이 인터페이스를 구현 하는 사용자 지정 보기 아마도 합니다. 선택적으로 설정할 수는 `CommandParameter` 개별 식별 하기 위해 속성 `Button` 개체 (또는 다른 요소)이 ViewModel 속성에 바인딩됩니다. 내부적으로 `Button` 호출은 `Execute` 메서드는 사용자가 누를 때마다는 `Button`를 전달 하는 `Execute` 메서드 해당 `CommandParameter`합니다.

`CanExecute` 메서드 및 `CanExecuteChanged` 이벤트는 다음과 같은 경우에 사용 됩니다. 여기서는 `Button` tap 잘못 되었을 수 현재,이 경우는 `Button` 자체를 비활성화 해야 합니다. `Button` 호출 `CanExecute` 때는 `Command` 속성이 먼저 언제는 `CanExecuteChanged` 이벤트가 발생 합니다. 경우 `CanExecute` 반환 `false`, `Button` 자체를 사용 하지 않도록 설정 하 고 생성 하지 `Execute` 호출 합니다.

Xamarin.Forms에 대 한 도움말 추가 하 여 Viewmodel 명령 실행을 구현 하는 두 클래스를 정의 `ICommand`: `Command` 및 `Command<T>` 여기서 `T` 에 대 한 인수의 형식이 `Execute` 및 `CanExecute`합니다. 이 두 클래스는 여러 생성자 정의 뒤에 `ChangeCanExecute` ViewModel 강제로 호출할 수 있는 메서드는 `Command` 를 발생 시키는 개체는 `CanExecuteChanged` 이벤트입니다.

다음은 전화 번호를 입력 하는 데 사용 되는 간단한 키패드에 대 한 ViewModel입니다. 에 `Execute` 및 `CanExecute` 메서드는 생성자에서 람다 함수 오른쪽으로 정의 됩니다.

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

이 ViewModel 있다고 가정는 `AddCharCommand` 속성이 바인딩되는 `Command` 으로 식별 되는 각각 여러 단추 (또는 다른 명령 인터페이스에 있는 모든 항목)의 속성은 `CommandParameter`합니다. 이러한 단추 추가 문자는 `InputString` 속성을 다음에 대 한 전화 번호를 서식이 `DisplayText` 속성입니다.

형식의 두 번째 속성 이기도 `ICommand` 라는 `DeleteCharCommand`합니다. 이 백 간격 단추에 바인딩된 하지만 삭제할 문자가 없는 경우 단추가 비활성화 해야 합니다.

다음 키패드는 하지 시각적으로 같이 명령 처럼 복잡 수 있습니다. 대신, 태그 명령 인터페이스의 사용 보다 명확 하 게 보여 주기 위해 최소한으로 축소 되었습니다.

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

`Command` 첫 번째 속성 `Button` 이 나타나는 태그에 바인딩된는 `DeleteCharCommand`; 나머지 부분에 바인딩된는 `AddCharCommand` 와 `CommandParameter` 동일 하 게에 표시 되는 문자는 `Button` 얼굴 합니다. 다음은 실행에서 프로그램이입니다.

[ ![](data-bindings-to-mvvm-images/keypad.png "MVVM 및 명령을 사용 하 여 계산기")](data-bindings-to-mvvm-images/keypad-large.png "MVVM 및 명령을 사용 하 여 계산기")

### <a name="invoking-asynchronous-methods"></a>비동기 메서드 호출

명령에서 비동기 메서드를 호출할 수도 수 있습니다. 사용 하 여 이렇게는 `async` 및 `await` 지정할 때는 키워드는 `Execute` 메서드:

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

있음을 나타냅니다는 `DownloadAsync` 메서드는는 `Task` 고 대기 해야 합니다.

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

[XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/) 프로그램이이 일련의 문서에서 모든 소스 코드를 포함 하는 ViewModel를 사용 하 여 홈 페이지에 대 한 합니다. 이 ViewModel 라는 세 가지 속성을 사용 하는 짧은 클래스의 정의 `Type`, `Title`, 및 `Description` 유형의 각 샘플 페이지, 제목과 간단한 설명을 포함 하는 합니다. ViewModel 라는 정적 속성을 정의 하는 또한 `All` 프로그램의 모든 페이지의 컬렉션입니다.

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

에 대 한 XAML 파일 `MainPage` 정의 `ListBox` 인 `ItemsSource` 속성이 있는 `All` 속성을 포함 하는 `TextCell` 표시 하기 위한는 `Title` 및 `Description` 각 페이지의 속성:

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

페이지는 스크롤 가능한 목록에 나와 있습니다.

[ ![](data-bindings-to-mvvm-images/mainpage.png "페이지를 스크롤할 수 있는 목록이")](data-bindings-to-mvvm-images/mainpage-large.png "스크롤할 수 있는 목록 페이지")

코드 숨김 파일에서 처리기는 사용자가 항목을 선택할 때 트리거됩니다. 처리기 집합은 `SelectedItem` 의 속성은 `ListBox` 다시 `null` 한 다음 선택한 페이지를 인스턴스화하고를 탐색:

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

## <a name="summary"></a>요약

XAML은 Xamarin.Forms 응용 프로그램에서 데이터 바인딩할 때에 특히 사용자 인터페이스를 정의 하기 위한 강력한 도구 및 MVVM 사용 됩니다. 코드의 모든 백그라운드 지원 사용자 인터페이스의 깨끗 한 잠재적으로 높은 표현 됩니다.


## <a name="related-links"></a>관련 링크

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [1 부입니다. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [2 부 합니다. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [3 부 합니다. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [4 부입니다. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
