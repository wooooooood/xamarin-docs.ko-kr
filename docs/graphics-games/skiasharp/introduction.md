---
title: SkiaSharp 플랫폼 독립적인 예제
description: 이 문서에서는 핵심 SkiaSharp 개념에 대 한 간략 한 소개를 제공 합니다. 특히, 가져오기 및 SKCanvas를 토대로 설명 합니다.
ms.prod: xamarin
ms.techonology: xamarin-skiasharp
ms.assetid: 19506F08-2603-465E-A806-6BD01638DE90
author: davidbritch
ms.author: dabritch
ms.date: 10/03/2018
ms.openlocfilehash: 4d0e57b98a479112b9fdf4f9c503418f3966cc73
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61158891"
---
# <a name="skiasharp-platform-independent-examples"></a>SkiaSharp 플랫폼 독립적인 예제

_SkiaSharp 개념에 플랫폼 독립적인 간략하게 제공_

SkiaSharp 풍부 하 고 강력한 2D 그래픽 2D 버퍼로 렌더링에 사용할 수 있는 API를 제공 합니다.  사용자 지정 사용자 인터페이스 요소 및 응용 프로그램에 통합할 수 있는 2 차원 그래픽을 구현 하려면 사용할 수 있습니다. SkiaSharp 바인딩됩니다.NET 합니다 [Skia](https://skia.org) 라이브러리 기능 및이 라이브러리의 기능을 상속 합니다.

플랫폼 간 라이브러리 현재 제품은 [NuGet 패키지](https://www.nuget.org/packages/SkiaSharp), NuGet 참조를 추가 하 여 프로젝트에 추가할 수 있습니다.

그릴 코드 만들어집니다는 `SkCanvas` 그리기 작업이 수행 되는 화면을 설명 하는 합니다.

## <a name="obtaining-an-skcanvas"></a>SKCanvas 가져오기

```csharp
using (var surface = SKSurface.Create (width: 640, height: 480, SKImageInfo.PlatformColorType, SKAlphaType.Premul)) {
    SKCanvas myCanvas = surface.Canvas;

    // Your drawing code goes here.
}
```

## <a name="drawing-on-skcanvas"></a>SKCanvas에 그리기

`SKCanvas` 알고 있을 수는 사용 하 여 비슷하며 다른 그리기 그리기 모델을 모델, 선택적 투명도 채널을 사용 하 여 색을 사용 하며 선, 타원, 텍스트 및 이미지를 그릴 수 있습니다.

SkiaSharp를 사용 하 여 수행할 수 있는 여러 다른 기능 중 일부에 지나지가 같습니다.  변수 아래 예제에서는 `canvas` SKCanvas 형식입니다.

### <a name="drawing-xamagon"></a>Xamagon 그리기

이 예제에서는 Xamarin의 로고를 Xamagon를 그립니다.

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

### <a name="drawing-with-image-filters"></a>이미지 필터를 사용 하 여 그리기

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

SkiaSharp 사용에 대 한 자세한 정보를 찾을 수 있습니다는 [API 설명서](https://docs.microsoft.com/dotnet/api/skiasharp)
