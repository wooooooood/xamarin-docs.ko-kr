---
title: "Siri 원격 인스턴스 및 Bluetooth 컨트롤러"
description: "이 문서에서는 Xamarin.tvOS 응용 프로그램에서 새 Siri 원격 인스턴스 및 Bluetooth 게임 컨트롤러를 지 원하는 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 5d74479e995c5c6ba6f6fd9fd23fbca78718ee31
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="siri-remote-and-bluetooth-controllers"></a>Siri 원격 인스턴스 및 Bluetooth 컨트롤러

_이 문서에서는 Xamarin.tvOS 응용 프로그램에서 새 Siri 원격 인스턴스 및 Bluetooth 게임 컨트롤러를 지 원하는 설명 합니다._


Xamarin.tvOS 응용 프로그램의 사용자의 인터페이스를 직접으로 ios 여기서은 탭 하지 장치의 화면에 나타나는 이미지 하지만 직접에서 사용 하 여 대화방에서 상호 작용 하지 것입니다는 [Siri 원격](#The-Siri-Remote)합니다.

게임 앱을 사용 하는 경우 필요에 따라 빌드할 수에 만든 제 3 자에 대 한 지원이 iOS (MFI) [Bluetooth 게임 컨트롤러](#Bluetooth-Game-Controllers) 도 응용 프로그램에서 합니다.

[ ![](remote-bluetooth-images/intro01.png "Bluetooth 원격 및 게임 컨트롤러")](remote-bluetooth-images/intro01.png)

이 문서에서는 설명는 [Siri 원격](#The-Siri-Remote), [화면 터치 제스처](#Touch-Surface-Gestures) 및 [Siri 원격 단추](#Siri-Remote-Buttons) 통해을 사용 하는 방법을 보여 줍니다. [제스처 및 스토리 보드](#Gestures-and-Storyboards), [제스처 및 코드](#Gestures-and-Code) 및 [하위 수준 이벤트 처리](#Low-Level-Event-Handling)합니다. 에서는 마지막으로, [게임 컨트롤러 작업](#Working-with-Game-Controllers) Xamarin.tvOS 응용 프로그램에서입니다.

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Siri 원격

사용자가 Apple TV 및 Xamarin.tvOS 앱와 상호 작용 수 됩니다는 주요 방법은 포함된 Siri 원격을 통해입니다. Apple 소파 및 TV 화면에서 공간을 표시 하는 Apple TV 사용자 인터페이스 화면에 사용자 사이의 간격을 브리징 하려면 원격 설계 되었습니다.

TvOS 응용 프로그램 개발자는 Siri 원격 터치가 속도계, 자이로스코프가 화면과 단추를 활용 하는 빠르고 편리한 시각적으로 뛰어난 사용자 인터페이스 만들기.

[ ![](remote-bluetooth-images/remote01.png "Siri 원격")](remote-bluetooth-images/remote01.png)

Siri 원격에는 다음 기능 및 tvOS 앱 내 예상된 사용:

<table width="100%" border="1px">
<tr>
    <td><b>기능</b></td>
    <td><b>일반 응용 프로그램 사용</b></td>
    <td><b>게임 앱 사용</b></td>
</tr>
<tr>
    <td valign="top"><b>화면을 터치</b><br/>탐색, 키를 선택 하 고 상황에 맞는 메뉴를 눌러을 살짝 밉니다.</td>
    <td valign="top"><b>통과/탭:</b><br/>포커스를 받을 수 항목 간 탐색 UI입니다.<br/><br/><b>클릭 합니다.</b><br/>선택한 (포커스 내) 항목을 활성화합니다.</td>
    <td valign="top"><b>통과/탭:</b><br/>게임 디자인에 따라 다르며 가장자리 탭 하 여 방향 패드도 사용할 수 있습니다.<br/><br/><b>클릭 합니다.</b><br/>기본 단추가 기능을 수행 합니다.</td>
</tr>
<tr>
    <td valign="top"><b>메뉴</b><br/>메뉴 또는 이전 화면으로 돌아가려면 키를 누릅니다.</td>
    <td valign="top">이전 화면으로 반환 하 고 기본 응용 프로그램 화면에서 Apple TV 홈 화면을 종료 합니다.</td>
    <td valign="top">일시 중지 하 고 게임 플레이 이전 화면으로 반환 하 고 기본 응용 프로그램 화면에서 Apple TV 홈 화면으로 종료 되거나 다시 시작 합니다.</td>
</tr>
<tr>
    <td valign="top"><b>Siri/Search</b><br/>Siri와 국가에서 단추를 누르고 음성 제어 모든 다른 국가에서 검색 화면에 표시 됩니다.</td>
    <td valign="top">N/A</td>
    <td valign="top">N/A</td>
</tr>
<tr>
    <td valign="top"><b>재생/일시 중지</b><br/>재생 하 고 미디어를 일시 중지 또는 응용 프로그램에서 보조 함수를 제공 합니다.</td>
    <td valign="top">미디어 재생, 일시 중지/다시 시작 재생을 시작합니다.</td>
    <td valign="top">보조 단추 함수를 수행 하는지, 아니면 소개 비디오를 건너뜁니다 (하는 경우 존재).</td>
</tr>
<tr>
    <td valign="top"><b>Home</b><br/>홈 화면으로 돌아가려면 키를 눌러, 실행 중인 응용 프로그램을 표시, 누르고에 장치를 절전 모드를 두 번 클릭 합니다.</td>
    <td valign="top">N/A</td>
    <td valign="top">N/A</td>
</tr>
<tr>
    <td valign="top"><b>볼륨</b><br/>컨트롤은 오디오/비디오 장치 볼륨을 연결 합니다.</td>
    <td valign="top">N/A</td>
    <td valign="top">N/A</td>
</tr>
</table>

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>터치 표면 제스처

Siri 원격 터치 화면은 다양 한 Xamarin.tvOS 앱에서에 응답할 수 있습니다. 있는 제스처를 한 손가락을 검색할 수 있습니다:

<table width="100%">
<tr>
    <td valign="top" width="30%"><img src="remote-bluetooth-images/Gesture01.png"></td>
    <td valign="top" width="30%"><img src="remote-bluetooth-images/Gesture02.png"></td>
    <td valign="top" width="30%"><img src="remote-bluetooth-images/Gesture03.png"></td>
</tr>
<tr>
    <td valign="top"><b>살짝 밉니다.</b><br/>선택 영역 (포커스 내) 화면에서 UI 요소 간에 이동 (위쪽, 아래쪽, 왼쪽 마우스 오른쪽 단추로). 큰 목록이 신속 하 게 관성을 사용 하 여 콘텐츠를 스크롤할 수 넘기기가 사용할 수 있습니다.</td>
    <td valign="top"><b>클릭 합니다.</b><br/>선택한 (포커스 내) 항목을 활성화 하거나 게임에서 단추 처럼 동작 합니다. 누른 채 클릭 상황에 맞는 메뉴 또는 보조 기능을 활성화할 수 있습니다.</td>
    <td valign="top"><b>누릅니다.</b><br/>가장자리에 터치 화면을 눌러 lightly 패드-D, 위쪽, 아래쪽, 왼쪽 또는 탭 영역에 따라 오른쪽에 포커스를 이동에 방향 단추 처럼 작동 합니다. 앱에 따라 용도 숨겨진된 컨트롤을 표시 합니다.</td>
</tr>
</table>

Apple 화면 터치 제스처를 사용 하기 위한 다음 제안 사항을 제공 합니다.

* **클릭 및 탭 구분** -클릭 하는 사용자가 의도적으로 작업 하 고 선택, 활성화 및 게임의 단추에 대 한 가장 적합 합니다. 탭 보다 미묘 이며 사용자는 자신의 손에 종종 Siri 원격 보유 하 고 Tap 이벤트를 쉽게 활성화할 실수로 수 때문에 가끔 사용 해야 합니다.
* **표준 제스처를 재정의 하지 않는** -사용자에 게 특정 제스처의 특정 작업을 수행 되는 기대 수준을 응용 프로그램에서 이러한 제스처의 함수 또는 의미를 재정의 하지 않아야 합니다. 한 가지 예외는 활성 도중 게임 앱입니다.
* **새 제스처 엄밀한 정의** -다시 사용자에 게 특정 제스처는 특정 작업을 수행 하는 것 이란 가정 합니다. 표준 동작을 수행 하려면 사용자 지정 제스처를 정의 하면 안 됩니다. 설정 되며 마찬가지로 게임 가장 일반적인 예외 사용자 지정 제스처 게임을 플레이 재미, 몰입를 추가할 수 있습니다.
* **필요한 경우 패드 탭에 응답** 터치 화면의 가장자리에서 포커스가 이동 게임 컨트롤러 또는, 방향 패드 아래쪽, 왼쪽 처럼 반응 하거나 마우스 오른쪽 단추로 모퉁이에서 누르기 Lightly-합니다. 필요한 경우 응용 프로그램 또는 게임에서 이러한 제스처에 응답 해야 합니다.

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Siri 원격 단추

터치 화면에 제스처를 외에도 앱은 터치 화면을 클릭 하거나 재생/일시 중지 단추를 사용자에 게 응답할 수 있습니다. 게임 컨트롤러 프레임 워크를 사용 하 여 Siri 원격에 액세스 하는 경우에 메뉴 단추를 누르는 동작을 검색할 수 있습니다. 

또한 메뉴 단추 누르기를 검색할 수 제스처 인식기를 사용 하 여 표준 `UIKit` 요소입니다. 메뉴 단추를 누르는 동작을 차단 하는 경우 현재 보기와 보기 컨트롤러 닫는 작업을 담당 하 고 이전에 반환 합니다.

> [!IMPORTANT]
> **참고:** 해야 **항상** 원격에서 재생/일시 중지 단추에 함수를 할당 합니다. 작동 하지 않는 단추가 끊어진 최종 사용자에 게 확인 앱을 만들 수 있습니다. 이 단추에 대 한 올바른 함수를 설정 하지 않은 경우 (터치 화면 클릭) 하 고 기본 단추와 동일한 기능을 할당 합니다.




<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>제스처 및 스토리 보드

Siri 원격 Xamarin.tvOS 응용 프로그램에서 사용 하는 가장 쉬운 방법은 인터페이스 디자이너에서 보기에 제스처 인식기를 추가 하는 것입니다.

제스처 인식기를 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**, 두 번 클릭은 `Main.storyboard` 파일을 열어 인터페이스 디자이너 편집 합니다.
2. 끌어서는 **탭 제스처 인식기** 에서 **라이브러리** 보기에 놓습니다. 

    [ ![](remote-bluetooth-images/storyboard01.png "Tap 제스처 인식기")](remote-bluetooth-images/storyboard01.png)
3. 확인 **선택** 에 **단추** 의 섹션은 **특성 검사기**: 

    [ ![](remote-bluetooth-images/storyboard02.png "선택 확인")](remote-bluetooth-images/storyboard02.png)
4. **선택** 제스처 사용자 클릭에 응답 하 의미는 **터치 화면** Siri 원격입니다. 에 대 한 응답 옵션도 제공는 **메뉴**, **재생/일시 중지**, **를**, **아래로**, **왼쪽** 및 **오른쪽** 단추입니다.
5. 다음으로 연결 하는 **동작** 에서 **탭 제스처 인식기** 메서드를 호출 하 고 `TouchSurfaceClicked`: 

    [ ![](remote-bluetooth-images/storyboard03.png "Tap 제스처 인식기에서 작업")](remote-bluetooth-images/storyboard03.png)
6. 변경 내용을 저장 하 고 Mac.에 Visual Studio로 반환 합니다.

편집 뷰 컨트롤러 (예제 `FirstViewController.cs`) 파일 트리거된 제스처를 처리 하는 다음 코드를 추가 합니다.

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

스토리 보드를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Hello, tvOS 퀵 스타트 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다. 구체적으로 [사용자 인터페이스 만들기](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface) 및 [콘센트 및 동작을 사용 하 여 코드를 작성](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) 섹션.

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>제스처 및 코드

필요에 따라 C# 코드에서 직접 제스처를 만들고 사용자 인터페이스에서 보기에 추가할 수 있습니다. 예를 들어 일련의 살짝 제스처 인식기를 추가 하려면 뷰 컨트롤러를 편집 하 고 다음 코드를 추가 합니다.

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

에 따라 사용자 정의 형식을 만드는 경우 `UIKit` Xamarin.tvOS 응용 프로그램에서 (예를 들어 `UIView`)를 통해 단추 누름의 하위 수준 처리를 제공할 수 있습니다 `UIPress` 이벤트입니다. 

A `UIPress` 이벤트는 tvOS에는 `UITouch` 이벤트를 제외 하 고 iOS의 경우에 `UIPress` Siri 원격에서 누르는 단추에 대 한 정보 또는 연결 된 다른 Bluetooth 장치 (예: 게임 컨트롤러)를 반환 합니다. `UIPress` 이벤트 단추를 누르는 동작 및 (Began, Canceled, 변경 또는 종료 됨) 상태에 설명 합니다. 

아날로그 단추 Bluetooth 게임 컨트롤러와 같은 장치에 대 한 `UIPress` 도 단추에 적용 되 고 force의 크기를 반환 합니다. `Type` 의 속성은 `UIPress` 이벤트 정의 물리적 단추에 변경 된 상태, 나머지 속성 하는 동안 발생 한 변경 내용에 설명 합니다.

다음 코드 예를 보여 줍니다 처리의 하위 수준 `UIPress` 에 대 한 이벤트는 `UIView`:

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

와 마찬가지로 `UITouch` 이벤트 중 하나 이상을 실행 하는 경우는 `UIPress` 이벤트 재정의 하지만, 4를 모두 구현 해야 합니다.

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Bluetooth 게임 컨트롤러

Apple TV, 제 3 자, iOS 용 만든와 함께 제공 되는 표준 Siri 원격 외에도 (MFI) Bluetooth 게임 컨트롤러 수 Apple TV 쌍을 이루는 있고 Xamarin.tvOS 앱을 제어 하는 데 사용 합니다.

[ ![](remote-bluetooth-images/game01.png "Bluetooth 게임 컨트롤러")](remote-bluetooth-images/game01.png)

게임 컨트롤러 게임을 개선 하 고 게임에 대 한 생생한의 의미를 제공 데 사용할 수 있습니다. 원격 인스턴스 및 컨트롤러 사이 전환할 필요가 사용 되므로 표준 Apple TV 인터페이스를 제어 하도 사용할 수 있습니다.

> [!IMPORTANT]
> **참고:** Bluetooth 게임 컨트롤러는 최종 사용자가 만들 수 있는 별도로 구매, 앱을 구입 사용자를 강제할 수 없습니다. 앱에서 게임 컨트롤러를 지 원하는 경우 게임 파일이 Apple TV의 모든 사용자가 사용할 수 있도록 Siri 원격도 지원 해야 합니다.


게임 컨트롤러에는 다음 기능 및 tvOS 앱 내 예상된 사용:
<table width="100%" border="1px">
<tr>
    <td><b>기능</b></td>
    <td><b>일반 응용 프로그램 사용</b></td>
    <td><b>게임 앱 사용</b></td>
</tr>
<tr>
    <td valign="top"><b>D-Pad</b></td>
    <td valign="top">UI 요소 (포커스 내 변경 내용)를 통해 이동합니다.</td>
    <td valign="top">게임에 따라 다릅니다.</td>
</tr>
<tr>
    <td valign="top"><b>A</b></td>
    <td valign="top">선택한 (포커스 내) 항목을 활성화합니다.</td>
    <td valign="top">기본 단추가 기능을 수행 하 고 대화 상자 작업을 확인 합니다.</td>
</tr>
<tr>
    <td valign="top"><b>B</b></td>
    <td valign="top">응용 프로그램의 주 화면에 있는 경우 홈 화면을 종료 하거나 이전 화면으로 반환 합니다.</td>
    <td valign="top">보조 단추 기능을 수행 하거나 이전 화면으로 돌아갑니다.</td>
</tr>
<tr>
    <td valign="top"><b>X</b></td>
    <td valign="top">미디어 재생 또는 일시 중지/재개 재생을 시작합니다.</td>
    <td valign="top">게임에 따라 다릅니다.</td>
</tr>
<tr>
    <td valign="top"><b>Y</b></td>
    <td valign="top">N/A</td>
    <td valign="top">게임에 따라 다릅니다.</td>
</tr>
<tr>
    <td valign="top"><b>메뉴</b></td>
    <td valign="top">응용 프로그램의 주 화면에 있는 경우 홈 화면을 종료 하거나 이전 화면으로 반환 합니다.</td>
    <td valign="top">일시 중지/다시 시작 게임 플레이 홈 화면으로 종료 또는 반환 이전 화면으로 응용 프로그램의 주 화면에 있는 경우.</td>
</tr>
<tr>
    <td valign="top"><b>왼쪽된 입력 단추</b></td>
    <td valign="top">왼쪽 탐색합니다.</td>
    <td valign="top">게임에 따라 다릅니다.</td>
</tr>
<tr>
    <td valign="top"><b>왼쪽된 트리거</b></td>
    <td valign="top">왼쪽 탐색합니다.</td>
    <td valign="top">게임에 따라 다릅니다.</td>
</tr>
<tr>
    <td valign="top"><b>오른쪽 입력 단추</b></td>
    <td valign="top">오른쪽 이동합니다.</td>
    <td valign="top">게임에 따라 다릅니다.</td>
</tr>
<tr>
    <td valign="top"><b>오른쪽 트리거가</b></td>
    <td valign="top">오른쪽 이동</td>
    <td valign="top">게임에 따라 다릅니다.</td>
</tr>
<tr>
    <td valign="top"><b>왼쪽된 엄지 스틱</b></td>
    <td valign="top">UI 요소 (포커스 내 변경 내용)를 통해 이동합니다.</td>
    <td valign="top">게임에 따라 다릅니다.</td>
</tr>
<tr>
    <td valign="top"><b>오른쪽 엄지</b></td>
    <td valign="top">N/A</td>
    <td valign="top">게임에 따라 다릅니다.</td>
</tr>
</table>

Apple 게임 컨트롤러를 사용 하기 위한 다음 제안 사항을 제공 합니다.

- **게임 컨트롤러 연결을 확인** -tvOS 앱을 시작 하 고 최종 사용자가 언제 든 지 중지할 수 있습니다. 항상 응용 프로그램 시작 또는 절전 모드 해제 시간에서 게임 컨트롤러 있는지 확인 하 고 필요에 따라 작업을 수행 해야 합니다.
- **Siri 원격 인스턴스 및 게임 컨트롤러 모두에서 작동 하는 응용 프로그램 사용자 확인** -Siri 원격 및 게임 컨트롤러 응용 프로그램을 사용 하도록 전환 하려면 사용자가 필요 하지 않습니다. 두 가지 유형의 모든 쉽게 탐색 하 고 예상 대로 작동 하는 컨트롤러를 종종 응용 프로그램을 테스트 합니다.
- **다시 방식으로 제공** -메뉴 버튼을 눌러서 이전 화면으로 항상 반환 해야 합니다. 기본 응용 프로그램 화면에서 사용자가을 메뉴 단추 Apple TV 홈 화면에이 반환 해야 합니다. 게임을 플레이 하는 동안 메뉴 단추에는 사용자에 게 기본 메뉴로 돌아가거나 게임 플레이 일시 중지/다시 시작 하는 기능을 제공 하는 경고를 표시 되어야 합니다.

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>게임 컨트롤러 작업

위에서 설명한 대로 Siri 원격 함께 제공 되는 경우 Apple TV, 사용자와 필요에 따라 연결할 수는 표준 뿐만 아니라 타사, iOS (MFI) Bluetooth 게임 컨트롤러를 수행 하며 Xamarin.tvOS 앱을 제어 하는 데 사용할.

Apple의 사용 응용 프로그램 필수 하위 수준 컨트롤러 입력을 [게임 컨트롤러 프레임 워크](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276) 있으며 그 tvOS에 대 한 다음 수정:

- 마이크로 게임 컨트롤러 프로필 (`GCMicroGamepad`) Siri 원격 대상에 추가 되었습니다.
- 새 `GCEventViewController` 응용 프로그램을 통해 게임 컨트롤러 이벤트에 라우트하도록 클래스를 사용할 수 있습니다. 참조는 [게임 컨트롤러 입력 결정](#Determining-Game-Controller-Input) 자세한 내용을 보려면 아래 섹션.

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>게임 컨트롤러 지원 요구 사항

Apple에 Xamarin.tvOS 앱 게임 컨트롤러를 지 원하는 경우 충족 해야 하는 몇 가지 특정 요구 사항이 있습니다.

- **Siri 원격을 지원 해야** -항상 Siri 원격을 지원 해야 합니다. 게임 타사 게임 컨트롤러 재생할 수를 사용할 수 없습니다.
- **확장 컨트롤 레이아웃을 지원 해야** -모든 tvOS 게임 컨트롤러는 formfitting, 컨트롤러를 확장 합니다.
- **게임 Stand-Alone 컨트롤러와 Playable 해야** -응용 프로그램에 확장 게임 컨트롤러를 지 원하는 경우 해당 게임 컨트롤러 에서만 재생할 수 여야 합니다.
- **재생/일시 중지 단추를 지원 해야** -게임을 플레이 하는 동안 재생/일시 중지 단추를 누를 경우 사용자에 게 게임 플레이 일시 중지/다시 시작 하는 기능을 제공 하는 경고를 표시 하거나 해야 주 메뉴에 반환 합니다.

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>게임 컨트롤러 지원을 사용 하도록 설정

게임 컨트롤러 지원 Xamarin.tvOS 응용 프로그램을 사용 하도록 설정 하려면 두 번 클릭은 `Info.plist` 파일에 **솔루션 탐색기** 편집 하기 위해 열려는:

[ ![](remote-bluetooth-images/game02.png "Info.plist 편집기")](remote-bluetooth-images/game02.png)

아래는 **게임 컨트롤러** 섹션에서 확인란을 선택 하 여 **게임 컨트롤러를 사용 하도록 설정**, 모든 응용 프로그램에서 지원 되는 게임 컨트롤러 유형을 확인 합니다.

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>Siri 원격을 사용 하 여 게임 컨트롤러

Apple TV와 함께 제공 되는 Siri 원격 제한 게임 컨트롤러로 사용할 수 있습니다. 마찬가지로 다른 게임 컨트롤러에에서 표시로 게임 컨트롤러 프레임 워크는 `GCController` 개체와 지원은 `GCMotion` 및 `GCMicroGamepad` 프로필입니다.

Siri 원격 게임 컨트롤러로 사용 되는 경우 다음과 같은 특징이 있습니다.

- 터치 화면 아날로그 입력된 데이터를 제공 하는 방향 패드도 사용할 수 있습니다.
- 원격 세로 또는 가로 방향에서 사용할 수 있습니다 및 응용 프로그램 프로필 개체 입력된 데이터를 자동으로 전환 해야 하는 경우를 결정 합니다.
- 단추를 누르면 터치 화면을 클릭 비슷합니다 **A** 게임 컨트롤러에 있습니다.
- 재생/일시 중지 단추는 단추 처럼 설정과 **X** 게임 컨트롤러에 있습니다.
- 메뉴 단추에는 사용자에 게 기본 메뉴로 돌아가거나 게임 플레이 일시 중지/다시 시작 하는 기능을 제공 하는 경고를 표시 되어야 합니다.

<a name="Summary" />

### <a name="determining-game-controller-input"></a>게임 컨트롤러 입력 확인

터치 이벤트와 병렬로 게임 컨트롤러 이벤트를 받을 수 있는 iOS의 경우와 달리 tvOS 높은 수준의 배달 하는 모든 하위 수준 이벤트를 처리 `UIKit` 이벤트입니다. 결과적으로, 낮은 수준의 게임 컨트롤러 이벤트에 대 한 액세스를 해야 하는 경우 해야 해제 `UIKit`의 기본 동작입니다.

TvOS를 사용 해야 하는 직접 게임 컨트롤러 입력을 처리 하려는 경우에 `GCEventViewController` (또는 해당 서브 클래스)의 사용자 인터페이스는 게임을 표시 합니다. 때마다는 `GCEventViewController` 는 *첫 번째 응답자*, 게임 컨트롤러 입력 캡처되고 게임 컨트롤러 프레임 워크를 통해 응용 프로그램에 배달 합니다.

사용할 수 있습니다는 `UserInteractionEnabled` 의 속성은 `GCEventViewController` 이벤트 처리 되 고 처리 방법을 전환 하려면 클래스입니다.

게임 컨트롤러 지원을 구현 하는 방법에 대 한 정보에 대 한 Apple의를 참조 하십시오 [게임 컨트롤러 작업](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html) 섹션은 [tvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html) 및 [게임 컨트롤러 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html)합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서는 Apple TV, 화면 터치 제스처 및 Siri 원격 단추와 함께 제공 되는 새로운 Siri 원격 검사가 수행 됩니다. 다음으로, 제스처 및 스토리 보드, 제스처 및 코드 및 하위 수준 이벤트를 다룹니다. 마지막으로, 게임 컨트롤러 작업을 설명 하는 경우.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
