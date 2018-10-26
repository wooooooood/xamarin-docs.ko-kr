---
title: tvOS 리소스 및 Xamarin에서 데이터 저장소
description: 이 문서에서는 리소스 및 Xamarin에 내장 된 tvOS 앱에서 영구 데이터 저장소를 사용 하는 방법을 설명 합니다. ICloud 데이터 저장소 및 주문형 리소스에 설명 합니다.
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 8f8ecc115738fb97f71b4ea6b2cdcc5c2714372d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108186"
---
# <a name="tvos-resources-and-data-storage-in-xamarin"></a>tvOS 리소스 및 Xamarin에서 데이터 저장소

_이 문서에서는 리소스 및 Xamarin.tvOS 앱에서 영구 데이터 저장소를 사용 하 여 작업을 설명 합니다._

<a name="tvOS-Resource-Limitations" />

## <a name="tvos-resource-limitations"></a>tvOS 리소스 제한

IOS 장치와 달리 새 Apple TV tvOS 앱 이나 데이터에 대 한 매우 제한 된 영구적이 고 로컬 저장소를 제공합니다. 매우 작은 항목 (예: 사용자 기본 설정)에 대 한 tvOS 앱 아직에 대 한 액세스 `NSUserDefaults` 사용 하 여는 [데이터는 500KB 제한인](https://forums.developer.apple.com/message/50696#50696)합니다. 그러나 Xamarin.tvOS 앱을 더 많은 양의 정보를 유지 하는 경우 저장 하 고 해당 데이터를 검색할 [iCloud](#iCloud-Data-Storage)합니다.

또한 tvOS 200MB Apple TV 앱의 크기를 제한합니다. 되도록 해야 앱이이 크기 이상으로 리소스에 필요한 경우 패키지 및 사용 하 여 로드 [주문형 리소스](#On-Demand-Resources) (최대는 추가 2GB). 이러한 한계 지정 되 면 반드시는 올바르게 시간 앱의 사용자에 대 한 최상의 환경을 제공 하는 추가 자산을 다운로드 합니다. 자세한 내용은 Apple의를 참조 하세요 [주문형 리소스 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)합니다.

<a name="Non-Persistent-Downloads" />

## <a name="non-persistent-downloads"></a>비영구적 다운로드

각 tvOS 앱에 추가 리소스 및 자산 다운로드 되는 임시 캐시 디렉터리에 제공 됩니다. 이 디렉터리는 응용 프로그램이 실행 중인으로 유지 됩니다. 그러나 Apple TV를 다른 앱 이나 시스템 사용량에 대 한 공간을 확보 해야 하는 대로이 캐시를 삭제할 수 있습니다.

결과적으로, 앱 시작은 다음에 사용할 수 있는 이전에 다운로드 한 콘텐츠를 사용할 수 없게 합니다. 항상 Xamarin.tvOS 앱 필요한 리소스의 존재 여부를 확인 하 고 필요에 따라 다운로드 해야 합니다.

> [!IMPORTANT]
> 가 다른 자산 및 필요에 따라 리소스를 다운로드할 수 있도록 하는 동안 예기치 않은 결과가 발생할 수 있습니다 Apple 앱의 캐시에 공간을 모두 사용에 대해 경고 합니다.




<a name="Managing-Resources" />

## <a name="managing-resources"></a>리소스 관리

TvOS 앱에 사용할 수 있는 정보의 비 영구적인 저장소를 제한 되어 있으므로 위의 설명 대로 신중 하 게 계획 Xamarin.tvOS 앱에 대 한 훌륭한 사용자 환경을 만들려면 이러한 제한이 필요 합니다.

<a name="iCloud-Data-Storage" />

### <a name="icloud-data-storage"></a>iCloud 데이터 저장소

Apple tv 저장소 제한 되므로 적당 하지 않습니다. 있습니다 매우 제한 된 영구적이 고 로컬 저장소 앱이 이전에 다운로드 한 모든 정보에서 사용할 수 있는 다음에 실행 될 때 보장 되지 않습니다.

결과적으로, Xamarin.tvOS 앱 데이터 저장소를 iCloud에 있는 모든 사용자 데이터를 저장 해야 합니다. Apple는 tvOS 앱에 대 한 두 개의 iCloud 기반 데이터 저장소 옵션을 제공합니다.

- **iCloud 키-값 저장소 (KVS)** -소량의 정보 (1MB 미만)는 앱에 필요할 수 있습니다 (예: 사용자 기본 설정), iCloud KVS 저장소를 사용할 수 있습니다. iCloud KVS 데이터가 클라우드에 모두 동일한 앱을 실행 하는 사용자의 장치를 자동으로 동기화 됩니다. 자세한 내용은 참조 하십시오 합니다 [키-값 저장소](~/ios/data-cloud/introduction-to-icloud.md) 부분 우리의 [iCloud 소개](~/ios/data-cloud/introduction-to-icloud.md) 문서 또는 Apple [iCloud에 있는 키-값 데이터에 대 한 디자인](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7) 설명서입니다.
- **CloudKit** -정보 (1MB 보다 큼), 큰 조각으로 저장소 Apple CloudKit 프레임 워크를 사용 합니다. 와 iCloud KVS 저장소, 달리 CloudKit 데이터 앱 (뿐 아니라 단일 사용자로 개인)의 모든 사용자 간에 공유할 수 있습니다. 자세한 내용은 참조 하세요 우리의 [CloudKit 소개](~/ios/data-cloud/intro-to-cloudkit.md) 설명서 또는 Apple [CloudKit 빠른 시작](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987)합니다.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

<a name="On-Demand-Resources" />

### <a name="on-demand-resources"></a>주문형 리소스

주문형 리소스 앱 콘텐츠 및 앱에서 필요에 따라 다운로드 한 앱 스토어에서 호스트 됩니다 (앱 번들에서 별도) 자산을 제공 합니다. 데이터는 추가 2GB까지 제공할 수 있습니다-주문형 리소스를 사용 하 여. 더 작게 할 초기 앱 다운로드를 통해 (tvOS 앱은 200MB의 최대 제한) 하면서 필요에 따라 다양 한 자산입니다.

TvOS 앱을 주문형으로 리소스를 요청 하면이 콘텐츠 응용 프로그램의 캐시 디렉터리를 저장 하 고 다운로드 시스템은 자동으로 관리 됩니다. 앱이이 콘텐츠를 다운로드 하 고 계속 하려면 사용 가능 대기 해야 합니다.

이러한 리소스 계속 신속한 출시 주기 따라서 앱의 여러 실행 전체에서 Apple TV에 캐시 될 수 있습니다. 그러나 앱 시작은 다음에 사용할 수 있는 이전에 다운로드 한 콘텐츠를 사용할 수 없게 합니다. 참조 된 [Non-persistent 다운로드](#Non-Persistent-Downloads) 대 한 자세한 내용은 위의 섹션입니다.

Xcode를 사용 하 여 주어진 리소스 태그와 연결 된 관련된 콘텐츠 (예: 게임 수준 2에 대 한 모든 자산)의 번들을 만듭니다. 나중에 앱이 리소스 태그를 지정 하 여 주문형 리소스를 요청 합니다. 앱 콘텐츠를 나타내는 사용자에 게 UI를 제공 해야 다운로드 하는 중입니다. 자세한 내용은 Apple의를 참조 하세요 [주문형 리소스 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)합니다.

> [!IMPORTANT]
> 앱은 요청 시에 리소스를 다운로드 하는 횟수와 개별 다운로드 크기 간에 적절 한 균형을 주의 해야 합니다. 새 콘텐츠를 다운로드 하도록 게임 플레이 지속적으로 중단 되거나 한 번에 다운로드할 너무 많은 시간이 걸리는 경우 사용자는 앱으로 생각 될 수 있습니다.




<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.tvOS 앱에서 tvOS 시스템에서 배치 크기, 리소스 및 데이터 저장소 제한 설명 했습니다. 이러한 제한 사항 및 앱에 대 한 훌륭한 사용자 환경을 만들기 위해를 해결 하는 옵션을 제공 했습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
