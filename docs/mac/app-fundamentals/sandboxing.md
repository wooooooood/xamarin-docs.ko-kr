---
title: Xamarin.Mac 앱 샌드 박싱
description: 이 문서에서는 Xamarin.Mac 응용 프로그램 앱 스토어에 릴리스하기 위해 샌드 박싱을 다룹니다. 모든 컨테이너 디렉터리, 자격, 사용자가 지정한 사용 권한, 권한 분리 커널 적용 등과 같은 샌드 박싱에 포함 된 요소를 설명 합니다.
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: e38ca07aeef1cbd8e121421ebcbad2207a1bb823
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865981"
---
# <a name="sandboxing-a-xamarinmac-app"></a>Xamarin.Mac 앱 샌드 박싱

_이 문서에서는 Xamarin.Mac 응용 프로그램 앱 스토어에 릴리스하기 위해 샌드 박싱을 다룹니다. 모든 컨테이너 디렉터리, 자격, 사용자가 지정한 사용 권한, 권한 분리 커널 적용 등과 같은 샌드 박싱에 포함 된 요소를 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Mac 응용 프로그램에서 C# 및.NET을 사용 하 여 작업을 하는 경우 Objective-c 또는 Swift를 작업할 때와 마찬가지로 sandbox 응용 프로그램에 동일한 수가 있습니다.

