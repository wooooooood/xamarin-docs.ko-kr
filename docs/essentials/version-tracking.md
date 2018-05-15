---
title: Xamarin.Essentials 버전 추적
description: VersionTracking 클래스를 사용 하면 응용 프로그램 버전을 확인 및 빌드 번호 첫 번째 경우 처럼 이러한 추가 정보를 보면 응용 프로그램이 적이 시작 시간 또는 최신 버전은 이전 빌드 정보, 및을 가져올 합니다.
ms.assetid: 670C7E8A-E882-4AC0-97D2-A53D90ADD6A3
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f6ab63c44307fca860ccb73744b35c006f25a9ed
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-version-tracking"></a>Xamarin.Essentials 버전 추적

![시험판 NuGet](~/media/shared/pre-release.png)

**VersionTracking** 클래스를 사용 하면 응용 프로그램 버전을 확인 및 빌드 번호는 첫 번째 하는 경우 이러한 추가 정보를 표시와 함께 응용 프로그램이 적이 시작 시간 또는 최신 버전은 이전 가져오기 빌드 정보 및 더 합니다.

## <a name="using-version-tracking"></a>버전 추적을 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

처음 사용할 때의 **VersionTracking** 클래스 현재 버전을 추적 시작 됩니다. 호출 해야 `Track` 초기 현재 버전 정보를 추적 하도록 로드 될 때마다 응용 프로그램에:

```csharp
VersionTracking.Track();
```

초기 후 `Track` 라고 버전 정보를 읽을 수 있습니다.

```csharp

// First time ever launched application
var firstLaunch = VersionTracking.IsFirstLaunchEver;

// First time launching current version
var firstLaunchCurrent = VersionTracking.IsFirstLaunchForCurrentVersion;

// First time launching current build
var firstLaunchBuild = VersionTracking.IsFirstLaunchForCurrentBuild;

// Current app version (2.0.0)
var currentVersion = VersionTracking.CurrentVersion;

// Current build (2)
var currentBuild = VersionTracking.CurrentBuild;

// Previous app version (1.0.0)
var previousVersion = VersionTracking.PreviousVersion;

// Previous app build (1)
var previousBuild = VersionTracking.PreviousBuild;

// First version of app installed (1.0.0)
var firstVersion = VersionTracking.FirstInstalledVersion;

// First build of app installed (1)
var firstBuild = VersionTracking.FirstInstalledBuild;

// List of versions installed (1.0.0, 2.0.0)
var versionHistory = VersionTracking.VersionHistory;

// List of builds installed (1, 2)
var buildHistory = VersionTracking.BuildHistory;
```

## <a name="platform-implementation-specifics"></a>플랫폼 구현 세부 사항

모든 버전 정보를 사용 하 여 저장 됩니다는 [기본 설정](preferences.md) Xamarin.Essentials API의 파일 이름으로 저장 하 고 **[귀하가-응용 프로그램-패키지-ID].xamarinessentials**합니다.

응용 프로그램 제거는 _LocalSettings_, 및 추적 정보를 삭제 하는 모든 버전.

## <a name="api"></a>API

- [버전 추적 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/VersionTracking)
- [버전 추적 API 설명서](xref:Xamarin.Essentials.VersionTracking)
