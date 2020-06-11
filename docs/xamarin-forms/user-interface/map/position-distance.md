---
제목: " Xamarin.Forms 지도 위치 및 거리" 설명: " Xamarin.Forms . Maps 네임 스페이스는 지도와 해당 핀의 위치를 지정할 때 일반적으로 사용 되는 위치 구조체와 지도를 배치할 때 선택적으로 사용할 수 있는 거리 구조체를 포함 합니다. "
assetid: 2F4EA3D2-1351-40AD-A71D-CF7F1F18F1E8: xamarin-forms author: davidbritch: dabritch:: 03/10/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-map-position-and-distance"></a>Xamarin.Forms지도 위치 및 거리

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps)네임 스페이스에는 [`Position`](xref:Xamarin.Forms.Maps.Position) 일반적으로 맵과 해당 핀을 배치할 때 사용 되는 구조체와 지도를 [`Distance`](xref:Xamarin.Forms.Maps.Distance) 배치할 때 선택적으로 사용할 수 있는 구조체가 포함 되어 있습니다.

## <a name="position"></a>위치

[`Position`](xref:Xamarin.Forms.Maps.Position)구조체는 위도 및 경도 값으로 저장 된 위치를 캡슐화 합니다. 이 구조체는 두 개의 읽기 전용 속성을 정의 합니다.

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)는 `double` 10 진수도에서 위치의 위도를 나타내는 형식의입니다.
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)는 `double` 10 진수 각도로 위치의 경도를 나타내는 형식의입니다.

[`Position`](xref:Xamarin.Forms.Maps.Position)개체는 생성자를 사용 하 여 생성 됩니다 `Position` . 여기에는 위도 및 경도 인수가 값으로 지정 되어야 합니다 `double` .

```csharp
Position position = new Position(36.9628066, -122.0194722);
```

개체를 만들 때 `Position` 위도 값은-90.0과 90.0 사이에 고정 경도 값은-180.0과 180.0 사이에 고정 됩니다.

> [!NOTE]
> 클래스에는 `GeographyUtils` `ToRadians` 도에서 라디안으로 값을 변환 하는 확장 메서드와 `double` `ToDegrees` `double` 값을 라디안에서 각도로 변환 하는 확장 메서드가 있습니다.

## <a name="distance"></a>Distance

[`Distance`](xref:Xamarin.Forms.Maps.Distance)구조체는 `double` 거리를 미터 단위로 나타내는 값으로 저장 된 거리를 캡슐화 합니다. 이 구조체는 세 개의 읽기 전용 속성을 정의 합니다.

- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)는의 범위에 해당 하는 킬로미터의 거리를 나타내는 형식의입니다 `double` `Distance` .
- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)`double`는의 범위 내 거리를 나타내는 형식의입니다 `Distance` .
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)는의 범위에 해당 하는 거리 (마일)를 `double` 나타내는 형식의입니다 `Distance` .

[`Distance`](xref:Xamarin.Forms.Maps.Distance)`Distance`로 지정 된 미터 인수가 필요한 생성자를 사용 하 여 개체를 만들 수 있습니다 `double` .

```csharp
Distance distance = new Distance(1450.5);
```

또는 [`Distance`](xref:Xamarin.Forms.Maps.Distance) [`FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers*) ,, [`FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters*) [`FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles*) 및 `BetweenPositions` 팩터리 메서드를 사용 하 여 개체를 만들 수 있습니다.

```csharp
Distance distance1 = Distance.FromKilometers(1.45); // argument represents the number of kilometers
Distance distance2 = Distance.FromMeters(1450.5);   // argument represents the number of meters
Distance distance3 = Distance.FromMiles(0.969);     // argument represents the number of miles
Distance distance4 = Distance.BetweenPositions(position1, position2);
```

## <a name="related-links"></a>관련 링크

- [Maps 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
