---
title: "CocosSharp의 엔터티"
description: "엔터티 방법은 게임 코드를 구성 하는 강력한 방법입니다. 가독성을 높일 수, 코드를 보다 쉽게 유지 관리, 기본 제공 부모/자식 기능을 활용 하 고 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D3261CE-AC96-4296-8A53-A76A42B927A8
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: fe722ce75f0322ab60bb6fd967ff2c498b2e7b20
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="entities-in-cocossharp"></a>CocosSharp의 엔터티

_엔터티 방법은 게임 코드를 구성 하는 강력한 방법입니다. 가독성을 높일 수, 코드를 보다 쉽게 유지 관리, 기본 제공 부모/자식 기능을 활용 하 고 있습니다._

엔터티 패턴 향상 된 코드 조직을 통해 CocosSharp와 개발자의 활동을 향상 시킬 수 있습니다. 이 연습에서는 두 개의 엔터티가 배송 엔터티 및 글머리 기호 엔터티를 만드는 방법을 보여 주는 예제는 실습 됩니다. 이러한 엔터티 됩니다 자체 포함 된 개체를 한 번에 인스턴스화되 즉 자동으로 렌더링 될 하며 해당 형식에 대해 적절 하 게 이동 논리를 수행 합니다. 

이 가이드는 다음 항목을 다룹니다.

 - 게임 엔터티 소개
 - 특정 엔터티 형식 및 일반
 - 프로젝트 설정
 - 엔터티 클래스 만들기
 - 엔터티 인스턴스를 추가 하는 `GameLayer`
 - 엔터티 논리에 응답할는 `GameLayer`

완성 된 게임은 다음과 같이 표시 됩니다.

![](entities-images/image1.png "완성 된 게임은 다음과 같습니다.")


# <a name="introduction-to-game-entities"></a>게임 엔터티 소개

게임 엔터티는 렌더링, 충돌, 물리학 또는 인공 intelligence 논리 필요한 개체를 정의 하는 클래스입니다. 다행히 게임의 코드 베이스에 있는 엔터티는 종종 게임 개념적 개체와 일치 합니다. True 이면 게임에 필요한 엔터티 식별 더 쉽게 수행할 수 있습니다. 

