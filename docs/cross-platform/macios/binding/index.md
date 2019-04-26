---
title: Objective-C 바인딩
description: 이 문서를 만드는 방법을 설명 하는 다양 한 가이드에 대 한 링크를 제공 C# 에 대 한 바인딩을 Objective-c 코드를 개발자가 Xamarin 응용 프로그램에서 기본 제공 라이브러리를 사용할 수 있도록 합니다.
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 3f1e1ce324e849c0c939d936eb6ee1470cf24a3b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266610"
---
# <a name="binding-objective-c"></a>Objective-C 바인딩

이 섹션에서는 다양 한 문서에서 호출 될 수 있으므로 Objective-c 라이브러리 바인딩 만들기를 포함 하는 C# Xamarin.iOS 또는 Xamarin.Mac을 사용 하 여 만든 응용 프로그램입니다.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[개요](~/cross-platform/macios/binding/overview.md)

이 문서 바인딩을 수행 하는 방법의 내부 구조 중 일부를 포함 합니다. 이 일부 기술 정보를 사용 하 여 고급 문서.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Objective-C 라이브러리 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md)

이 문서를 만드는 데 사용 하는 프로세스를 설명 합니다. C# Objective C Api 및 objective-c에서 코드를.NET에 사용 되는 관용구에 매핑되는 방법의 바인딩.
C Api만을 바인딩하는 경우이 P/Invoke 프레임 워크에 대 한 표준.NET 메커니즘을 사용 해야 합니다.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[바인딩 정의 참조 가이드](~/cross-platform/macios/binding/binding-types-reference.md)

바인딩 생성 프로세스를 추진 하는 바인딩 작성자가 사용할 수 있는 특성의 모든 설명 하는 참조 가이드입니다.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

목표 Sharpie는 바인딩의 첫 번째 패스를 부트스트랩 하는 데는 명령줄 도구입니다. 공용 API에 매핑할 네이티브 라이브러리의 헤더 파일을 구문 분석 하 여 작동 합니다 [바인딩 정의](~/cross-platform/macios/binding/objective-c-libraries.md) (수동으로 수행할 수 있는 프로세스).

## <a name="ios"></a>iOS

합니다 [iOS 바인딩 페이지](~/ios/platform/binding-objective-c/index.md) 에 다시 연결 공통 바인딩 리소스 또한 아래 예제입니다.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[연습: Objective-c 라이브러리 바인딩](~/ios/platform/binding-objective-c/walkthrough.md)

이 문서에서는 오픈 소스를 사용 하 여 바인딩 프로젝트 만들기의 단계별 연습은 [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) 예로 Objective C 프로젝트입니다. InfColorPicker 라이브러리는 사용자가 색 선택 영역을 보다 친숙 한 만들기, HSB 표현에 따라 색을 선택할 수 있는 재사용 가능한 뷰 컨트롤러를 제공 합니다. 목표 Sharpie 바인딩 프로세스를 지원 하기 위해 사용 됩니다.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[바인딩 샘플](https://github.com/mono/monotouch-bindings)

새 바인딩 프로젝트를 만들 때 대 한 참조를 사용 하는 수 있는 타사 바인딩의 컬렉션입니다.

## <a name="mac"></a>Mac

지금까지 [Mac 바인딩](~/mac/platform/binding.md) 매우 수동 프로세스가 되었습니다. 현재는 [다운로드할 수 있는 미리 보기](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) mac 용 Visual Studio의 향후 릴리스에 대 한 Mac 바인딩 프로젝트 지원



## <a name="related-links"></a>관련 링크

- [iOS Binding](~/ios/platform/binding-objective-c/index.md)
- [Mac 바인딩](~/mac/platform/binding.md)
- [Xamarin University 과정: Objective-c 바인딩 라이브러리를 빌드](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University 과정: 목표 Sharpie로는 Objective-c 바인딩 라이브러리 빌드](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
