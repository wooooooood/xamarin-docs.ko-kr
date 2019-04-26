---
title: IOS 및 macOS 용 네이티브 형식
description: 컴파일 대상 아키텍처를 기반으로 API를 통합 하는 Xamarin의 필요에 따라 32 비트 및 64 비트 네이티브 형식으로.NET 형식을 매핑하는 방법이 문서에서 설명 합니다.
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: fc2b91a9265fcf09e4f58d5de27a1fdef9350b2d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61199639"
---
# <a name="native-types-for-ios-and-macos"></a>IOS 및 macOS 용 네이티브 형식

Mac 및 iOS Api는 항상 32 비트 플랫폼 및 64 비트 플랫폼에서는 64 비트가 32 비트 아키텍처 관련 데이터 형식을 사용 합니다.

예를 들어, Objective-c로 매핑하는 `NSInteger` 데이터 형식을 `int32_t` 하 고 32 비트 시스템에서 `int64_t` 64 비트 시스템에서.

에 통합된 API,이 동작은 일치를 교체 하는 이전 사용 `int` (항상으로 정의 되는.NET `System.Int32`) 새 데이터 형식: `System.nint`합니다. 있습니다 생각할 수 있습니다 "n" 의미 "네이티브" 플랫폼의 기본 정수를 입력 합니다.

이러한 새로운 데이터 형식을 사용 하 여 동일한 소스 코드가 컴파일 플래그에 따라 32 비트 및 64 비트 아키텍처에 대해 컴파일됩니다.

## <a name="new-data-types"></a>새 데이터 형식

다음 표에서이 새로운 32/64 비트 환경에 맞게 데이터 유형을에서 변경 내용을 보여 줍니다.

|네이티브 형식|32 비트 지원 형식|64 비트 지원 형식|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

수 있도록 해당 이름을 선택에 C# 지금 보입니다 동일한 방식으로 더 많거나 적은 검색할 코드입니다.

### <a name="implicit-and-explicit-conversions"></a>암시적 변환과 명시적 변환

새 데이터 형식의 디자인 단일을 허용 하는 데 사용 됩니다 C# 소스 파일을 자연스럽 게 호스트 플랫폼 및 컴파일 설정에 따라 32 또는 64 비트 저장소를 사용 합니다.

이.NET 정수 계열 및 부동 소수점 데이터 형식에 암시적 변환과 명시적 변환에 플랫폼 특정 데이터 형식에서의 집합을 디자인할 주십시오 필요 합니다.

데이터 손실 (64 비트 공간에 저장 되는 32 비트 값)에 대 한 가능성이 있는 경우 암시적 변환 연산자가 제공 됩니다.

데이터 손실이 발생할 때 명시적 변환 연산자가 제공 됩니다 (64 비트 값 32 또는 잠재적으로 32 개의 저장소 위치에 저장 되 고 됨).

 `int`를 `uint` 및 `float` 는 모두 암시적으로 변환할 `nint`를 `nuint` 및 `nfloat` 32 비트 항상 32 또는 64 비트에 맞는 만큼 합니다.

 `nint``nuint` 하 고 `nfloat` 는 모두 암시적으로 변환할 `long`, `ulong` 및 `double` 32 또는 64 비트 값은 항상 64 비트 저장소에서 필요에 따라.

명시적 변환을 사용 해야 합니다 `nint`, `nuint` 하 고 `nfloat` 에 `int`, `uint` 및 `float` 하므로 네이티브 형식에는 저장소의 64 비트를 유지할 수도 있습니다.

명시적 변환에서 사용 해야 합니다 `long`, `ulong` 하 고 `double` 에 `nint`, `nuint` 및 `nfloat` 네이티브 형식이 이후에 저장소의 32 비트를 저장할 수 있습니다.

## <a name="coregraphics-types"></a>CoreGraphics 형식

CoreGraphics 사용 되는 지점, 크기 및 사각형 데이터 형식에서 실행 중인 장치에 따라 32 또는 64 비트를 사용 합니다.  호스트 플랫폼의 크기에 맞게 발생 하는 기존 데이터 구조를 사용 했습니다 바인딩하면에서는 원래 iOS 및 Mac Api (의 데이터 형식이 `System.Drawing`).

이동할 때는 **통합**, 인스턴스를 대체 해야 `System.Drawing` 사용 하 여 해당 `CoreGraphics` 다음 표에 나와 있는 것 처럼 해당:

|System.Drawing의 이전 유형|새 데이터 형식 CoreGraphics|설명|
|--- |--- |--- |
|`RectangleF`|`CGRect`|부동 지점 사각형 정보를 포함 합니다.|
|`SizeF`|`CGSize`|부동 포함 점 크기 정보 (width, height)|
|`PointF`|`CGPoint`|보유 부동 소수점, 지점 정보 (X, Y)|

데이터 구조의 요소를 저장 된 이전 데이터 사용 되는 형식 float 하나 사용 하 여 새 동안 `System.nfloat`합니다.

## <a name="related-links"></a>관련 링크

- [플랫폼 간 앱에서의 네이티브 형식 작업](~/cross-platform/macios/native-types-cross-platform.md)
- [클래식 vs 통합 API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
