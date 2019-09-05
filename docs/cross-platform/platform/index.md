---
title: Xamarin의 프로그래밍 언어 지원
description: 이 문서에서는 Xamarin에서 지 원하는 다양 한 프로그래밍 언어에 대해 설명 합니다. , F#, C#이식 가능한 Visual Basic.NET 및 Razor 템플릿에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
author: conceptdev
ms.author: crdun
ms.date: 02/18/2018
ms.openlocfilehash: 95c63d446f961738ad71242671d632620316b879
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290896"
---
# <a name="programming-language-support-in-xamarin"></a>Xamarin의 프로그래밍 언어 지원

## <a name="c"></a>C\#

### <a name="async-support-overviewcross-platformplatformasyncmd"></a>[비동기 지원 개요](~/cross-platform/platform/async.md)

의 버전 5 C# 에는 비동기 작업을 표현 하기 위한 두 개의 새 키워드인 비동기 및 대기가 도입 되었습니다. 이러한 키워드를 사용 하 여 작업 병렬 라이브러리를 활용 하는 간단한 코드를 작성 하 여 다른 스레드에서 장기 실행 작업 (예: 네트워크 액세스)을 실행 하 고 완료 시 결과에 쉽게 액세스할 수 있습니다. 최신 버전의 Xamarin.ios 및 Xamarin Android는 async 및 wait를 지원 합니다 .이 문서에서는 Xamarin과 함께 새 구문을 사용 하는 방법에 대 한 설명과 예제를 제공 합니다.

### <a name="c-6-language-featurescross-platformplatformcsharp-sixmd"></a>[C# 6 언어 기능](~/cross-platform/platform/csharp-six.md)

최신 버전의 C# 언어 – 버전 6은 계속 해 서 상용구를 줄이고, 명확성을 개선 하 고, 일관성을 유지할 수 있도록 언어를 계속 발전 시킵니다. 클리너 초기화 구문, 블록에서 `await` `catch/finally` 사용 하는 기능 및 null 조건 `?` 연산자가 특히 유용 합니다.

## <a name="ffsharpindexmd"></a>[F#](fsharp/index.md)

및 Xamarin을 사용 F# 하 여 모바일 앱을 빌드합니다.

## <a name="portable-visual-basicnetcross-platformplatformvisual-basicindexmd"></a>[이식 가능한 Visual Basic.NET](~/cross-platform/platform/visual-basic/index.md)

Visual Studio는 Xamarin 응용 프로그램에 통합 될 수 있는 Visual Basic.NET를 사용 하 여 이식 가능한 클래스 라이브러리를 만들 수 있도록 지원 합니다. 이 문서에서는 새 Visual Basic PCL을 만든 다음 샘플 Xamarin.ios, Xamarin Android 및 Windows Phone 응용 프로그램에서 사용 하는 방법을 보여 줍니다.

## <a name="building-html-views-using-razor-templatescross-platformplatformrazor-html-templatesindexmd"></a>[Razor 템플릿을 사용 하 여 HTML 뷰 빌드](~/cross-platform/platform/razor-html-templates/index.md)

Xamarin을 사용 하면 개발자는 코드에서 HTML 문자열을 수동으로 빌드하는 번거로움을 제외 하 고 C# 를 사용 하 여 원래 ASP.NET MVC와 함께 제공 되는 Razor 템플릿 엔진을 활용 하 여 Html, JAVASCRIPT 및 CSS와 데이터를 쉽게 결합할 수 있습니다.
이 문서에서는 Android 및 iOS 용 Xamarin에서 Razor 템플릿을 사용 하는 방법을 보여 줍니다.
