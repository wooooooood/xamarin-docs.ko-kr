---
title: SkiaSharp 비트맵에 애니메이션 적용
description: 순차적으로 일련의 비트맵을 표시 하 고 애니메이션된 GIF 파일을 렌더링 하 여 비트맵 애니메이션을 수행 하는 방법에 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97142ADC-E2FD-418C-8A09-9C561AEE5BFD
author: davidbritch
ms.author: dabritch
ms.date: 07/12/2018
ms.openlocfilehash: 33e17a01d8a13fcdaee27e5857c554a4a232c534
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305674"
---
# <a name="animating-skiasharp-bitmaps"></a>SkiaSharp 비트맵에 애니메이션 적용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp 그래픽에 애니메이션을 적용 하는 응용 프로그램은 일반적으로 16 밀리초 마다 고정 속도로 `SKCanvasView`에 `InvalidateSurface`를 호출 합니다. 화면을 무효화 하면 `PaintSurface` 처리기에 대 한 호출이 트리거되어 표시를 다시 그립니다. 시각적 개체에는 초당 60 번 그려지는,으로 애니메이션을 적용할 원활 하 게 표시 됩니다.

그러나 그래픽에 16 밀리초에서 렌더링할 너무 복잡 한 경우 애니메이션 흔들림 될 수 있습니다. 프로그래머가 30 배 또는 15 번 초 새로 고침 빈도 줄일 수도 있지만 되기도 하는 충분 하지 않습니다. 경우에 따라 그래픽은 복잡 하 게 하는 단순히 렌더링할 수 없습니다 실시간에서입니다.

하나의 솔루션 미리 일련의 비트맵에 있는 애니메이션의 개별 프레임을 렌더링 하 여 애니메이션에 대 한 준비 하는 것입니다. 애니메이션을 표시 하는 초당 60 번 이러한 비트맵을 순차적으로 표시 하는 데 필요한만 합니다.

물론, 비트맵, 많은 가능성이 이지만 큰 예산을 어떻게 3D 애니메이션된 영화 이루어집니다. 3D 그래픽은 실시간으로 렌더링할 훨씬 너무 복잡 합니다. 각 프레임을 렌더링 하는 많은 처리 시간이 필요 합니다. 동영상을 시청 하는 경우 표시 되는 비트맵의 일련 기본적으로 합니다.

SkiaSharp에서 유사 하 게 수행할 수 있습니다. 이 문서에서는 두 가지 유형의 비트맵 애니메이션을 보여 줍니다. 첫 번째 예제는 Mandelbrot 집합의 애니메이션.

![애니메이션 샘플](animating-images/AnimatingSample.png "애니메이션 샘플")

두 번째 예제 SkiaSharp 사용 하 여 애니메이션된 GIF 파일을 렌더링 하는 방법을 보여 줍니다.

## <a name="bitmap-animation"></a>비트맵 애니메이션

Mandelbrot 집합을 시각적으로 썼으며 이지만 computionally 매우 긴 경우 여기에 사용 된 만델브로트 집합 및 수학에 대 한 자세한 내용은 666 페이지에서 시작 [ _하는 xamarin.ios를 사용 하 여 Mobile Apps 만들기_ 의 20 장](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf) 을 참조 하세요. 다음 설명에서는 해당 배경 지식을)

[**만델브로트 애니메이션**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima) 샘플에서는 비트맵 애니메이션을 사용 하 여 만델브로트 집합에서 고정 소수점의 연속 확대를 시뮬레이션 합니다. 를 축소 하 여 다음 확대/축소 하 고 주기가 영구적으로 또는 프로그램을 종료할 때까지 반복 됩니다.

이 애니메이션에 대 한 응용 프로그램 로컬 저장소에 저장 하는 50 비트맵까지 만들어 프로그램을 준비 합니다. 각 비트맵은 이전 비트맵 너비와 높이 복합 평면의의 절반을 포함합니다. 프로그램에서는 이러한 비트맵이 정수 _확대/축소 수준을_나타내는 것으로 간주 됩니다. 그런 다음 비트맵이 차례로 표시 됩니다. 각 비트맵의 배율을 다른 하나의 비트맵에서 원활한 진행을 위해 애니메이션 효과가 적용 됩니다.

