---
title: CocosSharp로 바둑판식 화면 사용
description: 바둑판식으로 배열은 강력 하 고 유연 하 고 완성 된 응용 프로그램인 부분도 있고 등각 타일을 만들기 위한 매핑합니다 게임에 대 한 키를 누릅니다. CocosSharp 타일의 네이티브 파일 형식에 대 한 기본 제공 통합을 제공합니다.
ms.prod: xamarin
ms.assetid: 804C042C-F62A-4E6C-B10F-06528637F0E2
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 8e7ef890af264bb08827d86c635d555184f1ec00
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672510"
---
# <a name="using-tiled-with-cocossharp"></a>CocosSharp로 바둑판식 화면 사용

_바둑판식으로 배열은 강력 하 고 유연 하 고 완성 된 응용 프로그램인 부분도 있고 등각 타일을 만들기 위한 매핑합니다 게임에 대 한 키를 누릅니다. CocosSharp 타일의 네이티브 파일 형식에 대 한 기본 제공 통합을 제공합니다._

*바둑판식* 응용 프로그램을 만들기 위한 표준 *지도 타일* 게임 개발에 사용 합니다. 이 가이드에서는 기존.tmx 파일 (타일에서 만든 파일)을 가져와 CocosSharp 게임에서 사용 하는 방법을 안내 합니다. 이 가이드에 구체적으로 설명 합니다.

 - 지도 타일의 용도
 - .Tmx 파일 작업
 - 픽셀 아트를 렌더링 하기 위한 고려 사항
 - 타일 속성을 사용 하 여 런타임 시

때 완료 해야 다음 데모:

![](tiled-images/image1.png "이 가이드의 단계를 수행 하 여 만들어진 데모 앱")


## <a name="the-purpose-of-tile-maps"></a>지도 타일의 용도

지도 타일에서는 지난 수십 년간 게임 개발에 존재 했을 하지만 효율성 및 esthetics 2D 게임에서 여전히 일반적으로 사용 됩니다. 지도 타일 매우 높은 수준의 사용을 통해 고객이 타일 집합 – 타일 맵에서 사용 되는 원본 이미지는 효율성을 달성할 수 있습니다. 타일 집합은 하나의 파일에 결합 하는 이미지의 컬렉션입니다. 타일 집합 타일 맵에서 사용 되는 이미지를 참조 하지만 여러 작은 이미지를 포함 하는 파일에는 스프라이트 시트 이라고 합니다. 또는 스프라이트 게임 개발에 매핑합니다. 타일 집합 데모에서 사용 하는 타일 집합을 표를 추가 하 여 사용 하는 방법을 시각화할 수 했습니다.

![](tiled-images/image2.png "시각화 타일 집합 데모에 사용할 타일 집합을 표를 추가 하 여 사용 하는 방법 보기")

