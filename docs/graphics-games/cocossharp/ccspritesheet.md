---
title: 프레임 속도와 CCSpriteSheet 향상
description: CCSpriteSheet 결합 하 고 한 텍스처에서 여러 이미지 파일을 사용 하 여 기능을 제공 합니다. 질감 수가 감소 게임의 로드 시간 및 프레임 속도 향상 시킬 수 있습니다.
ms.prod: xamarin
ms.assetid: A1334030-750C-4C60-8B84-1A8A54B0D00E
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 9487ddf5ccdb1d0caf820b10446eaff0f80a97ed
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="improving-frame-rate-with-ccspritesheet"></a>프레임 속도와 CCSpriteSheet 향상

_CCSpriteSheet 결합 하 고 한 텍스처에서 여러 이미지 파일을 사용 하 여 기능을 제공 합니다. 질감 수가 감소 게임의 로드 시간 및 프레임 속도 향상 시킬 수 있습니다._

대부분의 게임 최적화 노력을 원활 하 게 실행 하 고 모바일 하드웨어에 신속 하 게 로드할 필요 합니다. `CCSpriteSheet` 클래스 CocosSharp 게임에서 발생 하는 여러 가지 일반적인 성능 문제를 해결할 수 있습니다. 이 가이드에서는 일반적인 성능 문제 및 해결 하는 방법을 사용 하는 `CCSpriteSheet` 클래스입니다.


## <a name="what-is-a-sprite-sheet"></a>Sprite 시트 이란?

A *sprite 시트*, 있는 참조할 수로 한 *질감 atlas*, 하나의 파일을 여러 이미지를 결합 하는 이미지입니다. 이 콘텐츠 부하 시간 뿐 아니라 런타임 성능 개선.

예를 들어 다음 그림은 세 개의 별도 이미지에서 만든 단순 sprite 시트입니다. 개별 이미지의 규모 같고 결과 sprite 시트 완전히 채울 필요 하지 않습니다.

![](ccspritesheet-images/image1.png "개별 이미지에는 모든 크기 수 있으며 결과 sprite 시트 완전히 채울 필요 하지 않습니다.")


### <a name="render-states"></a>상태를 렌더링 합니다.

시각적 CocosSharp 개체 (예: `CCSprite`) 꼭 짓 점 버퍼 만들어야 MonoGame 또는 OpenGL 같은 기존의 그래픽 API 렌더링 코드를 통해 렌더링 코드를 단순화 하 (에 설명 된 대로 [3D 그래픽을 그리기 MonoGame의 꼭 짓 점](~/graphics-games/monogame/3d/part2.md) 가이드). CocosSharp은이 클래스는 단순 불구 하 고 설정의 비용을 제거 하지 않습니다 *상태 렌더링*, 질감 또는 기타 렌더링 관련 상태 렌더링 코드 전환 해야 하는 횟수는 합니다.

CocosSharp의 내부 코드 렌더링 각 시각적 요소를 순서 대로 현재로 시작 하는 시각적 트리를 이동 하 여 `CCScene`합니다. 예를 들어 다음 장면을 것이 좋습니다.

![](ccspritesheet-images/image2.png "이 장면 고려")

CocosSharp 하 게 순서로 별 4 개 렌더링 합니다.

![](ccspritesheet-images/image3.png "CocosSharp 게 시퀀스에 4 개의 별을 렌더링")

각 이후 `CCSprite` 동일한 질감이 사용 하 여 CocosSharp 그룹화 할 수 있는 모든 네 개의 별. 이 코드 프레임 마다 하나의 렌더링 상태 할당 (별모양 질감의 할당) 필요합니다. 이 시나리오는 매우 효율적입니다.

물론, 매우 적은 게임 하나의 이미지를 사용합니다. 다음 장면 지구 그래픽을 도입합니다.

![](ccspritesheet-images/image4.png "이 장면 소개 지구 그래픽")

이상적으로 CocosSharp 먼저 (예: 별) 하나의 이미지만 다른 이미지 (지구)를 사용 하 여 스프라이트의 나머지 부분에서는 사용 하는 모든 스프라이트 그릴지:

![](ccspritesheet-images/image5.png "이상적으로 CocosSharp 지구 다른 이미지를 사용 하 여 스프라이트의 나머지 부분에서는 다음 별 등 먼저 단일 이미지를 사용 하는 모든 스프라이트 그릴지")

상태를 렌더링 하는 두 개의 필요 위의 순서: 첫 번째 세계 첫 번째 별, 1 개에 하나씩 있습니다.

모든 `CCSprite` 인스턴스는 동일한 자식인 `CCNode`, 다음 CocosSharp 렌더링 상태 변경 사항을 줄이기 위해 그리기 순서를 최적화 합니다. 반대로, if는 `CCSprite` CocosSharp을 렌더링을 최적화할 수 없는 인스턴스 구성 됩니다 (여부 등의 일부인 다양 한 엔터티 `CCNode` 인스턴스)를 다음 순서 최적이 아닐 수 있습니다. 다음은 때문에 5 개의 렌더링 상태는 최악의 가능한 그리기 순서입니다.

