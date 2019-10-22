---
title: Xamarin.ios 통합 응용 프로그램을 64 비트로 업데이트
description: 이 가이드에서는 Xamarin.ios 응용 프로그램을 64 비트 대상으로 업데이트 하는 방법을 설명 합니다. 또한이 변경 작업을 수행할 때 발생할 수 있는 오류 종류에 대 한 예를 제공 합니다.
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
author: conceptdev
ms.author: crdun
ms.date: 02/22/2018
ms.openlocfilehash: 5539bab417c5efc0064cd1753cb74c7524463ee5
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "70765918"
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>Xamarin.ios 통합 응용 프로그램을 64 비트로 업데이트

1 월 2018을 기반으로 하는 Apple에서는 새 [Mac 앱 스토어 제출이 64 비트를 대상](https://developer.apple.com/news/?id=06282017a)으로 해야 합니다. Mac 앱 스토어에서 이미 사용할 수 있는 앱은 64 2018 비트를 대상으로 업데이트 해야 합니다.

**새** xamarin.ios 프로젝트 템플릿을  >  **파일** 은 기본적으로 64 비트 응용 프로그램을 만듭니다. 따라서 최근에 만든 앱은 이미 64 비트와 호환 되며 변경 내용이 필요 하지 않습니다.

## <a name="targeting-64-bit"></a>64 비트 대상 지정

1. Xamarin.ios 앱에 대 한 **프로젝트 옵션** 창을 엽니다.

   ![프로젝트에 대 한 상황에 맞는 메뉴](mac-64-bit-images/1-contextual_menu-vsmac.png "프로젝트에 대 한 상황에 맞는 메뉴")

2. **Mac 빌드** 를 선택 하 고 **지원 되는 아키텍처** 를 **x86 \_64**로 설정 합니다.

   [![지원 되는 아키텍처를 x86_64로 설정](mac-64-bit-images/2-project_options-vsmac.png "지원 되는 아키텍처를 x86_64로 설정")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. 앱에 네이티브 참조 또는 바인딩 프로젝트와 같은 외부 종속성이 있는 경우 해당 종속성을 64 비트 대상으로 업데이트 합니다.

### <a name="errors"></a>오류

64 비트 지원으로 응용 프로그램을 처음 빌드 하거나 실행할 때 clang 또는 런타임 문제에서 링크 오류가 발생할 수 있습니다. 이러한 오류는 타사 종속성 (예: Xamarin.ios 또는 바인딩 프로젝트의 네이티브 참조 또는 수동으로 로드 된 시스템 수준 프레임 워크)이 64 비트로 업데이트 되지 않은 경우 발생할 수 있습니다.

> [!TIP]
> 프로젝트를 64 비트로 변환 하는 것은 주요 변경 내용으로, 다양 한 프로그래밍 오류를 간접적으로 발견할 수 있습니다. 특히 데이터 구조의 크기와 맞춤을 변경 하 여 프로젝트에 연결 된 p/invoke 시그니처와 네이티브 코드에 영향을 줄 수 있습니다. 제공 된 빌드 경고를 검토 하 고 응용 프로그램을 철저 하 게 테스트 하 여 잠재적인 문제를 파악 하는 것이 좋습니다.

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>64 비트를 대상으로 하지 않는 동적으로 연결 된 타사 종속성으로 인해 발생 하는 예제 오류:

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

이 오류는 런타임에 예상 되는 핸들 대신 `IntPtr.Zero`을 반환 하는 `dlopen`에 의해 수행 될 수 있습니다.

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>64 비트를 대상으로 하지 않는 정적으로 연결 된 타사 종속성으로 인해 발생 하는 예제 오류:

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

성공적으로 빌드하고 실행 하려면 이러한 종속성을 64 비트로 업데이트 하 고 앱을 다시 컴파일하십시오.
