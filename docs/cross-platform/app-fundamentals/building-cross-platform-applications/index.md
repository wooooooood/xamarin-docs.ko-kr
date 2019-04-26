---
title: 플랫폼 간 애플리케이션 빌드
description: 이 섹션에서는 요약 및 6 개 부분에서 – Xamarin 모바일 앱을 디자인 하 고 다음 테스트 및 다양 한 앱 스토어에 배포 작동 방식 이해에서 Xamarin 개발 플랫폼을 사용 하 여 응용 프로그램을 빌드하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: 683400e24844308769f0562552641216d45e7d11
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61276365"
---
# <a name="building-cross-platform-applications"></a>플랫폼 간 애플리케이션 빌드

가지 플랫폼 간 모바일 응용 프로그램 간에 코드 공유에 대 한 두 가지 옵션이 있습니다. 자산 프로젝트 및 이식 가능한 클래스 라이브러리를 공유 합니다. 이러한 옵션은 [여기에서 설명한](~/cross-platform/app-fundamentals/code-sharing.md); 대 한 자세한 내용은 [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md) 하 고 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md) 도 제공 됩니다.

<a name="Sections" />

 [개요](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [파트 1-Xamarin Mobile Platform 이해](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [Part 2-아키텍처](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [– 3 부는 Xamarin 플랫폼 간 솔루션 설정](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [4 – 부 다중 플랫폼 처리](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [실제 코드 공유 전략 – 5 부](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [6부 - 테스트 및 App Store 승인](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />

## <a name="case-studies"></a>사례 연구

이 문서에서 설명한 원칙은 연습 샘플 응용 프로그램에서에 배치 됩니다 *Tasky*, 뿐만 [미리 작성 된 응용 프로그램](https://xamarin.com/prebuilt) 처럼 [Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm)합니다.

 <a name="Tasky" />

### <a name="tasky"></a>Tasky

Tasky은 iOS, Android 및 Windows Phone 대 한 간단한 할 일 목록 응용 프로그램입니다.
Xamarin을 사용 하 여 플랫폼 간 응용 프로그램을 만드는 기본 사항을 설명 하 고 로컬 SQLite 데이터베이스를 사용 합니다.

 [![목록 tasky](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [ ![tasky 목록](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

읽기를 [Tasky 사례 연구](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)합니다.

## <a name="summary"></a>요약

이 섹션에서는 Xamarin의 응용 프로그램 개발 도구를 소개 하 고 여러 모바일 플랫폼을 대상으로 하는 응용 프로그램을 구축 하는 방법에 설명 합니다.

여러 플랫폼을 통해 해당 구조 코드 다시 사용 하기 위해 계층화 된 아키텍처를 설명 하 고 해당 아키텍처 내에서 사용할 수 있는 다양 한 소프트웨어 패턴에 설명 합니다.

일반적인 응용 프로그램 기능 (예: 파일 및 네트워크 작업) 및 플랫폼 간 방식으로 빌드할 수 있습니다 하는 방법의 예로 제공 됩니다.

마지막으로 간단 하 게 테스트에 대해 설명 하 고 작업에 이러한 원칙을 배치 하는 사례 연구에 대 한 참조를 제공 합니다.

## <a name="related-links"></a>관련 링크

- [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md)
- [사례 연구: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky 샘플 앱 (github)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Xamarin 모바일 응용 프로그램 개발: 플랫폼 간 C# 및 Xamarin.Forms 기본 사항 (Amazon)](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/)
- [사용한 모바일 개발 C# 에서 Greg Shackles (O'Reilly)](http://shop.oreilly.com/product/0636920024002.do)
- [전문 플랫폼 간 모바일 개발 C# Scott Olson, John Hunter, Ben Horgen, Kenny 베 (Wrox)](http://www.wrox.com/WileyCDA/WroxTitle/Professional-Cross-Platform-Mobile-Development-in-C-.productCd-1118157702.html)
