---
title: "소개"
ms.topic: article
ms.prod: xamarin
ms.assetid: cbce0659-fa03-447a-86ec-140438143230
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: b8d8aed397f72df53fdca7e455b8ef723e48e0a3
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="introduction"></a>소개

플랫폼에 관계 없이 엔터프라이즈 앱의 개발자에 게 문제에 직면 여러:

-   시간이 지남에 따라 변경 될 수 있는 응용 프로그램 요구 사항입니다.
-   새로운 비즈니스 기회 문제와 문제를 해결할 수 있습니다.
-   지속적인 피드백 범위 및 응용 프로그램의 요구 사항을 크게 영향을 줄 수 있는 개발 중입니다.

이러한 점을 염두에서 이므로 쉽게 수정 하거나 시간에 따라 확장할 수 있는 응용 프로그램을 빌드하는 것이 중요 합니다. 이러한 어댑터 기능에 대 한 디자인 응용 프로그램에서 독립적으로 개발 하 고 응용 프로그램의 나머지 부분에 영향을 주지 않고 격리 된 상태에서 테스트의 개별 부분을 허용 하는 아키텍처를 필요 하기 어려울 수 있습니다.

많은 엔터프라이즈 응용 프로그램은 둘 이상의 개발자를 요구 하도록 충분히 복잡 합니다. 여러 개발자가 작동할 수 있도록 하지 효과적으로 응용 프로그램의 여러 부분에 대해 독립적으로 조각을 가져왔는지 함께 원활 하 게 응용 프로그램에 통합 된 경우 확인 한 후 앱을 디자인 하는 방법을 결정 하는 상당히 어려운 수 있습니다.

디자인 하 고 결과에 어떤 응용 프로그램을 빌드에 대 한 기존의 접근 방식은 라고는 *모놀리식* 구성 요소는 구분 기호가 없는 지우기 서로 밀접 하는 응용 프로그램입니다. 일반적으로 모놀리식이 방법은 어렵고 비효율적인을 유지 하기 위해 앱에서 다른 구성 요소를 중단시 키 지 않고도 버그를 해결 하기 어려울 수 있기 때문에 되지 않은 응용 프로그램에 따라 하 고 새로운 기능을 추가 하거나 기존 기능을 바꿉니다를 어려울 수 있습니다.

이러한 문제에 대 한 효과적인 remedy는 응용 프로그램 통합 될 수 있는 쉽게 함께 앱에는 불연속, 느슨하게 결합 된 구성 요소를 분할 하는 합니다. 이러한 접근 방식은 여러 가지 이점이 있습니다.

-   개발, 테스트, 확장 및 서로 다른 개인 이나 팀에서 유지 관리 하는 개별 기능 수 있습니다.
-   다시 사용 하 고 인증 및 데이터 액세스를 같은 응용 프로그램의 가로 기능 및 응용 프로그램 특정 비즈니스 기능과 같은 세로 기능 간에 중요 확실 하 게 구분을 승격합니다. 이 종속성와 상호 작용 간에 응용 프로그램 구성 요소를 보다 쉽게 관리할 수 있습니다.
-   서로 다른 개인 또는 팀에 특정 작업이 나 해당 전문 지식에 따라 기능에 초점을 허용 하 여 역할 구분을 유지 관리할 수 있습니다. 특히 사용자 인터페이스와 응용 프로그램의 비즈니스 논리 사이의 구분을 제공합니다.

그러나 불연속, 느슨하게 결합 된 구성 요소에는 응용 프로그램을 분할 하는 경우 확인 해야 하는 많은 문제가 있습니다. 여기에는 다음이 포함됩니다.

