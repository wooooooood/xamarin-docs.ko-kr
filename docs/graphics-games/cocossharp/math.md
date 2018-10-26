---
title: CocosSharp 사용한 2D 수치 연산
description: 이 가이드에서는 게임 개발에 대 한 2D 수학을 다룹니다. CocosSharp를 사용 하 여 게임 개발의 일반적인 작업을 수행 하는 방법을 하 고 이러한 작업 뒤 수학에 설명 합니다.
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 60386b3629e8ed9d2fd1ff165cd2c04d9571b51a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106236"
---
# <a name="2d-math-with-cocossharp"></a>CocosSharp 사용한 2D 수치 연산

_이 가이드에서는 게임 개발에 대 한 2D 수학을 다룹니다. CocosSharp를 사용 하 여 게임 개발의 일반적인 작업을 수행 하는 방법을 하 고 이러한 작업 뒤 수학에 설명 합니다._

배치 하 고 코드를 사용 하 여 개체를 이동 하는 모든 규모의 개발 게임의 핵심적인 부분입니다. 위치 및 이동 게임 직선을 또는 회전 삼각 함수 사용을 따라 개체를 이동 해야 하는지 여부를 수학를 사용을 해야 합니다. 이 문서는 다음 항목을 설명 합니다.

 - 개발속도
 - 가속
 - CocosSharp 개체 회전
 - 회전을 사용 하 여 개발 속도 사용 하 여

개발자는 강력한 수학 배경이 없거나는 장기-잊어버린 학교에서 이러한 항목 걱정할 필요가 없습니다-이 문서는 개념 bite 큰 조각으로 나누고 실제 예제를 사용 하 여 이론적인 설명을 함께 표시 됩니다. 즉,이 문서는 전통적인 수학 학생 질문에 대답: "때 실제로 필요 합니까이 자료를 사용 하 시겠습니까?"


## <a name="requirements"></a>요구 사항

코드 샘플 폼을 상속 하는 개체를 사용 하 여 작업을 가정 하지만이 문서 CocosSharp 수학 측면에 중점을 `CCNode`입니다. 또한 이후 `CCNode` 값을 포함 하지 않는 코드 개발 속도 및 가속에 대 한 VelocityX, VelocityY, AccelerationX, 및 AccelerationY 같은 값을 제공 하는 엔터티를 사용 하 여 작업을 가정 합니다. 엔터티에 대 한 자세한 내용은 연습에서 참조 하세요 [CocosSharp의 엔터티](~/graphics-games/cocossharp/entities.md)합니다.


## <a name="velocity"></a>개발속도

이라는 용어를 사용 하는 게임 개발자 *속도* 개체를 – 이동 하는 방법을 설명 하기 위해 특별히 속도 무엇 인가 이동 방향을 한다는 이며 이동 합니다. 

개발 속도 두 가지 유형의 단위를 사용 하 여 정의 됩니다: 위치 단위와 시간 단위입니다. 예를 들어, 자동차의 속도 시간당 마일 또는 킬로미터 시간당으로 정의 됩니다. 게임 개발자를 자주 사용 하 여 픽셀 속도 개체를 정의 하는 초당 이동 합니다. 예를 들어, 글머리 기호 초당 300 픽셀의 속도로 이동할 수 있습니다. 즉, 글머리 기호에서 초당 300 픽셀 이동 인 경우 다음이 이동 하 게 됩니다 600 단위 900 단위 2 초에에서 3 초입니다. 보다 일반적으로 개발 속도 값 *추가* 개체의 위치 (앞으로 살펴보겠지만 아래).

속도 단위 속도를 설명 하기를 사용 하지만 용어 속도 용어 속도 속도 방향에 참조 하는 동안 값 방향에서 독립적으로 일반적으로 참조 합니다. 따라서 할당 (글머리 기호는 필요한 속성을 포함 하는 클래스를 가정)을 글머리 기호로 속도 다음과 같을 수 있습니다.


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


### <a name="implementing-velocity"></a>개발 속도 구현합니다.

