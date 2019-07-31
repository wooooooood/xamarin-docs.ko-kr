---
title: Xamarin.ios에서 Api 검색
description: 이 문서에서는 iOS 9에서 제공 되는 새 앱 검색 Api를 사용 하 여 사용자가 Xamarin.ios 앱 내에서 정보 및 기능을 검색할 수 있도록 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 334ae6db2efa3b9d3212d0faf4d5f1bb730abb3b
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68654104"
---
# <a name="search-apis-in-xamarinios"></a>Xamarin.ios에서 Api 검색

_이 문서에서는 iOS 9에서 제공 하는 앱 검색 Api를 사용 하 여 사용자가 Xamarin.ios 앱 내에서 정보 및 기능을 검색할 수 있도록 하는 방법을 설명 합니다._

검색은 Xamarin.ios 앱 내에서 정보 및 기능에 액세스 하는 뛰어난 새 방법을 제공 하기 위해 iOS 9에서 확장 되었습니다. 새 앱 검색 Api를 사용 하 여 앱 콘텐츠는 스포트라이트 및 Safari 검색 결과, 전달 및 Siri 미리 알림 및 제안을 통해 검색할 수 있게 됩니다. 이렇게 하면 사용자가 앱 내에서 작업 및 정보 심층에 빠르게 액세스할 수 있습니다.

또한 새 검색 Api를 사용 하면 사전 검색 구현 환경을 사용 하지 않고도 앱에서 검색을 보다 쉽게 통합할 수 있습니다. 따라서 Apple은 일반적으로 앱 검색을 사용 하 여 iOS 9 앱 콘텐츠를 검색 하는 데 몇 시간 정도 걸립니다.

