---
title: CCSpriteSheet 사용 하 여 프레임 속도 향상합니다.
description: CCSpriteSheet를 결합 하 고 하나의 질감의 여러 이미지 파일을 사용 하 여 기능을 제공 합니다. 텍스처 수를 줄이면 게임의 로드 시간 및 프레임 속도 개선할 수 있습니다.
ms.prod: xamarin
ms.assetid: A1334030-750C-4C60-8B84-1A8A54B0D00E
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: eab6153653a8c8df2068aaaf879d84d35473c541
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61245692"
---
# <a name="improving-frame-rate-with-ccspritesheet"></a>CCSpriteSheet 사용 하 여 프레임 속도 향상합니다.

_CCSpriteSheet를 결합 하 고 하나의 질감의 여러 이미지 파일을 사용 하 여 기능을 제공 합니다. 텍스처 수를 줄이면 게임의 로드 시간 및 프레임 속도 개선할 수 있습니다._

많은 게임을 원활 하 게 실행 하 고 모바일 하드웨어에서 신속 하 게 로드 최적화 노력 해야 합니다. `CCSpriteSheet` 클래스 CocosSharp 게임에서 발생 하는 여러 가지 일반적인 성능 문제를 해결 하는 데 도움이 수 있습니다. 이 가이드에서는 일반적인 성능 문제 및 해결 하는 방법을 사용 하는 `CCSpriteSheet` 클래스입니다.


## <a name="what-is-a-sprite-sheet"></a>스프라이트 시트 란?

A *스프라이트 시트*는 수 수 라고도 *텍스처 지도*, 하나의 파일에 여러 이미지를 결합 하는 이미지인 합니다. 이 콘텐츠 로드 시간 뿐만 아니라 런타임 성능을 개선할 수 있습니다.

예를 들어, 다음 그림은 세 개의 별도 이미지에서 만든 간단한 스프라이트 시트입니다. 개별 이미지 크기에 관계 없이 되며 결과 스프라이트 시트 완전히 채울 필요 하지 않습니다.

![](ccspritesheet-images/image1.png "개별 이미지에는 크기에 관계 없이 수 있으며 결과 스프라이트 시트 완전히 채울 필요 하지 않습니다.")


### <a name="render-states"></a>상태를 렌더링 합니다.

CocosSharp의 시각적 개체 (같은 `CCSprite`) 꼭 짓 점 버퍼를 생성 해야 하는 OpenGL, MonoGame 등 일반적인 그래픽 API 렌더링 코드를 통해 렌더링 코드를 단순화 (에 설명 된 대로 [3D 그래픽 그리기 MonoGame의 꼭 짓 점](~/graphics-games/monogame/3d/part2.md) 가이드). CocosSharp은 간소에도 불구 하 고 설정의 비용을 제거 하지 않습니다 *상태 렌더링*, 렌더링 코드 질감 또는 기타 렌더링 관련 상태 전환 해야 하는 횟수는 합니다.

CocosSharp의 내부 코드 요소를 렌더링 각 visual 순차적으로 현재 시작 하는 시각적 트리를 이동 하 여 `CCScene`입니다. 예를 들어, 다음 장면을 것이 좋습니다.

![](ccspritesheet-images/image2.png "이 장면을 것이 좋습니다.")

CocosSharp 순서로 4 개의 별표 렌더링 하는:

![](ccspritesheet-images/image3.png "CocosSharp 별 4 개를 렌더링 하는 순서로")

이후 각 `CCSprite` 여 같은 텍스처를 사용 하 여 CocosSharp 모든 별 4 개를 함께 그룹화 할 수 있습니다. 이 코드는 프레임 마다 하나의 렌더링 상태 할당 (스타 질감의 할당) 필요합니다. 이 시나리오는 매우 효율적입니다.

물론 거의 게임 하나의 이미지를 사용합니다. 다음 장면 지구 그래픽을 제공합니다.

![](ccspritesheet-images/image4.png "이 장면에서는 전 세계 그래픽")

이상적으로 CocosSharp 먼저 (별표)를 하나의 이미지를 다른 이미지 (지구)를 사용 하 여 스프라이트의 나머지 부분을 사용 하 여 모든 스프라이트를 그려야 합니다.

![](ccspritesheet-images/image5.png "이상적으로 CocosSharp 별표, 전 세계 다른 이미지를 사용 하 여 스프라이트 나머지 다음 같은 하나의 이미지를 처음 사용 하는 모든 스프라이트를 그려야")

상태를 렌더링 하는 두 개의 필요 위의 순서: 전 세계적으로 첫 번째 별, 첫 번째에 있습니다.

모든 `CCSprite` 인스턴스는 동일한 자식 `CCNode`, 다음 CocosSharp 렌더링 상태 변경을 위해 그리기 순서를 최적화 합니다. 반면, if, 합니다 `CCSprite` 인스턴스 CocosSharp를 렌더링을 최적화할 수 있도록 구성 됩니다 (다른 엔터티의 일부인 경우와 같이 `CCNode` 인스턴스)를 최적의 순서 설정 되지 않을 수 있습니다. 다음은 5 렌더링 상태의 결과 최악의 가능한 그리기 순서입니다.

