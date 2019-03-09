---
title: CocosSharp의 엔터티
description: 엔터티 패턴은 게임 코드를 구성 하는 강력한 방법입니다. 가독성 향상, 코드를 보다 쉽게 유지 관리 및 부모/자식 기본 제공 기능을 활용 합니다.
ms.prod: xamarin
ms.assetid: 1D3261CE-AC96-4296-8A53-A76A42B927A8
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 42034261c374183346c8072eb42014f43a4fe22c
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667609"
---
# <a name="entities-in-cocossharp"></a>CocosSharp의 엔터티

_엔터티 패턴은 게임 코드를 구성 하는 강력한 방법입니다. 가독성 향상, 코드를 보다 쉽게 유지 관리 및 부모/자식 기본 제공 기능을 활용 합니다._

엔터티 패턴 향상 된 코드는 조직을 통해 CocosSharp 사용 하 여 개발자의 작업을 개선할 수 있습니다. 이 연습에서는 두 엔터티 – 배송 엔터티와 글머리 기호 엔터티를 만드는 방법을 보여 주는 예제를 실습 됩니다. 이러한 엔터티는 자체 포함 된 개체 일 즉, 한 번 인스턴스화되는 자동으로 렌더링할 하며 해당 형식에 대 한 적절 하 게 이동 논리를 수행 합니다. 

이 가이드에서는 다음 항목을 다룹니다.

 - 게임 엔터티 소개
 - 특정 엔터티 형식 및 일반
 - 프로젝트 설정
 - 엔터티 클래스 만들기
 - 엔터티 인스턴스를 추가 합니다 `GameLayer`
 - 엔터티 논리에 대응 합니다 `GameLayer`

완료 된 게임은 다음과 같습니다.

![](entities-images/image1.png "완료 된 게임 다음과 같이 보입니다.")


## <a name="introduction-to-game-entities"></a>게임 엔터티 소개

게임 엔터티는 렌더링, 충돌, 물리, 인공 지능 논리를 필요로 하는 개체를 정의 하는 클래스입니다. 다행 스럽게도 게임의 코드 베이스에 있는 엔터티는 종종 개념적 게임 개체와 일치 합니다. True 이면 게임에 필요한 엔터티를 식별 더 쉽게 수행할 수 있습니다. 

