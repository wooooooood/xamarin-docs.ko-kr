---
title: 5 부입니다. 데이터 바인딩부터 MVVM
description: MVVM 패턴 3 소프트웨어 계층 간의 분리를 적용-; 뷰라고 XAML 사용자 인터페이스 기본 데이터 모델 호출 고 뷰와 모델 간의 중개자로 ViewModel 이라고 합니다.
ms.prod: xamarin
ms.assetid: 48B37D44-4FB1-41B2-9A5E-6D383B041F81
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: 2376ff986db985c3764c90c3af76ea74c2936a29
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563149"
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>5 부입니다. 데이터 바인딩부터 MVVM

_모델-뷰-ViewModel (MVVM) 아키텍처 패턴은 XAML을 사용 하 여 염두에서 이었으니까요. 세 소프트웨어 계층 간의 분리를 적용 하는 패턴-; 뷰라고 XAML 사용자 인터페이스 기본 데이터 모델 호출 고 뷰와 모델 간의 중개자로 ViewModel 이라고 합니다. 종종 뷰와 ViewModel이 XAML 파일에 정의 된 데이터 바인딩을 통해 연결 됩니다. 뷰에 대 한 BindingContext는 일반적으로 ViewModel의 인스턴스입니다._

## <a name="a-simple-viewmodel"></a>간단한 ViewModel

Viewmodel에 대 한 기본적인를 먼저 살펴보겠습니다 없는 프로그램.
이전 새 XML 네임 스페이스 선언을 XAML 파일을 다른 어셈블리에서 참조 클래스를 허용 하도록 정의 하는 방법을 살펴보았습니다. 여기는 대 한 XML 네임 스페이스 선언을 정의 하는 프로그램을 `System` 네임 스페이스:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

