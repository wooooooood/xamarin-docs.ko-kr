---
title: Xamarin.ios의 부동 소수점 작업
description: 이 문서에서는 Xamarin.ios에서 32 비트 및 64 비트 정밀도 부동 소수점 연산을 처리 하 고 성능에 관련 된 영향을 설명 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: af0e37b41a7bbe831bc629b1fd916f62819b3711
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022333"
---
# <a name="floating-point-operations-in-xamarinios"></a>Xamarin.ios의 부동 소수점 작업

Xamarin.ios는 기본적으로 ARM에서 64 비트 전체 자릿수를 사용 하 여 32 비트 및 64 비트 부동 소수점 연산을 수행 합니다.  

이 더 높은 정밀도는 데스크톱에서의 C# 부동 소수점 작업 (모바일의 경우)에서 필요로 하는 것과 거의 유사 하지만 성능에 영향을 미칠 수 있습니다.

32 비트 부동 소수점 연산을 사용 하도록 32 비트 부동 소수점 코드를 컴파일할 수 있습니다.  이렇게 하려면 최소한 Xamarin.ios 8.10을 사용 하 고 "mtouch 추가 인수" 항목의 iOS 빌드 옵션 패널에서 다음 값을 설정 해야 합니다.

```
--aot-options=-O=float32
```

이는 32 비트 부동 소수점을 사용 하 여 부동 소수점 작업을 수행할 수 있도록 정적 컴파일러 (Mono의 기본 제공 정적 컴파일러 또는 LLVM 구동)에 게 알립니다.
