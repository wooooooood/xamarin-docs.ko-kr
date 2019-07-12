---
title: 엔터프라이즈 앱 개발 소개
description: 이 장에서 엔터프라이즈 앱 개발에 대해 소개 하 고 eShopOnContainers 모바일 앱을 소개 합니다.
ms.prod: xamarin
ms.assetid: cbce0659-fa03-447a-86ec-140438143230
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 4fbb4047b95fd70f829cd79e4ea26b2958273297
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831176"
---
# <a name="introduction-to-enterprise-app-development"></a>엔터프라이즈 앱 개발 소개

플랫폼과 관계 없이 엔터프라이즈 앱의 개발자가 몇 가지 과제에 직면 합니다.

-   시간이 지나면서 달라질 수 있는 앱 요구 사항입니다.
-   새 비즈니스 기회와 도전을 제공 합니다.
-   지속적인 피드백의 범위 및 앱의 요구 사항을 크게 영향을 주는 개발 중입니다.

이러한 점을 염두에서 쉽게 수정 하거나 시간에 따라 확장할 수 있는 앱을 빌드하는 것이 반드시 합니다. 독립적으로 개발 하 고 앱의 나머지 부분에 영향을 주지 않고 격리 된 상태에서 테스트 응용 프로그램의 개별 부분을 허용 하는 아키텍처가 필요 하므로 이러한 어댑터 기능에 대 한 디자인 어려울 수 있습니다.

많은 엔터프라이즈 앱은 둘 이상의 개발자 요구 하도록 충분히 복잡 합니다. 여러 개발자가 작동할 수 있도록 하지 효과적으로 앱의 다른 부분에 대해 독립적으로 조각을 나오도록 함께 원활 하 게 앱에 통합 하는 경우를 확인 하는 동안 앱을 디자인 하는 방법을 결정 하는 중요 한 문제 수 있습니다.

설계 하 고 결과에 앱을 구축 하는 기존의 방법은 라고 하는 *모놀리식* 있는 구성 요소는 밀접 하 게 없는 명확 하 게 구분을 사용 하 여 앱. 일반적으로 앱에서 다른 구성 요소를 중단 하지 않고 버그를 해결 하기 어려울 수 있기 때문에 어려운 효율성이 떨어지고 유지 관리 되는 앱이 모놀리식 접근 방식 잠재 고객 및 새 기능을 추가 하거나 기존 기능을 대체 하려면 어려울 수 있습니다.

이러한 문제에 대 한 효과적인 remedy는 쉽게 통합할 수 있습니다 함께 앱에는 불연속, 느슨하게 결합 된 구성 요소에는 앱을 분할 하는 것입니다. 이러한 방식은 몇 가지 이점이 있습니다.

-   개발, 테스트, 확장 및 다른 개인 또는 팀에서 유지 관리 하는 개별 기능 수 있습니다.
-   다시 사용 하 고 인증 및 데이터 액세스와 같은 앱의 가로 기능 사이의 세로 기능도 앱 특정 비즈니스 기능 문제를 깔끔하게 분리 승격 합니다. 따라서, 종속성 및 보다 쉽게 관리 하도록 앱 구성 요소 간의 상호 작용 합니다.
-   서로 다른 개인 또는 팀에 특정 작업 또는 전문 지식을 따라 기능 부분에 집중할 수 있도록 하 여 역할 분리를 유지할 수 있습니다. 특히 사용자 인터페이스와 앱의 비즈니스 논리 간의 명확한 분리를 제공합니다.

그러나 불연속, 느슨하게 결합 된 구성 요소를 앱을 분할할 때 해결 해야 하는 많은 문제가 있습니다. 이러한 개체는 다음과 같습니다.

