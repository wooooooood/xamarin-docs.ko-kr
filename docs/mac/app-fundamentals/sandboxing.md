---
title: Xamarin.ios 앱 샌드 박싱
description: 이 문서에서는 앱 스토어에서 릴리스에 대 한 Xamarin.ios 응용 프로그램을 샌드 박싱 하는 방법에 대해 설명 합니다. 컨테이너 디렉터리, 자격, 사용자 결정 권한, 권한 분리, 커널 적용 등 샌드 박싱으로 이동 하는 모든 요소를 다룹니다.
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 02059c43d26c2e685abd685231fe5faf3d7a6bfe
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030108"
---
# <a name="sandboxing-a-xamarinmac-app"></a>Xamarin.ios 앱 샌드 박싱

_이 문서에서는 앱 스토어에서 릴리스에 대 한 Xamarin.ios 응용 프로그램을 샌드 박싱 하는 방법에 대해 설명 합니다. 컨테이너 디렉터리, 자격, 사용자 결정 권한, 권한 분리, 커널 적용 등 샌드 박싱으로 이동 하는 모든 요소를 다룹니다._

## <a name="overview"></a>개요

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용 하는 경우 목표-C 또는 Swift로 작업할 때와 동일한 방법으로 응용 프로그램을 샌드박스를 사용할 수 있습니다.

