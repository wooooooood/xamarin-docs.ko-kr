---
title: 'Xamarin.Essentials: 앱 정보'
description: 이 문서에서는 응용 프로그램에 대한 정보를 제공하는 Xamarin.Essentials의 AppInfo 클래스를 설명합니다. 예를 들어 앱 이름과 버전을 표시합니다.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 62ecf890ba6b3276db89e93c2699e0406dbe4d45
ms.sourcegitcommit: a635312ffec816ba357a92b66c8c5221c8d9044c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50965619"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: 앱 정보

![시험판 NuGet](~/media/shared/pre-release.png)

**AppInfo** 클래스는 응용 프로그램에 대한 정보를 제공합니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-appinfo"></a>AppInfo 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>응용 프로그램 정보 가져오기

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

## <a name="displaying-application-settings"></a>응용 프로그램 설정 표시

**AppInfo** 클래스는 운영 체제에서 유지 관리하는 응용 프로그램의 설정 페이지도 표시할 수 있습니다.

```csharp
// Display settings page
AppInfo.ShowSettingsUI();
```

이 설정 페이지에서는 사용자가 응용 프로그램 권한을 변경하고 기타 플랫폼 관련 작업을 수행할 수 있습니다.

## <a name="api"></a>API

- [AppInfo 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo API 문서](xref:Xamarin.Essentials.AppInfo)
