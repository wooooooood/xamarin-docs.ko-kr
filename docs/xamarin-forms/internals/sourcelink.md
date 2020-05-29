---
title: 소스 링크Xamarin.Forms
description: 이 문서에서는 소스 링크를 사용 하 여로 디버그 하는 방법을 설명 Xamarin.Forms 합니다.
zone_pivot_groups: ''
ms.prod: ''
ms.assetId: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 57db314538c42ef9d58691ba16ab68371ff092b7
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138319"
---
# <a name="source-link-with-xamarinforms"></a>소스 링크Xamarin.Forms

Xamarin.FormsNuGet 패키지는 소스 링크 매핑을 포함 합니다. 소스 링크는 NuGet 패키지에 포함 된 컴파일된 라이브러리를 소스 코드 리포지토리에 매핑합니다. Visual Studio는 디버깅 하는 동안 소스 코드 파일을 다운로드 하 고 개발자가 소스에서 빌드하지 않고 패키지를 디버그할 수 있도록 하는 코드를 단계별로 실행할 수 있도록 합니다.

소스 링크를 사용 하는 방법에 대 한 자세한 내용은 [소스 링크 설명서](/dotnet/standard/library-guidance/sourcelink)를 참조 하세요.

::: zone pivot="windows"

> [!WARNING]
> Visual Studio 2019은 **.net 디버거의** 소스 링크를 지원 하지만 현재 **Mono 디버거의**소스 링크를 지원 하지 않습니다. 따라서 소스 링크를 사용 하 여 UWP 앱을 디버그할 수 있지만 Android 또는 iOS 앱은 디버그할 수 없습니다. UWP 앱을 디버그할 때 디버깅 하려는 라이브러리의 PDB 파일이 응용 프로그램이 컴파일되는 **bin** 디렉터리의 **AppX** 폴더에 복사 되었는지 확인 해야 합니다.

## <a name="enable-source-link"></a>소스 링크 사용

소스 링크를 사용 하려면 외부 코드에 대해 디버깅을 사용 하도록 설정 해야 합니다. 그렇지 않으면 디버거가 현재 솔루션에 포함 되지 않은 코드에 대 한 이전 호출을 실행 합니다. Visual Studio 2019에서이는 **디버깅** 섹션의 **옵션** 메뉴에서 찾을 수 있습니다.

[![Visual Studio 2019에서 소스 링크 사용](sourcelink-images/sourcelink-enable-pc-cropped.png)](sourcelink-images/sourcelink-enable-pc.png#lightbox)

**내 코드만 사용** 이 사용 하지 않도록 설정 되어 있고 **소스 링크 지원 사용** 이 설정 되어 있는지 확인 합니다.

::: zone-end
::: zone pivot="macos"

## <a name="enable-source-link"></a>소스 링크 사용

소스 링크를 사용 하려면 외부 코드에 대해 디버깅을 사용 하도록 설정 해야 합니다. 그렇지 않으면 디버거가 현재 솔루션에 포함 되지 않은 코드에 대 한 이전 호출을 실행 합니다. 이 옵션은 **디버거** 섹션의 **기본 설정** 창에서 찾을 수 있습니다.

[![Mac용 Visual Studio에서 소스 링크 사용](sourcelink-images/sourcelink-enable-mac-cropped.png)](sourcelink-images/sourcelink-enable-mac.png#lightbox)

**외부 코드에 한 단계씩 코드 실행** 을 사용 하도록 설정 했는지 확인 합니다.

::: zone-end

## <a name="debug-xamarinforms-using-source-link"></a>Xamarin.Forms소스 링크를 사용 하 여 디버그

외부 패키지 디버깅이 사용 하도록 설정 된 경우 Visual Studio는 NuGet 패키지에 포함 된 소스 링크 매핑을 사용 하 여 외부 소스 코드를 다운로드 하 고 단계별로 실행 합니다. 이는에서 제공 하는 메서드에 대 한 호출에 중단점을 설정 하 여 테스트할 수 있습니다 Xamarin.Forms .

[![메서드에 설정 되는 중단점 Xamarin.Forms](sourcelink-images/breakpoint-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

**디버거** 옵션에서 지정한 설정에 따라 Visual Studio에서 소스 파일을 다운로드 하 고 있음을 경고 합니다.

[![Visual Studio 외부 코드 경고](sourcelink-images/external-code-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

Visual Studio가 파일을 다운로드 하도록 허용 하면 디버거가 외부 코드를 한 단계씩 실행 합니다.

::: zone pivot="windows"

## <a name="source-link-caching"></a>소스 링크 캐싱

소스 링크는 성능을 위해 캐싱을 사용 합니다. 소스에 대 한 캐싱 디렉터리 링크는 **기호** 섹션에서 **디버깅** 의 **옵션** 메뉴에서 정의 됩니다.

[![Visual Studio 소스 링크 캐싱](sourcelink-images/sourcelink-caching-pc-cropped.png)](sourcelink-images/sourcelink-caching-pc.png#lightbox)

이 메뉴를 사용 하 여 모든 디버그 기호에 대 한 캐싱 디렉터리를 지정 하 고 캐시 된 기호와 관련 된 문제가 발생 하는 경우 캐시를 지울 수 있습니다.

::: zone-end
::: zone pivot="macos"

## <a name="source-link-caching"></a>소스 링크 캐싱

소스 링크는 성능을 위해 캐싱을 사용 합니다. MacOS의 소스 링크에 대 한 캐싱 디렉터리는 `/Users/<username>/Library/Caches/VisualStudio/8.0/Symbols` 입니다. 이 폴더에는 원본 파일을 다운로드 하는 데 사용 된 리포지토리가 저장 된 하위 폴더가 포함 되어 있습니다. NuGet 패키지에 대 한 백업 리포지토리가 변경 된 경우 캐시를 새로 고치려면 이러한 폴더를 수동으로 삭제 해야 할 수 있습니다.

::: zone-end

## <a name="related-links"></a>관련 링크

- [소스 링크 설명서](/dotnet/standard/library-guidance/sourcelink)
- [GitHub의 소스 링크](https://github.com/dotnet/sourcelink)
