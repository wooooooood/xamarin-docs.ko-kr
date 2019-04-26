---
title: 엔터프라이즈 앱 개발에는 접두사
description: 이 장에서 Xamarin.Forms를 사용 하 여 엔터프라이즈 응용 프로그램 패턴에 접두사를 제공 합니다.
ms.prod: xamarin
ms.assetid: fbf32a44-1d33-4e16-a904-dc7ee5991e7c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fd085d2fb12e82233f6d3be2e2773a84539f837b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61298886"
---
# <a name="preface-to-enterprise-app-development"></a>엔터프라이즈 앱 개발에는 접두사

이 전자책 Xamarin.Forms를 사용 하 여 플랫폼 간 엔터프라이즈 앱 구축에 지침을 제공 합니다. Xamarin.Forms는 개발자가 쉽게 네이티브 사용자 인터페이스 레이아웃을 만들 iOS, Android 및 Windows 플랫폼 (UWP (유니버설)을 포함 하는 플랫폼 간에 공유할 수 있는 플랫폼 간 UI 도구 키트입니다. B2b 직원 (B2E) 기업 간, 및 모든 대상 플랫폼 간에 코드를 공유 하는 기능을 제공 하 고 (tco)의 총 비용을 절감 하는 데 B2c 앱에 비즈니스에 대 한 포괄적인 솔루션을 제공 합니다.

이 가이드는 융통성 있는, 유지 관리 및 테스트 가능 Xamarin.Forms 엔터프라이즈 앱을 개발 하기 위한 아키텍처 지침을 제공 합니다. 느슨한 결합을 유지 하면서 MVVM, 종속성 주입, 탐색, 유효성 검사 및 구성 관리를 구현 하는 방법에 지침은 제공 됩니다. 또한 이기도 지침 인증 및 권한 부여 IdentityServer, 컨테이너 화 된 마이크로 서비스 및 단위 테스트에서 데이터에 액세스를 사용 하 여 수행 합니다.

가이드에 대 한 소스 코드와 함께 제공 합니다 [eShopOnContainers 모바일 앱](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Mobile), 및 소스 코드에 대 한는 [eShopOnContainers 참조 응용 프로그램](https://github.com/dotnet-architecture/eShopOnContainers)합니다. EShopOnContainers 모바일 앱은 일련의 알려진 eShopOnContainers 참조 응용 프로그램 컨테이너 화 된 마이크로 서비스에 연결 하는 Xamarin.Forms를 사용 하 여 개발 하는 플랫폼 간 엔터프라이즈 앱. 그러나 eShopOnContainers 모바일 앱 데이터를 컨테이너 화 된 마이크로 서비스 배포를 방지 하려는 사용자에 대 한 모의 서비스에서 사용 하도록 구성할 수 있습니다.

## <a name="whats-left-out-of-this-guides-scope"></a>이 가이드의이 범위를 벗어납니다 남은

이 가이드의 목적은 Xamarin.Forms를 사용 하 여 이미 익숙한 독자를 하는 것입니다. Xamarin.Forms에 대 한 자세한 소개를 참조 하세요. 합니다 [Xamarin.Forms 설명서](~/xamarin-forms/index.yml), 및 [Creating Mobile Apps with Xamarin.Forms](https://aka.ms/xamebook)합니다.

이 가이드는 보완 [.NET 마이크로 서비스: 컨테이너 화 된.NET 응용 프로그램에 대 한 아키텍처](https://aka.ms/microservicesebook), 개발 및 컨테이너 화 된 마이크로 서비스 배포에 대해 중점적으로 설명 합니다. 다른 가이드를 읽어 볼 가치가 [Architecting 및 최신 웹 응용 프로그램 개발 ASP.NET Core 및 Microsoft Azure를 사용 하 여](http://aka.ms/WebAppEbook), [컨테이너 화 된 Docker 응용 프로그램 수명 주기를 Microsoft 플랫폼 및 도구](http://aka.ms/dockerlifecycleebook), 및 [모바일 앱 개발을 위한 Microsoft 플랫폼 및 도구](http://aka.ms/MobAppDev/StndPDF)합니다.

## <a name="who-should-use-this-guide"></a>누가이 가이드를 사용 해야 합니까

개발자 및 설계자가 설계 하 고 Xamarin.Forms를 사용 하 여 플랫폼 간 엔터프라이즈 앱을 구현 하는 방법에 알아보려면이 가이드에 대 한 대상은 주로입니다.

보조 대상 기술 의사 결정자 Xamarin.Forms를 사용 하 여 플랫폼 간 엔터프라이즈 앱 개발을 위한 선택 방법을에서 결정 하기 전에 아키텍처 및 기술 개요를 받고자 하는 경우

## <a name="how-to-use-this-guide"></a>이 가이드를 사용 하는 방법

이 가이드는 Xamarin.Forms를 사용 하 여 플랫폼 간 엔터프라이즈 앱 구축에 중점을 둡니다. 따라서 이러한 앱 및 해당 기술 고려 사항을 이해의 기초를 제공 하려면 전체에서 읽을 수 해야 합니다. 해당 샘플 앱과 함께 가이드를 시작 지점 또는 새로운 엔터프라이즈 앱을 만드는 방법에 대 한 참조를 역할도 할 수 있습니다. 새 앱 또는 앱의 구성 요소 부분을 구성 하는 방법을 보려면 템플릿으로 관련된 샘플 앱을 사용 합니다. 그런 다음 아키텍처 지침은이 가이드를 다시 참조 합니다.

Xamarin.Forms를 사용 하 여 플랫폼 간 엔터프라이즈 앱 개발에 대 한 일반적인 이해를 보장 하기 위해 팀 멤버에이 가이드를 전달 해도 됩니다. 일반적인 용어 집합에서 작동 하 고 기본 원칙 모두가 있는 아키텍처 패턴과 방법의 일관적인 적용을 보장 하는 데 도움이 됩니다.


## <a name="related-links"></a>관련 링크

- [2mb PDF 전자책 다운로드](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
