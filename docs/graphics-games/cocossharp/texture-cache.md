---
title: 질감 CCTextureCache를 사용 하 여 캐싱
description: CocosSharp의 CCTextureCache 클래스에는 캐시를 구성 하 고 콘텐츠를 언로드할 표준 방법을 제공 합니다. RAM, 그룹화 및 질감의 삭제 과정을 단순화 하에 전체가 들어 맞지 않을 수 있는 큰 게임에 특히 유용 합니다.
ms.prod: xamarin
ms.assetid: 1B5F3F85-9E68-42A7-B516-E90E54BA7102
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: c217d8a935ae971aab472b05968c0251366362b2
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783690"
---
# <a name="texture-caching-using-cctexturecache"></a>질감 CCTextureCache를 사용 하 여 캐싱

_CocosSharp의 CCTextureCache 클래스에는 캐시를 구성 하 고 콘텐츠를 언로드할 표준 방법을 제공 합니다. RAM, 그룹화 및 질감의 삭제 과정을 단순화 하에 전체가 들어 맞지 않을 수 있는 큰 게임에 대 한 특히 유용 합니다._

`CCTextureCache` 클래스는 CocosSharp 게임 개발의 필수적인 부분입니다. 대부분 CocosSharp 사용 하 여 게임는 `CCTextureCache` 명시적으로, CocosSharp 메서드 만큼 내부적으로 사용 하는 경우에 개체는 *공유* 질감 캐시 합니다.

이 가이드에서 설명 된 `CCTextureCache` 및 게임 개발에 중요 한 이유입니다. 구체적으로 설명 합니다.

 - 질감의 캐싱 중요 한 이유
 - 질감 수명
 - SharedTextureCache를 사용 하 여
 - 지연 로드 AddImage와 사전 로드와 비교
 - 텍스처를 삭제합니다.


## <a name="why-texture-caching-matters"></a>질감의 캐싱 중요 한 이유

질감 캐싱은 질감 로드 작업은 시간이 많이 걸리는 작업 하며 질감 있어야 런타임 시 상당한 양의 RAM 게임 개발에는 중요 한 고려 사항입니다.

모든 파일 작업와 마찬가지로 텍스처 디스크에서 로드 하면 비용이 많이 드는 작업 수 있습니다. 질감 로드 로드 되는 파일 (의 경우 처럼 jpg 및 png 이미지)에 압축 되 고 같은 처리 해야 하는 경우 시간이 오래 걸릴 수 있습니다. 질감 캐싱 응용 프로그램에서 디스크 파일을 로드 해야 하는 횟수를 줄일 수 있습니다.

위에서 설명 했 듯이 텍스처는 또한 많은 양의 런타임 메모리를 차지 합니다. 경우에 PNG 파일의 크기가 몇 킬로바이트만 배경 이미지의 크기는 iPhone 6 (1344 x 750)의 해상도를 ram – 4mb이 차지 하는 예를 들어 합니다. 질감 캐싱 응용 프로그램 내에서 질감 참조를 공유 하는 방법을도 쉽게 여러 게임 상태 간의 전환 될 때 모든 콘텐츠를 언로드할 수 제공 합니다.


## <a name="texture-lifespan"></a>질감 수명

수명이 짧은 될 수 있습니다 또는 CocosSharp 질감 응용 프로그램 실행의 전체 길이 대 한 메모리에 유지할 수 있습니다. 메모리를 최소화 하기 위해 사용량 응용 프로그램는 더 이상 필요 없는 질감 삭제 해야 합니다. 물론, 즉, 질감 삭제 및 로드 시간이 길어질 수 있거나 로드 하는 동안 성능이 저하 될 수 있는 나중에 다시 로드 될 수 있습니다. 

메모리 사용 및 부하 시간 간의 균형을 조절 해야 텍스처를 자주 로드 / 런타임 성능을 합니다. 작은 양의 질감 메모리를 사용 하는 게임 필요에 따라 메모리에 모든 질감을 유지할 수 있습니다. 있지만 더 큰 게임 언로드 질감 공간을 확보 해야 할 수 있습니다.

다음 다이어그램에서는 실행의 전체 길이 대 한 메모리에 공간을 차지 하지 필요에 따라 텍스처를 로드 하는 간단한 게임을 보여 줍니다.

![](texture-cache-images/image1.png "이 다이어그램에서는 실행의 전체 길이 대 한 메모리에 공간을 차지 하지 필요에 따라 텍스처를 로드 하는 간단한 게임 보여 줍니다.")

처음 두 막대 게임의 실행 시 즉시 필요한 질감을 나타냅니다. 다음 세 가지 막대는 필요에 따라 로드 각 수준에 대 한 질감을 나타냅니다.

