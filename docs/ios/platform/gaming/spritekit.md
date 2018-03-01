---
title: SpriteKit
ms.topic: article
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 79f7fbf52b544416223d225457d3148d68bc29e2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="spritekit"></a>SpriteKit

Sprite 키트, Apple에서 2D 게임 프레임 워크는 iOS 8 및 OS X Yosemite의 몇 가지 흥미로운 새로운 기능에 있습니다. 여기에 장면 키트, 셰이더 지원, 조명, 그림자, 제약 조건, 기본 맵 생성 및 물리학 향상 된 기능 통합을 포함 합니다. 특히, 새로운 물리 기능 매우 쉽게 게임에 실제 효과를 추가 합니다.

## <a name="physics-bodies"></a>물리 본문

Sprite 키트 2D, 고정 된 관계로 본문 물리학 API 포함합니다. 모든 sprite 관련된 물리학 본문이 (`SKPhysicsBody`) 물리학 세계에서 본문의 기 하 도형을 뿐만 아니라 mass 마찰와 같은 물리 속성을 정의 하 합니다.

## <a name="creating-a-physics-body-from-a-texture"></a>질감에서 물리 본문 만들기
Sprite 키트는 이제 해당 질감에서 스프라이트 물리학 본문 파생을 지원 합니다. 이렇게 하면 더 자연 스러운 보이는 충돌 구현 하기가 쉽습니다.

다음 충돌의 각 이미지의 표면에서 거의 바나나 및 원숭이 충돌 하는 방법을 확인 예를 들어 합니다.
 
![](spritekit-images/image13.png "각 이미지의 표면에서 거의 바나나 및 원숭이 충돌")

Sprite 키트 가능 하 게 물리학 본문을 만드는 코드의 한 줄으로 합니다. 호출 하기만 하면 `SKPhysicsBody.Create` 질감 및 크기와: sprite 합니다. PhysicsBody = SKPhysicsBody.Create (sprite 합니다. 질감, sprite 합니다. 크기).

## <a name="alpha-threshold"></a>Alpha 임계값

간단히 설정 하는 `PhysicsBody` 질감에서 파생 된 기 하 도형에 직접 속성을 응용 프로그램 설정할 수 있습니다 및 기 하 도형을 파생 하는 방법을 제어 하려면 alpha 임계값입니다. 

Alpha 임계값 픽셀 결과 물리학 본문에 포함 하는 데 필요한 최소 알파 값을 정의 합니다. 예를 들어 다음 코드는 약간 다른 물리학 본문에서 발생합니다.

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

다음과 같이 alpha 임계값 조정의 효과 바나나와 충돌 하는 경우는 원숭이 대체 되도록 이전 충돌 fine-tunes:

![](spritekit-images/image14.png "바나나와 충돌 하는 경우는 원숭이 대체")
 
## <a name="physics-fields"></a>물리 필드

또 다른 훌륭한 추가 Sprite 키트에 새 물리 필드 지원입니다. 보 텍 필드 등을 추가할 수 있도록 이러한 방사형 중력 필드와 군에 스프링 필드입니다.

물리 필드를 다른와 마찬가지로 장면에 추가 된 SKFieldNode 클래스를 사용 하 여 만들어집니다 `SKNode`합니다. 에 다양 한 팩터리 메서드 `SKFieldNode` 다른 물리학 필드를 만들 수 있습니다. 호출 하 여 스프링 필드를 만들 수 있습니다 `SKFieldNode.CreateSpringField()`를 호출 하 여 방사형 중력 필드 `SKFieldNode.CreateRadialGravityField()`등입니다.

`SKFieldNode` 필드 특성 필드 강도, 필드 영역 필드 강제로 감쇠 등을 제어 하는 속성에 있습니다.

## <a name="spring-field"></a>스프링 필드

예를 들어 다음 코드 스프링 필드 만들고 장면에 추가 합니다.

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

다음 스프라이트 추가 하 고 설정할 수 있습니다 해당 `PhysicsBody` 속성을 사용자가 화면을 터치 하면 다음 코드 에서처럼 물리학 필드는 스프라이트에 영향을 줍니다.

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

스프링 필드 노드 주위 같은 진동 바나나 프로그램이 합니다.

![](spritekit-images/image15.png "바나나 진동 스프링 필드 노드 주위와 같은")
 
## <a name="radial-gravity-field"></a>방사형 중력 필드

다른 필드를 추가 하는 것은 유사 합니다. 예를 들어, 다음 코드는 방사형 중력 필드를 만듭니다.

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

여기서는 바나나 끌어 오지 않습니다 방사형 필드에 대 한 다른 force 필드의 결과이 됩니다.

![](spritekit-images/image16.png "바나나는 방사형 필드 주위 찾아볼 수")