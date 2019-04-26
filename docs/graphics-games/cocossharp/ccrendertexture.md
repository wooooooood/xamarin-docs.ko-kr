---
title: 성능 및 CCRenderTexture 사용 하 여 시각 효과
description: CCRenderTexture는 그리기 호출을 줄여 CocosSharp 게임의 성능을 개선 하기 위해 개발자가 있으며 시각 효과 만드는 데 사용할 수 있습니다. 이 가이드 샘플과 함께 제공 CCRenderTexture 효과적으로이 클래스를 사용 하는 방법의 예제를 실습을 제공 합니다.
ms.prod: xamarin
ms.assetid: F02147C2-754B-4FB4-8BE0-8261F1C5F574
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 95227689303a8367785202956a6aaef921c1c593
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61085266"
---
# <a name="performance-and-visual-effects-with-ccrendertexture"></a>성능 및 CCRenderTexture 사용 하 여 시각 효과

_CCRenderTexture는 그리기 호출을 줄여 CocosSharp 게임의 성능을 개선 하기 위해 개발자가 있으며 시각 효과 만드는 데 사용할 수 있습니다. 이 가이드 샘플과 함께 제공 CCRenderTexture 효과적으로이 클래스를 사용 하는 방법의 예제를 실습을 제공 합니다._

`CCRenderTexture` 클래스 여러 CocosSharp 개체는 단일 텍스처를 렌더링 하기 위한 기능을 제공 합니다. 일단 만들어지면 `CCRenderTexture` 효율적으로 그래픽을 렌더링 하 고 시각 효과 구현 하려면 인스턴스를 사용할 수 있습니다. `CCRenderTexture` 여러 개체를를 한 번 단일 텍스처를 렌더링할 수 있습니다. 해당 텍스처 수 한 그리기 호출의 총 수를 줄이면 프레임 마다 다시 사용 합니다.

이 가이드를 사용 하는 방법을 검사는 `CCRenderTexture` 수집 가능 카드 게임 (CCG) 렌더링 카드의 성능을 향상 하는 개체입니다. 또한 보여 줍니다 어떻게 `CCRenderTexture` 전체 엔터티를 투명 하 게 만들 데 사용할 수 있습니다. 이 가이드를 참조 합니다 `CCRenderTexture` [샘플 프로젝트](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)합니다.

![](ccrendertexture-images/image1.png "이 가이드는 CCRenderTexture 샘플 프로젝트를 참조합니다.")


## <a name="card--a-typical-entity"></a>카드 – 일반적인 엔터티

사용 하는 방법을 살펴보기 전에 `CCRenderTexture` 에서는에서는 먼저 기능을 살펴보고 직접 사용 하 여 개체를 `Card` 사용 하 여이 프로젝트 전체에서 탐색 하는 엔터티를 `CCRenderTexture` 클래스입니다. 합니다 `Card` 클래스는 엔터티 패턴에 설명 된 일반적인 엔터티를 합니다 [엔터티 가이드](~/graphics-games/cocossharp/entities.md)합니다. 카드 클래스에는 모든 visual 구성 요소 (인스턴스 `CCSprite` 및 `CCLabel`) 필드로 나열 합니다.


```csharp
class Card : CCNode
{
    bool usesRenderTexture;
    List<CCNode> visualComponents = new List<CCNode>();
    CCSprite background;
    CCSprite colorIcon;
    CCSprite monsterSprite;
    CCLabel monsterNameDisplay;
    CCLabel hpDisplay;
    CCLabel descriptionDisplay;
```

