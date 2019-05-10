---
title: IOS 라이브러리 바인딩
description: 이 문서를 만드는 방법에 설명 합니다 C# 에 대 한 바인딩을 Objective-c 코드를 Xamarin.iOS 응용 프로그램에서 네이티브 라이브러리 및 CocoaPods를 사용할 수 있도록 합니다.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 8c4dcbe0baf74479e94f8663280e7654b4d58a9d
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978161"
---
# <a name="binding-ios-libraries"></a>IOS 라이브러리 바인딩

Xamarin.iOS 및 Xamarin.Mac CocoaPods 및 Objective-c 라이브러리 바인딩 방법을 알아보려면 다음이 링크를 수행 합니다.

- [**개요** ](~/cross-platform/macios/binding/overview.md) -
  바인딩이 작동 하는 방법에 대해 설명 합니다.
- [**Objective-c 라이브러리 바인딩** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Xamarin 프로젝트에서 사용 하기 위한 Objective-c 라이브러리를 바인딩하는 방법에 대 한 지침입니다.
- [**입력 정의 참조 가이드** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  모든 바인딩 생성 프로세스를 추진 하는 바인딩 작성자가 사용할 수 있는 특성에 설명 합니다.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

목표 Sharpie는 바인딩의 첫 번째 패스를 부트스트랩 하는 데는 명령줄 도구입니다.
공용 API에 매핑할 네이티브 라이브러리의 헤더 파일을 구문 분석 하 여 작동 합니다 [바인딩 정의](~/cross-platform/macios/binding/objective-c-libraries.md) (그렇지 않은 경우 수동으로 수행 되는 프로세스). 목표 Sharpie 자체적으로 바인딩을 만들지 않습니다 하지만 시작 하는 데 도움이!

목표 Sharpie 3.0 Cocoapods를 직접 바인딩하는 기능을 도입 되었습니다!

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[연습-iOS Objective-c 라이브러리 바인딩](walkthrough.md)

이 페이지에서는 오픈 소스를 사용 하 여 iOS 바인딩 프로젝트 만들기의 단계별 연습은 [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) 예로 Objective C 프로젝트입니다. 합니다 **InfColorPicker** 라이브러리는 사용자가 색 선택 영역을 보다 친숙 한 만들기, HSB 표현에 따라 색을 선택할 수 있는 재사용 가능한 뷰 컨트롤러를 제공 합니다.
목표 Sharpie 바인딩 프로세스를 지원 하기 위해 사용 됩니다.

## <a name="video"></a>비디오

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**iOS 바인딩 c에서 /C++ 비디오**

## <a name="related-links"></a>관련 링크

- [Objective-C 바인딩](~/cross-platform/macios/binding/index.md)
- [Mac 바인딩](~/mac/platform/binding.md)