테마가 지정 된 공백 예를 들어 [게임을 해결 하는 기](https://en.wikipedia.org/wiki/Shoot_%27em_up) 다음 엔터티를 포함할 수 있습니다.

 - `PlayerShip`
 - `EnemyShip`
 - `PlayerBullet`
 - `EnemyBullet`
 - `CollectableItem`
 - `HUD`
 - `Environment`

이러한 엔터티는 게임에서 자체 클래스와 각 인스턴스 인스턴스화 초과 거의 또는 전혀 설치 해야 합니다.


## <a name="general-vs-specific-entity-types"></a>특정 엔터티 형식 및 일반

엔터티가 시스템을 사용 하는 게임 개발자가 직면 하는 첫 번째 질문 중 하나 이며 해당 엔터티를 일반화 하려면 얼마나 많은 구현의 가장 구체적인 몇 가지 특성에 따라 다를 경우에 엔터티의 모든 형식에 대 한 클래스 정의 됩니다. 보다 일반적인 시스템 엔터티 그룹을 하나의 클래스로 결합를 사용자 지정할 수는 인스턴스를 허용 합니다.

예를 들어 다음 클래스를 정의 하는 공간 게임 이라고 생각 수 있습니다.

 - `PlayerDogfighter`
 - `PlayerBomber`
 - `EnemyMissileShip`
 - `EnemyLaserShip`

더 일반화 방법은 플레이어 제공에 대 한 단일 클래스 및 다른 운송 형식을 지원 하도록 구성할 수 있습니다는 적 출시에 대 한 단일 클래스를 만드는 것입니다. 사용자 지정 이미지를 로드 하 고 촬영, 이동 계수를 만들면 글머리 기호 엔터티의 유형을를 포함할 수 있습니다 및 AI 논리 적을 제공 합니다. 이 경우 엔터티 목록에 줄어들 수 있습니다.

 - `PlayerShip`
 - `EnemyShip`

물론, 움직임을 제어 하는 것에 대 한 인스턴스별 사용자 지정 함으로써 이러한 엔터티 형식을 더 일반화 수 있습니다. 플레이어 배송 인스턴스 적 배송 인스턴스 AI 논리를 수행할 수 있습니다 하는 동안 입력에서 읽은입니다. 즉, 엔터티를 일반화할 수는 단일 클래스에 더 합니다.

 - `Ship`

일반화는 게임 모든 엔터티에 대해 하나의 기본 클래스를 사용할 수 있습니다 더 – 계속할 수 있습니다. 호출할 수는이 단일 클래스 `GameEntity`전체 게임에서 모든 엔터티 인스턴스에 대해 사용 되는 클래스는 것, 글머리 기호 및 수집 가능한 항목 (파워업)와 같은 제공 되지 않은 엔터티를 포함 합니다.

이 시스템의 대부분의 일반 라고으로 *구성 요소가 시스템*입니다. 이러한 시스템에서는 게임 엔터티 물리학, AI, 같은 개별 구성 요소를 가질 수 있습니다 또는 동작 및 모양을 사용자 지정 하려면 추가 구성 요소를 렌더링 합니다. 순수 구성 요소 기반 시스템 유연성 최대화를 사용 하도록 설정 하 고 상속 같은 복잡 한 상속 체인을 사용 하 여 발생 하는 문제를 방지할 수 있습니다. 다른 일반화와 마찬가지로 유효한 구성 요소를 설정 하기 어려울 수 있습니다 시스템과 코드의 표현성을 줄일 수 있습니다.

사용 되는 일반화의 수준 등의 여러 고려 사항에 따라 달라 집니다.

 - 게임 크기-작은 게임 큰 게임 많은 클래스를 사용 하 여 관리 하기 어려울 수 있지만 특정 클래스를 만들려면 이용할 수 있습니다.
 - 데이터 기반 개발 – 게임 데이터 (이미지, 3D 모델 및 JSON 또는 XML과 같은 데이터 파일)에 의존 하는 엔터티 형식, 일반화 된 것에서 유용할 수 있습니다 하 고 데이터를 기반으로 세부 정보를 구성 합니다. 이 특히 개발 중 이나 게임 해제 된 후 새 콘텐츠를 추가 해야 하는 게임에 대 한 가져오기를입니다.
 - 게임 엔진 패턴-일부 게임 엔진 적극 권장 시스템 구성 요소 사용 다른 엔터티를 구성 하는 방법을 결정 하는 개발자를 허용 합니다. 개발자는 엔터티의 모든 유형을 구현 하도록 CocosSharp 구성 시스템의 사용이 필요 하지 않으므로 합니다. 

간단히 하기 위해 사용할 것는 특정 클래스 기반 접근 방식을 단일 배송 및 글머리 기호 엔터티를 사용 하 여이 자습서에 대 한 합니다.


## <a name="project-setup"></a>프로젝트 설정

이 엔터티 구현 시작 하기 전에 프로젝트를 만들려면 해야 합니다. CocosSharp 프로젝트 템플릿은 프로젝트 만들기를 간소화 하기 위해 사용할 것 것입니다. [이 게시물 확인](http://forums.xamarin.com/discussion/26822/cocossharp-project-templates-for-xamarin-studio) 템플릿 Mac 용 Visual Studio에서 CocosSharp 프로젝트 만들기에 대 한 정보에 대 한 합니다. 이 가이드의 나머지 부분에 프로젝트 이름을 사용할지 **EntityProject**합니다.

이제 프로젝트가 만들어지면 480 x 320 실행 게임의 해결 방법을 설정 합니다. 이렇게 하려면 호출 `CCScene.SetDefaultDesignResolution` 에 `GameAppDelegate.ApplicationDidFinishLaunching` 같이 메서드:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...

    // New code for resolution setting:
    CCScene.SetDefaultDesignResolution(480, 320, CCSceneResolutionPolicy.ShowAll);
    
    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);
    mainWindow.RunWithScene (scene);
} 
```

CocosSharp 해상도 사용 하 여 처리에 대 한 자세한 내용은 참조 하세요. 당사의 [CocosSharp에서 여러 해상도 처리에 대 한 가이드](~/graphics-games/cocossharp/resolutions.md)합니다.


## <a name="adding-content-to-the-project"></a>프로젝트에 콘텐츠 추가

프로젝트를 만든 후에 포함 된 파일 추가 [이 콘텐츠는 zip 파일](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)합니다. 이렇게 하려면 zip 파일을 다운로드 하 고 압축을 풉니다. 모두 추가 **ship.png** 하 고 **bullet.png** 에 **콘텐츠** 폴더입니다. **콘텐츠** 폴더 안에 속할를 **자산** 폴더에 Android 및 ios 프로젝트의 루트에 배치 됩니다. 추가 되 면에서 파일을 모두 표시 되어야 합니다 **콘텐츠** 폴더:

![](entities-images/image2.png "두 파일을 콘텐츠 폴더에 있어야 추가 되 면")


## <a name="creating-the-ship-entity"></a>배송 엔터티를 만들기

`Ship` 클래스 게임의 첫 번째 엔터티가 됩니다. 추가 하는 `Ship` 클래스를 먼저 라는 폴더를 만듭니다 **엔터티** 프로젝트의 루트 수준. 새 클래스를 추가 합니다 **엔터티** 라는 폴더 `Ship`:

![](entities-images/image3.png "배송 라는 엔터티 폴더에 새 클래스를 추가 합니다.")

첫 번째 변경을 만들어 보겠습니다 우리의 `Ship` 클래스에서 상속할 수 있도록 하는 것을 `CCNode` 클래스. `CCNode` 와 같은 일반적인 CocosSharp 클래스에 대 한 기본 클래스로 사용 됩니다 `CCSprite` 및 `CCLayer`, 및 다음 기능을 제공 합니다.

 - `Position` 화면에서 이리저리 우주선을 이동 하기 위한 속성입니다.
 - `Children` 추가 속성을 `CCSprite.`
 - `Parent` 연결에 사용할 수 있는 속성인 `Ship` 인스턴스를 다른 `CCNodes`합니다. 이 자습서의이 기능은 사용 하지 않습니다. 더 큰 게임 종종 다른 엔터티를 연결 하는 작업을 활용 하기 위해 `CCNodes`합니다. 
 - `AddEventListener` 우주선을 이동 하는 것에 대 한 입력에 응답 하기 위한 방법입니다.
 - `Schedule` 글머리 기호 해결 방법입니다.

추가 `CCSprite` 인스턴스는 배송 화면의 표시 될 수 있도록 합니다.


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Ship : CCNode
    {
        CCSprite sprite;

        public Ship () : base()
        {
            sprite = new CCSprite ("ship.png");
            // Center the Sprite in this entity to simplify
            // centering the Ship when it is instantiated
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);
        }
    }
}
```

