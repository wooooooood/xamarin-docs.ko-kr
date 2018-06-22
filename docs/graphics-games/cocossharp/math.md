---
title: 2D 수학 CocosSharp와
description: 이 가이드에서는 게임 개발에 대 한 2D 수학을 다룹니다. CocosSharp를 사용 하 여 일반적인 게임 개발 작업을 수행 하는 방법을 보여 주는 하 고 수학 뒤에 이러한 작업에 설명 합니다.
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 13279df69b7a22117c10d74f1a15c082aff13264
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921585"
---
# <a name="2d-math-with-cocossharp"></a>2D 수학 CocosSharp와

_이 가이드에서는 게임 개발에 대 한 2D 수학을 다룹니다. CocosSharp를 사용 하 여 일반적인 게임 개발 작업을 수행 하는 방법을 보여 주는 하 고 수학 뒤에 이러한 작업에 설명 합니다._

배치 하 고 코드를 사용 하 여 개체를 이동 하려면 모든 규모의 개발 게임의 핵심 부분입니다. 위치를 지정 하 고 이동 게임 직선으로, 또는 회전에 대 한 삼각 함수 사용에 따라 개체를 이동 해야 하는지 여부를 수학, 사용을 해야 합니다. 이 문서에서는 다음 항목을 설명 합니다.

 - 개발속도
 - 가속
 - CocosSharp 개체 회전
 - 회전을 사용 하 여 개발 속도와

개발자는 장기-잊은 학교에서 이러한 항목 또는 강력한 수학 배경 갖고 있지 않지만 걱정할 필요가 없습니다 우수한 성능을이 문서 물 크기의 조각으로 개념을 구분 합니다 이론적 설명 실제 예제를 함께 사용 해야 합니다. 즉,이 문서에서 오래 된 수학 학생 질문의 대답 됩니다: "때 I 실제로 사용 해야 합니다 이러한?"


## <a name="requirements"></a>요구 사항

코드 샘플 개체 폼 상속 하는 작업을 가정 CocosSharp의 수학 측에 주로이 문서에서는, `CCNode`합니다. 또한 이후 `CCNode` 값을 포함 하지 않는 개발 속도 가속에 대 한이 코드에서는 엔터티 VelocityX, VelocityY, AccelerationX, 및 AccelerationY 등의 값을 제공 하는 작업에 있습니다. 엔터티에 대 한 자세한 내용은 연습에서 참조 [CocosSharp의 엔터티](~/graphics-games/cocossharp/entities.md)합니다.


## <a name="velocity"></a>개발속도

이라는 용어를 사용 하는 게임 개발자 *속도* 에 개체를 이동 – 하는 방법을 설명 합니다. 특히 얼마나 빨리 점이 이동 방향을 한다는 이동 합니다. 

개발 속도 두 가지 유형의 단위를 사용 하 여 정의 된: 위치 단위와 시간 단위입니다. 예를 들어, 자동차의 속도가 시간당 마일 또는 킬로미터 시간당으로 정의 됩니다. 게임 개발자 얼마나 빨리 개체를 정의 하는 초 당 자주 사용 하 여 픽셀 이동 합니다. 예를 들어 글머리 초당 300 픽셀의 속도로 이동할 수 있습니다. 즉, 글머리 초당 300 픽셀에서 이동 하는, 다음 것은 이동 되어 600 단위 2 초 및 900 단위에 등에 3 초입니다. 보다 일반적으로, 속도 값 *추가* 개체의 위치 (하겠지만 아래).

속도의 단위를 설명 하기 위해 속도 사용 하지만 용어 속도 용어 속도 속도 방향을 둘 다를 참조 하는 동안 방향, 무관 하는 값을 일반적으로 참조 합니다. 따라서 글머리의 속도 (글머리 기호는 필요한 속성을 포함 하는 클래스를 가정)의 할당은 다음과 같을 수 있습니다.


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


### <a name="implementing-velocity"></a>개발 속도 구현합니다.

