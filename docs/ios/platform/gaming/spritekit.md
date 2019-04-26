---
title: Xamarin.iOS에서 SpriteKit
description: 이 문서는 물리 및 애니메이션을 통합, 조명 및 음영 등에 대 한 지원이 포함 되어 있습니다, SceneKit을 사용 하 여 통합 하는 Apple의 2D 그래픽 프레임 워크, SpriteKit을 설명 합니다. SpriteKit 2D 게임을 만드는 데 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 93971DAE-ED6B-48A8-8E61-15C0C79786BB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: ef1e9a98b76166f4ee5638d1ab9762896d1e3bc8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293901"
---
# <a name="spritekit-in-xamarinios"></a>Xamarin.iOS에서 SpriteKit

Apple에서 2D 그래픽 프레임 워크, SpriteKit iOS 8 및 OS X Yosemite의 몇 가지 흥미로운 새 기능에 있습니다. 여기에 SceneKit, 셰이더 지원, 조명, 그림자, 제약 조건, 법선 맵 생성 및 물리학 향상 된 기능을 사용 하 여 통합이 포함 됩니다. 특히 물리 기능과 매우 쉽게 게임에 실제 효과를 추가 합니다.

## <a name="physics-bodies"></a>물리학 본문

SpriteKit 2D, rigid 바디 물리학 API 포함합니다. 모든 스프라이트 연결된 물리학 본문이 (`SKPhysicsBody`) 물리학 전 세계에서 대용량 마찰을 제공할 뿐만 아니라 본문의 기 하 도형 같은 물리학 속성을 정의 하 합니다.

## <a name="creating-a-physics-body-from-a-texture"></a>질감에서 물리학 본문 만들기
SpriteKit는 이제 해당 질감에서 스프라이트 물리학 본문 파생을 지원 합니다. 이렇게 하면 더 자연 스러운 보이는 충돌 구현 하기가 쉽습니다.

예를 들어, 알 수 있습니다 다음 충돌의 각 이미지의 화면에 거의 monkey 고 banana 충돌 하는 방법.
 
![](spritekit-images/image13.png "각 이미지의 화면에 거의 monkey 고 banana 충돌")

SpriteKit 가능 하 게 이러한 물리학 본문을 만드는 단일 코드 줄을 사용 하 여 합니다. 호출 `SKPhysicsBody.Create` 질감 및 크기를 가진: 스프라이트 합니다. PhysicsBody SKPhysicsBody.Create (sprite =. 질감, 스프라이트 합니다. 크기).

## <a name="alpha-threshold"></a>알파 임계값

설정 하는 것 외에도 `PhysicsBody` 질감에서 파생 된 기 하 도형에 직접 속성을 응용 프로그램 설정 수 및 기 하 도형 파생 되는 방법을 제어 하려면 알파 임계값입니다. 

알파 임계값 픽셀 결과 물리학 본문에 포함 하는 데 필요한 최소 알파 값을 정의 합니다. 예를 들어, 다음 코드에서는 약간 다른 물리학 본문에서 발생 합니다.

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

같이 알파 임계값 트윅의 효과 미세 합니다 banana와 충돌 하는 경우는 monkey 대체 되도록 이전 충돌을 조정 하 여:

![](spritekit-images/image14.png "banana와 충돌 하는 경우는 monkey 대체")
 
## <a name="physics-fields"></a>물리학 필드

다른 유용한 추가할 SpriteKit 새 물리학 필드 지원입니다. 등 소용돌이 필드를 추가할 수 있도록 이러한 방사형 중력 필드 및 몇 가지 이름을 스프링 필드입니다.

물리학 필드를 다른 마찬가지로 장면에 추가 되는 SKFieldNode 클래스를 사용 하 여 만들어집니다 `SKNode`합니다. 에 다양 한 factory 메서드 `SKFieldNode` 물리학에 다른 필드를 만들 수 있습니다. 호출 하 여 spring 필드를 만들 수 있습니다 `SKFieldNode.CreateSpringField()`를 호출 하 여 방사형 무게 필드 `SKFieldNode.CreateRadialGravityField()`등입니다.

`SKFieldNode` 필드 특성 필드 강제로의 감쇠, 필드 수준 및 필드 영역 등을 제어 하는 속성이 있습니다.

## <a name="spring-field"></a>스프링 필드

예를 들어, 다음 코드 스프링 필드 만들고 장면에 추가 합니다.

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

그런 다음 스프라이트를 추가 하 고 설정할 수는 `PhysicsBody` 속성 물리학 필드는 사용자가 화면을 터치 하는 경우 다음 코드 처럼 스프라이트, 영향이 있도록:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    var touch = touches.AnyObject as UITouch;
    var pt = touch.LocationInNode (this);
    var node = SKSpriteNode.FromImageNamed ("TinyBanana");
    node.PhysicsBody = SKPhysicsBody.Create (node.Texture, node.Size);
    node.PhysicsBody.AffectedByGravity = false;
    node.PhysicsBody.AllowsRotation = true;
    node.PhysicsBody.Mass = 0.03f;
    node.Position = pt;
    AddChild (node);
}
```

이렇게 하면 필드 노드 주위 spring 같은 진동 bananas:

![](spritekit-images/image15.png "스프링 필드 노드 주위 같은 bananas 진동")
 
## <a name="radial-gravity-field"></a>방사형 무게 필드

다른 필드를 추가 하는 것은 유사 합니다. 예를 들어 다음 코드는 방사형 무게 필드를 만듭니다.

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

따라서 여기서는 bananas 가져온 방사형 필드에 대 한 다른 force 필드:

![](spritekit-images/image16.png "필드 주위를 bananas 방사형 가져온")
