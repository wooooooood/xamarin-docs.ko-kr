---
title: iOS 고급 개념 및 내부 요소
description: 이 항목에서는 .NET BCL (기본 클래스 라이브러리)의 Monotouch.dialog API 디자인, 어셈블리 및 클래스에 대해 설명 하 고 Mac용 Visual Studio Xcode의 Interface Builder 및 Apple의 도구 체인와 통합 하는 방법을 살펴봅니다.
ms.prod: xamarin
ms.assetid: 951713CD-D6AD-981C-A09E-4F2C98588D8B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
ms.openlocfilehash: 4157e73379be2adc7c92b8cdbc05c4cc4489daec
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022331"
---
# <a name="ios-advanced-concepts-and-internals"></a>iOS 고급 개념 및 내부 요소

_이 항목에서는 .NET BCL (기본 클래스 라이브러리)의 Monotouch.dialog API 디자인, 어셈블리 및 클래스에 대해 설명 하 고 Mac용 Visual Studio Xcode의 Interface Builder 및 Apple의 도구 체인와 통합 하는 방법을 살펴봅니다._

## <a name="api-designiosinternalsapi-designindexmd"></a>[API 디자인](~/ios/internals/api-design/index.md)

API 바인딩 뒤의 디자인 원칙을 설명 합니다.

## <a name="available-assembliescross-platforminternalsavailable-assembliesmd"></a>[사용 가능한 어셈블리](~/cross-platform/internals/available-assemblies.md)

.NET BCL (기본 클래스 라이브러리)에서 사용할 수 있는 어셈블리 및 클래스를 나열 합니다.

## <a name="xib-code-generationiosinternalsxib-code-generationmd"></a>[XIB 코드 생성](~/ios/internals/xib-code-generation.md)

또한 Mac용 Visual Studio 및 Xcode의 Interface Builder를 Interface Builder 사용 하 여 UI를 디자인 하는 방법을 설명 합니다.

> [!IMPORTANT]
> 이 문서에서는 Xcode의 Interface Builder와의 통합 Mac용 Visual Studio 설명 합니다. IOS 디자이너에 대 한 자세한 내용은 [Ios designer](~/ios/user-interface/designer/index.md) 문서를 참조 하세요.

## <a name="ios-architectureiosinternalsarchitecturemd"></a>[iOS 아키텍처](~/ios/internals/architecture.md)

Xamarin.ios 응용 프로그램은 Mono 실행 환경에서 실행 되며 AOT (전체 시간) 컴파일을 사용 하 여 코드를 ARM 어셈블리 C# 언어로 컴파일합니다. 이 가이드는 낮은 수준의 Xamarin.ios를 탐색 합니다.

## <a name="objective-c-selectorsiosinternalsobjective-c-selectorsmd"></a>[목표-C 선택기](~/ios/internals/objective-c-selectors.md)

목표-C 선택기 (메서드)를 직접 호출 하는 데 대 한 메모 및 사용입니다.

## <a name="limitationslimitationsmd"></a>[제한 사항](limitations.md)

Xamarin.ios에서 알아야 하는 문제 및 제한 사항입니다.
