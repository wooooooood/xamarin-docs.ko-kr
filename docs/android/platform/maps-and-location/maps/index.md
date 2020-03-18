---
title: Xamarin.Android에서 Google 맵 및 위치를 사용하는 방법
description: 이 문서에서는 Xamarin.Android에서 맵과 위치를 사용하는 방법에 대해 설명합니다. 기본 제공 맵 애플리케이션을 활용하는 것에서부터 Google Maps Android API V2를 직접 사용하는 것까지 모든 것을 다룹니다.
ms.prod: xamarin
ms.assetid: 425E0ED2-5380-6EBE-7059-256B6E9128B8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: d877f415bb96024bb41edc2be9aec108ae248e88
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020039"
---
# <a name="how-to-use-google-maps-and-location-with-xamarinandroid"></a>Xamarin.Android에서 Google 맵 및 위치를 사용하는 방법

_이 문서에서는 Xamarin.Android에서 맵과 위치를 사용하는 방법에 대해 설명합니다. 기본 제공 맵 애플리케이션을 활용하는 것에서부터 Google Maps Android API V2를 직접 사용하는 것까지 모든 것을 다룹니다._

## <a name="maps-overview"></a>맵 개요

매핑 기술은 모바일 디바이스에 대한 유비쿼터스 보완 기술입니다. 데스크톱 컴퓨터와 랩톱은 위치 인식을 기본 제공하지 않는 경향이 있습니다. 반면에 모바일 디바이스는 이러한 애플리케이션을 사용하여 디바이스를 찾고 변경된 위치 정보를 표시합니다. Android에는 디바이스에서 사용할 수 있는 위치 하드웨어를 사용하여 맵에 위치 데이터를 표시하는 강력한 기본 제공 기술이 있습니다. 이 문서에서는 다음을 포함하여 Xamarin.Android에서 제공하는 맵 애플리케이션에 대한 스펙트럼을 다룹니다. 

- 기본 제공 맵 애플리케이션을 사용하여 매핑 기능을 신속하게 추가합니다.
- 맵 API를 사용하여 맵의 표시를 제어합니다.
- 다양한 기술을 사용하여 그래픽 오버레이를 추가합니다.

이 섹션의 항목에서는 다양한 매핑 기능을 다룹니다.
먼저 Android의 기본 제공 맵 애플리케이션을 활용하는 방법과 위치의 파노라마 위치 보기를 표시하는 방법을 설명합니다. 그런 다음, 맵 API를 사용하여 애플리케이션 내에서 직접 매핑 기능을 통합하는 방법, 맵의 위치와 표시를 제어하는 방법 및 그래픽 오버레이를 추가하는 방법을 설명합니다.

## <a name="related-links"></a>관련 링크

- [MapsAndLocationDemo_v3(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/mapsandlocationdemo-v3)
- [작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Google Maps API 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [의도 목록: Android 디바이스에서 Google 애플리케이션 호출](https://developer.android.com/guide/appendix/g-app-intents.html)
- [위치 및 지도](https://developer.android.com/guide/topics/location/index.html)
