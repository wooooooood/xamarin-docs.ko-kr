---
title: CCDrawNode 사용한 기 하 도형 그리기
description: 이 문서에서는 CCDrawNode 선, 원 및 삼각형 같은 기본 개체를 그리기 위한 메서드를 제공 하는 설명 합니다.
ms.prod: xamarin
ms.assetid: 46A3C3CE-74CC-4A3A-AB05-B694AE182ADB
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: b910e136366c429de8bd2ba1ac959882b4d7201d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123190"
---
# <a name="drawing-geometry-with-ccdrawnode"></a>CCDrawNode 사용한 기 하 도형 그리기

_`CCDrawNode` 선, 원 및 삼각형 같은 기본 개체를 그리기 위한 메서드를 제공합니다._

`CCDrawNode` CocosSharp의 클래스는 일반적인 기 하 도형을 그리기 위한 여러 메서드를 제공 합니다. 상속 된 `CCNode` 클래스 및 일반적으로 추가할 `CCLayer` 인스턴스. 이 가이드를 사용 하는 방법에 설명 `CCDrawNode` 인스턴스 사용자 지정 렌더링을 수행 합니다. 또한 스크린샷 및 코드 예제를 사용 하 여 포괄적인 목록을 사용할 수 있는 그리기 함수를 제공합니다.


## <a name="creating-a-ccdrawnode"></a>CCDrawNode를 만들기

`CCDrawNode` 클래스 원, 사각형 및 선과 같은 기하학적 개체를 그리는 데 사용할 수 있습니다. 다음 코드 샘플을 만드는 방법을 표시 하는 예를 들어를 `CCDrawNode` 원을 그립니다 인스턴스는 `CCLayer` 클래스 구현:


```csharp
public class GameLayer : CCLayer
{
    public GameLayer ()
    {
        var drawNode = new CCDrawNode ();
        this.AddChild (drawNode);
        // Origin is bottom-left of the screen. This moves
        // the drawNode 100 pixels to the right and 100 pixels up
        drawNode.PositionX = 100;
        drawNode.PositionY = 100;

        drawNode.DrawCircle (
            center: new CCPoint (0, 0),
            radius: 20,
            color: CCColor4B.White);

    }
} 
```

이 코드는 런타임 시 다음 원을 생성합니다.

![](ccdrawnode-images/image1.png "이 코드는 런타임 시이 원을 생성합니다.")


## <a name="draw-method-details"></a>그리기 메서드 세부 정보

그리기와 관련 된 몇 가지 세부 정보를 살펴보겠습니다는 `CCDrawNode`:


### <a name="draw-methods-positions-are-relative-to-the-ccdrawnode"></a>CCDrawNode를 기준으로 draw 메서드 위치는

그리기 메서드는 모든 그리기에 대 한 하나 이상의 위치 값이 필요 합니다. 이 위치 값은 상대적으로 `CCDrawNode` 인스턴스. 즉를 `CCDrawNode` 자체에 위치 및 모든 그리기에 대 한 호출을 `CCDrawNode` 하나 이상의 위치 값을 사용할 수도 있습니다. 이러한 값을 결합 하는 방법을 이해 하려면 살펴보겠습니다 몇 가지 예입니다.

먼저 살펴보겠습니다는 `DrawCircle` 위의 예제:


```csharp
...
drawNode.PositionX = 100;
drawNode.PositionY = 100;

drawNode.DrawCircle (center: new CCPoint (0, 0),
...
```

이 경우에 `CCDrawNode` (100,100)에 배치 됩니다 고 그려진된 원이 (0, 0)에 상대적인는 `CCDrawNode`, 최대 및 게임 화면의 왼쪽 아래 모퉁이의 오른쪽에 100 픽셀 가운데 맞춤된 되 고 원에 결과.

`CCDrawNode` (화면 맨 아래 왼쪽), 원점에 배치 될 수도 있습니다 오프셋에 대 한 원에 의존 합니다.


```csharp
...
drawNode.PositionX = 0;
drawNode.PositionY = 0;

drawNode.DrawCircle (center: new CCPoint (50, 60),
...
```

위의 50 단위에서 원 중심의 결과 코드 (`drawNode.PositionX` + `CCPoint.X`) 화면 및 60 왼쪽의 오른쪽에 (`drawNode.PositionY` + `CCPoint.Y`) 화면 맨 위에 단위입니다.

