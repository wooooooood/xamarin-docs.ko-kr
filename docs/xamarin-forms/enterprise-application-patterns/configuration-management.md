---
title: 구성 관리
description: 이 장에서 eShopOnContainers 모바일 앱에서 앱 설정 및 사용자 설정을 제공 하는 구성 관리를 구현 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 6f32d8f328232bdfc644da57bdb3201c60010063
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61381905"
---
# <a name="configuration-management"></a>구성 관리

설정 동작을 앱을 다시 작성 하지 않고 변경할 수 있도록 코드에서 앱의 동작을 구성 하는 데이터 분리를 허용 합니다. 설정의 두 가지가: 앱 설정 및 사용자 설정 합니다.

앱 설정은 앱을 만들고 관리 하는 데이터입니다. 고정 된 웹 서비스 끝점, API 키 및 런타임 상태와 같은 데이터를 포함할 수 있습니다. 앱 설정 앱의 존재 여부에 연결 되 고 해당 앱에만 적합 합니다.

사용자 설정은 앱의 동작에 영향을 자주 다시 조정 필요 하지 않습니다 하는 앱의 사용자 지정 가능한 설정입니다. 예를 들어 앱에서 데이터를 검색 하 고 화면에 표시 하는 방법을 지정 하는 사용자를 할당할 수도 있습니다.

Xamarin.Forms에는 설정 데이터를 저장 하기 위해 사용할 수 있는 영구 사전을 포함 합니다. 이 사전을 사용 하 여 액세스할 수 합니다 [ `Application.Current.Properties` ](xref:Xamarin.Forms.Application.Properties) 앱을 절전 상태로 전환 하 고 앱 다시 시작 또는 다시 시작 하는 경우 복원 된 배치 된 모든 데이터와 속성에 저장 됩니다. 또한 합니다 [ `Application` ](xref:Xamarin.Forms.Application) 클래스에는 [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) 해당 설정이 필요한 경우를 저장 하는 앱을 허용 하는 메서드. 이 사전에 대 한 자세한 내용은 참조 하세요. [속성 사전](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary)합니다.

Xamarin.Forms 영구 사전을 사용 하 여 데이터를 저장 하는 단점은 아님을 쉽게 데이터를 바인딩할 것입니다. EShopOnContainers 모바일 앱에서 사용할 수 있는 Xam.Plugins.Settings 라이브러리를 사용 하는 따라서 [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/)합니다. 이 라이브러리에 유지 하 고 앱 및 사용자 설정, 각 플랫폼에서 제공 하는 기본 설정 관리를 사용 하는 동안 검색에 대 한 일관 된, 형식이 안전한, 플랫폼 간 접근 방식을 제공 합니다. 또한 데이터 바인딩 라이브러리에서 제공 하는 설정 데이터에 액세스 하는 데 쉽습니다.

> [!NOTE]
> Xam.Plugin.Settings 라이브러리에는 앱 및 사용자 설정을 저장할 수 있습니다, 그렇게 하면 두 구분 되지 않습니다.

## <a name="creating-a-settings-class"></a>설정 클래스 만들기

Xam.Plugins.Settings 라이브러리를 사용 하는 경우 단일 정적 클래스로 만들어야 하는 앱에 필요한 앱 및 사용자 설정이 포함 됩니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱에서 설정 클래스를 보여 줍니다.

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

설정을 읽고 기록할 수는 `ISettings` Xam.Plugins.Settings 라이브러리에서 제공 하는 API입니다. 이 라이브러리는 API에 액세스를 사용할 수 있는 단일 제공 `CrossSettings.Current`, 앱의 설정 클래스를 통해이 단일 항목을 노출 해야 하 고는 `ISettings` 속성입니다.

> [!NOTE]
> Plugin.Settings 및 Plugin.Settings.Abstractions 네임 스페이스에 대 한 using 지시문 Xam.Plugins.Settings 라이브러리 형식에 대 한 액세스를 필요로 하는 클래스에 추가 되어야 합니다.

## <a name="adding-a-setting"></a>설정 추가

각 설정 키, 기본 값, 및 속성으로 구성 됩니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱에 연결 하는 온라인 서비스에 대 한 기본 URL을 나타내는 사용자 설정에 대 한 모든 세 가지 항목을 보여 줍니다.

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

키는 항상 키 이름을 정의 하는 const 문자열, 설정에 대 한 기본값을 사용 하 여 되 필요한 형식의 정적 읽기 전용 값입니다. 기본값을 제공 하는 설정 되지 않은 설정이 검색 되는 경우 사용할 수 있는 유효한 값을 확인 합니다.

`UrlBase` 에서 두 메서드를 사용 하는 정적 속성을 `ISettings` 설정 값을 쓰거나 읽을 수 있는 API입니다. `ISettings.GetValueOrDefault` 메서드 플랫폼별 저장소에서 설정 값을 검색 하는 합니다. 설정에 대해 정의 된 값이 없는 경우 기본값 대신 검색 됩니다. 마찬가지로,는 `ISettings.AddOrUpdateValue` 메서드 플랫폼별 저장소 설정의 값을 유지를 사용 합니다.

