---
title: 'Xamarin.Essentials: 응용 프로그램 정보'
description: 이 문서에서 응용 프로그램에 대 한 정보를 제공 하는 Xamarin.Essentials AppInfo 클래스를 설명 합니다. 예를 들어 응용 프로그램 이름 및 버전을 노출합니다.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 25765fbbcbd2475bfcbc72ca5c4feefa8ef19d08
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782107"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: 응용 프로그램 정보

![시험판 NuGet](~/media/shared/pre-release.png)

**AppInfo** 클래스 응용 프로그램에 대 한 정보를 제공 합니다.

## <a name="using-appinfo"></a>AppInfo를 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

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

## <a name="api"></a>API

- [AppInfo 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo API 설명서](xref:Xamarin.Essentials.AppInfo)