-   사용자 인터페이스 컨트롤 및 해당 논리 간에 중요 확실 하 게 구분을 제공 하는 방법을 결정 합니다. Xamarin.Forms 엔터프라이즈 응용 프로그램을 만들 때 가장 중요 한 결정 사항 중 하나 인지를 코드 숨김 파일에서 비즈니스 논리 배치할지 여부를 사용자 인터페이스 컨트롤 및 해당 하는 논리를 응용 프로그램 자세히 간에 중요 확실 하 게 구분을 만들지 여부를 관리 하 고 테스트할 수 있습니다. 자세한 내용은 참조 [모델-뷰-ViewModel](~/xamarin-forms/enterprise-application-patterns/mvvm.md)합니다.
-   종속성 주입 컨테이너를 사용할 것인지를 결정 합니다. 종속성 주입 컨테이너 종속성 주입을 종속성과 함께 클래스의 인스턴스를 생성 하는 기능을 제공 하 여 개체 간의 결합 줄이는 및 컨테이너의 구성에 따라 해당 수명을 관리 합니다. 자세한 내용은 참조 [종속성 주입](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)합니다.
-   선택 하 고 느슨하게 플랫폼에서 제공 이벤트 사이 개체 및 형식 참조 하 여 연결 편리한 되지 않는 구성 요소 간의 메시지 기반 통신 연관 되어 있습니다. 자세한 내용은 소개를 참조 하십시오. [통신 간에 느슨하게 결합 된 구성 요소](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md)합니다.
-   탐색을 호출 하는 방법을 포함 하 여 페이지 사이 이동 하는 방법 결정 탐색 논리 있어야 합니다. 자세한 내용은 [탐색](~/xamarin-forms/enterprise-application-patterns/navigation.md)를 참조하세요.
-   정확성에 대 한 사용자 입력의 유효성을 검사 하는 방법을 결정 합니다. 사용자 입력의 유효성을 검사 하는 방법과 유효성 검사 오류에 대 한 사용자에 게 하는 방법에 대 한 결정 포함 되어야 합니다. 자세한 내용은 참조 [유효성 검사](~/xamarin-forms/enterprise-application-patterns/validation.md)합니다.
-   인증을 수행 하는 방법 및 권한 부여를 사용 하 여 리소스를 보호 하는 방법을 결정 해야 합니다. 자세한 내용은 참조 [인증 및 권한 부여](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md)합니다.
-   웹에서 원격 데이터에 액세스 하는 방법을 결정 서비스를 안정적으로 데이터를 검색 하는 방법 및 데이터를 캐시 하는 방법을 비롯 하 여 합니다. 자세한 내용은 참조 [원격 데이터 액세스](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md)합니다.
-   응용 프로그램을 테스트 하는 방법을 결정 합니다. 자세한 내용은 참조 [유닛 테스트](~/xamarin-forms/enterprise-application-patterns/unit-testing.md)합니다.

이 가이드는이 문제에 대 한 지침을 제공 하 고 핵심 패턴 및 Xamarin.Forms를 사용 하 여 플랫폼 간 엔터프라이즈 응용 프로그램을 구축 하기 위한 아키텍처에 중점을 둡니다. 지침 목표로 하 고 문제 프레젠테이션, 프레젠테이션 논리 및에 대 한 지원을 통해 엔터티를 분리 하 여 일반적인 Xamarin.Forms 엔터프라이즈 응용 프로그램 개발 시나리오를 해결 하 여 조정 가능한, 관리 및 테스트 가능한 코드를 생성 하는 데에 모델-뷰-MVVM () 패턴입니다.

## <a name="sample-application"></a>샘플 응용 프로그램

이 가이드에 있는 다음 기능을 포함 하는 온라인 상점 eShopOnContainers 샘플 응용 프로그램에 포함 됩니다.

-   인증 하 고 백 엔드 서비스에 대해 권한을 부여 합니다.
-   Shirts, 커피 잔 및 기타 마케팅 항목의 카탈로그를 검색 합니다.
-   카탈로그를 필터링 합니다.
-   카탈로그에서 항목을 주문 합니다.
-   사용자의 주문 기록을 볼 수 있습니다.
-   설정의 구성입니다.

### <a name="sample-application-architecture"></a>샘플 응용 프로그램 아키텍처

그림 1-1 샘플 응용 프로그램의 아키텍처에 대 한 고급 개요를 제공합니다.

![](introduction-images/architecture.png "eShopOnContainers 상위 수준 아키텍처")

**그림 1-1**: eShopOnContainers 상위 수준 아키텍처

세 개의 클라이언트 앱 샘플 응용 프로그램에 제공:

-   MVC 응용 프로그램 ASP.NET Core를 사용 하 여 개발 합니다.
-   단일 페이지 응용 프로그램 (SPA) 각도 2와 Typescript를 사용 하 여 개발 합니다. 웹 응용 프로그램에 대 한이 방법을 각 작업으로 서버에 대 한 왕복을 수행 하지 않습니다.
-   모바일 앱 수 있도록 지 원하는 iOS, Android 및 유니버설 Windows 플랫폼 (UWP) Xamarin.Forms를 사용 하 여 개발 합니다.

