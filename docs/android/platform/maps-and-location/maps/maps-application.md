---
title: Maps 애플리케이션 시작
description: Xamarin.Android 앱 내에서 기본 제공 Maps 애플리케이션을 시작하는 방법입니다.
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: 7b74f564f2b6e9613874a774258a7e999002e61a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027074"
---
# <a name="launching-the-maps-application"></a>Maps 애플리케이션 시작

Xamarin.Android에서 지도를 사용하는 가장 간단한 방법은 아래에 표시된 기본 제공 지도 애플리케이션을 활용하는 것입니다.

[![기본 제공 Google Maps 앱의 예제 스크린샷](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

지도 애플리케이션을 사용하는 경우 지도가 애플리케이션에 포함되지 않습니다. 대신 애플리케이션이 지도 애플리케이션을 시작하고 지도를 외부에서 로드합니다. 다음 섹션에서는 Xamarin.Android를 사용하여 위와 같은 지도를 시작하는 방법을 살펴봅니다.

## <a name="creating-the-intent"></a>의도 만들기

지도 애플리케이션 작업은 적절한 URI를 사용하여 의도를 만들고, 동작을 ActionView로 설정하고, StartActivity 메서드를 호출하는 등 쉽게 수행할 수 있습니다. 예를 들어 다음 코드는 지정된 위도 및 경도로 중심이 맞춰진 지도 애플리케이션을 시작합니다.

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

이 코드는 이전 스크린샷에 표시된 지도를 시작하는 데 필요합니다. 지도의 URI 체계는 위도 및 경도를 지정하는 것 외에 다른 여러 옵션을 지원합니다.

## <a name="geo-uri-scheme"></a>지역 URI 체계

위의 코드는 URI를 만들 때 지역 체계를 사용했습니다. 이 URI 체계는 다음과 같은 몇 가지 형식을 지원합니다.

- `geo:latitude,longitude` &ndash; 위도/경도로 중심이 맞춰진 지도 애플리케이션을 엽니다. 

- `geo:latitude,longitude?z=zoom` &ndash; 위도/경도로 중심이 맞춰져 있고 지정된 수준으로 확대/축소된 지도 애플리케이션을 엽니다. 확대/축소 수준의 범위는 1에서 23 사이입니다. 1은 지구 전체를 표시하며 23은 가장 가까운 확대/축소 수준입니다.

- `geo:0,0?q=my+street+address` &ndash; 지도 애플리케이션에서 특정 도로명 주소 위치를 엽니다. 

- `geo:0,0?q=business+near+city` &ndash; 지도 애플리케이션을 열고 주석이 추가된 검색 결과를 표시합니다. 

쿼리를 사용하는 URI 버전(즉, 도로명 주소 또는 검색어)은 Google의 geocoder 서비스를 사용하여 지도에 표시되는 위치를 검색합니다. 예를 들어 URI `geo:0,0?q=coop+Cambridge`는 다음과 같은 지도를 표시합니다.

[![검색어로 Google Maps를 보여주는 예제 스크린샷](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)

지역 URI 체계에 대한 자세한 내용은 [지도에 위치 표시](https://developer.android.com/guide/components/intents-common.html#Maps)를 참조하세요.

## <a name="street-view"></a>거리 뷰

Android는 지역 체계 외에 의도에서 거리 뷰를 로드하는 기능도 지원합니다. Xamarin.Android에서 시작된 거리 뷰 애플리케이션의 예는 다음과 같습니다.

[![거리 뷰의 예제 스크린샷](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

거리 뷰를 시작하려면 다음 코드에 나와 있는 것처럼 `google.streetview` URI 체계를 사용하면 됩니다.

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

위에서 사용된 google.streetview URI 체계는 다음과 같은 형식을 사용합니다.

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

아시다시피 다음과 같은 몇 가지 매개 변수가 지원됩니다.

- `lat` &ndash; 거리 뷰에 표시할 위치의 위도입니다.

- `lng` &ndash; 거리 뷰에 표시할 위치의 경도입니다.

- `pitch` &ndash; 가운데에서 측정한 거리 뷰 파노라마 앵글입니다. 90도는 똑바르게 아래로, -90도는 똑바르게 위로 보는 앵글입니다.

- `yaw` &ndash; 북쪽에서 시계 방향으로 측정한 거리 뷰 파노라마의 중심점입니다.

- `zoom` &ndash; 거리 뷰 파노라마의 확대/축소 승수입니다. 1.0 = 보통 확대/축소, 2.0 = 2배 확대/축소, 3.0 = 4배 확대/축소 등입니다.

- `mz` &ndash; 거리 뷰에서 지도 애플리케이션으로 갈 때 사용될 지도 확대/축소 수준입니다.

기본 제공 지도 애플리케이션이나 거리 뷰로 작업하는 것은 매핑 지원을 빠르게 추가하는 간편한 방법입니다. 그러나 Android의 Maps API를 통해 매핑 환경을 보다 세부적으로 제어할 수 있습니다.
