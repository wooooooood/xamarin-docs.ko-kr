---
title: SkiaSharp 비트맵에 애니메이션 적용
description: 순차적으로 일련의 비트맵을 표시 하 고 애니메이션된 GIF 파일을 렌더링 하 여 비트맵 애니메이션을 수행 하는 방법에 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97142ADC-E2FD-418C-8A09-9C561AEE5BFD
author: davidbritch
ms.author: dabritch
ms.date: 07/12/2018
ms.openlocfilehash: 69f77ef7959a53fa46210d7e6e68b9666692423b
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653092"
---
# <a name="animating-skiasharp-bitmaps"></a>SkiaSharp 비트맵에 애니메이션 적용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

일반적으로 SkiaSharp 그래픽에 애니메이션을 적용 하는 응용 프로그램 호출 `InvalidateSurface` 에 `SKCanvasView` 종종 16 밀리초 마다 고정 요금. 에 대 한 호출을 트리거하는 화면을 무효화 합니다 `PaintSurface` 처리기 표시를 다시 그려야 합니다. 시각적 개체에는 초당 60 번 그려지는,으로 애니메이션을 적용할 원활 하 게 표시 됩니다.

그러나 그래픽에 16 밀리초에서 렌더링할 너무 복잡 한 경우 애니메이션 흔들림 될 수 있습니다. 프로그래머가 30 배 또는 15 번 초 새로 고침 빈도 줄일 수도 있지만 되기도 하는 충분 하지 않습니다. 경우에 따라 그래픽은 복잡 하 게 하는 단순히 렌더링할 수 없습니다 실시간에서입니다.

하나의 솔루션 미리 일련의 비트맵에 있는 애니메이션의 개별 프레임을 렌더링 하 여 애니메이션에 대 한 준비 하는 것입니다. 애니메이션을 표시 하는 초당 60 번 이러한 비트맵을 순차적으로 표시 하는 데 필요한만 합니다.

물론, 비트맵, 많은 가능성이 이지만 큰 예산을 어떻게 3D 애니메이션된 영화 이루어집니다. 3D 그래픽은 실시간으로 렌더링할 훨씬 너무 복잡 합니다. 각 프레임을 렌더링 하는 많은 처리 시간이 필요 합니다. 동영상을 시청 하는 경우 표시 되는 비트맵의 일련 기본적으로 합니다.

SkiaSharp에서 유사 하 게 수행할 수 있습니다. 이 문서에서는 두 가지 유형의 비트맵 애니메이션을 보여 줍니다. 첫 번째 예제는 Mandelbrot 집합의 애니메이션.

![애니메이션 샘플](animating-images/AnimatingSample.png "애니메이션 샘플")

두 번째 예제 SkiaSharp 사용 하 여 애니메이션된 GIF 파일을 렌더링 하는 방법을 보여 줍니다.

## <a name="bitmap-animation"></a>비트맵 애니메이션

Mandelbrot 집합을 시각적으로 썼으며 이지만 computionally 매우 긴 경우 (여기에 수학 및 Mandelbrot 집합의 내용은 참조 하세요. [의 20 장 _Creating Mobile Apps with Xamarin.Forms_ ](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf) 666 페이지를 시작 합니다. 다음 설명에서는 해당 배경 지식을)

합니다 [ **Mandelbrot 애니메이션** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima) 샘플 Mandelbrot 집합에서 고정된 소수점의 지속적인 확대를 시뮬레이션 하기 위해 비트맵 애니메이션을 사용 합니다. 를 축소 하 여 다음 확대/축소 하 고 주기가 영구적으로 또는 프로그램을 종료할 때까지 반복 됩니다.

이 애니메이션에 대 한 응용 프로그램 로컬 저장소에 저장 하는 50 비트맵까지 만들어 프로그램을 준비 합니다. 각 비트맵은 이전 비트맵 너비와 높이 복합 평면의의 절반을 포함합니다. (프로그램에 이러한 비트맵을 나타내는 정수 라고 _확대/축소 수준_.) 비트맵 순서로 표시 됩니다. 각 비트맵의 배율을 다른 하나의 비트맵에서 원활한 진행을 위해 애니메이션 효과가 적용 됩니다.

