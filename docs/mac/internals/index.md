---
title: Xamarin.mac에서 내부 살펴보기
description: 이 문서는 Xamarin.Mac의 내부 작업을 설명 하는 다양 한 가이드에 연결 합니다. 미리 컴파일, Xamarin.Mac 아키텍처 및 Xamarin.Mac 등록 기관에 연결 된 문서에 설명합니다.
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: 872f26febf3abbe4d659773d2bf2d27348c64513
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61033245"
---
# <a name="under-the-hood-in-xamarinmac"></a>Xamarin.mac에서 내부 살펴보기

## <a name="ahead-of-time-compilation-aotaotmd"></a>[미리 컴파일 (AOT)의](aot.md)

계속 해 서 (AOT) 시간의 컴파일이 시작 성능 향상을 위한 강력한 최적화 기술입니다. 그러나 것도 달라 집니다에 빌드 시간, 응용 프로그램 크기 및 프로그램 실행 심오한 방식 작동 방식을 이해 있으므로.

## <a name="mac-architecturearchitecturemd"></a>[Mac 아키텍처](architecture.md)

Objective-c, 컴파일, 선택기, 기관, 앱 시작 및 생성자와 같은 개념을 포함 하 여 Xamarin.Mac의 관계입니다.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac registrar](registrar.md)

Xamarin.Mac는 Cocoa의 런타임, 관리 되는 클래스를 관리 되지 않는 Objective-c 클래스를 호출 하 여 이벤트가 발생할 때 호출할 수 있도록 관리 되는 환경 사이의 간극 합니다. 이 "매직"을 수행 하는 데 필요한 작업은 등록 기관에서 처리 하지만 "내부적인" 진행 상황을 이해 경우에 따라 유용할 수 있습니다.
