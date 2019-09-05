---
title: 3 부-Xamarin 플랫폼 간 솔루션 설정
description: 이 문서에서는 Xamarin에서 플랫폼 간 솔루션을 설정 하는 방법을 설명 합니다. 공유 프로젝트 및 .NET Standard 같은 다양 한 코드 공유 전략에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: f8b8f13f323f404554ca73c3e75c23713e0fbe35
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70288840"
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>3 부-Xamarin 플랫폼 간 솔루션 설정

사용 중인 플랫폼에 관계 없이 Xamarin 프로젝트는 모두 동일한 솔루션 파일 형식 (Visual Studio **.sln** 파일 형식)을 사용 합니다. 개별 프로젝트를 로드할 수 없는 경우 (예: Mac용 Visual Studio의 Windows 프로젝트)에도 개발 환경에서 솔루션을 공유할 수 있습니다.



새 플랫폼 간 응용 프로그램을 만들 때 첫 번째 단계는 빈 솔루션을 만드는 것입니다. 이 섹션에서는 플랫폼 간 모바일 앱을 빌드하기 위한 프로젝트 설정에서 수행 되는 작업에 대해 설명 합니다.

 <a name="Sharing_Code" />


## <a name="sharing-code"></a>코드 공유

플랫폼 간 코드 공유를 구현 하는 방법에 대 한 자세한 내용은 [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md) 문서를 참조 하세요.

 <a name="Shared_Asset_Projects" />


### <a name="shared-projects"></a>공유 프로젝트

코드 파일을 공유 하는 가장 간단한 방법은 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)를 사용 하는 것입니다.

이 메서드를 사용 하면 서로 다른 플랫폼 프로젝트에서 동일한 코드를 공유할 수 있으며, 컴파일러 지시문을 사용 하 여 플랫폼별 코드 경로를 다르게 포함할 수 있습니다.

 <a name="Portable_Class_Libraries" />


### <a name="portable-class-libraries-pcl"></a>PCL(이식 가능한 클래스 라이브러리)

지금까지 .NET 프로젝트 파일 (및 결과 어셈블리)은 특정 프레임 워크 버전을 대상으로 합니다. 이렇게 하면 다른 프레임 워크에서 프로젝트 또는 어셈블리를 공유할 수 없습니다.

PCL (이식 가능한 클래스 라이브러리)은 Xamarin.ios 및 Xamarin.ios와 같은 개별 CLI 플랫폼 뿐만 아니라 WPF, 유니버설 Windows 플랫폼 및 Xbox에서 사용할 수 있는 특수 한 형식의 프로젝트입니다. 라이브러리는 대상 플랫폼에 의해 제한 되는 전체 .NET framework의 하위 집합만 활용할 수 있습니다.

[이식 가능한 클래스 라이브러리에 대 한 Xamarin 지원](~/cross-platform/app-fundamentals/pcl.md) 에 대 한 자세한 내용을 읽고 여기에 설명 된 지침에 따라 [Taskyportable 샘플이](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable) 작동 하는 방식을 확인할 수 있습니다.


### <a name="net-standard"></a>.NET Standard

2016에 도입 된 [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) 프로젝트는 플랫폼 간에 코드를 공유 하 고, Windows, Xamarin 플랫폼 (IOS, Android, Mac) 및 Linux에서 사용할 수 있는 어셈블리를 생성 하는 쉬운 방법을 제공 합니다.

각 버전에서 사용 가능한 Api (1.0 ~ 1.6)가 더 쉽게 검색 되 고 각 버전이 더 낮은 버전 번호와 이전 버전과 호환 되는 경우를 제외 하 고 .NET Standard 라이브러리는 PCLs와 같이 만들고 사용할 수 있습니다.



 <a name="Populating_the_Solution" />


## <a name="populating-the-solution"></a>솔루션 채우기

코드를 공유 하는 데 사용 되는 방법에 관계 없이 전체 솔루션 구조는 코드 공유를 권장 하는 계층화 된 아키텍처를 구현 해야 합니다.
Xamarin 방식은 코드를 두 개의 프로젝트 형식으로 그룹화 하는 것입니다.

- **핵심 프로젝트** – 한 곳에서 다시 사용할 수 있는 코드를 작성 하 여 여러 플랫폼에서 공유 합니다. 캡슐화 원칙을 사용 하 여 가능한 모든 구현 세부 정보를 숨깁니다.
- **플랫폼별 응용 프로그램 프로젝트** – 최대한의 결합으로 재사용 가능한 코드를 사용 합니다. 플랫폼별 기능은이 수준에서 추가 되며 핵심 프로젝트에 노출 되는 구성 요소를 기반으로 빌드됩니다.


 <a name="Core_Project" />


### <a name="core-project"></a>핵심 프로젝트

공유 코드 프로젝트는 모든 플랫폼 (ie)에서 사용할 수 있는 어셈블리만 참조 해야 합니다. , `System` `System.Core` 및 와같은일반적인프레임워크네임스페이스`System.Xml`

공유 프로젝트는 다음과 같은 계층을 포함 하 여 가능한 한 많은 UI 기능을 구현 해야 합니다.

