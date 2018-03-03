---
title: "IOS 라이브러리 바인딩"
description: "IOS 네이티브 라이브러리 (및 CocoaPods) 하는 방법 Xamarin 앱에 액세스할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3afe1a03299e600502d49b1db039af4c6642e131
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="binding-ios-libraries"></a>IOS 라이브러리 바인딩

_IOS 네이티브 라이브러리 (및 CocoaPods) 하는 방법 Xamarin 앱에 액세스할 수 있습니다._

Objective C 라이브러리 및 CocoaPods Xamarin.iOS 및 Xamarin.Mac에 대 한 바인딩에 대 한 자세한 내용은 다음 링크:

- [**개요** ](~/cross-platform/macios/binding/overview.md) -
  바인딩 작동 하는 방법을 설명 합니다.
- [**Objective C 라이브러리 바인딩** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Xamarin 프로젝트에서 사용 하기 위해 Objective C 라이브러리를 바인딩하는 방법에 대 한 지침입니다.
- [**정의 참조 가이드 입력** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  바인딩 작성자는 바인딩 생성 프로세스를 진행 하는 데 사용할 수 있는 특성의 모든에 대해 설명 합니다.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

목표 Sharpie는 바인딩 첫 번째 패스를 부트스트랩 하려면 명령줄 도구입니다.
공용 API에 매핑할 네이티브 라이브러리의 헤더 파일을 구문 분석 하 여 작동는 [바인딩 정의](~/cross-platform/macios/binding/objective-c-libraries.md) (그렇지 않은 경우 수동으로 수행 되는 프로세스). 목표 Sharpie 자체적으로 바인딩을 만들지 않습니다 되지만 시작 할 수 있습니다!

목표 Sharpie 3.0 Cocoapods 직접 바인딩하는 기능을 도입 되었습니다!

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[연습-iOS Objective C 라이브러리 바인딩](walkthrough.md)

이 페이지는 오픈 소스를 사용 하 여 iOS 바인딩 프로젝트를 만드는 단계별 연습을 제공 [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) 예를 들어 Objective-c 프로젝트. **InfColorPicker** 라이브러리는 사용자가 색 선택 더 친숙 하을 HSB 표현에 따라 색을 선택할 수 있는 재사용 가능한 뷰 컨트롤러를 제공 합니다.
목표 Sharpie 바인딩 프로세스를 지원 하기 위해 사용 됩니다.



## <a name="related-links"></a>관련 링크

- [Objective C 바인딩](~/cross-platform/macios/binding/index.md)
- [Mac 바인딩](~/mac/platform/binding.md)
