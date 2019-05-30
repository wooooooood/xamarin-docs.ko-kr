---
title: 3 부-Xamarin 플랫폼 간 솔루션 설정
description: 이 문서에서는 Xamarin의 플랫폼 간 솔루션을 설정 하는 방법을 설명 합니다. 공유 프로젝트 및.NET Standard와 같은 전략을 공유 하는 다양 한 코드에 설명 합니다.
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: d20275bab4e4ce90f902a5e72321701d94b1d416
ms.sourcegitcommit: 4a1520dee7759f8355ea65c8bb3d1bac8ba58122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66354066"
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>3 부-Xamarin 플랫폼 간 솔루션 설정

사용 중인 플랫폼에 관계 없이 모든 Xamarin 프로젝트 형식을 사용 하 여 동일한 솔루션 파일 (Visual Studio **.sln** 파일 형식). 개별 프로젝트 (예: Mac 용 Visual Studio에서 Windows 프로젝트를) 로드할 수 없는 경우에 개발 환경에서 솔루션을 공유할 수 있습니다.



새 플랫폼 간 응용 프로그램을 만들 때 첫 번째 단계는 빈 솔루션을 만드는 것입니다. 이 섹션에서는 다음을 설명 합니다: 플랫폼 간 모바일 앱을 빌드하기 위한 프로젝트를 설정 합니다.

 <a name="Sharing_Code" />


## <a name="sharing-code"></a>코드 공유

참조 된 [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md) 플랫폼 간에 코드 공유를 구현 하는 방법에 대 한 자세한 설명은 문서.

 <a name="Shared_Asset_Projects" />


### <a name="shared-projects"></a>공유 프로젝트

코드 파일을 공유 하는 가장 간단한 방법은 사용 하는 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)합니다.

이 메서드를 사용 하면 다양 한 플랫폼 프로젝트에서 동일한 코드를 공유 하 고 컴파일러 지시문을 사용 하 여 다양 한 플랫폼 특정 코드 경로 포함할 수 있습니다.

 <a name="Portable_Class_Libraries" />


### <a name="portable-class-libraries-pcl"></a>PCL(이식 가능한 클래스 라이브러리)

지금까지.NET 프로젝트 파일 (및 결과 어셈블리)가 대상으로 지정 된 특정 프레임 워크 버전으로 합니다. 이 프로젝트 또는 다른 프레임 워크에 의해 공유 되는 어셈블리를 방지 합니다.

이식 가능한 클래스 라이브러리 (PCL)는 Xamarin.iOS 및 Xamarin.Android 뿐만 아니라 WPF, 유니버설 Windows 플랫폼, Xbox 등 서로 다른 CLI 플랫폼에서 사용할 수 있는 프로젝트의 특별 한 형식입니다. 라이브러리만 대상 플랫폼에서 제한 된 전체.NET framework의 하위 집합을 활용할 수 있습니다.

읽어보세요 Xamarin에 대 한 [이식 가능한 클래스 라이브러리에 대 한 지원](~/cross-platform/app-fundamentals/pcl.md) 의 지침 참조에 따라 하는 방법을 [TaskyPortable 샘플](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable) 작동 합니다.


### <a name="net-standard"></a>.NET Standard

2016 년에 도입 된 [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) 프로젝트는 Windows에서 사용할 수 있는 어셈블리를 생성 하는 플랫폼, Xamarin 플랫폼 (iOS, Android, Mac) 및 Linux에서 코드를 공유 하는 쉬운 방법을 제공 합니다.

.NET standard 라이브러리 만들고 (1.6 1.0)에서 각 버전에서 사용할 수 있는 Api를 더 쉽게 검색 하 고 각 버전은 이전 버전과 호환성 한다는 Pcl, 처럼 사용할 수 낮은 버전 번호를 사용 하 여 합니다.



 <a name="Populating_the_Solution" />


## <a name="populating-the-solution"></a>솔루션을 채우기

코드를 공유 하는 방법이 사용에 관계 없이 전체 솔루션 구조는 코드 공유를 권장 하는 계층화 된 아키텍처를 구현 해야 합니다.
두 가지 프로젝트 형식에 대 한 그룹 코드 하는 Xamarin 방법이입니다.

-   **Core 프로젝트** – 여러 플랫폼 간에 공유할 수를 한 곳에서 다시 사용할 수 있는 코드를 작성 합니다. 가능한 구현 세부 정보를 숨기려면 캡슐화의 원칙을 사용 합니다.
-   **플랫폼 관련 응용 프로그램 프로젝트** – 거의 결합 최대한으로 사용 하 여 다시 사용할 수 있는 코드를 사용 합니다. Core 프로젝트에서 노출 하는 구성 요소 기반으로 하는이 수준에서 플랫폼별 기능이 추가 됩니다.


 <a name="Core_Project" />


### <a name="core-project"></a>Core 프로젝트

공유 코드 프로젝트는 사용 가능한 모든 플랫폼에서 ie 어셈블리만 참조 해야 합니다. 공통 프레임 워크 네임 스페이스와 같은 `System`하십시오 `System.Core` 및 `System.Xml`합니다.

공유 프로젝트는 가능한 다음 계층을 포함할 수 있습니다는 많은 비 UI 기능을 구현 해야 합니다.

