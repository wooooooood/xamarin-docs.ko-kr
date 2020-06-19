---
title: XAML 핫 재 로드Xamarin.Forms
description: 모든 XAML 변경 후 프로젝트를 빌드할 필요가 없도록 실행 중인 응용 프로그램에서 XAML 파일에 대 한 변경 내용을 즉시 다시 로드 Xamarin.Forms 합니다.
ms.prod: xamarin
ms.assetid: E220F054-32EE-424C-A7E5-6156BE271519
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/14/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0655739c95ba58b8d93aae6d3987d54bd0582c7b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127454"
---
# <a name="xaml-hot-reload-for-xamarinforms"></a>XAML 핫 재 로드Xamarin.Forms

XAML 핫 다시 로드는 기존 워크플로에 연결 하 여 생산성을 높이고 시간을 절약 합니다. XAML 핫 다시 로드를 사용 하지 않으면 XAML 변경 내용을 확인할 때마다 앱을 빌드하고 배포 해야 합니다. 핫 다시 로드를 사용 하 여 XAML 파일을 저장 하면 변경 내용이 실행 중인 앱에 라이브 반영 됩니다. 또한 탐색 상태와 데이터가 유지 되므로 앱에서 사용자의 자리를 잃지 않고도 UI를 신속 하 게 반복할 수 있습니다. 따라서 XAML 핫 다시 로드를 사용 하 여 UI 변경의 유효성을 검사 하기 위해 앱을 다시 작성 하 고 배포 하는 시간을 줄일 수 있습니다.

> [!NOTE]
> WPF 또는 UWP 앱을 작성 하는 경우 [uwp 및 WPF에 대해 XAML 핫 다시 로드](/visualstudio/debugger/xaml-hot-reload)를 참조 하세요.
>
> 의 XAML 핫 다시 로드 Xamarin.Forms 는 현재 UWP 프로젝트에 대해 작동 _하지 않습니다_ Xamarin.Forms .

## <a name="system-requirements"></a>시스템 요구 사항

| IDE/프레임 워크 | 버전 필요 |
|------|------------------|
|Visual Studio 2019 | 16.4 이상
Mac용 Visual Studio 2019 | 8.4 이상
Xamarin.Forms | 4.1 이상

## <a name="enable-xaml-hot-reload-for-xamarinforms"></a>XAML 핫 다시 로드 사용Xamarin.Forms

템플릿에서 시작 하는 경우 XAML 핫 다시 로드는 기본적으로 켜져 있으며 프로젝트는 추가 설치 없이 작동 하도록 구성 됩니다. Android 또는 iOS 에뮬레이터, 시뮬레이터 또는 물리적 장치에서 앱을 디버그 하 고, XAML을 변경 하 고, 파일을 저장 하 여 XAML 핫 다시 로드를 트리거합니다.

기존 솔루션에서 작업 하는 경우 Xamarin.Forms XAML 핫 다시 로드를 사용 하기 위해 추가로 설치 하지 않아도 되지만 최상의 환경을 유지 하기 위해 구성을 다시 확인 해야 할 수도 있습니다. 먼저 IDE 설정에서 사용 하도록 설정 합니다.

* Windows에서는 **도구**옵션 xamarin **Enable Xamarin Hot Reload**  >  **Options**  >  **Xamarin**  >  **핫 다시 로드**에서 xamarin 핫 다시 로드 사용 확인란을 선택 합니다.
* Mac에서 **Visual Studio**xamarin XAML 핫 다시 로드 용 Visual Studio 기본 설정 도구에서 **xamarin 핫 다시 로드 사용** 확인란을 선택 합니다  >  **Preferences**  >  **Tools for Xamarin**  >  **XAML Hot Reload**.
  * 이전 버전의 Mac용 Visual Studio에서 메뉴는 **Visual Studio**  >  **기본 설정**  >  **프로젝트**  >  **Xamarin 핫 다시 로드**에 있습니다.

그런 다음 Android 및 iOS 빌드 설정에서 링커가 "링크 안 함" 또는 "링크 없음"으로 설정 되어 있는지 확인 합니다. 실제 iOS 장치에서 XAML 핫 다시 로드를 사용 하려면 **Mono 인터프리터** (visual studio 16.4 이상)를 사용 하도록 설정 하거나 추가 **mtouch Args** (visual studio 16.3 및 아래)에 **--인터프리터** 를 추가 해야 합니다.

다음 순서도를 사용 하 여 XAML 핫 다시 로드에 사용 하기 위해 기존 프로젝트의 설정을 확인할 수 있습니다.

![XAML 핫 다시 로드 설정](hot-reload-images/hotreloadflowchart.png "XAML 핫 다시 로드 설정 순서도")

## <a name="resilient-reloading"></a>복원 력 다시 로드