카드 인스턴스를 사용 하 여 렌더링할 수는 `CCRenderTexture`, 또는 각 시각적 구성 요소를 개별적으로 그려 합니다. 각 구성 요소는 독립 개체, 있지만 `CCNode` 사용 하면 사용 되는 엔터티를 부모로 지정는 `Card` 단일 개체로 – 적어도 대부분의 동작입니다. 예를 들어 경우는 `Card` 다음 포함된 된 모든 시각적 개체는 단일 개체로 표시 하는 카드를 확인 하려면 영향을 받는 엔터티 위치가 변경 되 면, 크기 조정 또는 회전 합니다. 단일 개체 처럼 동작 하는 카드를 확인 하려면 수정할 수 있습니다 합니다 `GameLayer.AddedToScene` 메서드를 설정 합니다 `useRenderTextures` 변수를 `false`:

    
```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    ...
}
```

합니다 `GameLayer` 코드를 이동 하지 않습니다 각 시각적 요소 독립적으로 아직 내에서 각 시각적 요소를 `Card` 엔터티 정확 하 게 위치:

![](ccrendertexture-images/image1.png "GameLayer 코드 각 시각적 요소를 독립적으로 이동 하지 않습니다 하지만 카드 엔터티 내에서 각 시각적 요소를 정확 하 게 위치")

샘플은 각 시각적 구성 요소 자체를 렌더링 하는 경우 발생할 수 있는 두 가지 문제를 노출 하도록 코딩 된:

- 여러 그리기 호출으로 인해 성능이 저하 될 수 있습니다.
- 나중에 살펴볼 것 처럼, 투명도 같은 특정 시각 효과 정확 하 게 구현할 수 없습니다.


### <a name="card-draw-calls"></a>카드 그리기 호출

코드 전체에서 발견 될 수 어떤의 단순화 된 버전 *수집 가능 카드 게임* (CCG)와 같은 "매직: 수집"또는"Hearthstone"입니다. 게임이 한 번에 세 개의 카드 표시 있고 소수의 가능한 단위 (파랑, 녹색 및 주황색). 반면 전체 게임 20 개가 넘는 카드 화면의 지정된 된 시간에 없고 플레이어 수백 개의 카드는 데크를 만들 때 선택할 수 있을 수 있습니다. 여기서 다루는 게임 현재 성능 문제에서 문제가 발생 하지 않는 경우에 유사한 구현 사용 하 여 전체 게임 될 수 있습니다.

CocosSharp는 그리기 호출을 노출 하 여 렌더링 성능에 몇 가지 정보 프레임당 수행을 제공 합니다. 우리의 `GameLayer.AddedToScene` 메서드 집합을 `GameView.Stats.Enabled` 에 `true`로 화면의 왼쪽 아래에 표시 되는 성능 정보:

![](ccrendertexture-images/image2.png "GameLayer.AddedToScene 메서드를 true로 화면의 왼쪽 아래에 표시 되는 성능 정보에서 결과 GameView.Stats.Enabled 설정")

세 개의 카드는 화면의 같더라도 했으므로 (6에서 각 카드 결과 텍스트를 그립니다. 호출을 한 번 더에 대 한 성능 정보 계정 표시) 19 개의 그리기 호출을 확인 합니다. 그리기 호출 CocosSharp 다양 한을 줄이기 위한 방법으로 제공 되므로 게임의 성능에 상당한 영향을 미칠 있습니다. 에 설명 된 방법 중 하나는 [CCSpriteSheet 가이드](~/graphics-games/cocossharp/ccspritesheet.md)합니다. 다른 기술을 사용 하는 것을 `CCRenderTexture` 이 가이드의 설명 대로 각 엔터티에 한 번의 호출까지 줄일 수 있습니다.


### <a name="card-transparency"></a>카드 투명도

우리의 `Card` 엔터티 포함을 `Opacity` 속성을 다음 코드 조각에 표시 된 대로 컨트롤 투명성:


```csharp
public override byte Opacity
{
    get
    {
        return base.Opacity;
    }
    set
    {
        base.Opacity = value;
        if (usesRenderTexture)
        {
            this.renderTexture.Sprite.Opacity = value;
        }
        else
        {
            foreach (var component in visualComponents)
            {
                component.Opacity = value;
            }
        }
    }
}
```

