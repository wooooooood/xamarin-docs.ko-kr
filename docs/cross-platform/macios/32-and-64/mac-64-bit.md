---
title: 응용 프로그램을 64 비트로 Xamarin.Mac 통합 업데이트
description: 이 가이드에는 대상 64 비트로 Xamarin.Mac 응용 프로그램을 업데이트 하는 방법을 설명 합니다. 또한이 변경 내용을 적용 하는 경우 발생할 수 있는 오류의 종류의 예를 제공 합니다.
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
author: conceptdev
ms.author: crdun
ms.date: 02/22/2018
ms.openlocfilehash: 9bd70fec5d6d3bbbc4855980e1542bd4e486acaa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266734"
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>응용 프로그램을 64 비트로 Xamarin.Mac 통합 업데이트

2018 년 1 월을 기준으로 Apple 새 필요 [Mac 앱 스토어 제출 64 비트 대상](https://developer.apple.com/news/?id=06282017a)합니다. Mac 앱 스토어에서 앱을 이미 사용 가능한 2018 년 6 월에서 64 비트 대상에 업데이트 되어야 합니다.

합니다 **파일** > **새로 만들기** 최근에 만든된 모든 앱을 이미 64 비트와 호환 하 고 변경 하지 않아도 됩니다 있도록 Xamarin.Mac 프로젝트 템플릿은 기본적으로 64 비트 응용 프로그램을 만듭니다.

## <a name="targeting-64-bit"></a>64 비트 대상 지정

1. 엽니다는 **프로젝트 옵션** Xamarin.Mac 앱에 대 한 창:

   ![프로젝트에 대 한 상황에 맞는 메뉴](mac-64-bit-images/1-contextual_menu-vsmac.png "프로젝트에 대 한 상황에 맞는 메뉴")

2. 선택 **Mac 빌드** 설정 **지원 되는 아키텍처** 하 **x86\_64**:

   [![지원 되는 아키텍처 x86_64 설정을](mac-64-bit-images/2-project_options-vsmac.png "x86_64에 지원 되는 아키텍처를 설정")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. 앱에 네이티브 참조 또는 바인딩 프로젝트와 같은 외부 종속성을 64 비트 대상에 업데이트 합니다.

### <a name="errors"></a>오류

처음으로 빌드 또는 64 비트 지원을 통해 응용 프로그램을 실행할 링크 오류 clang 또는 런타임 문제를 발생할 수 있습니다. 제 3 자 하는 경우 이러한 오류가 발생할 수 있습니다 종속성-예를 들어, 네이티브 참조 Xamarin.Mac 또는 바인딩 프로젝트의 경우 나 수동으로 로드 하는 시스템 전체 프레임 워크-64 비트로 업데이트 되지 않았습니다.

> [!TIP]
> 64 비트 프로젝트 변환 주요 변경 하 고 다양 한 프로그래밍 오류를 직접 발견할 수 있습니다. 특히 크기 및 맞춤의 p/invoke 서명 및 프로젝트에 연결 하는 네이티브 코드에 영향을 하는 데이터 구조는 변경할 수 있습니다. 응용 프로그램을 철저히 테스트 하 고 모든 빌드 지정 된 경고를 검토 하는 것이 좋습니다. 잠재적 문제를 검색 하려면 나중에 있습니다.

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>64 비트 대상 하지 않는 동적으로 연결 된 타사 종속성에서 발생 한 오류가 예제:

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

이 오류에서 런타임 시 수행할 수 `dlopen` 반환 `IntPtr.Zero` 예상된 핸들을 대신 합니다.

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>64 비트 대상 하지 않는 정적으로 연결 된 타사 종속성에서 발생 한 오류가 예제:

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

을 빌드하고 성공적으로 실행 하려면 64 비트로 이러한 종속성을 업데이트 하 고 앱을 다시 컴파일하십시오.