테마가 지정 된 공백 예를 들어 [그들 게임을 해결](http://en.wikipedia.org/wiki/Shoot_%27em_up) 엔터티는 포함 될 수 있습니다.

 - `PlayerShip`
 - `EnemyShip`
 - `PlayerBullet`
 - `EnemyBullet`
 - `CollectableItem`
 - `HUD`
 - `Environment`

이러한 엔터티는 게임에서 고유한 클래스와 각 인스턴스에 인스턴스화 초과 거의 없거나 전혀 없이 설치 해야 합니다.


# <a name="general-vs-specific-entity-types"></a>일반 vs입니다. 특정 엔터티 형식

엔터티 시스템을 사용 하 여 게임 개발자가 직면 하 고 첫 번째 질문 것이 해당 엔터티를 일반화할 수 정도입니다. 가장 구체적인 구현을 정의할 클래스 엔터티의 모든 형식에 대 한 몇 가지 특성에 따라 차이가 있는 경우에 있습니다. 보다 일반적인 시스템 하나의 클래스로 엔터티 그룹을 결합 하 고 인스턴스를 사용자 지정할 수를 허용 합니다.

예를 들어 다음 클래스를 정의 하는 공간 게임을 같이 가정해 봅니다 수 있습니다.

 - `PlayerDogfighter`
 - `PlayerBomber`
 - `EnemyMissileShip`
 - `EnemyLaserShip`

더 일반화 방법은 플레이어 제공에 대 한 단일 클래스와 다른 배송 형식을 지원 하도록 구성 될 수 있는 적 제공에 대 한 단일 클래스를 만드는 것입니다. 사용자 지정 이미지를 로드 하 고 어떤 유형의 촬영 이동 계수 때 만들 글머리 기호 엔터티를 포함 될 수 있습니다 및 적에 대 한 AI 논리를 제공 합니다. 이 경우 엔터티 목록 줄어들 수 있습니다.

 - `PlayerShip`
 - `EnemyShip`

물론, 움직임을 제어 하기 위한 인스턴스 당 사용자 지정을 허용 하 여 이러한 엔터티 형식은 더 일반화 될 수 있습니다. 플레이어 배송 인스턴스를 입력에서 읽을 것 적 배송 인스턴스 AI 논리를 수행할 수 있습니다. 즉, 엔터티를 일반화 될 수를 단일 클래스로 추가 합니다.

 - `Ship`

일반화 더욱 – 계속할 수 게임 모든 엔터티에 대 한 단일 기본 클래스를 사용할 수 있습니다. 이 단일 클래스를 호출할 수 있습니다는 `GameEntity`, 게임 중에서 모든 엔터티 인스턴스에 대해 사용 되는 클래스, 글머리 기호 및 수집 가능한 항목 (전원 ups) 등 함께 제공 되지 않은 엔터티를 포함 합니다.

이 시스템의 대부분 일반 종종 라고는 *구성 요소가 시스템*합니다. 이러한 시스템의 게임 엔터티 물리학 AI, 같은 개별 구성 요소를 가질 수 있습니다 또는 동작 및 모양을 사용자 지정 하려면 추가 구성 요소를 렌더링 합니다. 순수 구성 요소 기반 시스템 최고의 유연성을 사용 하도록 설정 하 고 상속 같은 복잡 한 상속 체인을 사용 하 여 발생 하는 문제를 방지할 수 있습니다. 다른 일반화와 마찬가지로 유효한 구성 요소를 설정 하기 어려울 수 있습니다 시스템과 코드 표현을 줄일 수 있습니다.

사용 되는 일반화 수준의 비롯 한 많은 고려 사항에 따라 달라 집니다.

 - 게임 크기 – 작은 게임을 더 큰 게임 다양 한 클래스를 사용 하 여 관리 하기 어려울 수 있지만 특정 클래스를 만드는 더 이용할 수 있습니다.
 - 데이터 기반 개발 – 일반화 된 엔터티 형식 발생에서 데이터 (예: 이미지, 3D 모델 및 JSON 또는 XML과 같은 데이터 파일)에 의존 하는 게임을 활용할 수 있습니다 및 데이터를 기반으로 고유 정보를 구성 합니다. 이 개발 중 이나 게임 해제 된 후 새 콘텐츠를 추가 하는 게임에 대 한 특히 가져오기입니다.
 - 게임 엔진 패턴 – 일부 게임 엔진 것을 적극 권장 구성 요소가 시스템의 사용 엔터티를 구성 하는 방법을 결정 하는 개발자를 사용 하는 다른 사용자입니다. CocosSharp 구성 시스템의 사용이 필요 하지 않으므로 개발자는 엔터티의 모든 유형을 구현 하도록 합니다. 

간단한 설명을 위해 사용할 예정 특정 클래스 기반 접근 방식을 단일 글머리 기호 및 배송 업체와이 자습서에 대 한 합니다.


# <a name="project-setup"></a>프로젝트 설정

이 엔터티를 구현 합니다. 시작 하기 전에 프로젝트를 만드는 필요 합니다. 사용할 예정 CocosSharp 프로젝트 템플릿 프로젝트 만들기를 간소화할 수 있습니다. [이 게시물 확인](http://forums.xamarin.com/discussion/26822/cocossharp-project-templates-for-xamarin-studio) Mac 템플릿에 대 한 Visual Studio에서 CocosSharp 프로젝트 만들기에 대 한 내용은 합니다. 이 가이드의 나머지 부분에서는 프로젝트 이름을 ´ ֲ **EntityProject**합니다.

프로젝트가 만들어지면 480 x 320에 실행 되도록 다루는 게임의 해상도 설정 합니다 했습니다. 이 작업을 수행 하려면 호출 `CCScene.SetDefaultDesignResolution` 에 `GameAppDelegate.ApplicationDidFinishLaunching` 메서드를 다음과 같이 합니다.


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...

    // New code for resolution setting:
    CCScene.SetDefaultDesignResolution(480, 320, CCSceneResolutionPolicy.ShowAll);
    
    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);
    mainWindow.RunWithScene (scene);
} 
```

관리 CocosSharp 해상도에 대 한 자세한 내용은 참조 하십시오. 우리의 [CocosSharp에 여러 해상도 처리에 대 한 안내서](~/graphics-games/cocossharp/resolutions.md)합니다.


# <a name="adding-content-to-the-project"></a>프로젝트에 콘텐츠 추가

이 프로젝트를 만든 후에 포함 된 파일 추가 합니다 [이 콘텐츠 zip 파일](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)합니다. 이 위해 zip 파일을 다운로드 하 고 압축을 풉니다. 모두 추가 **ship.png** 및 **bullet.png** 에 **콘텐츠** 폴더입니다. **콘텐츠** 폴더 됩니다는 **자산** 폴더에 Android, ios 프로젝트의 루트에 됩니다. 두 파일에 추가 되 면 확인할는 **콘텐츠** 폴더:

![](entities-images/image2.png "두 파일을 콘텐츠 폴더에 있어야 추가")


# <a name="creating-the-ship-entity"></a>배송 엔터티 만들기

`Ship` 클래스 게임의 첫 번째 엔터티 됩니다. 추가 하는 `Ship` 클래스, 먼저 라는 폴더를 만들어 **엔터티** 프로젝트의 루트 수준에서 합니다. 새 클래스 추가 **엔터티** 라는 폴더 `Ship`:

![](entities-images/image3.png "배송 라는 엔터티 폴더의 새 클래스를 추가 합니다.")

만들면 되는 첫 번째 변경 사항을 우리의 `Ship` 클래스에서 상속 하도록 하는 것은 `CCNode` 클래스입니다. `CCNode` 일반적인 CocosSharp 클래스에 대 한 기본 클래스로 사용 되며 같은 `CCSprite` 및 `CCLayer`, 목록에 다음과 같은 기능 및:

 - `Position` 화면 배송 이동 하기 위한 속성입니다.
 - `Children` 추가 하기 위한 속성을 `CCSprite.`
 - `Parent` 연결에 사용할 수 있는 속성 `Ship` 다른 인스턴스 `CCNodes`합니다. 사용 하지이 기능은이 자습서; 더 큰 게임 종종 활용 다른 엔터티 연결 `CCNodes`합니다. 
 - `AddEventListener` 배송 이동 하기 위한 입력에 응답 하기 위한 방법입니다.
 - `Schedule` 글머리 기호 해결 방법입니다.

추가 `CCSprite` 우리의 배송 화면에 표시 될 수 있도록 인스턴스:


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Ship : CCNode
    {
        CCSprite sprite;

        public Ship () : base()
        {
            sprite = new CCSprite ("ship.png");
            // Center the Sprite in this entity to simplify
            // centering the Ship when it is instantiated
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);
        }
    }
}
```

