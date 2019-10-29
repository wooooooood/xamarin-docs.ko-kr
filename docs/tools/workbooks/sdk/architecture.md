---
title: 아키텍처 개요
description: 이 문서에서는 Xamarin Workbooks의 아키텍처에 대해 설명 하 고 대화형 에이전트와 대화형 클라이언트가 함께 작동 하는 방식을 검토 합니다.
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: 7b3f2613e315bc05fedfb5b2fa70d11c2874ba65
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029615"
---
# <a name="architecture-overview"></a>아키텍처 개요

Xamarin Workbooks은 서로 함께 작동 해야 하는 두 가지 주요 구성 요소 ( _에이전트_ 와 _클라이언트_)를 제공 합니다.

## <a name="interactive-agent"></a>대화형 에이전트

에이전트 구성 요소는 .NET 응용 프로그램의 컨텍스트에서 실행 되는 작은 플랫폼별 어셈블리입니다.

Xamarin Workbooks은 iOS, Android, Mac, WPF 등의 여러 플랫폼에 대해 미리 작성 된 "빈" 응용 프로그램을 제공 합니다. 이러한 응용 프로그램은 명시적으로 에이전트를 호스팅합니다.

라이브 검사 (Xamarin Inspector) 중에 에이전트는 일반적인 개발 & 디버깅 워크플로의 일부로 IDE 디버거를 통해 기존 응용 프로그램에 삽입 됩니다.

## <a name="interactive-client"></a>대화형 클라이언트

클라이언트는 통합 문서/REPL 인터페이스를 제공 하는 웹 브라우저 화면을 호스트 하는 네이티브 셸 (Mac의 Cocoa, Windows의 WPF)입니다. SDK 관점에서 보면 모든 클라이언트 통합이 JavaScript 및 CSS에서 구현 됩니다.

클라이언트는 Roslyn를 통해 소스 코드를 작은 어셈블리로 컴파일하고 실행을 위해 연결 된 에이전트로 전송 합니다. 실행 결과는 렌더링을 위해 클라이언트로 다시 전송 됩니다. 통합 문서의 각 셀은 이전 셀의 어셈블리를 참조 하는 하나의 어셈블리를 생성 합니다.

에이전트는 모든 형식의 .NET 플랫폼에서 실행 될 수 있으며 실행 중인 응용 프로그램의 모든 항목에 액세스할 수 있기 때문에 플랫폼에 상관 없는 방식으로 결과를 serialize 해야 합니다.