[![실행 중인 앱의 예가](sandboxing-images/intro01.png "실행 중인 앱의 예")](sandboxing-images/intro01-large.png#lightbox)

이 문서에서는 Xamarin.Mac 응용 프로그램 및 샌드 박싱에 포함 된 요소의 모든 샌드 박싱을 사용 하 여 작업의 기초부터 다룰 것: 컨테이너 디렉터리, 자격, 사용자가 지정한 사용 권한, 권한 분리 및 커널 적용 합니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및이 문서를 사용 하는 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 특성 요소 Objective-C 개체 및 UI에 C# 클래스를 연결 하는 데 사용 합니다.

## <a name="about-the-app-sandbox"></a>앱 샌드박스 하는 방법에 대 한

앱 샌드박스 응용 프로그램은 시스템 리소스에 액세스를 제한 하 여 Mac에서 실행 되 고 악성 응용 프로그램에서 발생할 수 있는 피해에 대 한 강력한 방어를 제공 합니다.

샌드박스 응용 프로그램을 앱을 실행 하는 사용자의 모든 권한을 가진 및 액세스 하거나 사용자가 수행할 수 있는 모든 작업을 수행할 수 있습니다. 앱 보안 허점 (또는 사용 하는 모든 프레임 워크)를 포함 하는 경우 해커가 해당 약점을 악용 고 앱에서 실행 되는 Mac의 컨트롤을 사용 하 여 잠재적으로 수 있습니다.

응용 프로그램 별로 리소스에 대 한 액세스를 제한 하 여 샌드박스 응용 프로그램 도용, 손상 또는 악의적 사용자의 컴퓨터에서 실행 중인 응용 프로그램 부분에 대 한 줄을 제공 합니다.

앱 샌드박스는 두 가지 전략을 제공 하는 macOS (커널 수준에서 적용 됨)에 기본 제공 액세스 제어 기술:

1. 설명 하는 개발자를 사용 하는 앱 샌드박스 _어떻게_ 응용 프로그램 요소가 OS와, 이러한 방식으로 상호 작용할 수 있는 작업을 완료 하는 데 필요한 리소스에 대 한 액세스 권한만 부여 되 고 있습니다.
2. 앱 샌드박스 원활 하 게 열기를 통해 시스템에 대 한 추가 액세스 권한을 부여 및 저장 대화 상자, 끌어서 놓기 작업 및 다른 일반적인 사용자 상호 작용 할 수 있습니다.

### <a name="preparing-to-implement-the-app-sandbox"></a>앱 샌드박스 구현 준비

앱 샌드박스는 문서에 자세히 설명 하는 요소는 다음과 같습니다.

- 컨테이너 디렉터리
- 권한 부여
- 사용자가 지정한 사용 권한
- 권한 분리
- 커널 적용

이러한 세부 정보를 파악 한 후 앱 샌드박스 Xamarin.Mac 응용 프로그램에서 채택에 대 한 계획을 만들 수 있습니다.

첫째, 응용 프로그램 (대부분의 응용 프로그램은)는 샌드 박싱에 적합 한지 확인 해야 합니다. 다음으로, 모든 API 호환성 문제를 해결 하 고 앱 샌드박스의 어떤 요소가 필요 하다는 것을 확인 해야 합니다. 마지막으로, 응용 프로그램의 defense 수준 수를 최대화 하기 위해 권한 분할을 사용 하 여 살펴봅니다.

앱 샌드박스를 채택 하는 경우 응용 프로그램에서 사용 하는 일부 파일 시스템 위치는 이와 다릅니다. 특히, 응용 프로그램에 응용 프로그램 지원 파일, 데이터베이스, 캐시 및 사용자 문서 되지 않는 다른 파일에 사용 될 컨테이너 디렉터리 해야 합니다. MacOS 및 Xcode를 컨테이너에 기존 위치에서 이러한 형식의 파일을 마이그레이션하려면 지원을 제공 합니다.

## <a name="sandboxing-quick-start"></a>샌드 박싱 빠른 시작

이 섹션에서는 앱 샌드박스 시작의 예로 웹 보기 (구체적으로 요청 하지 않는 샌드 박싱에서 제한 되는 네트워크 연결이 필요)를 사용 하는 간단한 Xamarin.Mac 앱을 만듭니다.

응용 프로그램을 실제로 샌드 박싱 하 고 일반적인 앱 샌드박스 오류 문제를 해결 하는 방법을 알아봅니다 임을 확인 합니다.

### <a name="creating-the-xamarinmac-project"></a>Xamarin.Mac 프로젝트 만들기

샘플 프로젝트를 만들려면 다음을 수행 하겠습니다.

1. Mac 및 클릭에 대 한 Visual Studio를 시작 합니다 **새 솔루션...** 추가합니다.
2. **새 프로젝트** 대화 상자에서 **Mac** > **앱** > **Cocoa 앱**: 

    [![Cocoa 앱을 새로 만드는](sandboxing-images/sample01.png "새 Cocoa 앱 만들기")](sandboxing-images/sample01-large.png#lightbox)
3. 클릭 합니다 **다음** 단추를 입력 `MacSandbox` 프로젝트 이름 및 클릭에 대 한 합니다 **만들기** 단추: 

    [![앱 이름 입력](sandboxing-images/sample02.png "앱 이름 입력")](sandboxing-images/sample02-large.png#lightbox)
4. 에 **Solution Pad**를 두 번 클릭 합니다 **Main.storyboard** Xcode에서 편집 하기 위해 열려는 파일: 

    [![주 스토리 보드를 편집](sandboxing-images/sample03.png "주 스토리 보드를 편집 합니다.")](sandboxing-images/sample03-large.png#lightbox)
5. 끌어서를 **웹 보기** 창으로 크기를 조정 콘텐츠 영역을 입력 하 고 성장 하 고 창을 사용 하 여 축소 하도록 설정 합니다. 

    [![웹 보기를 추가](sandboxing-images/sample04.png "웹 뷰 추가")](sandboxing-images/sample04-large.png#lightbox)
6. 호출 웹 보기에 대 한 아웃렛 만들기 `webView`: 

    [![새 콘센트를 만드는](sandboxing-images/sample05.png "새 콘센트를 만들기")](sandboxing-images/sample05-large.png#lightbox)
7. Mac 및 두 번 클릭에 대 한 Visual Studio로 돌아가서를 **ViewController.cs** 파일을 **Solution Pad** 을 편집용으로 엽니다.
8. 다음 추가 문을 사용 하 여: `using WebKit;`
9. 확인 된 `ViewDidLoad` 다음과 같은 메서드 확인: 

    ```csharp
    public override void AwakeFromNib ()
    {
        base.AwakeFromNib ();
        webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
    }
    ```

10. 변경 내용을 저장합니다.

응용 프로그램을 실행 하 고 Apple 웹 사이트 같이 창에 표시 되는지 확인 합니다.

[![예제 앱 실행을 보여 주는](sandboxing-images/sample06.png "실행 앱의 예를 보여 주는")](sandboxing-images/sample06-large.png#lightbox)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>서명 및 앱 프로 비전

앱 샌드박스를 사용할 수 전에 먼저 프로 비전 하 고 Xamarin.Mac 응용 프로그램에 서명 해야 합니다.

다음을 수행할 수 있습니다.

1. Apple 개발자 포털에 로그인 합니다. 

    [![Apple 개발자 포털에 로그인](sandboxing-images/sign01.png "Apple 개발자 포털에 로그인")](sandboxing-images/sign01-large.png#lightbox)
2. 선택 **인증서, 식별자 및 프로필**: 

    [![인증서, 식별자 및 프로필 선택](sandboxing-images/sign02.png "인증서, 식별자 및 프로필 선택")](sandboxing-images/sign02-large.png#lightbox)
3. 아래 **Mac 앱**를 선택 **식별자**: 

    [![식별자를 선택 하면](sandboxing-images/sign03.png "식별자를 선택 합니다.")](sandboxing-images/sign03-large.png#lightbox)
4. 응용 프로그램에 대 한 새 ID를 만듭니다. 

    [![새 앱 ID 만들기](sandboxing-images/sign04.png "새 앱 ID 만들기")](sandboxing-images/sign04-large.png#lightbox)
5. 아래 **프로 비전 프로필**를 선택 **개발**: 

    [![개발을 선택 하면](sandboxing-images/sign05.png "개발 선택")](sandboxing-images/sign05-large.png#lightbox)
6. 새 프로필 만들기 및 선택 **Mac 앱 개발**: 

    [![새 프로필을 만들어](sandboxing-images/sign06.png "새 프로필 만들기")](sandboxing-images/sign06-large.png#lightbox)
7. 위에서 만든 앱 ID를 선택 합니다. 

    [![앱 ID 선택](sandboxing-images/sign07.png "앱 ID 선택")](sandboxing-images/sign07-large.png#lightbox)
8. 이 프로필에 대 한 개발자를 선택 합니다. 

    [![추가 개발자](sandboxing-images/sign08.png "추가 개발자")](sandboxing-images/sign08-large.png#lightbox)
9. 이 프로필에 대 한 컴퓨터를 선택 합니다. 

    [![허용된 된 컴퓨터 선택](sandboxing-images/sign09.png "허용된 된 컴퓨터를 선택 합니다.")](sandboxing-images/sign09-large.png#lightbox)
10. 프로필 이름을 지정 합니다. 

    [![프로필에 이름을 지정](sandboxing-images/sign10.png "프로필에 이름을 지정")](sandboxing-images/sign10-large.png#lightbox)
11. 클릭 합니다 **수행** 단추입니다.

> [!IMPORTANT]
> 일부에서 Apple 개발자 포털에서 직접 새 프로 비전 프로필을 다운로드 및 설치 하려면 두 번 클릭 해야 합니다. 중지 하는 것은 새 프로필에 액세스할 수 전에 Mac 용 Visual Studio를 다시 시작 해야 할 수 있습니다.

그런 다음 개발 컴퓨터에서 새 앱 ID와 프로필을 로드 해야 합니다. 다음을 수행 하겠습니다.

1. Xcode를 시작 하 고 선택 **기본 설정** 에서 합니다 **Xcode** 메뉴: 

    ![Xcode에 계정 편집](sandboxing-images/sign11.png "Xcode에 계정 편집")
2. 클릭는 **세부 정보를 확인 하는 중...**  단추: 

    ![세부 정보 보기 단추를 클릭](sandboxing-images/sign12.png "세부 정보 보기 단추를 클릭")
3. 클릭 합니다 **새로 고침** 단추 (왼쪽 아래 모서리에).
4. 클릭 합니다 **수행** 단추입니다.

다음으로, Xamarin.Mac 프로젝트에 새 앱 ID 및 프로 비전 프로필을 선택 해야 합니다. 다음을 수행 하겠습니다.

1. 에 **Solution Pad**를 두 번 클릭 합니다 **Info.plist** 파일을 편집용으로 엽니다.
2. 있는지 확인 합니다 **번들 식별자** 위에서 만든이 앱 ID와 일치 (예: `com.appracatappra.MacSandbox`): 

    [![번들 식별자를 편집](sandboxing-images/sign13.png "번들 식별자를 편집 합니다.")](sandboxing-images/sign13-large.png#lightbox)
3. 를 두 번 클릭 합니다 **Entitlements.plist** 파일을 확인 우리의 **iCloud 키-값 저장소** 및 **iCloud 컨테이너** 위에서 만든 앱 ID는 모두 일치 (예: `com.appracatappra.MacSandbox`): 

    [![Entitlements.plist 파일을 편집](sandboxing-images/sign17.png "Entitlements.plist 파일 편집")](sandboxing-images/sign17-large.png#lightbox)
4. 변경 내용을 저장합니다.
5. 에 **Solution Pad**, 편집을 위해 해당 옵션을 열려면 프로젝트 파일을 두 번 클릭 합니다.  

    ![Editign 솔루션의 옵션](sandboxing-images/sign14.png "Editign 솔루션의 옵션")
6. 선택 **Mac 서명**를 확인 한 다음 **응용 프로그램 번들에 서명** 하 고 **설치 관리자 패키지에 서명**합니다. 아래 **프로 비전 프로필**, 위에서 만든 것을 선택 합니다. 

    ![프로 비전 프로필 설정](sandboxing-images/sign15.png "프로 비전 프로필 설정")
7. 클릭 합니다 **수행** 단추입니다.

> [!IMPORTANT]
> 종료 하 고 새 앱 ID 및 Xcode에서 설치 된 프로 비전 프로필을 인식 하도록 하는 Mac 용 Visual Studio를 다시 시작 해야 합니다.

#### <a name="troubleshooting-provisioning-issues"></a>프로 비전 문제 해결

이 시점에서 응용 프로그램을 실행 하는 모든 항목에 서명이 되어 있고 올바르게 프로 비전 되었는지 확인 하려는 해야 합니다. 앱이 여전히 이전 처럼 실행, 모든 것이 좋습니다. 오류가 발생 하면 다음과 같은 대화 상자가 표시 될 수 있습니다.

[![문제 대화를 프로 비전 하는 예로](sandboxing-images/sign16.png "문제 대화를 프로 비전 하는 예제")](sandboxing-images/sign16-large.png#lightbox)

프로 비전 하 고 문제를 서명 합니다. 가장 일반적인 원인은 다음과 같습니다.

- 앱 번들 ID는 선택한 프로필의 앱 ID를 일치 하지 않습니다.
- 개발자 ID 선택한 프로필의 개발자 ID와 일치 하지 않습니다.
- 선택한 프로필의 일부로 등록 되지 않은에서 테스트할 Mac의 UUID입니다.

문제가 발생 하는 경우 Apple Developer 포털의 문제를 해결, Xcode에서 프로필을 새로 고침 및 mac 용 Visual Studio에서 빌드 정리를 수행합니다

### <a name="enable-the-app-sandbox"></a>앱 샌드박스를 사용 하도록 설정

프로젝트 옵션의 확인란을 선택 하 여 앱 샌드박스를 사용 합니다. 다음을 수행합니다.

1. 에 **Solution Pad**를 두 번 클릭 합니다 **Entitlements.plist** 파일을 편집용으로 엽니다.
2. 모두 선택 **권한 부여를 사용 하도록 설정** 하 고 **앱 샌드 박싱을 사용 하도록 설정**: 

    [![자격을 편집 하 고 샌드 박싱을 사용 하도록 설정](sandboxing-images/sign17.png "자격을 편집 하 고 샌드 박싱을 사용 하도록 설정")](sandboxing-images/sign17-large.png#lightbox)
3. 변경 내용을 저장합니다.

이 시점에서 앱 샌드박스를 설정한 있지만 웹 보기에 대 한 필요한 네트워크 액세스를 제공 하지 않았습니다. 이제 응용 프로그램을 실행 하는 경우 빈 창이 얻게 됩니다.

[![차단 되 고 웹 액세스를 보여 주는](sandboxing-images/sample08.png "차단 되 고 웹 액세스를 보여 주는")](sandboxing-images/sample08-large.png#lightbox)

### <a name="verifying-that-the-app-is-sandboxed"></a>앱 샌드박스 인지 확인

동작 차단 리소스 외에도 세 가지 주요 Xamarin.Mac 응용 프로그램을 성공적으로 샌드박스 되었으면 하기가:

1. Finder에서의 콘텐츠를 확인 합니다 `~/Library/Containers/` 폴더가-앱 샌드박스,이 경우 있습니다 됩니다 앱의 번들 식별자와 같은 폴더 (예: `com.appracatappra.MacSandbox`): 

    [![앱의 번들 열기](sandboxing-images/sample09.png "앱의 번들 열기")](sandboxing-images/sample09-large.png#lightbox)
2. 응용 프로그램 활동 모니터에서 샌드박스으로 간주 하는 시스템:
    - 작업 모니터 시작 (아래 `/Applications/Utilities`). 
    - 선택 **뷰** > **열** 있는지 확인 합니다 **샌드박스** 메뉴 항목을 선택 합니다.
    - 샌드박스 열을 읽고 확인 `Yes` 응용 프로그램에 대 한 합니다. 

    [![작업 모니터에서 앱을 확인](sandboxing-images/sample10.png "활동 모니터에서 앱 확인")](sandboxing-images/sample10-large.png#lightbox)
3. 이진 앱 샌드박스 인지 확인 합니다.
    - 터미널 앱을 시작 합니다.
    - 응용 프로그램을 이동할 `bin` 디렉터리입니다.
    - 이 명령을 실행 합니다. `codesign -dvvv --entitlements :- executable_path` (여기서 `executable_path` 응용 프로그램 경로): 

    [![명령줄에서 앱을 확인](sandboxing-images/sample11.png "명령줄에서 앱을 확인 합니다.")](sandboxing-images/sample11-large.png#lightbox)

### <a name="debugging-a-sandboxed-app"></a>샌드박스 응용 프로그램 디버깅

디버거는 기본적으로 샌드 박싱을 사용 하도록 설정 하면 수 없는 앱에 연결 되므로 TCP 통해 Xamarin.Mac 앱에 연결할 적절 한 권한을 사용 하지 않고 앱을 실행 하려고 하면 오류가 발생 하므로 *"연결할 수 없습니다. 디버거가"* 합니다. 

[![필요한 옵션 설정](sandboxing-images/debug01.png "필요한 옵션 설정")](sandboxing-images/debug01-large.png#lightbox)

합니다 **나가는 네트워크 연결을 허용 (클라이언트)** 권한 디버거에 대 한 필요를이 사용 하도록 설정 하면 일반적으로 디버깅 합니다. 없으면 디버깅할 수 없습니다, 이후 업데이트는 `CompileEntitlements` 에 대 한 대상 `msbuild` 디버그에 대 한 샌드박스를 작동 되는 앱만 빌드에 대 한 해당 권한을 자격을 자동으로 추가 합니다. 릴리스 빌드는 수정 되지 않은 자격 파일에서 지정 된 자격을 사용 해야 합니다.

### <a name="resolving-an-app-sandbox-violation"></a>앱 샌드박스 위반을 해결합니다.

응용 프로그램에서 리소스에 액세스 하려고 하는 샌드박스 Xamarin.Mac 명시적으로 허용 되지 앱 샌드박스 위반이 발생 합니다. 예를 들어, 웹 뷰에 더 이상 Apple 웹 사이트를 표시할 수 없게 합니다.

앱 샌드박스 위반의 가장 일반적인 원인은 Mac 용 Visual Studio에서 지정 된 자격 설정을 응용 프로그램의 요구 사항 일치 하지 않으면 발생 합니다. 마찬가지로 다시 예제에서는 누락 된 네트워크 연결 작업에서 웹 보기를 유지 하는 자격입니다.

#### <a name="discovering-app-sandbox-violations"></a>앱 샌드박스 위반 검색

사용 하 여 가장 빠른 방법은 문제를 검색 하는 앱 샌드박스 위반을 Xamarin.Mac 응용 프로그램에서 발생 하는 것 같으면 합니다 **콘솔** 앱.

다음을 수행합니다.

1. 해당 앱을 컴파일 및 mac 용 Visual Studio에서 실행
2. 엽니다는 **콘솔** 응용 프로그램 (에서 `/Applications/Utilties/`).
3. 선택 **모든 메시지** 사이드바에서 enter `sandbox` 검색에서: 

    [![콘솔에서 샌드 박싱 문제의 예로](sandboxing-images/resolve01.png "콘솔에서 샌드 박싱 문제의 예")](sandboxing-images/resolve01-large.png#lightbox)

위의 예제에서는 앱을 확인할 수 있습니다는 Kernal 차단 되는 `network-outbound` 앱 샌드박스로 인해, 해당 권한을 요청 하지 않은 것 이므로 트래픽을 합니다.

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>앱 샌드박스 자격 위반 수정

앱 샌드 박싱 위반을 찾는 방법까지 살펴본 했으므로 이제 응용 프로그램의 자격을 조정 하 여 해결할 수 있습니다 하는 방법을 확인해 보겠습니다.

다음을 수행합니다.

1. 에 **Solution Pad**를 두 번 클릭 합니다 **Entitlements.plist** 파일을 편집용으로 엽니다.
2. 아래는 **자격** 섹션을 확인 합니다 **나가는 네트워크 연결을 허용 (클라이언트)** 확인란을 선택: 

    [![자격 편집](sandboxing-images/sign17.png "자격 편집")](sandboxing-images/sign17-large.png#lightbox)
3. 응용 프로그램에 변경 내용을 저장 합니다.

예제 앱에 대 한 위의 작업을 수행, 다음 작성 하 고 실행을 예상 대로 웹 콘텐츠 표시 이제 됩니다.

## <a name="the-app-sandbox-in-depth"></a>심층 분석 앱 샌드박스

앱 샌드박스를 통해 제공 하는 액세스 제어 메커니즘은 몇 가지 및 이해 하기 쉬운입니다. 그러나 각 앱에서 앱 샌드박스를 사용할 수는 방법은 고유 하 고 앱의 요구 사항에 따라입니다.

Xamarin.Mac 응용 프로그램 악성 코드에서 악용 으로부터 보호 하기 위해 최상의 노력에 들어 있을 필요는 없으며 단일 취약점만 앱에서 (또는 사용 하는 라이브러리 또는 프레임 워크 중 하나)을 사용 하 여 앱의 상호 작용을 제어할 합니다 시스템입니다.

앱 샌드박스 시스템을 사용 하 여 응용 프로그램의 의도 한 상호 작용을 지정할 수 있으므로 방지는 인수 (또는 손상을 일으킬 수 있으므로 제한) 하도록 설계 되었습니다. 시스템은만 해당 작업을 수행 하기 위해 응용 프로그램에 필요한 리소스 등 아무에 대 한 액세스를 부여 합니다.

앱 샌드박스를 디자인할 때 최악의 시나리오를 디자인 하는 합니다. 응용 프로그램에 악성 코드가 손상 될 경우 파일 및 앱의 샌드박스에서 리소스에 액세스 하도록 제한 됩니다.

### <a name="entitlements-and-system-resource-access"></a>자격 및 시스템 리소스 액세스

위에서 말한 것 처럼 샌드박스 되지 않은 Xamarin.Mac 응용 프로그램 전체 권한 및 앱을 실행 하는 사용자의 액세스를 부여 됩니다. 악성 코드가 손상 되 면 보호 되지 않은 앱을 피해 가능성까지 wide를 사용 하 여 악의적인 동작에 대해 에이전트로 작동할 수 있습니다.

앱 샌드박스를 사용 하 여 수 있는 권한의 최소 집합을 제외한 모든 제거한 다음 다시 Xamarin.Mac 앱의 자격을 사용 하 여 필요한 전용으로 사용 하도록 설정 합니다. 

편집 하 여 응용 프로그램의 앱 샌드박스 리소스를 수정할 해당 **Entitlements.plist** 파일 및 검사 나 편집기 드롭다운 상자에서 필요한 권한을 선택 합니다.

[![자격 편집](sandboxing-images/sign17.png "자격 편집")](sandboxing-images/sign17-large.png#lightbox)

### <a name="container-directories-and-file-system-access"></a>컨테이너 디렉터리 및 파일 시스템 액세스

Xamarin.Mac 응용 프로그램은 앱 샌드박스를 채택 하는 경우 다음 위치에 대 한 액세스가 있습니다.

- **앱 컨테이너 디렉터리** -처음 실행 시 OS 만듭니다 특수 _컨테이너 디렉터리_ 만 액세스할 수 있는지 모든 해당 리소스가 데이터를 이동 하는 경우. 앱이이 디렉터리에 대 한 전체 읽기/쓰기 액세스를 해야 합니다.
- **앱 그룹 컨테이너 디렉터리** -앱을 하나 이상의 액세스를 부여할 수 있습니다 _그룹 컨테이너_ 는 동일한 그룹에 앱 간에 서 공유 됩니다.
- **사용자 지정 파일** -응용 프로그램은 명시적으로 열 또는 끌어서 놓을 응용 프로그램 사용자가 파일에 대 한 액세스를 자동으로 가져옵니다.
- **관련 항목** -적절 한 자격을 사용 하 여 응용 프로그램에 액세스할 수 있습니다 이름은 같지만 다른 확장명을 사용 하 여 파일입니다. 예를 들어, 둘 다로 저장 된 문서를 `.txt` 파일 및 `.pdf`합니다.
- **임시 디렉터리, 명령줄 도구 디렉터리 및 전 세계를 읽을 수 있는 특정 위치** -앱 파일 시스템에서 지정 된 대로 잘 정의 된 다른 위치에 대 한 다양 한 수준의 액세스 합니다.

#### <a name="the-app-container-directory"></a>앱 컨테이너 디렉터리

Xamarin.Mac 응용 프로그램의 앱 컨테이너 디렉터리에는 다음과 같은 특징이 있습니다.

- 사용자의 홈 디렉터리에서 숨겨진된 위치에 (일반적으로 `~Library/Containers`) 사용 하 여 액세스할 수 있습니다는 `NSHomeDirectory` 응용 프로그램 내에서 함수 (아래 참조). 홈 디렉터리에는 이기 때문에 각 사용자는 앱에 대 한 고유한 컨테이너를 받게 됩니다.
- 앱은 컨테이너 디렉터리와 모든 해당 하위 디렉터리와 그 파일에 읽기/쓰기 액세스를 제한 했습니다.
- 대부분의 macOS의 경로 찾기 Api는 앱의 container 기준입니다. 예를 들어, 컨테이너는 자체적인 **라이브러리** (을 통해 액세스할 `NSLibraryDirectory`), **응용 프로그램 지원** 하 고 **기본 설정** 하위 디렉터리입니다.
- 설정 하 고 간의 연결을 적용 하는 macOS 앱 및 코드 서명을 통해 컨테이너입니다. 사용 하 여 앱을 스푸핑 하려는 다른 앱 시도도 해당 **번들 식별자**, 코드 서명으로 인해 컨테이너에 액세스할 수 없습니다.
- 컨테이너를 생성 하는 사용자 파일에 없습니다. 데이터베이스, 캐시 또는 다른 특정 종류의 데이터와 같은 응용 프로그램에서 사용 하는 파일입니다.
- 에 대 한 _shoebox_ 앱 (예: Apple의 사진 앱)의 형식, 사용자의 콘텐츠 컨테이너로 이동 합니다.

> [!IMPORTANT]
> 아쉽게도 Xamarin.Mac 없는 아직 (달리 Xamarin.iOS), 100 %API 검사 결과적으로 `NSHomeDirectory` Xamarin.Mac의 현재 버전의 API 매핑되지 않았습니다.

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

Mac macOS 10.7.5부터 (이상)를 사용 하 여는 `com.apple.security.application-groups` 권한 그룹의 모든 응용 프로그램에 공통 되는 공유 컨테이너에 액세스할 수 있습니다. 데이터베이스 또는 다른 유형의 지원 파일 (예: 캐시)와 같은 비 사용자 연결 콘텐츠의이 공유 컨테이너를 사용할 수 있습니다.

그룹 컨테이너를 자동으로 추가 됩니다 각 앱의 샌드박스 컨테이너 (그룹의 일부인 경우)에 저장 된 `~/Library/Group Containers/<application-group-id>`합니다. 그룹 ID _해야_ 예를 들어 개발 팀 ID 및 기간을 사용 하 여 시작 합니다.

```plist
<team-id>.com.company.<group-name>
```

자세한 내용은 Apple의를 참조 하세요 [응용 프로그램 그룹에 응용 프로그램 추가](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19) 에 [자격 키 참조](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195)합니다.

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>앱 컨테이너의 외부 Powerbox 및 파일 시스템 액세스

샌드박스 Xamarin.Mac 응용 프로그램을 다음과 같은 방법으로 해당 컨테이너 외부에서 파일 시스템 위치에 액세스할 수 있습니다.:

- 특정 방향 (열기 및 저장 대화 상자 또는 끌어서 놓기와 같은 다른 메서드)를 통해 사용자입니다.
- 특정 파일 시스템 위치에 대 한 자격을 사용 하 여 (같은 `/bin` 또는 `/usr/lib`).
* 파일 시스템 위치 경우 환경 (예: 공유) 읽을 수 있는 특정 디렉터리에 있습니다.

_Powerbox_ 는 macOS를 샌드박스 Xamarin.Mac 앱의 파일 액세스 권한이 확장 사용자 상호 작용 하는 보안 기술입니다. Powerbox 없는 API를 갖지만 앱은 호출 하는 경우 투명 하 게 활성화 되는 `NSOpenPanel` 또는 `NSSavePanel`합니다. Powerbox 액세스 Xamarin.Mac 응용 프로그램에 대해 설정한 자격을 통해 활성화 됩니다.

샌드박스 응용 프로그램 개방적이 고 표시 하거나 저장 대화 상자를 Powerbox (및 없습니다 AppKit)에 의해 제시 된 창과 따라서 파일이 나 사용자가 액세스할 수 있는 디렉터리에 액세스할 수 있습니다.

사용자에서 열린 파일 또는 디렉터리를 선택 하거나 저장 대화 상자 (하거나 끌 때 앱의 아이콘에 하나), Powerbox 앱의 샌드박스에 연결 된 경로 추가 합니다.

또한 시스템 다음 샌드박스 응용 프로그램을 자동으로 허용합니다.

- 시스템 입력된 메서드를 연결 합니다.
- 사용자가 선택한 서비스를 호출 합니다 **Services** 메뉴 (로 표시 된 서비스에 대 한 유일한 _sandbox 앱에 대 한 안전한_ 서비스 공급자가).
- 열려 있는 파일에서 사용자가 선택 된 **열린 최근** 메뉴.
- 다른 응용 프로그램 간에 복사 및 붙여넣기를 사용 합니다.
- 다음 세상을 읽을 수 있는 위치에서 파일을 읽기:
    - `/bin`
    - `/sbin`
    - `/usr/bin`
    - `/usr/lib`
    - `/usr/sbin`
    - `/usr/share`
    - `/System`
- 만든 디렉터리에 파일을 읽고 `NSTemporaryDirectory`합니다.

기본 일 파일을 열거나 저장 샌드박스 Xamarin.Mac 앱에서 계속 액세스할 수 있습니다 (하지 않는 한 파일은 앱이 종료 될 때 열려) 앱이 종료 될 때까지 합니다. 열려 있는 파일이 자동으로 복원 됩니다 macOS 다시 시작 기능을 통해 앱의 샌드박스는 다음에 앱을 시작할 때입니다.

Xamarin.Mac 앱의 컨테이너 외부에 있는 파일에 대 한 지 속성을 제공 하려면 보안 범위 책갈피 (아래 참조)를 사용 합니다.

#### <a name="related-items"></a>관련된 항목

앱 샌드박스 앱을에서 확장명이 다른 하지만 같은 파일 이름에 있는 관련된 항목을 액세스할 수 있습니다. 기능에는 두 부분이 있습니다: a) 앱의 관련 된 확장 목록을 `Info.plst` 파일, b) 알려주는 코드를 샌드박스에 이러한 파일을 사용 하 여 새로운 앱 수행할 수 있습니다.

여기서 합리적이 지는 두 가지 시나리오가 있습니다.

1. 응용 프로그램 (확장명이 새) 파일의 다른 버전을 저장할 수 있어야 합니다. 예를 들어, 내보내기는 `.txt` 파일을 `.pdf` 파일입니다. 이 상황을 처리 하려면 사용 해야는 `NSFileCoordinator` 파일에 액세스 합니다. 호출 합니다 `WillMove(fromURL, toURL)` 메서드는 먼저 새 확장 파일 이동 및 호출 `ItemMoved(fromURL, toURL)`합니다.
2. 앱은 하나의 확장을 사용 하 여 기본 파일을 열고 다른 확장을 사용 하 여 몇 가지 지원 파일이 해야 합니다. 예를 들어, 동영상 및 자막 파일입니다. 사용 하 여는 `NSFilePresenter` 보조 파일에 액세스할 수 있습니다. 주 파일을 제공 합니다 `PrimaryPresentedItemURL` 속성 및 보조 파일을는 `PresentedItemURL` 속성입니다. 주 파일을 열 때 호출 합니다 `AddFilePresenter` 메서드는 `NSFileCoordinator` 보조 파일을 등록 하는 클래스입니다.

두 시나리오 모두에서 앱의 **Info.plist** 파일에는 앱을 열 수 있는 문서 형식 선언 해야 합니다. 모든 파일 형식에 대해 추가 합니다 `NSIsRelatedItemType` (값을 사용 하 여 `YES`)의 해당 항목에는 `CFBundleDocumentTypes` 배열입니다.

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>열고 샌드박스가 적용 된 앱을 사용 하 여 저장 대화 동작

에 다음과 같은 제한 사항이 지정 된 `NSOpenPanel` 및 `NSSavePanel` 샌드박스 Xamarin.Mac 앱에서 호출 하는 경우:

- 프로그래밍 방식으로 호출할 수 없습니다는 **확인** 단추입니다.
- 사용자의 선택은 프로그래밍 방식으로 변경할 수 없습니다는 `NSOpenSavePanelDelegate`합니다.

또한 다음 상속 수정은 진행에서 합니다.

- **샌드박스 응용 프로그램** - `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>책갈피 보안 범위 및 영구 리소스 액세스

위에서 설명한 대로 샌드박스가 적용 된 Xamarin.Mac 응용 프로그램 파일 또는 제공 된 PowerBox에서 직접 사용자 상호 작용을 통해 해당 컨테이너 외부의 리소스를 액세스할 수 있습니다. 그러나 이러한 리소스에 대 한 액세스를 자동으로 간에 지속 되지 않습니다 앱 시작 또는 시스템 다시 시작 합니다.

사용 하 여 _Security-Scoped 책갈피_, 샌드박스 Xamarin.Mac 응용 프로그램에서 사용자의 의도 유지 하 고 리소스 앱 후 지정 된 다시 시작에 대 한 액세스를 유지할 수 있습니다.

#### <a name="security-scoped-bookmark-types"></a>보안 범위 책갈피 형식

Security-Scoped 책갈피 및 영구 리소스 액세스를 사용할 때는 두 가지 sistine 사용:

- **App-Scoped 책갈피를 사용자 지정 파일 또는 폴더에 대 한 영구 액세스를 제공합니다.** 

    예를 들어 샌드박스 Xamarin.Mac 응용 프로그램 편집에 대 한 외부 문서를 여는 데 사용할 수 있습니다 (사용 하는 `NSOpenPanel`), 나중에 동일한 파일에 액세스할 수 있도록 앱 App-Scoped 책갈피를 만들 수 있습니다.
- **Document-Scoped 책갈피 하위 파일에는 특정 문서 지속적인 액세스를 제공합니다.** 

예를 들어, 프로젝트 파일을 만드는 비디오 편집 앱을 개별 이미지, 비디오 클립 및 단일 동영상으로 나중에 결합할 사운드 파일에 액세스할 수 있습니다.

사용자가 프로젝트에 리소스 파일을 가져옵니다 하는 경우 (통해를 `NSOpenPanel`), 파일이 항상 앱에 액세스할 수 있도록 앱 프로젝트에 저장 된 항목에 대 한 Document-Scoped 책갈피를 만듭니다.

Document-Scoped 책갈피는 책갈피 데이터 및 문서 자체를 열 수 있는 모든 응용 프로그램에서 해결할 수 있습니다. 이 이식성을 사용자가 다른 사용자와의 모든 책갈피 작동에 있는 프로젝트 파일을 보낼 수 있도록 지원 합니다.

> [!IMPORTANT]
> Document-Scoped 책갈피 수 _만_ 폴더가 아니라 단일 파일을 가리키고 해당 파일 시스템에서 사용 하는 위치에 있을 수 없습니다 (같은 `/private` 또는 `/Library`).

#### <a name="using-security-scoped-bookmarks"></a>보안 범위 책갈피 사용

Security-Scoped 책갈피 유형 중 하나를 사용 하 여, 다음 단계를 수행 해야 합니다.

1. **Security-Scoped 책갈피를 사용 해야 하는 Xamarin.Mac 앱에서 적절 한 자격 설정** -For App-Scoped 책갈피를 설정 합니다 `com.apple.security.files.bookmarks.app-scope` 자격 키를 `true`입니다. Document-Scoped 책갈피를 설정 합니다 `com.apple.security.files.bookmarks.document-scope` 에 자격 키 `true`합니다.
2. **Security-Scoped 책갈피를 만듭니다** -파일이 나 폴더를 사용자에 대 한 액세스 제공에 대 한이 작업을 수행 하겠습니다 (통해 `NSOpenPanel` 예를 들어), 앱에 대 한 영구 액세스를 해야 합니다. 사용 합니다 `public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)` 메서드는 `NSUrl` 책갈피를 만들려면 클래스.
3. **Security-Scoped 책갈피 해결** -앱 (예: 다시 시작) 후 다시 리소스에 액세스 해야 하는 경우 보안 범위가 지정 된 URL에 책갈피를 확인 해야 합니다. 사용 하 여는 `public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)` 메서드는 `NSUrl` 책갈피를 확인 하는 클래스입니다.
4. **Security-Scoped URL에서 파일에 액세스 하려면 시스템을 명시적으로 알려야** -이 단계를 위의 Security-Scoped URL 가져오기 후에 즉시 수행 해야 합니다.를 하려는 경우 또는 나중에 알게 된 후에 다시 리소스에 액세스 하려면 사용자의 액세스를 무시 합니다. 호출을 `StartAccessingSecurityScopedResource ()` 메서드를 `NSUrl` Security-Scoped URL 액세스를 시작 하는 클래스입니다.
5. **완료 되는 시스템을 명시적으로 알려야 Security-Scoped URL에서 파일에 액세스** -가능한 한 빨리 알려야 시스템 (예를 들어 사용자가 닫을) 더 이상 앱 파일에 액세스 해야 하는 경우. 호출을 `StopAccessingSecurityScopedResource ()` 메서드는 `NSUrl` Security-Scoped URL 액세스를 중지 하는 클래스입니다.

리소스에 대 한 액세스를 포기 하 한 후 다시 액세스를 다시 설정 하는 4 단계로 돌아가서 해야 합니다. Xamarin.Mac 앱을 다시 시작 하는 경우에 3 단계로 돌아가서 고 책갈피를 다시 확인 해야 합니다.

> [!IMPORTANT]
> Security-Scoped URL 리소스에 대 한 액세스를 해제 하지 못하면 커널 리소스 누수에 게시할 Xamarin.Mac 앱을 발생 합니다. 결과적으로, 앱 더 이상 다시 시작할 때까지 해당 컨테이너에 파일 시스템 위치를 추가할 수 없습니다.

### <a name="the-app-sandbox-and-code-signing"></a>앱 샌드박스 및 코드 서명

앱 샌드박스를 사용 하도록 설정 하 고 특정 요구 사항 (자격)을 통해 Xamarin.Mac 앱에 대 한 사용 적용 되려면 샌드 박싱에 대 한 로그인 프로젝트 코딩 해야 합니다. 앱 샌드 박싱 하는 데 필요한 자격을 앱의 서명과 연결 되기 때문에 서명 하는 코드를 수행 해야 합니다. 

macOS 앱의 컨테이너 및 다른 응용 프로그램이 없는 경우 앱 번들 ID를 스푸핑 하는 것도 해당 컨테이너에 액세스할 수 있습니다 이러한 방식으로 해당 코드 서명을 간의 링크를 적용 합니다. 이 메커니즘은 다음과 같습니다.

1. 설정 앱의 Container를 생성 하는 경우는 _액세스 제어 목록_ (ACL) 해당 컨테이너에 있습니다. 초기 액세스 제어 항목 목록에서 앱의 포함 _요구 사항 지정_ (DR) (업그레이드 않았습니다) 하는 경우 설명 하는 방법을 나중 버전의 앱을 인식 될 수 있습니다.
2. 동일한 번들 ID를 사용 하 여 앱이 시작 될 때마다 시스템 앱 코드 서명 요구 사항을 지정 된 컨테이너의 ACL의 항목 중 하나에 지정 된 일치 하는지 확인 합니다. 시스템 일치 하는 항목을 찾을 수 없는 경우 앱 시작 하지 못하도록 방지 합니다.

코드 서명 다음과 같은 방법으로 작동 합니다.

1. Xamarin.Mac 프로젝트를 만들기 전에 Apple Developer 포털에서 개발 인증서, 배포 인증서 및 개발자 ID 인증서를 가져옵니다.
2. Mac 앱 스토어에 Xamarin.Mac 앱을 배포 하는 경우에 Apple 코드 서명으로 서명 됩니다.

테스트 및 디버깅을 하는 경우 사용자가 등록 (을 사용할 앱 컨테이너를 만들려면) Xamarin.Mac 응용 프로그램의 버전을 사용할 것입니다. 나중에 테스트 또는 Apple 앱 스토어에서 버전을 설치 하려는 경우 Apple 서명을 사용 하 여 서명 됩니다 및 (없으므로 원래 앱 컨테이너와 동일한 코드 서명)를 시작 하지 못합니다. 이 이런 경우에 표시 됩니다 크래시 보고서 다음과 비슷합니다.

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

이 해결 하려면 앱의 Apple 서명 된 버전을 가리키도록 ACL 항목을 조정 해야 합니다.

만들고 다운로드 샌드 박싱 하는 데 필요한 프로 비전 프로필에 대 한 자세한 내용은 참조 하십시오 합니다 [서명 및 앱 프로 비전](#Signing_and_Provisioning_the_App) 위의 섹션입니다.

#### <a name="adjusting-the-acl-entry"></a>ACL 항목을 조정합니다.

Apple 서명된 된 버전의 Xamarin.Mac 앱이 실행 되도록 허용 하려면 다음을 수행 합니다.

1. 터미널 앱을 열고 (에서 `/Applications/Utilities`).
2. Apple 서명된 된 버전의 Xamarin.Mac 앱을 Finder 창을 엽니다.
3. 형식 `asctl container acl add -file` 터미널 창에서.
4. Finder 창에서 Xamarin.Mac 앱의 아이콘을 끌어서 터미널 창에 놓습니다.
5. 파일의 전체 경로 터미널에서 명령에 추가 됩니다.
6. 키를 눌러 **Enter** 명령을 실행 합니다.

컨테이너의 ACL에는 이제 두 버전의 Xamarin.Mac 앱에 대 한 지정 된 코드 요구 사항을 포함 하 고 macOS 버전 중 하나를 실행 하면 이제 키를 누릅니다.

#### <a name="display-a-list-of-acl-code-requirements"></a>ACL 코드 요구 사항 목록을 표시 합니다.

다음을 수행 하 여 컨테이너의 ACL에 코드 요구 사항 목록을 볼 수 있습니다.

1. 터미널 앱을 열고 (에서 `/Applications/Utilities`).
2. `asctl container acl list -bundle <container-name>`을 입력합니다.
3. 키를 눌러 **Enter** 명령을 실행 합니다.

`<container-name>` 일반적으로 Xamarin.Mac 응용 프로그램의 번들 식별자입니다.

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>앱 샌드박스 Xamarin.Mac 앱 디자인

앱 샌드박스 Xamarin.Mac 앱을 디자인 하는 경우 따라야 하는 일반적인 워크플로가 있습니다. 즉, 앱에서 샌드 박싱 구현 세부 사항이 지정된 하는 앱의 기능에 고유 하려고 합니다.

### <a name="six-steps-for-adopting-the-app-sandbox"></a>앱 샌드박스 채택을 위한 6 단계

일반적으로 앱 샌드박스 Xamarin.Mac 앱을 디자인 다음 단계로 구성 됩니다.

1. 앱 샌드 박싱에 적합 한지 확인 합니다.
2. 개발 및 배포 전략을 디자인 합니다.
3. 모든 API 호환성 문제를 해결 합니다.
4. 필요한 앱 샌드박스 자격 Xamarin.Mac 프로젝트에 적용 합니다.
5. XPC를 사용 하 여 권한 분리를 추가 합니다.
6. 마이그레이션 전략을 구현 합니다.

> [!IMPORTANT]
> 샌드박스 앱 번들에 포함 된 모든 도우미도 주 실행 파일 뿐만 아니라 해야 앱 또는 해당 번들의 도구입니다. Mac 앱 스토어에서 배포 된 모든 앱에 대 한 필수 이며, 가능 하면 수행 해야 할 다른 형식의 앱 배포 합니다.

목록을 Xamarin.Mac 앱의 번들에 모든 실행 가능 이진 파일에 대 한 터미널에서 다음 명령을 입력 합니다.

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

여기서 `[Your-App-Bundle]` 이름 및 응용 프로그램의 번들에 대 한 경로입니다.

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Xamarin.Mac 앱 샌드 박싱에 적합 한지 확인 합니다.

대부분의 Xamarin.Mac 앱은 앱 샌드박스 완벽 하 게 호환 따라서 샌드 박싱에 적합 합니다. 앱에서 앱 샌드박스 허용 하지 않는 동작에 필요한 경우는 대안을 고려해 야 합니다.

앱 다음 동작 중 하나에 필요한 경우 앱 샌드박스 호환 되지 않습니다.

- **서비스 권한 부여** -the App 샌드박스 사용 하 여에 설명 된 함수를 사용 하 여 작업할 수 없습니다 [권한 부여 서비스 C 참조](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826)합니다.
- **내게 필요한 옵션 Api** -화면 읽기 프로그램과 같은 보조 앱 샌드박스 또는 다른 응용 프로그램을 제어 하는 앱 수 없습니다.
- **임의의 앱에서 Apple 이벤트 보내기** -알 수 없는, 임의의 앱에 Apple 이벤트를 전송 해야 하는 앱을 하는 경우 샌드박스가 적용 된 일 수 없습니다. 호출된 앱 목록은 알려진 앱 샌드박스 수 및 자격 호출된 앱 목록에 포함 해야 합니다.
- **다른 태스크로 분산 알림에 사용자 지정 사전 정보 보내기** -포함할 수 없다는 것은 앱 샌드박스 사용 하 여는 `userInfo` 사전에 게시 하는 경우는 `NSDistributedNotificationCenter` 메시징 다른 작업에 대 한 개체입니다.
- **커널 확장 로드** -커널 확장 로드에서 앱 샌드박스를 통해 사용할 수 없습니다.
- **열기 및 저장 대화 상자에서 사용자 입력을 시뮬레이션** -프로그래밍 방식으로 열기를 조작 하거나 앱 샌드박스를 통해 저장 대화 상자 시뮬레이션 하거나 사용자 입력을 변경 하는 금지 됩니다.
- **액세스 또는 다른 앱에서 기본 설정을** -앱 샌드박스를 통해 다른 앱의 설정을 조작가 허용 되지 않습니다.
- **네트워크 설정 구성** -네트워크 설정을 조작 앱 샌드박스를 통해 사용할 수 없습니다.
- **다른 앱을 종료** -앱 샌드박스를 사용 하 여 금지 `NSRunningApplication` 다른 앱을 종료 합니다.

### <a name="resolving-api-incompatibilities"></a>API 호환성 문제를 해결합니다.

앱 샌드박스를 게시할 Xamarin.Mac 앱을 디자인할 때 몇 가지 macOS Api 사용과 비 호환성 문제가 발생할 수 있습니다.

다음은 몇 가지 일반적인 문제와 이러한 문제를 해결할 수행할 수 있는 작업입니다.

- **열기, 저장 및 문서를 추적할** 이외의 모든 기술을 사용 하 여 문서를 관리 하는 경우- `NSDocument`, 기본 제공된 앱 샌드박스 지원 하기 때문에 전환 해야 합니다. `NSDocument` PowerBox 사용 하며 자동으로 사용자가 Finder를 이동 하는 경우에 샌드박스 내에서 문서를 유지 하는 것에 대 한 지원을 제공 합니다.
- **파일 시스템 리소스에 대 한 액세스를 유지할** -앱이 해당 컨테이너 외부의 리소스에 대 한 영구 액세스 종속 Xamarin.Mac 책갈피 보안 범위를 사용 하 여 액세스를 유지할 경우.
- **앱에 대 한 로그인 항목을 만듭니다** -the App 샌드박스 사용 하 여 항목를 사용 하 여 로그인을 만들 수 없습니다 `LSSharedFileList` 사용 하 여 시작 서비스의 상태를 조작할 수 나 `LSRegisterURL`합니다. 사용 된 `SMLoginItemSetEnabled` 사과에 설명 된 대로 함수 [서비스 관리 프레임 워크를 사용 하 여 로그인 항목을 추가](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1) 설명서.
- **사용자 데이터에 액세스** -같은 POSIX 함수를 사용 하는 경우 `getpwuid` directory services에서 사용자의 홈 디렉터리를 가져오려면 Cocoa 또는 Core Foundation 기호를 사용 하 여 같은 것이 좋습니다. `NSHomeDirectory`합니다.
- **앱에 액세스 하는 기본 설정의 다른** -앱 샌드박스 지시 경로 찾기 Api 앱의 컨테이너에 해당 컨테이너 내에서 기본 설정을 수행을 수정 하 고 액세스의 다른 앱 설정을 사용할 수 없습니다. 
- **웹 보기에 포함 된 HTML5 비디오를 사용 하 여** -Xamarin.Mac 앱 WebKit를 사용 하 여 포함 된 HTML5 비디오를 재생 하려면, AV Foundation 프레임 워크에 대해 앱 링크 해야 합니다. 앱 샌드박스 하면 CoreMedia play에서 이러한 비디오가 고 그렇지 없습니다.

### <a name="applying-required-app-sandbox-entitlements"></a>필요한 앱 샌드박스 권한 부여를 적용합니다.

앱 샌드박스에서 실행 하 고 확인 하려는 모든 Xamarin.Mac 응용 프로그램에 대 한 자격을 편집 해야 합니다 **앱 샌드 박싱을 사용 하도록 설정** 확인란을 선택 합니다.

앱의 기능에 따라 OS 기능 또는 리소스에 액세스 하려면 다른 자격을 사용 하도록 설정 해야 할 수 있습니다. 따라서 앱 샌드 박싱 때 가장 잘 작동 하려면 앱을 실행 하는 데 필요한 최소한 요청 자격을 최소화할 수 자격 사용 무작위로 마십시오.

Xamarin.Mac 앱에서 요구 하는 자격을 확인 하려면 다음을 수행 합니다.

1. 앱 샌드박스를 사용 하도록 설정 하 고 Xamarin.Mac 앱을 실행 합니다.
2. 앱의 기능을 통해 실행 합니다.
3. 콘솔 앱을 엽니다 (사용할 수 있습니다 `/Applications/Utilities`)를 찾아서 `sandboxd` 위반 합니다 **모든 메시지** 로그.
4. 각 `sandboxd` 위반을 다른 파일 시스템 위치 대신 앱 컨테이너를 사용 하 여 문제를 해결 하거나 제한 된 OS 기능에 액세스할 수 있도록 앱 샌드박스 자격을 적용 합니다.
5. 다시 실행 하 고 모든 Xamarin.Mac 앱 기능을 다시 테스트 합니다.
6. 모든 반복 `sandboxd` 위반이 해결 되었습니다.

### <a name="add-privilege-separation-using-xpc"></a>권한 분리 XPC를 사용 하 여 추가

앱 샌드박스에 대 한 Xamarin.Mac 앱을 개발 하는 경우 권한 및 액세스의 조건에 앱의 동작을 살펴보고 자신의 XPC 서비스로 위험 수준이 높은 작업을 구분 하는 것이 좋습니다.

자세한 내용은 Apple의을 참조 하세요 [XPC 서비스 만들기](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6) 하 고 [디먼 및 서비스 프로그래밍 가이드](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i)합니다.

### <a name="implement-a-migration-strategy"></a>마이그레이션 전략을 구현 합니다.

샌드박스가 적용 된 새 버전이 이전에 샌드박스 없는 Xamarin.Mac 응용 프로그램을 해제 하는 경우에 현재 사용자는 원활한 업그레이드 경로 있는지 확인 해야 합니다. 

 컨테이너 마이그레이션 매니페스트를 구현 하는 방법에 자세한 내용은 Apple [샌드박스에서로 앱 마이그레이션](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1) 설명서.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 응용 프로그램을 자세히 살펴보고 샌드 박싱을 수행 했습니다. 먼저 만들었습니다 단순히 Xamarin.Mac 앱을 앱 샌드박스의 기본 사항을 보여 줍니다. 다음으로, 샌드박스 위반을 해결 하는 방법을 살펴보았습니다. 대 한 심층적인 했습니다 그런 다음 앱 샌드박스를 살펴보고 마지막으로 Xamarin.Mac 앱을 앱 샌드박스 디자인 살펴보았습니다.



## <a name="related-links"></a>관련 링크

- [앱 스토어에 게시](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [앱 샌드박스 하는 방법에 대 한](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [일반적인 앱 샌드 박싱 문제](https://developer.apple.com/library/content/qa/qa1773/_index.html)