- **데이터 계층** – 물리적 데이터 저장소 (예:)를 관리 하는 코드입니다.  [SQLite-NET](https://github.com/praeclarum/sqlite-net), [REALM.IO](https://realm.io/products/realm-mobile-database/) 또는 심지어 XML 파일 등의 대체 데이터베이스입니다. 데이터 계층 클래스는 일반적으로 데이터 액세스 계층 에서만 사용 됩니다.
- **데이터 액세스 계층** – 데이터 목록, 개별 데이터 항목에 액세스 하는 메서드와 같은 응용 프로그램의 기능에 필요한 데이터 작업을 지원 하 고 해당 항목을 만들고 편집 하 고 삭제 하는 API를 정의 합니다.
- **서비스 액세스 계층** – 응용 프로그램에 클라우드 서비스를 제공 하는 선택적 계층입니다. 원격 네트워크 리소스 (웹 서비스, 이미지 다운로드 등)에 액세스 하 고 결과를 캐시할 수 있는 코드를 포함 합니다.
- **비즈니스 계층** – 플랫폼 특정 응용 프로그램에 기능을 노출 하는 모델 클래스 및 외관 또는 관리자 클래스의 정의입니다.


 <a name="Platform-Specific_Application_Projects" />


### <a name="platform-specific-application-projects"></a>플랫폼별 응용 프로그램 프로젝트

플랫폼별 프로젝트는 핵심 공유 코드 프로젝트 뿐만 아니라 각 플랫폼의 SDK (Xamarin.ios, Xamarin Android, Xamarin.ios 또는 Windows)에 바인딩하는 데 필요한 어셈블리를 참조 해야 합니다.

플랫폼별 프로젝트는 다음을 구현 해야 합니다.

- **응용 프로그램 계층** – 비즈니스 계층 개체와 사용자 인터페이스 간의 플랫폼 특정 기능 및 바인딩/변환입니다.
- **사용자 인터페이스 계층** – 화면, 사용자 지정 사용자 인터페이스 컨트롤, 유효성 검사 논리 표시


<a name="Example" />


### <a name="example"></a>예제

응용 프로그램 아키텍처는 다음 다이어그램에 나와 있습니다.

 [![](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png "이 다이어그램에는 응용 프로그램 아키텍처가 나와 있습니다.")](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png#lightbox)

이 스크린샷에서는 shared Core 프로젝트, iOS 및 Android 응용 프로그램 프로젝트를 사용 하 여 솔루션을 설치 하는 방법을 보여 줍니다. 공유 프로젝트에는 각 아키텍처 계층 (비즈니스, 서비스, 데이터 및 데이터 액세스 코드)과 관련 된 코드가 포함 되어 있습니다.

 ![](setting-up-a-xamarin-cross-platform-solution-images/core-solution-example.png "공유 프로젝트에는 각 아키텍처 계층 (비즈니스, 서비스, 데이터 및 데이터 액세스 코드)과 관련 된 코드가 포함 되어 있습니다.")


 <a name="Project_References" />


## <a name="project-references"></a>프로젝트 참조

프로젝트 참조는 프로젝트에 대 한 종속성을 반영 합니다. 핵심 프로젝트는 코드를 쉽게 공유할 수 있도록 공용 어셈블리에 대 한 참조를 제한 합니다.
플랫폼별 응용 프로그램 프로젝트는 공유 코드와 대상 플랫폼을 활용 하는 데 필요한 기타 플랫폼별 어셈블리를 참조 합니다.

응용 프로그램은 각각 공유 프로젝트를 참조 하 고 다음 스크린샷에 표시 된 것 처럼 사용자에 게 기능을 제공 하는 데 필요한 사용자 인터페이스 코드를 포함 합니다.

![](setting-up-a-xamarin-cross-platform-solution-images/solution-android.png "응용 프로그램에 대 한 각 참조 공유 프로젝트 프로젝트") ![](setting-up-a-xamarin-cross-platform-solution-images/solution-ios.png "응용 프로그램에 대 한 각 참조 공유 프로젝트 프로젝트")


프로젝트를 구성 하는 방법에 대 한 구체적인 예제는 사례 연구에서 제공 됩니다.

 <a name="Adding_Files" />


## <a name="adding-files"></a>파일 추가

 <a name="Build_Action" />


### <a name="build-action"></a>빌드 작업

특정 파일 형식에 대 한 올바른 빌드 작업을 설정 하는 것이 중요 합니다. 이 목록에서는 몇 가지 일반적인 파일 형식에 대 한 빌드 작업을 보여 줍니다.

- **모든 C# 파일** -빌드 작업: 컴파일
- **Xamarin.ios의 이미지 & Windows** – 빌드 작업: 콘텐츠
- **Xamarin.ios의 XIB 및 Storyboard 파일** -빌드 작업: InterfaceDefinition
- **Android의 이미지 및 AXML 레이아웃** – 빌드 작업: AndroidResource
- **Windows 프로젝트의 XAML 파일** -빌드 작업: Page
- **Xamarin FORMS XAML 파일** – 빌드 작업: EmbeddedResource


일반적으로 IDE는 파일 형식을 검색 하 고 올바른 빌드 작업을 제안 합니다.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>대/소문자 구분

마지막으로 일부 플랫폼에는 대/소문자를 구분 하는 파일 시스템 (예:
iOS 및 Android)를 사용 하 여 일관 된 파일 명명 표준을 사용 하 고 코드에서 사용 하는 파일 이름이 filesystem과 정확히 일치 하는지 확인 합니다. 이는 코드에서 참조 하는 이미지 및 기타 리소스에 특히 중요 합니다.
