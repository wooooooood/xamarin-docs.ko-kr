---
title: 엔터프라이즈 응용 프로그램 개발에 대 한 접두사
description: 이 장에서 Xamarin.Forms를 사용 하 여 엔터프라이즈 응용 프로그램 패턴을 접두사를 제공 합니다.
ms.prod: xamarin
ms.assetid: fbf32a44-1d33-4e16-a904-dc7ee5991e7c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fd085d2fb12e82233f6d3be2e2773a84539f837b
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242370"
---
# <a name="preface-to-enterprise-app-development"></a>엔터프라이즈 응용 프로그램 개발에 대 한 접두사

이 eBook Xamarin.Forms를 사용 하 여 플랫폼 간 엔터프라이즈 앱 개발에 지침을 제공 합니다. Xamarin.Forms는 개발자가 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)를 포함 하 여 플랫폼 간에 공유할 수 있는 인터페이스 레이아웃 기본 사용자를 쉽게 만들 수 있는 플랫폼 간 UI 도구 키트입니다. 기업 간 (B2E) 직원에 게 비즈니스 (B2B) 및 모든 대상 플랫폼에서 코드를 공유 하는 기능을 제공 하 고 (tco)의 총 비용을 낮추는 데 (B2C) 소비자 앱과 비즈니스에 대 한 종합적인 솔루션을 제공 합니다.

이 가이드는 적응형, 관리 및 테스트 가능한 Xamarin.Forms 엔터프라이즈 응용 프로그램을 개발 하기 위한 아키텍처 지침을 제공 합니다. 느슨한 결합을 유지 하면서 MVVM, 종속성 주입, 탐색, 유효성 검사 및 구성 관리를 구현 하는 방법에 지침은 제공 됩니다. 또한 이기도 지침을 인증 및 권한 부여 IdentityServer, 색인화 microservices 및 단위 테스트에서 데이터에 액세스를 수행 합니다.

이 가이드에 대 한 소스 코드와 함께 제공는 [eShopOnContainers 모바일 앱](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Mobile), 및 소스 코드에 대 한는 [eShopOnContainers 응용 프로그램 참조](https://github.com/dotnet-architecture/eShopOnContainers)합니다. EShopOnContainers 모바일 앱에 일련의 컨테이너 화 된 microservices 알려진는 eShopOnContainers 참조 응용 프로그램을 연결 하는 Xamarin.Forms를 사용 하 여 개발 플랫폼 간 엔터프라이즈 앱이입니다. 그러나 eShopOnContainers 모바일 앱 컨테이너 화 된 microservices 배포를 방지 하려면 사용자에 게 모의 서비스에서 데이터를 사용 하도록 구성할 수 있습니다.

## <a name="whats-left-out-of-this-guides-scope"></a>이 가이드의이 범위 밖에 남아

이 가이드는 독자 Xamarin.Forms를 사용 하 던 들을 목표로 합니다. Xamarin.Forms에 대 한 자세한 소개를 참조 하십시오.는 [Xamarin.Forms 설명서](~/xamarin-forms/index.yml), 및 [Xamarin.Forms 사용 하 여 모바일 앱 만들기](https://aka.ms/xamebook)합니다.

가이드를 보완 하는 [.NET Microservices:.NET 응용 프로그램의 컨테이너 화 된 아키텍처](https://aka.ms/microservicesebook), 개발 및 색인화 microservices 배포에 대해 중점적으로 설명 합니다. 다른 가이드를 읽어 볼 가치가 포함 [Architecting 및 최신 웹 응용 프로그램 개발 ASP.NET Core 및 Microsoft Azure로](http://aka.ms/WebAppEbook), [컨테이너 화 된 Docker 응용 프로그램 수명 주기 Microsoft 플랫폼 및 도구](http://aka.ms/dockerlifecycleebook), 및 [Microsoft 플랫폼 및 모바일 앱 개발을 위한 도구](http://aka.ms/MobAppDev/StndPDF)합니다.

## <a name="who-should-use-this-guide"></a>이 가이드를 사용 해야 하는

이 가이드에 대 한 대상은 개발자 및 설계자를 설계 하 고 Xamarin.Forms를 사용 하 여 플랫폼 간 엔터프라이즈 응용 프로그램을 구현 하는 방법을 알아보려면는 주로입니다.

보조 대상 그룹을 결정 하기 전에 방법을 선택할 Xamarin.Forms를 사용 하 여 플랫폼 간 엔터프라이즈 응용 프로그램 개발에 대 한 아키텍처 및 기술 개요를 수신 하겠다고 기술 의사 결정권자입니다.

## <a name="how-to-use-this-guide"></a>이 가이드를 사용 하는 방법

이 가이드는 Xamarin.Forms를 사용 하 여 플랫폼 간 엔터프라이즈 앱 개발에 중점을 둡니다. 따라서 이러한 앱 및 해당 기술 고려 사항 이해의 기초를 제공 하려면 전체에서 읽을 수 해야 합니다. 샘플 응용 프로그램을 함께 가이드 시작 지점 또는 새 엔터프라이즈 응용 프로그램 만들기에 대 한 참조로도 사용할 수 있습니다. 새 응용 프로그램에 대 한 또는 응용 프로그램의 구성 요소 부분을 구성 하는 방법을 보려면 템플릿으로 관련된 샘플 응용 프로그램을 사용 합니다. 그런 다음 아키텍처 지침은이 가이드를 다시 참조 합니다.

이 가이드를 파악 하는 Xamarin.Forms를 사용 하 여 플랫폼 간 엔터프라이즈 응용 프로그램 개발 팀 멤버에 전달 하도록 자유롭게 합니다. 기본 원칙 및 용어의 공통 집합에서 작업 하는 모든 사람이 필요 아키텍처 패턴 및 사례 지속적으로 적용 하는 데 도움이 됩니다.


## <a name="related-links"></a>관련 링크

- [EBook (2mb PDF)를 다운로드 합니다.](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
