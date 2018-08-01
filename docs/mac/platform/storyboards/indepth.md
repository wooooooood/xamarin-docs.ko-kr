---
title: Xamarin.Mac의 스토리 보드와 작업
description: 이 문서 코드, 보기 컨트롤러 수명 주기, 응답자 체인에서 로드 하는 방법을 검사 Xamarin.Mac의 스토리 보드를 사용 하는 방법을 설명, segues 창 컨트롤러, 제스처 인식기 등입니다.
ms.prod: xamarin
ms.assetid: DF4DF7C2-DDD7-4A32-B375-5C5446301EC5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 72986ed4247c3b6f66f6f1813d74bf0a95d0de53
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792842"
---
# <a name="working-with-storyboards-in-xamarinmac"></a>Xamarin.Mac의 스토리 보드와 작업

스토리 보드 모든 컨트롤러 해당 보기의 기능 개요로 분리 하는 지정된 된 앱에 대 한 UI를 정의 합니다. Xcode의 인터페이스 작성기에서 이러한 컨트롤러 각각 자체 장면에서 실행 됩니다.

[![](indepth-images/intro01.png "Xcode의 인터페이스 작성기에서 스토리 보드")](indepth-images/intro01.png#lightbox)

스토리 보드는 리소스 파일 (확장명을 가진 `.storyboard`) 컴파일 및 배송 때 Xamarin.Mac 앱의 번들에 포함을 가져옵니다. 앱에 대 한 시작 스토리 보드를 정의 하려면 편집의 `Info.plist` 파일을 선택 된 **주 인터페이스** 드롭다운 상자에서: 

[![](indepth-images/sb01.png "Info.plist 편집기")](indepth-images/sb01.png#lightbox)

<a name="Loading-from-Code" />

## <a name="loading-from-code"></a>코드에서 로드

번 중지 해야 코드에서 특정 스토리 보드를 로드 하 고 뷰 컨트롤러를 수동으로 만들 수 있습니다. 이 작업을 수행 하려면 다음 코드를 사용할 수 있습니다.

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);
```

`FromName` 앱의 번들에 포함 되어 있는 지정 된 이름의 스토리 보드 파일을 로드 합니다. `InstantiateControllerWithIdentifier` 지정한 id의 뷰 컨트롤러 인스턴스를 만듭니다. UI를 디자인할 때는 Xcode의 인터페이스 구성 관리자에서 Id를 설정 합니다.

[![](indepth-images/sb02.png "스토리 보드 ID 설정")](indepth-images/sb02.png#lightbox)

사용할 수 있습니다는 `InstantiateInitialController` 초기 컨트롤러 인터페이스 작성기에 할당 된 뷰 컨트롤러를 로드 하는 메서드:

[![](indepth-images/sb03.png "초기 컨트롤러 설정")](indepth-images/sb03.png#lightbox)

표시는 **스토리 보드 진입점** 와 위의 열기 종료 화살표입니다.

<a name="View-Controllers" />

## <a name="view-controllers"></a>컨트롤러 보기

컨트롤러 보기 Mac 응용 프로그램 내에서 정보가 지정된 된 보기와 해당 정보를 제공 하는 데이터 모델 간의 관계를 정의 합니다. 스토리 보드의 각 최상위 장면이 Xamarin.Mac 앱 코드에서 하나의 보기 컨트롤러를 나타냅니다.

<a name="The-View-Controller-Lifecycle" />

### <a name="the-view-controller-lifecycle"></a>보기 컨트롤러 수명 주기

에 추가 된 새 메서드가 여러 개는 `NSViewController` macOS에서 스토리 보드를 지원 하기 위해 클래스입니다. 지정 된 보기 컨트롤러에 의해 제어 되 고 보기의 수명 주기에 응답 하기 위해 사용한 다음 메서드는 가장 중요 한 점은:

- `ViewDidLoad` -이 메서드는 스토리 보드 파일에서 뷰를 로드할 때 호출 됩니다.
- `ViewWillAppear` -이 메서드는 보기 화면에 표시 되는 바로 전에 호출 됩니다.
- `ViewDidAppear` -이 메서드는 보기 화면에 표시 된 후 바로 호출 됩니다.
- `ViewWillDisappear` -이 메서드는 보기 화면에서 제거 되기 바로 전에 호출 됩니다.
- `ViewDidDisappear` -이 메서드는 보기 화면에서 제거 된 후 바로 호출 됩니다.
- `UpdateViewConstraints` -이 메서드는 뷰를 정의 하는 제약 조건은 자동으로 레이아웃 위치 및 크기가 업데이트 해야 할 때 호출 됩니다.
- `ViewWillLayout` -이 메서드는 화면에이 보기의 하위 뷰가 표시 되는 바로 전에 호출 됩니다.
- `ViewDidLayout` -이 메서드는 화면에 보기 보기의 하위 뷰가 표시 되는 후 바로 호출 됩니다.

<a name="The-Responder-Chain" />

### <a name="the-responder-chain"></a>응답자 체인

또한 `NSViewControllers` 창의의 일부인 이제 _응답자 체인_:

[![](indepth-images/vc01.png "응답자 체인")](indepth-images/vc01.png#lightbox)

있으며 따라서 유선 업 잘라내기, 복사 및 붙여넣기 메뉴 항목 선택 등과 같은 이벤트에 응답 합니다. 이 자동 뷰-컨트롤러 통신 기능을 macOS 시에라 (10.12)에서 실행 되는 앱 에서만 발생 이상에 있습니다.

<a name="Containment" />

### <a name="containment"></a>포함

스토리 보드의 보기 컨트롤러 (예: 분할 뷰 컨트롤러 및 탭 뷰 컨트롤러)를 구현할 수 있습니다 _제약_는 "포함" 다른 하위 컨트롤러 보기:

[![](indepth-images/vc02.png "보기 컨트롤러 포함의 예")](indepth-images/vc02.png#lightbox)

자식 뷰 컨트롤러 메서드를 포함 하 고 해당 부모 뷰-컨트롤러를 표시 하 고 뷰 화면에서 제거를 사용 하에 넣어 속성 백업 키를 누릅니다.

MacOS에 기본 제공 되는 모든 컨테이너 보기 컨트롤러 Apple 사용자 고유의 사용자 지정 컨테이너 보기 컨트롤러를 만드는 경우에 따라 제안 되는 특정 레이아웃에는

[![](indepth-images/vc03.png "뷰-컨트롤러 레이아웃")](indepth-images/vc03.png#lightbox)

컬렉션 뷰 컨트롤러 자신의 뷰를 포함 하는 하나 이상의 보기 컨트롤러를 포함 하며 각 컬렉션 보기 항목의 배열을 포함 합니다.

<a name="Segues" />

## <a name="segues"></a>Segues

Segues 간의 모든 응용 프로그램의 UI를 정의 하는 백그라운드에서 관계를 제공 합니다. IOS에서 스토리 보드에서 작업에 익숙한 것을 알고 있다면 Segues iOS에는 일반적으로 전체 화면 보기 간 전환을 정의 대 한 합니다. 이 점에서 차이가 macOS 등 Segues 일반적으로 정의 하는 경우 "[제약](#Containment)" 한 장면 장면 부모의 자식은 위치입니다.

MacOS 등의 대부분의 응용 프로그램의 뷰 같은 분할 뷰와 탭 UI 요소를 사용 하 여 동일한 창 내에 함께 그룹화 하는 경향이 있습니다. IOS, 뷰 및 화면 해제로 전환할 수 필요한 달리 제한 된 물리적으로 인해 공간을 표시 합니다.

<a name="Presentation-Segues" />

### <a name="presentation-segues"></a>프레젠테이션 Segues

경우 제약으로 macOS의 경향 들어 있는 _프레젠테이션 Segues_ 모달 창, 시트 뷰 Popovers 등 사용 됩니다. 다음과 같은 기본 제공 segue 제공 macOS 형식:

- **표시** -는 Segue 대상 비 모달 창으로 표시 됩니다. 예를 들어이 유형의 Segue 사용 하 여 응용 프로그램에서 문서 창의 다른 인스턴스를 표시 하도록 합니다.
- **모달** -모달 창으로 Segue의 대상 수를 표시 합니다. 예를 들어이 유형의 Segue 사용 하 여 앱에 대 한 기본 설정 창에 표시 합니다.
- **시트** -부모 창에 연결 된 시트는 Segue 대상 제시 합니다. 예를 들어, 이러한 종류의 찾기 및 바꾸기 시트에 게 표시할 segue 사용 합니다.
- **Popover** -popover 창 에서처럼는 Segue의 대상 수를 표시 합니다. 예를 들어 사용자가 UI 요소를 클릭할 때 옵션을 표시 하이 Segue 유형을 사용 합니다.
- **사용자 지정** -개발자가 정의한 사용자 지정 Segue 유형을 사용 하 여 Segue의 대상 수를 표시 합니다. 참조는 [만드는 사용자 지정 Segues](#Creating-Custom-Segues) 자세한 내용을 보려면 아래 섹션.

프레젠테이션 Segues를 사용 하는 경우 재정의할 수 있습니다는 `PrepareForSegue` 초기화 프레젠테이션 및 변수에 대 한 부모 뷰-컨트롤러의 메서드 및 제공 되는 보기 컨트롤러에 모든 데이터를 제공 합니다.

<a name="Triggered-Segues" />

### <a name="triggered-segues"></a>트리거된 Segues

트리거된 Segues 명명된 Segues를 지정할 수 있도록 메시지를 표시 합니다 (통해 자신의 **식별자** 인터페이스 작성기의 속성)를 호출 하거나 단추를 클릭 하면 사용자와 같은 이벤트에 의해 트리거되는 `PerformSegue` 코드에서 메서드:

```csharp
// Display the Scene defined by the given Segue ID
PerformSegue("MyNamedSegue", this);
``` 

응용 프로그램의 UI를 레이아웃할 때 Segue ID Xcode의 인터페이스 작성기 내에서 정의 됩니다.

[![](indepth-images/sg02.png "입력 한 이름 Segue")](indepth-images/sg02.png#lightbox)

재정의 해야는 Segue의 원본으로 사용 되는 보기 컨트롤러에는 `PrepareForSegue` 메서드 및 모든 초기화는 Segue 실행 하기 전에 필요한 do 및 지정된 된 보기 컨트롤러 표시 됩니다.

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

재정의할 수 있습니다는 `ShouldPerfromSegue` 메서드 및 C# 코드를 통해는 Segue 실제로 실행 되는 여부 제어 합니다. 수동으로 발표 된 컨트롤러 보기에 대 한 호출의 `DismissController` 메서드를 더 이상 필요 없는 경우 표시 되지 않도록에서 제거 합니다.

<a name="Creating-Custom-Segues" />

### <a name="creating-custom-segues"></a>Segues 사용자 지정 만들기

앱에서 macOS에 정의 된 빌드 Segues 제공 하지 않는 Segue 형식이 필요로 하는 경우 시간이 있을 수 있습니다. 이 경우 만들 수 있습니다 지정할 수 있는 사용자 지정 Segue Xcode의 인터페이스 작성기에서 응용 프로그램의 UI를 레이아웃 하는 경우.

예를 들어 대상 장면 새 창에서 열기) (대신 창 내 현재 보기 컨트롤러를 대체 하는 새 Segue 유형을 만들려면 다음 코드 수 사용 합니다.

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

몇 가지 여기에 기능:

- 사용 하는 `Register` 목표-C/macOS을이 클래스를 노출 하는 특성입니다.
- 재정의 하는 것은 `Perform` 메서드를 실제로 사용자 지정 Segue의 동작을 수행 합니다.
- 창의 교체 하는 `ContentViewController` 컨트롤러는 Segue의 대상 (대상)로 정의 합니다.
- 사용 하 여 메모리 확보 원래 뷰 컨트롤러 제거는 `RemoveFromParentViewController` 메서드.

이 새로운 Segue 형식은 Xcode의 인터페이스 작성기를 사용 하려면 먼저 앱을 컴파일하면 다음 Xcode로 전환 하 고, 두 장면 사이의 새 Segue 추가 하려면 필요 합니다. 설정의 **스타일** 를 **사용자 지정** 및 **Segue 클래스** 를 `ReplaceViewSegue` (사용자 지정 Segue 클래스의 이름):

[![](indepth-images/sg01.png "Segue 클래스를 설정합니다.")](indepth-images/sg01.png#lightbox)

<a name="Triggered-Segues" />

## <a name="window-controllers"></a>창 컨트롤러

창 컨트롤러를 포함 하 고 macOS 응용 프로그램에서 만들 수 있는 다른 창 형식을 제어 합니다. 스토리 보드에는 다음과 같은 기능이 있습니다.

1. 콘텐츠 뷰 컨트롤러를 제공 해야 합니다. 자식 창에 있는 동일한 콘텐츠 보기 컨트롤러가 될 수 있습니다.
2. `Storyboard` 속성 창 컨트롤러에서 그렇지 않으면 로드 된 스토리 보드를 포함 될 `null` 스토리 보드에서 로드 되지 않습니다.
3. 호출할 수 있습니다는 `DismissController` 메서드를 지정 된 창을 닫고 보기에서 제거 합니다.

컨트롤러 보기와 같은 창 컨트롤러 구현는 `PerformSegue`, `PrepareForSegue` 및 `ShouldPerfromSegue` 메서드 Segue 작업의 원본으로 사용할 수 있습니다.

창 컨트롤러 macOS 응용 프로그램의 다음 기능에 대 한 책임이:

- 특정 창을 관리 합니다.
- (있는 경우)는 창의 제목 표시줄 및 도구 모음 관리 합니다.
- 창의 내용을 표시 하는 콘텐츠 보기 컨트롤러 관리 합니다.

<a name="Gesture-Recognizers" />

## <a name="gesture-recognizers"></a>제스처 인식기

제스처 인식기 macOS에 대 한 iOS에 상응 하는 거의 동일 하며 응용 프로그램의 UI 요소에 사용 하면 개발자가 쉽게 추가할 수 (예: 마우스 단추 클릭) 제스처 합니다.

그러나 여기서 제스처 iOS에 의해 결정 됩니다 (예: 두 손가락으로 화면 누르기), 응용 프로그램의 디자인 가장 macOS에서 제스처 하드웨어에 의해 결정 됩니다.

제스처 인식기를 사용 하 여 UI의 항목에 사용자 지정 상호 작용을 추가 하는 데 필요한 코드의 양을 상당히 줄일 수 있습니다. 단일 및 이중 클릭 사이의 자동으로 결정할 수를 따라 클릭 하 고 이벤트, 등으로 끕니다.

재정의 하는 대신는 `MouseDown` 보기 컨트롤러에 이벤트를 사용 해야 제스처 인식기 스토리 보드와 작업 하는 경우 사용자 입력된 이벤트를 처리 합니다.

다음 제스처 인식기 macOS 사용할 수 있습니다.

- `NSClickGestureRecognizer` -마우스 아래로 이벤트를 등록 합니다.
- `NSPanGestureRecognizer` -레지스터 마우스 단추를 끌어다 이벤트를 해제 합니다.
- `NSPressGestureRecognizer` 마우스 단추를 누른 시간 이벤트의 주어진된 시간에 대 한 등록 합니다.
- `NSMagnificationGestureRecognizer` -트랙 패드 하드웨어에서 확대 이벤트를 등록합니다.
- `NSRotationGestureRecognizer` -트랙 패드 하드웨어에서 회전 이벤트를 등록합니다.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>스토리 보드 참조 사용

따라서 제거 복잡성을 제거 하 고 그 결과 개별 만드는 Storyboard 쉽게 디자인, 스토리 보드 참조를 사용 하면 대규모의 복잡 한 스토리 보드 디자인 하 고 원래에서 참조 가져오기는 더 작은 스토리 보드를 중단 하 고 유지 관리 합니다.

또한 스토리 보드 참조를 제공할 수는 _앵커_ 동일한 스토리 보드 또는 다른 특정 장면 내에서 다른 장면에 있습니다.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>외부 스토리 보드를 참조합니다.

외부 스토리 보드에 대 한 참조를 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...**   >  **Mac** > **스토리 보드**합니다. 입력 한 **이름** 새 스토리 보드 및 클릭에 대 한는 **새로** 단추: 

    [![](indepth-images/ref01.png "새 스토리 추가")](indepth-images/ref01.png#lightbox)
2. 에 **솔루션 탐색기**, Xcode의 인터페이스 작성기에서 편집 하기 위해 열려는 새 스토리 보드 이름을 두 번 클릭 합니다.
2. 일반적으로 하는 변경 내용을 저장 하는 대로 새 스토리 보드의 장면의 레이아웃을 디자인 합니다. 

    [![](indepth-images/ref02.png "인터페이스 디자인")](indepth-images/ref02.png#lightbox)
3. 스토리 보드 인터페이스 작성기에서에 대 한 참조를 추가 하는 것으로 전환 합니다.
4. 끌어서는 **참조 스토리 보드** 에서 **라이브러리 개체** 디자인 화면으로: 

    [![](indepth-images/ref03.png "라이브러리에 대 한 스토리 보드 참조를 선택 하면")](indepth-images/ref03.png#lightbox)
5. 에 **특성 검사기**의 이름을 선택 된 **스토리 보드** 위에서 만든: 

    [![](indepth-images/ref04.png "참조를 구성합니다.")](indepth-images/ref04.png#lightbox)
6. 기존 장면에 (예: 단추) UI 위젯에 컨트롤을 클릭 하 고에 새 Segue 만들기는 **스토리 보드 참조** 방금 만든 합니다.  팝업 메뉴에서 선택 **표시** 는 Segue를 완료 하려면: 

    [![](indepth-images/ref06.png "설정 Segue 유형")](indepth-images/ref06.png#lightbox) 
8. 스토리 보드에 변경 내용을 저장 합니다.
9. 변경 내용을 동기화 하는 Mac에 대 한 Visual Studio로 반환 합니다.

앱이 실행 되 고는 Segue 스토리 보드 파일에 대 한 참조에 지정 된 외부 스토리 보드에서 초기 창 컨트롤러에서 만든 UI 요소에는 사용자가 클릭할 때 표시 됩니다.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>외부 스토리 보드의 특정 장면이 참조

특정 장면에 대 한 참조를 추가 하는 외부 스토리 보드 (및 초기 창 컨트롤러) 다음을 수행 합니다.

1. 에 **솔루션 탐색기**, Xcode의 인터페이스 작성기에서 편집 하기 위해 열려는 외부 스토리 보드를 두 번 클릭 합니다.
2. 새 장면을 추가 하 고 일반적인 방법으로 해당 레이아웃을 디자인 합니다. 

    [![](indepth-images/ref07.png "Xcode에서 레이아웃 디자인")](indepth-images/ref07.png#lightbox)
3. 에 **Identity 관리자**를 입력 한 **스토리 보드 ID** 새 장면이 창 컨트롤러에 대 한: 

    [![](indepth-images/ref08.png "스토리 보드 ID 설정")](indepth-images/ref08.png#lightbox)
3. 인터페이스 작성기에서에 대 한 참조를 추가 하는 것 스토리 보드를 엽니다.
4. 끌어서는 **참조 스토리 보드** 에서 **라이브러리 개체** 디자인 화면으로: 

    [![](indepth-images/ref03.png "스토리 보드 참조 라이브러리에서 선택")](indepth-images/ref03.png#lightbox)
5. 에 **Identity 관리자**의 이름을 선택 된 **스토리 보드** 및 **참조 ID가** (스토리 보드 ID) 위에서 만든 장면의: 

    [![](indepth-images/ref09.png "참조 ID를 설정합니다.")](indepth-images/ref09.png#lightbox)
6. 기존 장면에 (예: 단추) UI 위젯에 컨트롤을 클릭 하 고에 새 Segue 만들기는 **스토리 보드 참조** 방금 만든 합니다. 팝업 메뉴에서 선택 **표시** 는 Segue를 완료 하려면: 

    [![](indepth-images/ref06.png "설정 Segue 유형")](indepth-images/ref06.png#lightbox) 
8. 스토리 보드에 변경 내용을 저장 합니다.
9. 변경 내용을 동기화 하는 Mac에 대 한 Visual Studio로 반환 합니다.

때 응용 프로그램 실행 및 사용자가는 Segue와 장면에서 만든 UI 요소에는 주어진 **스토리 보드 ID** 스토리 보드 파일에 대 한 참조에 지정 된 외부 스토리 보드에서 표시 됩니다.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>동일한 스토리 보드의 특정 장면이 참조

특정 장면 동일한 스토리 보드에 대 한 참조를 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**를 편집 하기 위해 열려는 스토리 보드를 두 번 클릭 합니다.
2. 새 장면을 추가 하 고 일반적인 방법으로 해당 레이아웃을 디자인 합니다. 

    [![](indepth-images/ref11.png "Xcode에서 스토리 보드를 편집합니다.")](indepth-images/ref11.png#lightbox)
3. 에 **Identity 관리자**를 입력 한 **스토리 보드 ID** 새 장면이 창 컨트롤러에 대 한: 

    [![](indepth-images/ref12.png "스토리 보드 ID 설정")](indepth-images/ref12.png#lightbox)
3. 끌어서는 **참조 스토리 보드** 에서 **도구 상자** 디자인 화면으로: 

    [![](indepth-images/ref03.png "스토리 보드 참조 라이브러리에서 선택")](indepth-images/ref03.png#lightbox)
5. **특성 검사기**선택, **참조 ID** (스토리 보드 ID) 위에서 만든 장면의: 

    [![](indepth-images/ref13.png "참조 ID를 설정합니다.")](indepth-images/ref13.png#lightbox)
6. 기존 장면에 (예: 단추) UI 위젯에 컨트롤을 클릭 하 고에 새 Segue 만들기는 **스토리 보드 참조** 방금 만든 합니다. 팝업 메뉴에서 선택 **표시** 는 Segue를 완료 하려면: 

    [![](indepth-images/ref06.png "Segue 유형 선택")](indepth-images/ref06.png#lightbox) 
8. 스토리 보드에 변경 내용을 저장 합니다.
9. 변경 내용을 동기화 하는 Mac에 대 한 Visual Studio로 반환 합니다.

때 응용 프로그램 실행 및 사용자가는 Segue와 장면에서 만든 UI 요소에는 주어진 **스토리 보드 ID** 스토리 보드 파일에 대 한 참조에 지정 된 동일한 스토리 보드에 표시 됩니다.

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>복잡 한 스토리 보드 예제

스토리 보드 Xamarin.Mac 응용 프로그램에서 작업의 복잡 한 예제를 참조 하십시오는 [SourceWriter 샘플 응용 프로그램](https://developer.xamarin.com/samples/mac/SourceWriter/)합니다. SourceWriter는 코드 완성 및 간단한 구문 강조 기능을 제공하는 간단한 소스 코드 편집기입니다.

SourceWriter 코드는 완벽하게 주석 처리되어 있으며, 가능한 경우 Xamarin.Mac 지침 설명서에 핵심 기술 또는 메서드부터 관련 정보까지 다양한 링크가 제공됩니다.

## <a name="related-links"></a>관련 링크

- [MacStoryboard (샘플)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [창 작업](~/mac/user-interface/window.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