게임 큰지 하는 경우 장치 및 운영 체제에서 제공 하는 모든 RAM에 맞게 충분 한 질감 로드 결국 충분 한 것입니다. 이 해결 하기 위해 게임을 더 이상 필요 없는 경우 텍스처 데이터를 언로드할 수 없습니다. 예를 들어 다음 다이어그램에는 더 이상 필요 없는 다음 Level2Texture 다음 수준에 대 한 로드 하는 경우 Level1Texture 언로드하고 게임을 보여 줍니다. 최종 결과 지정된 된 시간에 세 개의 질감 메모리에 유지 됩니다. 

![](texture-cache-images/image2.png "최종 결과 지정된 된 시간에 세 개의 질감 메모리에 유지 됩니다.")


위에 표시 된 다이어그램을 언로드하여 질감 메모리 사용량을 줄일 수 있습니다 하지만 플레이어가 수준을 재생 하는 경우의 추가적인 로드 시간이 필요할 수 있습니다를 나타냅니다. UITexture 및 MainCharacter 질감 로드 되 고 언로드되지 않으므로 주목할 만한 이기도 합니다. 이 항상 메모리에 유지 되므로 이러한 질감 모든 수준에 필요한 것을 의미 합니다. 


## <a name="using-sharedtexturecache"></a>SharedTextureCache를 사용 하 여

통해 로드할 때 자동으로 CocosSharp 질감 캐시는 `CCSprite` 생성자입니다. 예를 들어 다음 코드는 텍스처 인스턴스를 하나만 만듭니다.


```csharp
for (int i = 0; i < 100; i++)
{
    CCSprite starSprite = new CCSprite ("star.png");
    starSprite.PositionX = i * 32;
    this.AddChild (starSprite);
} 
```

CocosSharp 자동으로 캐시는 `star.png` 텍스처는 비용이 많이 드는 방법도 여러 개 만들지를 방지 하기 위해 동일한 `CCTexture2D` 인스턴스. 이렇게 `AddImage` 호출 되 고 공유에서 `CCTextureCache` 인스턴스 특히 `CCTextureCache.SharedTextureCache.Shared`합니다. 이해 하는 방법을 `SharedTextureCache` 사용 되는 기능적으로 동일 호출 하는 다음 코드를 볼 수는 `CCSprite` 문자열 매개 변수로 생성자:


```

CCSprite starSprite = new CCSprite ();
 starSprite.Texture = CCTextureCache.SharedTextureCache.AddImage ("star.png");
```

`AddImage` 확인 인수 파일 (이 경우 `star.png`)가 이미 로드 되었습니다. 이 경우 캐시 된 인스턴스가 반환 됩니다. 파일 시스템에서 로드 되는 다음 하지 질감에 대 한 참조에 대해 내부적으로 저장 되어 있으면 후속 `AddImage` 호출 합니다. 즉는 `star.png` 이미지는 한 번만 로드 하 고 후속 호출 추가 질감 메모리 없음이나 추가 디스크 액세스가 필요 합니다.


## <a name="lazy-loading-vs-pre-loading-with-addimage"></a>지연 로드 AddImage와 사전 로드와 비교

`AddImage` 코드를 동일한 쓸 수 있도록 요청된 된 텍스처 이미 로드 여부 여부. 이 즉, 콘텐츠가; 필요할 때까지 로드 되지 않습니다. 그러나이이 예기치 않은 내용 로드로 인해 런타임 시 성능 문제가 발생할 수 있습니다.

예를 들어 고려 게임 플레이어의 무기 업그레이드할 수 있습니다. 업그레이드 무기 및 공격할 시각적으로 변경 됩니다, 새 텍스처 사용량으로 인해 발생 합니다. 콘텐츠를 지연 로드 하는 경우 다음 업그레이드 된 웨와 관련 된 질감 로드 되지 않습니다 처음에 플레이어는 업그레이드를 획득 하는 경우 나중에 그 보다 합니다. 

이 게임 플레이 중순 로드도 게임을 지정 하면 *pop*, 실행에 짧지만 띄는 고정은 합니다. 이 방지 하려면 코드를 미리 로드 하 고 필요할 수 있는 텍스처를 예측할 수 있습니다. 예를 들어 다음 데 사용할 수 있습니다 질감을 미리 로드 합니다.


```csharp
void PreLoadImages()
{
    var cache = CCTextureCache.SharedTextureCache;

    cache.AddImage ("powerup1.png");
    cache.AddImage ("powerup2.png");
    cache.AddImage ("powerup3.png");

    cache.AddImage ("enemy1.png");
    cache.AddImage ("enemy2.png");
    cache.AddImage ("enemy3.png");

    // pre-load any additional content here to 
    // prevent pops at runtime
} 
```

