---
title: 알려진된 문제 및 해결 방법
description: 이 문서에서는 Xamarin Workbooks에 대 한 알려진된 문제 및 해결 방법을 설명 합니다. CultureInfo 문제, JSON 문제 및 자세히 설명합니다.
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 221ed97db17da62f513448b6c85d4df205a7cbaf
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110315"
---
# <a name="known-issues--workarounds"></a>알려진된 문제 및 해결 방법

## <a name="persistence-of-cultureinfo-across-cells"></a>셀에서 CultureInfo의 지 속성

설정 `System.Threading.CurrentThread.CurrentCulture` 나 `System.Globalization.CultureInfo.CurrentCulture` 때문에 Mono 기반 통합 문서 대상 (Mac, iOS 및 Android)에서 통합 문서 셀 간에 지속 되지 않습니다는 [Mono의 버그 `AppContext.SetSwitch` ] [ appcontext-bug] 구현 .

### <a name="workarounds"></a>해결 방법

* 응용 프로그램-도메인 로컬 설정 `DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* 또는 통합 문서로 1.2.1 업데이트 이상을 할당을 다시 작성 됩니다는 `System.Threading.CurrentThread.CurrentCulture` 및 `System.Globalization.CultureInfo.CurrentCulture` (Mono 버그 해결 방법) 하 여 원하는 동작에 대 한 제공 합니다.

## <a name="unable-to-use-newtonsoftjson"></a>Newtonsoft.Json을 사용할 수 없습니다.

### <a name="workaround"></a>해결 방법

* 통합 문서에 업데이트 1.2.1, 그러면 Newtonsoft.Json 9.0.1을 설치 합니다.
  통합 문서 1.3, 알파 채널에서 현재 버전 10 이상 지원 합니다.

### <a name="details"></a>설명

Newtonsoft.Json 10 지원 하기 위해 제공 되는 버전의 통합 문서와 충돌 하는 Microsoft.CSharp에 종속 되는 릴리스된 `dynamic`합니다. 통합 문서 1.3 미리 보기 릴리스에서이 문제를 다뤘습니다 있지만 지금은 노력이 문제를 해결 하 여 고정 Newtonsoft.Json 9.0.1 버전에 맞게 합니다.

Newtonsoft.Json 10에 따라 명시적으로 또는 최신 NuGet 패키지는 에서만 지원 통합 문서 1.3, 현재는 알파 채널입니다.

## <a name="code-tooltips-are-blank"></a>코드 설명에 빈 값은

한 [(monaco) 편집기의 버그] [ monaco-bug] 결과는 텍스트 없이 코드 도구 설명 렌더링에 사용 되는 Mac 통합 앱에서 Safari/WebKit에서.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>해결 방법

* 도구 설명이 표시 되 면 클릭 렌더링할 텍스트를 강제로 됩니다.

* 또는 통합 문서로 1.2.1 업데이트 이상

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>SkiaSharp 렌더러는 통합 문서 1.3에서 누락

통합 문서 1.3부터 없어졌습니다 이죠 SkiaSharp 렌더러를 위해 제공 된 렌더러를 자체를 사용 하 여 SkiaSharp 0.99.0, 통합 문서에서 우리의 [SDK](~/tools/workbooks/sdk/index.md)합니다.

### <a name="workaround"></a>해결 방법

* SkiaSharp NuGet에서 최신 버전으로 업데이트 합니다. 작성 당시 1.57.1입니다.

## <a name="related-links"></a>관련 링크

- [버그 보고](~/tools/workbooks/install.md#reporting-bugs)
