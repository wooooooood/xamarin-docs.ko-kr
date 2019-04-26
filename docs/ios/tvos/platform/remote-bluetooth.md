---
title: Siri 원격 및 Bluetooth 컨트롤러 Xamarin에서 tvOS에 대 한
description: 이 문서에서는 Siri 원격 및 Bluetooth 게임 컨트롤러 Xamarin으로 작성 된 tvOS 앱에서 사용 하 여 작업 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: BDB9894A-236B-424B-9032-ACD12A6C5720
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 79022f7a454ea423fa3112a4c4ade2bcd471fbb8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60933065"
---
# <a name="siri-remote-and-bluetooth-controllers-for-tvos-in-xamarin"></a>Siri 원격 및 Bluetooth 컨트롤러 Xamarin에서 tvOS에 대 한

Xamarin.tvOS 앱의 사용자가 해당 인터페이스로 직접 ios를 탭 할 하지 장치의 화면에 나타나는 이미지 하지만 직접에서 사용 하 여 대화방에서 상호 작용 하지 합니다 [Siri 원격](#The-Siri-Remote)합니다.

게임 앱을 사용 하는 경우 필요에 따라에서 빌드할 수 있습니다에 대 한 제 3 자에 대 한 지원 (MFI) iOS [Bluetooth 게임 컨트롤러](#Bluetooth-Game-Controllers) 앱도 있습니다.

[![](remote-bluetooth-images/intro01.png "Bluetooth 원격 및 게임 컨트롤러")](remote-bluetooth-images/intro01.png#lightbox)

이 문서에 설명 합니다는 [Siri 원격](#The-Siri-Remote), [터치 화면 제스처](#Touch-Surface-Gestures) 및 [Siri 원격 단추](#Siri-Remote-Buttons) 통해 상호 작동 하는 방법을 보여 [제스처 및 스토리 보드](#Gestures-and-Storyboards), [제스처와 코드](#Gestures-and-Code) 하 고 [하위 수준 이벤트 처리](#Low-Level-Event-Handling)합니다. 마지막으로,이 설명 [게임 컨트롤러와 작업](#Working-with-Game-Controllers) Xamarin.tvOS 앱에서.

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Siri 원격

기본 사용자가 Apple TV 및 Xamarin.tvOS 앱 상호 작용 하는 방법은 포함된 Siri 원격입니다. Apple 소파 대화방 TV 화면에 표시 되는 Apple TV 사용자 인터페이스에 앉아 사용자 사이의 간격을 브리징 하려면 원격 설계 되었습니다.

TvOS 앱 개발자는 Siri 원격의 터치가 속도계, 자이로스코프가 화면과 단추를 활용 하는 빠르고 사용 하기 쉬운 시각적으로 멋진 사용자 인터페이스 만들기.

[![](remote-bluetooth-images/remote01.png "Siri 원격")](remote-bluetooth-images/remote01.png#lightbox)

Siri 원격에는 다음 기능 및 tvOS 앱 내에서 예상된 사용량:

|기능|일반 앱 사용|게임 앱 사용|
|---|---|---|
|**터치 화면**<br />탐색 키를 선택 하 고 상황에 맞는 메뉴를 누릅니다을 살짝 밉니다.|**Tap/Swipe**<br />포커스 항목 간의 UI 탐색 합니다.<br /><br />**클릭**<br />선택한 (포커스 내) 항목을 활성화합니다.|**Tap/Swipe**<br />게임 디자인에 따라 다르며 가장자리 탭 하 여 방향 패드도 사용할 수 있습니다.<br /><br />**클릭**<br />기본 단추 함수를 수행 합니다.|
|**메뉴**<br />이전 화면에서 메뉴에 반환 하는 키를 누릅니다.|이전 화면으로 반환 하 고 기본 앱 화면에서 Apple TV 홈 화면으로 종료 됩니다.|일시 중지 하 고 이전 화면으로 반환 하 고 기본 앱 화면에서 Apple TV 홈 화면으로 종료 게임 플레이 다시 시작 합니다.|
|**Siri/Search**<br />Siri 사용 하 여 국가에서 키를 누른 음성 컨트롤의 경우 모든 다른 국가에서 검색 화면을 표시 합니다.|N/A|N/A|
|**재생/일시 중지**<br />재생 하 고 미디어를 일시 중지 하거나 앱에서 보조 기능을 제공 합니다.|미디어 재생 및 일시 중지/계속할 재생을 시작합니다.|보조 단추 함수를 수행 하거나 소개 비디오를 건너뜁니다 (하는 경우 존재).|
|**Home**<br />홈 화면으로 돌아가려면 키를 눌러, 실행 중인 앱을 표시, 누른 장치를 절전 모드를 두 번 클릭 합니다.|N/A|N/A|
|**볼륨**<br />컨트롤에는 오디오/비디오 장치가 볼륨을 연결 합니다.|N/A|N/A|

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>터치 화면 제스처

Siri 원격의 터치 화면은 다양 한 Xamarin.tvOS 앱에서 응답할 수 있는 한 손가락 제스처를 검색할 수 있습니다.

|살짝 밀기|클릭 대상|탭|
|---|---|---|
|![](remote-bluetooth-images/Gesture01.png)|![](remote-bluetooth-images/Gesture02.png)|![](remote-bluetooth-images/Gesture03.png)|
|선택 영역 (포커스 내) 화면에서 UI 요소 간에 이동 (, 왼쪽, 아래쪽 오른쪽). 큰 목록을 신속 하 게 관성을 사용 하 여 콘텐츠를 스크롤할 수 살짝를 사용할 수 있습니다.|선택한 (포커스 내) 항목을 활성화 하거나 게임의 기본 단추 처럼 작동 합니다. 누른 채 클릭 상황에 맞는 메뉴 또는 보조 함수를 활성화할 수 있습니다.|방향 패드-D, 위쪽, 아래쪽, 왼쪽 또는 탭 영역 오른쪽에 따라 포커스를 이동 단추 처럼 작동 lightly 가장자리에서 터치 화면을 탭 합니다. 앱에 따라 숨겨진된 컨트롤을 표시 하기 위해 사용할 수 있습니다.|

Apple에서는 화면 터치 제스처를 사용 하 여 작업 하기 위한 다음 제안 사항을 제공 합니다.

* **클릭 및 탭을 구분할** -사용자가 의도적으로 작업이 클릭 하 고 게임의 기본 단추를 선택, 정품 인증에 적합 합니다. 탭은 더 미묘 하 고 사용자 속속들이에서 종종 Siri 원격 보유 하 고 탭 이벤트를 쉽게 활성화할 실수로 수 있으므로 꼭 필요할 때만 사용 해야 합니다.
* **표준 제스처를 재정의 하지** -사용자가 특정 제스처의 특정 작업을 수행 되는 기대 하는 앱의 의미 나 이러한 제스처의 함수를 재정의 하지 않아야 합니다. 게임 앱을를 활성 게임 플레이 중 변경 하는 예외가입니다.
* **새 제스처를 제한적으로 정의** -마찬가지로 사용자가 해당 특정 제스처를 특정 작업을 수행 하는 것 이란 가정 합니다. 표준 작업을 수행할 사용자 지정 제스처를 정의 하면 안 됩니다. 와 마찬가지로 게임은 가장 일반적인 예외는 사용자 지정 제스처 재미 있게 몰입 형 플레이 게임에 추가할 수 있습니다.
* **해당 하는 경우 탭 패드 응답** 가볍게 터치 화면 가장자리는 왼쪽 아래로 이동 포커스 게임 컨트롤러 또는 방향, 위쪽 방향 패드와 같은 react 또는 오른쪽 모서리에 있는 탭-합니다. 해당 하는 경우에 앱 또는 게임에서 다음이 제스처에 응답 해야 합니다.

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Siri 원격 단추

터치 화면에서 제스처를 외에도 앱 사용자가 터치 화면을 클릭 하거나 재생/일시 중지 단추를 누르면와 응답할 수 있습니다. Siri 원격 게임 컨트롤러 프레임 워크를 사용 하 여 액세스 하는 경우에 메뉴 단추를 눌렀는지를 감지할 수 있습니다. 

또한 메뉴 단추 누름 검색할 수는 제스처 인식기를 사용 하 여 표준 `UIKit` 요소입니다. 메뉴 단추를 눌렀는지를 가로챌 수에서는 현재 뷰와 뷰 컨트롤러를 닫는 담당를 이전에 반환 합니다.

> [!IMPORTANT]
> 수행 해야 합니다 **항상** 원격에서 Play/Pause 단추에 함수를 할당 합니다. 비기능적 단추 최종 사용자에 게 중단을 확인 하는 앱을 만들 수 있습니다. 이 단추는 올바른 함수가 없으면 (터치 화면 클릭) 단추와 동일한 기능을 할당 합니다.

<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>제스처 및 스토리 보드

Xamarin.tvOS 앱에서 Siri 원격을 사용 하는 가장 쉬운 방법은 인터페이스 디자이너에서 보기에 제스처 인식기를 추가 하는 것입니다.

제스처 인식기를 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**, 두 번 클릭 합니다 `Main.storyboard` 파일과 인터페이스 디자이너를 편집용으로 엽니다.
2. 끌어서를 **탭 제스처 인식기** 에서 **라이브러리** 보기에 놓습니다. 

    [![](remote-bluetooth-images/storyboard01.png "탭 제스처 인식기")](remote-bluetooth-images/storyboard01.png#lightbox)
3. 확인 **선택** 에 **단추** 섹션의 **특성 검사기**: 

    [![](remote-bluetooth-images/storyboard02.png "선택 확인")](remote-bluetooth-images/storyboard02.png#lightbox)
4. **선택** 제스처를 클릭 하는 사용자 응답할 것을 의미 합니다 **터치 화면** Siri 원격입니다. 에 대 한 응답 옵션도 있습니다 합니다 **메뉴**, **재생/일시 중지**, **위로**, **아래로**, **왼쪽** 및 **오른쪽** 단추입니다.
5. 다음으로 연결 하는 **동작** 에서 합니다 **탭 제스처 인식기** 호출 `TouchSurfaceClicked`: 

    [![](remote-bluetooth-images/storyboard03.png "탭 제스처 인식기에서 작업")](remote-bluetooth-images/storyboard03.png#lightbox)
6. 변경 내용을 저장 하 고 mac 용 Visual Studio로 돌아가서

편집 보기 컨트롤러 (예제 `FirstViewController.cs`) 파일과 트리거되지 제스처를 처리 하는 다음 코드를 추가 합니다.

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

스토리 보드를 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Tvos 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다. 특히 합니다 [사용자 인터페이스 만들기](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface) 하 고 [출 선 및 작업을 사용 하 여 코드를 작성](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) 섹션입니다.

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>제스처와 코드

필요에 따라 만들 수 있습니다 제스처 직접 C# 코드 및 사용자 인터페이스에는 보기에 추가 합니다. 예를 들어 일련의 살짝 밀기 제스처 인식기를 추가 하려면 보기 컨트롤러 편집한 다음 코드를 추가 합니다.

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

사용자 지정 형식을 기반으로 만드는 경우 `UIKit` Xamarin.tvOS 앱에서 (예를 들어 `UIView`), 단추 누름을 통해 하위 수준 처리를 제공할 수 있습니다 `UIPress` 이벤트입니다. 

A `UIPress` 이벤트는 tvOS에는 `UITouch` 제외 하 고 ios, 이벤트는 `UIPress` Siri 원격에서 단추에 대 한 정보를 누르는 또는 연결 된 다른 Bluetooth 장치 (예: 게임 컨트롤러)를 반환 합니다. `UIPress` 이벤트 단추를 누르는 및 (시작, 취소, 변경 또는 종료 됨) 상태를 설명 합니다. 

아날로그 단추 Bluetooth 게임 컨트롤러와 같은 장치에 대 한 `UIPress` 또한 force 단추에 적용 되 고 한 시간을 반환 합니다. 합니다 `Type` 의 속성을 `UIPress` 이벤트 정의 실제 단추 상태가 변경, 속성의 나머지 부분 하는 동안 발생 한 변경 내용에 설명 합니다.

다음 코드는 낮은 수준의 처리의 예가 나와 `UIPress` 에 대 한 이벤트를 `UIView`:

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

와 마찬가지로 `UITouch` 이벤트를 구현 하는 경우는 `UIPress` 이벤트 재정의 4를 모두 구현 해야 합니다.

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Bluetooth 게임 컨트롤러

Apple TV, 제 3 자에 대 한 iOS를 사용 하 여 제공 되는 표준 Siri 원격 외에도 (MFI) Bluetooth 게임 컨트롤러 Apple TV를 사용 하 여 쌍을 이루는 고 Xamarin.tvOS 앱을 제어 하는 데 사용 될 수 있습니다.

[![](remote-bluetooth-images/game01.png "Bluetooth Game Controllers")](remote-bluetooth-images/game01.png#lightbox)

게임 플레이 높이고 게임에서 집중 교육의 의미를 제공 게임 컨트롤러를 사용할 수 있습니다. 사용 하 여 원격 인스턴스 및 컨트롤러 사이 전환 하지 않아도 되므로 표준 Apple TV 인터페이스를 제어 하도 사용 수 있습니다.

> [!IMPORTANT]
> Bluetooth 게임 컨트롤러는 최종 사용자가 만들 수 있는 선택적 구매, 앱 사용자가 구매를 강제할 수 없습니다. 게임 컨트롤러를 지 원하는 앱도 지원 해야 Siri 원격 게임을 모든 Apple TV 사용자가 사용할 수 있도록 합니다.

게임 컨트롤러에는 다음 기능 및 tvOS 앱 내에서 예상된 사용량:

|기능|일반 앱 사용|게임 앱 사용|
|---|---|---|
|**D-Pad**|UI 요소 (포커스 변경)를 통해 이동합니다.|게임에 따라 달라 집니다.|
|**A**|선택한 (포커스 내) 항목을 활성화합니다.|주 단추 기능을 수행 하 고 대화 상자 작업을 확인 합니다.|
|**B**|홈 화면에 앱의 주 화면에 있는 경우 종료 또는 이전 화면으로 반환 합니다.|보조 단추 기능을 수행 하거나 이전 화면으로 반환 합니다.|
|**X**|미디어 재생 또는 일시 중지/다시 재생을 시작합니다.|게임에 따라 달라 집니다.|
|**Y**|N/A|게임에 따라 달라 집니다.|
|**메뉴**|홈 화면에 앱의 주 화면에 있는 경우 종료 또는 이전 화면으로 반환 합니다.|앱의 주 화면에 있는 경우 게임 플레이, 홈 화면으로 종료 또는 이전 화면으로 반환을 일시 중지/계속 합니다.|
|**왼쪽된 어깨 단추**|왼쪽 탐색합니다.|게임에 따라 달라 집니다.|
|**왼쪽된 트리거**|왼쪽 탐색합니다.|게임에 따라 달라 집니다.|
|**올바른 자격 증명 입력 단추**|오른쪽 이동합니다.|게임에 따라 달라 집니다.|
|**올바른 트리거**|오른쪽 이동|게임에 따라 달라 집니다.|
|**왼쪽된 엄지 스틱**|UI 요소 (포커스 변경)를 통해 이동합니다.|게임에 따라 달라 집니다.|
|**오른쪽 엄지 스틱**|N/A|게임에 따라 달라 집니다.|

Apple에서는 게임 컨트롤러를 사용 하 여 작업 하기 위한 다음 제안 사항을 제공 합니다.

- **게임 컨트롤러 연결 확인** -tvOS 앱 시작 및 최종 사용자가 언제 든 지 중지할 수 있습니다. 항상 응용 프로그램 시작 또는 절전 모드 해제 시간에 게임 컨트롤러의 현재 상태를 확인 하 고 필요에 따라 작업을 수행 해야 합니다.
- **Siri 원격 및 게임 컨트롤러 모두에서 사용자 앱 작동 확인** -Siri 원격 및 게임 컨트롤러 앱을 사용 하도록 전환 하는 사용자를 필요 하지 않습니다. 두 가지 유형의 모든 이동할 간단 하 고 예상 대로 작동 하는 컨트롤러를 사용 하 여 앱을 자주 테스트 합니다.
- **방법을 다시 제공** -메뉴 단추를 누르면 이전 화면을 항상 반환 해야 합니다. 기본 앱 화면에 사용자 인 경우 메뉴 단추 Apple TV 홈 화면에이 반환 해야 합니다. 게임 플레이 하는 동안 메뉴 단추 사용자 일시 중지/계속할 게임 플레이 하거나 주 메뉴에 반환 하는 기능을 제공 하는 경고를 표시 해야 합니다.

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>게임 컨트롤러 작업

위에서 설명한 대로 Siri 원격 함께 제공 되는 Apple TV에서 사용자를 사용 하 여 필요에 따라 연결할 수는 표준 외에도 타사, iOS (MFI) Bluetooth 게임 컨트롤러에 대 한 고 Xamarin.tvOS 앱 제어를 사용 합니다.

앱 하위 수준 컨트롤러 입력, 필요한 경우에 Apple의를 사용 하 여 다음 작업을 수 있습니다 [게임 컨트롤러 프레임 워크](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276) tvOS에 대 한 다음 수정 된:

- Micro 게임 컨트롤러 프로필 (`GCMicroGamepad`) Siri 원격 대상에 추가 되었습니다.
- 새 `GCEventViewController` 클래스 앱을 통해 게임 컨트롤러 이벤트 라우팅에 사용할 수 있습니다. 참조 된 [게임 컨트롤러 입력 결정](#determining-game-controller-input) 대 한 자세한 내용은 아래 섹션입니다.

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>게임 컨트롤러 지원 요구 사항

Apple에 앱 Xamarin.tvOS 게임 컨트롤러를 지 원하는 경우 충족 해야 하는 몇 가지 특정 요구 사항:

- **Siri 원격을 지원 해야** -항상 Siri 원격을 지원 해야 합니다. 게임 타사를 재생할 수 있는 게임 컨트롤러 필요 하지 않습니다.
- **확장 된 컨트롤의 레이아웃을 지원 해야** -모든 tvOS 게임 컨트롤러는 formfitting, 컨트롤러를 확장 합니다.
- **게임에 독립 실행형 컨트롤러를 사용 하 여 Playable 해야** -앱을 확장 하는 게임 컨트롤러를 지 원하는 경우 해당 게임 컨트롤러를 사용 하 여 전적으로 재생할 수 여야 합니다.
- **Play/Pause 단추를 지원 해야** -게임 플레이 하는 동안 사용자가 Play/Pause 단추를 누르면 게임 플레이 일시 중지/다시 시작 하는 기능을 사용자에 게 부여 경고를 표시 하거나 해야 주 메뉴로 돌아갑니다.

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>게임 컨트롤러 지원을 사용 하도록 설정

Xamarin.tvOS 앱에서 게임 컨트롤러 지원을 사용 하려면 두 번 클릭 합니다 `Info.plist` 파일를 **솔루션 탐색기** 열어 편집 하려면:

[![](remote-bluetooth-images/game02.png "Info.plist 편집기")](remote-bluetooth-images/game02.png#lightbox)

아래는 **게임 컨트롤러** 섹션에서 확인란을 선택 하 여 **게임 컨트롤러를 사용 하도록 설정**, 모든 앱에서 지원 되는 게임 컨트롤러 유형을 확인 합니다.

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>Siri 원격을 사용 하 여 게임 컨트롤러

Siri 원격 Apple TV 함께 제공 되는 제한 된 게임 컨트롤러로 사용할 수 있습니다. 다른 게임 컨트롤러와 마찬가지로 표시로 게임 컨트롤러 프레임 워크에는 `GCController` 개체 및 지원 합니다 `GCMotion` 및 `GCMicroGamepad` 프로필입니다.

Siri 원격에 게임 컨트롤러로 사용 되는 경우는 다음과 같은 특징이 있습니다.

- 터치 화면 아날로그 입력된 데이터를 제공 하는 방향 패드를 사용할 수 있습니다.
- 세로 또는 가로 방향으로 원격을 사용할 수 있습니다 및 앱 프로필 개체를 입력된 데이터를 자동으로 전환 해야 하는 경우를 결정 합니다.
- 단추를 누르면 터치 화면을 클릭 비슷합니다 **는** 게임 컨트롤러에 있습니다.
- Play/Pause 단추는 단추 처럼 작동 **X** 게임 컨트롤러에 있습니다.
- 메뉴 단추 사용자 일시 중지/계속할 게임 플레이 하거나 주 메뉴에 반환 하는 기능을 제공 하는 경고를 표시 해야 합니다.

<a name="Summary" />

### <a name="determining-game-controller-input"></a>게임 컨트롤러 입력 확인

TvOS 터치 이벤트와 함께 병렬로 게임 컨트롤러 이벤트를 받을 수 있는 iOS와 달리 높은 수준의 전달할 모든 하위 수준 이벤트를 처리할 `UIKit` 이벤트입니다. 결과적으로 낮은 수준의 게임 컨트롤러 이벤트에 대 한 액세스를 해야 하는 경우 해야 해제 `UIKit`의 기본 동작입니다.

TvOS 게임 컨트롤러 입력을 사용 해야 하는 직접 처리 하려는 경우에 `GCEventViewController` (또는 하위 클래스) 게임을 표시할 사용자 인터페이스의 합니다. 때마다를 `GCEventViewController` 되는 *첫 번째 응답자*, 게임 컨트롤러 입력 캡처되고 게임 컨트롤러 프레임 워크를 통해 앱에 전달 합니다.

사용할 수는 `UserInteractionEnabled` 의 속성을 `GCEventViewController` 이벤트 처리 되 고 처리 하는 방법을 전환 하려면 클래스.

게임 컨트롤러 지원 구현에 대 한 자세한 내용은 Apple의를 참조 하세요 [게임 컨트롤러와 작업](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html) 섹션을 [tvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html) 고 [게임 컨트롤러 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html)합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Apple TV, 화면 터치 제스처 및 Siri 원격 단추와 함께 제공 되는 새 Siri 원격 설명 했습니다. 다음으로, 제스처 및 스토리 보드, 제스처 및 코드 및 하위 수준 이벤트를 설명합니다. 마지막으로, 게임 컨트롤러를 사용 하 여 작업을 설명 하는 경우.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
