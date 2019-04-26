---
title: MonoGame GamePad 참조
description: 이 문서에서는 GamePad, MonoGame의 입력된 장치에 액세스 하기 위한 플랫폼 간 클래스를 설명 합니다. gamepad에서 입력을 읽는 방법에 설명 하 고 예제 코드를 제공 합니다.
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 235166b78dfbd4998086a2925a54137f1922f5d1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61385702"
---
# <a name="monogame-gamepad-reference"></a>MonoGame GamePad 참조

_GamePad 클래스인 표준, 플랫폼 간 MonoGame의 입력된 장치에 액세스 하기 위한 합니다._

`GamePad` 여러 MonoGame 플랫폼에서 입력된 장치에서 입력을 읽기 위해 사용할 수 있습니다. 이 가이드에서는 GamePad 클래스를 사용 하는 방법을 보여 줍니다. 각 입력된 장치 다른 레이아웃 및 제공 하는 단추의 수 있으므로이 가이드는 다양 한 장치 매핑을 보여 주는 다이어그램을 포함 합니다.

## <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>Xbox360GamePad 대신 GamePad

제공 하는 원래 XNA API는 `Xbox360GamePad` PC 또는 Xbox 360 게임 컨트롤러에서 입력을 읽기 위한 클래스입니다. MonoGame이를 교체한는 `GamePad` Xbox 360 컨트롤러 (예: iOS 또는 Xbox One) 대부분의 MonoGame 플랫폼에서 사용할 수 없으므로 클래스입니다. 이름 변경으로 사용 해도 합니다 `GamePad` 클래스는 비슷합니다는 `Xbox360GamePad` 클래스입니다.

## <a name="reading-input-from-gamepad"></a>GamePad에서 입력을 읽기

`GamePad` 클래스는 모든 MonoGame 플랫폼에서 입력을 읽기는 표준화 된 방법을 제공 합니다. 두 메서드를 통해 정보를 제공 합니다.

- `GetState` – 컨트롤러의 단추, 아날로그 스틱 및 패드의 현재 상태를 반환 합니다.
- `GetCapabilities` – 단추나 지원 진동 컨트롤러 특정에 있는지 여부와 같은 하드웨어의 기능에 대 한 정보를 반환 합니다.

### <a name="example-moving-a-character"></a>예제: 문자를 이동합니다.

다음 코드를 설정 하 여 문자를 이동 하려면 왼쪽된 엄지 스틱을 사용할 수 있는 방법을 보여 줍니다. 해당 `XVelocity` 고 `YVelocity` 속성입니다. 이 코드는 가정 `characterInstance` 개체의 인스턴스이고 `XVelocity` 및 `YVelocity` 속성:

```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```

### <a name="example-detecting-pushes"></a>예제: 푸시를 검색합니다.

`GamePadState` 특정 단추가 눌러져 있는지 여부와 같은 컨트롤러의 현재 상태에 대 한 정보를 제공 합니다. 점프 문자 등의 특정 작업을 확인 단추 푸시된 경우 필요 (마지막 프레임을 아래로 없습니다 하지만이 프레임 중단) 또는 해제 (되었지만 마지막 프레임 아래쪽 방향으로이 프레임).

이 유형의 논리를 지역 변수는 이전 프레임을 저장 하는 데 `GamePadState` 및 현재 프레임의 `GamePadState` 만들어야 합니다. 다음 예제를 저장 하 고 이전 프레임을 사용 하는 방법을 보여 줍니다 `GamePadState` 구현 되며, 이동 하려면:

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

### <a name="example-checking-for-buttons"></a>예제: 단추에 대 한 확인

`GetCapabilities` 컨트롤러는 특정 단추 또는 아날로그 스틱 등 특정 하드웨어에 있는지 확인 하려면 사용할 수 있습니다. 다음 코드에는 두 단추의 존재 해야 게임 컨트롤러에서 B 및 Y 단추를 확인 하는 방법을 보여 줍니다.

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
> MonoGame 3.5에 대 한 NuGet 패키지에는 무선 게임 컨트롤러에 대 한 지원이 포함 되지 않습니다. GamePad 클래스를 사용 하 여 iOS에서 원본의 MonoGame 3.5 빌드하거나 MonoGame 3.6 NuGet 이진 파일을 사용 하 여 필요 합니다.

### <a name="ios-game-controller"></a>iOS 게임 컨트롤러

`GamePad` 클래스 무선 컨트롤러에서 읽은 속성을 반환 합니다. 속성에는 `GamePad` 적용 범위 미흡 표준 iOS에 대 한 컨트롤러 하드웨어 제공, 다음 다이어그램에 나와 있는 것 처럼:

![](input-images/image1.png "에서 속성을 GamePad 제공 적용 범위 미흡 표준 iOS에 대 한 컨트롤러 하드웨어가이 다이어그램에 표시 된 대로")

## <a name="apple-tv"></a>Apple TV

Apple TV 게임 입력 Siri 원격 또는 무선 게임 컨트롤러를 사용할 수 있습니다.

### <a name="siri-remote"></a>Siri 원격

*Siri 원격* Apple TV 원시 입력된 장치입니다. 이벤트를 통해 Siri 원격에서 값을 읽을 수는 있지만 (에서처럼 합니다 [Siri 원격 및 Bluetooth 컨트롤러 가이드](~/ios/tvos/platform/remote-bluetooth.md)), `GamePad` 클래스 Siri 원격에서 값을 반환할 수 있습니다.

`GamePad` play 단추에서 입력 읽기 수 및 화면을 터치 합니다.

![](input-images/image2.png "GamePad play 단추를 통해 입력을 읽고 및 화면을 터치만 수 확인")

노출 이동 읽고 터치 이후 합니다 `DPad` 속성을 사용 하 여 값을 보고 하는 이동은 `ButtonState` 클래스입니다. 즉,만 사용할 수 있는 값으로 `ButtonState.Pressed` 또는 `ButtonState.Released`, 제스처 또는 숫자 값 대신 합니다.

### <a name="apple-tv-game-controller"></a>Apple TV 게임 컨트롤러

Apple TV에 대 한 게임 컨트롤러는 iOS 앱에 대 한 게임 컨트롤러에 동일 하 게 작동 합니다. 자세한 내용은 참조는 [iOS 게임 컨트롤러](#ios-game-controller) 섹션입니다. 

## <a name="xbox-one"></a>Xbox One

Xbox One 콘솔 Xbox One 게임 컨트롤러에서 입력을 읽기를 지원합니다.

### <a name="xbox-one-game-controller"></a>Xbox One 게임 컨트롤러

Xbox One 게임 컨트롤러가 Xbox One에 대 한 가장 일반적인 입력된 장치입니다. `GamePad` 클래스 게임 컨트롤러 하드웨어에서 입력된 값을 제공 합니다.

![](input-images/image3.png "게임 컨트롤러 하드웨어에서 입력된 값을 제공 하는 GamePad 클래스")

## <a name="summary"></a>요약

이 가이드의 MonoGame의 개요를 제공 `GamePad` 클래스를 일반적인 다이어그램과 입력 읽기 논리를 구현 하는 방법을 `GamePad` 구현 합니다.

## <a name="related-links"></a>관련 링크

- [MonoGame GamePad](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
