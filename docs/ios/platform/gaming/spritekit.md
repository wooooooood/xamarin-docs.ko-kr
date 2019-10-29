---
title: Xamarin.ios의 SpriteKit
description: 이 문서에서는 SceneKit와 통합 되 고, 물리학 및 애니메이션을 포함 하 고, 조명 및 음영에 대 한 지원을 포함 하는 SpriteKit, Apple의 2D 그래픽 프레임 워크에 대해 설명 합니다. SpriteKit는 2D 게임을 만드는 데 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 93971DAE-ED6B-48A8-8E61-15C0C79786BB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: e8829211ebf06eea224eade3f1b9d836207cdd64
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032466"
---
# <a name="spritekit-in-xamarinios"></a>Xamarin.ios의 SpriteKit

Apple의 2D 그래픽 프레임 워크인 SpriteKit에는 iOS 8 및 OS X Yosemite에 몇 가지 흥미로운 새로운 기능이 있습니다. 여기에는 SceneKit, 셰이더 지원, 조명, 그림자, 제약 조건, 일반적인 맵 생성 및 물리학 향상과의 통합이 포함 됩니다. 특히 새로운 물리학 기능을 통해 게임에 현실적인 효과를 매우 쉽게 추가할 수 있습니다.

## <a name="physics-bodies"></a>물리학 본문

SpriteKit에는 2 개의 엄격한 본문 물리학 API가 포함 되어 있습니다. 모든 스프라이트에는 질량, 마찰 등의 물리학 속성과 물리학 세계의 몸체 기 하 도형에 대 한 정의를 정의 하는 연결 된 물리학 본문 (`SKPhysicsBody`)이 있습니다.

## <a name="creating-a-physics-body-from-a-texture"></a>질감에서 물리학 본문 만들기
이제 SpriteKit는 질감에서 스프라이트의 물리학 본문을 파생 시키는 것을 지원 합니다. 이렇게 하면 보다 자연스럽 게 보이는 충돌을 쉽게 구현할 수 있습니다.

예를 들어 다음 충돌은 바나나 및 원숭이가 각 이미지의 표면에서 거의 충돌 하는 방식을 확인 합니다.

![](spritekit-images/image13.png "The banana and monkey collide nearly at the surface of each image")

SpriteKit를 사용 하면 코드 한 줄에서 이러한 물리학 본문을 만들 수 있습니다. 텍스처 및 크기: 스프라이트를 사용 하 여 `SKPhysicsBody.Create`를 호출 하기만 하면 됩니다. PhysicsBody = SKPhysicsBody (스프라이트. 텍스처, 스프라이트. 크기);

## <a name="alpha-threshold"></a>알파 임계값

응용 프로그램은 질감에서 파생 된 기 하 도형으로 직접 `PhysicsBody` 속성을 설정 하는 것 외에도, 응용 프로그램을 설정 하 고 알파 임계값을 설정 하 여 기 하 도형의 파생 방식을 제어할 수 있습니다 

알파 임계값은 픽셀을 결과의 물리학 본문에 포함 해야 하는 최소 알파 값을 정의 합니다. 예를 들어 다음 코드는 약간 다른 물리 본문을 생성 합니다.

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

이와 같이 알파 임계값을 조정 하는 것은 바나나와 충돌 하는 경우 원숭이가 넘는 이전 충돌을 세밀 하 게 조정 하는 효과입니다.

![](spritekit-images/image14.png "The monkey falls over when colliding with the banana")

## <a name="physics-fields"></a>물리 필드

SpriteKit 추가 하는 또 다른 좋은 방법은 새로운 물리학 필드 지원입니다. 이를 통해 소용돌이 필드, 방사형 무게 필드 및 스프링 필드와 같은 항목을 추가 하 여 이름을 지정할 수 있습니다.

물리 필드는 다른 `SKNode`와 마찬가지로 장면에 추가 되는 지 수 필드 노드 클래스를 사용 하 여 생성 됩니다. `SKFieldNode`에는 다양 한 팩터리 필드를 만드는 다양 한 팩터리 메서드가 있습니다. `SKFieldNode.CreateRadialGravityField()`를 호출 하 여 `SKFieldNode.CreateSpringField()`, 방사형 중력 필드를 호출 하 여 스프링 필드를 만들 수 있습니다.

필드 수준, 필드 영역 및 필드의 감쇠와 같은 필드 특성을 제어 하는 속성도 `SKFieldNode` 있습니다.

## <a name="spring-field"></a>스프링 필드

예를 들어 다음 코드는 스프링 필드를 만들어 장면에 추가 합니다.

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

그런 다음 사용자가 화면에 닿을 때 다음 코드와 같이 물리학 필드가 스프라이트에 영향을 주기 위해 스프라이트를 추가 하 고 해당 `PhysicsBody` 속성을 설정할 수 있습니다.

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

이로 인해 bananas는 field 노드 주위의 스프링 처럼 oscillate 됩니다.

![](spritekit-images/image15.png "The bananas oscillate like a spring around the field node")

## <a name="radial-gravity-field"></a>방사형 중력 필드

다른 필드를 추가 하는 것도 유사 합니다. 예를 들어 다음 코드는 방사형 무게 필드를 만듭니다.

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

이로 인해 bananas는 필드에 대해 radially를 가져오는 다른 force 필드가 됩니다.

![](spritekit-images/image16.png "The bananas are pulled radially around the field")
