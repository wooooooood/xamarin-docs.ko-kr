---
title: 사용자 지정 바인딩
description: 바인딩 프로세스를 제어 하는 메타 데이터를 편집 하 여는 Xamarin.Android 바인딩을 사용자 지정할 수 있습니다. 이러한 수동 수정 하 고 더 부합 되도록 결과 API 모양 지정에 대 한 빌드 오류를 확인 하는 데 필요한 경우가 C#/.NET 합니다. 이러한 가이드에는이 메타 데이터의 구조, 메타 데이터를 수정 하는 방법 및 메서드 매개 변수의 이름을 복구할 JavaDoc을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/25/2017
ms.openlocfilehash: 44bff372225ee1bf555eb3eeb34da918830980b4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102388"
---
# <a name="customizing-bindings"></a>사용자 지정 바인딩

_바인딩 프로세스를 제어 하는 메타 데이터를 편집 하 여는 Xamarin.Android 바인딩을 사용자 지정할 수 있습니다. 이러한 수동 수정 하 고 더 부합 되도록 결과 API 모양 지정에 대 한 빌드 오류를 확인 하는 데 필요한 경우가 C#/.NET 합니다. 이러한 가이드에는이 메타 데이터의 구조, 메타 데이터를 수정 하는 방법 및 메서드 매개 변수의 이름을 복구할 JavaDoc을 사용 하는 방법을 설명 합니다._


## <a name="overview"></a>개요
 
Xamarin.Android는 바인딩 프로세스를 대부분 자동화 그러나 경우에 따라 수동으로 수정 해야 다음 문제를 해결 합니다.

-   형식, 난독 처리 된 형식, 중복 된 이름이, 클래스 표시 문제 및 해결 되지 않는 경우도 누락으로 인 한 오류를 빌드를 확인 하 여 Xamarin.Android 도구입니다. 

-   Android API의 다른 형식에 바인딩하는 Xamarin.Android 매핑을 변경 C# (Java 매핑할 많은 개발자가 선호 하는 예를 들어 `int` 상수를 C# `enum` 상수).

-   바인딩할 필요가 없는 사용 하지 않는 형식을 제거 합니다. 

-   문자가 없는 기본 Java API에는 추가 형식입니다. 

바인딩 프로세스를 제어 하는 메타 데이터를 수정 하 여 이러한 변경의 일부나 전부를 만들 수 있습니다.


## <a name="guides"></a>안내선

다음 가이드 바인딩 프로세스를 제어 하는 메타 데이터를 설명 하 고 이러한 문제를 해결 하기 위해이 메타 데이터를 수정 하는 방법에 설명 합니다.

-   [Java 바인딩 메타 데이터](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) 는 Java 바인딩 메타 데이터의 개요를 제공 합니다.
    Java 바인딩 라이브러리를 완료 하는 데 필요한 경우에 따라 다양 한 수동 단계를 설명 하 고 더욱 긴밀 하 게.NET 디자인 지침을 수행 하려면 바인딩을 통해 노출 되는 API를 구체화 하는 방법을 설명 합니다.

-   [Javadoc 사용 하 여 매개 변수 이름 지정](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md) 바인딩된 Java 프로젝트에서 생성 한 Javadoc을 사용 하 여 Java 바인딩 프로젝트의 매개 변수 이름은 복구 하는 방법에 설명 합니다.


 

