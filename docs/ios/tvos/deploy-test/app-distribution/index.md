---
title: tvOS 앱 배포 개요
description: 이 문서는 Xamarin.tvOS 앱에 사용할 수 있는 배포 기술에 대 한 개요를 하 고 항목에 있는 자세한 문서에 대 한 포인터를 제공 합니다.
ms.prod: xamarin
ms.assetid: D5E0F446-C083-4E21-9788-FC84D32D00C4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: d2f4031eeddbaa206f38b7b1c2bb49d21482c175
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864941"
---
# <a name="tvos-app-distribution-overview"></a>tvOS 앱 배포 개요

_이 문서는 Xamarin.tvOS 앱에 사용할 수 있는 배포 기술에 대 한 개요를 하 고 항목에 있는 자세한 문서에 대 한 포인터를 제공 합니다._


Xamarin.tvOS 앱이 개발 되 면 아래 다이어그램에 강조 표시 된 섹션에 표시 된 대로 소프트웨어 개발 수명 주기의 다음 단계는 사용자에 게 앱을 배포 하입니다.


[![소프트웨어 개발 수명 주기 개요](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)


Apple는 Xamarin.tvOS 지 원하는 tvOS 앱을 배포 하는 다음 방법을 제공 합니다.

1. [**앱 스토어**](#Apple-TV-App-Store-Distribution)
2. [**사내(엔터프라이즈)** ](#In-House-Distribution) 
3. [**임시**](#Ad_Hoc_Distribution) 

이러한 모든 시나리오에서는 적절한 *프로비전 프로필*을 사용하여 애플리케이션을 프로비전해야 합니다. 프로비전 프로필은 애플리케이션 ID 및 의도된 배포 메커니즘뿐만 아니라 코드 서명 정보도 포함된 파일입니다. 앱 스토어 배포가 아닌 경우 앱을 배포할 수 있는 디바이스에 대한 정보도 포함되어 있습니다.

<a name="Apple-TV-App-Store-Distribution" />

## <a name="apple-tv-app-store-distribution"></a>Apple TV App Store 배포

TvOS 앱 Apple TV 장치에서 소비자에 게 배포 되는 기본 방법입니다. Apple TV App Store에 제출 된 모든 앱에는 Apple의 승인이 필요 합니다.

앱은 *iTunes Connect*라는 포털을 통해 앱 스토어에 제출됩니다. 하십시오 우리의 [iTunes Connect에서에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) 다음 항목에 대 한 정보에 대 한 가이드:

- 관리 계약, 세금 및 뱅킹 합니다.
- ITunes Connect 레코드 만들기.
- 앱 비디오 및 스크린샷 관리 합니다.
- 앱 이름, 설명, 새로운 관리 키워드 및 Url입니다.
- 일반 앱 정보를 유지 관리합니다.
- Game Center 정보 유지 관리합니다.
- 앱 검토 정보 유지 관리합니다.
- 가격 정보 유지 관리 합니다.
- 인 앱 구매 정보 유지 관리 합니다.
- 응용 프로그램 검토 보기.

TvOS 용으로 특별히 작성 되지 않으므로 하는 동안이 문서에서는 설정 하 고이 포털을 사용 하 여 Apple TV App Store에 게시에 대 한 앱을 준비 하는 방법에 설명 합니다. 많은 동일한 단계는 iOS 또는 tvOS 앱을 설정 하는 여부 필요 합니다.

위에 나열 된 단계를 모두 완료 되 면 당사의 [iTunes Connect에서에서 tvOS 앱 구성](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) 필요한 tvOS 앱 특정 정보를 추가 합니다.

**Apple Developer Program**에 속한 개발자만 iTunes Connect에 액세스할 수 있습니다. **Apple Developer Enterprise Program**의 구성원은 액세스할 수 없습니다.

Xamarin.tvOS 앱을 Apple TV App Store에 제출 하는 데 문제가 있는 경우를 참조 하십시오 우리의 [문제 해결](~/ios/tvos/troubleshooting.md) 가이드입니다. Xamarin.tvOS 해결 방법과 발생할 수 있는 몇 가지 알려진된 문제가 포함 됩니다.

자세한 내용은 참조 하십시오 합니다 [Apple TV App Store에 게시](~/ios/tvos/deploy-test/app-distribution/app-store-publishing.md) 가이드입니다.

<a name="In-House-Distribution" />

## <a name="in-house-distribution"></a>사내 배포

*엔터프라이즈 배포*라고도 하는 사내 배포를 사용하면 **Apple Developer Enterprise Program**의 구성원이 내부적으로 동일한 조직의 다른 구성원에게 앱을 배포할 수 있습니다. 사내 배포에는 앱 스토어 검토가 필요하지 않으며, 응용 프로그램을 설치할 수 있는 디바이스의 수가 제한되지 않는다는 이점이 있습니다. 그러나 **Apple Developer Enterprise Program** 구성원에게는 iTunes Connect에 대한 액세스 권한이 **없으므로** 정식 사용자가 앱을 배포해야 합니다.

설정 하 고 내부 응용 프로그램을 배포 하는 방법에 대 한 자세한 내용은 참조 하십시오 합니다 [사내 배포 가이드](~/ios/deploy-test/app-distribution/in-house-distribution.md)합니다. 이 문서는 특정 iOS에 있지만 동일한 기술을 tvOS 앱에 사용 됩니다.

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>임시 배포

Xamarin.tvOS 앱 모두에서 사용할 수 있는 임시 배포를 통해 사용자 테스트 수를 **Apple Developer Program**, 및 **Apple Developer Enterprise Program**, 최대 100 개의 Apple TV 장치 허용 및 테스트 합니다. ITunes Connect는 옵션이 아닌 경우 임시 배포에 대 한 모범 사례는 회사 내는 배포입니다.

설정 하 고 내부 앱을 배포 하는 방법에 대 한 자세한 내용은 참조 하십시오 합니다 [임시 배포 가이드](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)합니다. 다시이 문서는 iOS 관련 하지만 tvOS 앱과 동일한 기술을 사용 됩니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.tvOS 앱에 사용할 수 있는 배포 메커니즘에 간략하게 설명을 했습니다. 도입 된 Apple TV App Store, 임시 및 사내 배포 하 고 자세한 정보 링크를 제공 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
