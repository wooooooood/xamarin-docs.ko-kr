---
title: 엔터프라이즈 앱 개발에 대 한 앞으로
description: 이 장에서는 Xamarin.ios를 사용 하는 엔터프라이즈 응용 프로그램 패턴의 앞에를 제공 합니다.
ms.prod: xamarin
ms.assetid: fbf32a44-1d33-4e16-a904-dc7ee5991e7c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 4ce04ec5216872cb56424e8847eec357a5b3ac0e
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770749"
---
# <a name="preface-to-enterprise-app-development"></a>엔터프라이즈 앱 개발에 대 한 앞으로

이 전자책는 Xamarin.ios를 사용 하 여 플랫폼 간 엔터프라이즈 앱을 빌드하는 방법에 대 한 지침을 제공 합니다. Xamarin.ios는 개발자가 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)를 비롯 한 여러 플랫폼에서 공유할 수 있는 네이티브 사용자 인터페이스 레이아웃을 쉽게 만들 수 있도록 하는 플랫폼 간 UI 도구 키트입니다. 이 솔루션은 B2E (business to Employee), B2B (business to Business) 및 B2C (Business to Consumer) 앱에 대 한 종합적인 솔루션을 제공 하 여 모든 대상 플랫폼에서 코드를 공유 하 고 TCO (총 소유 비용)를 낮추는 기능을 제공 합니다.

이 가이드는 조정 가능 하 고, 유지 관리 가능 하며, 테스트 가능한 Xamarin.ios enterprise 앱을 개발 하기 위한 아키텍처 지침을 제공 합니다. MVVM, 종속성 주입, 탐색, 유효성 검사 및 구성 관리를 구현 하는 방법에 대 한 지침을 제공 하 고 느슨한 결합을 유지 합니다. 또한 IdentityServer를 사용 하 여 인증 및 권한 부여를 수행 하 고, 컨테이너 화 된 마이크로 서비스에서 데이터에 액세스 하 고, 단위 테스트를 수행 하는 방법도 제공 됩니다.

이 가이드는 [eShopOnContainers 모바일 앱](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Mobile)에 대 한 소스 코드와 [eShopOnContainers reference 앱](https://github.com/dotnet-architecture/eShopOnContainers)에 대 한 소스 코드와 함께 제공 됩니다. EShopOnContainers 모바일 앱은 eShopOnContainers 참조 앱 이라고 하는 일련의 컨테이너 화 된 마이크로 서비스에 연결 하는 Xamarin.ios를 사용 하 여 개발한 플랫폼 간 엔터프라이즈 앱입니다. 그러나 컨테이너 화 된 마이크로 서비스 배포를 방지 하려는 사용자를 위해 모의 서비스의 데이터를 사용 하도록 eShopOnContainers 모바일 앱을 구성할 수 있습니다.

## <a name="whats-left-out-of-this-guides-scope"></a>이 가이드의 범위에서 벗어난 내용

이 가이드는 Xamarin.ios를 이미 알고 있는 독자를 대상으로 합니다. Xamarin.ios에 대 한 자세한 소개는 xamarin.ios [설명서](~/xamarin-forms/index.yml)및 [xamarin.ios를 사용 하 여 Mobile Apps 만들기](https://aka.ms/xamebook)를 참조 하세요.

이 가이드는 .net 마이크로 [서비스를 보완 합니다. 컨테이너 화 된 마이크로 서비스 개발 및](https://aka.ms/microservicesebook)배포에 중점을 둔 컨테이너 화 된 .net 응용 프로그램에 대 한 아키텍처입니다. 다른 가이드에는 [ASP.NET Core 및 Microsoft Azure를 사용 하 여 최신 웹 응용 프로그램 설계 및 개발](https://aka.ms/WebAppEbook), [Microsoft 플랫폼 및 도구를 사용 하 여 Docker 응용 프로그램 수명 주기 컨테이너 화 된](https://aka.ms/dockerlifecycleebook), [microsoft 플랫폼 및 모바일 앱 개발을 위한 도구](https://aka.ms/MobAppDev/StndPDF)입니다.

## <a name="who-should-use-this-guide"></a>이 가이드를 사용 해야 하는 사람

이 가이드의 대상은 Xamarin.ios를 사용 하 여 플랫폼 간 엔터프라이즈 앱을 설계 하 고 구현 하는 방법을 배우는 데 주로 사용 되는 개발자 및 설계자입니다.

보조 대상은 Xamarin.ios를 사용 하 여 플랫폼 간 엔터프라이즈 앱 개발을 위해 선택할 방법을 결정 하기 전에 아키텍처 및 기술 개요를 받으려는 기술 의사 결정자입니다.

## <a name="how-to-use-this-guide"></a>이 가이드를 사용 하는 방법

이 가이드에서는 Xamarin.ios를 사용 하 여 플랫폼 간 엔터프라이즈 앱을 빌드하는 방법을 집중적으로 설명 합니다. 따라서 이러한 앱 및 기술 고려 사항을 이해 하기 위한 토대를 제공 하기 위해 전체를 읽어야 합니다. 이 가이드는 샘플 앱과 함께 새로운 엔터프라이즈 앱을 만들기 위한 시작 지점 또는 참조 역할을 할 수도 있습니다. 연결 된 샘플 앱을 새 앱의 템플릿으로 사용 하거나 앱 구성 요소 파트를 구성 하는 방법을 확인 합니다. 그런 다음 아키텍처에 대 한 지침을 보려면이 가이드를 다시 참조 하세요.

Xamarin을 사용 하 여 플랫폼 간 엔터프라이즈 앱 개발에 대 한 일반적인 이해를 돕기 위해이 가이드를 팀 멤버에 게 자유롭게 전달 하세요. 용어의 공통 집합에서 모든 작업을 수행 하는 경우에는 아키텍처 패턴 및 사례를 일관 되 게 적용 하는 데 도움이 됩니다.

## <a name="related-links"></a>관련 링크

- [다운로드 전자책 (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
