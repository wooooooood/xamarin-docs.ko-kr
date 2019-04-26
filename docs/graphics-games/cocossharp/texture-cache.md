---
title: 텍스처 캐싱 CCTextureCache를 사용 하 여
description: CocosSharp의 CCTextureCache 클래스에는 캐시를 구성 하 고 콘텐츠를 언로드할는 표준 방법을 제공 합니다. 큰 게임 RAM, 그룹화 및 질감 삭제 프로세스를 단순화에 전체가 들어 맞지 않을 수 있습니다에 대 한 특히 유용 합니다.
ms.prod: xamarin
ms.assetid: 1B5F3F85-9E68-42A7-B516-E90E54BA7102
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 232363d6ce1cb93499716b2c1247c48403cf6cea
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61302349"
---
# <a name="texture-caching-using-cctexturecache"></a>텍스처 캐싱 CCTextureCache를 사용 하 여

_CocosSharp의 CCTextureCache 클래스에는 캐시를 구성 하 고 콘텐츠를 언로드할는 표준 방법을 제공 합니다. RAM, 그룹화 및 질감 삭제 프로세스를 단순화에 전체가 들어 맞지 않을 수 있습니다는 큰 게임에 특히 유용 합니다._

`CCTextureCache` 클래스는 CocosSharp 게임 개발의 필수적인 부분입니다. 대부분의 CocosSharp 사용 하 여 게임을 `CCTextureCache` 만큼 CocosSharp 메서드 내부적으로 사용 하지를 명시적으로 하는 경우에 개체를 *공유* 질감 캐시 합니다.

이 가이드에서는 설명의 `CCTextureCache` 이유와 게임 개발을 위한 것입니다. 구체적으로 설명 합니다.

 - 캐싱 문제를 질감 이유
 - 질감 수명
 - SharedTextureCache를 사용 하 여
 - 지연 AddImage 사용 하 여 미리 로드 및 로드
 - 텍스처를 삭제합니다.


## <a name="why-texture-caching-matters"></a>캐싱 문제를 질감 이유

텍스처 캐싱은 게임 개발에서 중요 하 게 텍스처 로드 시간이 오래 걸리는 작업이 있으며 질감 런타임에 상당한 양의 RAM 요구 합니다.

모든 파일 작업의 경우와 마찬가지로 디스크에서 질감을 로드 하면 비용이 많이 드는 작업일 수 있습니다. 질감 로드 로드 되는 파일 (png 및 jpg 이미지의 경우)으로 압축 되 고 같은 처리에 필요한 경우 추가 시간이 걸릴 수 있습니다. 텍스처 캐싱 응용 프로그램 디스크에서 파일을 로드 해야 하는 횟수를 줄일 수 있습니다.

위에서 설명한 대로 질감은 또한 많은 양의 런타임 메모리를 차지 합니다. 예를 들어 iPhone 6 (1344 x 750)의 해상도로 크기의 배경 이미지는 PNG 파일 크기가 몇 킬로바이트만 경우에 ram – 4mb를 차지 합니다. 텍스처 캐싱 질감 참조 앱 내에서 공유 하는 방법 그리고 쉽게 다양 한 게임 상태 간에 전환 될 때 모든 콘텐츠를 언로드할 수를 제공 합니다.


## <a name="texture-lifespan"></a>질감 수명

CocosSharp 질감 앱의 실행의 전체 길이 대 한 메모리에 유지 될 수 있습니다 또는 단기적인 일 수 있습니다. 메모리를 최소화 하기 위해 사용량 앱은 더 이상 필요 없는 질감 삭제 해야 합니다. 물론, 즉, 질감 삭제 및 로드 시간을 늘리거나 로드 하는 동안 성능이 저하 될 수는 나중에 다시 로드 될 수 있습니다. 

메모리 사용량 및 로드 시간 간에 균형을 유지 해야 텍스처를 자주 로드 / 런타임 성능. 작은 크기의 질감 메모리를 사용 하는 게임, 필요에 따라 메모리에 모든 질감을 유지할 수 있지만 더 큰 게임 언로드 질감 공간을 확보 해야 할 수 있습니다.

다음 다이어그램은 필요에 따라 질감을 로드 하 고 실행의 전체 길이 대 한 메모리에 보관 하는 간단한 게임을 보여줍니다.

![](texture-cache-images/image1.png "이 다이어그램에서는 필요에 따라 질감을 로드 하 고 실행의 전체 길이 대 한 메모리에 보관 하는 간단한 게임을 보여 줍니다.")

처음 두 막대 게임의 실행 시 즉시 필요한 질감을 나타냅니다. 다음 세 개의 막대는 필요에 따라 로드 하는 각 수준에 대 한 질감을 나타냅니다.

