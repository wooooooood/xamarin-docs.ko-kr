---
title: SkiaSharp의 비트맵 기본 사항
description: 이 문서는 다양 한 원본에서 SkiaSharp 비트맵을 로드 하 고 Xamarin.Forms 응용 프로그램에서 표시 하는 방법을 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: f43779fd0a61bd3ad04f3f7445faa6517fb9c989
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645894"
---
# <a name="bitmap-basics-in-skiasharp"></a>SkiaSharp의 비트맵 기본 사항

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_다양 한 원본에서 비트맵을 로드 하 고 표시 합니다._

SkiaSharp 비트맵의 지원이 매우 광범위 하 게 됩니다. 이 문서에서는 기본 사항을 &mdash; 비트맵을 로드 하는 방법 및 표시 방법:

![](bitmaps-images/bitmapssample.png "두 비트맵의 표시")

비트맵의 많은 심층적 탐색을 섹션에서 찾을 수 있습니다 [SkiaSharp 비트맵](../bitmaps/index.md)합니다.

SkiaSharp 비트맵 형식의 개체인 [ `SKBitmap` ](xref:SkiaSharp.SKBitmap)합니다. 여러 가지 방법으로 비트맵을 만들 수 있지만이 문서에서는 자체를 제한 합니다 [ `SKBitmap.Decode` ](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream)) .NET에서 비트맵을 로드 하는 메서드를 `Stream` 개체입니다.

합니다 **기본 비트맵** 페이지에 **SkiaSharpFormsDemos** 프로그램에는 세 가지 소스에서 비트맵을 로드 하는 방법을 보여 줍니다.:

- 인터넷을 통한
- 실행 파일에 포함 된 리소스에서
- 사용자의 사진 라이브러리에서

세 `SKBitmap` 필드로 정의 된 다음 세 가지 원본에 대 한 개체를 [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) 클래스:

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

URL을 기반으로 하는 비트맵을 로드 하려면 사용 합니다 [ `HttpClient` ](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0) 클래스입니다. 하나의 인스턴스만 인스턴스화해야 `HttpClient` 재사용, 따라서 필드로 저장 합니다.

```csharp
HttpClient httpClient = new HttpClient();
```

사용 하는 경우 `HttpClient` iOS 및 Android 응용 프로그램에서 문서에 설명 된 대로 프로젝트 속성을 설정 하려는  **[전송 계층 보안 (TLS) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)** 합니다.

사용 하는 데 가장 편리한 이기 때문에 `await` 연산자 `HttpClient`, 코드를 실행할 수 없습니다는 `BasicBitmapsPage` 생성자입니다. 대신의 일부임을 `OnAppearing` 재정의 합니다. 여기에 URL 영역에 일부 샘플 비트맵을 사용 하 여 Xamarin 웹 사이트를 가리킵니다. 웹 사이트에서 패키지를 특정 너비로 비트맵 크기 조정에 대 한 사양을 추가 허용 합니다.


```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    // Load web bitmap.
    string url = "https://developer.xamarin.com/demo/IMG_3256.JPG?width=480";

    try
    {
        using (Stream stream = await httpClient.GetStreamAsync(url))
        using (MemoryStream memStream = new MemoryStream())
        {
            await stream.CopyToAsync(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            webBitmap = SKBitmap.Decode(stream);
            canvasView.InvalidateSurface();
        };
    }
    catch
    {
    }
}
```

Android 운영 체제를 사용할 때 예외가 발생 합니다 `Stream` 에서 반환 된 `GetStreamAsync` 에서 `SKBitmap.Decode` 메서드 주 스레드에서 시간이 많이 걸리는 작업을 수행 하기 때문에. 비트맵 파일의 내용을 복사할 따라서이 `MemoryStream` 개체를 사용 하 여 `CopyToAsync`입니다.

정적 `SKBitmap.Decode` 메서드는 비트맵 파일 디코딩 작업을 담당 합니다. 비트맵 형식으로 JPEG, PNG 및 GIF를 사용 하 여 작동 하 고 내부 SkiaSharp 형식으로 결과 저장 합니다. 이 시점에서 `SKCanvasView` 있도록 무효화 해야 하는 경우는 `PaintSurface` 처리기 디스플레이를 업데이트 합니다. 

## <a name="loading-a-bitmap-resource"></a>비트맵 리소스를 로드합니다.

코드 측면에서 비트맵을 로드 하는 가장 쉬운 방법은 비트맵 리소스를 포함 하 여 응용 프로그램에서 직접 됩니다. 합니다 **SkiaSharpFormsDemos** 라는 폴더를 포함 하는 프로그램 **Media** 라는 하나를 포함 하 여 파일, 비트맵 몇 개 포함 **monkey.png**합니다. 프로그램 리소스로 저장 하는 비트맵을 사용 해야 합니다 **속성** 파일에는 대화는 **빌드 작업** 의 **포함 리소스**!

