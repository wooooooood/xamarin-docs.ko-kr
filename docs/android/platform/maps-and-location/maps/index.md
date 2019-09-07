---
title: Xamarin Android에서 Google Maps 및 위치를 사용 하는 방법
description: 이 문서에서는 Xamarin. Android에서 맵 및 위치를 사용 하는 방법을 설명 합니다. 기본 제공 맵 응용 프로그램을 활용 하 여 Google Maps Android API v 2를 직접 사용 하는 것부터 모두 다룹니다.
ms.prod: xamarin
ms.assetid: 425E0ED2-5380-6EBE-7059-256B6E9128B8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: 194f82757b0b4cb5e148e06d4303dc0d22afb9b3
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761705"
---
# <a name="how-to-use-google-maps-and-location-with-xamarinandroid"></a>Xamarin Android에서 Google Maps 및 위치를 사용 하는 방법

_이 문서에서는 Xamarin. Android에서 맵 및 위치를 사용 하는 방법을 설명 합니다. 기본 제공 맵 응용 프로그램을 활용 하 여 Google Maps Android API v 2를 직접 사용 하는 것부터 모두 다룹니다._

## <a name="maps-overview"></a>맵 개요

매핑 기술은 모바일 장치에 대 한 다양 한 보수입니다. 데스크톱 컴퓨터와 랩톱은 위치 인식을 기본 제공 하지 않는 경향이 있습니다. 반면에 모바일 장치는 이러한 응용 프로그램을 사용 하 여 장치를 찾고 변경 된 위치 정보를 표시 합니다. Android에는 장치에서 사용할 수 있는 위치 하드웨어를 사용 하 여 지도에 위치 데이터를 표시 하는 강력한 기본 제공 기술이 있습니다. 이 문서에서는 다음을 포함 하 여 Xamarin의 응용 프로그램이 Xamarin에서 제공 해야 하는 사항에 대해 설명 합니다. 

- 기본 제공 맵 응용 프로그램을 사용 하 여 매핑 기능을 신속 하 게 추가 합니다.
- 지도 API를 사용 하 여 지도 표시를 제어 합니다.
- 다양 한 기술을 사용 하 여 그래픽 오버레이를 추가 합니다.

이 단원의 항목에서는 다양 한 매핑 기능을 다룹니다.
첫 번째는 Android의 기본 제공 맵 응용 프로그램을 활용 하는 방법과 위치의 파노라마 위치 보기를 표시 하는 방법을 설명 합니다. 그런 다음 맵 API를 사용 하 여 응용 프로그램 내에서 직접 매핑 기능을 통합 하는 방법, 지도의 위치와 표시를 제어 하는 방법 및 그래픽 오버레이를 추가 하는 방법을 설명 합니다.

## <a name="related-links"></a>관련 링크

- [MapsAndLocationDemo_v3 (sample)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/mapsandlocationdemo-v3)
- [작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Google Maps API 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [의도 목록: Android 장치에서 Google 응용 프로그램 호출](https://developer.android.com/guide/appendix/g-app-intents.html)
- [위치 및 지도](https://developer.android.com/guide/topics/location/index.html)