게임 큰 경우 장치와 OS에서 제공 하는 모든 RAM에 맞게 충분 한 질감을 로드 결국 충분 한 것입니다. 이 해결 하기 위해 게임을 더 이상 필요 없는 경우 질감 데이터를 언로드할 수 없습니다. 예를 들어 다음 다이어그램은 더 이상 필요 없는 그런 다음 수준에 대 한 Level2Texture를 로드 하는 경우 Level1Texture 언로드하고 게임을 보여줍니다. 최종 결과 지정된 된 시간에 3 개의 텍스처 메모리에 유지 됩니다. 

![](texture-cache-images/image2.png "최종 결과 지정된 된 시간에 3 개의 텍스처 메모리에 유지 됩니다.")


위에 표시 된 다이어그램 나타내지만 언로드 여 텍스처 메모리 사용량을 줄일 수 있습니다 플레이어 수준 재생 하기로 한 경우 추가 로드 시간이 필요할 수 있습니다. UITexture 및 MainCharacter 질감을 로드 하 고 언로드되지는 주목할 만한 이기도 합니다. 이 항상 메모리에 유지 되므로 이러한 질감 모든 수준에 필요 함을 의미 합니다. 


## <a name="using-sharedtexturecache"></a>SharedTextureCache를 사용 하 여

통해 로드 될 때 자동으로 CocosSharp 질감 캐시를 `CCSprite` 생성자입니다. 예를 들어 다음 코드는 텍스처 인스턴스를 하나만 만듭니다.


```csharp
for (int i = 0; i < 100; i++)
{
    CCSprite starSprite = new CCSprite ("star.png");
    starSprite.PositionX = i * 32;
    this.AddChild (starSprite);
} 
```

CocosSharp 자동으로 캐시 합니다 `star.png` 질감 다양 한 만드는 비용이 많이 드는 대체를 방지 하려면 동일한 `CCTexture2D` 인스턴스. 이렇게 `AddImage` 호출할에서 공유 `CCTextureCache` 인스턴스, 특히 `CCTextureCache.SharedTextureCache.Shared`합니다. 알아야 하는 방법을 `SharedTextureCache` 는 호출 하는 기능적으로 동일 하는 다음 코드를 볼 수는 `CCSprite` 문자열 매개 변수를 사용 하 여 생성자:


```

CCSprite starSprite = new CCSprite ();
 starSprite.Texture = CCTextureCache.SharedTextureCache.AddImage ("star.png");
```

`AddImage` 확인 인수 파일 (이 경우 `star.png`) 이미 로드 되었습니다. 그렇다면 캐시 된 인스턴스가 반환 됩니다. 파일 시스템에서 로드 하지 질감에 대 한 참조에 대해 내부적으로 저장 되어 있으면 후속 `AddImage` 호출 합니다. 다른 말로 `star.png` 이미지는 한 번만 로드 하 고 후속 호출 추가 텍스처 메모리 없거나 추가 디스크 액세스가 필요 합니다.


## <a name="lazy-loading-vs-pre-loading-with-addimage"></a>지연 AddImage 사용 하 여 미리 로드 및 로드

`AddImage` 쓸 동일한 코드를 사용 하면 요청된 된 텍스처 이미 로드 여부 여부. 이 즉, 콘텐츠가; 필요할 때까지 로드 되지 않습니다. 그러나 런타임에 예기치 않은 콘텐츠 로드로 인해 성능 문제가 발생할 수 있습니다.

예를 들어 게임을 플레이어의 무기를 업그레이드할 수 있는 것이 좋습니다. 업그레이드 무기 및 무기를 시각적으로 변경 됩니다, 사용 중인 새 질감의 결과입니다. 콘텐츠를 지연 로드 하는 경우 다음 업그레이드 무기와 연결 된 질감 로드 되지 않습니다 처음에 플레이어 업그레이드를 획득 하는 경우 나중에 없으며 대신 합니다. 

이 게임 플레이 중간 로드는 게임을 발생할 수 있습니다 *pop*를 실행에서 하는 짧지만 눈에 띄는 고정 되 합니다. 이 방지 하려면 코드를 미리 로드 하 고 필요할 수 있는 질감 예측할 수 있습니다. 예를 들어, 다음 사용할 수 있습니다 질감을 미리 로드 하려면.


```csharp
void PreLoadImages()
{
    var cache = CCTextureCache.SharedTextureCache;

    cache.AddImage ("powerup1.png");
    cache.AddImage ("powerup2.png");
    cache.AddImage ("powerup3.png");

    cache.AddImage ("enemy1.png");
    cache.AddImage ("enemy2.png");
    cache.AddImage ("enemy3.png");

    // pre-load any additional content here to 
    // prevent pops at runtime
} 
```

