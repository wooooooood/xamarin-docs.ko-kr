---
title: Objective-C 바인딩
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 721036993061d08dadf8b279e13981caaa51f91f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/10/2018
---
# <a name="binding-objective-c"></a>Objective-C 바인딩

이 섹션에서는 다양 한 Xamarin.iOS 또는 Xamarin.Mac를 사용 하 여 만든 C# 응용 프로그램에서 호출 될 수 있으므로 Objective-c 라이브러리에 대 한 바인딩을 만드는 포괄 하는 문서입니다.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[개요](~/cross-platform/macios/binding/overview.md)

이 문서 바인딩을 수행 방법의 내부 중 일부를 포함 합니다. 일부 기술적인 정보는 고급이 문서가.입니다.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Objective-C 라이브러리 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md)

이 문서에서는 C# Objective-c Api 및 Objective C의 관용구.NET에서 사용 되는 관용구에 매핑되는 방법의 바인딩을 만드는 데 사용 되는 프로세스를 설명 합니다.
방금 C Api에 바인딩하는 경우이, P/Invoke 프레임 워크에 대 한 표준.NET 메커니즘을 사용 해야 합니다.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[바인딩 정의 대 한 가이드](~/cross-platform/macios/binding/binding-types-reference.md)

모든 바인딩 작성자는 바인딩 생성 프로세스를 진행 하는 데 사용할 수 있는 특성을 설명 하는 참조 가이드입니다.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

목표 Sharpie는 바인딩 첫 번째 패스를 부트스트랩 하려면 명령줄 도구입니다. 공용 API에 매핑할 네이티브 라이브러리의 헤더 파일을 구문 분석 하 여 작동는 [바인딩 정의](~/cross-platform/macios/binding/objective-c-libraries.md) (수동으로 수행할 수도 있습니다는 프로세스).

## <a name="ios"></a>iOS

[iOS 바인딩 페이지](~/ios/platform/binding-objective-c/index.md) 에 다시 연결 이러한 일반적인 바인딩 리소스 뿐만 아니라 아래 예제입니다.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[연습: 바인딩 Objective C 라이브러리](~/ios/platform/binding-objective-c/walkthrough.md)

이 문서는 오픈 소스를 사용 하 여 바인딩 프로젝트를 만드는 단계별 연습을 제공 [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) 예를 들어 Objective-c 프로젝트. InfColorPicker 라이브러리는 사용자가 색 선택 더 친숙 하을 HSB 표현에 따라 색을 선택할 수 있는 재사용 가능한 뷰 컨트롤러를 제공 합니다. 목표 Sharpie 바인딩 프로세스를 지원 하기 위해 사용 됩니다.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[바인딩 예제](https://github.com/mono/monotouch-bindings)

새 바인딩 프로젝트를 만들 때 대 한 참조를 사용 하는 일 수 있는 타사 바인딩의 컬렉션입니다.

## <a name="mac"></a>Mac

지금까지 [Mac 바인딩](~/mac/platform/binding.md) 매우 수동 프로세스 되었습니다. ˇ ľ ř ˝는 [다운로드할 수 있는 미리 보기](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) Mac.에 대 한 Visual Studio의 이후 릴리스에 대 한 Mac 바인딩 프로젝트 지원



## <a name="related-links"></a>관련 링크

- [iOS 바인딩](~/ios/platform/binding-objective-c/index.md)
- [Mac 바인딩](~/mac/platform/binding.md)