CocosSharp 때문 움직임을 필요로 하는 개체를 직접 이동 논리를 구현 해야 합니다 속도 구현 하지 않습니다. 개발 속도 종종 구현 하는 새 게임 개발자는 실수의 개발 속도 만드는 프레임 속도에 따라 달라 합니다. 즉, 다음 *잘못 구현* 올바른 결과 제공 하도록 같지만 게임의 프레임 속도에 따라 달라 집니다.

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

게임 여 더 빠른 프레임 속도 (예: 초당 30 개 프레임 대신 초당 60 프레임)에서 실행 되 면 개체가 프레임 속도가 느린 속도로 실행 하는 경우 보다 빠르게 이동 하려면 표시 됩니다. 마찬가지로, 게임 (있음 장치의 리소스를 사용 하 여 백그라운드 프로세스에 의해 발생할 수 있습니다) 프레임 속도의 높음에서 프레임을 처리할 수 없는 경우 게임 속도가 표시 됩니다.

이를 고려해, 속도 주로 구현 되는 시간 값을 사용 하 여 합니다. 예를 들어 경우는 `seconds` 변수를 나타냅니다. 초 수 (또는 분수) 마지막 시간 속도 적용 된 후 다음 코드 프레임 속도 상관 없이 일관 된 이동을 소유 하는 개체를 초래 합니다.

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

더 낮은 프레임 속도로 실행 되는 게임을 작은 개체의 위치를 자주 업데이트 됩니다 하는 것이 좋습니다. 따라서 각 업데이트는 게임 더 자주 업데이트 하는 경우 보다 더 이상 이동 하는 개체에 발생 합니다. `seconds` 마지막 업데이트 이후 지난 시간을 보고 하 여이 대 한 계정 값입니다.

