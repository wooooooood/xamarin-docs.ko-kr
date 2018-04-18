---
title: 아키텍처 개요
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 5a836ec6e2e46316c46e4b6d6986d08601f84db1
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2018
---
# <a name="architecture-overview"></a>아키텍처 개요

Xamarin 통합 문서 상호 함께 작동 해야 하는 두 개의 주요 구성 기능: _에이전트_ 및 _클라이언트_합니다.

## <a name="interactive-agent"></a>대화형 에이전트

에이전트 구성 요소에는.NET 응용 프로그램의 컨텍스트에서 실행 되는 작은 플랫폼별 어셈블리입니다.

Xamarin 통합 문서는 다양 한 iOS, Android, Mac 및 WPF와 같은 플랫폼에 대 한 미리 작성 된 "empty" 응용 프로그램을 제공합니다. 명시적으로 이러한 응용 프로그램 에이전트를 호스트 합니다.

실시간 검사 (Xamarin 검사기)를 하는 동안 에이전트 일반 개발 및 디버깅 워크플로의 일환으로 기존 응용 프로그램에 IDE 디버거를 통해 삽입 됩니다.

## <a name="interactive-client"></a>대화형 클라이언트

클라이언트는 통합 문서/REPL 인터페이스를 표시할 웹 브라우저 화면을 호스트 하는 네이티브 셸 (Cocoa Mac에서, Windows에서 WPF). SDK의 관점에서 모든 클라이언트 통합 JavaScript 및 CSS에서 구현 됩니다.

클라이언트는 주로 (Roslyn)을 통해 작은 어셈블리에 소스 코드 컴파일 및 실행에 대 한 연결 된 에이전트를 통해 전송 합니다. 실행 결과를 렌더링 하기 위한 클라이언트에 전송 됩니다. 통합 문서에 있는 각 셀 이전 셀의 어셈블리를 참조 하는 하나의 어셈블리를 생성 합니다.

에이전트 모든 유형의.NET 플랫폼에서 실행 될 수 있습니다 실행 중인 응용 프로그램에는 항목에 액세스할 수 있으므로 주의 해야 플랫폼 제약 없는 방식으로 결과 직렬화 하는 데 있습니다.