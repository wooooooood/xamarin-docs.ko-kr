---
title: IOS 라이브러리 바인딩
description: 이 문서에서는 CocoaPods 응용 프로그램 C# 에서 네이티브 라이브러리 및를 사용할 수 있도록 목적-C 코드에 대 한 바인딩을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 61d1adfc997b34302f1f89f56653af906ca90135
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022218"
---
# <a name="binding-ios-libraries"></a>IOS 라이브러리 바인딩

다음 링크를 따라 CocoaPods 및 Xamarin.ios에 대 한 바인딩 목표-C 라이브러리 및에 대해 알아보세요.

- [**개요**](~/cross-platform/macios/binding/overview.md) -
  바인딩의 작동 방식을 설명 합니다.
- [**바인딩 목표-c 라이브러리**](~/cross-platform/macios/binding/objective-c-libraries.md) 는 Xamarin 프로젝트에서 사용할 목적-c 라이브러리를 바인딩하는 방법에 대 한 지침을 -
  합니다.
- [**형식 정의 참조 가이드**](~/cross-platform/macios/binding/binding-types-reference.md) -
  바인딩 생성 프로세스를 구동 하기 위해 바인딩 작성자가 사용할 수 있는 모든 특성을 설명 합니다.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

목표 Sharpie는 첫 번째 바인딩 패스를 부트스트랩 하는 데 도움이 되는 명령줄 도구입니다.
네이티브 라이브러리의 헤더 파일을 구문 분석 하 여 공용 API를 [바인딩 정의](~/cross-platform/macios/binding/objective-c-libraries.md) 에 매핑합니다 (수동으로 수행 하는 프로세스). 목표 Sharpie 자체 바인딩을 만들지 않지만 시작 하는 데 도움이 될 수 있습니다.

목표 Sharpie 3.0에는 Cocoapods를 직접 바인딩하는 기능이 도입 되었습니다.

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[연습-iOS 목표-C 라이브러리 바인딩](walkthrough.md)

이 페이지에서는 예를 들어 오픈 소스 [**Infcolorpicker**](https://github.com/InfinitApps/InfColorPicker) 목표-C 프로젝트를 사용 하 여 iOS 바인딩 프로젝트를 만드는 단계별 연습을 제공 합니다. **Infcolorpicker** 라이브러리는 다시 사용할 수 있는 뷰 컨트롤러를 제공 합니다 .이를 통해 사용자는이를 통해 HSB 표시를 기준으로 색을 선택 하 여 사용자에 게 더 쉽게 색을 선택할 수 있습니다.
목표 Sharpie는 바인딩 프로세스를 지 원하는 데 사용 됩니다.

## <a name="video"></a>비디오

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**C/C++ 비디오의 iOS 바인딩**

## <a name="related-links"></a>관련 링크

- [Objective-C 바인딩](~/cross-platform/macios/binding/index.md)
- [Mac 바인딩](~/mac/platform/binding.md)
