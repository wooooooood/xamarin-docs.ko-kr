---
title: 응용 프로그램 tvOS iTunes Connect에서 구성
description: 이 문서에서는 iOS 앱 tvOS 특정 구성에 대 한 iTunes Connect에서에서 구성에 대 한 추가 지침을 제공 합니다.
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d082a980572349da1b7e6155b3aa4de41512796f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>응용 프로그램 tvOS iTunes Connect에서 구성

_이 문서에서는 iOS 앱 tvOS 특정 구성에 대 한 iTunes Connect에서에서 구성에 대 한 추가 지침을 제공 합니다._


구성 및 iOS에 따라 만들 해야 하는 설정 외에도 [iTunes Connect에서에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) 가이드에서는이 문서는 Xamarin.tvOS 해제 하는 데 필요한 특정 구성에 설명 Apple TV 앱 스토어에 응용 프로그램입니다.

<a name="Adding-a-tvOS-Release-Version" />

## <a name="adding-a-tvos-release-version"></a>TvOS 릴리스 버전 추가

Apple TV 앱 스토어에 출시 될 새 응용 프로그램을 만드는 Apple TV 지원을 기존 iOS 앱에 추가, 해야는 iTunes Connect 레코드를 만들어 한 구성 또는 특정 안내 다음 iOS를 사용 하 여:

- [iTunes Connect 레코드 만들기](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [앱 비디오 및 스크린샷 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [이름, 설명, 새로운 기능, 키워드 및 URL 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [일반 정보를 유지 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

필요에 따라도 필요할 수 있습니다.

- [Game Center 정보 유지 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [인앱 구매 정보 유지 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

위의 단계를 완료 모든 앱의 iTunes Connect 레코드 및 왼쪽 세로 막대를 사용 하 여 tvOS 지원을 추가 하려면 선택을 엽니다.

[![](itunes-connect-images/connect01.png "왼쪽 세로 막대를 사용 하 여 tvOS 지원 추가")](itunes-connect-images/connect01.png#lightbox)

TvOS 특정 정보 화면 지정된 iTunes Connect 레코드에 대해 사용할 수 있습니다.

[![](itunes-connect-images/connect02.png "TvOS 특정 정보 화면")](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information" />

## <a name="tvos-version-information"></a>tvOS 버전 정보

왼쪽 세로 막대에서 선택 **제출 위한 1.0 준비** tvOS 응용 프로그램 섹션에서:

[![](itunes-connect-images/connect03.png "tvOS 버전 정보")](itunes-connect-images/connect03.png#lightbox)

이 화면에서 다음 정보를 제공 합니다.

- 필요한 스크린샷, 설명, 키워드 및 Url입니다.
- 버전 번호, 저작권 및 Age 등급 같은 일반 응용 프로그램 정보.
- 선택적 앱에서 바로 구매 합니다.
- 순위표 및 성과 옵션 게임 센터 지원 합니다.
- 필요한 연락처, 데모 계정 및 메모와 같은 응용 프로그램 검토 정보입니다.

필요한 정보를 입력 하 고 나면 클릭는 **저장** 변경 내용을 저장 하려면 화면의 오른쪽 위 모서리에 있는 단추:

[![](itunes-connect-images/connect04.png "tvOS 제출할 준비가 버전 정보")](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review" />

## <a name="preparing-to-submit-for-review"></a>검토를 위해 제출 하기 위해 준비

마지막으로 Xamarin.tvOS 응용 프로그램을 검토를 위해 Apple TV 앱 스토어에 제출할 준비가 러 우면로 응용 프로그램의 iTunes Connect 레코드 돌아가서는 **검토를 위해 제출** 화면의 오른쪽 위 모퉁이의 단추:

[![](itunes-connect-images/connect05.png "검토를 위해 제출 합니다.")](itunes-connect-images/connect05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>요약

이 문서는 tvOS 앱을 Apple TV 앱 스토어 iTunes Connect에서에서 필요한 tvOS 특정 설정의 개요를 제공 했습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
