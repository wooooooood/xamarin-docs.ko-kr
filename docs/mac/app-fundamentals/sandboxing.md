---
title: 샌드 박싱 Xamarin.Mac 응용 프로그램
description: 이 문서에서는 샌드 박싱 앱 스토어에서 릴리스에 대 한 Xamarin.Mac 응용 프로그램에 설명 합니다. 모든 컨테이너 디렉터리, 권한 부여, 사용자가 지정한 사용 권한, 권한 분할 및 커널 적용 같은 샌드 박싱 속하게 될 요소를 처리 합니다.
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a02d7639975de092b05f31bacedd6bde4c9392f9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30787911"
---
# <a name="sandboxing-a-xamarinmac-app"></a>샌드 박싱 Xamarin.Mac 응용 프로그램

_이 문서에서는 샌드 박싱 앱 스토어에서 릴리스에 대 한 Xamarin.Mac 응용 프로그램에 설명 합니다. 모든 컨테이너 디렉터리, 권한 부여, 사용자가 지정한 사용 권한, 권한 분할 및 커널 적용 같은 샌드 박싱 속하게 될 요소를 처리 합니다._

## <a name="overview"></a>개요

를 사용할 때 C# 및.NET Xamarin.Mac 응용 프로그램에서 Objective-c 또는 Swift에서 작업할 때 작업을 수행 하는 대로 샌드박스 응용 프로그램에 동일한 수가 있습니다.

