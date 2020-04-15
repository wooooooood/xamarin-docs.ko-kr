---
title: Javadoc을 사용하여 매개 변수 이름 지정
description: 이 문서에서는 Java 프로젝트에서 생성된 Javadoc을 사용하여 Java 바인딩 프로젝트에서 매개 변수 이름을 복구하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/20/2017
ms.openlocfilehash: 060c4759d39bc3b8c424ce46dc615644540fe9c2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027674"
---
# <a name="naming-parameters-with-javadoc"></a>Javadoc을 사용하여 매개 변수 이름 지정

_이 문서에서는 Java 프로젝트에서 생성된 Javadoc을 사용하여 Java 바인딩 프로젝트에서 매개 변수 이름을 복구하는 방법을 설명합니다._

## <a name="overview"></a>개요

기존 Java 라이브러리를 바인딩하는 경우 바인딩된 API에 대한 일부 메타데이터가 손실됩니다. 특히 메서드에 대한 매개 변수 이름이 손실될 수 있습니다. 매개 변수 이름은 `p0`, `p1` 등으로 표시됩니다. Java `.class` 파일이 Java 소스 코드에 사용된 매개 변수 이름을 유지하지 않기 때문입니다. 

원본 라이브러리에서 Javadoc HTML에 대한 액세스 권한이 있는 경우 Xamarin.Android Java 바인딩 프로젝트가 매개 변수 이름을 제공할 수 있습니다. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Javadoc HTML을 Java 바인딩 프로젝트에 통합

Javadoc HTML을 Java 바인딩 프로젝트에 통합하는 작업은 다음 단계로 구성되는 수동 프로세스입니다. 

1. 라이브러리에 대한 Javadoc 다운로드
2. `.csproj` 파일을 편집하고 `<JavaDocPaths>` 속성을 추가합니다.
3. 프로젝트 정리 및 다시 빌드

이 작업을 완료한 후에는 Java 바인딩 프로젝트에 의해 바인딩된 API에 원래 Java 매개 변수 이름이 있어야 합니다. 

> [!NOTE]
> JavaDoc 출력에는 상당한 차이가 있습니다. .JAR 바인딩 도구 체인은 가능한 모든 단일 순열을 지원하지 않으므로 일부 매개 변수의 이름이 제대로 지정되지 않을 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 Java 바인딩 프로젝트에서 Javadoc를 사용하여 바인딩된 API에 대해 의미 있는 매개 변수 이름을 제공하는 방법을 설명했습니다. 
