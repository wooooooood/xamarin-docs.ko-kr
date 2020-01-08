---
title: 3 부-Xamarin 플랫폼 간 솔루션 설정
description: 이 문서에서는 Xamarin에서 플랫폼 간 솔루션을 설정 하는 방법을 설명 합니다. 공유 프로젝트 및 .NET Standard 같은 다양 한 코드 공유 전략에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
author: davidortinau
ms.author: daortin
ms.date: 03/27/2017
ms.openlocfilehash: e7cde22115830a845ed82aa907195521f36b6866
ms.sourcegitcommit: d8af612b6b3218fea396d2f180e92071c4d4bf92
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75663275"
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>3 부-Xamarin 플랫폼 간 솔루션 설정

사용 중인 플랫폼에 관계 없이 Xamarin 프로젝트는 모두 동일한 솔루션 파일 형식 (Visual Studio **.sln** 파일 형식)을 사용 합니다. 개별 프로젝트를 로드할 수 없는 경우 (예: Mac용 Visual Studio의 Windows 프로젝트)에도 개발 환경에서 솔루션을 공유할 수 있습니다.

새 플랫폼 간 응용 프로그램을 만들 때 첫 번째 단계는 빈 솔루션을 만드는 것입니다. 이 섹션에서는 플랫폼 간 모바일 앱을 빌드하기 위한 프로젝트 설정에서 수행 되는 작업에 대해 설명 합니다.

## <a name="sharing-code"></a>코드 공유

플랫폼 간 코드 공유를 구현 하는 방법에 대 한 자세한 내용은 [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md) 문서를 참조 하세요.

### <a name="net-standard"></a>.NET 표준

[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) 프로젝트를 사용 하면 플랫폼 간에 코드를 공유 하 고, Windows, Xamarin 플랫폼 (IOS, Android, Mac) 및 Linux에서 사용할 수 있는 어셈블리를 만들 수 있습니다.
Xamarin 솔루션에 대 한 코드를 공유 하는 데 권장 되는 방법입니다.

### <a name="other-options"></a>기타 옵션

이전에 Xamarin에서 사용 되는 [PCLs (이식 가능한 클래스 라이브러리)](~/cross-platform/app-fundamentals/pcl.md)및 [공유 프로젝트가](~/cross-platform/app-fundamentals/shared-projects.md)사용 되었습니다. 새 프로젝트에는 두 가지 모두 권장 되지 않습니다. .NET Standard 사용 하도록 기존 앱을 마이그레이션하는 것을 고려해 야 합니다.

## <a name="populating-the-solution"></a>솔루션 채우기

코드를 공유 하는 데 사용 되는 방법에 관계 없이 전체 솔루션 구조는 코드 공유를 권장 하는 계층화 된 아키텍처를 구현 해야 합니다.
Xamarin 방식은 코드를 두 개의 프로젝트 형식으로 그룹화 하는 것입니다.

- **핵심 (또는 "공유") 프로젝트** – 한 곳에서 다시 사용할 수 있는 코드를 작성 하 여 여러 플랫폼에서 공유 합니다. 캡슐화 원칙을 사용 하 여 가능한 모든 구현 세부 정보를 숨깁니다.
- **플랫폼별 응용 프로그램 프로젝트** – 최대한의 결합으로 재사용 가능한 코드를 사용 합니다. 플랫폼별 기능은이 수준에서 추가 되며 핵심 프로젝트에 노출 되는 구성 요소를 기반으로 빌드됩니다.

### <a name="core-project"></a>핵심 프로젝트

코드를 공유 하는 핵심 프로젝트는 .NET Standard 해야 하며 모든 플랫폼에서 사용할 수 있는 어셈블리 (ie)만 참조 해야 합니다. `System`, `System.Core` 및 `System.Xml`와 같은 일반적인 프레임 워크 네임 스페이스입니다.

핵심 프로젝트는 다음과 같은 계층을 포함 하 여 가능한 한 많은 UI 기능을 구현 해야 합니다.