![](ccspritesheet-images/image6.png "때문에 5 개의 렌더링 상태는 최악의 가능한 그리기 순서를 보여 줍니다.")

렌더링 상태 그리기 순서의 시각적 트리를 준수 해야 하기 때문에 최적화 하기 어려울 수 있습니다 `CCNode` 인스턴스. 이 트리 (예: 엔터티 시각적 자식에 포함 된)를 사용 하기 쉽게 되도록 구조화 된 또는 미술가가에 정의 된 대로 원하는 시각적 레이아웃으로 인해 구성 된 경우가 많습니다.

물론, 이상적인 상황은 단일 렌더링 상태로 여러 이미지 되더라도 응용 프로그램입니다. CocosSharp 게임이 하나의 해당 파일을 로드 한 다음 모든 이미지를 단일 파일로 결합 하 여 수행할 수 (그에 따른 함께 **.plist** 파일)에 `CCSpriteSheet`합니다. 사용 하는 `CCSpriteSheet` 클래스는 충족 하는 많은 수의 이미지 또는 충족 하는 매우 게임에 대 한 더욱 중요 한 레이아웃 복잡 합니다. 

### <a name="load-times"></a>로드 시간

또한 게임의 로드 시간에 대 한 여러 가지 이유로 수행 하면 파일 하나에 여러 이미지를 결합:

 - 단일 파일에 여러 이미지를 결합 효율적인 압축을 통해 사용 되는 픽셀의 전체 수를 줄일 수 있습니다.
 - 더 적은 파일 로드 파일 당 오버 헤드는 줄어들지만,.png 헤더를 구문 분석 하는 등
 - 작은 검색 시간이 중요 한 Dvd 및 기존 컴퓨터 하드 드라이브와 같은 미디어를 디스크 기반 필요 적은 파일 로드

## <a name="using-ccspritesheet-in-code"></a>코드에서 CCSpriteSheet 사용

만들려면는 `CCSpriteSheet` 인스턴스, 이미지 및 각 프레임에 사용할 이미지의 영역을 정의 하는 파일의 코드를 제공 해야 합니다. 이미지가 로드 될 수는 **.png** 또는 **.xnb** 파일 (사용 하는 경우는 [콘텐츠 파이프라인](~/graphics-games/cocossharp/content-pipeline/index.md)). 프레임을 정의 하는 파일은 한 **.plist** 를 직접 만들 수 있는 파일 또는 *TexturePacker* (있음 아래에 대해 설명 합니다).

샘플 응용 프로그램을는 [다운로드할 수 있습니다](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/), 만듭니다는 `CCSpriteSheet` 에서 **.png** 및 **.plist** 다음 코드를 사용 하 여 파일:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

로드 된 후의 `CCSpriteSheet` 포함 한 `List` 의 `CCSpriteFrame` 인스턴스 – 전체 시트를 만드는 데 소스 이미지 중 하나에 해당 하는 각 인스턴스. 경우에 **SpriteSheetDemo** 프로젝트는 `CCSpriteSheet` 세 개의 이미지를 포함 합니다. **.plist** Mac 용 Visual Studio에서 나 사용할 수 있는 이미지를 확인 하려면 텍스트 편집기에서 파일을 검사할 수 있습니다. 보고 하는 경우는 **.plist** 세 프레임 (섹션 키 이름은 강조 하기 위해 생략)을 볼 수 있습니다 텍스트 편집기에서 파일:


```csharp
...
<dict>
    <key>frames</key>
    <dict>
        <key>farBackground.png</key>
        ...
        <key>foreground.png</key>
        ...
<key>forestBackground.png</key>
...
```

사용할 수는 `Find` 프레임을 찾으려면 이름순으로 다음과 같이 메서드 (강조 하기 위해 코드 생략 `CCSpriteFrame` 사용):


```csharp
CCSpriteFrame frame;
...
frame = sheet.Frames.Find(item=>item.TextureFilename == "farBackground.png"); 
CCSprite sprite = new CCSprite (frame); 
...
```

이후는 `CCSprite` 생성자 가져올 수 있습니다는 `CCSpriteFrame` 매개 변수를 코드에 대 한 세부 정보를 조사 하는 `CCSpriteFrame`, 어떤 질감 등 사용 하 여, 또는 마스터 sprite 시트에 있는 이미지의 영역입니다.


## <a name="creating-a-sprite-sheet-plist"></a>Sprite 시트.plist 만들기

.Plist 파일이 xml 기반 파일 생성 하 고 직접 편집할 수 있습니다. 마찬가지로, 하나의 큰 파일에 여러 파일을 결합 하 편집 프로그램 이미지를 사용할 수 있습니다. 페이지를 만들고 sprite 시트 시간이 매우 오래 걸릴 수 유지 관리, 이후 CocosSharp 형식의 파일을 내보낼 수 있는 TexturePacker 프로그램에서 살펴보겠습니다. TexturePacker 무료 및를 "프로" 버전으로 제공 되며 Windows 및 Mac OS에 사용할 수 있습니다. 이 가이드의 나머지 부분에서는 무료 버전을 사용 하 여 수행할 수 있습니다. 

