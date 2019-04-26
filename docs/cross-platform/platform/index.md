---
title: Xamarin의 언어 지원 프로그래밍
description: 이 문서에서는 Xamarin을 지 원하는 다양 한 프로그래밍 언어를 설명 합니다. 설명 C#, F#, 이식 가능한 Visual Basic.NET 및 Razor 템플릿.
ms.prod: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 715f63a0be54ba3342bd63c1c76d89656313359a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61035913"
---
# <a name="programming-language-support-in-xamarin"></a>Xamarin의 언어 지원 프로그래밍

## <a name="c"></a>C# 

###  <a name="async-support-overviewcross-platformplatformasyncmd"></a>[비동기 지원 개요](~/cross-platform/platform/async.md)

버전 5 C# 비동기 작업을 표현 하는 두 가지 새 키워드를 도입: async 및 await를 합니다. 이러한 키워드를 사용 하 여 다른 스레드에서 장기 실행 작업 (예: 네트워크 액세스)를 실행 하려면 작업 병렬 라이브러리를 활용 하는 간단한 코드를 작성 하 고 완료 시 결과 쉽게 액세스할 수 있습니다. 최신 버전의 Xamarin.iOS 및 Xamarin.Android 지원 async 및 await-이 문서에서는 설명 및 Xamarin을 사용 하 여 새 구문을 사용 하는 예제를 제공 합니다.

### <a name="c-6-language-featurescross-platformplatformcsharp-sixmd"></a>[C# 6 언어 기능](~/cross-platform/platform/csharp-six.md)

최신 버전의를 C# – 버전 6 – 언어 상용구를 덜, 향상 된 이해를 돕기 위해 자세한 일관성 있고 언어 진화 계속 합니다. 클리너 초기화 구문, 사용할 수 있다는 `await` 에 `catch/finally` 블록과 null 조건부 `?` 연산자는 특히 유용 합니다.

## <a name="ffsharpindexmd"></a>[F#](fsharp/index.md)

모바일 앱 빌드 F# 및 Xamarin입니다.

##  <a name="portable-visual-basicnetcross-platformplatformvisual-basicindexmd"></a>[이식 가능한 Visual Basic.NET](~/cross-platform/platform/visual-basic/index.md)

Visual Studio에서는 다음 Xamarin 응용 프로그램에 통합할 수 있는 Visual Basic.NET을 사용 하 여 이식 가능한 클래스 라이브러리 만들기를 지원 합니다. 이 문서에는 새 Visual Basic PCL을 만들고 샘플 Xamarin.iOS, Xamarin.Android 및 Windows Phone 응용 프로그램에 사용 하는 방법을 보여 줍니다.

##  <a name="building-html-views-using-razor-templatescross-platformplatformrazor-html-templatesindexmd"></a>[Razor 템플릿을 사용 하 여 빌드 HTML 보기](~/cross-platform/platform/razor-html-templates/index.md)

Xamarin을 통해 개발자와 함께 ASP.NET MVC를 사용 하 여 처음 도입 된 Razor 템플릿 엔진을 활용 하 여 C# 수동으로 코드에서 HTML 문자열을 작성 하는 번거로움 없이 HTML, Javascript 및 CSS를 사용 하 여 데이터를 쉽게 결합할 수 있습니다.
이 문서에서는 Android 및 iOS 용 Xamarin을 사용 하 여 Razor 템플릿을 사용 하는 방법에 설명 합니다.