-   사용자 인터페이스 컨트롤 사이의 논리는 문제를 깔끔하게 분리를 제공 하는 방법을 결정 합니다. 사용자 인터페이스 컨트롤 및 해당 하는 논리, 앱 자세히 간에 문제를 깔끔하게 분리를 만들려면 비즈니스 논리 코드 숨김 파일에 배치할 것인지 여부는 Xamarin.Forms 엔터프라이즈 앱을 만들 때 가장 중요 한 결정 중 하나 관리 하 고 테스트할 수 있습니다. 자세한 내용은 [-Model-view-viewmodel](~/xamarin-forms/enterprise-application-patterns/mvvm.md)합니다.
-   종속성 주입 컨테이너를 사용할지 여부를 결정 합니다. 종속성 주입 컨테이너 종속성 주입에서 해당 종속성을 사용 하 여 클래스의 인스턴스를 생성 하는 기능을 제공 하 여 개체 간의 결합을 줄이고 컨테이너의 구성에 따라 해당 수명을 관리 합니다. 자세한 내용은 [종속성 주입](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)합니다.
-   플랫폼 제공 이벤트 간에 이동 하 고 느슨하게 선택 개체와 형식을 참조 하 여 연결 하려면 편리 하 게 되지 않는 구성 요소 간 메시지 기반 통신 결합 되어 있습니다. 자세한 내용은 소개를 참조 하세요 [통신 간에 느슨하게 결합 된 구성 요소](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md)합니다.
-   탐색을 호출 하는 방법을 포함 하 여 페이지 간에 이동 하는 방법을 결정할 탐색 논리는 있어야 합니다. 자세한 내용은 [탐색](~/xamarin-forms/enterprise-application-patterns/navigation.md)을 참조하세요.
-   정확성에 대 한 사용자 입력의 유효성을 검사 하는 방법을 결정 합니다. 결정은 사용자 입력의 유효성을 검사 하는 방법 및 유효성 검사 오류에 대 한 사용자에 게 알릴 방법을 포함 해야 합니다. 자세한 내용은 [유효성 검사](~/xamarin-forms/enterprise-application-patterns/validation.md)합니다.
-   인증을 수행 하는 방법 및 권한 부여를 사용 하 여 리소스를 보호 하는 방법을 결정 합니다. 자세한 내용은 [인증 및 권한 부여](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md)합니다.
-   웹에서 원격 데이터에 액세스 하는 방법을 결정 서비스를 안정적으로 데이터를 검색 하는 방법 및 데이터를 캐시 하는 방법을 포함 합니다. 자세한 내용은 [원격 데이터 액세스](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md)합니다.
-   앱을 테스트 하는 방법을 결정 합니다. 자세한 내용은 [Unit Testing](~/xamarin-forms/enterprise-application-patterns/unit-testing.md)합니다.

이 가이드는이 문제에 대 한 지침을 제공 하 고 있는 핵심 패턴 및 Xamarin.Forms를 사용 하 여 플랫폼 간 엔터프라이즈 앱을 빌드하기 위한 아키텍처에 중점을 둡니다. 지침의 프레젠테이션, 프레젠테이션 논리 및 엔터티에 대 한 지원을 통해 문제를 분리 하 고 일반적인 Xamarin.Forms 엔터프라이즈 응용 프로그램 개발 시나리오를 해결 하 여 융통성 있는, 유지 관리 및 테스트 가능한 코드를 생성 하는 데 하려고 합니다 모델-뷰-ViewModel (MVVM) 패턴입니다.

## <a name="sample-application"></a>샘플 응용 프로그램

이 가이드는 다음 기능을 포함 하는 온라인 스토어는 eShopOnContainers 샘플 응용 프로그램에 포함 됩니다.

-   인증 하 고 백 엔드 서비스에 대해 권한을 부여 합니다.
-   셔츠, 커피 머그잔 및 기타 마케팅 항목의 카탈로그를 검색 합니다.
-   카탈로그를 필터링합니다.
-   카탈로그에서 항목을 주문 합니다.
-   사용자의 주문 기록을 볼 수 있습니다.
-   구성 설정입니다.

### <a name="sample-application-architecture"></a>샘플 응용 프로그램 아키텍처

그림 1-1 샘플 응용 프로그램의 아키텍처의 대략적인 개요를 제공합니다.

![](introduction-images/architecture.png "eShopOnContainers의 개략적 아키텍처")

**그림 1-1**: eShopOnContainers의 개략적 아키텍처

샘플 응용 프로그램에는 세 가지 클라이언트 앱을 사용 하 여 제공합니다.

