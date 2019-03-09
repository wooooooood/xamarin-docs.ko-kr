---
title: Xamarin.Android를 사용 하 여 Google Maps 및 위치를 사용 하는 방법
description: 이 문서에서는 Xamarin.Android를 사용 하 여 지도 및 위치를 사용 하는 방법을 설명 합니다. 직접 Google 맵 Android API V2를 사용 하 여 기본 제공 지도 응용 프로그램을 활용 하 여 모든 항목을 설명 합니다.
ms.prod: xamarin
ms.assetid: 425E0ED2-5380-6EBE-7059-256B6E9128B8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
---

# <a name="how-to-use-google-maps-and-location-with-xamarinandroid"></a>Xamarin.Android를 사용 하 여 Google Maps 및 위치를 사용 하는 방법

_이 문서에서는 Xamarin.Android를 사용 하 여 지도 및 위치를 사용 하는 방법을 설명 합니다. 직접 Google 맵 Android API V2를 사용 하 여 기본 제공 지도 응용 프로그램을 활용 하 여 모든 항목을 설명 합니다._

## <a name="maps-overview"></a>맵 개요

매핑 기술은 모바일 장치로 어디에서 나 보완 합니다. 데스크톱 컴퓨터, 랩톱 없는 기본 제공 하는 위치 인식 하는 경향이 있습니다. 반면에 모바일 장치 장치 찾기 변경 되는 위치 정보를 표시 하 고 이러한 응용 프로그램을 사용 합니다. Android 장치에서 사용할 수 있는 위치 하드웨어를 사용 하 여 지도에 위치 데이터를 표시 하는 강력 하 고 기본 제공 기술을 있습니다. 이 문서에서는 포함 하 여 제공 해야 하는 Xamarin.Android에서 맵 응용 프로그램의 스펙트럼을 다룹니다. 

-  기본 제공을 사용 하 여 신속 하 게 매핑 기능을 추가 하려면 응용 프로그램을 매핑합니다.
-  지도의 표시를 제어 하는 지도 API를 사용 합니다.
-  다양 한 기술을 사용 하 여 그래픽 오버레이 추가 합니다.

이 섹션의에서 항목에서는 다양 한 매핑 기능을 설명합니다.
첫째, Android의 기본 제공 지도 응용 프로그램을 활용 하는 방법 및 위치의 파노라마 거리 보기를 표시 하는 방법을 설명 합니다. 그런 다음 이러한 그래픽 오버레이 추가 하는 방법 뿐만 아니라 Maps API를 사용 하 여 컨트롤 위치를 지도 표시를 두는 방법을 보여 주는 응용 프로그램 내에서 직접 매핑 기능을 통합 하는 방법을 설명 합니다.


## <a name="related-links"></a>관련 링크

- [MapsAndLocationDemo_v3 (sample)](https://developer.xamarin.com/samples/monodroid/MapsAndLocationDemo_v3/)
- [작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Google Maps API 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [인 텐트 목록: Android 장치에서 Google 응용 프로그램 호출](https://developer.android.com/guide/appendix/g-app-intents.html)
- [위치 및 지도](https://developer.android.com/guide/topics/location/index.html)
