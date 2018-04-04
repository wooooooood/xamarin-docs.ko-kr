---
title: 성능 및 CCRenderTexture로 시각 효과
description: CCRenderTexture는 개발자가 그리기 호출을 줄여서 CocosSharp 게임의 성능을 향상 시킬 수 있으며 특정 시각 효과 만드는 데 사용할 수 있습니다. 이 가이드 실습 효과적으로이 클래스를 사용 하는 방법의 예제를 제공 하도록 CCRenderTexture 샘플을 포함 합니다.
ms.prod: xamarin
ms.assetid: F02147C2-754B-4FB4-8BE0-8261F1C5F574
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 25eba08c1c72ba6fefd39b949b504f8e6fabe983
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="performance-and-visual-effects-with-ccrendertexture"></a>성능 및 CCRenderTexture로 시각 효과

_CCRenderTexture는 개발자가 그리기 호출을 줄여서 CocosSharp 게임의 성능을 향상 시킬 수 있으며 특정 시각 효과 만드는 데 사용할 수 있습니다. 이 가이드 실습 효과적으로이 클래스를 사용 하는 방법의 예제를 제공 하도록 CCRenderTexture 샘플을 포함 합니다._

`CCRenderTexture` 클래스 여러 CocosSharp 개체 단일 텍스처를 렌더링 하기 위한 기능을 제공 합니다. 일단 만들어지면 `CCRenderTexture` 그래픽을 효율적으로 렌더링 하 고 시각 효과 구현 하려면 인스턴스를 사용할 수 있습니다. `CCRenderTexture` 여러 개체를를 한 번에 하나의 텍스처 렌더링할 수 있습니다. 그런 다음 해당 질감 수 있습니다 그리기 호출의 총 수를 줄이고 모든 프레임을 다시 사용 합니다.

이 가이드를 사용 하는 방법을 검사는 `CCRenderTexture` 수집 가능한 카드 게임 (CCG)에서 렌더링 카드의 성능을 향상 시키기 위해 개체입니다. 에 대해서도 설명 방법을 `CCRenderTexture` 전체 엔터티를 투명 하 게 만드는 데 사용할 수 있습니다. 이 가이드를 참조는 `CCRenderTexture` [샘플 프로젝트](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)합니다.

![](ccrendertexture-images/image1.png "이 가이드는 CCRenderTexture 샘플 프로젝트를 참조합니다.")


## <a name="card--a-typical-entity"></a>카드 – 일반적인 엔터티

사용 하는 방법을 살펴보기 전에 `CCRenderTexture` 개체와 직접 먼저 익히게 됩니다 우리는 `Card` 이 프로젝트 전체 탐색을 사용 해야 하는 엔터티는 `CCRenderTexture` 클래스입니다. `Card` 클래스는에 설명 된 엔터티 패턴을 하는 일반적인 엔터티는 [엔터티 가이드](~/graphics-games/cocossharp/entities.md)합니다. 카드 클래스에는 모든 visual 구성 요소 (인스턴스의 `CCSprite` 및 `CCLabel`) 필드로 나열 합니다.


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

카드 인스턴스를 사용 하 여 렌더링할 수는 `CCRenderTexture`, 또는 각 시각적 구성 요소를 개별적으로 그려 합니다. 각 구성 요소는 독립 개체는 `CCNode` 사용 하면 사용 되는 엔터티를 부모로 지정는 `Card` – 이상 대부분 단일 개체로 작동 합니다. 예를 들어 경우는 `Card` 포함된 된 모든 시각적 개체는 단일 개체로 같지 카드에 영향을 다음에 엔터티 위치가 변경, 크기 조정 또는 회전 합니다. 단일 개체 처럼 동작 하는 카드를 확인 하려면 수정할 수 있을 `GameLayer.AddedToScene` 설정 하는 메서드는 `useRenderTextures` 변수를 `false`:

    
```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    ...
}
```