미리이 로드 하는이 불필요 한 메모리에 발생할 수 있으며 시작 시간이 증가할 수 있습니다. 예를 들어 플레이어는 전원을 나타내는 얻을 실제로 수 있습니다는 `powerup3.png` 질감 불필요 하 게 로드 하도록 합니다. 물론 되므로 미리 로드 콘텐츠를 일반적으로 최상의 경우 RAM에 들어가는 게임의 잠재적인 pop를 방지 하기 위해 비용을 지불 하는 데 필요한 비용이 드는 수 있습니다.


## <a name="disposing-textures"></a>텍스처를 삭제합니다.

게임 최소 사양 장치에서 사용할 수 있는 것 보다 많은 질감 메모리를 필요 하지 않은 경우 질감 삭제 필요 하지 않습니다. 반면에 더 큰 게임 새 내용에 대 한 공간을 만들기 위해 질감 메모리를 확보 해야 합니다. 예를 들어 게임 많은 양의 환경에 대 한 질감을 저장 하는 메모리를 사용할 수 있습니다. 특정 수준에 있는 환경만 사용 되는 경우 다음 그 언로드할지 수준을 종료 될 때입니다.


### <a name="disposing-a-single-texture"></a>단일 텍스처를 삭제합니다.

호출 해야 먼저 단일 질감 제거는 `Dispose` 메서드를 다음에서 수동 설치 제거는 `CCTextureCache`합니다.

다음은 해당 질감 함께 배경 스프라이트 완전히 제거 하는 방법.


```csharp
void DisposeBackground()
{
    // Assuming this is called from a CCLayer:
    this.RemoveChild (backgroundSprite);

    CCTextureCache.SharedTextureCache.RemoveTexture (backgroundsprite.Texture);

    backgroundSprite.Texture.Dispose ();
} 
```

적은 수의 질감 하지만이 처리 수 있게 되 면 오류가 발생 하기 쉬운 더 큰 질감 집합을 처리 하는 경우 질감을 직접 삭제 효과적일 수 있습니다.

질감을 사용자 지정 (비공유)으로 그룹화 할 수 `CCTextureCache` 질감 정리를 간소화 하기 위해 인스턴스.

예를 들어 예로 콘텐츠 수준 특정을 사용 하 여 미리 채워진 경우 `CCTextureCache` 인스턴스. `CCTextureCache` 수준을 정의 하는 클래스에서 인스턴스를 정의할 수 있습니다 (발생할 수 있는 한 `CCLayer` 또는 `CCScene`):


```csharp
CCTextureCache levelTextures; 
```

`levelTextures` 인스턴스 수준 관련 질감을 미리 로드를 사용할 수 있습니다. 


```

void PreloadLevelTextures(CCApplication application)
{
    levelTextures = new CCTextureCache (application);

    levelTextures.AddImage ("Background.png");
    levelTextures.AddImage ("Foreground.png");
    levelTextures.AddImage ("Enemy1.png");
    levelTextures.AddImage ("Enemy2.png");
    levelTextures.AddImage ("Enemy3.png");

    levelTextures.AddImage ("Powerups.png");
    levelTextures.AddImage ("Particles.png");
} 
```

마지막으로 수준을 종료 될 때 질감 수를 통해 한 번에 모든 삭제는 `CCTextureCache`:


```csharp
void EndLevel()
{
    levelTextures.Dispose ();
    // Perform any other end-level cleanup
} 
```

Dispose 메서드 이러한 질감 사용 되는 메모리를 정리 하는 모든 내부 텍스처 삭제 합니다. 결합 `CCTextureCache.Shared` 수준 또는 게임 모드-특정 `CCTextureCache` 전체 게임 및 수준 종료,이 가이드의 시작 부분에 다이어그램 비슷합니다으로 언로드되고 일부를 통해 유지할 수 있도록 일부 질감에서 결과 인스턴스입니다. 

![](texture-cache-images/image2.png "한 수준 또는 게임 모드 특정 CCTextureCache 인스턴스 결과 전체 게임 및 수준 종료,이 가이드의 시작 부분에 다이어그램 비슷합니다으로 언로드되고 일부를 통해 유지할 수 있도록 일부 질감에서 CCTextureCache.Shared 결합")




## <a name="summary"></a>요약

이 가이드를 사용 하는 방법을 보여 줍니다는 `CCTextureCache` 균형 메모리 사용량 및 런타임 성능에는 클래스입니다. `CCTexturCache.SharedTextureCache` 암시적으로 로드 하 고 응용 프로그램의 수명에 대 한 질감 캐시를 사용 하거나 명시적으로 수 `CCTextureCache` 인스턴스 언로드 질감 메모리 사용을 줄일 데 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [https://github.com/mono/CocosSharp](https://github.com/mono/CocosSharp)
- [/api/type/CocosSharp.CCTextureCache/](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
