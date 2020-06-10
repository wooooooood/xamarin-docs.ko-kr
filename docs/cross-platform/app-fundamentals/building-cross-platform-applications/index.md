---
title: 플랫폼 간 애플리케이션 빌드
description: 이 섹션에서는이 항목의 요약에서는 6 개의 부분으로, xamarin 개발 플랫폼을 사용 하 여 응용 프로그램을 빌드하는 방법에 대해 설명 하 고, Xamarin이 모바일 앱을 설계 하는 방법을 이해 하 고, 다양 한 앱 스토어에서 테스트 및 배포 하는
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
author: davidortinau
ms.author: daortin
ms.date: 01/28/2016
ms.openlocfilehash: 2f7d09405f90ac9fc4c3ce80181baafa447df637
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571244"
---
# <a name="building-cross-platform-applications"></a>플랫폼 간 애플리케이션 빌드

플랫폼 간 모바일 응용 프로그램 간에 코드를 공유 하는 두 가지 옵션이 있습니다. 공유 자산 프로젝트와 이식 가능한 클래스 라이브러리입니다. 이러한 옵션은 [여기에 설명](~/cross-platform/app-fundamentals/code-sharing.md)되어 있습니다. [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md) 및 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md) 에 대 한 자세한 정보도 제공 됩니다.

<a name="Sections"></a>

 [개요](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [1 부-Xamarin Mobile Platform 이해](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [2 부-아키텍처](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [3 부-Xamarin 플랫폼 간 솔루션 설정](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [4 부-여러 플랫폼 처리](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [5 부-실제 코드 공유 전략](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [6 부-테스트 및 앱 스토어 승인](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies"></a>

## <a name="case-studies"></a>사례 연구

이 문서에서 설명 하는 원칙은 [XAMARIN CRM](https://xamarin.com/prebuilt/#xamarincrm)과 같은 [미리 빌드된 응용 프로그램](https://xamarin.com/prebuilt) 뿐만 아니라 샘플 응용 프로그램의 *tasky*에서 적용 됩니다.

 <a name="Tasky"></a>

### <a name="tasky"></a>Tasky

Tasky는 iOS, Android 및 Windows Phone에 대 한 간단한 할 일 목록 응용 프로그램입니다.
Xamarin을 사용 하 여 플랫폼 간 응용 프로그램을 만들고 로컬 SQLite 데이터베이스를 사용 하는 기본 사항을 보여 줍니다.

 [ ![ tasky 목록](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [ ![ tasky](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) 목록

[Tasky 사례 연구](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)를 읽습니다.

## <a name="summary"></a>요약

이 섹션에서는 Xamarin의 응용 프로그램 개발 도구를 소개 하 고 여러 모바일 플랫폼을 대상으로 하는 응용 프로그램을 빌드하는 방법을 설명 합니다.

여러 플랫폼에서 재사용할 수 있는 코드를 구조화 하는 계층화 된 아키텍처에 대해 설명 하 고 해당 아키텍처 내에서 사용할 수 있는 다양 한 소프트웨어 패턴을 설명 합니다.

예는 일반적인 응용 프로그램 기능 (예: 파일 및 네트워크 작업)과 플랫폼 간 방식으로 빌드하는 방법을 제공 합니다.

마지막으로 테스트에 대해 간략하게 설명 하 고 이러한 원칙을 작업에 배치 하는 사례 연구에 대 한 참조를 제공 합니다.

## <a name="related-links"></a>관련 링크

- [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md)
- [사례 연구: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky 샘플 앱 (github)](https://docs.microsoft.com/samples/xamarin/mobile-samples/taskyportable/)
- [Xamarin 모바일 응용 프로그램 개발: 플랫폼 간 c # 및 Xamarin. 양식 기본 사항 (Amazon)](https://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/)
- [Greg Shackles (O'Reilly)로 c #을 사용한 모바일 개발](https://shop.oreilly.com/product/0636920024002.do)
