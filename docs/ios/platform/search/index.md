---
title: "새 검색 Api"
description: "이 문서에서는 사용자가 정보 및 Xamarin.iOS 앱 내 기능을 검색할 수 있도록 iOS 9에서 제공 하는 새 응용 프로그램 검색 Api를 사용 하 여 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 2d802a96fcc8dad1d610b99a1cddffdc4398da38
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="new-search-apis"></a>새 검색 Api

_이 문서에서는 사용자가 정보 및 Xamarin.iOS 앱 내 기능을 검색할 수 있도록 iOS 9에서 제공 하는 새 응용 프로그램 검색 Api를 사용 하 여 설명 합니다._

검색에서 iOS 9 다양 한 방법으로 정보 및 Xamarin.iOS 앱 내 기능에 액세스할 수 있도록 확장 되었습니다. 새 응용 프로그램 검색 Api를 사용 하 여 응용 프로그램 콘텐츠 스포트라이트 및 Safari 검색 결과 전달 하 고 Siri 미리 알림 및 제안을 통해 검색할 수 있는 구성 됩니다. 따라서 사용자가 활동 및 응용 프로그램에서 세부적인 정보를 신속 하 게 액세스할 수 있습니다.

또한 새로운 검색 Api 쉽게 검색 이전 검색 구현 환경 없이 응용 프로그램에 통합할 수 있습니다. 이 인해 Apple iOS 9 응용 프로그램의 콘텐츠를 앱 검색을 사용 하 여 범용으로 검색할 수 있도록 하는 데 몇 시간이 걸릴 일반적으로 클레임입니다.

[ ![](images/intro01.png "전체적으로 검색할 수 있는 앱 검색을 사용 하 여 iOS 9 앱 콘텐츠의 예")](images/intro01.png)

앱 검색의 세 가지 별도 Api로 구성 됩니다.

1. [**NSUserActivity** ](nsuseractivity.md) -Apple iOS 8에에서 릴리스 하는 전달 API의 확장입니다. 공개적으로 및 개인적으로 응용 프로그램 상호 작용 내용을 검색할 수 있도록 하는 데 사용 됩니다) 사용자가 있습니다.

2. [**스포트라이트 핵심** ](corespotlight.md) -앱이 콘텐츠를 검색 결과에 표시 될 수 있습니다. 데이터베이스 API 위치 항목을 추가 및 제거할 수 있는 것이 가장 좋은 방법은 응용 프로그램 내에서 개인 콘텐츠 인덱스 처럼 작동 합니다.

3. [**WebMarkup** ](web-markup.md) -웹 인터페이스를 통해 해당 콘텐츠에 대 한 액세스를 제공 하는 앱에 대 한 (뿐 아니라 앱 내에서). Apple에서 탐색 하 고 사용자의 iOS 9 장치에 앱에 대 한 직접 링크를 제공 하는 특별 한 링크와 함께 웹 콘텐츠 표시할 수 있습니다.

## <a name="selecting-an-app-search-approach"></a>앱 검색 방법 선택

기술을 결정 하는 이러한 메서드를 구현 하는 유형의 응용 프로그램에서 제공 하는 상호 작용 및 표시 하는 콘텐츠의 형식에 따라 달라 집니다.

다음 지침을 따르세요.

- [**NSUserActivity** ](nsuseractivity.md) –이 프레임 워크를 사용 하 여 검색 기능은 공용 및 개인 콘텐츠 모두 및 앱에서 탐색 포인트의 검색 기능은 제공 합니다.

- [**스포트라이트 핵심** ](corespotlight.md) –이 프레임 워크를 사용 하 여 장치에 저장 된 개인 데이터에 대 한 검색을 제공 합니다.

- [**태그 웹** ](web-markup.md) –이 프레임 워크를 사용 하 여 응용 프로그램 내에 있지만 응용 프로그램의 웹 사이트의 경우에에서 뿐만 아니라에서 콘텐츠를 제공 하는 앱에 대 한 검색 기능은 제공 합니다.

그러나 Apple 설계 함께 작동 하도록 각 응용 프로그램 검색 방법을 명확 하 고 수 개별적으로 사용 합니다. 둘 이상의 접근 방식을 사용 하 여 특정 항목을 인덱스를 동일를 사용 하는지 확인 **항목 ID** 각 접근 방식에 따라서 개별 작업을 함께 연결 합니다.

