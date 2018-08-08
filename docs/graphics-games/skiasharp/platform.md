---
title: SkiaSharp 플랫폼 관련 메모
description: 이 문서에서는 SkiaSharp 관련 된 플랫폼 관련 세부 정보를 설명 합니다. IOS, Android, macOS, Windows, 및 Xamarin.Forms에 대 한 샘플 코드를 제공 합니다.
ms.prod: xamarin
ms.techonology: xamarin-skiasharp
ms.assetid: 1D90E0B3-A3A8-4286-BC54-9D67188A1C6C
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 05c6ae6553a2e869b9eb7e038abd7b1c34350551
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615810"
---
# <a name="skiasharp-platform-specific-notes"></a>SkiaSharp 플랫폼 관련 메모

아래 예제 이미지 버퍼를 수동으로 할당, 플랫폼에서 제공 하는 기존 RBGA 버퍼에 그리기 하는 일반적인 플랫폼 패턴을 설명 하기 위해 이렇게 합니다.

원하지 않는 경우이 관용구를 사용할 필요가 없습니다.  오버 로드를 만들고 이미지에 대 한 백업 저장소를 관리 하는 경우

## <a name="ios"></a>iOS

```csharp
var screenScale = UIScreen.MainScreen.Scale;
var width = (int)(Bounds.Width * screenScale);
var height = (int)(Bounds.Height * screenScale);

IntPtr buff = System.Runtime.InteropServices.Marshal.AllocCoTaskMem (width * height * 4);
try {
  using (var surface = SKSurface.Create (width, height, SKImageInfo.PlatformColorType, SKAlphaType.Premul, buff, width * 4)) {
    var skcanvas = surface.Canvas;
    skcanvas.Scale ((float)screenScale, (float)screenScale);
    using (new SKAutoCanvasRestore (skcanvas, true)) {
      // DoDraw (skcanvas);
    }
  }
  using (var colorSpace = CGColorSpace.CreateDeviceRGB ())
  using (var bContext = new CGBitmapContext (buff, width, height, 8, width * 4, colorSpace, (CGImageAlphaInfo)bitmapInfo))
  using (var image = bContext.ToImage ())
  using (var context = UIGraphics.GetCurrentContext ()) {
    // flip the image for CGContext.DrawImage
    context.TranslateCTM (0, Frame.Height);
    context.ScaleCTM (1, -1);
    context.DrawImage (Bounds, image);
  }
} finally {
  if (buff != IntPtr.Zero) {
    System.Runtime.InteropServices.Marshal.FreeCoTaskMem (buff);
  }
}
```

## <a name="android"></a>Android

```csharp
var width = (float)skiaView.Width;
var height = (float)skiaView.Height;

using (var bitmap = Bitmap.CreateBitmap (canvas.Width, canvas.Height, Bitmap.Config.Argb8888)) {
  try {
    using (var surface = SKSurface.Create (canvas.Width, canvas.Height, SKColorType.Rgba_8888, SKAlphaType.Premul, bitmap.LockPixels (), canvas.Width * 4)) {
      var skcanvas = surface.Canvas;
      skcanvas.Scale (((float)canvas.Width)/width, ((float)canvas.Height)/height);
      // DoDraw (skcanvas);
    }
  } finally {
    bitmap.UnlockPixels ();
  }
  canvas.DrawBitmap (bitmap, 0, 0, null);
}
```

## <a name="macos"></a>macOS

```csharp
var screenScale = (int)NSScreen.MainScreen.BackingScaleFactor * 2;
var width = (int)Bounds.Width * screenScale;
var height = (int)Bounds.Height * screenScale;

IntPtr buff = System.Runtime.InteropServices.Marshal.AllocCoTaskMem (width * height * 4);
try {
  using (var surface = SKSurface.Create (width, height, SKColorType.Rgba_8888, SKAlphaType.Premul, buff, width * 4)) {
    var skcanvas = surface.Canvas;
    skcanvas.Scale (screenScale, screenScale);
    // DoDraw (skcanvas);
  }
  int flag = ((int)CoreGraphics.CGBitmapFlags.ByteOrderDefault) | ((int)CoreGraphics.CGImageAlphaInfo.PremultipliedLast);
  using (var colorSpace = CoreGraphics.CGColorSpace.CreateDeviceRGB ())
  using (var bContext = new CoreGraphics.CGBitmapContext (buff, width, height, 8, width * 4, colorSpace, (CoreGraphics.CGImageAlphaInfo) flag))
  using (var image = bContext.ToImage ())
  using (var context = NSGraphicsContext.CurrentContext.GraphicsPort) {
    context.DrawImage (Bounds, image);
  }
} finally {
  if (buff != IntPtr.Zero) {
    System.Runtime.InteropServices.Marshal.FreeCoTaskMem (buff);
  }
}
```

## <a name="windows-desktop--mac-desktop"></a>Windows 데스크톱 / Mac 데스크톱

```csharp
var width = Width;
var height = Height;

using (var bitmap = new Bitmap(width, height, PixelFormat.Format32bppPArgb)) {
  var data = bitmap.LockBits(new Rectangle(0, 0, width, height), ImageLockMode.WriteOnly, bitmap.PixelFormat);
  using (var surface = SKSurface.Create(width, height, SKImageInfo.PlatformColorType, SKAlphaType.Premul, data.Scan0, width * 4)) {
    var skcanvas = surface.Canvas;
    // DoDraw (skcanvas);
  }
  bitmap.UnlockBits(data);
  e.Graphics.DrawImage(bitmap, new Rectangle(0, 0, Width, Height));
}
```

## <a name="xamarinforms"></a>Xamarin.Forms

응용 프로그램에 Xamarin.Forms에서 SkiaSharp 포함할 가이드를 참조 하십시오 [Xamarin.Forms에서 SkiaSharp 사용 하 여](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp iOS 통합 문서](https://developer.xamarin.com/workbooks/graphics/skiasharp/logo/skialogo-ios.workbook)