CocosSharp는 이동이 필요로 하는 개체 자체 이동 논리를 구현 해야 하므로 속도 구현 하지 않습니다. 개발 속도 자주 구현 하는 새 게임 개발자는 실수를 해당 속도 만드는 프레임 속도에 따라 달라 집니다. 즉, 다음과 *구현이 잘못* 올바른 결과 제공 하도록 같지만 게임의 프레임 속도에 따라 달라 집니다.

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

게임 프레임 속도가 (예: 초당 30 프레임이 대신 초당 60 프레임)에서 실행 되 면 개체는 느린 프레임 속도로 실행 하는 경우 보다 더 빠르게 이동할 표시 됩니다. 마찬가지로, 게임 높음 (장치 리소스를 사용 하 여 백그라운드 프로세스에서 발생할 수 있습니다)는 프레임 속도 프레임에서 처리할 수 없는 경우 게임 속도를 늦 추는 표시 됩니다.

이 계정에 속도 자주 구현 됩니다 시간 값을 사용 합니다. 예를 들어 경우는 `seconds` 변수 나타내는 초 수 (또는 소수) 마지막 시간 속도 적용 하므로 프레임 속도 관계 없이 일관 된 이동 소유 하는 개체를 다음 코드로 인해:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

게임 프레임 속도가 시 실행 하는 작은 개체의 위치를 자주 업데이트 됩니다 하는 것이 좋습니다. 따라서 각 업데이트는 게임을 더 자주 업데이트 된 경우 보다 더 이동 개체에서 발생 합니다. `seconds` 마지막 업데이트 이후 경과 된 시간은 어떻게 보고 하 여이 위해 계정 값입니다.

추가 이동 시간을 기준으로 하는 방법의 예제를 참조 하세요 [시간을 다루는이 작성법은 기반 이동](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/game_development/time_based_movement)합니다.


### <a name="calculating-positions-using-velocity"></a>개발 속도 사용 하 여 위치 계산

개발 속도 어느 정도의 시간 경과 후 개체 될 위치에 대 한 예측 하거나 게임을 실행 하지 않고도 개체의 동작을 조정 하는 데 사용할 수 있습니다. 예를 들어, 개발자에 게 발생 한 글머리 기호의 움직임을 구현 하는 인스턴스화된 후 글머리 기호의 속도 설정 해야 합니다. 개발 속도 설정 하기 위한 기초를 제공 하는 화면 크기를 사용할 수 있습니다. 즉, 개발자는 한다는 사실을 알고 있으면 2 초에 글머리 화면의 높이 이동 해야 다음 속도 2로 나눈 결과 화면의 높이를 설정 해야 합니다. 화면 800 픽셀 높이 인 경우 글머리 기호의 속도 400 (즉, 800/2)으로 설정 됩니다.

마찬가지로, 게임 논리 개체의 속도 지정 된 대상에 연결할 소요 됩니다을 계산 해야 합니다. 이 모바일 개체의 속도에서 이동할 거리를 나누어 계산할 수 있습니다. 다음 코드는 기간을 표시 하는 레이블에 텍스트를 할당 하는 방법을 표시 하는 예를 들어, 한 원거리 해당 대상에 도달할 때까지:


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


## <a name="acceleration"></a>가속

*가속* 는 게임 개발의 일반적인 개념 및 개발 속도와 비슷한 점이 많이 공유 합니다. 가속 개체 속도 되는지 아니면 (값이 어떻게 개발 속도 변경 시간이 지남에 따라) 속도 저하를 수량화 합니다. 가속 *추가* 속도 위치로 추가 하는 것 처럼 속도에. 가속의 일반 응용 프로그램 중력, 자동차, 속도 및 해당 thrusters 발생 공간 우주선을 포함 합니다. 

개발 속도 마찬가지로 가속 위치와 시간 단위;에 정의 된 그러나 가속의 시간 단위 라고 하는 *제곱* 가속 수학적으로 정의 하는 방법을 반영 하는 단위입니다. 즉, 게임 가속은 종종 단위로 *초당 픽셀 제곱*합니다.

