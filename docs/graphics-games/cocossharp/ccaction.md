---
title: "CCAction 사용한 애니메이션"
description: "CCAction 클래스 추가 애니메이션 CocosSharp 게임을 간소화합니다. 이러한 애니메이션 기능을 구현 하거나 폴란드어를 추가 하려면 사용할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 74DBD02A-6F10-4104-A61B-08CB49B733FB
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 2852cf0e141e8239cee8dbe580576f4571c919a3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="animating-with-ccaction"></a>CCAction 사용한 애니메이션

_CCAction 클래스 추가 애니메이션 CocosSharp 게임을 간소화합니다. 이러한 애니메이션 기능을 구현 하거나 폴란드어를 추가 하려면 사용할 수 있습니다._

`CCAction` CocosSharp 개체에 애니메이션 적용 하는 데 사용할 수 있는 기본 클래스가입니다. 이 가이드에서는 기본 제공 `CCAction` 위치, 크기 조정 및 회전 등의 일반적인 작업에 대 한 구현 합니다. 상속 하 여 사용자 지정 구현을 만드는 방법에 대해서도 설명 `CCAction`합니다.

이 가이드에서는 라는 프로젝트 **ActionProject** 있는 [다운로드할 수 있습니다](https://developer.xamarin.com/samples/mobile/CCAction)합니다. 이 가이드에서는 `CCDrawNode` 에서 클래스는 [CCDrawNode 된 기 하 도형 그리기](~/graphics-games/cocossharp/ccdrawnode.md) 가이드입니다.


# <a name="running-the-actionproject"></a>ActionProject 실행

**ActionProject** iOS 및 Android에 대 한 구현 될 수 있는 CocosSharp 솔루션입니다. 역할 둘 다 사용 하는 방법에 대 한 코드 샘플으로는 `CCAction` 클래스 및 일반적으로 실시간 데모로 `CCAction` 구현 합니다.

ActionProject 세 개를 표시, 실행 하는 경우 `CCLabel` 화면 및 두 개의 그려진 시각적 개체의 왼쪽에 인스턴스 `CCDrawNode` 다양 한 작업 보기에 대 한 인스턴스:

![](ccaction-images/image1.png "ActionProject 화면 및 다양 한 작업을 보기 위한 두 개의 CCDrawNode 인스턴스 그려진 시각적 개체의 왼쪽에 세 개의 CCLabel 인스턴스가 표시 됩니다.")

왼쪽에 있는 레이블의 표시 유형을 `CCAction` 화면의 누를 때 생성 됩니다. 기본적으로는 **위치** 값이 선택 되어 결과적으로 `CCMove` 동작을 빨간색 원이에 적용:

![](ccaction-images/image2.gif "위치 값이 선택 만들고 빨간색 원이에 적용할 되 CCMove 작업의 결과")

왼쪽에 있는 레이블을 클릭 하면 어떤 유형의 변경 `CCAction` 원에 수행 됩니다. 예를 들어,를 클릭 하 고 **위치** 레이블을 변경할 수 있는 다양 한 값을 번갈아 표시 합니다.

![](ccaction-images/image3.gif "변경할 수 있는 다양 한 값을 번갈아 표시 위치 레이블을 클릭 하면")


# <a name="common-variable-changing-ccactions"></a>일반적인 변수 변경 CCActions 

**ActionProject** 에서는 다음과 같은 `CCAction`-CocosSharp의 일부인 클래스 상속:

 - `CCMoveTo` – 변경는 `CCNode` 인스턴스의 `Position` 속성
 - `CCScaleTo` – 변경는 `CCNode` 인스턴스의 `Scale` 속성
 - `CCRotateTo` – 변경는 `CCNode` 인스턴스의 `Rotation` 속성

이 설명서에서는 이러한 작업으로 *변수 변경*의 변수는 직접 영향을 의미 하는 `CCNode` 에 추가 됩니다. 다른 유형의 작업 이라고 *감속/가속* 이 가이드의 뒷부분에서 설명 하는 동작입니다.

**ActionProject** 이러한 작업의 목적은 시간에 따라 변수를 수정 하는 것을 보여 줍니다. 특히, 이러한 `CCActions` 생성자 인수 두 개: 되려면 기간 및 할당할 값입니다. 코드의 다음 부분에서는 세 동작 유형에 만드는 방법을 보여 줍니다.


```csharp
switch (VariableOptions [currentVariableIndex])
{
    case "Position":
        coreAction = new CCMoveTo(timeToTake, touch.Location);

        break;
    case "Scale":
        var distance = CCPoint.Distance (touch.Location, drawNodeRoot.Position);
        var desiredScale = distance / DefaultCircleRadius;
        coreAction = new CCScaleTo(timeToTake, desiredScale);

        break;
    case "Rotation":
        float differenceY = touch.Location.Y - drawNodeRoot.PositionY;
        float differenceX = touch.Location.X - drawNodeRoot.PositionX;

        float angleInDegrees = -1 * CCMathHelper.ToDegrees(
            (float)System.Math.Atan2(differenceY, differenceX));

        coreAction = new CCRotateTo (timeToTake, angleInDegrees);

        break; 
...
}
```

동작이 생성 되는 CCNode에 다음과 같은 추가 됩니다.


```csharp
nodeToAddTo.AddAction (coreAction); 
```

`AddAction` 시작 된 `CCAction` 인스턴스의 동작 하며는 완료 될 때까지 해당 논리 프레임 후-프레임을 자동으로 수행 됩니다.

라는 단어로 끝나는 위에 나열 된 각 형식 *를* 의미는 `CCAction` 수정 됩니다.는 `CCNode` 하 여 인수 값 작업이 완료 되 면 최종 상태를 나타냅니다. 예를 들어 만들기는 `CCMoveTo` 100 및 Y = X의 위치와 = 200는 `CCNode` 인스턴스의 `Position` X로 설정 되 고 = 100, Y = 200에 관계 없이 지정 된 시간 후에는 `CCNode` 인스턴스 시작 위치 합니다.

각 "To" 클래스 "에서" 하 여 버전에서 현재 값으로 인수 값을 추가 합니다에 `CCNode`합니다. 예를 들어 만들기는 `CCMoveBy` 100 및 Y = X의 위치와 = 200 발생 합니다는 `CCNode` 작업이 시작 된 시점에 위치에서 오른쪽 100 단위 하 고 200 단위를 이동 하는 인스턴스.


# <a name="easing-actions"></a>감속/가속 작업

변수 변경 작업에 기본적으로 수행 *선형 보간을* – 작업은 일정 한 비율로에 원하는 값으로 이동 합니다. 보간 하는 경우 *위치* 연속적으로 이동 개체 즉시 시작 되며 시작 및 끝 동작의 이동을 중지 하 고 작업이 실행 되는 속도가 상수 유지 됩니다. 

비 선형 보간을 덜 jarring 이며 CocosSharp 다양 한 작업 변수 변경 동작을 수정 하는 데 사용할 수 있는 감속/가속 제공 하므로 폴란드어의 요소를 추가 합니다.

에 **ActionProject** 샘플에서는 이러한 유형의 두 번째 레이블을 클릭 하 여 감속/가속 작업 간에 전환할 수 있습니다 (데이터베이스가 기본 인  **<None>** ):

![](ccaction-images/image4.gif "사용자는 이러한 유형의 두 번째 레이블을 클릭 하 여 감속/가속 작업 간에 전환할 수 있습니다.")

감속/가속 작업은 특정 변수 설정 작업에 고정 되지 않은 때문에 특히 유용 합니다. 이 동일한 감속/가속 작업 할당 위치, 회전, 크기 조정, 또는 사용자 지정 작업 (이 가이드의 뒷부분에 나오는 표시 됩니다)를 사용할 수 있다는 것을 의미 합니다.

감속/가속 작업 변수 설정 작업을 래핑할 (변수 설정 작업에서 상속으로 `CCFiniteTimeAction`) 해당 생성자에 대 한 인수로 변수 설정 작업을 사용 하 여 합니다.

예를 들어, 레이블이로 설정 된 경우 **위치**, **CCEaseElastic**, 터치 검색 되 면 다음 코드는 실행 한 다음 (참고 코드는 생략 관련 줄 강조 표시):


```csharp
CCFiniteTimeAction coreAction = null; 
...
coreAction = new CCMoveTo(timeToTake, touch.Location); 
...
CCAction easing = null; 
...
easing = new CCEaseSineOut (coreAction); 
...
nodeToAddTo.AddAction (easing); 
```

응용 프로그램에 의해 표시 된 것과 같이 정확히 동일한 부드럽게에 적용할 수 있습니다 다른 변수 설정 작업 같은 `CCRotateTo`:

![](ccaction-images/image5.gif "정확히 동일한 감속/가속 CCRotateTo 등의 다른 변수 설정 작업에 적용할 수 있습니다.")


# <a name="easing-in-out-and-inout"></a>In, Out 및 InOut 감속/가속

모든 감속/가속 작업 `In`, `Out`, 또는 `InOut` 감속/가속 형식에 추가 합니다. 적용 되는 감속/가속 때 이러한 용어 참조: `In` 하면 시작 되 면 감속/가속 적용 되지 것입니다 `Out` 끝나도 의미 및 `InOut` 시작과 끝에 모두 의미 합니다.

`In` 변수 (둘 다 시작과 끝), 전체 보간 전체적으로 적용 되는 방법에 영향을 줍니다 작업 감속/가속 있지만 일반적으로 가장 인식할 수 있는 특성 감속/가속 작업을 시작할 때 적용 됩니다. 마찬가지로, `Out` 감속/가속 작업 보간 끝날 때의 동작에 따라 구분 됩니다. 예를 들어 `CCEaseBounceOut` 반송 동작의 끝에 개체에 발생 합니다.


## <a name="out"></a>Out

`Out` 일반적으로 감속/가속 보간의 끝에서 가장 눈에 띄는 변경 내용을 적용 합니다. 예를 들어 `CCEaseExponentialOut` 대상 값 가까워지면 변경 변수의 변동률 느려집니다.

![](ccaction-images/image6.gif "대상 값 가까워지면 CCEaseExponentialOut 변경 변수의 변동률을 느려집니다.")


## <a name="in"></a>입력

`In` 일반적으로 감속/가속에서 가장 큰 변화 보간의 시작 부분에 적용 됩니다. 예를 들어 `CCEaseExponentialIn` 동작의 시작 부분에 더 느리게 이동 합니다.

![](ccaction-images/image7.gif "CCEaseExponentialIn 동작의 시작 부분에 더 느리게 이동 합니다.")


## <a name="inout"></a>InOut

`InOut` 일반적으로 둘 다 시작과 끝에서 가장 눈에 띄는 변경 내용이 적용 됩니다. `InOut` 감속/가속가 일반적으로 대칭 키입니다. 예를 들어 `CCEaseExponentialInOut` 동작의 시작과 끝에 느린 이동 됩니다.

![](ccaction-images/image8.gif "시작 및 끝 동작의 CCEaseExponentialInOut 느린 이동 합니다.")


# <a name="implementing-a-custom-ccaction"></a>사용자 지정 CCAction 구현

모든까지 지금까지 설명한 클래스의 일반적인 기능을 제공 CocosSharp에 포함 됩니다. 사용자 지정 `CCAction` 구현을 추가적인 유연성을 제공할 수 있습니다. 예를 들어 한 `CCAction` 사용자 경험을 획득 될 때마다 경험 모음 원활 하 게 증가 되도록를 제어 경험 막대의 채워진된 비율을 사용할 수 있습니다.

`CCAction` 구현에는 일반적으로 두 개의 클래스가 필요합니다.

 - `CCFiniteTimeAction` 구현 – 유한 시간 동작 클래스는 시작 작업을 담당 합니다. 인스턴스화된 클래스 이며에 직접 추가 하거나는 `CCNode` 또는 감속/가속 작업 합니다. 인스턴스화하고 반환 해야 합니다는 `CCFiniteTimeActionState`, 업데이트 수행 됩니다.
 - `CCFiniteTimeActionState` 구현 – 유한 시간 동작 상태 클래스는는 작업에 관련 된 변수를 업데이트 하는 일을 담당 합니다. 시간 값에 따라 대상에서 값을 할당 하는 업데이트 함수를 구현 해야 합니다. 이 클래스 외부에 명시적으로 참조 되지 않은 `CCFiniteTimeAction` 생성 되어 있습니다. 단순히 "백그라운드" 작동합니다.

**ActionProject** 제공 사용자 지정 `CCFiniteTimeAction` 호출 구현 `LineWidthAction,` 의 빨간색 원을 위에 그린 노란색 줄 너비를 조정 하는 데 사용 됩니다. 유일한 일 인스턴스화하고 반환 하는 것 입니다는 `LineWidthState` 인스턴스:


```csharp
public class LineWidthAction : CCFiniteTimeAction
{
    float endWidth;

    public LineWidthAction (float duration, float width) : base(duration)
    {
        endWidth = width;
    }

    public override CCFiniteTimeAction Reverse ()
    {
        throw new NotImplementedException ();
    }

    protected override CCActionState StartAction (CCNode target)
    {
        return new LineWidthState (this, target, endWidth);
    }
}
```

위에서 설명 했 듯이 `LineWidthState` 선의 할당 한 작업을 수행 `Width` 크기에 따라 속성 `time` 통과:


```csharp
public class LineWidthState : CCFiniteTimeActionState
{
    float deltaWidth;
    float startWidth;

    LineNode castedTarget;

    public LineWidthState(LineWidthAction action, CCNode target, float endWidth) : base(action, target)
    {
        castedTarget = target as LineNode;

        if (castedTarget == null)
        {
            throw new InvalidOperationException ("The argument target must be a LineNode");
        }

        startWidth = castedTarget.Width;
        deltaWidth = endWidth - startWidth;
    }

    public override void Update (float time)
    {
        castedTarget.Width = startWidth + deltaWidth * time;
    }
} 
```

LineWidthAction 다음 애니메이션에 표시 된 대로 다양 한 방법으로 선 두께 변경 하려면 모든 감속/가속 작업으로 결합할 수 있습니다.

![](ccaction-images/image9.gif "감속/가속 행위가 다양 한 방법으로 선 두께 변경 하려면이 애니메이션에서 표시 된 것 처럼 LineWidthAction는 함께 사용할 수 있습니다.")


## <a name="interpolation-and-the-update-method"></a>보간 및 Update 메서드

유일한 논리 위의 클래스에 값을 저장 하는 것 외에 거주 하 고는 `LineWidthState.Update` 메서드. `startWidth` 변수 저장 대상의 너비 `LineNode` 동작의 시작 부분에 및 `deltaWidth` 변수 저장 된 작업 과정을 통해 얼마나 많은 값이 변경 됩니다.

대체 하 여는 `time` 값이 0 인 변수를 볼 수 있습니다 하는 대상 `LineNode` 시작 위치에 배치 됩니다.


```csharp
castedTarget.Width = startWidth + deltaWidth * 0; 
```

마찬가지로, 수 있다는 것을 알 대상 `LineNode` 시간 변수 값이 1 인 대체 하 여 대상에 됩니다.


```csharp
castedTarget.Width = startWidth + deltaWidth * 1; 
```

`time` 값 0과 1-사이의 일반적으로 됩니다-항상 그렇지는 않지만 및 `Update` 구현은이 경계를 가정 하지 않아야 합니다. 감속/가속 일부 메서드 (예: `CCEaseBackIn` 및 `CCEaseBackOut`) 0 ~ 1 범위를 벗어나는 시간 값을 제공 합니다.


# <a name="conclusion"></a>결론

보간 및 감속/가속는 특히 사용자 인터페이스를 만들 때 세련된 게임 만들기의 중요 한 부분입니다. 이 가이드에서 사용 하는 방법을 설명 `CCActions` 위치 및 회전 등의 표준 값 뿐만 아니라 사용자 지정 값을 보간 하 합니다. `LineWidthState` 및 `LineWidthAction` 클래스 사용자 지정 작업을 구현 하는 방법을 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [CCAction](https://developer.xamarin.com/api/type/CocosSharp.CCAction)
- [CCMoveTo](https://developer.xamarin.com/api/type/CocosSharp.CCMoveTo)
- [CCScaleTo](https://developer.xamarin.com/api/type/CocosSharp.CCScaleTo)
- [CCRotateTo](https://developer.xamarin.com/api/type/CocosSharp.CCRotateTo)
- [CCDrawNode](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode)
- [전체 샘플](https://developer.xamarin.com/samples/mobile/CCAction/)
