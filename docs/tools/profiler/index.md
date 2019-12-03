---
title: Xamarin Profiler
description: 이 가이드에서는 Xamarin Profiler의 주요 기능을 살펴봅니다. 프로파일러, 프로 파일링 및 사용 해야 하는 경우 및 Xamarin 응용 프로그램 프로 파일링을 위한 표준 워크플로에서 살펴봅니다.
ms.prod: xamarin
ms.assetid: 3247fcee-6acc-470d-ab87-c1c511d67363
author: davidortinau
ms.author: daortin
ms.date: 06/03/2018
ms.openlocfilehash: 8927e7b2a1b194d1bfab334736c3d024f0542b01
ms.sourcegitcommit: 60e955ce65194ffea987409157ccc7d5db87c2ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74690207"
---
# <a name="xamarin-profiler"></a>Xamarin Profiler

_이 가이드에서는 Xamarin Profiler의 주요 기능을 살펴봅니다. 프로파일러, 프로 파일링 및 사용 해야 하는 경우 및 Xamarin 응용 프로그램 프로 파일링을 위한 표준 워크플로에서 살펴봅니다._

응용 프로그램의 성공은 최종 사용자 환경에 따라 달라 집니다. 개발자는 앱에서 몇 가지 놀라운 기능을 구현 했을 수 있지만 앱이 느려지거나 작동이 중단 되는 경우 사용자가이를 제거할 가능성이 높습니다.

