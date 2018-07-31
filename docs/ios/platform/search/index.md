---
title: Xamarin.iOS의 검색 Api
description: 이 문서에서는 사용자가 정보 및 Xamarin.iOS 앱 내 기능을 검색할 수 있도록 iOS 9에서 제공 하는 새 앱 검색 Api를 사용 하 여 설명 합니다.
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 4e73e1bc34df8628790a3734e5b3b32a687fdf14
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351654"
---
# <a name="search-apis-in-xamarinios"></a>Xamarin.iOS의 검색 Api

_이 문서에서는 사용자가 정보 및 Xamarin.iOS 앱 내 기능을 검색할 수 있도록 iOS 9에서 제공 하는 앱 검색 Api를 사용 하 여 설명 합니다._

검색에서 iOS 9 다양 한 방법으로 정보 및 Xamarin.iOS 앱 내 기능에 액세스할 수 있도록 확장 되었습니다. 새 앱 검색 Api를 사용 하 여, 앱 콘텐츠 추천 및 Safari 검색 결과 핸드 오프 및 Siri 미리 알림 및 제안을 통해 검색할 수 있는 구성 됩니다. 따라서 사용자를 활동 및 앱 내에서 심층 정보를 신속 하 게 액세스할 수 있습니다.

또한 새 검색 Api를 쉽게 이전 검색 구현 환경 없이 앱에서 검색을 통합 합니다. 이 때문에 Apple는 일반적으로 몇 시간이 소요 iOS 9 앱의 콘텐츠를 앱 검색을 사용 하 여 전체적으로 검색할 수 있도록 클레임입니다.

[![](images/intro01.png "보편적으로 검색할 수 있는 앱 검색을 사용 하 여 iOS 9 앱 콘텐츠의 예")](images/intro01.png#lightbox)

앱 검색은 세 가지 별도 Api로 구성 됩니다.

1. [**NSUserActivity** ](nsuseractivity.md) -Apple iOS 8에서에서 릴리스된 핸드 오프 API의 확장입니다. 공개적으로 및 개인적으로 앱 상호 작용 기록 가능 데 사용 됩니다) 사용자가 있습니다.

2. [**핵심 스포트라이트** ](corespotlight.md) -앱이 검색 결과에 표시 되는 콘텐츠를 인덱싱할 수 있습니다. 데이터베이스 API는 것이 가장 좋은 방법은 앱 내의 개인 콘텐츠 인덱스 항목 추가 및 제거할 수 및 위치는 다음과 같이 작동 합니다.

3. [**WebMarkup** ](web-markup.md) -웹 인터페이스를 통해 해당 콘텐츠에 대 한 액세스를 제공 하는 앱에 대 한 (에서 뿐만 아니라 앱 내에서). Apple에서 크롤링할 수는 사용자의 iOS 9 장치에 앱 딥 링크를 제공 하는 특별 한 링크를 사용 하 여 웹 콘텐츠 표시할 수 있습니다.

## <a name="selecting-an-app-search-approach"></a>앱 검색 방법 선택

앱에서 제공 하는 상호 작용의 형식 및 표시 하는 콘텐츠의 형식을 종속 기술을 결정 하는 이러한 메서드를 구현 합니다.

다음 지침을 따르세요.

- [**NSUserActivity** ](nsuseractivity.md) –이 프레임 워크를 사용 하 여 공용 및 개인 콘텐츠와도 앱 내에서 탐색 지점 대화에 대 한 검색 가능성을 제공 합니다.

- [**핵심 스포트라이트** ](corespotlight.md) –이 프레임 워크를 사용 하 여 장치에 저장 된 개인 데이터에 대 한 검색 가능성을 제공 합니다.

- [**웹 태그** ](web-markup.md) –이 프레임 워크를 사용 하 여 앱의 웹 사이트 뿐만 아니라 앱 내에서 뿐만 아니라에서 콘텐츠를 제공 하는 앱에 대 한 검색 가능성을 제공 합니다.

그러나 Apple 설계 협력할 수 있도록 각 앱 검색 방법을 명확 하 고 수 개별적으로 사용 합니다. 둘 이상의 접근 방식을 사용 하 여 특정 항목 인덱스를 동일를 사용 하는지 확인 **항목 ID** 각 접근 방식에 따라서 개별 작업을 함께 연결 합니다.

