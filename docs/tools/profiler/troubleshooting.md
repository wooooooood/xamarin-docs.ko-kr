---
title: Xamarin Profiler 문제 해결
description: 이 문서에서는 Xamarin Profiler 관련 된 문제 해결 정보를 제공 합니다. 로깅 및 진단, IDE 및 기타 항목과 관련 된 문제를 설명 합니다.
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
author: davidortinau
ms.author: daortin
ms.date: 10/27/2017
ms.openlocfilehash: 5b4b4bdf85ec79a46a4e4c06504eb8b9b85af329
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84566959"
---
# <a name="xamarin-profiler-troubleshooting"></a>Xamarin Profiler 문제 해결

## <a name="logging-and-diagnostics"></a>로깅 및 진단

Xamarin 팀은 다음을 비롯 한 정보를 제공 하는 경우 문제를 추적 하는 데 도움이 될 수 있습니다.

- 문제, 충돌 또는 실패의 동영상 가이드 워크플로를 시작 합니다.
- 로그 출력 (아래 참조)
- 프로 파일링 세션에 대해 생성 되는 **mlpd입니다** (아래 참조).

### <a name="getting-log-outputs"></a>로그 출력 가져오기

Mac의 로그는에 저장 됩니다 `~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log` .

Windows에서는 `%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log` 문제를 제출할 때마다 최신 로그를 포함 하 여에 저장 됩니다.

진행 되는 동안 더 많은 로깅을 추가 하 고 있으므로이 출력이 증가 하 고 시간이 지남에 따라 더 유용 하 게 제공 됩니다.

<a name="gen_mlpd"></a>

### <a name="generating-mlpd-files"></a>Mlpd 파일 생성

**Mlpd** 파일은 mono 런타임 프로파일러의 압축 된 출력입니다. Xamarin Profiler GUI는 **mlpd** 에서 데이터를 읽고 사용자에 게 표시 합니다. **mlpd** 파일은 엔지니어가 프로파일러에서 데이터를 사용 하는 문제를 진단 하는 데 도움이 되기 때문에 Xamarin 용 디버깅 도구를 사용 하는 것이 좋습니다.

현재 세션에 대 한 **mlpd** 는 Mac의 디렉터리에 자동으로 저장 `/tmp` 되며 타임 스탬프로 식별할 수 있습니다. 로깅을 설정 하면 첫 번째 출력은 **mlpd** 파일의 경로가 됩니다. 일반적으로 **mlpd** 파일은 디렉터리에 저장 됩니다. ~/var/folders...

**파일 > 다른 이름으로 저장** ...을 선택 하 여 현재 세션에 대 한 **mlpd** 를 저장할 수도 있습니다. Profiler의 메뉴에서 다음을 수행 합니다.

**Mac용 Visual Studio**:

![](troubleshooting-images/image17.png "Saving .mlpd file in Visual Studio for Mac")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Saving .mlpd file in Visual Studio")

**Mlpd** 는 많은 정보를 포함 하며 파일 크기는 커집니다.

## <a name="troubleshooting"></a>문제 해결

아래 목록은 일반적인 알려진 문제, 해결 방법 및 프로파일러 사용에 대 한 팁과 요령을 보여 줍니다.

> [!NOTE]
> Windows 또는 Mac용 Visual Studio에서 Visual Studio Enterprise이 기능의 잠금을 해제 하려면 Visual Studio **Enterprise** 구독자 여야 합니다.

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>IOS profiler 옵션이 표시 되지 않거나 회색으로 표시 된 경우 [Visual Studio 및 Mac용 Visual Studio]

이 문제를 해결 하려면 다음 설정을 확인 합니다.

- 디버그 구성을 사용 중인지 확인 합니다.
- SGen 가비지 수집기를 사용 하 고 있는지 확인 합니다.
- 플랫폼이 [지원](~/tools/profiler/index.md#Profiler_Support)되는지 확인 합니다.
- 올바른 라이선스가 있는지 확인 합니다.
- 로그인 되어 있고 제대로 인증 되었는지 확인 합니다.
- [Visual Studio] [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/enterprise/) 를 사용 하 고 있고 유효한 엔터프라이즈 라이선스가 있어야 합니다.

#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>프로파일러를 시작 하려고 할 때 오류가 발생 합니다.

Visual Studio에서 프로파일러를 사용 하는 경우이 오류 상자가 표시 되 면 다음을 수행 합니다.

![](troubleshooting-images/error.png "Error box when using the profiler in Visual Studio")

일반적으로 시뮬레이터/에뮬레이터를 시작할 수 없기 때문입니다. 앱을 정상적으로 실행 하 고, 제공 하는 문제를 해결 한 후 프로파일러를 다시 사용 해 보세요.

#### <a name="to-watch-a-specific-thread"></a>특정 스레드를 시청 하려면

특별히 감시 하고자 하는 스레드가 있는 경우 생성이 시작 될 때 대신 스레드 이름을로 하는 것이 좋습니다 `ThreadName` `0x0` . 예를 들어 스레드 이름을로 설정 하려면 `UI` 다음 코드를 사용할 수 있습니다.

```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```

## <a name="related-links"></a>관련 링크

- [연습-Xamarin Profiler 사용](~/tools/profiler/index.md)
- [메모리 및 성능 모범 사례](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [릴리스 정보](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/profiler/preview/index.md)
