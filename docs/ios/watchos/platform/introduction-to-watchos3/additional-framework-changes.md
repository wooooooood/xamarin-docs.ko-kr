---
title: 추가 watchOS 3 프레임 워크 변경
description: 이 문서에서는 watchOS 3 및 Xamarin에서 작업 하는 방법을 도입 하는 다양 한 프레임 워크 변경 내용을 설명 합니다. 핵심 데이터, Core 동작, Foundation, HealthKit, HomeKit, PassKit 및 UIKit 설명 되어 있습니다.
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 745c39dab1f73870ce036791434ed9a0b05d681b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122623"
---
# <a name="additional-watchos-3-frameworks-changes"></a>추가 watchOS 3 프레임 워크 변경

_이 문서에서는 추가, 부 버전 변경 또는 watchOS 3에 대 한 기존 프레임 워크의 향상 된 기능을 설명 합니다._

IOS에 주요 변경 내용 외에도 Apple가 수정 및 여러 기존 프레임 워크의 향상 된 기능 watchOS 3에에서 있게 되었습니다.


## <a name="core-data"></a>핵심 데이터

조사식 3 OS에 대 한 핵심 데이터 프레임 워크 같이 향상 됩니다.:

- 루트 [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) 동시 오류 및 직렬화 없이 가져오는 개체를 지원 합니다.
- 합니다 [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) 클래스 SQLite 데이터 저장소의 풀을 관리 합니다.
- 합니다 [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) WAL 저널 모드 지원 새 쿼리 생성에에서 SQLite 데이터 저장소를 사용 하 여 개체 관리 되는 개체 컨텍스트 (MOC) 이후 페치 하기 위해 특정 데이터베이스 버전에 고정할 수 있습니다 위치 기능 및 오류가 있는 트랜잭션.
- 사용 하 여 대략적 `NSPersistenceContainer` 참조를 `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) 및 다른 핵심 데이터 구성 리소스.
- 에 추가 된 편리한 몇 가지 새로운 메서드 `NSManagedObject` 쉽게 페치를 수행 하 고 서브 클래스를 만들 수 있습니다.

자세한 내용은 Apple의를 참조 하세요 [핵심 데이터 프레임 워크 참조](https://developer.apple.com/reference/coredata)합니다.


## <a name="core-motion"></a>핵심 동작

조사식 3 OS의 핵심 동작 프레임 워크 같이 향상 됩니다.:

- 새 장치 동작 이벤트가 속도계 및 자이로스코프가 사용 하 여 동작 및 방향 업데이트를 제공 합니다. 업데이트 (최대 100 Hz의 요금)이 앱을 등록할 수 있습니다.
- 사용자 일시 중지 하는 경우 실시간 알림 및 실행을 재개할 새 Pedometer 이벤트를 신속 하 고 있습니다. 사용 된 [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer) 포그라운드 또는 백그라운드 pedometer 이벤트에 대 한 등록 합니다.


## <a name="foundation"></a>Foundation

Watch OS 3 Foundation 프레임 워크 같이 향상 됩니다.:

- 새 [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) 기간, 간격을 비교 하 고 간격 교차점에 대 한 테스트와 같은 날짜 및 시간 간격 계산을 수행 하는 클래스입니다.
- 에 추가 된 몇 가지 새 속성을 [NSLocal](https://developer.apple.com/reference/foundation/nslocale) 로컬 정보 및 사용 가능한 표시 형식을 가져오려고 클래스.
- 새 [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) 클래스 간에 서로 다른 단위의 측정값 (UOM)을 변환 하거나 다른 UOMs의 값에 대해 계산을 수행 합니다.
- 새 [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) 최종 사용자에 게 표시 하는 것에 대 한 지역화 된 측정값의 서식을 지정 하는 클래스입니다.
- 새 [NSUnit](https://developer.apple.com/reference/foundation/nsunit) 하 고 [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) 특정 UOMs 나타내기 위한 클래스입니다.


## <a name="healthkit"></a>HealthKit

Watch OS 3 HealthKit 프레임 워크 같이 향상 됩니다.:

- 새 [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) 클래스를 지정 합니다 `ActivityType` 및 `LocationType` 만납니다의 합니다.
- 새 [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) 및 `WheelchairUse` 메서드를 [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) 휠체어 사용 하기 위한 클래스를 추가한 관련 상태 데이터입니다.
- 날씨 형식에 대 한 새 메타 데이터 키를 추가한 (같은 `HKWeatherConditionClear` 하 고 `HKWeatherConditionCloudy`) 및 운동 형식 (같은 `HKWorkoutActivityTypeFlexibility` 및 `HKWorkoutActivityTypeWheelchairRunPace`) 추가 되었습니다.


## <a name="homekit"></a>HomeKit

Watch OS 3 HomeKit 프레임 워크 같이 향상 됩니다.:

- 추가 보기 및 HomeKit 상호 작용 하는 기능 연결 IP 카메라입니다.
- 몇 가지 새로운 서비스 및 특성을 추가 합니다.
- 자세한 컨텍스트 및 기본 서비스 및 링크 서비스 accessories의 구성을 추가 합니다.


## <a name="passkit"></a>PassKit

Watch OS 3 PassKit framework 같이 향상 됩니다.:

- Apple Watch 실제 상품 및 서비스 모두에서 지불을 안전 하 고 앱을 지원 하기 위해 프레임 워크를 확장 합니다.
- 다음 클래스는 이제 사용할 수 있습니다: [PKPayment](https://developer.apple.com/reference/passkit/pkpayment)를 [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod)를 [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest) 및 [PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>UIKit

Watch OS 3의 UIKit 프레임 워크 같이 향상 됩니다.:

- 를 지원 하기 위해 동적 형식 레이블에 텍스트 필드 및 입력란 사용 하 여 새 `PreferredFontForTextStyle` 메서드는 `UIFont` 클래스입니다.
- `ColorWithDisplayP3` 와이드 컬러를 지원 하기 위해 메서드가 추가 되었습니다.


## <a name="related-links"></a>관련 링크

- [Getting Started (샘플)](https://developer.xamarin.com/samples/monotouch/WatchKit/)
- [WatchOS 3의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
