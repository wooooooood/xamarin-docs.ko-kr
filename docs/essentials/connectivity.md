---
title: 'Xamarin.Essentials: 연결'
description: Xamarin.Essentials의 Connectivity 클래스를 사용하면 디바이스의 네트워크 상태 변경 내용을 모니터링하고 현재 네트워크 액세스 및 현재 연결된 방식을 확인할 수 있습니다.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 01/08/2019
ms.custom: video
ms.openlocfilehash: c70510f7b47f93c6119532b6a1c06f6c2e9e56ea
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "67855765"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials: 연결

**Connectivity** 클래스를 사용하면 디바이스의 네트워크 상태 변경 내용을 모니터링하고 현재 네트워크 액세스 및 현재 연결된 방식을 확인할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

**Connectivity** 기능에 액세스하려면 다음과 같은 플랫폼 특정 설정이 필요합니다.

# <a name="android"></a>[Android](#tab/android)

`AccessNetworkState` 권한이 필요하며 Android 프로젝트에서 구성해야 합니다. 이 권한은 다음과 같은 방법으로 추가할 수 있습니다.

**속성** 폴더 아래의 **AssemblyInfo.cs** 파일을 열고 다음을 추가합니다.

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

또는 Android 매니페스트를 업데이트합니다.

**속성** 폴더 아래의 **AndroidManifest.xml** 파일을 열고 **매니페스트** 노드 내부에 다음을 추가합니다.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

또는 Android 프로젝트를 마우스 오른쪽 단추로 클릭하고 프로젝트의 속성을 엽니다. **Android 매니페스트** 아래에서 **필요한 권한:** 영역을 찾아 **액세스 네트워크 상태** 권한을 확인합니다. 그러면 **AndroidManifest.xml** 파일이 자동으로 업데이트됩니다.

# <a name="ios"></a>[iOS](#tab/ios)

추가 설정이 필요하지 않습니다.

# <a name="uwp"></a>[UWP](#tab/uwp)

추가 설정이 필요하지 않습니다.

-----

## <a name="using-connectivity"></a>Connectivity 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

현재 네트워크 액세스를 확인합니다.

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[네트워크 액세스](xref:Xamarin.Essentials.NetworkAccess)는 다음 범주로 구분됩니다.

* **인터넷** - 로컬 및 인터넷 액세스입니다.
* **ConstrainedInternet** - 제한된 인터넷 액세스입니다. 웹 포털에 대한 로컬 액세스가 제공되지만 인터넷에 액세스하려면 포털을 통해 특정 자격 증명을 제공해야 하는 종속 포털 연결을 나타냅니다.
* **로컬** - 로컬 네트워크 액세스 전용입니다.
* **없음** - 사용 가능한 연결이 없습니다.
* **알 수 없음** - 인터넷 연결을 확인할 수 없습니다.

디바이스에서 현재 사용 중인 [연결 프로필](xref:Xamarin.Essentials.ConnectionProfile) 유형을 확인할 수 있습니다.

```csharp
var profiles = Connectivity.ConnectionProfiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

연결 프로필 또는 네트워크 액세스가 변경될 때마다 트리거 시 이벤트를 수신할 수 있습니다.

```csharp
public class ConnectivityTest
{
    public ConnectivityTest()
    {
        // Register for connectivity changes, be sure to unsubscribe when finished
        Connectivity.ConnectivityChanged += Connectivity_ConnectivityChanged;
    }

    void Connectivity_ConnectivityChanged(object sender, ConnectivityChangedEventArgs e)
    {
        var access = e.NetworkAccess;
        var profiles = e.ConnectionProfiles;
    }
}
```

## <a name="limitations"></a>제한 사항

`NetworkAccess`에서 `Internet`를 보고해도 웹에 대한 모든 권한을 사용할 수 없는 경우도 있습니다. 각 플랫폼에서 연결이 작동하는 방식으로 인해 연결이 사용 가능한 것만 보장할 수 있습니다. 예를 들어 디바이스가 Wi-Fi 네트워크에 연결되어 있지만 라우터의 인터넷 연결이 끊어졌을 수 있습니다. 이 경우 인터넷이 보고되더라도 활성 연결을 사용할 수 없습니다.

## <a name="api"></a>API

* [Connectivity 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Connectivity API 문서](xref:Xamarin.Essentials.Connectivity)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Connectivity-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
