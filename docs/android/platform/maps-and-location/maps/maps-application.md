---
title: 지도 응용 프로그램 시작
description: 기본 제공 맵 내 응용 프로그램에서 Xamarin.Android 앱을 시작 하는 방법입니다.
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: fa32783617fce99514560677184f17be904cd42d
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670328"
---
# <a name="launching-the-maps-application"></a>지도 응용 프로그램 시작

Xamarin.Android에는 maps를 사용 하는 가장 간단한 방법은 다음과 같습니다. 아래에 표시 된 기본 제공 지도 응용 프로그램을 활용 하 여

[![기본 제공 Google Maps 앱의 스크린샷 예제](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

지도 응용 프로그램을 사용 하면 지도 응용 프로그램의 일부가 되지 않습니다. 대신 응용 프로그램 맵 응용 프로그램을 시작 하 고 외부에서 지도 로드 됩니다. 다음 섹션에서는 Xamarin.Android를 사용 하 여 위와 같은 지도 시작 하는 방법을 검사 합니다.


## <a name="creating-the-intent"></a>의도 만들기

맵 작업 응용 프로그램은 적절 한 URI를 사용 하 여 의도 만들기, 작업, ActionView을 설정 및 StartActivity 메서드를 호출 하는 것 만큼 쉽습니다. 예를 들어, 다음 코드 가운데에 지정 된 위도 및 경도 maps 응용 프로그램을 시작 합니다.

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

이 코드는 이전 스크린샷에 표시 된 맵 시작 하는 데 필요한 모든입니다. 위도 및 경도 지정 하는 것 외에도 지도 대 한 URI 체계는 다른 몇 가지 옵션을 지원 합니다.


## <a name="geo-uri-scheme"></a>지역 URI 체계

위의 코드는 URI를 만듭니다 지역 구성표를 사용 합니다. 이 URI 체계는 아래와 같은 여러 가지 형식을으로 지원 합니다.

-   `geo:latitude,longitude` &ndash; 지도 응용 프로그램을 중심에 위도/경도 엽니다. 

-   `geo:latitude,longitude?z=zoom` &ndash; 응용 프로그램의 중심에 위도/경도 및 지정된 된 수준으로 확대 하는 맵을 엽니다. 확대/축소 수준 범위는 1에서 23: 1 전체 지구를 표시 하 고 23 가장 가까운 확대/축소 수준입니다.

-   `geo:0,0?q=my+street+address` &ndash; 주소의 위치를 지도 응용 프로그램을 엽니다. 

-   `geo:0,0?q=business+near+city` &ndash; Maps 응용 프로그램을 열고 주석이 추가 된 검색 결과 표시 합니다. 


그런 다음 지도에 표시 되는 위치를 검색할 Google의 geocoder 서비스를 사용 하는 쿼리 (즉 거리 주소 또는 검색 용어)를 사용 하는 URI의 버전입니다. 예를 들어, URI `geo:0,0?q=coop+Cambridge` 아래에 표시 된 맵에 결과:

[![Google Maps 검색 용어를 사용 하 여 보여 주는 예제 스크린 샷](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)



지역 URI 체계에 대 한 자세한 내용은 참조 하세요. [지도에 위치 표시](https://developer.android.com/guide/components/intents-common.html#Maps)합니다.


## <a name="street-view"></a>거리 보기

지역 구성표 외에도 Android 의도에서 거리 보기를 로드도 지원 합니다. Xamarin.Android에서 시작 하는 거리 보기 응용 프로그램의 예는 아래 나와 있습니다.

[![거리 보기의 스크린샷 예제](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

거리 보기를 시작 하려면 사용 된 `google.streetview` URI 체계를 다음 코드 에서처럼:

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

위에서 사용 된 google.streetview URI 구성표는 다음 형식을 사용 합니다.

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

알 수 있듯이 아래에 나열 된 지원 되는 여러 매개 변수를 가지 있습니다.

-   `lat` &ndash; 거리 보기에 표시 될 위치의 위도입니다.

-   `lng` &ndash; 거리 보기에 표시 될 위치의 경도입니다.

-   `pitch` &ndash; 거리 보기 panorama, 센터 90도 아래로 직선 되 고 90도 측정 한 각도 수직으로 됩니다.

-   `yaw` &ndash; 거리 보기 파노라마의 보기 가운데 North 로부터 단위로 시계 방향으로 측정 합니다.

-   `zoom` &ndash; 파노라마 거리 보기에 대 한 승수를 확대/축소, 1.0 = 일반 확대/축소, 2.0 확대/축소 된 2 = 3.0 x = 확대/축소 된 4 x, 등입니다.

-   `mz` &ndash; 거리 보기에서 맵 응용 프로그램으로 이동 하는 경우 사용할 지도 확대/축소 수준입니다.


기본 제공 작업 응용 프로그램 매핑하거나 거리 보기는 신속 하 게 매핑 지원을 추가 하는 간편한 방법입니다. 그러나 Android의 Maps API 매핑 환경 보다 세부적으로 제어를 제공합니다.
