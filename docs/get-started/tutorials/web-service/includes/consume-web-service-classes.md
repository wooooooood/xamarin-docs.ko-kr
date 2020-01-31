---
ms.openlocfilehash: c92a97b336e89214bbd95021ad8fb9a56f64cc8c
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2020
ms.locfileid: "67659860"
---
이 연습에서는 `RestService` 클래스를 사용하는 사용자 인터페이스를 만듭니다. 그러면 [OpenWeatherMap](https://openweathermap.org/) 웹 API에서 데이터를 검색하게 됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **솔루션 탐색기**의 **WebServiceTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="0.4*" />
                <ColumnDefinition Width="0.6*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
            </Grid.RowDefinitions>
            <Entry x:Name="cityEntry"
                   Grid.ColumnSpan="2"
                   Text="Seattle" />
            <Button Grid.ColumnSpan="2"
                    Grid.Row="1"
                    Text="Get Weather"
                    Clicked="OnButtonClicked" />                
            <Label Grid.Row="2"
                   Text="Location:" />
            <Label Grid.Row="2"
                   Grid.Column="1"
                   Text="{Binding Title}" />            
            <Label Grid.Row="3"
                   Text="Temperature:" />
            <Label Grid.Row="3"
                   Grid.Column="1"
                   Text="{Binding Main.Temperature}" />            
            <Label Grid.Row="4"
                   Text="Wind Speed:" />
            <Label Grid.Row="4"
                   Grid.Column="1"
                   Text="{Binding Wind.Speed}" />            
            <Label Grid.Row="5"
                   Text="Humidity:" />
            <Label Grid.Row="5"
                   Grid.Column="1"
                   Text="{Binding Main.Humidity}" />            
            <Label Grid.Row="6"
                   Text="Visibility:" />
            <Label Grid.Row="6"
                   Grid.Column="1"
                   Text="{Binding Weather[0].Visibility}" />
        </Grid>
    </ContentPage>
    ```

    이 코드는 [`Grid`](xref:Xamarin.Forms.Grid)에서 [`Entry`](xref:Xamarin.Forms.Entry), [`Button`](xref:Xamarin.Forms.Button) 및 일련의 [`Label`](xref:Xamarin.Forms.Label) 인스턴스로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. `Entry`는 해당하는 [`Text`](xref:Xamarin.Forms.Entry.Text) 속성을 설정하여 "Seattle"로 미리 채워져 있습니다. `Button`은 해당하는 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트를 다음 단계에서 만들 `OnButtonClicked`라는 이벤트 처리기로 설정합니다. `Label` 인스턴스의 절반은 `WeatherData` 속성에 대한 나머지 인스턴스 데이터 바인딩을 사용하여 정적 텍스트를 표시합니다. 런타임 시 데이터 바인딩을 사용하는 `Label` 인스턴스는 해당 바인딩 식에서 사용할 `WeatherData` 개체에 해당하는 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 속성을 살펴봅니다. 데이터 바인딩에 대한 자세한 내용은 [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)을 참조하세요.

    또한 [`Entry`](xref:Xamarin.Forms.Entry)에는 `x:Name` 특성으로 지정된 이름이 있습니다. 이렇게 하면 코드 숨김 파일이 할당된 이름을 사용하여 개체에 액세스할 수 있습니다.

1. **솔루션 탐색기**의 **WebServiceTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace WebServiceTutorial
    {
        public partial class MainPage : ContentPage
        {
            RestService _restService;

            public MainPage()
            {
                InitializeComponent();
                _restService = new RestService();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                if (!string.IsNullOrWhiteSpace(cityEntry.Text))
                {
                    WeatherData weatherData = await _restService.GetWeatherDataAsync(GenerateRequestUri(Constants.OpenWeatherMapEndpoint));
                    BindingContext = weatherData;
                }
            }

            string GenerateRequestUri(string endpoint)
            {
                string requestUri = endpoint;
                requestUri += $"?q={cityEntry.Text}";
                requestUri += "&units=imperial"; // or units=metric
                requestUri += $"&APPID={Constants.OpenWeatherMapAPIKey}";
                return requestUri;
            }
        }
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button)을 누를 때 실행되는 `OnButtonClicked` 메서드는 `RestService.GetWeatherDataAsync` 메서드를 호출하여 [`Entry`](xref:Xamarin.Forms.Entry)에 이름이 입력된 도시에 대한 날씨 데이터를 검색합니다. `GetWeatherDataAsync` 메서드에는 호출되는 웹 API의 URI를 나타내는 `string` 인수가 필요하고, 이 인수는 `GenerateRequestUri` 메서드에 의해 생성됩니다. 이 메서드는 `OpenWeatherMapEndpoint` 상수에 저장된 엔드포인트 주소를 사용하고 여기에 다음을 지정하는 쿼리 매개 변수를 추가합니다.

    - 날씨 데이터를 요청하는 도시
    - 날씨 데이터를 반환할 단위
    - 개인 API 키

    요청된 날씨 데이터를 검색하면 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)가 `WeatherData` 개체로 설정됩니다. `BindingContext` 속성에 대한 자세한 내용은 [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md) 가이드의 [바인딩 컨텍스트로 바인딩](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context)을 참조하세요.

    > [!IMPORTANT]
    > [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 속성은 시각적 트리를 통해 상속됩니다. 따라서 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체에 설정되어 있기 때문에, `ContentPage`의 자식 개체는 [`Label`](xref:Xamarin.Forms.Label) 인스턴스를 포함한 해당 값을 상속합니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Button`](xref:Xamarin.Forms.Button)을 눌러 시애틀의 현재 날씨 데이터를 검색합니다.

    [![iOS 및 Android에서 시애틀 날씨 데이터 스크린샷](../images/consume-web-service.png "시애틀 날씨 데이터")](../images/consume-web-service-large.png#lightbox "시애틀 날씨 데이터")

    > [!IMPORTANT]
    > 개인 OpenWeatherMap API 키는 `Constants` 클래스의 `OpenWeatherMapAPIKey` 상수 값으로 설정되어야 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac용 Visual Studio](#tab/vsmac)

1. **Solution Pad**의 **WebServiceTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="0.4*" />
                <ColumnDefinition Width="0.6*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
            </Grid.RowDefinitions>
            <Entry x:Name="cityEntry"
                   Grid.ColumnSpan="2"
                   Text="Seattle" />
            <Button Grid.ColumnSpan="2"
                    Grid.Row="1"
                    Text="Get Weather"
                    Clicked="OnButtonClicked" />                
            <Label Grid.Row="2"
                   Text="Location:" />
            <Label Grid.Row="2"
                   Grid.Column="1"
                   Text="{Binding Title}" />            
            <Label Grid.Row="3"
                   Text="Temperature:" />
            <Label Grid.Row="3"
                   Grid.Column="1"
                   Text="{Binding Main.Temperature}" />            
            <Label Grid.Row="4"
                   Text="Wind Speed:" />
            <Label Grid.Row="4"
                   Grid.Column="1"
                   Text="{Binding Wind.Speed}" />            
            <Label Grid.Row="5"
                   Text="Humidity:" />
            <Label Grid.Row="5"
                   Grid.Column="1"
                   Text="{Binding Main.Humidity}" />            
            <Label Grid.Row="6"
                   Text="Visibility:" />
            <Label Grid.Row="6"
                   Grid.Column="1"
                   Text="{Binding Weather[0].Visibility}" />
        </Grid>
    </ContentPage>
    ```

    이 코드는 [`Grid`](xref:Xamarin.Forms.Grid)에서 [`Entry`](xref:Xamarin.Forms.Entry), [`Button`](xref:Xamarin.Forms.Button) 및 일련의 [`Label`](xref:Xamarin.Forms.Label) 인스턴스로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. `Entry`는 해당하는 [`Text`](xref:Xamarin.Forms.Entry.Text) 속성을 설정하여 "Seattle"로 미리 채워져 있습니다. `Button`은 해당하는 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트를 다음 단계에서 만들 `OnButtonClicked`라는 이벤트 처리기로 설정합니다. `Label` 인스턴스의 절반은 `WeatherData` 속성에 대한 나머지 인스턴스 데이터 바인딩을 사용하여 정적 텍스트를 표시합니다. 런타임 시 데이터 바인딩을 사용하는 `Label` 인스턴스는 해당 바인딩 식에서 사용할 `WeatherData` 개체에 해당하는 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 속성을 살펴봅니다. 데이터 바인딩에 대한 자세한 내용은 [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)을 참조하세요.

    또한 [`Entry`](xref:Xamarin.Forms.Entry)에는 `x:Name` 특성으로 지정된 이름이 있습니다. 이렇게 하면 코드 숨김 파일이 할당된 이름을 사용하여 개체에 액세스할 수 있습니다.

    Xamarin.Forms에서 REST 기반 웹 서비스를 사용하는 방법에 대한 자세한 내용은 [RESTful Web Service 사용(가이드)](~/xamarin-forms/data-cloud/web-services/rest.md)을 참조하세요.

1. **Solution Pad**의 **WebServiceTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace WebServiceTutorial
    {
        public partial class MainPage : ContentPage
        {
            RestService _restService;

            public MainPage()
            {
                InitializeComponent();
                _restService = new RestService();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                if (!string.IsNullOrWhiteSpace(cityEntry.Text))
                {
                    WeatherData weatherData = await _restService.GetWeatherDataAsync(GenerateRequestUri(Constants.OpenWeatherMapEndpoint));
                    BindingContext = weatherData;
                }
            }

            string GenerateRequestUri(string endpoint)
            {
                string requestUri = endpoint;
                requestUri += $"?q={cityEntry.Text}";
                requestUri += "&units=imperial"; // or units=metric
                requestUri += $"&APPID={Constants.OpenWeatherMapAPIKey}";
                return requestUri;
            }
        }
    }
    ```

    [`Button`](xref:Xamarin.Forms.Button)을 누를 때 실행되는 `OnButtonClicked` 메서드는 `RestService.GetWeatherDataAsync` 메서드를 호출하여 [`Entry`](xref:Xamarin.Forms.Entry)에 이름이 입력된 도시에 대한 날씨 데이터를 검색합니다. `GetWeatherDataAsync` 메서드에는 호출되는 웹 API의 URI를 나타내는 `string` 인수가 필요하고, 이 인수는 `GenerateRequestUri` 메서드에 의해 생성됩니다. 이 메서드는 `OpenWeatherMapEndpoint` 상수에 저장된 엔드포인트 주소를 사용하고 여기에 다음을 지정하는 쿼리 매개 변수를 추가합니다.

    - 날씨 데이터를 요청하는 도시
    - 날씨 데이터를 반환할 단위
    - 개인 API 키

    요청된 날씨 데이터를 검색하면 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)가 `WeatherData` 개체로 설정됩니다. `BindingContext` 속성에 대한 자세한 내용은 [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md) 가이드의 [바인딩 컨텍스트로 바인딩](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context)을 참조하세요.

    > [!IMPORTANT]
    > [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 속성은 시각적 트리를 통해 상속됩니다. 따라서 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체에 설정되어 있기 때문에, `ContentPage`의 자식 개체는 [`Label`](xref:Xamarin.Forms.Label) 인스턴스를 포함한 해당 값을 상속합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. [`Button`](xref:Xamarin.Forms.Button)을 눌러 시애틀의 현재 날씨 데이터를 검색합니다.

    [![iOS 및 Android에서 시애틀 날씨 데이터의 스크린샷](../images/consume-web-service.png "시애틀 날씨 데이터")](../images/consume-web-service-large.png#lightbox "시애틀 날씨 데이터")

    > [!IMPORTANT]
    > 개인 OpenWeatherMap API 키는 `Constants` 클래스의 `OpenWeatherMapAPIKey` 상수 값으로 설정되어야 합니다.

    Xamarin.Forms에서 REST 기반 웹 서비스를 사용하는 방법에 대한 자세한 내용은 [RESTful Web Service 사용(가이드)](~/xamarin-forms/data-cloud/web-services/rest.md)을 참조하세요.