각 포함 리소스에는 프로젝트 이름, 폴더 및 파일 이름으로 구성 된 *리소스 ID* 가 있으며, 모두 마침표로 연결 됩니다. **SkiaSharpFormsDemos.Media.monkey.png**. 해당 리소스를 지정 하 여이 리소스에 대 한 액세스를 얻을 수 있습니다 인수로 ID는 [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) 메서드를 [ `Assembly` ](xref:System.Reflection.Assembly) 클래스:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    resourceBitmap = SKBitmap.Decode(stream);
}
```

이렇게 `Stream` 개체에 직접 전달할 수는 `SKBitmap.Decode` 메서드.

## <a name="loading-a-bitmap-from-the-photo-library"></a>사진 라이브러리에서 비트맵을 로드합니다.

도 사용자 장치의 그림 라이브러리에서 사진을 로드 하는 것이 가능 합니다. 이 기능 자체는 Xamarin.Forms에서 제공 되지 않습니다. 작업을 사용 하려면 종속성 서비스를 문서에 설명 된 것과 같은 [사진 그림 라이브러리에서 선택](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)합니다.

**IPhotoLibrary.cs** 파일을 **SkiaSharpFormsDemos** 프로젝트 및 세 가지 **PhotoLibrary.cs** 플랫폼 프로젝트에서에서 파일에 문서에서 변형 되었습니다. 또한 Android **MainActivity.cs** 문서에 설명 된 대로 파일이 수정 되었는지 및 iOS 프로젝트에 두 줄의 아래쪽에 사진 라이브러리에 액세스할 수 있는 권한이 부여 된를 **info.plist**  파일입니다.

`BasicBitmapsPage` 추가 하는 생성자를 `TapGestureRecognizer` 에 `SKCanvasView` 탭의 알림을 받을. 탭에는 `Tapped` 처리기는 그림 선택 종속성 서비스 및 호출에 대 한 액세스를 가져옵니다 `PickPhotoAsync`합니다. 경우는 `Stream` 개체가 반환 되 면 다음에 전달 되는 `SKBitmap.Decode` 메서드:

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();

    using (Stream stream = await photoLibrary.PickPhotoAsync())
    {
        if (stream != null)
        {
            libraryBitmap = SKBitmap.Decode(stream);
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

있음을 `Tapped` 처리기도 호출 합니다 `InvalidateSurface` 메서드의 `SKCanvasView` 개체입니다. 이에 대 한 새 호출을 생성 합니다 `PaintSurface` 처리기입니다.

## <a name="displaying-the-bitmaps"></a>비트맵을 표시합니다.

`PaintSurface` 처리기가 세 가지 비트맵을 표시 해야 합니다. 처리기 휴대폰 세로 모드일 캔버스를 세 부분으로 세로로 분할 되었다고 가정 합니다.

첫 번째 비트맵 가장 간단한를 사용 하 여 표시 됩니다 [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint)) 메서드. 지정 하면 다음과 같습니다. X 및 Y 좌표를 비트맵의 왼쪽 위 모퉁이 위치 하 게 되는 위치

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

하지만 `SKPaint` 매개 변수를 정의 기본값은 `null` 및 무시할 수 있습니다. 일대일 매핑을 통해을 디스플레이 화면의 픽셀 비트맵의 픽셀 전송 하기만 하면 됩니다. 이 응용 프로그램을 볼 `SKPaint` 의 다음 섹션에서 인수 [ **SkiaSharp 투명도**](transparency.md)합니다.

프로그램을 사용 하 여 비트맵의 픽셀 크기를 가져올 수는 [ `Width` ](xref:SkiaSharp.SKBitmap.Width) 하 고 [ `Height` ](xref:SkiaSharp.SKBitmap.Height) 속성입니다. 이러한 속성에 비트맵 캔버스의 위 / 3의 가운데에 배치 하는 좌표를 계산 하려면 프로그램 허용:

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

다른 두 비트맵의 버전으로 표시 됩니다 [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint)) 사용 하 여는 `SKRect` 매개 변수:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

세 번째 버전의 [ `DrawBitmap` ](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) 에 두 개의 `SKRect` 표시 되지만 해당 버전에 비트맵의 사각형 하위 집합을 지정 하는 것에 대 한 인수는이 문서에서 사용 되지 않습니다.

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

이 스크린 샷에 monkey 가로로 늘어나는 이유는 사각형의 크기에 비트맵이 늘어납니다.

[![](bitmaps-images/basicbitmaps-small.png "비트맵 기본 페이지의 3 배가 스크린샷")](bitmaps-images/basicbitmaps-large.png#lightbox "삼중 기본 비트맵 페이지 스크린샷")

세 번째 이미지는 &mdash; 프로그램을 실행 하 고 고유한 그림 라이브러리에서 사진을 로드 하는 경우에 볼 수 있습니다 &mdash; 도 내에서 사각형은 사각형의 표시는 비트맵의 가로 세로 비율을 유지 하기 위해 위치 및 크기 조정 됩니다. 비트맵 및 대상 사각형의 크기를 기준으로 하는 비율 크기 조정 및 해당 영역에 있는 사각형 가운데 맞춤 계산 필요 하기 때문에이 계산 되는 좀 더 복잡:

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

비트맵이 없습니다 아직 그림 라이브러리에서 로드 된 경우 해당 `else` 블록 텍스트 화면을 누릅니다 하 라는 메시지를 표시 합니다.

다양 한 수준의 투명도 및에 대 한 다음 문서를 사용 하 여 비트맵을 표시할 수 있습니다 [ **SkiaSharp 투명도** ](transparency.md) 에 대해 설명 하는 방법입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [사진 그림 라이브러리에서 선택](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
