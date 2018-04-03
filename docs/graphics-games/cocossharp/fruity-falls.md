---
title: 게임 정보 Fruity 위치 합니다.
description: 이 가이드에 일반적인 CocosSharp와 게임 디자인 물리학, 콘텐츠 관리, 게임 상태 등 게임 개발 개념을 다루는 Fruity 까지의 게임을 검토 합니다.
ms.topic: article
ms.prod: xamarin
ms.assetid: A5664930-F9F0-4A08-965D-26EF266FED24
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: d37b289249e5c9e2c23b45c998d1e24960637ba6
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2018
---
# <a name="fruity-falls-game-details"></a>게임 정보 Fruity 위치 합니다.

_이 가이드에 일반적인 CocosSharp와 게임 디자인 물리학, 콘텐츠 관리, 게임 상태 등 게임 개발 개념을 다루는 Fruity 까지의 게임을 검토 합니다._

Fruity 위치 합니다.는 간단 하 고 물리학 기반 게임 플레이어가 빨강 및 노란색 과일 점수를 얻는 색이 지정 된 버킷으로 정렬 합니다. 게임의 목적은 점수를 얻는 것 만큼 가능한 게임 종료 잘못 된 bin에 과일 놓기 수 없습니다.

![](fruity-falls-images/image1.png "게임의 목적은 점수를 얻는 것 만큼 가능한 게임 종료 잘못 된 bin에 과일 놓기 수 없습니다.")

Fruity 대체 확장에 도입 된 개념은 [BouncingGame 가이드](~/graphics-games/cocossharp/bouncing-game.md) 다음 추가:

 - Png의 형태로 콘텐츠
 - 고급 물리학
 - 게임 상태 (장면 간 전환)
 - 단일 클래스에 포함 된 변수를 통해 게임 계수를 조정 하는 기능
 - 기본 제공 시각적 디버깅 지원
 - 게임 엔터티를 사용 하 여 코드 조직
 - 재미 있고 재생 값에 초점을 맞춘 게임 디자인

동안는 [BouncingGame 가이드](~/graphics-games/cocossharp/bouncing-game.md) 을 모두 함께 게임 완제품 하는 방법을 보여 줍니다 Fruity 위치 합니다.를 중심으로 CocosSharp의 핵심 개념을 소개 합니다. 이 가이드는 BouncingGame 참조, 하므로 판독기 먼저 반드시 알아두어야는 [BouncingGame 가이드](~/graphics-games/cocossharp/bouncing-game.md) 이 가이드를 읽기 전에 합니다.

이 가이드에서는 구현과 내릴 수 있도록 게임 통찰력을 제공 Fruity의 디자인을 설명 합니다. 다음 항목을 다룹니다.