타일 맵 타일 집합에서 개별 타일을 정렬합니다. 각 타일 지도 타일의 자체 복사본을 저장할 필요가 없습니다 드릴 set – 대신 여러 타일 맵이 참조할 수 동일한 타일 집합입니다. 이 타일 집합 외에도 타일 맵에서 필요 거의 메모리 것을 의미 합니다. 와 같은 큰 게임 플레이 영역을 만드는 데는 경우에 많은 타일 맵 만들 수 있도록이 [platformer 스크롤](https://en.wikipedia.org/wiki/Platform_game) 환경입니다. 다음은 동일한 타일 집합을 사용 하 여 가능한 배열.

![](tiled-images/image3.png "이 이미지에서는 동일한 타일 집합을 사용 하 여 가능한 정렬을 보여 줍니다.")

![](tiled-images/image4.png "이 이미지에서는 동일한 타일 집합을 사용 하 여 가능한 정렬을 보여 줍니다.")


## <a name="working-with-tmx-files"></a>.Tmx 파일 작업

.Tmx 파일 형식이 될 수 있는 타일 응용 프로그램에서 만든 XML 파일 [바둑판식 웹 사이트에서 무료로 다운로드할](http://www.mapeditor.org/)합니다. .Tmx 파일 형식 맵 타일에 대 한 정보를 저장합니다. 일반적으로 게임 수준 또는 별도 각 영역에 대해 하나의.tmx 파일을 해야 합니다.

이 가이드에서 CocosSharp; 기존.tmx 파일을 사용 하는 방법에 중점을 두고합니다 그러나 추가 자습서 있습니다 online [바둑판식 맵 편집기에 대 한 소개이 내용을](http://gamedevelopment.tutsplus.com/tutorials/introduction-to-tiled-map-editor--gamedev-2838)합니다.

압축을 풀 해야 합니다 [콘텐츠 zip 파일](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true) 게임에서 사용 하 합니다. 첫 번째 점은 맵 타일을.tmx 파일 (dungeon.tmx)를 사용 하 여 1 개 또는 타일을 정의 하는 자세한 이미지 파일 (dungeon_1.png) 데이터를 설정 합니다. 런타임에.tmx를 로드 하려면 이러한 파일을 모두 포함 해야 하는 게임 추가 프로젝트를 둘 다 **콘텐츠** 폴더 (에 포함 되어 있는 합니다 **자산** Android 프로젝트에서 폴더). 특히 라는 폴더에 파일을 추가 **tilemaps** 안에 **콘텐츠** 폴더:

![](tiled-images/image5.png "콘텐츠 폴더 안에 tilemaps 라는 폴더에 파일 추가")

합니다 **dungeon.tmx** 이제에 런타임 시 파일을 로드할 수는 `CCTileMap` 개체입니다. 다음으로 수정 합니다 `GameLayer` (또는 해당 루트 컨테이너 개체) 포함 하는 `CCTileMap` 인스턴스 및 자식으로 추가할:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");
        this.AddChild (tileMap);
    }
} 
```

살펴보겠습니다 게임 실행 하는 경우 타일 맵 화면 왼쪽 아래 모서리에 표시 됩니다.

![](tiled-images/image6.png "화면의 왼쪽 아래 모서리에서 해당 타일 맵이 표시 게임을 실행 하는 경우")


## <a name="considerations-for-rendering-pixel-art"></a>픽셀 아트를 렌더링 하기 위한 고려 사항

픽셀 아트, 비디오 게임 개발의 컨텍스트에서 일반적으로 직접에서 만들어지고 낮은 해상도 경우가 2D visual 아트를 가리킵니다. 픽셀 아트 제한적으로 수 시간이 많이 소요 만들어, 픽셀 아트 타일 집합은 종종 16 또는 32 픽셀 너비 및 높이 등과 같은 저해상도 타일을 포함 합니다. 런타임 시 크기를 조정 하지, 픽셀 아트는 대부분의 요즘 휴대폰 및 태블릿 용 너무 작아 합니다.

표시 된 차원에 대 한 호출 추가 게임의 GameAppDelegate.cs 파일에서 조정할 수 있습니다 `CCScene.SetDefaultDesignResolution`:


```csharp
 public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");

    CCSize windowSize = mainWindow.WindowSizeInPixels;

    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to be (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDefaultDesignResolution (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    
    // Determine whether to use the high or low def versions of our images
    // Make sure the default texel to content size ratio is set correctly
    // Of course you're free to have a finer set of image resolutions e.g (ld, hd, super-hd)
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }

    // New code:
    CCScene.SetDefaultDesignResolution (380, 240, CCSceneResolutionPolicy.ShowAll);

    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);

    mainWindow.RunWithScene (scene);
} 
```

대 한 자세한 내용은 `CCSceneResolutionPolicy`에서 지침을 참조 하십시오 [CocosSharp의 해결 방법 처리](~/graphics-games/cocossharp/resolutions.md)합니다.

게임, 이제 실행 하는 경우이 장치의 전체 화면을 게임 사용을 살펴보겠습니다.

![](tiled-images/image7.png "장치의 전체 화면을 게임 사용")

마지막으로 우리의 타일 맵에 앤티 앨리어싱 사용 하지 않도록 설정 하려고 합니다. `Antialiased` 속성 확대 되는 개체를 렌더링할 때 흐림 효과 적용 합니다. 앤티 앨리어싱 잡아 모양을 그래픽 개체를 줄이는 데 유용할 수 있지만 자체 렌더링 아티팩트를 생겨날 수 있습니다. 특히, 앤티 앨리어싱 각 타일의 콘텐츠를 흐리게합니다. 그러나 각 타일의 가장자리는 흐려지지 않습니다, 인접 한 타일을 사용 하 여 혼합 하는 대신 차별화 개별 타일 수 있습니다. 또한 드릴 픽셀 아트 게임에 종종 유지 관리를 잡아 모양을 유지를 *retro* 생각 합니다.

설정할 `Antialiased` 하 `false` 생성 한 후의 `tileMap`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();

    tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

    // new code:
    tileMap.Antialiased = false;

    this.AddChild (tileMap);
} 
```

이제 타일 지도가 흐리게 나타납니다.

![](tiled-images/image8.png "이제 타일 맵이 흐리게 표시 됩니다.")


## <a name="using-tile-properties-at-runtime"></a>타일 속성을 사용 하 여 런타임 시