다음으로, 받는 사람 추가 우리의 `GameLayer` 게임에서 표시 하려면:


```csharp
public class GameLayer : CCLayer
{
    Ship ship;

    public GameLayer ()
    {
        ship = new Ship ();
        ship.PositionX = 240;
        ship.PositionY = 50;
        this.AddChild (ship);
    } 
    ...
```

이제 살펴보겠습니다 게임 우리의 배송 엔터티 실행 하는 경우:

![](entities-images/image4.png "게임을 실행 하는 경우, 배송 엔터티 표시 됩니다.")


## <a name="why-inherit-from-ccnode-instead-of-ccsprite"></a>CCSprite 대신 CCNode에서 상속 하는 이유?

이 시점에서 우리의 `Ship` 클래스에 대 한 간단한 래퍼는는 `CCSprite` 인스턴스. 이후 `CCSprite` 에서 상속 `CCNode`에서 직접 상속 있을 수 있습니다 `CCSprite`의 코드 줄인 것는 `Ship.cs`합니다. 또한에서 직접 상속 `CCSprite` 메모리 개체 수를 줄이고 종속성 트리를 더욱 작게 만드는 방법을 여 성능을 향상 시킬 수 있습니다.

상속에서는 이러한 이점을 불구 하 고 `CCNode` 중 일부를 숨기는 `CCSprite` 각 인스턴스에서 속성입니다. 예를 들어는 `Texture` 외부의 속성을 수정 하지 않아야는 `Ship` 클래스에서 상속 하 고 `CCNode` 을이 속성을 숨길 수 있습니다. 엔터티는 공용 멤버 더욱 게임 커질수록 및 추가 개발자 팀에 추가 될 때 특히 중요 합니다.