- [GameController 클래스](#gamecontroller-class)
- [게임 엔터티](#game-entities)
- [과일 그래픽](#fruit-graphics)
- [물리](#physics)
- [콘텐츠 게임](#game-content)
- [GameCoefficients](#gamecoefficients)


## <a name="gamecontroller-class"></a>GameController 클래스

Fruity 대체 PCL 프로젝트에 포함 되어는 `GameController` 게임을 인스턴스화하고 장면 간에 이동 하는 일을 담당 하는 클래스입니다. 이 클래스는 iOS 및 Android 프로젝트에서 중복 되는 코드를 제거 하기 위해 사용 됩니다.

내에 포함 된 코드는 `Initialize` 메서드는 코드에`Initialize` 하지만 변경 되지 않은 CocosSharp 서식 파일에서 메서드 수정 횟수를 포함 합니다.

기본적으로는 `GameView.ContentManager.SearchPaths` 장치의 해상도에 따라 달라 집니다. 아래에서 자세히 설명, Fruity 위치 합니다. 사용 하 여 장치 해상도 관계 없이 동일한 콘텐츠 하므로 `Initialize` 메서드 추가 `Images` 에 하위 폴더 제외) (사용 하 여 경로 `SearchPaths`:


```csharp
contentSearchPaths.Add ("Images");
```

상속 되는 클래스를 포함 하는 새 CocosSharp 템플릿을 `CCLayer`, 게임 시각적 개체 및 논리가이 클래스에 추가 되어야 함을 암시 합니다. 대신, Fruity 위치 합니다. 사용 하 여 여러 `CCLayer` 인스턴스 제어를 그리기 순서. 이러한 `CCLayer` 인스턴스에 포함 되어는 `GameView` 클래스에서 상속 하는 `CCScene`합니다. 첫 번째 되는 여러 백그라운드에서 Fruity 폭포에도 포함 되어는 `TitleScene`합니다. 따라서는 `Initialize` 메서드를 만드는 데는 `TitleScene` 에 전달 되는 인스턴스 `RunWithScene`:


```csharp
var scene = new TitleScene (GameView);
GameView.Director.RunWithScene (scene);
```

Fruity 폭포에 대 한 콘텐츠 미적인 이유로 저해상도 픽셀 아트도 만들어졌습니다. `GameView.DesignResolution` 게임은만 384 단위 너비, 높이 512 되도록 설정 됩니다.


```csharp
// We use a lower-resolution display to get a pixellated appearance
int width = 384;
int height = 512;
GameView.DesignResolution = new CCSizeI (width, height); 
```

마지막으로 `GameController` 클래스에서 호출 될 수 있는 정적 메서드를 제공 `CCGameScene` 다른 전환 `CCScene`합니다. 이 메서드는 간에 이동 하는 데는 `TitleScene` 및 `GameScene`합니다.


## <a name="game-entities"></a>게임 엔터티

Fruity 까지의에서는 대부분의 게임 개체에 대 한 엔터티 패턴 활용 합니다. 이 패턴에 대 한 자세한 내용은에서 확인할 수 있습니다는 [CocosSharp의 엔터티 안내](~/graphics-games/cocossharp/entities.md)합니다.

엔터티를 구현 하는 게임 개체에서 엔터티 폴더에서 찾을 수 있습니다는 **FruityFalls.Common** 프로젝트:

![](fruity-falls-images/image2.png "FruityFalls.Common 프로젝트에서 엔터티 폴더")

엔터티는 개체에서 상속 된 `CCNode`, 하며 시각적 개체, 충돌 및 모든 프레임 동작이 있을 수 있습니다.

`Fruit` 개체가 나타내는 일반적인 CocosSharp 엔터티: 시각적 개체, 충돌 및 모든 프레임 논리를 포함 합니다. 해당 생성자의 과일을 초기화 하는:


```csharp
public Fruit ()
{
    CreateFruitGraphic ();
    if (GameCoefficients.ShowCollisionAreas)
    {
        CreateDebugGraphic ();
    }
    CreateCollision ();
    CreateExtraPointsLabel ();
    Acceleration.Y = GameCoefficients.FruitGravity;
}
```


### <a name="fruit-graphics"></a>과일 그래픽

`CreateFruitGraphic` 메서드 만듭니다는 `CCSprite` 인스턴스 및에 추가 `Fruit`합니다. `IsAntialiased` 게임 픽셀 디자인으로 속성이 false로 설정 됩니다. 이 값은 모든에서 false로 설정 `CCSprite` 및 `CCLabel` 끝 인스턴스:


```csharp
private void CreateFruitGraphic()
{
    graphic = new CCSprite ("cherry.png");
    graphic.IsAntialiased = false;
    this.AddChild (graphic);
}
```

플레이어와 연결 될 때마다 한 `Fruit` 인스턴스는 `Paddle`, 해당 과일의 소수점 값 1 씩 증가 합니다. 이 숙련 된 플레이어 추가 점수에 대 한 과일 사용 해야 추가 챌린지를 제공 합니다. 사용 하 여 과일의 지점 값이 표시 됩니다는 `extraPointsLabel` 인스턴스.

`CreateExtraPointsLabel` 메서드 만듭니다는 `CCLabel` 인스턴스 및에 추가 `Fruit`:


```csharp
private void CreateExtraPointsLabel()
{
    extraPointsLabel = new CCLabel("", "Arial", 12, CCLabelFormat.SystemFont);
    extraPointsLabel.IsAntialiased = false;
    extraPointsLabel.Color = CCColor3B.Black;
    this.AddChild(extraPointsLabel);
}
```

Fruity 폭포에는 두 가지 종류의 과일 – 체리 및 lemons 포함 되어 있습니다. `CreateFruitGraphic` 할당을 통해 기본 visual 하지만 과일 시각적 개체를 변경할 수 있습니다는 `FruitColor` 속성을 호출 하 여 `UpdateGraphics`:


```csharp
private void UpdateGraphics()
{
if (GameCoefficients.ShowCollisionAreas)
    {
        debugGrahic.Clear ();
        const float borderWidth = 4;
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius,
            CCColor4B.Black);
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius - borderWidth,
            fruitColor.ToCCColor ());
    }
    if (this.FruitColor == FruitColor.Yellow)
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("lemon.png");
        extraPointsLabel.Color = CCColor3B.Black;
        extraPointsLabel.PositionY = 0;
    }
    else
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("cherry.png");
        extraPointsLabel.Color = CCColor3B.White;
        extraPointsLabel.PositionY = -8;
    }
}
```

첫 번째 if 문이 `UpdateGraphics` 충돌 영역을 시각화 하는 데 사용 되는 디버그 그래픽을 업데이트 합니다. 이러한 시각 효과 게임의 최종 릴리스는 하도록 만들어졌지만 물리학 디버깅 하기 위해 개발 중에 유지 될 전에 비활성화 되어 있습니다. 두 번째 파트를 변경은 `graphic.Texture` 호출 하 여 `CCTextureCache.SharedTextureCache.AddImage`합니다. 이 메서드는 파일 이름으로 질감에 대 한 액세스를 제공합니다. 이 메서드에 대 한 자세한 내용은에서 찾을 수 있습니다는 [질감 캐시 가이드](~/graphics-games/cocossharp/texture-cache.md)합니다.

`extraPointsLabel` 과일 이미지와는 반대로 변경 하지 않으려면 색 조정 됩니다 및 해당 `PositionY` 값을 조정 센터로 `CCLabel` 과일의에서 `CCSprite`:

![](fruity-falls-images/image3.png "과일 이미지와는 반대로 변경 하지 않으려면 extraPointsLabel 색 조정 되 고 PositionY 값은 CCSprite 과일에 CCLabel 가운데 맞춤")

![](fruity-falls-images/image4.png "과일 이미지와는 반대로 변경 하지 않으려면 extraPointsLabel 색 조정 되 고 PositionY 값은 CCSprite 과일에 CCLabel 가운데 맞춤")


### <a name="collision"></a>충돌

Fruity 까지의 Geometry 폴더에 있는 개체를 사용 하는 사용자 지정 충돌 솔루션을 구현 합니다.

![](fruity-falls-images/image5.png "개체를 사용 하 여 기 하 도형 폴더에는 사용자 지정 충돌 솔루션을 구현 하는 Fruity 위치 합니다.")

Fruity 폭포에는 충돌 중 하나를 사용 하 여 구현 되는 `Circle` 또는 `Polygon` 일반적으로 엔터티의 자식으로 추가 되 고 이러한 두 가지 형식 중 하나가 지정 된 개체입니다. 예를 들어는 `Fruit` 개체에는 `Circle` 호출 `Collision` 에서 초기화 하는 해당 `CreateCollision` 메서드:


```csharp
private void CreateCollision()
{
    Collision = new Circle ();
    Collision.Radius = GameCoefficients.FruitRadius;
    this.AddChild (Collision);
}
```

`Circle` 클래스에서 상속는 `CCNode` 클래스를 다루는 게임 엔터티에 자식으로 추가할 수 있습니다.


```csharp
public class Circle : CCNode
{
    ...
}
```

`Polygon` 만들기는 비슷합니다 `Circle` 만들기, 에서처럼는 `Paddle` 클래스:


```csharp
private void CreateCollision()
{
    Polygon = Polygon.CreateRectangle(width, height);
    this.AddChild (Polygon);
}
```

검사 하는 충돌 논리 [이 가이드의 뒷부분에 나오는](#collision)합니다.


## <a name="physics"></a>물리

물리 Fruity 폭포에 두 가지 범주로 구분할 수 있습니다: 이동 및 충돌 합니다. 


### <a name="movement-using-velocity-and-acceleration"></a>개발 속도 가속을 사용 하 여 이동

Fruity 위치 합니다. 사용 하 여 `Velocity` 및 `Acceleration` 를 해당 엔터티의 움직임을 제어 하는 값은 [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)합니다. 라는 메서드에서 이동 논리를 구현 하는 엔터티 `Activity`는 프레임 마다 한 번씩 호출 됩니다. 예를 들어 이동 구현을 볼 수 있습니다는 `Fruit` 클래스의 `Activity` 메서드:

```csharp
public void Activity(float frameTimeInSeconds)
{
    timeUntilExtraPointsCanBeAdded -= frameTimeInSeconds;
    
    // linear approximation:
    this.Velocity += Acceleration * frameTimeInSeconds;

    // This is a linear approximation to drag. It's used to
    // keep the object from falling too fast, and eventually
    // to slow its horizontal movement. This makes the game easier
    this.Velocity -= Velocity * GameCoefficients.FruitDrag * frameTimeInSeconds;
    
    this.Position += Velocity * frameTimeInSeconds;
}
```
`Fruit` 입니다. 즉 얼마나 빨리 과일을 기준으로 속도 저하 하는 값을 이동 하는 개체는 끌어서 구현 내에서 고유 합니다. 이 구현은 끌어서의 제공는 *터미널 속도*, 과일 인스턴스 수를 최대 속도 합니다. 또한 끌기의 재생을 더 쉽게 게임을 낮추는 과일 수평 이동 속도가 느려집니다.

`Paddle` 개체도 적용 됩니다. `Velocity` 에 해당 `Activity` 메서드를 했지만 해당 `Velocity` 사용 하 여 계산 되는 `desiredLocation` 값:


```csharp
public void Activity(float frameTimeInSeconds)
{
    // This code will cause the cursor to lag behind the touch point
    // Increasing this value reduces how far the paddle lags
    // behind the player’s finger. 
    const float velocityCoefficient = 4;
    // Get the velocity from current location and touch location
    Velocity = (desiredLocation - this.Position) * velocityCoefficient;
    this.Position += Velocity * frameTimeInSeconds;
    ...
}
```

일반적으로 사용 하는 게임 `Velocity` 자신의 개체 do의 위치를 직접 제어를 적어도 초기화 한 다음 엔터티 위치를 설정 합니다. 직접 설정 하지 않고는 `Paddle` 위치는 `Paddle.HandleInput` 메서드 할당은 `desiredLocation` 값:

```csharp
public void HandleInput(CCPoint touchPoint)
{
    desiredLocation = touchPoint;
}
```

### <a name="collision"></a>충돌

와 같은 구현 하 여 과일 및 기타 collidable 개체 간의 반 현실적인 충돌 Fruity 위치 합니다.는 `Paddle` 및 `GameScene.Splitter`합니다. 충돌을 디버깅 하려면 Fruity 까지의 충돌 영역 표시할 수 있습니다 변경 하 여는 `GameCoefficients.ShowDebugInfo` 에 `GameCoefficients.cs` 파일:


```csharp
public static class GameCoefficients
{
    ... 
    public const bool ShowCollisionAreas = true;
    ...
}
```

이 값을 설정 `true` 충돌 영역 쓰기가 가능 합니다.  

![](fruity-falls-images/image6.png "충돌 영역 쓰기가 가능이 값을 true로 설정")

충돌 논리에서 시작 되는 `GameScene.Activity` 메서드:


```csharp
private void Activity(float frameTimeInSeconds)
{
    if (hasGameEnded == false)
    {
        paddle.Activity(frameTimeInSeconds);
        foreach (var fruit in fruitList)
        {
            fruit.Activity(frameTimeInSeconds);
        }
        spawner.Activity(frameTimeInSeconds);
        DebugActivity();
        PerformCollision();
    }
}
```

`PerformCollision` 모든 충돌 처리 `Fruit` 다른 개체를 사용 하 여 인스턴스: 


```csharp
private void PerformCollision()
{
    // reverse for loop since fruit may be destroyed:
    for(int i = fruitList.Count - 1; i > -1; i--)
    {
        var fruit = fruitList[i];
        FruitVsPaddle(fruit);
        FruitPolygonCollision(fruit, splitter.Polygon, CCPoint.Zero);
        FruitVsBorders(fruit);
        FruitVsBins(fruit);
    }
}
```

#### <a name="fruitvsborders"></a>FruitVsBorders

`FruitVsBorders` 다른 클래스에 포함 된 논리에 의존 하지 않고 충돌에 대 한 자체 논리를 수행 하는 충돌 합니다. 이러한 차이 과일와 화면의 테두리 간의 충돌 완벽 하 게 실선입니다. – 신중 하 게 패 이동 하 여 화면의 가장자리에 밀어넣을 수를 과일 수 있기 때문에 존재 합니다. 과일 패,으로 적중 될 때 화면에서 튀어 하지만 플레이어 과일을 느리게 푸시 경우 가장자리 과거와 화면 밖 이동 합니다.


```csharp
private void FruitVsBorders(Fruit fruit)
{
    if(fruit.PositionX - fruit.Radius < 0 && fruit.Velocity.X < 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionX + fruit.Radius > gameplayLayer.ContentSize.Width && fruit.Velocity.X > 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionY + fruit.Radius > gameplayLayer.ContentSize.Height && fruit.Velocity.Y > 0)
    {
        fruit.Velocity.Y *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
}
```

#### <a name="fruitvsbins"></a>FruitVsBins

`FruitVsBins` 메서드는 두 개의 저장소 중 하나에 모든 과일 떨어졌습니다 있는지를 확인 합니다. 그렇다면 플레이어는으로 점수가 (과일/bin 색 일치) 하는 경우 또는 (색이 일치 하지 않음) 하는 경우 게임이 종료:


```csharp
private void FruitVsBins(Fruit fruit)
{
    foreach(var bin in fruitBins)
    {
        if(bin.Polygon.CollideAgainst(fruit.Collision))
        {
            if(bin.FruitColor == fruit.FruitColor)
            {
                // award points:
                score += 1 + fruit.ExtraPointValue;
                scoreText.Score = score;
                Destroy(fruit);
            }
            else
            {
                EndGame();
            }
            break;
        }
    }
}
```

#### <a name="fruitvspaddle-and-fruitpolygoncollision"></a>FruitVsPaddle 및 FruitPolygonCollision

패 및 분할자 (두 개의 bin 분리 영역)와 비교 과일이 비교 과일이 충돌의 경우 둘 다는 `FruitPolygonCollision` 메서드. 이 메서드에 세 부분으로 이루어져 있습니다.

1. 개체가 충돌 하는지 여부를 테스트 합니다.
1. 더 이상 겹치게 합니다 (방금 Fruity 까지의에서 과일) 개체를 이동
1. 다음 코드에서는 위에 그려집니다 바운스 시뮬레이션 하는 개체 (방금 Fruity 까지의에서 과일)의 개발 속도 조정 합니다.


```csharp
private static bool FruitPolygonCollision(Fruit fruit, Polygon polygon, CCPoint polygonVelocity)
{
    // Test whether the fruit collides
    bool didCollide = polygon.CollideAgainst(fruit.Collision);
    if (didCollide)
    {
        var circle = fruit.Collision;
        // Get the separation vector to reposition the fruit so it doesn't overlap the polygon
        var separation = CollisionResponse.GetSeparationVector(circle, polygon);
        fruit.Position += separation;
        // Adjust the fruit's Velocity to make it bounce:
        var normal = separation;
        normal.Normalize();
        fruit.Velocity = CollisionResponse.ApplyBounce(
            fruit.Velocity, 
            polygonVelocity, 
            normal, 
            GameCoefficients.FruitCollisionElasticity);
    }
    return didCollide;
}
```

Fruity 까지의 충돌 응답은 한 면 –만 과일의 개발 속도 및 위치를 조정 합니다. 다른 게임에이 인해 푸시는 돌 또는 두 대의 자동차 충돌 서로 문자 같은 관련된 개체를 모두 충돌 있을 수 있다는 됩니다.

뒤에 포함 된 충돌 논리 수학은 `Polygon` 및 `CollisionResponse` 클래스가이 가이드의 범위를 벗어납니다. 하지만로 작성 된, 이러한 방법을 사용할 수 있는 다양 한 유형의 게임에 대 한 합니다. 다각형 및 `CollisionResponse` 클래스는도 사각형이 아닌 및 볼록 다각형을 지원 하므로 다양 한 유형의 게임에 대 한이 코드를 사용할 수 있습니다.

 


## <a name="game-content"></a>콘텐츠 게임

즉시 아트 Fruity 폭포에는 BouncingGame에서 게임을 구분합니다. 게임 디자인은 유사 하지만, 플레이어가 두 게임 표시 하는 방법의 차이 즉시 표시 됩니다. 종종 게이머 해당 시각적 개체에서 게임을 시도할 것인지 결정 합니다. 따라서 것은 개발자 시각적으로 매력적인을 만들 때의 리소스를 투자는 매우 중요 게임.

아트 Fruity 대체에 대 한 다음과 같은 목표를 만들었습니다.

 - 일관 된 테마
 - 문자 하나로 – Xamarin 원숭이에 중점을 둔
 - 만들려는 완화 즐거운 밝은 색 경험
 - 게임 플레이 개체를 쉽게 시각적으로 추적할 수 있도록 배경 및 전경 간의 대비
 - 리소스를 많이 애니메이션 없이 단순 시각 효과 만들 수 있는 기능


### <a name="content-location"></a>콘텐츠 위치

Fruity 위치 합니다. Android 프로젝트에 이미지 폴더에 모든 해당 콘텐츠를 포함:

![](fruity-falls-images/image7.png "Fruity 위치 합니다. Android 프로젝트에 이미지 폴더에 모든 내용을 포함")

같은 파일, 복제를 방지 하려면 iOS 프로젝트에 연결 된 하 고 두 프로젝트 모두에 영향을 줄 파일 등의 변경 합니다.

![](fruity-falls-images/image8.png "같은 파일, 복제를 방지 하려면 iOS 프로젝트에 연결 되 고 두 프로젝트 모두에 영향을 줄 파일 등의 변경")

콘텐츠 내에서 포함 되지 않음을 주목할는 **Ld** 또는 **Hd** 기본 CocosSharp 서식 파일의 구성 요소인 폴더입니다. **Ld** 및 **Hd** 폴더의 콘텐츠-전화 및 태블릿 같은 고해상도 장치에 대 한 같은 저해상도 장치에 대 한 두 개의 집합을 제공 하는 게임을 위한 사용 하는 데 사용 됩니다. Fruity 까지의 아트는 의도적으로 사용 하 여 만들어지므로 픽셀 미적인, 다양 한 화면 크기에 대 한 콘텐츠를 제공 하는 필요 하지 않습니다. 따라서는 **Ld** 및 **Hd** 폴더는 프로젝트에서 완전히 제거 되었습니다.


### <a name="gamescene-layering"></a>GameScene 계층화

이 가이드 앞부분에서 설명한 것 처럼는 `GameScene` 모든 게임 개체 인스턴스화, 위치 지정 및 개체 간 논리 (예: 충돌)을 담당 합니다. 모든 개체는 4 개 중 하나에 추가 됩니다 `CCLayer` 인스턴스:


```csharp
CCLayer backgroundLayer;
CCLayer gameplayLayer;
CCLayer foregroundLayer;
CCLayer hudLayer;
```

이러한 4 단계 계층에 생성 됩니다는 `CreateLayers` 메서드. 참고에 추가 되 고 `GameScene` 뒤에 순서 대로:


```csharp
private void CreateLayers()
{
    backgroundLayer = new CCLayer();
    this.AddLayer(backgroundLayer);
    gameplayLayer = new CCLayer();
    this.AddLayer(gameplayLayer);
    foregroundLayer = new CCLayer();
    this.AddLayer(foregroundLayer);
    hudLayer = new CCLayer();
    this.AddLayer(hudLayer);
}
```

모든 시각적 개체를 사용 하 여 계층에 추가 됩니다 계층을 만든 후의 `AddChild` 메서드를 에서처럼는 `CreateBackground` 메서드:


```csharp
private void CreateBackground()
{
    var background = new CCSprite("background.png");
    background.AnchorPoint = new CCPoint(0, 0);
    background.IsAntialiased = false;
    backgroundLayer.AddChild(background);
}
```


### <a name="vine-entity"></a>줄기 엔터티

`Vine` 아무런 영향을 주지 게임에 – 엔터티 내용에 고유 하 게 사용 됩니다. 20 개의 구성 된 `CCSprite` 인스턴스는 줄기 항상 화면 맨 위까지 올라가는 되도록 시행 착오 하 여 선택 하는 번호:


```csharp
public Vine ()
{
    const int numberOfVinesToAdd = 20;
    for (int i = 0; i < numberOfVinesToAdd; i++)
    {
        var graphic = new CCSprite ("vine.png");
        graphic.AnchorPoint = new CCPoint(.5f, 0);
        graphic.PositionY = i * graphic.ContentSize.Height;
        this.AddChild (graphic);
    }
}
```

첫 번째 `CCSprite` 아래쪽 가장자리는 줄기의 위치와 일치 하도록 배치 됩니다. AnchorPoint를 설정 하 여 이렇게 `new CCPoint(.5f, 0)`합니다. `AnchorPoint` 속성 사용 *좌표 정규화* 0과 1의 크기에 관계 없이 어떤 범위는 `CCSprite`:

![](fruity-falls-images/image9.png "AnchorPoint 속성은 정규화 된 좌표를 범위는 0과 1은 CCSprite 크기에 관계 없이 사용")

사용 하 여 후속 줄기 스프라이트 배치는 `graphic.ContentSize.Height` 이미지의 높이 픽셀 단위로 반환 하는 값입니다. 다음 다이어그램은 줄기 그래픽 위쪽으로 배열 하는 방법을 보여 줍니다.

![](fruity-falls-images/image10.png "이 다이어그램에서는 줄기 그래픽을 위쪽으로 타일을 보여 줍니다.")

시작 이후는 `Vine` 엔터티는 줄기 맨 아래에 다음에 배치 하는 것은 간단에 표시 된 대로 `Paddle.CreateVines` 메서드:


```csharp
private void CreateVines()
{
    // Increasing this value moves the vines closer to the 
    // center of the paddle.
    const int vineDistanceFromEdge = 4;
    leftVine = new Vine ();
    var leftEdge = -width / 2.0f;
    leftVine.PositionX = leftEdge + vineDistanceFromEdge;
    this.AddChild (leftVine);
    rightVine = new Vine ();
    var rightEdge = width / 2.0f;
    rightVine.PositionX = rightEdge - vineDistanceFromEdge;
    this.AddChild (rightVine);
}
```

줄기 인스턴스에 `Paddle` 엔터티도 흥미로운 회전 동작을 내포 합니다. 이후는 `Paddle` 플레이어 입력 (이 가이드에서 아래에서 자세히 설명)를 따라 회전 전체 엔터티는 `Vine` 인스턴스는이 회전에 의해 영향을 주었습니다. 그러나 회전는 `Vine` 와 함께 인스턴스는 `Paddle` 다음 애니메이션에 표시 된 대로 visual 악영향을 생성 합니다.

![](fruity-falls-images/image11.gif "그러나 패 들 함께 줄기 인스턴스 회전의 결과 바람직하지 않은 시각 효과 다음 애니메이션에 표시 된 대로")

막기 위한 것은 `Paddle` 회전은 `leftVine` 및 `rightVine` 인스턴스는 회전에 표시 된 대로 패 들의 반대 방향은 `Paddle.Activity` 메서드:


```csharp
public void Activity(float frameTimeInSeconds)
{
    ...
    // This value adds a slight amount of rotation
    // to the vine. Having a small amount of tilt looks nice.
    float vineAngle = this.Velocity.X / 100.0f;
    leftVine.Rotation = -this.Rotation + vineAngle;
    rightVine.Rotation = -this.Rotation + vineAngle;
}
```

적은 양의 회전 통해 줄기에 다시 추가 됩니다는 `vineAngle` 계수입니다. vines 회전 얼마나 조정 하려면이 값을 변경할 수 있습니다.


## <a name="gamecoefficients"></a>GameCoefficients

Fruity 까지의 라는 클래스를 포함 하므로 좋은 모든 게임은 반복 제품 `GameCoefficients` 를 게임을 실행 하는 방법을 제어 합니다. 이 클래스에서 게임 제어 물리학, 레이아웃, 생성, 및 점수 매기기에 사용 되는 표현 변수를 포함 합니다.

예를 들어 과일 물리학 다음 변수에 의해 제어 됩니다.


```csharp
public static class GameCoefficients
{
    ...
    
    // The strength of the gravity. Making this a 
    // smaller (bigger negative) number will make the
    // fruit fall faster. A larger (smaller negative) number
    // will make the fruit more floaty.
    public const float FruitGravity = -60;
    // The impact of "air drag" on the fruit, which gives
    // the fruit terminal velocity (max falling speed) and slows
    // the fruit's horizontal movement (makes the game easier)
    public const float FruitDrag = .1f;
    // Controls fruit collision bouncyness. A value of 1
    // means no momentum is lost. A value of 0 means all
    // momentum is lost. Values greater than 1 create a spring-like effect
    public const float FruitCollisionElasticity = .5f;
    
    ...
}
```

폭포, 가로 얼마나 빨리 과일을 조정 하려면 이러한 변수를 사용할 수 이름에서 알 수 있듯이 패 바운스 고 시간에 따라 이동 느려집니다.

코드 또는 데이터 파일 (예: XML)에 저장 된 게임 계수 프로그래머가 아닌 게임 디자이너와 함께 팀에 대 한 특히 유용할 수 있습니다.

`GameCoefficients` 클래스 정보 또는 충돌 영역을 생성 하는 등 디버그 정보를 설정 하는 것에 대 한 값이 포함 되어 있습니다.


```csharp
public static class GameCoefficients
{
    ...
    // This controls whether debug information is displayed on screen.
    public const bool ShowDebugInfo = false;
    public const bool ShowCollisionAreas = false;
    ...
}
```


## <a name="conclusion"></a>결론

이 가이드 Fruity 까지의 게임을 탐색 합니다. 것 콘텐츠, 물리학 게임 상태 관리를 비롯 한 개념을 다룹니다.

## <a name="related-links"></a>관련된 링크

- [CocosSharp API 설명서](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [완료 된 프로젝트 (샘플)](https://developer.xamarin.com/samples/mobile/FruityFalls/)
