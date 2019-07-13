---
title: Xamarin.Mac의 스토리 보드를 사용 하 여 작업
description: 이 문서 응답자 체인, 코드 및 뷰 컨트롤러 수명 주기에서이 로드 하는 방법 검사, Xamarin.Mac의 스토리 보드를 사용 하는 방법에 설명, segue 제스처 인식기 창 컨트롤러 등입니다.
ms.prod: xamarin
ms.assetid: DF4DF7C2-DDD7-4A32-B375-5C5446301EC5
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: e24b448bedc60a537bfcd4a5bfbdbe9562163818
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865929"
---
# <a name="working-with-storyboards-in-xamarinmac"></a>Xamarin.Mac의 스토리 보드를 사용 하 여 작업

스토리 보드를 모든 해당 뷰 컨트롤러의 기능 개요를 분해할 지정된 된 앱에 대 한 UI를 정의 합니다. Xcode의 Interface Builder에서 각 이러한 컨트롤러 자체 장면에 상주합니다.

[![](indepth-images/intro01.png "Xcode의 Interface Builder에서 스토리 보드")](indepth-images/intro01.png#lightbox)

스토리 보드는 리소스 파일 (확장명을 가진 `.storyboard`) 컴파일되고 제공 될 때 Xamarin.Mac 앱의 번들에 포함 됩니다. 편집할 앱에 대 한 시작 스토리 보드를 정의 하려면의 `Info.plist` 파일을 선택 합니다 **주 인터페이스** 드롭다운 상자에서: 

[![](indepth-images/sb01.png "Info.plist 편집기")](indepth-images/sb01.png#lightbox)

<a name="Loading-from-Code" />

## <a name="loading-from-code"></a>코드에서 로드

코드에서 특정 스토리 보드를 로드 하 고 뷰 컨트롤러를 수동으로 만들 해야 할 때 시간이 있을 수 있습니다. 이 작업을 수행 하려면 다음 코드를 사용할 수 있습니다.

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);
```

`FromName` 앱의 번들에 포함 된 지정 된 이름을 사용 하 여 스토리 보드 파일을 로드 합니다. `InstantiateControllerWithIdentifier` 지정한 Id를 사용 하 여 뷰 컨트롤러의 인스턴스를 만듭니다. UI를 디자인할 때는 Xcode의 Interface Builder에서 Id를 설정 합니다.

[![](indepth-images/sb02.png "Storyboard ID를 설정합니다.")](indepth-images/sb02.png#lightbox)

필요한 경우 사용할 수는 `InstantiateInitialController` Interface Builder에서 초기 컨트롤러를 할당 된 뷰 컨트롤러를 로드 하는 방법.

[![](indepth-images/sb03.png "초기 컨트롤러 설정")](indepth-images/sb03.png#lightbox)

으로 표시 되는 **스토리 보드 진입점** 및 위의 오픈 종료 화살표입니다.

<a name="View-Controllers" />

## <a name="view-controllers"></a>뷰 컨트롤러

뷰 컨트롤러는 지정 된 Mac 앱 내에서 정보 보기 및 해당 정보를 제공 하는 데이터 모델 간의 관계를 정의 합니다. 스토리 보드의 각 최상위 장면 Xamarin.Mac 앱의 코드에서 한 뷰 컨트롤러를 나타냅니다.

<a name="The-View-Controller-Lifecycle" />

### <a name="the-view-controller-lifecycle"></a>뷰 컨트롤러 수명 주기

에 추가 된 새 메서드가 여러 개를 `NSViewController` macOS에서 스토리 보드를 지원 하기 위해 클래스입니다. 가장 중요 한 점은 다음과 같습니다. 다음 메서드를 사용 하 여 지정 된 뷰 컨트롤러에서 제어 하 고 뷰의 수명 주기에 응답할

- `ViewDidLoad` -이 메서드는 스토리 보드 파일에서 뷰를 로드할 때 호출 됩니다.
- `ViewWillAppear` -이 메서드는 보기 화면에 표시 되는 바로 전에 호출 됩니다.
- `ViewDidAppear` -이 메서드는 보기 화면에 표시 된 후 바로 호출 됩니다.
- `ViewWillDisappear` -이 메서드는 보기 화면에서 제거 되기 바로 전에 호출 됩니다.
- `ViewDidDisappear` -이 메서드는 화면에서 보기를 제거한 후 바로 호출 됩니다.
- `UpdateViewConstraints` -이 메서드는 뷰를 정의 하는 제약 조건을 자동 레이아웃 위치 및 크기가 업데이트 해야 하는 경우 호출 됩니다.
- `ViewWillLayout` -이 메서드는이 뷰의 하위 뷰는 화면에 배치 되기 직전에 호출 됩니다.
- `ViewDidLayout` -이 메서드는 뷰의 하위 화면에 배치 된 후 바로 호출 됩니다.

<a name="The-Responder-Chain" />

### <a name="the-responder-chain"></a>응답자 체인

또한 `NSViewControllers` 창의의 일부인 이제 _응답자 체인_:

[![](indepth-images/vc01.png "응답자 체인")](indepth-images/vc01.png#lightbox)

되며 따라서 했으니를 받고 잘라내기, 복사 및 붙여넣기 메뉴 항목 선택 등의 이벤트에 응답 합니다. MacOS Sierra (10.12)에서 실행 되는 앱이 자동 뷰 컨트롤러 실시간 접속 에서만 발생 이상.

<a name="Containment" />

### <a name="containment"></a>Containment

스토리 보드의 보기 컨트롤러 (예: 분할 뷰 컨트롤러 및 탭 뷰 컨트롤러)를 구현할 수 있습니다 _포함_, "를 포함할 수 있습니다" 다른 하위 뷰 컨트롤러:

[![](indepth-images/vc02.png "뷰 컨트롤러 포함의 예")](indepth-images/vc02.png#lightbox)

메서드를 포함 하는 자식 뷰 컨트롤러 및 속성에 넣어 표시 하 고 뷰 화면에서 제거를 사용 하려면 해당 부모 뷰 컨트롤러를 다시 합니다.

MacOS에 기본 제공 되는 모든 컨테이너 보기 컨트롤러에 Apple 사용자 고유의 사용자 지정 컨테이너 뷰 컨트롤러를 만드는 경우에 따라 제안 되는 특정 레이아웃

[![](indepth-images/vc03.png "뷰 컨트롤러 레이아웃")](indepth-images/vc03.png#lightbox)

컬렉션 뷰 컨트롤러에는 자체 뷰가 포함 된 하나 이상의 뷰 컨트롤러를 포함 하는 각 컬렉션 뷰 항목의 배열을 포함 합니다.

<a name="Segues" />

## <a name="segues"></a>Segue

Segue 모든 앱의 UI를 정의 하는 백그라운드에서 간의 관계를 제공 합니다. IOS 스토리 보드에서 작업에 익숙한 경우 알고 Segue iOS에는 일반적으로 전체 화면 뷰 간의 전환 정의 대 한 합니다. 일반적으로 고 Segues를 정의 하는 경우이 반해 macOS에서 "[포함](#Containment)" 여기서 하나 장면은 장면 부모의 자식입니다.

MacOS의 경우 대부분의 앱 분할 뷰와 탭과 같은 UI 요소를 사용 하 여 동일한 창 내에서 해당 뷰를 그룹화 하는 경우가 많습니다. 여기서 뷰에 화면 밖으로 전환 해야, iOS와 달리 제한 된 물리적으로 인해 공간을 표시 합니다.

<a name="Presentation-Segues" />

### <a name="presentation-segues"></a>프레젠테이션 Segue

포함으로 macOS의 성향을 들어 경우가 위치 _프레젠테이션 Segue_ 모달 Windows, 시트 뷰 Popovers 등 사용 됩니다. macOS 제공 다음 기본 제공 segue 형식:

- **표시** -Segue의 대상 비 모달 창으로 표시 됩니다. 예를 들어 앱에서 문서 창의 다른 인스턴스를 제공 합니다.이 유형의 Segue 사용 합니다.
- **모달** -모달 창으로 Segue의 대상 표시 합니다. 예를 들어, 앱에 대 한 기본 설정 창을 표시 하려면이 유형의 Segue 사용 합니다.
- **시트** -Segue의 대상 부모 창에 연결 된 시트를 제시 합니다. 예를 들어 이러한 유형의 segue 찾기 및 바꾸기 시트를 제공 합니다.
- **팝 오버** -Segue의 대상 팝 오버 창 처럼 표시 합니다. 예를 들어 사용자가 UI 요소를 클릭할 때 옵션을 표시 하려면이 Segue 형식을 사용 합니다.
- **사용자 지정** -개발자가 정의한 사용자 지정 Segue 형식을 사용 하 여 Segue의 대상 표시 합니다. 참조 된 [만들기 사용자 지정 Segue](#Creating-Custom-Segues) 대 한 자세한 내용은 아래 섹션.

프레젠테이션 Segue를 사용 하는 경우 재정의할 수 있습니다는 `PrepareForSegue` 초기화 하는 프레젠테이션 및 변수에 대 한 부모 뷰 컨트롤러의 메서드에 제공 되는 보기 컨트롤러에 모든 데이터를 제공 합니다.

<a name="Triggered-Segues" />

### <a name="triggered-segues"></a>트리거 Segue

트리거된 고 Segues를 사용 하면 명명 된 고 Segues를 지정할 수 있도록 (통해 자신의 **식별자** Interface Builder에서 속성)를 호출 하거나 단추를 클릭 하 여 사용자와 같은 이벤트에 의해 트리거되는 `PerformSegue` 코드에서 메서드:

```csharp
// Display the Scene defined by the given Segue ID
PerformSegue("MyNamedSegue", this);
``` 

Segue ID는 앱의 UI를 레이아웃할 때 Xcode의 Interface Builder 내에서 정의 됩니다.

[![](indepth-images/sg02.png "입력 한 이름 Segue")](indepth-images/sg02.png#lightbox)

재정의 해야 Segue의 소스 역할을 하는 보기 컨트롤러에는 `PrepareForSegue` 메서드 초기화 Segue를 실행 하기 전에 필요한 수행 및 지정 된 뷰 컨트롤러 표시 됩니다.

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

재정의할 수 있습니다 합니다 `ShouldPerformSegue` 메서드 및 Segue를 통해 실제로 실행 되는 여부 제어 C# 코드입니다. 수동으로 표시 보기 컨트롤러에 대 한 호출의 `DismissController` 더 이상 필요 없는 경우 표시에서 제거 하는 방법입니다.

<a name="Creating-Custom-Segues" />

### <a name="creating-custom-segues"></a>Segue 사용자 지정 만들기

앱에 macOS에서 정의 되는 빌드-고 Segues에서 제공 하지 않는 Segue 형식 관리가 필요한 시기 번 있을 수 있습니다. 이 경우 만들 수 있습니다 할당할 수 있는 사용자 지정 Segue Xcode의 Interface Builder에서 앱의 UI 레이아웃 하는 경우.

예를 들어 (장면 대상 새 창에서 열기) 대신 창 내에서 현재 뷰 컨트롤러를 대체 하는 새 Segue 형식을 만들려면 다음 코드를 사용할 수 했습니다.

```csharp
using System;
using AppKit;
using Foundation;

namespace OnCardMac
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Swap the controllers
            source.View.Window.ContentViewController = destination;

            // Release memory
            source.RemoveFromParentViewController ();
        }
        #endregion

    }
        
}
```

몇 가지 사항이 여기:

- 사용 된 `Register` 주기-C/macOS에이 클래스를 노출 하는 특성입니다.
- 재정의 `Perform` 실제로 사용자 지정 Segue의 동작을 수행 하는 방법입니다.
- 창의 교체 하는 `ContentViewController` Segue의 대상 (대상)에서 정의 된 것을 사용 하 여 컨트롤러입니다.
- 원래 뷰 컨트롤러를 사용 하 여 메모리 확보를 제거 하는 것은 `RemoveFromParentViewController` 메서드.

이 새 Segue 형식에서 Xcode의 Interface Builder를 사용 하려면 먼저 앱을 컴파일 Xcode로 전환 하 고 새 두 장면 사이의 Segue를 추가 해야 합니다. 설정 합니다 **스타일** 에 **사용자 지정** 하며 **Segue 클래스** 에 `ReplaceViewSegue` (사용자 지정 Segue 클래스의 이름):

[![](indepth-images/sg01.png "Segue 클래스를 설정합니다.")](indepth-images/sg01.png#lightbox)

<a name="Triggered-Segues" />

## <a name="window-controllers"></a>창 컨트롤러

창 컨트롤러 포함 및 macOS 앱을 만들 수 있는 다른 창 형식을 제어 합니다. 스토리 보드에는 다음과 같은 기능이 있습니다.

1. 콘텐츠 뷰 컨트롤러를 제공 해야 합니다. 자식 창에는 동일한 콘텐츠 뷰 컨트롤러 됩니다.
2. 합니다 `Storyboard` 속성은 다른 창 컨트롤러에서 로드 된 스토리 보드를 포함 합니다. `null` 스토리 보드에서 로드 되지 않은 경우.
3. 호출할 수 있습니다는 `DismissController` 메서드를 지정 된 창을 닫고 보기에서 제거 합니다.

뷰 컨트롤러와 마찬가지로 창 컨트롤러 구현 합니다 `PerformSegue`, `PrepareForSegue` 및 `ShouldPerformSegue` 메서드 Segue 작업의 원본으로 사용할 수 있습니다.

창 컨트롤러 macOS 앱의 다음 기능을 담당 합니다.

- 특정 창을 관리합니다.
- (있는 경우)는 창의 제목 표시줄 및 도구 모음 관리 합니다.
- 창의 내용을 표시 하는 콘텐츠 뷰 컨트롤러를 관리 합니다.

<a name="Gesture-Recognizers" />

## <a name="gesture-recognizers"></a>제스처 인식기

MacOS에 대 한 제스처 인식기 iOS에 상응 하는 거의 동일 하 고 앱의 ui 요소를 사용 하면 개발자가 제스처 (예: 마우스 단추 클릭)를 쉽게 추가할 합니다.

그러나 여기서 제스처 iOS에 의해 결정 됩니다 (예: 두 손가락을 사용 하 여 화면 탭), 앱의 디자인 가장 macOS에서 제스처 하드웨어에 따라 결정 됩니다.

제스처 인식기를 사용 하 여 UI에서 항목에 사용자 지정 상호 작용을 추가 하는 데 필요한 코드의 양을 크게 줄일 수 있습니다. 단일 및 이중 클릭 사이의 자동으로 결정할 수를 클릭 하 고 이벤트 등을 끕니다.

재정의 하지 않고는 `MouseDown` 보기 컨트롤러의 이벤트를 사용 해야 제스처 인식기를 스토리 보드를 작업 하는 경우 사용자 입력된 이벤트를 처리할 수 있습니다.

다음 제스처 인식기 macOS에서 사용할 수 있습니다.

- `NSClickGestureRecognizer` -마우스 아래로 및 위로 이벤트를 등록 합니다.
- `NSPanGestureRecognizer` 등록 단추 아래로 끌어 마우스를 이벤트입니다.
- `NSPressGestureRecognizer` 마우스 단추를 누른 시간 이벤트의가 주어진된 시간에 대 한 등록 합니다.
- `NSMagnificationGestureRecognizer` -트랙 패드 하드웨어에서 확대 이벤트를 등록합니다.
- `NSRotationGestureRecognizer` -트랙 패드 하드웨어에서 회전 이벤트를 등록합니다.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>스토리 보드 참조 사용

스토리 보드 참조를 사용 하는 대규모의 복잡 한 스토리 보드 디자인을 수행 하 고 더 작은 스토리 보드에 원본에서 참조 가져오기 있으므로 복잡성을 제거 하 고 결과 개별 스토리 보드 디자인 및 유지 관리를 쉽게으로 나누는 수 있습니다.

또한 스토리 보드 참조를 제공할 수는 _앵커_ 동일한 스토리 보드 또는 다른 특정 장면 내에서 다른 장면으로 합니다.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>스토리 보드를 외부 참조

스토리 보드를 외부에 대 한 참조를 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**, 프로젝트 이름을 마우스 오른쪽 단추로 **추가** > **새 파일...**   >  **Mac** > **스토리 보드**합니다. 입력을 **이름을** 새 스토리 보드 및 클릭 합니다 **새로 만들기** 단추: 

    [![](indepth-images/ref01.png "새 스토리 보드를 추가합니다.")](indepth-images/ref01.png#lightbox)
2. 에 **솔루션 탐색기**, Xcode의 Interface Builder에서 편집을 위해 열려면 새 스토리 보드 이름을 두 번 클릭 합니다.
3. 일반적으로 하는 변경 내용을 저장 하는 대로 새 스토리 보드의 백그라운드에서의 레이아웃을 디자인 합니다. 

    [![](indepth-images/ref02.png "인터페이스 디자인")](indepth-images/ref02.png#lightbox)
4. 인터페이스 작성기에서에 대 한 참조를 추가 하 게 되는 스토리 보드로 전환 합니다.
5. 끌어서를 **참조를 스토리 보 딩** 에서 합니다 **라이브러리 개체** 디자인 화면으로: 

    [![](indepth-images/ref03.png "라이브러리에 대 한 스토리 보드 참조 선택")](indepth-images/ref03.png#lightbox)
6. 에 **특성 검사기**의 이름을 선택 합니다 **스토리 보드** 위에서 만든: 

    [![](indepth-images/ref04.png "참조를 구성합니다.")](indepth-images/ref04.png#lightbox)
7. 에 새 Segue를 만들고 제어는 기존 장면에서 UI 위젯 (예: 단추)를 클릭 합니다 **스토리 보드 참조** 방금 만든 합니다.  팝업 메뉴에서 선택 **표시** Segue를 완료 하려면: 

    [![](indepth-images/ref06.png "Segue 형식 설정")](indepth-images/ref06.png#lightbox) 
8. 스토리 보드에 변경 내용을 저장 합니다.
9. 변경 내용을 동기화 하려면 Mac 용 Visual Studio로 돌아갑니다.

앱이 실행 되 고 스토리 보드 참조에 지정 된 외부 스토리 보드에서 초기 창 컨트롤러에서 Segue를 생성 하는 사용자가 UI 요소에 표시 됩니다.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>스토리 보드를 외부에서 특정 장면의 참조

특정 장면에 대 한 참조를 추가 하려면 스토리 보드를 외부 (및 초기 창 컨트롤러) 다음을 수행 합니다.

1. 에 **솔루션 탐색기**를 열어 Xcode의 Interface Builder에서 편집 하는 외부 스토리 보드를 두 번 클릭 합니다.
2. 새 장면을 추가 하 고 평소와 같이 해당 레이아웃을 디자인 합니다. 

    [![](indepth-images/ref07.png "Xcode에서 레이아웃 디자인")](indepth-images/ref07.png#lightbox)
3. 에 **검사기**, 입력을 **스토리 보드 ID** 새 장면의 창 컨트롤러에 대 한: 

    [![](indepth-images/ref08.png "Storyboard ID를 설정합니다.")](indepth-images/ref08.png#lightbox)
4. Interface Builder에서에 대 한 참조를 추가 하 게 하려는 스토리 보드를 엽니다.
5. 끌어서를 **참조를 스토리 보 딩** 에서 합니다 **라이브러리 개체** 디자인 화면으로: 

    [![](indepth-images/ref03.png "스토리 보드 참조 라이브러리에서 선택")](indepth-images/ref03.png#lightbox)
6. 에 **검사기**의 이름을 선택 합니다 **스토리 보드** 및 **참조 ID** (스토리 보드 ID) 위에서 만든 장면의: 

    [![](indepth-images/ref09.png "참조 ID를 설정합니다.")](indepth-images/ref09.png#lightbox)
7. 에 새 Segue를 만들고 제어는 기존 장면에서 UI 위젯 (예: 단추)를 클릭 합니다 **스토리 보드 참조** 방금 만든 합니다. 팝업 메뉴에서 선택 **표시** Segue를 완료 하려면: 

    [![](indepth-images/ref06.png "Segue 형식 설정")](indepth-images/ref06.png#lightbox) 
8. 스토리 보드에 변경 내용을 저장 합니다.
9. 변경 내용을 동기화 하려면 Mac 용 Visual Studio로 돌아갑니다.

경우 앱 실행 및 사용자가 만든 Segue를 사용 하 여 장면에서 UI 요소에는 주어진 **스토리 보드 ID** 스토리 보드 참조에 지정 된 외부 스토리 보드에서 표시 됩니다.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>동일한 스토리 보드에서 특정 장면의 참조

특정 장면 동일한 스토리 보드에 대 한 참조를 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**를 열어 편집 하는 스토리 보드를 두 번 클릭 합니다.
2. 새 장면을 추가 하 고 평소와 같이 해당 레이아웃을 디자인 합니다. 

    [![](indepth-images/ref11.png "Xcode에서 스토리 보드를 편집합니다.")](indepth-images/ref11.png#lightbox)
3. 에 **검사기**, 입력을 **스토리 보드 ID** 새 장면의 창 컨트롤러에 대 한: 

    [![](indepth-images/ref12.png "Storyboard ID를 설정합니다.")](indepth-images/ref12.png#lightbox)
4. 끌어서를 **참조를 스토리 보 딩** 에서 합니다 **도구 상자** 디자인 화면으로: 

    [![](indepth-images/ref03.png "스토리 보드 참조 라이브러리에서 선택")](indepth-images/ref03.png#lightbox)
5. **특성 검사기**를 선택 **참조 ID** (스토리 보드 ID) 위에서 만든 장면의: 

    [![](indepth-images/ref13.png "참조 ID를 설정합니다.")](indepth-images/ref13.png#lightbox)
6. 에 새 Segue를 만들고 제어는 기존 장면에서 UI 위젯 (예: 단추)를 클릭 합니다 **스토리 보드 참조** 방금 만든 합니다. 팝업 메뉴에서 선택 **표시** Segue를 완료 하려면: 

    [![](indepth-images/ref06.png "Segue 형식 선택")](indepth-images/ref06.png#lightbox) 
7. 스토리 보드에 변경 내용을 저장 합니다.
8. 변경 내용을 동기화 하려면 Mac 용 Visual Studio로 돌아갑니다.

경우 앱 실행 및 사용자가 만든 Segue를 사용 하 여 장면에서 UI 요소에는 주어진 **스토리 보드 ID** 스토리 보드 참조에 지정 된 동일한 스토리 보드에 표시 됩니다.

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>복잡 한 스토리 보드 예제

Xamarin.Mac 앱에서 스토리 보드를 사용 하 여 작업의 복잡 한 예제를 참조 하십시오 합니다 [SourceWriter 샘플 앱](https://developer.xamarin.com/samples/mac/SourceWriter/)합니다. SourceWriter는 코드 완성 및 간단한 구문 강조 기능을 제공하는 간단한 소스 코드 편집기입니다.

SourceWriter 코드는 완벽하게 주석 처리되어 있으며, 가능한 경우 Xamarin.Mac 지침 설명서에 핵심 기술 또는 메서드부터 관련 정보까지 다양한 링크가 제공됩니다.

## <a name="related-links"></a>관련 링크

- [MacStoryboard (샘플)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows 작업](~/mac/user-interface/window.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
