---
title: 'Xamarin.Forms셰이프: 경로 태그 구문'
description: Xamarin.Forms경로 태그 구문을 사용 하면 XAML에서 경로 기 하 도형을 조밀 지정할 수 있습니다.
ms.prod: xamarin
ms.assetid: A2C1BD59-1A16-4E26-A825-0338E2AF9E65
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 943636ac82163c3c575577bb4c56f6433cf73339
ms.sourcegitcommit: 7fc658bbdcb8130cd9d611e55e79a1830fc5d5a2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85132949"
---
# <a name="xamarinforms-shapes-path-markup-syntax"></a>Xamarin.Forms셰이프: 경로 태그 구문

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Xamarin.Forms경로 태그 구문을 사용 하면 XAML에서 경로 기 하 도형을 조밀 지정할 수 있습니다. 구문은 속성에 문자열 값으로 지정 됩니다 `Path.Data` .

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Data="M13.908992,16.207977 L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983Z" />
```

경로 태그 구문은 선택적 `FillRule` 값과 하나 이상의 그림 설명으로 구성 됩니다. 이 구문은 `<Path Data="` *[fillrule]* *figureDescription* *[figureDescription]* 로 나타낼 수 있습니다. * `" ... />`

이 구문에서 다음을 수행 합니다.

- *Fillrule* 은 `Xamarin.Forms.Shapes.FillRule` geometry에서 또는를 사용 해야 하는지 여부를 지정 하는 선택적입니다 `EvenOdd` `Nonzero` `FillRule` . `F0`는 채우기 규칙을 지정 `EvenOdd` 하 고는 `F1` 채우기 규칙을 지정 합니다 `Nonzero` .
-  *figureDescription* 은 이동 명령, 그리기 명령 및 선택적 닫기 명령으로 구성 된 그림을 나타냅니다. Move 명령은 그림의 시작점을 지정 합니다. 그리기 명령은 그림의 내용을 설명 하 고 선택적 닫기 명령은 그림을 닫습니다.

위의 예제에서 path 태그 구문은 이동 명령 ()을 사용 하 여 시작점을 지정 하 `M` 고, 줄 명령 ()을 사용 하 여 일련의 직선을 지정 하 `L` 고, close 명령 ()을 사용 하 여 경로를 닫습니다 `Z` .

경로 태그 구문에서는 명령 앞 이나 뒤에 공백이 필요 하지 않습니다. 또한 두 개의 숫자를 쉼표 또는 공백으로 구분할 필요가 없지만이는 문자열이 명확 하지 않은 경우에만 수행할 수 있습니다.

> [!TIP]
> 경로 태그 언어는 SVG (스케일러블 벡터 그래픽) 이미지 경로 정의와 호환 되는 구문을 사용 하므로 SVG 형식에서 그래픽을 이식 하는 데 유용할 수 있습니다.

## <a name="move-command"></a>명령 이동

이동 명령은 새 그림의 시작점을 지정 합니다. 이 명령의 구문은 `M` *점* 또는 시작점 `m` *startPoint*입니다.

이 구문에서 시작점 *은* [`Point`](xref:Xamarin.Forms.Point) 새 그림의 시작점을 지정 하는 구조체입니다. Move 명령 뒤에 여러 개의 점이 나열 되 면 해당 점에 선이 그려집니다.

`M 10,10`는 유효한 move 명령의 예입니다.

## <a name="draw-commands"></a>그리기 명령

그리기 명령은 여러 도형 명령으로 구성될 수 있습니다. 사용할 수 있는 그리기 명령은 다음과 같습니다.

- 줄 ( `L` 또는 `l` )입니다.
- 가로 선 ( `H` 또는 `h` )입니다.
- 세로 선 ( `V` 또는 `v` )입니다.
- 입방 형 3 차원 곡선 ( `C` 또는 `c` )입니다.
- 정방형 3 차원 곡선 ( `Q` 또는 `q` )입니다.
- 부드러운 입방 형 3 차원 곡선 ( `S` 또는 `s` )입니다.
- 부드러운 정방형 3 차원 곡선 ( `T` 또는 `t` )입니다.
- 타원형 원호 ( `A` 또는 `a` )입니다.

각 그리기 명령은 대문자 또는 소문자를 사용 하 여 지정 됩니다. 동일한 유형의 명령을 두 번 이상 입력 하는 경우 중복 명령 항목을 생략할 수 있습니다. 예를 들면 `L 100,200 300,400` 와 같습니다 `L 100, 200 L 300,400` .

### <a name="line-command"></a>줄 명령

줄 명령은 현재 지점과 지정 된 끝점 사이에 직선을 만듭니다. 이 명령의 구문은 `L` *끝점* 또는 `l` *끝점*입니다.

이 구문에서 *끝점* 은 선의 끝점을 [`Point`](xref:Xamarin.Forms.Point) 나타내는입니다.

`L 20,30`및 `L 20 30` 은 올바른 줄 명령의 예입니다.

### <a name="horizontal-line-command"></a>가로선 명령

가로 선 명령은 현재 지점과 지정 된 x 좌표 사이에 가로선을 만듭니다. 이 명령의 구문은 `H` *x* 또는 `h` *x*입니다.

이 구문에서 *x* 는 `double` 선 끝점의 x 좌표를 나타내는입니다.

`H 90`은 유효한 수평선 명령의 예입니다.

### <a name="vertical-line-command"></a>세로줄 명령

세로 선 명령은 현재 지점과 지정 된 y 좌표 사이에 세로줄을 만듭니다. 이 명령의 구문은 `V` *y* 또는 `v` *y*입니다.

이 구문에서 *y* 는 `double` 선 끝점의 y 좌표를 나타내는입니다.

`V 90`은 유효한 수직선 명령의 예입니다.

### <a name="cubic-bezier-curve-command"></a>입방 형 3 차원 곡선 명령

입방 형 3 차원 곡선 명령은 지정 된 두 제어점을 사용 하 여 현재 지점과 지정 된 끝점 간에 입방 형 3 차원 곡선을 만듭니다. 이 명령의 구문은 다음과 같습니다. `C` *controlPoint1* *controlPoint2* *endpoint* 또는 `c` *controlPoint1* *controlPoint2* *endpoint*.

이 구문에서 다음을 수행 합니다.

- *controlPoint1* 는 곡선의 [`Point`](xref:Xamarin.Forms.Point) 시작 접선을 결정 하는 곡선의 첫 번째 제어점을 나타내는입니다.
- *controlPoint2* 는 곡선의 [`Point`](xref:Xamarin.Forms.Point) 끝 접선을 결정 하는 곡선의 두 번째 제어점을 나타내는입니다.
- *끝점* 은 [`Point`](xref:Xamarin.Forms.Point) 곡선이 그려지는 점을 나타내는입니다.

`C 100,200 200,400 300,200`는 유효한 입방 형 3 차원 곡선 명령의 예입니다.

### <a name="quadratic-bezier-curve-command"></a>정방형 3 차원 곡선 명령

정방형 3 차원 곡선 명령은 지정 된 제어점을 사용 하 여 현재 지점과 지정 된 끝점 사이에 정방형 3 차원 곡선을 만듭니다. 이 명령의 구문은: `Q` *controlPoint* *endpoint* 또는 `q` *controlPoint* *endpoint*입니다.

이 구문에서 다음을 수행 합니다.

- *controlPoint* 는 곡선의 [`Point`](xref:Xamarin.Forms.Point) 시작 접선 및 끝 접선을 결정 하는 곡선의 제어점을 나타내는입니다.
- *끝점* 은 [`Point`](xref:Xamarin.Forms.Point) 곡선이 그려지는 점을 나타내는입니다.

`Q 100,200 300,200`은 유효한 정방형 3차원 곡선 명령의 예입니다.

### <a name="smooth-cubic-bezier-curve-command"></a>부드러운 입방 형 3 차원 곡선 명령

부드러운 입방 형 3 차원 곡선 명령은 지정 된 제어점을 사용 하 여 현재 지점과 지정 된 끝점 간에 입방 형 3 차원 곡선을 만듭니다. 이 명령의 구문은: `S` *controlPoint2* *endpoint* 또는 `s` *controlPoint2* *endpoint*입니다.  

이 구문에서 다음을 수행 합니다.

- *controlPoint2* 는 곡선의 [`Point`](xref:Xamarin.Forms.Point) 끝 접선을 결정 하는 곡선의 두 번째 제어점을 나타내는입니다.
- *끝점* 은 [`Point`](xref:Xamarin.Forms.Point) 곡선이 그려지는 점을 나타내는입니다.

첫 번째 제어점은 현재 점을 기준으로 이전 명령의 두 번째 제어점에 대 한 리플렉션으로 간주 됩니다. 이전 명령이 없거나 이전 명령이 입방 형 3 차원 곡선 명령 또는 부드러운 입방 형 3 차원 곡선 명령이 아니면 첫 번째 제어점은 현재 지점과 일치 하는 것으로 간주 됩니다.

`S 100,200 200,300`는 유효한 부드러운 입방 형 3 차원 곡선 명령의 예입니다.

### <a name="smooth-quadratic-bezier-curve-command"></a>부드러운 정방형 3 차원 곡선 명령

부드러운 정방형 3 차원 곡선 명령은 제어점을 사용 하 여 현재 지점과 지정 된 끝점 사이에 정방형 3 차원 곡선을 만듭니다. 이 명령의 구문은 `T` *끝점* 또는 `t` *끝점*입니다.

이 구문에서 *끝점* 은 [`Point`](xref:Xamarin.Forms.Point) 곡선이 그려지는 점을 나타내는입니다.

제어점은 현재 점을 기준으로 이전 명령의 제어점의 리플렉션으로 간주됩니다. 이전 명령이 없거나 이전 명령이 정방형 3 차원 곡선 또는 부드러운 정방형 3 차원 곡선 명령이 아닌 경우 제어점은 현재 지점과 일치 하는 것으로 간주 됩니다.

`T 100,30`는 유효한 부드러운 정방형 3 차원 곡선 명령의 예입니다.

### <a name="elliptical-arc-command"></a>타원형 원호 명령

타원형 원호 명령은 현재 지점과 지정 된 끝점 사이에 타원형 호를 만듭니다. 이 명령의 구문은 `A` *size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endpoint* 또는 `a` *size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endpoint*입니다.

이 구문에서 다음을 수행 합니다.

- `size`[`Size`](xref:Xamarin.Forms.Size)원호의 x 및 y 반지름을 나타내는입니다.
- `rotationAngle`는 `double` 타원의 회전 각도 (도)를 나타내는입니다.
- `isLargeArcFlag`원호의 각도가 180도 이상이 면 1로 설정 하 고 그렇지 않으면 0으로 설정 해야 합니다.
- `sweepDirectionFlag`원호를 양의 각도 방향으로 그리면 1로 설정 하 고 그렇지 않으면 0으로 설정 해야 합니다.
- `endPoint`호를 [`Point`](xref:Xamarin.Forms.Point) 그릴입니다.

`A150,150 0 1,0 150,-150`는 유효한 타원형 호 명령의 예입니다.

## <a name="close-command"></a>닫기 명령

닫기 명령은 현재 그림을 종료 하 고 현재 점을 그림의 시작 지점에 연결 하는 선을 만듭니다. 따라서이 명령은 그림의 마지막 세그먼트와 첫 번째 세그먼트 사이에 선 조인을 만듭니다.

Close 명령의 구문은 `Z` 또는 `z` 입니다.

## <a name="additional-values"></a>추가 값

표준 숫자 값 대신 다음과 같은 대/소문자를 구분 하는 특수 값을 사용할 수도 있습니다.

- `Infinity``double.PositiveInfinity`를 나타냅니다.
- `-Infinity``double.NegativeInfinity`를 나타냅니다.
- `NaN``double.NaN`를 나타냅니다.

또한 대/소문자를 구분 하지 않는 과학적 표기법을 사용할 수도 있습니다. 따라서 `+1.e17` 는 유효한 값입니다.

## <a name="related-links"></a>관련 링크

- [ShapeDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms형상의](geometries.md)