TexturePacker 수 [TexturePacker 웹 사이트에서 다운로드할](https://www.codeandweb.com/texturepacker)합니다. 열 때 TexturePacker 프로젝트를 로드 않아도 됩니다. 시작 화면을 사용 하면 최근에 사용한 프로젝트 (다른 프로젝트 생성 된) 경우 스프라이트 추가 열고 sprite 시트에 사용할 형식을 선택 수 있습니다. CocosSharp는 Cocos2D 데이터 형식을 사용합니다.

![](ccspritesheet-images/image7.png "CocosSharp는 Cocos2D 데이터 형식을 사용합니다.")

이미지 파일 (예: **.png**) 끌어서 놓기로 Windows 탐색기에서 Windows 또는 mac Finder에서 TexturePacker에 추가할 수 있습니다 파일을 추가할 때마다 TexturePacker sprite 시트 미리 보기를 자동으로 업데이트 합니다.

![](ccspritesheet-images/image8.png "파일을 추가할 때마다 TexturePacker sprite 시트 미리 보기를 자동으로 업데이트")

Sprite 시트를 내보내려면 클릭는 **게시 sprite 시트** 단추 고 sprite 시트에 대 한 위치를 선택 합니다. TexturePacker는.plist 파일 및 이미지 파일에 저장 됩니다.

결과 파일을 사용 하려면.png와.plist CocosSharp 프로젝트에 추가 합니다. CocosSharp 프로젝트에 파일 추가 대 한 자세한 내용은 참조는 [BouncingGame 가이드](~/graphics-games/cocossharp/bouncing-game.md)합니다. 파일을 추가한 후에 로드 될 수는 `CCSpriteSheet` 위 코드의 앞부분에 표시 된 대로:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

### <a name="considerations-for-maintaining-a-texturepacker-sprite-sheet"></a>TexturePacker sprite 시트를 유지 관리 하기 위한 고려 사항

게임 개발 되 예술가 수 추가, 제거 또는 아트 수정 합니다. 변경 내용은 업데이트 된 sprite 시트가 필요합니다. 다음 고려 사항이 sprite 시트 유지 관리를 쉬워질 수 있습니다.

 - 프로젝트의 폴더에는 원본 파일 (sprite 시트를 만드는 데 파일)를 유지 하 고 버전 제어에 추가 되어 있는지 확인 합니다. 이러한 파일은 변경 될 때마다 sprite 시트를 다시 만드는 데 필요 합니다.
 - 원본 파일을 Mac/Visual Studio 용 Visual Studio에 추가 하지 않거나 추가 된 경우 설정의 **빌드 작업** 를 **None**합니다. 경우 파일이 추가 되 고 있는 플랫폼 특정 **빌드 작업**, 결과 응용 프로그램의 파일 크기가 늘어납니다 불필요 하 게 합니다.
 - 사용 하는 것이 좋습니다 *폴더 스마트* TexturePacker에 있습니다. 스마트 폴더 sprite 시트에 포함 된 이미지를 자동으로 추가합니다. 이 실패로 이미지의 다 수 포함 된 많은 경우 게임을 개발 하는 시간을 절약할 수 있습니다. 

    ![](ccspritesheet-images/image9.png "이 기능은 이미지의 다 수 포함 된 많은 경우 게임을 개발 하는 시간을 저장할 수 있습니다.")
 - Sprite 시트 질감 크기에 유의 합니다. 일부 이전 전화 하드웨어 2048 x 2048 보다 큰 질감 크기를 지원 하지 않습니다. 또한, 2048 x 2048의 32 비트 이미지를 거의 17 (mb) ram – 많은 양의 메모리를 사용합니다.
 - TexturePacker 포함 되지 않습니다 폴더 sprite 이름에 기본적으로 있으므로 이름 충돌은 가능한. 이름을 폴더를 포함 하려면 여부 개발의 시작 부분에 결정 하는 것이 좋습니다. 더 큰 게임 충돌을 방지 하려면 폴더 이름을 사용 하 여 고려해 야 합니다. 폴더 경로 포함 하려면 클릭 **고급 표시** 에 **데이터** 섹션 및 확인 **Prepend 폴더 이름**합니다. 

    ![](ccspritesheet-images/image10.png "폴더 경로 포함 하려면 클릭 데이터 섹션에 설명 된 고급 표시 및 Prepend 폴더 이름 확인")

## <a name="summary"></a>요약

이 가이드에서는 만들고 사용 하는 방법을 설명는 `CCSpriteSheet` 클래스입니다. 에 로드할 수 있는 파일을 생성 하는 방법에 대해서도 설명 `CCSpriteSheet` TexturePacker 프로그램을 사용 하 여 인스턴스.

## <a name="related-links"></a>관련된 링크

- [CCSpriteSheet](https://developer.xamarin.com/api/type/CocosSharp.CCSpriteSheet/)
- [전체 데모 (샘플)](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)
