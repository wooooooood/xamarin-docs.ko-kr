---
title: Xamarin.Forms eBook를 사용 하 여 엔터프라이즈 응용 프로그램 패턴
description: 이 eBook 적응형, 관리 및 테스트 가능한 Xamarin.Forms 엔터프라이즈 응용 프로그램을 개발 하기 위한 아키텍처 지침을 제공 합니다.
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: c401465d8a57abe1d5a1cfaf9ee2616888332ea3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242165"
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Xamarin.Forms eBook를 사용 하 여 엔터프라이즈 응용 프로그램 패턴

_융통성 있는, 관리 및 테스트 가능한 Xamarin.Forms 엔터프라이즈 응용 프로그램을 개발 하기 위한 아키텍처 지침_

![](images/cover-sml.png "Xamarin.Forms eBook를 사용 하 여 엔터프라이즈 응용 프로그램 패턴")

이 eBook 느슨한 결합을 유지 하면서 모델-뷰-MVVM () 패턴, 종속성 주입, 탐색, 유효성 검사 및 구성 관리를 구현 하는 방법에 지침을 제공 합니다. 또한 이기도 지침을 인증 및 권한 부여 IdentityServer, 색인화 microservices 및 단위 테스트에서 데이터에 액세스를 수행 합니다.

## <a name="prefaceprefacemd"></a>[서문](preface.md)

이 장에서 가이드 및 목표로 사용자의 범위와 목적을 설명 합니다.

## <a name="introductionintroductionmd"></a>[소개](introduction.md)

엔터프라이즈 앱의 개발자가 개발 하는 동안 응용 프로그램의 아키텍처를 변경할 수 있는 몇 가지 직면 합니다. 따라서 그는 수정 하거나 시간에 따라 확장 될 수 있도록 응용 프로그램을 구축 해야 합니다. 이러한 어댑터 기능에 대 한 디자인 것이 어려울 수 있지만 일반적으로 통합 될 수 있는 쉽게 함께 앱에는 불연속, 느슨하게 결합 된 구성 요소를 응용 프로그램을 분할 합니다.

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

모델-뷰-MVVM () 패턴 사용자 인터페이스 (UI)에서 응용 프로그램의 비즈니스 및 프레젠테이션 논리를 명확 하 게 구분 하는 데 도움이 됩니다. 응용 프로그램 논리 및 UI 명확한 구분을 유지 관리 여러 개발 문제를 해결 하는 데 도움이 및 응용 프로그램 테스트, 유지 관리 및 발전시키를 쉽게 만들 수 있습니다. 코드 다시 사용 하 여 기회를 크게 향상 시킬 수 있게 해 및 더 많은 작업을 디자이너 UI의 해당 부분 응용 프로그램의 개발 하는 경우 쉽게 공동 합니다.

## <a name="dependency-injectiondependency-injectionmd"></a>[종속성 주입](dependency-injection.md)

종속성 주입 이러한 형식에 의존 하는 코드에서 구체적인 형식을 분리할 수 있습니다. 일반적으로 구현 하거나 이러한 형식을 확장 하는 구체적인 형식 고 등록과 인터페이스 및 추상 형식 간의 매핑을의 목록을 보유 하는 컨테이너를 사용 합니다.

종속성 주입 컨테이너를 클래스 인스턴스를 인스턴스화할 때 컨테이너의 구성에 따라 해당 수명을 관리 하는 기능을 제공 하 여 개체 간의 결합을 줄입니다. 개체를 만드는 동안 컨테이너에는 개체에 필요한 모든 종속성을 삽입 합니다. 이러한 종속성 아직 만들지 않은, 컨테이너 만들고 먼저 해당 종속성을 해결 합니다.

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[느슨하게 결합된 구성 요소 간 통신](communicating-between-loosely-coupled-components.md)

Xamarin.Forms는 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) 클래스 구현 게시-구독 패턴, 개체 및 형식 참조 하 여 연결 편리한 되지 않는 구성 요소 간에 메시지 기반 통신을 허용 합니다. 이 메커니즘에는 게시자와 구독자에 대 한 참조, 구성 요소를 독립적으로 개발 하 고 테스트 가능 하도록 구성 요소 간의 종속성을 줄이는 데 도움이 필요 없이 통신할 수 있습니다.

## <a name="navigationnavigationmd"></a>[탐색](navigation.md)

Xamarin.Forms에는 일반적으로 UI와 사용자의 상호 작용 또는 내부 논리 기반 상태 변경으로 인해 앱 자체에서 간격은 페이지 탐색에 대 한 지원이 포함 되어 있습니다. 그러나 탐색 MVVM 패턴을 사용 하는 앱에서 구현 복잡할 수 있습니다.

