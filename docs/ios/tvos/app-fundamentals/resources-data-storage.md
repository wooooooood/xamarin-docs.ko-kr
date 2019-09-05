---
title: Xamarin에서 리소스 및 데이터 저장소 tvOS
description: 이 문서에서는 Xamarin으로 빌드된 tvOS 앱에서 리소스 및 영구 데이터 저장소를 사용 하는 방법을 설명 합니다. ICloud 데이터 저장소 및 주문형 리소스에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: e9c27233e0905435e14505195b7b0e4d3283790b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283821"
---
# <a name="tvos-resources-and-data-storage-in-xamarin"></a>Xamarin에서 리소스 및 데이터 저장소 tvOS

_이 문서에서는 tvOS 앱에서 리소스 및 영구 데이터 저장소를 사용 하는 방법을 설명 합니다._

<a name="tvOS-Resource-Limitations" />

## <a name="tvos-resource-limitations"></a>tvOS 리소스 제한

IOS 장치와 달리 새로운 Apple TV는 tvOS apps 또는 데이터에 대해 매우 제한 된 영구적 로컬 저장소를 제공 합니다. 매우 작은 항목 (예: 사용자 기본 설정)의 경우 tvOS 앱은 여전히 500의 `NSUserDefaults` 데이터를 [제한](https://forums.developer.apple.com/message/50696#50696)하 여에 액세스할 수 있습니다. 그러나 tvOS 앱이 더 많은 양의 정보를 유지 해야 하는 경우에는 [iCloud](#iCloud-Data-Storage)에서 해당 데이터를 저장 하 고 검색 해야 합니다.

또한 tvOS는 Apple TV 앱의 크기를 200MB로 제한 합니다. 앱이이 크기를 초과 하는 리소스를 필요로 하는 경우 [요청 시 리소스](#On-Demand-Resources) 를 사용 하 여 패키지 하 고 로드 해야 합니다 (최대 추가 2gb). 이러한 제한 사항이 있을 경우 앱의 사용자에 게 최상의 환경을 제공 하기 위해 추가 자산의 다운로드 시간을 올바르게 지정 하는 것이 중요 합니다. 자세한 내용은 Apple의 [주문형 리소스 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)를 참조 하세요.

<a name="Non-Persistent-Downloads" />

## <a name="non-persistent-downloads"></a>영구적이 지 않은 다운로드

각 tvOS 앱은 추가 리소스 및 자산이 다운로드 되는 임시 캐시 디렉터리를 제공 합니다. 이 디렉터리는 앱이 계속 실행 되는 동안 보존 됩니다. 그러나 Apple TV에서 다른 앱 또는 시스템 사용을 위한 공간을 확보 해야 하므로이 캐시를 삭제할 수 있습니다.

따라서 앱은 다음에 시작 될 때 이전에 다운로드 한 콘텐츠를 사용할 수 없습니다. TvOS 앱은 항상 필요한 리소스가 있는지 확인 하 고 필요에 따라 다운로드 해야 합니다.

> [!IMPORTANT]
> 필요에 따라 다른 자산 및 리소스를 다운로드 하는 기능을 사용 하는 경우 Apple은 예기치 않은 결과를 일으킬 수 있으므로 앱 캐시의 모든 공간을 사용 하지 않도록 경고 합니다.




<a name="Managing-Resources" />

## <a name="managing-resources"></a>리소스 관리

위에서 설명한 것 처럼 tvOS apps에 사용할 수 있는 정보를 영구적으로 저장 하는 것이 제한 되어 있으므로 tvOS 앱에 대 한 뛰어난 사용자 환경을 만들기 위해 이러한 제한 사항을 신중히 계획 해야 합니다.

<a name="iCloud-Data-Storage" />

### <a name="icloud-data-storage"></a>iCloud 데이터 저장소

Apple TV의 저장소는 제한 되어 있기 때문에 영구적이 고 영구적인 로컬 저장소를 사용할 수 있는 것은 아닙니다. 앱은 다음에 실행 될 때 이전에 다운로드 한 모든 정보를 사용할 수 있다는 보장이 없습니다.

결과적으로 tvOS 앱은 모든 사용자 데이터를 iCloud 데이터 저장소에 저장 해야 합니다. Apple은 tvOS apps에 대해 두 개의 iCloud 기반 데이터 저장소 옵션을 제공 합니다.

- **Icloud 키-값 저장소 (KVS)** -앱에 필요할 수 있는 작은 정보 (1mb 미만)의 경우 Icloud KVS Storage를 사용할 수 있습니다. iCloud KVS 데이터는 클라우드 및 동일한 앱을 실행 하는 모든 사용자의 장치에 자동으로 동기화 됩니다. 자세한 내용은 icloud 문서 [소개](~/ios/data-cloud/introduction-to-icloud.md) 의 [키-값 저장소](~/ios/data-cloud/introduction-to-icloud.md) 섹션 또는 [Icloud의 키-값 데이터에 대 한 Apple 디자인](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7) 설명서를 참조 하세요.
- **Cloudkit** -더 많은 정보 (1mb 이상)를 저장 하는 경우 Apple의 Cloudkit 프레임 워크를 사용 합니다. ICloud KVS 저장소와 달리 CloudKit 데이터는 앱의 모든 사용자 간에 공유 될 수 있으며 단일 사용자 전용으로 사용할 수 있습니다. 자세한 내용은 [cloudkit](~/ios/data-cloud/intro-to-cloudkit.md) 설명서 또는 Apple의 [cloudkit 소개 빠른 시작](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987)를 참조 하세요.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

<a name="On-Demand-Resources" />

### <a name="on-demand-resources"></a>주문형 리소스

주문형 리소스는 앱 스토어에서 호스팅되며 앱에서 요구 하는 대로 다운로드 되는 앱 콘텐츠 및 자산 (앱 번들과 분리 됨)을 제공 합니다. 요청 시 리소스를 사용 하 여 최대 추가 2GB의 데이터를 제공할 수 있습니다. 이를 통해 초기 앱 다운로드 크기를 줄일 수 있습니다 (tvOS apps는 최대 200MB로 제한 됨). 하지만 필요에 따라 풍부한 자산을 제공 합니다.

TvOS 앱이 요청 시 리소스를 요청 하면 시스템에서이 콘텐츠를 다운로드 하 여 앱의 캐시 디렉터리에 저장 하는 것을 자동으로 관리 합니다. 앱에서이 콘텐츠를 다운로드 하 여 계속 하기 전에 사용할 수 있게 될 때까지 기다려야 합니다.

이러한 리소스는 앱의 여러 시작에서 Apple TV에 계속 캐시 될 수 있으므로 시작 주기가 빨라집니다. 그러나 다음에 앱을 시작할 때 사용할 수 있는 이전에 다운로드 한 콘텐츠는 사용할 수 없습니다. 자세한 내용은 위의 [비영구 다운로드](#Non-Persistent-Downloads) 섹션을 참조 하세요.

Xcode를 사용 하 여 리소스 태그 부여와 관련 된 관련 콘텐츠 (예: 게임 수준 2의 모든 자산)의 번들을 만듭니다. 나중에 앱은이 리소스 태그를 지정 하 여 요청 시 리소스를 요청 합니다. 앱은 콘텐츠를 다운로드 중임을 나타내는 UI를 사용자에 게 제공 해야 합니다. 자세한 내용은 Apple의 [주문형 리소스 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)를 참조 하세요.

> [!IMPORTANT]
> 앱이 요청 시 리소스를 다운로드 해야 하는 횟수와 개별 다운로드의 크기 간에 적절 한 균형을 맞추도록 주의 해야 합니다. 게임 플레이를 지속적으로 중단 하 여 새 콘텐츠를 다운로드 하거나 단일 다운로드에 너무 많은 시간이 소요 되는 경우 사용자가 앱을 사용 하지 못할 수 있습니다.




<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 tvOS 시스템에 의해 tvOS 앱에 배치 된 크기, 리소스 및 데이터 저장소 제한 사항에 대해 설명 했습니다. 이러한 제한 사항 및 제안 사항을 해결 하는 옵션을 제공 하 여 앱에 대 한 뛰어난 사용자 환경을 만들 수 있습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
