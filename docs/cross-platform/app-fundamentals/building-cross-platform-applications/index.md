---
title: 크로스 플랫폼 응용 프로그램 빌드
description: 이 섹션에 요약 외에 여섯 부분에서 모바일 앱을 디자인 한 다음 테스트와 다양 한 응용 프로그램 저장소에 배포 하려면 Xamarin 작동 방식 이해 – Xamarin 개발 플랫폼을 사용 하 여 응용 프로그램을 빌드할 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: fba13ab921949cd2361e78535d5ffc96952a1336
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/10/2018
---
# <a name="sharing-code-options"></a>코드 공유 옵션

플랫폼 간 모바일 응용 프로그램 사이 코드를 공유 하는 데에 두 가지가: 공유 자산 프로젝트 및 이식 가능한 클래스 라이브러리입니다. 이러한 옵션은 [여기에서 설명한](~/cross-platform/app-fundamentals/code-sharing.md);에 대 한 자세한 내용은 [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md) 및 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md) 제공 됩니다.

<a name="Sections" />

## <a name="building-cross-platform-mobile-apps"></a>크로스 플랫폼 모바일 앱 빌드

 [개요](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [1 – 모바일 Xamarin 플랫폼 이해](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [Part 2-아키텍처](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [3 – 부 Xamarin 크로스 플랫폼 솔루션 설정](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [-4 부를 다루는 여러 플랫폼](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [유용한 코드 전략을 공유-5 부](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [6부 - 테스트 및 App Store 승인](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />


## <a name="case-studies"></a>사례 연구

이 문서에서 설명 하는 원칙 연습 예제 응용 프로그램에서에 배치 됩니다 *Tasky*,으로 [미리 응용 프로그램을 빌드한](https://xamarin.com/prebuilt) 같은 [Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm)합니다.

 <a name="Tasky" />


### <a name="tasky"></a>Tasky

Tasky iOS, Android 및 Windows Phone 대 한 간단한 할 일 목록 응용 프로그램이입니다.
가상 컴퓨터는 기본적인 Xamarin을 사용한 플랫폼 간 응용 프로그램을 만드는 방법을 보여 줍니다와 로컬 SQLite 데이터베이스를 사용 합니다.

 [![tasky 목록](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [ ![tasky 목록](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

읽기는 [Tasky 사례 연구](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)합니다.


## <a name="summary"></a>요약

이 섹션 Xamarin의 응용 프로그램 개발 도구를 소개 하 고 여러 모바일 플랫폼을 대상으로 하는 응용 프로그램을 구축 하는 방법을 설명 합니다.

여러 플랫폼에서 계층화 된 아키텍처를 해당 구조체 코드 다시 사용에 대 한 처리 하 고 해당 아키텍처에서 사용할 수 있는 다양 한 소프트웨어 패턴에 설명 합니다.

일반적인 응용 프로그램 기능 (예: 파일 및 네트워크 작업) 및 플랫폼 간 방식으로 구축 방법의 예로 제공 됩니다.

마지막으로 간단 하 게 테스트를 설명 하 고 작업에 이러한 원칙을 저장 하는 사례 연구에 대 한 참조를 제공 합니다.



## <a name="related-links"></a>관련 링크

- [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md)
- [사례 연구: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky 샘플 응용 프로그램 (github)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Xamarin 모바일 응용 프로그램 개발: 플랫폼 간 C# 및 Xamarin.Forms 기초](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/))
- [Greg으로 C#을 사용한 모바일 개발 Shackles (O'Reilly)](http://shop.oreilly.com/product/0636920024002.do)
- [C# Scott Olson, John 헌터, Ben Horgen, Kenny 베 (Wrox) 전문 플랫폼 간 모바일 개발](http://www.wiley.com/WileyCDA/WileyTitle/productCd-1118157702.html)
