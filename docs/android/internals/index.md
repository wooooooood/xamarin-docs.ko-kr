---
title: 고급 개념 및 내부 요소
description: Xamarin.ios 및 API 디자인의 기반이 되는 기본 아키텍처입니다.
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/21/2018
ms.openlocfilehash: 97382243ac5f767d94a782b895401c1f2f8ae554
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027852"
---
# <a name="advanced-concepts-and-internals"></a>고급 개념 및 내부 요소

_이 섹션에는 Xamarin Android의 아키텍처, API 디자인 및 제한 사항에 대해 설명 하는 항목이 포함 되어 있습니다. 또한 해당 가비지 수집 구현 및 Xamarin.ios에서 사용할 수 있는 어셈블리에 대해 설명 하는 항목이 포함 되어 있습니다. Xamarin.ios는 [오픈 소스](https://github.com/xamarin/xamarin-android)이므로 소스 코드를 검사 하 여 xamarin.ios의 내부 기능을 이해 하는 것도 가능 합니다._

## <a name="architectureandroidinternalsarchitecturemd"></a>[아키텍처](~/android/internals/architecture.md)

이 문서에서는 Xamarin Android 응용 프로그램의 기본 아키텍처에 대해 설명 합니다. Android 런타임 가상 머신과 함께 Mono 실행 환경 내에서 Xamarin Android 응용 프로그램을 실행 하는 방법과 Android 호출 가능 래퍼 및 관리 되는 호출 가능 래퍼와 같은 주요 개념을 설명 합니다. 

## <a name="api-designandroidinternalsapi-designmd"></a>[API 디자인](~/android/internals/api-design.md)

Mono의 일부인 핵심 기본 클래스 라이브러리 외에도 Xamarin.ios는 개발자가 Mono를 사용 하 여 네이티브 Android 응용 프로그램을 만들 수 있도록 다양 한 Android Api에 대 한 바인딩을 제공 합니다.

Xamarin.ios의 핵심에는 C# 전 세계와 java 세계를 연결 하 고 개발자에 게 또는 다른 .net 언어에서 C# java api에 대 한 액세스 권한을 제공 하는 interop 엔진이 있습니다.

## <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[어셈블리](~/cross-platform/internals/available-assemblies.md)

Xamarin Android는 여러 어셈블리를 제공 합니다. Silverlight가 데스크톱 .NET 어셈블리의 확장 된 하위 집합이 면 Xamarin. Android도 여러 Silverlight 및 데스크톱 .NET 어셈블리의 확장 된 하위 집합입니다. 
