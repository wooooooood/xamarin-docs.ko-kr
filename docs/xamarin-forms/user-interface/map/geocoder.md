---
title: Xamarin.Forms지도 지 오 코딩
description: 이 문서에서는를 사용 하 여 geocode 및 역방향 geocode 맵 데이터를 사용 하는 방법을 설명 합니다 Xamarin.Forms . Maps Geocoder 클래스입니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fe099235857f6bd0531539e3aa84e41bf59b50ba
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139869"
---
# <a name="xamarinforms-map-geocoding"></a>Xamarin.Forms지도 지 오 코딩

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps)네임 스페이스는 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) 개체에 저장 되는 문자열 주소와 위도 및 경도 좌표 간을 변환 하는 클래스를 제공 합니다 [`Position`](xref:Xamarin.Forms.Maps.Position) . 구조체에 대 한 자세한 내용은 [`Position`](xref:Xamarin.Forms.Maps.Position) [지도 위치 및 거리](position-distance.md)를 참조 하세요.

## <a name="geocode-an-address"></a>Geocode 주소

주소는 인스턴스를 만들고 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) 인스턴스에서 메서드를 호출 하 여 위도 및 경도 좌표에 대 한 지 오 코딩 될 수 있습니다 [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) `Geocoder` .

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

IEnumerable<Position> approximateLocations = await geoCoder.GetPositionsForAddressAsync("Pacific Ave, San Francisco, California");
Position position = approximateLocations.FirstOrDefault();
string coordinates = $"{position.Latitude}, {position.Longitude}";
```

[`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*)메서드는 주소를 나타내는 인수를 사용 하 `string` 고 주소를 [`Position`](xref:Xamarin.Forms.Maps.Position) 나타낼 수 있는 개체의 컬렉션을 비동기적으로 반환 합니다.

## <a name="reverse-geocode-an-address"></a>역방향 geocode 주소

위도 및 경도 좌표는 인스턴스를 만들고 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) 인스턴스에서 메서드를 호출 하 여 주소에 역방향 지 오 코딩 될 수 있습니다 [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) `Geocoder` .

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

Position position = new Position(37.8044866, -122.4324132);
IEnumerable<string> possibleAddresses = await geoCoder.GetAddressesForPositionAsync(position);
string address = possibleAddresses.FirstOrDefault();
```

[`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*)메서드는 [`Position`](xref:Xamarin.Forms.Maps.Position) 위도 및 경도 좌표로 구성 된 인수를 사용 하 고, 위치 근처의 주소를 나타내는 문자열의 컬렉션을 비동기적으로 반환 합니다.

## <a name="related-links"></a>관련 링크

- [Maps 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.Forms지도 위치 및 거리](position-distance.md)
- [Geocoder API](xref:Xamarin.Forms.Maps.Geocoder)
