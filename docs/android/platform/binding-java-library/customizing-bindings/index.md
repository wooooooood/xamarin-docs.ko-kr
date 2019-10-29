---
title: 바인딩 사용자 지정
description: 바인딩 프로세스를 제어 하는 메타 데이터를 편집 하 여 Xamarin Android 바인딩을 사용자 지정할 수 있습니다. 이러한 수동 수정은 일반적으로 C#빌드 오류를 해결 하 고 생성 된 API를 셰이핑 하는 데 필요 합니다. 이러한 가이드에서는이 메타 데이터의 구조, 메타 데이터를 수정 하는 방법 및 JavaDoc를 사용 하 여 메서드 매개 변수의 이름을 복구 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/25/2017
ms.openlocfilehash: 04f3720d8684129476c955819390e91330a7800a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020651"
---
# <a name="customizing-bindings"></a>바인딩 사용자 지정

_바인딩 프로세스를 제어 하는 메타 데이터를 편집 하 여 Xamarin Android 바인딩을 사용자 지정할 수 있습니다. 이러한 수동 수정은 일반적으로 C#빌드 오류를 해결 하 고 생성 된 API를 셰이핑 하는 데 필요 합니다. 이러한 가이드에서는이 메타 데이터의 구조, 메타 데이터를 수정 하는 방법 및 JavaDoc를 사용 하 여 메서드 매개 변수의 이름을 복구 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Xamarin Android는 대부분의 바인딩 프로세스를 자동화 합니다. 그러나 다음과 같은 문제를 해결 하기 위해 수동으로 수정 해야 하는 경우도 있습니다.

- 누락 된 형식, 난독 처리 된 형식, 중복 이름, 클래스 표시 유형 문제 및 Xamarin.ios 도구에서 해결할 수 없는 기타 상황에 의해 발생 하는 빌드 오류를 해결 합니다. 

- Xamarin.ios를 사용 하 여 Android API를의 다른 형식에 바인딩하는 것과 같은 C# 매핑을 변경 합니다. 예를 들어 대부분의 개발자는 Java `int`C# 상수를`enum`상수에 매핑하는 것을 선호 합니다.

- 바인딩하지 않아도 되는 사용 되지 않는 형식을 제거 합니다. 

- 기본 Java API에 해당 항목이 없는 형식을 추가 합니다. 

바인딩 프로세스를 제어 하는 메타 데이터를 수정 하 여 이러한 변경 내용의 일부나 전부를 만들 수 있습니다.

## <a name="guides"></a>가이드

다음 가이드에서는 바인딩 프로세스를 제어 하는 메타 데이터를 설명 하 고 이러한 문제를 해결 하기 위해이 메타 데이터를 수정 하는 방법을 설명 합니다.

- [Java 바인딩 메타 데이터](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) 는 java 바인딩에 전달 되는 메타 데이터에 대 한 개요를 제공 합니다.
    Java 바인딩 라이브러리를 완료 하는 데 필요한 여러 수동 단계를 설명 하 고, 바인딩에 의해 노출 되는 API를 표시 하 여 .NET 디자인 지침을 보다 자세히 설명 하는 방법을 설명 합니다.

- [Javadoc를 사용 하 여 매개 변수 이름 지정](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md) 은 바인딩된 java 프로젝트에서 생성 된 Javadoc을 사용 하 여 Java 바인딩 프로젝트에서 매개 변수 이름을 복구 하는 방법을 설명 합니다.