# <a name="adding-input-to-the-ship"></a>배송에 대 한 입력을 추가합니다.

우리의 배송 화면에 표시 되었습니다. 입력을 추가할 예정입니다. 이 접근 방식에서 취한 접근 방식을 유사한 됩니다는 [CocosSharp 가이드를 소개](~/graphics-games/cocossharp/first-game/part2.md)의 동작에 대 한 코드 배치할 것 이라는 점을 제외 하면는 `Ship` 클래스 아닌 포함 하는 `CCLayer` 또는 `CCScene`.

코드를 추가 `Ship` 위로 이동 하는 사용자가 여 화면을 터치 하는 아무 곳에 나 지원 하려면:


```csharp
public class Ship : CCNode
{
    CCSprite sprite;

    CCEventListenerTouchAllAtOnce touchListener;

    public Ship () : base()
    {
        sprite = new CCSprite ("ship.png");
        // Center the Sprite in this entity to simplify
        // centering the Ship on screen
        sprite.AnchorPoint = CCPoint.AnchorMiddle;
        this.AddChild(sprite);

        touchListener = new CCEventListenerTouchAllAtOnce();
        touchListener.OnTouchesMoved = HandleInput;
        AddEventListener(touchListener, this);

    }

    private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
    {
        if(touches.Count > 0)
        {
            CCTouch firstTouch = touches[0];

            this.PositionX = firstTouch.Location.X;
            this.PositionY = firstTouch.Location.Y;
        } 
    }
} 
```

많은 해결 그들 게임 구현을 기존 컨트롤러 기반 이동을 모방 최대 개발 속도입니다. 즉, 더 짧은 코드를 유지 하는 즉시 이동 하기만 하면 구현 합니다.


# <a name="creating-the-bullet-entity"></a>글머리 기호 엔터티 만들기

간단한 게임 두 번째 엔터티가 글머리 기호를 표시 하기 위한 엔터티가 있습니다. 마찬가지로 `Ship` 엔터티에 `Bullet` 엔터티에 포함 됩니다는 `CCSprite` 화면에 표시 되도록 합니다. 이동에 대 한 논리 달리 이동;에 대 한 사용자 입력에 종속 되지 않습니다. 대신, `Bullet` 인스턴스 속도 속성을 사용 하 여 직선으로 이동 합니다.

먼저 새 클래스 파일을 추가 하겠습니다 우리의 **엔터티** 폴더 버전을 호출 **글머리 기호**:

![](entities-images/image5.png "엔터티 폴더에 새 클래스 파일을 추가 하 고 글머리 기호 호출")