둘 이상의 접근 방식을 사용 하 여 뿐만 아니라 하면 내용을 최종 사용자가 찾을 수는 있지만 검색 내에서 항목의 순위를 향상 시킬 수 있도록 해줍니다.

순위 프로세스 동안 개발자에 게 대부분 투명 한 사용자 지정된 된 항목과 상호 작용 따져 많이 (예: 링크를 눌러 사용자)이이 순위 시.
다양 한 정보 항목을 제공 하는 사용자가 수 다시 돌아온 겐 콘텐츠에 상호 작용할 수 있으므로 해당 순위를 발생 시키는 확인할 수 있습니다.

## <a name="what-content-to-index"></a>콘텐츠 인덱스를

Apple에 앱에 대 한 검색 인덱스를 제공 하는 콘텐츠 및 작업에 대 한 다음 제안 사항을 제공 합니다.

 - 모든 콘텐츠 표시, 생성 또는 사용자가 앱 내에서 큐 레이트 합니다.
 - 탐색 지점 및 앱 내 기능입니다.
 - 새 메시지, 콘텐츠 또는 다른 유형의 최근에 장치에 다운로드 된 앱에서 표시 되는 항목 등입니다.

## <a name="app-search-enhancements"></a>앱 검색 기능 향상

IOS 10에서에서 핵심 스포트라이트를 같은 앱 검색에 몇 가지 향상 된 기능 제공:

- **(사용 하 여 차등 개인) 라우드 딥 링크 인기도** -검색 결과에서 앱 딥 링크 된 콘텐츠를 홍보 하는 방법을 제공 합니다.
- **앱에서 검색** -새 `CSSearchQuery` 클래스를 설명 하는 메일, 메시지 및 메모 앱의 작동 방식을 유사한 앱 스포트라이트 검색 기능을 제공 합니다.
- **연속 검색** -추천 또는 Safari에서 검색을 시작 하 고 앱을 열 수 있도록 하 고 검색을 계속 합니다.
- **유효성 검사 결과의 시각화** -Apple [앱 검색 API 유효성 검사 도구](https://search.developer.apple.com/appsearch-validation-tool) preforming 테스트 하는 경우 이제 웹 사이트의 태그 및 딥 링크에 대 한 시각적 표현을 표시 됩니다.
- **앱 이미지 공유 메시지** -스포트라이트 검색에서 표시할 메시지 (메시지 앱 확장을)를 통해 공유에 대해 제공 된 인기 있는 앱에서 이미지를 허용 합니다.

자세한 내용을 참조 하세요 우리의 [앱 검색 기능 향상](~/ios/platform/search/app-search-enhancements.md) 가이드입니다.

### <a name="proactive-suggestions"></a>자동 제안

iOS 10 시스템 사전에 존재 하는 유용한 정보를 자동으로 사용자에 게 적절 한 시간을 허용 하 여 주행 engagement 앱에는 새로운 방법을 제공 합니다. IOS와 마찬가지로 9 앱 내의 다음 위치에서 시스템에서 사용자에 게 표시할 수 있는 기능을 노출할 수 있습니다 하는 iOS 10 사용 하 여 추천, 전달 및 Siri 제안을 사용 하 여 앱에 심층 검색을 추가 하는 기능을 제공 합니다.

- 응용 프로그램 전환기
- 잠금 화면
- CarPlay
- 맵
- Siri 상호 작용
- QuickType 제안 

앱과 같은 기술의 모음을 사용 하 여 시스템에이 기능을 노출 [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), 핵심 스포트라이트, MapKit, Media Player 및 UIKit 웹 태그입니다.

자세한 내용을 참조 하세요 우리의 [자동 제안](~/ios/platform/search/proactive-suggestions.md) 가이드입니다.

## <a name="summary"></a>요약

이 문서에서는 이러한 새 부분이 Xamarin.iOS 앱에 대 한 검색 API 기능은 iOS 9 제공 합니다. 포함 [NSUserActivity](nsuseractivity.md)를 [핵심 스포트라이트](corespotlight.md) 하 고 [웹 태그](web-markup.md) 콘텐츠 인덱싱에 대 한 메서드. 지정 된 검색 방법을 사용할 경우 및 있어야 할 콘텐츠 형식에 대해 간략히 설명 마쳤이 인덱싱됩니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [앱 검색 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
