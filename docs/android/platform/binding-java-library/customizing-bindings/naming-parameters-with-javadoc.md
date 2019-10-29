---
title: Javadoc를 사용 하 여 매개 변수 명명
description: 이 문서에서는 java 프로젝트에서 생성 된 Javadoc를 사용 하 여 Java 바인딩 프로젝트에서 매개 변수 이름을 복구 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/20/2017
ms.openlocfilehash: 060c4759d39bc3b8c424ce46dc615644540fe9c2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027674"
---
# <a name="naming-parameters-with-javadoc"></a>Javadoc를 사용 하 여 매개 변수 명명

_이 문서에서는 java 프로젝트에서 생성 된 Javadoc를 사용 하 여 Java 바인딩 프로젝트에서 매개 변수 이름을 복구 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

기존 Java 라이브러리를 바인딩하는 경우 바인딩된 API에 대 한 일부 메타 데이터가 손실 됩니다. 특히 메서드의 매개 변수 이름입니다. 매개 변수 이름은 `p0`, `p1`등으로 표시 됩니다. Java `.class` 파일은 Java 소스 코드에 사용 된 매개 변수 이름을 유지 하지 않기 때문입니다. 

원본 라이브러리의 Javadoc HTML에 대 한 액세스 권한이 있는 경우 Xamarin.ios Java 바인딩 프로젝트는 매개 변수 이름을 제공할 수 있습니다. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Javadoc HTML을 Java 바인딩 프로젝트에 통합

Javadoc HTML을 Java 바인딩 프로젝트에 통합 하는 작업은 다음 단계로 구성 된 수동 프로세스입니다. 

1. 라이브러리에 대 한 Javadoc 다운로드
2. `.csproj` 파일을 편집 하 고 `<JavaDocPaths>` 속성을 추가 합니다.
3. 프로젝트를 정리 하 고 다시 빌드합니다.

이 작업을 완료 한 후에는 Java 바인딩 프로젝트에 의해 바인딩된 Api에 원래 Java 매개 변수 이름이 있어야 합니다. 

> [!NOTE]
> JavaDoc 출력에는 상당한 차이가 있습니다. 여. JAR 바인딩 도구 체인는 가능한 모든 단일 순열을 지원 하지 않으므로 일부 매개 변수의 이름이 제대로 지정 되지 않을 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 Java 바인딩 프로젝트에서 Javadoc를 사용 하 여 바인딩된 Api에 대 한 매개 변수 이름을 제공 하는 방법에 대해 설명 했습니다. 
