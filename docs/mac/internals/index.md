---
title: Xamarin.ios의 내부에서
description: 이 문서는 Xamarin.ios의 내부 작동을 설명 하는 다양 한 가이드에 연결 됩니다. 링크 된 문서는 미리 컴파일, Xamarin.ios 아키텍처 및 Xamarin.ios 등록 기관에 대해 미리 설명 합니다.
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 11/10/2017
ms.openlocfilehash: 51cf479ba07a769f5d7a875bb3f1203caef2ad0b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290104"
---
# <a name="under-the-hood-in-xamarinmac"></a>Xamarin.ios의 내부에서

## <a name="ahead-of-time-compilation-aotaotmd"></a>[컴파일 시간 (AOT)](aot.md)

AOT (사전) 컴파일은 시작 성능을 향상 시키기 위한 강력한 최적화 기술입니다. 그러나 빌드 시간, 응용 프로그램 크기 및 프로그램 실행에는 다양 한 방식으로 영향을 주므로 작동 방식을 이해 하는 것이 유용 합니다.

## <a name="mac-architecturearchitecturemd"></a>[Mac 아키텍처](architecture.md)

컴파일, 선택기, 등록 기관, 앱 시작 및 생성기와 같은 개념을 포함 하 여, 목적과 관련 된 xamarin.ios의 관계입니다.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac registrar](registrar.md)

Xamarin.ios는 관리 되는 세계와 Cocoa의 런타임 간 격차를 연결 하 여 관리 되는 클래스가 관리 되지 않는 목표-C 클래스를 호출할 수 있도록 하 고 이벤트가 발생할 때 다시 호출 됩니다. 이러한 "매직"을 수행 하는 데 필요한 작업은 등록자에 의해 처리 되지만 "내부적으로" 진행 상황을 이해 하는 것이 도움이 될 수 있습니다.
