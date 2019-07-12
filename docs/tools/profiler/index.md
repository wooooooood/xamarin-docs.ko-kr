---
title: Xamarin Profiler
description: 이 가이드에서는 Xamarin Profiler의 주요 기능을 살펴봅니다. Xamarin 응용 프로그램 프로 파일링에 대 한 프로파일러, 프로 파일링 및 때 사용 해야, 및 표준 워크플로 찾습니다.
ms.prod: xamarin
ms.assetid: 3247fcee-6acc-470d-ab87-c1c511d67363
author: lobrien
ms.author: laobri
ms.date: 06/03/2018
ms.openlocfilehash: d80363cd339d5d3177ae063df2a20d7938f59169
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832396"
---
# <a name="xamarin-profiler"></a>Xamarin Profiler

_이 가이드에서는 Xamarin Profiler의 주요 기능을 살펴봅니다. Xamarin 응용 프로그램 프로 파일링에 대 한 프로파일러, 프로 파일링 및 때 사용 해야, 및 표준 워크플로 찾습니다._

응용 프로그램의 성공 최종 사용자 환경에 따라 달라 집니다. 개발자로 서 수 구현한 정말 놀라운 기능 일부 앱에서 하지만 앱 느림 또는 전체 크래시 인 경우 사용자는 가능성이 없앨 수 있습니다.

