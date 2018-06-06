---
title: SkiaSharp 소개
description: 이 문서에서는 핵심 SkiaSharp 개념에 대 한 간략 한 소개를 제공 합니다. 특히, 가져오기 및는 SKCanvas에 그리기 설명 합니다.
ms.prod: xamarin
ms.assetid: 19506F08-2603-465E-A806-6BD01638DE90
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: a42836a49560a73b9e35ef97bfb2ba83d15812e3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783063"
---
# <a name="an-introduction-to-skiasharp"></a>SkiaSharp 소개

_이렇게 하면 SkiaSharp 개념에 대 한 간략 한 소개_

SkiaSharp 풍부 하 고 강력한 2D 그래픽은 2D 버퍼에 렌더링 하는 데 사용할 수 있는 API를 제공 합니다.  사용자 지정 사용자 인터페이스 요소와 응용 프로그램에 통합할 수 있는 2 차원 그래픽을 구현 하려면이 사용할 수 있습니다.  SkiaSharp는에 대 한.NET 바인딩을 [Skia](https://skia.org) 라이브러리 기능 및이 라이브러리의 상속 합니다.

라이브러리는 현재 플랫폼으로 사용할 수 있는 [NuGet 패키지](https://www.nuget.org/packages/SkiaSharp), NuGet 참조를 추가 하 여 프로젝트에 추가할 수 있습니다.

그리기, 사용자 코드를 만듭니다는 `SkCanvas` 그리기 작업이 수행 됩니다 있는 화면을 설명 하는 합니다.

## <a name="obtaining-an-skcanvas"></a>SKCanvas 가져오기

```csharp
using (var surface = SKSurface.Create (width: 640, height: 480, SKImageInfo.PlatformColorType, SKAlphaType.Premul)) {
    SKCanvas myCanvas = surface.Canvas;

    // Your drawing code goes here.
}
```

## <a name="drawing-on-skcanvas"></a>SKCanvas에 그리기

`SKCanvas` 사용 하 여 다른 드로잉 개념상에서와 비슷한 그리기 모델 모델으로 저장할 수 있는지, 선택적 투명도 채널 색을 사용 하 고 선, 타원, 텍스트 및 이미지를 그릴 수 있습니다.

다음은 몇 가지 SkiaSharp로 수행할 수 있는 다른 여러 가지입니다.  변수 아래 예제에서 `canvas` SKCanvas 유형입니다.

### <a name="drawing-xamagon"></a>Xamagon 그리기

이 예제에서는 Xamarin의 로고는 Xamagon 그립니다.

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

### <a name="drawing-with-image-filters"></a>이미지 필터:를 사용 하 여 그리기

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

SkiaSharp 사용에 대 한 자세한 내용은에서 확인할 수 있습니다는 [API 설명서를 온라인](https://developer.xamarin.com/api/namespace/SkiaSharp/)


## <a name="related-links"></a>관련 링크

- [SkiaSharp iOS 통합 문서](https://developer.xamarin.com/workbooks/graphics/skiasharp/logo/skialogo-ios.workbook)
