---
title: 목표 Sharpie를 사용 하 여 바인딩 만들기
description: 이 섹션에서는 목표 Sharpie, Objective-c 라이브러리 바인딩 만들기 프로세스를 자동화 하는 데 Xamarin의 명령줄 도구를 소개
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 53fcbbc408ae147405a3285d9391457051d6e16e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261203"
---
# <a name="creating-bindings-with-objective-sharpie"></a>목표 Sharpie를 사용 하 여 바인딩 만들기

_이 섹션에서는 목표 Sharpie, Objective-c 라이브러리 바인딩 만들기 프로세스를 자동화 하는 데 Xamarin의 명령줄 도구를 소개_

- [개요](#overview) & [기록](#history)
- [시작](get-started.md)
- [도구 및 명령](tools.md)
- [기능](platform/index.md)
- [예제](examples/index.md)
- [연습을 완료](~/ios/platform/binding-objective-c/walkthrough.md)
- [릴리스 기록](releases.md)

## <a name="overview"></a>개요

목표 Sharpie는 바인딩의 첫 번째 패스를 부트스트랩 하는 데는 명령줄 도구입니다.
공용 API에 매핑할 네이티브 라이브러리의 헤더 파일을 구문 분석 하 여 작동 합니다 [바인딩 정의](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) (이전에 수동으로 수행한 프로세스).

목표 Sharpie 바인딩을으로 정확 하 게 하 고 최대한 철저 한 이므로 헤더 파일을 구문 분석 하는 Clang을 사용 합니다. 이 품질 바인딩을 생성 하는 데 걸리는 시간과 크게 줄일 수 있습니다.

> [!IMPORTANT]
> 목표 Sharpie는 고급 기술 자료의 Objective C (및 확장명, C)를 사용 하 여 숙련 된 Xamarin 개발자를 위한 도구. Objective-c 라이브러리 바인딩 하기 전에 명령줄 (및 네이티브 라이브러리의 작동 방식을 이해)에서 네이티브 라이브러리를 빌드하는 방법 확실히 알고가 있어야 합니다.

## <a name="history"></a>기록

된 진화 하 고 사용 하 여 목표 Sharpie 내부적으로 Xamarin에서 지난 3 년에 대 한 합니다. 목표 Sharpie 활용, 어차피 iOS 8, Mac OS X 10.10부터 Xamarin.iOS 및 Xamarin.Mac에 도입 된 Api 및 watchOS 2.0 목표 Sharpie 전체적으로 부트스트랩 된 합니다. Xamarin에 크게 의존 목표 Sharpie 내부적으로 자체 제품을 구축 하기 위한

그러나 목표 Sharpie Objective-c 및 C, clang 컴파일러 명령줄에서 사용 하는 방법 및 결합 하는 방법을 일반적으로 네이티브 라이브러리는에 대 한 고급 지식이 필요한 고급 도구 이며 이 높은 표시줄 때문에 생각 했던 GUI 있으면 해당 잘못 기대치를 설정 하는 마법사 및 이와 같이 목표 Sharpie 전용인 현재 명령줄 도구를 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [목표 Sharpie 다운로드](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
- [연습: Objective-c 라이브러리 바인딩](~/ios/platform/binding-objective-c/walkthrough.md)
- [Objective-C 라이브러리 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md)
- [바인딩 세부 정보](~/cross-platform/macios/binding/overview.md)
- [바인딩 유형 참조 가이드](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C 개발자용 Xamarin](~/ios/get-started/objective-c-developers/index.md)
- [Xamarin University 과정: Objective-c 바인딩 라이브러리를 빌드](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University 과정: 목표 Sharpie로는 Objective-c 바인딩 라이브러리 빌드](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