그린된 개체 하지 않는 한 수정할 수 없습니다 그리기 메서드를 호출한 후 합니다 `CCDrawNode.Clear` 메서드를 호출 하므로 모든 위치에서 수행 해야 합니다 `CCDrawNode` 자체입니다.

개체를 그린 `CCNodes` 도 영향을 받는 합니다 `CCNode` 인스턴스의 `Rotation` 및 `Scale` 속성.


### <a name="draw-methods-do-not-need-to-be-called-every-frame"></a>그리기 메서드는 프레임 마다 호출할 필요가 없습니다.

그리기 메서드는 영구 시각적 개체 만들기를 한 번만 호출 해야 합니다. 에 대 한 호출 위의 예에서 `DrawCircle` 의 생성자에는 `GameLayer` – `DrawCircle` 화면 새로 고쳐지면 원을 다시 그리는 모든 프레임을 호출할 필요가 없습니다.

이 일반적으로 렌더링 됩니다 것 하나만 프레임에 대 한 화면 및 프레임 마다 호출 해야 하는 MonoGame 그리기 메서드에서 다릅니다.

그리기 메서드는 프레임 마다 호출 된 경우 개체를 호출 하는 내부 누적 결국 `CCDrawNode` 인스턴스 개체가 그려지는 프레임 속도가 떨어지는 발생 합니다.


### <a name="each-ccdrawnode-supports-multiple-draw-calls"></a>각 CCDrawNode 여러 그리기 호출을 지원

`CCDrawNode` 여러 셰이프를 그릴 인스턴스를 사용할 수 있습니다. 이렇게 하면 복잡 한 시각적 개체를 단일 개체에 막을 수 있습니다. 예를 들어, 다음 코드는 할 수 하나를 사용 하 여 여러 원을 렌더링 `CCDrawNode`:


```csharp
for (int i = 0; i < 8; i++)
{
    drawNode.DrawCircle (
        center: new CCPoint (i*15, 0),
        radius: 20,
        color: CCColor4B.White);
} 
```

이 인해 다음 그래픽:

![](ccdrawnode-images/image2.png "이 인해이 그래픽")


## <a name="draw-call-examples"></a>그리기 호출 예제

다음 그리기 호출에서 사용할 수 있는 `CCDrawNode`:

