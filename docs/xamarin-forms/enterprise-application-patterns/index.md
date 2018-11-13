---
title: Xamarin.Forms 전자책을 사용 하 여 엔터프라이즈 응용 프로그램 패턴
description: 이 전자책 융통성 있는, 유지 관리 및 테스트 가능 Xamarin.Forms 엔터프라이즈 응용 프로그램을 개발 하기 위한 아키텍처 지침을 제공 합니다.
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: f972a32f8daf920f2121e5aa56923c0f3a7f808a
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528444"
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Xamarin.Forms 전자책을 사용 하 여 엔터프라이즈 응용 프로그램 패턴

_융통성 있는, 유지 관리 및 테스트 가능 Xamarin.Forms 엔터프라이즈 응용 프로그램을 개발 하기 위한 아키텍처 지침_

![](images/cover-sml.png "Xamarin.Forms 전자책을 사용 하 여 엔터프라이즈 응용 프로그램 패턴")

이 전자책 하면서도 느슨한 결합 모델-뷰-ViewModel (MVVM) 패턴, 종속성 주입, 탐색, 유효성 검사 및 구성 관리를 구현 하는 방법에 지침을 제공 합니다. 또한 이기도 지침 인증 및 권한 부여 IdentityServer, 컨테이너 화 된 마이크로 서비스 및 단위 테스트에서 데이터에 액세스를 사용 하 여 수행 합니다.

## <a name="prefaceprefacemd"></a>[서문](preface.md)

이 장에서 가이드 및 목표로 사용자의 범위와 목적을 설명 합니다.

## <a name="introductionintroductionmd"></a>[소개](introduction.md)

엔터프라이즈 앱의 개발자가 개발 하는 동안 앱의 아키텍처를 변경할 수 있는 몇 가지 문제에 직면 합니다. 따라서 수정 하거나 시간에 따라 확장할 수 있도록 앱을 작성 하는 중요 한 것입니다. 이러한 어댑터 기능에 대 한 디자인 어려울 수 있지만 일반적으로 앱을 쉽게 통합할 수 있습니다 함께 앱에는 불연속, 느슨하게 결합 된 구성 요소로 분할 합니다.

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

완전히 사용자 인터페이스 (UI)에서 응용 프로그램의 비즈니스 및 프레젠테이션 논리와 분리-Model-view-viewmodel (MVVM) 패턴 수 있습니다. 응용 프로그램 논리 및 UI 명확한 구분을 유지 관리 다양 한 개발 문제를 해결 하는 데 도움이 및 응용 프로그램을 테스트 하 고 유지 관리 하며 진화를 쉽게 만들 수 있습니다. 코드 다시 사용 하 여 기회를 크게 향상 시킬 수 있습니다 하 고 개발자 및 UI 디자이너를 더 쉽게 공동 작업할 수는 앱의 각 해당 부분을 개발 하는 경우.

## <a name="dependency-injectiondependency-injectionmd"></a>[종속성 주입](dependency-injection.md)

종속성 주입 구체적인 형식을 이러한 형식에 의존 하는 코드에서 분리할 수 있습니다. 일반적으로 등록 및 인터페이스 및 추상 형식 간의 매핑 목록을 보유 하는 컨테이너 및 구현 하거나 이러한 형식을 확장 하는 구체적인 형식을 사용 합니다.

종속성 주입 컨테이너 클래스 인스턴스를 인스턴스화하고 컨테이너의 구성에 따라 해당 수명을 관리 하는 기능을 제공 하 여 개체 간의 결합을 줄입니다. 개체를 만드는 동안 컨테이너에 있는 개체에 필요한 모든 종속성을 삽입 합니다. 이러한 종속성이 아직 만들지 않은 경우, 컨테이너를 만들고 먼저 해당 종속성을 확인 합니다.

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[느슨하게 결합된 구성 요소 간 통신](communicating-between-loosely-coupled-components.md)

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) 클래스 구현 게시-구독 패턴을 편리 하 게 개체 및 형식을 참조 하 여 연결 되지 않는 구성 요소 간 메시지 기반 통신을 허용 합니다. 이 메커니즘에는 게시자 및 구독자도 구성 요소를 독립적으로 개발 하 고 테스트를 허용 하는 동안 구성 요소 간의 종속성을 줄이기 위해 서로에 대 한 참조가 필요 없이 전달할 수 있습니다.

## <a name="navigationnavigationmd"></a>[탐색](navigation.md)

Xamarin.Forms에는 일반적으로 UI 사용 하 여 사용자의 상호 작용 또는 내부 논리 기반 상태 변경으로 인해 앱 자체에서 발생 하는 페이지 탐색에 대 한 지원이 포함 됩니다. 그러나 탐색 MVVM 패턴을 사용 하는 앱에서 구현 복잡할 수 있습니다.

