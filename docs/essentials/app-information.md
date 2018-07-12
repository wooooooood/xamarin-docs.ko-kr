---
title: 'Xamarin.Essentials: 앱 정보'
description: 이 문서에서 응용 프로그램에 대 한 정보를 제공 하는 Xamarin.Essentials AppInfo 클래스를 설명 합니다. 예를 들어, 앱 이름 및 버전을 노출합니다.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e79b3003f41b8de22950624e44e8c9e0e7e7e31
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831509"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: 앱 정보

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **AppInfo** 클래스는 응용 프로그램에 대 한 정보를 제공 합니다.

## <a name="using-appinfo"></a>AppInfo를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>응용 프로그램 정보 얻기:

다음 정보는 API를 통해 노출 됩니다.

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

합니다 **AppInfo** 클래스는 응용 프로그램에 대 한 운영 체제에서 유지 관리 하는 설정 페이지를 표시할 수도 있습니다.

```csharp
// Display settings page
AppInfo.OpenSettings();
```

이 설정 페이지에서는 사용자가 응용 프로그램 사용 권한 변경를 다른 플랫폼 관련 작업을 수행 합니다.

## <a name="api"></a>API

- [AppInfo 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo API 설명서](xref:Xamarin.Essentials.AppInfo)
