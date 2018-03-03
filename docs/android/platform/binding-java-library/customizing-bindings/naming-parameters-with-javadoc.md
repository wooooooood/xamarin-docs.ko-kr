---
title: "Javadoc와 매개 변수 이름 지정"
description: "이 문서에서는 Java 프로젝트에서 생성 된 Javadoc를 사용 하 여 매개 변수 이름에는 Java 바인딩 프로젝트를 복구 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/20/2017
ms.openlocfilehash: 84dfe88e912241eb0024143bca568ae75e5bfa28
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="naming-parameters-with-javadoc"></a>Javadoc와 매개 변수 이름 지정

_이 문서에서는 Java 프로젝트에서 생성 된 Javadoc를 사용 하 여 매개 변수 이름에는 Java 바인딩 프로젝트를 복구 하는 방법을 설명 합니다._

<a name="Overview" />

## <a name="overview"></a>개요

기존 Java 라이브러리를 바인딩하는 경우 바인딩된 API에 대 한 일부 메타 데이터가 손실 됩니다. 특히 메서드에 매개 변수 이름입니다. 매개 변수 이름으로 나타납니다. `p0`, `p1`등입니다. 즉,이 Java `.class` 파일 Java 소스 코드에 사용 된 매개 변수 이름을 유지 하지 않습니다. 

Xamarin.Android Java 바인딩 프로젝트 Javadoc HTML 해야 원본 라이브러리에서 액세스 하는 경우 매개 변수 이름을 제공할 수 있습니다. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>프로젝트를 바인딩 Java에 Javadoc HTML 통합

Javadoc HTML Java 바인딩 프로젝트 통합 하는 다음 단계로 구성 되는 수동 프로세스입니다. 

1.  라이브러리의 Javadoc 다운로드
2.  편집 된 `.csproj` 파일을 추가 `<JavaDocPaths>` 속성:
3.  정리 하 고 프로젝트를 다시 작성

이 작업이 완료 되 면 원래 Java 매개 변수 이름은 Java 바인딩 프로젝트에 의해 바인딩된 Api에 존재 해야 합니다. 


> [!NOTE]
> **참고:** 는 다양 한 JavaDoc 출력에 대 한 분산 합니다. 합니다. JAR 바인딩 도구 체인에 모든 단일 가능한 순열 지원 하지 않습니다 하며 결과적으로 일부 매개 변수 수 하지 제대로 명명 됩니다.

<a name="Summary" />

## <a name="summary"></a>요약

바인딩된 Api에 대 한 의미 매개 변수 이름을 제공 Java 바인딩 프로젝트에서 Javadoc를 사용 하는 방법을 설명 하는이 문서. 

