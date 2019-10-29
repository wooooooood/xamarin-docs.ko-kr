---
title: 목표 Sharpie를 사용 하 여 바인딩 만들기
description: 이 섹션에서는 목적 Sharpie 라이브러리에 대 한 바인딩을 만드는 프로세스를 자동화 하는 데 사용 되는 Xamarin의 명령줄 도구인 목표를 소개 합니다.
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
author: davidortinau
ms.author: daortin
ms.date: 10/11/2017
ms.openlocfilehash: 4d6ab6cf48c5c365a4d8d05ef108a4d3a5d16134
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016187"
---
# <a name="creating-bindings-with-objective-sharpie"></a>목표 Sharpie를 사용 하 여 바인딩 만들기

_이 섹션에서는 목적 Sharpie 라이브러리에 대 한 바인딩을 만드는 프로세스를 자동화 하는 데 사용 되는 Xamarin의 명령줄 도구인 목표를 소개 합니다._

- [개요](#overview) & [기록](#history)
- [시작](get-started.md)
- [도구 및 명령](tools.md)
- [기능](platform/index.md)
- [예제](examples/index.md)
- [연습 완료](~/ios/platform/binding-objective-c/walkthrough.md)
- [릴리스 기록](releases.md)

## <a name="overview"></a>개요

목표 Sharpie는 첫 번째 바인딩 패스를 부트스트랩 하는 데 도움이 되는 명령줄 도구입니다.
네이티브 라이브러리의 헤더 파일을 구문 분석 하 여 공용 API를 [바인딩 정의](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) 에 매핑합니다 (이전에 수동으로 수행한 프로세스).

목표 Sharpie는 Clang 구문 분석 헤더 파일을 사용 하므로 바인딩은 정확히 정확 하 고 철저 하 게 사용할 수 있습니다. 그러면 품질 바인딩을 생성 하는 데 소요 되는 시간과 노력을 크게 줄일 수 있습니다.

> [!IMPORTANT]
> 목표 Sharpie는 목표에 대 한 고급 지식을 갖춘 숙련 된 Xamarin 개발자를 위한 도구입니다 (및 확장, C). 목표-C 라이브러리를 바인딩하려면 먼저 명령줄에서 네이티브 라이브러리를 빌드하는 방법에 대 한 확실 한 지식이 있어야 합니다 (그리고 네이티브 라이브러리가 작동 하는 방식을 이해 하는 것이 좋습니다).

## <a name="history"></a>기록

지난 3 년간 Xamarin에서 내부적으로 Sharpie 목표를 달성 하 고 노력 했습니다. Sharpie의 결집에 대 한 자세한 내용은 iOS 8, Mac OS X 10.10 및 watchOS 2.0 이후로 Xamarin.ios 및 Xamarin.ios에 도입 된 Api는 전적으로 목표 Sharpie를 사용 하 여 부트스트랩 되었습니다. Xamarin은 자체 제품을 빌드하기 위해 내부적으로 목표 Sharpie을 많이 사용 합니다.

그러나 목표 Sharpie는 목표에 대 한 고급 지식이 필요 하 고, 명령줄에서 clang 컴파일러를 사용 하는 방법, 일반적으로 네이티브 라이브러리를 함께 배치 하는 방법 등의 고급 도구입니다. 이 고급 막대 때문에 GUI 마법사를 사용 하 여 잘못 된 기대치를 설정 하는 것으로 생각 하는 것 처럼 목표 Sharpie는 현재 명령줄 도구로만 제공 됩니다.

## <a name="related-links"></a>관련 링크

- [목표 Sharpie 다운로드](https://aka.ms/objective-sharpie)
- [연습: 목표-C 라이브러리 바인딩](~/ios/platform/binding-objective-c/walkthrough.md)
- [Objective-C 라이브러리 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md)
- [바인딩 세부 정보](~/cross-platform/macios/binding/overview.md)
- [바인딩 형식 참조 가이드](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C 개발자용 Xamarin](~/ios/get-started/objective-c-developers/index.md)