- **데이터 계층** – 물리적 데이터 저장소 (예:)를 관리 하는 코드입니다. [SQLite-NET](https://www.nuget.org/packages/sqlite-net-pcl/) 또는 심지어 XML 파일입니다. 데이터 계층 클래스는 일반적으로 데이터 액세스 계층 에서만 사용 됩니다.
- **데이터 액세스 계층** – 데이터 목록, 개별 데이터 항목에 액세스 하는 메서드와 같은 응용 프로그램의 기능에 필요한 데이터 작업을 지원 하 고 해당 항목을 만들고 편집 하 고 삭제 하는 API를 정의 합니다.
- **서비스 액세스 계층** – 응용 프로그램에 클라우드 서비스를 제공 하는 선택적 계층입니다. 원격 네트워크 리소스 (웹 서비스, 이미지 다운로드 등)에 액세스 하 고 결과를 캐시할 수 있는 코드를 포함 합니다.
- **비즈니스 계층** – 플랫폼 특정 응용 프로그램에 기능을 노출 하는 모델 클래스 및 외관 또는 관리자 클래스의 정의입니다.

### <a name="platform-specific-application-projects"></a>플랫폼별 응용 프로그램 프로젝트

플랫폼별 프로젝트는 .NET Standard 프로젝트 뿐만 아니라 각 플랫폼의 SDK (Xamarin.ios, Xamarin Android, Xamarin.ios 또는 Windows)에 바인딩하는 데 필요한 어셈블리를 참조 해야 합니다.

플랫폼별 프로젝트는 다음을 구현 해야 합니다.

- **응용 프로그램 계층** – 비즈니스 계층 개체와 사용자 인터페이스 간의 플랫폼 특정 기능 및 바인딩/변환입니다.
- **사용자 인터페이스 계층** – 화면, 사용자 지정 사용자 인터페이스 컨트롤, 유효성 검사 논리 표시

## <a name="project-references"></a>프로젝트 참조

프로젝트 참조는 프로젝트에 대 한 종속성을 반영 합니다. 핵심 프로젝트는 코드를 쉽게 공유할 수 있도록 공용 어셈블리에 대 한 참조를 제한 합니다.
플랫폼별 응용 프로그램 프로젝트는 .NET Standard 프로젝트를 참조 하 고 대상 플랫폼을 활용 하는 데 필요한 기타 플랫폼별 어셈블리를 참조 합니다.

프로젝트를 구성 하는 방법에 대 한 구체적인 예제는 사례 연구에서 제공 됩니다.

## <a name="adding-files"></a>파일 추가

### <a name="build-action"></a>빌드 작업

특정 파일 형식에 대 한 올바른 빌드 작업을 설정 하는 것이 중요 합니다. 이 목록에서는 몇 가지 일반적인 파일 형식에 대 한 빌드 작업을 보여 줍니다.

- **모든 C# 파일** -빌드 작업: 컴파일
- **Xamarin.ios의 이미지 & Windows** – 빌드 작업: 콘텐츠
- **Xamarin.ios의 XIB 및 Storyboard 파일** -빌드 작업: 인터페이스 정의
- **Android의 이미지 및 XML 레이아웃** – 빌드 작업: AndroidResource
- **Windows 프로젝트의 XAML 파일** -빌드 작업: 페이지
- **Xamarin FORMS XAML 파일** -빌드 작업: EmbeddedResource

일반적으로 IDE는 파일 형식을 검색 하 고 올바른 빌드 작업을 제안 합니다.

### <a name="case-sensitivity"></a>대/소문자 구분

마지막으로 일부 플랫폼에는 대/소문자를 구분 하는 파일 시스템 (예:
iOS 및 Android)를 사용 하 여 일관 된 파일 명명 표준을 사용 하 고 코드에서 사용 하는 파일 이름이 filesystem과 정확히 일치 하는지 확인 합니다. 이는 코드에서 참조 하는 이미지 및 기타 리소스에 특히 중요 합니다.