다음으로 배송 추가한 우리의 `GameLayer` 게임에서 표시 하는지 확인 하려면:


```csharp
public class GameLayer : CCLayer
{
    Ship ship;

    public GameLayer ()
    {
        ship = new Ship ();
        ship.PositionX = 240;
        ship.PositionY = 50;
        this.AddChild (ship);
    } 
    ...
```

게임이 실행 하는 경우 이제는 배송 엔터티를 살펴봅니다.

![](entities-images/image4.png "게임을 실행 하는 경우, 배송 엔터티가 표시 됩니다.")


### <a name="why-inherit-from-ccnode-instead-of-ccsprite"></a>CCSprite 대신 CCNode에서 상속 하는 이유는?

이 시점에서 우리의 `Ship` 클래스에 대 한 간단한 래퍼인는 `CCSprite` 인스턴스. 이후 `CCSprite` 에서 상속 `CCNode`에서 직접 상속 없습니다 것 `CCSprite`, 코드 저하는 `Ship.cs`합니다. 또한에서 직접 상속 `CCSprite` 메모리 내 개체의 수를 줄이고 종속성 트리를 더 작은 함으로써 성능을 향상 시킬 수 있습니다.

상속에서는 이러한 혜택에도 불구 하 고 `CCNode` 의 일부를 숨기는 `CCSprite` 각 인스턴스의 속성입니다. 예를 들어 합니다 `Texture` 외부 속성을 수정 해서는 안 합니다 `Ship` 클래스에서 상속 하 고 `CCNode` 이 속성을 숨길 수 있습니다. Public 멤버는 엔터티의 더욱 게임을 더 큰 늘어날수록 그리고 추가 개발자 팀에 추가 될 때 특히 중요 합니다.


