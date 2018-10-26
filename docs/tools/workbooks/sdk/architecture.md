---
title: 아키텍처 개요
description: 이 문서는 Xamarin Workbooks, 대화형 에이전트 및 대화형 클라이언트의 함께 작동 하는 방법을 검사의 아키텍처를 설명 합니다.
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 352e8d0191512184ffd7d82fa0dfa0bc79fa24ca
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107263"
---
# <a name="architecture-overview"></a>아키텍처 개요

Xamarin Workbooks 서로 함께 작동 해야 하는 두 개의 주요 구성 기능: _에이전트_ 하 고 _클라이언트_합니다.

## <a name="interactive-agent"></a>대화형 에이전트

에이전트 구성 요소에는.NET 응용 프로그램의 컨텍스트에서 실행 되는 작은 플랫폼별 어셈블리입니다.

Xamarin Workbooks 플랫폼, iOS, Android, Mac, WPF 등 다양 한 미리 빌드된 "empty" 응용 프로그램을 제공합니다. 이러한 응용 프로그램은 명시적으로 에이전트를 호스트 합니다.

라이브 검사 (Xamarin Inspector) 하는 동안 에이전트 일반 개발 및 디버깅 워크플로의 일부분으로 기존 응용 프로그램에 IDE 디버거를 통해 주입 됩니다.

## <a name="interactive-client"></a>대화형 클라이언트

클라이언트는 통합 문서/REPL 인터페이스를 제공 하기 위한 웹 브라우저 화면을 호스트 하는 기본 셸 (Cocoa mac, Windows에서 WPF). SDK의 관점에서 모든 클라이언트 통합 JavaScript 및 CSS에서 구현 됩니다.

클라이언트는 (Roslyn)을 통해 작은 어셈블리에 소스 코드를 컴파일 및 실행에 대 한 연결 된 에이전트에 전송 합니다. 렌더링에 대 한 실행의 결과를 클라이언트로 다시 보내집니다. 통합 문서에 있는 각 셀 이전 셀의 어셈블리를 참조 하는 하나의 어셈블리를 생성 합니다.

에이전트 모든 형식의.NET 플랫폼에서 실행 될 수 있습니다 실행 중인 응용 프로그램에서 모든 항목에 대 한 액세스 권한이 있으므로 기울여야 플랫폼과 상관 없는 방식으로 결과 serialize 하 합니다.