20 장에서에서 설명 하는 최종 프로그램 같은 _Creating Mobile Apps with Xamarin.Forms_에서 Mandelbrot 집합의 계산 **Mandelbrot 애니메이션** 8을 사용 하 여 비동기 메서드 매개 변수입니다. 매개 변수는 복잡 한 중심점 너비 및 높이 중심점을 둘러싼 복합 평면의 포함 합니다. 다음 세 매개 변수는 픽셀 너비 및 만들려는 비트맵의 높이 및 최대 재귀 계산에 대 한 반복입니다. `progress` 매개 변수를 사용이 계산의 진행률을 표시 합니다. `cancelToken` 매개 변수는이 프로그램에서 사용 되지 않습니다.

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

형식의 개체를 반환 하는 메서드 `BitmapInfo` 비트맵 만들기에 대 한 정보를 제공 합니다.

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

합니다 **Mandelbrot 애니메이션** XAML 파일에 두 개의 `Label` 뷰를 `ProgressBar`, 및 `Button` 뿐만 `SKCanvasView`:

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

어느 시점에서에서는 아마도 변경 하려는 `COUNT` 값을 50 애니메이션의 전체 범위를 확인 합니다. 50 보다 큰 값 유용합니다. 48 정도의 확대/축소 수준, 배정밀도 부동 소수점 숫자의 해상도 Mandelbrot 집합 계산에 대 한 부족 한 됩니다. 이 문제는 684 페이지에서 설명 _Creating Mobile Apps with Xamarin.Forms_합니다.

`center` 값이 매우 중요 합니다. 이 애니메이션 확대/축소의 초점 이기도 합니다. 파일의 세 가지 값의 20 장 최종 스크린샷 세 개에 사용 된 _Creating Mobile Apps with Xamarin.Forms_ 페이지 684, 하지만 고유한 값 중 하나를 사용 하는 장에서 프로그램을 사용 하 여 실험할 수 있습니다.

합니다 **Mandelbrot 애니메이션** 이러한 샘플 저장 `COUNT` 로컬 응용 프로그램 저장소에는 비트맵입니다. 50 비트맵 20mb가 넘는 장치에서 저장소 이러한 비트맵 차지 하 고 저장소 양을 알아야 할 수 있도록 하며 시점에서 모두 삭제 하려고 할 수 있습니다. 아래쪽의 두 가지 방법의 용도는 `MainPage` 클래스:

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

로컬 응용 프로그램 저장소에 저장 된 비트맵 통합 합니다 `center` 값은 파일 이름에 변경한 경우를 `center` 설정을 기존 비트맵 저장소에 대체 되지 것입니다 및 공간을 차지 계속 합니다.

같습니다. 메서드는 `MainPage` 는 파일 이름을 생성 하기 위한 사용 뿐만 `MakePixel` 색 구성 요소를 기반으로 한 픽셀 값을 정의 하기 위한 메서드:

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

합니다 `zoomLevel` 매개 변수를 `FilePath` 범위는 0에서는 `COUNT` 상수 1을 뺀 값입니다.

합니다 `MainPage` 생성자 호출을 `LoadAndStartAnimation` 메서드:

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

`LoadAndStartAnimation` 메서드는 만들어진 있습니다 프로그램 이전에 실행 될 때 모든 비트맵을 로드 하려면 응용 프로그램 로컬 저장소에 액세스 하는 일을 담당 합니다. 반복 하 고 `zoomLevel` 값을 0에서 `COUNT`합니다. 파일이 존재 하는 경우에 로드 된 `bitmaps` 배열입니다. 특정 비트맵을 만드는 데 필요한이 고, 그렇지 `center` 하 고 `zoomLevel` 를 호출 하 여 값 `Mandelbrot.CalculateAsync`합니다. 해당 메서드가이 메서드를 색상으로 변환 하는 각 픽셀에 대 한 반복 수를 가져옵니다.

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

