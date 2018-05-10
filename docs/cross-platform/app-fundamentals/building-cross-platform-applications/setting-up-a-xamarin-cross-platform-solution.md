---
title: -3 부 Xamarin 크로스 플랫폼 솔루션 설정
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: a13765805a3bc6be05522700960b032acbc864b5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>-3 부 Xamarin 크로스 플랫폼 솔루션 설정

어떤 플랫폼을 사용 하는 관계 없이 Xamarin 프로젝트 모두 사용 하 여 같은 솔루션 파일 형식 (Visual Studio **.sln** 파일 형식). 솔루션 (예: Mac 용 Visual Studio에서 Windows 프로젝트) 개별 프로젝트를 로드할 수 없는 경우에 개발 환경에 걸쳐 공유할 수 있습니다.



새 크로스 플랫폼 응용 프로그램을 만들 때 첫 번째 단계 빈 솔루션을 만드는 것입니다. 이 섹션 다음: 플랫폼 간 모바일 앱을 빌드하기 위한 프로젝트를 설정 합니다.

 <a name="Sharing_Code" />


## <a name="sharing-code"></a>코드 공유

참조는 [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md) 플랫폼 간에 코드 공유를 구현 하는 방법에 대 한 자세한 설명은 문서.

 <a name="Shared_Asset_Projects" />


### <a name="shared-projects"></a>공유 프로젝트

코드 파일을 공유 하는 가장 간단한 방법은 사용 되는 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)합니다.

이 메서드를 사용 하면 서로 다른 플랫폼 프로젝트에서 동일한 코드를 공유 하 고 컴파일러 지시문을 사용 하 여 다른, 플랫폼 특정 코드 경로 포함 하도록 수 있습니다.

 <a name="Portable_Class_Libraries" />


### <a name="portable-class-libraries-pcl"></a>이식 가능한 클래스 라이브러리 PCL)

지금까지.NET 프로젝트 파일 (및 결과 어셈블리) 대상이 지정 되었습니다 특정 프레임 워크 버전입니다. 이렇게 하면 프로젝트 또는 다른 프레임 워크에 의해 공유 되는 어셈블리 수 없습니다.

이식 가능한 클래스 라이브러리 (PCL)는 특별 한 유형의 Xamarin.iOS 및 Xamarin.Android를으로 WPF, 유니버설 Windows 플랫폼 및 Xbox 같은 서로 다른 CLI 플랫폼에서 사용할 수 있는 프로젝트입니다. 만 라이브러리 대상 플랫폼에 의해 제한 전체.NET framework의 하위 집합을 활용할 수 있습니다.

자세한 내용은 Xamarin에 대 한 [이식 가능한 클래스 라이브러리에 대 한 지원](~/cross-platform/app-fundamentals/pcl.md) 볼 수 있는 지침을 따릅니다 방법을 [TaskyPortable 샘플](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable) 작동 합니다.


### <a name="net-standard"></a>.NET Standard

2016에 도입 된 [.NET 표준](~/cross-platform/app-fundamentals/net-standard.md) 프로젝트 플랫폼에서는 Windows에서 사용할 수 있는 어셈블리를 생성, Xamarin 플랫폼 (iOS, Android, Mac) 및 Linux에서 코드를 공유 하는 쉬운 방법을 제공 합니다.

표준.NET 라이브러리를 작성 및 제외 하 고 (1.6으로 1.0)에서 각 버전에서 사용할 수 있는 Api 보다 쉽게 검색 하 고 각 버전은 이전 버전과 호환성 PCLs, 처럼 사용 더 낮은 버전 번호가 있습니다.



 <a name="Populating_the_Solution" />


## <a name="populating-the-solution"></a>솔루션을 채우기

코드를 공유 하는 방법이 사용에 관계 없이 전체 솔루션 구조 코드 공유를 유도 하는 계층화 된 아키텍처를 구현 해야 합니다.
두 가지 프로젝트 형식에 그룹 코드 하는 Xamarin 방법이입니다.

-   **코어 프로젝트** – 여러 플랫폼 간에 공유할 수를 한 곳에서 다시 사용할 수 있는 코드를 작성 합니다. 캡슐화의 원칙을 사용 하 여 가능한 구현 세부 정보를 숨깁니다.
-   **플랫폼 관련 응용 프로그램 프로젝트** – 거의 결합을 최대한으로 사용 하 여 재사용 가능한 코드를 사용 합니다. Core 프로젝트에 노출 하는 구성 요소 기반이 수준에서 플랫폼 관련 기능을 추가 합니다.


 <a name="Core_Project" />


### <a name="core-project"></a>코어 프로젝트

공유 코드 프로젝트에서 사용할 수 있는 모든 플랫폼-ie 어셈블리 참조 해야 합니다. 공통 프레임 워크 네임 스페이스와 같은 `System`, `System.Core` 및 `System.Xml`합니다.

공유 프로젝트 가능한 다음 계층을 포함할 수 있는 그대로 많은 UI가 아닌 기능을 구현 해야 합니다.