추가 수정 합니다는 `Bullet.cs` 코드 다음과 같습니다.


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Bullet : CCNode
    {
        CCSprite sprite;

        public float VelocityX
        {
            get;
            set;
        }

        public float VelocityY
        {
            get;
            set;
        }

        public Bullet () : base()
        {
            sprite = new CCSprite ("bullet.png");
            // Making the Sprite be centered makes
            // positioning easier.
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);

            this.Schedule (ApplyVelocity);
        }

        void ApplyVelocity(float time)
        {
            PositionX += VelocityX * time;
            PositionY += VelocityY * time;
        }
    }
} 
```

에 사용 되는 파일을 변경 하는 것 외의 `CCSprite` 를 `bullet.png`의 코드 `ApplyVelocity` 두 계수를 기반으로 하는 이동 논리가 포함 되어: `VelocityX` 및 `VelocityY`합니다.

`Schedule` 메서드를 사용 하면 모든 프레임 호출 될 대리자를 추가 합니다. 추가할 예정이 경우에 `ApplyVelocity` 메서드 우리의 글머리 기호는 개발 속도 값에 따라 이동 되도록 합니다. `Schedule` 메서드는 `Action<float>`, float 매개 변수 이동 시간 기반 구현 하는 데 사용 되는 마지막 프레임 이후 초 단위로 시간을 지정 합니다. 이후로 값은 초 단위로 측정 됩니다 다음 우리의 속도 값에서 이동을 나타내는 *초당 픽셀*합니다.


# <a name="adding-bullets-to-gamelayer"></a>GameLayer에 글머리 기호 추가

모든 추가 하기 전에 `Bullet` 인스턴스를 다루는 게임 해 드립니다는 컨테이너를 구체적으로 `List<Bullet>`합니다. 수정 된 `GameLayer` 글머리 기호 목록을 포함 하도록 합니다.


```csharp
    public class GameLayer : CCLayer
    {
        Ship ship;
        List<Bullet> bullets;

        public GameLayer ()
        {
            ship = new Ship ();
            ship.PositionX = 240;
            ship.PositionY = 50;
            this.AddChild (ship);

            bullets = new List<Bullet> ();
        }
        ... 
```

다음을 채우는 해야는 `Bullet` 목록입니다. 만들어야 하는 경우에 대 한 논리는 `Bullet` 에 포함 되어야 합니다는 `Ship` 엔터티에 되지만 `GameLayer` 는를 글머리 기호 목록에 저장 합니다. 팩터리 패턴을 허용 하는 데 사용할는 `Ship` 만들 엔터티가 `Bullet` 인스턴스. 이 팩터리에 이벤트를 노출 하는 `GameLayer` 처리할 수 있습니다. 

이렇게 하려면 먼저 추가 하겠습니다 폴더를 호출 하는 프로젝트에 **팩터리**, 다음 라는 새 클래스를 추가 하 고 `BulletFactory`:

![](entities-images/image6.png "Factories를 프로젝트에 폴더를 추가 하 고 BulletFactory 라는 새 클래스 추가")

다음으로 구현 된 `BulletFactory` 단일 항목 클래스:


```csharp
using System;

namespace EntityProject
{
    public class BulletFactory
    {
        static Lazy<BulletFactory> self = 
            new Lazy<BulletFactory>(()=>new BulletFactory());

        // simple singleton implementation
        public static BulletFactory Self
        {
            get
            {
                return self.Value;
            }
        }

        public event Action<Bullet> BulletCreated;

        private BulletFactory()
        {

        }

        public Bullet CreateNew()
        {
            Bullet newBullet = new Bullet ();

            if (BulletCreated != null)
            {
                BulletCreated (newBullet);
            }

            return newBullet;
        }
    }
} 
```

`Ship` 엔터티를 만들어 처리할 `Bullet` 인스턴스 – 구체적으로 처리할 수 있으므로 얼마나 자주 `Bullet` 인스턴스 만들 (글머리 기호는 발생 빈도, 즉)의 위치 및 해당 개발 속도입니다.

수정는 `Ship` 새 엔터티의 생성자 `Schedule` 를 호출 하 고 다음이 메서드를 다음과 같이 구현 합니다.


```csharp
...
public Ship () : base()
{
    sprite = new CCSprite ("ship.png");
    // Center the Sprite in this entity to simplify
    // centering the Ship on screen
    sprite.AnchorPoint = CCPoint.AnchorMiddle;
    this.AddChild(sprite);

    touchListener = new CCEventListenerTouchAllAtOnce();
    touchListener.OnTouchesMoved = HandleInput;
    AddEventListener(touchListener, this);

    Schedule (FireBullet, interval: 0.5f);

}

void FireBullet(float unusedValue)
{
    Bullet newBullet = BulletFactory.Self.CreateNew ();
    newBullet.Position = this.Position;
    newBullet.VelocityY = 100;
} 
...
```

새로운의 작성을 위한 마지막 단계는 `Bullet` 인스턴스에 `GameLayer` 코드입니다. 추가 하는 이벤트 처리기는 `BulletCreated` 새로 만든 추가 하는 이벤트 `Bullet` 해당 목록에:


```csharp
...
public GameLayer ()
{
    ship = new Ship ();
    ship.PositionX = 240;
    ship.PositionY = 50;
    this.AddChild (ship);

    bullets = new List<Bullet> ();
    BulletFactory.Self.BulletCreated += HandleBulletCreated;
}

void HandleBulletCreated(Bullet newBullet)
{
    AddChild (newBullet);
    bullets.Add (newBullet);
}
... 
```

이제 참조 고 게임을 실행할 수는 `Ship` 촬영 `Bullet` 인스턴스:

![](entities-images/image1.png "게임을 실행 하 고 배송에 글머리 기호 인스턴스 촬영 합니다.")


# <a name="why-gamelayer-has-ship-and-bullets-members"></a>GameLayer 배송 및 글머리 기호 멤버에서 하는 이유

우리의 `GameLayer` 우리의 엔터티 인스턴스에 대 한 참조를 두 개의 필드를 정의 하는 클래스 (`ship` 및 `bullets`), 하지만 이러한 아무 작업도 수행 합니다. 또한 엔터티는 고유한 동작을 해결 합니다. 따라서 이유 않은 추가 `ship` 및 `bullets` 필드를 `GameLayer`?

이러한 멤버를 추가 하는 이유는 하기 때문에 전체 게임 구현에서 논리는 `GameLayer` 서로 다른 엔터티 간의 상호 작용에 대 한 합니다. 예를 들어 플레이어에서 제거할 수 있습니다. 적 포함 하도록이 게임을 추가로 개발 될 수 있습니다. 이러한 적 포함 됩니다는 `List` 에 `GameLayer`, 및 테스트 하는 논리 있는지 여부를 `Bullet` 인스턴스 충돌에서 수행 된 적은 `GameLayer` 도 합니다. 즉,는 `GameLayer` 루트 *소유자* 모든 엔터티의 인스턴스를 되며 엔터티 인스턴스 간의 상호 작용을 담당 합니다.


# <a name="bullet-destruction-considerations"></a>글머리 기호 소멸 고려 사항

게임에 제거에 대 한 코드를 현재 부족 `Bullet` 인스턴스. 각 `Bullet` 인스턴스에 화면에서 이동 하기 위한 논리는 하지만 모든 화면 밖으로 삭제 하기 위해 코드를 추가 하지는 않았지만 `Bullet` 인스턴스.

또한 파괴 `Bullet` 인스턴스에 속해 있지 `GameLayer`합니다. 예를 들어, 화면에서 벗어난 경우 제거 되 고 대신는 `Bullet` 엔터티 시간이 지난 후 자체를 삭제 하는 논리가 있을 수 있습니다. 이 경우는 `Bullet` 에 제거 되어야 할지 통신 하는 방법이 필요는 `GameLayer`와 비슷하며는 `Ship` 에 전달 된 엔터티는 `GameLayer` 하는 새 `Bullet` 통해 만들어졌는지는 `BulletFactory`합니다.

가장 간단한 방법은 소멸을 지원 하기 위해 팩터리 클래스의 책임을 확장 하는 것입니다. 엔터티 인스턴스 제거 되 고와 같은 다른 개체에서 처리할 수 있는 공장 알림을 받을 수는 다음의 `GameLayer` 해당 목록에서 엔터티 인스턴스를 제거 합니다. 


# <a name="summary"></a>요약

이 가이드에서는에서 상속 하 여 CocosSharp 엔터티를 만드는 방법을 보여 줍니다.는 `CCNode` 클래스입니다. 이러한 엔터티는 만들기 자신의 시각적 개체 및 사용자 지정 논리를 처리 하는 자체 포함 된 개체입니다. 이 가이드는 루트 엔터티 컨테이너 (충돌 및 기타 엔터티 상호 작용 논리)에 속해 있는 코드에서 엔터티 (이동 및 다른 엔터티의 생성) 안에 속하는 코드를 지정 합니다.

## <a name="related-links"></a>관련 링크

- [CocosSharp API 설명서](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [콘텐츠 zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)
