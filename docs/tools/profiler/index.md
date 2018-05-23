---
title: Xamarin Profiler
description: 이 가이드는 Xamarin 프로파일러의 주요 기능을 탐색합니다. Xamarin 응용 프로그램 프로 파일링에 대 한 프로파일러, 프로 파일링 / 때 사용 해야, 및 표준 워크플로 찾습니다.
ms.prod: xamarin
ms.assetid: 3247fcee-6acc-470d-ab87-c1c511d67363
author: topgenorth
ms.author: toopge
ms.date: 10/27/2017
ms.openlocfilehash: 81c6a5682fc91b49a0f7495f06e7f7b6d3f76330
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2018
---
# <a name="xamarin-profiler"></a>Xamarin Profiler

_이 가이드는 Xamarin 프로파일러의 주요 기능을 탐색합니다. Xamarin 응용 프로그램 프로 파일링에 대 한 프로파일러, 프로 파일링 / 때 사용 해야, 및 표준 워크플로 찾습니다._

최종 사용자 환경에 응용 프로그램의 성공 여부에 따라 다릅니다. 개발자 있습니다 구현 했을 수 실제로 훌륭한 기능 일부 응용 프로그램에서 하지만 응용 프로그램 중단 이나 충돌의 경우 사용자는 가능성이 없앨 수 있습니다.

