---
title: Xamarin.iOS에서 부동 소수점 연산
description: 이 문서는 Xamarin.iOS에서 32 비트 및 64 비트 정밀도 부동 소수점 작업을 처리 하는 방법을 설명 합니다 하 고 성능 관련된 영향을 설명 합니다.
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.openlocfilehash: c5ee1b833e309c78c7338298cbe5c8800afb1ba1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116240"
---
# <a name="floating-point-operations-in-xamarinios"></a>Xamarin.iOS에서 부동 소수점 연산

Xamarin.iOS는 기본적으로 수행 32 비트 및 64 비트 부동 소수점 연산과 ARM에서 64 비트 정밀도 사용 합니다.  

이 높은 정밀도 부동 소수점 연산에서에서 개발자는 가깝습니다 하는 동안 C# 모바일, 데스크톱, 성능에 영향을 클 수 있습니다.

컴파일할에 32 비트 부동 소수점 코드 32 비트 부동 소수점 작업을 사용 하는 것이 가능 합니다.  이 작업을 수행 하려면 이상을 사용 해야 Xamarin.iOS 8.10 및 iOS에서 설정 옵션의 패널 빌드를 "mtouch 추가 인수" 항목에는 다음 값을 줄:

     --aot-options=-O=float32

이 정적 컴파일러 알려줍니다 (Mono의 기본 제공 정적 컴파일러 또는 LLVM 기반 것) 32 비트 부동 소수점 수를 사용 하 여 부동 소수점 작업을 수행 합니다.
