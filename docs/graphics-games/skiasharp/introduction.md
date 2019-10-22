---
title: SkiaSharp 플랫폼 독립적인 예
description: 이 문서에서는 핵심 SkiaSharp 개념을 간략하게 소개 합니다. 특히, 고 대 중 캔버스를 가져오고 그리는 방법을 설명 합니다.
ms.prod: xamarin
ms.techonology: xamarin-skiasharp
ms.assetid: 19506F08-2603-465E-A806-6BD01638DE90
author: davidbritch
ms.author: dabritch
ms.date: 10/03/2018
ms.openlocfilehash: 4d0e57b98a479112b9fdf4f9c503418f3966cc73
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "64749925"
---
# <a name="skiasharp-platform-independent-examples"></a>SkiaSharp 플랫폼 독립적인 예

_이는 SkiaSharp의 개념에 대 한 간단한 플랫폼 독립적인 소개를 제공 합니다._

SkiaSharp는 2D 버퍼로 렌더링 하는 데 사용할 수 있는 풍부 하 고 강력한 2D 그래픽 API를 제공 합니다.  이러한 요소를 사용 하 여 응용 프로그램에 통합할 수 있는 사용자 지정 사용자 인터페이스 요소 및 2D 그래픽을 구현할 수 있습니다. SkiaSharp는 기능 및 [해당 라이브러리의](https://skia.org) 기능을 상속 하는 .NET의 .net 바인딩입니다.

라이브러리는 현재 플랫폼 간 [Nuget 패키지로](https://www.nuget.org/packages/SkiaSharp)사용할 수 있으며, nuget 참조를 추가 하 여 프로젝트에 추가할 수 있습니다.

그리기를 위해 코드에서 그리기 작업을 수행할 화면을 설명 하는 `SkCanvas`을 만듭니다.

## <a name="obtaining-an-skcanvas"></a>가 나 캔버스 가져오기

```csharp
using (var surface = SKSurface.Create (width: 640, height: 480, SKImageInfo.PlatformColorType, SKAlphaType.Premul)) {
    SKCanvas myCanvas = surface.Canvas;

    // Your drawing code goes here.
}
```

## <a name="drawing-on-skcanvas"></a>가 나 캔버스에서 그리기

@No__t_0는 사용자가 잘 알고 있는 다른 그리기 모델에 대 한 스피릿와 비슷한 그리기 모델을 사용 하며, 선택적 투명도 채널에서 색을 사용 하 고 선, 원호, 텍스트 및 이미지를 그릴 수 있습니다.

다음은 SkiaSharp를 사용 하 여 수행할 수 있는 몇 가지 다양 한 작업입니다.  아래 예제에서 변수 `canvas`은 (는) 서 형식 Canvas입니다.

### <a name="drawing-xamagon"></a>Xamagon 그리기

이 예제에서는 Xamagon에 Xamarin 로고를 그립니다.

```csharp
// clear the canvas / fill with white
canvas.Clear (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x2c, 0x3e, 0x50);
  paint.StrokeCap = SKStrokeCap.Round;

  // create the Xamagon path
  using (var path = new SKPath ()) {
    path.MoveTo (71.4311121f, 56f);
    path.CubicTo (68.6763107f, 56.0058575f, 65.9796704f, 57.5737917f, 64.5928855f, 59.965729f);
    path.LineTo (43.0238921f, 97.5342563f);
    path.CubicTo (41.6587026f, 99.9325978f, 41.6587026f, 103.067402f, 43.0238921f, 105.465744f);
    path.LineTo (64.5928855f, 143.034271f);
    path.CubicTo (65.9798162f, 145.426228f, 68.6763107f, 146.994582f, 71.4311121f, 147f);
    path.LineTo (114.568946f, 147f);
    path.CubicTo (117.323748f, 146.994143f, 120.020241f, 145.426228f, 121.407172f, 143.034271f);
    path.LineTo (142.976161f, 105.465744f);
    path.CubicTo (144.34135f, 103.067402f, 144.341209f, 99.9325978f, 142.976161f, 97.5342563f);
    path.LineTo (121.407172f, 59.965729f);
    path.CubicTo (120.020241f, 57.5737917f, 117.323748f, 56.0054182f, 114.568946f, 56f);
    path.LineTo (71.4311121f, 56f);
    path.Close ();

    // draw the Xamagon path
    canvas.DrawPath (path, paint);
  }
}
```

### <a name="drawing-text"></a>텍스트 그리기

```csharp
// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.TextSize = 64.0f;
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x42, 0x81, 0xA4);
  paint.IsStroke = false;

  // draw the text
  canvas.DrawText ("Skia", 0.0f, 64.0f, paint);
}
```

### <a name="drawing-bitmaps"></a>비트맵 그리기

```csharp
Stream fileStream = File.OpenRead ("MyImage.png");

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  canvas.DrawBitmap(bitmap, SKRect.Create(Width, Height), paint);
}
```

### <a name="drawing-with-image-filters"></a>이미지 필터로 그리기

```csharp
Stream fileStream = File.OpenRead ("MyImage.png"); // open a stream to an image file

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  // create the image filter
  using (var filter = SKImageFilter.CreateBlur(5, 5)) {
    paint.ImageFilter = filter;

    // draw the bitmap through the filter
    canvas.DrawBitmap(bitmap, SKRect.Create(width, height), paint);
  }
}
```

## <a name="more-information"></a>추가 정보

SkiaSharp 사용에 대 한 자세한 내용은 [API 설명서](https://docs.microsoft.com/dotnet/api/skiasharp) 에서 찾을 수 있습니다.
