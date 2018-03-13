---
title: "앱 배포 개요"
description: "이 문서는 Xamarin.tvOS 응용 프로그램에 사용할 수 있는 배포 기술에 대 한 개요를 제공 하 고 역할을 항목에 있는 더 자세한 문서에 대 한 포인터."
ms.topic: article
ms.prod: xamarin
ms.assetid: D5E0F446-C083-4E21-9788-FC84D32D00C4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 3e96e98f90c7f4c849a9f679b2de819ccaabfec0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="app-distribution-overview"></a>앱 배포 개요

_이 문서는 Xamarin.tvOS 응용 프로그램에 사용할 수 있는 배포 기술에 대 한 개요를 제공 하 고 역할을 항목에 있는 더 자세한 문서에 대 한 포인터._


Xamarin.tvOS 응용 프로그램을 개발한 후 아래 다이어그램의 강조 표시 된 섹션에 나와 있는 것 처럼 소프트웨어 개발 수명 주기에서 다음 단계는 사용자에 게 앱을 배포할 수입니다.


[![소프트웨어 개발 수명 주기 개요](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)


Apple에는 Xamarin.tvOS에서 지원 되는 tvOS 앱을 배포 하려면 다음 방법을 제공 합니다.

1. [**앱 스토어**](#Apple-TV-App-Store-Distribution)
2. [**사내(엔터프라이즈)**](#In-House-Distribution) 
2. [**임시**](#Ad_Hoc_Distribution) 

이러한 모든 시나리오에서는 적절한 *프로비전 프로필*을 사용하여 응용 프로그램을 프로비전해야 합니다. 프로비전 프로필은 응용 프로그램 ID 및 의도된 배포 메커니즘뿐만 아니라 코드 서명 정보도 포함된 파일입니다. 앱 스토어 배포가 아닌 경우 앱을 배포할 수 있는 장치에 대한 정보도 포함되어 있습니다.

<a name="Apple-TV-App-Store-Distribution" />

## <a name="apple-tv-app-store-distribution"></a>Apple TV App Store 배포

Apple TV 장치에는 소비자에 게 tvOS 앱을 배포 하는 기본적인 방법은 이것이입니다. Apple TV 앱 스토어에 제출 하는 모든 앱에는 Apple에는 승인이 필요 합니다.

앱은 *iTunes Connect*라는 포털을 통해 앱 스토어에 제출됩니다. 참조 하십시오 우리의 [iTunes Connect에서에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) 다음 항목에 대 한 내용은 가이드:

- 관리 계약, 세금 및 은행입니다.
- iTunes Connect 레코드를 만듭니다.
- 응용 프로그램 비디오 및 스크린샷을 보려면 관리입니다.
- 관리 응용 프로그램 이름, 설명, 새로운 기능, 키워드 및 Url입니다.
- 일반 응용 프로그램 정보를 유지 관리합니다.
- 게임 센터 정보를 유지 관리합니다.
- 응용 프로그램 검토 정보를 유지 관리합니다.
- 가격 정보를 유지 관리 합니다.
- 앱에서 바로 구매 정보를 유지 관리합니다.
- 응용 프로그램 검토 볼 수 있습니다.

TvOS에 대 한 구체적으로 기록 되지 하는 동안에이 문서에서는 설정 하 고이 포털을 사용 하 여 Apple TV 앱 스토어에 게시에 대 한 앱을 준비 하는 방법에 설명 합니다. 동일한 단계는 많이 필요한 tvOS 또는 iOS 앱을 설정 하는 여부입니다.

위에 나열 된 단계를 모두 완료 되 면 참조 우리의 [프로그램 tvOS 앱 iTunes Connect에서에서 구성](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) 필요한 tvOS 응용 프로그램 특정 정보를 추가 합니다.

**Apple Developer Program**에 속한 개발자만 iTunes Connect에 액세스할 수 있습니다. **Apple Developer Enterprise Program**의 구성원은 액세스할 수 없습니다.

Apple TV 앱 스토어에 Xamarin.tvOS 앱 제출 하는 데 문제가 있는 경우를 참조 하십시오 우리의 [문제 해결](~/ios/tvos/troubleshooting.md) 가이드입니다. Xamarin.tvOS에이 해결 하는 방법 및 발생할 수 있는 몇 가지 알려진된 문제가 포함 됩니다.

자세한 내용은 참조 하십시오는 [Apple TV 앱 스토어에 게시](~/ios/tvos/deploy-test/app-distribution/app-store-publishing.md) 가이드입니다.

<a name="In-House-Distribution" />

## <a name="in-house-distribution"></a>사내 배포

*엔터프라이즈 배포*라고도 하는 사내 배포를 사용하면 **Apple Developer Enterprise Program**의 구성원이 내부적으로 동일한 조직의 다른 구성원에게 앱을 배포할 수 있습니다. 사내 배포에는 앱 스토어 검토가 필요하지 않으며, 응용 프로그램을 설치할 수 있는 장치의 수가 제한되지 않는다는 이점이 있습니다. 그러나 **Apple Developer Enterprise Program** 구성원에게는 iTunes Connect에 대한 액세스 권한이 **없으므로** 정식 사용자가 앱을 배포해야 합니다.

설치 하 고 내부 응용 프로그램을 배포 하는 방법에 대 한 자세한 내용은 참조 하십시오는 [사내 배포 가이드](~/ios/deploy-test/app-distribution/in-house-distribution.md)합니다. IOS에만이 문서는 있지만 tvOS 앱에 동일한 기술이 사용 됩니다.

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>임시 배포

Xamarin.tvOS 앱 둘 다에서 사용할 수 있는 임시 배포를 통해 사용자 테스트 될 수 있습니다는 **Apple 개발자 프로그램**, 및 **Apple Developer Enterprise Program**, 최대 100 개의 Apple TV 장치 테스트 해야 합니다. 임시 배포에 대 한 모범 사례는 iTunes Connect는 옵션이 없는 경우 회사 내에서 분포입니다.

설치 하 고 내부 응용 프로그램을 배포 하는 방법에 대 한 자세한 내용은 참조 하십시오는 [임시 배포 가이드](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)합니다. IOS에만이 문서는 이번 tvOS 앱에 동일한 기술이 사용 됩니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서 Xamarin.tvOS 앱에 사용할 수 있는 배포 메커니즘의 간략 한 개요를 제공 했습니다. Apple TV 앱 스토어에서 임시 및 도입 사내에서 배포 하 고 보다 자세한 정보에 대 한 링크를 제공 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