![](ccspritesheet-images/image6.png "5 렌더링 상태의 결과 최악의 가능한 그리기 순서를 보여 줍니다.")

렌더링 상태를 그리기 순서의 시각적 트리를 준수 해야 하기 때문에 최적화 하기 어려울 수 있습니다 `CCNode` 인스턴스. 종종이 트리 (예: 엔터티 시각적 자식이 포함)를 사용 하기 쉬운 구조화 되었거나 아티스트에 정의 된 대로 원하는 시각적 레이아웃으로 인해 구성 합니다.

물론 가장 바람직한 상황은 여러 이미지 불구 하 고 단일 렌더링 상태를 것입니다. CocosSharp 게임 하나의 파일을 로드 한 다음 모든 이미지를 단일 파일로 결합 하 여 수행할 수 (해당 곁들여서 **.plist** 파일)에 `CCSpriteSheet`합니다. 사용 하는 `CCSpriteSheet` 클래스는 많은 수의 이미지를 포함 하는 또는 매우 있는 게임에 대 한 더욱 중요 한 레이아웃 복잡 합니다. 

### <a name="load-times"></a>로드 시간

게임의 로드 시간을 다양 한 이유로 속도도 파일 하나에 여러 이미지를 결합:

 - 여러 이미지를 단일 파일로 결합 효율적인 압축을 통해 사용 하는 픽셀의 전체 수를 줄일 수 있습니다.
 - 더 적은 파일 로드 파일당 오버 헤드는 줄어들지만,.png 헤더를 구문 분석 하는 등
 - 디스크 기반 미디어 Dvd 및 기존 컴퓨터 하드 드라이브와 같은 중요 한 경우를 검색 하는 작은 필요한 적은 파일 로드

## <a name="using-ccspritesheet-in-code"></a>CCSpriteSheet를 사용 하 여 코드에서

만들려는 `CCSpriteSheet` 인스턴스, 이미지 및 각 프레임에 사용할 이미지의 영역을 정의 하는 파일 코드를 제공 해야 합니다. 로 이미지를 로드할 수 있습니다는 **.png** 하거나 **.xnb** 파일 (사용 하는 경우는 [콘텐츠 파이프라인](~/graphics-games/cocossharp/content-pipeline/index.md)). 프레임을 정의 하는 파일은는 **.plist** 를 직접 만들 수 있는 파일 또는 *TexturePacker* (아래 설명 하겠습니다)입니다.

샘플 앱입니다 [에서 다운로드할 수 있습니다](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)를 만듭니다를 `CCSpriteSheet` 에서 **.png** 및 **.plist** 다음 코드를 사용 하 여 파일:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

한 번 로드는 `CCSpriteSheet` 포함을 `List` 의 `CCSpriteFrame` 인스턴스 – 전체 시트를 만드는 데 원본 이미지 중 하나에 해당 하는 각 인스턴스에 합니다. 경우에 **SpriteSheetDemo** 프로젝트를 `CCSpriteSheet` 세 개의 이미지가 포함 되어 있습니다. 합니다 **.plist** Mac 용 Visual Studio 또는 임의의 텍스트 편집기를 사용할 수 있는 이미지 파일을 검사할 수 있습니다. 보면 합니다 **.plist** 세 프레임 (섹션 키 이름은 강조 하기 위해 생략)를 보면 텍스트 편집기에서 파일:


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

사용할 수 있습니다 합니다 `Find` 다음과 같은 이름으로 프레임을 찾는 방법 (코드 강조 하기 위해 생략 `CCSpriteFrame` 사용):


```csharp
CCSpriteFrame frame;
...
frame = sheet.Frames.Find(item=>item.TextureFilename == "farBackground.png"); 
CCSprite sprite = new CCSprite (frame); 
...
```

하므로 `CCSprite` 생성자 가져올 수 있습니다를 `CCSpriteFrame` 매개 변수를 코드의 세부 정보를 조사 하려면 하지에 `CCSpriteFrame`는 텍스처와 같은 사용, 또는 마스터 스프라이트 시트에는 이미지의 영역입니다.


## <a name="creating-a-sprite-sheet-plist"></a>스프라이트 시트.plist 만들기

.Plist 파일이 생성 되어으로 직접 편집할 수는 xml 기반 파일입니다. 마찬가지로, 하나의 큰 파일로 결합 하는 여러 파일을 편집 프로그램 이미지를 사용할 수 있습니다. 를 만들고 스프라이트 시트 시간이 매우 오래 걸릴 수 유지 관리 하는 이후 살펴보겠습니다 TexturePacker 프로그램이 CocosSharp 형식으로 파일을 내보낼 수 있습니다. TexturePacker 무료 및 "전문가" 버전을 제공 되며 Windows 및 Mac OS에 대해 사용할 수 있습니다. 무료 버전을 사용 하 여이 가이드의 나머지 부분을 따를 수 있습니다. 

