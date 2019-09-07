---
title: Xamarin.ios를 사용 하는 엔터프라이즈 응용 프로그램 패턴 전자책
description: 이 전자책는 조정 가능 하 고, 유지 관리 가능 하며, 테스트 가능한 Xamarin.ios enterprise 응용 프로그램을 개발 하기 위한 아키텍처 지침을 제공 합니다.
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: a26fdd931f539da990e21166eec361fd1702de9c
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760211"
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Xamarin.ios를 사용 하는 엔터프라이즈 응용 프로그램 패턴 전자책

_조정 가능 하 고, 유지 관리 가능 하며, 테스트 가능한 Xamarin을 개발 하기 위한 아키텍처 지침. Forms enterprise 응용 프로그램_

![](images/cover-sml.png "Xamarin.ios를 사용 하는 엔터프라이즈 응용 프로그램 패턴 전자책")

이 전자책는 느슨한 결합을 유지 하면서 MVVM (모델 뷰-ViewModel) 패턴, 종속성 주입, 탐색, 유효성 검사 및 구성 관리를 구현 하는 방법에 대 한 지침을 제공 합니다. 또한 IdentityServer를 사용 하 여 인증 및 권한 부여를 수행 하 고, 컨테이너 화 된 마이크로 서비스에서 데이터에 액세스 하 고, 단위 테스트를 수행 하는 방법도 제공 됩니다.

## <a name="prefaceprefacemd"></a>[서문](preface.md)

이 장에서는 가이드의 목적과 범위 및 대상 사용자에 대해 설명 합니다.

## <a name="introductionintroductionmd"></a>[소개](introduction.md)

엔터프라이즈 앱 개발자는 개발 중에 앱의 아키텍처를 변경할 수 있는 몇 가지 문제를 직면 하 고 있습니다. 따라서 시간이 지남에 따라 앱을 수정 하거나 확장할 수 있도록 앱을 빌드하는 것이 중요 합니다. 이러한 적응성 설계는 어려울 수 있지만 일반적으로 앱에 쉽게 통합 될 수 있는 느슨하게 결합 된 개별 구성 요소로 앱을 분할 해야 합니다.

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

MVVM (모델-뷰-ViewModel) 패턴은 응용 프로그램의 비즈니스 및 프레젠테이션 논리를 UI (사용자 인터페이스)와 완전히 분리 하는 데 도움이 됩니다. 응용 프로그램 논리와 UI를 명확 하 게 분리 하면 수많은 개발 문제를 해결 하 고 응용 프로그램을 더 쉽게 테스트 하 고 유지 관리 하 고 진화 시킬 수 있습니다. 또한 코드 다시 사용 기회를 크게 향상 시킬 수 있으며, 개발자와 UI 디자이너는 앱의 각 부분을 개발할 때 더 쉽게 공동 작업을 수행할 수 있습니다.

## <a name="dependency-injectiondependency-injectionmd"></a>[종속성 주입](dependency-injection.md)

종속성 주입을 통해 이러한 형식에 종속 된 코드에서 구체적인 형식을 분리할 수 있습니다. 일반적으로 인터페이스와 추상 형식 간의 등록 및 매핑 목록과 이러한 형식을 구현 하거나 확장 하는 구체적인 형식을 포함 하는 컨테이너를 사용 합니다.

종속성 주입 컨테이너를 사용 하면 클래스 인스턴스를 인스턴스화하고 컨테이너의 구성에 따라 수명을 관리 하는 기능을 제공 하 여 개체 간의 결합을 줄일 수 있습니다. 개체를 만드는 동안 컨테이너는 개체에 필요한 모든 종속성을 삽입 합니다. 이러한 종속성을 아직 만들지 않은 경우 컨테이너는 먼저 종속성을 만들고 확인 합니다.

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[느슨하게 결합된 구성 요소 간 통신](communicating-between-loosely-coupled-components.md)

Xamarin.Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 클래스는 게시-구독 패턴을 구현하여 개체 및 형식 참조로 연결하기 불편한 구성 요소 사이의 메시지 기반 통신을 허용합니다. 이 메커니즘을 통해 게시자와 구독자는 서로에 대 한 참조 없이 통신할 수 있으며, 구성 요소 간의 종속성을 줄이고 구성 요소를 독립적으로 개발 하 고 테스트할 수 있습니다.

## <a name="navigationnavigationmd"></a>[탐색](navigation.md)

Xamarin에는 내부 논리 기반 상태 변경으로 인해 일반적으로 사용자가 UI와 상호 작용 하거나 앱 자체에서 발생 하는 페이지 탐색에 대 한 지원이 포함 됩니다. 그러나 MVVM 패턴을 사용 하는 앱에서 탐색을 구현 하는 것은 복잡할 수 있습니다.