XAML 핫 다시 로드에서 다시 로드할 수 없는 변경 작업을 수행 하면 IntelliSense를 사용 하 여 오류 메시지가 표시 됩니다. 이러한 변경 내용에는 잘못 된 편집이 포함 되어 있으며,이는 XAML을 잘못 삽입 하거나 컨트롤을 존재 하지 않는 이벤트 처리기에 연결 합니다. 잘못 된 편집 기능을 사용 하는 경우에도 앱을 다시 시작 하지 않고 계속 다시 로드할 수 있습니다. XAML 파일의 다른 위치에서 다른 변경을 수행 하 고 저장을 누릅니다. 강제 편집은 다시 로드 되지 않지만 다른 변경 내용은 계속 적용 됩니다.

## <a name="reload-on-multiple-platforms-at-once"></a>한 번에 여러 플랫폼에서 다시 로드

XAML 핫 다시 로드는 Visual Studio 및 Mac용 Visual Studio에서 동시 디버깅을 지원 합니다. Android와 iOS 대상을 동시에 배포 하 여 두 플랫폼에 변경 내용이 한 번에 반영 되는지 확인할 수 있습니다. 여러 플랫폼에서 디버깅 하려면 다음을 참조 하세요.
* **Windows** [방법: 여러 개의 시작 프로젝트 설정](https://docs.microsoft.com/visualstudio/ide/how-to-set-multiple-startup-projects?view=vs-2019)
* **Mac** [여러 시작 프로젝트 설정](https://docs.microsoft.com/visualstudio/mac/set-startup-projects?view=vsmac-2019)

## <a name="known-limitations"></a>알려진 제한 사항

* Xamarin.FormsUWP 및 macOS와 같은 다른 대상은 아직 지원 *되지 않습니다* . [여기](https://developercommunity.visualstudio.com/idea/661682/xaml-hot-reload-for-xamarinforms-on-uwp.html)에서 UWP 지원의 진행률을 추적할 수 있습니다.
* XAML 핫 다시 로드 세션 중에는 파일 또는 NuGet 패키지를 추가, 제거 또는 이름을 변경할 수 없습니다. 파일이 나 NuGet 패키지를 추가 하거나 제거 하는 경우 XAML 핫 다시 로드를 계속 사용 하도록 앱을 다시 빌드하고 다시 배포 합니다.
* 최적의 환경을 위해 링커를 **연결 하지** 않거나 **링크 없음** 으로 설정 합니다. **링크 SDK only 설정만** 대부분의 시간 동안 작동 하지만 특정 한 경우에는 실패할 수 있습니다. 링커 설정은 Android 및 iOS 빌드 옵션에서 찾을 수 있습니다.
* 실제 iPhone에서 디버깅 하려면 인터프리터에서 XAML 핫 다시 로드를 사용 해야 합니다. 이렇게 하려면 프로젝트 설정을 열고 iOS 빌드 탭을 선택 하 고 **Mono 인터프리터 설정 사용** 이 설정 되어 있는지 확인 합니다. 속성 페이지 위쪽의 **플랫폼** 옵션을 **iPhone**으로 변경 해야 할 수 있습니다.
* 컨트롤을 해당 값을 사용 하 여 다른 필드나 속성에 할당 하 여 만든 모든 참조는 다시 `x:Name` 로드 되지 않습니다.
* AppShell에서 셸 응용 프로그램의 시각적 계층 구조를 업데이트 하면 응용 프로그램의 상태를 유지 관리 하는 문제가 발생할 수 있습니다. 문제가 발생 하는 경우 다시 로드를 계속 하려면 앱을 다시 빌드합니다.
* XAML 핫 다시 로드는 이벤트 처리기, 사용자 지정 컨트롤, 페이지 코드 및 추가 클래스를 포함 하 여 c # 코드를 다시 로드할 수 없습니다.

## <a name="more-resources"></a>기타 참고 자료

* [XAML 핫 다시 로드를 위한 팁과 요령](https://devblogs.microsoft.com/xamarin/tips-tricks-xaml-hot-reload/)
* [심층 분석을 위한 XAML 핫 다시 로드 Xamarin.Forms : Xamarin Show](https://www.youtube.com/watch?v=crhjjPjzknk)

## <a name="troubleshooting"></a>문제 해결

* XAML 핫 재 로드를 초기화 하지 못한 경우:
  * 버전을 업데이트 Xamarin.Forms 합니다.
  * 최신 버전의 IDE를 사용할 수 있는지 확인 합니다.
  * Android 또는 iOS 링커 설정을 프로젝트의 빌드 설정에서 **링크 안 함** 으로 설정 합니다.
* XAML 파일을 저장할 때 아무 작업도 수행 되지 않으면 IDE에서 XAML 핫 다시 로드를 사용 하도록 설정 해야 합니다.
* 실제 iPhone에서 디버깅 하 고 앱이 응답 하지 않는 경우 인터프리터를 사용 하도록 설정 되었는지 확인 합니다. 설정 하려면 Mono 인터프리터 (visual Studio 16.4/8.4 이상)를 **사용 하도록 설정** 하거나 iOS 빌드 설정의 **추가 mtouch 인수** 필드 (visual studio 16.3/8.3 및 이전)에 **--인터프리터** 를 추가 합니다.

버그를 보고 하려면 Windows에서 **도움말**  >  **사용자 의견 보내기**  >  **문제 보고** 를 사용 하 고 **Help**  >  Mac에서**문제를 보고** 하는 데 도움을 줍니다.