## <a name="adding-input-to-the-ship"></a>우주선에 대 한 입력 추가

우리의 배송 화면에 표시 되는 이제 입력을 추가 될 예정입니다. 인식이 수행 하는 방법은 비슷합니다 됩니다는 [BouncingGame 가이드](~/graphics-games/cocossharp/bouncing-game.md)의 이동에 대 한 코드를 배치할 것을 제외 하 고, 합니다 `Ship` 클래스 포함 하는 대신 `CCLayer` 또는 `CCScene`합니다.

코드를 추가 합니다 `Ship` 이동 사용자는 화면을 터치 하는 아무 곳에 나 지원 하도록 합니다.


```csharp
public class Ship : CCNode
{
    CCSprite sprite;

    CCEventListenerTouchAllAtOnce touchListener;

    public Ship () : base()
    {
        sprite = new CCSprite ("ship.png");
        // Center the Sprite in this entity to simplify
        // centering the Ship on screen
        sprite.AnchorPoint = CCPoint.AnchorMiddle;
        this.AddChild(sprite);

        touchListener = new CCEventListenerTouchAllAtOnce();
        touchListener.OnTouchesMoved = HandleInput;
        AddEventListener(touchListener, this);

    }

    private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
    {
        if(touches.Count > 0)
        {
            CCTouch firstTouch = touches[0];

            this.PositionX = firstTouch.Location.X;
            this.PositionY = firstTouch.Location.Y;
        } 
    }
} 
```

많은 문제를 해결 기 게임 구현을 기존 컨트롤러 기반 이동 모방 최대 개발 속도. 즉, 짧은 코드를 유지 하는 즉시 이동 구현 하기만 하면 됩니다.


## <a name="creating-the-bullet-entity"></a>글머리 기호 엔터티를 만들기

이 간단한 게임의 두 번째 엔터티에 글머리 기호를 표시 하는 것에 대 한 엔터티입니다. 마찬가지로 `Ship` 엔터티는 `Bullet` 엔터티에 포함 됩니다는 `CCSprite` 화면에 표시 되도록 합니다. 이동에 대 한 논리와 달리 이동;에 대 한 사용자 입력에 종속 되지 않습니다. 대신, `Bullet` 인스턴스 속도 속성을 사용 하 여 직선으로 이동 합니다.

먼저 새 클래스 파일을 추가한 우리의 **엔터티** 폴더 호출 **글머리 기호**:

![](entities-images/image5.png "엔터티 폴더에 새 클래스 파일을 추가 하 고 호출 글머리 기호")

