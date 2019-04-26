---
title: iTunes Connect에서 tvOS 앱 구성
description: 이 문서에서는 iOS tvOS 특정 구성에 대 한 iTunes Connect에서에서 앱 구성에 대 한 추가 지침을 제공 합니다.
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3f4ef00cfe990de2d5afd461d7a110d32bc4a236
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61413234"
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>iTunes Connect에서 tvOS 앱 구성

_이 문서에서는 iOS tvOS 특정 구성에 대 한 iTunes Connect에서에서 앱 구성에 대 한 추가 지침을 제공 합니다._


구성 및 iOS 다음에서 확인 해야 하는 설정 하는 것 외에도 [iTunes Connect에서에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) 가이드에서는이 문서에서는 설명 Xamarin.tvOS를 해제 해야 하는 특정 구성 Apple TV App Store에서 앱입니다.

<a name="Adding-a-tvOS-Release-Version" />

## <a name="adding-a-tvos-release-version"></a>TvOS 릴리스 버전을 추가합니다.

Apple TV 지원 기존 iOS 앱에 추가 해야 iTunes Connect 레코드를 만들 있고 구성 또는 Apple TV 앱 스토어에 출시 될 새 앱을 만들면 다음 iOS를 사용 하 여 특정을 안내 합니다.

- [iTunes Connect 레코드 만들기](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [앱 비디오 및 스크린샷 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [이름, 설명, 새로운 기능, 키워드 및 URL 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [일반 정보를 유지 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

필요에 따라 필요할 수도 있습니다.

- [Game Center 정보 유지 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [인앱 구매 정보 유지 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

위의 단계가 완료 된 모든 앱의 iTunes Connect 레코드 및 왼쪽 세로 막대를 사용 하 여 tvOS 지원을 추가 하기 위해 선택한을 엽니다.

[![](itunes-connect-images/connect01.png "왼쪽 세로 막대를 사용 하 여 tvOS 지원 추가")](itunes-connect-images/connect01.png#lightbox)

TvOS 특정 정보 화면 지정된 iTunes Connect 레코드를 사용할 수 있습니다.

[![](itunes-connect-images/connect02.png "TvOS 특정 정보 화면")](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information" />

## <a name="tvos-version-information"></a>tvOS 버전 정보

왼쪽 사이드바에서 선택 **1.0 제출 준비** tvOS 앱 섹션:

[![](itunes-connect-images/connect03.png "tvOS 버전 정보")](itunes-connect-images/connect03.png#lightbox)

이 화면에서 다음 정보를 제공 합니다.

- 필수 스크린샷, 설명, 키워드 및 Url입니다.
- 버전 번호, 저작권 연령별 등급 등 일반 앱 정보입니다.
- 선택적 앱 내 구매 합니다.
- 순위표 및 성과 사용 하 여 선택적 Game Center 지원 합니다.
- 필요한 정보, 연락처 및 데모 계정 등 앱 검토 정보입니다.

필요한 정보를 입력 하 고 나면 클릭 합니다 **저장할** 변경 내용을 저장 하려면 화면의 오른쪽 위 모서리의 단추:

[![](itunes-connect-images/connect04.png "tvOS 제출할 준비가 버전 정보")](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review" />

## <a name="preparing-to-submit-for-review"></a>검토를 위해 제출 하기 위한 준비

Xamarin.tvOS 앱 검토를 위해 Apple TV App Store에 제출 하는 데 준비가 인 경우 앱의 iTunes Connect 레코드를 반환 하 고 클릭 합니다 **검토를 위해 제출** 화면 오른쪽 위 모서리에 있는 단추:

[![](itunes-connect-images/connect05.png "검토를 위해 제출")](itunes-connect-images/connect05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 iTunes Connect에서에서 tvOS 앱을 Apple TV App Store를 릴리스 하는 데 필요한 tvOS 특정 설정의 개요를 제공 했습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
