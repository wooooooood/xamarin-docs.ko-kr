---
title: SkiaSharp 비트맵에 애니메이션 적용
description: 일련의 비트맵을 순차적으로 표시 하 고 애니메이션 GIF 파일을 렌더링 하 여 비트맵 애니메이션을 수행 하는 방법을 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 97142ADC-E2FD-418C-8A09-9C561AEE5BFD
author: davidbritch
ms.author: dabritch
ms.date: 07/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 763f44c26d653aa32429b2aa764989e18e8b8078
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139973"
---
# <a name="animating-skiasharp-bitmaps"></a>SkiaSharp 비트맵에 애니메이션 적용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp 그래픽에 애니메이션을 적용 하는 응용 프로그램은 일반적으로 일반적으로 `InvalidateSurface` `SKCanvasView` 16 밀리초 마다 고정 속도로를 호출 합니다. 화면을 무효화 하면 처리기에 대 한 호출을 트리거하여 표시를 다시 `PaintSurface` 그립니다. 시각적 개체는 1 초에 60 번 다시 그려질 때 부드럽게 애니메이션으로 표시 됩니다.

그러나 그래픽이 너무 복잡 하 여 16 밀리초 안에 렌더링 되지 않으면 애니메이션이 떨림 될 수 있습니다. 프로그래머는 새로 고침 빈도를 30 초 또는 15 배가 되도록 선택할 수 있지만, 경우에 따라 충분 하지 않습니다. 때로는 그래픽이 매우 복잡 하 여 실시간으로 렌더링 되지 않을 수도 있습니다.

한 가지 해결 방법은 애니메이션의 개별 프레임을 일련의 비트맵에 렌더링 하 여 사전에 애니메이션을 준비 하는 것입니다. 애니메이션을 표시 하려면 이러한 비트맵을 1 초에 60 번 순서 대로 표시 하기만 하면 됩니다.

물론 매우 많은 비트맵이 될 수 있지만,이는 큰 예산 3D 애니메이션 동영상이 만들어지는 방법입니다. 3D 그래픽은 매우 복잡 하 여 실시간으로 렌더링할 수 없습니다. 각 프레임을 렌더링 하려면 많은 처리 시간이 필요 합니다. 영화를 볼 때 표시 되는 것은 기본적으로 일련의 비트맵입니다.

SkiaSharp에서 유사한 작업을 수행할 수 있습니다. 이 문서에서는 두 가지 유형의 비트맵 애니메이션을 보여 줍니다. 첫 번째 예제는 만델브로트 집합의 애니메이션입니다.

![애니메이션 샘플](animating-images/AnimatingSample.png "애니메이션 샘플")

두 번째 예제에서는 SkiaSharp를 사용 하 여 애니메이션 GIF 파일을 렌더링 하는 방법을 보여 줍니다.

## <a name="bitmap-animation"></a>비트맵 애니메이션

만델브로트 집합은 시각적으로 환상적인 긴 하지만 computionally. 여기에 사용 된 만델브로트 집합 및 수학에 대 한 자세한 내용은 666 페이지에서 시작 [ _하는 xamarin.ios를 사용 하 여 Mobile Apps 만들기_ 의 20 장](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf) 을 참조 하세요. 다음 설명에서는 배경 지식이 있다고 가정 합니다.

[**만델브로트 애니메이션**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima) 샘플에서는 비트맵 애니메이션을 사용 하 여 만델브로트 집합에서 고정 소수점의 연속 확대를 시뮬레이션 합니다. 확대 한 후에는 축소 하 고 나 서 프로그램을 종료할 때까지 주기가 계속 반복 됩니다.

프로그램은 응용 프로그램 로컬 저장소에 저장 되는 최대 50 개의 비트맵을 만들어이 애니메이션을 준비 합니다. 각 비트맵은 이전 비트맵으로 복잡 한 평면의 너비와 높이의 절반을 포함 합니다. 프로그램에서는 이러한 비트맵이 정수 _확대/축소 수준을_나타내는 것으로 간주 됩니다. 그런 다음 비트맵이 차례로 표시 됩니다. 각 비트맵의 크기 조정에 애니메이션을 적용 하 여 한 비트맵에서 다른 비트맵으로 부드러운 진행을 제공 합니다.