-   **데이터 계층** -예: 실제 데이터 저장소는 담당 하는 코드입니다.  [SQLite NET](https://github.com/praeclarum/sqlite-net), 대체 데이터베이스와 같은 [Realm.io](https://realm.io/products/realm-mobile-database/) 또는 XML 파일입니다. 데이터 계층 클래스는 대개 데이터 액세스 계층 에서만 사용 됩니다.
-   **데이터 액세스 계층** – 필요한 데이터 작업을 지 원하는 응용 프로그램의 기능에 대 한 액세스 데이터를 개별 데이터 항목의 목록을 만들 수도 및 메서드를 편집 및 삭제와 같은 API를 정의 합니다.
-   **서비스 액세스 계층** – 응용 프로그램에 클라우드를 제공 하는 선택적 계층 서비스입니다. 원격 네트워크 리소스 (웹 서비스, 이미지 다운로드 등)에 액세스 하는 코드를 포함 하 고 결과의 캐싱을 것 같습니다.
-   **비즈니스 계층** – 모델 클래스 및 플랫폼 관련 응용 프로그램에 기능을 노출 하는 외관 또는 관리자 클래스의 정의 합니다.


 <a name="Platform-Specific_Application_Projects" />


### <a name="platform-specific-application-projects"></a>플랫폼 관련 응용 프로그램 프로젝트

플랫폼별 프로젝트에는 핵심 공유 코드 프로젝트 뿐만 아니라 각 플랫폼 SDK (Xamarin.iOS, Xamarin.Android, Xamarin.Mac, 또는 Windows)에 바인딩하는 데 필요한 어셈블리 참조 해야 합니다.

플랫폼별 프로젝트를 구현 해야 합니다.

-   **응용 프로그램 계층** – 플랫폼 특정 기능 및 바인딩/비즈니스 계층 개체와 사용자 인터페이스 간에 변환 합니다.
-   **사용자 인터페이스 레이어** – 화면, 사용자 지정 사용자 인터페이스 컨트롤, 유효성 검사 논리를 표시 합니다.


<a name="Example" />


### <a name="example"></a>예제

이 다이어그램에는 응용 프로그램 아키텍처를 보여 줍니다.

 [ ![](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png "이 다이어그램에는 응용 프로그램 아키텍처 보여 줍니다.")](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png#lightbox)

이 스크린샷은 공유 코어 프로젝트, iOS 및 Android 응용 프로그램 프로젝트를 사용 하 여 솔루션 설치를 보여 줍니다. 공유 프로젝트에는 각 아키텍처 계층 (비즈니스, 서비스, 데이터 및 데이터 액세스 코드)와 관련 된 코드가 포함 됩니다.

 ![](setting-up-a-xamarin-cross-platform-solution-images/core-solution-example.png "공유 프로젝트에는 각 아키텍처 계층 (비즈니스, 서비스, 데이터 및 데이터 액세스 코드)와 관련 된 코드가 포함 된")


 <a name="Project_References" />


## <a name="project-references"></a>프로젝트 참조

프로젝트 참조는 프로젝트에 대 한 종속성을 반영합니다. 핵심 프로젝트에서 코드를 쉽게 공유할 수 있도록 공용 어셈블리에 해당 참조가 제한 합니다.
플랫폼 관련 응용 프로그램 프로젝트는 공유 코드와 대상 플랫폼을 활용 하는 데 필요한 다른 플랫폼 관련 어셈블리를 참조 합니다.

응용 프로그램 프로젝트 각각 공유 프로젝트를 참조 하 고이 스크린 샷에 표시 된 것 처럼 사용자에 게 있는 기능에 필요한 사용자 인터페이스 코드를 포함 합니다.

![](setting-up-a-xamarin-cross-platform-solution-images/solution-android.png "응용 프로그램에 대 한 각 참조 공유 프로젝트 프로젝트") ![ ] (setting-up-a-xamarin-cross-platform-solution-images/solution-ios.png "응용 프로그램에 대 한 각 참조 공유 프로젝트 프로젝트")


구체적인 예제는 프로젝트 구성 방법을 사례 연구에서 제공 됩니다.

 <a name="Adding_Files" />


## <a name="adding-files"></a>파일 추가

 <a name="Build_Action" />


### <a name="build-action"></a>빌드 작업

올바른 빌드 작업을 파일 형식을 특정 설정 하는 것이 유용 합니다. 이 목록에는 몇 가지 일반적인 파일 형식에 대 한 빌드 작업이 나와 있습니다.

-  **모든 C# 파일** – 빌드 작업: 컴파일
-   **Xamarin.iOS 및 Windows에서 이미지** – 빌드 작업: 콘텐츠
-   **Xamarin.iOS에서 XIB 및 스토리 보드 파일** – 빌드 작업: InterfaceDefinition
-   **이미지와 Android에서 AXML 레이아웃** – 빌드 작업: AndroidResource
-  **Windows 프로젝트에 XAML 파일** – 빌드 작업: 페이지
-  **Xamarin.Forms XAML 파일** – 빌드 작업: 포함 리소스


일반적으로 IDE 파일 종류를 검색 하 고 올바른 빌드 동작을 제안 합니다.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>대/소문자 구분

마지막으로, 대/소문자 구분 파일 시스템 (예: 일부 플랫폼 한지를 기억.
iOS 및 Android) 하 일관적인 파일 명명 표준을 사용 하 고 코드에서 사용 하는 파일 이름을 파일 시스템은 정확히 일치 하는지 확인 해야 합니다. 이 이미지 및 코드에서 참조 하는 기타 리소스에 대 한 특히 중요 합니다.
