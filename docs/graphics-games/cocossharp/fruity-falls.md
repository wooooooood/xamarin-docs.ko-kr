---
title: Fruity Falls 게임 세부 정보
description: 이 가이드에 일반적인 CocosSharp와 물리학, 콘텐츠 관리, 게임 상태 게임 디자인 등 게임 개발 개념을 다루는 Fruity 됩니다 게임을 검토 합니다.
ms.prod: xamarin
ms.assetid: A5664930-F9F0-4A08-965D-26EF266FED24
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 959f5eb149ad375d686b17a85eb3d3b8fbdf3659
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114251"
---
# <a name="fruity-falls-game-details"></a>Fruity Falls 게임 세부 정보

_이 가이드에 일반적인 CocosSharp와 물리학, 콘텐츠 관리, 게임 상태 게임 디자인 등 게임 개발 개념을 다루는 Fruity 됩니다 게임을 검토 합니다._

Fruity Falls는 간단 하 고 물리학 기반 게임 플레이어 점수를 얻는 색이 지정 된 버킷으로 빨간색 및 노란색 과일을 정렬 합니다. 게임의 목표 포인트가 만큼 가능한 게임 끝에 잘못 된 bin에 과일 삭제 수입니다.

![](fruity-falls-images/image1.png "게임의 목표 점수를 얻는 것 만큼 가능한 게임 끝에 잘못 된 bin에 과일 삭제 수")

Fruity Falls의 개념을 확장 합니다 [BouncingGame 가이드](~/graphics-games/cocossharp/bouncing-game.md) 다음을 추가 하 여:

 - Png로의 형태로 콘텐츠
 - 고급 물리학
 - 게임 상태 (장면 사이 전환)
 - 단일 클래스에 포함 된 변수를 통해 게임 계수를 조정 하는 기능
 - 기본적으로 시각적 디버깅 지원
 - 게임 엔터티를 사용 하 여 코드 구성
 - 재미 있고 재생 값에 초점을 맞춘 게임 디자인

하지만 합니다 [BouncingGame 가이드](~/graphics-games/cocossharp/bouncing-game.md) 으로 완성된 된 게임 제품에 모두 함께 가져오는 방법을 보여 줍니다 Fruity 됩니다 CocosSharp의 핵심 개념을 소개에 초점입니다. 이 가이드는 BouncingGame를 참조 하므로 판독기 먼저 반드시 알아두어야 합니다 [BouncingGame 가이드](~/graphics-games/cocossharp/bouncing-game.md) 이 가이드를 읽기 전에 합니다.

이 가이드에서는 Fruity 됩니다 디자인 정보를 활용 한 나만의 게임을 내릴 수 있도록 제공 하 고 구현을 설명 합니다. 다음 항목을 다룹니다.