- [`DrawCatmullRom`](#drawcatmullrom)
- [`DrawCircle`](#drawcircle)
- [`DrawCubicBezier`](#drawcubicbezier)
- [`DrawEllipse`](#drawellipse)
- [`DrawLineList`](#drawlinelist)
- [`DrawPolygon`](#drawpolygon)
- [`DrawQuadBezier`](#drawquadbezier)
- [`DrawRect`](#drawrect)
- [`DrawSegment`](#drawsegment)
- [`DrawSolidArc`](#drawsolidarc)
- [`DrawSolidCircle`](#drawsolidcircle)
- [`DrawTriangleList`](#drawtrianglelist)


### <a name="drawcardinalspline"></a>DrawCardinalSpline

`DrawCardinalSpline` 다양 한 지점을 통해 곡선을 만듭니다. 

`config` 매개 변수는 스플라인 전달 하는 지점을 정의 합니다. 아래 예제에는 점이 4 개 통과 스플라인 보여 줍니다.

`tension` 뾰족한 또는 스플라인에서 점을 round 표시 방법 매개 변수입니다. A `tension` 곡선된 스플라인 값이 0 하면 및 `tension` 직선 및 하드 가장자리 그린 스플라인 값이 1 발생 합니다.

스플라인 곡선 이지만, CocosSharp 스플라인 직선을 사용 하 여 그립니다. `segments` 매개 변수는 스플라인 그리기 하는 데 얼마나 많은 세그먼트를 제어 합니다. 더 큰 숫자로 성능 대신 곡선된 스플라인 발생합니다. 

코드 예제:


```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (50, 70));
splinePoints.Add (new CCPoint (0, 140));
splinePoints.Add (new CCPoint (100, 210));

drawNode.DrawCardinalSpline (
    config: splinePoints,
    tension: 0,
    segments: 64,
    color:CCColor4B.Red); 
```

![](ccdrawnode-images/image3.png "세그먼트 매개 변수 개수 세그먼트의 스플라인 그리기를 사용 하는 제어")


### <a name="drawcatmullrom"></a>DrawCatmullRom

`DrawCatmullRom` 가변 개수의 비슷합니다 통해 곡선을 만듭니다 `DrawCardinalLine`합니다. 이 메서드는 장력 매개 변수를 포함 하지 않습니다.

코드 예제:

```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (80, 90));
splinePoints.Add (new CCPoint (100, 0));
splinePoints.Add (new CCPoint (0, 130)); 

drawNode.DrawCatmullRom (
    points: splinePoints,
    segments: 64); 
```

![](ccdrawnode-images/image4.png "DrawCatmullRom 가변 개수의 DrawCardinalLine 비슷합니다 통해 곡선을 만듭니다.")


### <a name="drawcircle"></a>DrawCircle

`DrawCircle` 원의 둘레 만듭니다는 지정 된 `radius`합니다.

코드 예제:

```csharp
drawNode.DrawCircle (
    center:new CCPoint (0, 0),
    radius:20,
    color:CCColor4B.Yellow); 
```

![](ccdrawnode-images/image5.png "DrawCircle 지정 된 반경 원의 경계를 만듭니다.")


### <a name="drawcubicbezier"></a>DrawCubicBezier

`DrawCubicBezier` 제어점을 사용 하 여 두 점 사이의 경로 설정 하려면 두 지점 간의 곡선을 그립니다.

코드 예제:

```csharp
drawNode.DrawCubicBezier (
    origin: new CCPoint (0, 0),
    control1: new CCPoint (50, 150),
    control2: new CCPoint (250, 150),
    destination: new CCPoint (170, 0),
    segments: 64,
    lineWidth: 1,
    color: CCColor4B.Green); 
```

 ![](ccdrawnode-images/image6.png "DrawCubicBezier 두 지점 간의 곡선을 그립니다.")


### <a name="drawellipse"></a>DrawEllipse

`DrawEllipse` 윤곽선을 만듭니다는 *타원*는 종종 라고 타원 (하지만 두 잡다한 동일 하지 않은). 타원의 모양을 정의할 수 있는 한 `CCRect` 인스턴스.

코드 예제:


```csharp
drawNode.DrawEllipse (
    rect: new CCRect (0, 0, 130, 90),
    lineWidth: 2,
    color: CCColor4B.Gray); 
```

![](ccdrawnode-images/image8.png "사용한 타원으로 자주 참조 되는 타원의 개요를 만듭니다.")


### <a name="drawline"></a>DrawLine

`DrawLine` 지정 된 너비의 줄을 사용 하 여 지점에 연결합니다. 이 메서드는 유사한 `DrawSegment`round 끝점 달리 플랫 끝점을 만들면 제외 하 고 있습니다.

코드 예제:


```csharp
drawNode.DrawLine (
    from: new CCPoint (0, 0),
    to: new CCPoint (150, 30),
    lineWidth: 5,
    color:CCColor4B.Orange); 
```

![](ccdrawnode-images/image9.png "지정 된 너비의 줄을 사용 하 여 지점에 연결 하는 DrawLine")


### <a name="drawlinelist"></a>DrawLineList

`DrawLineList` 각 쌍으로 지정 된 지점에 연결 하 여 여러 줄을 만듭니다는 `CCV3F_C4B` 배열입니다. `CCV3F_C4B` 위치와 색에 대 한 값을 포함 하는 구조체입니다. `verts` 두 지점에서 각 줄은 정의 된 대로 매개 변수 요소 수는 짝수를 항상 포함 해야 합니다.

코드 예제:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First line:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    // second line, will blend from white to red:
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(120,120), CCColor4B.Red)
};

drawNode.DrawLineList (verts); 
```

 ![](ccdrawnode-images/image10.png "각 줄은 두 지점에서 정의 된 대로 verts 매개 지점 수는 짝수를 항상 포함 해야")




### <a name="drawpolygon"></a>DrawPolygon

`DrawPolygon` 가변 너비 및 색 윤곽선이 있는 채워진 다각형을 만듭니다.

코드 예제:


```csharp
CCPoint[] verts = new CCPoint[] {
    new CCPoint(0,0),
    new CCPoint(0, 100),
    new CCPoint(50, 150),
    new CCPoint(100, 100),
    new CCPoint(100, 0)
};

drawNode.DrawPolygon (verts,
    count: verts.Length,
    fillColor: CCColor4B.White,
    borderWidth: 5,
    borderColor: CCColor4B.Red,
    closePolygon: true); 
```

![](ccdrawnode-images/image11.png "DrawPolygon 가변 너비 및 컬러 윤곽선이 있는 채워진 다각형을 만듭니다.")


### <a name="drawquadbezier"></a>DrawQuadBezier

`DrawQuadBezier` 두 개의 점을 선으로 연결합니다. 유사 하 게 동작 `DrawCubicBezier` 만 단일 제어 지점을 지원 합니다.

코드 예제:


```csharp
drawNode.DrawQuadBezier (
    origin:new CCPoint (0, 0),
    control:new CCPoint (200, 0),
    destination:new CCPoint (0, 300),
    segments:64,
    lineWidth:1,
    color:CCColor4B.White);
```

![](ccdrawnode-images/image12.png "DrawQuadBezier 선으로 두 점을 연결합니다")


### <a name="drawrect"></a>DrawRect

`DrawRect` 가변 너비 및 색 윤곽선이 있는 채워진 사각형을 만듭니다.

코드 예제: 


```csharp
var shape = new CCRect (
    0, 0, 100, 200);
drawNode.DrawRect(shape,
    fillColor:CCColor4B.Blue,
    borderWidth: 4,
    borderColor:CCColor4B.White); 
```

![](ccdrawnode-images/image13.png "DrawRect 가변 너비 및 컬러 윤곽선이 있는 채워진 사각형을 만듭니다.")


### <a name="drawsegment"></a>DrawSegment

`DrawSegment` 가변 너비 및 색의 선을 사용 하 여 두 점을 연결 합니다. 비슷합니다 `DrawLine`플랫 끝점 대신 둥근 끝점을 만들면 제외 하 고 있습니다.

코드 예제:


```csharp
drawNode.DrawSegment (from: new CCPoint (0, 0),
    to: new CCPoint (100, 200),
    radius: 5,
    color:new CCColor4F(1,1,1,1)); 
```

![](ccdrawnode-images/image14.png "DrawSegment 가변 너비 및 색의 선을 사용 하 여 두 점을 연결합니다")


### <a name="drawsolidarc"></a>DrawSolidArc

`DrawSolidArc` 지정 된 색 및 radius 채워진 쐐기형을 만듭니다.

코드 예제:


```csharp
drawNode.DrawSolidArc(
    pos:new CCPoint(100, 100),
    radius:200,
    startAngle:0,
    sweepAngle:CCMathHelper.Pi/2, // this is in radians, clockwise
    color:CCColor4B.White); 
```

![](ccdrawnode-images/image15.png "DrawSolidArc 채워진 쐐기형 지정 된 색 및 radius를 만듭니다.")


### <a name="drawsolidcircle"></a>DrawSolidCircle

`DrawCircle` 지정 된 반경의 채워진 원을 만듭니다.

코드 예제:


```csharp
drawNode.DrawSolidCircle(
    pos: new CCPoint (100, 100),
    radius: 50,
    color: CCColor4B.Yellow); 
```

![](ccdrawnode-images/image16.png "DrawCircle 지정 된 반경의 채워진 원을 만듭니다.")


### <a name="drawtrianglelist"></a>DrawTriangleList

`DrawTriangleList` 삼각형의 목록을 만듭니다. 각 삼각형 정의한 세 개의 `CCV3F_C4B` 배열에는 인스턴스. 에 전달 된 배열에 꼭 짓 점 수를 `verts` 매개 변수 3의 배수 여야 합니다. 에 포함 된 유일한 정보는 `CCV3F_C4B` 는 verts 및 해당 색-위치는 `DrawTriangleList` 메서드 질감을 사용 하 여 그리기 삼각형을 지원 하지 않습니다.

코드 예제:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First triangle:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    // second triangle, each point has different colors:
    new CCV3F_C4B( new CCPoint(90,0), CCColor4B.Yellow),
    new CCV3F_C4B( new CCPoint(120,60), CCColor4B.Red),
    new CCV3F_C4B( new CCPoint(150,0), CCColor4B.Blue)
};

drawNode.DrawTriangleList (verts); 
```

![](ccdrawnode-images/image17.png "DrawTriangleList은 삼각형의 목록을 만듭니다.")


## <a name="summary"></a>요약

이 가이드를 만드는 방법을 설명는 `CCDrawNode` 기본 기반 렌더링을 수행 합니다. 각 그리기 호출의 예제를 제공 합니다.

## <a name="related-links"></a>관련 링크

- [CCDrawNode API](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
- [전체 샘플](https://developer.xamarin.com/samples/mobile/CCDrawNode/)
