---
title: Coin Time 게임 세부 정보
description: 이 가이드는 동전 시간 게임을 타일 맵 작업 생성, 엔터티, 스프라이트, 애니메이션 및 효율적인 충돌 구현에서 구현 세부 사항을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5D285684-0417-4E16-BD14-2D1F6DEFBB8B
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: af914e10eb93aa0668743a113ffe647a939fea75
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61395900"
---
# <a name="coin-time-game-details"></a>Coin Time 게임 세부 정보

_이 가이드는 동전 시간 게임을 타일 맵 작업 생성, 엔터티, 스프라이트, 애니메이션 및 효율적인 충돌 구현에서 구현 세부 사항을 설명 합니다._

주요 고려 사항인 시간은 iOS 및 Android 용 게임을 전체 platformer입니다. 게임의 목표는 모든 수준의 동전을 수집 하 고 다음 종료 도어 적 및 장애를 방지 하는 동안 도달 하는 것입니다.

![](cointime-images/image1.png "게임의 목표는 모든 수준의 동전을 수집 하 여 다음 종료 도어 적 및 장애를 방지 하는 동안 도달")

이 가이드에서 주요 고려 사항인 경우 다음 항목을 다루는 구현 세부 사항을 설명 합니다.

- [Tmx 파일 작업](#working-with-tmx-files)
- [로드 수준](#level-loading)
- [새 엔터티를 추가합니다.](#adding-new-entities)
- [애니메이션이 적용 된 엔터티](#animated-entities)


## <a name="content-in-coin-time"></a>주요 고려 사항인 시점의 콘텐츠

주요 고려 사항인 시간은 전체 CocosSharp 프로젝트를 구성할 수 있습니다 하는 방법을 나타내는 샘플 프로젝트입니다. Coin Time의 추가 및 콘텐츠 유지 관리를 간소화 하는 것을 구성 합니다. 사용 하 여 **.tmx** 에서 만든 파일 [바둑판식](http://www.mapeditor.org) 수준 및 애니메이션을 정의 하는 XML 파일에 대 한 합니다. 수정 또는 새 콘텐츠 추가 최소한의 노력으로 수행할 수 있습니다. 

하는 방법을 전문 게임이 이렇게 동전 시간 학습 및 실험에 대 한 유효한 프로젝트를 사용 하면, 사례도 반영 됩니다. 이 가이드는 추가 및 수정 콘텐츠를 간소화 하기 위해 수행 하는 방법을 설명 합니다.


## <a name="working-with-tmx-files"></a>Tmx 파일 작업

주요 고려 사항인 시간 수준에서 출력 하는 구성 된.tmx 파일 형식을 사용 하 여 정의 된 합니다 [바둑판식](http://www.mapeditor.org) 타일 맵 편집기. 타일을 사용 하는 자세한 내용은 참조는 [Cocos 날카로운 가이드로 바둑판식 화면 사용](~/graphics-games/cocossharp/tiled.md)합니다. 

각 수준에 포함 된 자체.tmx 파일에 정의 된 **CoinTime/자산/내용/수준** 폴더입니다. 모든 주요 고려 사항인 시간 수준에 정의 되어 있는 하나의 tileset 파일을 공유 합니다 **mastersheet.tsx** 파일입니다. 이 파일에는 타일의 solid 충돌에 있는지 여부 또는 엔터티 인스턴스에서 타일 바꿔야 하는지 여부와 같은 각 타일에 대 한 사용자 지정 속성을 정의 합니다. Mastersheet.tsx 파일 속성을을 한 번만 정의 하 고 모든 수준에서 사용할 수 있습니다. 


### <a name="editing-a-tile-map"></a>타일 맵을 편집

지도 타일을 편집 하려면.tmx 파일을 두 번 클릭 하거나 타일에서 파일 메뉴를 열고 타일에서.tmx 파일을 엽니다. Coin Time 수준 타일 맵은 3 계층을 포함 합니다. 

- **엔터티** –이 계층에 런타임 시 엔터티의 인스턴스를 사용 하 여 대체 될 타일이 포함 되어 있습니다. 플레이어, 동전, 적 및 최종-의-수준 도어를 예로 들 수 있습니다.
- **지형** –이 계층에 일반적으로 단색 충돌 하는 타일이 포함 되어 있습니다. Solid 충돌 이동 하지 않고 이러한 타일에 워크 플레이어 수 있습니다. Solid 충돌 타일 벽 및 최대값이 역할도 할 수 있습니다.
- **백그라운드** – 배경 계층에 고정 배경으로 사용 되는 타일이 포함 되어 있습니다. 이 계층 깊이 시차를 통해 모양 만들기 수준에서 전체 카메라를 움직이면 스크롤되지 않습니다.

나중에 탐색 하는 모든 동전 시간 수준에서이 세 가지 계층 수준 로드 코드에 필요 합니다.

#### <a name="editing-terrain"></a>지형 편집

타일을 클릭 하 여 배치할 수는 **mastersheet** tileset 및 타일을 클릭 하면 다음 매핑됩니다. 예를 들어, 수준의 새 지형에 그릴:

1. 지형 계층 선택
1. 그릴 타일 클릭
1. 클릭 하 고 또는 푸시, 타일을 그릴 지도 위에 끌어서

    ![](cointime-images/image2.png "1 그릴 타일 클릭")

tileset의 왼쪽 위 지형 동전 시간에서의 모든 포함합니다. 지형 solid가 포함 된 **SolidCollision** 타일 속성 화면 왼쪽에 표시 된 속성:

![](cointime-images/image3.png "Solid는 지형 타일 속성 화면 왼쪽에 표시 된 것 처럼 SolidCollision 속성 포함")

#### <a name="editing-entities"></a>엔터티 편집

엔터티 추가 또는 지형 마찬가지로 – 수준에서 제거할 수 있습니다. 합니다 **mastersheet** tileset 있으므로 배치에 대 한 중간 가로 방향으로, 모든 엔터티는 표시 되지 않는 오른쪽으로 스크롤 없이:

![](cointime-images/image4.png "Mastersheet tileset 있으므로 배치에 대 한 중간 가로 방향으로, 모든 엔터티는 표시 되지 않는 오른쪽으로 스크롤 없이")

에 새 엔터티를 배치 해야 합니다 **엔터티** 계층입니다.

![](cointime-images/image5.png "엔터티 계층에서 새 엔터티를 배치할")

CoinTime 코드 찾습니다 합니다 **EntityType** 엔터티에 의해 바꿔야 하는 타일을 식별 하는 수준을 로드할 때: 

![](cointime-images/image6.png "CoinTime 코드만 EntityType에 대해 엔터티에 의해 바꿔야 하는 타일을 식별 하는 수준 로드 될 때")

파일을 수정 하 고 저장 되 면 변경 내용을 자동으로 표시 됩니다 프로젝트 작성 및 실행 하는 경우:

![](cointime-images/image7.png "파일을 수정 하 고 저장 되 면 변경 내용을 자동으로 표시 됩니다 프로젝트 작성 및 실행 하는 경우")


### <a name="adding-new-levels"></a>새 수준 추가

수준 동전 시간에 추가 하는 프로세스는 코드 변경 없이 및 몇 가지 작은 변경 내용만 프로젝트에 필요 합니다. 새 수준을 추가 합니다.

1. 에 있는 폴더를 열고 <**CoinTime 루트 > \CoinTime\Assets\Content\levels**
1. 복사한, 수준 중 하나로 같은 **level0.tmx**
1. 수준 번호 시퀀스를 기존 수준 사용 하 여 같은 계속 새.tmx 파일 이름 바꾸기 **level8.tmx**
1. Visual Studio 또는 Mac 용 Visual Studio에서 Android 수준 폴더에 새.tmx 파일을 추가 합니다. 파일을 사용 하는지 확인 합니다 **AndroidAsset** 빌드 작업입니다.

    ![](cointime-images/image8.png "파일에서 AndroidAsset 빌드 작업을 사용 하는지 확인")

1. IOS 수준 폴더에 새.tmx 파일을 추가 합니다. 원래 위치에서 파일에 연결 하 고 사용 하는지 확인 해야 합니다 **BundleResource** 빌드 작업입니다.

    ![](cointime-images/image9.png "원래 위치에서 파일에 연결 하 고 BundleResource 빌드 작업에서 사용 하는지 확인 하도록 합니다.")

새 수준 수준 9로 수준 선택 화면에 표시 됩니다 (파일 이름 수준 0부터 시작 하지만 수준 단추 숫자 1로 시작):

![](cointime-images/image10.png "새 수준 0에서 9 수준 수준 파일 이름은 시작으로 수준 선택 화면에 표시 되어야 하지만 수준 단추 숫자 1 사용 하 여 시작")


## <a name="level-loading"></a>로드 수준

에서 설명한 것 처럼 새 수준이 필요한 코드 변경 없음 – 이름이 올바르게 지정 하에 추가 된 경우 자동으로 게임 수준을 검색 합니다 **수준을** 올바른 빌드 작업 폴더 (**BundleResource**나 **AndroidAsset**).

에 포함 된 수준의 수를 결정 하기 위한 논리는 `LevelManager` 클래스입니다. 인스턴스를 `LevelManager` (singleton 패턴 사용), 생성 되는 `DetermineAvailbleLevels` 메서드는:


```csharp
private void DetermineAvailableLevels()
{
    // This game relies on levels being named "levelx.tmx" where x is an integer beginning with
    // 1. We have to rely on MonoGame's TitleContainer which doesn't give us a GetFiles method - we simply
    // have to check if a file exists, and if we get an exception on the call then we know the file doesn't
    // exist. 
    NumberOfLevels = 0;
    while (true)
    {
        bool fileExists = false;
        try
        {
            using(var stream = TitleContainer.OpenStream("Content/levels/level" + NumberOfLevels + ".tmx"))
            {
            }
            // if we got here then the file exists!
            fileExists = true;
        }
        catch
        {
            // do nothing, fileExists will remain false
        }
        if (!fileExists)
        {
            break;
        }
        else
        {
            NumberOfLevels++;
        }
    }
}
```

CocosSharp 의존 하 고 파일이 있는 경우를 감지 하는 데 크로스 플랫폼 방법을 제공 하지 않습니다는 `TitleContainer` 스트림을 열기를 시도 하는 클래스입니다. 예외가 발생 한 후 파일 스트림 열기에 대 한 코드에서 throw 하는 경우 존재 하지 않으며 while 루프 중단 됩니다. 루프가 완료 되 면는 `NumberOfLevels` 속성 프로젝트의 일부인 유효한 수준 수를 보고 합니다.

합니다 `LevelSelectScene` 클래스가 사용 하는 `LevelManager.NumberOfLevels` 에서 만들기 단추를 확인 하는 `CreateLevelButtons` 메서드:


```csharp
private void CreateLevelButtons()
{
    const int buttonsPerPage = 6;
    int levelIndex0Based = buttonsPerPage * pageNumber;
    int maxLevelExclusive = System.Math.Min (levelIndex0Based + 6, LevelManager.Self.NumberOfLevels);
    int buttonIndex = 0;
    float centerX = this.ContentSize.Center.X;
    const float topRowOffsetFromCenter = 16;
    float topRowY = this.ContentSize.Center.Y + topRowOffsetFromCenter;
    for (int i = levelIndex0Based; i < maxLevelExclusive; i++)
    {
        ...
    }
}
```

`NumberOflevels` 속성 단추 만들어야 결정 하는 데 사용 됩니다. 이 코드는 사용자가 현재 보고 있는 만들고만 최대 6 개의 단추 페이지는 페이지를 고려 합니다. 단추 호출 인스턴스를 클릭 하면는 `HandleButtonClicked` 메서드:


```csharp
private void HandleButtonClicked(object sender, EventArgs args)
{
    // levelNumber is 1-based, so subtract 1:
    var levelIndex = (sender as Button).LevelNumber - 1;
    LevelManager.Self.CurrentLevel = levelIndex;
    CoinTime.GameAppDelegate.GoToGameScene ();
}
```

이 메서드를 할당 합니다 `CurrentLevel` 속성에서 사용 되는 `GameScene` 수준을 로드 하는 경우. 설정한 후 합니다 `CurrentLevel`, `GoToGameScene` 메서드 발생에서 장면 전환 `LevelSelectScene` 에 `GameScene`입니다.

합니다 `GameScene` 생성자 호출 `GoToLevel`, 로드 수준 논리를 수행 하는:


```csharp
private void GoToLevel(int levelNumber)
{
    LoadLevel (levelNumber);
    CreateCollision();
    ProcessTileProperties ();
    touchScreen = new TouchScreenInput(gameplayLayer);
    secondsLeft = secondsPerLevel;
}
```

다음에서 호출 된 메서드 살펴보겠습니다 알아보겠습니다 `GoToLevel`합니다.


### <a name="loadlevel"></a>LoadLevel

합니다 `LoadLevel` 메서드는.tmx 파일을 로드 하 고 추가 하는 `GameScene`합니다. 이 메서드는 엔터티 충돌 등 모든 대화형 개체를 만들지 않습니다 – 라고도 하는 수준에 대 한 시각적 개체를 만들기만 합니다 *환경*합니다.


```csharp
private void LoadLevel(int levelNumber)
{
    currentLevel = new CCTileMap ("level" + levelNumber + ".tmx");
    currentLevel.Antialiased = false;
    backgroundLayer = currentLevel.LayerNamed ("Background");
    // CCTileMap is a CCLayer, so we'll just add it under all entities
    this.AddChild (currentLevel);
    // put the game layer after
    this.RemoveChild(gameplayLayer);
    this.AddChild(gameplayLayer);
    this.RemoveChild (hudLayer);
    this.AddChild (hudLayer);
}
```

합니다 `CCTileMap` 생성자에 전달 된 수준 수를 사용 하 여 생성 된 파일 이름을 `LoadLevel`입니다. 만들기 및 사용에 대 한 자세한 내용은 `CCTileMap` 인스턴스를 참조 합니다 [가이드 CocosSharp로 바둑판식 화면 사용](~/graphics-games/cocossharp/tiled.md)합니다.

현재 CocosSharp 없도록 계층 제거 하 고 해당 부모에 다시 추가 하지 않고 다시 정렬 `CCScene` (되는 `GameScene` 이 경우) 이므로 메서드의 마지막 몇 줄에는 계층의 순서를 변경 해야 합니다.


### <a name="createcollision"></a>CreateCollision

`CreateCollision` 메서드 구문을 `LevelCollision` 하는 데 사용 되는 인스턴스 *solid 충돌* 플레이어와 환경.


```csharp
private void CreateCollision()
{
    levelCollision = new LevelCollision();
    levelCollision.PopulateFrom(currentLevel);
}
```

이 충돌 없이 플레이어 수준을 통해 대체 하 고 게임은 재생할 수 없습니다. Solid 충돌 처음부터 방법을 플레이어 수 하 고 벽을 살펴보는 최대값이 통해 구성 되며, 이동 또는 플레이어를 방지 합니다.

추가 코드 없이 –만 파일을 수정 하려면 바둑판식으로 배열 된 동전 시간에서 충돌을 추가할 수 있습니다. 


### <a name="processtileproperties"></a>ProcessTileProperties

수준 로드 된 후 충돌 만들어지면 `ProcessTileProperties` 타일 속성을 기반으로 하는 논리를 수행 하기 위해 호출 됩니다. 주요 고려 사항인 시간 포함을 `PropertyLocation` 속성과 이러한 속성을 사용 하 여 타일의 좌표를 정의 하기 위한 구조체:


```csharp
public struct PropertyLocation
{
    public CCTileMapLayer Layer;
    public CCTileMapCoordinates TileCoordinates;
    public float WorldX;
    public float WorldY;
    public Dictionary<string, string> Properties;
}
```

이 구조체를 생성 하는 엔터티 인스턴스를 만들고에서 불필요 한 타일을 제거 합니다 `ProcessTileProperties` 메서드:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```

확인 키가 하나, 각 타일 속성을 평가 하는 foreach 루프 `EntityType` 또는 `RemoveMe`합니다. `EntityType` 엔터티 인스턴스를 만들어야 함을 나타냅니다. `RemoveMe` 런타임 시 타일을 완전히 제거 해야 한다는 것을 나타냅니다.

속성이 합니다 `EntityType` 키를 찾을 수, 한 다음 `TryCreateEntity` 라고, 일치 하는 속성을 사용 하 여 엔터티를 만들려고 시도 하는 `EntityType` 키:


```csharp
private bool TryCreateEntity(string entityType, float worldX, float worldY)
{
    CCNode entityAsNode = null;
    switch (entityType)
    {
    case "Player":
        player = new Player ();
        entityAsNode = player;
        break;
    case "Coin":
        Coin coin = new Coin ();
        entityAsNode = coin;
        coins.Add (coin);
        break;
    case "Door":
        door = new Door ();
        entityAsNode = door;
        break;
    case "Spikes":
        var spikes = new Spikes ();
        this.damageDealers.Add (spikes);
        entityAsNode = spikes;
        break;
    case "Enemy":
        var enemy = new Enemy ();
        this.damageDealers.Add (enemy);
        this.enemies.Add (enemy);
        entityAsNode = enemy;
        break;
    }
    if(entityAsNode != null)
    {
        entityAsNode.PositionX = worldX;
        entityAsNode.PositionY = worldY;
        gameplayLayer.AddChild (entityAsNode);
    }
    return entityAsNode != null;
}
```


## <a name="adding-new-entities"></a>새 엔터티를 추가합니다.

해당 게임 개체에 대 한 엔터티 패턴을 사용 하는 주요 고려 사항인 시간 (에 나와 있는 합니다 [CocosSharp의 엔터티 가이드](~/graphics-games/cocossharp/entities.md)). 상속 하는 모든 엔터티 `CCNode`, 즉, 자식으로 추가할 수는 `gameplayLayer`합니다.

각 엔터티 형식 목록 또는 단일 인스턴스를 통해 직접 참조도 됩니다. 예를 들어, 합니다 `Player` 에서 참조 하는 합니다 `player` 필드 및 모든 `Coin` 에서 참조 하는 인스턴스를 `coins` 목록. 엔터티에 대 한 직접 참조를 유지 합니다. (참조를 통해 달리는 `gameLayer.Children` 목록) 이러한 엔터티를 읽기 쉽게 액세스 하 고 잠재적으로 비용이 많이 드는 형식 캐스팅을 제거 하는 코드입니다.

기존 코드는 새 엔터티를 만드는 방법의 예제에서는 엔터티 형식의 숫자를 제공 합니다. 다음 단계를 새 엔터티를 만드는 데 사용할 수 있습니다.


### <a name="1---define-a-new-class-using-the-entity-pattern"></a>1-엔터티 패턴을 사용 하 여 새 클래스를 정의 합니다.

엔터티를 만들기 위한 유일한 요구 사항은에서 상속 되는 클래스를 만들 때 `CCNode`합니다. 대부분의 엔터티는 일부 시각적 개체를 같은 `CCSprite`, 생성자에서 엔터티의 자식으로 추가 해야 합니다.

CoinTime 제공 된 `AnimatedSpriteEntity` 애니메이션이 적용 된 엔터티를 생성을 간소화 하는 클래스입니다. 애니메이션에서 자세히 다룰 합니다 [애니메이션 엔터티 섹션](#animated-entities)합니다.


### <a name="2--add-a-new-entry-to-the-trycreateentity-switch-statement"></a>2-TryCreateEntity switch에 새 항목을 추가 합니다.

새 엔터티 인스턴스를 인스턴스화해야 합니다 `TryCreateEntity`합니다. 엔터티 충돌, AI, 또는 입력을 읽기와 같은 모든 프레임 논리가 필요한 경우 그런 다음 `GameScene` 개체에 대 한 참조를 유지 해야 합니다. 여러 인스턴스가 필요한 경우 (같은 `Coin` 또는 `Enemy` 인스턴스)를 클릭 하 고 새 `List` 에 추가 해야 합니다 `GameScene` 클래스입니다.


### <a name="3--modify-tile-properties-for-the-new-entity"></a>3-새 엔터티에 대 한 타일 속성을 수정 합니다.

코드에서는 새 엔터티가 생성 되 면 새 엔터티를 tileset에 추가 해야 합니다. 모든 수준에 열어 편집할 수는 tileset `.tmx` 파일입니다. 

tileset에서.tmx 별도로 저장 되는 **mastersheet.tsx** 파일을 편집할 수 전에 가져와야 하므로:

![](cointime-images/image11.png "편집할 수 전에 가져와야는 tileset.tsx 파일에서 별도 저장 되어")

가져올 속성 선택한 타일에는 편집할 수 고 EntityType을 추가할 수 있습니다.

![](cointime-images/image12.png "가져올 속성 선택한 타일에는 편집할 수 고 EntityType을 추가할 수 있습니다")

값 속성을 만든 후 새 일치 하도록 설정할 수 있습니다 `case` 에서 `TryCreateEntity`:

![](cointime-images/image13.png "속성을 만든 후 해당 값 TryCreateEntity에 새 사례와 일치 하도록 설정할 수 있습니다.")

tileset를 변경한 후 내보내야 하며 – 그러면 변경 내용을 다른 모든 수준에 대해 사용할 수 있습니다.

![](cointime-images/image14.png "tileset를 변경한 후 내보낸 여야 합니다.")

기존는 tileset 덮어쓸지 **mastersheet.tsx** tileset:

![](cointime-images/image15.png "그 tileset 기존 mastersheet.tsx tileset을 덮어써야 합니다.")


## <a name="entity-tile-removal"></a>엔터티 타일 제거

타일 지도 게임에 로드 되 면 개별 타일은 정적 개체입니다. 엔터티 이동 같은 사용자 지정 동작에 해야 하므로 엔터티를 만들 때 주요 고려 사항인 시간 코드 타일을 제거 합니다.

`ProcessTileProperties` 사용 하 여 엔터티를 만드는 타일을 제거 하는 논리가 포함 된 `RemoveTile` 메서드:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            ...
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        ...
    }
}
```

타일의 자동으로 제거 동전 등 적 tileset의 하나만 타일을 차지 하는 엔터티에 대 한 충분 한 경우 더 큰 엔터티 속성 및 추가 논리가 필요합니다.

문을 완전히 그릴 두 타일을 필요 합니다.

![](cointime-images/image16.png "문을 완전히 그릴 두 타일 필요")

엔터티를 만들기 위한 속성을 포함 하는 도어에 아래쪽 타일 (**EntityType** 로 설정 **도어**):

![](cointime-images/image17.png "도어에서 아래쪽 타일에는 EntityType 도어로 엔터티 만들기에 대 한 속성이 포함 되어 있습니다.")

문 아래쪽 타일만 도어 인스턴스가 만들어질 때을 제거는 런타임에 맨 위 타일을 제거 하려면 추가 논리가 필요 합니다. 맨 위 타일에는 **RemoveMe** 속성으로 설정 **true**:

![](cointime-images/image18.png "맨 위 타일에로 RemoveMe 속성이 true")



타일을 제거 하려면이 속성은 사용 `ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        ...
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```


## <a name="entity-offsets"></a>엔터티 오프셋

타일에서 만든 엔터티는 타일의 중심을 사용 하 여 엔터티의 가운데 정렬 하 여 배치 됩니다. 와 같은 큰 엔터티 `Door`를 올바르게 배치 될 추가 속성 및 논리를 사용 합니다. 

아래쪽 도어 타일을 정의 하는 합니다 `Door` 엔터티 배치를 지정을 **y 오프셋** 값 4입니다. 이 속성을 사용 하지 않고는 `Door` 인스턴스 타일의 가운데에 배치 됩니다.

![](cointime-images/image19.png "이 속성을 사용 하지 않고 도어 인스턴스 타일의 가운데에 배치 됩니다.")

 

이 적용 하 여 문제가 해결 되기 합니다 **y 오프셋** 값 `ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            ...
        }
...
    }
}
```


## <a name="animated-entities"></a>애니메이션이 적용 된 엔터티

주요 고려 사항인 시간 애니메이션이 적용 된 여러 엔터티를 포함합니다. `Player` 하 고 `Enemy` 워크 애니메이션을 재생 하는 엔터티 및 `Door` 모든 동전 수집한 후 엔터티 열기 애니메이션을 재생 합니다.


### <a name="achx-files"></a>.achx 파일

주요 고려 사항인 시간 애니메이션.achx 파일에 정의 됩니다. 간에 정의 된 각 애니메이션이 `AnimationChain` 태그에 정의 된 다음 애니메이션에 표시 된 대로 **propanimations.achx**:


```xml
<AnimationChain>
  <Name>Spikes</Name>
  <ColorKey>0</ColorKey>
  <Frame>
    <FlipHorizontal>false</FlipHorizontal>
    <FlipVertical>false</FlipVertical>
    <TextureName>..\images\mastersheet.png</TextureName>
    <FrameLength>0.1</FrameLength>
    <LeftCoordinate>1152</LeftCoordinate>
    <RightCoordinate>1168</RightCoordinate>
    <TopCoordinate>128</TopCoordinate>
    <BottomCoordinate>144</BottomCoordinate>
    <RelativeX>0</RelativeX>
    <RelativeY>0</RelativeY>
  </Frame>
</AnimationChain> 
```

이 애니메이션 정적 이미지를 표시 하는 급증 엔터티에 결과 단일 프레임을 포함 합니다. 엔터티는 단일 또는 다중 프레임 애니메이션 표시 여부를.achx 파일을 사용할 수 있습니다. 추가 프레임 코드에서 변경 하지 않고도.achx 파일에 추가할 수 있습니다. 

프레임에 표시할 이미지를 정의 합니다 `TextureName` 매개 변수 및 디스플레이의 좌표를 `LeftCoordinate`, `RightCoordinate`, `TopCoordinate`, 및 `BottomCoordinate` 태그입니다. 애니메이션-사용 되는 이미지의 프레임의 픽셀 좌표를 나타냅니다 **mastersheet.png** 이 경우.

![](cointime-images/image20.png "이러한 이미지에는 애니메이션의 프레임의 픽셀 좌표를 나타냅니다.")

`FrameLength` 속성 애니메이션 프레임을 표시 해야 하는 시간 (초) 수를 정의 합니다. 단일 프레임 애니메이션이이 값을 무시 합니다.

주요 고려 사항인 시간.achx 파일의 다른 모든 AnimationChain 속성은 무시 됩니다.


### <a name="animatedspriteentity"></a>AnimatedSpriteEntity

애니메이션 논리에 포함 되어는 `AnimatedSpriteEntity` 대부분의 엔터티 사용에 대 한 기본 클래스로 사용 되는 클래스는 `GameScene`합니다. 다음과 같은 기능을 제공합니다.

 - 로드 `.achx` 파일
 - 로드 된 애니메이션의 애니메이션 캐시
 - 애니메이션을 표시 하기 위한 CCSprite 인스턴스
 - 현재 프레임을 변경 하는 것에 대 한 논리

급증 생성자는 로드 하 고 애니메이션을 사용 하는 방법의 간단한 예제를 제공 합니다.


```csharp
public Spikes ()
{
    LoadAnimations ("Content/animations/propanimations.achx");
    CurrentAnimation = animations [0];
}
```

합니다 **propAnimations.achx** 생성자 인덱스로이 애니메이션에 액세스 하므로 애니메이션을 하나 포함 합니다. 여러 애니메이션을 포함 하는.achx 파일로 경우 애니메이션에 표시 된 대로 이름으로 참조할 수 있습니다는 `Enemy` 생성자:


```csharp
walkLeftAnimation = animations.Find (item => item.Name == "WalkLeft");
walkRightAnimation = animations.Find (item => item.Name == "WalkRight");
```


## <a name="summary"></a>요약

이 가이드에서는 동전 시간의 구현 세부 정보를 다룹니다. 주요 고려 사항인 시간 완전 한 게임을 할 만들어지지만 쉽게 수정 및 확장할 수 있는 프로젝트입니다. 판독기가 새 수준을 추가 하 고 자세히 동전 시간 구현 되는 방식을 이해 하기 위해 새 엔터티를 만드는 시간 수준에 대 한 수정을 지출 하는 것이 좋습니다.

## <a name="related-links"></a>관련 링크

- [게임 프로젝트로, 스토리 (샘플)](https://developer.xamarin.com/samples/mobile/CoinTime/)
