---
title: 사용 하 여 CocosSharp와 바둑판식으로 배열
description: 바둑판식으로 배열은 강력한 유연 하 고, 하며 직교 및 아이소메트릭 타일을 만들기 위한 완성 된 응용 프로그램 게임에 대 한 매핑 CocosSharp 타일의 기본 파일 형식에 대 한 기본 제공 통합을 제공합니다.
ms.prod: xamarin
ms.assetid: 804C042C-F62A-4E6C-B10F-06528637F0E2
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: a356ddc0412eb1dce1b35e060e6c9127525de804
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="using-tiled-with-cocossharp"></a>사용 하 여 CocosSharp와 바둑판식으로 배열

_바둑판식으로 배열은 강력한 유연 하 고, 하며 직교 및 아이소메트릭 타일을 만들기 위한 완성 된 응용 프로그램 게임에 대 한 매핑 CocosSharp 타일의 기본 파일 형식에 대 한 기본 제공 통합을 제공합니다._

*타일* 응용 프로그램을 만들기 위한 표준 *맵 타일* 게임 개발에 사용 합니다. 이 가이드에서는 기존.tmx 파일 (타일에서 만든 파일)를 수행 하 고 CocosSharp 게임에서 사용 하는 방법을 단계별로 안내 합니다. 특히이 가이드에서는 설명 합니다.

 - 지도 타일의 목적
 - .Tmx 파일 작업
 - 픽셀 아트를 렌더링 하기 위한 고려 사항
 - 타일 속성을 사용 하 여 런타임 시

때 완료 되는 했으므로 다음 데모:

![](tiled-images/image1.png "이 가이드의 단계에 따라 만든 데모 응용 프로그램")


## <a name="the-purpose-of-tile-maps"></a>지도 타일의 목적

타일 지도 년 게임 개발에 있었을 있지만 효율성과 esthetics에 2D 게임에서 여전히 일반적으로 사용 됩니다. 지도 타일 매우 높은 수준의 타일 집합이 – 맵 타일에서 사용 하는 원본 이미지의 사용을 통해 효율성을 얻을 수 있습니다. 타일 집합은 하나의 파일로 결합 하는 이미지의 컬렉션입니다. 타일 집합 타일 맵에서 사용 되는 이미지를 참조 하는 하지만 여러 개의 더 작은 이미지를 포함 하는 파일은 sprite 시트 라고도 또는 sprite 게임 개발에 매핑합니다. 사용 방법의 데모에 사용할 예정 된 타일 집합에는 표를 추가 하 여 타일 집합을 시각화할 수 있습니다.

![](tiled-images/image2.png "데모에 사용 되는 타일 집합에는 표를 추가 하 여 타일 집합이 사용 되는 방법의 시각화 된 보기")

