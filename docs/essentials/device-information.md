---
title: 'Xamarin.Essentials: 장치 정보'
description: 이 문서에서는 응용 프로그램이 실행 중인 장치에 대한 정보를 제공하는 Xamarin.Essentials의 DeviceInfo 클래스를 설명합니다.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 530b04446703d78452357b2c9f9089e59ebf6e6c
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674815"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials: 장치 정보

![시험판 NuGet](~/media/shared/pre-release.png)

**DeviceInfo** 클래스는 응용 프로그램이 실행 중인 장치에 대한 정보를 제공합니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-deviceinfo"></a>DeviceInfo 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

다음 정보는 API를 통해 표시됩니다.

```csharp
// Device Model (SMG-950U, iPhone10,6)
var device = DeviceInfo.Model;

// Manufacturer (Samsung)
var manufacturer = DeviceInfo.Manufacturer;

// Device Name (Motz's iPhone)
var deviceName = DeviceInfo.Name;

// Operating System Version Number (7.0)
var version = DeviceInfo.VersionString;

// Platform (Android)
var platform = DeviceInfo.Platform;

// Idiom (Phone)
var idiom = DeviceInfo.Idiom;

// Device Type (Physical)
var deviceType = DeviceInfo.DeviceType;
```

## <a name="platformsxrefxamarinessentialsdeviceinfoplatforms"></a>[플랫폼](xref:Xamarin.Essentials.DeviceInfo.Platforms)

`DeviceInfo.Platform`은 운영 체제에 매핑되는 상수 문자열에 상호 연결됩니다. 값은 `Platforms` 클래스를 사용하여 확인할 수 있습니다.

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** – Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** - 지원되지 않음

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Idioms](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom`은 응용 프로그램이 실행 중인 장치 유형에 매핑되는 상수 문자열에 상호 연결됩니다. 값은 `Idioms` 클래스를 사용하여 확인할 수 있습니다.

- **DeviceInfo.Idioms.Phone** - 전화
- **DeviceInfo.Idioms.Tablet** – 태블릿
- **DeviceInfo.Idioms.Desktop** – 데스크톱
- **DeviceInfo.Idioms.TV** – TV
- **DeviceInfo.Idioms.Unsupported** - 지원되지 않음

## <a name="device-type"></a>장치 유형

`DeviceInfo.DeviceType`은 응용 프로그램이 물리적 또는 가상 장치에서 실행 중인지 판별하는 열거형에 상호 연결됩니다. 가상 장치는 시뮬레이터 또는 에뮬레이터입니다.

## <a name="platform-implementation-specifics"></a>플랫폼 구현 관련 정보

# <a name="iostabios"></a>[iOS](#tab/ios)

iOS는 개발자가 특정 iOS 장치의 이름을 가져올 수 있는 API를 노출하지 않습니다. 대신에 iPhone X를 참조하는 _iPhone10,6_과 같은 하드웨어 식별자가 반환됩니다. 이러한 식별자의 매핑은 Apple에서 제공하지 않지만 [iPhone Wiki](https://www.theiphonewiki.com/wiki/Models)(비공식 소스)에서 찾을 수 있습니다.

--------------

## <a name="api"></a>API

- [DeviceInfo 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [DeviceInfo API 문서](xref:Xamarin.Essentials.DeviceInfo)
