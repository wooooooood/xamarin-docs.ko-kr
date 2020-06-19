---
title: SkiaSharp의 비트맵 기본 사항
description: 이 문서에서는 다양 한 소스에서 SkiaSharp 비트맵을 로드 하 여 응용 프로그램에 표시 하 Xamarin.Forms 고 샘플 코드를 사용 하 여이를 보여 주는 방법을 설명 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1e4c170f818dc62640b1cd72ec3b70f48d227d93
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137738"
---
# <a name="bitmap-basics-in-skiasharp"></a>SkiaSharp의 비트맵 기본 사항

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_다양 한 소스에서 비트맵을 로드 하 고 표시 합니다._

SkiaSharp에서 비트맵을 지 원하는 것은 매우 광범위 합니다. 이 문서에서는 비트맵을 로드 하 고 표시 하는 방법에 대 한 기본 사항만 다룹니다 &mdash; .

![](bitmaps-images/basicbitmaps-small.png "The display of two bitmaps")

비트맵에 대 한 훨씬 더 심층적 탐색은 [SkiaSharp 비트맵](../bitmaps/index.md)섹션에서 찾을 수 있습니다.

SkiaSharp 비트맵은 형식의 개체입니다 [`SKBitmap`](xref:SkiaSharp.SKBitmap) . 비트맵을 만드는 방법에는 여러 가지가 있지만이 문서는 [`SKBitmap.Decode`](xref:SkiaSharp.SKBitmap.Decode(System.IO.Stream)) .net 개체에서 비트맵을 로드 하는 메서드로 자신을 제한 `Stream` 합니다.

**SkiaSharpFormsDemos** 프로그램의 **기본 비트맵** 페이지는 세 가지 다른 원본에서 비트맵을 로드 하는 방법을 보여 줍니다.

- 인터넷을 통해
- 실행 파일에 포함 된 리소스에서
- 사용자의 사진 라이브러리에서

