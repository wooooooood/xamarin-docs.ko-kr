---
title: 크로스 플랫폼 응용 프로그램 개요 구성
description: 이 문서에서는 플랫폼 간 응용 프로그램을 구축 하는 높은 수준의 개요를 제공 합니다. 값에 설명 C#, MVC/MVVM 및 네이티브 Ui와 같은 패턴을 디자인 합니다.
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 1eb308e0095c29d8ab0d0bdf1f74b807fd2ab97f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61275616"
---
# <a name="building-cross-platform-applications-overview"></a>크로스 플랫폼 응용 프로그램 개요 구성

이 가이드에서는 Xamarin 플랫폼 및 코드 다시 사용을 최대화 하 고 모든 주요 모바일 플랫폼에서 고품질 네이티브 환경을 제공 하는 플랫폼 간 응용 프로그램을 설계 하는 방법을 소개 합니다: iOS, Android 및 Windows Phone입니다.

이 문서에서는 이러한 방식을 사용는 생산성 및 유틸리티 (게임 내 응용 프로그램)에 포커스가 있지만 생산성 앱과 게임 앱을 일반적으로 적용 됩니다. 참조를 [MonoGame 문서 소개](~/graphics-games/monogame/introduction/index.md) 확인해 [Visual Studio Tools for Unity](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-tools-for-unity) 플랫폼 간 게임 개발 지침에 대 한 합니다.

구 "작성-어디에서 나 한 번 실행할" 유명한 하는 경우가 많습니다 실행 여러 플랫폼에서 수정 되지 않은 코드 베이스를 단일의 이점을 설명 합니다. 코드 다시 사용의 장점은 하는 동안 해당 접근 하면 종종 맞지 않는 제네릭 모양의 사용자 인터페이스 및 응용 프로그램을 가장 낮은 공통 분모 기능 집합을 원활 하 게 대상 플랫폼 중 하나로.

Xamarin이 아니 네요을 "쓰기-어디에서 나 한 번 실행할" 플랫폼, 각 플랫폼에 맞게 네이티브 사용자 인터페이스를 구현 하는 기능 이므로 해당 장점 중 하나입니다. 그러나 아직 공유할 수 있기 신중 하 게 설계를 사용 하 여 대부분의 비 사용자 코드를 인터페이스 및의 장점 최대한: 데이터 저장소 및 비즈니스 논리 코드를 한 번 작성 하 고 각 플랫폼에서 네이티브 Ui를 제공 합니다. 이 문서에서는이 목표를 달성 하는 일반적인 아키텍처 방법을 설명 합니다.

Xamarin 플랫폼 간 앱을 만들기 위한 요점이 요약은 다음과 같습니다.

-   **사용 하 여 C#**  -에서 앱을 작성 C#합니다. 기존 코드를 작성 C# iOS 및 Xamarin을 매우 쉽게 사용 하 여 Android로 이식 하 고 분명 Windows 앱에서 사용할 수 있습니다.
-   **MVC 또는 MVVM 디자인 패턴을 활용** -모델/보기/컨트롤러 패턴을 사용 하 여 응용 프로그램의 사용자 인터페이스를 개발 합니다. 컨트롤러 보기/모델/방법 또는 모델/보기/ViewModel 접근을 사용 하 여 응용 프로그램을 설계 있는 "모델"과 나머지를 명확 하 게 구분 합니다. 응용 프로그램의 어느 부분이 되며 각 플랫폼 (iOS, Android, Windows, Mac)의 네이티브 사용자 인터페이스 요소를 사용 하 여 사용 하 여이 지침으로 분할 응용 프로그램을 두 가지 구성 요소를 결정 합니다. "Core" 및 "User Interface"입니다.
-   **네이티브 Ui를 빌드하** -다른 사용자 인터페이스 계층을 제공 하는 각 OS 별 응용 프로그램 (에서 구현 된 C# 디자인 도구를 사용 하 여 네이티브 ui):

1.  Ios의 경우 네이티브 모양의 응용 프로그램을 시각적으로 UI를 만드는 Xamarin iOS 디자이너를 사용 하 여 필요에 따라 만들 UIKit Api를 사용 합니다.
1.  Android에서는 Xamarin의 UI 디자이너 활용 하 고는 네이티브 모양의 응용 프로그램을 만드는 Android.Views를 사용 합니다.
1.  Windows에서 사용할 XAML 프레젠테이션 계층의 경우 Visual Studio 또는 Blend의 UI 디자이너에서 생성 합니다.
1.  Mac에서 Xcode에서 만든 프레젠테이션 계층에 대 한 스토리 보드를 사용 합니다.

Xamarin.Forms 프로젝트는 모든 플랫폼에서 지원 됩니다 및 Xamarin.Forms XAML을 사용 하 여 플랫폼 간에 공유할 수 있는 사용자 인터페이스를 만들 수 있습니다. 

코드 다시 사용 하 여 크기는 주로 코드의 양을 특정 공유 핵심 이며 코드의 양을 사용자 인터페이스에 보관 되어에 따라 달라 집니다. 핵심 코드 부분이 사용자와 직접 상호 작용 하지 않습니다 하지만 대신 수집 되며이 정보를 표시 하는 응용 프로그램 부분에 대 한 서비스를 제공 합니다.

코드 다시 사용 하 여 크기를 늘리려면와 같은 이러한 모든 시스템에서 일반적인 서비스를 제공 하는 플랫폼 구성 요소를 채택할 수 있습니다.

1.   [SQLite-net](https://www.nuget.org/packages/sqlite-net-pcl/) 로컬 SQL 저장소
1.   [Xamarin 플러그 인](https://xamarin.com/plugins) 카메라, 연락처 및 지리적 위치를 포함 하는 장치 특정 기능에 액세스 하기 위한
1.   [NuGet 패키지](https://nuget.org) 와 호환 되는 Xamarin 프로젝트와 같은 [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/),
1.  네트워킹, 웹 서비스, IO 등.NET framework 기능을 사용합니다.


이러한 구성 요소 중 일부에서 구현 되는 *Tasky* 사례 연구 합니다.

 <a name="Separate_Reusable_Code_into_a_Core_Library" />


## <a name="separate-reusable-code-into-a-core-library"></a>핵심 라이브러리를 재사용 가능한 코드를 분리

책임 분리의 원칙을 응용 프로그램 아키텍처를 계층화 하 고 다음는 플랫폼에 독립적으로 다시 사용할 수 있는 핵심 라이브러리는 핵심 기능을 이동 하 여 수행 하 여 플랫폼 간에 공유 아래 그림으로 코드를 최대화할 수 있습니다. 보여 줍니다.

 ![](overview-images/layers2.png "책임 분리의 원칙을 응용 프로그램 아키텍처를 계층화 하 고 다음는 플랫폼에 독립적으로 다시 사용할 수 있는 핵심 라이브러리는 핵심 기능을 이동 하 여 수행 하 여 플랫폼 간에 공유 되는 코드를 최대화할 수 있습니다.")

 <a name="Case_Studies" />


## <a name="case-studies"></a>사례 연구

이 문서의 – 함께 제공 되는 하나의 사례 연구 *Tasky Pro*합니다. 각 사례 연구 구현의 실제 예제에서이 문서에 설명 된 개념을 설명 합니다. 코드는 오픈 소스에서 사용할 수 있습니다 [github](https://github.com/xamarin/mobile-samples/)합니다.