`GameLayer` 코드 이동 하지 않고 각 시각적 요소 개별적으로 아직 내의 각 시각적 요소는 `Card` 엔터티 위치가 올바르게:

![](ccrendertexture-images/image1.png "카드 엔터티 내에서 각 시각적 요소 위치가 올바르게 아직 GameLayer 코드 각 시각적 요소를 개별적으로 이동 하지 않습니다.")

샘플은 자체 각 시각적 구성 요소를 렌더링 하는 경우 발생할 수 있는 두 가지 문제를 노출 하도록 코딩:

- 여러 그리기 호출으로 인해 성능이 저하 될 수 있습니다.
- 나중에 살펴볼 것 처럼, 투명도 같은 특정 시각적 효과 정확 하 게 구현할 수 없습니다.


### <a name="card-draw-calls"></a>카드 그리기 호출

이 코드는 전체에서 확인할 수 있습니다 어떻게의 단순화 *수집 가능한 카드 게임* (CCG) 예: "매직:: The 수집" 또는 "Hearthstone"입니다. 게임만 3 개의 카드 한 번에 하며 표시 (파란색, 녹색 및 주황색)에 사용 가능한 단위 수가 적은 합니다. 반면 전체 게임 20 개가 넘는 카드 화면의 지정된 된 시간에 없고 플레이어는 수백 개의 카드는 데크를 만들 때 선택할 수 있을 수 있습니다. 게임 현재 성능 문제에서 문제가 발생 하지 않는 경우에 비슷한 구현과 함께 전체 게임 않을 수 있습니다.

CocosSharp는 프레임당을 수행 하는 그리기 호출을 노출 하 여 렌더링 성능에 대 한 일부 정보를 제공 합니다. 우리의 `GameLayer.AddedToScene` 메서드 집합은 `GameView.Stats.Enabled` 를 `true`, 화면 왼쪽 아래에 표시 되는 성능 정보에서 결과:

![](ccrendertexture-images/image2.png "GameView.Stats.Enabled 화면 왼쪽 아래에 표시 되는 성능 정보에서 그 결과 true로 설정 하는 GameLayer.AddedToScene 메서드")

화면에 3 개의 카드 하더라도 있는지 (6 개에서 각 카드 결과 그리기 호출, 성능 정보 계정 한 개에 대 한 표시 텍스트) 19 개의 그리기 호출을 확인 합니다. 그리기 호출 CocosSharp는 다양 한을 줄일 수 있는 방법 제공 하므로 게임의 성능에 상당한 영향을 줄 합니다. 에 설명 된 방법 중 하나는 [CCSpriteSheet 가이드](~/graphics-games/cocossharp/ccspritesheet.md)합니다. 또 다른 기법을 사용 하는 `CCRenderTexture` 이 가이드에 설명 된 대로 한 번의 호출까지 각 엔터티를 줄일 수 있습니다.


### <a name="card-transparency"></a>카드 투명도

우리의 `Card` 엔터티를 포함 한 `Opacity` 다음 코드 조각에 표시 된 대로 컨트롤 투명도로 속성:


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

Setter에서는 사용 하 여 질감 또는 각 구성 요소를 개별적으로 렌더링 렌더링 함을 확인 합니다. 변경 효과 볼는 `opacity` 값을 `127` (약 절반 불투명도)에서 `GameLayer.AddedToScene` 되 고 있는 각 구성 요소에서 만들어집니다는 `Opacity` 값 `127`:


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

게임 카드와 해당 일부 투명도 검은색 배경 이므로 어둡게를 렌더링 이제 됩니다.

![](ccrendertexture-images/image3.png "게임 카드와 해당 일부 투명도 검은색 배경 이므로 어둡게를 이제 렌더링 합니다.")

얼핏 보기에 두 명의 카드 제대로 적용 된 투명 하 게 하는 경우이 같습니다. 그러나 스크린샷에서 다양 한 표시 문제를 표시합니다.