_Xamarin.ios를 사용 하 여 Mobile Apps 만들기_의 20 장에서 설명 하는 최종 프로그램과 마찬가지로, **만델브로트 애니메이션** 에서 만델브로트 집합을 계산 하는 것은 매개 변수가 8 개인 비동기 메서드입니다. 매개 변수는 복잡 한 중심점 너비 및 높이 중심점을 둘러싼 복합 평면의 포함 합니다. 다음 세 매개 변수는 픽셀 너비 및 만들려는 비트맵의 높이 및 최대 재귀 계산에 대 한 반복입니다. `progress` 매개 변수는이 계산의 진행 상태를 표시 하는 데 사용 됩니다. `cancelToken` 매개 변수는이 프로그램에서 사용 되지 않습니다.

```csharp
static class Mandelbrot
{
    public static Task<BitmapInfo> CalculateAsync(Complex center,
                                                  double width, double height,
                                                  int pixelWidth, int pixelHeight,
                                                  int iterations,
                                                  IProgress<double> progress,
                                                  CancellationToken cancelToken)
    {
        return Task.Run(() =>
        {
            int[] iterationCounts = new int[pixelWidth * pixelHeight];
            int index = 0;

            for (int row = 0; row < pixelHeight; row++)
            {
                progress.Report((double)row / pixelHeight);
                cancelToken.ThrowIfCancellationRequested();

                double y = center.Imaginary + height / 2 - row * height / pixelHeight;

                for (int col = 0; col < pixelWidth; col++)
                {
                    double x = center.Real - width / 2 + col * width / pixelWidth;
                    Complex c = new Complex(x, y);

                    if ((c - new Complex(-1, 0)).Magnitude < 1.0 / 4)
                    {
                        iterationCounts[index++] = -1;
                    }
                    // http://www.reenigne.org/blog/algorithm-for-mandelbrot-cardioid/
                    else if (c.Magnitude * c.Magnitude * (8 * c.Magnitude * c.Magnitude - 3) < 3.0 / 32 - c.Real)
                    {
                        iterationCounts[index++] = -1;
                    }
                    else
                    {
                        Complex z = 0;
                        int iteration = 0;

                        do
                        {
                            z = z * z + c;
                            iteration++;
                        }
                        while (iteration < iterations && z.Magnitude < 2);

                        if (iteration == iterations)
                        {
                            iterationCounts[index++] = -1;
                        }
                        else
                        {
                            iterationCounts[index++] = iteration;
                        }
                    }
                }
            }
            return new BitmapInfo(pixelWidth, pixelHeight, iterationCounts);
        }, cancelToken);
    }
}
```

메서드는 비트맵을 만들기 위한 정보를 제공 하는 `BitmapInfo` 형식의 개체를 반환 합니다.

```csharp
class BitmapInfo
{
    public BitmapInfo(int pixelWidth, int pixelHeight, int[] iterationCounts)
    {
        PixelWidth = pixelWidth;
        PixelHeight = pixelHeight;
        IterationCounts = iterationCounts;
    }

    public int PixelWidth { private set; get; }

    public int PixelHeight { private set; get; }

    public int[] IterationCounts { private set; get; }
}
```

**만델브로트 Animation** XAML 파일에는 두 개의 `Label` 뷰, `ProgressBar`및 `SKCanvasView``Button` 포함 됩니다.

```csharp
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="MandelAnima.MainPage"
             Title="Mandelbrot Animation">

    <StackLayout>
        <Label x:Name="statusLabel"
               HorizontalTextAlignment="Center" />
        <ProgressBar x:Name="progressBar" />

        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <StackLayout Orientation="Horizontal"
                     Padding="5">
            <Label x:Name="storageLabel"
                   VerticalOptions="Center" />

            <Button x:Name="deleteButton"
                    Text="Delete All"
                    HorizontalOptions="EndAndExpand"
                    Clicked="OnDeleteButtonClicked" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

세 가지 중요 한 상수 및 비트맵의 배열을 정의 하 여 코드 숨김 파일을 시작 합니다.

```csharp
public partial class MainPage : ContentPage
{
    const int COUNT = 10;           // The number of bitmaps in the animation.
                                    // This can go up to 50!

    const int BITMAP_SIZE = 1000;   // Program uses square bitmaps exclusively

    // Uncomment just one of these, or define your own
    static readonly Complex center = new Complex(-1.17651152924355, 0.298520986549558);
    //   static readonly Complex center = new Complex(-0.774693089457127, 0.124226621261617);
    //   static readonly Complex center = new Complex(-0.556624880053304, 0.634696788141351);

