---
title: Xamarin.ios 맵 위치 및 거리
description: Xamarin.ios 네임 스페이스는 지도와 해당 핀을 배치할 때 일반적으로 사용 되는 위치 구조체와 지도를 배치할 때 선택적으로 사용할 수 있는 거리 구조체를 포함 합니다.
ms.prod: xamarin
ms.assetid: 2F4EA3D2-1351-40AD-A71D-CF7F1F18F1E8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/30/2019
ms.openlocfilehash: a68eab7bb3e6da764f5a35af4461d6af2be50ebe
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426284"
---
# <a name="xamarinforms-map-position-and-distance"></a>Xamarin.ios 맵 위치 및 거리

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) 네임 스페이스에는 맵과 해당 핀을 배치할 때 일반적으로 사용 되는 [`Position`](xref:Xamarin.Forms.Maps.Position) 구조체와 지도를 배치할 때 선택적으로 사용할 수 있는 [`Distance`](xref:Xamarin.Forms.Maps.Distance) 구조체가 포함 되어 있습니다.

## <a name="position"></a>위치

[`Position`](xref:Xamarin.Forms.Maps.Position) 구조체는 위도 및 경도 값으로 저장 된 위치를 캡슐화 합니다. 이 구조체는 두 개의 읽기 전용 속성을 정의 합니다.

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)는 10 진수도 위치의 위도를 나타내는 `double`형식의입니다.
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)는 10 진수 각도로 위치의 경도를 나타내는 `double`형식의입니다.

[`Position`](xref:Xamarin.Forms.Maps.Position) 개체는 `Position` 생성자를 사용 하 여 생성 됩니다. 여기에는 `double` 값으로 지정 된 위도 및 경도 인수가 필요 합니다.

```csharp
Position position = new Position(36.9628066, -122.0194722);
```

> [!NOTE]
> `Position` 개체를 만들 때 위도 값은-90.0과 90.0 사이에 고정 경도 값은-180.0과 180.0 사이에 고정 됩니다.

## <a name="distance"></a>거리

[`Distance`](xref:Xamarin.Forms.Maps.Distance) 구조체는 거리를 미터 단위로 나타내는 `double` 값으로 저장 된 거리를 캡슐화 합니다. 이 구조체는 세 개의 읽기 전용 속성을 정의 합니다.

- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)`double`형식으로, `Distance`에 따라 범위를 조정 하는 거리 (킬로미터)를 나타냅니다.
- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)는 `Distance`에 포함 된 거리를 미터 단위로 나타내는 `double`유형입니다.
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)는 `Distance`에 포함 되는 거리 (마일)를 나타내는 `double`유형입니다.

[`Distance`](xref:Xamarin.Forms.Maps.Distance) 개체는 `Distance` 생성자를 사용 하 여 만들 수 있습니다 .이 경우에는 `double`으로 지정 된 미터 인수가 필요 합니다.

```csharp
Distance distance = new Distance(1450.5);
```

또는 [`FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers*), [`FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters*)및 [`FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles*) 팩터리 메서드를 사용 하 여 [`Distance`](xref:Xamarin.Forms.Maps.Distance) 개체를 만들 수 있습니다.

```csharp
Distance distance1 = Distance.FromKilometers(1.45); // argument represents the number of kilometers
Distance distance2 = Distance.FromMeters(1450.5);   // argument represents the number of meters
Distance distance3 = Distance.FromMiles(0.969);     // argument represents the number of miles
```

## <a name="related-links"></a>관련 링크

- [Maps 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
