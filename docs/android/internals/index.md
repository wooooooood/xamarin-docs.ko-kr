---
title: 고급 개념 및 내부 기능
description: 기본 아키텍처 Xamarin.Android 및 해당 API 디자인 합니다.
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/21/2018
ms.openlocfilehash: 79e61db4c27a2d29b4ee0a9d39f2d25ea5d93303
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2018
---
# <a name="advanced-concepts-and-internals"></a>고급 개념 및 내부 기능

_이 섹션에서는 아키텍처, API 디자인 및 Xamarin.Android의 제한 사항에 설명 하는 항목이 포함 되어 있습니다. 가비지 컬렉션 구현 및 Xamarin.Android에서 사용할 수 있는 어셈블리를 설명 하는 항목이 포함 됩니다. Xamarin.Android 이므로 [오픈 소스](https://github.com/xamarin/xamarin-android), 해당 소스 코드를 검사 하 여 Xamarin.Android의 내부 작업을 이해할 수 이기도 합니다._


##  <a name="architectureandroidinternalsarchitecturemd"></a>[아키텍처](~/android/internals/architecture.md)

이 문서에서는 Xamarin.Android 응용 프로그램 뒤 기본 아키텍처를 설명 합니다. 가상 컴퓨터를 Android 런타임과 함께 모노 실행 환경 내 Xamarin.Android 응용 프로그램을 실행 하는 방법에 대해 설명 하 고 Android 호출 가능 래퍼 및 관리 되는 호출 가능 래퍼 등의 주요 개념에 설명 합니다. 



##  <a name="api-designandroidinternalsapi-designmd"></a>[API 디자인](~/android/internals/api-design.md)

핵심 모노의 일부인 기본 클래스 라이브러리 뿐만 아니라 Xamarin.Android 모노를 사용 하 여 네이티브 Android 응용 프로그램을 만들려면 통해 개발자는 다양 한 Android Api에 대 한 바인딩을 포함 되어 있습니다.

Xamarin.Android의 핵심 있습니다 interop 엔진에서 Java와 C# 브리지 당시 이며 C# 또는 다른.NET 언어에서 Java Api에 액세스할 수 있는 개발자에 게 제공 합니다.



##  <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[어셈블리](~/cross-platform/internals/available-assemblies.md)

Xamarin.Android 여러 어셈블리가 포함 되어 있습니다. Silverlight는 데스크톱.NET 어셈블리의는 확장 된 하위 집합을와 마찬가지로 Xamarin.Android 여러 Silverlight 및 데스크톱.NET 어셈블리는 확장 된 하위 집합 이기도 합니다. 

