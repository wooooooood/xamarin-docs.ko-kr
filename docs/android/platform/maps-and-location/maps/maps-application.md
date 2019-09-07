---
title: Maps 응용 프로그램 시작
description: Xamarin Android 앱 내에서 기본 제공 맵 응용 프로그램을 시작 하는 방법입니다.
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: b950326eb5a124d5040caa0044309630a2a53d38
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761667"
---
# <a name="launching-the-maps-application"></a>Maps 응용 프로그램 시작

Xamarin.ios에서 maps를 사용 하는 가장 간단한 방법은 아래에 표시 된 기본 제공 맵 응용 프로그램을 활용 하는 것입니다.

[![기본 제공 Google Maps 앱의 예제 스크린샷](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

Maps 응용 프로그램을 사용 하는 경우 맵은 응용 프로그램에 포함 되지 않습니다. 대신 응용 프로그램은 맵 응용 프로그램을 시작 하 고 맵을 외부적으로 로드 합니다. 다음 섹션에서는 Xamarin.ios를 사용 하 여 위와 같은 지도를 시작 하는 방법을 살펴봅니다.

## <a name="creating-the-intent"></a>의도 만들기

맵 응용 프로그램 작업은 적절 한 URI를 사용 하 여 의도를 만들고, 작업을 ActionView로 설정 하 고, StartActivity 메서드를 호출 하는 것 만큼 쉽습니다. 예를 들어 다음 코드는 지정 된 위도 및 경도를 중심으로 맵 응용 프로그램을 시작 합니다.

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

이 코드는 이전 스크린샷에 표시 된 맵을 시작 하는 데 필요 합니다. 위도 및 경도를 지정 하는 것 외에도 맵에 대 한 URI 체계는 다른 여러 옵션을 지원 합니다.

## <a name="geo-uri-scheme"></a>지역 URI 체계

위의 코드는 지역 체계를 사용 하 여 URI를 만듭니다. 이 URI 체계는 다음과 같이 몇 가지 형식을 지원 합니다.

- `geo:latitude,longitude`&ndash; Lat/lon를 중심으로 맵 응용 프로그램을 엽니다. 

- `geo:latitude,longitude?z=zoom`&ndash; Lat/lon를 중심으로 맵 응용 프로그램을 열고 지정 된 수준으로 확대/축소 합니다. 확대/축소 수준의 범위는 1에서 23 사이입니다. 1은 전체 지구를 표시 하 고 23은 가장 가까운 확대/축소 수준입니다.

- `geo:0,0?q=my+street+address`&ndash; 지도 응용 프로그램을 여는 주소의 위치를 엽니다. 

- `geo:0,0?q=business+near+city`&ndash; Maps 응용 프로그램을 열고 주석이 추가 된 검색 결과를 표시 합니다. 

쿼리를 사용 하는 URI 버전 (즉, 주소 또는 검색어)은 Google의 geocoder 서비스를 사용 하 여 맵에 표시 되는 위치를 검색 합니다. 예를 들어, URI `geo:0,0?q=coop+Cambridge` 는 아래와 같은 맵을 생성 합니다.

[![검색 단어를 포함 하는 Google 지도를 보여 주는 예제 스크린샷](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)

지역 URI 체계에 대 한 자세한 내용은 [지도에 위치 표시](https://developer.android.com/guide/components/intents-common.html#Maps)를 참조 하세요.

## <a name="street-view"></a>주소 보기

Android는 지역 구성표 외에도 의도에서 거리 보기를 로드 하는 것을 지원 합니다. Xamarin.ios에서 시작 된 응용 프로그램 보기 응용 프로그램의 예는 다음과 같습니다.

[![주소 보기의 예제 스크린샷](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

주소 보기를 시작 하려면 다음 코드에 나와 `google.streetview` 있는 것 처럼 URI 체계를 사용 하면 됩니다.

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

위에서 사용 되는 google. streetview URI 체계는 다음과 같은 형식을 사용 합니다.

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

여기에 나와 있는 것 처럼 다음과 같이 지원 되는 몇 가지 매개 변수가 있습니다.

- `lat`&ndash; 주소 보기에 표시할 위치의 위도입니다.

- `lng`&ndash; 주소 보기에 표시할 위치의 경도입니다.

- `pitch`&ndash; 가운데에서 90도를 똑바로 하 고-90도를 수직으로 측정 한 거리 보기 파노라마 (도)입니다.

- `yaw`&ndash; 왼쪽에서 시계 방향으로 측정 한 거리 보기 파노라마 (왼쪽)입니다.

- `zoom`&ndash; 거리 확대/축소 승수, 즉 1.0 = 일반 확대/축소, 2.0 = 확대 2, 3.0 = 확대/축소

- `mz`&ndash; 지도 응용 프로그램으로 이동 하는 데 사용 되는 지도 확대/축소 수준입니다.

기본 제공 맵 응용 프로그램 또는 주소 보기를 사용 하면 매핑 지원을 신속 하 게 추가할 수 있습니다. 그러나 Android의 Maps API를 통해 매핑 환경을 보다 세부적으로 제어할를 제공 합니다.