이 장에서 `NavigationService` 모델 보기에서에서 보기 모델 첫 탐색 하는 데 사용 되는 클래스입니다. 보기에서 탐색 논리를 배치 하면 모델 클래스를 통해 자동화 된 테스트 논리를 실행할 수 있습니다 의미 합니다. 또한 뷰 모델은 특정 비즈니스 규칙 적용 되도록 컨트롤 탐색 논리를 구현한 다음 수 있습니다.

## <a name="validationvalidationmd"></a>[유효성 검사](validation.md)

사용자 로부터 입력을 허용 하는 모든 앱에 유효한 입력 되었는지 확인 해야 합니다. 유효성 검사 없이 사용자는 응용 프로그램에서 실패를 발생 시키는 데이터를 제공할 수 있습니다. 비즈니스 규칙을 적용 하 고 공격자가 악성 데이터 주입 하지 않도록 설정 하는 유효성 검사 합니다.

모델-뷰-ViewModel (MVVM)의 컨텍스트에서 패턴, 보기 모델 또는 모델 종종 해야 데이터 유효성 검사를 수행 하 고 사용자 해결할 수 있도록 보기로 유효성 검사 오류를 알립니다.

## <a name="configuration-managementconfiguration-managementmd"></a>[구성 관리](configuration-management.md)

설정 동작을 앱을 다시 작성 하지 않고 변경할 수 있도록 코드에서 앱의 동작을 구성 하는 데이터 분리를 허용 합니다. 앱 설정은 앱을 만들고 관리 하는 데이터 및 사용자 설정 설정은 사용자 지정할 수 있는 앱의 앱의 동작에 영향을 자주 다시 조정 필요 하지 않습니다.

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[컨테이너화된 마이크로 서비스](containerized-microservices.md)

마이크로 서비스 응용 프로그램 개발 및 배포는 최신 클라우드 응용 프로그램의 민첩성, 확장성 및 안정성 요구 사항에 적합 한 방법을 제공 합니다. 이러한 수 있는 스케일 아웃 독립적으로 즉,는 특정 기능 영역의 크기를 조정할 수 더 많은 처리 전원 또는 네트워크 대역폭을 불필요 하 게 영역의 크기 조정 없이 요청을 지원 해야 하는 마이크로 서비스의 주요 장점 중 하나는 증가 되는 수요에 발생 하는 응용 프로그램입니다.

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[인증 및 권한 부여](authentication-and-authorization.md)

ASP.NET MVC 웹 응용 프로그램을 사용 하 여 통신 하는 Xamarin.Forms 앱에 인증 및 권한 부여를 통합 하는 방법은 여러 가지가 있습니다. 여기에서 인증 및 권한 부여 IdentityServer 4를 사용 하는 컨테이너 화 된 id 마이크로 서비스를 사용 하 여 수행 됩니다. IdentityServer는 전달자 토큰 인증을 수행 하도록 ASP.NET Core Id와 통합 되는 ASP.NET Core 용 오픈 소스 OpenID Connect 및 OAuth 2.0 프레임 워크입니다.

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[원격 데이터에 액세스](accessing-remote-data.md)

많은 최신 웹 기반 솔루션을 통해 응용 프로그램 원격 클라이언트에 대 한 기능을 제공 하기 위해 웹 서버에서 호스팅되는 웹 서비스를 사용 합니다. 웹 API를 구성 하는 웹 서비스를 노출 하는 작업 및 클라이언트 앱 데이터 또는 API를 노출 하는 작업이 구현 하는 방법을 알 필요 없이 web API를 사용할 수 있어야 합니다.

## <a name="unit-testingunit-testingmd"></a>[단위 테스트](unit-testing.md)

모델 및 MVVM 응용 프로그램에서 뷰 모델을 테스트 하는 것은 다른 클래스를 테스트와 동일 하 고 동일한 도구 및 기술을 사용할 수 있습니다. 그러나 몇 가지 일반적인 모델 패턴 및 특정 단위 테스트 기술을 활용할 수 있는 보기 모델 클래스입니다.

## <a name="feedback"></a>사용자 의견

이 프로젝트에는 커뮤니티 사이트는 질문을 게시 고 피드백을 제공할 수 있습니다. 커뮤니티 사이트에 위치한 [GitHub](https://github.com/dotnet-architecture/eShopOnContainers)합니다. 또는 전자책에 대 한 피드백 수에 이메일로 전송 된다는 [ dotnet-architecture-ebooks-feedback@service.microsoft.com ](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com)합니다.


## <a name="related-links"></a>관련 링크

- [2mb PDF 전자책 다운로드](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
