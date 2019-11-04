---
title: Xamarin.ios 맵
description: 지도 컨트롤은 지도를 표시 하 고 Xamarin.ios NuGet 패키지가 필요 합니다.
ms.prod: xamarin
ms.assetid: B669B5EE-D24C-4C69-93E1-2CA5CC9108B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
ms.openlocfilehash: 013e126b76de08442327707cd0502f207826dad8
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425593"
---
# <a name="xamarinforms-map"></a>Xamarin.ios 맵

## <a name="initialization-and-configurationsetupmd"></a>[초기화 및 구성](setup.md)

응용 프로그램에서 maps 기능을 사용 하려면 [Xamarin.ios](https://www.nuget.org/packages/Xamarin.Forms.Maps/) NuGet 패키지가 필요 합니다. 또한 사용자의 위치에 액세스 하려면 응용 프로그램에 대 한 위치 권한이 있어야 합니다.

## <a name="map-controlmapmd"></a>[맵 컨트롤](map.md)

[`Map`](xref:Xamarin.Forms.Maps.Map) 컨트롤은 지도를 표시 하 고 주석을 추가 하는 플랫폼 간 뷰입니다. 각 플랫폼에 대해 네이티브 맵 컨트롤을 사용 하 여 사용자에 게 빠르고 친숙 한 지도 환경을 제공 합니다.

## <a name="position-and-distanceposition-distancemd"></a>[위치 및 거리](position-distance.md)

[`Position`](xref:Xamarin.Forms.Maps.Position) 구조체는 일반적으로 지도와 해당 핀을 배치할 때 사용 되며, 지도를 배치할 때 선택적으로 사용할 수 있는 [`Distance`](xref:Xamarin.Forms.Maps.Distance) 구조체를 사용 합니다.

## <a name="pinspinsmd"></a>[핀](pins.md)

[`Map`](xref:Xamarin.Forms.Maps.Map) 컨트롤을 사용 하면 위치를 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 개체로 표시할 수 있습니다. `Pin`은 탭 할 때 정보 창을 여는 지도 표식입니다.

## <a name="polygons-and-polylinespolygonsmd"></a>[다각형 및 폴리라인](polygons.md)

`Polygon` 및 `Polyline` 요소를 사용 하면 지도의 특정 영역을 강조 표시할 수 있습니다. `Polygon`는 스트로크 및 채우기 색을 가질 수 있는 완전히 둘러싸인 도형입니다. `Polyline`는 영역을 완전히 묶지 않는 줄입니다.

## <a name="geocodinggeocodermd"></a>[지오코딩](geocoder.md)

[`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) 클래스는 [`Position`](xref:Xamarin.Forms.Maps.Position) 개체에 저장 되어 있는 문자열 주소와 위도 및 경도 좌표 간을 변환 합니다.

## <a name="launch-the-native-map-appnative-map-appmd"></a>[네이티브 맵 앱 시작](native-map-app.md)

각 플랫폼의 네이티브 맵 앱은 xamarin.ios `Launcher` 클래스를 통해 Xamarin.ios 응용 프로그램에서 시작할 수 있습니다.
