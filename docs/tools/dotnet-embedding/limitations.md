---
title: ".NET 포함 제한 사항"
ms.topic: article
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 110eb4169103f99c1538a1846fd231069863530c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="net-embedding-limitations"></a>.NET 포함 제한 사항


이 문서 (Embeddinator 4000)를 포함 하는.NET의 제한 사항을 설명 하 고 가능한 경우 항상 해당 작업에 대 한 해결 방법을 제공 합니다.

## <a name="general"></a>일반

### <a name="use-more-than-one-embedded-library-in-a-project"></a>프로젝트에서 여러 개 포함 된 라이브러리를 사용 하 여

동일한 응용 프로그램 내에 함께 두 개의 모노 런타임 있어야는 것이 불가능 합니다. 즉, 동일한 응용 프로그램 내 서로 다른 두 개의 생성 된 4000 embeddinator libaries를 사용할 수 없습니다.

**해결 방법:** (서로 다른 프로젝트)에서 여러 어셈블리를 포함 하는 단일 라이브러리를 만드는 데 생성자를 사용할 수 있습니다.

### <a name="subclassing"></a>서브클래싱

embeddinator 대상 언어 및 플랫폼에 대 한 즉시 사용할 Api 집합을 노출 하 여 응용 프로그램 내 모노 런타임 통합 래핑할 수 있습니다.

하지만 두 가지 방식으로 통합 되지 예를 들어 하위 클래스는 관리 되는 형식 수 없으며 관리 되는 코드는 관리 코드가이 공존 인식 하지 않습니다. 이후 내부의 네이티브 코드를 다시 호출할 수 있습니다.

필요에 따라 수 있습니다 이러한 제한의 해결 방법은 부분에 예:

* 관리 코드가 네이티브 코드에 p/invoke 수 있습니다. 이 위해서는; 네이티브 코드에서 사용자 지정을 허용 하도록 관리 되는 코드를 사용자 지정

* Xamarin.iOS 같은 제품을 사용 하 고 수 있도록 만드는 ObjC (이 경우) 하위 클래스는 관리 되는 일부 NSObject 하위 클래스는 관리 되는 라이브러리를 노출 합니다.


## <a name="objc-generated-code"></a>ObjC 생성 코드

### <a name="nullability"></a>Null 허용 여부

.NET에 메타 데이터가 있으면는 얼굴 null 참조는 허용 되는 경우 API에 대해 합니다. 대부분의 Api를 발생 시킵니다 `ArgumentNullException` 대처할 수 없는 경우는 `null` 인수입니다. 이 더 나은 것을 방지할 ObjC 처리 하는 예외는 문제가 있는입니다.

에서는 null이 아닌 인수를 기본 관리 되는 예외를 최소화 하 고 헤더 파일에 정확 하 게 null 허용 여부 주석을 생성할 수 없습니다 (`NS_ASSUME_NONNULL_BEGIN`) 하 고 몇 가지 특정, 전체 자릿수 조치가 가능한 경우, null 허용 여부 주석을 추가 합니다.
