---
title: 추가 watchOS 3 프레임 워크 변경 내용
description: 이 문서에서는 watchOS 3에 도입 된 다양 한 프레임 워크 변경 내용 및 Xamarin에서 작업 하는 방법을 설명 합니다. 핵심 데이터, 핵심 동작, 파운데이션, HealthKit, HomeKit, PassKit 및 UIKit에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 34f192938ac583e39232312377142015aa6d3811
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287560"
---
# <a name="additional-watchos-3-frameworks-changes"></a>추가 watchOS 3 프레임 워크 변경 내용

_이 문서에서는 watchOS 3의 기존 프레임 워크에 대 한 추가, 사소한 변경 또는 향상 된 기능을 설명 합니다._

Apple에서는 iOS의 주요 변경 사항 외에도 watchOS 3의 여러 기존 프레임 워크를 수정 하 고 향상 시켰습니다.


## <a name="core-data"></a>핵심 데이터

다음은 watch OS 3 용 핵심 데이터 프레임 워크에 대 한 향상 된 기능입니다.

- Root [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) 개체는 serialization 없이 동시에 오류 및 가져오기를 지원 합니다.
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) 클래스는 SQLite 데이터 저장소의 풀을 유지 관리 합니다.
- 모드 (관리 개체 컨텍스트)를 사용 하는 새 쿼리 생성 기능을 사용 하 여 [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) 개체를 WAL 저널 모드에 저장 하면 나중에 인출 하 고 오류를 발생 시킬 수 있습니다.
- 상위 수준 `NSPersistenceContainer` 을 사용 하 여 `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) 및 기타 핵심 데이터 구성 리소스를 참조 합니다.
- 더 쉽게 페치를 수행 하 고 하위 `NSManagedObject` 클래스를 만들 수 있도록 몇 가지 새로운 편의 방법이 추가 되었습니다.

자세한 내용은 Apple의 [핵심 데이터 프레임 워크 참조](https://developer.apple.com/reference/coredata)를 참조 하세요.


## <a name="core-motion"></a>핵심 동작

다음은 조사식 OS 3의 핵심 동작 프레임 워크에 대 한 향상 된 기능입니다.

- 새 장치 동작 이벤트는가 속도계 및 자이로스코프가를 사용 하 여 동작 및 방향 업데이트를 제공 합니다. 앱은이 업데이트를 등록할 수 있습니다 (최대 100Hz).
- 새 Pedometer 이벤트를 사용 하면 사용자가 일시 중지 하 고 실행을 다시 시작할 때 실시간으로 신속 하 게 알림을 받을 수 있습니다. [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer) 를 사용 하 여 포그라운드 또는 background pedometer 이벤트에 등록 합니다.


## <a name="foundation"></a>Mfc

다음은 watch OS 3 용 Foundation framework에 대 한 향상 된 기능입니다.

- 새 [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) 클래스를 사용 하 여 간격을 비교 하 고 간격 교차로 테스트 하는 기간 등의 날짜 및 시간 간격 계산을 수행할 수 있습니다.
- 로컬 정보 및 사용 가능한 표시 형식을 얻기 위해 [Nslocal](https://developer.apple.com/reference/foundation/nslocale) 클래스에 몇 가지 새 속성이 추가 되었습니다.
- 새 [Nsmeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) 클래스를 사용하여 다른 Uom (측정 단위) 간에 변환하거나 다른 uom의 값에 대한 계산을 수행합니다.
- 새 [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) 클래스를 사용 하 여 최종 사용자에 게 표시 하기 위해 지역화 된 측정값의 서식을 지정 합니다.
- 새 [Nsunit](https://developer.apple.com/reference/foundation/nsunit) 및 [nsunit](https://developer.apple.com/reference/foundation/nsdimension) 클래스를 사용 하 여 특정 uoms를 나타냅니다.


## <a name="healthkit"></a>HealthKit

Watch OS 3 용 HealthKit 프레임 워크에 대 한 다음과 같은 향상 된 기능이 향상 되었습니다.

- 새 [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) 클래스를 사용 하 여 체력 `ActivityType` 의 `LocationType` 및를 지정 합니다.
- 휠체어 관련 상태 데이터를 `WheelchairUse` 사용 하기 위해 새 [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) 및 [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) 클래스의 메서드가 추가 되었습니다.
- 날씨 유형 (예 `HKWeatherConditionClear` : 및 `HKWeatherConditionCloudy`)에 대 한 새 메타 데이터 키가 추가 되 고 `HKWorkoutActivityTypeFlexibility` , 및 등 `HKWorkoutActivityTypeWheelchairRunPace`의 체력 유형 (예: 및)이 추가 되었습니다.


## <a name="homekit"></a>HomeKit

Watch OS 3 용 HomeKit 프레임 워크에 대 한 다음과 같은 향상 된 기능이 향상 되었습니다.

- HomeKit 연결 된 IP 카메라를 확인 하 고 상호 작용 하는 기능이 추가 되었습니다.
- 몇 가지 새로운 서비스와 특성이 추가 되었습니다.
- 기본 서비스 및 링크 서비스의 보조 프로그램에 대 한 추가 컨텍스트와 구성이 추가 되었습니다.


## <a name="passkit"></a>PassKit

Watch OS 3 용 PassKit 프레임 워크에 대 한 다음과 같은 향상 된 기능이 향상 되었습니다.

- 는 물리적 상품 및 서비스의 Apple Watch에 대 한 안전한 앱 내 지불을 지원 하도록 프레임 워크를 확장 합니다.
- 이제 다음 클래스를 사용할 수 있습니다. [Pkpayment](https://developer.apple.com/reference/passkit/pkpayment), [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod), [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest) 및 [PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>UIKit

다음은 watch OS 3 용 UIKit 프레임 워크에 대 한 향상 된 기능입니다.

- 레이블에서 동적 형식을 지원 하기 위해 텍스트 필드와 텍스트 상자는 `PreferredFontForTextStyle` `UIFont` 클래스의 새 메서드를 사용 합니다.
- 와이드 색을 지원 하기 위해 메서드가추가되었습니다.`ColorWithDisplayP3`


## <a name="related-links"></a>관련 링크

- [watchOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS%20watchos)
- [WatchOS 3의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