    SKBitmap[] bitmaps = new SKBitmap[COUNT];   // array of bitmaps
    ···
}
```

어느 시점에서 `COUNT` 값을 50로 변경 하 여 애니메이션의 전체 범위를 볼 수 있습니다. 50 보다 큰 값 유용합니다. 48 정도의 확대/축소 수준, 배정밀도 부동 소수점 숫자의 해상도 Mandelbrot 집합 계산에 대 한 부족 한 됩니다. 이 문제는 _xamarin.ios를 사용 하 여 Mobile Apps 만들기_의 684 페이지에서 설명 합니다.

`center` 값이 매우 중요 합니다. 이 애니메이션 확대/축소의 초점 이기도 합니다. 파일의 세 가지 값은 684 페이지에서 _xamarin.ios를 사용 하 여 Mobile Apps를 만드는 방법_ 20 장에서 사용 된 세 가지 방법 중 하나 이며, 해당 챕터의 프로그램을 시험 하 여 고유한 값 중 하나를 얻을 수 있습니다.

**만델브로트 Animation** 샘플은 이러한 `COUNT` 비트맵을 로컬 응용 프로그램 저장소에 저장 합니다. 50 비트맵 20mb가 넘는 장치에서 저장소 이러한 비트맵 차지 하 고 저장소 양을 알아야 할 수 있도록 하며 시점에서 모두 삭제 하려고 할 수 있습니다. 이는 `MainPage` 클래스의 맨 아래에 있는 다음 두 메서드의 용도입니다.

```csharp
public partial class MainPage : ContentPage
{
    ···
    void TallyBitmapSizes()
    {
        long fileSize = 0;

        foreach (string filename in Directory.EnumerateFiles(FolderPath()))
        {
            fileSize += new FileInfo(filename).Length;
        }

        storageLabel.Text = $"Total storage: {fileSize:N0} bytes";
    }