시간 기반 이동을 추가 하는 방법의 예제를 보려면 [시간을 다루는이 조리법 기반 이동을](https://developer.xamarin.com/recipes/cross-platform/game_development/time_based_movement/)합니다.


### <a name="calculating-positions-using-velocity"></a>개발 속도 사용 하 여 위치를 계산 합니다.

개발 속도 어느 정도의 시간이 지남에 후 개체 될 위치에 대 한 예측을 수행 하거나 게임을 실행 하지 않고도 개체의 동작을 조정 하는 데 사용할 수 있습니다. 예를 들어 개발자 발생할된 글머리의 움직임을 구현 하는 인스턴스화된 후 글머리 기호의 개발 속도 설정 해야 합니다. 화면 크기 개발 속도 설정 하기 위한 기초를 제공 데 사용할 수 있습니다. 즉, 개발자는 알고 있는 경우 글머리 기호 2 초에 화면의 높이 이동 해야 다음 속도 2로 나눈 결과 화면의 높이로 설정 해야 합니다. 화면이 800 픽셀 높이 경우 글머리 기호의 속도 400 (즉, 800/2)으로 설정 됩니다.

마찬가지로, 게임 논리 개체의 속도 지정 된 대상에 도달 하 소요 될 계산 해야 합니다. 있던 개체의 속도 의해 여행 거리를 분할 하 여이 계산할 수 있습니다. 예를 들어 다음 코드를 보여 줍니다 기간 표시 하는 레이블에 텍스트를 할당 하는 방법을 원거리에 도달할 때까지:


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


## <a name="acceleration"></a>가속

*가속* 는 게임 개발의 일반적인 개념 및 개발 속도와 많은 공통점이 공유 합니다. 가속은 개체가 속도 또는 (속도 값에 따른 변경 내역 시간)의 속도가 느려지지 있는지 수량화 합니다. 가속 *추가* 속도 위치를 추가 하는 것 처럼 개발 속도입니다. 가속 일반 응용 프로그램 등이 무게, 속도 향상, 자동차의 thrusters 발생 하는 공간 선적 있습니다. 

개발 속도 유사 가속 위치와 시간 단위;에 정의 된 그러나 가속의 시간 단위 라고는 *제곱* 가속 수학적으로 정의 반영 하는 단위입니다. 즉, 게임 가속은 종종 단위로 *초당 픽셀 제곱*합니다.

개체가 제곱 초당 10 단위는 X 가속을 사용 하면 의미의 속도 1 초 마다 10 씩 증가 합니다. 1 초는 것이 초, 후 두 당 10 단위에 이동 후 정지에서 시작 하는 경우 등, 두 번째 당 20 단위 초여야 합니다.

가속 2 차원에서 이므로 다음과 같이 할당할 수 있습니다는 X 및 Y 구성 요소를 필요 합니다.


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


### <a name="acceleration-vs-deceleration"></a>가속 감속 비교

가속 및 감속 매일 음성의 경우에 따라 구분 되는지, 이지만 두 기술 차이가 없습니다. 중력 가속 결과적 force입니다. 개체 위쪽으로 throw 되 면 다음 중력 느려집니다 것 (감속) 하지만 개체 오르기 중지 하 고와 동일한 방향으로 중력에 참가 하는 다음 무게는 그 시간을 단축 (가속화). 아래와 같이 가속이 적용이 같습니다 이동의 같은 방향 또는 반대 방향으로 적용 되는 여부. 


### <a name="implementing-acceleration"></a>가속 구현

가속 개발 속도 유사 구현할 때 – CocosSharp에서 자동으로 구현 되지 않았습니다 이며 가속 시간 기반 프레임 기반 가속) (대비 원하는 구현 합니다. 따라서 개발 속도) (함께 간단한 가속 구현 같을 수 있습니다.

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

위의 코드는 무엇 이라고는 *선형 근사값* 가속 구현에 대 한 합니다. 효과적으로 가속와는 상당히 밀접 수준의 정확도 구현 하지만 가속 완벽 하 게 정확한 모델 않습니다. 위의 개념 가속 구현 방법을 보여 주기 위해 포함 됩니다.

다음 구현은 가속 및 개발 속도의 수학적으로 정확 하 게 적용 것입니다.


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

위의 코드를 가장 확실 한 차이점은는 `halfSecondsSquared` 변수와 가속의 위치를 적용 하려면 사용 합니다. 수학 이유가이 자습서의 범위를 벗어납니다 수학이 뒤에 관심 있는 개발자는 자세한 정보를 찾을 수 있지만 [가속을 통합 하는 방법에 대 한이 설명입니다.](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

영향 `halfSecondSquare` 가속 수학적으로 정확 하 게 하 고 예상 프레임 속도 상관 없이 동작 하는 합니다. 가속 된 선형 근사값 프레임 속도-는 낮을수록 framerate 삭제는 근사값 덜 정확 하 게 됩니다. 사용 하 여 `halfSecondsSquared` 보장 코드 프레임 속도 관계 없이 동일 하 게 동작 합니다.


## <a name="angles-and-rotation"></a>각도 및 회전

시각적 개체와 같은 `CCSprite` 통해 회전을 지원 한 `Rotation` 변수입니다. 이 각도로 회전을 설정 하는 값을 할당할 수 있습니다. 예를 들어, 다음 코드를 보여 줍니다 회전 하는 방법을 `CCSprite` 인스턴스:


```csharp
CCSprite unrotatedSprite = new CCSprite("star.png");
unrotatedSprite.IsAntialiased = false;
unrotatedSprite.PositionX = 100;
unrotatedSprite.PositionY = 100;
this.AddChild (unrotatedSprite);

CCSprite rotatedSprite = new CCSprite("star.png");
rotatedSprite.IsAntialiased = false;
// This sprite is moved to the right so it doesn’t overlap the first
rotatedSprite.PositionX = 130;
rotatedSprite.PositionY = 100;
rotatedSprite.Rotation = 45;
this.AddChild (rotatedSprite); 
```

그 결과는 다음과 같습니다.

![](math-images/image1.png "이 인해이 스크린 샷")

(이 지금 회전 수학적으로 적용의 반대) 시계 방향으로 45도 회전 인지 확인 합니다.

![](math-images/image2.png "회전 각도 시계 방향으로 45도 지금 회전 수학적으로 적용의 반대 되")

일반적 CocosSharp 및 일반 수학 회전을 다음과 같이 시각화할 수 있습니다.

![](math-images/image3.png "다음과 같이 CocosSharp 및 일반 수학 회전 일반적 시각화할 수")

![](math-images/image4.png "다음과 같이 CocosSharp 및 일반 수학 회전 일반적 시각화할 수")

이러한 구분은 중요 하기 때문에 `System.Math` 클래스 되므로이 클래스에 익숙한 개발자 작업할 때 각도 반전 필요가 시계 반대 방향으로 회전 사용 하 여 `CCNode` 인스턴스.

위의 다이어그램에 회전 각도; 표시 드릴 그러나 일부 수치 연산 함수 (등의 함수는 `System.Math` 네임 스페이스) 및 반환 값에 *라디안* 도 대신 합니다. 이 가이드 뒷부분에서 두 명의 단위 형식 간에 변환 하는 방법을 살펴보겠습니다.


### <a name="rotating-to-face-a-direction"></a>방향이를 회전

위에 나와 있는 것 처럼 `CCSprite` 를 사용 하 여 회전할 수는 `Rotation` 속성입니다. `Rotation` 에서 제공 속성 `CCNode` (기본 클래스에 대 한 `CCSprite`), 즉, 회전에서 상속 하는 엔터티만에 적용할 수 있습니다 `CCNode` 도 합니다. 

일부 게임 대상을 발생할 수 있 되므로 회전 해야 개체를 필요로 합니다. 예로 플레이어 대상 또는 사용자의 화면을 터치가 있는 지점으로 비행 공간 배를 촬영 하는 컴퓨터 제어 적이 있습니다. 그러나 회전 값 해야 먼저 수 기준으로 계산 회전 되 고 엔터티의 위치 및 연결 대상의 위치.

이 프로세스는 몇을 가지 단계가 필요합니다.

 - 식별 된 *오프셋* 의 대상입니다. 오프셋 회전 엔터티와 대상 엔터티 간의 X 및 Y 거리를 나타냅니다.
 - 아크탄젠트 trigonometry 함수 (아래에서 자세히 설명)를 사용 하 여 오프셋에서 각도 계산 합니다.
 - 0도 회전 되지 않은 경우 회전 엔터티 가리키는 방향과 오른쪽을 향하는 간의 차이 대 한 조정 합니다.
 - 수학 회전 (시계 반대 방향으로) 및 CocosSharp 회전 (방향) 간의 차이 대 한 조정 합니다.

엔터티를 대상에 맞서게 회전 하는 다음 함수 (엔터티 작성으로 가정):


```csharp
// This function assumes that it is contained in a CCNode-inheriting object
public void FacePoint(float targetX, float targetY)
{
    // Calculate the offset - the target's position relative to "this"
    float xOffset = targetX - this.PositionX;
    float yOffset = targetY - this.PositionY;

    // Make sure the target isn't the same point as "this". If so,
    // then rotation cannot be calculated.
    if (targetX != this.PositionX || targetY != this.Position.Y)
    {

        // Call Atan2 to get the radians representing the angle from 
        // "this" to the target
        float radiansToTarget = (float)System.Math.Atan2 (yOffset, xOffset);

        // Since CCNode uses degrees for its rotation, we need to convert
        // from radians
        float degreesToTarget = CCMathHelper.ToDegrees (radiansToTarget);

        // The direction that the entity faces when unrotated. In this case
        // the entity is facing "up", which is 90 degrees 
        const float forwardAngle = 90;

        // Adjust the angle we want to rotate by subtracting the
        // forward angle.
        float adjustedForDirecitonFacing = degreesToTarget - forwardAngle;

        // Invert the angle since CocosSharp uses clockwise rotation
        float cocosSharpAngle = adjustedForDirecitonFacing * -1;

        // Finally assign the rotation
        this.Rotation = rotation = cocosSharpAngle;
    }
} 
```

위의 코드를 사용 하 여 엔터티를 회전 하는 지점 사용자 다음과 같은 화면을 터치가 직면 수 있습니다.


```csharp
private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    if(touches.Count > 0)
    {
        CCTouch firstTouch = touches[0];
        FacePoint (firstTouch.Location.X, firstTouch.Location.Y);
    }
} 
```

이 코드는 다음과 같은 동작이 발생합니다.

![](math-images/image5.gif "이 코드에서는이 동작에서 발생")

#### <a name="using-atan2-to-convert-offsets-to-angles"></a>각도 오프셋과 Atan2를 사용 하 여 변환 하려면

`System.Math.Atan2` 사용 하 오프셋 각도를 변환할 수 있습니다. 함수 이름 `Atan2` 삼각 함수 아크탄젠트에서 가져옵니다. "2" 접미사는 표준에서이 함수를 구분할 `Atan` 아크탄젠트의 수학 동작에 엄격 하 게 일치 하는 함수입니다. 아크탄젠트 값은-90 사이의 값을 반환 하는 함수 및 + 90도 (또는 해당 라디안에서)입니다. 일반적으로 컴퓨터 게임을 비롯 한 대부분의 응용 프로그램의 값을 전체 360도 필요 하므로 `Math` 클래스를 포함 `Atan2` 이 요구를 충족 하기 위해 합니다.

위의 코드를 통과 하는지 Y 매개 변수를 먼저 다음 X 매개 변수를 호출 하는 경우 확인 된 `Atan2` 메서드. 이 이전 버전과 X, Y 위치 좌표의 순서는 일반적인에서입니다. 자세한 내용은 [Atan2 문서 참조](https://msdn.microsoft.com/library/system.math.atan2(v=vs.110).aspx)합니다.

주목할 만한 이기도에서 반환 된 값을 `Atan2` 라디안으로 각도 측정 하는 데 사용 되는 다른 단위인 됩니다. 이 가이드 라디안의 세부 정보를 포함 하지 않지만 염두에서에 둬야 하의 모든 삼각 함수는 `System.Math` 네임 스페이스 사용 하 여 radians, 모든 값도 CocosSharp 개체에 사용 되기 전에 변환 해야 합니다. 라디안에서 자세한 정보를 찾을 수 [라디안 Wikipedia 페이지에서](http://en.wikipedia.org/wiki/Radian)합니다.

#### <a name="forward-angle"></a>정방향 각도

한 번의 `FacePoint` 메서드는 각도를 라디안으로 변환, 정의 `forwardAngle` 값입니다. 이 값은 해당 회전 값이 0 인 경우 엔터티 연결 각도를 나타냅니다. 이 예제는 엔터티 향하도록 위쪽으로, (CocosSharp 회전) 대비 수학 회전을 사용 하는 경우 90도 가정 합니다. 여기에서 사용 된 수학 회전 CocosSharp 회전 아직 반전 하지 않은 것 때문입니다.

다음 테이블에 나와 있는 어떤 엔터티는 `forwardAngle` 90도 아래와 같습니다.

![](math-images/image6.png "90도 forwardAngle가 있는 엔터티 모양을 표시합니다")


### <a name="angled-velocity"></a>각 진된 개발 속도

지금까지 각도에 오프셋을 변환 하는 방법을 알아보았습니다. 이 섹션, 다른 방법으로-는 각도 가져와 들어가고 X로 변환 및 Y 값입니다. 일반적인 예로, 향하고 방향 또는 촬영 글머리 배송 향하도록 방향으로 이동 하는 공간 선적 이동 자동차 있습니다. 

개념적으로, 해당 속도 엔터티 향하도록 각도로 회전 한 다음 회전 되지 않은 경우 원하는 개발 속도 먼저 정의 하 여 개발 속도 계산할 수 있습니다. 을이 개념을 보여 주기 위해 개발 속도 (및 가속) 테이블로 시각화할 수는 2 차원 *벡터* (은 일반적으로 그려질 화살표로). X 사용 하 여 개발 속도 값에 대 한 벡터 = 100이 고 Y = 0을 다음과 같이 시각화할 수 있습니다.

![](math-images/image7.png "X 사용 하 여 개발 속도 값에 대 한 벡터 = 100이 고 Y = 0을 다음과 같이 시각화할 수 있습니다.")

새 개발 속도 결과적으로이 벡터를 회전할 수 있습니다. 예를 들어 벡터를 (시계 반대 방향으로 회전 사용) 45도 회전 결과 다음과 같습니다.

![](math-images/image8.png "벡터를 시계 반대 방향으로 회전 결과 사용 하 여이 45도 회전")

다행히 속도, 가속도, 및에 위치 모두 회전할 수 값이 적용 되는 방법에 관계 없이 동일한 코드와 함께 합니다. CocosSharp 회전 값으로 벡터를 회전 하려면 다음과 같은 일반 용도의 함수를 사용할 수 있습니다.


```csharp
// Rotates the argument vector by degrees specified by
// cocosSharpDegrees. In other words, the rotation
// value is expected to be clockwise.
// The vector parameter is modified, so it is both an in and out value
void RotateVector(ref CCVector2 vector, float cocosSharpDegrees)
{
    // Invert the rotation to get degrees as is normally
    // used in math (counterclockwise)
    float mathDegrees = -cocosSharpDegrees;

    // Convert the degrees to radians, as the System.Math
    // object expects arguments in radians
    float radians = CCMathHelper.ToRadians (mathDegrees);

    // Calculate the "up" and "right" vectors. This is essentially
    // a 2x2 matrix that we'll use to rotate the vector
    float xAxisXComponent = (float)System.Math.Cos (radians);
    float xAxisYComponent = (float)System.Math.Sin (radians);
    float yAxisXComponent = (float)System.Math.Cos (radians + CCMathHelper.Pi / 2.0f);
    float yAxisYComponent = (float)System.Math.Sin (radians + CCMathHelper.Pi / 2.0f);

    // Store the original vector values which will be used
    // below to perform the final operation of rotation.
    float originalX = vector.X;
    float originalY = vector.Y;

    // Use the axis values calculated above (the matrix values)
    // to rotate and assign the vector.
    vector.X = originalX * xAxisXComponent + originalY * yAxisXComponent;
    vector.Y = originalX * xAxisYComponent + originalY * yAxisYComponent;
} 
```

완전히 이해는 `RotateVector` 메서드는이 문서의 범위를 벗어납니다 일부 선형 대 수와 함께 코사인 및 사인 삼각 함수에 잘 모르는 필요 합니다. 그러나 한 번만 구현 된 `RotateVector` 속도 벡터를 포함 하 여 모든 벡터를 회전 하려면 메서드를 사용할 수 있습니다. 예를 들어 다음 코드를 발생 시킬 수로 지정 된 방향의 글머리 기호는 `rotation` 값:


```csharp
// Create a Bullet instance
Bullet newBullet = new Bullet();

// Define the velocity of the bullet when 
// rotation is 0
CCVector2 velocity = new CCVector2 (0, 100);

// Modify the velocity according to rotation
RotateVector (ref velocity, rotation);

// Assign the newBullet's velocity using the
// rotated vector
newBullet.VelocityX = velocity.X;
newBullet.VelocityY = velocity.Y;

// Set the bullet's rotation so it faces
// the direction that it's flying
newBullet.Rotation = rotation; 
```

이 코드는 다음과 같이 발생할 수 있습니다.

![](math-images/image9.png "이 코드는이 스크린샷 같은 화면이 생성 될 수 있습니다.")


## <a name="summary"></a>요약

이 가이드에서는 2D 게임 개발의 일반적인 수학적 개념을 다룹니다. 할당 및 개발 속도 및 가속을 구현 하는 방법을 보여 줍니다. 하 고 개체와 모든 방향에서 이동 벡터를 회전 하는 방법을 설명 합니다.
