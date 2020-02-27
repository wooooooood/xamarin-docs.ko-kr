---
title: Xamarin.Forms 바인딩 모드
description: 이 문서에서는 BindingMode 열거형의 멤버를 통해 지정되는 바인딩 모드를 사용하여 원본과 대상 간의 정보 흐름을 제어하는 방법을 설명합니다. 바인딩할 수 있는 모든 속성에는 기본 바인딩 모드가 있으며, 속성이 데이터 바인딩 대상일 때 적용되는 모드를 나타냅니다.
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/01/2018
ms.openlocfilehash: 3bf1ab647faa4b6c4735585ddfeaeb704d7d3f41
ms.sourcegitcommit: 6d86aac422d6ce2131930d18ada161d117c8c61b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/24/2020
ms.locfileid: "77567080"
---
# <a name="xamarinforms-binding-mode"></a>Xamarin.Forms 바인딩 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

[이전 문서](basic-bindings.md)에서는 **Alternative Code Binding**(대체 코드 바인딩)과 **Alternative XAML Binding**(대체 XAML 바인딩) 페이지에 `Scale` 속성이 있는 `Label`이 `Slider`의 `Value`에 바인딩되는 것을 설명했습니다. `Slider` 초기 값이 0이라서 `Label`의 `Scale` 속성이 1이 아닌 0으로 설정되어 `Label`이 사라졌습니다.

[**DataBindingDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos) 샘플의 **Reverse Binding**(역방향 바인딩) 페이지는 이전 문서의 프로그램과 유사하며, 데이터 바인딩이 `Label`에 정의되지 않고 `Slider`에 정의되는 것만 다릅니다.

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

처음에는 반대 방향처럼 보일 수 있습니다. 이제 `Label`은 데이터 바인딩 소스이고 `Slider`는 대상입니다. 바인딩은 `Label`의 `Opacity` 속성을 참조하며, 기본값은 1일입니다.

예상대로 `Slider`는 `Label`의 초기 `Opacity` 값에서 1로 초기화됩니다. 왼쪽의 iOS 스크린샷이 이 경우를 보여줍니다.

