---
title: 지도 응용 프로그램 시작
description: 기본 제공 지도에서 응용 프로그램 Xamarin.Android 앱 내에서 시작 하는 방법입니다.
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/25/2018
ms.openlocfilehash: d15b6e544f58f03272c711236b579ca568e09539
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935024"
---
# <a name="launching-the-maps-application"></a>지도 응용 프로그램 시작

Xamarin.Android의 지도를 작성 하려면 가장 간단한 방법은 아래에 표시 된 기본 제공 지도 응용 프로그램을 활용 하 여:

[![기본 제공 Google 맵 응용 프로그램의 예제 스크린 샷](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

지도 응용 프로그램을 사용 하는 경우 지도 응용 프로그램의 일부가 되지 않습니다. 대신, 응용 프로그램 맵 응용 프로그램을 시작 하 고 지도 외부적으로 로드 됩니다. 다음 섹션에는 위의 그림과 같이 지도 시작 하기 위해 Xamarin.Android를 사용 하는 방법을 검사 합니다.


## <a name="creating-the-intent"></a>의도 만들기

맵 작업 응용 프로그램은 적합 한 URI를 사용 하 여 프로그램 의도 만들기, 작업, ActionView을 설정 및 StartActivity 메서드를 호출 하기만 합니다. 예를 들어 다음 코드는 특정된 위도 및 경도에서 가운데 지도 응용 프로그램을 시작:

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

이 코드는 이전 스크린 샷에 표시 된 맵 시작 하는 데 필요한 모든. 위도 및 경도 지정 하는 것 외에도 지도 대 한 URI 체계는 다른 여러 가지 옵션을 지원 합니다.


## <a name="geo-uri-scheme"></a>지역 URI 체계

URI를 만드는 데 지역 체계를 사용 하는 위의 코드. 이 URI 체계 아래와 같은 여러 가지 형식으로 지원 합니다.

-   `geo:latitude,longitude` &ndash; Lon lat/중심이 지도 응용 프로그램을 엽니다. 

-   `geo:latitude,longitude?z=zoom` &ndash; 응용 프로그램/lon lat 중심이 및 지정된 된 수준으로 확대/축소 됨 맵을 엽니다. 확대/축소 수준을 범위는 1에서 23: 1 표시는 전체 지구 및 23 가장 가까운 확대/축소 수준입니다.

-   `geo:0,0?q=my+street+address` &ndash; 주소의 위치를 지도 응용 프로그램을 엽니다. 

-   `geo:0,0?q=business+near+city` &ndash; 지도 응용 프로그램 열리고 주석이 추가 된 검색 결과 표시 합니다. 


그런 다음 지도에 표시 되는 위치를 검색할 Google의 geocoder 서비스를 사용 하는 쿼리 (즉 집 주소 또는 검색 용어)을 사용 하는 URI의 버전이 있습니다. 예를 들어 URI `geo:0,0?q=coop+Cambridge` 아래에 표시 된 맵에 결과:

[![검색 용어와 Google 맵을 보여 주는 예제 스크린 샷](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)



지역 URI 구성표에 대 한 자세한 내용은 참조 [지도에 위치를 표시 합니다.](http://developer.android.com/guide/components/intents-common.html#Maps)합니다.


## <a name="street-view"></a>성 보기

지역 구성표 외에도 Android 프로그램 의도에서 번 지 뷰를 로드도 지원 합니다. Xamarin.Android에서 시작 된 거리 보기 응용 프로그램의 예는 다음과 같습니다.

[![거리 보기의 예에서는 스크린 샷](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

거리 보기를 시작 하려면 사용 된 `google.streetview` URI 체계를 다음 코드와 같이:

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

위에서 사용한 google.streetview URI 체계는 다음과 같은 형식을 사용 합니다.

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

볼 수 있듯이 여러 가지 매개 변수가 지원, 아래와 같이:

-   `lat` &ndash; 거리 보기에 표시 되는 위치의 위도입니다.

-   `lng` &ndash; 거리 보기에 표시 되는 위치의 경도입니다.

-   `pitch` &ndash; 90도 다운 직선 도와-90도 중심에서 측정 되는 거리 보기 파노라마의 각도 위로입니다.

-   `yaw` &ndash; 거리 보기 파노라마의 보기 중심의 북부에서도 시계 방향으로 측정 합니다.

-   `zoom` &ndash; 파노라마 거리 보기에 대 한 승수를 확대/축소, 1.0 = 정상 확대/축소, 2.0 = 확대/축소 된 2 3.0 x = 확대/축소 된 4 x, 등입니다.

-   `mz` &ndash; 거리 보기에서 지도 응용 프로그램으로 이동 하는 경우 사용할 수 있는 지도 확대/축소 수준입니다.


기본 제공 작업 응용 프로그램을 매핑하거나 거리 보기는 신속 하 게 매핑 지원을 추가 하는 쉬운 방법입니다. 그러나, Android의 지도 API 매핑 환경 보다 세부적으로 제어를 제공합니다.