우리의 배경 검정 이므로 찾아야 우리의 카드의 모든 부분 투명도 인해 짙을 수록 될 것입니다. 즉, 더는 카드 되 면 짙을 수록 됩니다. 불투명도 0에는 `Card` 완전히 투명 하 게 됩니다 (완전히 검정색)입니다. 하지만 우리의 카드의 일부가 하지 않은 될 짙을 수록 때 불투명도로 변경 되었습니다 `127`합니다. 설상가상으로 우리의 카드의 일부가 실제로 꽉 밝은 더 투명 상태가 된 경우. 검은색 했어야 우리의 카드의 부분을 살펴 보겠습니다 *하기 전에* – 투명 한 것 몬스터 그래픽 주위 검은색 윤곽선과 특히 자세한 설명 합니다. म 나란히 배치, 투명도 적용의 영향을 보면 수 있습니다.

![](ccrendertexture-images/image4.png "투명도 적용의 영향을 볼 수을 나란히 배치 하는 경우")

위에서 설명 했 듯이 카드의 모든 부분이 잘 짙을 수록 더 투명 때 있지만 다양 한 영역의이 그렇지 않은 경우:

- 로봇의 개요는 더 밝은 (회색으로 검정에서 이동)
- 설명 텍스트가 더 밝은 (회색으로 검정에서 이동)
- 로봇의 녹색 부분 덜 포화 하지만 짙을 수록 많지

이 발생 하는 이유를 시각화 하려면 각 시각적 구성 요소는 독립적으로 그려지는, 각 부분적으로 투명 하 게 유지 해야 합니다. 그린 첫 번째 시각적 구성 요소는 카드의 배경입니다. 후속 투명 요소 카드 맨 위에 그려집니다 및 카드 백그라운드에서 영향을 받습니다. 우리의 카드에서 일부 텍스트를 제거 하 고 로봇 그래픽 아래로 이동, 카드 배경 로봇에 미치는 영향에 확인할 수 있습니다. 맨 위에 있는 상자에서 주황색 줄 로봇을에서 볼 수 있습니다 및 카드의 가운데에 파란색 stripe 겹치는 로봇 영역 짙을 수록 그려짐을 사항을 참고 하십시오.

![](ccrendertexture-images/image5.png "맨 위에 있는 상자에서 주황색 줄 로봇을에서 볼 수 있습니다 및 카드의 가운데에 파란색 stripe 겹치는 로봇 영역 짙을 수록 그려짐을")

사용 하는 `CCRenderTexture` 투명 하 게 전체 카드는 카드 내 개별 구성 요소 렌더링에 영향을 주지 않고이 가이드의 뒷부분에 나와 있듯이을 수 있습니다.


## <a name="using-ccrendertexture"></a>CCRenderTexture를 사용 하 여

렌더링에 설정 됩니다는 각 구성 요소를 개별적으로 렌더링 문제가 확인 되었습니다, 이제는 `CCRenderTexture` 및 동작을 비교 합니다.

