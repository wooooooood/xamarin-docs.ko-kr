---
title: Xamarin.Essentials 장치 정보
description: DeviceInfo 클래스에서 실행 되 고 응용 프로그램이 장치에 대 한 정보를 제공 합니다.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a398f2697b03cec26f2c786b11e7a3c332cb5f63
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials 장치 정보

![시험판 NuGet](~/media/shared/pre-release.png)

**DeviceInfo** 클래스 응용 프로그램에서 실행 되는 장치에 대 한 정보를 제공 합니다.

## <a name="using-deviceinfo"></a>DeviceInfo를 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

다음 정보는 API를 통해 노출 됩니다.

```csharp
// Device Model (SMG-950U)
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

`DeviceInfo.Platform` 운영 체제에 매핑되는 상수 문자열에 연결 됩니다. 값으로 확인할 수 있습니다는 `Platforms` 클래스:

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** – Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** 지원 되지 않는 –

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[관용구](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` 하면서 안정적일 응용 프로그램 종류의 장치에 매핑되는 상수 문자열에서 실행 됩니다. 값으로 확인할 수 있습니다는 `Idioms` 클래스:

- **DeviceInfo.Idioms.Phone** – 전화
- **DeviceInfo.Idioms.Tablet** – 태블릿
- **DeviceInfo.Idioms.Desktop** – 데스크톱
- **DeviceInfo.Idioms.TV** – TV
- **DeviceInfo.Idioms.Unsupported** 지원 되지 않는 –

## <a name="device-type"></a>장치 유형

`DeviceInfo.DeviceType` 응용 프로그램이 실행 하는 물리적 또는 가상 장치 인지 확인 하는 열거형을 연관 시킵니다. 가상 장치 시뮬레이터 또는 에뮬레이터입니다.

## <a name="api"></a>API

- [DeviceInfo 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [DeviceInfo API 설명서](xref:Xamarin.Essentials.DeviceInfo)