_Xamarin.ios를 사용 하 여 Mobile Apps 만들기_의 20 장에서 설명 하는 최종 프로그램과 마찬가지로, **만델브로트 애니메이션** 에서 만델브로트 집합을 계산 하는 것은 매개 변수가 8 개인 비동기 메서드입니다. 매개 변수에는 복합 중심점을 포함 하 고 해당 중심점을 둘러싼 복합 평면의 너비와 높이를 포함 합니다. 다음 세 매개 변수는 만들 비트맵의 픽셀 너비 및 높이와 재귀 계산에 대 한 최대 반복 횟수입니다. `progress`매개 변수는이 계산의 진행률을 표시 하는 데 사용 됩니다. `cancelToken`매개 변수는이 프로그램에서 사용 되지 않습니다.

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

메서드는 `BitmapInfo` 비트맵을 만들기 위한 정보를 제공 하는 형식의 개체를 반환 합니다.

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

**만델브로트 Animation** XAML 파일에는 및 라는 두 개의 뷰가 포함 되어 있습니다 `Label` `ProgressBar` `Button` `SKCanvasView` .

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

코드를 시작 하는 파일은 세 가지 중요 한 상수와 비트맵 배열을 정의 합니다.

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

특정 시점에서 `COUNT` 애니메이션의 전체 범위를 보려면 값을 50로 변경 하는 것이 좋습니다. 50 이상의 값은 유용 하지 않습니다. 48의 확대/축소 수준을 기준으로 하는 경우에는 배정밀도 부동 소수점 숫자의 해상도가 만델브로트 집합 계산에 충분 하지 않게 됩니다. 이 문제는 _xamarin.ios를 사용 하 여 Mobile Apps 만들기_의 684 페이지에서 설명 합니다.

`center`값은 매우 중요 합니다. 이는 애니메이션 확대/축소의 중심입니다. 파일의 세 가지 값은 684 페이지에서 _xamarin.ios를 사용 하 여 Mobile Apps를 만드는 방법_ 20 장에서 사용 된 세 가지 방법 중 하나 이며, 해당 챕터의 프로그램을 시험 하 여 고유한 값 중 하나를 얻을 수 있습니다.

**만델브로트 애니메이션** 샘플은 이러한 `COUNT` 비트맵을 로컬 응용 프로그램 저장소에 저장 합니다. 50 비트맵은 장치에 20mb를 초과 해야 하므로 이러한 비트맵이 차지 하 고 있는 저장소의 양을 파악 하는 것이 좋습니다. 이는 클래스의 맨 아래에 있는 다음 두 메서드의 용도입니다 `MainPage` .

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

프로그램이 동일한 비트맵을 메모리에 유지 하므로 로컬 저장소에서 비트맵을 삭제할 수 있습니다. 그러나 다음에 프로그램을 실행할 때 비트맵을 다시 만들어야 합니다.

로컬 응용 프로그램 저장소에 저장 된 비트맵의 `center` 파일 이름에는 값이 포함 되어 있으므로 설정을 변경 하면 `center` 기존 비트맵이 저장소에서 대체 되지 않으며 계속 공간을 차지 하 게 됩니다.

다음은에서 파일 이름을 생성 하는 데 사용 하는 메서드와 `MainPage` `MakePixel` 색 구성 요소를 기반으로 픽셀 값을 정의 하는 방법입니다.

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

`zoomLevel` `FilePath` 0에서 `COUNT` 상수로 1을 뺀 값 범위에 대 한 매개 변수입니다.

`MainPage`생성자는 메서드를 호출 합니다 `LoadAndStartAnimation` .

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

`LoadAndStartAnimation`메서드는 이전에 프로그램이 실행 되었을 때 만들어진 모든 비트맵을 로드 하기 위해 응용 프로그램 로컬 저장소에 액세스 하는 일을 담당 합니다. `zoomLevel`0에서 사이의 값을 반복 `COUNT` 합니다. 파일이 있으면 배열에 로드 `bitmaps` 합니다. 그렇지 않으면를 호출 하 여 특정 및 값에 대 한 비트맵을 만들어야 `center` `zoomLevel` `Mandelbrot.CalculateAsync` 합니다. 이 메서드는 각 픽셀의 반복 횟수를 가져오므로이 메서드가 색으로 변환 됩니다.

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