- [GameController 클래스](#gamecontroller-class)
- [게임 엔터티](#game-entities)
- [과일 그래픽](#fruit-graphics)
- [물리학](#physics)
- [콘텐츠 게임](#game-content)
- [GameCoefficients](#gamecoefficients)


## <a name="gamecontroller-class"></a>GameController 클래스

Fruity 속하는 PCL 프로젝트에는 `GameController` 게임을 인스턴스화하고 장면 사이 이동 하는 일을 담당 하는 클래스입니다. 이 클래스는 iOS 및 Android 프로젝트에서 코드 중복을 제거 하기 위해 사용 됩니다.

내에 포함 된 코드를 `Initialize` 메서드는 코드와 비슷합니다는`Initialize` 메서드는 변경 되지 않은 CocosSharp 템플릿을 하지만에서 수정 횟수를 포함 합니다.

기본적으로 `GameView.ContentManager.SearchPaths` 장치의 해상도에 따라 달라 집니다. Fruity 됩니다 장치 해상도 관계 없이 동일한 콘텐츠를 사용 하 여에서 설명한 대로 아래 자세히 하므로 `Initialize` 메서드를 추가 합니다 `Images` (사용 하 여 하위 폴더가 없으므로) 경로 `SearchPaths`:


```csharp
contentSearchPaths.Add ("Images");
```

상속 하는 클래스를 포함 하는 새 CocosSharp 템플릿 `CCLayer`,이 클래스에 게임 시각적 개체 및 논리를 추가할 수 있는지를 의미 합니다. Fruity 됩니다 여러를 사용 하는 대신 `CCLayer` 인스턴스 제어를 그리기 순서입니다. 이러한 `CCLayer` 내에 포함 된 인스턴스를 `GameView` 클래스에서 상속 되는 `CCScene`합니다. Fruity Falls 또한 첫 번째은 여러 작업을 `TitleScene`입니다. 따라서 합니다 `Initialize` 메서드를 인스턴스화하는 `TitleScene` 에 전달 되는 인스턴스 `RunWithScene`:


```csharp
var scene = new TitleScene (GameView);
GameView.Director.RunWithScene (scene);
```

Fruity 됩니다에 대 한 콘텐츠는 시각적인 이유로 저해상도 픽셀 예술으로 생성 되었습니다. `GameView.DesignResolution` 게임 너비는 384 단위 이며만 512 설치 되도록 설정 됩니다.


```csharp
// We use a lower-resolution display to get a pixellated appearance
int width = 384;
int height = 512;
GameView.DesignResolution = new CCSizeI (width, height); 
```

마지막으로, 합니다 `GameController` 클래스에서 호출할 수 있는 정적 메서드를 제공 `CCGameScene` 전환 다른 `CCScene`합니다. 사이 이동 하려면이 메서드는 사용 된 `TitleScene` 하며 `GameScene`합니다.


## <a name="game-entities"></a>게임 엔터티

Fruity Falls에서는 대부분의 게임 개체에 대 한 엔터티 패턴을 활용 합니다. 이 패턴에 대 한 자세한 설명은에서 찾을 수 있습니다 합니다 [CocosSharp의 엔터티 가이드](~/graphics-games/cocossharp/entities.md)합니다.

엔터티 폴더에서 엔터티를 구현 하는 게임 개체를 찾을 수 있습니다 합니다 **FruityFalls.Common** 프로젝트:

![](fruity-falls-images/image2.png "FruityFalls.Common 프로젝트에서 엔터티 폴더")

엔터티는 개체에서 상속 하는 `CCNode`, 시각적 개체, 충돌 및 모든 프레임 동작 해야 할 수 있습니다.

`Fruit` 개체 일반적인 CocosSharp 엔터티를 나타냅니다.: 시각적 개체, 충돌 및 프레임의 모든 논리를 포함 합니다. 생성자는 과일을 초기화 하는 일을 담당 합니다.


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

`CreateFruitGraphic` 메서드를 만듭니다를 `CCSprite` 인스턴스를 추가 하는 데는 `Fruit`합니다. `IsAntialiased` 모양을에 게임을 잡아 속성이 false로 설정 됩니다. 이 값은 모두 false로 설정 `CCSprite` 고 `CCLabel` 게임 전체 인스턴스:


```csharp
private void CreateFruitGraphic()
{
    graphic = new CCSprite ("cherry.png");
    graphic.IsAntialiased = false;
    this.AddChild (graphic);
}
```

플레이어와 연결 될 때마다를 `Fruit` 인스턴스는 `Paddle`, 해당 과일 지점 값 1 씩 증가 합니다. 추가적인 도전을 요구할 추가 지점에 대 한 과일을 다루어야 하는 숙련 된 플레이어를 제공 합니다. 사용 하 여 과일의 지점 값이 표시 됩니다는 `extraPointsLabel` 인스턴스.

합니다 `CreateExtraPointsLabel` 메서드를 만듭니다를 `CCLabel` 인스턴스를 추가 하는 데는 `Fruit`:


```csharp
private void CreateExtraPointsLabel()
{
    extraPointsLabel = new CCLabel("", "Arial", 12, CCLabelFormat.SystemFont);
    extraPointsLabel.IsAntialiased = false;
    extraPointsLabel.Color = CCColor3B.Black;
    this.AddChild(extraPointsLabel);
}
```

Fruity Falls 과일 – 체리 및 lemons 두 가지 유형이 포함 되어 있습니다. 합니다 `CreateFruitGraphic` 할당을 통해 기본 시각적 개체를 하지만 과일 시각적 개체를 변경할 수 있습니다 합니다 `FruitColor` 속성을 호출 하는 `UpdateGraphics`:


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

첫 번째 경우 문에서 `UpdateGraphics` 충돌 영역을 시각화 하는 데 사용 되는 디버그 그래픽을 업데이트 합니다. 이러한 시각적 개체 전에 꺼져 게임의 최종 릴리스를 했지만 물리학을 디버깅 하기 위해 개발 중 유지할 수 있습니다. 두 번째 파트를 변경 합니다 `graphic.Texture` 를 호출 하 여 속성 `CCTextureCache.SharedTextureCache.AddImage`합니다. 이 메서드는 파일 이름으로 질감에 대 한 액세스를 제공합니다. 이 메서드에 대 한 자세한 정보를 찾을 수 있습니다 합니다 [텍스처 캐싱 가이드](~/graphics-games/cocossharp/texture-cache.md)합니다.

합니다 `extraPointsLabel` 색 대비 과일 이미지와 함께 유지 하도록 조정 됩니다 및 해당 `PositionY` 값을 조정 센터로 합니다 `CCLabel` 과일의에서 `CCSprite`:

![](fruity-falls-images/image3.png "ExtraPointsLabel 색 대비 과일 이미지를 사용 하 여 유지 하기 위해 조정 하 고 해당 PositionY 값 CCSprite 과일에 CCLabel 가운데 조정 됩니다.")

![](fruity-falls-images/image4.png "ExtraPointsLabel 색 대비 과일 이미지를 사용 하 여 유지 하기 위해 조정 하 고 해당 PositionY 값 CCSprite 과일에 CCLabel 가운데 조정 됩니다.")


### <a name="collision"></a>충돌

Fruity Falls 기 하 도형 폴더에 개체를 사용 하는 사용자 지정 충돌 솔루션을 구현 합니다.

![](fruity-falls-images/image5.png "Fruity Falls 기 하 도형 폴더에 개체를 사용 하는 사용자 지정 충돌 솔루션 구현")

Fruity 됩니다에서 충돌 중 하나를 사용 하 여 구현 되는 `Circle` 또는 `Polygon` 엔터티의 자식으로 추가 되 고 이러한 두 형식 중 하나를 사용 하 여 일반적으로 개체입니다. 예를 들어, 합니다 `Fruit` 개체에는 `Circle` 호출 `Collision` 초기화 하는 해당 `CreateCollision` 메서드:


```csharp
private void CreateCollision()
{
    Collision = new Circle ();
    Collision.Radius = GameCoefficients.FruitRadius;
    this.AddChild (Collision);
}
```

`Circle` 클래스에서 상속을 `CCNode` 클래스, 게임의 엔터티를 자식으로 추가할 수 있습니다.


```csharp
public class Circle : CCNode
{
    ...
}
```

`Polygon` 만들기는 비슷합니다 `Circle` 에 표시 된 대로 생성의 `Paddle` 클래스:


```csharp
private void CreateCollision()
{
    Polygon = Polygon.CreateRectangle(width, height);
    this.AddChild (Polygon);
}
```

충돌 논리 가려집니다 [이 가이드의 뒷부분에 나오는](#collision)합니다.


## <a name="physics"></a>물리학

Fruity 됩니다에서 물리학 두 가지 범주로 구분할 수 있습니다: 이동 및 충돌 합니다. 


### <a name="movement-using-velocity-and-acceleration"></a>개발 속도 및 가속을 사용 하 여 이동

Fruity Falls 사용 `Velocity` 하 고 `Acceleration` 비슷합니다 해당 엔터티의 움직임을 제어 하는 값을 [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md). 라는 메서드에서 해당 이동 논리를 구현 하는 엔터티 `Activity`, 하는 값은 프레임 마다 한 번씩 호출 됩니다. 예를 들어, 이동 구현의 보면 합니다 `Fruit` 클래스의 `Activity` 메서드:

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
`Fruit` 개체가 끌어서 구현에서 고유-과일을 속도 관련 하 여 개발 속도 저하 하는 값은 이동 됩니다. 이 구현의 끌어서 제공을 *터미널 속도*, 과일 인스턴스를 해당 하는 최대 속도 합니다. 또한 끌어서 쉽게 게임 약간 재생 하는 과일의 수평 이동 속도가 느려집니다.

`Paddle` 개체도 적용 됩니다 `Velocity` 에서 해당 `Activity` 메서드를 했지만 해당 `Velocity` 사용 하 여 계산 되는 `desiredLocation` 값:


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

일반적으로 사용 하는 게임 `Velocity` 직접적 해당 개체 수행의 위치를 제어 하려면 초기화 후 적어도 엔터티 위치를 설정 합니다. 직접 설정 하는 대신 합니다 `Paddle` 위치를 `Paddle.HandleInput` 메서드 할당을 `desiredLocation` 값:

```csharp
public void HandleInput(CCPoint touchPoint)
{
    desiredLocation = touchPoint;
}
```

### <a name="collision"></a>충돌

Fruity Falls 같은 과일 및 기타 collidable 개체 간의 반 현실적인 충돌을 구현 합니다 `Paddle` 및 `GameScene.Splitter`합니다. 충돌을 디버깅을 위해 Fruity 됩니다 충돌 영역 표시할 수를 변경 하 여 합니다 `GameCoefficients.ShowDebugInfo` 에 `GameCoefficients.cs` 파일:


```csharp
public static class GameCoefficients
{
    ... 
    public const bool ShowCollisionAreas = true;
    ...
}
```

이 값을 설정 `true` 충돌 영역 쓰기가 가능 합니다.  

![](fruity-falls-images/image6.png "충돌 영역의 렌더링을 사용 하면이 값을 true로 설정")

충돌 논리에서 시작 된 `GameScene.Activity` 메서드:


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

`PerformCollision` 모든 충돌 처리 `Fruit` 다른 개체와 관련 된 인스턴스: 


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

`FruitVsBorders` 다른 클래스에 포함 된 논리에 의존 하는 것이 아니라 충돌에 대 한 자체 논리를 수행 하는 충돌 합니다. 이러한 차이 과일 화면의 테두리 사이의 충돌을 완벽 하 게 견고한 아닙니다 – 과일을 신중 하 게 패 들 이동 하 여 화면의 가장자리에 밀어넣을 수 있기 때문에 존재 합니다. 과일, 패 들을 사용 하 여 적중 될 때 화면에서 바운스 됩니다 하지만 플레이어는 느린 과일 푸시 경우 가장자리를 넘어 및 화면에서 벗어났는지 이동 됩니다.


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

`FruitVsBins` 메서드는 두 개의 bin 중 하나로 모든 과일 떨어졌습니다 있는지 확인 하는 일을 담당 합니다. 그렇다면 플레이어는 점수 (과일/bin 색 일치) 하는 경우 또는 (색이 일치 하지 않음) 하는 경우 게임이 종료 하는 것:


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

패 들 및 과일을 분할 (두 개의 bin 구분 영역) 및 비교 과일 둘 다 사용 하는 충돌을 `FruitPolygonCollision` 메서드. 이 메서드는 세 부분으로 구성 합니다.

1. 개체가 충돌 하는지 여부를 테스트 합니다.
1. 더 이상 겹치게 합니다 (방금 Fruity 됩니다에서 과일) 개체를 이동
1. 다음 코드에서는 위에서 작성 한 포인트는 반송을 시뮬레이션 하기 위해 개체 (방금 Fruity 됩니다에서 과일)의 속도 조정 합니다.


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

Fruity Falls 충돌 응답은 편향 적 –만 과일의 속도 위치가 조정 됩니다. 다른 게임 푸시를 볼더의 또는 두 대의 자동차 충돌 서로 문자와 같은 관련 된 개체를 모두에 영향을 줍니다 충돌이 있을 주목할 만한 가치가 있습니다.

에 포함 된 충돌 논리 뒤 수학 합니다 `Polygon` 및 `CollisionResponse` 클래스가이 가이드의 범위를 벗어납니다 하지만로 작성 된, 이러한 방법을 사용할 수 있는 다양 한 유형의 게임에 대 한 합니다. 다각형 및 `CollisionResponse` 클래스는도 사각형이 아닌 및 볼록 다각형을 지원 하므로 다양 한 유형의 게임에 대 한이 코드를 사용할 수 있습니다.

 


## <a name="game-content"></a>콘텐츠 게임

즉시 Fruity 됩니다에서 아트를 BouncingGame에서 게임을 구분합니다. 게임 디자인은 유사 하지만, 플레이어가 두 게임 표시 되는 방법의 차이 즉시 표시 됩니다. 종종 게이머 해당 시각적 개체에서 게임을 시도할 것인지 결정 합니다. 따라서 것은 매우 중요 한 개발자 리소스를 만드는 시각적으로 뛰어난 투자는 게임입니다.

Fruity 됩니다에 대 한 아트 다음과 같은 목적으로 만들어졌습니다.

 - 일관적인 테마
 - 하나의 문자 – Xamarin monkey 강조
 - 밝은 색을 완화 즐겁게 만드는 환경
 - 게임 플레이 개체는 시각적으로 추적 하려면이 간편 하므로 배경색 및 전경색 간의 대조
 - 리소스 집약적 애니메이션 없이 간단한 시각적 효과를 만들 수 있는 기능


### <a name="content-location"></a>콘텐츠 위치

Fruity Falls Android 프로젝트에 이미지 폴더 콘텐츠를 모두 포함합니다.

![](fruity-falls-images/image7.png "Fruity Falls Android 프로젝트에 이미지 폴더에 콘텐츠를 모두 포함")

중복을 방지 하려면 iOS 프로젝트에 연결 된 이러한 동일한 파일 및 파일에 따라서 변경 두 프로젝트 모두에 영향을 줄:

![](fruity-falls-images/image8.png "이러한 동일한 파일 중복을 방지 하려면 iOS 프로젝트에 연결 되 고 두 프로젝트 모두에 영향을 줄 파일에 따라서 변경")

콘텐츠 내에서 포함 되지 않은 주목할 만한 가치가 합니다 **Ld** 또는 **Hd** 기본 CocosSharp 템플릿의 일부인 폴더입니다. 합니다 **Ld** 하 고 **Hd** 두 저해상도 장치, 휴대폰 및 태블릿과 같은 더 높은 해상도 장치에 대 한 예: 하나는 콘텐츠를 제공 하는 게임에 사용 되는 폴더는입니다. Fruity 됩니다 아트는 의도적으로 사용 하 여 만들어지므로 잡아 시각적인, 다양 한 화면 크기에 대 한 콘텐츠를 제공할 필요가 없습니다. 따라서 합니다 **Ld** 하 고 **Hd** 폴더는 프로젝트에서 완전히 제거 되었습니다.


### <a name="gamescene-layering"></a>GameScene 쌓기

이 가이드의 앞부분에서 설명 했 듯이 `GameScene` 모든 게임 개체 인스턴스화, 위치 지정 및 개체 간 논리 (예: 충돌)을 담당 합니다. 모든 개체는 네 가지 중 하나에 추가 됩니다 `CCLayer` 인스턴스:


```csharp
CCLayer backgroundLayer;
CCLayer gameplayLayer;
CCLayer foregroundLayer;
CCLayer hudLayer;
```

이러한 네 가지 계층에서 만들어집니다는 `CreateLayers` 메서드. 에 추가 되는 여 `GameScene` 뒤 순서에서:


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

모든 시각적 개체를 사용 하 여 계층에 추가 됩니다 레이어를 만든 후 합니다 `AddChild` 에 표시 된 것과 같이 메서드에 `CreateBackground` 메서드:


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

`Vine` 콘텐츠에 대 한 엔터티는 고유 하 게 – 영향이 게임 플레이 더 합니다. 20 개의 구성 된 `CCSprite` 는 줄기는 항상 화면의 맨 신념 시행 착오가 선택한 여러 인스턴스:


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

첫 번째 `CCSprite` 아래쪽 가장자리를 줄기의 위치를 일치 하도록 배치 됩니다. AnchorPoint를 설정 하 여 이렇게 `new CCPoint(.5f, 0)`합니다. 합니다 `AnchorPoint` 속성에서 사용 하 *좌표를 정규화* 범위는 0과 1의 크기에 관계 없이 `CCSprite`:

![](fruity-falls-images/image9.png "AnchorPoint 속성 범위는 0과 1의 CCSprite의 크기에 관계 없이 정규화 된 좌표를 사용")

후속 줄기 스프라이트를 사용 하 여 배치를 `graphic.ContentSize.Height` 이미지의 높이를 픽셀 단위로 반환 하는 값입니다. 다음 다이어그램은 줄기 그래픽 타일 상향 하는 방법을 보여줍니다.

![](fruity-falls-images/image10.png "이 다이어그램에서는 줄기 그래픽 상향 타일을 보여 줍니다.")

원점부터 합니다 `Vine` 엔터티는 줄기 맨 아래에 다음에 배치 하는 것은 간단에 표시 된 대로 `Paddle.CreateVines` 메서드:


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

인스턴스는 줄기는 `Paddle` 엔터티 수도 흥미로운 회전 동작입니다. 하므로 합니다 `Paddle` 전체적으로 엔터티 (이 가이드에서는 아래에 자세히 설명)는 플레이어 입력에 따라 회전를 `Vine` 인스턴스 또한이 회전 영향을 받는 합니다. 그러나 회전를 `Vine` 와 함께 인스턴스는 `Paddle` 다음 애니메이션에 표시 된 것과 같이 바람직하지 않은 시각적 효과 생성 합니다.

![](fruity-falls-images/image11.gif "그러나 회전 패 들과 함께 줄기 인스턴스 생성 바람직하지 않은 시각 효과 다음 애니메이션에 표시 된 대로")

막기 위한 것을 `Paddle` 회전 합니다 `leftVine` 및 `rightVine` 인스턴스는 회전 에서처럼 패 들의 반대 방향은 `Paddle.Activity` 메서드:


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

적은 양의 회전 통해 줄기에 다시 추가 되는 `vineAngle` 계수입니다. Vines를 회전 하는 정도 조정 하려면이 값을 변경할 수 있습니다.


## <a name="gamecoefficients"></a>GameCoefficients

Fruity 됩니다 라는 클래스를 포함 하므로 모든 적절 한 게임은 반복의 제품 `GameCoefficients` 게임을 실행 하는 방법을 제어 합니다. 이 클래스는 컨트롤 물리학, 레이아웃, 생성 및 점수 매기기에 게임에서 사용 되는 표현 변수를 포함 합니다.

예를 들어, 과일 물리학 다음 변수를 통해 제어 됩니다.


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

가로 됩니다 속도 과일을 조정 하려면 이러한 변수를 사용할 수는 이름에서 알 수 있듯이 이동 시간 및 패 들 바운스 느려집니다.

코드 또는 데이터 파일 (예: XML)에 저장 된 게임 계수는 또한 프로그래머가 아닌는 게임 디자이너를 사용 하 여 팀에 대 한 특히 유용할 수 있습니다.

`GameCoefficients` 클래스 정보 또는 충돌 영역을 생성 하는 등 디버그 정보를 설정 하는 값이 포함 되어 있습니다.


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

이 가이드는 Fruity 됩니다 게임을 살펴봅니다. 이 콘텐츠, 물리, 게임 상태 관리를 비롯 한 개념을 설명 합니다.

## <a name="related-links"></a>관련 링크

- [CocosSharp API 설명서](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [완성 된 프로젝트 (샘플)](https://developer.xamarin.com/samples/mobile/FruityFalls/)