Setter를 사용 하 여 지 알림: 질감 또는 각 구성 요소를 개별적으로 렌더링을 렌더링 합니다. 효과 보려면 변경 합니다 `opacity` 값을 `127` (약 절반 불투명)에서 `GameLayer.AddedToScene` 각 구성 요소에 만들어집니다는 `Opacity` 값 `127`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    const byte opacity = 127;
    ...
}
```

게임은 배경에 검정 이므로 음영이 짙을 수록 표시할을 일으킨 몇 가지 투명도 사용 하 여 카드를 렌더링 이제 됩니다.

![](ccrendertexture-images/image3.png "게임은 이제 배경에 검정 이므로 음영이 짙을 수록 표시할을 일으킨 몇 가지 투명도 사용 하 여 카드를 렌더링")

얼핏 보기에 것의 카드 제대로 적용 된 투명 한 것 처럼 보일 수 있습니다. 그러나 스크린샷에서 다양 한 시각적 문제를 표시합니다.

이 백그라운드 검은색 이므로 카드의 모든 부분 투명도 인해 음영이 짙을 수록 할 예상할 수 있습니다. 즉, 더 투명 한 카드 되 면 음영이 짙을 수록 됩니다. 불투명도 0에는 `Card` 완전히 투명 하 게 됩니다 (검정 완전히). 그러나이 카드의 일부 하지 않은 짙어집니다 불투명도를 변경 되었을 때 `127`합니다. 게다가 카드의 일부 실제로 인해 밝게 더욱 명백 합니다. 검은색 였던이 카드의 일부를 살펴보겠습니다 *하기 전에* 투명 – 있었던 정보 텍스트를 특히 및 몬스터 그래픽 주위 black 개요. 이 나란히 배치할 투명도 적용의 영향을 확인할 수 있습니다.

![](ccrendertexture-images/image4.png "투명도 적용의 영향을 확인할 수 있습니다 나란히 배치 하는 경우")

위에서 설명한 대로 카드의 모든 부분이 잘 있어야 음영이 짙을 수록 더 투명 하는 경우 있지만 다양 한 영역에서에서이 비율이 높을수록 좋다고:

- 로봇 개요가 더 (회색으로 검정에서 이동)
- 설명 텍스트가 더 밝은 (회색으로 검정에서 이동)
- 로봇의 녹색 부분을 덜 포화 있지만 음영이 짙을 수록 작업량이

이 발생 하는 이유를 시각화를 위해 각 시각적 구성 요소는 독립적으로 그린, 각 부분적으로 투명 한 점을 염두 해야 합니다. 그린 첫 번째 시각적 구성 요소는 카드의 배경입니다. 후속 투명 요소 카드 위에 그려집니다 및 카드 백그라운드에서 영향을 받습니다. 이 카드에서 일부 텍스트를 제거 하 고 로봇 그래픽을 아래로 이동, 카드 백그라운드 로봇에 미치는 영향을 확인할 수 있습니다. 로봇을에서 맨 위 상자에서 주황색 줄을 볼 수 있습니다 하 고 카드의 가운데에 파란색 stripe 겹치는 로봇의 영역 음영이 짙을 수록 그려지는 알 수 있습니다.

![](ccrendertexture-images/image5.png "로봇을에서 맨 위 상자에서 주황색 줄을 볼 수 있습니다 하 고는 카드의 가운데에 파란색 stripe 겹치는 로봇의 영역에서 음영이 짙을 수록 그려집니다.")

사용 하는 `CCRenderTexture` 투명 하 게 하려면 전체 카드를 카드 내 개별 구성 요소 렌더링에 영향을 주지 않고이 가이드의 뒷부분에서 살펴볼 수 있습니다.


## <a name="using-ccrendertexture"></a>CCRenderTexture를 사용 하 여

렌더링을 설정 하겠습니다에서는 우리가 확인 한 각 구성 요소를 개별적으로 렌더링 문제가 했으므로 `CCRenderTexture` 및 동작을 비교 합니다.

렌더링을 사용할 수 있도록를 `CCRenderTexture`를 변경 합니다 `userRenderTextures` 변수를 `true` 에서 `GameLayer.AddedToScene`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = true;
```