개체에 있는 경우 제곱 초당 10 개 단위 X 가속, 의미의 속도 10 초 마다 증가 합니다. 1 초는 것이 10 초, 후 두 단위에서 이동 후 낮아집니다에서 시작 하는 경우 등, 두 번째 당 20 명의 단위 시간 (초)입니다.

다음과 같이 할당할 수 있도록 X 및 Y 구성 요소를 필요로 하는 2 차원에서 가속:


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


### <a name="acceleration-vs-deceleration"></a>가속 및 감속

매일 음성의 가속 및 감속을 보다는 구별 경우도 있지만 차이가 기술 둘 사이입니다. 중력 가속은 강제 됩니다. 개체 위쪽으로 throw 되 면 다음 중력 느려집니다 것 (감속) 하지만 개체 climbing 중지 하 고 속도가 늦는 무게와 같은 방향으로 다음 무게는 속도 높이기 것 (가속화). 아래와 같이 가속이 적용 동일 이동 같은 방향 또는 반대 방향으로 적용 되는 여부를 합니다. 


### <a name="implementing-acceleration"></a>가속 구현

가속 개발 속도 유사 구현 하는 경우 – CocosSharp에 의해 자동으로 구현 되지 않았습니다 이며 가속 시간 기반 프레임 기반 가속) (달리 원하는 구현 합니다. 따라서 간단한 가속 (및 속도) 구현 처럼 보일 수 있습니다.

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

위의 코드는 무엇 이라고 하는 *선형 근사값* 가속 구현에 대 한 합니다. 효과적으로 상당히 밀접 수준의 정확도 사용 하 여 가속 구현 있지만 가속 완벽 하 게 정확한 모델을 아닙니다. 가속 구현 하는 방법에 대 한 개념을 설명 하는 데 위에 포함 됩니다.

다음 구현은 수학적으로 정확 하 게 응용 프로그램을 개발 속도 및 가속:


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

위의 코드를 가장 확실 한 차이점은는 `halfSecondsSquared` 변수와 위치로 가속을 적용 하려면 사용 합니다. 수학 이유로이 자습서의 범위를 벗어납니다. 하지만이 수학에 관심이 있는 개발자의 자세한 정보를 찾을 수 [가속을 통합 하는 방법을이 설명 합니다.](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

실질적인 영향 `halfSecondSquare` 가속 수학적으로 정확 하 게 하 고 예측 가능한 방식으로 프레임 속도 관계 없이 동작 하는 합니다. 가속 된 선형 근사값 될 프레임 속도 – 낮을수록 framerate를 삭제 된 근사값 덜 정확 하 게 됩니다. 사용 하 여 `halfSecondsSquared` 보장 코드 프레임 속도 관계 없이 동일 하 게 동작 합니다.


## <a name="angles-and-rotation"></a>각도 및 회전

시각적 개체와 같은 `CCSprite` 를 통해 회전을 지원 한 `Rotation` 변수입니다. 이 회전 각도 (도) 설정 하는 값을 할당할 수 있습니다. 다음 코드를 회전 하는 방법을 표시 하는 예를 들어, 한 `CCSprite` 인스턴스:


```csharp
CCSprite unrotatedSprite = new CCSprite("star.png");
unrotatedSprite.IsAntialiased = false;
unrotatedSprite.PositionX = 100;
unrotatedSprite.PositionY = 100;
this.AddChild (unrotatedSprite);

CCSprite rotatedSprite = new CCSprite("star.png");
rotatedSprite.IsAntialiased = false;
// This sprite is moved to the right so it doesn’t overlap the first
rotatedSprite.PositionX = 130;
rotatedSprite.PositionY = 100;
rotatedSprite.Rotation = 45;
this.AddChild (rotatedSprite); 
```

그 결과는 다음과 같습니다.

![](math-images/image1.png "이 인해이 스크린 샷")

(이 기록 이유로 회전 수학적으로 적용 되는 방식을의 반대) 시계 방향으로 45도 회전 인지 확인 합니다.

![](math-images/image2.png "회전은 시계 방향으로 45도 회전 수학적으로 적용 되는 방식을의 반대는 역사적인 이유는")

일반적 CocosSharp 및 일반 수학에 대 한 회전을 다음과 같이 시각화할 수 있습니다.

![](math-images/image3.png "CocosSharp 및 일반 수학에 대 한 회전 같이 시각화할 수 일반적")

![](math-images/image4.png "CocosSharp 및 일반 수학에 대 한 회전 같이 시각화할 수 일반적")

이러한 구분은 중요 하기 때문에 `System.Math` 클래스는이 클래스를 사용 하 여 친숙 한 개발자에서 작업할 때 각도 반전 필요가 시계 반대 방향으로 회전 사용 하 여 `CCNode` 인스턴스.

위의 다이어그램도; 회전으로 표시 되도록 드릴 그러나 일부 수학 함수의 (등의 함수는 `System.Math` 네임 스페이스) 예상 하 고 값을 반환 *라디안* 도 하는 대신 합니다. 이 가이드 뒷부분에서 두 명의 단위 형식 간에 변환 하는 방법을 살펴보겠습니다.


### <a name="rotating-to-face-a-direction"></a>방향 얼굴에 대 한 회전

위에 나와 있는 것 처럼 `CCSprite` 를 사용 하 여 회전할 수는 `Rotation` 속성입니다. 합니다 `Rotation` 속성에서 제공 됩니다 `CCNode` (에 대 한 기본 클래스 `CCSprite`), 즉, 회전에서 상속 하는 엔터티에 적용할 수 있습니다 `CCNode` 도 합니다. 

일부 게임 대상 과연 있도록 회전할 개체를 필요로 합니다. 예로 플레이어 대상 또는 사용자 화면에 접촉 되어 있는 지점 방향으로 움직이는 공간 우주선을 해결 하는 컴퓨터 제어 적이 있습니다. 그러나 회전 값을 먼저 계산 되어야 합니다 따라 회전 되 고 엔터티의 위치 및 대상의 얼굴의 위치에.

이 프로세스에는 많은 단계가 필요합니다.

 - 식별 된 *오프셋* 대상입니다. 오프셋 X 및 Y 회전 엔터티와 대상 엔터티 간의 거리를 가리킵니다.
 - 아크탄젠트 trigonometry 함수 (아래에서 자세히 설명)를 사용 하 여 오프셋에서 각도 계산 합니다.
 - 0도 회전 되지 않은 경우 회전 엔터티 가리키는 방향과 오른쪽을 향하는 사이의 차이 조정 합니다.
 - 수학 회전 (시계 반대 방향으로) 및 CocosSharp 회전 (방향) 간의 차이 조정합니다.

다음 함수 (엔터티 작성로 가정)을 대상 엔터티를 회전:


```csharp
// This function assumes that it is contained in a CCNode-inheriting object
public void FacePoint(float targetX, float targetY)
{
    // Calculate the offset - the target's position relative to "this"
    float xOffset = targetX - this.PositionX;
    float yOffset = targetY - this.PositionY;

    // Make sure the target isn't the same point as "this". If so,
    // then rotation cannot be calculated.
    if (targetX != this.PositionX || targetY != this.Position.Y)
    {

        // Call Atan2 to get the radians representing the angle from 
        // "this" to the target
        float radiansToTarget = (float)System.Math.Atan2 (yOffset, xOffset);

        // Since CCNode uses degrees for its rotation, we need to convert
        // from radians
        float degreesToTarget = CCMathHelper.ToDegrees (radiansToTarget);

        // The direction that the entity faces when unrotated. In this case
        // the entity is facing "up", which is 90 degrees 
        const float forwardAngle = 90;

        // Adjust the angle we want to rotate by subtracting the
        // forward angle.
        float adjustedForDirecitonFacing = degreesToTarget - forwardAngle;

        // Invert the angle since CocosSharp uses clockwise rotation
        float cocosSharpAngle = adjustedForDirecitonFacing * -1;

        // Finally assign the rotation
        this.Rotation = rotation = cocosSharpAngle;
    }
} 
```

위의 코드를 다음과 같이 화면에 접촉 되어 지점 향하도록 지정 하므로 엔터티를 회전 하려면 사용할 수 있습니다.


```csharp
private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    if(touches.Count > 0)
    {
        CCTouch firstTouch = touches[0];
        FacePoint (firstTouch.Location.X, firstTouch.Location.Y);
    }
} 
```

이 코드는 다음과 같은 동작이 발생합니다.

![](math-images/image5.gif "이 코드에서는이 동작이 발생")

#### <a name="using-atan2-to-convert-offsets-to-angles"></a>각도 오프셋과 Atan2를 사용 하 여 변환

`System.Math.Atan2` 각도 오프셋을 변환할 수 있습니다. 함수 이름을 `Atan2` 삼각 함수 아크탄젠트에서 제공 됩니다. "2" 접미사를 표준에서이 함수를 구분할 `Atan` 아크탄젠트의 수치 연산 동작을 엄격 하 게 일치 하는 함수입니다. 아크탄젠트는-90 사이의 값을 반환 하는 함수 및 + 90도 (또는 해당 라디안에서). 컴퓨터 게임을 비롯 한 대부분의 응용 프로그램은 종종 값의 전체를 360도 필요 하므로 `Math` 클래스에 포함 되어 `Atan2` 이 요구를 충족 하기 위해.

위의 코드는 Y 매개 변수를 전달 먼저 다음 X 매개 변수를 호출 하는 경우 확인 된 `Atan2` 메서드. 일반적인 X, Y 좌표 위치 순서에서 이전 버전과입니다. 자세한 내용은 [Atan2 문서를 참조 하세요](https://msdn.microsoft.com/library/system.math.atan2(v=vs.110).aspx)합니다.

또한 주목할 만한 가치가에서 반환 된 값을 `Atan2` 라디안에서 각도 측정 하는 데 사용 되는 다른 단위는 됩니다. 이 가이드 라디안의 세부 정보를 포함 하지 않습니다 하지만 유의 하는 모든 삼각 함수에는 `System.Math` 네임 스페이스 사용 라디안 값도 CocosSharp 개체에 사용 되기 전에 변환 해야 합니다. Radians 대 한 자세한 정보를 찾을 수 있습니다 [라디안 Wikipedia 페이지에서](http://en.wikipedia.org/wiki/Radian)합니다.

#### <a name="forward-angle"></a>정방향 각도

한 번 합니다 `FacePoint` 메서드는 각도를 라디안으로 변환, 정의 `forwardAngle` 값입니다. 이 값은 해당 회전 값이 0 인 경우 엔터티 연결 각도를 나타냅니다. 이 예제는 엔터티는 마주보는 상향식으로 (반대로 CocosSharp 회전) 수치 회전을 사용 하는 경우 90도 가정 합니다. 여기에서 사용 수치 회전 CocosSharp에 대 한 회전을 아직 반전 하지 않은 것 이므로 합니다.

다음으로 어떤 엔터티를 보여 줍니다.는 `forwardAngle` 90도 다음과 같을 수 있습니다.

![](math-images/image6.png "90도의 forwardAngle 사용 하 여 엔터티의 모양을 보여 줍니다.")


### <a name="angled-velocity"></a>각 진된 속도

지금까지 각도에 오프셋을 변환 하는 방법을 살펴보았습니다. 이 섹션에서는 다른 방법으로 이동 –는 각도 가져와 변환 하 X 및 Y 값입니다. 일반적인 예로 자동차를 향하고 방향 또는 우주선 연결 되는 방향으로 이동 하는 글머리 기호가 해결 공간 우주선을 이동 합니다. 

개념적으로 첫 번째 정의 되지 않은 회전 하는 경우 원하는 속도 다음 엔터티는 연결 되는 각도를 해당 속도 회전 하 여 개발 속도 계산할 수 있습니다. 을이 개념을 보여 주기 위해 개발 속도 (및 가속)로 시각화할 수는 2 차원 *벡터* (하는 일반적으로 그린 화살표). X 사용 하 여 개발 속도 값에 대 한 벡터 100 및 Y = = 0을 다음과 같이 시각화할 수 있습니다.

![](math-images/image7.png "X 사용 하 여 개발 속도 값에 대 한 벡터 100 및 Y = = 0을 다음과 같이 시각화할 수 있습니다.")

새 속도의 결과가이 벡터를 회전할 수 있습니다. 예를 들어, 다음 결과 벡터 (시계 반대 방향으로 회전 사용) 45도 각도로 회전 합니다.

![](math-images/image8.png "벡터를이 시계 반대 방향으로 회전 결과 사용 하 여 45도 회전")

다행 스럽게도 개발 속도 가속화 및 심지어 위치 모두 회전할 수 있습니다는 값이 적용 되는 방법에 관계 없이 동일한 코드를 사용 하 여. CocosSharp 회전 값으로 벡터를 회전 하려면 다음과 같은 일반적인 용도의 함수를 사용할 수 있습니다.


```csharp
// Rotates the argument vector by degrees specified by
// cocosSharpDegrees. In other words, the rotation
// value is expected to be clockwise.
// The vector parameter is modified, so it is both an in and out value
void RotateVector(ref CCVector2 vector, float cocosSharpDegrees)
{
    // Invert the rotation to get degrees as is normally
    // used in math (counterclockwise)
    float mathDegrees = -cocosSharpDegrees;

    // Convert the degrees to radians, as the System.Math
    // object expects arguments in radians
    float radians = CCMathHelper.ToRadians (mathDegrees);

    // Calculate the "up" and "right" vectors. This is essentially
    // a 2x2 matrix that we'll use to rotate the vector
    float xAxisXComponent = (float)System.Math.Cos (radians);
    float xAxisYComponent = (float)System.Math.Sin (radians);
    float yAxisXComponent = (float)System.Math.Cos (radians + CCMathHelper.Pi / 2.0f);
    float yAxisYComponent = (float)System.Math.Sin (radians + CCMathHelper.Pi / 2.0f);

    // Store the original vector values which will be used
    // below to perform the final operation of rotation.
    float originalX = vector.X;
    float originalY = vector.Y;

    // Use the axis values calculated above (the matrix values)
    // to rotate and assign the vector.
    vector.X = originalX * xAxisXComponent + originalY * yAxisXComponent;
    vector.Y = originalX * xAxisYComponent + originalY * yAxisYComponent;
} 
```

전체에 대 한 이해는 `RotateVector` 메서드는이 문서의 범위를 벗어납니다 일부 선형 대 수와 함께 사인 및 코사인 삼각 함수에 잘 모르는 필요 합니다. 그러나 한 번 구현한 후는 `RotateVector` 속도 벡터를 포함 하 여 모든 벡터를 회전 하려면 메서드를 사용할 수 있습니다. 예를 들어, 다음 코드를 발생 시킬 수 하 여 지정 된 방향에 있는 글머리는 `rotation` 값:


```csharp
// Create a Bullet instance
Bullet newBullet = new Bullet();

// Define the velocity of the bullet when 
// rotation is 0
CCVector2 velocity = new CCVector2 (0, 100);

// Modify the velocity according to rotation
RotateVector (ref velocity, rotation);

// Assign the newBullet's velocity using the
// rotated vector
newBullet.VelocityX = velocity.X;
newBullet.VelocityY = velocity.Y;

// Set the bullet's rotation so it faces
// the direction that it's flying
newBullet.Rotation = rotation; 
```

이 코드와 같이 생성할 수 있습니다.

![](math-images/image9.png "이 코드는이 스크린샷과 비슷하게 생성 될 수 있습니다.")


## <a name="summary"></a>요약

이 가이드에서는 2D 게임 개발의 일반적인 수학적 개념을 설명합니다. 할당 하 고 개발 속도 및 가속을 구현 하는 방법을 보여 줍니다 하 고 개체 및 모든 방향에서 이동 벡터를 회전 하는 방법에 설명 합니다.
