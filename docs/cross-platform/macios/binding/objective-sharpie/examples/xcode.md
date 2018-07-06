---
title: Xcode 프로젝트를 사용 하 여 실제 예제
description: 이 문서는 Xcode 프로젝트 목표 Sharpie, C# 바인딩을 Objective-c 코드를 만드는 과정을 간소화 하기를 직접 입력을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 05c55dc7cd20de2d216d1f267ea5a73631748a0a
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855249"
---
# <a name="real-world-example-using-an-xcode-project"></a>Xcode 프로젝트를 사용 하 여 실제 예제

**이 예제에서는 합니다 [Facebook에서 POP 라이브러리](https://github.com/facebook/pop)합니다.**

새 버전 3.0에서는 목표 Sharpie는 Xcode 프로젝트 입력으로 지원합니다. 이러한 프로젝트는 올바른 헤더 파일 및 기본 라이브러리를 컴파일하는 데 필요한 및 너무 바인딩해야 하므로 필요한 컴파일러 플래그를 지정 합니다. 첫 번째 선택 목표 Sharpie _대상_ 와 달리 지시 하지 하는 경우 프로젝트의 기본 구성입니다.

목표 Sharpie를 프로젝트 및 헤더 파일을 구문 분석 하려고 하기 전에를 작성 해야 합니다. 프로젝트에는 항상 바인딩하지 하려고 하기 전에 전체 프로젝트를 빌드하는 것이 가장 좋습니다 외부 소비 및 통합에 대 한 헤더 파일 구조가 올바르게는 빌드 단계 있는 경우가 많습니다.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

## <a name="related-links"></a>관련 링크

- [Objective-c 바인딩 라이브러리를 빌드할 Xamarin University 과정:](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University 과정: 목표 Sharpie 사용 하 여 Objective-c 바인딩 라이브러리를 빌드](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)