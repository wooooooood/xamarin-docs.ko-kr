---
title: "부동 소수점"
ms.topic: article
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ea29132ad4ac55f6fb151ac2125ab1add82c8518
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="floating-point"></a>부동 소수점

Xamarin.iOS는 기본적으로 수행 합니다 32 비트 및 64 비트 부동 소수점 연산 ARM에서 64 비트 전체 자릿수를 사용 하 여 합니다.  

이 높은 정밀도 더 가까운 모바일에서 바탕 화면에서 C#에서 부동 소수점 연산에서 개발자가 필요로 하는 동안 성능에 미치는 영향 중요할 수 있습니다.

32 비트 부동 소수점 연산에 사용 하려면 32 비트 부동 지점 코드를 컴파일할 수는.  이 작업을 수행 하려면 최소한 사용 Xamarin.iOS 8.10와 ios에서 설정 옵션의 패널에는 빌드는 "mtouch 불필요 한 인수" 항목에는 다음 값 줄:

     --aot-options=-O=float32

이 정적 컴파일러에 알립니다 (모노의 기본 제공 정적 컴파일러, 또는 원하는 LLVM에서 지 원하는 하나)를 32 비트 부동 소수점을 사용 하 여 부동 소수점 연산 수행 합니다.
