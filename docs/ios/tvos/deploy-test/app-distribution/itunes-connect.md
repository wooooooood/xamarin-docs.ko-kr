---
title: iTunes Connect에서 tvOS 앱 구성
description: 이 문서에서는 iOS에 대 한 추가 가이드를 제공 하 여 tvOS 특정 구성에 대해 iTunes Connect에서 앱을 구성 합니다.
ms.prod: xamarin
ms.assetid: 86C7C5BD-C97D-4F1D-B611-A7694557BFDF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 68c0fb9e034f432c619bc188553996bd7bacdee8
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573692"
---
# <a name="configure-your-tvos-app-in-itunes-connect"></a>iTunes Connect에서 tvOS 앱 구성

_이 문서에서는 iOS에 대 한 추가 가이드를 제공 하 여 tvOS 특정 구성에 대해 iTunes Connect에서 앱을 구성 합니다._

IOS [에서 앱 구성 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) 가이드에 따라 수행 해야 하는 구성 및 설정 외에도,이 문서에서는 Apple TV 앱 스토어에서 tvOS 앱을 해제 하는 데 필요한 특정 구성을 다룹니다.

<a name="Adding-a-tvOS-Release-Version"></a>

## <a name="adding-a-tvos-release-version"></a>TvOS 릴리스 버전 추가

Apple TV 앱 스토어에서 릴리스할 새 앱을 만들거나 기존 iOS 앱에 Apple TV 지원을 추가 하는 경우에는 iTunes Connect 레코드를 만들고 다음 iOS 관련 가이드를 사용 하 여 구성 해야 합니다.

- [iTunes Connect 레코드 만들기](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [앱 비디오 및 스크린샷 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#managing)
- [이름, 설명, 새로운 기능, 키워드 및 URL 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#metadata)
- [일반 정보 유지 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#general)

필요에 따라 다음을 요구할 수도 있습니다.

- [Game Center 정보 유지 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#game-center)
- [인앱 구매 정보 유지 관리](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#iap)

위의 모든 단계가 완료 되 면 앱의 iTunes Connect 레코드를 열고 왼쪽 세로 막대를 사용 하 여 tvOS 지원을 추가 하도록 선택 합니다.

[![](itunes-connect-images/connect01.png "Add tvOS support using the left hand sidebar")](itunes-connect-images/connect01.png#lightbox)

그러면 지정 된 iTunes Connect 레코드에 대해 tvOS 특정 정보 화면을 사용할 수 있습니다.

[![](itunes-connect-images/connect02.png "The tvOS specific information screen")](itunes-connect-images/connect02.png#lightbox)

<a name="tvOS-Version-Information"></a>

## <a name="tvos-version-information"></a>tvOS 버전 정보

왼쪽 세로 막대의 tvOS 앱 섹션에서 **1.0 제출 준비** 를 선택 합니다.

[![](itunes-connect-images/connect03.png "tvOS Version Information")](itunes-connect-images/connect03.png#lightbox)

이 화면에서 다음 정보를 제공 합니다.

- 필수 스크린샷, 설명, 키워드 및 Url입니다.
- 버전 번호, 저작권 및 연령 등급과 같은 일반적인 앱 정보입니다.
- 선택적 앱 내 구매.
- 순위표 및 성과를 사용 하는 선택적 Game Center 지원.
- 연락처, 데모 계정 및 메모와 같은 필수 앱 검토 정보

필요한 정보를 입력 한 후 화면의 오른쪽 위에 있는 **저장** 단추를 클릭 하 여 변경 내용을 저장 합니다.

[![](itunes-connect-images/connect04.png "tvOS Version Information ready for submission")](itunes-connect-images/connect04.png#lightbox)

<a name="Submitting-for-Review"></a>

## <a name="preparing-to-submit-for-review"></a>검토를 위해 제출 준비

TvOS 앱을 검토를 위해 Apple TV 앱 스토어에 제출할 준비가 되 면 앱의 iTunes Connect 레코드로 돌아가 화면의 오른쪽 위에 있는 **검토를 위해 제출** 단추를 클릭 합니다.

[![](itunes-connect-images/connect05.png "Submit for Review")](itunes-connect-images/connect05.png#lightbox)

<a name="Summary"></a>

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱을 Apple TV 앱 스토어에 릴리스 하기 위해 iTunes Connect에 필요한 tvOS 특정 설정에 대 한 개요를 제공 했습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
