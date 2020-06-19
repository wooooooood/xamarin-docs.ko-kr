---
title: 구성 관리
description: 이 장에서는 eShopOnContainers mobile 앱이 구성 관리를 구현 하 여 앱 설정 및 사용자 설정을 제공 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f6f61503f619c08ed3e4eae2adf6ddb2c474f99f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84571482"
---
# <a name="configuration-management"></a>구성 관리

설정을 사용 하면 응용 프로그램의 동작을 구성 하는 데이터를 분리 하 여 앱을 다시 빌드하지 않고도 동작을 변경할 수 있습니다. 설정에는 앱 설정 및 사용자 설정의 두 가지 유형이 있습니다.

앱 설정은 앱에서 만들고 관리 하는 데이터입니다. 고정 웹 서비스 끝점, API 키 및 런타임 상태와 같은 데이터가 포함 될 수 있습니다. 앱 설정은 앱의 존재 여부와 연결 되며 해당 앱에만 의미가 있습니다.

사용자 설정은 앱의 동작에 영향을 주는 앱의 사용자 지정 가능한 설정 이며 자주 다시 조정할 필요가 없습니다. 예를 들어 앱을 사용 하면 사용자가 데이터를 검색할 위치와 화면에 표시 하는 방법을 지정할 수 있습니다.

Xamarin.Forms에는 설정 데이터를 저장 하는 데 사용할 수 있는 영구 사전이 포함 되어 있습니다. 이 사전은 속성을 사용 하 여 액세스할 수 [`Application.Current.Properties`](xref:Xamarin.Forms.Application.Properties) 있으며, 응용 프로그램에 배치 된 모든 데이터는 앱이 절전 모드로 전환 될 때 저장 되 고 앱이 다시 시작 되거나 다시 시작 될 때 복원 됩니다. 또한 클래스에는 [`Application`](xref:Xamarin.Forms.Application) [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) 필요할 때 앱에서 해당 설정을 저장할 수 있도록 하는 메서드도 있습니다. 이 사전에 대 한 자세한 내용은 [속성 사전](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary)을 참조 하세요.

영구적 사전을 사용 하 여 데이터를 저장 하는 경우 Xamarin.Forms 에는 데이터를 쉽게 바인딩할 수 없다는 단점이 있습니다. 따라서 eShopOnContainers 모바일 앱은 [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/)에서 사용할 수 있는 xam. s a s. 설정 라이브러리를 사용 합니다. 이 라이브러리는 각 플랫폼에서 제공 하는 네이티브 설정 관리를 사용 하는 동시에 앱 및 사용자 설정을 유지 하 고 검색 하기 위한 일관 되 고 형식이 안전한 플랫폼 간 방법을 제공 합니다. 또한 데이터 바인딩을 사용 하 여 라이브러리에서 제공 하는 설정 데이터에 액세스 하는 것도 간단 합니다.

> [!NOTE]
> Xam. Plugin. 설정 라이브러리는 앱과 사용자 설정을 둘 다 저장할 수 있지만 둘 사이를 구분 하지 않습니다.

## <a name="creating-a-settings-class"></a>설정 클래스 만들기

Xam. s s d. 설정 라이브러리를 사용 하는 경우 앱에 필요한 앱 및 사용자 설정을 포함 하는 단일 정적 클래스를 만들어야 합니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱의 Settings 클래스를 보여 줍니다.

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

설정은 API를 통해 읽고 쓸 수 있으며 `ISettings` ,이는 Xam. 플러그 인 설정 라이브러리에서 제공 됩니다. 이 라이브러리는 API에 액세스 하는 데 사용할 수 있는 단일 항목을 제공 `CrossSettings.Current` 하며, 응용 프로그램의 설정 클래스는 속성을 통해이 singleton을 노출 해야 합니다 `ISettings` .

> [!NOTE]
> 플러그 인에 대 한 지시문을 사용 하 고 있습니다. 설정 및 플러그 인에 대 한 액세스를 필요로 하는 클래스에는 설정 및 플러그 인을 추가 해야 합니다.

## <a name="adding-a-setting"></a>설정 추가

각 설정은 키, 기본 값 및 속성으로 구성 됩니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱이 연결 하는 온라인 서비스에 대 한 기준 URL을 나타내는 사용자 설정에 대 한 세 가지 항목을 모두 보여 줍니다.

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

키는 항상 키 이름을 정의 하는 const 문자열이 며,이 값은 설정의 기본값은 필수 형식의 정적 읽기 전용 값입니다. 기본값을 제공 하면 설정 되지 않은 설정이 검색 되는 경우 유효한 값을 사용할 수 있습니다.

`UrlBase`정적 속성은 API의 두 가지 메서드를 사용 하 여 `ISettings` 설정 값을 읽거나 씁니다. `ISettings.GetValueOrDefault`메서드는 플랫폼별 저장소에서 설정 값을 검색 하는 데 사용 됩니다. 설정에 대해 정의 된 값이 없는 경우에는 기본값이 대신 검색 됩니다. 마찬가지로, 메서드를 사용 하 여 `ISettings.AddOrUpdateValue` 설정의 값을 플랫폼별 저장소에 유지 합니다.

클래스 내에서 기본값을 정의 하는 대신 `Settings` 문자열은 `UrlBaseDefault` 클래스에서 해당 값을 가져옵니다 `GlobalSetting` . 다음 코드 예제에서는 `BaseEndpoint` `UpdateEndpoint` 이 클래스의 속성과 메서드를 보여 줍니다.

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

