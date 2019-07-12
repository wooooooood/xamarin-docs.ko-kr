---
title: Xamarin Profiler 문제 해결
description: 이 문서에서는 Xamarin Profiler와 관련 된 문제 해결 정보를 제공 합니다. 관련 항목 로깅 및 진단, IDE 및 기타 항목을 설명 합니다.
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
author: lobrien
ms.author: laobri
ms.date: 10/27/2017
ms.openlocfilehash: e4a4376291ff56433c8cd9785989af2983a80c1c
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832158"
---
# <a name="xamarin-profiler-troubleshooting"></a>Xamarin Profiler 문제 해결

## <a name="logging-and-diagnostics"></a>로깅 및 진단

Xamarin 팀 정보를 제공 하는 경우 문제를 추적 하는 데 도움이 수 포함 합니다.

- 문제, 충돌 및 실패에 이르는 워크플로 스크린 캐스트 합니다.
- 로그 출력 (아래 참조)입니다.
- 합니다 **.mlpd** 프로 파일링 세션 (아래 참조)에 대해 생성 되 고 있습니다.

### <a name="getting-log-outputs"></a>로그 출력 가져오기

Mac에서 로그에 저장 됩니다 `~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`합니다.

Windows에서에 저장 된 이러한 `%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log` 문제를 제출할 때마다 최신 로그를 포함 하세요.

자세한 로깅 서,이 출력 증가 하며 시간이 지남에 따라 더 유용할 수 있으므로 추가 될 예정입니다.

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>.Mlpd 파일 생성

**.mlpd** 파일이 압축 된 mono 런타임이 프로파일러 출력 합니다. Xamarin Profiler GUI에서 데이터를 읽습니다.는 **.mlpd** 사용자에 대 한 표시 합니다. **.mlpd** 파일 데이터를 사용 하 여 있을 수는 Profiler 문제를 진단 하는 엔지니어 수 있기 때문에 Xamarin 용 디버깅 도구 유용 합니다.

합니다 **.mlpd** 현재 세션은 Mac의 자동 저장에 대 한 `/tmp` 디렉터리 타임 스탬프에서 식별할 수 있습니다. 로깅을 설정 하면 첫 번째 출력에 대 한 경로 수를 **.mlpd** 파일입니다. 합니다 **.mlpd** 파일은 일반적으로 ~/var/폴더를 시작 하는 디렉터리에 저장 하는 중...

합니다 **.mlpd** 를 선택 하 여 현재 세션으로 저장할 수도 있습니다에 대 한 **파일 > 다른 이름으로 저장 하는 중...** Profiler의 메뉴에서:

**Visual Studio for Mac**:

![](troubleshooting-images/image17.png "Mac 용 Visual Studio에서.mlpd 파일 저장")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Visual Studio에서.mlpd 파일 저장")

것이 중요 한 사실은 **.mlpd** 많은 정보를 포함 하 고 파일 크기가 커집니다.

## <a name="troubleshooting"></a>문제 해결

아래 목록에는 일반적인 문제, 해결 방법 및 위한 팁과 요령을 Profiler를 사용 하 여 보여 줍니다.

> [!NOTE]
> Visual Studio 해야 **Enterprise** mac 용 Windows 하거나 Visual Studio Enterprise 또는 Visual Studio에서이 기능을 잠금 해제를 위해 구독자

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>IOS 프로파일러 옵션을 볼 수 없는 또는 그는 회색으로 [Visual Studio 및 Mac 용 Visual Studio]

이 문제를 해결할 수 있도록 다음 설정을 확인 합니다.

- 디버그 구성을 사용 하 고 있는지 확인 합니다.
- SGen 가비지 수집기를 사용 하 고 있는지 확인 합니다.
- 플랫폼 확인 [지원](~/tools/profiler/index.md#Profiler_Support)합니다.
- 올바른 라이선스가 있는지 확인 합니다.
- 와 올바르게 인증 된 로그온 했는지 확인 합니다.
- [Visual Studio] 사용 해야 [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/enterprise/) 있고 유효한 엔터프라이즈 라이선스가 있어야 합니다.

#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>프로파일러를 시작 하려고 할 때 오류가 발생

Visual Studio에서 프로파일러를 사용 하는 경우이 오류 상자에 실행 합니다.

![](troubleshooting-images/error.png "Visual Studio에서 프로파일러를 사용 하는 경우 오류 상자")

시뮬레이터를 시작 하는 것으로 인해 일반적으로 / 에뮬레이터. 시도 하 고 앱을 정상적으로 실행에 제공 하 고 다시는 Profiler를 사용 하려고 하는 문제를 해결 합니다.

#### <a name="to-watch-a-specific-thread"></a>특정 스레드를 조사 하려면

스레드는 특히 시청 하려는 경우 것이 이상적 이름을 가져오려면 생성 시작할 때 스레드 `ThreadName` 대신 `0x0`합니다. 예를 들어도 스레드 이름을 설정 하려면 `UI`, 다음 코드를 사용할 수 있습니다.

```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```

## <a name="related-links"></a>관련 링크

- [연습-Xamarin Profiler 사용](~/tools/profiler/index.md)
- [메모리 및 성능 모범 사례](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [릴리스 정보](https://developer.xamarin.com/releases/profiler/preview/)