지금까지 모노가을 갖춘 강력한 명령줄 프로파일러 호출 된 모노 런타임에서 실행 되는 프로그램에 대 한 정보를 수집는 [모노 로그 프로파일러](http://www.mono-project.com/docs/debug+profile/profile/profiler/)합니다. Xamarin 프로파일러 모노 로그 프로파일러 및 Android, iOS, tvOS, 및 Mac 및 Android, ios, Mac 응용 프로그램 및 Windows에서 tvOS 응용 프로그램을 프로 파일링 지원에 대 한 그래픽 인터페이스를 표시 합니다.

Xamarin 프로파일러는 프로 파일링 하기 위해 사용할 수 있는 계기의 개수가-할당, 주기 및 시간 프로파일러 합니다. 이 가이드는 이러한 계기의 측정 및 응용 프로그램을 분석 하는 방법을 설명 하 고 각 화면에 표시 되는 데이터의 의미를 명확히 합니다.

이 가이드를 프로 파일링 하는 일반적인 시나리오를 검사 하 고 분석 하 고 iOS 및 Android 응용 프로그램을 최적화 하는 도구로 프로파일러 소개 합니다.

## <a name="download-and-install"></a>다운로드 및 설치

> [!NOTE]
> 해야 합니다는 [Visual Studio Enterprise](https://www.visualstudio.com/vs/compare/) mac에서 Mac 용 Windows의 어느 Visual Studio Enterprise 또는 Visual Studio에서이 기능을 잠금 해제를 위해 구독자

Xamarin 프로파일러는 독립 실행형 응용 프로그램 되며 Mac 용 Visual Studio 및 Visual Studio IDE 내에서 프로 파일링 할 수 있도록 통합 됩니다.

플랫폼에 대 한 설치 패키지를 다운로드 합니다.

- [**macOS**](https://dl.xamarin.com/profiler/profiler-mac.pkg)
- [**Windows**](https://dl.xamarin.com/profiler/profiler-windows.msi)

를 다운로드 한 설치 관리자를 시스템 Xamarin 프로파일러 추가를 시작 합니다.

## <a name="profilers-and-profiling"></a>프로파일러 및 프로 파일링

프로 파일링은 응용 프로그램을 개발 하는 중요 하 고 종종 간과 하기 단계입니다. 프로 파일링는 형태의 **동적 프로그램 분석** -실행 중 이며 사용 중인 동안에 프로그램을 분석 합니다. 프로파일러는 시간 복잡도, 특정 메서드를 사용 중인 할당 된 메모리에 대 한 정보를 수집 하는 데이터 마이닝 도구. 프로파일러를 사용 하면 심층 드릴 하 고 코드에서 문제 영역을 정확 하 게 이러한 메트릭을 분석할 수 있습니다.

중간; 최적화 하지 반드시를 디자인 하 고 응용 프로그램을 개발 하는 경우 즉, 거의 액세스할 수 있는 영역에서 코드를 개발 하는 시간을 소비 합니다. 프로 파일링의 거듭제곱입니다. 프로파일러 가장에 대 한 정보는 일반적으로 코드 베이스를 부분의 사용 찾은 수 시간 함으로써 기능 향상을 할애 해야 있는 영역을 제공 합니다. 개발자는 대부분의 응용 프로그램에서 소요 된 위치와 응용 프로그램에서 메모리 사용 되는 방법을 이해 하려면 주의 해야 합니다.

프로 파일링 하는 것은 모든 유형의 개발에 유용 하지만 모바일 개발에 특히 중요 합니다. 최적화 되지 않은 코드는 데스크톱 컴퓨터에 보다 모바일 플랫폼에서 훨씬 더욱 분명 하 게 하 고 효율적으로 실행 되는 아름 다운 하 고 최적화 된 코드에 응용 프로그램의 성공 여부에 따라 달라 집니다.

## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin 프로파일러에서 응용 프로그램 프로필 수 있는 방법을 개발자에 게 제공 Mac 또는 Visual Studio에 대 한 Visual Studio 내 합니다. 프로파일러가 수집 하 고 사용할 수 있는 다음 개발자가 응용 프로그램의 동작을 분석 하는 앱에 대 한 정보를 표시 합니다. Xamarin 프로파일러는 응용 프로그램을 프로 파일링 하려면 다양 한 방법으로 여러 가지 즉 메모리 프로 파일링 및 통계 샘플링 합니다. 할당 및 시간 프로파일러를 통해 수행 됩니다이 각각를 계측 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

현재, Mac (을 통해 Visual Studio Mac 용)에서 Xamarin.iOS, Xamarin.Android, 및 Xamarin.Mac 응용 프로그램을 테스트 하려면 Xamarin 프로파일러를 사용할 수 있습니다. 프로파일러는 IDE에서 별도 프로세스 및, Mac 용 Visual Studio에서 시작 하는 것 외에도 사용할 수 있습니다는 독립 실행형 응용 프로그램으로.exe를 검사 하 고 `.mlpd` 생성 된 파일에서 파일은 [모노 로그 프로파일러](http://www.mono-project.com/docs/debug+profile/profile/profiler/).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

현재, Xamarin 프로파일러 Xamarin.Android에서 앱을 테스트 (Visual Studio 및 Mac 용 Visual Studio)를 통해 Windows 데 사용할 수 있습니다. 프로파일러 IDE에서 별도 프로세스 이며, Visual Studio를 시작 하는 것 외에도 사용할 수 있습니다는 독립 실행형 응용 프로그램으로.exe를 검사 하 고 `.mlpd` 생성 된 파일에서 파일의 [모노 로그 프로파일러](http://www.mono-project.com/docs/debug+profile/profile/profiler/) .

-----

<a name="Profiler_Support" />

## <a name="profiler-support"></a>프로파일러 지원

Xamarin 프로파일러에 대 한 지원은 다음 플랫폼에서 제공 됩니다.

 - Mac (macOS, 엔터프라이즈 라이선스가 있는)에 대 한 visual Studio
    - Android
        - 장치 및 에뮬레이터
    - iOS
        - 장치 및 시뮬레이터
    - tvOS (시간 계측 지원 되지 않습니다.)
        - 장치 및 시뮬레이터
    - Mac

 - Visual Studio (만 **엔터프라이즈** 버전)
    - Android
        - 장치 및 에뮬레이터
    - iOS [실험적]
        - 장치 및 시뮬레이터
    - tvOS
        - 장치 및 시뮬레이터

설정할 수 있는 참고 **만** 프로필 **디버그** 구성 합니다.

## <a name="profiler-basics"></a>프로파일러 기본 사항

이 섹션 Xamarin 프로파일러의 부분을 소개 하 고 해당 기능 tours 합니다.

### <a name="allow-profiling-in-your-app"></a>응용 프로그램에서 Profiling 허용

응용 프로그램을 성공적으로 프로 파일링 하기 전에 응용 프로그램의 프로젝트 옵션에서 프로 파일링을 허용 해야 합니다.

 - iOS:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  **빌드 > iOS 디버그 > 프로 파일링 사용**

  ![](images/ios-options-mac.png "iOS, Mac 용 Visual Studio에서 옵션 대화 상자")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **속성 > iOS 빌드 > 프로 파일링 사용**

  ![](images/ios-project-options-vs.png "iOS에서 Visual Studio 옵션 대화 상자")

-----

 - Android:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  **빌드 > Android 디버그 > 개발자 Instrumentation 사용**

  ![Mac 용 Visual Studio에서 android 옵션 대화 상자](images/android-project-options.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **빌드 > Android 디버그 > 개발자 Instrumentation 사용**

  ![Mac 용 Visual Studio에서 android 옵션 대화 상자](images/android-project-options-vs-sml.png)

-----

### <a name="launching-the-profiler"></a>프로파일러를 시작합니다.

Xamarin 프로파일러는 iOS 또는 Android 응용 프로그램 프로 파일링 하는 경우 IDE에서 또는 독립 실행형 응용 프로그램으로 실행할 수 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="launching-from-visual-studio-for-mac"></a>Mac 용 Visual Studio에서 시작

1. 먼저, 응용 프로그램을 Visual Studio에서, Mac 용 로드 하 고 (기본값) 디버그 구성을 선택 해야 합니다.
2. 찾아 **실행 > 프로 파일링 시작**, Mac 용 Visual Studio에서 또는 **분석 > Xamarin 프로파일러** 아래 다이어그램에 볼 수 있듯이 프로파일러를 열려면 Visual Studio에서:

  ![](images/start-profiling-xs.png "Mac 용 Visual Studio에서 프로파일러를 시작합니다.")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="launching-from-visual-studio"></a>Visual Studio에서 시작

1.  먼저, 응용 프로그램을 Visual Studio에서 로드 하 고 위에서 (기본값) 디버그 구성을 선택 해야 합니다.
2.  찾아 **분석 > Xamarin 프로파일러** 아래 다이어그램에 볼 수 있듯이 프로파일러를 열려면 Visual Studio에서:

![Visual Studio에서 프로파일러를 시작합니다.](images/start-profiling-vs.png)

-----

메뉴 항목 표시 되지 않은 경우 참조는 [문제 해결 가이드](~/tools/profiler/troubleshooting.md)합니다.

이 프로파일러를 시작 하 고 자동으로 응용 프로그램을 프로 파일링을 시작 합니다.

프로파일러를 사용 하 여 메모리 및 성능 측정할 수 있습니다. 이 시간 프로파일러 및 할당을 통해 달성 기기 다음 섹션에서 자세히 설명 합니다.

#### <a name="saving-and-loading-profiler-sessions"></a>저장 하 고 프로파일러 세션을 로드

언제 든 지 프로 파일링 세션을 저장 하려면 선택 **파일 > 다른 이름으로 저장...**  프로파일러 메뉴 모음에서 합니다. 이렇게 하면 파일에 저장 _mlpd_ 형식, 데이터 프로 파일링을 위한 특수 한 고도로 압축 된 형식입니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

설치한 후 아래 스크린샷에 표시 된 것 처럼 Xamarin 프로파일러 응용 프로그램 폴더에서 찾을 수 있습니다.

![](images/applications.png "Mac에서 독립 실행형 프로파일러를 열려면")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

설치 된 후 응용 프로그램 디렉터리에 Xamarin 프로파일러 응용 프로그램을 찾을 수 있습니다.

![](images/applications-vs.png "Windows에서 독립 실행형 프로파일러를 열려면 ")

-----

로드할 수 있습니다 *.mlpd* 독립 실행형 응용 프로그램을 여 프로파일러로 파일을 선택 하면 **대상 선택** 파일을 로드 하 고 있습니다.

자세한 내용은 참조 [.mlpd 파일 생성 ](~/tools/profiler/troubleshooting.md#gen_mlpd)합니다.

## <a name="profiler-features"></a>Profiler 기능

Xamarin 프로파일러는 아래 그림과 같이 5 개의 섹션으로 구성 됩니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](images/profiler-mac-sml.png "Mac 용 Visual Studio에서 프로파일러 섹션")](images/profiler-mac.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/profiler-vs.png "Visual Studio에서 프로파일러 섹션")](images/profiler-vs.png#lightbox)

-----

- **도구 모음** – 대상 프로세스를 선택, 앱의 실행 시간을 확인 및 profiler 응용 프로그램을 구성 하는 분할 뷰를 선택 합니다. 프로 파일링을 시작/중지 하는 옵션을 제공이 프로파일러 위쪽에 있는, 합니다.
- **목록 계측** – 프로 파일링 세션에 대해 로드 된 모든 계기를 나열 합니다.
- **데이터 계열** -이러한 차트 관련 가로로 계측 목록에서 관련 장비입니다. 눈금을 변경 하려면 (시간 프로파일러 아래에 표시) 슬라이더를 사용할 수 있습니다.
- **세부 정보 영역을 계측할** -현재 계측 선택한 뷰에서 표시 되는 데이터를 포함 합니다. 이러한 보기 아래 섹션에서 자세히 살펴보겠습니다.
- **검사기 보기** – 세그먼트 컨트롤에서 선택할 수 있는 단원이 포함 됩니다. 섹션을 선택 하는 수단과에 종속 되어 작성과: 구성 설정, 통계, 스택 추적 정보 및 루트에 대 한 경로입니다.

### <a name="allocations"></a>할당

할당 계측 가비지가 수집 하 고 생성 될 응용 프로그램의 개체에 대 한 자세한 정보를 제공 합니다.

프로파일러 위쪽에는 프로 파일링 하는 동안 정기적으로 할당 된 메모리의 크기를 표시 하는 할당 형 표시 됩니다. 현재 할당 그래프에서 해당 지점에서 할당의 총 수 및 힙 크기가 아니라 됩니다. 관점에서 이동 하지 않습니다, 그리고만 증가 합니다. 이 스택에 할당 하는 개체가 포함 됩니다. 사용 되는 런타임 버전에 따라 차트 다르게 – 동일한 앱에 대해서도 보일 수 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](images/allocations1.png "할당 방법")](images/allocations1.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/allocations1-vs.png "할당 방법")](images/allocations1-vs.png#lightbox)

-----

개발자가 자신의 응용 프로그램은 사용 하 여 하 고 메모리를 해제 하는 방법을 분석에 사용할 수 있는 다른 데이터 뷰는 할당 수단과 있습니다. 이러한 뷰는 다음과 같습니다.

 -   **할당** – 모든 할당의 목록이 표시 되며, 클래스 이름 별로 그룹화 합니다. 이 클래스와 메서드를 사용 하 고, 사용 빈도 및 사용 되는 클래스의 전체 크기의 훌륭한 개요를 제공 합니다. 클래스에 두 번 클릭 하면 표시 됩니다는 할당 된 메모리: 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/allocations3.png "할당 탭")](images/allocations3.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations2-vs.png "할당 탭")](images/allocations2-vs.png#lightbox)

-----

할당에 대 한 관리자 보기 루트에 스택 추적 및 경로 대 한 필터링 및 그룹화 개체, 할당 된 메모리에 통계를 제공 및는 최상위 할당에 대 한 옵션 뿐만 아니라 뷰를 제공 합니다.

 -   **호출 트리** – 응용 프로그램에서 모든 스레드의 전체 호출 트리 표시 되며, 각 노드에 할당 된 메모리에 대 한 정보를 포함 합니다. 요소는 목록에서 옵션을 선택 하면 모든 형제 노드 나타납니다 회색으로 표시 합니다. 트리를 확장 하거나 드릴 다운 하는 요소를 두 번 클릭 수 있습니다. 이 데이터 뷰를 표시할 때 디스플레이 설정 관리자 보기 표시 되는 방식을 변경 하려면 사용할 수 있습니다. 현재 두 가지 옵션이 있습니다.
    1.  **호출 트리를 반전** –이 위쪽에서 아래쪽으로 스택 추적을 고려 합니다. 이것이 편리한 보기 옵션을 최하위 메서드 나타나듯이 여기서 CPU가 된 시간을 소비 하는입니다.
    2.  **별도 스레드에 의해** –이 옵션은 스레드에서 호출 트리를 구성 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/allocations2.png "호출 트리 탭")](images/allocations2.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations3-vs.png "호출 트리 탭")](images/allocations3-vs.png#lightbox)

-----

 -   **스냅숏을** – 메모리 스냅숏에 대 한 내용은이 창에 표시 됩니다. 라이브 응용 프로그램을 프로 파일링 하는 동안 이러한 요소를 생성 하려면 클릭는 _카메라_ 메모리 유지 하 고 해제를 확인 하려는 각 지점에서 도구 모음 단추입니다. 내부에서 발생 하는 작업을 탐색 하는 각 스냅숏을 클릭할 수 있습니다. Note 앱 프로 파일링 라이브 상태일 때 스냅숏을 작성만 수 있습니다. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/allocations4.png "스냅숏 탭")](images/allocations4.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations4-vs.png "스냅숏 탭")](images/allocations4-vs.png#lightbox)

-----

### <a name="time-profiler"></a>시간 프로파일러

응용 프로그램의 각 메서드에 소요 시간 정확 하 게 측정 하는 시간 프로파일러 계측 합니다. 응용 프로그램은 정기적으로 일시 중지 하 고 각 활성 스레드의에서 스택 추적을 실행 합니다. 계측 세부 정보 영역에서 각 행 열어본 실행 경로를 보여 줍니다.

그림 차트 아래 스크린샷에 표시 된 대로 샘플을 실행할 때 앱에서 받은 수를 표시 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![시간 프로파일러 계측](images/time1.png)](images/time1.png#lightbox) 

[![시간 프로파일러 계측 – 샘플 목록](images/time3.png)](images/time3.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![시간 프로파일러 계측](images/time1-vs.png)](images/time1-vs.png#lightbox) 

[![시간 프로파일러 계측 – 샘플 목록](images/time3-vs.png)](images/time3-vs.png#lightbox) 

-----

- **호출 트리** – 각 방법에 소요 된 시간의 양을 보여 줍니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/time2.png "시간 프로파일러 계측-호출 트리")](images/time2.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/time2-vs.png "시간 프로파일러 계측-호출 트리")](images/time2-vs.png#lightbox) 

-----

### <a name="cycles"></a>주기

C# 및 F # 관리 코드를 사용 하 여 매우 일반적이 고 아쉽게도 매우 쉽게 삭제 되지 않을 개체에 대 한 참조를 만들 수 수 있습니다. 이 intrument 해당 개체를 식별 및 응용 프로그램에서 참조 하는 주기를 표시할 수 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![주기 계측](images/cycles-vs.png)](images/time1-vs.png#lightbox) 

-----

## <a name="profiling-applications"></a>응용 프로그램 프로 파일링

현재, 기본 디버그 구성만 프로 파일링 할 수 있습니다.

다른 구성 사용 하 여 앱을 프로 파일링 하는 경우 다음 메시지 대화 상자가 나타납니다.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![프로 파일링 오류 대화 상자](images/image001.png)](images/image001.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/image1vs.png "프로 파일링 오류 대화 상자")](images/image1vs.png#lightbox) 

-----

선택 **업데이트** 를 계속 합니다.

### <a name="sgen-garbage-collector-and-profiling"></a>SGen 가비지 수집기 및 프로 파일링

[SGen](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/) 모든 Xamarin 플랫폼에 대해 가비지 수집기가 사용 됩니다.

SGen 세 가지 힙으로 응용 프로그램의 개체를 할당 하는 세대 GC는-학교, 주요 힙과 대형 개체 공간입니다. 가비지 수집의 높이려면 실행 수 있습니다. SGen Xamarin.Android에 대 한 기본 GC는 현재 Xamarin.iOS 통합 응용 프로그램 및입니다.

Xamarin.iOS 응용 프로그램에서 클래식 API를 사용 하 여 Boehm GC – 보수적인, 세대 가비지 수집기를 사용 합니다. 보수적인 그대로 확률이 프로파일러를 사용 하는 경우 정확 하지 않은 결과를 야기할 수 있는 사용 가능한 메모리를 확보 합니다. 이러한 이유로 할당 계측 Boehm 가비지 수집기와 함께 사용할 수 없습니다.

메시지가 표시 됩니다는 메시지 대화 상자와 앱 Boehm GC를 사용 하는 경우, 하는 동안 Xamarin 신중 하 게 연구 및 철저 한 테스트 없이 Boehm SGen 사용 하는 기존 iOS 응용 프로그램 전환 권장 하지 않습니다. Xamarin도 좋지 않습니다 프로 파일링 하 고 이러한 결과 정확한 벤치 마크의 메모리 사용량을 제공 하지 않으며으로 다시 전환에 대 한 SGen으로 전환 합니다.

메모리 관리에 대 한 자세한 내용은 참조는 [메모리 및 성능 모범 사례](~/cross-platform/deploy-test/memory-perf-best-practices.md) 가이드입니다.

## <a name="summary"></a>요약

이 가이드에는 프로 파일링 하 고 개발자에 게 유용 방법을 살펴보았습니다. 다음 일부 기록 및 작동 방법에 대 한 정보를 제공 하는 Xamarin 프로파일러 도입 되었습니다. 마지막 내용을 Xamarin 프로파일러 기능 하 고 할당 및 시간 프로파일러 수단을 탐색 합니다.

## <a name="related-links"></a>관련 링크

- [메모리 및 성능 모범 사례](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [릴리스 정보](https://developer.xamarin.com/releases/profiler/preview/)