프로그램에서 장치의 사진 라이브러리 아닌 로컬 응용 프로그램 저장소에 이러한 비트맵을 저장 하는 것을 확인 합니다. .NET Standard 2.0 라이브러리를 사용 하면 친숙 한을 사용 하 여 `File.OpenRead` 고 `File.WriteAllBytes` 이 작업에 대 한 메서드.

메서드를 시작 하는 모든 비트맵 만들거나 메모리에 로드 한 후에 `Stopwatch` 개체와 호출 `Device.StartTimer`합니다. `OnTimerTick` 16 밀리초 마다 호출 됩니다.

`OnTimerTick` 계산을 `time` 6000 시간이 0 까지의 시간 (밀리초)의 값 `COUNT`, 각 비트맵의 표시를 위한 6 초 apportions입니다. 합니다 `progress` 사용 하 여 값을 `Math.Sin` 주기의 시작 부분에서 성능이 저하 됩니다 하는 사인 곡선 애니메이션을 만드는 값 및 느린 것으로 끝 방향을 반대로 바꿉니다.

합니다 `progress` 값을 0에서 범위 `COUNT`합니다. 즉, 변수의 정수 부분과 `progress` 인덱스인 합니다 `bitmaps` 배열에 생성 되 고 소수 부분의 `progress` 해당 특정 비트맵을 확대/축소 수준을 나타냅니다. 이러한 값에 저장 됩니다는 `bitmapIndex` 및 `bitmapProgress` 필드 및 하 여 표시 되는 `Label` 및 `Slider` XAML 파일에. `SKCanvasView` 비트맵 디스플레이를 업데이트 무효화 됩니다.

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

마지막으로, 합니다 `PaintSurface` 처리기는 `SKCanvasView` 가로 세로 비율을 유지 하면서 가능한 한 큰 비트맵을 표시 하려면 대상 사각형을 계산 합니다. 소스 사각형을 기반으로 합니다 `bitmapProgress` 값입니다. 합니다 `fraction` 값 범위는 0에서 계산 여기 `bitmapProgress` 0.25 경우 전체 비트맵을 표시 하려면 0 `bitmapProgress` 는 너비와 높이 확대/축소를 효과적으로 비트맵의 절반을 표시할 1:

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

실행 중인 프로그램이 다음과 같습니다.

