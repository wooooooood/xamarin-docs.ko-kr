---
title: 제한 사항
description: Xamarin 라이브 플레이어의 몇 가지 제한
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: 0068540ec385ab3be56865c7728eb3128154a2ea
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="limitations"></a>제한 사항

![미리 보기 기능](~/media/shared/preview.png)

## <a name="device-requirements"></a>장치 요구 사항
Xamarin Player 라이브 응용 프로그램에서는 다음 장치를 지원합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

- Android 4.2 이상입니다.
- ARM-v7a, ARM v8a, ARM64 v8a, x86 또는 x86_64 프로세서.

# <a name="iostabios"></a>[iOS](#tab/ios)

- iOS 9.0 이상이 있습니다.
- ARM64 프로세서입니다.

-----

## <a name="limitations"></a>제한 사항

Xamarin Player 라이브 아래 항목을 포함 하 여 실행할 수는 작업에 몇 가지 제한 사항이 있습니다.

### <a name="xamarinforms"></a>Xamarin.Forms
- 사용자 지정 렌더러 지원 되지 않습니다.
- 효과 지원 되지 않습니다.
- 사용자 지정 바인딩 가능 속성을 사용 하 여 사용자 지정 컨트롤은 지원 되지 않습니다.
- 포함 된 리소스가 지원 되지 않습니다 (즉. PCL에 이미지 또는 기타 리소스를 포함).
- 제 3 자 MVVM 프레임 워크 (즉, 지원 되지 않습니다. 프리즘, Mvvm 크로스, Mvvm 조명 등).
- Ios 자산 카탈로그 지원 되지 않습니다.

### <a name="other-project-types"></a>다른 프로젝트 형식
- 네이티브 Android 또는 iOS 프로젝트 (Android XML 또는 스토리 보드를 사용 하 여 사용자 인터페이스)에 대 한 라이브 플레이어를 사용 하는 것이 없습니다.

### <a name="misc"></a>기타
- 반사를 제한적으로 지원 (현재 영향을 줌 SQLite 및 Json.NET와 같은 일부 인기 있는 NuGets). 다른 NuGets 계속 지원할 수 있습니다.
- 일부 시스템 클래스는 재정의할 수 없습니다 (예를 들어 서브 클래스를 구현할 수 없습니다).
- 그러나 프로 비전 해야 하는 일부 플랫폼 기능 (구성 된 사진 갤러리 액세스과 같은 일반적인 작업에 대 한) Xamarin Player 라이브 앱에서 작동 하지 않습니다.
- 사용자 지정 대상 및 빌드 단계는 무시 됩니다. 예를 들어 도구 Fody, Retit, AutoFac, 및 같은 AutoMapper 통합 될 수 없습니다.
- F # 프로젝트는 Android에서 지원 되지 않습니다 및 제한적으로 iOS에서 지원
- 사용자 정의 제네릭 클래스와 인터페이스를 사용 하는 고급 시나리오를 지원할 수 있습니다.

추가 문제를 보고 하십시오 [: bugzilla](https://aka.ms/live-player-report-issue)합니다.


## <a name="related-links"></a>관련 링크

- [문제 해결](~/tools/live-player/troubleshooting.md)
- [설정](~/tools/live-player/install.md)
