---
title: Xamarin에서 tvOS 용 siri 원격 및 Bluetooth 컨트롤러
description: 이 문서에서는 Xamarin으로 작성 된 tvOS apps에서 Siri 원격 및 Bluetooth 게임 컨트롤러를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: BDB9894A-236B-424B-9032-ACD12A6C5720
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: c0338fce694d61dc19484c56dbc00bb854d0d0d7
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915778"
---
# <a name="siri-remote-and-bluetooth-controllers-for-tvos-in-xamarin"></a>Xamarin에서 tvOS 용 siri 원격 및 Bluetooth 컨트롤러

TvOS 앱의 사용자는 iOS를 사용 하 여 장치 화면에서 이미지를 탭 하지만 [Siri 원격](#The-Siri-Remote)을 사용 하 여 대화방에서 간접적으로 상호 작용 하지 않습니다.

앱이 게임 인 경우에는 앱에서 iOS (MFI) [Bluetooth 게임 컨트롤러](#Bluetooth-Game-Controllers) 에 대해 만든 타사에 대 한 지원을 선택적으로 빌드할 수 있습니다.

[![](remote-bluetooth-images/intro01.png "The Bluetooth Remote and Game Controller")](remote-bluetooth-images/intro01.png#lightbox)

이 문서에서는 [Siri 원격](#The-Siri-Remote), [터치 서피스 제스처](#Touch-Surface-Gestures) 및 [siri 원격 단추](#Siri-Remote-Buttons) 에 대해 설명 하 고 [제스처와 스토리 보드](#Gestures-and-Storyboards), [제스처, 코드](#Gestures-and-Code) 및 [낮은 수준의 이벤트 처리](#Low-Level-Event-Handling)를 통해 작업 하는 방법을 보여 줍니다. 마지막으로 tvOS 앱에서 [게임 컨트롤러를 사용](#Working-with-Game-Controllers) 하는 방법을 설명 합니다.

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Siri 원격

사용자가 Apple TV 및 tvOS 앱과 상호 작용 하는 주요 방법은 포함 된 Siri 원격을 통하는 것입니다. Apple은 소파에 앉아 있는 사용자와 TV 화면에서 대화방에 표시 되는 Apple TV 사용자 인터페이스 간의 거리를 연결 하도록 원격을 설계 했습니다.

TvOS 앱 개발자의 과제는 Siri 원격의 터치 surface,가 속도계, 자이로스코프가 및 단추를 활용 하는 빠르고 간편 하 고 시각적으로 매력적인 사용자 인터페이스를 만드는 것입니다.

[![](remote-bluetooth-images/remote01.png "The Siri Remote")](remote-bluetooth-images/remote01.png#lightbox)

Siri 원격은 tvOS 앱 내에서 다음과 같은 기능 및 예상 된 사용을 포함 합니다.

|기능|일반 앱 사용|게임 앱 사용|
|---|---|---|
|**터치 표면**<br />탐색을 클릭 하 여 선택 하 고 선택 하 여 상황에 맞는 메뉴를 선택 합니다.|**탭 하거나 살짝 밀기**<br />포커스를 받을 항목 간의 UI 탐색<br /><br />**클릭 대상**<br />선택한 (포커스 내) 항목을 활성화 합니다.|**탭 하거나 살짝 밀기**<br />게임 디자인에 따라 달라 지 며 가장자리를 눌러 D-패드로 사용할 수 있습니다.<br /><br />**클릭 대상**<br />기본 단추 함수를 수행 합니다.|
|**메뉴**<br />키를 눌러 이전 화면 또는 메뉴로 돌아갑니다.|이전 화면으로 돌아갑니다. 주 앱 화면에서 Apple TV 홈 화면을 종료 합니다.|게임을 일시 중지 했다가 다시 시작 하 고 이전 화면으로 돌아갑니다. 주 앱 화면에서 Apple TV 홈 화면으로 돌아갑니다.|
|**Siri/검색**<br />Siri를 사용 하는 국가에서 음성 제어를 길게 누르고 다른 모든 국가에서는 검색 화면을 표시 합니다.|해당 없음|해당 없음|
|**재생/일시 중지**<br />미디어를 재생 및 일시 중지 하거나 앱에서 보조 기능을 제공 합니다.|미디어 재생을 시작 하 고 재생을 일시 중지/다시 시작 합니다.|보조 단추 함수를 수행 하거나 소개 비디오 (있는 경우)를 건너뜁니다.|
|**홈**<br />키를 눌러 홈 화면으로 돌아간 다음, 두 번 클릭 하 여 실행 중인 앱을 표시 하 고,를 두 번 클릭 하 여 장치를 대기 합니다.|해당 없음|해당 없음|
|**볼륨**<br />연결 된 오디오/비디오 장비 볼륨을 제어 합니다.|해당 없음|해당 없음|

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>터치 표면 제스처

Siri 원격의 터치 표면은 tvOS 앱에서 응답할 수 있는 여러 단일 손가락 제스처를 검색할 수 있습니다.

|살짝 밀기|그런 다음|탭|
|---|---|---|
|![](remote-bluetooth-images/Gesture01.png)|![](remote-bluetooth-images/Gesture02.png)|![](remote-bluetooth-images/Gesture03.png)|
|화면에 있는 UI 요소 (위쪽, 왼쪽, 오른쪽) 사이에서 선택 (포커스)을 이동 합니다. 살짝 밀기는 관성을 사용 하 여 많은 콘텐츠 목록을 빠르게 스크롤 하는 데 사용할 수 있습니다.|선택한 (포커스 내) 항목을 활성화 하거나 게임의 기본 단추 처럼 동작 합니다. 클릭 및 유지는 상황에 맞는 메뉴 또는 보조 함수를 활성화할 수 있습니다.|가장자리에 대 한 터치 표면을 가볍게 누르면 D-패드의 방향 단추 처럼 동작 하 고 탭 영역에 따라 포커스를 위쪽, 아래쪽, 왼쪽 또는 오른쪽으로 이동 합니다. 앱에 따라를 사용 하 여 숨겨진 컨트롤을 표시할 수 있습니다.|

Apple은 터치 Surface 제스처를 사용할 때 다음과 같은 제안 사항을 제공 합니다.

- 클릭 **과 탭 간 구분** 은 사용자의 의도적인 작업 이며 선택, 활성화 및 게임의 기본 단추에 대해 적합 합니다. 사용자가 직접 Siri 원격을 보유 하 고 있으며 실수로 탭 이벤트를 쉽게 활성화할 수 있으므로 탭은 좀 더 미묘한 데 사용 해야 합니다.
- **표준 제스처를 다시 정의 하지 않음** -특정 제스처가 특정 작업을 수행 하는 것으로 예상 되는 경우 앱에서 이러한 제스처의 의미 또는 기능을 다시 정의 하지 않아야 합니다. 한 가지 예외는 활성 게임 중에 게임 앱입니다.
- **새 제스처를 드물게 정의** 합니다. 사용자는 특정 제스처가 특정 작업을 수행 하는 것으로 예상할 수 있습니다. 표준 동작을 수행 하는 사용자 지정 제스처를 정의 하지 않아야 합니다. 이번에도 게임은 사용자 지정 제스처가 게임에 재미 있고 몰입 형 게임을 추가할 수 있는 가장 일반적인 예외입니다.
- **해당 하는 경우에는 d-Pad 탭에 응답** 합니다. 터치 표면의 모퉁이 가장자리에 있는 가볍게 누르기는 게임 컨트롤러에서 포커스를 이동 하거나 위쪽, 아래쪽, 왼쪽 또는 오른쪽으로 d-pad와 같이 반응 합니다. 적절 한 경우 앱 또는 게임에서 이러한 제스처에 응답 해야 합니다.

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Siri 원격 단추

터치 표면의 제스처 외에도 앱은 터치 표면을 클릭 하거나 재생/일시 중지 단추를 눌러 사용자에 게 응답할 수 있습니다. 게임 컨트롤러 프레임 워크를 사용 하 여 Siri 원격에 액세스 하는 경우 누른 메뉴 단추를 검색할 수도 있습니다.

또한 표준 `UIKit` 요소를 포함 하는 제스처 인식기를 사용 하 여 메뉴 단추 누름을 감지할 수 있습니다. 누른 메뉴 단추를 가로채 면 현재 뷰 및 뷰 컨트롤러를 닫고 이전으로 돌아갈 책임이 있습니다.

> [!IMPORTANT]
> **항상** 원격의 재생/일시 중지 단추에 함수를 할당 해야 합니다. 작동 하지 않는 단추가 있으면 최종 사용자에 게 앱이 분리 될 수 있습니다. 이 단추에 대 한 올바른 함수가 없는 경우 기본 단추와 동일한 기능을 할당 합니다 (터치 Surface 클릭).

<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>제스처 및 Storyboard

TvOS 앱에서 Siri 원격으로 작업 하는 가장 쉬운 방법은 인터페이스 디자이너에서 보기에 제스처 인식기를 추가 하는 것입니다.

제스처 인식기를 추가 하려면 다음을 수행 합니다.

1. **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭 하 고이 파일을 열어 인터페이스 디자이너를 편집 합니다.
2. **라이브러리** 에서 **탭 제스처 인식기** 를 끌어서 뷰에 놓습니다.

    [![](remote-bluetooth-images/storyboard01.png "A Tap Gesture Recognizer")](remote-bluetooth-images/storyboard01.png#lightbox)
3. **특성 검사자**의 **단추** 섹션에서 **선택을 선택** 합니다.

    [![](remote-bluetooth-images/storyboard02.png "Check Select")](remote-bluetooth-images/storyboard02.png#lightbox)
4. **선택** 은 사용자가 Siri 원격에서 **Touch Surface** 를 클릭 하 여 사용자에 게 응답 하는 동작을 의미 합니다. **메뉴**, **재생/일시 중지**, **위쪽**, **아래쪽**, **왼쪽** 및 **오른쪽** 단추에 응답 하는 옵션도 있습니다.
5. 그런 다음 **탭 제스처 인식기** 에서 **작업** 을 연결 하 고 `TouchSurfaceClicked`호출 합니다.

    [![](remote-bluetooth-images/storyboard03.png "An Action from the Tap Gesture Recognizer")](remote-bluetooth-images/storyboard03.png#lightbox)
6. 변경 내용을 저장 하 고 Mac용 Visual Studio로 돌아갑니다.

뷰 컨트롤러 (예: `FirstViewController.cs`) 파일을 편집 하 고 트리거되는 제스처를 처리 하는 다음 코드를 추가 합니다.

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class FirstViewController : UIViewController
    {
        ...

        #region Custom Actions
        partial void TouchSurfaceClicked (Foundation.NSObject sender) {
            // Handle click here
            ...
        }
        #endregion
    }
}
```

스토리 보드 사용에 대 한 자세한 내용은 [Hello, tvOS 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)를 참조 하세요. 특히 [사용자 인터페이스를 만들고](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface) , [출 선 및 작업 섹션을 사용 하 여 코드를 작성](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) 합니다.

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>제스처 및 코드

필요에 따라 코드에서 C# 제스처를 직접 만들고 사용자 인터페이스의 보기에 추가할 수 있습니다. 예를 들어 일련의 살짝 밀기 제스처 인식자를 추가 하려면 보기 컨트롤러를 편집 하 고 다음 코드를 추가 합니다.

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class SecondViewController : UIViewController
    {
        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Wire-up gestures
            var upGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Up";
                ButtonLabel.Text = "Swiped Up";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Up
            };
            this.View.AddGestureRecognizer (upGesture);

            var downGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Down";
                ButtonLabel.Text = "Swiped Down";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Down
            };
            this.View.AddGestureRecognizer (downGesture);

            var leftGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Left";
                ButtonLabel.Text = "Swiped Left";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Left
            };
            this.View.AddGestureRecognizer (leftGesture);

            var rightGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Right";
                ButtonLabel.Text = "Swiped Right";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Right
            };
            this.View.AddGestureRecognizer (rightGesture);
        }
        #endregion
    }
}
```

<a name="Low-Level-Event-Handling" />

## <a name="low-level-event-handling"></a>하위 수준 이벤트 처리

TvOS 앱의 `UIKit`을 기반으로 사용자 지정 형식 (예: `UIView`)을 만드는 경우 `UIPress` 이벤트를 통해 단추 누름의 하위 수준 처리 기능을 제공할 수도 있습니다.

`UIPress` 이벤트는 Siri 원격 또는 기타 연결 된 Bluetooth 장치 (예: 게임 컨트롤러)에서 단추를 누르는 것에 대 한 정보를 반환 하는 경우를 `UIPress` 제외 하 고는 `UITouch` 이벤트를 iOS에 tvOS 하는 것입니다. `UIPress` 이벤트는 누른 단추와 해당 상태 (시작 됨, 취소 됨, 변경 됨 또는 종료 됨)를 설명 합니다.

Bluetooth 게임 컨트롤러와 같은 장치에서 아날로그 단추를 사용할 경우에도 단추에 적용 되는 힘의 양이 반환 `UIPress`. `UIPress` 이벤트의 `Type` 속성은 상태를 변경 하는 실제 단추를 정의 하는 반면 나머지 속성은 발생 한 변경 사항을 설명 합니다.

다음 코드는 `UIView`에 대 한 하위 수준 `UIPress` 이벤트를 처리 하는 예를 보여 줍니다.

```csharp
using System;
using Foundation;
using UIKit;

namespace tvRemote
{
    public partial class EventView : UIView
    {
        #region Computed Properties
        public override bool CanBecomeFocused {
            get {
                return true;
            }
        }
        #endregion

        #region
        public EventView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void PressesBegan (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesBegan (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Red;
                }
            }
        }

        public override void PressesCancelled (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesCancelled (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }

        public override void PressesChanged (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesChanged (presses, evt);
        }

        public override void PressesEnded (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesEnded (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }
        #endregion
    }
}
```

`UITouch` 이벤트와 마찬가지로 `UIPress` 이벤트 재정의를 구현 해야 하는 경우 4 가지를 모두 구현 해야 합니다.

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Bluetooth 게임 컨트롤러

Apple TV와 함께 제공 되는 표준 Siri 원격 외에도, 타사의 iOS (MFI) Bluetooth 게임 컨트롤러를 Apple TV와 페어링 하 고 tvOS 앱을 제어 하는 데 사용할 수 있습니다.

[![](remote-bluetooth-images/game01.png "Bluetooth Game Controllers")](remote-bluetooth-images/game01.png#lightbox)

게임 컨트롤러를 사용 하 여 게임 플레이를 개선 하 고 게임에서 집중 교육의 의미를 제공할 수 있습니다. 또한 사용자가 원격 및 컨트롤러 간을 전환할 필요가 없도록 표준 Apple TV 인터페이스를 제어 하는 데 사용할 수 있습니다.

> [!IMPORTANT]
> Bluetooth 게임 컨트롤러는 최종 사용자가 수행할 수 있는 선택적 구매 이며 앱이 사용자를 강제로 구매할 수 없습니다. 앱이 게임 컨트롤러를 지 원하는 경우 모든 Apple TV 사용자가 게임을 사용할 수 있도록 Siri 원격도 지원 해야 합니다.

게임 컨트롤러에는 tvOS 앱 내에서 다음과 같은 기능 및 예상 된 용도가 있습니다.

|기능|일반 앱 사용|게임 앱 사용|
|---|---|---|
|**D-패드**|UI 요소 (변경 내용 포커스)를 탐색 합니다.|게임에 따라 다릅니다.|
|**변수를 잠그기 위한**|선택한 (포커스 내) 항목을 활성화 합니다.|기본 단추 함수를 수행 하 고 대화 상자 작업을 확인 합니다.|
|**B**|응용 프로그램의 주 화면에 있으면 이전 화면으로 들어가거나 홈 화면으로 돌아갑니다.|보조 단추 함수를 수행 하거나 이전 화면으로 돌아갑니다.|
|**X**|미디어 재생을 시작 하거나 재생을 일시 중지/다시 시작 합니다.|게임에 따라 다릅니다.|
|**예**|해당 없음|게임에 따라 다릅니다.|
|**메뉴**|응용 프로그램의 주 화면에 있으면 이전 화면으로 들어가거나 홈 화면으로 돌아갑니다.|게임 일시 중지/재개, 이전 화면으로 돌아갑니다. 또는 앱의 주 화면에서 홈 화면으로 돌아갑니다.|
|**왼쪽 어깨 단추**|왼쪽 탐색.|게임에 따라 다릅니다.|
|**왼쪽 트리거**|왼쪽 탐색.|게임에 따라 다릅니다.|
|**오른쪽 어깨 단추**|오른쪽으로 이동 합니다.|게임에 따라 다릅니다.|
|**오른쪽 트리거**|오른쪽 탐색|게임에 따라 다릅니다.|
|**왼쪽 엄지 스틱**|UI 요소 (변경 내용 포커스)를 탐색 합니다.|게임에 따라 다릅니다.|
|**오른쪽 엄지 스틱**|해당 없음|게임에 따라 다릅니다.|

Apple은 게임 컨트롤러를 사용할 때 다음과 같은 제안 사항을 제공 합니다.

- **게임 컨트롤러 연결 확인** -최종 사용자가 언제 든 지 tvOS 앱을 시작 하 고 중지할 수 있습니다. 앱 시작 또는 활성 시간에 게임 컨트롤러가 있는지 항상 확인 하 고 필요에 따라 작업을 수행 해야 합니다.
- **Siri 원격 및 게임 컨트롤러 모두에서 앱이 작동 하는지 확인** -사용자가 앱을 사용 하기 위해 Siri 원격과 게임 컨트롤러 간을 전환할 필요가 없습니다. 모든 유형의 컨트롤러를 사용 하 여 앱을 자주 테스트 하 여 쉽게 탐색 하 고 예상 대로 작동 하는지 확인 합니다.
- 메뉴 단추를 **다시 누르는 방법** 으로는 항상 이전 화면으로 돌아가야 합니다. 사용자가 주 앱 화면에 있는 경우 메뉴 단추를 누르면 Apple TV 홈 화면으로 돌아옵니다. 게임을 진행 하는 동안 메뉴 단추는 플레이를 일시 중지/다시 시작 하거나 주 메뉴로 돌아올 수 있는 기능을 사용자에 게 제공 하는 경고를 표시 해야 합니다.

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>게임 컨트롤러 작업

위에서 설명한 것 처럼 Apple TV와 함께 제공 되는 표준 Siri 원격 외에도 사용자는 필요에 따라 iOS (MFI) Bluetooth 게임 컨트롤러용 타사를 연결 하 고이를 사용 하 여 tvOS 앱을 제어할 수 있습니다.

앱에서 하위 수준 컨트롤러를 입력 해야 하는 경우 tvOS에 대해 다음과 같이 수정 된 Apple [Game Controller 프레임 워크](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276) 를 사용할 수 있습니다.

- Siri 원격을 대상으로 하는 마이크로 게임 컨트롤러 프로필 (`GCMicroGamepad`)이 추가 되었습니다.
- 새 `GCEventViewController` 클래스를 사용 하 여 앱을 통해 게임 컨트롤러 이벤트를 라우팅할 수 있습니다. 자세한 내용은 아래의 [게임 컨트롤러 입력 확인](#determining-game-controller-input) 섹션을 참조 하세요.

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>게임 컨트롤러 지원 요구 사항

Apple에는 tvOS 앱이 게임 컨트롤러를 지 원하는 경우 충족 해야 하는 몇 가지 특정 요구 사항이 있습니다.

- **Siri 원격을 지원 해야** 합니다. 항상 Siri 원격을 지원 해야 합니다. 게임에서 타사 게임 컨트롤러를 재생 하도록 요구할 수 없습니다.
- **확장 된 컨트롤 레이아웃을 지원 해야 합니다** . TvOS Game controller는 모두 formfitting 확장 컨트롤러입니다.
- **독립형 컨트롤러를 사용 하 여 게임을 재생할 수 있어야 합니다** . 앱이 확장 된 게임 컨트롤러를 지 원하는 경우 해당 게임 컨트롤러를 통해서만 재생할 수 있어야 합니다.
- **재생/일시 중지 단추를 지원 해야 합니다** . 게임 도중 사용자가 재생/일시 중지 단추를 누를 경우 플레이를 일시 중지/다시 시작 하거나 주 메뉴로 돌아올 수 있는 기능을 사용자에 게 제공 하는 경고를 표시 해야 합니다.

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>게임 컨트롤러 지원 사용

TvOS 앱에서 게임 컨트롤러 지원을 사용 하도록 설정 하려면 **솔루션 탐색기** 에서 `Info.plist` 파일을 두 번 클릭 하 여 편집을 위해 엽니다.

[![](remote-bluetooth-images/game02.png "The Info.plist editor")](remote-bluetooth-images/game02.png#lightbox)

**게임 컨트롤러** 섹션에서 **게임 컨트롤러를 사용 하도록 설정**하 여 검사를 수행한 다음 앱에서 지원할 모든 게임 컨트롤러 유형을 확인 합니다.

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>Siri 원격을 게임 컨트롤러로 사용

Apple TV와 함께 제공 되는 Siri 원격은 제한 된 게임 컨트롤러로 사용할 수 있습니다. 다른 게임 컨트롤러와 마찬가지로 게임 컨트롤러 프레임 워크는 `GCController` 개체로 표시 되 고 `GCMotion` 및 `GCMicroGamepad` 프로필을 모두 지원 합니다.

Siri 원격은 게임 컨트롤러로 사용 될 때 다음과 같은 특징이 있습니다.

- 터치 표면은 아날로그 입력 데이터를 제공 하는 D 패드로 사용할 수 있습니다.
- 원격은 세로 또는 가로 방향으로 사용할 수 있으며 앱은 프로필 개체가 입력 데이터를 자동으로 대칭 이동 해야 하는지 여부를 결정 합니다.
- 터치 Surface를 클릭 하면 게임 컨트롤러에서 단추 **A** 를 누르는 것 처럼 작동 합니다.
- 재생/일시 중지 단추는 게임 컨트롤러에서 단추 **X** 처럼 작동 합니다.
- 메뉴 단추를 클릭 하면 플레이를 일시 중지/다시 시작 하거나 주 메뉴로 돌아갈 수 있는 권한이 사용자에 게 제공 됩니다.

<a name="Summary" />

### <a name="determining-game-controller-input"></a>게임 컨트롤러 입력 결정

게임 컨트롤러 이벤트가 터치 이벤트와 병렬로 수신 될 수 있는 iOS와 달리 tvOS는 모든 하위 수준 이벤트를 처리 하 여 높은 수준의 `UIKit` 이벤트를 제공 합니다. 따라서 낮은 수준의 게임 컨트롤러 이벤트에 액세스 해야 하는 경우 `UIKit`의 기본 동작을 해제 해야 합니다.

TvOS에서 게임 컨트롤러 입력을 직접 처리 하려는 경우에는 게임의 사용자 인터페이스를 표시 하는 데 `GCEventViewController` (또는 하위 클래스)를 사용 해야 합니다. `GCEventViewController` *첫 번째 응답자*일 때마다 게임 컨트롤러 입력이 캡처되고 게임 컨트롤러 프레임 워크를 통해 앱에 전달 됩니다.

`GCEventViewController` 클래스의 `UserInteractionEnabled` 속성을 사용 하 여 이벤트를 처리 하 고 처리 하는 방법을 전환할 수 있습니다.

게임 컨트롤러 지원을 구현 하는 방법에 대 한 자세한 내용은 [앱 프로그래밍 가이드 tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html) And [Game Controller 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html)에서 Apple의 [게임 컨트롤러 작업](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html) 섹션을 참조 하세요.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Apple TV, 터치 Surface 제스처 및 Siri 원격 단추와 함께 제공 되는 새 Siri 원격에 대해 설명 했습니다. 다음으로는 제스처와 스토리 보드, 제스처, 코드 및 하위 수준 이벤트 작업을 다룹니다. 마지막으로, 게임 컨트롤러 작업에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