이 장에서 `NavigationService` 모델 보기에서에서 보기 중심 모델 탐색을 수행 하는 데 사용 되는 클래스입니다. 자동화 된 테스트를 통해 논리를 수행 될 수 모델 클래스를 의미 보기에서 탐색 논리를 배치 합니다. 또한 뷰 모델 탐색 특정 비즈니스 규칙 적용 되도록을 제어 하는 논리를 구현 다음 수 있습니다.

## <a name="validationvalidationmd"></a>[유효성 검사](validation.md)

사용자 로부터 입력을 허용 하는 모든 앱의 입력이 올바른지 확인 해야 합니다. 유효성 검사 없이 사용자는 응용 프로그램에서 실패를 발생 시키는 데이터를 제공할 수 있습니다. 비즈니스 규칙을 적용 하 고는 공격자가 악성 데이터를 삽입 하지 않도록 설정 하는 유효성 검사 합니다.

ViewModel 모델 (MVVM)의 컨텍스트에서 패턴을 보기 모델 또는 모델 데이터 유효성 검사를 수행 하 고 보기에 유효성 검사 오류를 해결할 수 있도록 신호를 자주 필요 합니다.

## <a name="configuration-managementconfiguration-managementmd"></a>[구성 관리](configuration-management.md)

설정 동작을 응용 프로그램을 다시 작성 하지 않고 변경할 수 있도록 코드에서 응용 프로그램의 동작을 구성 하는 데이터 분리를 허용 합니다. 응용 프로그램 설정은 응용 프로그램을 만들고 관리 하 고, 데이터 및 사용자 설정을 사용자 지정할 수 있는 하는 설정은 응용 프로그램의 응용 프로그램의 동작에 영향을 주고 자주 다시 조정 필요 하지 않습니다.

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[컨테이너화된 마이크로 서비스](containerized-microservices.md)

Microservices 응용 프로그램 개발 및 배포 하는 최신 클라우드 응용 프로그램의 민첩성, 배율 및 안정성 요구 사항에 적합 한 방법을 제공 합니다. 있습니다 수 없게 된다는 확장 독립적으로 즉, 특정 기능 영역의 확장 수 있다는 불필요 하 게 영역을 크기 조정 없이 요청을 지원 하기 위해 더 많은 처리 전원 또는 네트워크 대역폭을 필요로 하는 microservices의 주요 이점 중 하나 늘어난된 수요에 발생 하는 응용 프로그램입니다.

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[인증 및 권한 부여](authentication-and-authorization.md)

ASP.NET MVC 웹 응용 프로그램와 통신 하는 Xamarin.Forms 앱에 인증 및 권한 부여를 통합 하는 방법은 여러 가지가 있습니다. 여기에서 인증 및 권한 부여 IdentityServer 4를 사용 하는 컨테이너 화 된 identity 마이크로 서비스도 수행 됩니다. IdentityServer는 전달자 토큰 인증을 수행 하도록 ASP.NET Core Id와 통합 되는 ASP.NET Core에 대 한 OAuth 2.0 및 OpenID Connect에서 프레임 워크는 오픈 소스입니다.

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[원격 데이터에 액세스](accessing-remote-data.md)

많은 최신 웹 기반 솔루션을 통해 응용 프로그램 원격 클라이언트에 대 한 기능을 제공 하도록 웹 서버에 의해 호스팅되는 웹 서비스를 사용 합니다. Web API를 구성 하는 웹 서비스를 노출 하는 작업 및 클라이언트 응용 프로그램 데이터 또는 API가 노출 하는 작업의 구현 방법을 몰라도 web API를 활용 하 여 수 있어야 합니다.

## <a name="unit-testingunit-testingmd"></a>[단위 테스트](unit-testing.md)

모델 및 MVVM 응용 프로그램에서 뷰 모델을 테스트 다른 클래스를 테스트 동일 하며 동일한 도구 및 기법을 사용할 수 있습니다. 그러나 몇 가지는 일반적으로 모델에 있는 패턴 및 특정 단위 테스트 기법을 활용할 수 있는 뷰 모델 클래스입니다.

## <a name="feedback"></a>사용자 의견

이 프로젝트에는 커뮤니티 사이트에 질문을 게시 및 피드백을 제공할 수 있습니다. 커뮤니티 사이트에 있는 [GitHub](https://github.com/dotnet-architecture/eShopOnContainers)합니다. 또는 eBook에 대 한 피드백 수 전자 메일로 보낼 [ dotnet-architecture-ebooks-feedback@service.microsoft.com ](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com)합니다.


## <a name="related-links"></a>관련 링크

- [EBook (2mb PDF)를 다운로드 합니다.](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