[![](images/intro01.png "앱 검색을 사용 하 여 범용으로 검색 가능한 iOS 9 앱 콘텐츠의 예")](images/intro01.png#lightbox)

앱 검색은 세 가지 별도의 Api로 구성 됩니다.

1. [**NSUserActivity**](nsuseractivity.md) -Apple에서 iOS 8에 릴리스된 핸드 오프 API의 확장입니다. 사용자가 공개적으로 또는 개인적으로 앱 상호 작용 기록을 검색할 수 있도록 하는 데 사용 됩니다.

2. [**핵심 스포트라이트**](corespotlight.md) -앱이 검색 결과에 표시 되도록 콘텐츠를 인덱싱할 수 있습니다. 항목을 추가 및 제거할 수 있고 앱 내에서 전용 콘텐츠를 인덱싱하는 가장 좋은 방법 인 데이터베이스 API 처럼 작동 합니다.

3. 웹 인터페이스를 통해 해당 콘텐츠에 대 한 액세스를 제공 하는 앱에 대 한 [**Webmarkup**](web-markup.md) -앱 내 에서만 사용할 수 있습니다. 웹 콘텐츠는 Apple에서 크롤링하는 특수 링크를 사용 하 여 표시 될 수 있으며 사용자의 iOS 9 장치에서 앱에 대 한 딥 링크를 제공 합니다.

## <a name="selecting-an-app-search-approach"></a>앱 검색 방법 선택

구현할 이러한 메서드를 결정 하는 것은 앱에서 제공 하는 상호 작용의 형식 및 제공 하는 콘텐츠 형식에 따라 달라 집니다.

다음 지침을 따르십시오.

- [**NSUserActivity**](nsuseractivity.md) –이 프레임 워크를 사용 하 여 공용 및 개인 콘텐츠에 대 한 높이려는을 제공 하 고 앱 내의 탐색 지점도 높이려는 합니다.

- [**핵심 스포트라이트**](corespotlight.md) –이 프레임 워크를 사용 하 여 장치에 저장 된 개인 데이터에 대 한 높이려는을 제공 합니다.

- [**웹 태그**](web-markup.md) –이 프레임 워크를 사용 하 여 앱 뿐만 아니라 앱의 웹 사이트에 있는 콘텐츠를 제공 하는 앱에 대 한 높이려는를 제공 합니다.

각 앱 검색 방법은 서로 다르며 개별적으로 사용할 수 있지만 Apple에서 함께 작동 하도록 설계 되었습니다. 여러 가지 방법을 사용 하 여 특정 항목을 인덱싱하는 경우 각 접근 방식에 동일한 **항목 ID** 를 사용 하 여 개별 링크가 함께 작동 하도록 해야 합니다.

두 개 이상의 방법을 사용 하면 콘텐츠가 최종 사용자에 게 서 검색 될 뿐만 아니라 검색 내에서 항목의 순위를 향상 시킬 수 있습니다.

순위 프로세스는 개발자에 게 가장 투명 하지만, 지정 된 항목과의 사용자 상호 작용은이 순위 (예: 사용자 눌러 링크)에 따라 크게 따져.
풍부한 정보 항목을 제공 하 여 사용자가 콘텐츠와 상호 작용 하 여 순위를 enticed 할 수 있도록 합니다.

## <a name="what-content-to-index"></a>인덱싱할 내용

Apple은 앱에서 검색 인덱스를 제공 하는 콘텐츠와 작업에 대해 다음과 같은 제안 사항을 제공 합니다.

- 앱 내에서 사용자가 보거나 만들거나 큐 레이트 하는 모든 콘텐츠입니다.
- 앱 내의 탐색 지점과 기능.
- 최근에 장치에 다운로드 된 앱에 의해 표시 되는 새로운 메시지, 콘텐츠 또는 기타 항목 유형과 같은 항목입니다.

## <a name="app-search-enhancements"></a>앱 검색 기능 향상

IOS 10의 핵심 스포트라이트는 다음과 같은 앱 검색에 대 한 몇 가지 향상 된 기능을 제공 합니다.

- **라우드 소싱 딥 링크 인기도 (차등 개인 정보 포함)** -검색 결과에서 딥 링크 된 앱 콘텐츠를 승격 하는 방법을 제공 합니다.
- **앱 내 검색** -새 `CSSearchQuery` 클래스를 사용 하 여 메일, 메시지 및 메모 앱이 작동 하는 방식과 유사한 앱 내 스포트라이트 검색 기능을 제공 합니다.
- **연속 검색** -사용자가 스포트라이트 또는 Safari에서 검색을 시작한 다음 앱을 열고 검색을 계속할 수 있습니다.
- **유효성 검사 결과 시각화** -Apple의 [앱 검색 API 유효성 검사 도구](https://search.developer.apple.com/appsearch-validation-tool) 는 이제 테스트를 미리 구성할 때 웹 사이트의 태그 및 딥 링크를 시각적으로 표시 합니다.
- **메시지 앱 이미지 공유** -메시지에서 공유할 수 있도록 제공 되는 인기 있는 앱 이미지 (메시지 앱 확장을 통해)는 스포트라이트 검색에 표시 됩니다.

자세히 알아보려면 [앱 검색 기능 향상](~/ios/platform/search/app-search-enhancements.md) 가이드를 참조 하세요.

### <a name="proactive-suggestions"></a>자동 제안

iOS 10은 시스템이 적절 한 시간에 자동으로 유용한 정보를 사용자에 게 자동으로 제공할 수 있도록 하 여 앱에 대 한 참여를 유도 하는 새로운 방법을 제공 합니다. IOS 9에서 스포트라이트, 전달 및 Siri 제안을 사용 하 여 앱에 심층 검색을 추가 하는 기능을 제공 하는 것 처럼 iOS 10에서 앱은 다음 위치에서 시스템을 통해 사용자에 게 제공할 수 있는 기능을 노출할 수 있습니다.

- 앱 전환기
- 잠금 화면
- CarPlay
- 지도
- Siri 상호 작용
- QuickType 제안 

앱은 [NSUserActivity](xref:Foundation.NSUserActivity), 웹 마크업, Core 스포트라이트, mapkit, Media Player 및 uikit와 같은 기술 컬렉션을 사용 하 여 시스템에이 기능을 노출 합니다.

자세히 알아보려면 [사전 권장 사항](~/ios/platform/search/proactive-suggestions.md) 가이드를 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 iOS 9에서 Xamarin.ios 앱에 대해 제공 하는 새로운 검색 API 기능에 대해 살펴보았습니다. 콘텐츠 인덱싱에 대 한 [NSUserActivity](nsuseractivity.md), [핵심 스포트라이트](corespotlight.md) 및 [웹 마크업](web-markup.md) 메서드를 다룹니다. 지정 된 검색 방법이 사용 되어야 하는 경우와 인덱싱되는 콘텐츠 형식에 대 한 간단한 설명으로 완료 되었습니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [앱 검색 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