프로그램 사용할 수 `x:Static` 정적에서 현재 날짜 및 시간을 얻을 수 `DateTime.Now` 속성 설정 `DateTime` 값을 `BindingContext` 에 `StackLayout`:

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext` 매우 특별 한 속성: 설정 하는 경우는 `BindingContext` 해당 요소의 모든 자식에 의해 상속 된 요소에 대해 합니다. 즉, 모든 자식을 합니다 `StackLayout` 이 동일한 `BindingContext`, 고 해당 개체의 속성에 대 한 간단한 바인딩을 포함할 수 있습니다.

에 **One-Shot DateTime** 자식의 두 프로그램을 포함 하는 속성에 대 한 바인딩을 `DateTime` 값과 동일 하지만 다른 두 명의 자식이 포함 바인딩 경로 없는 것으로 보이는 바인딩. 즉 합니다 `DateTime` 자체 값이 사용 되는 `StringFormat`:

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

물론, 큰 문제는 날짜 및 시간 설정 페이지 처음 빌드할 때 면 이며 변경 되지 않습니다.

[![](data-bindings-to-mvvm-images/oneshotdatetime.png "날짜 및 시간을 표시 하는 보기")](data-bindings-to-mvvm-images/oneshotdatetime-large.png#lightbox "날짜 및 시간을 표시 하는 보기")

XAML 파일에는 항상 현재 시간을 보여 주는 클록 표시할 수 있습니다 하지만 일부 코드를 지원 해야 합니다. 경우 MVVM, 모델 및 ViewModel에 관하여 생각은 코드로 완전히 작성 하는 클래스입니다. 뷰는 데이터 바인딩을 통해 ViewModel에 정의 된 속성을 참조 하는 XAML 파일 경우가 많습니다.

적절 한 모델은 ViewModel의 무시 되며 적절 한 ViewModel 뷰의 무시 합니다. 그러나 자주 프로그래머가 조정 ViewModel 특정 사용자 인터페이스와 연결 된 데이터 형식에 의해 노출 되는 데이터 형식입니다. 예를 들어, 모델을 8 비트 ASCII 문자열을 포함 하는 데이터베이스에 액세스 하는 경우 ViewModel 단독 사용자 인터페이스에서 유니코드 사용에 맞게 유니코드 문자열을 해당 문자열 간에 변환 해야 합니다.

(예: 여기에 표시 된) MVVM의 간단한 예제에서는 종종 모델이 없는 전혀 패턴만 뷰를 포함 하 고 데이터 바인딩을 사용 하 여 연결 된 ViewModel입니다.

라는 단일 속성만 사용 하 여 클록에 대 한 ViewModel 다음과 같습니다 `DateTime`를 업데이트 하는 하지만 `DateTime` 매초 속성:

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

Viewmodel 일반적으로 구현 합니다 `INotifyPropertyChanged` 클래스에서 발생 하는 인터페이스를 `PropertyChanged` 해당 속성 중 하나가 변경 될 때마다 이벤트. 이 처리기를 연결 하는 Xamarin.Forms에 데이터 바인딩 메커니즘인 `PropertyChanged` 속성이 변경 될 때 알림을 받을 수 있도록 이벤트 및 새 값으로 업데이트 대상 유지 합니다.

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

[![](data-bindings-to-mvvm-images/clock.png "ViewModel 통해 시간과 날짜를 표시 하는 보기")](data-bindings-to-mvvm-images/clock-large.png#lightbox "ViewModel 통해 시간과 날짜를 표시 하는 보기")

개별 속성에 액세스할 수 이기도 합니다 `DateTime` 마침표를 사용 하 여 속성을 구분 하 여 ViewModel의 속성:

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>대화형 MVVM

MVVM의 상호작용 형 보기는 기본 데이터 모델을 기반으로 양방향 데이터 바인딩을 경우가 매우 자주 사용 됩니다.

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

로 변경 합니다 `Hue`, `Saturation`, 및 `Luminosity` 속성 원인을 `Color` 속성을 변경 하려면 및 변경을 `Color` 다른 세 가지 속성을 변경 하면 합니다. 이 처럼 보이지만, 무한 루프를 제외 하 고 클래스를 호출 하지 않습니다는 `PropertyChanged` 이벤트 속성이 실제로 변경 하지 않는 한 합니다. 이 끝을 제어할 수 없는 그렇지 않은 경우 피드백 루프를 넣습니다.

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

각 바인딩에 `Label` 기본값인 `OneWay`합니다. 만 값을 표시 해야 합니다. 하지만 각각의 바인딩에서 `Slider` 는 `TwoWay`합니다. 따라서는 `Slider` ViewModel에서 초기화 되어야 합니다. 에 `Color` 속성이 `Aqua` ViewModel 인스턴스화될 때. 하지만 변경은 `Slider` 다음 새 색을 계산 하는 ViewModel의 속성에 대 한 새 값을 설정 해야 합니다.

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "양방향 데이터 바인딩을 사용 하 여 MVVM")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "양방향 데이터 바인딩을 사용 하 여 MVVM")

## <a name="commanding-with-viewmodels"></a>Viewmodel을 사용 하 여 명령 실행

MVVM 패턴은 대부분의 경우에서 데이터 항목의 조작에 제한 됩니다: 뷰의 사용자 인터페이스 개체 병렬 ViewModel에 있는 데이터 개체입니다.

그러나 경우에 따라 뷰를 포함 해야 ViewModel에 다양 한 작업을 트리거하는 단추 합니다. ViewModel이 없어야 합니다. 하지만 `Clicked` 단추에 대 한 처리기 하므로 특정 사용자 인터페이스 패러다임을 ViewModel에 연결 될 것입니다.

보다는 특정 사용자 인터페이스 개체와 독립적일 수 있지만 ViewModel에 내에서 호출 될 메서드를 허용 하는 Viewmodel을 허용 하는 *명령* 인터페이스 존재 합니다. 이 명령은 인터페이스는 Xamarin.Forms에서 다음과 같은 요소에서 지원 됩니다.

-  `Button`
-  `MenuItem`
-  `ToolbarItem`
-  `SearchBar`
-  `TextCell` (및 따라서도 `ImageCell`)
-  `ListView`
-  `TapGestureRecognizer`

제외 된 `SearchBar` 및 `ListView` 요소, 두 속성을 정의 하는 이러한 요소:

-  `Command` 형식의  `System.Windows.Input.ICommand`
-  `CommandParameter` 형식의  `Object`

`SearchBar` 정의 `SearchCommand` 및 `SearchCommandParameter` 속성을 하는 동안 합니다 `ListView` 정의 `RefreshCommand` 형식의 속성 `ICommand`합니다.

`ICommand` 두 메서드와 하나의 이벤트 인터페이스를 정의 합니다.

-  `void Execute(object arg)`
-  `bool CanExecute(object arg)`
-  `event EventHandler CanExecuteChanged`

ViewModel 형식의 속성을 정의할 수 `ICommand`입니다. 이러한 속성을 바인딩할 수 있습니다 합니다 `Command` 의 각 속성 `Button` 다른 요소 또는이 인터페이스를 구현 하는 사용자 지정 보기를 가장 합니다. 선택적으로 설정할 수 있습니다 합니다 `CommandParameter` 개별 식별 하기 위해 속성 `Button` 개체 (또는 다른 요소)이 ViewModel 속성에 바인딩되어 있는 합니다. 내부적으로 `Button` 호출을 `Execute` 사용자가 누를 때마다 메서드는 `Button`에 전달 합니다 `Execute` 메서드 해당 `CommandParameter`합니다.

`CanExecute` 메서드 및 `CanExecuteChanged` 이벤트의 경우에 사용 되는 `Button` 탭 않을, 현재 유효한 경우는 `Button` 자체를 비활성화 해야 합니다. `Button` 호출 `CanExecute` 때 합니다 `Command` 속성이 먼저 언제는 `CanExecuteChanged` 이벤트가 발생 합니다. 경우 `CanExecute` 반환 `false`서 `Button` 자체를 사용 하지 않도록 설정 하 고 생성 하지 않습니다 `Execute` 호출 합니다.

Xamarin.Forms Viewmodel에 명령 추가 도움말을 구현 하는 두 개의 클래스를 정의 `ICommand`: `Command` 하 고 `Command<T>` 여기서 `T` 에 대 한 인수의 형식이 `Execute` 및 `CanExecute`. 이 두 클래스는 여러 생성자를 정의 및 `ChangeCanExecute` ViewModel 하도록 호출할 수 있는 메서드를 `Command` 시키려면 개체는 `CanExecuteChanged` 이벤트입니다.

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

`Command` 첫 번째 속성 `Button` 이 표시 되는 태그에 바인딩되어를 `DeleteCharCommand`; 나머지 부분에 바인딩된를 `AddCharCommand` 사용 하 여를 `CommandParameter` 에 표시 되는 문자는 동일 합니다 `Button` 얼굴. 작업에서 프로그램이 다음과 같습니다.

[![](data-bindings-to-mvvm-images/keypad.png "MVVM 및 명령을 사용 하 여 계산기")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "명령과 MVVM을 사용 하 여 계산기")

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

합니다 [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/) 이 문서 시리즈의 모든 소스 코드를 포함 하는 프로그램은 홈 페이지에 대 한 ViewModel을 사용 합니다. 이 ViewModel은 라는 세 가지 속성을 사용 하 여 간단한 클래스의 정의 `Type`, `Title`, 및 `Description` 유형의 각 샘플 페이지, 제목 및 간단한 설명을 포함 하는 합니다. ViewModel 라는 정적 속성을 정의 하는 또한 `All` 프로그램의 모든 페이지의 컬렉션입니다.

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

[![](data-bindings-to-mvvm-images/mainpage.png "페이지의 스크롤 가능한 목록이")](data-bindings-to-mvvm-images/mainpage-large.png#lightbox "스크롤 가능한 목록이 페이지")

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

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [1부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [2부. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [3부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [4부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