지금는 `CCTileMap` .tmx 파일을 로드 하 고, 표시 하지만 상호 작용 하는 방법이 없습니다. 특히이 보물 상자) (예: 특정 타일 사용자 지정 논리를 포함 해야 합니다. 에서는 런타임에 한 번 식별 된 이러한 속성에 대응 하는 다양 한 방법 및 사용자 지정 타일 속성을 검색 하는 방법을 단계별로 실행 합니다.

모든 코드를 작성 했습니다 전에 속성 타일을 통해 타일 지도가을 추가 해야 합니다. 이렇게 하려면 타일 프로그램에서 dungeon.tmx 파일을 엽니다. 게임 프로젝트에서 사용 되는 파일을 열고 해야 합니다.

열기 되 면 해당 속성을 보려면 설정 타일에 보물 상자를 선택 합니다.

![](tiled-images/image9.png "열기 면 보물 나 속성을 보려면로 타일 선택")

보물 나 속성 나타나지 않으면 보물 나 마우스 오른쪽 단추로 클릭 하 고 선택 **타일 속성**:

![](tiled-images/image10.png "보물 나 속성 나타나지 않으면 보물 나 마우스 오른쪽 단추로 클릭 하 고 타일 속성을 선택")

바둑판식으로 배열 된 속성 이름과 값을 사용 하 여 구현 됩니다. 속성을 추가 하려면 클릭 합니다 **+** 단추에 이름을 입력 **IsTreasure**, 클릭 **확인**, 값을 입력 합니다 **true**: 

![](tiled-images/image11.png "속성을 추가 하려면 단추를 클릭, 이름을 입력 IsTreasure, 확인을 클릭 하 고 값이 true를 입력 합니다.")

파일을 저장할.tmx 속성을 수정한 후 두는 것을 잊지 마세요.

마지막으로,이 새로 추가 된 속성에 대 한 확인 하는 코드를 추가 합니다. 각 반복 됩니다 `CCTileMapLayer` (2 계층 지도가 있음), 각 행과 열 있는 모든 타일에 대 한 확인을 통해 다음을 `IsTreasure` 속성:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

        // new code:
        tileMap.Antialiased = false;

        this.AddChild (tileMap);

        HandleCustomTileProperties (tileMap);
    }

    void HandleCustomTileProperties(CCTileMap tileMap)
    {
        // Width and Height are equal so we can use either
        int tileDimension = (int)tileMap.TileTexelSize.Width;

        // Find out how many rows and columns are in our tile map
        int numberOfColumns = (int)tileMap.MapDimensions.Size.Width;
        int numberOfRows = (int)tileMap.MapDimensions.Size.Height;

        // Tile maps can have multiple layers, so let's loop through all of them:
        foreach (CCTileMapLayer layer in tileMap.TileLayersContainer.Children)
        {
            // Loop through the columns and rows to find all tiles
            for (int column = 0; column < numberOfColumns; column++)
            {
                // We're going to add tileDimension / 2 to get the position
                // of the center of the tile - this will help us in 
                // positioning entities, and will eliminate the possibility
                // of floating point error when calculating the nearest tile:
                int worldX = tileDimension * column + tileDimension / 2;
                for (int row = 0; row < numberOfRows; row++)
                {
                    // See above on why we add tileDimension / 2
                    int worldY = tileDimension * row + tileDimension / 2;

                    HandleCustomTilePropertyAt (worldX, worldY, layer);
                }
            }
        }
    }

    void HandleCustomTilePropertyAt(int worldX, int worldY, CCTileMapLayer layer)
    {
        CCTileMapCoordinates tileAtXy = layer.ClosestTileCoordAtNodePosition (new CCPoint (worldX, worldY));

        CCTileGidAndFlags info = layer.TileGIDAndFlags (tileAtXy.Column, tileAtXy.Row);

        if (info != null)
        {
            Dictionary<string, string> properties = null;

            try
            {
                properties = tileMap.TilePropertiesForGID (info.Gid);
            }
            catch
            {
                // CocosSharp 
            }

            if (properties != null && properties.ContainsKey ("IsTreasure") && properties["IsTreasure"] == "true" )
            {
                layer.RemoveTile (tileAtXy);

                // todo: Create a treasure chest entity
            }
        }
    }
} 
```

대부분의 코드는 쉽게 이해할 수 있지만 보물 타일의 처리에 논의 해야 했습니다. 이 경우 되 보물 상자로 식별 되는 모든 타일을 제거 합니다. 즉, 보물 상자 효과 충돌 하는 데 플레이어를 열면 보물 내용의 점수가 부여 런타임에 사용자 지정 코드 확인 해야 합니다. 보물에 반응 해야 할 수는 또한 (모양 변경)을 열고 적을 수 되었을 때 모든 화면 표시만 대 한 논리를 있을 수 있습니다.

즉, 보물 나 것이 좋고 중인 엔터티 대신 간단한 타일에서는 `CCTileMap`합니다. 게임 엔터티에 대 한 자세한 내용은 참조는 [CocosSharp의 엔터티 가이드](~/graphics-games/cocossharp/entities.md)합니다.


## <a name="summary"></a>요약

이 연습에서는 타일 CocosSharp 응용 프로그램으로 만든.tmx 파일을 로드 하는 방법에 설명 합니다. 앱 해상도 보다 낮은 해상도 픽셀 아트에 대 한 계정를 수정 하는 방법 및 해당 속성을 엔터티 인스턴스를 만드는 것과 사용자 지정 논리를 수행 하 여 타일을 찾는 방법을 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [바둑판식으로 배열 된 웹 사이트](http://www.mapeditor.org/)
- [콘텐츠 zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true)
