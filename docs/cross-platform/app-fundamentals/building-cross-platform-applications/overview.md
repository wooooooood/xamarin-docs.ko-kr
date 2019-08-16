---
title: 플랫폼 간 응용 프로그램 빌드 개요
description: 이 문서에서는 플랫폼 간 응용 프로그램을 빌드하기 위한 개략적인 개요를 제공 합니다. 의 C#값, MVC/MVVM와 같은 디자인 패턴, 네이티브 ui를 설명 합니다.
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: c97cfda836f59d4cdbcd234744f723eb9429ace4
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526853"
---
# <a name="building-cross-platform-applications-overview"></a>플랫폼 간 응용 프로그램 빌드 개요

이 가이드에서는 Xamarin 플랫폼 및 플랫폼 간 응용 프로그램을 설계 하 여 코드 재사용을 최대화 하 고 모든 주요 모바일 플랫폼 (iOS, Android 및 Windows Phone)에서 고품질의 기본 환경을 제공 하는 방법을 소개 합니다.

이 문서에서 사용 되는 방법은 생산성 앱과 게임 앱 모두에 일반적으로 적용 되지만, 생산성 및 유틸리티 (비 게임 응용 프로그램)에 중점을 둡니다. 플랫폼 간 게임 개발 지침은 [MonoGame 문서 소개](~/graphics-games/monogame/introduction/index.md) 또는 체크 아웃 [Visual Studio Tools for Unity](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-tools-for-unity) 를 참조 하세요.

"한 번 쓰기, 모든 위치에서 실행"은 여러 플랫폼에서 수정 되지 않은 상태로 실행 되는 단일 코드 베이스의 virtues을 extol 하는 데 종종 사용 됩니다. 코드를 다시 사용 하는 이점을 얻을 수 있지만, 이러한 접근 방식은 일반적으로 가장 낮은-분모 기능 집합을 포함 하 고 있고 대상 플랫폼에 적합 하지 않은 일반 사용자 인터페이스가 있는 응용 프로그램을 만드는 경우가 많습니다.

강력한 기능 중 하나는 각 플랫폼에 대해 네이티브 사용자 인터페이스를 구현 하는 기능이 기 때문에 Xamarin은 "한 번만 작성 되 고, 모든" 플랫폼에서 실행 되는 것은 아닙니다. 그러나 확인이 가능한 디자인을 사용 하는 경우에도 대부분의 사용자 인터페이스 코드를 공유할 수 있으며, 데이터 저장소 및 비즈니스 논리 코드를 한 번 작성 하 고 각 플랫폼에 기본 Ui를 제공 하는 것이 좋습니다. 이 문서에서는이 목표를 달성 하는 일반적인 아키텍처 접근 방법을 설명 합니다.

Xamarin 플랫폼 간 앱을 만들기 위한 핵심 사항은 다음과 같습니다.

- 에서 C#앱을 **사용 C#**  합니다. 에서 C# 작성 된 기존 코드는 Xamarin을 매우 쉽게 사용 하 여 IOS 및 Android로 이식할 수 있으며 Windows 앱에서 사용할 수 있습니다.
- **MVC 또는 MVVM 디자인 패턴 활용** -모델/뷰/컨트롤러 패턴을 사용 하 여 응용 프로그램의 사용자 인터페이스를 개발 합니다. 모델/뷰/컨트롤러 접근 방식이 나 "모델"과 나머지를 명확 하 게 구분 하는 모델/뷰/ViewModel 방식을 사용 하 여 응용 프로그램을 설계 합니다. 응용 프로그램에서 각 플랫폼 (iOS, Android, Windows, Mac)의 네이티브 사용자 인터페이스 요소를 사용 하는 부분을 확인 하 고 응용 프로그램을 두 개의 구성 요소로 분할 하기 위한 지침으로 사용 합니다. "Core" 및 "사용자 인터페이스"입니다.
- **네이티브 Ui 빌드** -각 OS 특정 응용 프로그램은 다른 사용자 인터페이스 계층을 제공 합니다 (네이티브 C# UI 디자인 도구를 사용 하 여에서 구현 됨).

1. IOS에서 UIKit Api를 사용 하 여 네이티브 응용 프로그램을 만들고, 선택적으로 Xamarin의 iOS designer를 활용 하 여 UI를 시각적으로 만듭니다.
1. Android에서 Android. 뷰를 사용 하 여 네이티브 응용 프로그램을 만들고 Xamarin의 UI 디자이너를 활용 합니다.
1. Windows에서는 Visual Studio 또는 Blend의 UI 디자이너에서 만든 프레젠테이션 계층에 대 한 XAML을 사용 합니다.
1. Mac에서는 Xcode에서 만든 프레젠테이션 계층에 대해 Storyboard를 사용 합니다.

Xamarin Forms 프로젝트는 모든 플랫폼에서 지원 되며 Xamarin.ios XAML을 사용 하 여 플랫폼 간에 공유할 수 있는 사용자 인터페이스를 만들 수 있습니다. 

코드를 다시 사용 하는 양은 공유 코어에 유지 되는 코드의 양과 사용자 인터페이스에 따라 달라 지는 코드의 양에 따라 크게 달라 집니다. 핵심 코드는 사용자와 직접 상호 작용 하지 않고, 대신이 정보를 수집 하 고 표시 하는 응용 프로그램의 일부에 대 한 서비스를 제공 하는 것입니다.

코드를 다시 사용 하는 양을 늘리려면 다음과 같은 모든 시스템에서 공통 서비스를 제공 하는 플랫폼 간 구성 요소를 채택할 수 있습니다.

1. [SQLite-](https://www.nuget.org/packages/sqlite-net-pcl/) 로컬 SQL 저장소의 경우
1. 카메라, 연락처 및 지리적 위치를 포함 하 여 장치 특정 기능에 액세스 하기 위한 [Xamarin 플러그인](https://xamarin.com/plugins)
1. Xamarin 프로젝트와 호환 되는 [NuGet 패키지](https://nuget.org) (예: [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/))
1. 네트워킹, 웹 서비스, IO 등에 .NET framework 기능 사용.


이러한 구성 요소 중 일부는 *Tasky* 사례 연구에서 구현 됩니다.

 <a name="Separate_Reusable_Code_into_a_Core_Library" />


## <a name="separate-reusable-code-into-a-core-library"></a>재사용 가능한 코드를 핵심 라이브러리로 분리

응용 프로그램 아키텍처를 계층화 한 후 플랫폼의 핵심 기능을 재사용 가능한 핵심 라이브러리로 이동 하 여 책임 분리의 원칙에 따라 아래 그림 처럼 플랫폼 간에 코드 공유를 최대화할 수 있습니다. 예제

 ![](overview-images/layers2.png "응용 프로그램 아키텍처를 계층화 한 후 플랫폼의 핵심 기능을 재사용 가능한 핵심 라이브러리로 이동 하 여 책임 분리 원칙에 따라 플랫폼 간 코드 공유를 최대화할 수 있습니다.")

 <a name="Case_Studies" />


## <a name="case-studies"></a>사례 연구

이 문서는 *Tasky Pro*와 함께 제공 되는 한 가지 사례 연구입니다. 각 사례 연구에서는 실제 예제에서이 문서에 설명 된 개념의 구현에 대해 설명 합니다. 코드는 오픈 소스 이며 [github](https://github.com/xamarin/mobile-samples/)에서 사용할 수 있습니다.
