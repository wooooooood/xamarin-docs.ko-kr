---
title: "크로스 플랫폼 응용 프로그램 개요 문서"
ms.topic: article
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 8e7b25f615939c0ae902e09c728615188d1cb86e
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2018
---
# <a name="building-cross-platform-applications-overview"></a>크로스 플랫폼 응용 프로그램 개요 문서

이 가이드에서는 Xamarin 플랫폼 및 코드 다시 사용을 최대화 하 고 모든 주요 모바일 플랫폼에 높은 품질 네이티브 경험을 제공 하는 플랫폼 간 응용 프로그램을 설계 하는 방법을 소개: iOS, Android 및 Windows Phone 합니다.

이 문서에서는 이러한 방식을 사용 생산성과 유틸리티 (비 게임 응용 프로그램)에 포커스가 있는 이지만 생산성 앱 및 게임 앱 모두에 일반적으로 적용 합니다. 참조는 [MonoGame 문서 소개](https://developer.xamarin.com/guides/cross-platform/game_development/monogame/introduction/) 하거나 체크 아웃 [Visual Studio Tools for Unity](https://docs.microsoft.com/en-us/visualstudio/cross-platform/visual-studio-tools-for-unity) 플랫폼 간 게임 개발 지침에 대 한 합니다.

구 "쓰기-어디에서 나 한 번 실행할" 하는 대개 단일 얻을 수 있는 장점 실행 여러 플랫폼에서 수정 되지 않은 코드 베이스 합니다. 코드 다시 사용의 이점은 있으면, 해당 접근 하면 종종 최저 공통 분모 기능 집합에 있는 응용 프로그램와 맞지 않는 제네릭 수준의 사용자 인터페이스 원활 하 게는 대상 플랫폼 중 하나로 합니다.

Xamarin은 하지 방금는 "쓰기-어디에서 나 한 번 실행할" 플랫폼의 장점 중 하나는 각 플랫폼에 대해 특별히 네이티브 사용자 인터페이스를 구현 하는 기능 때문에 합니다. 그러나 신중 하 게 디자인을 공유할 수는 대부분의 비 사용자 코드를 인터페이스 및의 장점 극대화: 데이터 저장소 및 비즈니스 논리 코드를 한 번 작성 하 고 각 플랫폼에 네이티브 Ui를 제공 합니다. 이 문서에서는 이러한 목표를 달성 하는 일반 아키텍처 접근 방법을 설명 합니다.

Xamarin 플랫폼 간 앱을 만들기 위한 요점이 요약 다음과 같습니다.

-   **C#을 사용 하 여** -C#에서 응용 프로그램을 작성 합니다. C#에서 작성 된 기존 코드 수로 iOS 및 Android를 매우 쉽게 Xamarin을 사용 하 여 포트 있고 Windows 앱에서 분명히 사용 합니다.
-   **MVC 또는 MVVVM 디자인 패턴을 활용** -모델/뷰/컨트롤러 패턴을 사용 하 여 응용 프로그램의 사용자 인터페이스를 개발 합니다. 모델/뷰/컨트롤러 방법이 나 모델/뷰/ViewModel 접근 방식을 사용 하 여 응용 프로그램을 설계는 "모델"와 나머지 명확히 구분 합니다. 응용 프로그램의 어느 부분 및 각 플랫폼 (iOS, Android, Windows, Mac)의 기본 사용자 인터페이스 요소를 사용 하 여 사용 하 여이 지침으로 분할 하면 응용 프로그램을 두 가지 구성 요소를 확인: "Core" 및 "사용자 인터페이스"입니다.
-   **네이티브 Ui 빌드** -각 운영 체제 관련 응용 프로그램은 다양 한 사용자 인터페이스 계층 (에서 구현 된 C# 사용 하 여 네이티브 UI 디자인 도구)를 제공 합니다.

1.  Ios에서 필요에 따라 Xamarin iOS 디자이너 UI를 시각적으로 만들를 활용 하는 기본 수준의 응용 프로그램을 만드는 UIKit Api를 사용 합니다.
1.  Android에서는 Xamarin의 UI 디자이너 활용 하기 위해는 네이티브 모양 응용 프로그램을 만들려면 Android.Views를 사용 합니다.
1.  Windows에서 사용 하려는 XAML 프레젠테이션 계층의 경우 Visual Studio 또는 Blend의 UI 디자이너에서 만든 합니다.
1.  Mac에서 Xcode에서 만든 프레젠테이션 계층에 대 한 스토리 보드를 사용 합니다.

Xamarin.Forms 프로젝트는 모든 플랫폼에서 지원 됩니다 및 Xamarin.Forms XAML을 사용 하 여 플랫폼 간에 공유할 수 있는 사용자 인터페이스를 만들 수 있습니다. 

코드 다시 사용 하 여 양의 코드의 양에 특정 공유 코어 및 코드의 양에 사용자 인터페이스에 유지 되 크게 달라 집니다. 직접 사용자 상호 작용 하지 않고 대신 수집 되 고이 정보를 표시 하는 응용 프로그램의 일부에 대 한 서비스를 제공 하는 핵심 코드가 됩니다.

코드 다시 사용 하 여 크기를 늘리려면 이러한 모든 시스템에서와 같은 일반적인 서비스를 제공 하는 플랫폼 구성 요소를 적용할 수 있습니다.

1.   [SQLite net](https://www.nuget.org/packages/sqlite-net-pcl/) SQL의 로컬 저장소
1.   [Xamarin 플러그 인](https://xamarin.com/plugins) 카메라, 연락처 및 지리적 위치를 포함 하는 장치 관련 기능에 액세스 하기 위한
1.   [NuGet 패키지](https://nuget.org) 와 호환 되는 Xamarin 프로젝트와 같은 [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/),
1.  .NET framework 기능을 사용 하 여 네트워킹, 웹 서비스, IO 등에입니다.


구현 되는 이러한 구성 요소 중 일부는 *Tasky* 사례 연구.

 <a name="Separate_Reusable_Code_into_a_Core_Library" />


## <a name="separate-reusable-code-into-a-core-library"></a>핵심 라이브러리에 다시 사용할 수 있는 코드를 분리 합니다.

응용 프로그램 아키텍처를 레이어로 표시 한 다음 다시 사용할 수 있는 핵심 라이브러리에 종속 되지 않으므로 플랫폼 핵심 기능을 이동 하 여 책임 분리의 원칙을 따르면 아래 그림에 나와 있듯이 플랫폼 간에 공유 되는 코드를 최대화할 수 있습니다. 보여 줍니다.

 ![](part-0-overview-images/layers2.png "책임 분리의 원칙을 응용 프로그램 아키텍처를 레이어로 표시 한 다음 다시 사용할 수 있는 핵심 라이브러리에 종속 되지 않으므로 플랫폼 핵심 기능을 이동 하 여 수행 하 여 플랫폼 간에 공유 되는 코드를 최대화할 수 있습니다.")

 <a name="Case_Studies" />


## <a name="case-studies"></a>사례 연구

이 문서 – 함께 제공 되는 하나의 사례 연구는 *Tasky Pro*합니다. 각 사례 연구에서는 실제 예의이 문서에 설명 된 개념의 구현을 설명 합니다. 코드는 오픈 소스에서 사용할 수 있는 및 [github](https://github.com/xamarin/mobile-samples/)합니다.