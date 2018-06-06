---
title: 목표 Sharpie를 사용 하 여 바인딩 만들기
description: 이 섹션에서는 목표 Sharpie, Objective C 라이브러리에 대 한 바인딩을 만드는 과정을 자동화 하는 데 Xamarin의 명령줄 도구를 소개
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: ae644038aa8b54f0d57b61767882dec8754040c8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780759"
---
# <a name="creating-bindings-with-objective-sharpie"></a>목표 Sharpie를 사용 하 여 바인딩 만들기

_이 섹션에서는 목표 Sharpie, Objective C 라이브러리에 대 한 바인딩을 만드는 과정을 자동화 하는 데 Xamarin의 명령줄 도구를 소개_

- [개요](#overview) & [기록](#history)
- [시작](get-started.md)
- [도구 및 명령](tools.md)
- [기능](platform/index.md)
- [예제](examples/index.md)
- [연습을 완료](~/ios/platform/binding-objective-c/walkthrough.md)
- [릴리스 기록](releases.md)

## <a name="overview"></a>개요

목표 Sharpie는 바인딩 첫 번째 패스를 부트스트랩 하려면 명령줄 도구입니다.
공용 API에 매핑할 네이티브 라이브러리의 헤더 파일을 구문 분석 하 여 작동는 [바인딩 정의](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) (이전에 수동으로 수행한 프로세스).

목표 Sharpie 바인딩이으로 정확 하 게 하 고 가능한 철저 한 이므로 Clang 구문 분석 헤더 파일을 사용 합니다. 이렇게 품질 바인딩을 생성 하는 데 걸리는 시간과 크게 줄일 수 있습니다.

> [!IMPORTANT]
> 목표 Sharpie는 고급 정보 Objective C의 (및 확장명, C)로 숙련 된 Xamarin 개발자를 위한 도구입니다. Objective C 라이브러리를 바인딩하는 명령줄 (및 네이티브 라이브러리의 작동 방식을 이해)에서 네이티브 라이브러리를 작성 하는 방법 확실히 알고가 있어야 합니다.

## <a name="history"></a>기록

된 진화 했으며에서 사용 하 여 목표 Sharpie 내부적으로 Xamarin 지난 3 년에 대 한 합니다. 목표 Sharpie의 척도가, iOS 8, Mac OS X 10.10 이후 Xamarin.iOS 및 Xamarin.Mac에 도입 된 Api 및 2.0 watchOS 목표 Sharpie로 완전 하 게 부트스트랩 되었습니다. Xamarin에 크게 의존 목표 Sharpie 내부적으로 자체 제품 빌드를 위한 합니다.

그러나 목표 Sharpie는 Objective C 및 C, 명령줄에 clang 컴파일러를 사용 하는 방법 및 방법을 일반적으로 네이티브 라이브러리 배치 된 고급 지식을 요구 하는 고급 도구입니다. 이 높은 표시줄 때문에 더 쉬워집니다 해당 필요 GUI 잘못 기대치를 설정 하는 마법사 및 이와 같이 목표 Sharpie은 현재 으로만 사용할 수 있는 명령줄 도구입니다.

## <a name="related-links"></a>관련 링크

- [목표 Sharpie 다운로드](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
- [연습: 바인딩 Objective C 라이브러리](~/ios/platform/binding-objective-c/walkthrough.md)
- [Objective-C 라이브러리 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md)
- [바인딩 세부 정보](~/cross-platform/macios/binding/overview.md)
- [바인딩 형식에 대 한 가이드](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C 개발자용 Xamarin](~/ios/get-started/objective-c-developers/index.md)