이 사전 로드 메모리가 낭비 되 발생할 수 있습니다 하 고 시작 시간을 늘릴 수 있습니다. 예를 들어, 플레이어가 파워업 나타내는 얻을 실제로 수는 `powerup3.png` 질감을 불필요 하 게 로드 하도록 합니다. 물론이 있으므로 일반적으로 미리 로드 콘텐츠를 RAM에 맞도록 하는 경우 잠재적인 pop에서 게임 플레이 하지 않으려면 요금을 지불 하는 데 필요한 비용이 수 있습니다.


## <a name="disposing-textures"></a>텍스처를 삭제합니다.

게임 최소 사양 장치에서 사용 가능한 것 보다 더 많은 질감 메모리를 필요 하지 않은 경우 질감 삭제 필요 하지 않습니다. 반면에 더 큰 게임 새 콘텐츠에 대 한 공간을 만들기 위해 텍스처 메모리를 확보 해야 합니다. 예를 들어 게임에는 많은 양의 environment에 대 한 질감을 저장 하는 메모리를 사용할 수 있습니다. 특정 수준에서 환경만 사용 되는 경우 다음이 언로드하도록 수준을 종료 될 때입니다.


### <a name="disposing-a-single-texture"></a>단일 텍스처를 삭제합니다.

호출 해야 먼저 단일 텍스처를 제거 합니다 `Dispose` 메서드를 다음에서 수동 설치 제거를 `CCTextureCache`합니다.

다음은 해당 질감 함께 백그라운드 스프라이트를 완전히 제거 하는 방법입니다.


```csharp
void DisposeBackground()
{
    // Assuming this is called from a CCLayer:
    this.RemoveChild (backgroundSprite);

    CCTextureCache.SharedTextureCache.RemoveTexture (backgroundsprite.Texture);

    backgroundSprite.Texture.Dispose ();
} 
```

질감을 직접 삭제 소수의 질감 있지만이 사용 하 여 처리할 수 있게 되 면 오류가 큰 텍스처 집합을 처리 하는 경우에 효과적일 수 있습니다.

질감 (비공유) 사용자 지정 그룹화 할 수 있습니다 `CCTextureCache` 질감 정리를 단순화 하기 위해 인스턴스.

예를 들어 예제 콘텐츠가 수준 특정을 사용 하 여 미리 로드 되었다고 여기서 `CCTextureCache` 인스턴스. 합니다 `CCTextureCache` 수준을 정의 하는 클래스의 인스턴스를 정의할 수 있습니다 (발생할 수 있는 한 `CCLayer` 또는 `CCScene`):


```csharp
CCTextureCache levelTextures; 
```

`levelTextures` 인스턴스 수준 별 질감을 미리 로드 하려면 사용할 수 있습니다. 


```

void PreloadLevelTextures(CCApplication application)
{
    levelTextures = new CCTextureCache (application);

    levelTextures.AddImage ("Background.png");
    levelTextures.AddImage ("Foreground.png");
    levelTextures.AddImage ("Enemy1.png");
    levelTextures.AddImage ("Enemy2.png");
    levelTextures.AddImage ("Enemy3.png");

    levelTextures.AddImage ("Powerups.png");
    levelTextures.AddImage ("Particles.png");
} 
```

마지막 수준 끝나면 텍스처 수를 통해 한 번에 모든 삭제 된 `CCTextureCache`:


```csharp
void EndLevel()
{
    levelTextures.Dispose ();
    // Perform any other end-level cleanup
} 
```

Dispose 메서드가에서는 이러한 텍스처에서 사용 하는 메모리를 정리 하는 모든 내부 텍스처를 삭제 합니다. 결합 `CCTextureCache.Shared` 수준 또는 게임 모드-특정 `CCTextureCache` 전체 게임 및 수준 종료,이 가이드의 시작 부분에 다이어그램과 유사으로 언로드 중 일부를 통해 유지할 수 있도록 몇 가지 질감에서 결과 인스턴스입니다. 

![](texture-cache-images/image2.png "CCTextureCache.Shared 수준 또는 게임 모드별 CCTextureCache 인스턴스 결과 전체 게임 및 수준 종료,이 가이드의 시작 부분에 다이어그램과 유사으로 언로드 중 일부를 통해 유지할 수 있도록 몇 가지 질감의 결합")




## <a name="summary"></a>요약

이 가이드를 사용 하는 방법을 보여 줍니다는 `CCTextureCache` 분산 메모리 사용량 및 런타임 성능에는 클래스입니다. `CCTexturCache.SharedTextureCache` 명시적으로 또는 암시적으로 로드 하 고 응용 프로그램의 수명 동안 질감 캐시를 사용 하는 동안 수 `CCTextureCache` 인스턴스를 사용 하 여 메모리 사용량을 줄이기 위해 텍스처를 언로드할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [https://github.com/mono/CocosSharp](https://github.com/mono/CocosSharp)
- [/api/type/CocosSharp.CCTextureCache/](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
