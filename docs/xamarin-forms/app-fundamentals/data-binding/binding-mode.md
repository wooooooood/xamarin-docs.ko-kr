---
title: Xamarin.Forms 바인딩 모드
description: 이 문서에서는 원본 및 대상 BindingMode 열거형의 멤버와 지정 된 바인딩 모드를 사용 하 여 간 정보의 흐름을 제어 하는 방법을 설명 합니다. 바인딩할 수 있는 속성이 모든 속성이 데이터 바인딩 대상 해당 하는 경우 적용 모드를 나타내는 기본 바인딩 모드를 보여 줍니다.
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/01/2018
ms.openlocfilehash: 420c1de0691de419180dd497a9031ea5e7dd1054
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "37935669"
---
# <a name="xamarinforms-binding-mode"></a>Xamarin.Forms 바인딩 모드

에 [이전 문서](basic-bindings.md)의 **대체 코드 바인딩** 및 **대신 XAML 바인딩** 추천 하는 페이지를 `Label` 사용 하 여 해당 `Scale` 속성 바인딩되는 `Value` 의 속성을 `Slider`합니다. 때문에 `Slider` 초기 값이 0 이면이 인해를 `Scale` 의 속성을 `Label` 1이 아닌 0으로 설정 됩니다 및 `Label` 사라졌습니다.

에 [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) 샘플을 **역방향 바인딩** 페이지는 이전 문서에서는 프로그램와 유사 합니다 에데이터바인딩이정의된제외하고`Slider` 대신에 `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.ReverseBindingPage"
             Title="Reverse Binding">
    <StackLayout Padding="10, 0">

        <Label x:Name="label"
               Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                VerticalOptions="CenterAndExpand"
                Value="{Binding Source={x:Reference label},
                                Path=Opacity}" />
    </StackLayout>
</ContentPage>
```

처음에 울 수 있지만 이전 버전과: 이제는 `Label` 는 데이터 바인딩 소스 및 `Slider` 대상입니다. 바인딩 참조를 `Opacity` 의 속성을 `Label`, 1의 기본 값.

예상할 수 있듯이 합니다 `Slider` 초기에서 값을 1로 초기화 됩니다 `Opacity` 의 값 `Label`합니다. 이 iOS 스크린 샷 왼쪽에 표시 됩니다.