프로그램은 이러한 비트맵을 장치의 사진 라이브러리가 아닌 로컬 응용 프로그램 저장소에 저장 합니다. .NET Standard 2.0 라이브러리를 사용 하면 `File.OpenRead` 이 작업에 익숙한 및 메서드를 사용할 수 있습니다 `File.WriteAllBytes` .

모든 비트맵이 생성 되거나 메모리로 로드 된 후 메서드는 개체를 시작 `Stopwatch` 하 고를 호출 `Device.StartTimer` 합니다. `OnTimerTick`메서드는 16 밀리초 마다 호출 됩니다.

`OnTimerTick``time`0 ~ 6000 시간 범위의 값 (밀리초)을 계산 합니다 .이 값은 `COUNT` 각 비트맵이 표시 될 때까지 6 초 이내입니다. 값은 값을 사용 하 여 `progress` `Math.Sin` 주기 시작에서 속도가 느려지고 방향이 반대로 사인 곡선는 속도를 저하 하는 애니메이션을 만듭니다.

`progress`값의 범위는 0에서 `COUNT` 까지입니다. 즉,의 정수 부분은 `progress` 배열의 인덱스이 고 `bitmaps` 의 소수 부분은 `progress` 특정 비트맵의 확대/축소 수준을 나타냅니다. 이러한 값은 및 필드에 저장 되며 `bitmapIndex` `bitmapProgress` `Label` XAML 파일의 및에 표시 됩니다 `Slider` . 는 `SKCanvasView` 비트맵 디스플레이를 업데이트 하기 위해 무효화 됩니다.

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

마지막으로 `PaintSurface` 의 처리기는 `SKCanvasView` 가로 세로 비율을 유지 하면서 최대한 크게 비트맵을 표시 하는 대상 사각형을 계산 합니다. 소스 사각형은 값을 기반으로 `bitmapProgress` 합니다. 여기에서 계산 되는 값의 범위는 0 `fraction` 부터 `bitmapProgress` 전체 비트맵 0.25을 표시 하 고, `bitmapProgress` 가 1 이면 비트맵의 너비와 높이의 절반을 표시 하 고, 효과적으로 확대/축소 하는 것입니다.

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

GIF (Graphics 교환 형식) 사양에는 단일 GIF 파일에 연속 해 서 반복적으로 표시 될 수 있는 장면의 여러 순차 프레임을 포함할 수 있도록 하는 기능이 포함 되어 있습니다. 이러한 파일을 _애니메이션 gif_라고 합니다. 웹 브라우저는 애니메이션 gif를 재생할 수 있으며, SkiaSharp를 사용 하면 응용 프로그램에서 애니메이션 GIF 파일의 프레임을 추출 하 여 순차적으로 표시할 수 있습니다.

[SkiaSharpFormsDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 샘플에는 DemonDeLuxe에 의해 생성 되 고 위키백과의 [뉴턴의 크레들에 놓기](https://en.wikipedia.org/wiki/Newton%27s_cradle) 페이지에서 다운로드 한 **Newtons_cradle_animation_book_2.gif** 이라는 애니메이션 GIF 리소스가 포함 되어 있습니다. **애니메이션 GIF** 페이지에는 해당 정보를 제공 하 고를 인스턴스화하는 XAML 파일이 포함 되어 있습니다 `SKCanvasView` .

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

애니메이션 GIF 파일을 재생 하기 위해 코드 숨김이 일반화 되지 않습니다. 사용 가능한 일부 정보 (특히, 반복 횟수)를 무시 하 고 애니메이션 GIF를 반복 해 서 재생 합니다.

애니메이션 GIF 파일의 프레임을 추출 하는 데 지 수 Issharp를 사용 하는 것은 어디서 나 설명 하지 않으므로 다음 코드에 대 한 설명은 평소 보다 자세히 설명 합니다.

애니메이션 GIF 파일의 디코딩은 페이지의 생성자에서 발생 하며,이를 `Stream` 위해 비트맵을 참조 하는 개체를 사용 하 여 개체를 만든 다음 개체를 사용 해야 합니다 `SKManagedStream` [`SKCodec`](xref:SkiaSharp.SKCodec) . [`FrameCount`](xref:SkiaSharp.SKCodec.FrameCount)속성은 애니메이션을 구성 하는 프레임 수를 나타냅니다.

이러한 프레임은 결국 개별 비트맵으로 저장 되므로 생성자는를 사용 하 여 `FrameCount` `SKBitmap` `int` 각 프레임의 기간에 대해 두 개의 배열 뿐만 아니라 형식의 배열을 할당 합니다.

[`FrameInfo`](xref:SkiaSharp.SKCodec.FrameInfo)클래스의 속성은 `SKCodec` [`SKCodecFrameInfo`](xref:SkiaSharp.SKCodecFrameInfo) 각 프레임 마다 하나씩 값의 배열 이지만이 프로그램이 해당 구조에서 수행 하는 유일한 작업은 [`Duration`](xref:SkiaSharp.SKCodecFrameInfo.Duration) 프레임의 (밀리초)입니다.

`SKCodec`는 형식의 속성을 정의 [`Info`](xref:SkiaSharp.SKCodec.Info) [`SKImageInfo`](xref:SkiaSharp.SKImageInfo) 하지만 `SKImageInfo` 이 값은 (적어도이 이미지의 경우) 색 형식이 임을 나타냅니다 `SKColorType.Index8` . 즉, 각 픽셀은 색 형식의 인덱스입니다. 신경를 방지 하기 위해 프로그램은 해당 구조의 및 정보를 사용 하 여 [`Width`](xref:SkiaSharp.SKImageInfo.Width) [`Height`](xref:SkiaSharp.SKImageInfo.Height) 자체의 전체 색 값을 생성 `ImageInfo` 합니다. 각 `SKBitmap` 이에서 만들어집니다.

`GetPixels`의 메서드는 `SKBitmap` `IntPtr` 해당 비트맵의 픽셀 비트를 참조 하는를 반환 합니다. 이러한 픽셀 비트는 아직 설정 되지 않았습니다. 는 `IntPtr` 의 메서드 중 하나로 전달 됩니다 [`GetPixels`](xref:SkiaSharp.SKCodec.GetPixels(SkiaSharp.SKImageInfo,System.IntPtr,SkiaSharp.SKCodecOptions)) `SKCodec` . 이 메서드는 GIF 파일의 프레임을에서 참조 하는 메모리 공간에 복사 합니다 `IntPtr` . [`SKCodecOptions`](xref:SkiaSharp.SKCodecOptions)생성자는 프레임 번호를 나타냅니다.

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

값에도 불구 하 고 `IntPtr` `unsafe` 는 `IntPtr` c # 포인터 값으로 변환 되지 않으므로 코드가 필요 하지 않습니다.

각 프레임을 추출한 후에는 생성자가 모든 프레임의 기간을 요약 한 다음 누적 된 기간을 사용 하 여 다른 배열을 초기화 합니다.

코드 숨겨진 파일의 나머지 부분은 애니메이션 전용입니다. `Device.StartTimer`메서드는 타이머를 시작 하는 데 사용 되 고, `OnTimerTick` 콜백은 개체를 사용 하 여 `Stopwatch` 경과 된 시간 (밀리초)을 결정 합니다. 누적 된 기간 배열을 반복 하면 현재 프레임을 찾기에 충분 합니다.

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

`currentframe`변수가 변경 될 때마다 `SKCanvasView` 이 무효화 되 고 새 프레임이 표시 됩니다.

[![애니메이션 GIF](animating-images/AnimatedGif.png "애니메이션 GIF")](animating-images/AnimatedGif-Large.png#lightbox)

물론 프로그램을 직접 실행 하 여 애니메이션을 확인할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [만델브로트 애니메이션 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-mandelanima)
