---
title: Xamarin.ios에서 Storyboard 사용
description: 이 문서에서는 Xamarin.ios에서 storyboard를 사용 하 여 코드에서 로드 하는 방법, 보기 컨트롤러 수명 주기, 응답자 체인, segue, 창 컨트롤러, 제스처 인식기 등을 검사 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: DF4DF7C2-DDD7-4A32-B375-5C5446301EC5
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 6aca181b2942bbde854df41c8f9741106cda6776
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70279307"
---
# <a name="working-with-storyboards-in-xamarinmac"></a>Xamarin.ios에서 Storyboard 사용

스토리 보드는 지정 된 앱에 대 한 모든 UI를 해당 뷰 컨트롤러의 기능 개요로 분할 하 여 정의 합니다. Xcode의 Interface Builder에서 이러한 각 컨트롤러는 자체 장면에 상주 합니다.

[![Xcode의 Interface Builder storyboard](indepth-images/intro01.png)](indepth-images/intro01.png#lightbox)

스토리 보드는 컴파일되어 제공 될 때 xamarin.ios 앱의 번들 `.storyboard`에 포함 되는 리소스 파일 (확장명 포함)입니다. 앱에 대 한 시작 Storyboard를 정의 하려면 해당 `Info.plist` 파일을 편집 하 고 드롭다운 상자에서 **주 인터페이스** 를 선택 합니다. 

[![Info.plist 편집기](indepth-images/sb01.png)](indepth-images/sb01.png#lightbox)

<a name="Loading-from-Code" />

## <a name="loading-from-code"></a>코드에서 로드

코드에서 특정 Storyboard를 로드 하 고 뷰 컨트롤러를 수동으로 만들어야 하는 경우가 있을 수 있습니다. 다음 코드를 사용 하 여이 작업을 수행할 수 있습니다.

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);
```

는 `FromName` 앱의 번들에 포함 된 지정 된 이름을 사용 하 여 스토리 보드 파일을 로드 합니다. 는 `InstantiateControllerWithIdentifier` 지정 된 id를 사용 하 여 뷰 컨트롤러의 인스턴스를 만듭니다. UI를 디자인할 때 Xcode의 Interface Builder에 Id를 설정 합니다.

[![스토리 보드 ID 설정](indepth-images/sb02.png)](indepth-images/sb02.png#lightbox)

필요에 따라 `InstantiateInitialController` 메서드를 사용 하 여 Interface Builder에서 초기 컨트롤러가 할당 된 뷰 컨트롤러를 로드할 수 있습니다.

[![초기 컨트롤러 설정](indepth-images/sb03.png)](indepth-images/sb03.png#lightbox)

**스토리 보드 진입점** 및 위의 열린 끝 화살표로 표시 됩니다.

<a name="View-Controllers" />

## <a name="view-controllers"></a>컨트롤러 보기

뷰 컨트롤러는 Mac 앱 내에서 지정 된 정보 보기와 해당 정보를 제공 하는 데이터 모델 간의 관계를 정의 합니다. 스토리 보드의 각 최상위 장면은 Xamarin.ios 앱의 코드에서 하나의 뷰 컨트롤러를 나타냅니다.

<a name="The-View-Controller-Lifecycle" />

### <a name="the-view-controller-lifecycle"></a>뷰 컨트롤러 수명 주기

Macos에서 storyboard를 지원 하기 위해 `NSViewController` 여러 가지 새로운 메서드가 클래스에 추가 되었습니다. 가장 중요 한 것은 다음 메서드가 지정 된 뷰 컨트롤러에서 제어 하는 뷰의 수명 주기에 응답 하는 데 사용 하는 것입니다.

- `ViewDidLoad`-이 메서드는 뷰가 Storyboard 파일에서 로드 될 때 호출 됩니다.
- `ViewWillAppear`-이 메서드는 뷰가 화면에 표시 되기 직전에 호출 됩니다.
- `ViewDidAppear`-이 메서드는 뷰가 화면에 표시 된 후 바로 호출 됩니다.
- `ViewWillDisappear`-이 메서드는 뷰가 화면에서 제거 되기 직전에 호출 됩니다.
- `ViewDidDisappear`-이 메서드는 뷰가 화면에서 제거 된 후 바로 호출 됩니다.
- `UpdateViewConstraints`-이 메서드는 뷰 자동 레이아웃 위치 및 크기를 정의 하는 제약 조건을 업데이트 해야 할 때 호출 됩니다.
- `ViewWillLayout`-이 메서드는이 뷰의 하위 뷰 화면에 배치 되기 직전에 호출 됩니다.
- `ViewDidLayout`-이 메서드는 하위 뷰 뷰가 화면에 배치 된 후 바로 호출 됩니다.

<a name="The-Responder-Chain" />

### <a name="the-responder-chain"></a>응답자 체인

또한 이제는 창의 응답자 체인의 일부입니다. `NSViewControllers`

[![응답자 체인](indepth-images/vc01.png)](indepth-images/vc01.png#lightbox)

그리고 잘라내기, 복사 및 붙여넣기 메뉴 항목 선택 등의 이벤트를 수신 하 고 응답 하기 위해 연결 되어 있습니다. 이 자동 보기 컨트롤러는 macOS Sierra (10.12) 이상에서 실행 되는 앱에 대해서만 발생 합니다.

<a name="Containment" />

### <a name="containment"></a>Containment

스토리 보드에서 뷰 컨트롤러 (예: 분할 뷰 컨트롤러 및 탭 뷰 컨트롤러)는 이제 다른 하위 뷰 컨트롤러를 "포함" 할 수 있도록 _포함_을 구현할 수 있습니다.

[![뷰 컨트롤러 포함의 예](indepth-images/vc02.png)](indepth-images/vc02.png#lightbox)

자식 뷰 컨트롤러에는 부모 뷰 컨트롤러에 다시 연결 하 고 화면에서 보기를 표시 하 고 제거 하는 데 사용할 수 있는 메서드와 속성이 포함 되어 있습니다.

MacOS에 기본 제공 되는 모든 컨테이너 뷰 컨트롤러에는 고유한 사용자 지정 컨테이너 뷰 컨트롤러를 만들 때 따라야 하는 Apple 제안에 해당 하는 레이아웃이 있습니다.

[![뷰 컨트롤러 레이아웃](indepth-images/vc03.png)](indepth-images/vc03.png#lightbox)

컬렉션 뷰 컨트롤러에는 각각 자체 뷰를 포함 하는 하나 이상의 뷰 컨트롤러를 포함 하는 컬렉션 뷰 항목의 배열이 포함 되어 있습니다.

<a name="Segues" />

## <a name="segues"></a>Segue

Segue는 앱의 UI를 정의 하는 모든 장면 간의 관계를 제공 합니다. IOS의 스토리 보드에서 작업 하는 데 익숙한 경우 Segue for iOS는 일반적으로 전체 화면 보기 간의 전환을 정의 한다는 것을 알고 있습니다. 이는 Segue가 일반적으로 "[포함](#Containment)"을 정의 하는 경우 macos와 다릅니다. 여기서 한 장면은 부모 장면의 자식입니다.

MacOS에서 대부분의 앱은 분할 뷰 및 탭과 같은 UI 요소를 사용 하 여 동일한 창 내에서 보기를 함께 그룹화 하는 경향이 있습니다. IOS와 달리 보기는 물리적 표시 공간이 제한 되어 있으므로 화면을 전환 하거나 화면에서 해제 해야 합니다.

<a name="Presentation-Segues" />

### <a name="presentation-segues"></a>프레젠테이션 Segue

포함에 대 한 macOS 성향을 지정 된 경우 모달 창, 시트 보기 및 Popovers와 같은 _프레젠테이션 segue_ 이 사용 되는 경우가 있습니다. macOS는 다음과 같은 기본 제공 segue 유형을 제공 합니다.

- **Show** -Segue의 대상을 모달이 아닌 창으로 표시 합니다. 예를 들어이 유형의 Segue을 사용 하 여 앱에 문서 창의 다른 인스턴스를 표시 합니다.
- **Modal** -Segue의 대상을 모달 창으로 표시 합니다. 예를 들어이 유형의 Segue을 사용 하 여 앱에 대 한 기본 설정 창을 표시 합니다.
- **Sheet** -Segue의 대상을 부모 창에 연결 된 시트로 표시 합니다. 예를 들어이 유형의 segue을 사용 하 여 찾기 및 바꾸기 시트를 표시 합니다.
- **팝 오버** -Segue의 대상을 팝 오버 창에 표시 합니다. 예를 들어 사용자가 UI 요소를 클릭 하는 경우이 Segue 유형을 사용 하 여 옵션을 표시 합니다.
- **사용자 지정** -개발자가 정의한 사용자 지정 Segue 형식을 사용 하 여 Segue의 대상을 표시 합니다. 자세한 내용은 아래의 [사용자 지정 Segue 만들기](#Creating-Custom-Segues) 섹션을 참조 하세요.

Presentation segue를 사용 하는 `PrepareForSegue` 경우 프레젠테이션 용 부모 뷰 컨트롤러의 메서드를 재정의 하 여 및 변수를 초기화 하 고 표시 되는 뷰 컨트롤러에 데이터를 제공할 수 있습니다.

<a name="Triggered-Segues" />

### <a name="triggered-segues"></a>트리거된 Segue

트리거된 segue를 사용 하 여 명명 된 segue (Interface Builder의 **식별자** 속성을 통해)를 지정 하 고 사용자가 단추를 클릭 하거나 코드에서 메서드를 `PerformSegue` 호출 하는 등의 이벤트에 의해 트리거될 수 있습니다.

```csharp
// Display the Scene defined by the given Segue ID
PerformSegue("MyNamedSegue", this);
``` 

Segue ID는 앱 UI의 레이아웃을 지정 하는 경우 Xcode의 Interface Builder 내부에 정의 됩니다.

[![Segue 이름 입력](indepth-images/sg02.png)](indepth-images/sg02.png#lightbox)

Segue의 소스로 작동 하는 뷰 컨트롤러에서 `PrepareForSegue` 메서드를 재정의 하 고, Segue를 실행 하 고 지정 된 뷰 컨트롤러를 표시 하기 전에 초기화를 수행 해야 합니다.

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

필요에 따라 `ShouldPerformSegue` 메서드를 재정의 하 고 Segue가 코드를 통해 C# 실제로 실행 되는지 여부를 제어할 수 있습니다. 수동으로 표시 된 뷰 컨트롤러의 경우 `DismissController` 메서드를 호출 하 여 더 이상 필요 하지 않은 경우 표시에서 제거 합니다.

<a name="Creating-Custom-Segues" />

### <a name="creating-custom-segues"></a>사용자 지정 Segue 만들기

앱에 macOS에 정의 된 Segue가 제공 하지 않는 Segue 형식이 필요한 경우가 있습니다. 이 경우 응용 프로그램의 UI를 레이아웃할 때 Xcode의 Interface Builder에 할당 될 수 있는 사용자 지정 Segue를 만들 수 있습니다.

예를 들어 새 창에서 대상 장면을 열지 않고 창 내에서 현재 뷰 컨트롤러를 대체 하는 새 Segue 형식을 만들려면 다음 코드를 사용할 수 있습니다.

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

다음 몇 가지 사항을 참고 하세요.

- `Register` 특성을 사용 하 여이 클래스를 목표-C/macos에 노출 합니다.
- 메서드를 `Perform` 재정의 하 여 사용자 지정 Segue의 작업을 실제로 수행 합니다.
- 창의 `ContentViewController` 컨트롤러를 Segue 대상 (대상)에 의해 정의 된 것으로 바꿉니다.
- 메서드를 `RemoveFromParentViewController` 사용 하 여 메모리를 확보 하기 위해 원래 뷰 컨트롤러를 제거 합니다.

Xcode의 Interface Builder에서이 새 Segue 형식을 사용 하려면 먼저 앱을 컴파일한 다음 Xcode로 전환 하 고 두 장면 사이에 새 Segue를 추가 해야 합니다. **스타일** 을 **custom** 으로 설정 하 고 **Segue 클래스** 를 `ReplaceViewSegue` (사용자 지정 Segue 클래스의 이름)로 설정 합니다.

[![Segue 클래스 설정](indepth-images/sg01.png)](indepth-images/sg01.png#lightbox)

<a name="Triggered-Segues" />

## <a name="window-controllers"></a>창 컨트롤러

Window 컨트롤러는 macOS 앱에서 만들 수 있는 다양 한 창 유형을 포함 하 고 제어 합니다. 스토리 보드의 경우 다음과 같은 기능이 있습니다.

1. 콘텐츠 뷰 컨트롤러를 제공 해야 합니다. 이는 자식 창에 있는 것과 동일한 콘텐츠 뷰 컨트롤러입니다.
2. 속성 `Storyboard` 에는 창 컨트롤러를 로드 한 storyboard가 포함 되 고, 그렇지 `null` 않으면 스토리 보드에서 로드 되지 않습니다.
3. 메서드를 `DismissController` 호출 하 여 지정 된 창을 닫고 뷰에서 제거할 수 있습니다.

뷰 컨트롤러와 마찬가지로 창 컨트롤러는 `PerformSegue`을 `PrepareForSegue` 구현 하 고 `ShouldPerformSegue` 및 메서드를 Segue 작업의 소스로 사용할 수 있습니다.

Window 컨트롤러는 macOS 앱의 다음 기능을 담당 합니다.

- 특정 창을 관리 합니다.
- 창의 제목 표시줄과 도구 모음을 관리 합니다 (사용 가능한 경우).
- 콘텐츠 뷰 컨트롤러를 관리 하 여 창의 내용을 표시 합니다.

<a name="Gesture-Recognizers" />

## <a name="gesture-recognizers"></a>제스처 인식기

MacOS에 대 한 제스처 인식기는 iOS의 해당 항목과 거의 같으며 개발자가 앱 UI의 요소에 제스처 (예: 마우스 단추 클릭)를 쉽게 추가할 수 있습니다.

그러나 iOS의 제스처가 앱의 디자인에 의해 결정 되는 경우 (예: 두 손가락으로 화면 누르기) macOS에서 대부분의 제스처는 하드웨어에 의해 결정 됩니다.

제스처 인식기를 사용 하면 UI에서 항목에 사용자 지정 상호 작용을 추가 하는 데 필요한 코드의 양을 크게 줄일 수 있습니다. 두 번 클릭과 단일 클릭 사이에 자동으로 결정 될 수 있으므로 이벤트 등을 클릭 하 고 끕니다.

뷰 컨트롤러에서 이벤트 `MouseDown` 를 재정의 하는 대신, storyboard를 사용할 때 사용자 입력 이벤트를 처리 하기 위해 제스처 인식기를 사용 해야 합니다.

MacOS에서 사용할 수 있는 제스처 인식기는 다음과 같습니다.

- `NSClickGestureRecognizer`-마우스 아래로 이벤트를 등록 합니다.
- `NSPanGestureRecognizer`-마우스 단추를 아래로 등록 하 고 이벤트를 끌어서 놓습니다.
- `NSPressGestureRecognizer`-지정 된 시간 이벤트에 대해 마우스 단추를 누른 상태로 등록 합니다.
- `NSMagnificationGestureRecognizer`-트랙 패드 하드웨어에서 확대 이벤트를 등록 합니다.
- `NSRotationGestureRecognizer`-트랙 패드 하드웨어에서 회전 이벤트를 등록 합니다.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>스토리 보드 참조 사용

스토리 보드 참조를 사용 하면 크고 복잡 한 Storyboard 디자인을 수행 하 여 원본에서 참조 하는 작은 Storyboard로 나눌 수 있으므로 복잡성을 제거 하 고 결과 개별 스토리 보드를 디자인 및 유지 관리 하기 쉽게 만들 수 있습니다.

또한 Storyboard 참조는 동일한 Storyboard 내의 다른 장면에 _앵커_ 를 제공 하거나 다른 장면에 특정 장면을 제공할 수 있습니다.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>외부 Storyboard 참조

외부 스토리 보드에 대 한 참조를 추가 하려면 다음을 수행 합니다.

1. **솔루션 탐색기**에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다.  > MacStoryboard > . 새 스토리 보드의 **이름을** 입력 하 고 **새로 만들기** 단추를 클릭 합니다. 

    [![새 스토리 보드 추가](indepth-images/ref01.png)](indepth-images/ref01.png#lightbox)
2. **솔루션 탐색기**에서 새 스토리 보드 이름을 두 번 클릭 하 여 Xcode의 Interface Builder에서 편집할 수 있도록 엽니다.
3. 일반적인 방법으로 새 Storyboard의 배경 레이아웃을 디자인 하 고 변경 내용을 저장 합니다. 

    [![인터페이스 디자인](indepth-images/ref02.png)](indepth-images/ref02.png#lightbox)
4. Interface Builder에 대 한 참조를 추가할 Storyboard로 전환 합니다.
5. **개체 라이브러리** 의 **스토리 보드 참조** 를 Design Surface 끌어 옵니다. 

    [![라이브러리에서 스토리 보드 참조 선택](indepth-images/ref03.png)](indepth-images/ref03.png#lightbox)
6. **특성 검사자**에서 위에서 만든 **Storyboard** 의 이름을 선택 합니다. 

    [![참조 구성](indepth-images/ref04.png)](indepth-images/ref04.png#lightbox)
7. 기존 장면에서 UI 위젯 (예: 단추)을 클릭 하 고 방금 만든 **스토리 보드 참조** 에 새 Segue을 만듭니다.  팝업 메뉴에서 **표시** 를 선택 하 여 Segue를 완료 합니다. 

    [![Segue 형식 설정](indepth-images/ref06.png)](indepth-images/ref06.png#lightbox) 
8. 스토리 보드에 변경 내용을 저장 합니다.
9. Mac용 Visual Studio로 돌아와서 변경 내용을 동기화 합니다.

앱이 실행 되 고 사용자가 Segue에서 만든 UI 요소를 클릭 하면 Storyboard 참조에 지정 된 외부 Storyboard의 초기 창 컨트롤러가 표시 됩니다.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>외부 Storyboard의 특정 장면 참조

초기 창 컨트롤러가 아닌 외부 Storyboard를 특정 장면에 추가 하려면 다음을 수행 합니다.

1. **솔루션 탐색기**에서 외부 스토리 보드를 두 번 클릭 하 여 Xcode의 Interface Builder에서 편집할 수 있도록 엽니다.
2. 새 장면을 추가 하 고 일반적으로 다음과 같이 레이아웃을 디자인 합니다. 

    [![Xcode에서 레이아웃 디자인](indepth-images/ref07.png)](indepth-images/ref07.png#lightbox)
3. **Id 검사자**에서 새 장면의 창 컨트롤러에 대 한 **스토리 보드 ID** 를 입력 합니다. 

    [![스토리 보드 ID 설정](indepth-images/ref08.png)](indepth-images/ref08.png#lightbox)
4. Interface Builder에 대 한 참조를 추가할 스토리 보드를 엽니다.
5. **개체 라이브러리** 의 **스토리 보드 참조** 를 Design Surface 끌어 옵니다. 

    [![라이브러리에서 스토리 보드 참조 선택](indepth-images/ref03.png)](indepth-images/ref03.png#lightbox)
6. **Identity Inspector**에서 위에서 만든 장면의 **스토리 보드** 이름 및 **참조 ID** (storyboard id)를 선택 합니다. 

    [![참조 ID 설정](indepth-images/ref09.png)](indepth-images/ref09.png#lightbox)
7. 기존 장면에서 UI 위젯 (예: 단추)을 클릭 하 고 방금 만든 **스토리 보드 참조** 에 새 Segue을 만듭니다. 팝업 메뉴에서 **표시** 를 선택 하 여 Segue를 완료 합니다. 

    [![Segue 형식 설정](indepth-images/ref06.png)](indepth-images/ref06.png#lightbox) 
8. 스토리 보드에 변경 내용을 저장 합니다.
9. Mac용 Visual Studio로 돌아와서 변경 내용을 동기화 합니다.

앱이 실행 되 고 사용자가 Segue에서 만든 UI 요소를 클릭 하면 스토리 보드 참조에 지정 된 외부 Storyboard에서 지정 된 **STORYBOARD ID** 가 있는 장면이 표시 됩니다.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>동일한 Storyboard에서 특정 장면 참조

동일한 Storyboard에 특정 장면에 대 한 참조를 추가 하려면 다음을 수행 합니다.

1. **솔루션 탐색기**에서 스토리 보드를 두 번 클릭 하 여 편집용으로 엽니다.
2. 새 장면을 추가 하 고 일반적으로 다음과 같이 레이아웃을 디자인 합니다. 

    [![Xcode에서 storyboard 편집](indepth-images/ref11.png)](indepth-images/ref11.png#lightbox)
3. **Id 검사자**에서 새 장면의 창 컨트롤러에 대 한 **스토리 보드 ID** 를 입력 합니다. 

    [![스토리 보드 ID 설정](indepth-images/ref12.png)](indepth-images/ref12.png#lightbox)
4. **도구 상자** 의 **스토리 보드 참조** 를 Design Surface 끌어 옵니다. 

    [![라이브러리에서 스토리 보드 참조 선택](indepth-images/ref03.png)](indepth-images/ref03.png#lightbox)
5. **특성 검사자**에서 위에서 만든 장면의 **참조 ID** (Storyboard id)를 선택 합니다. 

    [![참조 ID 설정](indepth-images/ref13.png)](indepth-images/ref13.png#lightbox)
6. 기존 장면에서 UI 위젯 (예: 단추)을 클릭 하 고 방금 만든 **스토리 보드 참조** 에 새 Segue을 만듭니다. 팝업 메뉴에서 **표시** 를 선택 하 여 Segue를 완료 합니다. 

    [![Segue 유형 선택](indepth-images/ref06.png)](indepth-images/ref06.png#lightbox) 
7. 스토리 보드에 변경 내용을 저장 합니다.
8. Mac용 Visual Studio로 돌아와서 변경 내용을 동기화 합니다.

앱이 실행 되 고 사용자가 Segue를 만든 UI 요소를 클릭 하면 스토리 보드 참조에 지정 된 동일한 Storyboard에 지정 된 **STORYBOARD ID** 가 있는 장면이 표시 됩니다.

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>복잡 한 Storyboard 예

Xamarin.ios 앱에서 Storyboard를 사용 하는 복잡 한 예는 [Sourcewriter 샘플 앱](https://docs.microsoft.com/samples/xamarin/mac-samples/sourcewriter)을 참조 하세요. SourceWriter는 코드 완성 및 간단한 구문 강조 기능을 제공하는 간단한 소스 코드 편집기입니다.

SourceWriter 코드는 완벽하게 주석 처리되어 있으며, 가능한 경우 Xamarin.Mac 지침 설명서에 핵심 기술 또는 메서드부터 관련 정보까지 다양한 링크가 제공됩니다.

## <a name="related-links"></a>관련 링크

- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows 작업](~/mac/user-interface/window.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
