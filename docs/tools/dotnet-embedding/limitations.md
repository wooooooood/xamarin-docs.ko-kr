---
title: .NET 포함 제한 사항
description: 이 문서에는.NET 포함, 다른 프로그래밍 언어로.NET 코드를 사용할 수 있는 도구 제한 사항을 설명 합니다.
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 7a162d632c98b4e412fa1b7b0c0c40ac945ff09f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114355"
---
# <a name="net-embedding-limitations"></a>.NET 포함 제한 사항

이 문서.NET 포함의 제한 사항에 설명 하 고 가능한 경우에 해결 방법을 제공 합니다.

## <a name="general"></a>일반

### <a name="use-more-than-one-embedded-library-in-a-project"></a>프로젝트에서 여러 개 포함 된 라이브러리를 사용 합니다.

동일한 응용 프로그램 내에서 존재 하는 두 개의 Mono 런타임이 할는 것이 불가능 합니다. 즉, 동일한 응용 프로그램 내에서 서로 다른 두 개의.NET 포함에서 생성 된 라이브러리를 사용할 수 없습니다.

**해결 방법:** (서로 다른 프로젝트)에서 여러 어셈블리를 포함 하는 단일 라이브러리를 만들려면 생성기를 사용할 수 있습니다.

### <a name="subclassing"></a>서브클래싱

.NET 포함 응용 프로그램 내에서 Mono 런타임 통합 대상 언어 및 플랫폼에 대 한 준비 쉬운 Api 집합을 노출 하 여 줄일 수 있습니다.

하지만 이것이 양방향 통합을 예: 관리 되는 형식 하위 클래스입니다. 수 없으며 관리 되는 코드를이 공존 인식 되지 않았기 때문에 네이티브 코드 내에서 다시 호출 하는 관리 되는 코드를 예상 합니다.

필요에 따라 수 있습니다 이러한 제한 사항을 해결 부분에 예:

* 관리 코드 네이티브 코드에 p/invoke 있습니다. 이렇게 하려면 네이티브 코드에서 사용자 지정을 허용 하도록 관리 되는 코드를 사용자 지정

* Xamarin.iOS와 같은 제품을 사용 하 고 Objective-c (이 경우)를 하위 클래스로 일부 관리 NSObject 서브 클래스를 허용 하는 관리 되는 라이브러리를 노출 합니다.

## <a name="objective-c-generated-code"></a>Objective-c로 생성 된 코드

### <a name="nullability"></a>Null 허용 여부

Null 참조는 허용 되는 경우 알려주세요는.NET 또는 하지 API에 대 한 메타 데이터가 있습니다. 대부분의 Api를 발생 시킵니다 `ArgumentNullException` 없습니다 대처 하는 경우는 `null` 인수입니다. Objective-c로 처리 하는 예외는 더 나은 방지할 무언가 문제가 됩니다.

에서는 기본값 null이 아닌 인수의 수 없습니다. 헤더 파일의 정확한 null 허용 여부 주석을 생성 하 고 관리 되는 예외를 최소화 하려면 이후 (`NS_ASSUME_NONNULL_BEGIN`) 하 고 몇 가지 특정 전체 자릿수는 가능한 경우, null 허용 여부 주석을 추가 합니다.

### <a name="bitcode-ios"></a>Bitcode (iOS)

현재.NET 포함 지원 하지 않습니다 bitcode ios의 경우 일부 Xcode 프로젝트 템플릿을 사용 하도록 설정 됩니다. 이 성공적으로 생성 된 연결 프레임 워크를 사용할 수 없게 해야 합니다.

![Bitcode 옵션](images/ios-bitcode-option.png)