-   **데이터 계층** – 예를 들어 실제 데이터 저장소의 담당 하는 코드입니다.  [SQLite-NET](https://github.com/praeclarum/sqlite-net)에서 대체 데이터베이스와 같은 [Realm.io](https://realm.io/products/realm-mobile-database/) 또는 XML 파일입니다. 데이터 계층 클래스는 일반적으로 데이터 액세스 계층 에서만 사용 됩니다.
-   **데이터 액세스 계층** – 데이터를 개별 데이터 항목의 목록에 액세스 하 고 만드는 메서드를 편집 및 삭제와 같은 응용 프로그램의 기능에 대해 필요한 데이터 작업을 지 원하는 API를 정의 합니다.
-   **서비스 액세스 계층** – 응용 프로그램에 클라우드를 제공 하는 선택적 계층 서비스입니다. 원격 네트워크 리소스 (웹 서비스, 이미지 다운로드 등)에 액세스 하는 코드를 포함 하며 가능한 결과 캐시 합니다.
-   **비즈니스 계층** – 모델 클래스와 기능을 플랫폼 관련 응용 프로그램을 노출 하는 외관 또는 관리자 클래스의 정의 합니다.


 <a name="Platform-Specific_Application_Projects" />


### <a name="platform-specific-application-projects"></a>플랫폼 관련 응용 프로그램 프로젝트

플랫폼별 프로젝트에는 핵심 공유 코드 프로젝트 및 각 플랫폼의 SDK (Xamarin.iOS, Xamarin.Android, Xamarin.Mac, 또는 Windows)에 바인딩하는 데 필요한 어셈블리 참조 해야 합니다.

플랫폼별 프로젝트를 구현 해야 합니다.

-   **응용 프로그램 계층** – 플랫폼 특정 기능 및 비즈니스 계층 개체와 사용자 인터페이스 간의 바인딩/변환 합니다.
-   **사용자 인터페이스 레이어** – 화면 사용자 지정 사용자 인터페이스 컨트롤, 유효성 검사 논리는 프레젠테이션 합니다.


<a name="Example" />


### <a name="example"></a>예제

이 다이어그램에서 응용 프로그램 아키텍처를 보여 줍니다.

 [ ![](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png "이 다이어그램에서 응용 프로그램 아키텍처를 보여 줍니다.")](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png#lightbox)

이 스크린샷은 공유 Core 프로젝트, iOS 및 Android 응용 프로그램 프로젝트를 사용 하 여 솔루션 설치를 보여 줍니다. 공유 프로젝트에는 각 아키텍처 계층 (비즈니스, 서비스, 데이터 및 데이터 액세스 코드)와 관련 된 코드가 포함 됩니다.

 ![](setting-up-a-xamarin-cross-platform-solution-images/core-solution-example.png "공유 프로젝트에는 각 아키텍처 계층 (비즈니스, 서비스, 데이터 및 데이터 액세스 코드)와 관련 된 코드가 포함 되어 있습니다.")


 <a name="Project_References" />


## <a name="project-references"></a>프로젝트 참조

프로젝트 참조를 프로젝트에 대 한 종속성을 반영합니다. Core 프로젝트의 코드를 쉽게 공유할 수 있도록 공용 어셈블리에 대 한 해당 참조를 제한 합니다.
플랫폼 관련 응용 프로그램 프로젝트는 공유 코드와 대상 플랫폼을 활용 하기 위해 필요한 다른 플랫폼별 어셈블리를 참조 합니다.

응용 프로그램 프로젝트 각 공유 프로젝트를 참조 하 고이 스크린 샷에 표시 된 것 처럼 사용자에 게 있는 기능에 필요한 사용자 인터페이스 코드를 포함 합니다.

![](setting-up-a-xamarin-cross-platform-solution-images/solution-android.png "응용 프로그램에 대 한 각 참조 공유 프로젝트 프로젝트") ![](setting-up-a-xamarin-cross-platform-solution-images/solution-ios.png "응용 프로그램에 대 한 각 참조 공유 프로젝트 프로젝트")


프로젝트를 구성 해야 하는 방법의 예는 사례 연구에 제공 됩니다.

 <a name="Adding_Files" />


## <a name="adding-files"></a>파일 추가

 <a name="Build_Action" />


### <a name="build-action"></a>빌드 작업

올바른 빌드 작업 파일 형식에 대 한 특정 설정 하는 것이 반드시 합니다. 이 목록에는 몇 가지 일반적인 파일 형식에 대 한 빌드 작업을 보여 줍니다.

-  **모든 C# 파일** – 빌드 작업: Compile
-   **Xamarin.iOS 및 Windows에서 이미지** – 빌드 작업: 콘텐츠
-   **Xamarin.iOS에서 스토리 보드 및 XIB 파일** – 빌드 작업: InterfaceDefinition
-   **이미지 및 Android에서 AXML 레이아웃** – 빌드 작업: AndroidResource
-  **Windows 프로젝트에서 XAML 파일** – 빌드 작업: 페이지
-  **Xamarin.Forms XAML 파일** – 빌드 작업: EmbeddedResource


일반적으로 IDE 파일 형식을 감지 하 고 올바른 빌드 작업을 제안 합니다.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>대/소문자 구분

마지막으로 일부 플랫폼 (예: 대/소문자 구분 파일 시스템에 있는지를 기억합니다
iOS 및 Android)를 일관 된 파일 이름 지정 표준을 사용 하 여 코드에서 사용 하는 파일 이름은 파일 시스템 정확 하 게 일치 하는지 확인 해야 합니다. 이 이미지 및 코드에서 참조 하는 기타 리소스에 대 한 특히 중요 합니다.
