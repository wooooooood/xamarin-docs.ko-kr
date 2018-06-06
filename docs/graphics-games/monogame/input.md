---
title: MonoGame 게임 패드 참조
description: 이 문서에서는 게임 패드, MonoGame에서 입력된 장치에 액세스 하기 위한 플랫폼 간 클래스에 설명 합니다. 게임 패드에서 입력을 읽는 방법에 설명 하 고 예제 코드를 제공 합니다.
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: f8a833dbab2b8a69f328cd26cb69db84f3083222
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783326"
---
# <a name="monogame-gamepad-reference"></a>MonoGame 게임 패드 참조

_게임 패드 MonoGame에서 입력된 장치에 액세스 하기 위한 표준, 플랫폼 간 클래스입니다._

`GamePad` 여러 MonoGame 플랫폼에서 입력된 장치에서 입력을 읽기 위해 사용할 수 있습니다. 이 가이드에는 게임 패드 클래스와 함께 작업 하는 방법을 보여 줍니다. 각 입력된 장치를 다른 레이아웃 및 제공 하는 단추 수 있으므로이 가이드에서는 다양 한 장치 매핑을 보여 주는 다이어그램입니다.

## <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>게임 패드 Xbox360GamePad에 대 한 대체 값으로

제공 하는 원래 XNA API는 `Xbox360GamePad` Xbox 360 또는 PC에서 게임 컨트롤러에서 입력을 읽기에 대 한 클래스입니다. MonoGame은이를 대체 한 `GamePad` 클래스 이후 Xbox 360 컨트롤러 대부분 MonoGame 플랫폼 (예: iOS 또는 Xbox One)에서 사용할 수 없습니다. 이름 변경의 사용도 불구 하 고는 `GamePad` 클래스는 비슷합니다는 `Xbox360GamePad` 클래스입니다.

## <a name="reading-input-from-gamepad"></a>게임 패드에서 입력을 읽기

`GamePad` 모든 MonoGame 플랫폼에서 입력을 읽기의 표준화 된 방법을 제공 합니다. 두 개의 메서드를 통해 정보를 제공합니다.

- `GetState` – 컨트롤러의 단추, 아날로그 스틱 및 패드의 현재 상태를 반환 합니다.
- `GetCapabilities` – 단추 또는 지원 진동 컨트롤러 특정에 있는지 여부와 같은 하드웨어의 기능에 대 한 정보를 반환 합니다.

### <a name="example-moving-a-character"></a>예: 문자를 이동합니다.

다음 코드를 설정 하 여 문자를 이동 하려면 왼쪽된 엄지 스틱을 사용할 수 있는 방법을 보여 줍니다. 그 `XVelocity` 및 `YVelocity` 속성입니다. 이 코드를 가정 하는 `characterInstance` 에 있는 개체의 인스턴스가 `XVelocity` 및 `YVelocity` 속성:

```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```

### <a name="example-detecting-pushes"></a>예: 푸시를 검색합니다.

`GamePadState` 특정 단추를 누를 여부와 같은 컨트롤러의 현재 상태에 대 한 정보를 제공 합니다. 점프, 문자를 확인 하는 등 특정 작업을 확인 하는 중 단추 푸시된 하는 경우 필요 (마지막 프레임 아래로 없습니다. 하지만이 프레임 다운) 또는 해제 (마지막 프레임 아래로 하지만 방향으로이 프레임은). 

이 유형의 논리를 이전 프레임을 저장 하는 로컬 변수를 수행 하려면 `GamePadState` 및 현재 프레임의 `GamePadState` 만들어야 합니다. 다음 예제에서는 저장 하 고 이전 프레임을 사용 하는 방법을 보여 줍니다. `GamePadState` 구현 되며, 이동 하려면:

```csharp
// At class scope:
// Store the last frame's and this frame's GamePadStates.
// "new" them so that code doesn't have to perform null checks:
GamePadState lastFrameGamePadState = new GamePadState();
GamePadState currentGamePadState = new GamePadState();
protected override void Update(GameTime gameTime)
{
    // store off the last state before reading the new one:
    lastFrameGamePadState = currentGamePadState;
    currentGamePadState = GamePad.GetState(PlayerIndex.One);
    bool wasAButtonPushed = 
currentGamePadState.Buttons.A == ButtonState.Pressed
        && lastFrameGamePadState.Buttons.A == ButtonState.Released;
    if(wasAButtonPushed)
    {
        MakeCharacterJump();
    }
...
}
```