TexturePacker 수 [TexturePacker 웹 사이트에서 다운로드 한](https://www.codeandweb.com/texturepacker)합니다. 열리면 TexturePacker 프로젝트를 로드 없습니다. 시작 화면을 사용 하면 스프라이트 추가 (다른 프로젝트 생성 된) 경우 최근에 사용한 프로젝트를 열고 스프라이트 시트에 사용할 형식을 선택 수 있습니다. CocosSharp는 Cocos2D 데이터 형식을 사용 합니다.

![](ccspritesheet-images/image7.png "CocosSharp는 Cocos2D 데이터 형식을 사용합니다")

이미지 파일 (같은 **.png**) 삭제 하 여 끌기-이러한 Windows 탐색기에서 Windows 또는 mac에서 Finder TexturePacker에 추가할 수 있습니다 파일이 추가 될 때마다 TexturePacker 스프라이트 시트 미리 보기를 자동으로 업데이트 합니다.

![](ccspritesheet-images/image8.png "파일이 추가 될 때마다 자동으로 TexturePacker 스프라이트 시트 미리 보기 업데이트")

스프라이트 시트를 내보내려면 클릭 합니다 **게시 스프라이트 시트** 단추 및 스프라이트 시트의 위치를 선택 합니다. TexturePacker는.plist 파일 및 이미지 파일에 저장 됩니다.

결과 파일을 사용 하려면.png 및.plist CocosSharp 프로젝트에 추가 합니다. CocosSharp 프로젝트에 파일 추가에 대 한 내용은 참조는 [BouncingGame 가이드](~/graphics-games/cocossharp/bouncing-game.md)합니다. 파일을 추가한 후에 로드 될 수는 `CCSpriteSheet` 위의 코드에서 이전에 나온 대로:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

### <a name="considerations-for-maintaining-a-texturepacker-sprite-sheet"></a>TexturePacker 스프라이트 시트를 유지 관리 하기 위한 고려 사항

게임 개발 되 아티스트 수 추가, 제거 또는 아트 수정 합니다. 변경한 업데이트 스프라이트 시트를 필요합니다. 다음 고려 사항을 스프라이트 시트 유지 관리를 줄일 수 있습니다.

 - 프로젝트의 폴더에 원래 파일 (sprite 시트를 만드는 데 파일)을 유지 하 고 버전 제어에 추가 되 고 있는지 확인 합니다. 이러한 파일은 변경 될 때마다 스프라이트 시트를 다시 만드는 데 필요 합니다.
 - Mac/Visual Studio 용 Visual Studio에 원래 파일을 추가 하지 않거나에 추가 되 면 설정 합니다 **빌드 작업** 하 **None**. 파일이 추가 되 고 플랫폼 특정 한 경우 **빌드 작업**, 결과 앱의 파일 크기가 늘어납니다 불필요 하 게 합니다.
 - 사용 하는 것이 좋습니다 *폴더를 스마트* TexturePacker에서. 스마트 폴더 스프라이트 시트에 포함 된 이미지를 자동으로 추가합니다. 이 기능은 많은 수의 이미지를 사용 하 여 많은 경우 게임 개발 시간을 저장할 수 있습니다. 

    ![](ccspritesheet-images/image9.png "이 기능은 많은 수의 이미지를 사용 하 여 많은 경우 게임 개발 시간을 저장할 수 있습니다.")
 - 스프라이트 시트 질감 크기 눈여겨봐야 합니다. 일부 이전 phone 하드웨어는 2048x2048 보다 큰 질감 크기를 지원 하지 않습니다. 또한 2048x2048의 32 비트 이미지를 거의 17 (mb) ram-많은 양의 메모리를 사용합니다.
 - TexturePacker 포함 되지 않습니다 폴더 스프라이트 이름에 기본적으로 이름 충돌이 있을 수 있으므로. 폴더를 포함 하려면 이름이 아니면 개발의 시작 부분에 없습니다. 결정 하는 것이 좋습니다. 더 큰 게임 충돌을 방지 하려면 폴더 이름을 사용 하 여 고려해 야 합니다. 폴더 경로 포함 하려면 클릭 **고급 표시** 에 **데이터** 섹션 및 확인 **앞에 추가 폴더 이름을**입니다. 

    ![](ccspritesheet-images/image10.png "폴더 경로 포함 하려면 클릭 고급 데이터 섹션에 표시 하 고 앞에 추가 폴더 이름 확인")

## <a name="summary"></a>요약

이 가이드에서는 만들고 사용 하 여 `CCSpriteSheet` 클래스입니다. 에 로드할 수 있는 파일을 생성 하는 방법에 대해서도 설명 `CCSpriteSheet` TexturePacker 프로그램을 사용 하 여 인스턴스.

## <a name="related-links"></a>관련 링크

- [CCSpriteSheet](https://developer.xamarin.com/api/type/CocosSharp.CCSpriteSheet/)
- [전체 데모 (샘플)](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)