지도 타일 타일 집합에서 각 타일을 정렬합니다. 각 타일 지도 타일의 자체 복사본을 저장할 필요가 없습니다 드릴 설정 – 대신 여러 타일 맵을 참조할 수 동일한 타일 집합입니다. 이 타일 집합 외에도 타일 지도 필요 매우 적은 양의 메모리 것을 의미 합니다. 사용 되는 같은 큰 게임 플레이 영역을 만들 경우에 많은 수의 타일 지도 만들 수 있도록이 [platformer 스크롤](http://en.wikipedia.org/wiki/Platform_game) 환경입니다. 다음은 동일한 타일 집합을 사용 하 여 가능한 배열입니다.

![](tiled-images/image3.png "이 이미지는 동일한 타일 집합을 사용 하 여 가능한 정렬 작업")

![](tiled-images/image4.png "이 이미지는 동일한 타일 집합을 사용 하 여 가능한 정렬 작업")


## <a name="working-with-tmx-files"></a>.Tmx 파일 작업

.Tmx 파일 형식이 될 수 있는 타일 응용 프로그램에서 만든 XML 파일 [타일 웹 사이트에서 무료로 다운로드할](http://www.mapeditor.org/)합니다. .Tmx 파일 형식 지도 타일에 대 한 정보를 저장합니다. 일반적으로 게임을 각 수준 또는 별도 영역에 대해 하나의.tmx 파일을 갖습니다.

이 가이드에서는 기존.tmx CocosSharp; 파일을 사용 하는 방법에 중점을 둡니다. 그러나 추가 자습서 있습니다 포함 하 여 온라인 [타일 맵 편집기가이 소개](http://gamedevelopment.tutsplus.com/tutorials/introduction-to-tiled-map-editor--gamedev-2838)합니다.

압축을 풀 해야는 [콘텐츠 zip 파일](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true) 게임에서 사용 하도록 합니다. 첫 번째은 타일 지도.tmx 파일 (dungeon.tmx)를 사용 하 여 하나 뿐 아니라 또는 타일을 정의 하는 자세한 이미지 파일 (dungeon_1.png) 데이터를 설정 합니다. 런타임 시.tmx 로드할 이러한 파일을 모두 포함 하 여 게임 해야 하므로 해당 프로젝트의 둘 다 추가 **콘텐츠** 폴더 (에 포함 되어 있는 **자산** Android 프로젝트의 폴더). 특히, 라는 폴더에 파일을 추가할 **tilemaps** 내는 **콘텐츠** 폴더:

![](tiled-images/image5.png "파일을 콘텐츠 폴더에 저장 tilemaps 라고 하는 폴더에 추가")

**dungeon.tmx** 이제에 런타임에 파일을 로드할 수는 `CCTileMap` 개체입니다. 다음으로 수정는 `GameLayer` (또는 해당 하는 루트 컨테이너 개체)를 포함 하는 `CCTileMap` 인스턴스 및 자식으로 추가 하려면:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");
        this.AddChild (tileMap);
    }
} 
```

살펴보게 되겠지만 게임을 실행 하는 경우 타일 맵 화면 왼쪽 아래 모서리에 표시 됩니다.

![](tiled-images/image6.png "화면의 왼쪽 아래에 해당 타일 맵이 표시 게임을 실행 하는 경우")


## <a name="considerations-for-rendering-pixel-art"></a>픽셀 아트를 렌더링 하기 위한 고려 사항

비디오 게임 개발의 컨텍스트에서 픽셀 아트 일반적으로 보유 하 여 만들어지고는 종종 낮은 해상도 2D visual 아트를 참조 합니다. 픽셀 아트 제한적으로 수 시간 픽셀 아트 타일 집합 보통 16 또는 32 픽셀 너비와 높이 등 저해상도 타일을 포함 하므로 만들기를 많이 사용 합니다. 런타임 시 확장 되지 면 픽셀 아트가 대부분의 휴대폰 및 태블릿에 비해 너무 작습니다.

표시 된 차원에 대 한 호출 추가 여기서 다루는 게임 GameAppDelegate.cs 파일에서 사용자가 조정할 수 `CCScene.SetDefaultDesignResolution`:


```csharp
 public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");

    CCSize windowSize = mainWindow.WindowSizeInPixels;

    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to be (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDefaultDesignResolution (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    
    // Determine whether to use the high or low def versions of our images
    // Make sure the default texel to content size ratio is set correctly
    // Of course you're free to have a finer set of image resolutions e.g (ld, hd, super-hd)
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }

    // New code:
    CCScene.SetDefaultDesignResolution (380, 240, CCSceneResolutionPolicy.ShowAll);

    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);

    mainWindow.RunWithScene (scene);
} 
```

대 한 자세한 내용은 `CCSceneResolutionPolicy`의 지침을 참조 [CocosSharp 해결 방법을 처리](~/graphics-games/cocossharp/resolutions.md)합니다.

게임을 지금 실행 게임 차지 장치의 전체 화면 보겠습니다.

![](tiled-images/image7.png "게임 차지 장치의 전체 화면")

마지막으로 우리의 타일 지도에 앤티 앨리어싱 사용 하지 않도록 설정 해야 합니다. `Antialiased` 속성 확대 되는 개체를 렌더링할 때 흐림 효과 적용 합니다. 앤티 앨리어싱 그래픽 개체의 픽셀 모양을 줄이는 데 유용할 수 있지만 자체 렌더링 아티팩트를도 발생할 수 있습니다. 특히, 앤티 앨리어싱 각 타일의 콘텐츠를 흐리게합니다. 그러나 각 타일의 가장자리 하지 흐려지는, 인접 한 타일을 사용 하 여 혼합 하는 대신 강조 개별 타일 수 있게 해줍니다. 또한 드릴 픽셀 아트 게임에 종종의 픽셀 모양을 유지 하기 위해 보존 한 *복고풍* 생각 합니다.

설정 `Antialiased` 를 `false` 생성 한 후의 `tileMap`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();

    tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

    // new code:
    tileMap.Antialiased = false;

    this.AddChild (tileMap);
} 
```

이제 우리 타일 맵 흐리게 표시 됩니다.

![](tiled-images/image8.png "이제 타일 맵이 흐리게 표시 됩니다.")


## <a name="using-tile-properties-at-runtime"></a>타일 속성을 사용 하 여 런타임 시

