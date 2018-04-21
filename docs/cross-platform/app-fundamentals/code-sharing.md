---
title: 코드 공유 옵션
description: '이 문서 비교 하 여 플랫폼 간 프로젝트 간에 코드 공유의 다양 한 방법: 공유 프로젝트, 이식 가능한 클래스 라이브러리 및.NET Standard, 이점 및 단점이 각각의 포함 합니다.'
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ec3cb08b227b268c4c755545e8bc76b9e234d093
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/20/2018
---
# <a name="sharing-code-options"></a>코드 공유 옵션

_이 문서 비교 하 여 플랫폼 간 프로젝트 간에 코드 공유의 다양 한 방법: 공유 프로젝트, 이식 가능한 클래스 라이브러리 및.NET Standard, 이점 및 단점이 각각의 포함 합니다._

플랫폼 간 응용 프로그램 사이 코드를 공유 하기 위한 세 가지 대체 방법을

-   [**공유 프로젝트** ](#Shared_Projects) – 공유 자산 프로젝트 유형을 사용 하 여 소스 코드, 구성 및 플랫폼 특정 요구 사항을 관리 하 필요에 따라 #if 컴파일러 지시문을 사용 합니다.
-   [**이식 가능한 클래스 라이브러리** ](#Portable_Class_Libraries) –를 지원 하기 위해 원하는 플랫폼을 대상으로 하는 PCL 이식 가능한 클래스 라이브러리 () 만들기 및 인터페이스를 사용 하 여 플랫폼 특정 기능을 제공 합니다.
-   [**.NET 표준 라이브러리** ](#Net_Standard) – 표준.NET 프로젝트 플랫폼 특정 기능을 삽입 하는 인터페이스를 사용 하는 PCLs 비슷하게 작동 합니다.

여러 플랫폼에서 단일 코드 베이스를 사용할 수 있는이 다이어그램에 표시 된 아키텍처를 지 원하는 코드를 공유 하는 전략의 목표가입니다.

 ![](code-sharing-images/conceptualarchitecture.png "공유 코드 응용 프로그램 아키텍처")

이 문서에는 응용 프로그램에 대 한 적절 한 프로젝트 형식을 선택할 수 있도록 세 가지 방법을 비교 합니다.

<a name="Shared_Projects" />

## <a name="shared-projects"></a>공유 프로젝트

코드 파일을 공유 하는 가장 간단한 방법은 사용 하는 것을 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)합니다.

이 스크린 샷와 세 개의 응용 프로그램 프로젝트 (Android, iOS 및 Windows Phone)에 포함 된 솔루션 파일을 보여 줍니다.는 **Shared** 일반적인 C# 소스 코드 파일에 포함 된 프로젝트:

 ![](code-sharing-images/sharedsolution.png "공유 프로젝트 솔루션")

개념적 아키텍처는 각 프로젝트에서 모든 공유 소스 파일을 포함 하는 위치는 다음 다이어그램에 표시 됩니다.

 ![](code-sharing-images/sharedassetproject.png "공유 프로젝트 다이어그램")


### <a name="example"></a>예제

플랫폼 간 응용 프로그램을 지 원하는 iOS, Android 및 Windows Phone 응용 프로그램 프로젝트 각 플랫폼에 대해 필요 합니다. 공통 코드 공유 프로젝트에서 실행 됩니다.

예제 솔루션 폴더와 프로젝트 (프로젝트 이름을 선택한 표현성에 대 한 프로젝트에 이러한 명명 지침에 따라 필요가 없습니다) 다음이 포함 됩니다.

-   **Shared** -모든 프로젝트에 공통적인 코드를 포함 하는 공유 프로젝트.
-   **AppAndroid** – Xamarin.Android 응용 프로그램 프로젝트입니다.
-   **AppiOS** – Xamarin.iOS 응용 프로그램 프로젝트입니다.
-   **AppWinPhone** – Windows Phone 응용 프로그램 프로젝트입니다.


이러한 방식으로 세 개의 응용 프로그램 프로젝트를 동일한 소스 코드 (C# 파일 공유에서)를 공유 합니다. 공유 코드에 대 한 모든 편집 세 프로젝트 모두에서 공유 됩니다.


### <a name="benefits"></a>이점

-  여러 프로젝트 간에 코드를 공유할 수 있습니다.
-  공유 코드 (예: 컴파일러 지시문을 사용 하는 플랫폼에 따라 분기 될 수 있는. 사용 하 여 `#if __ANDROID__` 에 설명 된 대로 [크로스 플랫폼 응용 프로그램](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) 문서).
-  응용 프로그램 프로젝트는 공유 코드에서 활용할 수 있는 플랫폼 관련 참조를 포함할 수 있습니다 (사용 하는 등 `Community.CsharpSqlite.WP7` Tasky 샘플 Windows Phone 대 한).



### <a name="disadvantages"></a>단점

-  대부분의 다른 프로젝트 종류와 달리 공유 프로젝트에 '출력' 어셈블리를 있습니다. 컴파일하는 동안 파일 참조 하는 프로젝트의 일부로 간주 되며 해당 어셈블리로 컴파일됩니다. 어셈블리 코드를 공유 하려면 다음 이식 가능한 클래스 라이브러리 또는.NET 표준 경우 더 나은 합니다.
-  '비활성' 컴파일러 지시문 내에서 코드에 영향을 주는 리팩터링 코드는 업데이트 되지 않습니다.


 <a name="Shared_Remarks" />

### <a name="remarks"></a>설명

코드를 작성 하는 응용 프로그램 개발자에 대 한 적합 한 솔루션은 해당 응용 프로그램에서 공유 (및 다른 개발자에 게 배포 하지) 하기 위한 용도로 사용 됩니다.

 <a name="Portable_Class_Libraries" />


## <a name="portable-class-libraries"></a>이식 가능한 클래스 라이브러리


이식 가능한 클래스 라이브러리는 [여기에서 자세히 설명한](~/cross-platform/app-fundamentals/pcl.md)합니다.

 ![](code-sharing-images/portableclasslibrary.png "이식 가능한 클래스 라이브러리 다이어그램")


### <a name="benefits"></a>이점

-  여러 프로젝트 간에 코드를 공유할 수 있습니다.
-  리팩터링 작업은 항상 영향을 받는 모든 참조를 업데이트 합니다.


### <a name="disadvantages"></a>단점

-  컴파일러 지시문을 사용할 수 없습니다.
-  .NET framework의 하위 집합은 선택한 프로필에 따라 결정을 사용 하려면 사용할 수 있는 (참조는 [PCL 소개](~/cross-platform/app-fundamentals/pcl.md) 자세한 정보에 대 한).


### <a name="remarks"></a>설명

결과 어셈블리를 다른 개발자와 공유 하려는 경우 좋은 솔루션입니다.



<a name="Net_Standard" />

## <a name="net-standard-libraries"></a>.NET 표준 라이브러리

표준.NET은 [여기에서 자세히 설명한](~/cross-platform/app-fundamentals/net-standard.md)합니다.

![](code-sharing-images/netstandard.png ".NET 표준 다이어그램")

### <a name="benefits"></a>이점

-  여러 프로젝트 간에 코드를 공유할 수 있습니다.
-  리팩터링 작업은 항상 영향을 받는 모든 참조를 업데이트 합니다.
-  대형 노출 영역의.NET 클래스 라이브러리 BCL (기본) PCL 프로필 보다 ´ ù.

### <a name="disadvantages"></a>단점

 -  컴파일러 지시문을 사용할 수 없습니다.

### <a name="remarks"></a>설명

표준.NET과 비슷합니다 PCL 하지만 더 많은 수의 BCL 클래스의 및 플랫폼 지원에 대 한 더 간단한 모델입니다.



## <a name="summary"></a>요약

선택 하는 전략을 공유 하는 코드는 대상 플랫폼에 의해 구현 됩니다. 프로젝트에 가장 적합 한 방법을 선택 합니다.

PCL 또는 표준.NET에는 공유 코드 라이브러리 (특히 NuGet에 게시)를 구축 하기 위한 좋은 선택 됩니다. 공유 프로젝트는 응용 프로그램 개발자는 플랫폼 간 앱에서 다양 한 플랫폼 관련 기능을 사용 하려는 잘 작동 합니다.


## <a name="related-links"></a>관련 링크

- [크로스 플랫폼 응용 프로그램 빌드 (본문)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)
- [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [사례 연구: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky 샘플 (github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [PCL (github)를 사용 하 여 tasky 샘플](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
