---
title: CCAction을 사용한
description: CCAction 클래스 추가 애니메이션 CocosSharp 게임을 간소화합니다. 이러한 애니메이션 기능을 구현 하거나 폴란드어를 추가 하려면 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 74DBD02A-6F10-4104-A61B-08CB49B733FB
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: c486bb2e78579360e0f935219cd82958fedee34b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61357702"
---
# <a name="animating-with-ccaction"></a>CCAction을 사용한

_CCAction 클래스 추가 애니메이션 CocosSharp 게임을 간소화합니다. 이러한 애니메이션 기능을 구현 하거나 폴란드어를 추가 하려면 사용할 수 있습니다._

`CCAction` CocosSharp 개체에 애니메이션 효과를 사용할 수 있는 기본 클래스입니다. 이 가이드에서는 기본 제공 `CCAction` 위치, 크기 조정 및 회전 같은 일반적인 태스크의 구현입니다. 상속 하 여 사용자 지정 구현을 만드는 방법에 대해서도 살펴봅니다 `CCAction`합니다.

이 가이드에서는 라는 프로젝트를 **ActionProject** 는 [에서 다운로드할 수 있습니다](https://developer.xamarin.com/samples/mobile/CCAction)합니다. 이 가이드에서는 사용 합니다 `CCDrawNode` 에서 설명 하는 클래스를 [CCDrawNode 사용한 기 하 도형 그리기](~/graphics-games/cocossharp/ccdrawnode.md) 가이드입니다.


## <a name="running-the-actionproject"></a>ActionProject 실행

**ActionProject** iOS 및 Android 용 빌드되는 CocosSharp 솔루션입니다. 둘 다 사용 하는 방법에 대 한 코드 샘플으로 제공 합니다 `CCAction` 클래스와 일반적인 실시간 데모 `CCAction` 구현 합니다.

ActionProject 세 개를 표시, 실행 하는 경우 `CCLabel` 화면과 두 그린 시각적 개체의 왼쪽에 인스턴스 `CCDrawNode` 다양 한 작업 보기에 대 한 인스턴스:

![](ccaction-images/image1.png "ActionProject 화면 및 다양 한 작업 보기에 대 한 두 CCDrawNode 인스턴스 그린 시각적 개체의 왼쪽에서 세 가지 CCLabel 인스턴스가 표시 됩니다.")

왼쪽에 있는 레이블을 나타내는 형식을 `CCAction` 화면의 누를 때 생성 됩니다. 기본적으로 **위치** 값을 선택 하면 그 결과 `CCMove` 작업 만들어지고 빨간색 원에 적용:

![](ccaction-images/image2.gif "위치 값을 선택 하면 만들고 빨간색 원에 적용 되는 CCMove 작업의 결과")

어떤 유형의 변경 왼쪽에 있는 레이블을 클릭 하면 `CCAction` 원에 수행 됩니다. 예를 들어,를 클릭 하 여 **위치** 변경할 수 있는 다양 한 값 레이블을 번갈아 표시 됩니다.

![](ccaction-images/image3.gif "위치 레이블을 클릭 하는 변경할 수 있는 다른 값을 통해 순환")

## <a name="common-variable-changing-ccaction-classes"></a>일반적인 변수 변경 CCAction 클래스

합니다 **ActionProject** 다음을 사용 하 여 `CCAction`-CocosSharp의 일부인 클래스를 상속 합니다.

 - `CCMoveTo` -변경 된 `CCNode` 인스턴스의 `Position` 속성
 - `CCScaleTo` -변경 된 `CCNode` 인스턴스의 `Scale` 속성
 - `CCRotateTo` -변경 된 `CCNode` 인스턴스의 `Rotation` 속성

이 가이드에서는 이러한 작업을 *변수 변경*는 직접 영향을 미치기 변수에 의미를 `CCNode` 에 추가 되는 합니다. 다른 유형의 작업 이라고 *감속/가속* 이 가이드의 뒷부분에 나와 있는 작업입니다.

합니다 **ActionProject** 이러한 작업의 목적은 시간이 지남에 따라 변수를 수정 하는 방법을 보여 줍니다. 특히 이러한 `CCActions` 생성자 인수 두 개: 되려면 기간 및 할당할 값입니다. 다음 부분 코드는 세 가지 유형의 작업이 만들어지는 방법을 보여 줍니다.


```csharp
switch (VariableOptions [currentVariableIndex])
{
    case "Position":
        coreAction = new CCMoveTo(timeToTake, touch.Location);

        break;
    case "Scale":
        var distance = CCPoint.Distance (touch.Location, drawNodeRoot.Position);
        var desiredScale = distance / DefaultCircleRadius;
        coreAction = new CCScaleTo(timeToTake, desiredScale);

        break;
    case "Rotation":
        float differenceY = touch.Location.Y - drawNodeRoot.PositionY;
        float differenceX = touch.Location.X - drawNodeRoot.PositionX;

        float angleInDegrees = -1 * CCMathHelper.ToDegrees(
            (float)System.Math.Atan2(differenceY, differenceX));

        coreAction = new CCRotateTo (timeToTake, angleInDegrees);

        break; 
...
}
```

작업을 만든 후를 CCNode에 다음과 같은 추가 됩니다.


```csharp
nodeToAddTo.AddAction (coreAction); 
```

`AddAction` 시작 된 `CCAction` 인스턴스의 동작 하며는 완료 될 때까지 해당 논리 프레임 후-프레임을 자동으로 수행 합니다.

라는 단어로 끝나는 위에 나열 된 각 유형에 *에* 의미를 `CCAction` 수정를 `CCNode` 를 인수 값 작업을 마쳤을 때 최종 상태를 나타내도록 합니다. 예를 들어, 만들기를 `CCMoveTo` 100 및 Y = X의 위치를 사용 하 여 = 200 결과의 `CCNode` 인스턴스의 `Position` X로 설정 되 고 = 100를 Y = 200 끝에 관계 없이 지정 된 시간에는 `CCNode` 인스턴스 시작 위치.

각 "To" 클래스도 버전이 "에서"를 추가 하는 인수 값을 현재 값에 `CCNode`합니다. 예를 들어, 만들기를 `CCMoveBy` 100 및 Y = X의 위치를 사용 하 여 = 200 하면를 `CCNode` 인스턴스 작업을 시작한 경우에 위치에서 오른쪽 100 단위를 200 단위 이동 합니다.


## <a name="easing-actions"></a>감속/가속 작업

기본적으로 변수 변경 작업 수행 *선형 보간을* – 작업을 일정 한 속도로 원하는 값으로 이동 합니다. 효과 주는 하는 경우 *위치* 선형적으로 이동 개체가 즉시 시작 하 고 중지 작업의 시작과 끝에서 이동 하 고 작업이 실행 속도 상수 유지 됩니다. 

비선형 보간 덜 jarring 이며 CocosSharp 다양 한 변수 변경 동작을 수정 하려면 사용할 수 있는 작업을 간소화 하므로 폴란드어의 요소를 추가 합니다.

에 **ActionProject** 샘플에서는 이러한 유형의 두 번째 레이블을 클릭 하 여 감속/가속 작업 간에 전환할 수 있습니다 (기본값은 **<None>**):

![](ccaction-images/image4.gif "사용자는 이러한 유형의 두 번째 레이블을 클릭 하 여 감속/가속 작업 간에 전환할 수 있습니다.")

감속/가속 작업은 특정 변수 설정 작업에 고정 되지 않은 때문에 특히 유용 합니다. 즉, 동일한 감속/가속 작업 할당 위치, 회전, 규모 또는 사용자 지정 작업 (이 가이드의 뒷부분에 표시 됩니다)를 사용할 수 있도록 합니다.

감속/가속 작업 변수 설정 작업을 래핑할 (변수 설정 작업에서 상속 된다면 `CCFiniteTimeAction`) 해당 생성자에 인수로 변수 설정 작업을 그대로 사용 하 여 합니다.

예를 들어, 레이블이 설정 된 경우 **위치**하십시오 **CCEaseElastic**, 터치 감지 되 면 다음 코드를 실행 합니다 (참고 관련 줄을 강조 표시를 코드는 생략 되었습니다):


```csharp
CCFiniteTimeAction coreAction = null; 
...
coreAction = new CCMoveTo(timeToTake, touch.Location); 
...
CCAction easing = null; 
...
easing = new CCEaseSineOut (coreAction); 
...
nodeToAddTo.AddAction (easing); 
```

응용 프로그램에서 표시 된 것과 같이 정확히 동일한 간소화에 적용할 수 있습니다 다른 변수 설정 작업 같은 `CCRotateTo`:

![](ccaction-images/image5.gif "CCRotateTo 등의 다른 변수 설정 작업에 적용할 수는 정확 하 게 동일한 감속/가속")


## <a name="easing-in-out-and-inout"></a>, Out 및 InOut 감속/가속

모든 감속/가속 작업 `In`, `Out`, 또는 `InOut` 감속/가속 형식을 추가 합니다. 이러한 용어는 감속/가속 적용 될 때: `In` 감속/가속를 시작할 때 적용 되지 것입니다 `Out` 끝에 의미 및 `InOut` 시작과 끝에 모두 의미 합니다.

`In` 작업 감속/가속 변수 (둘 다 시작과 끝), 전체 보간 전체 적용 되는 방식으로 영향을 줍니다 하지만 일반적으로 감속/가속 작업의 가장 인식할 만한 특징에서 이루어지는 시작 합니다. 마찬가지로, `Out` 감속/가속 작업의 보간의 끝에 해당 동작에 따라 구분 됩니다. 예를 들어 `CCEaseBounceOut` 반송 동작의 끝에 개체에 발생 합니다.


### <a name="out"></a>Out

`Out` 일반적으로 감속/가속 보간의 끝에서 가장 눈에 띄는 변경 내용을 적용 합니다. 예를 들어 `CCEaseExponentialOut` 대상 값 가까워질수록 변경 변수의 변동률 느려집니다.

![](ccaction-images/image6.gif "대상 값 가까워질수록 CCEaseExponentialOut 변경 변수의 변동률을 느려집니다.")


### <a name="in"></a>입력

`In` 일반적으로 감속/가속에서 가장 두드러지게 변경의 보간의 시작 부분에 적용 됩니다. 예를 들어 `CCEaseExponentialIn` 동작의 시작 부분에서 느리게 이동 합니다.

![](ccaction-images/image7.gif "작업의 시작 부분에 CCEaseExponentialIn 느리게 이동")


### <a name="inout"></a>InOut

`InOut` 일반적으로 가장 눈에 띄는 변경 내용을 모두 시작 및 끝 부분에 적용 됩니다. `InOut` 감속/가속가 일반적으로 대칭 키입니다. 예를 들어 `CCEaseExponentialInOut` 작업의 시작과 끝에서 느리게 이동 합니다.

![](ccaction-images/image8.gif "작업의 시작과 끝에서 CCEaseExponentialInOut 느리게 이동")


## <a name="implementing-a-custom-ccaction"></a>사용자 지정 CCAction 구현

지금까지 설명한 클래스의 모든 공통 기능을 제공 하는 CocosSharp에 포함 됩니다. 사용자 지정 `CCAction` 구현은 추가적인 유연성을 제공할 수 있습니다. 예를 들어, 한 `CCAction` 사용자 환경을 수입이 때마다 환경을 막대를 원활 하 게 증가 되도록 환경 막대의 채워진된 비율 제어을 사용할 수 있습니다.

`CCAction` 구현에는 일반적으로 두 개의 클래스가 필요합니다.

 - `CCFiniteTimeAction` 구현 제한 된 시간 동작 클래스는 작업을 시작 하는 일을 담당 합니다. 인스턴스화된 클래스 되 고 직접 추가 하거나는 `CCNode` 또는 감속/가속 작업 합니다. 인스턴스화하고 반환 해야 합니다는 `CCFiniteTimeActionState`, 업데이트가 수행 됩니다.
 - `CCFiniteTimeActionState` 구현 제한 된 시간 작업 상태 클래스는 작업에 관련 된 변수를 업데이트 하는 일을 담당 합니다. 시간 값에 따라 대상의 값을 할당 하는 업데이트 함수를 구현 해야 합니다. 이 클래스 외부에 명시적으로 참조 되지 않은 `CCFiniteTimeAction` 만드는 것입니다. 단순히 "내부적인" 작동합니다.

**ActionProject** 사용자 지정을 제공 `CCFiniteTimeAction` 이라는 구현과 `LineWidthAction,` 빨간색 원을 위에 그려진 노란색 줄의 너비를 조정 하는 데 사용 됩니다. 인스턴스화하고 반환 하는 유일한 작업을 `LineWidthState` 인스턴스:


```csharp
public class LineWidthAction : CCFiniteTimeAction
{
    float endWidth;

    public LineWidthAction (float duration, float width) : base(duration)
    {
        endWidth = width;
    }

    public override CCFiniteTimeAction Reverse ()
    {
        throw new NotImplementedException ();
    }

    protected override CCActionState StartAction (CCNode target)
    {
        return new LineWidthState (this, target, endWidth);
    }
}
```

위에서 설명한 대로 합니다 `LineWidthState` 줄의 할당 하는 작업을 수행 `Width` 정도 따라 속성 `time` 경과:


```csharp
public class LineWidthState : CCFiniteTimeActionState
{
    float deltaWidth;
    float startWidth;

    LineNode castedTarget;

    public LineWidthState(LineWidthAction action, CCNode target, float endWidth) : base(action, target)
    {
        castedTarget = target as LineNode;

        if (castedTarget == null)
        {
            throw new InvalidOperationException ("The argument target must be a LineNode");
        }

        startWidth = castedTarget.Width;
        deltaWidth = endWidth - startWidth;
    }

    public override void Update (float time)
    {
        castedTarget.Width = startWidth + deltaWidth * time;
    }
} 
```

LineWidthAction 다음 애니메이션에 표시 된 대로 다양 한 방법으로 줄 너비를 변경 하려면 모든 감속/가속 작업으로 결합할 수 있습니다.

![](ccaction-images/image9.gif "다양 한 방법으로 선 두께 변경 하는 감속/가속 작업이이 애니메이션에 표시 된 대로 LineWidthAction은 함께 사용할 수 있습니다.")


### <a name="interpolation-and-the-update-method"></a>보간 및 Update 메서드

거주 하는 유일한 논리를 위의 클래스에 값을 저장 하는 것 외의 `LineWidthState.Update` 메서드. 합니다 `startWidth` 변수에 저장 대상의 너비 `LineNode` 작업을 시작 및 `deltaWidth` 변수에 저장 된 작업 과정을 통해 얼마나 많은 값이 변경 됩니다.

대체 하 여 합니다 `time` 값이 0 인 변수를 확인할 수 있는 대상 `LineNode` 시작 위치에 배치 됩니다.


```csharp
castedTarget.Width = startWidth + deltaWidth * 0; 
```

마찬가지로, 확인할 수 있는 대상 `LineNode` 값 1 사용 하 여 시간 변수를 대체 하 여 대상에 배치 됩니다.


```csharp
castedTarget.Width = startWidth + deltaWidth * 1; 
```

합니다 `time` 값은 일반적으로 0과-1 사이 여야 합니다-항상 그렇지는 않지만 및 `Update` 구현이 이러한 경계를 가정 하지 않아야 합니다. 감속/가속 일부 메서드 (같은 `CCEaseBackIn` 고 `CCEaseBackOut`) 0 ~ 1 범위 밖의 시간 값을 제공 합니다.


## <a name="conclusion"></a>결론

보간은 감속/가속와 사용자 인터페이스를 만드는 경우에 특히 고급 게임을 만드는 중요 한 부분입니다. 이 가이드를 사용 하는 방법에 설명 `CCActions` 위치 등 회전 표준 값 뿐만 아니라 사용자 지정 값을 보간 하 합니다. 합니다 `LineWidthState` 고 `LineWidthAction` 클래스에는 사용자 지정 작업을 구현 하는 방법을 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [CCAction](https://developer.xamarin.com/api/type/CocosSharp.CCAction)
- [CCMoveTo](https://developer.xamarin.com/api/type/CocosSharp.CCMoveTo)
- [CCScaleTo](https://developer.xamarin.com/api/type/CocosSharp.CCScaleTo)
- [CCRotateTo](https://developer.xamarin.com/api/type/CocosSharp.CCRotateTo)
- [CCDrawNode](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode)
- [전체 샘플](https://developer.xamarin.com/samples/mobile/CCAction/)