[![역방향 바인딩](binding-mode-images/reversebinding-small.png "역방향 바인딩")](binding-mode-images/reversebinding-large.png#lightbox "역방향 바인딩")

하지만 놀랍게도 `Slider`은(는) Android 스크린샷에 표시된 것처럼 계속 작동합니다. 이를 통해 알 수 있는 것은 데이터 바인딩은 `Label`보다는 `Slider`가 바인딩 대상일 때 더 잘 작동하며 그 이유는 초기화가 예상대로 작동하기 때문이라는 점입니다.

**Reverse Binding**(역방향 바인딩) 샘플과 이전 샘플의 차이는 *바인딩 모드*입니다.

## <a name="the-default-binding-mode"></a>기본 바인딩 모드

바인딩 모드는 [`BindingMode`](xref:Xamarin.Forms.BindingMode) 열거형의 멤버를 통해 지정됩니다.

- [`Default`](xref:Xamarin.Forms.BindingMode.Default)
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) &ndash; 데이터가 원본과 대상 사이에서 양방향으로 이동
- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) &ndash; 데이터가 원본에서 대상으로 이동
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; 데이터가 대상에서 원본으로 이동
- `BindingContext`이(가) 변경되는 경우에만 [`OneTime`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; 데이터가 원본에서 대상으로 이동(Xamarin.Forms 3.0의 새로운 기능)

바인딩할 수 있는 모든 속성에는 바인딩할 수 있는 속성이 생성될 때 설정되는 기본 바인딩 모드가 있으며, `BindableProperty` 개체의 [`DefaultBindingMode`](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) 속성에 제공됩니다. 기본 바인딩 모드는 속성이 데이터 바인딩 대상일 때 적용되는 모드를 나타냅니다.

`Rotation`, `Scale` 및 `Opacity`와 같은 대부분의 속성에 대한 기본 바인딩 모드는 `OneWay`입니다. 이러한 속성이 데이터 바인딩 대상인 경우에는 대상 속성이 원본에서 설정됩니다.

하지만 `Slider`의 `Value` 속성에 대한 기본 바인딩 모드는 `TwoWay`입니다. 즉, `Value` 속성이 데이터 바인딩 대상이면 대상이 원본에서(일반적으로) 설정되지만 원본 역시 대상에서 설정됩니다. 따라서 `Slider`가 초기 `Opacity` 값에서 설정될 수 있습니다.

양방향 바인딩은 무한 루프를 만드는 것처럼 보일 수도 있지만 그런 상황은 발생하지 않습니다. 바인딩할 수 있는 속성은 속성이 실제로 변경되는 경우가 아니면 속성의 변화를 신호로 알리지 않습니다. 이를 통해 무한 루프가 방지됩니다.

### <a name="two-way-bindings"></a>양방향 바인딩

바인딩할 수 있는 대부분의 속성에는 `OneWay`라는 기본 바인딩 모드가 있지만 다음 속성의 기본 바인딩 모드는 `TwoWay`입니다.

- `DatePicker`의 `Date` 속성
- `Editor`, `Entry`, `SearchBar`, `EntryCell`의 `Text` 속성
- `ListView`의 `IsRefreshing` 속성
- `MultiPage`의 `SelectedItem` 속성
- `Picker`의 `SelectedIndex` 및 `SelectedItem` 속성
- `Slider`, `Stepper`의 `Value` 속성
- `Switch`의 `IsToggled` 속성
- `SwitchCell`의 `On` 속성
- `TimePicker`의 `Time` 속성

특정 속성이 `TwoWay`로 정의되는 데에는 매우 합당한 이유가 있습니다.

데이터 바인딩이 MVVM(Model-View-ViewModel) 애플리케이션 아키텍처에 사용되는 경우에는 ViewModel 클래스가 데이터 바인딩 소스이고 View(`Slider`와 같은 뷰로 구성됨)는 데이터 바인딩 대상이 됩니다. MVVM 바인딩은 이전 샘플의 바인딩보다 **Reverse Binding**(역방향 바인딩) 샘플과 더 유사합니다. 페이지의 각 뷰를 ViewModel의 해당 속성 값으로 초기화할 가능성이 높지만 뷰의 변경 내용은 ViewModel 속성에도 영향을 미쳐야 합니다.

기본 바인딩 모드가 `TwoWay`인 속성이 MVVM 시나리오에서 가장 많이 사용되는 속성입니다.

### <a name="one-way-to-source-bindings"></a>One-Way-to-Source 바인딩

바인딩할 수 있는 읽기 전용 속성의 기본 바인딩 모드는 `OneWayToSource`입니다. 기본 바인딩 모드로 `OneWayToSource`가 사용되는 바인딩할 수 있는 읽기/쓰기 속성은 하나뿐입니다.

- `ListView`의 `SelectedItem` 속성

`SelectedItem` 속성에 바인딩을 하면 바인딩 소스가 설정되어야 하기 때문입니다. 이 문서의 뒷부분에 나오는 예제에서 이 동작이 재정의됩니다.

### <a name="one-time-bindings"></a>One-Time 바인딩

`Entry`의 `IsTextPredictionEnabled` 속성을 포함하여 몇 가지 속성은 `OneTime`의 기본 바인딩 모드가 있습니다.

바인딩 모드가 `OneTime`인 대상 속성은 바인딩 컨텍스트가 변경되는 경우에만 업데이트됩니다. 이러한 대상 속성의 바인딩은 원본 속성의 변경 내용을 모니터링할 필요가 없기 때문에 바인딩 인프라가 간소화됩니다.

## <a name="viewmodels-and-property-change-notifications"></a>ViewModels 및 Property-Change 알림

**Simple Color Selector**(간단한 색 선택기) 페이지는 간단한 ViewModel 사용을 보여줍니다. 데이터 바인딩을 사용하여 사용자가 색상, 채도 및 광도에 대한 세 가지 `Slider` 요소를 사용하여 색상을 선택할 수 있습니다.

ViewModel은 데이터 바인딩 소스입니다. ViewModel 은 바인딩할 수 있는 속성을 정의하지는 않지만 속성 값이 변경되면 바인딩 인프라에 알릴 수 있는 알림 메커니즘을 구현합니다.  이 알림 메커니즘은 [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) 인터페이스이며 [`PropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged)라는 단일 이벤트를 정의합니다. 공용 속성 중 하나의 값이 변경되면 이 인터페이스를 구현하는 클래스가 이벤트를 발생시킵니다. 속성이 전혀 변경되지 않으면 이벤트가 실행될 필요가 없습니다. (`INotifyPropertyChanged` 인터페이스도 `BindableObject`에 의해 구현되며 `PropertyChanged` 이벤트는 바인딩할 수 있는 속성의 값이 변하면 실행됩니다.)

`HslColorViewModel` 클래스는 5개 속성을 정의합니다. `Hue`, `Saturation`, `Luminosity` 및 `Color` 속성은 서로 관련됩니다. 세 가지 색 구성 요소 중 하나라도 값이 변경되면 `Color` 속성이 다시 계산되고 네 가지 속성 모두에 대해 `PropertyChanged` 이벤트가 실행됩니다.

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

`Color` 속성이 변경되면 `NamedColor` 클래스(**DataBindingDemos** 솔루션에도 포함되어 있음)의 정적 `GetNearestColorName` 메서드가 가장 가까이 명명된 색을 가져다가 `Name` 속성을 설정합니다. `Name` 속성에는 비공개 `set` 접근자가 있기 때문에 클래스 외부에서는 설정할 수 없습니다.

ViewModel이 바인딩 소스로 설정되면 바인딩 인프라는 `PropertyChanged` 이벤트에 처리기를 연결합니다. 이런 방식으로 속성에 대한 변경 내용을 바인딩에 알릴 수 있으며 그런 다음, 변경된 값에서 대상 속성을 설정할 수 있습니다.

하지만 대상 속성(또는 대상 속성의 `Binding` 정의)에 `OneTime`의 `BindingMode`가 있으면 바인딩 인프라가 `PropertyChanged` 이벤트에 대한 처리기를 연결할 필요가 없습니다. 대상 속성은 `BindingContext`가 변경되는 경우에만 업데이트됩니다. 원본 속성 자체가 변경되는 경우에는 업데이트되지 않습니다.

**Simple Color Selector**(간단한 색 선택기) XAML 파일은 페이지의 리소스 사전에서 `HslColorViewModel`을 인스턴스화하고 `Color` 속성을 초기화합니다. `Grid`의 `BindingContext` 속성은 해당 리소스를 참조하는 `StaticResource` 바인딩 확장으로 설정됩니다.

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

`BoxView`, `Label` 및 세 가지 `Slider`뷰는 `Grid`에서 바인딩 컨텍스트를 상속받습니다. 이러한 뷰는 모두 ViewModel의 원본 속성을 참조하는 바인딩 대상입니다. `BoxView`의 `Color`속성과 `Label`의 `Text` 속성의 경우 데이터 바인딩은 `OneWay`입니다. 뷰의 속성은 ViewModel의 속성에서 설정됩니다.

단 `Slider`의 `Value` 속성은 `TwoWay`입니다. 이렇게 하면 각 `Slider`을 ViewModel에서 설정하고 각 `Slider`에서 ViewModel을 설정할 수 있습니다.

프로그램을 처음 실행하는 경우 `BoxView`, `Label` 및 세 가지 `Slider` 요소는 모두 ViewModel을 인스턴스화했을 때 설정된 초기 `Color` 설정을 기반으로 ViewModel에서 설정됩니다. 왼쪽의 iOS 스크린샷이 이 경우를 보여줍니다.

[![단순 색 선택기](binding-mode-images/simplecolorselector-small.png "단순 색 선택기")](binding-mode-images/simplecolorselector-large.png#lightbox "단순 색 선택기")

슬라이더를 조작하면 그에 따라 `BoxView` 및 `Label`이(가) Android 스크린샷에 설명된 것처럼 업데이트됩니다.

리소스 사전에서 ViewModel을 인스턴스화하는 것이 일반적인 방법 중 하나입니다. `BindingContext` 속성에 대한 속성 요소 태그 내에서 ViewModel을 인스턴스화하는 것도 가능합니다. **Simple Color Selector**(간단한 색 선택기) XAML 파일에서 `HslColorViewModel`을 리소스 사전에서 제거하고 다음과 같이 `Grid`의 `BindingContext` 속성으로 설정합니다.

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>

    ···

</Grid>
```

바인딩 컨텍스트는 다양한 방식으로 설정할 수 있습니다. 코드 숨김 파일이 ViewModel을 인스턴스화하고 페이지의 `BindingContext` 속성으로 설정하는 경우도 있습니다. 이 모두가 유효한 방식입니다.

## <a name="overriding-the-binding-mode"></a>바인딩 모드 재정의

대상 속성의 기본 바인딩 모드가 특정 데이터 바인딩에 적합하지 않으면 `Binding`의 [`Mode`](xref:Xamarin.Forms.BindingBase.Mode) 속성(또는 `Binding` 태그 확장의 [`Mode`](xref:Xamarin.Forms.Xaml.BindingExtension.Mode) 속성)을 `BindingMode` 열거형의 멤버 중 하나로 설정하여 재정의할 수 있습니다.

단, `Mode` 속성을 `TwoWay`로 설정해도 항상 예상대로 작동하지는 않습니다. 예를 들어, 바인딩 정의에 `TwoWay`가 포함되도록 **Alternative XAML Binding**(대체 XAML 바인딩) XAML 파일을 수정해 보겠습니다.

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

`Slider`가 `Scale` 속성의 초기 값인 1로 초기화될 것으로 예상할 수도 있지만 그렇게 되지 않습니다. `TwoWay` 바인딩이 초기화되면 먼저 원본을 통해 대상이 설정됩니다. 즉, `Scale` 속성이 `Slider` 기본값인 0으로 설정됩니다. `TwoWay` 바인딩이 `Slider`에 설정되면 `Slider`는 원본을 통해 초기 설정됩니다.

**Alternative XAML Binding**(대체 XAML 바인딩) 샘플에서 바인딩 모드를 `OneWayToSource`로 설정할 수 있습니다.

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

이제 `Slider`가 1(`Scale`의 기본값)로 초기화되지만 `Slider`를 조작해도 `Scale` 속성에 영향을 주지 않기 때문에 별로 유용하지 않습니다.

> [!NOTE]
> 또한 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 클래스는 `VisualElement`의 크기를 가로 및 세로 방향으로 다르게 확장할 수 있는 [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) 및 [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) 속성도 정의합니다.

`TwoWay`를 사용하여 기본 바인딩 모드를 재정의하는 매우 유용한 애플리케이션은 `ListView`의 `SelectedItem` 속성과 관련이 있습니다. 기본 바인딩 모드는 `OneWayToSource`입니다. 데이터 바인딩이 ViewModel의 원본 속성을 참조하도록 `SelectedItem` 속성에 설정되면 해당 원본 속성은 `ListView` 선택 항목에서 설정됩니다. 하지만 경우에 따라 `ListView`를 ViewModel에서 초기화해야 하는 경우도 있습니다.

**Sample Settings**(샘플 설정) 페이지는 이 기술을 보여줍니다. 이 페이지는 `SampleSettingsViewModel` 파일과 같이 ViewModel에 정의되는 경우가 많은 애플리케이션 설정의 간단한 구현을 나타냅니다.

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
        if (object.Equals(storage, value))
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

각 애플리케이션 설정은 `SaveState`라는 메서드로 Xamarin.Forms 속성 사전에 저장되고 이 사전에서 생성자로 로드되는 속성입니다. 클래스의 맨 아래에는 ViewModel을 간소화하고 오류를 줄이도록 도와주는 두 가지 메서드가 있습니다. 맨 아래 `OnPropertyChanged` 메서드에는 호출 속성으로 설정되는 선택적 매개 변수가 있습니다. 이것을 사용하면 속성 이름을 문자열로 지정할 때 맞춤법 오류가 방지됩니다.

클래스의 `SetProperty` 메서드는 훨씬 더 많은 작업을 수행합니다. 속성으로 설정되는 값을 필드로 저장되는 값과 비교하고 두 값이 같지 않을 때만 `OnPropertyChanged`를 호출합니다.

`SampleSettingsViewModel` 클래스는 배경색에 대한 두 가지 속성을 정의합니다. `BackgroundNamedColor` 속성은 `NamedColor` 형식이며, 이것은 **DataBindingDemos** 솔루션에도 포함되는 클래스입니다. `BackgroundColor` 속성은 `Color` 유형이며 `NamedColor` 개체의 `Color` 속성에서 가져옵니다.

`NamedColor` 클래스는 .NET 리플렉션을 사용하여 Xamarin.Forms `Color` 구조체의 정적인 공용 필드를 모두 열거하고 정적 `All` 속성에서 액세스할 수 있는 컬렉션에 이름을 사용하여 저장합니다.

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

**DataBindingDemos** 프로젝트의 `App` 클래스는 `SampleSettingsViewModel` 유형의 `Settings`라는 속성을 정의합니다. 이 속성은 `App` 클래스가 인스턴스화될 때 초기화되고 `SaveState` 메서드는 `OnSleep` 메서드가 호출될 때 호출됩니다.

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

애플리케이션 수명 주기 메서드에 대한 자세한 내용은 [**앱 수명 주기**](~/xamarin-forms/app-fundamentals/app-lifecycle.md) 문서를 참조하세요.

그 밖에 거의 모든 것은 **SampleSettingsPage.xaml** 파일에서 처리됩니다. 페이지의 `BindingContext`는 `Binding` 태그 확장을 사용하여 설정됩니다. 바인딩 소스는 프로젝트의 `App` 클래스 인스턴스인 정적 `Application.Current` 속성이며 `Path`는 `SampleSettingsViewModel` 개체인 `Settings` 속성으로 설정됩니다.

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

페이지의 모든 자식은 바인딩 컨텍스트를 상속받습니다. 이 페이지의 다른 바인딩 대부분은 `SampleSettingsViewModel`의 속성에 지정됩니다. `BackgroundColor` 속성은 `StackLayout`의 `BackgroundColor` 속성을 설정하는 데 사용되고 `Entry`, `DatePicker`, `Switch`, `Stepper` 속성은 모두 ViewModel의 다른 속성에 바인딩됩니다.

`ListView`의 `ItemsSource` 속성은 정적 `NamedColor.All` 속성으로 설정됩니다. 이것은 `ListView`를 모든 `NamedColor` 인스턴스로 채웁니다. `ListView`의 각 항목의 경우 해당 항목에 대한 바인딩 컨텍스트는 `NamedColor` 개체로 설정됩니다. `ViewCell`의 `BoxView`와 `Label`은 `NamedColor`의 속성에 바인딩됩니다.

`ListView`의 `SelectedItem` 속성은 `NamedColor` 유형이며 `SampleSettingsViewModel`의 `BackgroundNamedColor` 속성에 바인딩됩니다.

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

`SelectedItem`의 기본 바인딩 모드는 `OneWayToSource`이며, 선택한 항목의 ViewModel 속성을 설정합니다. `TwoWay` 모드는 `SelectedItem`이 ViewModel에서 초기화될 수 있도록 합니다.

단, `SelectedItem`이 이렇게 설정되면 선택한 항목을 표시하도록 `ListView`가 자동으로 스크롤되지 않습니다. 코드 숨김 파일에 약간의 코드가 필요합니다.

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

왼쪽의 iOS 스크린샷은 프로그램이 처음 실행될 때를 보여줍니다. `SampleSettingsViewModel`의 생성자는 배경색을 흰색으로 초기화하며, 이것이 `ListView`에 선택된 항목입니다.

[![샘플 설정](binding-mode-images/samplesettings-small.png "샘플 설정")](binding-mode-images/samplesettings-large.png#lightbox "샘플 설정")

다른 스크린샷은 변경된 설정을 보여 줍니다. 이 페이지로 실험할 때는 프로그램을 절전 상태로 두거나 실행 중인 디바이스나 에뮬레이터에서 종료해야 합니다. Visual Studio 디버거에서 프로그램을 종료하면 `App` 클래스의 `OnSleep` 재정의가 호출되지 않습니다.

다음 문서에서는 `Label`의 `Text` 속성에 설정된 데이터 바인딩의 [**문자열 서식**](string-formatting.md)을 지정하는 방법을 알아봅니다.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Xamarin.Forms 서적의 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
