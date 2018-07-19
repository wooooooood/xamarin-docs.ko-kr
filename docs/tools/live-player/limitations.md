---
title: Xamarin Live Player의 제한 사항
description: 이 문서에는 Xamarin Live Player의 제한 사항을 설명 합니다. 장치 요구 사항에 설명, 작동, 프로젝트 형식 및 기타 다른 항목을 기능입니다.
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: ea71391382f9e1ecb80cbf5f2d5bf127e0d6d1be
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815284"
---
# <a name="limitations-of-xamarin-live-player"></a>Xamarin Live Player의 제한 사항

![미리 보기 기능](~/media/shared/preview.png)

## <a name="device-requirements"></a>장치 요구 사항
Xamarin Live Player 앱에서는 다음 장치를 지원합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

- Android 4.2 이상입니다.
- ARM-v7a, v8a ARM, ARM64-v8a x86 또는 x86_64 프로세서.

# <a name="iostabios"></a>[iOS](#tab/ios)

- iOS 9.0 이상입니다.
- ARM64 프로세서입니다.

-----

## <a name="limitations"></a>제한 사항

몇 가지 제한 사항이에 Xamarin Live Player 아래 항목을 포함 하 여 실행할 수 있습니다.

### <a name="xamarinforms"></a>Xamarin.Forms

- 사용자 지정 렌더러 지원 되지 않습니다.
- 작업이 지원 되지 않습니다.
- 사용자 지정 바인딩 가능한 속성을 사용 하 여 사용자 지정 컨트롤을 사용 하는 것이 없습니다.
- 포함 된 리소스는 지원 되지 않습니다 (ie. PCL에 이미지 또는 기타 리소스를 포함).
- 타사 MVVM 프레임 워크 (즉 지원 되지 않습니다. Prism, Mvvm 간, Mvvm Light 등).
- Ios 자산 카탈로그 지원 되지 않습니다.

### <a name="other-project-types"></a>기타 프로젝트 형식

- 네이티브 Android 또는 iOS 프로젝트 (Android XML 또는 스토리 보드를 사용 하 여 사용자 인터페이스)에 대 한 live Player는 사용 하는 것이 없습니다.

### <a name="misc"></a>기타

- 리플렉션에 대 한 지원이 제한 됩니다 (현재 SQLite Json.NET 등 몇 가지 인기 있는 Nuget을 줌). 다른 Nuget 계속 지원할 수 있습니다.
- 일부 시스템 클래스는 재정의할 수 없습니다 (예를 들어 하위 클래스를 구현할 수 없습니다).
- (그러나 구성 된 사진 갤러리 액세스과 같은 일반적인 작업에 대 한) Xamarin Live Player 앱에서 프로 비전 해야 하는 일부 플랫폼 기능 작업 수 없습니다.
- 사용자 지정 대상 및 빌드 단계는 무시 됩니다. 예를 들어 Fody, Refit, AutoFac을 및 AutoMapper와 같은 도구를 통합할 수 없습니다.
- F # 프로젝트가 Android에서 지원 되지 않습니다 iOS에 대 한 지원이 제한 됩니다
- 사용자 지정 제네릭 클래스 및 인터페이스를 사용 하 여 고급 시나리오를 지원할 수 있습니다.

추가 문제를 보고 하십시오 [bugzilla](https://aka.ms/live-player-report-issue)합니다.

## <a name="related-links"></a>관련 링크

- [문제 해결](~/tools/live-player/troubleshooting.md)
- [설정](~/tools/live-player/install.md)
