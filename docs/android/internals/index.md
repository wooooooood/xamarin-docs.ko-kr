---
title: 고급 개념 및 내부 요소
description: Xamarin.Android 및 해당 API 디자인 기반이 되는 기본 아키텍처입니다.
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/21/2018
ms.openlocfilehash: f5844dd4340afa0596219a33ed1e479a0dbcfa76
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114173"
---
# <a name="advanced-concepts-and-internals"></a>고급 개념 및 내부 요소

_이 섹션에서는 아키텍처, API 디자인 및 Xamarin.Android의 제한 사항에 설명 하는 항목을 포함 합니다. 또한 가비지 컬렉션 구현 및 Xamarin.Android에서 사용할 수 있는 어셈블리를 설명 하는 항목이 포함 되어 있습니다. Xamarin.Android 이므로 [오픈 소스](https://github.com/xamarin/xamarin-android), 소스 코드를 검사 하 여 Xamarin.Android의 내부 작업을 파악할 수 이기도 합니다._


##  <a name="architectureandroidinternalsarchitecturemd"></a>[아키텍처](~/android/internals/architecture.md)

이 문서에서는 Xamarin.Android 응용 프로그램 기반이 되는 기본 아키텍처를 설명 합니다. Android 런타임 가상 컴퓨터를 사용 하 여 함께 Mono 실행 환경 내에서 Xamarin.Android 응용 프로그램을 실행 하는 방법에 대해 설명 하 고 Android 호출 가능 래퍼 및 관리 되는 호출 가능 래퍼 등의 주요 개념에 설명 합니다. 



##  <a name="api-designandroidinternalsapi-designmd"></a>[API 디자인](~/android/internals/api-design.md)

가장 핵심적인 Mono의 일부인 기본 클래스 라이브러리 외에도 Xamarin.Android 개발자 Mono를 사용 하 여 네이티브 Android 응용 프로그램을 만들 수 있도록 다양 한 Android Api에 대 한 바인딩을 제공 합니다.

Xamarin.Android의 핵심 있습니다 interop 엔진 Java 대상과 브리지 C# 세계 이며 다른.NET 언어 또는 C#에서 Java Api에 대 한 액세스를 사용 하 여 개발자에 게 제공 합니다.



##  <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[어셈블리](~/cross-platform/internals/available-assemblies.md)

Xamarin.Android는 여러 어셈블리를 사용 하 여 제공 됩니다. Silverlight는 데스크톱.NET 어셈블리의 확장된 된 하위 집합을 처럼 Xamarin.Android 여러 Silverlight 및 데스크톱.NET 어셈블리는 확장된 하위 집합 이기도 합니다. 

