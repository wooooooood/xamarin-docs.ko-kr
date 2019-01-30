---
title: 'Xamarin.Essentials: 앱 정보'
description: 이 문서에서는 애플리케이션에 대한 정보를 제공하는 Xamarin.Essentials의 AppInfo 클래스를 설명합니다. 예를 들어 앱 이름과 버전을 표시합니다.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 01/29/2019
ms.custom: video
ms.openlocfilehash: fa6910c380545527f930f340536f47b548b74b12
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233213"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: 앱 정보

**AppInfo** 클래스는 애플리케이션에 대한 정보를 제공합니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-appinfo"></a>AppInfo 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>애플리케이션 정보 가져오기

다음 정보는 API를 통해 표시됩니다.

```csharp
// Application Name
var appName = AppInfo.Name;

// Package Name/Application Identifier (com.microsoft.testapp)
var packageName = AppInfo.PackageName;

// Application Version (1.0.0)
var version = AppInfo.VersionString;

// Application Build Number (1)
var build = AppInfo.BuildString;
```

## <a name="displaying-application-settings"></a>애플리케이션 설정 표시

**AppInfo** 클래스는 운영 체제에서 유지 관리하는 애플리케이션의 설정 페이지도 표시할 수 있습니다.

```csharp
// Display settings page
AppInfo.ShowSettingsUI();
```

이 설정 페이지에서는 사용자가 애플리케이션 권한을 변경하고 기타 플랫폼 관련 작업을 수행할 수 있습니다.

## <a name="platform-implementation-specifics"></a>플랫폼 구현 관련 정보

# <a name="androidtabandroid"></a>[Android](#tab/android)

앱 정보는 다음 필드에 대한 `AndroidManifest.xml`에서 가져옵니다.

- **빌드** – `manifest` 노드의 `android:versionCode`
- **이름** - `application` 노드의 `android:label`
- **PackageName**: `manifest` 노드의 `package`
- **VersionString** – `application` 노드의 `android:versionName`

# <a name="iostabios"></a>[iOS](#tab/ios)

앱 정보는 다음 필드에 대한 `Info.plist`에서 가져옵니다.

- **빌드** – `CFBundleVersion`
- **이름** -  설정된 경우 `CFBundleDisplayName`이고 그렇지 않으면 `CFBundleName`임
- **PackageName**: `CFBundleIdentifier`
- **VersionString** – `CFBundleShortVersionString`

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

앱 정보는 다음 필드에 대한 `Package.appxmanifest`에서 가져옵니다.

- **빌드** – `Identity` 노드의 `Version`에서 `Build`를 사용함
- **이름** - `Properties` 노드의 `DisplayName`
- **PackageName**: `Identity` 노드의 `Name`
- **VersionString** – `Identity` 노드의 `Version`


--------------

## <a name="api"></a>API

- [AppInfo 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo API 문서](xref:Xamarin.Essentials.AppInfo)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/App-Information-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