### <a name="card-draw-calls"></a>카드 그리기 호출

그리기 호출에서 19 개가 초 성 4 바이트로 줄일 살펴보겠습니다 게임, 이제 실행 하는 경우 (각 카드 하나에 6에서 감소):

![](ccrendertexture-images/image6.png "게임 실행 된 경우 이제, 그리기 호출에서 감소 4 19 개가 초 성 각 카드 하나에 6에서 감소")

언급 한 대로이 유형의 감소 화면의 더 많은 visual 엔터티를 사용 하 여 게임에 상당한 영향을 수 있습니다.


### <a name="card-transparency"></a>카드 투명도

한 번 합니다 `useRenderTextures` 로 설정 되어 `true`, 투명 한 카드 다르게 렌더링 됩니다.

![](ccrendertexture-images/image7.png "useRenderTextures true이 고, 투명 한 카드에 설정 되 면 다르게 렌더링 됩니다.")

(왼쪽) 및 (오른쪽) 없이 렌더링 질감을 사용 하 여 투명 한 로봇 카드를 비교해 보겠습니다.

![](ccrendertexture-images/image8.png "(오른쪽) 없이 렌더링 질감 (왼쪽된) vs를 사용 하 여 투명 한 로봇 카드 비교")

세부 정보 텍스트 (밝은 회색 대신 검정) 및 로봇 스프라이트 (밝은 대신 어둡게 및 낮춘)는 가장 확실 한 차이가 발생 합니다.


## <a name="ccrendertexture-details"></a>CCRenderTexture 세부 정보

이제 살펴보았으므로 다음 사용 하는 이점은 `CCRenderTexture`에서 사용 하는 방법에 대해 살펴보겠습니다는 `Card` 엔터티.

`CCRenderTexture` 렌더링의 대상이 될 수 있는 캔버스입니다. 게임 화면을 비교할 때 두 가지 큰 차이가 있습니다.

1. `CCRenderTexture` 중간 프레임을 유지 합니다. 즉는 `CCRenderTexture` 변경이 발생할 때만 렌더링 하도록 해야 합니다. 여기서에서는 `Card` 엔터티 되지 변경 되므로 한 번만 렌더링 됩니다. 있는 경우 `Card` 구성 요소 변경 후 카드를 다시 그리도록 해야 해당 `CCRenderTexture`합니다. 예를 들어, HP 값 (상태 지점) 공격을 변경 하는 경우 다음 카드 해야 새 HP 값을 반영 하도록 자체적으로 렌더링 합니다.
1. `CCRenderTexture` 픽셀 크기를 화면에 연결 되지 않습니다. `CCRenderTexture` 장치의 해상도 보다 크거나 작을 수 있습니다. 합니다 `Card` 코드에서는 `CCRenderTexture` 해당 배경 스프라이트의 크기를 사용 하 여 합니다. 카드에 대 한 참조를 포함 한 `CCRenderTexture` 호출 `renderTexture`:


```csharp
CCRenderTexture renderTexture;
```

`renderTexture` 남아 인스턴스 `null` 때까지 합니다 `UseRenderTexture` 속성은 true, 호출 하는에 할당 `SwitchToRenderTexture`:


```csharp
private void SwitchToRenderTexture()
{
    // The card needs to be moved to the origin (0,0) so it's rendered on the render target. 
    // After it's rendered to the CCRenderTexture, it will be moved back to its old position
    var oldPosition = this.Position;
    // Make sure visuals are part of the card so they get rendered
    bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
    if (!areVisualComponentsAlreadyAdded)
    {
        // Temporarily add them so we can render the object:
        foreach (var component in visualComponents)
        {
            this.AddChild(component);
        }
    }
    // Create the render texture if it hasn't yet been made:
    if (renderTexture == null)
    {
        // Even though the game is zoomed in to create a pixellated look, we are using
        // high-resolution textures. Therefore, we want to have our canvas be 2x as big as 
        // the background so fonts don't appear pixellated
        var pixelResolution = background.ContentSize * 2;
        var unitResolution = background.ContentSize;
        renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
        //renderTexture.Sprite.Scale = .5f;
    }
    // We don't want the render target to be a child of this when we update it:
    if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
    {
        this.Children.Remove(renderTexture.Sprite);
    }
    // Move this instance back to the origin so it is rendered inside the render texture:
    this.Position = CCPoint.Zero;
    // Clears the CCRenderTexture
    renderTexture.BeginWithClear(CCColor4B.Transparent);
    // Visit renders this object and all of its children
    this.Visit();
    // Ends the rendering, which means the CCRenderTexture's Sprite can be used
    renderTexture.End();
    // We no longer want the individual components to be drawn, so remove them:
    foreach (var component in visualComponents)
    {
        this.RemoveChild(component);
    }
    // Move this back to its original position:
    this.Position = oldPosition;
    // add the render texture sprite to this:
    renderTexture.Sprite.AnchorPoint = CCPoint.Zero;
    this.AddChild(renderTexture.Sprite);
}
```

`SwitchToRenderTexture` 질감 새로 고쳐야 할 때마다 메서드를 호출할 수 있습니다. 카드를 사용 하 여 이미 있는지 여부를 호출할 수 있습니다 해당 `CCRenderTexture` 모드로 전환 하는 또는 `CCRenderTexture` 처음으로 합니다.

다음 섹션에서는 탐색을 `SwitchToRenderTexture` 메서드. 


### <a name="ccrendertexture-size"></a>CCRenderTexture 크기