    void OnDeleteButtonClicked(object sender, EventArgs args)
    {
        foreach (string filepath in Directory.EnumerateFiles(FolderPath()))
        {
            File.Delete(filepath);
        }

        TallyBitmapSizes();
    }
}
```

프로그램은 메모리에 유지 하므로 프로그램 동일한 그러한 비트맵에 애니메이션 효과 하는 동안 로컬 저장소에 비트맵을 삭제할 수 있습니다. 하지만 프로그램을 실행 하면 다음에 해야 비트맵을 다시 만듭니다.

로컬 응용 프로그램 저장소에 저장 된 비트맵은 파일 이름에 `center` 값을 통합 하므로 `center` 설정을 변경 하는 경우 기존 비트맵이 저장소에서 대체 되지 않으며 계속 공간을 차지 하 게 됩니다.

다음은 파일 이름을 생성 하는 데 사용 하는 메서드와 색 구성 요소를 기반으로 픽셀 값을 정의 하는 `MakePixel` 메서드 `MainPage`입니다.

```csharp
public partial class MainPage : ContentPage
{
    ···
    // File path for storing each bitmap in local storage
    string FolderPath() =>
        Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);

    string FilePath(int zoomLevel) =>
        Path.Combine(FolderPath(),
                     String.Format("R{0}I{1}Z{2:D2}.png", center.Real, center.Imaginary, zoomLevel));

    // Form bitmap pixel for Rgba8888 format
    uint MakePixel(byte alpha, byte red, byte green, byte blue) =>
        (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

`zoomLevel` 매개 변수는 0부터 `COUNT` 상수에서 1을 뺀 범위 `FilePath` 합니다.

`MainPage` 생성자는 `LoadAndStartAnimation` 메서드를 호출 합니다.

```csharp
public partial class MainPage : ContentPage
{
    ···
    public MainPage()
    {
        InitializeComponent();

        LoadAndStartAnimation();
    }
    ···
}
```

`LoadAndStartAnimation` 메서드는 이전에 프로그램이 실행 되었을 때 만들어진 모든 비트맵을 로드 하기 위해 응용 프로그램 로컬 저장소에 액세스 하는 책임이 있습니다. 0에서 `COUNT``zoomLevel` 값을 반복 합니다. 파일이 있으면 `bitmaps` 배열에 로드 합니다. 그렇지 않으면 `Mandelbrot.CalculateAsync`를 호출 하 여 특정 `center`에 대 한 비트맵을 만들고 값을 `zoomLevel` 해야 합니다. 해당 메서드가이 메서드를 색상으로 변환 하는 각 픽셀에 대 한 반복 수를 가져옵니다.

```csharp
public partial class MainPage : ContentPage
{
    ···
    async void LoadAndStartAnimation()
    {
        // Show total bitmap storage
        TallyBitmapSizes();

        // Create progressReporter for async operation
        Progress<double> progressReporter =
            new Progress<double>((double progress) => progressBar.Progress = progress);

        // Create (unused) CancellationTokenSource for async operation
        CancellationTokenSource cancelTokenSource = new CancellationTokenSource();

        // Loop through all the zoom levels
        for (int zoomLevel = 0; zoomLevel < COUNT; zoomLevel++)
        {
            // If the file exists, load it
            if (File.Exists(FilePath(zoomLevel)))
            {
                statusLabel.Text = $"Loading bitmap for zoom level {zoomLevel}";

                using (Stream stream = File.OpenRead(FilePath(zoomLevel)))
                {
                    bitmaps[zoomLevel] = SKBitmap.Decode(stream);
                }
            }
            // Otherwise, create a new bitmap
            else
            {
                statusLabel.Text = $"Creating bitmap for zoom level {zoomLevel}";

                CancellationToken cancelToken = cancelTokenSource.Token;

                // Do the (generally lengthy) Mandelbrot calculation
                BitmapInfo bitmapInfo =
                    await Mandelbrot.CalculateAsync(center,
                                                    4 / Math.Pow(2, zoomLevel),
                                                    4 / Math.Pow(2, zoomLevel),
                                                    BITMAP_SIZE, BITMAP_SIZE,
                                                    (int)Math.Pow(2, 10), progressReporter, cancelToken);

                // Create bitmap & get pointer to the pixel bits
                SKBitmap bitmap = new SKBitmap(BITMAP_SIZE, BITMAP_SIZE, SKColorType.Rgba8888, SKAlphaType.Opaque);
                IntPtr basePtr = bitmap.GetPixels();

                // Set pixel bits to color based on iteration count
                for (int row = 0; row < bitmap.Width; row++)
                    for (int col = 0; col < bitmap.Height; col++)
                    {
                        int iterationCount = bitmapInfo.IterationCounts[row * bitmap.Width + col];
                        uint pixel = 0xFF000000;            // black

                        if (iterationCount != -1)
                        {
                            double proportion = (iterationCount / 32.0) % 1;
                            byte red = 0, green = 0, blue = 0;

                            if (proportion < 0.5)
                            {
                                red = (byte)(255 * (1 - 2 * proportion));
                                blue = (byte)(255 * 2 * proportion);
                            }
                            else
                            {
                                proportion = 2 * (proportion - 0.5);
                                green = (byte)(255 * proportion);
                                blue = (byte)(255 * (1 - proportion));
                            }

                            pixel = MakePixel(0xFF, red, green, blue);
                        }

                        // Calculate pointer to pixel
                        IntPtr pixelPtr = basePtr + 4 * (row * bitmap.Width + col);

                        unsafe     // requires compiling with unsafe flag
                        {
                            *(uint*)pixelPtr.ToPointer() = pixel;
                        }
                    }

                // Save as PNG file
                SKData data = SKImage.FromBitmap(bitmap).Encode();

                try
                {
                    File.WriteAllBytes(FilePath(zoomLevel), data.ToArray());
                }
                catch
                {
                    // Probably out of space, but just ignore
                }

                // Store in array
                bitmaps[zoomLevel] = bitmap;

                // Show new bitmap sizes
                TallyBitmapSizes();
            }

            // Display the bitmap
            bitmapIndex = zoomLevel;
            canvasView.InvalidateSurface();
        }

        // Now start the animation
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }
    ···
}
```

프로그램에서 장치의 사진 라이브러리 아닌 로컬 응용 프로그램 저장소에 이러한 비트맵을 저장 하는 것을 확인 합니다. .NET Standard 2.0 라이브러리를 사용 하면이 작업에 익숙한 `File.OpenRead` 및 `File.WriteAllBytes` 메서드를 사용할 수 있습니다.

모든 비트맵이 생성 되거나 메모리로 로드 된 후 메서드는 `Stopwatch` 개체를 시작 하 고 `Device.StartTimer`를 호출 합니다. `OnTimerTick` 메서드는 16 밀리초 마다 호출 됩니다.

`OnTimerTick`는 `time` 값을 밀리초 단위로 계산 하며,이 값은 0 ~ 6000 시간 `COUNT`이며, 각 비트맵이 표시 되는 데 6 초 정도 걸립니다. `progress` 값은 `Math.Sin` 값을 사용 하 여 주기 시작에서 속도가 느려지고 방향이 반전 되는 끝에서 느려지는 사인 곡선 애니메이션을 만듭니다.

`progress` 값의 범위는 0에서 `COUNT`까지입니다. 즉, `progress`의 정수 부분은 `bitmaps` 배열의 인덱스이 고, `progress`의 소수 부분은 특정 비트맵의 확대/축소 수준을 나타냅니다. 이러한 값은 `bitmapIndex` 및 `bitmapProgress` 필드에 저장 되며 XAML 파일의 `Label` 및 `Slider` 표시 됩니다. `SKCanvasView`은 비트맵 표시를 업데이트 하는 데 무효가 됩니다.

```csharp
public partial class MainPage : ContentPage
{
    ···
    Stopwatch stopwatch = new Stopwatch();      // for the animation
    int bitmapIndex;
    double bitmapProgress = 0;
    ···
    bool OnTimerTick()
    {
        int cycle = 6000 * COUNT;       // total cycle length in milliseconds

        // Time in milliseconds from 0 to cycle
        int time = (int)(stopwatch.ElapsedMilliseconds % cycle);

        // Make it sinusoidal, including bitmap index and gradation between bitmaps
        double progress = COUNT * 0.5 * (1 + Math.Sin(2 * Math.PI * time / cycle - Math.PI / 2));

        // These are the field values that the PaintSurface handler uses
        bitmapIndex = (int)progress;
        bitmapProgress = progress - bitmapIndex;

        // It doesn't often happen that we get up to COUNT, but an exception would be raised
        if (bitmapIndex < COUNT)
        {
            // Show progress in UI
            statusLabel.Text = $"Displaying bitmap for zoom level {bitmapIndex}";
            progressBar.Progress = bitmapProgress;

            // Update the canvas
            canvasView.InvalidateSurface();
        }

        return true;
    }
    ···
}
```

마지막으로, `SKCanvasView`의 `PaintSurface` 처리기는 가로 세로 비율을 유지 하면서 최대한 크게 비트맵을 표시 하는 대상 사각형을 계산 합니다. 소스 사각형은 `bitmapProgress` 값을 기반으로 합니다. 여기에서 계산 된 `fraction` 값은 `bitmapProgress` 0부터 전체 비트맵 0.25을 표시 하 고 `bitmapProgress`가 1 이면 비트맵의 너비와 높이의 절반을 표시 하 고 효과적으로 확대/축소 합니다.

```csharp
public partial class MainPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        if (bitmaps[bitmapIndex] != null)
        {
            // Determine destination rect as square in canvas
            int dimension = Math.Min(info.Width, info.Height);
            float x = (info.Width - dimension) / 2;
            float y = (info.Height - dimension) / 2;
            SKRect destRect = new SKRect(x, y, x + dimension, y + dimension);

            // Calculate source rectangle based on fraction:
            //  bitmapProgress == 0: full bitmap
            //  bitmapProgress == 1: half of length and width of bitmap
            float fraction = 0.5f * (1 - (float)Math.Pow(2, -bitmapProgress));
            SKBitmap bitmap = bitmaps[bitmapIndex];
            int width = bitmap.Width;
            int height = bitmap.Height;
            SKRect sourceRect = new SKRect(fraction * width, fraction * height,
                                           (1 - fraction) * width, (1 - fraction) * height);

            // Display the bitmap
            canvas.DrawBitmap(bitmap, sourceRect, destRect);
        }
    }
    ···
}
```

실행 중인 프로그램은 다음과 같습니다.

[![만델브로트 애니메이션](animating-images/MandelbrotAnimation.png "만델브로트 애니메이션")](animating-images/MandelbrotAnimation-Large.png#lightbox)

## <a name="gif-animation"></a>GIF 애니메이션

형식 GIF (Graphics Interchange) 사양 장면의 루프에서 자주 연속적으로 표시 될 수 있는 여러 순차 프레임을 포함 하도록 단일 GIF 파일을 허용 하는 기능이 있습니다. 이러한 파일을 _애니메이션 gif_라고 합니다. 웹 브라우저는 애니메이션된 Gif를 재생할 수 있습니다 하 고 SkiaSharp 응용 프로그램 애니메이션된 GIF 파일에서 프레임을 추출 하 고 순차적 표시를 허용 합니다.

[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플에는 DemonDeLuxe에 의해 생성 되 고 위키백과의 [뉴턴의 크레들에 놓기](https://en.wikipedia.org/wiki/Newton%27s_cradle) 페이지에서 다운로드 한 **Newtons_cradle_animation_book_2** 라는 애니메이션 gif 리소스가 포함 되어 있습니다. **애니메이션 GIF** 페이지에는 해당 정보를 제공 하 고 `SKCanvasView`를 인스턴스화하는 XAML 파일이 포함 되어 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.AnimatedGifPage"
             Title="Animated GIF">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="0"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Label Text="GIF file by DemonDeLuxe from Wikipedia Newton's Cradle page"
               Grid.Row="1"
               Margin="0, 5"
               HorizontalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

코드 숨김 파일을 모든 애니메이션된 GIF 파일을 재생 하려면 일반화 되지 됩니다. 반복 횟수 및 루프에서 애니메이션된 GIF 재생 하기만 하면 특히를 사용할 수 있는 정보의 일부 무시 됩니다.

애니메이션된 GIF 파일의 프레임을 추출 하려면 SkisSharp 사용 하지 않는 것 어디서 나 문서화 대 한 설명은 아래 코드는 평소 보다 더 자세한 이므로:

애니메이션 GIF 파일의 디코딩은 페이지의 생성자에서 발생 하며, 비트맵을 참조 하는 `Stream` 개체를 사용 하 여 `SKManagedStream` 개체를 만든 다음 [`SKCodec`](xref:SkiaSharp.SKCodec) 개체를 사용 해야 합니다. [`FrameCount`](xref:SkiaSharp.SKCodec.FrameCount) 속성은 애니메이션을 구성 하는 프레임 수를 나타냅니다.

이러한 프레임은 궁극적으로 개별 비트맵으로 저장 되므로 생성자는 `FrameCount`을 사용 하 여 각 프레임의 기간에 대해 두 개의 `int` 배열과 각 프레임의 기간에 대해 두 개의 배열을 `SKBitmap` 할당 하 고 누적 된 기간을 줄이기 위해를 사용 합니다.

`SKCodec` 클래스의 [`FrameInfo`](xref:SkiaSharp.SKCodec.FrameInfo) 속성은 각 프레임 마다 하나씩 [`SKCodecFrameInfo`](xref:SkiaSharp.SKCodecFrameInfo) 값의 배열 이지만이 프로그램은 해당 구조에서 사용 하는 유일한 작업은 프레임의 [`Duration`](xref:SkiaSharp.SKCodecFrameInfo.Duration) (밀리초)입니다.

`SKCodec` [`SKImageInfo`](xref:SkiaSharp.SKImageInfo)형식의 [`Info`](xref:SkiaSharp.SKCodec.Info) 라는 속성을 정의 하지만 해당 `SKImageInfo` 값은 (적어도이 이미지) 색 형식이 `SKColorType.Index8`임을 나타냅니다. 즉, 각 픽셀은 색 형식의 인덱스입니다. 신경를 방지 하기 위해 프로그램은 [`Width`](xref:SkiaSharp.SKImageInfo.Width) 를 사용 하 고 해당 구조의 정보를 [`Height`](xref:SkiaSharp.SKImageInfo.Height) 하 여 자체의 전체 색 `ImageInfo` 값을 생성 합니다. 해당에서 각 `SKBitmap` 만들어집니다.

`SKBitmap`의 `GetPixels` 메서드는 해당 비트맵의 픽셀 비트를 참조 하는 `IntPtr` 반환 합니다. 이러한 픽셀 비트 아직 설정 되지 않았습니다. 이 `IntPtr`은 `SKCodec`의 [`GetPixels`](xref:SkiaSharp.SKCodec.GetPixels(SkiaSharp.SKImageInfo,System.IntPtr,SkiaSharp.SKCodecOptions)) 메서드 중 하나로 전달 됩니다. 이 메서드는 GIF 파일의 프레임을 `IntPtr`에서 참조 하는 메모리 공간에 복사 합니다. [`SKCodecOptions`](xref:SkiaSharp.SKCodecOptions) 생성자는 프레임 번호를 나타냅니다.

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;
    ···

    public AnimatedGifPage ()
    {
        InitializeComponent ();

        string resourceID = "SkiaSharpFormsDemos.Media.Newtons_cradle_animation_book_2.gif";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        using (SKCodec codec = SKCodec.Create(skStream))
        {
            // Get frame count and allocate bitmaps
            int frameCount = codec.FrameCount;
            bitmaps = new SKBitmap[frameCount];
            durations = new int[frameCount];
            accumulatedDurations = new int[frameCount];

            // Note: There's also a RepetitionCount property of SKCodec not used here

            // Loop through the frames
            for (int frame = 0; frame < frameCount; frame++)
            {
                // From the FrameInfo collection, get the duration of each frame
                durations[frame] = codec.FrameInfo[frame].Duration;

                // Create a full-color bitmap for each frame
                SKImageInfo imageInfo = code.new SKImageInfo(codec.Info.Width, codec.Info.Height);
                bitmaps[frame] = new SKBitmap(imageInfo);

                // Get the address of the pixels in that bitmap
                IntPtr pointer = bitmaps[frame].GetPixels();

                // Create an SKCodecOptions value to specify the frame
                SKCodecOptions codecOptions = new SKCodecOptions(frame, false);

                // Copy pixels from the frame into the bitmap
                codec.GetPixels(imageInfo, pointer, codecOptions);
            }

            // Sum up the total duration
            for (int frame = 0; frame < durations.Length; frame++)
            {
                totalDuration += durations[frame];
            }

            // Calculate the accumulated durations
            for (int frame = 0; frame < durations.Length; frame++)
            {
                accumulatedDurations[frame] = durations[frame] +
                    (frame == 0 ? 0 : accumulatedDurations[frame - 1]);
            }
        }
    }
    ···
}
```

`IntPtr` 값에도 불구 하 고 `IntPtr` C# 포인터 값으로 변환 되지 않으므로 `unsafe` 코드가 필요 하지 않습니다.

각 프레임을 추출한 후 생성자, 모든 프레임의 기간을 합계 하 고 누적된 기간을 사용 하 여 다른 배열을 초기화 합니다.

코드 숨김 파일의 나머지 애니메이션께 바 칩니다. `Device.StartTimer` 메서드는 타이머를 시작 하는 데 사용 되 고 `OnTimerTick` 콜백은 `Stopwatch` 개체를 사용 하 여 경과 된 시간 (밀리초)을 결정 합니다. 누적된 기간 배열을 통해 반복 하는 것은 충분 한 현재 프레임을 찾으려면:

```csharp
public partial class AnimatedGifPage : ContentPage
{
    SKBitmap[] bitmaps;
    int[] durations;
    int[] accumulatedDurations;
    int totalDuration;

    Stopwatch stopwatch = new Stopwatch();
    bool isAnimating;

    int currentFrame;
    ···
    protected override void OnAppearing()
    {
        base.OnAppearing();

        isAnimating = true;
        stopwatch.Start();
        Device.StartTimer(TimeSpan.FromMilliseconds(16), OnTimerTick);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();

        stopwatch.Stop();
        isAnimating = false;
    }

    bool OnTimerTick()
    {
        int msec = (int)(stopwatch.ElapsedMilliseconds % totalDuration);
        int frame = 0;

        // Find the frame based on the elapsed time
        for (frame = 0; frame < accumulatedDurations.Length; frame++)
        {
            if (msec < accumulatedDurations[frame])
            {
                break;
            }
        }

        // Save in a field and invalidate the SKCanvasView.
        if (currentFrame != frame)
        {
            currentFrame = frame;
            canvasView.InvalidateSurface();
        }

        return isAnimating;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        // Get the bitmap and center it
        SKBitmap bitmap = bitmaps[currentFrame];
        canvas.DrawBitmap(bitmap,info.Rect, BitmapStretch.Uniform);
    }
}
```

`currentframe` 변수가 변경 될 때마다 `SKCanvasView` 무효화 되 고 새 프레임이 표시 됩니다.

[![애니메이션 GIF](animating-images/AnimatedGif.png "애니메이션 GIF")](animating-images/AnimatedGif-Large.png#lightbox)

프로그램을 실행 해야 하는 물론, 직접 애니메이션을 볼 수 있습니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [만델브로트 애니메이션 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima)