둘 이상의 접근 방식을 사용 하 여 뿐만 아니라 하면 콘텐츠는 최종 사용자가 찾을 수 있지만 검색 내에서 항목의 순위를 향상 시킬 수 있도록 해줍니다.

순위 프로세스 동안 대부분의 개발자에 게 투명 한 주어진 항목과 사용자 상호 작용 정도와 과도 하 게 (예: 링크를 눌러 사용자)이이 순위 시.
다양 하 고 정보를 제공 하는 항목을 제공 하는 사용자는 수 다시 돌아온 겐 하 여 콘텐츠를 보고 상호작용할 순위가 위쪽을 확인할 수 있습니다.

## <a name="what-content-to-index"></a>콘텐츠 인덱스를

Apple 응용 프로그램에 대 한 검색 인덱스를 제공 하는 콘텐츠 및 작업에 대 한 다음 제안 사항을 제공 합니다.

 - 모든 콘텐츠 표시, 만든 또는 앱 내에서 사용자가 큐 레이트 합니다.
 - 탐색 지점 및 응용 프로그램의 기능입니다.
 - 새 메시지, 콘텐츠 또는 다른 종류의 장치에 다운로드 된 최근에 앱에 의해 표시 되는 항목 같이.

## <a name="app-search-enhancements"></a>앱 검색의 향상 된 기능

IOS 10에서에서 핵심 스포트라이트 여러 개선 된 기능 앱 검색와 같은 제공:

- **(차등 개인 정보)를 Crowdsourced 딥 링크 인기도** -검색 결과의 앱 딥 링크 된 콘텐츠를 승격 하는 방법을 제공 합니다.
- **앱에서 검색** -새로운 `CSSearchQuery` 메일, 메시지 및 메모 앱 작동 방법을 비슷합니다 앱에서 스포트라이트 검색 기능을 제공 하는 클래스입니다.
- **연속 검색** -스포트라이트 또는 Safari, 검색을 시작한 다음 앱을 열 수 있도록 허용 하 고 해당 검색을 계속 합니다.
- **유효성 검사 결과의 시각화** -Apple의 [앱 검색 API 유효성 검사 도구](https://search.developer.apple.com/appsearch-validation-tool) preforming 테스트 하는 경우 이제 웹 사이트의 태그와 딥 링크 시각적 표시 됩니다.
- **응용 프로그램 이미지 공유 메시지** -스포트라이트 검색에서 표시할 메시지 (메시지 응용 프로그램 확장)를 통해 공유 하는 데 제공 하는 인기 있는 앱에서 이미지를 허용 합니다.

자세한 내용은 참조 하세요 우리의 [앱 검색 향상](~/ios/platform/search/app-search-enhancements.md) 가이드입니다.

### <a name="proactive-suggestions"></a>자동 관리 제안

iOS 10 시스템에 사전 유용한 정보를 자동으로 사용자에 게 표시 적절 한 시간에 허용 하 여 새를 응용 프로그램 구동 engagement 방법에 설명 합니다. IOS와 마찬가지로 9 스포트라이트, 전달 및 Siri 제안 iOS 10 응용 프로그램은 다음 위치 내에서 시스템 사용자에 게 표시 될 수 있는 기능을 노출할 수를 사용 하 여 앱에 심층 검색을 추가 하는 기능을 제공 합니다.

- 응용 프로그램 전환기
- 잠금 화면
- CarPlay
- 맵
- Siri 상호 작용
- QuickType 제안 

와 같은 기술의 모음을 사용 하 여 시스템에이 기능을 노출 하는 응용 프로그램 [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), 코어 스포트라이트, MapKit, 미디어 플레이어 및 UIKit 웹 태그입니다.

자세한 내용은 참조 하세요 우리의 [사전 제안](~/ios/platform/search/proactive-suggestions.md) 가이드입니다.

## <a name="summary"></a>요약

이 문서의 새 검사가 수행 Xamarin.iOS 앱에 대 한 검색 API 기능 iOS 9를 제공 합니다. 포함 되어 [NSUserActivity](nsuseractivity.md), [코어 스포트라이트](corespotlight.md) 및 [웹 태그](web-markup.md) 콘텐츠 인덱싱에 대 한 메서드. 주어진된 검색 접근을 사용 해야 하는 시기 및 유형의 콘텐츠를의 내용은 보충 마쳤으나 인덱싱됩니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [앱 검색 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