지금까지 Mono가을 갖춘 강력한 명령줄 프로파일러 이라는 Mono 런타임에서 실행 되는 프로그램에 대 한 정보를 수집 합니다 [Mono 로그 프로파일러](https://www.mono-project.com/docs/debug+profile/profile/profiler/)합니다. Xamarin Profiler Mono 로그 프로파일러 및 Android, iOS, tvOS 및 Mac 및 Android, ios, Mac 응용 프로그램 및 Windows에서 tvOS 응용 프로그램을 프로 파일링 지원에 대 한 그래픽 인터페이스를 표시 합니다.

Xamarin Profiler에 계측 프로 파일링 하기 위해 사용할 수 있는 많은-할당, 주기 및 시간 Profiler. 이 가이드에서는 이러한 기기가 무엇을 측정 하 고 응용 프로그램을 분석 하는 방법을 설명 하 고 각 화면에 표시 되는 데이터의 의미를 명확히 구분 합니다.

이 가이드는 일반적인 프로 파일링 시나리오를 검사 하 고 분석 하 고 iOS 및 Android 응용 프로그램을 최적화 하는 도구로 프로파일러를 소개 합니다.

## <a name="download-and-install"></a>다운로드 및 설치

> [!NOTE]
> 해야 합니다는 [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/) mac에서 Mac에 대 한 Windows 하거나 Visual Studio Enterprise 또는 Visual Studio에서이 기능을 잠금 해제를 위해 구독자

Xamarin Profiler 독립 실행형 응용 프로그램이 고 Mac 용 Visual Studio 및 Visual Studio IDE 내에서 프로 파일링을 사용 하도록 설정 하려면 통합 됩니다.

플랫폼에 대 한 설치 패키지를 다운로드 합니다.

- [**macOS**](https://dl.xamarin.com/profiler/profiler-mac.pkg)
- [**Windows**](https://dl.xamarin.com/profiler/profiler-windows.msi)

다운로드 한 후 Xamarin Profiler 시스템에 추가할 설치 관리자를 시작 합니다.

## <a name="profilers-and-profiling"></a>프로파일러 및 프로 파일링

프로 파일링은 응용 프로그램 개발에서 중요 하 고 간과 단계입니다. 프로 파일링는 형태의 **동적 프로그램 분석** -실행 중 이며 사용 중인 프로그램을 분석 합니다. 프로파일러는 시간 복잡도, 특정 메서드를 사용 중인 할당 된 메모리에 대 한 정보를 수집 하는 데이터 마이닝 도구. 프로파일러를 사용 하면 심층 드릴 하 고 코드에서 문제 영역을 정확 하 게 이러한 메트릭을 분석할 수 있습니다.

중간; 최적화 하지 반드시를 디자인 하 고 응용 프로그램을 개발 하는 경우 즉, 거의 액세스할 수 있는 영역에서 코드를 개발 하는 시간을 소비 합니다. 이것이 프로 파일링 합니다. 프로파일러는 가장에 대 한 정보는 일반적으로 코드 베이스를 부분 사용 찾아서 사용 하면 시간 함으로써 기능 향상을 지출 해야 있는 영역을 제공 합니다. 개발자는 대부분의 응용 프로그램에서 소요 된 위치 및 응용 프로그램에서 메모리 사용량을 이해 하려면 주의 해야 합니다.

프로 파일링 하는 것은 모든 형식의 개발을 하는 데 도움이 이지만 모바일 개발에 특히 중요 합니다. 최적화 되지 않은 코드는 데스크톱 컴퓨터 보다 모바일 플랫폼에서 훨씬 더 눈에 띄는 하 고 효율적으로 실행 되는 멋진 및 최적화 된 코드에 앱의 성공 여부에 따라 달라 집니다.

## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin Profiler 응용 프로그램에서 프로 파일링 하는 방법을 개발자에 게 제공 Mac 또는 Visual Studio 용 Visual Studio 내에서. 프로파일러가 수집 하 고 사용할 수 있는 다음 개발자가 응용 프로그램의 동작을 분석 하는 앱에 대 한 정보를 표시 합니다. Xamarin Profiler를 사용 하 여 응용 프로그램 프로 파일링 하는 다양 한 방법의 여러 가지 즉 메모리 프로 파일링 및 통계 샘플링 합니다. 이러한을 통해 수행 되 고 할당 시간 Profiler 각각 계측 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

현재 (통해 Mac 용 Visual Studio) Mac에서 Xamarin.iOS, Xamarin.Android 및 Xamarin.Mac 응용 프로그램을 테스트 하려면 Xamarin Profiler는 사용할 수 있습니다. 프로파일러 IDE에서 별도 프로세스 이며 따라서 Mac 용 Visual Studio에서 시작, 하는 것 외에도 사용할 수 있습니다 독립 실행형 응용 프로그램으로.exe를 검사 하 고 `.mlpd` 에서 생성 된 파일을 [mono 로그 프로파일러](https://www.mono-project.com/docs/debug+profile/profile/profiler/).

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

현재 Visual Studio 및 Mac 용 Visual Studio) (통해 Windows에서 Xamarin.Android 앱을 테스트 하려면 Xamarin Profiler는 사용할 수 있습니다. 프로파일러 IDE에서 별도 프로세스 이며 따라서 Visual Studio에서를 실행 하는 것 외에도 사용할 수 있습니다 독립 실행형 응용 프로그램으로.exe를 검사 하 고 `.mlpd` 에서 생성 된 파일을 [mono 로그 프로파일러](https://www.mono-project.com/docs/debug+profile/profile/profiler/) .

-----

<a name="Profiler_Support" />

## <a name="profiler-support"></a>Profiler 지원

Xamarin Profiler에 대 한 지원은 다음 플랫폼에서 사용할 수 있습니다.

- (엔터프라이즈 라이선스를 사용 하 여 macOS) Mac 용 visual Studio
    - Android
        - 장치 및 에뮬레이터
    - iOS
        - 장치 및 시뮬레이터
    - tvOS (시간 계측 지원 되지 않습니다.)
        - 장치 및 시뮬레이터
    - Mac

- Visual Studio (만 **Enterprise** 버전)
    - Android
        - 장치 및 에뮬레이터
    - iOS [실험적]
        - 장치 및 시뮬레이터
    - tvOS
        - 장치 및 시뮬레이터

수 있습니다 **만** 프로필 **디버그** 구성 합니다.

## <a name="profiler-basics"></a>Profiler 기본 사항

이 섹션 Xamarin Profiler의 일부를 소개 하 고 해당 기능을 둘러보고 합니다.

### <a name="allow-profiling-in-your-app"></a>앱에서 Profiling 허용

앱을 성공적으로 프로 파일링 하기 전에 앱의 프로젝트 옵션에서 프로 파일링을 허용 해야 합니다.

- iOS:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  **빌드 > iOS 디버그 > 프로 파일링 사용**

  ![](images/ios-options-mac.png "iOS의 Mac 용 Visual Studio 옵션 대화 상자")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  **속성 > iOS 빌드 > 프로 파일링 사용**

  ![](images/ios-project-options-vs.png "iOS의 Visual Studio 옵션 대화 상자")

-----

- Android:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  **빌드 > Android 디버그 > 개발자 계측을 사용 하도록 설정**

  ![Mac 용 Visual Studio의 android 옵션 대화 상자](images/android-project-options.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  **빌드 > Android 디버그 > 개발자 계측을 사용 하도록 설정**

  ![Mac 용 Visual Studio의 android 옵션 대화 상자](images/android-project-options-vs-sml.png)

-----

### <a name="launching-the-profiler"></a>Profiler를 시작합니다.

Xamarin Profiler iOS 또는 Android 응용 프로그램 프로 파일링 할 때 IDE에서 또는 독립 실행형 응용 프로그램을 시작할 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

#### <a name="launching-from-visual-studio-for-mac"></a>Mac 용 Visual Studio에서 시작

1. 먼저, Mac 용 Visual Studio에 로드 하는 응용 프로그램 및 (기본값) 디버그 구성을 선택 했는지 확인 합니다.
2. 이동할 **실행 > 프로 파일링 시작**Mac 용 Visual Studio에서 또는 **분석 > Xamarin Profiler** 아래 다이어그램에 설명 된 대로 Profiler를 열려면 Visual Studio에서:

  ![](images/start-profiling-xs.png "Mac 용 Visual Studio에서 Profiler를 시작합니다.")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

#### <a name="launching-from-visual-studio"></a>Visual Studio에서 시작

1.  먼저, 응용 프로그램을 Visual Studio에 로드 하 고 위에서 지정 된 대로 (기본값) 디버그 구성을 선택 해야 합니다.
2.  이동할 **분석 > Xamarin Profiler** 아래 다이어그램에 설명 된 대로 Profiler를 열려면 Visual Studio에서:

![Visual Studio에서 Profiler를 시작합니다.](images/start-profiling-vs.png)

-----

메뉴 항목 표시 되지 않으면 참조를 [문제 해결 가이드](~/tools/profiler/troubleshooting.md)합니다.

이 Profiler를 시작 하 고 자동으로 응용 프로그램을 프로 파일링을 시작 합니다.

메모리 및 성능 측정 하는 Profiler는 사용할 수 있습니다. 이 할당 및 시간 Profiler를 위해 계측 다음 섹션에서 자세히 살펴보겠습니다.

#### <a name="saving-and-loading-profiler-sessions"></a>저장 및 Profiler 세션 로드

언제 든 지 프로 파일링 세션을 저장 하려면 선택 **파일 > 다른 이름으로 저장 하는 중...**  Profiler 메뉴 모음에서. 이 파일에 저장 _mlpd_ 형식, 프로 파일링 데이터에 대 한 특수 한 고도로 압축 된 형식입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

설치한 후 아래 스크린샷에 표시 된 것과 같이 Xamarin Profiler 응용 프로그램 폴더에서 찾을 수 있습니다.

![](images/applications.png "Mac에서 독립 실행형 Profiler 열기")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

설치한 후 응용 프로그램 디렉터리에 Xamarin Profiler 응용 프로그램을 찾을 수 있습니다.

![](images/applications-vs.png "Windows에서 독립 실행형 Profiler 열기 ")

-----

로드할 수 있습니다 *.mlpd* 독립 실행형 응용 프로그램을 열어 Profiler 파일 선택 **대상 선택** 파일을 로드 하 고 있습니다.

자세한 내용은 [.mlpd 파일 생성](~/tools/profiler/troubleshooting.md#gen_mlpd)합니다.

## <a name="profiler-features"></a>Profiler 기능

Xamarin Profiler 아래 그림과 같이 5 개 섹션으로 구성 됩니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](images/profiler-mac-sml.png "Mac 용 Visual Studio에서 Profiler 섹션")](images/profiler-mac.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](images/profiler-vs.png "Visual Studio에서 Profiler 섹션")](images/profiler-vs.png#lightbox)

-----

- **도구 모음** – 프로 파일링을 시작/중지 하는 옵션을 앱의 실행 시간을 확인 하며 profiler 응용 프로그램을 구성 하는 분할 뷰를 선택 합니다. 대상 프로세스 선택 제공이 프로파일러 위쪽에 있는, 합니다.
- **계측 목록** – 모든 계측 프로 파일링 세션에 대 한 로드를 나열 합니다.
- **차트 그림** – 이러한 차트와 가로 방향으로 계측 목록에서 관련 계측 관련 됩니다. 눈금을 변경 하려면 (시간 Profiler 아래에 표시 됨) 슬라이더를 사용할 수 있습니다.
- **세부 정보 영역 계측** -현재 계측의 선택한 뷰에서 표시 되는 데이터를 포함 합니다. 이러한 뷰 아래 섹션에서 자세히 살펴보겠습니다.
- **검사기 보기** –이 분할 된 컨트롤에서 선택할 수 있는 섹션을 포함 합니다. 섹션을 선택 하는 계측에 종속 되어 포함 되어 있습니다. 구성 설정, 통계, 스택 추적 정보 및 경로 루트입니다.

### <a name="allocations"></a>할당

생성 된와 가비지 수집 할당 계측 응용 프로그램의 개체에 대 한 자세한 정보를 제공 합니다.

프로파일러 위쪽에는 프로 파일링 하는 동안 정기적으로 할당 된 메모리의 양을 표시 하는 할당 차트. 현재 할당 그래프 되어 할당의 총 힙 크기가 아니라 시점의 시간입니다. 어떤 의미에서 이동 하지 않습니다, 그리고만 증가 합니다. 이 스택에 할당 된 개체를 포함 합니다. 사용 되는 런타임 버전에 따라 차트 다르게 – 동일한 앱에 대해서도 보일 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](images/allocations1.png "할당 방법")](images/allocations1.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](images/allocations1-vs.png "할당 방법")](images/allocations1-vs.png#lightbox)

-----

개발자가 해당 응용 프로그램은 사용 하 여 하 고 메모리를 해제 하는 방법을 분석을 허용 하는 할당 방법의 다른 데이터 뷰 있습니다. 이러한 뷰는 다음과 같습니다.

- **할당** – 모든 할당의 목록이 표시 되며, 클래스 이름으로 그룹화 합니다. 클래스 및 메서드를 사용 하 고, 사용 빈도 및 사용 하는 클래스의 전체 크기의 훌륭한 개요를 제공 합니다. 클래스에 두 번 클릭 하면 할당 된 메모리가 표시 됩니다. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![](images/allocations3.png "할당 탭")](images/allocations3.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![](images/allocations2-vs.png "할당 탭")](images/allocations2-vs.png#lightbox)

-----

검사기 보기 할당에 대 한 루트에 스택 추적 및 경로 대 한 필터링 및 그룹화 개체, 할당 된 메모리에서 통계를 제공 및 상위 할당에 대 한 옵션 및 뷰를 제공 합니다.

- **호출 트리** –이 응용 프로그램에서 모든 스레드의 전체 호출 트리를 표시 하 고 각 노드에 할당 된 메모리에 대 한 정보를 포함 합니다. 요소는 목록에서 옵션을 선택 하면 모든 형제 노드 나타납니다 회색입니다. 트리를 확장 하거나를 드릴 다운 하려면 요소를 두 번 클릭 수 있습니다. 이 데이터 뷰를 표시할 때 표시 되는 방식을 변경 하려면 디스플레이 설정 검사기 보기를 사용할 수 있습니다. 현재 두 가지 옵션이 있습니다.
    1.  **호출 트리 반전** –이 위쪽에서 아래쪽 스택 추적을 고려 합니다. 이 편리 하 게 보기 옵션을 최하위 메서드를 나타냅니다. 여기서는 CPU가 된 시간을 할애 합니다.
    2.  **별도 스레드에서** –이 옵션은 스레드에 의해 호출 트리를 구성 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![](images/allocations2.png "호출 트리 탭")](images/allocations2.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![](images/allocations3-vs.png "호출 트리 탭")](images/allocations3-vs.png#lightbox)

-----

- **스냅숏** – 메모리 스냅숏에 대 한 내용은이 창에 표시 됩니다. 라이브 응용 프로그램을 프로 파일링 하는 동안 이러한를 생성 하려면 클릭 합니다 _카메라_ 메모리 유지 하 고 출시를 참조 하려는 각 시점에서 도구 모음 단추입니다. 내부에서 발생 하는 상황을 탐색 하려면 각 스냅숏을 클릭할 수 있습니다. 앱을 프로 파일링 라이브 상태일 때 스냅숏을 수행할만 있습니다 note 합니다. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![](images/allocations4.png "스냅숏 탭")](images/allocations4.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![](images/allocations4-vs.png "스냅숏 탭")](images/allocations4-vs.png#lightbox)

-----

### <a name="time-profiler"></a>시간 Profiler

시간 Profiler 계측 응용 프로그램의 각 메서드에서 얼마나 많은 시간이 소요 되는 정확 하 게 측정 합니다. 응용 프로그램은 정기적으로 일시 중지 하 고 각 활성 스레드의 스택 추적을 실행 합니다. 계측 세부 정보 영역에서 각 행 실행 경로를 준수를 보여 줍니다.

점도 차트 아래 스크린샷에 표시 된 대로 실행 될 때 앱에서 받은 샘플의 수를 표시 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![시간 Profiler 계측](images/time1.png)](images/time1.png#lightbox) 

[![시간 Profiler 계측 – 샘플 목록](images/time3.png)](images/time3.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![시간 Profiler 계측](images/time1-vs.png)](images/time1-vs.png#lightbox) 

[![시간 Profiler 계측 – 샘플 목록](images/time3-vs.png)](images/time3-vs.png#lightbox) 

-----

- **호출 트리** – 각 메서드에 소요 된 시간의 양을 보여 줍니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![](images/time2.png "시간 Profiler 계측 – 호출 트리")](images/time2.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![](images/time2-vs.png "시간 Profiler 계측 – 호출 트리")](images/time2-vs.png#lightbox) 

-----

### <a name="cycles"></a>주기

사용 하 여 C# 및 F# 관리 코드, 흔히 하 고 하지만 매우 쉽게 삭제 되지 것입니다 하는 개체에 대 한 참조를 만들 수 있습니다. 이 계측을 사용 하면 해당 개체를 정확 하 게 파악 하 고 응용 프로그램에서 참조 하는 주기를 표시할 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![계측 주기](images/cycles.m751-sml.png)](images/cycles.m751.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![계측 주기](images/cycles-vs-sml.png)](images/cycles-vs.png#lightbox) 

-----

## <a name="profiling-applications"></a>응용 프로그램 프로 파일링

현재 기본 디버그 구성만을 프로 파일링 할 수 있습니다.

다른 구성 사용 하 여 앱을 프로 파일링 할 경우 다음 메시지 대화 상자가 나타납니다.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![프로 파일링 오류 대화 상자](images/image001.png)](images/image001.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](images/image1vs.png "프로 파일링 오류 대화 상자")](images/image1vs.png#lightbox) 

-----

선택 **업데이트** 계속 합니다.

### <a name="sgen-garbage-collector-and-profiling"></a>SGen 가비지 수집기 및 프로 파일링

합니다 [SGen](https://www.mono-project.com/docs/advanced/garbage-collector/sgen/) 가비지 수집기는 모든 Xamarin 플랫폼에 사용 됩니다.

SGen은 세 가지 힙 응용 프로그램의 개체를 할당 하는 세대 GC에-학교, 주요 힙 및 큰 개체 공간입니다. 이 가비지 수집이 더 실행할 수 있습니다. SGen은 현재 Xamarin.Android에 대 한 기본 GC 및 Xamarin.iOS 통합 응용 프로그램입니다.

Boehm GC – 보수적인 비 세대 가비지 수집기를 사용 하는 클래식 API를 사용 하 여 Xamarin.iOS 응용 프로그램. 일반적인 경우 프로파일러를 사용 하는 경우 정확 하지 않은 결과가 발생할 수 있는 사용 가능한 메모리를 확보 하려면 작은 것입니다. 이러한 이유로 할당 계측 Boehm 가비지 수집기와 함께 사용할 수 없습니다.

메시지가 표시 됩니다 메시지 대화 상자를 사용 하 여 앱 Boehm GC를 사용 하는 경우, 하는 동안 Xamarin 신중 하 게 연구 및 철저 한 테스트 없이 Boehm SGen 사용 하는 기존 iOS 응용 프로그램 전환 권장 하지 않습니다. Xamarin도 권장 되지 않습니다 SGen 프로 파일링 하 고 이러한 결과 정확 하 게 벤치 마크의 메모리 사용량을 제공 하지 않습니다으로 다시 전환에 대 한 전환 합니다.

메모리 관리에 대 한 자세한 내용은 참조는 [메모리 및 성능 모범 사례](~/cross-platform/deploy-test/memory-perf-best-practices.md) 가이드입니다.

## <a name="summary"></a>요약

이 가이드에는 프로 파일링 및 개발자에 게 유용 하는 방법을 살펴보았습니다. 그런 다음 일부 기록 및 작동 방식에 대 한 정보를 제공 하는 Xamarin Profiler를 도입 했습니다. 마지막 Xamarin Profiler 기능의 내용을 하 고 할당 및 시간 Profiler 계측을 탐색 합니다.

## <a name="related-links"></a>관련 링크

- [메모리 및 성능 모범 사례](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [릴리스 정보](https://developer.xamarin.com/releases/profiler/preview/)
