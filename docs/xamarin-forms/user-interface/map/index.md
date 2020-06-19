---
title: Xamarin.Forms매핑할
description: 지도 컨트롤은 지도를 표시 하 고가 필요 합니다 Xamarin.Forms . NuGet 패키지를 매핑합니다.
ms.prod: xamarin
ms.assetid: B669B5EE-D24C-4C69-93E1-2CA5CC9108B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2461ffa8168207e6a57fae005f752be48772a34a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139830"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms매핑할

## <a name="initialization-and-configuration"></a>[초기화 및 구성](setup.md)

[ Xamarin.Forms 입니다. Maps](https://www.nuget.org/packages/Xamarin.Forms.Maps/) NuGet 패키지는 응용 프로그램에서 maps 기능을 사용 하는 데 필요 합니다. 또한 사용자의 위치에 액세스 하려면 응용 프로그램에 대 한 위치 권한이 있어야 합니다.

## <a name="map-control"></a>[맵 컨트롤](map.md)

[`Map`](xref:Xamarin.Forms.Maps.Map)컨트롤은 지도를 표시 하 고 주석을 추가 하는 플랫폼 간 뷰입니다. 각 플랫폼에 대해 네이티브 맵 컨트롤을 사용 하 여 사용자에 게 빠르고 친숙 한 지도 환경을 제공 합니다.

## <a name="position-and-distance"></a>[위치 및 거리](position-distance.md)

[`Position`](xref:Xamarin.Forms.Maps.Position)구조체는 일반적으로 지도와 해당 핀을 배치할 때 사용 되며, [`Distance`](xref:Xamarin.Forms.Maps.Distance) 지도를 배치할 때 선택적으로 사용할 수 있는 구조체를 사용 합니다.

## <a name="pins"></a>[핀](pins.md)

컨트롤을 사용 하면 [`Map`](xref:Xamarin.Forms.Maps.Map) 위치가 개체로 표시 될 수 있습니다 [`Pin`](xref:Xamarin.Forms.Maps.Pin) . 는 `Pin` 탭 할 때 정보 창을 여는 지도 표식입니다.

## <a name="polygons-polylines-and-circles"></a>[다각형, 폴리라인 및 원](polygons.md)

`Polygon`, `Polyline` 및 `Circle` 요소를 사용 하면 지도의 특정 영역을 강조할 수 있습니다. 는 `Polygon` 스트로크 및 채우기 색을 가질 수 있는 완전히 둘러싸인 도형입니다. 는 `Polyline` 영역을 완전히 묶지 않는 선입니다. 는 `Circle` 지도의 원형 영역을 강조 표시 합니다.

## <a name="geocoding"></a>[지오코딩](geocoder.md)

[`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder)클래스는 개체에 저장 되는 위도 및 경도 좌표와 문자열 주소를 변환 합니다 [`Position`](xref:Xamarin.Forms.Maps.Position) .

## <a name="launch-the-native-map-app"></a>[기본 지도 앱 시작](native-map-app.md)

각 플랫폼의 네이티브 맵 앱은 클래스를 통해 응용 프로그램에서 시작할 수 있습니다 Xamarin.Forms Xamarin.Essentials `Launcher` .
