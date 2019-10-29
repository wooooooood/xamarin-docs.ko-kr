---
title: .NET 포함 제한 사항
description: 이 문서에서는 다른 프로그래밍 언어로 .NET 코드를 사용할 수 있도록 하는 도구인 .NET 포함의 제한 사항에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 4e2b653365a747b30016a1fbd42b8a01c4c87848
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029753"
---
# <a name="net-embedding-limitations"></a>.NET 포함 제한 사항

이 문서에서는 .NET 포함의 제한 사항에 대해 설명 하 고 가능한 경우이에 대 한 해결 방법을 제공 합니다.

## <a name="general"></a>일반

### <a name="use-more-than-one-embedded-library-in-a-project"></a>프로젝트에 포함 된 라이브러리를 둘 이상 사용

동일한 응용 프로그램 내에 두 개의 Mono 런타임이 공동으로 존재 하는 것은 가능 하지 않습니다. 즉, 동일한 응용 프로그램 내에서 두 개의 서로 다른 .NET 포함 생성 라이브러리를 사용할 수 없습니다.

**해결 방법:** 생성기를 사용 하 여 여러 프로젝트의 여러 어셈블리를 포함 하는 단일 라이브러리를 만들 수 있습니다.

### <a name="subclassing"></a>서브클래싱하

.NET 포함은 대상 언어 및 플랫폼에 대 한 바로 사용할 수 있는 Api 집합을 노출 하 여 응용 프로그램 내에서 Mono 런타임의 통합을 용이 하 게 합니다.

그러나이는 양방향 통합이 아닙니다. 예를 들어 관리 되는 코드는이 공동 공존을 인식 하지 못하므로 관리 되는 형식을 하위 클래스로 지정할 수 없으며 관리 되는 코드가 네이티브 코드 내부에서 콜백 될 것으로 예측할 수 없습니다.

사용자의 요구에 따라 이러한 제한의 일부를 해결할 수 있습니다 (예:).

* 관리 코드는 네이티브 코드에 p/invoke를 사용할 수 있습니다. 이렇게 하려면 네이티브 코드의 사용자 지정을 허용 하도록 관리 코드를 사용자 지정 해야 합니다.

* Xamarin.ios와 같은 제품을 사용 하 여 관리 되는 라이브러리 (이 경우에는 관리 되는 NSObject 서브 클래스의 하위 클래스)를 제공 합니다.

## <a name="objective-c-generated-code"></a>목표-C 생성 된 코드

### <a name="nullability"></a>여부가

.NET에는 null 참조를 사용할 수 있는지 여부를 나타내는 메타 데이터가 없으므로 API에 대 한 메타 데이터가 없습니다. 대부분의 Api는 `null` 인수를 처리할 수 없는 경우 `ArgumentNullException`를 throw 합니다. 이것은 목표-C 예외를 처리 하는 것이 더 좋습니다.

헤더 파일에서 정확한 null 허용 여부 주석을 생성할 수 없으며 관리 되는 예외를 최소화 하려는 경우 기본적으로 null이 아닌 인수 (`NS_ASSUME_NONNULL_BEGIN`)로 지정 하 고 전체 자릿수가 가능한 경우 null 허용 여부 주석을 추가 합니다.

### <a name="bitcode-ios"></a>Bitcode (iOS)

현재 .NET 포함은 일부 Xcode 프로젝트 템플릿에 대해 사용 하도록 설정 된 iOS에서 bitcode를 지원 하지 않습니다. 생성 된 프레임 워크를 성공적으로 연결 하려면이 기능을 사용 하지 않도록 설정 해야 합니다.

![Bitcode 옵션](images/ios-bitcode-option.png)
