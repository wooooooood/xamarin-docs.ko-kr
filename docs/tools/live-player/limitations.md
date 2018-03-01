---
title: "제한 사항"
description: "Xamarin 라이브 플레이어의 몇 가지 제한"
ms.topic: article
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 08/04/2017
ms.openlocfilehash: d551c0a82fb8f970ce5a00ec9e64ac7f49c81a44
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="limitations"></a>제한 사항

![미리 보기 기능](~/media/shared/preview.png)

## <a name="device-requirements"></a>장치 요구 사항
Xamarin Player 라이브 응용 프로그램에서는 다음 장치를 지원합니다.

### <a name="ios"></a>iOS
- iOS 9.0 이상이 있습니다.
- ARM64 프로세서입니다.
- 확인 된 [앱 스토어](https://itunes.apple.com/us/app/xamarin-live-player/id1228841832?mt=8) 지원 되는 장치 목록에 대 한 합니다.

### <a name="android"></a>Android
- Android 4.2 이상입니다.
- ARM-v7a, ARM v8a, ARM64 v8a, x86 또는 x86_64 프로세서.


## <a name="limitations"></a>제한 사항

Xamarin Player 라이브 아래 항목을 포함 하 여 실행할 수는 작업에 몇 가지 제한 사항이 있습니다.

### <a name="android"></a>Android
- Android 사용자 인터페이스 AXML 파일에 두고 설계 현재 지원 되지 않습니다.

### <a name="ios"></a>iOS
- 일부 iOS 스토리 보드 기능은 지원 되지 않습니다.
- iOS XIB 파일은 지원 되지 않습니다.

### <a name="xamarinforms"></a>Xamarin.Forms
- 사용자 지정 렌더러 지원 되지 않습니다.
- 효과 지원 되지 않습니다.
- 사용자 지정 바인딩 가능 속성을 사용 하 여 사용자 지정 컨트롤은 지원 되지 않습니다.
- 포함 된 리소스가 지원 되지 않습니다 (즉. PCL에 이미지 또는 기타 리소스를 포함).

### <a name="misc"></a>기타
- 반사를 제한적으로 지원 (현재 영향을 줌 SQLite 및 Json.NET와 같은 일부 인기 있는 NuGets). 다른 NuGets 계속 지원 됩니다.
- 일부 시스템 클래스는 재정의할 수 없습니다 (예를 들어 서브 클래스를 구현할 수 없습니다).
- 그러나 프로 비전 해야 하는 일부 플랫폼 기능 (구성 된 사진 갤러리 액세스과 같은 일반적인 작업에 대 한) Xamarin Player 라이브 앱에서 작동 하지 않습니다.
- 사용자 지정 대상 및 빌드 단계는 무시 됩니다. 예를 들어 Fody와 같은 도구를 통합 될 수 없습니다.

> [!WARNING]
> **참고:** Xamarin Studio에서 Xamarin Player 라이브 작동 하지 않습니다

추가 문제를 보고 하십시오 [: bugzilla](https://aka.ms/live-player-report-issue)합니다.


## <a name="related-links"></a>관련 링크

- [문제 해결](~/tools/live-player/troubleshooting.md)
- [설정](~/tools/live-player/install.md)