`BaseEndpoint`속성이 설정 될 때마다 `UpdateEndpoint` 메서드가 호출 됩니다. 이 메서드는 일련의 속성을 업데이트 합니다 .이 속성은 모두 `UrlBase` `Settings` eShopOnContainers 모바일 앱이 연결 하는 다른 끝점을 나타내는 클래스에서 제공 하는 사용자 설정을 기반으로 합니다.

## <a name="data-binding-to-user-settings"></a>사용자 설정에 대 한 데이터 바인딩

EShopOnContainers 모바일 앱에서은 `SettingsView` 두 가지 사용자 설정을 노출 합니다. 이러한 설정을 사용 하면 앱이 Docker 컨테이너로 배포 된 마이크로 서비스에서 데이터를 검색 해야 하는지 여부 또는 앱이 인터넷에 연결 되지 않아도 되는 모의 서비스에서 데이터를 검색 해야 하는지 여부를 구성할 수 있습니다. 컨테이너 화 된 마이크로 서비스에서 데이터를 검색 하도록 선택 하는 경우 마이크로 서비스에 대 한 기본 끝점 URL을 지정 해야 합니다. 그림 7-1에서는 `SettingsView` 사용자가 컨테이너 화 된 마이크로 서비스에서 데이터를 검색 하도록 선택한 경우를 보여 줍니다.

![](configuration-management-images/settings-endpoint.png "User settings exposed by the eShopOnContainers mobile app")

**그림 7-1**: eShopOnContainers 모바일 앱에서 노출 하는 사용자 설정

데이터 바인딩을 사용 하 여 클래스에 의해 노출 되는 설정을 검색 하 고 설정할 수 있습니다 `Settings` . 이는 클래스의 속성에 액세스 하 `Settings` 고 설정 값이 변경 된 경우 속성 변경 알림을 발생 시키는 모델 속성을 보기 위해 뷰 바인딩의 컨트롤에 의해 이루어집니다. EShopOnContainers 모바일 앱에서 모델을 생성 하 고 보기에 연결 하는 방법에 대 한 자세한 내용은 [뷰 모델 로케이터를 사용 하 여 자동으로 뷰 모델 만들기](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically-creating-a-view-model-with-a-view-model-locator)를 참조 하세요.

다음 코드 예제에서는 [`Entry`](xref:Xamarin.Forms.Entry) `SettingsView` 사용자가 컨테이너 화 된 마이크로 서비스에 대 한 기본 끝점 URL을 입력할 수 있도록 하는의 컨트롤을 보여 줍니다.

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

이 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤은 `Endpoint` `SettingsViewModel` 양방향 바인딩을 사용 하 여 클래스의 속성에 바인딩됩니다. 다음 코드 예제에서는 끝점 속성을 보여 줍니다.

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

`Endpoint`속성이 설정 되어 있으면 제공 된 `UpdateEndpoint` 값이 유효 하 고 속성 변경 알림이 발생 한 경우 메서드가 호출 됩니다. 다음 코드 예제는 `UpdateEndpoint` 메서드를 보여줍니다.

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

이 메서드는 `UrlBase` `Settings` 사용자가 입력 한 기본 끝점 URL 값을 사용 하 여 클래스의 속성을 업데이트 합니다. 그러면 해당 값이 플랫폼별 저장소에 유지 됩니다.

`SettingsView`을 탐색 하는 경우 `InitializeAsync` 클래스의 메서드가 `SettingsViewModel` 실행 됩니다. 다음 코드 예제에서는 이 메서드를 보여줍니다.

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

메서드는 속성을 `Endpoint` 클래스의 속성 값으로 설정 합니다 `UrlBase` `Settings` . 속성에 액세스 하는 경우 Xam. s a s. `UrlBase` 설정 라이브러리에서 플랫폼별 저장소의 설정 값을 검색 합니다. 메서드를 호출 하는 방법에 대 한 자세한 내용은 `InitializeAsync` [탐색 하는 동안 매개 변수 전달](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing-parameters-during-navigation)을 참조 하세요.

이 메커니즘을 사용 하면 사용자가 SettingsView로 이동할 때마다 사용자 설정이 플랫폼별 저장소에서 검색 되 고 데이터 바인딩을 통해 표시 됩니다. 그런 다음 사용자가 설정 값을 변경 하는 경우 데이터 바인딩은 플랫폼별 저장소에 즉시 다시 저장 되도록 합니다.

## <a name="summary"></a>요약

설정을 사용 하면 응용 프로그램의 동작을 구성 하는 데이터를 분리 하 여 앱을 다시 빌드하지 않고도 동작을 변경할 수 있습니다. 앱 설정은 앱에서 만들고 관리 하는 데이터 이며, 사용자 설정은 앱의 동작에 영향을 주는 앱의 사용자 지정 가능한 설정 이며 자주 다시 조정할 필요가 없습니다.

Xam. s a s. 설정 라이브러리는 앱 및 사용자 설정을 유지 하 고 검색 하기 위한 일관 되 고 형식이 안전한 크로스 플랫폼 방식을 제공 하며, 데이터 바인딩을 사용 하 여 라이브러리를 사용 하 여 만든 설정에 액세스할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [다운로드 전자책 (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