[![바인딩 역방향](binding-mode-images/reversebinding-small.png "바인딩 역방향")](binding-mode-images/reversebinding-large.png#lightbox "역방향 바인딩")

경우도 있겠지만 놀 랐는 `Slider` 스크린샷에서 Android 및 UWP 시연 하려면 계속 됩니다. 데이터 바인딩의 작동 더 나은 경우에는 제안에 `Slider` 바인딩 대상 대신 `Label` 예상할 수 있는 초기화 하는 방식 때문에 합니다.

간의 차이 **역방향 바인딩** 샘플과 이전 샘플에서는 합니다 *바인딩 모드*합니다.

## <a name="the-default-binding-mode"></a>기본 바인딩 모드

바인딩 모드의 구성원으로 지정 된 [ `BindingMode` ](xref:Xamarin.Forms.BindingMode) 열거형:

- [`Default`](xref:Xamarin.Forms.BindingMode.Default)
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) &ndash; 데이터 원본과 대상 간의 양방향 이동
- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) &ndash; 데이터 원본에서 대상 이동
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; 데이터 원본으로 대상에서 이동합니다.
- [`OneTime`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; 대상으로 하지만 경우에만 원본에서 데이터 이동은 `BindingContext` 변경 (새 Xamarin.Forms 3.0)

기본 바인딩 가능 속성 만들어질 때 설정 되는 모드를 바인딩 및에서 사용할 수 있는 모든 바인딩 가능한 속성에는 [ `DefaultBindingMode` ](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) 의 속성을 `BindableProperty` 개체입니다. 이 기본 바인딩 모드 데이터 바인딩 대상 속성이 해당 하는 경우 적용 모드를 나타냅니다.

와 같은 대부분의 속성에 대 한 기본 바인딩 모드 `Rotation`, `Scale`, 및 `Opacity` 는 `OneWay`합니다. 그런 다음 이러한 속성은 데이터 바인딩 대상, 대상 속성 원본에서 설정 됩니다.

그러나에 대 한 기본 바인딩 모드를 `Value` 속성을 `Slider` 는 `TwoWay`합니다. 즉, 해당 경우에는 `Value` 속성이 데이터 바인딩 대상 다음 대상 (일반적으로) 원본에서 설정 되어 있지만 대상에서 원본도 설정 됩니다. 이 통해 합니다 `Slider` 초기에서 설정할 `Opacity` 값입니다.

이 양방향 바인딩에 무한 루프를 만드는 것 처럼 보일 수 있지만 발생 하지 않습니다. 바인딩 가능한 속성 속성이 실제로 변경 하지 않는 한 속성 변경을 신호 되지 않습니다. 이 무한 루프를 방지합니다.

### <a name="two-way-bindings"></a>양방향 바인딩으로 설정

가장 바인딩 가능한 속성의 기본 바인딩 모드를 가지 `OneWay` 있으 나 다음 속성의 기본 바인딩 모드를 `TwoWay`:

- `Date` 속성 `DatePicker`
- `Text` 속성 `Editor`하십시오 `Entry`, `SearchBar`, 및 `EntryCell`
- `IsRefreshing` 속성 `ListView`
- `SelectedItem` 속성 `MultiPage`
- `SelectedIndex` 및 `SelectedItem` 의 속성 `Picker`
- `Value` 속성의 `Slider` 및 `Stepper`
- `IsToggled` 속성 `Switch`
- `On` 속성 `SwitchCell`
- `Time` 속성 `TimePicker`

이러한 특정 속성으로 정의 된 `TwoWay` 매우 합당 한 이유가 있습니다.

데이터 바인딩 소스 및 보기와 같은 구성 된 뷰는 ViewModel 클래스는 데이터 바인딩 모델-뷰-ViewModel (MVVM) 응용 프로그램 아키텍처를 사용 하 여 사용 하는 경우 `Slider`, 데이터 바인딩 대상이 됩니다. MVVM 바인딩와 유사 합니다 **바인딩 역방향** 이전 샘플에서 바인딩 보다 더 많은 샘플입니다. 각 페이지의 보기에는 ViewModel의 해당 속성의 값을 사용 하 여 초기화 하지만 보기에서 변경 해야 ViewModel 속성에는 영향을 매우 높습니다.

기본 바인딩 모드를 사용 하 여 속성 `TwoWay` MVVM 시나리오에서 사용할 가능성이 가장 높은 속성에 해당 됩니다.

### <a name="one-way-to-source-bindings"></a>한-단방향-에-소스 바인딩

읽기 전용으로 바인딩할 수 있는 속성의 기본 바인딩 모드를 사용할 `OneWayToSource`합니다. 기본 바인딩 모드를 가진 하나의 읽기/쓰기 바인딩 가능한 속성은 `OneWayToSource`:

- `SelectedItem` 속성 `ListView`

이유는 바인딩에 `SelectedItem` 속성을 설정 하 여 바인딩 소스 유발 합니다. 이 문서 뒷부분의 예는 해당 동작을 재정의합니다.

### <a name="one-time-bindings"></a>일회성 바인딩

몇 가지 속성의 기본 바인딩 모드를 가지 `OneTime`합니다. 이러한 항목은 다음과 같습니다.

- `IsTextPredictionEnabled` 속성 `Entry`
- `Text`를 `BackgroundColor`, 및 `Style` 의 속성 `Span`합니다.

바인딩 모드를 사용 하 여 속성을 대상으로 `OneTime` 바인딩 컨텍스트를 변경 하는 경우에 업데이트 됩니다. 이 대 한 이러한 대상 속성에 바인딩 소스 속성의 변경 내용을 모니터링할 필요가 없기 때문에 바인딩 인프라를 단순해 집니다.

## <a name="viewmodels-and-property-change-notifications"></a>Viewmodel 및 속성 변경 알림

합니다 **간단한 색 선택기** 페이지 간단한 ViewModel의 사용을 보여 줍니다. 데이터 바인딩을 3 개를 사용 하 여 색을 선택 하는 데 사용할 수 `Slider` 색상, 채도 및 명도 대 한 요소입니다.

ViewModel에는 데이터 바인딩 소스입니다. ViewModel 않습니다 *되지* 바인딩 가능한 속성을 정의 하지만 바인딩 인프라는 속성의 값이 변경 될 때 알림을 받을 수 있도록 알림 메커니즘을 구현 하는 것 않습니다. 이 알림 메커니즘은는 [ `INotifyPropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) 이라는 단일 속성을 정의 하는 인터페이스 [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged)합니다. 일반적으로이 인터페이스를 구현 하는 클래스의 공용 속성 중 하나에 값을 변경할 때 이벤트를 발생 시킵니다. 이벤트 속성은 변경 되지 않습니다 하는 경우 실행 될 필요가 없습니다. (합니다 `INotifyPropertyChanged` 가 인터페이스를 구현도 `BindableObject` 및 `PropertyChanged` 바인딩 가능한 속성 값이 변경 될 때마다 이벤트가 발생 합니다.)

`HslColorViewModel` 5 속성을 정의 하는 클래스:를 `Hue`, `Saturation`, `Luminosity`, 및 `Color` 속성은 서로 관련 되어 있습니다. 세 가지 중 하나가 색 구성 요소 변경 값 때 합니다 `Color` 속성은 다시 계산 및 `PropertyChanged` 모든 네 가지 속성에 대 한 이벤트가:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Hue
    {
        set
        {
            if (color.Hue != value)
            {
                Color = Color.FromHsla(value, color.Saturation, color.Luminosity);
            }
        }
        get
        {
            return color.Hue;
        }
    }

    public double Saturation
    {
        set
        {
            if (color.Saturation != value)
            {
                Color = Color.FromHsla(color.Hue, value, color.Luminosity);
            }
        }
        get
        {
            return color.Saturation;
        }
    }

    public double Luminosity
    {
        set
        {
            if (color.Luminosity != value)
            {
                Color = Color.FromHsla(color.Hue, color.Saturation, value);
            }
        }
        get
        {
            return color.Luminosity;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Hue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Saturation"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Luminosity"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

때를 `Color` 속성 변경, 정적 `GetNearestColorName` 에서 메서드는 `NamedColor` 클래스 (에 포함 합니다 **DataBindingDemos** 솔루션) 가장 가까운 명명 된 색을 가져오고 설정를 `Name` 속성. 이렇게 `Name` 속성이 개인 `set` 접근자 클래스 외부에서 설정할 수 없습니다.

ViewModel은 바인딩 원본으로 설정 되 면 바인딩 인프라에 처리기를 연결 합니다 `PropertyChanged` 이벤트입니다. 이러한 방식으로 바인딩 속성을 변경 알림을 받을 수와 변경된 된 값에서 대상 속성을 설정한 수 있습니다.

그러나 경우 대상 속성 (또는 `Binding` 대상 속성에 정의)에 `BindingMode` 의 `OneTime`, 처리기에 연결 하는 바인딩 인프라에 대 한 필요는 없습니다를 `PropertyChanged` 이벤트입니다. 대상 속성이 업데이트 될 경우에만 `BindingContext` 변경과 소스 속성 자체가 변경 될 때 되지 않습니다.

**간단한 색 선택기** XAML 파일은 합니다 `HslColorViewModel` 페이지의 리소스 사전에 초기화를 `Color` 속성입니다. `BindingContext` 의 속성을 `Grid` 로 설정 됩니다는 `StaticResource` 바인딩 해당 리소스를 참조 하는 확장:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SimpleColorSelectorPage">

    <ContentPage.Resources>
        <ResourceDictionary>
            <local:HslColorViewModel x:Key="viewModel"
                                     Color="MediumTurquoise" />

            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <Grid BindingContext="{StaticResource viewModel}">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <BoxView Color="{Binding Color}"
                 Grid.Row="0" />

        <StackLayout Grid.Row="1"
                     Margin="10, 0">

            <Label Text="{Binding Name}"
                   HorizontalTextAlignment="Center" />

            <Slider Value="{Binding Hue}" />

            <Slider Value="{Binding Saturation}" />

            <Slider Value="{Binding Luminosity}" />
        </StackLayout>
    </Grid>
</ContentPage>
```

합니다 `BoxView`, `Label`, 및 3 `Slider` 보기에서 바인딩 컨텍스트를 상속 받습니다.는 `Grid`합니다. 이러한 뷰는 ViewModel에 원본 속성을 참조 하는 모든 바인딩 대상입니다. 에 대 한는 `Color` 의 속성을 `BoxView`, 및 `Text` 의 속성을 `Label`, 데이터 바인딩은 `OneWay`: 보기에서 속성을 ViewModel의 속성에서 설정 됩니다.

`Value` 의 속성을 `Slider`, 것 인데, `TwoWay`합니다. 이렇게 하면 각 `Slider` ViewModel에서 그리고 각 설정할 ViewModel 설정할 `Slider`합니다.

프로그램을 처음 실행할 때 합니다 `BoxView`, `Label`, 및 3 `Slider` 요소는 초기에 따라 ViewModel에서 모두 설정 `Color` ViewModel 인스턴스화된 경우 속성을 설정 합니다. 이 왼쪽에 있는 iOS 스크린샷에 표시 됩니다.

[![단순한 색 선택기](binding-mode-images/simplecolorselector-small.png "단순한 색 선택기")](binding-mode-images/simplecolorselector-large.png#lightbox "단순한 색 선택기")

슬라이더를 조작 하면 합니다 `BoxView` 고 `Label` Android 및 UWP 스크린샷에서 나온 것 처럼 적절히 업데이트 됩니다.

리소스 사전에 ViewModel을 인스턴스화하는 하나의 일반적인 방법입니다. 에 대 한 속성 요소 태그 내의 ViewModel을 인스턴스화하고 이기도 합니다 `BindingContext` 속성입니다. **간단한 색 선택기** XAML 파일을 제거는 `HslColorViewModel` 리소스 사전에서로 설정 합니다 `BindingContext` 의 속성은 `Grid` 같이:

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>

    ···

</Grid>
```

바인딩 컨텍스트는 여러 가지 방법으로 설정할 수 있습니다. 경우에 따라 코드 숨김 파일을 ViewModel을 인스턴스화하고로 설정 합니다는 `BindingContext` 페이지의 속성입니다. 이들은 모두 유효한 접근 방식입니다.

## <a name="overriding-the-binding-mode"></a>바인딩 모드를 재정의합니다.

대상 속성에 기본 바인딩 모드를 특정 데이터 바인딩에 대 한 적합 없는 경우 설정 하 여 재정의할 수는 [ `Mode` ](xref:Xamarin.Forms.BindingBase.Mode) 속성을 `Binding` (또는 [ `Mode` ](xref:Xamarin.Forms.Xaml.BindingExtension.Mode) 의 속성을 `Binding` 태그 확장)의 멤버 중 하나에 `BindingMode` 열거형입니다.

그러나 설정 된 `Mode` 속성을 `TwoWay` 예상한 대로 작동 하지 않습니다. 예를 들어 수정를 시도 합니다 **대신 XAML 바인딩** 포함 하 여 XAML 파일 `TwoWay` 바인딩 정의에서:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

예상 되는 합니다 `Slider` 의 초기 값으로 초기화 됩니다를 `Scale` 속성을 1 이지만 발생 하지 않습니다. 경우는 `TwoWay` 바인딩 인스턴스화될, 대상 원본에서 가장 먼저 설정 됩니다, 즉를 `Scale` 속성는 `Slider` 0의 기본값. 경우는 `TwoWay` 바인딩은에 설정 됩니다는 `Slider`, 그런 다음 `Slider` 원본의 초기 설정 됩니다.

바인딩 모드 설정할 수 있습니다 `OneWayToSource` 에 **대신 XAML 바인딩** 샘플:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

이제는 `Slider` 1로 초기화 됩니다 (기본값인 `Scale`) 하지만 조작 하기는 `Slider` 영향을 주지 않습니다는 `Scale` 속성,이 기능은 매우 유용 합니다.

> [!NOTE]
> [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 클래스도 정의 [ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX) 및 [ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY) 확장 될 수 있는 속성을 `VisualElement` 에서 다르게는 가로 및 세로 방향입니다.

사용 하 여 기본 바인딩 모드를 재정의 하는 매우 유용한 응용 프로그램 `TwoWay` 포함 된 `SelectedItem` 의 속성 `ListView`합니다. 기본 바인딩 모드는 `OneWayToSource`합니다. 에 데이터 바인딩이 설정 된 경우는 `SelectedItem` 소스 속성에서 설정 됩니다는 ViewModel에 소스 속성을 참조할 속성을 `ListView` 선택 합니다. 그러나 일부 경우에 필요할 수 있습니다는 `ListView` ViewModel에서 초기화 되어야 합니다.

합니다 **샘플 설정을** 페이지에는이 기술을 보여 줍니다. 이 페이지를 자주 같이 ViewModel에 정의 된 응용 프로그램 설정의 간단한 구현을 나타냅니다 `SampleSettingsViewModel` 파일:

```csharp
public class SampleSettingsViewModel : INotifyPropertyChanged
{
    string name;
    DateTime birthDate;
    bool codesInCSharp;
    double numberOfCopies;
    NamedColor backgroundNamedColor;

    public event PropertyChangedEventHandler PropertyChanged;

    public SampleSettingsViewModel(IDictionary<string, object> dictionary)
    {
        Name = GetDictionaryEntry<string>(dictionary, "Name");
        BirthDate = GetDictionaryEntry(dictionary, "BirthDate", new DateTime(1980, 1, 1));
        CodesInCSharp = GetDictionaryEntry<bool>(dictionary, "CodesInCSharp");
        NumberOfCopies = GetDictionaryEntry(dictionary, "NumberOfCopies", 1.0);
        BackgroundNamedColor = NamedColor.Find(GetDictionaryEntry(dictionary, "BackgroundNamedColor", "White"));
    }

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public DateTime BirthDate
    {
        set { SetProperty(ref birthDate, value); }
        get { return birthDate; }
    }

    public bool CodesInCSharp
    {
        set { SetProperty(ref codesInCSharp, value); }
        get { return codesInCSharp; }
    }

    public double NumberOfCopies
    {
        set { SetProperty(ref numberOfCopies, value); }
        get { return numberOfCopies; }
    }

    public NamedColor BackgroundNamedColor
    {
        set
        {
            if (SetProperty(ref backgroundNamedColor, value))
            {
                OnPropertyChanged("BackgroundColor");
            }
        }
        get { return backgroundNamedColor; }
    }

    public Color BackgroundColor
    {
        get { return BackgroundNamedColor?.Color ?? Color.White; }
    }

    public void SaveState(IDictionary<string, object> dictionary)
    {
        dictionary["Name"] = Name;
        dictionary["BirthDate"] = BirthDate;
        dictionary["CodesInCSharp"] = CodesInCSharp;
        dictionary["NumberOfCopies"] = NumberOfCopies;
        dictionary["BackgroundNamedColor"] = BackgroundNamedColor.Name;
    }

    T GetDictionaryEntry<T>(IDictionary<string, object> dictionary, string key, T defaultValue = default(T))
    {
        return dictionary.ContainsKey(key) ? (T)dictionary[key] : defaultValue;
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

각 응용 프로그램 설정 라는 메서드에서 Xamarin.Forms 속성 사전에 저장 되는 속성은 `SaveState` 생성자에는 사전에서 로드 합니다. 클래스의 아래쪽에 있는 두 가지 방법이 ViewModels를 간소화 하 고 있도록 오류 발생 가능성이 적습니다. `OnPropertyChanged` 맨 아래에서 메서드가 호출 속성으로 설정 되는 선택적 매개 변수입니다. 이 문자열 속성의 이름을 지정할 때 맞춤법 오류를 방지 합니다.

합니다 `SetProperty` 클래스의 메서드는 더: 필드로 저장 된 값을 사용 하 여 속성에 설정 되는 값과 비교 하 고 호출 `OnPropertyChanged` 두 값이 같은 경우입니다.

`SampleSettingsViewModel` 배경색에 두 속성을 정의 하는 클래스: 합니다 `BackgroundNamedColor` 형식의 속성은 `NamedColor`는 클래스에도 포함 되어는 **DataBindingDemos** 솔루션. `BackgroundColor` 형식의 속성은 `Color`에서 가져온 및는 `Color` 의 속성을 `NamedColor` 개체.

합니다 `NamedColor` 클래스.NET 리플렉션을 사용 하 여 Xamarin.Forms에 모든 정적 공용 필드가 열거 `Color` 구조 및 정적에서 액세스할 수 있는 컬렉션의 이름으로 저장 하기 `All` 속성:

```csharp
public class NamedColor : IEquatable<NamedColor>, IComparable<NamedColor>
{
    // Instance members
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    public bool Equals(NamedColor other)
    {
        return Name.Equals(other.Name);
    }

    public int CompareTo(NamedColor other)
    {
        return Name.CompareTo(other.Name);
    }

    // Static members
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof(Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                                (int)(255 * color.R),
                                                (int)(255 * color.G),
                                                (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        all.Sort();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }

    public static NamedColor Find(string name)
    {
        return ((List<NamedColor>)All).Find(nc => nc.Name == name);
    }

    public static string GetNearestColorName(Color color)
    {
        double shortestDistance = 1000;
        NamedColor closestColor = null;

        foreach (NamedColor namedColor in NamedColor.All)
        {
            double distance = Math.Sqrt(Math.Pow(color.R - namedColor.Color.R, 2) +
                                        Math.Pow(color.G - namedColor.Color.G, 2) +
                                        Math.Pow(color.B - namedColor.Color.B, 2));

            if (distance < shortestDistance)
            {
                shortestDistance = distance;
                closestColor = namedColor;
            }
        }
        return closestColor.Name;
    }
}
```

합니다 `App` 클래스를 **DataBindingDemos** 라는 속성을 정의 하는 프로젝트 `Settings` 형식의 `SampleSettingsViewModel`합니다. 이 속성은 초기화 때 합니다 `App` 클래스가 인스턴스화되면 및 `SaveState` 메서드를 호출한 경우는 `OnSleep` 메서드는:

```csharp
public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        Settings = new SampleSettingsViewModel(Current.Properties);

        MainPage = new NavigationPage(new MainPage());
    }

    public SampleSettingsViewModel Settings { private set; get; }

    protected override void OnStart()
    {
        // Handle when your app starts
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Settings.SaveState(Current.Properties);
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
    }
}
```

응용 프로그램 수명 주기 메서드에 대 한 자세한 내용은 문서 참조 [ **앱 수명 주기**](~/xamarin-forms/app-fundamentals/app-lifecycle.md)합니다.

다른 거의 모든 것에서 처리 되는 **SampleSettingsPage.xaml** 파일입니다. `BindingContext` 페이지의 사용 하도록 설정 됩니다는 `Binding` 태그 확장: 바인딩 소스는 정적 `Application.Current` 인스턴스 속성의는 `App` 프로젝트에서 클래스 및 `Path` 로 설정 됩니다는 `Settings` 속성은 `SampleSettingsViewModel` 개체:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SampleSettingsPage"
             Title="Sample Settings"
             BindingContext="{Binding Source={x:Static Application.Current},
                                      Path=Settings}">

    <StackLayout BackgroundColor="{Binding BackgroundColor}"
                 Padding="10"
                 Spacing="10">

        <StackLayout Orientation="Horizontal">
            <Label Text="Name: "
                   VerticalOptions="Center" />

            <Entry Text="{Binding Name}"
                   Placeholder="your name"
                   HorizontalOptions="FillAndExpand"
                   VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Birth Date: "
                   VerticalOptions="Center" />

            <DatePicker Date="{Binding BirthDate}"
                        HorizontalOptions="FillAndExpand"
                        VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Do you code in C#? "
                   VerticalOptions="Center" />

            <Switch IsToggled="{Binding CodesInCSharp}"
                    VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Number of Copies: "
                   VerticalOptions="Center" />

            <Stepper Value="{Binding NumberOfCopies}"
                     VerticalOptions="Center" />

            <Label Text="{Binding NumberOfCopies}"
                   VerticalOptions="Center" />
        </StackLayout>

        <Label Text="Background Color:" />

        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
                  VerticalOptions="FillAndExpand"
                  RowHeight="40">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     HeightRequest="32"
                                     WidthRequest="32"
                                     VerticalOptions="Center" />

                            <Label Text="{Binding FriendlyName}"
                                   FontSize="24"
                                   VerticalOptions="Center" />
                        </StackLayout>                        
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

페이지의 모든 자식을 바인딩 컨텍스트를 상속합니다. 속성에이 페이지에 대 한 다른 바인딩 대부분 `SampleSettingsViewModel`합니다. `BackgroundColor` 속성을 설정 하는 `BackgroundColor` 의 속성을 `StackLayout`, 및 `Entry`, `DatePicker`, `Switch`, 및 `Stepper` 속성 모든 ViewModel의 다른 속성에 바인딩된.

`ItemsSource` 의 속성을 `ListView` 정적으로 설정 되어 `NamedColor.All` 속성입니다. 그러면 채워집니다 합니다 `ListView` 모든는 `NamedColor` 인스턴스. 각 항목에 대 한 합니다 `ListView`, 항목에 대 한 바인딩 컨텍스트를로 `NamedColor` 개체입니다. `BoxView` 및 `Label` 에 `ViewCell` 속성에 바인딩된 `NamedColor`합니다.

`SelectedItem` 의 속성을 `ListView` 형식입니다 `NamedColor`에 바인딩됩니다를 `BackgroundNamedColor` 속성의 `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

에 대 한 기본 바인딩 모드 `SelectedItem` 는 `OneWayToSource`, 선택된 된 항목의 ViewModel 속성을 설정 하는 합니다. 합니다 `TwoWay` 모드에서는 `SelectedItem` ViewModel에서 초기화 합니다.

그러나 경우는 `SelectedItem` 이런 방식이으로 설정 됩니다는 `ListView` 선택한 항목을 표시 하려면 자동으로 스크롤되지 않습니다. 필요한 코드 숨김 파일에 코드를 조금 같습니다.

```csharp
public partial class SampleSettingsPage : ContentPage
{
    public SampleSettingsPage()
    {
        InitializeComponent();

        if (colorListView.SelectedItem != null)
        {
            colorListView.ScrollTo(colorListView.SelectedItem,
                                   ScrollToPosition.MakeVisible,
                                   false);
        }
    }
}
```

왼쪽에 있는 iOS 스크린 샷을 처음 실행할 때 프로그램을 보여 줍니다. 생성자 `SampleSettingsViewModel` 초기화의 배경색을 흰색으로 및에서 선택한 항목에는 `ListView`:

[![예제 설정](binding-mode-images/samplesettings-small.png "샘플 설정")](binding-mode-images/samplesettings-large.png#lightbox "설정 샘플")

다른 두 개의 스크린샷에 표시 설정이 변경된 합니다. 이 페이지를 사용 하 여 시험해 보려는 경우 프로그램이 절전 모드로 전환 하거나 장치 또는 실행 중인 에뮬레이터에서 종료를 배치 해야 합니다. Visual Studio 디버거에서 프로그램 종료를 발생 하지 것입니다는 `OnSleep` 의 재정의 `App` 클래스를 호출할 수 있습니다.

다음 문서에서 지정 하는 방법을 볼 [ **문자열 서식 지정** ](string-formatting.md) 에서 설정 되는 데이터 바인딩의 합니다 `Text` 속성 `Label`합니다.


## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