### <a name="example-checking-for-buttons"></a>예: 단추에 대 한 검사

`GetCapabilities` 데 사용할 수 있는 컨트롤러 특정 단추 또는 아날로그 메모리 등의 특정 하드웨어에 있는지 확인 합니다. 다음 코드에는 두 단추의 현재 상태를 요구 하는 게임 컨트롤러에 B 및 Y 단추에 대 한 확인 하는 방법을 보여 줍니다.

```csharp
var capabilities = GamePad.GetCapabilities(PlayerIndex.One);
bool hasBButton = capabilities.HasBButton;
bool hasXButton = capabilities.HasXButton;
if(!hasBButton || !hasXButton)
{
    NotifyUserOfMissingButtons();
}
```

## <a name="ios"></a>iOS

iOS 앱 무선 게임 컨트롤러 입력을 지원합니다.

> [!IMPORTANT]
> MonoGame 3.5에 대 한 NuGet 패키지는 무선 게임 컨트롤러에 대 한 지원이 포함 되지 않습니다. 원본의 MonoGame 3.5 빌드하거나 MonoGame 3.6 NuGet 이진 파일을 사용 하 여 iOS에서 게임 패드 클래스를 사용 하 여이 필요 합니다. 

### <a name="ios-game-controller"></a>iOS 게임 컨트롤러

`GamePad` 클래스 무선 컨트롤러에서 읽은 속성을 반환 합니다. 속성에는 `GamePad` 올바른 검사는 표준 iOS에 대 한 컨트롤러 하드웨어를 제공는 다음 다이어그램에 나와 있는 것 처럼:

![](input-images/image1.png "에서 속성은 게임 패드 제공 올바른 검사 표준 iOS에 대 한 컨트롤러 하드웨어가이 다이어그램에 나와 있는 것 처럼")

## <a name="apple-tv"></a>Apple TV

Apple TV 게임 입력에 대 한 Siri 원격 또는 무선 게임 컨트롤러를 사용할 수 있습니다.

### <a name="siri-remote"></a>Siri 원격

*Siri 원격* Apple TV에 대 한 기본 입력된 장치 됩니다. 이벤트를 통해 Siri 원격에서 값을 읽을 수는 있지만 (에서처럼는 [Siri 원격 인스턴스 및 Bluetooth 컨트롤러 가이드](~/ios/tvos/platform/remote-bluetooth.md)), `GamePad` 클래스 Siri 원격에서 값을 반환할 수 있습니다.

다음에 유의 `GamePad` 있는 재생 단추에서 입력을 읽기 수 및 화면을 터치 합니다. 

![](input-images/image2.png "게임 패드 수만 있는 재생 단추에서 입력을 읽고와 화면을 터치")

터치 이후 표면 이동을 읽기는 `DPad` 속성을 사용 하 여 값을 보고 이동은 `ButtonState` 클래스입니다. 즉, 값으로 사용할 수만 `ButtonState.Pressed` 또는 `ButtonState.Released`숫자 값 이나 제스처는 달리, 합니다.

### <a name="apple-tv-game-controller"></a>Apple TV 게임 컨트롤러

Apple TV에 대 한 게임 컨트롤러 iOS 앱에 대 한 게임 컨트롤러를 동일 하 게 작동 합니다. 자세한 내용은 참조는 [iOS 게임 컨트롤러 섹션](#iOS_Game_Controller)합니다. 

## <a name="xbox-one"></a>Xbox One

Xbox One 콘솔 Xbox One 게임 컨트롤러에서 입력을 읽기를 지원합니다.

### <a name="xbox-one-game-controller"></a>Xbox 한 게임 컨트롤러

Xbox One 게임 컨트롤러는 Xbox One에 대 한 가장 일반적인 입력된 장치. `GamePad` 클래스 게임 컨트롤러 하드웨어에서 입력된 값을 제공 합니다.

![](input-images/image3.png "게임 컨트롤러 하드웨어에서 입력된 값을 제공 하는 게임 패드 클래스")

## <a name="summary"></a>요약

이 가이드의 MonoGame의 개요를 제공 `GamePad` 클래스 입력 읽기 논리 및 공통의 다이어그램을 구현 하는 방법을 `GamePad` 구현 합니다.

## <a name="related-links"></a>관련 링크

- [MonoGame GamePad](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
