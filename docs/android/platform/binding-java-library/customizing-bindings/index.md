---
title: 사용자 지정 바인딩
description: 바인딩 프로세스를 제어 하는 메타 데이터를 편집 하 여 Xamarin.Android 바인딩을 사용자 지정할 수 있습니다. 이러한 수동 수정이 고, C#와 보다 일치 되도록 결과 API를 만들기 위한 빌드 오류를 해결 하는 데 필요한 종종 /.NET 합니다. 이러한 가이드에는이 메타 데이터의 구조, 메타 데이터를 수정 하는 방법 및 JavaDoc를 복구할 메서드 매개 변수 이름을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 09/25/2017
ms.openlocfilehash: bb4f3b24be2072cb8b33893899a23951ace63607
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763139"
---
# <a name="customizing-bindings"></a>사용자 지정 바인딩

_바인딩 프로세스를 제어 하는 메타 데이터를 편집 하 여 Xamarin.Android 바인딩을 사용자 지정할 수 있습니다. 이러한 수동 수정이 고, C#와 보다 일치 되도록 결과 API를 만들기 위한 빌드 오류를 해결 하는 데 필요한 종종 /.NET 합니다. 이러한 가이드에는이 메타 데이터의 구조, 메타 데이터를 수정 하는 방법 및 JavaDoc를 복구할 메서드 매개 변수 이름을 사용 하는 방법을 설명 합니다._


## <a name="overview"></a>개요
 
Xamarin.Android는 바인딩 프로세스;의 대부분을 자동화합니다 그러나 경우에 따라 수동으로 수정은 해야 다음 문제를 해결 합니다.

-   해결 빌드 오류 형식, 난독 처리 된 형식, 중복 된 이름이, 클래스 가시성 문제 및 확인할 수 없는 경우도 않았기 때문에 발생 하 여 Xamarin.Android 도구. 

-   Xamarin.Android를 사용 하 여 C#에서 다른 형식에는 Android API 바인딩 매핑을 변경 (예를 들어 Java 매핑할 많은 개발자에 게 것을 선호 `int` C# 상수 `enum` 상수).

-   바인딩될 필요 하지 않은 사용 하지 않는 종류를 제거 합니다. 

-   Java API에서 해당 하는 항목이 있는 형식에 추가 합니다. 

바인딩 프로세스를 제어 하는 메타 데이터를 수정 하 여 이러한 변경의 일부 또는 전부를 만들 수 있습니다.


## <a name="guides"></a>안내선

바인딩 프로세스를 제어 하는 메타 데이터를 설명 하 고 이러한 문제를 해결 하기 위해이 메타 데이터를 수정 하는 방법을 설명 하는 다음 가이드:

-   [Java 바인딩 메타 데이터](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) Java 바인딩 진입 하는 메타 데이터의 개요를 제공 합니다.
    Java 바인딩 라이브러리를 완료 하는 데 필요한 경우에 따라 다양 한 수동 단계를 설명 하 고.NET 디자인 지침을 준수 하는 바인딩을 통해 노출 되는 API를 셰이핑 하는 방법을 설명 합니다.

-   [Javadoc로 매개 변수 이름 지정](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md) 바인딩된 Java 프로젝트에서 생성 된 Javadoc를 사용 하 여 매개 변수 이름에는 Java 바인딩 프로젝트를 복구 하는 방법에 설명 합니다.


 

