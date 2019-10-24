---
title: Xamarin.ios 맵 지 오 코딩
description: 이 문서에서는 Xamarin.ios Geocoder 클래스를 사용 하 여 geocode 및 역방향 geocode 맵 데이터를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: DE7DB31A-8921-4614-8B49-DAEF1E7B03B3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/22/2019
ms.openlocfilehash: ce1f6751c0381ed41058784fbea3ebedefbdac6d
ms.sourcegitcommit: e4c23187874488ff55794d0e81a9bba30d2c2cd6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72778795"
---
# <a name="xamarinforms-map-geocoding"></a>Xamarin.ios 맵 지 오 코딩

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Xamarin.ios 맵은 [`Position`](xref:Xamarin.Forms.Maps.Position) 개체에 저장 된 문자열 주소와 위도 및 경도 좌표 간을 변환 하는 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) 클래스를 제공 합니다.

## <a name="geocode-an-address"></a>Geocode 주소

주소는 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) 인스턴스를 만들고 `Geocoder` 인스턴스에서 [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) 메서드를 호출 하 여 위도 및 경도 좌표에 대 한 지 오 코딩 될 수 있습니다.

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder;

IEnumerable<Position> approximateLocations = await geoCoder.GetPositionsForAddressAsync("Pacific Ave, San Francisco, California");
Position position = approximateLocations.FirstOrDefault();
string coordinates = $"{position.Latitude}, {position.Longitude}";
```

[@No__t_1](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) 메서드는 주소를 나타내는 `string` 인수를 사용 하 고 주소를 나타낼 수 있는 [`Position`](xref:Xamarin.Forms.Maps.Position) 개체의 컬렉션을 비동기적으로 반환 합니다.

## <a name="reverse-geocode-an-address"></a>역방향 geocode 주소

위도 및 경도 좌표는 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) 인스턴스를 만들고 `Geocoder` 인스턴스에서 [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) 메서드를 호출 하 여 주소에 역방향 지 오 코딩 될 수 있습니다.

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder;

Position position = new Position(37.8044866, -122.4324132);
IEnumerable<string> possibleAddresses = await geoCoder.GetAddressesForPositionAsync(position);
string address = possibleAddresses.FirstOrDefault();
```

[@No__t_1](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) 메서드는 위도 및 경도 좌표로 구성 된 [`Position`](xref:Xamarin.Forms.Maps.Position) 인수를 사용 하 고, 위치 근처의 주소를 나타내는 문자열의 컬렉션을 비동기적으로 반환 합니다.

## <a name="related-links"></a>관련 링크

- [Maps 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Geocoder API](xref:Xamarin.Forms.Maps.Geocoder)