[![실행 중인 응용 프로그램의 예로](sandboxing-images/intro01.png "실행 중인 응용 프로그램의 예")](sandboxing-images/intro01-large.png#lightbox)

이 문서에서는 샌드 박싱 Xamarin.Mac 응용 프로그램과 모든 샌드 박싱으로 이동 하는 요소에 작업의 기본 사항을 설명 합니다: 컨테이너 디렉터리, 권한 부여, 사용자가 지정한 사용 권한, 권한 분할 및 커널 적용 합니다. 것이 가장 좋습니다를 통해 협력 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서는 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 섹션으로이 문서에서 사용할 수 있는 주요 개념 및 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 특성 요소 Objective-C 개체 및 UI에 C# 클래스를 연결 하는 데 사용 합니다.

## <a name="about-the-app-sandbox"></a>샌드박스 응용 프로그램에 대 한

앱 샌드박스 응용 프로그램은 시스템 리소스에 액세스를 제한 하 여 Mac에서 실행 되 고 악성 응용 프로그램에 의해 발생할 수 있는 손상에 대 한 강력한 철저 한 방어를 제공 합니다.

비 샌드 박싱된 응용 프로그램을 응용 프로그램을 실행 하는 사용자의 모든 권한을 가진 및 액세스 하거나 사용자가 수행할 수 있는 모든 작업을 수행할 수 있습니다. 응용 프로그램 보안 허점을 (또는 사용 하는 모든 프레임 워크) 있으면 해커가 이러한 취약점을 악용 고 Mac에서 실행 되는 제어를 응용 프로그램을 사용 하 여 잠재적으로 수 있습니다.

응용 프로그램 별로 리소스에 대 한 액세스를 제한 하 여 샌드박스 응용 프로그램 도난 완화, 손상 또는 악의적인 사용자의 컴퓨터에서 실행 중인 응용 프로그램 부분을 막는 최전방의 줄을 제공 합니다.

앱 샌드박스는 다음과 같이 두 가지 전략을 제공 하는 macOS (커널 수준에서 적용 됨)에 기본 제공 되는 액세스 제어 기술:

1. 설명 하기 위해 개발자를 사용 하는 앱 샌드박스 _어떻게_ 응용 프로그램은 상호 작용 하는 운영 체제, 이러한 방식으로, 부여 되지 않는 작업을 수행 하는 데 필요 하 고 더 이상 액세스 권한.
2. 앱 샌드박스 원활 하 게 열기를 통해 시스템에 대 한 추가 액세스 권한을 부여 하 고 저장 대화 상자, 끌어서 놓기 작업 및 기타, 공통 사용자 상호 작용을 수 있습니다.

### <a name="preparing-to-implement-the-app-sandbox"></a>앱 샌드박스 구현 준비

문서에 자세히 설명 하는 앱 샌드박스의 요소는 다음과 같습니다.

- 컨테이너 디렉터리
- 권한 부여
- 사용자가 지정한 사용 권한
- 권한 분할
- 커널 적용

이러한 세부 정보를 파악 한 후 앱 샌드박스 Xamarin.Mac 응용 프로그램에서 채택에 대 한 계획을 만들 수 있습니다.

첫째, 응용 프로그램에 적합 한 (대부분의 응용 프로그램은) 샌드 박싱 인지 확인 해야 합니다. 다음으로, API 비 호환성을 해결 하 고 필요한 앱 샌드박스는 요소를 결정 해야 합니다. 마지막으로, 응용 프로그램의 철저 한 방어 수준을 최대화 하기 위해 권한 분할을 사용 하 여 찾습니다.

앱 샌드박스를 채택 하는 경우 응용 프로그램이 사용 하는 일부 파일 시스템 위치 달라 집니다. 특히, 응용 프로그램 응용 프로그램 지원 파일, 데이터베이스, 캐시 및 사용자 문서 되지 않은 다른 파일에 사용할 컨테이너 디렉터리를 갖습니다. MacOS와 Xcode를 둘 다에 컨테이너에 이전 위치에서 이러한 유형의 파일을 마이그레이션할 지원을 제공 합니다.

## <a name="sandboxing-quick-start"></a>샌드 박싱 빠른 시작

이 섹션에서는 앱 샌드박스 시작의 예를 들어 웹 보기 (구체적으로 요청 하지 않는 샌드 박싱에서 제한 되는 네트워크 연결 필요)를 사용 하는 간단한 Xamarin.Mac 앱을 만들겠습니다.

우리는 응용 프로그램이 실제로 샌드박스 이므로 일반적인 샌드박스 응용 프로그램 오류를 해결 하는 방법에 알아봅니다으로 확인 합니다.

### <a name="creating-the-xamarinmac-project"></a>Xamarin.Mac 프로젝트 만들기

우리의 샘플 프로젝트를 만들려면 다음을 실행 해 보겠습니다.

1. 클릭 하 고 Mac에 대 한 Visual Studio를 시작 합니다.는 **새 솔루션...** 추가합니다.
2. **새 프로젝트** 대화 상자에서 **Mac** > **앱** > **Cocoa 앱**: 

    [![새 Cocoa 앱 만들기](sandboxing-images/sample01.png "새 Cocoa 앱 만들기")](sandboxing-images/sample01-large.png#lightbox)
3. 클릭는 **다음** 단추, 입력 `MacSandbox` 클릭 이나 프로젝트 이름에는 **만들기** 단추: 

    [![응용 프로그램 이름 입력](sandboxing-images/sample02.png "앱 이름 입력")](sandboxing-images/sample02-large.png#lightbox)
4. 에 **솔루션 패드**, 두 번 클릭는 **Main.storyboard** Xcode에서 편집을 위해 열 파일입니다. 

    [![주 스토리 보드 편집](sandboxing-images/sample03.png "주 스토리 보드를 편집 합니다.")](sandboxing-images/sample03-large.png#lightbox)
5. 끌어서는 **웹 보기** 는 창으로 크기를 조정 콘텐츠 영역을 입력 하 고 확장 및 축소 창을 사용 하도록 설정 합니다. 

    [![웹 보기 추가](sandboxing-images/sample04.png "웹 보기를 추가 합니다.")](sandboxing-images/sample04-large.png#lightbox)
6. 웹 보기에 대 한 콘센트 만들기 `webView`: 

    [![만드는 새 콘센트](sandboxing-images/sample05.png "새 콘센트 만들기")](sandboxing-images/sample05-large.png#lightbox)
7. Mac 및 두 번 클릭에 대 한 Visual Studio로 되돌아가려면는 **ViewController.cs** 파일을 **솔루션 패드** 를 편집 하기 위해 엽니다.
8. 다음 추가 문을 사용 하 여: `using WebKit;`
9. 확인 된 `ViewDidLoad` 다음과 같은 메서드 보기: 

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();
    webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
}
```

10. 변경 내용을 저장합니다.

응용 프로그램을 실행 하 고 Apple 웹 사이트에서 다음과 같은 창에 표시 되는지 확인 합니다.

[![예제 응용 프로그램을 실행 표시](sandboxing-images/sample06.png "는 예제 응용 프로그램을 실행 표시")](sandboxing-images/sample06-large.png#lightbox)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>서명 및 앱 프로 비전

앱 샌드박스를 사용 하도록 설정할 수 있습니다, 전에 먼저 프로 비전 하 고 Xamarin.Mac 응용 프로그램에 서명 해야 합니다.

다음을 수행 하도록 합니다.

1. Apple 개발자 포털에 로그인 합니다. 

    [![Apple 개발자 포털에 로그인 할](sandboxing-images/sign01.png "Apple 개발자 포털에 로그인")](sandboxing-images/sign01-large.png#lightbox)
2. 선택 **인증서, 식별자 및 프로필**: 

    [![인증서, 식별자 및 프로필 선택](sandboxing-images/sign02.png "인증서, 식별자 및 프로필 선택")](sandboxing-images/sign02-large.png#lightbox)
3. 아래 **Mac 앱**선택, **식별자**: 

    [![식별자를 선택 하면](sandboxing-images/sign03.png "식별자를 선택 합니다.")](sandboxing-images/sign03-large.png#lightbox)
4. 응용 프로그램에 대 한 새 ID를 만듭니다. 

    [![새 앱 ID를 만들어](sandboxing-images/sign04.png "새 앱 ID를 만들어")](sandboxing-images/sign04-large.png#lightbox)
5. 아래 **프로 비전 프로필**선택, **개발**: 

    [![개발을 선택 하면](sandboxing-images/sign05.png "개발을 선택 합니다.")](sandboxing-images/sign05-large.png#lightbox)
6. 새 프로필을 만들고 선택 **Mac 응용 프로그램 개발**: 

    [![새 프로필 작성](sandboxing-images/sign06.png "새 프로필 작성")](sandboxing-images/sign06-large.png#lightbox)
7. 위에서 만든 앱 ID를 선택 합니다. 

    [![앱 ID를 선택 하면](sandboxing-images/sign07.png "앱 ID를 선택 합니다.")](sandboxing-images/sign07-large.png#lightbox)
8. 이 프로필에 대 한 개발자가 제공을 선택 합니다. 

    [![추가 개발자](sandboxing-images/sign08.png "추가 개발자")](sandboxing-images/sign08-large.png#lightbox)
9. 이 프로필에 대 한 컴퓨터를 선택 합니다. 

    [![허용 된 컴퓨터를 선택 하면](sandboxing-images/sign09.png "허용 된 컴퓨터를 선택 합니다.")](sandboxing-images/sign09-large.png#lightbox)
10. 프로필 이름을 지정 합니다. 

    [![프로필에 이름을 지정](sandboxing-images/sign10.png "프로 파일에 이름을 지정")](sandboxing-images/sign10-large.png#lightbox)
11. 클릭는 **수행** 단추입니다.

> [!IMPORTANT]
> 경우에 따라서는 Apple 개발자 포털에서 직접 새 프로비저닝 프로필을 다운로드 하 여 두 번 클릭 설치를 할 수 있습니다. 중지 하 고 새 프로필에 액세스할 수 됩니다 Mac 용 Visual Studio를 다시 시작을 할 수도 있습니다.

다음으로 개발 컴퓨터에서 새 앱 ID와 프로필을 로드 해야 합니다. 다음을 수행 하겠습니다.

1. Xcode를 시작 하 고 선택 **기본 설정** 에서 **Xcode** 메뉴: 

    ![Xcode에 계정 편집](sandboxing-images/sign11.png "Xcode에 계정 편집")
2. 클릭는 **세부 정보를 확인 중...**  단추: 

    ![자세히 보기 단추를 클릭 하면](sandboxing-images/sign12.png "자세히 보기 단추를 클릭 하면")
3. 클릭는 **새로 고침** 단추 (왼쪽 아래 모서리에).
4. 클릭는 **수행** 단추입니다.

다음으로, Xamarin.Mac 프로젝트에 새 앱 ID 및 프로비저닝 프로필을 선택 해야 합니다. 다음을 수행 하겠습니다.

1. 에 **솔루션 패드**, 두 번 클릭은 **Info.plist** 편집을 위해 열 파일입니다.
2. 확인 된 **번들 식별자** 위에서 만든 우리의 응용 프로그램 ID와 일치 (예: `com.appracatappra.MacSandbox`): 

    [![번들 식별자 편집](sandboxing-images/sign13.png "번들 식별자를 편집 합니다.")](sandboxing-images/sign13-large.png#lightbox)
3. 다음으로 두 번 클릭은 **Entitlements.plist** 파일을 확인 우리의 **iCloud 키-값 저장소** 및 **iCloud 컨테이너** 모두 일치 위에서 만든 우리의 앱 ID (예: `com.appracatappra.MacSandbox`): 

    [![Entitlements.plist 파일을 편집](sandboxing-images/sign17.png "Entitlements.plist 파일 편집")](sandboxing-images/sign17-large.png#lightbox)
3. 변경 내용을 저장합니다.
4. 에 **솔루션 패드**, 프로젝트 파일 편집을 위해 해당 옵션을 열려면 두 번 클릭 합니다.  

    ![Editign 솔루션의 옵션](sandboxing-images/sign14.png "Editign 솔루션의 옵션")
5. 선택 **Mac 서명**, 다음 확인 **응용 프로그램 번들을 서명** 및 **설치 관리자 패키지에 서명**합니다. 아래 **프로비저닝 프로필**, 위에서 만든 항목을 선택 합니다. 

    ![프로 비전 프로필 설정](sandboxing-images/sign15.png "프로 비전 프로필 설정")
6. 클릭는 **수행** 단추입니다.

> [!IMPORTANT]
> 종료 되 고 새 앱 ID와 Xcode에 의해 설치 된 프로비저닝 프로필을 인식 하도록 올려 Mac 용 Visual Studio를 다시 시작 해야 합니다.

#### <a name="troubleshooting-provisioning-issues"></a>프로 비전 문제 해결

이 시점에서 하려고 노력 해야 응용 프로그램을 실행 하 고 있는지 모든 것에 서명이 되어 있고 올바르게 프로 비전 해야 합니다. 앱 여전히 실행 되는 경우 이전 처럼, 모든 것이 좋습니다. 오류가 발생 하면 다음과 같은 대화 상자가 나타날 수 있습니다.

[![프로비저닝 문제가 대화 상자 예](sandboxing-images/sign16.png "프로비저닝 문제가 대화 상자 예제")](sandboxing-images/sign16-large.png#lightbox)

프로 비전 하 고 서명 문제 가장 일반적인 원인은 다음과 같습니다.

- 앱 번들 ID에는 선택한 프로필의 앱 ID 일치 하지 않습니다.
- 개발자 ID에는 선택한 프로필의 개발자 ID와 일치 하지 않습니다.
- 선택한 프로필의 일부로 등록 되지 않은에서 테스트 되 고 Mac의 UUID입니다.

문제가 발생 하는 경우 Apple 개발자 포털에서 문제 해결, Xcode에서 프로필 새로 고치고 Mac.에 대 한 Visual Studio에서 클린 빌드를 수행 합니다.

### <a name="enable-the-app-sandbox"></a>앱 샌드박스를 사용 하도록 설정

프로젝트 옵션의 확인란을 선택 하 여 앱 샌드박스를 사용 합니다. 다음을 수행합니다.

1. 에 **솔루션 패드**, 두 번 클릭은 **Entitlements.plist** 편집을 위해 열 파일입니다.
2. 모두 선택 **자격 사용 하도록 설정** 및 **앱 샌드 박싱을 사용 하도록 설정**: 

    [![자격을 편집 하 고 샌드 박싱을 사용 하도록 설정](sandboxing-images/sign17.png "자격 편집 및 샌드 박싱을 사용 하도록 설정")](sandboxing-images/sign17-large.png#lightbox)
3. 변경 내용을 저장합니다.

이 시점에서 앱 샌드박스 사용 하도록 설정한 하지만 웹 보기에 대 한 필요한 네트워크 액세스를 제공 하지 않았습니다. 지금 응용 프로그램을 실행 하는 경우 빈 창이 받습니다.

[![차단 되 고 웹 액세스를 보여 주는](sandboxing-images/sample08.png "차단 되 고 웹 액세스를 보여 주는")](sandboxing-images/sample08-large.png#lightbox)

### <a name="verifying-that-the-app-is-sandboxed"></a>샌드 박싱된 응용 프로그램 인지 확인

차단 동작 리소스 외에도 세 가지 주요 방법은 Xamarin.Mac 응용 프로그램을 성공적으로 샌드박스 되었음을 알 수 있습니다.

1. 검색기의 콘텐츠를 확인는 `~/Library/Containers/` 폴더-샌드박스가 적용 하는 경우 됩니다 앱의 번들 식별자와 같은 명명 된 폴더 (예: `com.appracatappra.MacSandbox`): 

    [![앱의 번들 열기](sandboxing-images/sample09.png "앱의 번들 열기")](sandboxing-images/sample09-large.png#lightbox)
2. 시스템에서는 작업 모니터에서 차단으로 응용 프로그램을 확인합니다.
    - 작업 모니터를 시작할 (아래 `/Applications/Utilities`). 
    - 선택 **보기** > **열** 되어 있는지 확인 하 고는 **샌드박스** 메뉴 항목을 선택 합니다.
    - 샌드박스 열의 레이블은 있는지 확인 하십시오. `Yes` 응용 프로그램: 

    [![작업 모니터의 응용 프로그램을 확인 하는 중](sandboxing-images/sample10.png "작업 모니터의 응용 프로그램을 확인 하는 중")](sandboxing-images/sample10-large.png#lightbox)
3. 샌드 박싱된 응용 프로그램 이진 인지 확인 합니다.
    - 터미널 앱을 시작 합니다.
    - 응용 프로그램으로 이동 `bin` 디렉터리입니다.
    - 이 명령을 실행: `codesign -dvvv --entitlements :- executable_path` (여기서 `executable_path` 응용 프로그램의 경로): 

    [![명령줄에서 응용 프로그램 확인](sandboxing-images/sample11.png "명령줄에서 응용 프로그램 확인")](sandboxing-images/sample11-large.png#lightbox)

### <a name="debugging-a-sandboxed-app"></a>샌드 박싱된 응용 프로그램 디버깅

기본적으로 샌드 박싱을 사용 하도록 설정 하면 해당 하지 않음을 응용 프로그램에 연결할 수, TCP 통해 Xamarin.Mac 앱에 디버거 연결 적절 한 권한을 사용 하지 않고 앱을 실행 하려고 하면 오류가 발생 하므로 *"연결할 수 없습니다. 디버거"* 합니다. 

[![필요한 옵션 설정](sandboxing-images/debug01.png "필요한 옵션 설정")](sandboxing-images/debug01-large.png#lightbox)

**나가는 네트워크 연결 허용 (클라이언트)** 권한에 디버거에 대 한 필요한,이 중 하나를 사용 하도록 설정 하면 정상적으로 디버깅 합니다. 없이 디버깅할 수 없습니다, 이후 업데이트는 `CompileEntitlements` 에 대 한 대상 `msbuild` 가 디버그에 대 한 보안으로 보호 하는 앱만 빌드에 대해 해당 사용 권한을 자격에 자동으로 추가 합니다. 릴리스 빌드는 수정 되지 않은 자격 파일에 지정 된 자격을 사용 해야 합니다.

### <a name="resolving-an-app-sandbox-violation"></a>응용 프로그램 샌드박스 위반이 해결

응용 프로그램에서 있는 리소스에 액세스 하려고 샌드박스 Xamarin.Mac 명시적으로 허용 되지 앱 샌드박스 위반이 발생 합니다. 예를 들어 우리의 웹 보기 Apple 웹 사이트를 표시 하기 위해서는 더 이상입니다.

응용 프로그램 샌드박스 위반의 가장 일반적인 원인은 Mac 용 Visual Studio에서 지정 되는 권한 설정은 응용 프로그램의 요구 사항을 일치 하지 않으면 발생 합니다. 다시 다시 예에서 누락 된 네트워크 연결 웹 보기에서 작업 중인 상태로 유지 하는 권한 부여 합니다.

#### <a name="discovering-app-sandbox-violations"></a>응용 프로그램 샌드박스 위반 검색

사용 하 여 가장 빠른 방법은 문제를 검색 하는 앱 샌드박스 위반이 Xamarin.Mac 응용 프로그램에서 발생 하는 의심 되는 경우는 **콘솔** 응용 프로그램입니다.

다음을 수행합니다.

1. 에 응용 프로그램을 컴파일하고 Mac.에 대 한 Visual Studio에서 실행
2. 열기는 **콘솔** 응용 프로그램 (에서 `/Applications/Utilties/`).
3. 선택 **모든 메시지** 사이드바에서 입력 `sandbox` 검색에: 

    [![콘솔에서 샌드 박싱 문제의 예](sandboxing-images/resolve01.png "콘솔에서 샌드 박싱 문제의 예")](sandboxing-images/resolve01-large.png#lightbox)

위의 예제에서는 앱에서 볼 수 있습니다는 Kernal 차단는 `network-outbound` 트래픽 앱 샌드박스로 인해, 해당 권한을 요청 하지 않았으므로 म 때문입니다.

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>자격과 위반 샌드박스 응용 프로그램 수정

샌드 박싱 위반 응용 프로그램을 찾는 방법을 살펴본 했으므로 이제 응용 프로그램의 자격을 조정 하 여 해결할 수 있습니다 어떻게 확인해 보겠습니다.

다음을 수행합니다.

1. 에 **솔루션 패드**, 두 번 클릭은 **Entitlements.plist** 편집을 위해 열 파일입니다.
2. 아래는 **자격** 섹션을 검사는 **나가는 네트워크 연결 허용 (클라이언트)** 확인란: 

    [![자격 편집](sandboxing-images/sign17.png "자격 편집")](sandboxing-images/sign17-large.png#lightbox)
3. 응용 프로그램에 변경 내용을 저장 합니다.

म 위의 모든 예제에 대 한 작업, 다음 빌드를 실행할 예상 대로 웹 콘텐츠 표시 이제 됩니다.

## <a name="the-app-sandbox-in-depth"></a>심층 분석 앱 샌드박스

샌드박스 응용 프로그램에서 제공 하는 액세스 제어 메커니즘은 몇 가지 및 이해 하기 쉬운입니다. 그러나 각 응용 프로그램에 의해 앱 샌드박스를 채택 될 됩니다 방법은 고유 하 고 응용 프로그램의 요구 사항에 따라입니다.

악성 코드에 의해 악용 되 Xamarin.Mac 응용 프로그램을 보호 하기 위해 최상의 노력에 들어 있을 필요는 없으며 단일 취약점만 응용 프로그램 중 하나 (또는 라이브러리 또는 프레임 워크 중 하나를 소비)와 응용 프로그램의 상호 작용을 제어할 수는 시스템입니다.

앱 샌드박스 함으로써 시스템과 응용 프로그램의 의도 한 상호 작용을 지정할 수는 인수를 방지 합니다 (또는 발생할 수 있으므로 손상을 제한) 하도록 설계 되었습니다. 시스템에만 응용 프로그램의 작업을 수행 하는 데 필요한 리소스 하며 그 이상의에 대 한 액세스 권한을 부여할 됩니다.

앱 샌드박스를 디자인할 때 최악의 시나리오를 디자인 하는 합니다. 응용 프로그램에 악성 코드가 손상 될 경우에 파일 및 응용 프로그램의 샌드박스의 리소스에만 액세스 하도록 제한 됩니다.

### <a name="entitlements-and-system-resource-access"></a>자격 및 시스템 리소스 액세스

위에서 설명한 것 처럼 샌드박스 되지 않은 Xamarin.Mac 응용 프로그램에 대 한 모든 권한 및 응용 프로그램을 실행 하는 사용자의 액세스 권한이 부여 됩니다. 악성 코드가 손상 될 경우 보호 되지 않은 응용 프로그램 범위의 잠재적 피해를 수행 하는 데 와이드와 유해 동작에 대 한 에이전트 작동할 수 있습니다.

앱 샌드박스를 사용 하면 최소한의 권한이 있는 사용자를 제외한 모든 제거 다음 Xamarin.Mac 앱의 자격을 사용 하 여 필요한 전용으로 다시 사용 하도록 설정 합니다. 

편집 하 여 응용 프로그램의 응용 프로그램 샌드박스 리소스를 수정할 해당 **Entitlements.plist** 파일을 확인 하거나 편집기 드롭다운 상자에서 필요한 권한을 선택 하 합니다.

[![자격 편집](sandboxing-images/sign17.png "자격 편집")](sandboxing-images/sign17-large.png#lightbox)

### <a name="container-directories-and-file-system-access"></a>컨테이너 디렉터리 및 파일 시스템 액세스

Xamarin.Mac 응용 프로그램이 앱 샌드박스를 채택, 다음 위치에 액세스를 권한이 있습니다.

- **응용 프로그램 컨테이너 디렉터리** -처음 실행 시 운영 체제는 특별 한 만듭니다 _컨테이너 디렉터리_ 만 액세스할 수 있다는 모든 리소스가 데이터를 이동 하는 경우. 앱이이 디렉터리에 대 한 모든 읽기/쓰기 액세스를 갖습니다.
- **응용 프로그램 그룹의 컨테이너 디렉터리** -응용 프로그램 하나 이상에 대 한 액세스를 부여할 수 _그룹 컨테이너_ 동일한 그룹에 응용 프로그램에서 공유할입니다.
- **사용자 지정 파일** -응용 프로그램 파일은 명시적으로 열 또는 끌어서 놓을 응용 프로그램 사용자가을에 대 한 액세스를 자동으로 가져옵니다.
- **관련 항목** -사용을 적절 한 자격 응용 프로그램 있습니다 사용할 이름은 같지만 다른 확장명을 사용 하 여 파일입니다. 예를 들어 둘 다로 저장 된 문서는 `.txt` 파일 및 `.pdf`합니다.
- **임시 디렉터리, 명령줄 도구 디렉터리 및 특정 세계를 읽을 수 있는 위치** -응용 프로그램 파일 시스템에 의해 지정 된 대로 잘 정의 된 다른 위치에 다양 한 수준의 액세스에 있습니다.

#### <a name="the-app-container-directory"></a>응용 프로그램 컨테이너 디렉터리

Xamarin.Mac 응용 프로그램의 응용 프로그램 컨테이너 디렉터리에 다음과 같은 특징이 있습니다.

- 사용자의 홈 디렉터리에 숨겨진된 위치에 (일반적으로 `~Library/Containers`)으로 액세스할 수 있습니다는 `NSHomeDirectory` 응용 프로그램 내에서 함수 (아래 참조). 홈 디렉터리에서 이기 때문에 각 사용자는 앱에 대 한 자신의 컨테이너를 받게 됩니다.
- 앱에서 컨테이너 디렉터리와 모든 하위 디렉터리 및 파일 내에 읽기/쓰기 액세스를 제한 없이 합니다.
- 응용 프로그램의 컨테이너를 기준으로 경로 찾기 Api macOS의 대부분입니다. 예를 들어 컨테이너 갖습니다 자체 **라이브러리** (을 통해 액세스 `NSLibraryDirectory`), **응용 프로그램 지원** 및 **기본 설정** 하위 디렉터리입니다.
- macOS 설정 하 고 간의 연결을 적용 하 고 앱 및 코드 서명을 통해 컨테이너입니다. 가 다른 앱 시도 사용 하 여 응용 프로그램을 통해 스푸핑 하에 해당 **번들 식별자**, 코드 서명 때문에 컨테이너에 액세스할 수 없습니다.
- 컨테이너가 생성 된 사용자 파일에 있지 않습니다. 대신 데이터베이스, 캐시 또는 다른 특정 종류의 데이터와 같은 응용 프로그램에서 사용 하는 파일에 대 한는입니다.
- 에 대 한 _신발 상자_ 유형의 (예: Apple의 사진 응용 프로그램) 앱을 사용자의 콘텐츠를 컨테이너에 되돌려집니다.

> [!IMPORTANT]
> 그러나 Xamarin.Mac 없는 아직 (달리 Xamarin.iOS) 100 %API 검사 결과적으로 `NSHomeDirectory` API Xamarin.Mac의 현재 버전에 매핑되지 않았습니다.

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

#### <a name="the-app-group-container-directory"></a>응용 프로그램 그룹의 컨테이너 디렉터리

Mac macOS 10.7.5부터 (이상), 응용 프로그램이 사용할 수는 `com.apple.security.application-groups` 권한 그룹에 모든 응용 프로그램에 공통 된 공유 컨테이너에 액세스할 수 있습니다. 데이터베이스 또는 다른 유형의 지원 파일 (예: 캐시)와 같은 비 사용자 웹 콘텐츠에 대 한 공유이 컨테이너를 사용할 수 있습니다.

그룹 컨테이너 자동으로 추가 되어 각 응용 프로그램의 샌드박스 컨테이너 (그룹의 일부인 경우)에 저장 된 `~/Library/Group Containers/<application-group-id>`합니다. 그룹 ID _해야_ 개발 팀 ID, 마침표, 예를 들어 시작:

```plist
<team-id>.com.company.<group-name>
```

자세한 내용은 Apple의를 참조 하십시오 [응용 프로그램 그룹에 응용 프로그램 추가](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19) 에 [자격 키 참조](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195)합니다.

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>응용 프로그램 컨테이너 외부 Powerbox 및 파일 시스템 액세스

샌드박스 Xamarin.Mac 응용 프로그램은 다음과 같은 방법으로 해당 컨테이너의 외부 파일 시스템 위치를 액세스할 수 있습니다.

- 열기 및 저장 대화 상자 또는 끌어서 놓기 등의 다른 방법을) (통해 사용자의 특정 방향입니다.
- 특정 파일 시스템 위치에 대 한 자격을 사용 하 여 (예: `/bin` 또는 `/usr/lib`).
* 파일 시스템 위치 중인 경우 세계 (예: 공유) 읽을 수 있는 특정 디렉터리입니다.

_Powerbox_ 는 macOS 샌드박스 Xamarin.Mac 앱의 파일 액세스 권한을 확장 하는 사용자와 상호 작용 하는 보안 기술입니다. Powerbox 없는 API를 갖지만 응용 프로그램 호출 때 투명 하 게 활성화 되는 `NSOpenPanel` 또는 `NSSavePanel`합니다. Powerbox 액세스가 Xamarin.Mac 응용 프로그램에 대해 설정한 자격을 통해 사용 됩니다.

때는 샌드 박싱된 응용 프로그램 열려 있는 표시 또는 저장 대화 상자, 창 Powerbox (및 하지 AppKit)를 제시 하며, 파일 또는 디렉터리의 사용자가 액세스할 수 있는 권한이

사용자 열기에서 파일 또는 디렉터리를 선택 하거나 저장 대화 상자 (하거나 끌 때 응용 프로그램의 아이콘 중 하나), Powerbox 응용 프로그램의 샌드박스에 연결 된 경로 추가 합니다.

또한 시스템 다음 샌드 박싱된 응용 프로그램을 자동으로 가능합니다.

- 시스템에 연결 하는 메서드를 입력 합니다.
- 사용자가 선택한 서비스에 문의 **서비스** 메뉴 (으로 표시 된 서비스에 대 한 유일한 _샌드박스 응용 프로그램에 안전_ 서비스 공급자에 의해).
- 사용자가 선택한 열려 있는 파일의 **열기** 메뉴.
- 다른 응용 프로그램 간에 복사 및 붙여넣기를 사용 합니다.
- 다음 세계를 읽을 수 있는 위치에서 파일을 읽을:
    - `/bin`
    - `/sbin`
    - `/usr/bin`
    - `/usr/lib`
    - `/usr/sbin`
    - `/usr/share`
    - `/System`
- 파일 읽기와 쓰기에서 만든 디렉터리에서 `NSTemporaryDirectory`합니다.

기본적 열거나 샌드박스 Xamarin.Mac 앱에서 저장 파일 (없는 경우 파일은 앱이 종료 될 때 계속 열려) 응용 프로그램 종료 될 때까지 액세스할 수 있는 남아 있습니다. 열려 있는 파일이 자동으로 복원 됩니다 macOS 다시 시작 기능을 통해 응용 프로그램의 sandbox에 다음에 응용 프로그램이 시작 됩니다.

Xamarin.Mac 응용 프로그램의 컨테이너 외부에 있는 파일에 대 한 지 속성을 제공 하려면 보안 범위 책갈피 (아래 참조)를 사용 합니다.

#### <a name="related-items"></a>관련된 항목

앱 샌드박스 앱을 여러 확장 프로그램 이지만 동일한 파일 이름을 가진 관련된 항목에 액세스할 수 있습니다. 기능에는 두 부분이: a)에서 응용 프로그램의 관련된 확장의 목록이 `Info.plst` 파일, b) 알려주는 코드를 샌드박스에 이러한 파일을 사용 하 여 어떤 응용 프로그램 수행할 수 있습니다.

두 개의 적합 한 시나리오가이 있습니다.

1. 응용 프로그램 (확장명이 새) 파일의 다른 버전을 저장할 수 있어야 합니다. 예를 들어 내보내기는 `.txt` 파일을 한 `.pdf` 파일입니다. 사용이 상황을 처리 해야 합니다는 `NSFileCoordinator` 파일에 액세스 합니다. 호출 하 여 `WillMove(fromURL, toURL)` 메서드 먼저 새 확장 프로그램에 파일을 이동 하 고 다음 호출 `ItemMoved(fromURL, toURL)`합니다.
2. 응용 프로그램이 다른 확장명을 가진 하나의 확장명을 가진 주 파일 및 몇 가지 지원 파일이 열려면 해야 합니다. 예를 들어, 동영상 및 부제목 파일입니다. 사용 하 여 프로그램 `NSFilePresenter` 보조 파일에 대 한 액세스 권한을 얻으려고 합니다. 주 파일을 제공 된 `PrimaryPresentedItemURL` 속성 및 보조 파일을는 `PresentedItemURL` 속성입니다. 주 파일을 열 때 호출 된 `AddFilePresenter` 의 메서드는 `NSFileCoordinator` 보조 파일을 등록 하는 클래스입니다.

두 시나리오 모두에서 앱의 **Info.plist** 파일에는 앱을 열 수 있는 문서 유형을 선언 해야 합니다. 모든 파일 형식에 대 한 추가 `NSIsRelatedItemType` (값이 `YES`)의 해당 항목에는 `CFBundleDocumentTypes` 배열입니다.

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>열기 및 대화 상자 동작 샌드 박싱된 응용 프로그램과 저장

에 다음 제한 사항이 `NSOpenPanel` 및 `NSSavePanel` 샌드박스 Xamarin.Mac 응용 프로그램에서이 호출할 때:

- 프로그래밍 방식으로 호출할 수 없습니다는 **확인** 단추입니다.
- 에 사용자의 선택은 프로그래밍 방식으로 변경할 수 없습니다는 `NSOpenSavePanelDelegate`합니다.

또한 다음 상속 수정 위치에 있습니다.

- **비 샌드 박싱된 응용 프로그램** - `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>책갈피 보안 범위 및 영구 리소스 액세스

위에서 설명한 대로 샌드박스 Xamarin.Mac 응용 프로그램 파일 또는 제공 된 PowerBox에서 직접 사용자 상호 작용을 통해 해당 컨테이너 외부의 리소스를 액세스할 수 있습니다. 그러나 이러한 리소스에 대 한 액세스 지속 되지 않는 자동으로 실행 하는 경우 응용 프로그램 또는 시스템 다시 시작 합니다.

사용 하 여 _Security-Scoped 책갈피_, 샌드박스 Xamarin.Mac 응용 프로그램에서 사용자 의도 유지 하 고 지정한 후 응용 프로그램 리소스를 다시 시작에 대 한 액세스를 유지할 수 있습니다.

#### <a name="security-scoped-bookmark-types"></a>보안 범위 책갈피 형식

Security-Scoped 책갈피 및 영구 리소스 액세스를 사용할 때에 다음과 같은 두 가지 sistine 사례가:

- **App-Scoped 책갈피는 사용자가 지정한 파일 또는 폴더에 대 한 영구 액세스를 제공합니다.** 

    예를 들어 샌드박스 Xamarin.Mac 응용 프로그램 편집을 위해 외부 문서를 여는 데 사용할 수 있습니다 (사용 하는 `NSOpenPanel`), 나중에 같은 파일에 액세스할 수 있도록 앱 App-Scoped 책갈피를 만들 수 있습니다.
- **Document-Scoped 책갈피는 특정 문서에 지속적으로 액세스 하위 파일을 제공합니다.** 

예를 들어, 프로젝트 파일을 만드는 한 비디오 편집 응용 프로그램에 개별 이미지, 비디오 클립 및 단일 동영상으로 나중에 결합 됩니다는 사운드 파일에 액세스 합니다.

사용자는 프로젝트에 리소스 파일을가 하는 경우 (통해는 `NSOpenPanel`), 파일은 항상 응용 프로그램에 액세스할 수 있도록 응용 프로그램은 프로젝트에 저장 된 항목에 대 한 Document-Scoped 책갈피를 만듭니다.

Document-Scoped 책갈피는 책갈피 데이터와 문서 자체를 열 수 있는 모든 응용 프로그램에서 해결할 수 있습니다. 이 이식성을 사용자가 다른 사용자와도을 작업의 모든 책갈피 있는 프로젝트 파일을 보낼 수 있도록 지원 합니다.

> [!IMPORTANT]
> Document-Scoped 책갈피 수 _만_ 단일 파일 및 폴더가 아니라을 가리키고 해당 파일 시스템에서 사용 되는 위치에 있을 수 없습니다 (예: `/private` 또는 `/Library`).

#### <a name="using-security-scoped-bookmarks"></a>보안 범위 책갈피를 사용 하 여

어떤 유형의 Security-Scoped 책갈피를 사용 하 여 다음 단계를 수행 해야 합니다.

1. **Security-Scoped 책갈피를 사용 하는 Xamarin.Mac 응용 프로그램에서 적절 한 자격을 설정** -For App-Scoped 책갈피 설정 된 `com.apple.security.files.bookmarks.app-scope` 자격 키를 `true`합니다. Document-Scoped 책갈피에 대 한 설정에서 `com.apple.security.files.bookmarks.document-scope` 자격 키를 `true`합니다.
2. **Security-Scoped 책갈피 만들기** -파일 또는 폴더를 사용자가 액세스를 제공 합니다.이 작업을 수행 합니다 (통해 `NSOpenPanel` 예를 들어), 응용 프로그램에 대 한 영구 액세스를 해야 합니다. 사용 하 여는 `public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)` 의 메서드는 `NSUrl` 클래스 여 책갈피를 만듭니다.
3. **Security-Scoped 책갈피 해결** -응용 프로그램 (예: 다시 시작) 후 다시 리소스에 액세스 해야 하는 경우 보안 범위 URL에 책갈피를 해결 해야 합니다. 사용 하 여는 `public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)` 의 메서드는 `NSUrl` 책갈피를 해결 하는 클래스입니다.
4. **Security-Scoped URL에서 파일에 액세스 하려면 시스템을 명시적으로 알려야** -이 단계에서 위의 Security-Scoped URL 가져오기 직후 작업을 수행 해야 하거나, 리소스에 대 한 액세스 권한을 다시 얻으려면 되 고 나면 나중에 하려는 경우 사용자의 액세스는 무시 합니다. 호출의 `StartAccessingSecurityScopedResource ()` 의 메서드는 `NSUrl` 시작 Security-Scoped URL에 액세스 하는 클래스입니다.
5. **작업이 완료 되는 시스템을 명시적으로 알려야 Security-Scoped URL에서 파일에 액세스** -가능한 한 빨리 알려야 시스템 (예를 들어 경우는 사용자가 닫을) 더 이상 응용 프로그램이 파일에 액세스 해야 하는 경우. 호출 된 `StopAccessingSecurityScopedResource ()` 의 메서드는 `NSUrl` Security-Scoped URL에 액세스를 중지 하는 클래스입니다.

리소스에 대 한 액세스를 잃어 후 액세스를 다시 설정 하기 위해 다시 4 단계를 반환 해야 합니다. Xamarin.Mac 앱을 다시 시작 하는 경우에 3 단계로 돌아가세요을 책갈피를 다시 확인 해야 합니다.

> [!IMPORTANT]
> Security-Scoped URL 리소스에 대 한 액세스를 해제 하는 오류는 Xamarin.Mac 앱에서 커널 리소스 누수를 발생 합니다. 결과적으로, 응용 프로그램 더 이상 됩니다 다시 시작할 때까지 해당 컨테이너를 파일 시스템 위치를 추가할 수 없습니다.

### <a name="the-app-sandbox-and-code-signing"></a>샌드박스 응용 프로그램 및 코드 서명

앱 샌드박스를 사용 하도록 설정 하 고 (자격)를 통해 Xamarin.Mac 앱에 대 한 특정 요구 사항을 설정 후 적용 되려면 샌드 박싱에 대 한 서명할 프로젝트를 코딩 해야 합니다. 코드 서명을 앱의 서명과 앱 샌드 박싱 하는 데 필요한 권한에 연결 되기 때문에 수행 해야 합니다. 

macOS는 응용 프로그램의 컨테이너와 다른 응용 프로그램 번들 id입니다. 앱을 스푸핑은 경우에 해당 컨테이너에 액세스할 수 있습니다 이러한 방식으로 해당 코드 서명 사이 링크를 적용 합니다. 이 메커니즘은 다음과 같습니다.

1. 시스템 응용 프로그램의 컨테이너를 만들 때 설정는 _액세스 제어 목록_ (ACL) 해당 컨테이너에 있습니다. 초기 액세스 제어 항목 목록에서 응용 프로그램의 포함 _지정 된 요구 사항을_ (DR) (업그레이드 않았기) 경우 설명 하는 방법을 이후 버전의 앱을 인식할 수 있습니다.
2. 번들 id가 같은 앱이 시작 될 때마다 시스템 응용 프로그램의 코드 서명 요구 사항을 지정 된 컨테이너의 ACL의 항목 중 하나에 지정 된 일치 하는지 확인 합니다. 시스템에서 일치 하는 찾지 못하면에서 시작 하는 응용 프로그램 수 없습니다.

서명 works 다음과 같은 방법으로 코드:

1. Xamarin.Mac 프로젝트를 만들기 전에 Apple 개발자 포털에서 인증서를 개발, 배포 인증서 및 개발자 ID 인증서를 가져옵니다.
2. Mac 앱 스토어 Xamarin.Mac 앱 배포 하 고, 경우에 Apple 코드 서명을 사용 하 여 서명 됩니다.

디버깅을 테스트할 때 Xamarin.Mac 응용 프로그램 사용자가 등록 (하는 만드는 데 사용할 응용 프로그램 컨테이너)의 버전을 사용 합니다. 나중 테스트 하거나 Apple 앱 스토어에서 버전을 설치 하려는 경우 Apple 서명으로 서명 고 (없으므로 원래 응용 프로그램 컨테이너와 같은 코드 서명을) 시작 되지 것입니다. 이 경우에 발생 합니다 충돌 보고를 다음과 비슷합니다.

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

이 해결 하려면 Apple 서명 된 버전의 앱을 가리키도록 ACL 항목을 조정 해야 합니다.

만들고 프로 비전 프로필 샌드 박싱 하는 데 필요한 다운로드에 대 한 자세한 내용은 참조 하십시오는 [서명 및 앱 프로 비전](#Signing_and_Provisioning_the_App) 위의 섹션.

#### <a name="adjusting-the-acl-entry"></a>ACL 항목을 조정합니다.

Apple 서명된 된 버전을 Xamarin.Mac 응용 프로그램 실행을 허용 하려면 다음을 수행 합니다.

1. 터미널 앱을 엽니다 (에서 `/Applications/Utilities`).
2. Apple 서명 된 버전의 Xamarin.Mac 앱 Finder 창을 엽니다.
3. 형식 `asctl container acl add -file ` 터미널 창에서.
4. Finder 창에서 Xamarin.Mac 응용 프로그램의 아이콘을 끌어서 터미널 창에 놓습니다.
5. 파일의 전체 경로 터미널의 명령에 추가 됩니다.
6. 키를 눌러 **Enter** 명령을 실행 합니다.

컨테이너의 ACL에 이제 Xamarin.Mac 응용 프로그램의 두 버전 모두에 대 한 지정 된 코드 요구 사항을 포함 및 macOS 둘 중 한 버전을 실행 하면 이제 합니다.

#### <a name="display-a-list-of-acl-code-requirements"></a>ACL 코드 요구 사항 목록이 표시

다음을 수행 하 여 컨테이너의 ACL에 코드 요구 사항 목록을 볼 수 있습니다.

1. 터미널 앱을 엽니다 (에서 `/Applications/Utilities`).
2. `asctl container acl list -bundle <container-name>`를 입력합니다.
3. 키를 눌러 **Enter** 명령을 실행 합니다.

`<container-name>` 일반적으로 Xamarin.Mac 응용 프로그램에 대 한 번들 식별자입니다.

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>응용 프로그램 샌드박스에 대 한 Xamarin.Mac 응용 프로그램 디자인

앱 샌드박스에 대 한 Xamarin.Mac 응용 프로그램을 디자인할 때 뒤에 야 하는 일반적인 워크플로가 있습니다. 즉, 샌드박스 응용 프로그램의 구현 세부 사항이 지정된 하는 앱의 기능에 고유 하려고 합니다.

### <a name="six-steps-for-adopting-the-app-sandbox"></a>앱 샌드박스 채택 6 단계

일반적으로 앱 샌드박스에 대 한 Xamarin.Mac 응용 프로그램을 디자인 다음 단계로 구성 됩니다.

1. 샌드 박싱에 적합 한 응용 프로그램 인지 확인 합니다.
2. 개발 및 배포 전략을 디자인 합니다.
3. API 비 호환성을 해결 합니다.
4. 필요한 앱 샌드박스 자격 Xamarin.Mac 프로젝트에 적용 됩니다.
5. 권한 분할 XPC를 사용 하 여 추가 합니다.
6. 마이그레이션 전략을 구현 합니다.

> [!IMPORTANT]
> 샌드박스 앱 번들에 포함 된 모든 도우미도 주 실행 파일 뿐만 아니라 해야 응용 프로그램 또는 해당 번들의 도구입니다. Mac 앱 스토어에서 배포 된 모든 앱에 필요한 이므로, 가능한 경우 수행 해야 할 다른 형태의 응용 프로그램 배포.

목록이 Xamarin.Mac 앱의 번들에 모든 실행 가능 이진 파일에 대 한 터미널에 다음 명령을 입력 합니다.

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

여기서 `[Your-App-Bundle]` 이름 및 응용 프로그램의 번들에 대 한 경로입니다.

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Xamarin.Mac 앱 샌드 박싱에 적합 한지 확인

대부분 Xamarin.Mac 앱은 앱 샌드박스 완벽 하 게 호환 지므로 샌드 박싱에 적합 합니다. 앱에서 앱 샌드박스 허용 하지 않는 동작을 필요한 경우 다른 접근 방식은 고려해 야 합니다.

앱에 필요한 다음 동작 중 하나를 경우 앱 샌드박스와 호환 되지 않습니다.

- **권한 부여 서비스** -에 설명 된 함수와 함께 사용할 수 없습니다와 앱 샌드박스에, [권한 부여 서비스 C 참조](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826)합니다.
- **내게 필요한 옵션 Api** -화면 판독기와 같은 샌드박스 보조 앱 또는 다른 응용 프로그램을 제어 하는 앱 수는 없습니다.
- **임의의 앱 Apple 이벤트 보내기** -알 수 없는, 임의 앱에 Apple 이벤트를 전송 해야 하는 응용 프로그램을 하는 경우 샌드박스 수 없습니다. 호출된 응용 프로그램의 알려진된 목록에 대 한 앱 샌드박스 이루어질 수 하 고 권리 호출된 응용 프로그램 목록을 포함 해야 합니다.
- **다른 작업에 사용자 정보 사전 Distributed 알림을 보낼** -포함할 수 없습니다와 앱 샌드박스에는 `userInfo` 사전에 게시할 때는 `NSDistributedNotificationCenter` 메시징 다른 작업에 대 한 개체입니다.
- **커널 확장을 로드** -커널 확장 로드가 있는 앱 샌드박스에 허용 되지 않습니다.
- **열기 및 저장 대화 상자에 사용자 입력을 시뮬레이션** -프로그래밍 방식으로 열기를 조작 하거나 앱 샌드박스에서 저장 대화 상자를 시뮬레이션 하거나 사용자 입력을 변경 하는 금지 합니다.
- **액세스 또는 다른 앱에서 기본 설정을** -앱 샌드박스에서 다른 앱의 설정을 조작가 허용 되지 않습니다.
- **네트워크 설정 구성** -앱 샌드박스에서 조작 네트워크 설정을 사용할 수 없습니다.
- **다른 앱을 종료** -를 사용 하 여 응용 프로그램 샌드박스에 금지 `NSRunningApplication` 다른 앱을 종료 합니다.

### <a name="resolving-api-incompatibilities"></a>API 호환성 문제를 해결합니다.

앱 샌드박스에 대 한 Xamarin.Mac 응용 프로그램을 디자인할 때 일부 macOS Api 사용 하 여 비 호환성 문제가 발생할 수 있습니다.

다음은 몇 가지 일반적인 문제 및 해결을 위해 수행할 수 있는 것입니다.

- **열기, 저장 하 고, 문서 추적** 이외의 모든 기술을 사용 하 여 문서를 관리 하는 경우- `NSDocument`, 앱 샌드박스에 대 한 기본 제공된 지원으로 인해를 전환 해야 합니다. `NSDocument` 자동으로 PowerBox 작동 하 고 사용자가 파인더를 이동 하는 경우 샌드박스에 내에서 문서를 유지 하도록 지원 합니다.
- **파일 시스템 리소스에 대 한 액세스를 유지할** -응용 프로그램은 해당 컨테이너의 외부 리소스에 대 한 영구 액세스에 따라 달라 집니다 Xamarin.Mac 책갈피 보안 범위를 사용 하 여 액세스를 유지 하는 경우.
- **응용 프로그램에 대 한 로그인 항목을 만듭니다.** -the App 샌드박스와 항목을 사용 하는 로그인을 만들 수 없습니다 `LSSharedFileList` 사용 하 여 시작 서비스의 상태를 조작할 수 나 `LSRegisterURL`합니다. 사용 하 여는 `SMLoginItemSetEnabled` 사과에 설명 된 대로 작동 [서비스 관리 프레임 워크를 사용 하 여 로그인 항목을 추가](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1) 설명서입니다.
- **사용자 데이터에 액세스** -같은 POSIX 함수를 사용 하는 경우 `getpwuid` 디렉터리 서비스에서 사용자의 홈 디렉터리를 가져오려면 Cocoa 또는 코어 Foundation 기호를 사용 하 여 같은 것이 좋습니다 `NSHomeDirectory`합니다.
- **앱에 액세스 하는 기본 설정의 다른** -앱 샌드박스 지시 경로 찾기 Api 응용 프로그램의 컨테이너에 해당 컨테이너 내에서 기본 설정을 수행을 수정 하 고 액세스에서 다른 응용 프로그램 기본 설정을 사용할 수 없습니다. 
- **웹 보기에 포함 된 HTML5 비디오를 사용 하 여** -WebKit를 사용 하 여 포함 된 HTML5 비디오를 재생 하는 Xamarin.Mac 응용 프로그램, AV Foundation 프레임 워크에 대 한 앱 연결 해야 합니다. 앱 샌드박스 하면 CoreMedia play에서 이러한 비디오 그렇지 않으면 없습니다.

### <a name="applying-required-app-sandbox-entitlements"></a>필요한 앱 샌드박스 자격 적용

앱 샌드박스에서 실행 하 고 확인 하려는 모든 Xamarin.Mac 응용 프로그램에 대 한 자격을 편집 해야 합니다는 **앱 샌드 박싱을 사용 하도록 설정** 확인란을 선택 합니다.

앱의 기능에 따라, OS 기능 또는 리소스에 액세스 하는 다른 자격을 사용 하도록 할 수 있습니다. 따라서 앱 샌드 박싱은 응용 프로그램을 실행 하는 데 필요한 최소한 요청이 자격 최소화 하면 자격 사용 임의로 방금 마십시오.

Xamarin.Mac 앱을 사용 하려면 어떤 자격을 확인 하려면 다음을 수행 합니다.

1. 앱 샌드박스를 사용 하도록 설정 하 고 Xamarin.Mac 응용 프로그램을 실행 합니다.
2. 앱의 기능을 통해 실행 합니다.
3. 콘솔 앱을 엽니다 (에서 사용할 수 있는 `/Applications/Utilities`)를 찾아서 `sandboxd` 위반은 **모든 메시지** 로그 합니다.
4. 각 `sandboxd` 위반을 다른 파일 시스템 위치 하는 대신 응용 프로그램 컨테이너를 사용 하 여 문제를 해결 하거나 제한 된 OS 기능에 액세스할 수 있도록 앱 샌드박스 자격을 적용 합니다.
5. 다시 실행 하 고 다시 모든 Xamarin.Mac 앱 기능을 테스트 합니다.
6. 모든 올 때까지 반복 `sandboxd` 위반이 해결 되었습니다.

### <a name="add-privilege-separation-using-xpc"></a>권한 분할 XPC를 사용 하 여 추가

앱 샌드박스에 대 한 Xamarin.Mac 응용 프로그램을 개발할 때는 응용 프로그램의 동작 권한 및 액세스 하 고 약관에 확인 한 후 위험 수준이 높은 작업을 자신의 XPC 서비스도 구분 하는 것이 좋습니다.

자세한 내용은 Apple의을 참조 하십시오. [XPC 서비스를 만드는](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6) 및 [데몬 및 서비스 프로그래밍 가이드](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i)합니다.

### <a name="implement-a-migration-strategy"></a>마이그레이션 전략을 구현 합니다.

이전에 샌드박스 없었던 Xamarin.Mac 응용 프로그램의 샌드박스가 적용 된 새 버전이 공개 하는 경우 현재 사용자가 원활한 업그레이드 경로 확인 해야 합니다. 

 Apple의 읽기에 컨테이너 마이그레이션 매니페스트를 구현 하는 방법에 대 한 내용은 [샌드박스로 앱 마이그레이션](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1) 설명서입니다.

## <a name="summary"></a>요약

이 문서를 자세히 살펴보려면 샌드 박싱 Xamarin.Mac 응용 프로그램을 수행 했습니다. 첫째,는 단순히 Xamarin.Mac 앱 앱 샌드박스의 기본 표시를 만들었는지 여부입니다. 다음으로, 샌드박스 위반을 해결 하는 방법을 살펴보았습니다. 한 깊이 있는 제거한 다음, 앱 샌드박스를 살펴보고 마지막으로, 앱 샌드박스에 대 한 Xamarin.Mac 응용 프로그램 디자인 살펴보았습니다.



## <a name="related-links"></a>관련 링크

- [앱 스토어에 게시](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [샌드박스 응용 프로그램에 대 한](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [일반적인 앱 샌드 박싱 문제](https://developer.apple.com/library/content/qa/qa1773/_index.html)