렌더링에 사용할 수 있도록는 `CCRenderTexture`, 변경 된 `userRenderTextures` 변수를 `true` 에 `GameLayer.AddedToScene`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = true;
```


### <a name="card-draw-calls"></a>카드 그리기 호출

그리기 호출에서 19 4 바이트로 줄일 보면 게임을 지금 실행 하는 경우 (각 카드 하나에 6 개에서 감소):

![](ccrendertexture-images/image6.png "게임을 지금를 실행 하는 경우 그리기 호출에서 감소-4 개로 19 각 카드 하나에 6 개에서 감소")

위에서 언급 한 대로이 유형의 감소는 화면에 시각적 추가 엔터티와 함께 게임에 큰 영향을 받을 수 있습니다.


### <a name="card-transparency"></a>카드 투명도

한 번는 `useRenderTextures` 로 설정 된 `true`, 투명 한 카드 다르게 렌더링 됩니다.

![](ccrendertexture-images/image7.png "useRenderTextures true이 고, 투명 한 카드에 설정 되 고 나면 다르게 렌더링 합니다.")

(왼쪽) 및 (오른쪽) 없이 렌더링 텍스처를 사용 하 여 투명 한 로봇 카드를 비교해 보겠습니다.

![](ccrendertexture-images/image8.png "(오른쪽) 없이 렌더링 질감 (왼쪽된) vs를 사용 하 여 투명 한 로봇 카드 비교")

가장 확실 한 차이점은 세부 정보 텍스트 (밝은 회색 대신, 검정) 및 로봇 sprite (밝은 대신 어두운 및 흐릿한).


## <a name="ccrendertexture-details"></a>CCRenderTexture 세부 정보

사용 하는 이점을 버퍼 오버런 `CCRenderTexture`에서 사용 방법에 대해 살펴보겠습니다는 `Card` 엔터티.

`CCRenderTexture` 캔버스 렌더링 대상이 될 수입니다. 게임 화면에 비해 두 가지 주요 차이점이 있습니다.

1. `CCRenderTexture` 사이 프레임을 유지 합니다. 즉, 한 `CCRenderTexture` 변경이 발생할 때 렌더링만 해야 합니다. 이 경우에는 `Card` 엔터티는 바뀌지 되므로 한 번만 렌더링 됩니다. 있는 경우 `Card` 카드를 그리도록 해야 하는 다음 구성 요소가 변경 해당 `CCRenderTexture`합니다. 예를 들어, HP 값 (상태 포인트) 공격 하는 경우 변경 된 경우 카드 새 HP 값을 반영 하도록 자체적으로 렌더링 해야 합니다.
1. `CCRenderTexture` 픽셀 치수 화면에 연결 되지 않습니다. A `CCRenderTexture` 장치의 해상도 보다 크거나 작을 수 있습니다. `Card` 코드 만듭니다는 `CCRenderTexture` 해당 배경 sprite의 크기를 사용 하 여 합니다. 카드에 대 한 참조를 포함 한 `CCRenderTexture` 호출 `renderTexture`:


```csharp
CCRenderTexture renderTexture;
```

`renderTexture` 남아 인스턴스 `null` 될 때까지 `UseRenderTexture` 속성은 true, 되는 호출에 할당 된 `SwitchToRenderTexture`:


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

`SwitchToRenderTexture` 질감을 새로 고칠 필요가 될 때마다 메서드를 호출할 수 있습니다. 카드 이미 사용 하 고 있는지 여부를 호출할 수 있습니다는 `CCRenderTexture` 전환 하 고 또는 `CCRenderTexture` 처음으로 합니다.

다음 섹션에서는 `SwitchToRenderTexture` 메서드. 


### <a name="ccrendertexture-size"></a>CCRenderTexture 크기

CCRenderTexture 생성자에는 두 개의 차원 집합이 필요합니다. 크기를 제어 하는 첫 번째는 `CCRenderTexture` 그린 것 시점과 두 번째 픽셀 너비와 콘텐츠 높이 지정 합니다. `Card` 엔터티 인스턴스화합니다 해당 `CCRenderTexture` 백그라운드를 사용 하 여 [ContentSize](https://developer.xamarin.com/api/property/CocosSharp.CCSprite.ContentSize/)합니다. 게임에는 `DesignResolution` 512에서 384, 에서처럼 `ViewController.LoadGame` iOS에서 및 `MainActivity.LoadGame` Android에서:


```csharp
int width = 512;
int height = 384;
// Set world dimensions
gameView.DesignResolution = new CCSizeI(width, height);
```

`CCRenderTexture` 와 생성자가 호출 됩니다는 `background.ContentSize` 첫 번째 매개 변수로 나타내는 `CCRenderTexture` 배경으로 크기 `CCSprite`합니다. 카드 배경 이후 `CCSprite` 높이가 200 픽셀인 이면 카드 차지할 대략 화면의 세로 높이의 절반입니다.

두 번째 매개 변수가 전달 되는 `CCRenderTexture` 의 픽셀 해상도 지정 하는 생성자는 `CCRenderTexture`합니다. 에 설명 된 대로 [CocosSharp 해상도 가이드](~/graphics-games/cocossharp/resolutions.md), 너비와 높이 게임 단위로 표시 된 영역의 종종 같지 않습니다는 화면 픽셀 해상도로 합니다. 마찬가지로, 시각적 개체 표시 고해상도 장치 보여줍니다는 CCRenderTexture 크기 보다 큰 해상도 사용할 수 있습니다.

픽셀 해상도 두 번 찾고 픽셀에서 텍스트를 방지 하기 위해 CCRenderTexture의 크기:


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize * 2;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

일치 시킬 pixelResolution 값 비교를 변경할 수 있습니다는 `background.ContentSize` 되 고 두 배가) (제외 하 고 결과 비교: 


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

![](ccrendertexture-images/image9.png "비교를 배경과 일치 하도록 pixelResolution 값을 변경할 수 있습니다. 되 고 두 배로 증가 하지 않고 contentSize 하 고 결과 비교 합니다.")


### <a name="rendering-to-a-ccrendertexture"></a>CCRenderTexture로 렌더링

일반적으로 시각적 개체 CocosSharp에서 명시적으로 렌더링 되지 않습니다. 대신 시각적 개체에 추가 되는 `CCLayer` 에 포함 되어 있는 한 `CCScene`합니다. CocosSharp 자동으로 렌더링 된 `CCScene` 및 호출 되 고 렌더링 코드 없이 모든 프레임에 해당 시각적 계층 구조입니다. 

반면,는 `CCRenderTexture` 를 명시적으로 그려야 합니다. 이 렌더링 세 단계로 나눌 수 있습니다.

1. `CCRenderTexture.BeginWithClear` 호출을 나타내는 모든 후속 렌더링이 호출 하는 대상 `CCRenderTexture`합니다.
1. 개체에서 상속 `CCNode` (같은 `Card` 엔터티)으로 렌더링 되는 `CCRenderTexture` 호출 하 여 `Visit`합니다.
1. `CCRenderTexture.End` 해당 렌더링을 나타내는 호출 되는 `CCRenderTexture` 완료 되었습니다.

임의 개수의 개체를 렌더링할 수는 `CCRenderTexture` 사이의 해당 `Begin` 및 `End` 호출 합니다. 렌더링 하기 전에 필요한 모든 표시 개체는 자식으로 추가 됩니다.


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

`renderTexture` 되어서는 안 카드의를 렌더링할 때 제거 됩니다.


```csharp
// We don't want the render texture to be a child of the card 
// when we call Visit
if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
{
    this.RemoveChild(renderTexture.Sprite);
}
```

이제는 `Card` 인스턴스 자체에 렌더링할 수는 `CCRenderTexture` 인스턴스:


```csharp
// Clears the CCRenderTexture
renderTexture.BeginWithClear(CCColor4B.Transparent);
// Visit renders this object and all of its children
this.Visit();
// Ends the rendering, which means the CCRenderTexture's Sprite can be used
renderTexture.End();
```

개별 구성 요소가 제거 렌더링이 완료 되 면 및 `CCRenderTexture` 다시 추가 합니다.


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

이 가이드에 설명 된 `CCRenderTexture` 클래스를 사용 하 여는 `Card` 는 수집 가능한 카드 게임에 사용할 수 있는 엔터티. 사용 하는 방법에 알아보았습니다는 `CCRenderTexture` 프레임 속도 향상 시키고 엔터티 수준의 투명도 제대로 구현 클래스입니다.

이 가이드를 사용 하지만 `CCRenderTexture` 엔터티 내에 포함 된,이 클래스 수도 있고 여러 엔터티를 렌더링 하는 데 사용 전체 `CCLayer` 화면의 효과 및 성능 향상에 대 한 인스턴스.

## <a name="related-links"></a>관련 링크

- [CCRenderTexture API 참조](https://developer.xamarin.com/api/type/CocosSharp.CCRenderTexture/)
- [전체 프로젝트 (샘플)](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)
