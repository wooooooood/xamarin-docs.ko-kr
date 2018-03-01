---
title: "리소스 및 데이터 저장소"
description: "이 문서에서는 리소스와 Xamarin.tvOS 응용 프로그램에서 영구 데이터 저장소 작업을 수행 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 768ee4a2f33475b5327bf9c0fd006f1da580d836
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="resources-and-data-storage"></a>리소스 및 데이터 저장소

_이 문서에서는 리소스와 Xamarin.tvOS 응용 프로그램에서 영구 데이터 저장소 작업을 수행 합니다._

<a name="tvOS-Resource-Limitations" />

## <a name="tvos-resource-limitations"></a>tvOS 리소스 제한

IOS 장치와 달리 새 Apple TV tvOS 응용 프로그램 또는 데이터에 대 한 매우 제한 영구, 로컬 저장소를 제공합니다. 매우 작은 항목 (예: 사용자 기본 설정)에 대 한 tvOS 앱에 아직에 대 한 액세스 `NSUserDefaults` 와 [제한을 500KB 데이터](https://forums.developer.apple.com/message/50696#50696)합니다. 그러나 Xamarin.tvOS 앱을 더 많은 양의 정보를 유지 하는 경우이 저장 하 고 해야 해당 데이터를 검색할 [iCloud](#iCloud-Data-Storage)합니다.

또한 tvOS 200MB Apple TV 앱의 크기를 제한합니다. 되어야 할 앱이이 크기를 초과 하는 리소스를 필요한 경우 패키지 하 고 사용 하 여 로드 [주문형 리소스](#On-Demand-Resources) (최대 2GB 추가로). 이러한 제한을 제공 하는 하면 올바르게 때 해당 응용 프로그램의 사용자에 대 한 최상의 경험을 제공 하는 추가 자산을 다운로드 합니다. 자세한 내용은 Apple의를 참조 하십시오 [주문형 리소스 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)합니다.

<a name="Non-Persistent-Downloads" />

## <a name="non-persistent-downloads"></a>비 영구적인 다운로드

각 tvOS 앱 정의와 추가 리소스 및 자산에 다운로드 되는 임시 캐시 디렉터리에 제공 됩니다. 이 디렉터리는 응용 프로그램이 계속 실행 상태로 유지 됩니다. 그러나 Apple TV에 다른 응용 프로그램 또는 시스템 사용을 위한 공간을 확보 해야 하는 대로이 캐시를 삭제할 수 있습니다.

결과적으로, 응용 프로그램 시작 다음에 사용할 수 없게 하는 이전에 다운로드 한 콘텐츠 사용할 수 없습니다. 항상 Xamarin.tvOS 앱 필요한 리소스의 존재 여부를 확인 하 고 필요에 따라이 다운로드 해야 합니다.

> [!IMPORTANT]
> **참고:** 다른 자산 및 필요에 따라 리소스를 다운로드 하는 기능을 사용 하는 동안 Apple 경고를 표시 모든 응용 프로그램의 캐시에 대 한 공간 사용에 대 한 예기치 않은 결과가 발생할 수 있습니다.




<a name="Managing-Resources" />

## <a name="managing-resources"></a>리소스 관리

위에서 설명한 것 처럼, 제한, 되어 tvOS 앱에서는 사용할 수 있는 정보의 비영구 저장소 이러한 제한 사항은 신중 하 게 계획 Xamarin.tvOS 앱에 대 한 뛰어난 사용자 환경을 만드는 데 필요 합니다.

<a name="iCloud-Data-Storage" />

### <a name="icloud-data-storage"></a>icloud와 데이터 저장소

Apple TV에 저장소 제한 되므로 뿐만 아니라는 있는 매우 제한 된 영구, 로컬 저장소 응용 프로그램에 이전에 다운로드 한 모든 정보에서 사용할 수 있는 다음에 실행 하지 않을 수도 있습니다.

결과적으로, Xamarin.tvOS 응용 프로그램 데이터 저장소는 iCloud에 모든 사용자 데이터를 저장 해야 합니다. Apple 앱 tvOS에 대 한 두 개의 iCloud 기반 데이터 저장소 옵션을 제공합니다.

- **키-값 저장소 (은 KVS) iCloud** -소량의 정보 (1MB 미만)는 응용 프로그램 필요할 수 있습니다 (예: 사용자 기본 설정), iCloud은 KVS 저장소를 사용할 수 있습니다. iCloud은 KVS 데이터 클라우드와 동일한 응용 프로그램을 실행 하는 사용자의 장치를 모두 자동으로 동기화 됩니다. 자세한 내용은 참조 하십시오는 [키-값 저장소](~/ios/data-cloud/introduction-to-icloud.md) 의 섹션 우리의 [iCloud 소개](~/ios/data-cloud/introduction-to-icloud.md) 문서 또는 Apple의 [iCloud에 키 / 값 데이터에 대 한 디자인](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7) 설명서입니다.
- **CloudKit** -Apple의 CloudKit 프레임 워크를 사용 하는 정보 (1MB 보다 큼), 큰 조각으로 저장 합니다. KVS 저장소 iCloud와 달리 CloudKit 데이터 응용 프로그램 (으로 단일 사용자에 게 개인 되 고)의 모든 사용자 간에 공유할 수 있습니다. 자세한 내용은 양식에서 참조 하세요 우리의 [CloudKit 소개](~/ios/data-cloud/intro-to-cloudkit.md) 설명서 또는 Apple의 [CloudKit 빠른 시작](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987)합니다.

<a name="On-Demand-Resources" />

### <a name="on-demand-resources"></a>요청 시 리소스

요청 시 리소스 응용 프로그램 콘텐츠 및 앱 스토어에서 호스팅되는 응용 프로그램 요구에 따라 다운로드 한 자산 (별도의 앱 번들)을 제공 합니다. 최대 2GB의 데이터 추가 제공 될 수 요청 시 리소스를 사용 합니다. 더 작은 초기 응용 프로그램 다운로드를 활성화 (tvOS 앱은 200MB의 최대 제한)을 필요에 따라 풍부한 자산을 제공 하면서 합니다.

TvOS 응용 프로그램에서 요청 시 리소스를 요청 하면이 콘텐츠 응용 프로그램의 캐시 디렉터리를 저장 하 고 다운로드 시스템이 자동으로 관리 됩니다. 앱이이 콘텐츠를 다운로드 하 고 계속할 수 있습니다 사용할 수 있는 대해 기다려야 합니다.

이러한 리소스 계속 여러 실행 하는 경우 응용 프로그램의 따라서 신속한 실행 주기 전체에서 Apple TV에 캐시 될 수 있습니다. 그러나 응용 프로그램 시작 다음에 사용할 수 없게 하는 모든 이전에 다운로드 한 콘텐츠 사용할 수 없습니다. 참조는 [Non-persistent 다운로드](#Non-Persistent-Downloads) 자세한 내용은 섹션.

관련된 내용 (예: 경기 수준 2에 대 한 모든 자산)의 번들으로 제공 리소스 태그와 연결 된를 만들려면 Xcode를 사용 합니다. 나중에 앱이 리소스 태그를 지정 하 여 요청 시 리소스를 요청 합니다. 해당 콘텐츠를 나타내는 사용자에 게 UI를 제공 해야 앱 다운로드 하는 중입니다. 자세한 내용은 Apple의를 참조 하십시오 [주문형 리소스 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)합니다.

> [!IMPORTANT]
> **참고:** 응용 프로그램에 요청 시 리소스를 다운로드 하는 횟수와 개별 다운로드 크기 간의 적절 한 균형을 주의 해야 합니다. 새 콘텐츠를 다운로드할 게임 플레이 지속적으로 중단 되거나 한 번 다운로드 너무 많은 시간이 걸리는 경우에 사용자 응용 프로그램으로 생각 될 수 있습니다.




<a name="Summary" />

## <a name="summary"></a>요약

이 문서 tvOS 체제별 Xamarin.tvOS 응용 프로그램에 배치 크기, 리소스 및 데이터 저장소 제한 검사가 수행 됩니다. 이러한 제한 사항 및 기능을 앱에 대 한 뛰어난 사용자 환경을 제안 해결 하는 옵션을 제시한 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