-   ASP.NET Core MVC 응용 프로그램을 개발 합니다.
-   단일 페이지 응용 프로그램 (SPA) Typescript 및 Angular 2를 사용 하 여 개발 합니다. 이 방법은 웹 응용 프로그램에 대 한 각 작업을 사용 하 여 서버에 대 한 왕복 작업이 수행 피할 수 있습니다.
-   IOS, Android 및 Windows 플랫폼 (UWP (유니버설)을 지 원하는 Xamarin.Forms를 사용 하 여 모바일 앱을 개발 합니다.

웹 응용 프로그램에 대 한 정보를 참조 하세요 [Architecting 및 최신 웹 응용 프로그램 개발 ASP.NET Core 및 Microsoft Azure를 사용 하 여](https://aka.ms/WebAppEbook)입니다.

샘플 응용 프로그램에 다음 백 엔드 서비스에 포함 됩니다.

-   id 마이크로 서비스, ASP.NET Core Id와 IdentityServer 사용 하는 합니다.
-   카탈로그 마이크로 서비스는 데이터 기반 만들기, 읽기, 업데이트, EntityFramework 코어를 사용 하 여 SQL Server 데이터베이스를 사용 하는 (CRUD) 서비스를 삭제 합니다.
-   정렬 마이크로 서비스는 도메인 기반 디자인 패턴을 사용 하는 도메인 기반 서비스입니다.
-   basket 마이크로 서비스는 Redis Cache를 사용 하는 데이터 기반 CRUD 서비스입니다.

이러한 백 엔드 서비스는 ASP.NET Core MVC를 사용 하 여 마이크로 서비스로 구현 됩니다 하 고 단일 Docker 호스트 내에서 고유한 컨테이너로 배포 됩니다. 종합적으로 이러한 백 엔드 서비스는 eShopOnContainers 참조 응용 프로그램을 이라고 합니다. 클라이언트 앱은 REST Representational State Transfer () 웹 인터페이스를 통해 백 엔드 서비스와 통신합니다. 마이크로 서비스 및 Docker에 대 한 자세한 내용은 참조 하십시오 [컨테이너 화 된 마이크로 서비스](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md)합니다.

백 엔드 서비스 구현에 대 한 정보를 참조 하세요. [.NET 마이크로 서비스: 컨테이너화된 .NET 애플리케이션을 위한 아키텍처](https://aka.ms/microservicesebook)를 참조하세요.

### <a name="mobile-app"></a>모바일 앱

이 가이드 Xamarin.Forms를 사용한 플랫폼 간 엔터프라이즈 앱을 만드는 데 집중 하 고 예를 들어 eShopOnContainers 모바일 앱을 사용 합니다. 그림 1-2에서는 앞부분에서 설명한 기능을 제공 하는 eShopOnContainers 모바일 앱에서 페이지를 보여 줍니다.

[![](introduction-images/screenshots.png "EShopOnContainers 모바일 앱")](introduction-images/screenshots-large.png#lightbox "eShopOnContainers 모바일 앱")

**그림 1-2**: EShopOnContainers 모바일 앱

모바일 앱은 eShopOnContainers 참조 응용 프로그램에서 제공 하는 백 엔드 서비스를 사용 합니다. 그러나 백 엔드 서비스를 배포 하지 않도록 하려는 사용자에 대 한 모의 서비스에서 데이터를 사용 하도록 구성할 수 있습니다.

다음 Xamarin.Forms 기능을 연습 하는 eShopOnContainers 모바일 앱:

-   XAML
-   컨트롤
-   바인딩
-   변환기
-   스타일
-   애니메이션
-   명령
-   동작
-   트리거
-   효과
-   사용자 지정 렌더러
-   MessagingCenter
-   사용자 지정 컨트롤

이 기능에 대 한 자세한 내용은 참조는 [Xamarin.Forms 설명서](~/xamarin-forms/index.yml), 및 [Creating Mobile Apps with Xamarin.Forms](https://aka.ms/xamebook)합니다.

또한 단위 테스트는 eShopOnContainers 모바일 앱의 클래스 중 일부에 대해 제공 됩니다.

#### <a name="mobile-app-solution"></a>모바일 앱 솔루션

EShopOnContainers 모바일 앱 솔루션에 프로젝트 소스 코드 및 기타 리소스를 구성합니다. 프로젝트의 모든 범주도 소스 코드 및 기타 리소스를 구성 하는 폴더를 사용 합니다. 다음 표에서 eShopOnContainers 모바일 앱을 구성 하는 프로젝트를 설명 합니다.

|Project|Description|
|--- |--- |
|eShopOnContainers.Core|이 프로젝트는 공유 코드와 공유 UI를 포함 하는 이식 가능한 클래스 라이브러리 (PCL) 프로젝트.|
|eShopOnContainers.Droid|이 프로젝트는 Android 관련 코드를 보유 하 고 Android 앱에 대 한 진입점입니다.|
|eShopOnContainers.iOS|이 프로젝트는 iOS 관련 코드 및 iOS 앱에 대 한 진입점입니다.|
|eShopOnContainers.UWP|이 프로젝트는 유니버설 Windows 플랫폼 (UWP) 특정 코드를 보유 하 고 Windows 앱에 대 한 진입점입니다.|
|eShopOnContainers.TestRunner.Droid|이 프로젝트는 Android test runner eShopOnContainers.UnitTests 프로젝트용입니다.|
|eShopOnContainers.TestRunner.iOS|이 프로젝트는 iOS test runner eShopOnContainers.UnitTests 프로젝트용입니다.|
|eShopOnContainers.TestRunner.Windows|이 프로젝트는 eShopOnContainers.UnitTests 프로젝트에 대 한 유니버설 Windows 플랫폼 테스트 러너입니다.|
|eShopOnContainers.UnitTests|이 프로젝트는 eShopOnContainers.Core 프로젝트에 대 한 단위 테스트를 포함 합니다.|

EShopOnContainers 모바일 앱에서 클래스를 거의 또는 전혀 수정을 사용 하 여 Xamarin.Forms 앱에서 다시 사용할 수 있습니다.

##### <a name="eshoponcontainerscore-project"></a>eShopOnContainers.Core 프로젝트

EShopOnContainers.Core PCL 프로젝트에는 다음 폴더가 포함 되어 있습니다.

|폴더|설명|
|--- |--- |
|애니메이션|애니메이션을 XAML에서 사용할 수 있도록 하는 클래스가 포함 되어 있습니다.|
|동작|클래스를 보려면 노출 되는 동작을 포함 합니다.|
|컨트롤|앱에서 사용 되는 사용자 지정 컨트롤을 포함 합니다.|
|변환기|사용자 지정 논리를 바인딩에 적용 되는 값 변환기를 포함 합니다.|
|효과|포함 된 `EntryLineColorEffect` 특정의 테두리 색을 변경 하는 데 사용 되는 클래스 `Entry` 컨트롤입니다.|
|예외|사용자 지정 포함 `ServiceAuthenticationException`합니다.|
|확장|에 대 한 확장 메서드를 포함 합니다 `VisualElement` 고 `IEnumerable` 클래스입니다.|
|도우미|앱에 대 한 도우미 클래스를 포함합니다.|
|모델|앱에 대 한 모델 클래스를 포함합니다.|
|속성|포함 `AssemblyInfo.cs`,.NET 어셈블리 메타 데이터 파일.|
|서비스|앱에 제공 되는 서비스를 구현 하는 인터페이스와 클래스를 포함 합니다.|
|트리거|포함 된 `BeginAnimation` XAML의 애니메이션을 호출 하는 데 사용 되는 트리거.|
|유효성 검사|데이터 입력 유효성 검사에 관련 된 클래스를 포함 합니다.|
|ViewModels|페이지에 노출 되는 응용 프로그램 논리를 포함 합니다.|
|뷰|앱에 대 한 페이지를 포함합니다.|

##### <a name="platform-projects"></a>플랫폼 프로젝트

플랫폼 프로젝트 효과 구현, 사용자 지정 렌더러 구현 및 기타 플랫폼별 리소스를 포함 합니다.

## <a name="summary"></a>요약

Xamarin의 플랫폼 간 모바일 앱 개발 도구 및 플랫폼 포괄적인 솔루션을 제공할 B2E, B2B 및 B2C 모바일 클라이언트에 대 한 앱을 모든 대상 플랫폼 (iOS, Android 및 Windows) 및을 줄일 수 있도록 코드를 공유 하는 기능을 제공 합니다 총 소유 비용입니다. 앱은 네이티브 플랫폼 모양과 느낌을 유지 하는 동안 해당 사용자 인터페이스와 앱 논리 코드를 공유할 수 있습니다.

엔터프라이즈 앱의 개발자가 개발 하는 동안 앱의 아키텍처를 변경할 수 있는 몇 가지 문제에 직면 합니다. 따라서 수정 하거나 시간에 따라 확장할 수 있도록 앱을 작성 하는 중요 한 것입니다. 이러한 어댑터 기능에 대 한 디자인 어려울 수 있지만 일반적으로 앱을 쉽게 통합할 수 있습니다 함께 앱에는 불연속, 느슨하게 결합 된 구성 요소로 분할 합니다.


## <a name="related-links"></a>관련 링크

- [2mb PDF 전자책 다운로드](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
