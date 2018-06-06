---
title: 64 비트로 업데이트 Xamarin.Mac 통합 응용 프로그램
description: 이 가이드에서는 64 비트 대상에 Xamarin.Mac 응용 프로그램을 업데이트 하는 방법에 설명 합니다. 또한이 변경 사항을 때 발생할 수 있는 오류 종류의 예를 제공 합니다.
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/22/2018
ms.openlocfilehash: aa97f9a68ea4acc4234233a22d10c99cde3e6d6c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780700"
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>64 비트로 업데이트 Xamarin.Mac 통합 응용 프로그램

1 월 2018 당시 Apple에서는 요구 되는 새 [Mac 앱 스토어 제출에는 64 비트 대상](https://developer.apple.com/news/?id=06282017a)합니다. Mac 앱 스토어에서 이미 사용할 수 있는 앱 년 6 월 2018 하 여 64 비트 대상에 업데이트 되어야 합니다.

**파일** > **새로** Xamarin.Mac 프로젝트 템플릿은 최근에 만든된 모든 앱을 이미 64 비트와 호환 하 고 변경 하지 않아도 됩니다 수 있도록 기본적으로 64 비트 응용 프로그램을 만듭니다.

## <a name="targeting-64-bit"></a>64 비트 대상 지정

1. 열기는 **프로젝트 옵션** 창 고 Xamarin.Mac 앱:

   ![프로젝트에 대 한 상황에 맞는 메뉴](mac-64-bit-images/1-contextual_menu-vsmac.png "프로젝트에 대 한 상황에 맞는 메뉴")

2. 선택 **Mac 빌드** 설정 **지원 되는 아키텍처** 를 **x86\_64**:

   [![지원 되는 아키텍처 x86_64 설정](mac-64-bit-images/2-project_options-vsmac.png "x86_64에 지원 되는 아키텍처를 설정 합니다.")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. 응용 프로그램에 네이티브 참조 또는 바인딩 프로젝트와 같은 외부 종속성을 64 비트 대상에 업데이트 합니다.

### <a name="errors"></a>오류

처음으로 작성 하거나 64 비트 지원을 통해 응용 프로그램을 실행할 링크 오류 clang 또는 런타임 문제를 발생할 수 있습니다. 제 3 자 하는 경우 이러한 오류가 발생할 수 있습니다 종속성-Xamarin.Mac 또는 바인딩 프로젝트 또는 수동으로 로드 된 시스템 수준 프레임 워크의 예를 들어, 네이티브 참조-64 비트로 업데이트 되지 않았습니다.

> [!TIP]
> 64 비트 프로젝트 변환 크게 변경 된 사항 이므로 다양 한 프로그래밍 오류를 직접 발견할 수 있습니다. 특히 서명 p/invoke 및 프로젝트에 연결 하는 네이티브 코드에 영향을 미치므로 데이터 구조의 맞춤과 크기는 변경 될 수 있습니다. 지정 된 모든 빌드 경고를 검토 하는 것이 좋습니다. 응용 프로그램 및 테스트 throughly 나중에 잠재적 문제를 검색 하려면.

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>64 비트 대상 하지 않는 동적으로 연결 된 타사 종속성으로 인해 발생 하는 예제 오류:

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

이 오류는 런타임 시 수행할 수 `dlopen` 반환 `IntPtr.Zero` 예상된 핸들 대신 합니다.

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>64 비트 대상 하지 않는 정적으로 연결 된 타사 종속성으로 인해 발생 하는 예제 오류:

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

를 빌드하고 성공적으로 실행 하려면 64 비트로 이러한 종속성을 업데이트 하 고 앱을 다시 컴파일하십시오.

