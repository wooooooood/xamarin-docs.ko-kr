---
title: 추가 watchOS 3 프레임 워크 변경 내용
description: 이 문서에서는 추가, 부 버전 변경 또는 watchOS 3에 대 한 기존 프레임 워크의 향상 된 기능에 설명 합니다.
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 572aaff9d010ec1ec1f48db2878859e445e2fb20
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="additional-watchos-3-frameworks-changes"></a>추가 watchOS 3 프레임 워크 변경 내용

_이 문서에서는 추가, 부 버전 변경 또는 watchOS 3에 대 한 기존 프레임 워크의 향상 된 기능에 설명 합니다._

IOS에 주요 변경 내용 외에도 Apple가 한 수정 및 여러 기존 프레임 워크의 향상 된 기능 watchOS 3입니다.


## <a name="core-data"></a>핵심 데이터

조사식 3 OS에 대 한 핵심 데이터 프레임 워크 같이 향상 됩니다.:

- 루트 [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) 동시 오류가 있는 및 serialization 없이 인출 개체 지원 합니다.
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) 클래스 SQLite 데이터 저장소의 풀을 관리 합니다.
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) 이후를 가져오기 위해 특정 데이터베이스 버전을 관리 하는 개체 컨텍스트 (모드)에 고정 수 새 쿼리 생성 WAL 저널 모드 지원에서 SQLite 데이터 저장소를 사용 하 여 개체 기능 및 오류가 있는 트랜잭션.
- 상위 수준를 사용 하 여 `NSPersistenceContainer` 참조에는 `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) 및 기타 핵심 데이터 구성 리소스입니다.
- 에 추가 된 몇 가지 새로운 편의 메서드 `NSManagedObject` 쉽게 인출을 수행 하 고 하위 클래스를 만들 수 있습니다.

자세한 내용은 Apple의를 참조 하십시오 [핵심 데이터 프레임 워크 참조](https://developer.apple.com/reference/coredata)합니다.


## <a name="core-motion"></a>코어 동작

조사식 3 운영 체제의 핵심 동작 프레임 워크 같이 향상 됩니다.:

- 새 장치 이동 이벤트를 사용 하 여가 속도계 및 자이로스코프가 동작 및 방향 업데이트를 제공 합니다. 응용 프로그램 응용 프로그램 업데이트 (최대 100 Hz의 속도로)이에 대 한 등록할 수 있습니다.
- 새 Pedometer 이벤트를 사용 하면 신속 하 고 올려 놓을 때 실시간으로 알림 및 실행이 다시 시작 합니다. 사용 하 여는 [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer) 포그라운드 또는 백그라운드 pedometer 이벤트에 대 한 등록 합니다.


## <a name="foundation"></a>Foundation

조사식 OS 3의 Foundation 프레임 워크 같이 향상 됩니다.:

- 새로운 [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) 을 간격을 비교 하 고 간격 교차점에 대 한 테스트 기간, 같은 날짜 및 시간 간격 계산을 수행 하는 클래스입니다.
- 몇 가지 새 속성에 추가 된는 [NSLocal](https://developer.apple.com/reference/foundation/nslocale) 로컬 정보 및 사용 가능한 표시 형식을 얻으려고 하는 클래스입니다.
- 새로운 [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) 간에 서로 다른 단위의 측정값 (UOM)으로 변환 하거나 다른 UOMs의 값에 대해 계산을 수행 하는 클래스입니다.
- 새로운 [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) 최종 사용자에 게 표시 하는 것에 대 한 지역화 된 측정값의 서식을 지정 하려면 클래스입니다.
- 새로운 [NSUnit](https://developer.apple.com/reference/foundation/nsunit) 및 [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) 특정 UOMs 나타내기 위한 클래스입니다.


## <a name="healthkit"></a>HealthKit

조사식 OS 3의 HealthKit 프레임 워크 같이 향상 됩니다.:

- 새로운 [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) 지정 하는 클래스는 `ActivityType` 및 `LocationType` 는 워크 아웃의 합니다.
- 새 [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) 및 `WheelchairUse` 의 메서드는 [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) 클래스 장애 자용 사용 하기 위한 추가 된 관련 상태 데이터입니다.
- 날씨 형식에 대 한 새 메타 데이터 키가 추가 되었기 (같은 `HKWeatherConditionClear` 및 `HKWeatherConditionCloudy`) 및 워크 아웃 형식 (같은 `HKWorkoutActivityTypeFlexibility` 및 `HKWorkoutActivityTypeWheelchairRunPace`) 추가 되었습니다.


## <a name="homekit"></a>HomeKit

조사식 OS 3의 HomeKit 프레임 워크 같이 향상 됩니다.:

- 연결 된 IP 보고 HomeKit와 상호 작용 하는 기능이 추가 카메라 합니다.
- 몇 가지 새로운 서비스 및 특성을 추가 합니다.
- 더 많은 컨텍스트 및 기본 서비스 및 서비스 연결의 보조 프로그램의 구성을 추가 합니다.


## <a name="passkit"></a>PassKit

조사식 OS 3의 PassKit 프레임 워크 같이 향상 됩니다.:

- Apple Watch 물리적 상품 및 서비스 모두에 안전 하 고, 앱에서 지불을 지원 하기 위해 프레임 워크를 확장 합니다.
- 다음 클래스는 이제 사용할 수 있는: [PKPayment](https://developer.apple.com/reference/passkit/pkpayment), [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod), [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest) 및 [PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>UIKit

조사식 OS 3의 UIKit 프레임 워크 같이 향상 됩니다.:

- 를 지원 하려면 동적 형식 레이블에 텍스트 필드 및 입력란 사용 새 `PreferredFontForTextStyle` 의 메서드는 `UIFont` 클래스입니다.
- `ColorWithDisplayP3` 메서드는 광범위 한 색을 지원 하기 위해 추가 되었습니다.


## <a name="related-links"></a>관련 링크

- [시작 하기 (샘플)](https://developer.xamarin.com/samples/monotouch/WatchKit/)
- [WatchOS 3의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
