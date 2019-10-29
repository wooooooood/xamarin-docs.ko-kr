---
title: IOS 및 macOS의 네이티브 형식
description: 이 문서에서는 Xamarin의 Unified API 컴파일 대상 아키텍처에 따라 필요에 따라 .NET 형식을 32 비트 및 64 비트 네이티브 형식에 매핑하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
author: davidortinau
ms.author: daortin
ms.date: 01/25/2016
ms.openlocfilehash: 1168dfe0f2120e87c57b46011caba5e7a8a0c020
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015490"
---
# <a name="native-types-for-ios-and-macos"></a>IOS 및 macOS의 네이티브 형식

Mac 및 iOS Api는 32 비트 플랫폼에서 항상 32 비트인 아키텍처 관련 데이터 형식을 사용 하 고 64 비트 플랫폼에서는 64 비트를 사용 합니다.

예를 들어, 목표-C는 32 비트 시스템의 `int32_t` `NSInteger` 데이터 형식을 매핑하고 64 비트 시스템에서 `int64_t` 합니다.

이 동작을 일치 시키기 위해 통합 API에서는 `int`의 이전 사용 (.NET은 항상 `System.Int32`로 정의 됨)을 새 데이터 형식 (`System.nint`)으로 바꿉니다. "N"을 의미 "native"로 간주할 수 있으므로 플랫폼의 네이티브 정수 형식입니다.

이러한 새 데이터 형식을 사용 하는 경우 컴파일 플래그에 따라 32 비트 및 64 비트 아키텍처에 대해 동일한 소스 코드를 컴파일합니다.

## <a name="new-data-types"></a>새 데이터 형식

다음 표에서는이 새로운 32/64 비트 세계와 일치 하도록 데이터 형식에 대 한 변경 내용을 보여 줍니다.

|네이티브 형식|32 비트 지원 유형|64 비트 지원 유형|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

이러한 이름을 선택 하 여 코드에서 C# 오늘 표시 되는 것과 동일한 방식으로 코드를 볼 수 있습니다.

### <a name="implicit-and-explicit-conversions"></a>암시적 변환과 명시적 변환

새 데이터 형식의 디자인은 단일 C# 원본 파일이 호스트 플랫폼 및 컴파일 설정에 따라 32 또는 64 비트 저장소를 자연스럽 게 사용할 수 있도록 하기 위한 것입니다.

이를 통해 플랫폼별 데이터 형식에서 .NET 정수 계열 및 부동 소수점 데이터 형식으로의 암시적 변환과 명시적 변환 집합을 디자인 해야 했습니다.

데이터 손실이 발생할 수 없는 경우 (32 비트 값이 64 비트 공간에 저장 됨)에는 암시적 변환 연산자가 제공 됩니다.

명시적 변환 연산자는 데이터 손실이 발생할 가능성이 있는 경우에 제공 됩니다. 64 비트 값은 32 또는 잠재적으로 32 저장소 위치에 저장 됩니다.

`int``uint` 및 `float`는 32 비트가 항상 32 또는 64 비트에 적합 하므로 `nint`, `nuint` 및 `nfloat`으로 암시적으로 변환할 수 있습니다.

`nint`, `nuint` 및 `nfloat`은 32 또는 64 비트 값이 항상 64 비트 저장소에 맞는 `long`, `ulong` 및 `double`으로 암시적으로 변환할 수 있습니다.

네이티브 형식이 64 비트의 저장소를 보유할 수 있으므로 `nint`, `nuint` 및 `nfloat`에서 `int`, `uint` 및 `float`로의 명시적 변환을 사용 해야 합니다.

네이티브 형식에 32 비트의 저장소만 저장할 수 있으므로 `long`, `ulong` 및 `double`에서 `nint`, `nuint` 및 `nfloat`로의 명시적 변환을 사용 해야 합니다.

## <a name="coregraphics-types"></a>CoreGraphics 형식

CoreGraphics와 함께 사용 되는 점, 크기 및 사각형 데이터 형식은 실행 중인 장치에 따라 32 또는 64 비트를 사용 합니다.  처음에 iOS 및 Mac Api를 바인딩한 경우 호스트 플랫폼의 크기 (`System.Drawing`의 데이터 형식)와 일치 하는 기존 데이터 구조를 사용 했습니다.

**통합**으로 이동 하는 경우 다음 표에 표시 된 대로 `System.Drawing`의 인스턴스를 해당 `CoreGraphics`으로 바꾸어야 합니다.

|시스템 그리기의 이전 형식|새 데이터 형식 CoreGraphics|설명|
|--- |--- |--- |
|`RectangleF`|`CGRect`|부동 소수점 사각형 정보를 포함 합니다.|
|`SizeF`|`CGSize`|부동 소수점 크기 정보 (너비, 높이)를 포함 합니다.|
|`PointF`|`CGPoint`|부동 소수점, 점 정보 (X, Y)를 포함 합니다.|

새 데이터 형식은 데이터 구조의 요소를 저장 하는 데 사용 되는 반면, 새 데이터 형식은 `System.nfloat`를 사용 합니다.

## <a name="related-links"></a>관련 링크

- [플랫폼 간 앱에서의 네이티브 형식 작업](~/cross-platform/macios/native-types-cross-platform.md)
- [클래식 vs Unified API 차이점](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
