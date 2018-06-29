---
title: 'Xamarin.Essentials: 응용 프로그램 정보'
description: 이 문서에서 응용 프로그램에 대 한 정보를 제공 하는 Xamarin.Essentials AppInfo 클래스를 설명 합니다. 예를 들어 응용 프로그램 이름 및 버전을 노출합니다.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e79b3003f41b8de22950624e44e8c9e0e7e7e31
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080276"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: 응용 프로그램 정보

![시험판 NuGet](~/media/shared/pre-release.png)

**AppInfo** 클래스 응용 프로그램에 대 한 정보를 제공 합니다.

## <a name="using-appinfo"></a>AppInfo를 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>응용 프로그램 정보 가져오기:

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

**AppInfo** 클래스 응용 프로그램에 대 한 운영 체제에서 유지 관리 하는 설정의 페이지를 표시할 수도 있습니다.

```csharp
// Display settings page
AppInfo.OpenSettings();
```

이 설정 페이지 응용 프로그램 사용 권한 변경 하 고 다른 플랫폼 특정 태스크를 수행할 수 있습니다.

## <a name="api"></a>API

- [AppInfo 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo API 설명서](xref:Xamarin.Essentials.AppInfo)