이 장에서는 모델 `NavigationService` 보기에서 모델을 처음 탐색할 때 사용 되는 클래스를 제공 합니다. 뷰 모델 클래스에 탐색 논리를 배치 하면 자동화 된 테스트를 통해 논리를 수행할 수 있습니다. 또한 뷰 모델은 특정 비즈니스 규칙이 적용 되도록 탐색을 제어 하는 논리를 구현할 수 있습니다.

## <a name="validationvalidationmd"></a>[유효성 검사](validation.md)

사용자의 입력을 허용 하는 모든 앱은 입력이 올바른지 확인 해야 합니다. 유효성 검사를 수행 하지 않으면 사용자가 응용 프로그램 실패를 유발 하는 데이터를 제공할 수 있습니다. 유효성 검사는 비즈니스 규칙을 적용 하 고 공격자가 악성 데이터를 삽입 하는 것을 방지 합니다.

MVVM (모델-뷰-ViewModel) 패턴의 컨텍스트에서는 사용자가 수정할 수 있도록 데이터 유효성 검사를 수행 하 고 뷰에 유효성 검사 오류를 알리기 위해 뷰 모델 또는 모델이 종종 필요 합니다.

## <a name="configuration-managementconfiguration-managementmd"></a>[구성 관리](configuration-management.md)

설정을 사용 하면 응용 프로그램의 동작을 구성 하는 데이터를 분리 하 여 앱을 다시 빌드하지 않고도 동작을 변경할 수 있습니다. 앱 설정은 앱에서 만들고 관리 하는 데이터 이며, 사용자 설정은 앱의 동작에 영향을 주는 앱의 사용자 지정 가능한 설정 이며 자주 다시 조정할 필요가 없습니다.

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[컨테이너화된 마이크로 서비스](containerized-microservices.md)

마이크로 서비스는 최신 클라우드 응용 프로그램의 민첩성, 확장성 및 안정성 요구 사항에 적합 한 응용 프로그램 개발 및 배포에 대 한 접근 방식을 제공 합니다. 마이크로 서비스의 주요 이점 중 하나는 독립적으로 확장 될 수 있다는 것입니다. 즉, 필요에 따라의 영역을 불필요 하 게 확장 하지 않고도 요구를 지원 하기 위해 더 많은 처리 능력과 네트워크 대역폭이 필요한 특정 기능 영역을 확장할 수 있습니다. 요구가 증가 하지 않은 응용 프로그램입니다.

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[인증 및 권한 부여](authentication-and-authorization.md)

ASP.NET MVC 웹 응용 프로그램과 통신 하는 Xamarin. Forms 앱에 인증 및 권한 부여를 통합 하는 방법에는 여러 가지가 있습니다. 여기서는 IdentityServer 4를 사용 하는 컨테이너 화 된 identity 마이크로 서비스를 사용 하 여 인증 및 권한 부여를 수행 합니다. IdentityServer는 ASP.NET Core Id와 통합 되어 전달자 토큰 인증을 수행 하는 ASP.NET Core에 대 한 오픈 소스 Openid connect Connect 및 OAuth 2.0 프레임 워크입니다.

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[원격 데이터에 액세스](accessing-remote-data.md)

많은 최신 웹 기반 솔루션은 웹 서버에서 호스트 되는 웹 서비스를 사용 하 여 원격 클라이언트 응용 프로그램에 대 한 기능을 제공 합니다. 웹 서비스에서 노출 하는 작업은 web API를 구성 하 고, 클라이언트 앱은 API에서 노출 하는 데이터 또는 작업이 구현 되는 방식을 몰라도 웹 API를 활용할 수 있어야 합니다.

## <a name="unit-testingunit-testingmd"></a>[단위 테스트](unit-testing.md)

MVVM 응용 프로그램에서 모델을 테스트 하 고 모델을 확인 하는 것은 다른 클래스를 테스트 하는 것과 동일 하며, 동일한 도구와 기법을 사용할 수 있습니다. 그러나 모델 클래스를 모델링 하 고 보는 데 일반적으로 사용할 수 있는 몇 가지 패턴이 있습니다. 이러한 패턴은 특정 유닛 테스트 기법을 활용 합니다.

## <a name="feedback"></a>사용자 의견

이 프로젝트에는 질문을 게시 하 고 피드백을 제공할 수 있는 커뮤니티 사이트가 있습니다. 커뮤니티 사이트는 [GitHub](https://github.com/dotnet-architecture/eShopOnContainers)에 있습니다. 또는 전자책에 대 한 피드백을에 [dotnet-architecture-ebooks-feedback@service.microsoft.com](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com)전자 메일로 보낼 수 있습니다.

## <a name="related-links"></a>관련 링크

- [다운로드 전자책 (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