[![Mandelbrot 애니메이션](animating-images/MandelbrotAnimation.png "Mandelbrot 애니메이션")](animating-images/MandelbrotAnimation-Large.png#lightbox)

## <a name="gif-animation"></a>GIF 애니메이션

형식 GIF (Graphics Interchange) 사양 장면의 루프에서 자주 연속적으로 표시 될 수 있는 여러 순차 프레임을 포함 하도록 단일 GIF 파일을 허용 하는 기능이 있습니다. 이러한 파일 이라고 _애니메이션 된 Gif_합니다. 웹 브라우저는 애니메이션된 Gif를 재생할 수 있습니다 하 고 SkiaSharp 응용 프로그램 애니메이션된 GIF 파일에서 프레임을 추출 하 고 순차적 표시를 허용 합니다.

합니다 [SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플 이라는 애니메이션된 GIF 리소스가 포함 **Newtons_cradle_animation_book_2.gif** DemonDeLuxe 만들고에서 다운로드를 [뉴턴의 크레들에 놓기를 ](https://en.wikipedia.org/wiki/Newton%27s_cradle) Wikipedia의 페이지입니다. 합니다 **애니메이션 GIF** 해당 정보를 제공 하 고 인스턴스화하는 XAML 파일을 포함 하는 페이지는 `SKCanvasView`:

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

페이지의 생성자에서 발생 하 고는 애니메이션된 GIF 파일 디코딩 합니다 `Stream` 비트맵을 참조 하는 개체를 만드는 데 사용할를 `SKManagedStream` 개체 차례로 [ `SKCodec` ](xref:SkiaSharp.SKCodec) 개체입니다. 합니다 [ `FrameCount` ](xref:SkiaSharp.SKCodec.FrameCount) 속성 애니메이션을 구성 하는 프레임의 수를 나타냅니다.

이러한 프레임 생성자를 사용 하므로 결과적으로 개별 비트맵으로 저장 됩니다 `FrameCount` 형식의 배열을 할당할 `SKBitmap` 두 뿐만 아니라 `int` 누적 된 기간 동안 각 프레임의와 (보다 쉽게 애니메이션 논리) 배열 재생 시간입니다.

합니다 [ `FrameInfo` ](xref:SkiaSharp.SKCodec.FrameInfo) 속성을 `SKCodec` 클래스는 배열을 [ `SKCodecFrameInfo` ](xref:SkiaSharp.SKCodecFrameInfo) 값, 각 프레임을 하지만이 프로그램은 해당 구조에서 유일한 항목에 대 한이를 [ `Duration` ](xref:SkiaSharp.SKCodecFrameInfo.Duration) 프레임의 시간 (밀리초)입니다.

`SKCodec` 라는 속성을 정의 [ `Info` ](xref:SkiaSharp.SKCodec.Info) 형식의 [ `SKImageInfo` ](xref:SkiaSharp.SKImageInfo)하지만 `SKImageInfo` 값을 나타냅니다 (적어도이 이미지에 대 한) 색 형식이 `SKColorType.Index8`, 즉 각 픽셀이 색 유형으로 인덱스 됩니다. 색상표를 사용 하지 않으려면 프로그램을 사용 하는 [ `Width` ](xref:SkiaSharp.SKImageInfo.Width) 하 고 [ `Height` ](xref:SkiaSharp.SKImageInfo.Height) 컬러를 소유 하는 정보를 생성 하는 구조는 `ImageInfo` 값. 각 `SKBitmap` 에서 만들어집니다.

합니다 `GetPixels` 메서드의 `SKBitmap` 반환을 `IntPtr` 해당 비트맵의 픽셀 비트를 참조 합니다. 이러한 픽셀 비트 아직 설정 되지 않았습니다. `IntPtr` 중 하나에 전달 되는 [ `GetPixels` ](xref:SkiaSharp.SKCodec.GetPixels(SkiaSharp.SKImageInfo,System.IntPtr,SkiaSharp.SKCodecOptions)) 메서드의 `SKCodec`합니다. 해당 메서드에 복사 프레임 GIF 파일에서 참조 하는 메모리 공간을 `IntPtr`입니다. 합니다 [ `SKCodecOptions` ](xref:SkiaSharp.SKCodecOptions) 생성자 프레임 수를 나타냅니다.

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

불구 하 고는 `IntPtr` 값을 no `unsafe` 되므로 코드 작업이 필요 합니다 `IntPtr` 는 C# 포인터 값으로 변환 되지 않습니다.

각 프레임을 추출한 후 생성자, 모든 프레임의 기간을 합계 하 고 누적된 기간을 사용 하 여 다른 배열을 초기화 합니다.

코드 숨김 파일의 나머지 애니메이션께 바 칩니다. `Device.StartTimer` 메서드, 타이머를 시작 하는 및 `OnTimerTick` 콜백을 사용 하는 `Stopwatch` 경과 시간 (밀리초) 결정 하는 개체입니다. 누적된 기간 배열을 통해 반복 하는 것은 충분 한 현재 프레임을 찾으려면:

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

때마다 합니다 `currentframe` 변수 변경은 `SKCanvasView` 무효화 됩니다 하 고 새 프레임에 표시 됩니다:

[![애니메이션 GIF](animating-images/AnimatedGif.png "애니메이션 GIF")](animating-images/AnimatedGif-Large.png#lightbox)

프로그램을 실행 해야 하는 물론, 직접 애니메이션을 볼 수 있습니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Mandelbrot 애니메이션 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima)
