---
title: "Xamarin 프로파일러 문제 해결"
description: "Xamarin 프로파일러 문제 해결"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 10/27/2017
ms.openlocfilehash: a6858a38575b723e81dea5e08b27a129f07e44b8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-profiler-troubleshooting"></a>Xamarin 프로파일러 문제 해결

_Xamarin 프로파일러 문제 해결_

## <a name="logging-and-diagnostics"></a>로깅 및 진단

Xamarin 팀은 우리 정보를 제공 하는 경우 문제를 추적 하는 데 도움이 수 포함 하 여:

- 문제, 충돌, 또는 오류 및이 워크플로에 캐스트 합니다.
- 로그 (아래 참조)를 출력합니다.
- **.mlpd** 프로 파일링 세션 (아래 참조)에 대해 생성 되 고 있습니다.

### <a name="getting-log-outputs"></a>로그 출력 가져오기
Mac에서 로그에 저장 됩니다 `~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`합니다.

Windows에 저장 된 이러한 `%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log` 문제를 제출 때마다 최신 로그를 포함 하세요.

자세한 로깅 있네요 하므로이 출력은 증가 하 고 시간이 더 유용 하 게 될 때 추가할 예정입니다.

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>.Mlpd 파일 생성

**.mlpd** 파일은 압축 된 모노 런타임 프로파일러 출력 합니다. Xamarin 프로파일러 GUI에서 데이터를 읽는 **.mlpd** 사용자에 대해 표시 됩니다. **.mlpd** 파일은 엔지니어가 프로파일러 데이터와 함께 있을 수는 문제를 진단할 수 있기 때문에 Xamarin에 대 한 디버깅 도구 유용 합니다.

**.mlpd** 현재 세션은 Mac의 자동 저장에 대 한 `/tmp` 디렉터리는 타임 스탬프에서 식별할 수 있습니다. 로깅을 설정 하는 경우 첫 번째 출력에 대 한 경로 됩니다는 **.mlpd** 파일입니다. **.mlpd** 파일은 일반적으로 ~/var/폴더를 시작 하는 디렉터리에 저장 중...

**.mlpd** 선택 하 여 현재 세션으로 저장할 수도 있습니다에 대 한 **파일 > 다른 이름으로 저장...** 메뉴:

**Visual Studio for Mac**:

![](troubleshooting-images/image17.png "Mac 용 Visual Studio에서.mlpd 파일을 저장 하는 중")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Visual Studio에서.mlpd 파일을 저장합니다.")


사항에 유의 해야 **.mlpd** 많은 정보를 포함 하 고 파일 크기는 큰 됩니다.

## <a name="troubleshooting"></a>문제 해결

아래 목록에서는 일반적인 문제, 해결 방안 및 위한 팁과 요령 프로파일러를 사용 하 여 보여 줍니다.

> [!NOTE]
> **참고**: Visual Studio 있어야 **엔터프라이즈** 구독자 Mac.에 대 한 Windows의 어느 Visual Studio Enterprise 또는 Visual Studio에서이 기능을 잠금 해제 하려면

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>IOS 프로파일러 옵션을 볼 수 없는 또는 것은 회색으로 표시 [Visual Studio 및 Mac 용 Visual Studio]

이 문제를 해결할 수 있도록 다음 설정을 확인 합니다.

- 디버그 구성을 사용 하 고 있는지 확인 하십시오.
- SGen 가비지 수집기를 사용 하 고 있는지 확인 합니다.
- 플랫폼은 확인 [지원](~/tools/profiler/index.md#Profiler_Support)합니다.
- 올바른 라이선스가 있는지 확인 합니다.
- 인증에 고 적절히 로그온 했는지 확인 합니다.
- [Visual Studio] 사용 해야 [Visual Studio Enterprise](https://www.visualstudio.com/vs/enterprise/) 및 Enterprise 라이선스를가지고 있습니다.


#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>프로파일러를 시작 하려고 할 때 오류가 발생

Visual Studio에서 프로파일러를 사용 하는 경우이 오류 상자를 실행 합니다.

![](troubleshooting-images/error.png "Visual Studio에서 프로파일러를 사용 하는 경우 오류 상자")

일반적으로 시뮬레이터를 시작 하려면 수 없어서 / 에뮬레이터입니다. 시도 하는 앱을 정상적으로 실행를 제공 하며, 하 고 프로파일러를 다시 사용 하려고 하는 문제를 해결 합니다.

#### <a name="to-watch-a-specific-thread"></a>특정 스레드를 조사 하려면

특히 시청 하고자 하는 스레드를 사용 하도록 설정한 경우 것이 이상적는 매우 시작 하 고 생성 get 얻을 수 있도록에 스레드 이름을 `ThreadName` 대신 `0x0`합니다. 예를 들어 ui 스레드 이름을 설정 하려면, 다음 코드를 사용할 수 있습니다.


```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```



## <a name="related-links"></a>관련 링크

- [Xamarin 프로파일러를 사용 하 여 연습-](~/tools/profiler/index.md)
- [메모리 및 성능 모범 사례](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [릴리스 정보](https://developer.xamarin.com/releases/profiler/preview/)
