---
title: 비트맵 기본 사항
description: 다양 한 소스에서 비트맵을 로드 하 고 표시 합니다.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 04/03/2017
ms.openlocfilehash: b86068c2ed5063c25f76e81fdf477550b1437984
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="bitmap-basics"></a>비트맵 기본 사항

_다양 한 소스에서 비트맵을 로드 하 고 표시 합니다._

비트맵을 SkiaSharp 지원은 매우 광범위 하 게 합니다. 이 문서에서는 기본 사항을 &mdash; 비트맵을 로드 하는 방법 및 표시 하는 방법을:

![](bitmaps-images/bitmapssample.png "두 개의 비트맵의 표시")

SkiaSharp 비트맵은 형식의 개체로 [ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/)합니다. 여러 가지 방법으로 비트맵을 만들 수 있지만이 문서의 사용 하도록 제한 하는 [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/SkiaSharp.SKStream/) 에서 비트맵을 로드 하는 메서드는 [ `SKStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStream/) 비트맵 파일을 참조 하는 개체입니다. 사용 하는 것은 [ `SKManagedStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKManagedStream/) 에서 파생 된 클래스 `SKStream` .NET 허용 하는 생성자를 있기 때문에 [ `Stream` ](https://developer.xamarin.com/api/type/System.IO.Stream/) 개체입니다.

**기본 비트맵** 페이지에 **SkiaSharpFormsDemos** 프로그램에서는 서로 다른 세 원본의 비트맵을 로드 하는 방법을 보여 줍니다.

- 인터넷을 통한
- 실행 파일에 포함 된 리소스에서
- 사용자의 사진 라이브러리에서

세 가지 `SKBitmap` 에 필드로 정의 된 다음 세 가지 원본에 대 한 개체는 [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) 클래스:

```csharp
public class BasicBitmapsPage : ContentPage
{
    SKCanvasView canvasView;
    SKBitmap webBitmap;
    SKBitmap resourceBitmap;
    SKBitmap libraryBitmap;

    public BasicBitmapsPage()
    {
        Title = "Basic Bitmaps";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ...
    }
    ...
}
```

## <a name="loading-a-bitmap-from-the-web"></a>웹에서 비트맵을 로드합니다.

URL을 기반으로 하는 비트맵을 로드 하려면 사용할 수 있습니다는 [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) 에 실행 한 다음 코드에 나와 있는 것 처럼이 클래스는 `BasicBitmapsPage` 생성자입니다. 여기에서 URL 영역 몇 가지 샘플 비트맵 Xamarin 웹 사이트에 있는를 가리킵니다. 비트맵을 특정 너비를 크기 조정에 대 한 사양을 추가할 수 있도록 하는 웹 사이트에서 패키지:

```csharp
Uri uri = new Uri("http://developer.xamarin.com/demo/IMG_3256.JPG?width=480");
WebRequest request = WebRequest.Create(uri);
request.BeginGetResponse((IAsyncResult arg) =>
{
    try
    {
        using (Stream stream = request.EndGetResponse(arg).GetResponseStream())
        using (MemoryStream memStream = new MemoryStream())
        {
            stream.CopyTo(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            using (SKManagedStream skStream = new SKManagedStream(memStream))
            {
                webBitmap = SKBitmap.Decode(skStream);
            }
        }
    }
    catch
    {
    }

    Device.BeginInvokeOnMainThread(() => canvasView.InvalidateSurface());

}, null);
```

비트맵을 성공적으로 다운로드 되 면 콜백 메서드가에 전달 된 `BeginGetResponse` 메서드를 실행 합니다. `EndGetResponse` 호출에 있어야는 `try` 차단 경우 오류가 발생 했습니다. `Stream` 에서 가져온 개체 `GetResponseStream` 아니므로 일부 플랫폼에서는 적절 한 비트맵 내용이에 복사 되는 `MemoryStream` 개체입니다. 이 시점에서 `SKManagedStream` 개체를 만들 수 있습니다. 이 구문은 이제 JPEG 또는 PNG 파일 일 수도 있는 비트맵 파일을 참조 합니다. `SKBitmap.Decode` 메서드 비트맵 파일 디코딩하고 내부 SkiaSharp 형식으로 결과 저장 합니다.

에 전달 된 콜백 메서드 `BeginGetResponse` 않음을 의미 하는 생성자가 실행을 완료 한 후 실행은 `SKCanvasView` 있도록 무효화 될 필요는 `PaintSurface` 처리기 디스플레이를 업데이트 합니다. 그러나는 `BeginGetResponse` 사용 해야 하는 콜백 보조 스레드에서 실행은 실행 `Device.BeginInvokeOnMainThread` 실행 하 여 `InvalidateSurface` 메서드는 사용자 인터페이스 스레드에서 합니다.

## <a name="loading-a-bitmap-resource"></a>비트맵 리소스를 로드합니다.

코드에서는 비트맵을 로드 하는 가장 쉬운 방법은 비트맵 리소스를 포함 하 여 응용 프로그램에 직접 됩니다. **SkiaSharpFormsDemos** 라는 폴더를 포함 하는 프로그램 **미디어** 파일이 들어 있는 비트맵 라는 **monkey.png**합니다. 에 **속성** 대화가이 파일에 대 한 이러한 파일을 지정 해야 합니다는 **빌드 작업** 의 **포함 리소스**!

포함 된 각 리소스에는 *리소스 ID* 프로젝트 이름, 폴더 및 기간을 하 여 연결 된 모든 파일 이름에는 구성 된: **SkiaSharpFormsDemos.Media.monkey.png**합니다. 해당 리소스를 지정 하 여이 리소스에 대 한 액세스를 얻을 수에 대 한 인수로 ID는 [ `GetManifestResourceStream` ](https://developer.xamarin.com/api/member/System.Reflection.Assembly.GetManifestResourceStream/p/System.String/) 의 메서드는 [ `Assembly` ](https://developer.xamarin.com/api/type/System.Reflection.Assembly/) 클래스:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
using (SKManagedStream skStream = new SKManagedStream(stream))
{
    resourceBitmap = SKBitmap.Decode(skStream);
}
```

이 `Stream` 개체로 바로 변환 될 수는 `SKManagedStream` 개체입니다.

## <a name="loading-a-bitmap-from-the-photo-library"></a>사진 라이브러리에서 비트맵을 로드합니다.

사용자 사진 장치의 그림 라이브러리에서 로드할 수 이기도 합니다. 이 기능은 자체 Xamarin.Forms에 제공한 것입니다. 작업에 필요한 문서에 설명 된 것과 같은 종속성 서비스 [사진 그림 라이브러리에서 선택](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)합니다.

**IPicturePicker.cs** 파일과 세 개의 **PicturePickerImplementation.cs** 의 다양 한 프로젝트에 복사 된 파일에서 해당 문서는 **SkiaSharpFormsDemos**솔루션, 새 네임 스페이스 이름을 지정 합니다. 또한 Android **MainActivity.cs** 는 문서에 설명 된 대로 파일은 수정 하 고 iOS 프로젝트에는 사진 라이브러리의 아래쪽 두 줄으로 액세스할 수 있는 권한이 부여 된는 **info.plist**  파일입니다.

`BasicBitmapsPage` 추가 하는 생성자는 `TapGestureRecognizer` 에 `SKCanvasView` 탭의 알림을 받을 수 있습니다. 탭을 수신 시는 `Tapped` 처리기가 호출 그림 선택 종속성 서비스를 이용할 `GetImageStreamAsync`합니다. 경우는 `Stream` 개체가 반환 되 면 다음에 내용이 복사 되는 `MemoryStream`플랫폼의 일부 여 필요에 따라 합니다. 나머지 코드는 두 개의 다른 기술 하는 것과 비슷합니다.

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPicturePicker picturePicker = DependencyService.Get<IPicturePicker>();

    using (Stream stream = await picturePicker.GetImageStreamAsync())
    {
        if (stream != null)
        {
            using (MemoryStream memStream = new MemoryStream())
            {
                stream.CopyTo(memStream);
                memStream.Seek(0, SeekOrigin.Begin);

                using (SKManagedStream skStream = new SKManagedStream(memStream))
                {
                    libraryBitmap = SKBitmap.Decode(skStream);
                }
            }
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

다음에 유의 `Tapped` 처리기 호출의 `InvalidateSurface` 의 메서드는 `SKCanvasView` 개체입니다. 이 코드 생성에 대 한 새 호출은 `PaintSurface` 처리기입니다.

## <a name="displaying-the-bitmaps"></a>비트맵을 표시합니다.

`PaintSurface` 처리기를 세 비트맵을 표시 해야 합니다. 처리기 휴대폰 세로 모드일 세 개의 균등 한 부분으로 캔버스를 세로로 분할 되었다고 가정 합니다.

첫 번째 비트맵은 가장 간단한와 표시 [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) 메서드. 지정 하면 모든은 X 및 Y 좌표 바이트인 비트맵의 왼쪽 위 모퉁이를 배치할:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

하지만 `SKPaint` 매개 변수를 정의 기본 값에 `null` 및 무시할 수 있습니다. 비트맵의 픽셀의 일대일 매핑 사용 하 여 표시 화면 픽셀에 전송 하기만 하면 됩니다.

프로그램에서 사용 하 여 비트맵의 픽셀 크기를 가져올 수는 [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) 및 [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/) 속성입니다. 이러한 속성 비트맵 캔버스의 위 / 3의 가운데에 배치 하는 좌표를 계산 하는 프로그램을 사용 합니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    if (webBitmap != null)
    {
        float x = (info.Width - webBitmap.Width) / 2;
        float y = (info.Height / 3 - webBitmap.Height) / 2;
        canvas.DrawBitmap(webBitmap, x, y);
    }
    ...
}
```

다른 두 개의 비트맵의 버전으로 표시 됩니다 [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) 와 `SKRect` 매개 변수:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

세 번째 버전의 [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) 에 두 개의 `SKRect` 비트맵을 디스플레이 있지만 해당 버전의 사각형 하위 집합을 지정 하기 위한 인수는이 문서에서 사용 되지 않습니다.

포함된 리소스 비트맵에서 로드 하는 비트맵을 표시 하는 코드는 다음과 같습니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (resourceBitmap != null)
    {
        canvas.DrawBitmap(resourceBitmap,
            new SKRect(0, info.Height / 3, info.Width, 2 * info.Height / 3));
    }
    ...
}
```

이 때문에이 스크린 샷에 원숭이 확장 된 가로로 사각형의 크기에 비트맵이 늘어납니다.

[![](bitmaps-images/basicbitmaps-small.png "기본 비트맵 페이지의 삼중 스크린샷")](bitmaps-images/basicbitmaps-large.png#lightbox "기본 비트맵 페이지의 삼중 스크린샷")

세 번째 이미지 &mdash; 프로그램을 실행 하 고 사용자 고유의 그림 라이브러리에서 사진을 로드 하는 경우에 볼 수 있습니다 &mdash; 사각형 않고 사각형의 안에 표시 됩니다는 비트맵의 가로 세로 비율을 유지 하기 위해 위치 및 크기가 조정 됩니다. 이 계산이 자릿수 비트맵 및 대상 사각형의 크기에 따라 요소 및 해당 영역에 있는 사각형 가운데 맞추기 계산 필요 하기 때문에 약간 더 개입 됩니다.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (libraryBitmap != null)
    {
        float scale = Math.Min((float)info.Width / libraryBitmap.Width,
                               info.Height / 3f / libraryBitmap.Height);

        float left = (info.Width - scale * libraryBitmap.Width) / 2;
        float top = (info.Height / 3 - scale * libraryBitmap.Height) / 2;
        float right = left + scale * libraryBitmap.Width;
        float bottom = top + scale * libraryBitmap.Height;
        SKRect rect = new SKRect(left, top, right, bottom);
        rect.Offset(0, 2 * info.Height / 3);

        canvas.DrawBitmap(libraryBitmap, rect);
    }
    else
    {
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Blue;
            paint.TextAlign = SKTextAlign.Center;
            paint.TextSize = 48;

            canvas.DrawText("Tap to load bitmap",
                info.Width / 2, 5 * info.Height / 6, paint);
        }
    }
}
```

비트맵이 없습니다 그림 라이브러리에서 아직 로드 하는 경우 다음의 `else` 블록 사용자 화면에 게 묻는 텍스트를 표시 합니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [사진 그림 라이브러리에서 선택](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