웹 응용 프로그램에 대 한 정보를 참조 하십시오. [Architecting 및 최신 웹 응용 프로그램 개발 ASP.NET Core 및 Microsoft Azure로](http://aka.ms/WebAppEbook)합니다.

샘플 응용 프로그램에는 다음과 같은 백 엔드 서비스에 포함 됩니다.

-   identity 마이크로 서비스, ASP.NET Core Id와 IdentityServer 사용 하는 합니다.
-   데이터 기반 만들기, 카탈로그 마이크로 서비스 읽기, 업데이트, EntityFramework 코어를 사용 하 여 SQL Server 데이터베이스를 사용 하는 (CRUD) 서비스를 삭제 합니다.
-   정렬 마이크로 서비스를 도메인 기반 디자인 패턴을 사용 하는 도메인 기반 서비스인 합니다.
-   바구니 마이크로 서비스, Redis 캐시를 사용 하는 데이터 기반 CRUD 서비스인 합니다.

이러한 백 엔드 서비스 ASP.NET Core MVC를 사용 하 여 microservices로 구현 되 고 단일 Docker 호스트 내에서 고유 컨테이너로 배포 됩니다. 을 통칭 하 여이 백 엔드 서비스는 라고는 eShopOnContainers 응용 프로그램을 참조 합니다. 클라이언트 응용 프로그램이 REPRESENTATIONAL State Transfer () 웹 인터페이스를 통해 백 엔드 서비스와 통신 합니다. Microservices 및 Docker에 대 한 자세한 내용은 참조 [컨테이너 화 된 Microservices](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md)합니다.

백 엔드 서비스의 구현에 대 한 정보를 참조 하십시오. [.NET Microservices:.NET 응용 프로그램의 컨테이너 화 된 아키텍처](https://aka.ms/microservicesebook)합니다.

### <a name="mobile-app"></a>모바일 앱

이 가이드 Xamarin.Forms를 사용 하 여 플랫폼 간 엔터프라이즈 앱 개발을 중점적으로 다루며 eShopOnContainers 모바일 앱을 사용 하 여 예를 들어 합니다. 그림 1-2에서는 앞부분에서 설명한 기능을 제공 하는 eShopOnContainers 모바일 앱에서 페이지를 보여 줍니다.

[![](introduction-images/screenshots.png "EShopOnContainers 모바일 앱")](introduction-images/screenshots-large.png "eShopOnContainers 모바일 앱")

**그림 1-2**: eShopOnContainers 모바일 앱

모바일 앱 eShopOnContainers 참조 응용 프로그램에서 제공 하는 백 엔드 서비스를 사용 합니다. 그러나 백 엔드 서비스 배포를 방지 하려면 사용자에 게 모의 서비스에서 데이터를 사용 하도록 구성할 수 있습니다.

다음과 같은 Xamarin.Forms 기능을 실행 하는 eShopOnContainers 모바일 앱:

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

이 기능에 대 한 자세한 내용은 참조는 [Xamarin.Forms 설명서](~/xamarin-forms/index.yml), 및 [Xamarin.Forms 사용 하 여 모바일 앱 만들기](https://aka.ms/xamebook)합니다.

또한 단위 테스트 eShopOnContainers 모바일 앱의 클래스 중 일부에 대해 제공 됩니다.

#### <a name="mobile-app-solution"></a>모바일 응용 프로그램 솔루션

EShopOnContainers 모바일 응용 프로그램 솔루션 프로젝트도 소스 코드 및 기타 리소스를 구성합니다. 모든 프로젝트 폴더를 사용 하 여 범주로 소스 코드 및 기타 리소스를 구성 하 합니다. 다음 표에서 eShopOnContainers 모바일 앱 구성 하는 프로젝트를 설명 합니다.

<table>
<thead>
<tr class="header">
<th>프로젝트</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>eShopOnContainers.Core</td>
<td>이 프로젝트는 공유 코드와 공유 UI를 포함 하는 이식 가능한 클래스 라이브러리 (PCL) 프로젝트.</td>
</tr>
<tr class="even">
<td>eShopOnContainers.Droid</td>
<td>이 프로젝트는 특정 Android 코드를 보유 하 고 Android 앱에 대 한 진입점입니다.</td>
</tr>
<tr class="odd">
<td>eShopOnContainers.iOS</td>
<td>이 프로젝트는 특정 코드 iOS를 보유 하 고 iOS 앱에 대 한 진입점입니다.</td>
</tr>
<tr class="even">
<td>eShopOnContainers.UWP</td>
<td>이 프로젝트는 유니버설 Windows 플랫폼 (UWP) 특정 코드를 보유 하 고 Windows 앱에 대 한 진입점입니다.</td>
</tr>
<tr class="odd">
<td>eShopOnContainers.TestRunner.Droid</td>
<td>이 프로젝트는 eShopOnContainers.UnitTests 프로젝트에 대 한 Android test runner입니다.</td>
</tr>
<tr class="even">
<td>eShopOnContainers.TestRunner.iOS</td>
<td>이 프로젝트는 eShopOnContainers.UnitTests 프로젝트에 대 한 iOS test runner입니다.</td>
</tr>
<tr class="odd">
<td>eShopOnContainers.TestRunner.Windows</td>
<td>이 프로젝트는 eShopOnContainers.UnitTests 프로젝트에 대 한 유니버설 Windows 플랫폼 test runner입니다.</td>
</tr>
<tr class="even">
<td>eShopOnContainers.UnitTests</td>
<td>이 프로젝트는 eShopOnContainers.Core 프로젝트에 대 한 단위 테스트를 포함 합니다.</td>
</tr>
</tbody>
</table>

EShopOnContainers 모바일 앱에서 클래스를 수정 하지 않고 모든 Xamarin.Forms 앱에서 다시 사용할 수 있습니다.

##### <a name="eshoponcontainerscore-project"></a>eShopOnContainers.Core Project

EShopOnContainers.Core PCL 프로젝트에는 다음 폴더가 포함 되어 있습니다.

<table>
<thead>
<tr class="header">
<th>폴더</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>애니메이션</td>
<td>애니메이션을 XAML에서 사용할 수 있는 클래스를 포함 합니다.</td>
</tr>
<tr class="even">
<td>동작</td>
<td>클래스를 보려면 노출 되는 동작을 포함 합니다.</td>
</tr>
<tr class="odd">
<td>컨트롤</td>
<td>응용 프로그램에서 사용 되는 사용자 지정 컨트롤을 포함 합니다.</td>
</tr>
<tr class="even">
<td>변환기</td>
<td>바인딩에 사용자 지정 논리를 적용 하는 값 변환기를 포함 합니다.</td>
</tr>
<tr class="odd">
<td>효과</td>
<td>포함 된 <code>EntryLineColorEffect</code> 특정의 테두리 색을 변경 하는 데 사용 되는 클래스 <code>Entry</code> 컨트롤입니다.</td>
</tr>
<tr class="even">
<td>예외</td>
<td>사용자 지정 포함 <code>ServiceAuthenticationException</code>합니다.</td>
</tr>
<tr class="odd">
<td>확장</td>
<td>에 대 한 확장 메서드가 포함 되어는 <code>VisualElement</code> 및 <code>IEnumerable<T> </code> 클래스입니다.</td>
</tr>
<tr class="even">
<td>도우미</td>
<td>응용 프로그램에 대 한 도우미 클래스를 포함합니다.</td>
</tr>
<tr class="odd">
<td>모델</td>
<td>응용 프로그램에 대 한 모델 클래스를 포함합니다.</td>
</tr>
<tr class="even">
<td>속성</td>
<td>포함 <code>AssemblyInfo.cs</code>,.NET 어셈블리 메타 데이터 파일.</td>
</tr>
<tr class="odd">
<td>서비스</td>
<td>응용 프로그램에 제공 되는 서비스를 구현 하는 클래스 및 인터페이스가 포함 되어 있습니다.</td>
</tr>
<tr class="even">
<td>트리거</td>
<td>포함의 <code>BeginAnimation</code> 트리거 XAML에서 애니메이션을 호출 하는 데 사용 됩니다.</td>
</tr>
<tr class="odd">
<td>유효성 검사</td>
<td>데이터 입력을 확인 하는 관련 된 클래스를 포함 합니다.</td>
</tr>
<tr class="even">
<td>ViewModels</td>
<td>페이지에 노출 되는 응용 프로그램 논리를 포함 합니다.</td>
</tr>
<tr class="odd">
<td>보기</td>
<td>응용 프로그램에 대 한 페이지에 포함 되어 있습니다.</td>
</tr>
</tbody>
</table>

##### <a name="platform-projects"></a>플랫폼 프로젝트

플랫폼 프로젝트 효과 구현, 사용자 지정 렌더러 구현 및 기타 플랫폼 관련 리소스를 포함 합니다.

## <a name="summary"></a>요약

Xamarin 플랫폼 간 모바일 앱 개발 도구 및 플랫폼 포괄적인 솔루션을 제공 B2E, B2B, 및 B2C 모바일 클라이언트 응용 프로그램을 모든 대상 플랫폼 (iOS, Android 및 Windows) 및을 줄일 수 있도록 지 원하는 코드를 공유 하는 기능을 제공 하는 총 소유 비용입니다. 앱은 네이티브 플랫폼 모양과 느낌을 유지 하면서 해당 사용자 인터페이스 및 응용 프로그램 논리 코드를 공유할 수 있습니다.

엔터프라이즈 앱의 개발자가 개발 하는 동안 응용 프로그램의 아키텍처를 변경할 수 있는 몇 가지 직면 합니다. 따라서 그는 수정 하거나 시간에 따라 확장 될 수 있도록 응용 프로그램을 구축 해야 합니다. 이러한 어댑터 기능에 대 한 디자인 것이 어려울 수 있지만 일반적으로 통합 될 수 있는 쉽게 함께 앱에는 불연속, 느슨하게 결합 된 구성 요소를 응용 프로그램을 분할 합니다.


## <a name="related-links"></a>관련 링크

- [EBook (2mb PDF)를 다운로드 합니다.](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