CCRenderTexture 생성자는 두 개의 차원 집합이 필요합니다. 크기를 제어 하는 첫 번째는 `CCRenderTexture` 를 그려집니다 시간과 두 번째 픽셀 너비와 해당 내용의 높이 지정 합니다. 합니다 `Card` 엔터티를 인스턴스화합니다 해당 `CCRenderTexture` 백그라운드를 사용 하 여 [ContentSize](https://developer.xamarin.com/api/property/CocosSharp.CCSprite.ContentSize/)합니다. 게임에는 `DesignResolution` 의 512에서 384 에서처럼 `ViewController.LoadGame` iOS에서 및 `MainActivity.LoadGame` Android에서:


```csharp
int width = 512;
int height = 384;
// Set world dimensions
gameView.DesignResolution = new CCSizeI(width, height);
```

`CCRenderTexture` 생성자를 사용 하 여 호출 합니다 `background.ContentSize` 첫 번째 매개 변수로 나타내는 합니다 `CCRenderTexture` 배경으로 정도로 커야 `CCSprite`. 카드 백그라운드 이후 `CCSprite` 은 200 픽셀 너비, 카드 차지할 약 세로 화면 높이의 절반입니다.

두 번째 매개 변수가 전달 되는 `CCRenderTexture` 생성자의 픽셀 해상도 지정 합니다 `CCRenderTexture`합니다. 에 설명 된 대로 합니다 [CocosSharp 해상도 가이드](~/graphics-games/cocossharp/resolutions.md), 게임 단위로 볼 수 있는 영역의 높이 너비 아닙니다 종종 화면의 픽셀 해상도와 동일 합니다. 마찬가지로, 시각적 개체 표시 고해상도 장치에서 선명한는 CCRenderTexture 크기 보다 큰 해상도 사용할 수 있습니다.

픽셀 해상도 두 번 보이는 잡아에서 텍스트를 방지 하기 위해 CCRenderTexture의 크기:


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize * 2;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

비교할 pixelResolution 값에 맞게 변경할 수 있습니다는 `background.ContentSize` (되는 이중) 제외 하 고 결과 비교: 


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

![](ccrendertexture-images/image9.png "비교를 배경에 맞게 pixelResolution 값을 변경할 수 있습니다. 두 배가 되 없이 contentSize 및 결과 비교 합니다.")


### <a name="rendering-to-a-ccrendertexture"></a>CCRenderTexture로 렌더링

일반적으로 시각적 개체 CocosSharp에서 명시적으로 렌더링 되지 않습니다. 대신 시각적 개체에 추가 되는 `CCLayer` 의 일부인는 `CCScene`합니다. CocosSharp를 자동으로 렌더링 된 `CCScene` 및 호출 되 고 렌더링 코드 없이 모든 프레임에 해당 시각적 개체 계층 구조입니다. 

반면,는 `CCRenderTexture` 를 명시적으로 그려야 합니다. 이 렌더링 3 단계로 나눌 수 있습니다.

1. `CCRenderTexture.BeginWithClear` 가 호출 모든 후속 렌더링이 호출을 대상으로 나타내는 `CCRenderTexture`합니다.
1. 개체에서 상속 `CCNode` (같은 합니다 `Card` 엔터티)에 렌더링 됩니다는 `CCRenderTexture` 호출 하 여 `Visit`합니다.
1. `CCRenderTexture.End` 를 호출 하는 렌더링을 나타내는 `CCRenderTexture` 완료 됩니다.

임의 개수의 개체 렌더링 될 수는 `CCRenderTexture` 간에 해당 `Begin` 및 `End` 호출 합니다. 렌더링 하기 전에 필요한 모든 표시 개체는 자식으로 추가 됩니다.


```csharp
bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
if (!areVisualComponentsAlreadyAdded)
{
    // Temporarily add them so we can render the object:
    foreach (var component in visualComponents)
    {
        this.AddChild(component);
    }
}
```

`renderTexture` 안 카드의 일부를 렌더링할 때 제거 됩니다.


```csharp
// We don't want the render texture to be a child of the card 
// when we call Visit
if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
{
    this.RemoveChild(renderTexture.Sprite);
}
```

이제는 `Card` 인스턴스를 직접 렌더링할 수는 `CCRenderTexture` 인스턴스:


```csharp
// Clears the CCRenderTexture
renderTexture.BeginWithClear(CCColor4B.Transparent);
// Visit renders this object and all of its children
this.Visit();
// Ends the rendering, which means the CCRenderTexture's Sprite can be used
renderTexture.End();
```

개별 구성 요소는 제거 렌더링이 완료 되 면 및 `CCRenderTexture` 다시 추가 됩니다.


```csharp
// We no longer want the individual components to be drawn, so remove them:
foreach (var component in visualComponents)
{
    this.RemoveChild(component);
}
// add the render target sprite to this:
this.AddChild(renderTexture.Sprite);
```

## <a name="summary"></a>요약

이 가이드에 설명 된 `CCRenderTexture` 클래스를 사용 하 여를 `Card` 수집 가능한 카드 게임에서 사용 될 수 있는 엔터티. 사용 하는 방법에 알아보았습니다는 `CCRenderTexture` 프레임 속도 개선 하 고 제대로 엔터티 수준 투명도 구현 하는 클래스입니다.

이 가이드는 데 사용 되지만 `CCRenderTexture` 엔터티 내에 포함 된,이 클래스 수도 있고 여러 엔터티를 렌더링 하는 데 전체 `CCLayer` 화면 전체 효과 및 성능 향상에 대 한 인스턴스.

## <a name="related-links"></a>관련 링크

- [CCRenderTexture API 참조](https://developer.xamarin.com/api/type/CocosSharp.CCRenderTexture/)
- [전체 프로젝트 (샘플)](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)