지금까지 mono는 mono [로그 프로파일러](https://www.mono-project.com/docs/debug+profile/profile/profiler/)라고 하는 mono 런타임에서 실행 되는 프로그램에 대 한 정보를 수집 하기 위한 강력한 명령줄 프로파일러를 제공 합니다. Xamarin Profiler Mono 로그 프로파일러를 위한 그래픽 인터페이스 이며, Mac에서 Android, iOS, tvOS 및 Mac 응용 프로그램을 프로 파일링 하 고 Windows에서 Android, iOS 및 tvOS 응용 프로그램을 프로 파일링 하도록 지원 합니다.

Xamarin Profiler에는 프로 파일링에 사용할 수 있는 여러 가지 악기 (할당, 주기 및 시간 프로파일러)가 있습니다. 이 가이드에서는 이러한 계측의 기능 및 응용 프로그램을 분석 하는 방법을 알아보고 각 화면에 표시 되는 데이터의 의미를 명확 하 게 설명 합니다.

이 가이드는 일반적인 프로 파일링 시나리오를 검사 하 고 프로파일러를 iOS 및 Android 응용 프로그램을 분석 하 고 최적화 하는 데 도움이 되는 도구로 도입 합니다.

## <a name="download-and-install"></a>다운로드 및 설치

> [!NOTE]
> Windows의 Visual Studio Enterprise 또는 Mac에서 Mac용 Visual Studio에서이 기능의 잠금을 해제 하려면 [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/) 구독자 여야 합니다.

Xamarin Profiler는 독립 실행형 응용 프로그램으로, IDE 내에서 프로 파일링을 사용할 수 있도록 Mac용 Visual Studio 및 Visual Studio와 통합 됩니다.

플랫폼용 설치 패키지를 다운로드 합니다.

- [**macOS**](https://dl.xamarin.com/profiler/profiler-mac-1.6.10-15.pkg)
- [**Windows**](https://dl.xamarin.com/profiler/XamarinProfiler.Windows.Installer.1.6.10-15.msi)

다운로드 되 면 설치 관리자를 시작 하 여 시스템에 Xamarin Profiler를 추가 합니다.

## <a name="profilers-and-profiling"></a>프로파일러 및 프로 파일링

프로 파일링은 응용 프로그램 개발에서 자주 간과 되는 중요 한 단계입니다. 프로 파일링은 **동적 프로그램 분석** 의 일종으로, 실행 중이 고 사용 중인 프로그램을 분석 합니다. 프로파일러는 시간 복잡성, 특정 메서드 사용 및 할당 된 메모리에 대 한 정보를 수집 하는 데이터 마이닝 도구입니다. 프로파일러를 사용 하면 이러한 메트릭을 자세히 드릴 하 고 분석 하 여 코드의 문제 영역을 파악할 수 있습니다.

응용 프로그램을 디자인 하 고 개발 하는 경우에는 조기에 최적화 하지 않는 것이 중요 합니다. 즉, 거의 액세스 하지 않을 영역에서 코드를 개발 하는 데 시간이 소요 됩니다. 이는 프로 파일링의 기능입니다. 프로파일러는 코드 베이스에서 가장 일반적으로 사용 되는 부분에 대 한 통찰력을 제공 하 고 향상 된 시간을 소요 하는 영역을 찾는 데 도움이 됩니다. 개발자는 응용 프로그램에서 대부분의 시간이 소요 되는 위치와 응용 프로그램에서 메모리를 사용 하는 방법을 이해 해야 합니다.

프로 파일링은 모든 유형의 개발에 유용 하지만 모바일 개발에서 특히 중요 합니다. 최적화 되지 않은 코드는 데스크톱 컴퓨터 보다 모바일 플랫폼에서 훨씬 더 눈에 띄게 되며, 앱의 성공은 효율적으로 실행 되는 멋진 코드와 최적화 된 코드에 따라 달라 집니다.

## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin Profiler는 개발자에 게 Mac용 Visual Studio 또는 Visual Studio 내부에서 응용 프로그램을 프로 파일링 하는 방법을 제공 합니다. 프로파일러는 응용 프로그램의 동작을 분석 하는 데 사용할 수 있는 앱에 대 한 정보를 수집 하 고 표시 합니다. 응용 Xamarin Profiler 프로그램을 프로 파일링 하는 방법에는 메모리 프로 파일링 및 통계 샘플링과 같은 여러 가지 방법이 있습니다. 이러한 설정은 각각 할당 및 시간 프로파일러 계측을 통해 수행 됩니다.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

현재 Xamarin Profiler를 사용 하 여 Mac에서 Xamarin.ios, Xamarin Android 및 Xamarin.ios 응용 프로그램을 테스트할 수 있습니다 (Mac용 Visual Studio를 통해). 프로파일러는 IDE와는 별개의 프로세스 이며, Mac용 Visual Studio에서 시작 하는 것 외에도 독립 실행형 응용 프로그램으로 사용 하 여 [mono 로그 프로파일러에서](https://www.mono-project.com/docs/debug+profile/profile/profiler/)생성 된 .exe 및 `.mlpd` 파일을 검사할 수 있습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

현재 Xamarin Profiler는 Windows에서 Xamarin Android 앱을 테스트 하는 데 사용할 수 있습니다 (Visual Studio 및 Mac용 Visual Studio를 통해). 프로파일러는 IDE와는 별개의 프로세스 이며, Visual Studio에서 시작 하는 것 외에도 [mono 로그 프로파일러에서](https://www.mono-project.com/docs/debug+profile/profile/profiler/)생성 된 .exe 및 `.mlpd` 파일을 검사 하는 독립 실행형 응용 프로그램으로 사용할 수 있습니다.

-----

<a name="Profiler_Support" />

## <a name="profiler-support"></a>프로파일러 지원

Xamarin Profiler에 대 한 지원은 다음 플랫폼에서 제공 됩니다.

- Mac용 Visual Studio (macOS, Enterprise License)
  - Android
    - 장치 및 에뮬레이터
  - iOS
    - 장치 및 시뮬레이터
  - tvOS (시간 계측은 지원 되지 않음)
    - 장치 및 시뮬레이터
  - Mac

- Visual Studio ( **Enterprise** 버전에만 해당)
  - Android
    - 장치 및 에뮬레이터
  - iOS [실험적]
    - 장치 및 시뮬레이터
  - tvOS
    - 장치 및 시뮬레이터

**디버그** 구성 **만** 프로 파일링 할 수 있습니다.

## <a name="profiler-basics"></a>프로파일러 기본 사항

이 섹션에서는 Xamarin Profiler의 일부를 소개 하 고 해당 기능을 둘러보기.

### <a name="allow-profiling-in-your-app"></a>앱에서 프로 파일링 허용

앱을 성공적으로 프로 파일링 하려면 앱의 프로젝트 옵션에서 프로 파일링을 허용 해야 합니다.

- iOS:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  **프로 파일링을 사용 하도록 설정 하 > iOS 디버그 > 빌드**

  ![Mac용 Visual Studio의 iOS 옵션 대화 상자](images/ios-options-mac.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  **프로 파일링을 사용 하도록 설정 > iOS 빌드 > 속성**

  ![Visual Studio의 iOS 옵션 대화 상자](images/ios-project-options-vs.png)

-----

- Android:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  **개발자 계측을 사용 하도록 설정 하 > Android 디버그 > 빌드**

  ![Mac용 Visual Studio의 Android 옵션 대화 상자](images/android-project-options.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  **개발자 계측을 사용 하도록 설정 하 > Android 디버그 > 빌드**

  ![Mac용 Visual Studio의 Android 옵션 대화 상자](images/android-project-options-vs-sml.png)

-----

### <a name="launching-the-profiler"></a>프로파일러 시작

IOS 또는 Android 응용 프로그램을 프로 파일링 하거나 독립 실행형 응용 프로그램으로 프로 파일링 할 때 IDE에서 Xamarin Profiler를 시작할 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

#### <a name="launching-from-visual-studio-for-mac"></a>Mac용 Visual Studio에서 시작

1. 먼저 Mac용 Visual Studio에서 응용 프로그램을 로드 했는지 확인 하 고 (기본값) 디버그 구성을 선택 합니다.
2. 아래 다이어그램에 나와 있는 것 처럼 Mac용 Visual Studio에서 **프로 파일링을 시작**하거나 Visual Studio에서 **> Xamarin Profiler를 분석** 하 여 프로파일러를 열 > 실행 합니다.

  ![Mac용 Visual Studio에서 프로파일러 시작](images/start-profiling-xs.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

#### <a name="launching-from-visual-studio"></a>Visual Studio에서 시작

1. 먼저 Visual Studio에서 응용 프로그램을 로드 했는지 확인 하 고 위에 지정 된 대로 (기본값) 디버그 구성을 선택 합니다.
2. 아래 다이어그램에 나와 있는 것 처럼 Visual Studio에서 **Xamarin Profiler > 분석** 을 찾아 프로파일러를 엽니다.

![Visual Studio에서 프로파일러 시작](images/start-profiling-vs.png)

-----

메뉴 항목이 표시 되지 않는 경우 [문제 해결 가이드](~/tools/profiler/troubleshooting.md)를 참조 하세요.

그러면 프로파일러가 시작 되 고 응용 프로그램의 프로 파일링이 자동으로 시작 됩니다.

프로파일러는 메모리 및 성능을 측정 하는 데 사용할 수 있습니다. 다음 섹션에서 자세히 설명 하는 할당 및 시간 프로파일러 계측을 통해이를 달성 합니다.

#### <a name="saving-and-loading-profiler-sessions"></a>프로파일러 세션 저장 및 로드

언제 든 지 프로 파일링 세션을 저장 하려면 프로파일러 메뉴 모음에서 **파일 > 다른 이름으로 저장 ...** 을 선택 합니다. 이렇게 하면 프로 파일링 데이터에 대 한 특수 하 고 매우 압축 된 형식인 _.mlpd_ 형식으로 파일을 저장 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

설치 된 후에는 아래 스크린샷에 나와 있는 것 처럼 응용 프로그램 폴더에서 Xamarin Profiler를 찾을 수 있습니다.

![Mac에서 독립 실행형 프로파일러 열기](images/applications.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

응용 프로그램을 설치한 후에는 응용 프로그램 디렉터리에서 Xamarin Profiler 응용 프로그램을 찾을 수 있습니다.

![Windows에서 독립 실행형 프로파일러 열기](images/applications-vs.png)

-----

독립 실행형 응용 프로그램을 열고 **대상 선택** 및 파일 로드를 선택 하 여 *. Mlpd* 파일을 프로파일러에 로드할 수 있습니다.

자세한 내용은 [mlpd 파일 생성](~/tools/profiler/troubleshooting.md#gen_mlpd)을 참조 하세요.

## <a name="profiler-features"></a>프로파일러 기능

Xamarin Profiler은 아래 그림과 같이 5 개의 섹션으로 구성 됩니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[Mac용 Visual Studio의 프로파일러 섹션 ![](images/profiler-mac-sml.png)](images/profiler-mac.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[Visual Studio의 ![Profiler 섹션](images/profiler-vs.png)](images/profiler-vs.png#lightbox)

-----

- **도구 모음** – 프로파일러 맨 위에 있는 옵션을 제공 하 여 프로 파일링 시작/중지, 대상 프로세스 선택, 앱 실행 시간 보기, 프로파일러 응용 프로그램을 구성 하는 분할 뷰 선택 옵션을 제공 합니다.
- **계측 목록** – 프로 파일링 세션에 대해 로드 된 모든 계측을 나열 합니다.
- **차트 플롯** – 이러한 차트는 계측 목록의 관련 악기에 가로로 관련 됩니다. 슬라이더 (시간 프로파일러 아래에 표시 됨)를 사용 하 여 소수 자릿수를 변경할 수 있습니다.
- **계측 세부 정보 영역** -현재 계측의 선택 된 보기에 의해 표시 되는 데이터를 포함 합니다. 아래 섹션에서 이러한 보기를 자세히 살펴보겠습니다.
- **Inspector 보기** – 분할 된 컨트롤에서 선택할 수 있는 섹션이 포함 되어 있습니다. 섹션은 선택한 계측에 따라 다르며 구성 설정, 통계, 스택 추적 정보 및 루트 경로를 포함 합니다.

### <a name="allocations"></a>피크

할당 계측은 응용 프로그램이 만들어지고 가비지 수집 될 때 응용 프로그램의 개체에 대 한 자세한 정보를 제공 합니다.

프로파일러 맨 위에는 할당 차트가 있습니다 .이 차트는 프로 파일링 중에 일정 한 간격으로 할당 된 메모리의 양을 표시 합니다. 현재 할당 그래프는 해당 시점의 힙 크기가 아니라 총 할당 수입니다. 따라서 중단 되지 않으며 계속 증가 합니다. 여기에는 스택에 할당 된 개체가 포함 됩니다. 사용 되는 런타임 버전에 따라 동일한 앱에 대해서도 차트가 다르게 보일 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![할당 계측](images/allocations1.png)](images/allocations1.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![할당 계측](images/allocations1-vs.png)](images/allocations1-vs.png#lightbox)

-----

개발자가 응용 프로그램에서 메모리를 사용 하 고 해제 하는 방법을 분석할 수 있도록 하는 할당 계측에는 다양 한 데이터 뷰가 있습니다. 이러한 보기는 아래에 설명 되어 있습니다.

- **할당** -모든 할당 목록을 표시 하 고 클래스 이름별로 그룹화 합니다. 사용 되는 클래스 및 메서드, 사용 빈도 및 사용 되는 클래스의 집합적 크기에 대 한 유용한 개요를 제공 합니다. 클래스를 두 번 클릭 하면 할당 된 메모리가 표시 됩니다. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![할당 탭](images/allocations3.png)](images/allocations3.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![할당 탭](images/allocations2-vs.png)](images/allocations2-vs.png#lightbox)

-----

할당에 대 한 검사기 보기는 개체를 필터링 및 그룹화 하 고, 할당 된 메모리에 대 한 통계를 제공 하 고, 스택 추적 및 루트 경로에 대 한 뷰를 제공 하는 옵션을 제공 합니다.

- **호출 트리** – 응용 프로그램의 모든 스레드에 대 한 전체 호출 트리를 표시 하 고 각 노드에 할당 된 메모리에 대 한 정보를 포함 합니다. 목록에서 요소를 선택 하면 모든 형제 노드가 회색으로 표시 됩니다. 트리를 확장 하거나 요소를 두 번 클릭 하 여 드릴 다운할 수 있습니다. 이 데이터 뷰를 표시 하는 경우 디스플레이 설정 검사자 보기를 사용 하 여 표시 되는 방식을 변경할 수 있습니다. 현재 다음과 같은 두 가지 옵션이 있습니다.
    1. **반전 된 호출 트리** -스택 추적을 위에서 아래로 간주 합니다. 이 옵션은 CPU가 시간을 소비 하는 가장 깊은 메서드를 나타내므로 편리 하 게 볼 수 있습니다.
    2. **스레드별로 분리** –이 옵션은 스레드를 통해 호출 트리를 구성 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![호출 트리 탭](images/allocations2.png)](images/allocations2.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![호출 트리 탭](images/allocations3-vs.png)](images/allocations3-vs.png#lightbox)

-----

- **스냅숏** -이 창에는 메모리 스냅숏에 대 한 정보가 표시 됩니다. 라이브 응용 프로그램을 프로 파일링 하는 동안 이러한 항목을 생성 하려면 유지 되 고 릴리스되는 메모리를 확인 하려는 각 지점에서 _카메라_ 단추를 클릭 합니다. 그런 다음 각 스냅숏을 클릭 하 여 내부적으로 발생 하는 상황을 탐색할 수 있습니다. 스냅숏은 앱을 실시간으로 프로 파일링 하는 경우에만 수행할 수 있습니다. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![스냅숏 탭](images/allocations4.png)](images/allocations4.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![스냅숏 탭](images/allocations4-vs.png)](images/allocations4-vs.png#lightbox)

-----

### <a name="time-profiler"></a>시간 프로파일러

시간 프로파일러 계측은 응용 프로그램의 각 메서드에서 얼마나 많은 시간이 소요 되는지 정확 하 게 측정 합니다. 응용 프로그램은 일정 한 간격으로 일시 중지 되 고 각 활성 스레드에서 스택 추적이 실행 됩니다. 계측 세부 정보 영역의 각 행에는 이어지는 실행 경로가 표시 됩니다.

아래 스크린샷에 표시 된 그림 차트에는 앱이 실행 될 때 앱에서 받은 샘플 수가 표시 됩니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![시간 프로파일러 계측](images/time1.png)](images/time1.png#lightbox) 

[![Time Profiler 계측 – 샘플 목록](images/time3.png)](images/time3.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![시간 프로파일러 계측](images/time1-vs.png)](images/time1-vs.png#lightbox) 

[![Time Profiler 계측 – 샘플 목록](images/time3-vs.png)](images/time3-vs.png#lightbox) 

-----

- **호출 트리** – 각 메서드에서 소요 된 시간을 표시 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![Time Profiler 계측 – 호출 트리](images/time2.png)](images/time2.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![Time Profiler 계측 – 호출 트리](images/time2-vs.png)](images/time2-vs.png#lightbox) 

-----

### <a name="cycles"></a>주기

C# 및 F# 관리 코드를 사용 하면 매우 일반적 일 수 있으며, 삭제 되지 않는 개체에 대 한 참조를 만드는 것은 매우 쉽습니다. 이 계측을 통해 해당 개체를 파악 하 고 응용 프로그램에서 참조 되는 주기를 표시할 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![주기 계측](images/cycles.m751-sml.png)](images/cycles.m751.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![주기 계측](images/cycles-vs-sml.png)](images/cycles-vs.png#lightbox) 

-----

## <a name="profiling-applications"></a>응용 프로그램 프로 파일링

현재는 기본 디버그 구성만 프로 파일링 할 수 있습니다.

다른 구성으로 앱을 프로 파일링 하는 경우 다음과 같은 메시지 대화 상자가 표시 됩니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![프로 파일링 오류 대화 상자](images/image001.png)](images/image001.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![프로 파일링 오류 대화 상자](images/image1vs.png)](images/image1vs.png#lightbox) 

-----

**업데이트** 를 선택 하 여 계속 합니다.

### <a name="sgen-garbage-collector-and-profiling"></a>SGen 가비지 수집기 및 프로 파일링

[SGen](https://www.mono-project.com/docs/advanced/garbage-collector/sgen/) 가비지 수집기는 모든 Xamarin 플랫폼에 사용 됩니다.

SGen는 세대 GC로, 응용 프로그램의 개체를 Nursery, 주요 힙 및 큰 개체 공간의 세 힙에 할당 합니다. 이렇게 하면 가비지 수집을 speedier 실행할 수 있습니다. SGen는 현재 Xamarin.ios 및 Xamarin.ios 통합 응용 프로그램에 대 한 기본 GC입니다.

Classic API 사용 하는 xamarin.ios 응용 프로그램은 Boehm GC – 세대이 아닌 보수적인 가비지 수집기를 사용 했습니다. 기본적으로 사용 가능한 메모리를 확보 하는 것은 적지만 프로파일러를 사용할 때 결과가 부정확 해질 수 있습니다. 이러한 이유로 할당 계측은 Boehm 가비지 수집기에서 사용할 수 없습니다.

앱이 Boehm GC를 사용 하는 경우 메시지 대화 상자가 표시 되지만, Xamarin에서는 신중한 연구 및 철저 한 테스트 없이 Boehm를 사용 하는 기존 iOS 응용 프로그램을 SGen로 전환 하지 않는 것이 좋습니다. 또한 Xamarin은 프로 파일링을 위해 SGen로 전환한 후 다시 전환 하는 것을 권장 하지 않습니다. 이러한 결과가 메모리 사용에 대 한 정확한 벤치 마크를 제공 하지 않기 때문입니다.

메모리 관리에 대 한 자세한 내용은 [메모리 및 성능 모범 사례](~/cross-platform/deploy-test/memory-perf-best-practices.md) 가이드를 참조 하세요.

## <a name="summary"></a>요약

이 가이드에서는 프로 파일링 이란 무엇 이며 개발자에 게 어떤 도움이 되는지 살펴봅니다. 그런 다음 Xamarin Profiler를 도입 하 여 작동 방식에 대 한 기록과 정보를 제공 합니다. 마지막으로 Xamarin Profiler의 기능을 홍콩과 순회할 하 고 할당 및 시간 프로파일러 계측을 탐색 했습니다.

## <a name="related-links"></a>관련 링크

- [메모리 및 성능 모범 사례](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [릴리스 정보](/xamarin/tools/profiler/release-notes/)
