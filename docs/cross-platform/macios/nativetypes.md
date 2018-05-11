---
title: 네이티브 형식
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 4d11d053cf4471a98cbba0f7c97be3bef39276fb
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/10/2018
---
# <a name="native-types"></a>네이티브 형식

차이의 핵심인 Mac와 iOS Api 사용 하 여 32 비트 및 64 비트 플랫폼에서 64 비트 32 비트 플랫폼에서 항상 되는 아키텍처별 데이터 형식입니다.

예를 들어 Objective-c 매핑하는 `NSInteger` 데이터 형식입니다 `int32_t` 32 비트 시스템 및 `int64_t` 64 비트 시스템에서 합니다.

통합된 API에서이 동작에 맞게을 교체 하는 이전을 사용 하는 `int` (.net에서으로 정의 되 고 항상 `System.Int32`)을 새 데이터 형식: `System.nint`합니다.  생각할 수 있습니다 "n" 같은 의미로 "네이티브" 플랫폼의 기본 정수를 입력 합니다.

이러한 새 데이터 형식으로 32 비트, 32 비트 및 64 비트 또는 64 비트 컴파일 플래그에 따라 동일한 소스 코드는 컴파일됩니다.

## <a name="new-data-types"></a>새 데이터 형식

다음 표에서 이러한 새로운 32/64 비트 환경에 맞게 데이터 유형을의 변화를 보여줍니다.

|네이티브 형식|32 비트 지원 형식|64 비트 지원 형식|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

C# 코드를 찾을 차이가 오늘 유사할 것와 동일한 방식으로 이러한 이름을 선택 했습니다.

### <a name="implicit-and-explicit-conversions"></a>암시적 변환과 명시적 변환

새 데이터 형식이 디자인 단일 C# 원본 파일을 자연스럽 게 호스트 플랫폼 및 컴파일 설정에 따라 32 또는 64 비트 저장소를 사용할 수 있도록 하는 데 사용 됩니다.

이.NET 정수 계열 및 부동 소수점 데이터 형식에 암시적 변환과 명시적 변환 하 고 플랫폼 특정 데이터 형식에서 집합이 디자인을 위해서는 합니다.

암시적 변환 연산자는 데이터 손실 (32 비트 값 64 비트 공간에 저장 됨)에 대 한 가능성이 없을 때 제공 됩니다.

데이터 손실 가능성이 없는 경우 명시적 변환 연산자가 제공 됩니다 (64 비트 값에 저장 되는 32 개 32 잠재적으로 저장소 위치).

 `int``uint` 및 `float` 는 모두 암시적으로 변환할 `nint`, `nuint` 및 `nfloat` 32 또는 64 비트 항상 32 비트에 맞는 만큼 합니다.

 `nint``nuint` 및 `nfloat` 는 모두 암시적으로 변환할 `long`, `ulong` 및 `double` 32 또는 64 비트 값 항상 저장소 64 비트에에서 맞는 만큼 합니다.

명시적 변환에서 사용 해야 `nint`, `nuint` 및 `nfloat` 에 `int`, `uint` 및 `float` 이후 네이티브 형식에는 저장소의 64 비트를 유지할 수도 있습니다.

명시적 변환에서 사용 해야 `long`, `ulong` 및 `double` 에 `nint`, `nuint` 및 `nfloat` 네이티브 형식이 이후에을 보유할 저장소의 32 비트입니다.

## <a name="coregraphics-types"></a>CoreGraphics 형식

CoreGraphics 함께 사용 되는 포인트, 크기 및 사각형 데이터 형식에는 32 또는 64 비트에서 실행 중인 장치에 따라 사용 합니다.  호스트 플랫폼의 크기와 일치 하도록 수행 된 기존 데이터 구조는 iOS 및 Mac Api 원래 바인딩된 म 때 사용한 (의 데이터 형식이 `System.Drawing`).

이동할 때 **통합**, 인스턴스를 대체 해야 `System.Drawing` 와 해당 `CoreGraphics` 함수와 다음 표에 나와 있는 것 처럼:

|System.Drawing에서 이전 종류|새 데이터 형식 CoreGraphics|설명|
|--- |--- |--- |
|`RectangleF`|`CGRect`|부동 지점 사각형 정보를 포함 합니다.|
|`SizeF`|`CGSize`|보류 부동 소수점 크기 정보 (너비, 높이)|
|`PointF`|`CGPoint`|보유 부동 소수점, 지점 정보 (X, Y)|

이전 데이터 사용 되는 형식 부동 소수점 데이터 구조의 요소를 저장 하는 동안 하나 사용 하 여 새 `System.nfloat`합니다.

## <a name="related-links"></a>관련 링크

- [플랫폼 간 앱에서의 네이티브 형식 작업](~/cross-platform/macios/native-types-cross-platform.md)
- [클래식 vs 통합 API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
