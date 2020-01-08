---
title: Objective-C 바인딩
description: 이 문서는 목표-C 코드에 대 한 바인딩을 만드는 C# 방법을 설명 하는 다양 한 가이드의 링크를 제공 하 여 개발자가 Xamarin 응용 프로그램에서 선반 드 라이브러리를 사용할 수 있도록 합니다.
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: davidortinau
ms.author: daortin
ms.date: 01/25/2016
ms.openlocfilehash: b7764d63991ec636043982509319e7097ef2091b
ms.sourcegitcommit: d8af612b6b3218fea396d2f180e92071c4d4bf92
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75663257"
---
# <a name="binding-objective-c"></a>Objective-C 바인딩

이 섹션에는 목표-C 라이브러리에 대 한 바인딩을 만드는 과정을 다루는 여러 문서가 포함 되어 있으므로 Xamarin.ios 또는 C# xamarin.ios를 사용 하 여 만든 응용 프로그램에서 호출할 수 있습니다.

## <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[개요](~/cross-platform/macios/binding/overview.md)

이 문서에는 바인딩이 발생 하는 방법에 대 한 일부 내부 정보가 포함 되어 있습니다. 몇 가지 기술 정보가 포함 된 고급 문서입니다.

## <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Objective-C 라이브러리 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md)

이 문서에서는 관용구 Api의 바인딩을 만드는 C# 데 사용 되는 프로세스와 목적-c의이 .net에서 사용 되는 관용구에 매핑되는 방법에 대해 설명 합니다.
C Api만 바인딩하는 경우에는 P/Invoke 프레임 워크로 표준 .NET 메커니즘을 사용 해야 합니다.

## <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[바인딩 정의 참조 가이드](~/cross-platform/macios/binding/binding-types-reference.md)

바인딩 생성 프로세스를 구동 하기 위해 바인딩 작성자가 사용할 수 있는 모든 특성을 설명 하는 참조 가이드입니다.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

목표 Sharpie는 첫 번째 바인딩 패스를 부트스트랩 하는 데 도움이 되는 명령줄 도구입니다. 네이티브 라이브러리의 헤더 파일을 구문 분석 하 여 공용 API를 [바인딩 정의](~/cross-platform/macios/binding/objective-c-libraries.md) (수동으로 수행할 수도 있는 프로세스)에 매핑하는 방식으로 작동 합니다.

## <a name="ios"></a>iOS

[IOS 바인딩 페이지](~/ios/platform/binding-objective-c/index.md) 는 아래 예제와 함께 이러한 일반적인 바인딩 리소스에 다시 연결 됩니다.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[연습: 목표-C 라이브러리 바인딩](~/ios/platform/binding-objective-c/walkthrough.md)

이 문서에서는 예를 들어 오픈 소스 [Infcolorpicker](https://github.com/InfinitApps/InfColorPicker) 목표-C 프로젝트를 사용 하 여 바인딩 프로젝트를 만드는 단계별 연습을 제공 합니다. InfColorPicker 라이브러리는 다시 사용할 수 있는 뷰 컨트롤러를 제공 합니다 .이를 통해 사용자는이를 통해 HSB 표시를 기준으로 색을 선택 하 여 사용자에 게 더 쉽게 색을 선택할 수 있습니다. 목표 Sharpie는 바인딩 프로세스를 지 원하는 데 사용 됩니다.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[바인딩 샘플](https://github.com/mono/monotouch-bindings)

새 바인딩 프로젝트를 만들 때 참조로 사용할 수 있는 타사 바인딩의 컬렉션입니다.

## <a name="mac"></a>Mac

[Mac 바인딩](~/mac/platform/binding.md) 지침에 따라 macos 라이브러리를 바인딩합니다. **새 프로젝트** 창에서 새 **Mac 바인딩 라이브러리** 를 만들 수 있습니다.

[![파일 새 mac 바인딩 프로젝트 대화 상자](images/new-bindings-library-sml.png)](images/new-bindings-library.png#lightbox)

## <a name="related-links"></a>관련 링크

- [iOS 바인딩](~/ios/platform/binding-objective-c/index.md)
- [Mac 바인딩](~/mac/platform/binding.md)
