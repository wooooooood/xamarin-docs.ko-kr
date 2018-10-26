---
title: Javadoc 사용 하 여 매개 변수 이름 지정
description: 이 문서에서는 Java 프로젝트에서 생성 되는 Javadoc을 사용 하 여 Java 바인딩 프로젝트의 매개 변수 이름은 복구 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/20/2017
ms.openlocfilehash: e394377043953a297afed36a3ce0747a3e6d1512
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104416"
---
# <a name="naming-parameters-with-javadoc"></a>Javadoc 사용 하 여 매개 변수 이름 지정

_이 문서에서는 Java 프로젝트에서 생성 되는 Javadoc을 사용 하 여 Java 바인딩 프로젝트의 매개 변수 이름은 복구 하는 방법에 설명 합니다._


## <a name="overview"></a>개요

기존 Java 라이브러리를 바인딩하는 경우 바인딩된 API에 대 한 일부 메타 데이터는 손실 됩니다. 특히 메서드 매개 변수의 이름입니다. 매개 변수 이름으로 표시 됩니다 `p0`, `p1`등입니다. 왜냐하면 Java `.class` 파일 Java 소스 코드에 사용 된 매개 변수 이름을 유지 하지 않습니다. 

Xamarin.Android Java 바인딩 프로젝트를 원본 라이브러리의 Javadoc HTML로 액세스할 수 있는 경우 매개 변수 이름을 제공할 수 있습니다. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Javadoc HTML 프로젝트 바인딩 Java 통합

Java 바인딩 프로젝트 Javadoc HTML 통합 다음 단계를 구성 하는 수동 프로세스입니다. 

1.  라이브러리에 대 한 Javadoc 다운로드
2.  편집 된 `.csproj` 파일과 추가 `<JavaDocPaths>` 속성:
3.  정리 하 고 프로젝트를 다시 작성

이 작업이 완료 되 면 원래 Java 매개 변수 이름은 Java 바인딩 프로젝트에 의해 바인딩된 Api에 있어야 합니다. 


> [!NOTE]
> JavaDoc 출력에 대 한 분산 많은 방법이 있습니다. 합니다. JAR 바인딩 도구 체인은 단일 모든 가능한 순열을 지원 하지 않습니다 및 결과적으로 일부 매개 변수 수 올바르게 이름을 지정 합니다.


## <a name="summary"></a>요약

이 문서는 방법을 설명 바인딩된 Api에 대 한 매개 변수 이름을 의미 Java 바인딩 프로젝트에서 Javadoc을 사용 합니다. 

