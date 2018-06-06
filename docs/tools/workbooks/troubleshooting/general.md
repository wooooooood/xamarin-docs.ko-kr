---
title: 알려진된 문제 및 해결 방법
description: 이 문서에서는 Xamarin 통합 문서에 대해 알려진된 문제 및 해결 방법을 설명 합니다. CultureInfo 문제, JSON 문제 등을 설명합니다.
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: topgenorth
ms.author: toopge
ms.openlocfilehash: b6dc3b119d3e85369a71638f2519b2ef0c85446c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794035"
---
# <a name="known-issues--workarounds"></a>알려진된 문제 및 해결 방법

## <a name="persistence-of-cultureinfo-across-cells"></a>여러 CultureInfo의 지 속성

설정 `System.Threading.CurrentThread.CurrentCulture` 또는 `System.Globalization.CultureInfo.CurrentCulture` 로 인해 모노 기반 통합 문서 대상 (Mac, iOS 및 Android)에서 통합 문서는 여러 지속 되지 않는 한 [모노의의 버그 `AppContext.SetSwitch` ] [ appcontext-bug] 구현 .

### <a name="workarounds"></a>해결 방법

* 응용 프로그램 도메인 로컬 설정 `DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* 또는 통합 문서로 1.2.1 업데이트 이후 할당을 다시 작성 하는 `System.Threading.CurrentThread.CurrentCulture` 및 `System.Globalization.CultureInfo.CurrentCulture` (모노 버그 중심 작업) 원하는 동작에 대 한 제공 하기.

## <a name="unable-to-use-newtonsoftjson"></a>Newtonsoft.Json을 사용할 수 없습니다.

### <a name="workaround"></a>해결 방법

* 통합 문서에 1.2.1, 9.0.1 Newtonsoft.Json을 설치 하는 업데이트 합니다.
  1.3, 통합 문서는 알파 채널에 현재 버전 10 이상이 지원 합니다.

### <a name="details"></a>설명

Newtonsoft.Json 10 지원 하기 위해 함께 제공 되는 버전의 통합 문서와 충돌 하는 Microsoft.CSharp에 종속 되기 추락는 릴리스된 `dynamic`합니다. 통합 문서 1.3 미리 보기 릴리스에서이 문제를 다뤘습니다 하지만 지금은 왔습니다이 고정 Newtonsoft.Json 9.0.1 버전을 구체적으로.

Newtonsoft.Json 10에 따라 명시적으로 또는 최신 NuGet 패키지는 에서만 지원 통합 문서 1.3에서 현재 알파 채널입니다.

## <a name="code-tooltips-are-blank"></a>도구 설명 코드는 빈 값

한 [Monaco 편집기의 버그] [ monaco-bug] 텍스트 없이 코드 도구 설명 렌더링에 발생 하는 Mac 통합 앱에서 사용 되는 Safari/WebKit에서 합니다.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>해결 방법

* 표시 되 면 도구 설명을 클릭할 렌더링할 텍스트를 강제로 됩니다.

* 1.2.1 통합 문서에 업데이트 또는 이상 버전

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>통합 문서 1.3에서 누락 된 SkiaSharp 렌더러는

통합 문서 1.3 부터는 없어졌습니다 처음 출시 된 SkiaSharp 렌더러 SkiaSharp 사용 하 여에 자체는 렌더러를 제공 하기 위해 0.99.0, 통합 문서에서 우리의 [SDK](~/tools/workbooks/sdk/index.md)합니다.

### <a name="workaround"></a>해결 방법

* SkiaSharp NuGet에서 최신 버전으로 업데이트 합니다. 작성 시점에 1.57.1입니다.

## <a name="related-links"></a>관련 링크

- [버그를 보고](~/tools/workbooks/install.md#reporting-bugs)
