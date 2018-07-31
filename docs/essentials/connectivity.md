---
title: 'Xamarin.Essentials: 연결'
description: Xamarin.Essentials 연결 클래스를 사용 하 여 장치의 네트워크 상태 변화에 대 한 모니터링는 현재 네트워크 액세스 및 현재 연결 된 방식을 확인할 수 있습니다.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 96b4ee0487034c651bec1dfb168fed7567b63c96
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353700"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials: 연결

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **연결** 장치의 네트워크 상태 변화에 대 한 모니터링 클래스 사용의 현재 네트워크 액세스 및 현재 연결 된 방식을 확인 합니다.

## <a name="getting-started"></a>시작

액세스 하는 **연결** 플랫폼 특정 설정 기능은 필요 합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`AccessNetworkState` 권한이 필요 하며 Android 프로젝트에 구성 되어야 합니다. 이 다음과 같은 방법으로 추가할 수 있습니다.

엽니다는 **AssemblyInfo.cs** 파일을 **속성** 폴더 추가:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

또는 Android 매니페스트를 업데이트 합니다.

열기를 **AndroidManifest.xml** 파일을 **속성** 폴더 내부에 다음 줄을 추가 합니다 **매니페스트** 노드.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

또는 Android 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 프로젝트의 속성을 엽니다. 아래 **Android 매니페스트** 찾을 합니다 **필요한 권한:** 영역과 확인을 **액세스 네트워크 상태** 권한. 이 자동으로 업데이트 합니다 **AndroidManifest.xml** 파일입니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

추가 설정이 필요 없습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

추가 설정이 필요 없습니다.

-----

## <a name="using-connectivity"></a>연결을 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

현재 네트워크 액세스를 확인 합니다.

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[네트워크 액세스](xref:Xamarin.Essentials.NetworkAccess) 다음 범주에 해당 합니다.

* **인터넷** -로컬 및 인터넷 액세스 합니다.
* **ConstrainedInternet** – 인터넷 액세스를 제한 합니다. 웹 포털에 대 한 로컬 액세스를 사용 하지만 구체적인 자격 증명에 포털을 통해 제공 되는 인터넷에 액세스할 필요는 captive 포털 연결을 나타냅니다.
* **로컬** – 로컬 네트워크만 액세스 합니다.
* **None** – 없는 연결을 사용할 수 있습니다.
* **알 수 없는** – 인터넷 연결을 확인할 수 없습니다.

유형을 확인할 수 있습니다 [연결 프로필](xref:Xamarin.Essentials.ConnectionProfile) 장치에서 적극적으로 사용 합니다.

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

연결 프로필 또는 네트워크 변경 내용에 액세스할 때마다 트리거될 때 이벤트를 받을 수 있습니다.

```csharp
public class ConnectivityTest
{
    public ConnectivityTest()
    {
        // Register for connectivity changes, be sure to unsubscribe when finished
        Connectivity.ConnectivityChanged += Connectivity_ConnectivityChanged;
    }

    void Connectivity_ConnectivityChanged(ConnectivityChangedEventArgs  e)
    {
        var access = e.NetworkAccess;
        var profiles = e.Profiles;
    }
}
```

## <a name="limitations"></a>제한 사항

수 있다는 것을 확인 해야 하는 `Internet` 에 의해 보고 됩니다 `NetworkAccess` 하지만 전체 웹 액세스를 사용할 수 없습니다. 연결 각 플랫폼에서 작동 하는 방식으로 인해 것만 보장할 수 연결을 사용할 수 있습니다. Wi-fi 네트워크에 장치를 연결할 수 있습니다 예를 들어 있지만 라우터가 인터넷에서 연결이 끊어졌습니다. 활성 연결을 사용할 수 없는 않고 이런에서 인터넷으로 보고 될 수 있습니다.

## <a name="api"></a>API

* [소스 코드 연결](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [연결 API 설명서](xref:Xamarin.Essentials.Connectivity)
