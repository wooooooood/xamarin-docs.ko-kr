---
title: 공유 코드 개요
description: 이 문서에는 플랫폼 간 프로젝트 간에 코드 공유의 여러 가지 방법을 비교 합니다. 공유 프로젝트, 이식 가능한 클래스 라이브러리 및.NET Standard, 이점 및 각각의 단점을 포함 합니다.
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
author: conceptdev
ms.author: crdun
ms.date: 08/06/2018
ms.openlocfilehash: 98b5786ae4f071b4d8e8f854561db97aee037fdc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61228116"
---
# <a name="sharing-code-overview"></a>공유 코드 개요

_이 문서는 플랫폼 간 프로젝트 간에 코드 공유의 다양 한 방법 비교:.NET Standard, 공유 프로젝트 및 이식 가능한 클래스 라이브러리, 이점 및 각각의 단점을 포함 합니다._

플랫폼 간 응용 프로그램 간에 코드를 공유 하는 방법은 세 가지가 있습니다.

- [**.NET 표준 라이브러리** ](#Net_Standard) –.NET Standard 프로젝트는 여러 플랫폼 간에 공유할 수 있는 코드를 구현할 수 있습니다 하 고 (버전)에 따라 많은.NET Api에 액세스할 수 있습니다. .NET 표준 1.0 ~ 1.6.NET BCL (Xamarin 앱에서 사용할 수 있는.NET Api 포함)의 최상의 검사를 제공 하는.NET Standard 2.0 점점 더 커지는 Api 집합을 구현 합니다.
- [**프로젝트를 공유** ](#Shared_Projects) – 공유 자산 프로젝트 유형을 사용 하 여 소스 코드를 구성 하 고 사용 하 여 `#if` 플랫폼 특정 요구 사항을 관리 하는 데 필요한 만큼 컴파일러 지시문입니다.
- [**이식 가능한 클래스 라이브러리** ](#Portable_Class_Libraries) (사용 되지 않음) – 이식 가능한 클래스 라이브러리 (Pcl) 공용 API 화면을 사용 하 여 여러 플랫폼을 대상 하 인터페이스를 사용 하 여 플랫폼 특정 기능을 제공 합니다. 최신 버전의 Visual Studio에서 Pcl 사용 되지 않는 &ndash; 대신.NET Standard를 사용 합니다.

코드 공유 전략의 목표는 여러 플랫폼에서 단일 코드 베이스를 사용할 수 있는이 다이어그램에 표시 된 아키텍처를 지원 하기 위해서입니다.

 ![공유 코드 응용 프로그램 아키텍처](code-sharing-images/conceptualarchitecture.png "공유 코드 응용 프로그램 아키텍처")

이 문서에서는 응용 프로그램에 대 한 올바른 프로젝트 형식을 선택 하는 데 사용할 수 있는 메서드를 비교 합니다.

<a name="Net_Standard" />

## <a name="net-standard-libraries"></a>.NET 표준 라이브러리

[.NET standard](~/cross-platform/app-fundamentals/net-standard.md) 라이브러리 Xamarin.Android 및 Xamarin.iOS와 같은 플랫폼 간 프로젝트를 포함 하 여 다른 프로젝트 형식에서 참조 될 수 있는 기본 클래스 라이브러리는 잘 정의 된 집합을 제공 합니다. 기존.NET Framework 코드를 사용 하 여 최대 호환성에 대 한.NET standard 2.0은 사용 하는 것이 좋습니다.

![.NET standard 다이어그램](code-sharing-images/netstandard.png ".NET Standard 다이어그램")

### <a name="benefits"></a>이점

- 여러 프로젝트 간에 코드를 공유할 수 있습니다.
- 리팩터링 작업이 항상 적용된 되는 모든 참조를 업데이트합니다.
- .NET 기본 클래스 라이브러리 (BCL)의 더 큰 노출 영역을 PCL 프로필 보다 제품은입니다. 특히,.NET Standard 2.0에.NET Framework와 동일한 API 표면이 거의 하며 새 앱에 기존 Pcl 포팅 권장 됩니다.

### <a name="disadvantages"></a>단점

- 와 같은 컴파일러 지시문을 사용할 수 없습니다 `#if __IOS__`합니다.

### <a name="remarks"></a>설명

.NET standard는 [PCL 비슷합니다](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries), 하지만 플랫폼 지원과 더 많은 BCL의 클래스에 대 한 더 간단한 모델입니다.

<a name="Shared_Projects" />

## <a name="shared-projects"></a>공유 프로젝트

[공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md) 코드 파일 및 참조 하는 모든 프로젝트에 포함 된 자산을 포함 합니다. 공유 프로젝트에서 자체적으로 컴파일된 출력을 생성 되지는 않습니다.

이 스크린샷은 포함 된 세 개의 응용 프로그램 프로젝트 (Android, iOS 및 Windows) 솔루션 파일을 사용 하 여는 **공유** 일반적인 C# 소스 코드 파일을 포함 하는 프로젝트:

![프로젝트 솔루션을 공유](code-sharing-images/sharedsolution.png "프로젝트 솔루션 공유")

개념적 아키텍처는 각 프로젝트에 모든 공유 소스 파일을 포함 하는 위치는 다음 다이어그램에 표시 됩니다.

![공유 프로젝트 다이어그램](code-sharing-images/sharedassetproject.png "공유 프로젝트 다이어그램")

### <a name="example"></a>예제

IOS, Android 및 Windows를 지 원하는 플랫폼 간 응용 프로그램을 각 플랫폼에 대 한 응용 프로그램 프로젝트를 사용 해야 합니다. 일반적인 코드 공유 프로젝트에 상주합니다.

예제 솔루션은 다음 폴더와 프로젝트 (프로젝트 이름을 선택한 경우, 표현성을 프로젝트 이러한 명명 지침을 수행 하지 않아도) 포함 됩니다.

- **공유** – 모든 프로젝트에 공통 코드를 포함 하는 공유 프로젝트입니다.
- **AppAndroid** – Xamarin.Android 응용 프로그램 프로젝트입니다.
- **AppiOS** -Xamarin.iOS 응용 프로그램 프로젝트입니다.
- **AppWindows** – Windows 응용 프로그램 프로젝트입니다.

이러한 방식으로 세 가지 응용 프로그램 프로젝트를 같은 소스 코드 (C#의 파일 공유)를 공유 합니다. 공유 코드 편집 내용을 모든 세 개의 프로젝트 간에 공유 됩니다.

### <a name="benefits"></a>이점

- 여러 프로젝트 간에 코드를 공유할 수 있습니다.
- 공유 코드는 컴파일러 지시문 (예:를 사용 하 여 플랫폼에 따라 분기 될 수 있는 사용 하 여 `#if __ANDROID__` 에 설명 된 대로 합니다 [크로스 플랫폼 응용 프로그램 빌드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) 문서).
- 응용 프로그램 프로젝트는 공유 코드를 활용할 수 있는 플랫폼 특정 참조를 포함할 수 있습니다 (사용 하는 등 `Community.CsharpSqlite.WP7` Tasky 샘플 Windows Phone에서).

### <a name="disadvantages"></a>단점

- 리팩터링에 '비활성' 컴파일러 지시문 내에서 코드에 영향을 주는 해당 지시문 내에서 코드를 업데이트 되지 않습니다.
- 대부분의 다른 프로젝트 형식의 경우 달리 공유 프로젝트에는 '출력' 어셈블리를 있습니다. 컴파일하는 동안 파일을 참조 하는 프로젝트의 일부로 처리 되며 해당 어셈블리로 컴파일됩니다. 어셈블리 코드를 공유 하려는 경우 다음.NET Standard 또는 이식 가능한 클래스 라이브러리는 더 나은 솔루션입니다.

<a name="Shared_Remarks" />

### <a name="remarks"></a>설명

해당 앱에서 공유 (및 다른 개발자에 게 배포 하지 않음)에 사용할 수 있는 코드를 작성 하는 응용 프로그램 개발자를 위한 좋은 솔루션입니다.

<a name="Portable_Class_Libraries" />

## <a name="portable-class-libraries"></a>이식 가능한 클래스 라이브러리

> [!TIP]
> 이식 가능한 클래스 라이브러리를 통해.NET standard 2.0 라이브러리를 사용 하는 것이 좋습니다.

이식 가능한 클래스 라이브러리는 [여기에서 자세히 설명한](~/cross-platform/app-fundamentals/pcl.md)합니다.

![이식 가능한 클래스 라이브러리 다이어그램](code-sharing-images/portableclasslibrary.png "이식 가능한 클래스 라이브러리 다이어그램")

### <a name="benefits"></a>이점

- 여러 프로젝트 간에 코드를 공유할 수 있습니다.
- 리팩터링 작업이 항상 적용된 되는 모든 참조를 업데이트합니다.

### <a name="disadvantages"></a>단점

- 최신 버전의 Visual Studio에서 사용 되지 않음,.NET Standard 라이브러리는 대신 권장 됩니다. 이 참조 하 여 [차이점 설명은](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries) PCL 및.NET Standard 사이입니다.
- 컴파일러 지시문을 사용할 수 없습니다.
- 만.NET framework의 하위 집합을 사용 하려면 사용 가능한 선택한 프로필에 따라 결정 (참조를 [PCL 소개](~/cross-platform/app-fundamentals/pcl.md) 자세한 정보에 대 한).

### <a name="remarks"></a>설명

PCL 템플릿은 최신 버전의 Visual Studio에서 사용 되지 않는 것으로 간주 됩니다.

## <a name="summary"></a>요약

코드 공유 전략 선택 하면 대상으로 하는 플랫폼에 의해 구현 됩니다. 프로젝트에 가장 적합 한 방법을 선택 합니다.

.NET standard는 (특히 NuGet에 게시) 공유할 수 있는 코드 라이브러리를 빌드에 적합 합니다. 공유 프로젝트는 플랫폼 간 앱에서 다양 한 플랫폼 특정 기능을 사용 하려는 응용 프로그램 개발자가 적합 합니다.

PCL 프로젝트는 Visual Studio에서 지원 되는 데 계속,.NET Standard는 새 프로젝트에 대 한 권장 됩니다.

## <a name="related-links"></a>관련 링크

- [크로스 플랫폼 응용 프로그램 빌드 (본문)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)
- [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [사례 연구: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky 샘플 (github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [PCL (github)를 사용 하 여 tasky 샘플](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
