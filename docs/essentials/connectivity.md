---
title: Xamarin.Essentials 연결
description: 연결 클래스를 사용 하 여 장치의 네트워크 상태 변화에 대 한 모니터링, 현재 네트워크 액세스와 현재 연결 된 방식을 확인 수 있습니다.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 99faa518f44dca51cd1bbe2562a83893c48d2917
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials 연결

![시험판 NuGet](~/media/shared/pre-release.png)

**연결** 클래스 통해 장치의 네트워크 상태 변화에 대 한 모니터링 현재 네트워크 액세스와 현재 연결 된 방식을 확인 합니다.

## <a name="getting-started"></a>시작

액세스는 **연결** 는 다음과 같은 플랫폼 특정 설치 기능이 필요 합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`AccessNetworkState` 권한이 필요 하 고 Android 프로젝트에서 구성 해야 합니다. 다음과 같은 방법으로 추가할 수 있습니다.

열기는 **AssemblyInfo.cs** 에서 파일의 **속성** 폴더 추가:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

또는 Android 매니페스트를 업데이트 합니다.

열기는 **AndroidManifest.xml** 아래 파일는 **속성** 폴더 내에 다음 코드를 추가 하 고는 **매니페스트** 노드.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

또는 Anroid 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 프로젝트의 속성을 엽니다. 아래 **Android 매니페스트** 찾을 **필요한 권한:** 영역 및 검사는 **액세스 네트워크 상태** 권한. 이 자동으로 업데이트는 **AndroidManifest.xml** 파일입니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

추가 설치 하지 않아도 됩니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

추가 설치 하지 않아도 됩니다.

-----

## <a name="using-connectivity"></a>연결을 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

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

[네트워크 액세스](xref:Xamarin.Essentials.NetworkAccess) 다음 범주에 속합니다.

* **인터넷** – 로컬 및 인터넷 액세스 합니다.
* **ConstrainedInternet** – 인터넷 액세스를 제한 합니다. 여기서는 웹 포털에 대 한 로컬 액세스를 사용 하지만 특정 자격 증명 포털을 통해 제공 되는 인터넷에 액세스할 수 있어야 고정 포털 연결을 나타냅니다.
* **로컬** – 로컬 네트워크만 액세스 합니다.
* **None** – 없는 연결을 사용할 수 있습니다.
* **알 수 없는** – 인터넷 연결을 확인할 수 없습니다.

어떤 유형의 확인할 수 있습니다 [연결 프로필](xref:Xamarin.Essentials.ConnectionProfile) 적극적으로 사용 하는 장치:

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

연결 프로필 또는 네트워크 변경 내용에 액세스 될 때마다 트리거되는 경우의 이벤트를 받을 수 있습니다.

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

수 있다는 것에 유의 해야 하는 `Internet` 을 보고 `NetworkAccess` 하지만 웹에 대 한 모든 권한을 사용할 수 없습니다. 각 플랫폼에서 연결을 작동 하는 방식으로 인해만 연결을 사용할 수 보장할 수 있습니다. 장치가 Wi-fi 네트워크에 연결 수는 예를 들어 있지만 라우터가 인터넷에서 연결이 끊어졌습니다. 이 인스턴스의 인터넷 보고 될 수 있습니다, 하지만 활성 연결에 사용할 수 없습니다.

## <a name="api"></a>API

* [소스 코드 연결](https://github.com/xamarin/Essentials/tree/master/Essentials/Connectivity)
* [연결 API 설명서](xref:Xamarin.Essentials.Connectivity)