내에서 기본 값을 정의 하는 대신 합니다 `Settings` 클래스를 `UrlBaseDefault` 문자열에서 값을 가져옵니다는 `GlobalSetting` 클래스입니다. 다음 코드 예제는 `BaseEndpoint` 속성 및 `UpdateEndpoint` 이 클래스의 메서드:

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

때마다 합니다 `BaseEndpoint` 속성이 설정 되어를 `UpdateEndpoint` 메서드가 호출 됩니다. 이 메서드는 일련의 속성을 기반으로 하는 모든 업데이트를 `UrlBase` 에서 제공 하는 사용자 설정의 `Settings` eShopOnContainers 모바일 앱에 연결 하는 다양 한 끝점을 나타내는 클래스입니다.

## <a name="data-binding-to-user-settings"></a>사용자 설정에 데이터 바인딩

EShopOnContainers 모바일 앱에서의 `SettingsView` 두 사용자 설정을 제공 합니다. 이러한 설정에는 앱을 Docker 컨테이너로 배포 되는 마이크로 서비스에서 데이터를 검색 해야 하는지 여부 또는 앱 인터넷 연결이 필요 하지 않은 모의 서비스에서 데이터를 검색 해야 여부를 구성할을 수 있습니다. 컨테이너 화 된 마이크로 서비스에서 데이터를 검색할를 선택할 때 마이크로 서비스에 대 한 기본 끝점 URL은 지정 해야 합니다. 그림 7-1은 `SettingsView` 컨테이너 화 된 마이크로 서비스에서 데이터를 검색 하는 사용자가 선택 하는 경우.

![](configuration-management-images/settings-endpoint.png "EShopOnContainers 모바일 앱에서 노출 하는 사용자 설정")

**그림 7-1**: EShopOnContainers 모바일 앱에서 노출 하는 사용자 설정

데이터 바인딩을 검색 하 고 설정에 의해 노출 되는 설정을 사용할 수는 `Settings` 클래스입니다. 컨트롤의 속성에 액세스 하는 보기 모델 속성을 보기 바인딩의 이렇게는 `Settings` 클래스 및 속성을 발생 시키는 변경 알림 설정 값이 변경 합니다. EShopOnContainers 모바일 앱에서 뷰를 생성 하는 방법에 대 한 정보를 모델링 하 고 연결 합니다 뷰를 참조 하세요 [자동으로 보기 모델 로케이터를 사용 하 여 뷰 모델을 만들기](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)합니다.

다음 코드 예제는 [ `Entry` ](xref:Xamarin.Forms.Entry) 에서 제어를 `SettingsView` 컨테이너 화 된 마이크로 서비스에 대 한 기본 끝점 URL을 입력 하 여 사용자를 허용 하는:

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

이 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤에 바인딩되는 `Endpoint` 속성을 `SettingsViewModel` 양방향 바인딩을 사용 하 여 클래스입니다. 다음 코드 예제에서는 끝점 속성을 보여 줍니다.

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

경우는 `Endpoint` 속성이 설정 되어를 `UpdateEndpoint` 메서드가 호출 되 면 제공 하는 제공 된 값이 유효 하면 속성 변경 알림이 발생 합니다. 다음 코드 예제는 `UpdateEndpoint` 메서드를 보여줍니다.

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

이 메서드를 업데이트 합니다 `UrlBase` 속성에는 `Settings` 플랫폼별 저장소에 유지 하도록 할 수 있는 사용자가 입력 한 기본 끝점 URL 값을 사용 하 여 클래스입니다.

경우는 `SettingsView` 을 탐색 하면를 `InitializeAsync` 에서 메서드를 `SettingsViewModel` 클래스에서 실행 됩니다. 다음 코드 예제에서는 이 메서드를 보여줍니다.

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

메서드 집합을 `Endpoint` 속성의 값을는 `UrlBase` 속성에는 `Settings` 클래스입니다. 액세스는 `UrlBase` 속성으로 인해 Xam.Plugins.Settings 라이브러리 플랫폼별 저장소에서 설정 값을 검색 합니다. 하는 방법에 대 한 내용은 `InitializeAsync` 메서드가 호출 되 면 참조 [매개 변수 중 탐색 전달](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation)합니다.

이 메커니즘 있는지 여부를 확인는 사용자는 SettingsView로 이동, 때마다 사용자 설정 플랫폼별 저장소에서 검색할 데이터 바인딩을 통해 제공 합니다. 사용자 설정 값을 변경 하는 경우 데이터 바인딩 그러면 플랫폼별 저장소에 다시 적용 즉시 됩니다.

## <a name="summary"></a>요약

설정 동작을 앱을 다시 작성 하지 않고 변경할 수 있도록 코드에서 앱의 동작을 구성 하는 데이터 분리를 허용 합니다. 앱 설정은 앱을 만들고 관리 하는 데이터 및 사용자 설정 설정은 사용자 지정할 수 있는 앱의 앱의 동작에 영향을 자주 다시 조정 필요 하지 않습니다.

크로스 플랫폼 방법을 유지 하 고 앱 및 사용자 설정 및 데이터 바인딩을 검색 하는 라이브러리를 사용 하 여 만든 설정에 액세스 하려면 사용할 수, Xam.Plugins.Settings 라이브러리 형식 안전성이 일관성을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [2mb PDF 전자책 다운로드](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