`SKBitmap`이러한 세 개의 소스에 대 한 세 개체는 클래스의 필드로 정의 됩니다 [`BasicBitmapsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) .

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

## <a name="loading-a-bitmap-from-the-web"></a>웹에서 비트맵 로드

URL을 기반으로 비트맵을 로드 하려면 클래스를 사용할 수 있습니다 [`HttpClient`](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0) . 인스턴스를 하나만 인스턴스화하고 `HttpClient` 다시 사용 하 여 필드로 저장 해야 합니다.

```csharp
HttpClient httpClient = new HttpClient();
```

`HttpClient`IOS 및 Android 응용 프로그램에서을 사용 하는 경우 **[TLS (전송 계층 보안) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)** 문서에 설명 된 대로 프로젝트 속성을 설정 하는 것이 좋습니다.

연산자를와 함께 사용 하는 것이 가장 편리 하기 때문에 `await` `HttpClient` 생성자에서 코드를 실행할 수 없습니다 `BasicBitmapsPage` . 대신 재정의의 일부입니다 `OnAppearing` . 여기에서 URL은 몇 가지 샘플 비트맵이 있는 Xamarin 웹 사이트의 영역을 가리킵니다. 웹 사이트의 패키지를 통해 특정 너비로 비트맵 크기를 조정 하는 사양을 추가할 수 있습니다.

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

            webBitmap = SKBitmap.Decode(memStream);
            canvasView.InvalidateSurface();
        };
    }
    catch
    {
    }
}
```

Android 운영 체제는 `Stream` `GetStreamAsync` `SKBitmap.Decode` 주 스레드에서 긴 작업을 수행 하기 때문에 메서드에서 반환 된를 사용 하는 경우 예외를 발생 시킵니다. 이러한 이유로 비트맵 파일의 내용은를 `MemoryStream` 사용 하 여 개체로 복사 됩니다 `CopyToAsync` .

정적 `SKBitmap.Decode` 메서드는 비트맵 파일을 디코딩하는 역할을 담당 합니다. JPEG, PNG 및 GIF 비트맵 형식에서 작동 하 고 결과를 내부 SkiaSharp 형식으로 저장 합니다. 이 시점에서 `SKCanvasView` 처리기가 표시를 업데이트할 수 있도록를 무효화 해야 합니다 `PaintSurface` .

## <a name="loading-a-bitmap-resource"></a>비트맵 리소스 로드

코드를 기준으로 비트맵을 로드 하는 가장 쉬운 방법은 응용 프로그램에 직접 비트맵 리소스를 포함 하는 것입니다. **SkiaSharpFormsDemos** 프로그램에는 명명 된 **monkey.png**를 포함 하 여 여러 비트맵 파일을 포함 하는 **Media** 라는 폴더가 포함 됩니다. 프로그램 리소스로 저장 된 비트맵의 경우 **속성** 대화 상자를 사용 하 여 **포함 리소스**의 **빌드 작업** 을 파일에 지정 해야 합니다.

포함 된 각 리소스에는 프로젝트 이름, 폴더 및 파일 이름으로 구성 되는 *리소스 ID* 가 있습니다. 여기에는 모두 마침표로 연결 됨: **SkiaSharpFormsDemos.Media.monkey.png**. 클래스의 메서드에 대 한 인수로 리소스 ID를 지정 하 여이 리소스에 대 한 액세스 권한을 얻을 수 있습니다 [`GetManifestResourceStream`](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) [`Assembly`](xref:System.Reflection.Assembly) .

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
{
    resourceBitmap = SKBitmap.Decode(stream);
}
```

이 `Stream` 개체는 메서드에 직접 전달 될 수 있습니다 `SKBitmap.Decode` .

## <a name="loading-a-bitmap-from-the-photo-library"></a>사진 라이브러리에서 비트맵 로드

사용자가 장치의 그림 라이브러리에서 사진을 로드할 수도 있습니다. 이 기능은 단독으로 제공 되지 않습니다 Xamarin.Forms . 작업에는 [그림 라이브러리에서 사진 선택](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)문서에 설명 된 것과 같은 종속성 서비스가 필요 합니다.

**SkiaSharpFormsDemos** 프로젝트의 **IPhotoLibrary.cs** 파일과 플랫폼 프로젝트의 3 개 **PhotoLibrary.cs** 파일이 해당 문서에서 수정 되었습니다. 또한 문서에 설명 된 대로 Android **MainActivity.cs** 파일이 수정 되었으며, **info.plist** 파일의 아래쪽에 두 줄이 있는 사진 라이브러리에 액세스할 수 있는 권한이 iOS 프로젝트에 부여 되었습니다.

생성자는 탭에 대 한 알림이 표시 되도록에를 `BasicBitmapsPage` 추가 합니다 `TapGestureRecognizer` `SKCanvasView` . 탭이 수신 되 면 처리기는 `Tapped` 그림 선택 종속성 서비스에 대 한 액세스 권한을 가져오고를 호출 `PickPhotoAsync` 합니다. `Stream`반환 된 개체는 메서드에 전달 됩니다 `SKBitmap.Decode` .

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

`Tapped`처리기는 또한 `InvalidateSurface` 개체의 메서드를 호출 합니다 `SKCanvasView` . 그러면 처리기에 대 한 새 호출이 생성 `PaintSurface` 됩니다.

## <a name="displaying-the-bitmaps"></a>비트맵 표시

`PaintSurface`처리기는 세 개의 비트맵을 표시 해야 합니다. 처리기는 전화가 세로 모드 이며 캔버스를 세로 방향으로 3 개의 동일한 부분으로 나눕니다.

첫 번째 비트맵이 가장 간단한 방법으로 표시 됩니다 [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,System.Single,System.Single,SkiaSharp.SKPaint)) . 비트맵의 왼쪽 위 모퉁이가 배치 되는 X 및 Y 좌표는 다음과 같이 지정 해야 합니다.

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

`SKPaint`매개 변수가 정의 되어 있지만 기본값은 `null` 이며 무시 해도 됩니다. 비트맵의 픽셀은 단순히 일 대 일 매핑을 사용 하 여 디스플레이 표면의 픽셀에 전송 됩니다. `SKPaint` [**SkiaSharp 투명도**](transparency.md)에 대 한 다음 섹션에서이 인수에 대 한 응용 프로그램을 볼 수 있습니다.

프로그램은 및 속성을 사용 하 여 비트맵의 픽셀 크기를 가져올 수 있습니다 [`Width`](xref:SkiaSharp.SKBitmap.Width) [`Height`](xref:SkiaSharp.SKBitmap.Height) . 이러한 속성을 사용 하면 프로그램에서 좌표를 계산 하 여 캔버스의 세 번째 중심 가운데에 비트맵을 배치할 수 있습니다.

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

다른 두 비트맵은 [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKPaint)) 매개 변수를 사용 하 여 버전으로 표시 됩니다 `SKRect` .

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

세 번째 버전의에는 [`DrawBitmap`](xref:SkiaSharp.SKCanvas.DrawBitmap(SkiaSharp.SKBitmap,SkiaSharp.SKRect,SkiaSharp.SKRect,SkiaSharp.SKPaint)) `SKRect` 표시할 비트맵의 사각형 하위 집합을 지정 하는 두 개의 인수가 있지만이 문서에서는 해당 버전이 사용 되지 않습니다.

포함 된 리소스 비트맵에서 로드 된 비트맵을 표시 하는 코드는 다음과 같습니다.

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

비트맵이 사각형의 크기에 맞게 확장 되므로 이러한 스크린샷에는 원숭이가 가로로 확장 됩니다.

[![](bitmaps-images/basicbitmaps-small.png "A triple screenshot of the Basic Bitmaps page")](bitmaps-images/basicbitmaps-large.png#lightbox "A triple screenshot of the Basic Bitmaps page")

&mdash;프로그램을 실행 하 고 자체 그림 라이브러리에서 사진을 로드 하는 경우에만 표시 되는 세 번째 이미지는 &mdash; 사각형 내에도 표시 되지만 사각형의 위치와 크기는 비트맵의 가로 세로 비율을 유지 하기 위해 조정 됩니다. 이 계산은 비트맵 및 대상 사각형의 크기를 기반으로 하 고 해당 영역에서 사각형을 가운데에 정렬 하 여 배율 인수를 계산 해야 하기 때문에 좀 더 복잡 합니다.

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

그림 라이브러리에서 아직 비트맵을 로드 하지 않은 경우 블록은 사용자에 게 `else` 화면을 탭 하 라는 메시지를 표시 하는 텍스트를 표시 합니다.

다양 한 수준의 투명도를 사용 하 여 비트맵을 표시할 수 있으며, [**SkiaSharp 투명도**](transparency.md) 에 대 한 다음 문서에서 방법을 설명 합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [사진 라이브러리에서 사진 선택](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