[![실행 중인 앱의 예](sandboxing-images/intro01.png "실행 중인 앱의 예")](sandboxing-images/intro01-large.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램에서 샌드 박싱을 사용 하는 기본 사항과 샌드 박싱으로 이동 하는 모든 요소 (컨테이너 디렉터리, 자격, 사용자 결정 권한, 권한 분리 및 커널 적용)에 대해 다룹니다. [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 사용 하는 것이 가장 좋습니다. 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하 고,에서 사용할 주요 개념 및 기술을 설명 하 고 있습니다. 이 문서를 참조 하세요.

[Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서의 [목적에 따라 C# 클래스/메서드를](~/mac/internals/how-it-works.md) 표시 하는 방법에 대 한 자세한 내용을 확인할 수 있습니다 .이 항목에서는 C# 클래스를 목표에 연결 하는 데 사용 되는 `Register` 및 `Export` 특성에 대해 설명 합니다. 개체 및 UI 요소

## <a name="about-the-app-sandbox"></a>앱 샌드박스 정보

앱 샌드박스는 응용 프로그램이 시스템 리소스에 대 한 액세스를 제한 하 여 Mac에서 악의적인 응용 프로그램을 실행 하는 경우 발생할 수 있는 손상 으로부터 강력한 방어 기능을 제공 합니다.

샌드박스가 적용 되지 않은 응용 프로그램에는 앱을 실행 하 고 사용자가 액세스할 수 있는 모든 작업을 수행 하는 사용자에 대 한 모든 권한이 있습니다. 앱에 보안 구멍이 포함 된 경우 (또는이를 사용 하는 프레임 워크) 해커는 잠재적으로 이러한 약점을 활용 하 고 앱을 사용 하 여 실행 중인 Mac을 제어할 수 있습니다.

샌드박스가 적용 된 앱은 응용 프로그램 별로 리소스에 대 한 액세스를 제한 하 여 사용자의 컴퓨터에서 실행 되는 응용 프로그램의 일부에 대 한 도난, 손상 또는 악의적인 의도를 방어 하는 줄을 제공 합니다.

앱 샌드박스는 두 가지 전략을 제공 하는 macOS (커널 수준에서 적용)에 기본 제공 되는 액세스 제어 기술입니다.

1. 개발자는 앱 샌드박스를 사용 하 여 응용 프로그램을 OS와 상호 작용 _하는 방법을_ 설명할 수 있습니다. 이러한 방식으로 작업을 완료 하는 데 필요한 액세스 권한만 부여 됩니다.
2. 사용자는 앱 샌드박스를 사용 하 여 열기 및 저장 대화 상자, 끌어서 놓기 작업 및 기타 일반적인 사용자 상호 작용을 통해 시스템에 대 한 추가 액세스를 원활 하 게 부여할 수 있습니다.

### <a name="preparing-to-implement-the-app-sandbox"></a>앱 샌드박스 구현을 준비 하는 중

이 문서에서 자세히 설명 하는 앱 샌드박스 요소는 다음과 같습니다.

- 컨테이너 디렉터리
- 권리가
- 사용자가 결정 한 권한
- 권한 구분
- 커널 적용

이러한 세부 정보를 이해 하면 Xamarin.ios 응용 프로그램에서 앱 샌드박스를 채택 하는 계획을 만들 수 있습니다.

먼저 응용 프로그램이 샌드 박싱에 적합 한 후보 인지 확인 해야 합니다. 대부분의 응용 프로그램은입니다. 다음에는 모든 API 비호환 문제를 해결 하 고 필요한 앱 샌드박스 요소를 결정 해야 합니다. 마지막으로, 권한 분리를 사용 하 여 응용 프로그램의 방어 수준을 최대화 합니다.

앱 샌드박스를 채택 하는 경우 응용 프로그램에서 사용 하는 일부 파일 시스템 위치는 달라질 수 있습니다. 특히 응용 프로그램에는 응용 프로그램 지원 파일, 데이터베이스, 캐시 및 사용자 문서가 아닌 다른 모든 파일에 사용 되는 컨테이너 디렉터리가 포함 됩니다. MacOS와 Xcode는 이러한 파일 유형을 레거시 위치에서 컨테이너로 마이그레이션하는 기능을 지원 합니다.

## <a name="sandboxing-quick-start"></a>샌드 박싱 빠른 시작

이 섹션에서는 앱 샌드박스를 시작 하는 예제로 웹 보기 (특별히 요청 되지 않은 경우 샌드 박싱 아래에 제한 된 네트워크 연결 필요)를 사용 하는 간단한 Xamarin Mac 앱을 만듭니다.

응용 프로그램이 실제로 샌드 박싱 되는지 확인 하 고 일반적인 앱 샌드박스 오류를 해결 하는 방법을 알아봅니다.

### <a name="creating-the-xamarinmac-project"></a>Xamarin.ios 프로젝트 만들기

다음을 수행 하 여 샘플 프로젝트를 만들어 보겠습니다.

1. Mac용 Visual Studio를 시작 하 고 **새 솔루션** 을 클릭 합니다. 추가합니다.
2. **새 프로젝트** 대화 상자에서 **Mac**  > **앱**  > **cocoa 앱**을 선택 합니다.

    [![새 Cocoa 앱 만들기](sandboxing-images/sample01.png "새 Cocoa 앱 만들기")](sandboxing-images/sample01-large.png#lightbox)
3. **다음** 단추를 클릭 하 고 프로젝트 이름에 `MacSandbox`를 입력 한 다음 **만들기** 단추를 클릭 합니다.

    [![앱 이름 입력](sandboxing-images/sample02.png "앱 이름 입력")](sandboxing-images/sample02-large.png#lightbox)
4. **Solution Pad**에서 **주 storyboard** 파일을 두 번 클릭 하 여 Xcode에서 편집할 수 있도록 엽니다.

    [![주 스토리 보드 편집](sandboxing-images/sample03.png "주 스토리 보드 편집")](sandboxing-images/sample03-large.png#lightbox)
5. **웹 뷰** 를 창으로 끌어 놓고 크기를 조정 하 여 콘텐츠 영역을 채운 다음 창에서 확대/축소로 설정 합니다.

    [![웹 보기 추가](sandboxing-images/sample04.png "웹 보기 추가")](sandboxing-images/sample04-large.png#lightbox)
6. `webView`이라는 웹 보기의 콘센트를 만듭니다.

    [![새 콘센트 만들기](sandboxing-images/sample05.png "새 콘센트 만들기")](sandboxing-images/sample05-large.png#lightbox)
7. Mac용 Visual Studio로 돌아가서 **Solution Pad** **ViewController.cs** 파일을 두 번 클릭 하 여 편집을 위해 엽니다.
8. 다음 using 문을 추가 합니다. `using WebKit;`
9. `ViewDidLoad` 메서드를 다음과 같이 만듭니다.

    ```csharp
    public override void AwakeFromNib ()
    {
        base.AwakeFromNib ();
        webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
    }
    ```

10. 변경 내용을 저장합니다.

응용 프로그램을 실행 하 고 다음과 같이 Apple 웹 사이트가 창에 표시 되는지 확인 합니다.

[![예제 앱 실행 표시](sandboxing-images/sample06.png "예제 앱 실행 표시")](sandboxing-images/sample06-large.png#lightbox)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>앱 서명 및 프로 비전

앱 샌드박스를 사용 하도록 설정 하기 전에 먼저 Xamarin.ios 응용 프로그램을 프로 비전 하 고 서명 해야 합니다.

다음 작업을 수행 합니다.

1. Apple 개발자 포털에 로그인 합니다.

    [![Apple 개발자 포털에 로그인](sandboxing-images/sign01.png "Apple 개발자 포털에 로그인")](sandboxing-images/sign01-large.png#lightbox)
2. **인증서, 식별자 & 프로필을 선택 합니다**.

    [![인증서, 식별자 & 프로필 선택](sandboxing-images/sign02.png "인증서, 식별자 & 프로필 선택")](sandboxing-images/sign02-large.png#lightbox)
3. **Mac 앱**에서 **식별자**:를 선택 합니다.

    [![식별자 선택](sandboxing-images/sign03.png "식별자 선택")](sandboxing-images/sign03-large.png#lightbox)
4. 응용 프로그램에 대 한 새 ID를 만듭니다.

    [![새 앱 ID 만들기](sandboxing-images/sign04.png "새 앱 ID 만들기")](sandboxing-images/sign04-large.png#lightbox)
5. 프로 **비전 프로필**에서 **개발**을 선택 합니다.

    [![개발 선택](sandboxing-images/sign05.png "개발 선택")](sandboxing-images/sign05-large.png#lightbox)
6. 새 프로필을 만들고 **Mac 앱 개발**을 선택 합니다.

    [![새 프로필 만들기](sandboxing-images/sign06.png "새 프로필 만들기")](sandboxing-images/sign06-large.png#lightbox)
7. 위에서 만든 앱 ID를 선택 합니다.

    [![앱 ID 선택](sandboxing-images/sign07.png "앱 ID 선택")](sandboxing-images/sign07-large.png#lightbox)
8. 이 프로필의 개발자를 선택 합니다.

    [![개발자 추가](sandboxing-images/sign08.png "개발자 추가")](sandboxing-images/sign08-large.png#lightbox)
9. 이 프로필에 대 한 컴퓨터 선택:

    [![허용 된 컴퓨터 선택](sandboxing-images/sign09.png "허용 된 컴퓨터 선택")](sandboxing-images/sign09-large.png#lightbox)
10. 프로필에 이름을 지정 합니다.

    [![프로필에 이름 지정](sandboxing-images/sign10.png "프로필에 이름 지정")](sandboxing-images/sign10-large.png#lightbox)
11. **완료** 단추를 클릭 합니다.

> [!IMPORTANT]
> 경우에 따라 Apple의 개발자 포털에서 새 프로 비전 프로필을 직접 다운로드 하 고 두 번 클릭 하 여 설치 해야 할 수도 있습니다. 새 프로필에 액세스할 수 있으려면 먼저 Mac용 Visual Studio를 중지 하 고 다시 시작 해야 할 수도 있습니다.

다음으로, 개발 컴퓨터에서 새 앱 ID 및 프로필을 로드 해야 합니다. 다음 작업을 수행해 보겠습니다.

1. Xcode을 시작 하 고 **Xcode** 메뉴에서 **기본 설정** 을 선택 합니다.

    ![Xcode에서 계정 편집](sandboxing-images/sign11.png "Xcode에서 계정 편집")
2. **자세히 보기** ... 단추를 클릭 합니다.

    ![자세히 보기 단추 클릭](sandboxing-images/sign12.png "자세히 보기 단추 클릭")
3. 왼쪽 아래에 있는 **새로 고침** 단추를 클릭 합니다.
4. **완료** 단추를 클릭 합니다.

다음으로, Xamarin.ios 프로젝트에서 새 앱 ID 및 프로 비전 프로필을 선택 해야 합니다. 다음 작업을 수행해 보겠습니다.

1. **Solution Pad**에서 info.plist 파일을 두 번 클릭 하 여 편집을 위해 엽니다 **.**
2. **번들 식별자** 가 위에서 만든 앱 ID와 일치 하는지 확인 합니다 (예: `com.appracatappra.MacSandbox`).

    [![번들 식별자 편집](sandboxing-images/sign13.png "번들 식별자 편집")](sandboxing-images/sign13-large.png#lightbox)
3. 그런 다음 info.plist 파일을 두 번 클릭 하 고 **Icloud 키-값 저장소** 와 **icloud 컨테이너** 모두 위에서 만든 앱 ID와 일치 하는지 확인 **합니다** (예: `com.appracatappra.MacSandbox`).

    [![Info.plist 파일 편집](sandboxing-images/sign17.png "Info.plist 파일 편집")](sandboxing-images/sign17-large.png#lightbox)
4. 변경 내용을 저장합니다.
5. **Solution Pad**에서 프로젝트 파일을 두 번 클릭 하 여 편집을 위한 옵션을 엽니다.

    ![솔루션의 옵션 Editign](sandboxing-images/sign14.png "솔루션의 옵션 Editign")
6. **Mac 서명**을 선택 하 고 **응용 프로그램 번들에 서명** 및 **설치 관리자 패키지에 서명**을 선택 합니다. 프로 **비전 프로필**아래에서 위에서 만든 프로필을 선택 합니다.

    ![프로 비전 프로필 설정](sandboxing-images/sign15.png "프로 비전 프로필 설정")
7. **완료** 단추를 클릭 합니다.

> [!IMPORTANT]
> Xcode에 의해 설치 된 새 앱 ID 및 프로 비전 프로필을 인식 하기 위해 Mac용 Visual Studio를 종료 하 고 다시 시작 해야 할 수 있습니다.

#### <a name="troubleshooting-provisioning-issues"></a>프로 비전 문제 해결

이 시점에서 응용 프로그램을 실행 하 고 모든 것이 올바르게 서명 되 고 프로 비전 되었는지 확인 해야 합니다. 앱이 이전 처럼 계속 실행 되는 경우 모든 것이 좋습니다. 오류가 발생할 경우 다음과 같은 대화 상자가 표시 될 수 있습니다.

[![프로 비전 문제 예 대화 상자](sandboxing-images/sign16.png "프로 비전 문제 예 대화 상자")](sandboxing-images/sign16-large.png#lightbox)

프로 비전 및 서명 문제의 가장 일반적인 원인은 다음과 같습니다.

- 앱 번들 ID가 선택한 프로필의 앱 ID와 일치 하지 않습니다.
- 개발자 ID가 선택한 프로필의 개발자 ID와 일치 하지 않습니다.
- 테스트 중인 Mac의 UUID가 선택한 프로필의 일부로 등록 되지 않았습니다.

문제가 발생 하는 경우 Apple 개발자 포털에서 문제를 해결 하 고 Xcode에서 프로필을 새로 고친 후 Mac용 Visual Studio에서 정리 빌드를 수행 합니다.

### <a name="enable-the-app-sandbox"></a>앱 샌드박스 사용

프로젝트 옵션에서 확인란을 선택 하 여 앱 샌드박스를 사용 하도록 설정 합니다. 다음을 수행합니다.

1. **Solution Pad**에서 **info.plist** 파일을 두 번 클릭 하 여 편집을 위해 엽니다.
2. **자격 사용** 및 **앱 샌드 박싱 사용**을 모두 선택 합니다.

    [![자격 편집 및 샌드 박싱 사용](sandboxing-images/sign17.png "자격 편집 및 샌드 박싱 사용")](sandboxing-images/sign17-large.png#lightbox)
3. 변경 내용을 저장합니다.

이 시점에서 앱 샌드박스를 사용 하도록 설정 했지만 웹 보기에 필요한 네트워크 액세스를 제공 하지 않았습니다. 이제 응용 프로그램을 실행 하면 빈 창이 표시 됩니다.

[![차단 되는 웹 액세스 표시](sandboxing-images/sample08.png "차단 되는 웹 액세스 표시")](sandboxing-images/sample08-large.png#lightbox)

### <a name="verifying-that-the-app-is-sandboxed"></a>앱이 샌드 박싱 되었는지 확인 하는 중

리소스 차단 동작 외에도 다음과 같은 세 가지 주요 방법으로 Xamarin.ios 응용 프로그램이 성공적으로 샌드 박싱 되었는지 확인할 수 있습니다.

1. Finder에서 `~/Library/Containers/` 폴더의 콘텐츠를 확인 합니다. 앱이 샌드 박싱 되 면 앱의 번들 식별자와 같은 폴더 (예: `com.appracatappra.MacSandbox`)가 있습니다.

    [![앱의 번들 열기](sandboxing-images/sample09.png "앱의 번들 열기")](sandboxing-images/sample09-large.png#lightbox)
2. 시스템은 작업 모니터에서 앱이 샌드 박싱된로 표시 됩니다.
    - 작업 모니터 (`/Applications/Utilities`)를 시작 합니다.
    -  > **열** **보기** 를 선택 하 고 **Sandbox** 메뉴 항목이 선택 되어 있는지 확인 합니다.
    - 샌드박스 열이 응용 프로그램에 대 한 `Yes`를 읽도록 합니다.

    [![작업 모니터에서 앱을 확인 하는 중](sandboxing-images/sample10.png "작업 모니터에서 앱을 확인 하는 중")](sandboxing-images/sample10-large.png#lightbox)
3. 앱 이진 파일이 샌드 박싱 되었는지 확인 합니다.
    - 터미널 앱을 시작 합니다.
    - 응용 프로그램 `bin` 디렉터리로 이동 합니다.
    - 다음 명령을 실행 합니다. `codesign -dvvv --entitlements :- executable_path` (여기서 `executable_path`는 응용 프로그램의 경로).

    [![명령줄에서 앱을 확인 하는 중](sandboxing-images/sample11.png "명령줄에서 앱을 확인 하는 중")](sandboxing-images/sample11-large.png#lightbox)

### <a name="debugging-a-sandboxed-app"></a>샌드박스 응용 프로그램 디버깅

디버거는 TCP를 통해 Xamarin.ios 앱에 연결 합니다. 즉, 기본적으로 샌드 박싱을 사용 하도록 설정 하면 앱에 연결할 수 없습니다. 따라서 적절 한 사용 권한을 설정 하지 않고 앱을 실행 하려고 하면 *"디버거에 연결할 수 없습니다." 오류가 발생 합니다.* .

[![필수 옵션 설정](sandboxing-images/debug01.png "필수 옵션 설정")](sandboxing-images/debug01-large.png#lightbox)

디버거를 사용 하도록 설정 하는 데 필요한 **송신 네트워크 연결 허용 (클라이언트)** 권한이 디버거를 사용 하도록 설정 하는 것이 일반적입니다. 이 없이 디버그할 수 없으므로 디버그 빌드 전용으로 샌드 박싱된 앱에 대 한 자격에 해당 권한을 자동으로 추가 하도록 `msbuild`에 대 한 `CompileEntitlements` 대상을 업데이트 했습니다. 릴리스 빌드에서는 자격 파일에 지정 된 자격을 수정 되지 않은 상태로 사용 해야 합니다.

### <a name="resolving-an-app-sandbox-violation"></a>앱 샌드박스 위반 해결

샌드박스가 적용 된 Xamarin.ios 응용 프로그램에서 명시적으로 허용 되지 않는 리소스에 액세스 하려고 하면 앱 샌드박스 위반이 발생 합니다. 예를 들어 웹 보기에서 더 이상 Apple 웹 사이트를 표시할 수 없습니다.

앱 샌드박스 위반의 가장 일반적인 소스는 Mac용 Visual Studio에 지정 된 권한 설정이 응용 프로그램의 요구 사항과 일치 하지 않을 때 발생 합니다. 그런 다음,이 예에서는 웹 보기가 작동 하지 않도록 하는 누락 된 네트워크 연결 자격을 다시 사용 합니다.

#### <a name="discovering-app-sandbox-violations"></a>앱 샌드박스 위반 검색

Xamarin.ios 응용 프로그램에서 앱 샌드박스 위반이 발생 하는 것으로 의심 되는 경우 문제를 검색 하는 가장 빠른 방법은 **콘솔** 앱을 사용 하는 것입니다.

다음을 수행합니다.

1. 문제의 앱을 컴파일하고 Mac용 Visual Studio에서 실행 합니다.
2. `/Applications/Utilties/`에서 **콘솔** 응용 프로그램을 엽니다.
3. 사이드바에서 **모든 메시지** 를 선택 하 고 검색에 `sandbox`을 입력 합니다.

    [![콘솔의 샌드 박싱 문제의 예](sandboxing-images/resolve01.png "콘솔의 샌드 박싱 문제의 예")](sandboxing-images/resolve01-large.png#lightbox)

위의 예제 앱에서는 해당 권한을 요청 하지 않았으므로 응용 프로그램 샌드박스에서 Kernal이 `network-outbound` 트래픽을 차단 하 고 있음을 알 수 있습니다.

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>자격으로 앱 샌드박스 위반 수정

이제 앱 샌드 박싱 위반을 찾는 방법을 살펴보았으므로 응용 프로그램의 자격을 조정 하 여 어떻게 해결할 수 있는지 살펴보겠습니다.

다음을 수행합니다.

1. **Solution Pad**에서 **info.plist** 파일을 두 번 클릭 하 여 편집을 위해 엽니다.
2. **자격** 섹션에서 **송신 네트워크 연결 허용 (클라이언트)** 확인란을 선택 합니다.

    [![자격 편집](sandboxing-images/sign17.png "자격 편집")](sandboxing-images/sign17-large.png#lightbox)
3. 응용 프로그램에 대 한 변경 내용을 저장 합니다.

예제 앱에 대해 위의 작업을 수행 하는 경우이를 빌드하고 실행 하면 이제 웹 콘텐츠가 예상 대로 표시 됩니다.

## <a name="the-app-sandbox-in-depth"></a>응용 프로그램 샌드박스 심층 분석

앱 샌드박스에서 제공 되는 액세스 제어 메커니즘은 이해 하기 쉽고 간단 합니다. 그러나 앱 샌드박스를 각 앱에서 채택 하는 방식은 응용 프로그램의 요구 사항에 따라 달라 집니다.

Xamarin.ios 응용 프로그램이 악성 코드에 의해 악용 되지 않도록 보호 하는 것이 가장 좋습니다. 앱 (또는 사용 하는 라이브러리 또는 프레임 워크 중 하나)에는 단일 취약성이 있어야 합니다. 컴퓨터.

응용 프로그램 샌드박스는 시스템에서 응용 프로그램의 의도 된 상호 작용을 지정할 수 있도록 허용 하 여 인수을 방지 하거나 원인이 될 수 있는 손상을 제한 하도록 설계 되었습니다. 시스템은 응용 프로그램에서 작업을 수행 하는 데 필요한 리소스에 대 한 액세스 권한만 부여 하 고 다른 작업은 수행 하지 않습니다.

앱 샌드박스를 설계할 때 최악의 시나리오에 대 한 디자인을 합니다. 응용 프로그램이 악성 코드에 의해 손상 되 면 앱의 샌드박스에서 파일 및 리소스에만 액세스 하도록 제한 됩니다.

### <a name="entitlements-and-system-resource-access"></a>자격 및 시스템 리소스 액세스

위에서 살펴본 대로 샌드 박싱 되지 않은 Xamarin.ios 응용 프로그램에는 앱을 실행 하는 사용자에 대 한 모든 권한 및 액세스 권한이 부여 됩니다. 악성 코드에 의해 손상 된 경우 보호 되지 않는 앱은 악의적 동작에 대 한 에이전트로 작동할 수 있으며,이로 인해 피해를 일으킬 수 있는 범위가 광범위 합니다.

앱 샌드박스를 사용 하도록 설정 하면 최소한의 권한 집합만 제거 하 여 Xamarin.ios 앱의 자격을 사용 하 여 필요에 따라 다시 사용 하도록 설정할 수 있습니다.

**Info.plist** 파일을 편집 하 고 편집기 드롭다운 상자에서 필요한 권한을 확인 하거나 선택 하 여 응용 프로그램의 앱 샌드박스 리소스를 수정할 수 있습니다.

[![자격 편집](sandboxing-images/sign17.png "자격 편집")](sandboxing-images/sign17-large.png#lightbox)

### <a name="container-directories-and-file-system-access"></a>컨테이너 디렉터리 및 파일 시스템 액세스

Xamarin.ios 응용 프로그램에서 앱 샌드박스를 사용 하는 경우 다음 위치에 액세스할 수 있습니다.

- **앱 컨테이너 디렉터리** -처음 실행 하는 경우 OS는 해당 리소스가 모두 이동 하는 특수 _컨테이너 디렉터리_ 를 만들어 액세스할 수 있습니다. 앱에는이 디렉터리에 대 한 전체 읽기/쓰기 권한이 있습니다.
- **앱 그룹 컨테이너 디렉터리** -앱에 동일한 그룹의 앱 간에 공유 되는 하나 이상의 _그룹 컨테이너_ 에 대 한 액세스 권한을 부여할 수 있습니다.
- **사용자 지정 파일** -응용 프로그램은 사용자가 명시적으로 열거나 끌어 응용 프로그램에 끌어 놓은 파일에 대 한 액세스 권한을 자동으로 가져옵니다.
- **관련 항목** -적절 한 자격을 가진 응용 프로그램은 이름이 같지만 확장명이 다른 파일에 액세스할 수 있습니다. 예를 들어 `.txt` 파일 및 `.pdf` 저장 된 문서입니다.
- **임시 디렉터리, 명령줄 도구 디렉터리 및 특정 세계에서 읽을 수 있는 위치** -앱은 시스템에 지정 된 대로 잘 정의 된 다른 위치에 있는 파일에 대 한 다양 한 액세스 권한을 가집니다.

#### <a name="the-app-container-directory"></a>앱 컨테이너 디렉터리

Xamarin.ios 응용 프로그램의 앱 컨테이너 디렉터리에는 다음과 같은 특징이 있습니다.

- 사용자의 홈 디렉터리 (일반적으로 `~Library/Containers`)의 숨겨진 위치에 있으며, 응용 프로그램 내에서 `NSHomeDirectory` 함수 (아래 참조)를 사용 하 여 액세스할 수 있습니다. 홈 디렉터리에 있기 때문에 각 사용자는 앱에 대 한 자체 컨테이너를 얻게 됩니다.
- 앱에는 컨테이너 디렉터리와 그 안에 포함 된 모든 하위 디렉터리 및 파일에 대 한 제한 없는 읽기/쓰기 권한이 있습니다.
- 대부분의 macOS 경로-Api 검색은 앱의 컨테이너를 기준으로 합니다. 예를 들어 컨테이너에는 자체 **라이브러리** (`NSLibraryDirectory`을 통해 액세스), **응용 프로그램 지원** 및 **기본 설정** 하위 디렉터리가 있습니다.
- macOS는 코드 서명을 통해 및 앱과 해당 컨테이너 간의 연결을 설정 하 고 적용 합니다. 다른 앱에서 **번들 식별자**를 사용 하 여 앱을 스푸핑 하려고 시도 하더라도 코드 서명으로 인해 컨테이너에 액세스할 수 없습니다.
- 사용자가 생성 한 파일에 대 한 컨테이너가 아닙니다. 대신 응용 프로그램에서 사용 하는 파일 (예: 데이터베이스, 캐시 또는 다른 특정 데이터 형식)에 대 한 것입니다.
- _Shoebox_ 유형의 앱 (예: Apple Photo 앱)의 경우 사용자 콘텐츠가 컨테이너로 이동 합니다.

> [!IMPORTANT]
> 불행 하 게 Xamarin.ios는 아직 100% API를 제공 하지 않습니다 (Xamarin.ios와 달리). 따라서 `NSHomeDirectory` API는 현재 버전의 Xamarin.ios에서 매핑되지 않았습니다.

임시 해결 방법으로 다음 코드를 사용할 수 있습니다.

```csharp
[System.Runtime.InteropServices.DllImport("/System/Library/Frameworks/Foundation.framework/Foundation")]
public static extern IntPtr NSHomeDirectory();

public static string ContainerDirectory {
    get {
        return ((NSString)ObjCRuntime.Runtime.GetNSObject(NSHomeDirectory())).ToString ();
        }
}
```

#### <a name="the-app-group-container-directory"></a>앱 그룹 컨테이너 디렉터리

Mac macOS 10.7.5 이상 (이상)부터 응용 프로그램은 `com.apple.security.application-groups` 자격을 사용 하 여 그룹의 모든 응용 프로그램에 공통 된 공유 컨테이너에 액세스할 수 있습니다. 데이터베이스 또는 기타 지원 파일 유형 (예: 캐시)과 같이 사용자가 작성 하지 않은 콘텐츠에 대해이 공유 컨테이너를 사용할 수 있습니다.

그룹 컨테이너는 그룹의 일부인 경우 각 앱의 샌드박스 컨테이너에 자동으로 추가 되 고 `~/Library/Group Containers/<application-group-id>`에 저장 됩니다. 그룹 ID는 개발 팀 ID 및 마침표로 시작 _해야 합니다_ . 예를 들면 다음과 같습니다.

```plist
<team-id>.com.company.<group-name>
```

자세한 내용은 Apple의 응용 프로그램을 [권한 키 참조](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195)의 [응용 프로그램 그룹에 추가](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19) 를 참조 하세요.

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>앱 컨테이너 외부의 powerbox 및 파일 시스템 액세스

샌드박스가 적용 된 Xamarin.ios 응용 프로그램은 다음과 같은 방법으로 컨테이너 외부의 파일 시스템 위치에 액세스할 수 있습니다.

- 사용자의 특정 방향 (열기 및 저장 대화 상자 또는 끌어서 놓기를 사용 하는 다른 메서드를 통해).
- 특정 파일 시스템 위치 (예: `/bin` 또는 `/usr/lib`)에 대 한 자격을 사용 합니다.
- 파일 시스템 위치가 세계에서 읽을 수 있는 특정 디렉터리에 있는 경우 (예: 공유)

_Powerbox_ 는 사용자와 상호 작용 하 여 샌드박스가 적용 된 xamarin.ios 앱의 파일 액세스 권한을 확장 하는 macos 보안 기술입니다. Powerbox에는 API가 없지만 앱이 `NSOpenPanel` 또는 `NSSavePanel`를 호출할 때 투명 하 게 활성화 됩니다. Powerbox 액세스는 Xamarin.ios 응용 프로그램에 대해 설정한 자격을 통해 사용 하도록 설정 됩니다.

샌드박스가 적용 된 앱이 열기 또는 저장 대화 상자를 표시 하는 경우,이 창은 AppKit가 아닌 Powerbox에 의해 제공 되므로 사용자가 액세스할 수 있는 모든 파일이 나 디렉터리에 액세스할 수 있습니다.

사용자가 열기 또는 저장 대화 상자에서 파일 또는 디렉터리를 선택 하거나 앱의 아이콘으로 끌면 Powerbox는 앱의 샌드박스에 연결 된 경로를 추가 합니다.

또한 시스템은 샌드박스가 적용 된 앱에 대해 다음을 자동으로 허용 합니다.

- 시스템 입력 메서드에 대 한 연결입니다.
- 서비스 메뉴에서 사용자가 선택한 서비스를 호출 합니다 (서비스 공급자가 _샌드박스 앱에 안전_ 으로 **표시 된 서비스** 에만 해당).
- 파일 열기 **최근 열기** 메뉴에서 사용자가 선택 합니다.
- 다른 응용 프로그램 간에 복사 & 붙여넣기를 사용 합니다.
- 다음과 같이 전 세계에서 읽을 수 있는 위치에서 파일을 읽습니다.
  - `/bin`
  - `/sbin`
  - `/usr/bin`
  - `/usr/lib`
  - `/usr/sbin`
  - `/usr/share`
  - `/System`
- `NSTemporaryDirectory`에서 만든 디렉터리에서 파일을 읽고 씁니다.

기본적으로 샌드 박싱된 앱에서 열거나 저장 한 파일은 앱이 종료 될 때까지 계속 액세스할 수 있습니다 (앱이 종료 될 때 파일이 열려 있지 않은 경우). 열려 있는 파일은 다음에 앱이 시작 될 때 macOS 다시 시작 기능을 통해 앱의 샌드박스에 자동으로 복원 됩니다.

Xamarin.ios 앱 컨테이너 외부에 있는 파일에 지 속성을 제공 하려면 보안 범위 책갈피를 사용 합니다 (아래 참조).

#### <a name="related-items"></a>관련 항목

앱 샌드박스를 사용 하면 앱에서 파일 이름이 같고 확장명이 다른 관련 항목에 액세스할 수 있습니다. 이 기능에는 앱이 이러한 파일을 사용 하 여 수행할 작업을 샌드박스에 알리기 위해 앱의 `Info.plst` 파일 b) 코드에 있는 관련 확장의 목록이 포함 됩니다.

이를 수행 하는 두 가지 시나리오가 있습니다.

1. 앱은 다른 버전의 파일 (새 확장명 사용)을 저장할 수 있어야 합니다. 예를 들어 `.txt` 파일을 `.pdf` 파일로 내보냅니다. 이러한 상황을 처리 하려면 `NSFileCoordinator`을 사용 하 여 파일에 액세스 해야 합니다. `WillMove(fromURL, toURL)` 메서드를 먼저 호출 하 고 파일을 새 확장으로 이동한 다음 `ItemMoved(fromURL, toURL)`를 호출 합니다.
2. 앱은 확장을 여러 개 포함 하는 주 파일 및 확장을 사용 하는 여러 지원 파일을 열어야 합니다. 예를 들어 영화와 부제목 파일이 있습니다. `NSFilePresenter`를 사용 하 여 보조 파일에 액세스할 수 있습니다. `PrimaryPresentedItemURL` 속성에 주 파일을 제공 하 고 `PresentedItemURL` 속성에 보조 파일을 제공 합니다. 주 파일을 열면 `NSFileCoordinator` 클래스의 `AddFilePresenter` 메서드를 호출 하 여 보조 파일을 등록 합니다.

두 시나리오 모두에서 앱의 **info.plist** 파일은 앱이 열 수 있는 문서 형식을 선언 해야 합니다. 모든 파일 형식에 대해 `CFBundleDocumentTypes` 배열의 항목에 `NSIsRelatedItemType` (`YES` 값 포함)을 추가 합니다.

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>샌드박스 앱을 사용 하 여 대화 동작 열기 및 저장

다음 제한은 샌드박스가 적용 된 Xamarin.ios 앱에서 호출할 때 `NSOpenPanel` 및 `NSSavePanel`에 배치 됩니다.

- **확인** 단추를 프로그래밍 방식으로 호출할 수 없습니다.
- `NSOpenSavePanelDelegate`에서 사용자가 선택한 항목을 프로그래밍 방식으로 변경할 수 없습니다.

또한 다음과 같은 상속 수정이 준비 됩니다.

- **샌드 박싱 되지 않는 앱**  -  `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>보안 범위 책갈피 및 영구 리소스 액세스

위에서 설명한 것 처럼, 샌드박스가 적용 된 Xamarin.ios 응용 프로그램은 PowerBox에서 제공 하는 직접 사용자 상호 작용을 통해 컨테이너 외부의 파일 또는 리소스에 액세스할 수 있습니다. 그러나 이러한 리소스에 대 한 액세스는 앱 시작 또는 시스템 다시 시작 간에 자동으로 지속 되지 않습니다.

_보안 범위 책갈피_를 사용 하면 샌드박스가 적용 된 xamarin.ios 응용 프로그램에서 사용자 의도를 유지 하 고 앱을 다시 시작한 후에 지정 된 리소스에 대 한 액세스를 유지할 수 있습니다.

#### <a name="security-scoped-bookmark-types"></a>보안 범위 책갈피 형식

보안 범위 책갈피 및 영구 리소스 액세스를 사용할 때 다음과 같은 두 가지 sistine 사용 사례가 있습니다.

- **앱 범위 책갈피는 사용자 지정 파일 또는 폴더에 대 한 영구 액세스를 제공 합니다.**

    예를 들어, 샌드박스가 적용 된 Xamarin.ios 응용 프로그램에서를 사용 하 여 편집을 위해 외부 문서를 열 수 있는 경우 (`NSOpenPanel` 사용) 앱은 나중에 동일한 파일에 다시 액세스할 수 있도록 앱 범위 책갈피를 만들 수 있습니다.
- **문서 범위 책갈피는 하위 파일에 대 한 영구 액세스를 제공 합니다.**

예를 들어, 개별 이미지, 비디오 클립 및 소리 파일에 액세스할 수 있는 프로젝트 파일을 만드는 비디오 편집 앱은 나중에 단일 동영상으로 결합 됩니다.

사용자가 프로젝트에 리소스 파일을 가져올 때 (`NSOpenPanel`을 통해) 앱은 파일을 앱에서 항상 액세스할 수 있도록 프로젝트에 저장 된 항목에 대 한 문서 범위의 책갈피를 만듭니다.

문서 범위 책갈피는 책갈피 데이터와 문서 자체를 열 수 있는 모든 응용 프로그램에서 확인할 수 있습니다. 이렇게 하면 이식성이 지원 되므로 사용자가 프로젝트 파일을 다른 사용자에 게 보내고 모든 책갈피를 사용할 수 있습니다.

> [!IMPORTANT]
> 문서 범위 책갈피는 폴더가 아니라 단일 _파일만 가리킬 수 있으며 해당_ 파일은 시스템에서 사용 하는 위치 (예: `/private` 또는 `/Library`)에 있을 수 없습니다.

#### <a name="using-security-scoped-bookmarks"></a>보안 범위 책갈피 사용

보안 범위가 지정 된 두 가지 유형의 책갈피를 사용 하 여 다음 단계를 수행 해야 합니다.

1. **보안 범위 책갈피를 사용 해야 하는 xamarin.ios 앱에서 적절 한 자격을 설정** 합니다. 앱 범위 책갈피의 경우 `com.apple.security.files.bookmarks.app-scope` 권한 키를 `true`로 설정 합니다. 문서 범위 책갈피의 경우 `com.apple.security.files.bookmarks.document-scope` 권한 키를 `true`로 설정 합니다.
2. **보안 범위 책갈피 만들기** -사용자가 액세스를 제공한 모든 파일 또는 폴더 (예: `NSOpenPanel`)에 대해이 작업을 수행 하 여 앱이 영구적으로 액세스할 수 있어야 합니다. `NSUrl` 클래스의 `public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)` 메서드를 사용 하 여 책갈피를 만듭니다.
3. **보안 범위 책갈피 해결** -앱에서 리소스에 다시 액세스 해야 하는 경우 (예: 다시 시작 후), 보안 범위 URL로 책갈피를 확인 해야 합니다. `NSUrl` 클래스의 `public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)` 메서드를 사용 하 여 책갈피를 확인 합니다.
4. **보안 범위 url에서 파일에 액세스 하려는 시스템에 명시적으로 알림** -이 단계는 위의 보안 범위 url을 가져온 후 즉시 수행 해야 하며, 이후에는 나중에 리소스에 대 한 액세스 권한을 다시 확보 하려는 경우에 수행 해야 합니다. 액세스를 하므로. `NSUrl` 클래스의 `StartAccessingSecurityScopedResource ()` 메서드를 호출 하 여 보안 범위가 지정 된 URL에 대 한 액세스를 시작 합니다.
5. **보안 범위 URL에서 파일에 액세스 하는 작업을 시스템에 명시적으로 알리기** -가능한 한 빨리 앱이 파일에 더 이상 액세스할 필요가 없는 경우 (예: 사용자가 닫은 경우) 시스템에 알려야 합니다. `NSUrl` 클래스의 `StopAccessingSecurityScopedResource ()` 메서드를 호출 하 여 보안 범위가 지정 된 URL에 대 한 액세스를 중지 합니다.

리소스에 대 한 액세스를 잃어 한 후 4 단계로 돌아가서 액세스를 다시 설정 해야 합니다. Xamarin.ios 앱을 다시 시작 하는 경우 3 단계로 돌아가서 책갈피를 다시 확인 해야 합니다.

> [!IMPORTANT]
> 보안 범위 URL 리소스에 대 한 액세스를 해제 하지 못하면 Xamarin.ios 앱이 커널 리소스를 누설 하 게 됩니다. 따라서 앱이 다시 시작 될 때까지 해당 컨테이너에 파일 시스템 위치를 더 이상 추가할 수 없습니다.

### <a name="the-app-sandbox-and-code-signing"></a>앱 샌드박스 및 코드 서명

앱 샌드박스를 사용 하도록 설정 하 고 Xamarin.ios 앱에 대 한 특정 요구 사항 (자격을 통해)을 사용 하도록 설정한 후 샌드 박싱을 적용 하려면 프로젝트에 서명 해야 합니다. 앱 샌드 박싱에 필요한 자격이 앱의 서명에 연결 되기 때문에 코드 서명을 수행 해야 합니다.

macOS는 앱의 컨테이너와 코드 서명 간에 링크를 적용 합니다. 이러한 방식으로 앱 번들 ID를 스푸핑 하는 경우에도 다른 응용 프로그램이 해당 컨테이너에 액세스할 수 없습니다. 이 메커니즘은 다음과 같이 작동 합니다.

1. 시스템은 앱의 컨테이너를 만들 때 해당 컨테이너의 ACL ( _Access Control 목록_ )을 설정 합니다. 목록의 초기 액세스 제어 항목에는 앱의 이후 버전을 인식 (업그레이드 한 경우) 할 수 있는 방법을 설명 하는 앱의 _지정 된 요구 사항_ (DR)이 포함 되어 있습니다.
2. 동일한 번들 ID를 사용 하는 앱이 시작 될 때마다 시스템은 앱의 코드 서명이 컨테이너 ACL의 항목 중 하나에 지정 된 지정 요구 사항과 일치 하는지 확인 합니다. 시스템에서 일치 하는 항목을 찾지 못하면 앱을 시작할 수 없습니다.

코드 서명은 다음과 같은 방식으로 작동 합니다.

1. Xamarin.ios 프로젝트를 만들기 전에 Apple Developer 포털에서 개발 인증서, 배포 인증서 및 개발자 ID 인증서를 가져옵니다.
2. Mac 앱 스토어는 Xamarin.ios 앱을 배포할 때 Apple 코드 서명으로 서명 됩니다.

테스트 하 고 디버그할 때 서명 된 Xamarin.ios 응용 프로그램의 버전을 사용 하 게 됩니다 (앱 컨테이너를 만드는 데 사용 됨). 나중에 Apple 앱 스토어에서 버전을 테스트 하거나 설치 하려는 경우 Apple 서명으로 서명 되 고 원래 앱 컨테이너와 동일한 코드 서명이 없기 때문에 시작 되지 않습니다. 이 경우 다음과 유사한 충돌 보고서를 받게 됩니다.

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

이 문제를 해결 하려면 Apple 서명 된 앱 버전을 가리키도록 ACL 항목을 조정 해야 합니다.

샌드 박싱에 필요한 프로 비전 프로필을 만들고 다운로드 하는 방법에 대 한 자세한 내용은 위의 [앱 서명 및 프로 비전](#Signing_and_Provisioning_the_App) 섹션을 참조 하세요.

#### <a name="adjusting-the-acl-entry"></a>ACL 항목 조정

Apple 서명 된 버전의 Xamarin.ios 앱을 실행 하도록 허용 하려면 다음을 수행 합니다.

1. 터미널 앱 (`/Applications/Utilities`)을 엽니다.
2. Apple 서명 버전의 Xamarin.ios 앱에 대 한 Finder 창을 엽니다.
3. 터미널 창에 `asctl container acl add -file`을 입력 합니다.
4. Finder 창에서 Xamarin.ios 앱의 아이콘을 끌어 터미널 창에 놓습니다.
5. 파일의 전체 경로는 터미널의 명령에 추가 됩니다.
6. **Enter** 키를 눌러 명령을 실행 합니다.

이제 컨테이너의 ACL에는 두 버전의 Xamarin.ios 앱에 대 한 지정 된 코드 요구 사항이 포함 되어 있습니다. 이제 macOS에서 두 버전 중 하나를 실행할 수 있습니다.

#### <a name="display-a-list-of-acl-code-requirements"></a>ACL 코드 요구 사항 목록 표시

다음을 수행 하 여 컨테이너의 ACL에서 코드 요구 사항의 목록을 볼 수 있습니다.

1. 터미널 앱 (`/Applications/Utilities`)을 엽니다.
2. `asctl container acl list -bundle <container-name>`을 입력합니다.
3. **Enter** 키를 눌러 명령을 실행 합니다.

`<container-name>`은 일반적으로 Xamarin.ios 응용 프로그램에 대 한 번들 식별자입니다.

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>앱 샌드박스에 대 한 Xamarin Mac 앱 디자인

앱 샌드박스에 대해 Xamarin.ios 앱을 디자인할 때 따라야 하는 일반적인 워크플로가 있습니다. 즉, 앱에서 샌드 박싱을 구현 하는 구체적인 사항은 지정 된 앱의 기능에 대해 고유 합니다.

### <a name="six-steps-for-adopting-the-app-sandbox"></a>앱 샌드박스를 채택 하는 6 단계

앱 샌드박스에 대 한 Xamarin.ios 앱 디자인은 일반적으로 다음 단계로 구성 됩니다.

1. 앱이 샌드 박싱에 적합 한지 확인 합니다.
2. 개발 및 배포 전략을 설계 합니다.
3. API 비호환 문제를 해결 합니다.
4. 필요한 앱 샌드박스 자격을 Xamarin.ios 프로젝트에 적용 합니다.
5. XPC를 사용 하 여 권한 분리를 추가 합니다.
6. 마이그레이션 전략을 구현 합니다.

> [!IMPORTANT]
> 앱 번들에서 주 실행 파일을 sandbox 하 고 해당 번들에 포함 된 모든 도우미 앱 또는 도구를 사용할 수도 있습니다. Mac 앱 스토어에서 배포 되는 모든 앱에 필요 하며, 가능한 경우 다른 형식의 앱 배포에 대해 수행 해야 합니다.

Xamarin.ios 앱의 번들에 있는 모든 실행 파일의 목록을 보려면 터미널에서 다음 명령을 입력 합니다.

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

여기서 `[Your-App-Bundle]`는 응용 프로그램 번들의 이름과 경로입니다.

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Xamarin.ios 앱이 샌드 박싱에 적합 한지 확인

대부분의 Xamarin.ios 앱은 앱 샌드박스에 완전히 호환 되므로 샌드 박싱에 적합 합니다. 앱에서 앱 샌드박스에 허용 하지 않는 동작이 필요한 경우에는 다른 방법을 고려해 야 합니다.

앱이 다음 동작 중 하나를 필요로 하는 경우 앱 샌드박스에 호환 되지 않습니다.

- **권한 부여 서비스** -앱 샌드박스를 사용 하면 [권한 부여 서비스 C 참조](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826)에 설명 된 함수를 사용할 수 없습니다.
- **내게 필요한 옵션 api** -화면 판독기 또는 다른 응용 프로그램을 제어 하는 앱과 같은 보조 앱은 사용할 수 없습니다.
- **임의 앱에 Apple 이벤트 보내기** -앱이 apple 이벤트를 알 수 없는 임의 앱으로 보내야 하는 경우 샌드 박싱 할 수 없습니다. 호출 된 앱의 알려진 목록에 대해서는 앱이 계속 샌드 박싱 될 수 있으며, 자격에는 호출 된 앱 목록을 포함 해야 합니다.
- **다른 작업에 대 한 분산 알림에서 사용자 정보 사전을 보냅니다** . 앱 샌드박스를 사용 하는 경우 메시징 다른 작업을 위해 `NSDistributedNotificationCenter` 개체에 게시할 때 `userInfo` 사전을 포함할 수 없습니다.
- **커널 확장 로드** -커널 확장의 로드는 앱 샌드박스에서 금지 됩니다.
- **열기 및 저장 대화 상자에서 사용자 입력 시뮬레이션** -사용자 입력을 시뮬레이션 하거나 변경 하기 위해 열기 또는 저장 대화 상자를 프로그래밍 방식으로 조작 하는 것은 앱 샌드박스에서 금지 됩니다.
- **다른 앱의 기본 설정 액세스 또는 설정** -앱 샌드박스에서 다른 앱의 설정을 조작 하는 것은 금지 됩니다.
- **네트워크 설정 구성** -네트워크 설정을 조작 하는 것은 앱 샌드박스에서 금지 됩니다.
- **다른 앱 종료** -앱 샌드박스에서 `NSRunningApplication`를 사용 하 여 다른 앱을 종료 하지 못하도록 합니다.

### <a name="resolving-api-incompatibilities"></a>API 비 호환성 해결

앱 샌드박스에 대해 Xamarin.ios 앱을 디자인할 때 일부 macOS Api를 사용 하는 것과의 호환성 문제가 발생할 수 있습니다.

다음은 문제를 해결 하기 위해 수행할 수 있는 몇 가지 일반적인 문제입니다.

- **문서 열기, 저장 및 추적** -`NSDocument` 이외의 기술을 사용 하 여 문서를 관리 하는 경우 앱 샌드박스에 대 한 기본 제공 지원으로 인해 문서를 전환 해야 합니다. `NSDocument`는 PowerBox에서 자동으로 작동 하며 사용자가 Finder에서 문서를 이동 하는 경우 샌드박스 내에서 문서를 유지 하기 위한 지원을 제공 합니다.
- **파일 시스템 리소스에 대 한 액세스 유지** -Xamarin.ios 앱이 컨테이너 외부의 리소스에 대 한 영구 액세스에 종속 되는 경우 보안 범위 책갈피를 사용 하 여 액세스를 유지 합니다.
- **앱에 대 한 로그인 항목 만들기** -앱 샌드박스를 사용 하면 `LSSharedFileList`를 사용 하 여 로그인 항목을 만들 수 없으며 `LSRegisterURL`를 사용 하 여 시작 서비스의 상태를 조작할 수 없습니다. Apple의 [서비스 관리 프레임 워크 설명서를 사용 하 여 로그인 항목 추가](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1) 에 설명 된 대로 `SMLoginItemSetEnabled` 함수를 사용 합니다.
- **사용자 데이터 액세스** -`getpwuid`와 같은 POSIX 함수를 사용 하 여 디렉터리 서비스에서 사용자의 홈 디렉터리를 가져오는 경우 `NSHomeDirectory`와 같은 cocoa 또는 Core Foundation 기호를 사용 하는 것이 좋습니다.
- **다른 앱의 기본 설정에 액세스** -앱 샌드박스는 경로를 검색 하는 api를 앱 컨테이너에 전달 하기 때문에 해당 컨테이너 내에서 기본 설정을 수정 하 고 다른 앱 기본 설정에 액세스 하는 것은 허용 되지 않습니다.
- **웹 보기에서 HTML5 Embedded 비디오 사용** -Xamarin.ios 앱이 WebKit를 사용 하 여 임베디드 HTML5 비디오를 재생 하는 경우 AV 기반 프레임 워크에 대해 앱을 연결 해야 합니다. 앱 샌드박스에서 이러한 비디오를 재생 하는 것을 CoreMedia 수 있습니다.

### <a name="applying-required-app-sandbox-entitlements"></a>필수 앱 샌드박스 자격 적용

앱 샌드박스에서 실행 하려는 모든 Xamarin.ios 응용 프로그램에 대 한 자격을 편집 하 고 **앱 샌드 박싱 사용** 확인란을 선택 해야 합니다.

앱의 기능에 따라 다른 자격을 사용 하도록 설정 하 여 OS 기능 또는 리소스에 액세스 해야 할 수도 있습니다. 앱 샌드 박싱은 앱을 실행 하는 데 필요한 최소 권한으로 요청 하는 자격을 최소화 하 여 임의로 자격을 사용 하도록 설정 하는 것이 가장 좋습니다.

Xamarin.ios 앱에 필요한 자격을 확인 하려면 다음을 수행 합니다.

1. 앱 샌드박스를 사용 하도록 설정 하 고 Xamarin.ios 앱을 실행 합니다.
2. 앱의 기능을 실행 합니다.
3. `/Applications/Utilities`에서 사용할 수 있는 콘솔 앱을 열고 **모든 메시지** 로그에서 `sandboxd` 위반을 찾습니다.
4. 각 `sandboxd` 위반에 대해 다른 파일 시스템 위치 대신 앱 컨테이너를 사용 하거나 앱 샌드박스 자격을 적용 하 여 제한 된 OS 기능에 액세스할 수 있도록 하는 방법으로 문제를 해결 합니다.
5. 모든 Xamarin.ios 앱 기능을 다시 실행 하 고 테스트 합니다.
6. 모든 `sandboxd` 위반이 해결 될 때까지 반복 합니다.

### <a name="add-privilege-separation-using-xpc"></a>XPC를 사용 하 여 권한 분리 추가

앱 샌드박스에 대 한 Xamarin.ios 앱을 개발할 때 권한 및 액세스 약관에서 앱의 동작을 확인 한 다음, 높은 위험 수준의 작업을 자체 XPC 서비스로 분리 하는 것이 좋습니다.

자세한 내용은 Apple의 [만들기 XPC 서비스](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6) 및 [디먼 And services 프로그래밍 가이드](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i)를 참조 하세요.

### <a name="implement-a-migration-strategy"></a>마이그레이션 전략 구현

이전에 샌드 박싱 되지 않았던 새 샌드 박싱된 버전의 Xamarin.ios 응용 프로그램을 릴리스 하는 경우 현재 사용자에 게 원활한 업그레이드 경로가 있는지 확인 해야 합니다.

 컨테이너 마이그레이션 매니페스트를 구현 하는 방법에 대 한 자세한 내용은 Apple의 [앱을 샌드박스 문서로 마이그레이션](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1) 을 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램 샌드 박싱에 대해 자세히 살펴봅니다. 먼저 앱 Sandbox의 기본 사항을 보여 주는 간단한 Xamarin.ios 앱을 만들었습니다. 다음으로 sandbox 위반을 해결 하는 방법을 살펴보았습니다. 그런 다음 앱 샌드박스를 자세히 살펴보고 앱 샌드박스에 대해 Xamarin.ios 앱을 디자인 하는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [앱 스토어에 게시](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [앱 샌드박스 정보](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [일반적인 앱 샌드 박싱 문제](https://developer.apple.com/library/content/qa/qa1773/_index.html)