추가 수정 된 `Bullet.cs` 다음과 같이 코드:


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Bullet : CCNode
    {
        CCSprite sprite;

        public float VelocityX
        {
            get;
            set;
        }

        public float VelocityY
        {
            get;
            set;
        }

        public Bullet () : base()
        {
            sprite = new CCSprite ("bullet.png");
            // Making the Sprite be centered makes
            // positioning easier.
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);

            this.Schedule (ApplyVelocity);
        }

        void ApplyVelocity(float time)
        {
            PositionX += VelocityX * time;
            PositionY += VelocityY * time;
        }
    }
} 
```

에 사용 되는 파일을 변경 하는 것 외 합니다 `CCSprite` 하 `bullet.png`의 코드 `ApplyVelocity` 두 계수를 기반으로 하는 이동 논리가 포함 되어 있습니다: `VelocityX` 및 `VelocityY`합니다.

`Schedule` 메서드 프레임 마다 호출 되는 대리자를 추가할 수 있습니다. 이 경우 추가 될 예정 된 `ApplyVelocity` 메서드는 글머리 기호는 개발 속도 값에 따라 이동 되도록 합니다. 합니다 `Schedule` 메서드는 `Action<float>`float 매개 변수는 시간 (초) 이후 이동 시간을 기준으로 구현 하는 데 사용 하는 마지막 프레임 크기를 지정 하는 위치입니다. 이후에 값은 초 단위로 측정 됩니다 다음 속도 값이 나타내는 이동을 *초당 픽셀*합니다.


## <a name="adding-bullets-to-gamelayer"></a>GameLayer 글머리 기호 추가

모든 추가 하기 전에 `Bullet` 게임이 인스턴스 만들겠습니다 컨테이너를 구체적으로 `List<Bullet>`합니다. 수정 된 `GameLayer` 글머리 기호 목록을 포함 하도록 합니다.


```csharp
    public class GameLayer : CCLayer
    {
        Ship ship;
        List<Bullet> bullets;

        public GameLayer ()
        {
            ship = new Ship ();
            ship.PositionX = 240;
            ship.PositionY = 50;
            this.AddChild (ship);

            bullets = new List<Bullet> ();
        }
        ... 
```

채울 해야 다음은 `Bullet` 목록입니다. 만들어야 하는 경우에 대 한 논리를 `Bullet` 에 포함 되어야 합니다는 `Ship` 엔터티를 하지만 `GameLayer` 글머리 기호 목록을 저장을 담당 합니다. 팩터리 패턴을 사용 하 여 허용 합니다 `Ship` 만들 엔터티의 `Bullet` 인스턴스. 이 팩터리는 이벤트를 노출 하는 `GameLayer` 처리할 수 있습니다. 

이렇게 하려면 먼저 추가한 폴더를 호출 하는 프로젝트에 **팩터리**, 다음 라는 새 클래스를 추가 하 고 `BulletFactory`:

![](entities-images/image6.png "팩터리를 프로젝트에 폴더를 추가 하 고 BulletFactory 라는 새 클래스 추가")

구현에서는 다음으로 `BulletFactory` singleton 클래스:


```csharp
using System;

namespace EntityProject
{
    public class BulletFactory
    {
        static Lazy<BulletFactory> self = 
            new Lazy<BulletFactory>(()=>new BulletFactory());

        // simple singleton implementation
        public static BulletFactory Self
        {
            get
            {
                return self.Value;
            }
        }

        public event Action<Bullet> BulletCreated;

        private BulletFactory()
        {

        }

        public Bullet CreateNew()
        {
            Bullet newBullet = new Bullet ();

            if (BulletCreated != null)
            {
                BulletCreated (newBullet);
            }

            return newBullet;
        }
    }
} 
```

`Ship` 엔터티 만들기 처리할 `Bullet` 인스턴스 – 특히 처리할 빈도 `Bullet` 인스턴스 (예: 얼마나 자주 글머리 발생)을 만들어야 해당 위치 및 해당 속도입니다.

수정 된 `Ship` 새 엔터티 생성자 `Schedule` 호출 및 그런 다음이 메서드를 다음과 같이 구현:


```csharp
...
public Ship () : base()
{
    sprite = new CCSprite ("ship.png");
    // Center the Sprite in this entity to simplify
    // centering the Ship on screen
    sprite.AnchorPoint = CCPoint.AnchorMiddle;
    this.AddChild(sprite);

    touchListener = new CCEventListenerTouchAllAtOnce();
    touchListener.OnTouchesMoved = HandleInput;
    AddEventListener(touchListener, this);

    Schedule (FireBullet, interval: 0.5f);

}

void FireBullet(float unusedValue)
{
    Bullet newBullet = BulletFactory.Self.CreateNew ();
    newBullet.Position = this.Position;
    newBullet.VelocityY = 100;
} 
...
```

새로 만들기를 처리 하는 마지막 단계입니다 `Bullet` 인스턴스는 `GameLayer` 코드입니다. 이벤트 처리기를 추가 합니다 `BulletCreated` 새로 만든 추가 하는 이벤트 `Bullet` 적절 한 목록에:


```csharp
...
public GameLayer ()
{
    ship = new Ship ();
    ship.PositionX = 240;
    ship.PositionY = 50;
    this.AddChild (ship);

    bullets = new List<Bullet> ();
    BulletFactory.Self.BulletCreated += HandleBulletCreated;
}

void HandleBulletCreated(Bullet newBullet)
{
    AddChild (newBullet);
    bullets.Add (newBullet);
}
... 
```

이제 게임을 실행 하 고 볼 수 있습니다 합니다 `Ship` 해결 `Bullet` 인스턴스:

![](entities-images/image1.png "게임을 실행 하 고 우주선에 글머리 기호 인스턴스 해결 됩니다.")


## <a name="why-gamelayer-has-ship-and-bullets-members"></a>왜 GameLayer 멤버가 배송 및 글머리 기호

우리의 `GameLayer` 이 엔터티 인스턴스에 대 한 참조를 저장할 필드를 정의 하는 클래스 (`ship` 및 `bullets`), 상호 아무 작업도 수행 하지 않지만 합니다. 또한 엔터티 이동 및 해결 하는 등 자신의 동작에 대 한 책임이 있습니다. 따라서 이유 않은 추가 `ship` 하 고 `bullets` 필드를 `GameLayer`?

이러한 멤버를 추가한 이유는 전체 게임 구현 논리에 필요 하기 때문에 `GameLayer` 여러 엔터티 간의 상호 작용에 대 한 합니다. 예를 들어이 게임 추가로 개발할 수 플레이어에서 제거할 수는 적을 포함 합니다. 이러한 적에 보관 됩니다는 `List` 에 `GameLayer`, 및 테스트 하는 논리 여부를 `Bullet` 인스턴스가 충돌할에서 수행 된 적을 `GameLayer` 도. 즉, 합니다 `GameLayer` 루트인 *소유자* 모든 엔터티의 인스턴스를 되며 엔터티 인스턴스 간의 상호 작용을 담당 합니다.


## <a name="bullet-destruction-considerations"></a>글머리 기호 소멸 고려 사항

여기서 다루는 게임 제거에 대 한 코드를 현재 없는 `Bullet` 인스턴스. 각 `Bullet` 화면에서 이동 하기 위한 논리가 인스턴스 하지만 모든 화면 밖으로 삭제 하는 코드를 추가 하지는 않았지만 `Bullet` 인스턴스.

또한의 소멸 `Bullet` 인스턴스에 속할 수 없습니다 `GameLayer`합니다. 예를 들어, 화면 밖 때 소멸 되 고 대신는 `Bullet` 엔터티 자체를 특정 시간 후에 제거할 논리 있을 수 있습니다. 이 경우는 `Bullet` 를 제거 해야 해당 통신 하는 방법이 필요를 `GameLayer`, 유사를 `Ship` 에 전달 된 엔터티를 `GameLayer` 는 새 `Bullet` 를 통해 생성 되었는지를 `BulletFactory`.

가장 간단한 방법은 소멸을 지원 하기 위해 팩터리 클래스의 책임을 확장 하는 것입니다. 팩터리와 같은 다른 개체에서 처리할 수 있는 엔터티 인스턴스 소멸 되 고 알림을 받을 수는 다음을 `GameLayer` 해당 목록에서 엔터티 인스턴스를 제거 합니다. 

## <a name="summary"></a>요약

이 가이드에서는에서 상속 하 여 CocosSharp 엔터티를 만드는 방법의 `CCNode` 클래스입니다. 이러한 엔터티는 자체 포함 된 개체에 고유한 시각적 개체 및 사용자 지정 논리의 생성을 처리 합니다. 이 가이드는 루트 엔터티 컨테이너 (충돌 및 기타 엔터티 상호 작용 논리)에 속하는 코드에서 엔터티 (이동 및 다른 엔터티를 생성) 내에 속하는 코드를 지정 합니다.

## <a name="related-links"></a>관련 링크

- [CocosSharp API 설명서](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [콘텐츠 zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)