지금까지 했으므로 `CCTileMap` .tmx 파일을 로드 하 고, 표시 했지만 상호 작용할 수 있는 방법이 없습니다. 특히, 우리의 보물 상자) (예: 특정 타일 사용자 지정 논리를가 필요 합니다. म 합니다 속성 사용자 지정 타일 및 한 번 런타임에 결정 된 이러한 속성에 대응 하는 다양 한 방법으로 검색 하는 방법을 단계별로 설명 합니다.

전에 코드를 작성할 것 우리의 타일 맵 타일을 통해에 속성을 추가 해야 합니다. 이렇게 하려면 타일 프로그램에서 dungeon.tmx 파일을 엽니다. 게임 프로젝트에서 사용 되는 파일을 열어 해야 합니다.

열기 되 면 해당 속성을 볼 설정 타일에 보물 상자를 선택 합니다.

![](tiled-images/image9.png "열기 되 면 해당 속성을 볼 설정 타일에 보물 상자 선택")

보물 상자 속성 표시 되지 않으면 보물 상자를 마우스 오른쪽 단추로 클릭 하 고 선택 **타일 속성이**:

![](tiled-images/image10.png "보물 상자 속성 표시 되지 않으면 보물 상자를 마우스 오른쪽 단추로 클릭 하 고 타일 속성을 선택")

바둑판식으로 배열 된 속성 이름 및 값을 사용 하 여 구현 됩니다. 속성을 추가 하려면 클릭는 **+** 단추를 이름을 입력 **IsTreasure**, 클릭 **확인**, 다음 값을 입력 **true**: 

![](tiled-images/image11.png "속성을 추가 하려면 단추를 클릭, 입력 IsTreasure 이름, 확인을 클릭 한 다음 입력 값이 true")

파일을 저장할.tmx 속성을 수정한 후 두는 것을 잊지 마십시오.

마지막으로,이 새로 추가 된 속성에 대 한 조회 하는 코드를 추가 합니다. 각 루프를 실행 합니다 `CCTileMapLayer` (우리의 지도 계층 2), 각 행과 열을 포함 하는 모든 타일에 대 한 확인을 통해 다음의 `IsTreasure` 속성:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

        // new code:
        tileMap.Antialiased = false;

        this.AddChild (tileMap);

        HandleCustomTileProperties (tileMap);
    }

    void HandleCustomTileProperties(CCTileMap tileMap)
    {
        // Width and Height are equal so we can use either
        int tileDimension = (int)tileMap.TileTexelSize.Width;

        // Find out how many rows and columns are in our tile map
        int numberOfColumns = (int)tileMap.MapDimensions.Size.Width;
        int numberOfRows = (int)tileMap.MapDimensions.Size.Height;

        // Tile maps can have multiple layers, so let's loop through all of them:
        foreach (CCTileMapLayer layer in tileMap.TileLayersContainer.Children)
        {
            // Loop through the columns and rows to find all tiles
            for (int column = 0; column < numberOfColumns; column++)
            {
                // We're going to add tileDimension / 2 to get the position
                // of the center of the tile - this will help us in 
                // positioning entities, and will eliminate the possibility
                // of floating point error when calculating the nearest tile:
                int worldX = tileDimension * column + tileDimension / 2;
                for (int row = 0; row < numberOfRows; row++)
                {
                    // See above on why we add tileDimension / 2
                    int worldY = tileDimension * row + tileDimension / 2;

                    HandleCustomTilePropertyAt (worldX, worldY, layer);
                }
            }
        }
    }

    void HandleCustomTilePropertyAt(int worldX, int worldY, CCTileMapLayer layer)
    {
        CCTileMapCoordinates tileAtXy = layer.ClosestTileCoordAtNodePosition (new CCPoint (worldX, worldY));

        CCTileGidAndFlags info = layer.TileGIDAndFlags (tileAtXy.Column, tileAtXy.Row);

        if (info != null)
        {
            Dictionary<string, string> properties = null;

            try
            {
                properties = tileMap.TilePropertiesForGID (info.Gid);
            }
            catch
            {
                // CocosSharp 
            }

            if (properties != null && properties.ContainsKey ("IsTreasure") && properties["IsTreasure"] == "true" )
            {
                layer.RemoveTile (tileAtXy);

                // todo: Create a treasure chest entity
            }
        }
    }
} 
```

대부분의 코드는 이해 하기 쉬우며, 하지만 보물 타일의 처리에 논의 해야 했습니다. 이 경우 보물 상자로 식별 하는 모든 타일 제거 합니다. 즉, 보물 상자 효과 충돌 하 고 열 때 보물의 내용의 플레이어 수 런타임에 사용자 지정 코드 확인 해야 합니다. 보물 중인에 반응 해야 할 수는 또한 (시각적인 모양을 변경)을 열고 적 패배 했습니다 모든 화면 표시 되는 경우에에 대 한 논리가 있을 수 있습니다.

보물 상자 엔터티 되 고 이라기 보다는에서 간단한 타일에서 이익을 얻을 즉,는 `CCTileMap`합니다. 게임 엔터티에 대 한 자세한 내용은 참조는 [CocosSharp의 엔터티 안내](~/graphics-games/cocossharp/entities.md)합니다.


## <a name="summary"></a>요약

이 연습에서는 CocosSharp 응용 프로그램에 타일에서 만든.tmx 파일을 로드 하는 방법을 설명 합니다. 엔터티 인스턴스를 만드는 것과 사용자 지정 논리를 수행 하려면 해당 속성에 의해 타일을 확인 하는 방법 및 저해상도 픽셀 아트에 대 한 설명 하기 위해 앱 해상도 수정 하는 방법을 보여 줍니다.

## <a name="related-links"></a>관련된 링크

- [바둑판식으로 배열 된 웹 사이트](http://www.mapeditor.org/)
- [콘텐츠 zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true)
