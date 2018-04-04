---
title: 구성 관리
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 9318f86077c9cdb98e073e4816dae4fdabbfe568
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="configuration-management"></a>구성 관리

설정 동작을 응용 프로그램을 다시 작성 하지 않고 변경할 수 있도록 코드에서 응용 프로그램의 동작을 구성 하는 데이터 분리를 허용 합니다. 설정의 두 가지: 응용 프로그램 설정 및 사용자 설정 합니다.

응용 프로그램 설정은 응용 프로그램을 만들고 관리 하는 데이터입니다. 고정된 웹 서비스 끝점, API 키 및 런타임 상태 등의 데이터를 포함할 수 있습니다. 응용 프로그램 설정은 응용 프로그램의 존재 여부에 연결 된 이므로 해당 응용 프로그램에 의미 있는 있습니다.

사용자 설정은 사용자 지정 가능한 설정을 응용 프로그램의 응용 프로그램의 동작에 영향을 주고 자주 다시 조정 필요 하지 않습니다. 예를 들어 응용 프로그램에 사용자 데이터를 검색할 수 있는 위치 및 화면에 표시 하는 방법을 지정할 수 있습니다.

Xamarin.Forms에 설정 데이터를 저장 하는 데 사용할 수 있는 영구 사전에 포함 됩니다. 사용 하 여이 사전에 액세스할 수 있습니다는 [ `Application.Current.Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) 앱 절전 상태로 전환 하 고 앱을 다시 시작 하거나 다시 시작 될 때 복원 된 속성과에 배치 된 모든 데이터가 저장 됩니다. 또한는 [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) 클래스에는 [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) 필요할 때 저장에서 설정을 유지 하려면 응용 프로그램 수 방법입니다. 이 사전에 대 한 자세한 내용은 참조 [속성 사전의](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary)합니다.

Xamarin.Forms 영구 사전을 사용 하 여 데이터를 저장 하는 단점은 아님을 쉽게에 바인딩된 데이터입니다. 따라서 eShopOnContainers 모바일 앱 사용 Xam.Plugins.Settings 라이브러리에서 사용할 수 있는 [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/)합니다. 이 라이브러리에 유지 하 고 각 플랫폼에서 제공 하는 기본 설정 관리를 사용 하는 동안 응용 프로그램 및 사용자 설정을 검색 하는 위한 일관 된, 형식이 안전한, 플랫폼 간 방식을 제공 합니다. 또한 데이터 바인딩을 사용 하는 라이브러리에 의해 노출 되는 설정 데이터에 액세스 하는 것이 쉽습니다.

> [!NOTE]
> Xam.Plugin.Settings 라이브러리 응용 프로그램 및 사용자를 모두 설정을 저장할 수 있습니다, 그렇게 하면 두 구별 되지 않습니다.

## <a name="creating-a-settings-class"></a>설정 클래스 만들기

Xam.Plugins.Settings 라이브러리를 사용할 때는 단일 정적 클래스를 만들어야 하는 응용 프로그램에 필요한 응용 프로그램 및 사용자 설정이 포함 됩니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱에서 설정 클래스를 보여 줍니다.

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

설정을 읽고 통해 기록 된 수의 `ISettings` Xam.Plugins.Settings 라이브러리에서 제공 하는 API입니다. 이 라이브러리는 API에 액세스 하려면 사용할 수 있는 단일 제공 `CrossSettings.Current`, 응용 프로그램 설정 클래스를 통해이 단일 항목을 노출 해야 하 고는 `ISettings` 속성입니다.

> [!NOTE]
> Plugin.Settings 및 Plugin.Settings.Abstractions 네임 스페이스에 대 한 지시문을 사용 하 여 Xam.Plugins.Settings 라이브러리 형식에 대 한 액세스를 필요로 하는 클래스에 추가 해야 합니다.

## <a name="adding-a-setting"></a>설정 추가

각 설정 키, 기본 값, 및 속성을 구성 됩니다. 다음 코드 예제에서는 세 eShopOnContainers 모바일 앱에 연결 하는 온라인 서비스에 대 한 기본 URL을 나타내는 사용자 설정에 대 한 항목을 보여 줍니다.

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

키가 항상 키 이름을 정의 하는 const 문자열, 되 고 필요한 종류의 정적 읽기 전용 값 설정에 대 한 기본값입니다. 기본값을 제공 하는 설정 되지 않은 설정이 검색 되는 경우 사용할 수 있는 유효한 값을 확인 합니다.

`UrlBase` 에서 두 메서드를 사용 하는 정적 속성은 `ISettings` API를 읽거나 쓰도록 설정 값입니다. `ISettings.GetValueOrDefault` 플랫폼 특정 저장소에서 설정 값을 검색할 메서드를 사용 합니다. 설정에 대해 정의 된 값이 없는 경우 대신 기본 값을 검색 합니다. 마찬가지로,는 `ISettings.AddOrUpdateValue` 메서드 플랫폼 특정 저장소로 설정 값을 유지 하는 데 사용 됩니다.

내부 기본값을 정의 하는 대신는 `Settings` 클래스는 `UrlBaseDefault` 문자열에서 값을 가져옵니다는 `GlobalSetting` 클래스입니다. 다음 코드 예제는 `BaseEndpoint` 속성 및 `UpdateEndpoint` 이 클래스의 메서드와:

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

때마다는 `BaseEndpoint` 속성이 설정 되어는 `UpdateEndpoint` 메서드를 호출 합니다. 이 메서드는 일련의 속성을 기반으로 중 일부는 업데이트는 `UrlBase` 에서 제공 하는 사용자 설정의 `Settings` eShopOnContainers 모바일 앱에 연결 하는 다른 끝점을 나타내는 클래스입니다.

## <a name="data-binding-to-user-settings"></a>사용자 설정에 데이터 바인딩

EShopOnContainers 모바일 앱에서의 `SettingsView` 두 사용자 설정을 제공 합니다. 이러한 설정을 응용 프로그램에서 microservices, Docker 컨테이너 형태로 배포 되는 데이터를 검색 해야 하는지 여부 또는 응용 프로그램 인터넷 연결이 필요 하지 않은 모의 서비스에서 데이터를 검색 해야 하는지 여부의 구성을 사용 합니다. 에서 컨테이너 화 된 microservices에서 데이터를 검색 하도록 선택한 경우에 microservices에 대 한 기본 끝점 URL 지정 되어야 합니다. 그림 7-1은 `SettingsView` 때 사용자가 선택 하면 컨테이너 화 된 microservices에서 데이터를 검색 합니다.

![](configuration-management-images/settings-endpoint.png "EShopOnContainers 모바일 응용 프로그램에 의해 노출 되는 사용자 설정")

**그림 7-1**: eShopOnContainers 모바일 앱에 의해 노출 되는 사용자 설정

데이터 바인딩을 검색 하 고 설정에 의해 노출 되는 설정을 사용할 수는 `Settings` 클래스입니다. 컨트롤에서 속성에 액세스 하는 보기 모델 속성을 보기 바인딩에서 이렇게는 `Settings` 클래스 및 속성을 발생 시키는 알림 설정 값이 변경 되었으면 변경 합니다. EShopOnContainers 모바일 앱에서 보기를 생성 하는 방법에 대 한 정보를 모델링 보기에 연결에 대 한 참조 [보기 모델 로케이터에 뷰 모델을 자동으로 만드는](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)합니다.

다음 코드 예제는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 에서 제어는 `SettingsView` 색인화 microservices에 대 한 기본 끝점 URL을 입력 하는 사용자를 허용 하 합니다.

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

이 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 컨트롤에 바인딩되는 `Endpoint` 의 속성은 `SettingsViewModel` 클래스, 양방향 바인딩을 사용 합니다. 다음 코드 예제에서는 끝점 속성을 보여 줍니다.

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

경우는 `Endpoint` 속성이 설정 되어는 `UpdateEndpoint` 메서드가 호출 되 면 제공 된 값이 유효 하 고 속성 변경 알림이 발생 합니다. 다음 코드 예제는 `UpdateEndpoint` 메서드:

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

이 메서드는 업데이트는 `UrlBase` 속성에는 `Settings` 플랫폼 특정 저장소에 유지 될 수는 사용자가 입력 한 기본 끝점 URL 값을 사용 하 여 클래스입니다.

경우는 `SettingsView` 탐색 되는 `InitializeAsync` 에서 메서드는 `SettingsViewModel` 클래스에서 실행 됩니다. 다음 코드 예제에서는이 메서드를 보여 줍니다.

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

메서드는 `Endpoint` 속성의 값으로는 `UrlBase` 속성에는 `Settings` 클래스. 에 액세스 하는 `UrlBase` 속성으로 인해 Xam.Plugins.Settings 라이브러리 플랫폼 특정 저장소 로부터 설정 값을 검색 합니다. 방법에 대 한 내용은 `InitializeAsync` 메서드가 호출 되 면 참조 [탐색 하는 동안 매개 변수 전달](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation)합니다.

이 메커니즘을 수행 하면 사용자는 SettingsView로 이동, 때마다 사용자 설정을 하 플랫폼 특정 저장소에서 검색할 데이터 바인딩을 통해 제공 되 합니다. 설정 값을 변경 하는 경우 데이터 바인딩 그러면 플랫폼 특정 저장소에 다시 적용 즉시 됩니다.

## <a name="summary"></a>요약

설정 동작을 응용 프로그램을 다시 작성 하지 않고 변경할 수 있도록 코드에서 응용 프로그램의 동작을 구성 하는 데이터 분리를 허용 합니다. 응용 프로그램 설정은 응용 프로그램을 만들고 관리 하 고, 데이터 및 사용자 설정을 사용자 지정할 수 있는 하는 설정은 응용 프로그램의 응용 프로그램의 동작에 영향을 주고 자주 다시 조정 필요 하지 않습니다.

라이브러리를 사용 하 여 만든 설정에 액세스 하는 유지 하 고 응용 프로그램 및 사용자 설정 및 데이터 바인딩 검색 하기 위한 플랫폼 간 방식은 사용할 수 있습니다, 그리고 Xam.Plugins.Settings 라이브러리를 일관성 있는 형식 안전을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [EBook (2mb PDF)를 다운로드 합니다.](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
