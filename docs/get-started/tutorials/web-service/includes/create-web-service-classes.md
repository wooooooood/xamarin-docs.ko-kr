웹 브라우저가 페이지를 검색하고 서버에 데이터를 보내는 데 사용하는 동일한 HTTP 동사를 사용하여 HTTP를 통해 REST 요청이 이루어집니다. 이 연습에서는 GET 동사를 사용하여 [OpenWeatherMap](https://openweathermap.org/) 웹 API에서 데이터를 검색하는 클래스를 만듭니다. 웹 API를 사용하여 지정된 위치에 대한 일기 예보 데이터를 검색할 수 있습니다. 이 웹 API를 사용하려면 API 키에 등록해야 합니다.

> [!div class="nextstepaction"]
> [API 키에 등록](https://home.openweathermap.org/users/sign_up)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **솔루션 탐색기**의 **WebServiceTutorial** 프로젝트에서 `Constants`라는 새 클래스를 프로젝트에 추가합니다. 그런 다음, **Constants.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public const string OpenWeatherMapEndpoint = "https://api.openweathermap.org/data/2.5/weather";
            public const string OpenWeatherMapAPIKey = "INSERT_API_KEY_HERE";
        }
    }
    ```

    이 코드는 두 개의 상수를 정의합니다. `OpenWeatherMapEndpoint` 상수는 웹 요청을 수행할 엔드포인트를 정의하고, `OpenWeatherMapAPIKey` 상수는 [OpenWeatherMap](https://openweathermap.org/) 서비스에 대한 개인 API 키를 정의합니다.

    > [!IMPORTANT]
    > 개인 OpenWeatherMap API 키를 `OpenWeatherMapAPIKey` 상수 값으로 설정해야 합니다.

1. **솔루션 탐색기**의 **WebServicesTutorial** 프로젝트에서 `WeatherData`라는 새 클래스를 프로젝트에 추가합니다. 그런 다음, **WeatherData.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class WeatherData
        {
            [JsonProperty("name")]
            public string Title { get; set; }

            [JsonProperty("weather")]
            public Weather[] Weather { get; set; }

            [JsonProperty("main")]
            public Main Main { get; set; }

            [JsonProperty("visibility")]
            public long Visibility { get; set; }

            [JsonProperty("wind")]
            public Wind Wind { get; set; }
        }

        public class Main
        {
            [JsonProperty("temp")]
            public double Temperature { get; set; }

            [JsonProperty("humidity")]
            public long Humidity { get; set; }
        }

        public class Weather
        {
            [JsonProperty("main")]
            public string Visibility { get; set; }
        }

        public class Wind
        {
            [JsonProperty("speed")]
            public double Speed { get; set; }
        }
    }
    ```

    이 코드는 웹 서비스에서 검색되는 JSON 데이터를 모델링하는 데 사용되는 네 가지 클래스를 정의합니다. 각 속성은 `JsonProperty` 특성을 사용하여 데코레이트되고 JSON 필드 이름을 포함합니다. Newtonsoft.Json은 모델 개체에 JSON 데이터를 deserialize할 때 CLR 속성에 대한 JSON 필드 이름의 매핑을 사용합니다.

    > [!NOTE]
    > 위의 클래스 정의는 단순화되고, 웹 서비스에서 검색되는 JSON 데이터를 완벽하게 모델링하지 않습니다. 전체 데이터 모델 예제는 [날씨 앱](https://developer.xamarin.com/samples/xamarin-forms/Weather/) 샘플을 참조하세요.

1. **솔루션 탐색기**의 **WebServiceTutorial** 프로젝트에서 `RestService`라는 새 클래스를 프로젝트에 추가합니다. 그런 다음, **RestService.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.Diagnostics;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class RestService
        {
            HttpClient _client;

            public RestService()
            {
                _client = new HttpClient();
            }

            public async Task<WeatherData> GetWeatherDataAsync(string uri)
            {
                WeatherData weatherData = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        weatherData = JsonConvert.DeserializeObject<WeatherData>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return weatherData;
            }
        }
    }
    ```

    이 코드는 [OpenWeatherMap](https://openweathermap.org/) 웹 API에서 지정된 위치에 대한 날씨 데이터를 검색하는 `GetWeatherDataAsync` 단일 메서드를 정의합니다. 이 메서드는 `HttpClient.GetAsync` 메서드를 사용하여 `uri` 인수에서 지정된 웹 API에 GET 요청을 보냅니다. 웹 API는 `HttpResponseMessage` 개체에 저장되는 응답을 보냅니다. 응답에는 HTTP 요청의 성공 또는 실패 여부를 나타내는 HTTP 상태 코드가 포함됩니다. 요청이 성공하면 웹 API는 HTTP 상태 코드 200(확인)과 `HttpResponseMessage.Content` 속성인 JSON 응답을 사용하여 응답합니다. 이 JSON 데이터는 `JsonConvert.DeserializeObject` 메서드를 사용하는 `WeatherData` 개체에 deserialize되기 전에 `HttpContent.ReadAsStringAsync` 메서드를 사용하는 `string`에 대해 읽힙니다. 이 메서드는 JSON 필드 이름과 `WeatherData` 클래스에 정의된 CLR 속성 간의 매핑을 사용하여 deserialization을 수행합니다.

1. 오류가 없는지 확인하기 위해 솔루션을 빌드합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad**의 **WebServiceTutorial** 프로젝트에서 `Constants`라는 새 클래스를 프로젝트에 추가합니다. 그런 다음, **Constants.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public static string OpenWeatherMapEndpoint = "https://api.openweathermap.org/data/2.5/weather";
            public static string OpenWeatherMapAPIKey = "INSERT_API_KEY_HERE";
        }
    }
    ```

    이 코드는 두 개의 상수를 정의합니다. `OpenWeatherMapEndpoint` 상수는 웹 요청을 수행할 엔드포인트를 정의하고, `OpenWeatherMapAPIKey` 상수는 [OpenWeatherMap](https://openweathermap.org/) 서비스에 대한 개인 API 키를 정의합니다.

    > [!IMPORTANT]
    > 개인 OpenWeatherMap API 키를 `OpenWeatherMapAPIKey` 상수 값으로 설정해야 합니다.

1. **Solution Pad**의 **WebServicesTutorial** 프로젝트에서 `WeatherData`라는 새 클래스를 프로젝트에 추가합니다. 그런 다음, **WeatherData.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class WeatherData
        {
            [JsonProperty("name")]
            public string Title { get; set; }

            [JsonProperty("weather")]
            public Weather[] Weather { get; set; }

            [JsonProperty("main")]
            public Main Main { get; set; }

            [JsonProperty("visibility")]
            public long Visibility { get; set; }

            [JsonProperty("wind")]
            public Wind Wind { get; set; }
        }

        public class Main
        {
            [JsonProperty("temp")]
            public double Temperature { get; set; }

            [JsonProperty("humidity")]
            public long Humidity { get; set; }
        }

        public class Weather
        {
            [JsonProperty("main")]
            public string Visibility { get; set; }
        }

        public class Wind
        {
            [JsonProperty("speed")]
            public double Speed { get; set; }
        }
    }
    ```

    이 코드는 웹 서비스에서 검색되는 JSON 데이터를 모델링하는 데 사용되는 네 가지 클래스를 정의합니다. 각 속성은 `JsonProperty` 특성을 사용하여 데코레이트되고 JSON 필드 이름을 포함합니다. Newtonsoft.Json은 모델 개체에 JSON 데이터를 deserialize할 때 CLR 속성에 대한 JSON 필드 이름의 매핑을 사용합니다.

    > [!NOTE]
    > 위의 클래스 정의는 단순화되고, 웹 서비스에서 검색되는 JSON 데이터를 완벽하게 모델링하지 않습니다. 전체 데이터 모델 예제는 [날씨 앱](https://developer.xamarin.com/samples/xamarin-forms/Weather/) 샘플을 참조하세요.

1. **Solution Pad**의 **WebServiceTutorial** 프로젝트에서 `RestService`라는 새 클래스를 프로젝트에 추가합니다. 그런 다음, **RestService.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.Diagnostics;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class RestService
        {
            HttpClient _client;

            public RestService()
            {
                _client = new HttpClient();
            }

            public async Task<WeatherData> GetWeatherDataAsync(string uri)
            {
                WeatherData weatherData = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        weatherData = JsonConvert.DeserializeObject<WeatherData>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return weatherData;
            }
        }
    }
    ```

    이 코드는 [OpenWeatherMap](https://openweathermap.org/) 웹 API에서 지정된 위치에 대한 날씨 데이터를 검색하는 `GetWeatherDataAsync` 단일 메서드를 정의합니다. 이 메서드는 `HttpClient.GetAsync` 메서드를 사용하여 `uri` 인수에서 지정된 웹 API에 GET 요청을 보냅니다. 웹 API는 `HttpResponseMessage` 개체에 저장되는 응답을 보냅니다. 응답에는 HTTP 요청의 성공 또는 실패 여부를 나타내는 HTTP 상태 코드가 포함됩니다. 요청이 성공하면 웹 API는 HTTP 상태 코드 200(확인)과 `HttpResponseMessage.Content` 속성인 JSON 응답을 사용하여 응답합니다. 이 JSON 데이터는 `JsonConvert.DeserializeObject` 메서드를 사용하는 `WeatherData` 개체에 deserialize되기 전에 `HttpContent.ReadAsStringAsync` 메서드를 사용하는 `string`에 대해 읽힙니다. 이 메서드는 JSON 필드 이름과 `WeatherData` 클래스에 정의된 CLR 속성 간의 매핑을 사용하여 deserialization을 수행합니다.

1. 오류가 없는지 확인하기 위해 솔루션을 빌드합니다.
