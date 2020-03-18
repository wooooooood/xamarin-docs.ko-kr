---
title: 바인딩 사용자 지정
description: 바인딩 프로세스를 제어하는 메타데이터를 편집하여 Xamarin.Android 바인딩을 사용자 지정할 수 있습니다. 이러한 수동 수정은 C# 빌드 오류를 해결하고 결과 API를 C#/.NET과 더 일관되게 셰이핑하는 데 종종 필요합니다. 이러한 가이드에서는 이 메타데이터의 구조, 메타데이터를 수정하는 방법 및 JavaDoc를 사용하여 메서드 매개 변수의 이름을 복구하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/25/2017
ms.openlocfilehash: 04f3720d8684129476c955819390e91330a7800a
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020651"
---
# <a name="customizing-bindings"></a>바인딩 사용자 지정

_바인딩 프로세스를 제어하는 메타데이터를 편집하여 Xamarin.Android 바인딩을 사용자 지정할 수 있습니다. 이러한 수동 수정은 C# 빌드 오류를 해결하고 결과 API를 C#/.NET과 더 일관되게 셰이핑하는 데 종종 필요합니다. 이러한 가이드에서는 이 메타데이터의 구조, 메타데이터를 수정하는 방법 및 JavaDoc를 사용하여 메서드 매개 변수의 이름을 복구하는 방법을 설명합니다._

## <a name="overview"></a>개요

Xamarin.Android는 대부분의 바인딩 프로세스를 자동화합니다. 그러나 다음과 같은 문제를 해결하기 위해 수동으로 수정해야 하는 경우도 있습니다.

- 누락된 형식, 난독 처리된 형식, 중복 이름, 클래스 표시 유형 문제, 이밖에 Xamarin.Android 도구로 해결할 수 없는 상황에 의해 발생하는 빌드 오류를 해결 

- Xamarin.Android가 Android API를 C#의 다른 형식에 바인딩하기 위해 사용하는 매핑을 변경(예: 대부분의 개발자는 Java `int` 상수를 C# `enum` 상수에 매핑하는 것을 선호)

- 바인딩할 필요가 없는 사용되지 않는 형식을 제거 

- 기본 Java API에 해당 항목이 없는 형식을 추가 

바인딩 프로세스를 제어하는 메타데이터를 수정하여 이러한 변경 내용을 일부 또는 전부 만들 수 있습니다.

## <a name="guides"></a>가이드

다음 가이드에서는 바인딩 프로세스를 제어하는 메타데이터를 설명하고 이러한 문제를 해결하기 위해 이 메타데이터를 수정하는 방법을 설명합니다.

- [Java 바인딩 메타데이터](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)는 Java 바인딩에 사용되는 메타데이터를 개략적으로 설명합니다.
    Java 바인딩 라이브러리를 완료하는 데 필요한 여러 수동 단계를 설명하고, 바인딩에 의해 노출되는 API가 .NET 디자인 지침을 보다 엄격히 준수하도록 셰이핑하는 방법을 설명합니다.

- [Javadoc을 사용하여 매개 변수 이름 지정](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md)은 바인딩된 Java 프로젝트에서 생성된 Javadoc을 사용하여 Java 바인딩 프로젝트에서 매개 변수 이름을 복구하는 방법을 설명합니다.
