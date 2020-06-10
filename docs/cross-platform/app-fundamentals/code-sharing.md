---
title: 코드 공유 개요
description: 이 문서에서는 플랫폼 간 프로젝트 간에 코드를 공유 하는 여러 가지 방법, 즉 공유 프로젝트, 이식 가능한 클래스 라이브러리 및 각각의 장점과 단점을 포함 하는 .NET Standard를 비교 합니다.
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
author: davidortinau
ms.author: daortin
ms.date: 08/06/2018
ms.openlocfilehash: 5edfd8216892eb28a2b1ad14d3ccee1668b21a43
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571222"
---
# <a name="sharing-code-overview"></a>코드 공유 개요

_이 문서에서는 플랫폼 간 프로젝트 간에 코드를 공유 하는 다양 한 방법 (.NET Standard, 공유 프로젝트 및 이식 가능한 클래스 라이브러리)을 비교 하 여 각각의 장점과 단점을 포함 합니다._

플랫폼 간 응용 프로그램 간에 코드를 공유 하는 방법에는 세 가지가 있습니다.

- [**.NET Standard 라이브러리**](#Net_Standard) – .NET Standard 프로젝트는 여러 플랫폼에서 공유 되는 코드를 구현할 수 있으며, 버전에 따라 많은 수의 .net api에 액세스할 수 있습니다. .NET Standard 1.0-1.6은 점진적으로 더 큰 Api 집합을 구현 하는 반면 .NET Standard 2.0은 .NET BCL (Xamarin 앱에서 사용할 수 있는 .NET Api 포함)의 최상의 적용을 제공 합니다.
- [**공유 프로젝트**](#Shared_Projects) – 공유 자산 프로젝트 형식을 사용 하 여 소스 코드를 구성 하 고 `#if` 플랫폼 특정 요구 사항을 관리 하는 데 필요한 컴파일러 지시문을 사용 합니다.
- [**이식 가능한 클래스 라이브러리**](#Portable_Class_Libraries) (사용 되지 않음) – Pcls (이식 가능한 클래스 라이브러리)는 공용 API 화면을 사용 하 여 여러 플랫폼을 대상으로 하 고 인터페이스를 사용 하 여 플랫폼별 기능을 제공할 수 있습니다. PCLs는 최신 버전의 Visual Studio에서 사용 되지 않습니다 &ndash; . 대신 .NET Standard를 사용 합니다.

코드 공유 전략의 목표는 단일 코드 베이스를 여러 플랫폼에서 사용할 수 있는이 다이어그램에 표시 된 아키텍처를 지 원하는 것입니다.

 ![공유 코드 응용 프로그램 아키텍처](code-sharing-images/conceptualarchitecture.png "공유 코드 응용 프로그램 아키텍처")

이 문서에서는 응용 프로그램에 적합 한 프로젝트 형식을 선택 하는 데 도움이 되는 방법을 비교해 볼 수 있습니다.

<a name="Net_Standard"></a>

## <a name="net-standard-libraries"></a>.NET Standard 라이브러리

[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) 라이브러리는 Xamarin.ios 및 xamarin.ios와 같은 플랫폼 간 프로젝트를 포함 하 여 다양 한 프로젝트 형식에서 참조할 수 있는 기본 클래스 라이브러리의 잘 정의 된 집합을 제공 합니다. .NET Standard 2.0은 기존 .NET Framework 코드와의 호환성을 최대화 하는 데 권장 됩니다.

![.NET Standard 다이어그램](code-sharing-images/netstandard.png ".NET Standard 다이어그램")

### <a name="benefits"></a>이점

- 여러 프로젝트 간에 코드를 공유할 수 있습니다.
- 리팩터링 작업은 항상 영향을 받는 모든 참조를 업데이트 합니다.
- .NET BCL (기본 클래스 라이브러리)의 더 큰 노출 영역을 PCL 프로필에서 사용할 수 있습니다. 특히 .NET Standard 2.0은 .NET Framework와 거의 동일한 API 표면을가지고 있으며 새 앱에 권장 되며 기존 PCLs를 이식 하는 것이 좋습니다.

### <a name="disadvantages"></a>단점

- 와 같은 컴파일러 지시문을 사용할 수 없습니다 `#if __IOS__` .

### <a name="remarks"></a>설명

.NET Standard는 [PCL과 유사](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries)하지만 플랫폼 지원에 대 한 간단한 모델과 BCL의 많은 클래스가 있습니다.

<a name="Shared_Projects"></a>

## <a name="shared-projects"></a>공유 프로젝트

[공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md) 는 코드 파일 및 해당 파일을 참조 하는 프로젝트에 포함 된 자산을 포함 합니다. 프로젝트 공유는 컴파일된 출력을 자체에 생성 하지 않습니다.

이 스크린샷에서는 일반적인 c # 소스 코드 파일을 포함 하는 **공유** 프로젝트와 함께 세 가지 응용 프로그램 프로젝트 (Android, IOS 및 Windows 용)를 포함 하는 솔루션 파일을 보여 줍니다.

![공유 프로젝트 솔루션](code-sharing-images/sharedsolution.png "공유 프로젝트 솔루션")

개념적 아키텍처는 다음 다이어그램에 표시 됩니다. 각 프로젝트에는 모든 공유 원본 파일이 포함 됩니다.

![공유 프로젝트 다이어그램](code-sharing-images/sharedassetproject.png "공유 프로젝트 다이어그램")

### <a name="example"></a>예제

IOS, Android 및 Windows를 지 원하는 플랫폼 간 응용 프로그램에는 각 플랫폼에 대 한 응용 프로그램 프로젝트가 필요 합니다. 공용 코드는 공유 프로젝트에 있습니다.

예제 솔루션에는 다음 폴더 및 프로젝트가 포함 되어 있습니다 (프로젝트 이름이 표현을 용으로 선택 되었으며 프로젝트에서 이러한 명명 지침을 따르지 않아도 됨).

- **공유** -모든 프로젝트에 공통 된 코드를 포함 하는 공유 프로젝트입니다.
- **Appandroid** – Xamarin android 응용 프로그램 프로젝트입니다.
- **Appios** – xamarin.ios 응용 프로그램 프로젝트입니다.
- **Appwindows** – windows 응용 프로그램 프로젝트입니다.

이러한 방식으로 세 가지 응용 프로그램 프로젝트는 동일한 소스 코드 (공유의 c # 파일)를 공유 합니다. 공유 코드에 대 한 모든 편집은 세 프로젝트 모두에서 공유 됩니다.

### <a name="benefits"></a>이점

- 여러 프로젝트 간에 코드를 공유할 수 있습니다.
- 컴파일러 지시문을 사용 하 여 공유 코드를 분기할 수 있습니다 (예: `#if __ANDROID__` [플랫폼 간 응용 프로그램 작성](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) 문서에 설명 된 대로를 사용 합니다.
- 응용 프로그램 프로젝트에는 공유 코드에서 활용할 수 있는 플랫폼별 참조가 포함 될 수 있습니다 (예: `Community.CsharpSqlite.WP7` Windows Phone에 대 한 Tasky 샘플에서 사용).

### <a name="disadvantages"></a>단점

- ' 비활성 ' 컴파일러 지시문 내의 코드에 영향을 주는 리팩터링는 해당 지시문 내의 코드를 업데이트 하지 않습니다.
- 대부분의 다른 프로젝트 형식과 달리 공유 프로젝트에는 ' output ' 어셈블리가 없습니다. 컴파일하는 동안 파일은 참조 하는 프로젝트의 일부로 처리 되 고 해당 어셈블리로 컴파일됩니다. 코드를 어셈블리로 공유 하려는 경우 .NET Standard 또는 이식 가능한 클래스 라이브러리는 더 나은 솔루션입니다.

<a name="Shared_Remarks"></a>

### <a name="remarks"></a>설명

응용 프로그램 개발자가 응용 프로그램에서 공유 하 고 다른 개발자에 게 배포 하지 않는 코드를 작성 하는 데 적합 한 솔루션입니다.

<a name="Portable_Class_Libraries"></a>

## <a name="portable-class-libraries"></a>이식 가능한 클래스 라이브러리

> [!TIP]
> .NET Standard 2.0 라이브러리는 이식 가능한 클래스 라이브러리 보다 권장 됩니다.

이식 가능한 클래스 라이브러리 [에 대해서는 여기에서 자세히 설명](~/cross-platform/app-fundamentals/pcl.md)합니다.

![이식 가능한 클래스 라이브러리 다이어그램](code-sharing-images/portableclasslibrary.png "이식 가능한 클래스 라이브러리 다이어그램")

### <a name="benefits"></a>이점

- 여러 프로젝트 간에 코드를 공유할 수 있습니다.
- 리팩터링 작업은 항상 영향을 받는 모든 참조를 업데이트 합니다.

### <a name="disadvantages"></a>단점

- 최신 버전의 Visual Studio에서 더 이상 사용 되지 않는 .NET Standard 라이브러리를 대신 사용 하는 것이 좋습니다. PCL과 .NET Standard 간의 [차이점에 대 한 설명은](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries) 다음을 참조 하세요.
- 컴파일러 지시문을 사용할 수 없습니다.
- 선택한 프로필에 따라 결정 되는 .NET framework의 하위 집합만 사용할 수 있습니다. 자세한 내용은 [PCL 소개](~/cross-platform/app-fundamentals/pcl.md) 를 참조 하세요.

### <a name="remarks"></a>설명

최신 버전의 Visual Studio에서는 PCL 템플릿이 더 이상 사용 되지 않는 것으로 간주 됩니다.

## <a name="summary"></a>요약

선택한 코드 공유 전략은 대상으로 하는 플랫폼에 따라 결정 됩니다. 프로젝트에 가장 적합 한 방법을 선택 합니다.

.NET Standard은 공유할 수 있는 코드 라이브러리를 빌드하기 위한 최상의 선택입니다 (특히 NuGet에서 게시). 공유 프로젝트는 응용 프로그램 개발자가 플랫폼 간 앱에서 다양 한 플랫폼별 기능을 사용 하도록 계획 하는 데 적합 합니다.

PCL 프로젝트는 Visual Studio에서 계속 지원 되지만 .NET Standard 새 프로젝트에 권장 됩니다.

## <a name="related-links"></a>관련 링크

- [플랫폼 간 응용 프로그램 빌드 (주 문서)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)
- [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [사례 연구: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky 샘플 (github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [PCL을 사용 하 여 tasky 샘플 (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
