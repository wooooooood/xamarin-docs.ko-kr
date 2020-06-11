---
제목: "터치 조작" 설명: "이 문서에서는 행렬 변환을 사용 하 여 터치 끌기, 집기 및 회전을 구현 하는 방법을 설명 하 고 샘플 코드를 사용 하 여이를 보여 줍니다."
ms. prod: xamarin. 기술: xamarin-skiasharp assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650 author: davidbritch: dabritch: 09/14/2018:-loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="touch-manipulations"></a>터치 조작

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Matrix 변환을 사용 하 여 터치 끌기, 집기 및 회전 구현_

모바일 장치의 경우와 같은 다중 터치 환경에서는 사용자가 화면에서 개체를 조작 하는 데 종종 손가락을 사용 합니다. 한 손가락 끌기를 비롯 한 두 손가락으로 이루어진 일반적인 제스처는 개체를 이동 하거나 크기를 조정 하거나 회전할 수도 있습니다. 이러한 제스처는 일반적으로 transform 매트릭스를 사용 하 여 구현 되며이 문서에서는이 작업을 수행 하는 방법을 보여 줍니다.

![](touch-images/touchmanipulationsexample.png "A bitmap subjected to translation, scaling, and rotation")

여기에 표시 된 모든 샘플에서는 Xamarin.Forms [**효과에서 이벤트를 호출**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)하는 문서에 제공 된 터치 추적 효과를 사용 합니다.

## <a name="dragging-and-translation"></a>끌어서 변환

Matrix 변환의 가장 중요 한 응용 프로그램 중 하나는 터치 처리입니다. 단일 [`SKMatrix`](xref:SkiaSharp.SKMatrix) 값은 일련의 터치 작업을 통합할 수 있습니다. 

단일 손가락 끌기의 경우 값은 `SKMatrix` 변환을 수행 합니다. 이는 **비트맵 끌기** 페이지에서 보여 줍니다. XAML 파일은에서를 인스턴스화합니다 `SKCanvasView` Xamarin.Forms `Grid` . `TouchEffect`다음의 컬렉션에 개체가 추가 되었습니다 `Effects` `Grid` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.BitmapDraggingPage"
             Title="Bitmap Dragging">
    
    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

이론적으로 개체는 `TouchEffect` 의 컬렉션에 직접 추가 될 수 `Effects` `SKCanvasView` 있지만 일부 플랫폼에서는 작동 하지 않습니다. 는 `SKCanvasView` 이 구성의와 크기가 동일 하기 때문에 `Grid` 작업에도 연결 하는 것이 좋습니다 `Grid` .

코드 숨김이 해당 생성자에서 비트맵 리소스에 로드 되 고 처리기에 해당 파일이 표시 됩니다 `PaintSurface` .

```csharp
public partial class BitmapDraggingPage : ContentPage
{
    // Bitmap and matrix for display
    SKBitmap bitmap;
    SKMatrix matrix = SKMatrix.MakeIdentity();
    ···

    public BitmapDraggingPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, new SKPoint());
    }
}
```

추가 코드가 없으면 `SKMatrix` 값이 항상 식별 매트릭스가 되며 비트맵의 표시에 영향을 주지 않습니다. `OnTouchEffectAction`XAML 파일에 설정 된 처리기의 목표는 터치 조작을 반영 하도록 행렬 값을 변경 하는 것입니다.

`OnTouchEffectAction`처리기는 먼저 Xamarin.Forms `Point` 값을 SkiaSharp 값으로 변환 `SKPoint` 합니다. 이는 `Width` `Height` 의 및 속성 `SKCanvasView` (장치 독립적 단위) 및 `CanvasSize` 속성 (픽셀 단위)을 기반으로 크기를 조정 하는 간단한 문제입니다.

```csharp
public partial class BitmapDraggingPage : ContentPage
{
    ···
    // Touch information
    long touchId = -1;
    SKPoint previousPoint;
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point = 
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Find transformed bitmap rectangle
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = matrix.MapRect(rect);

                // Determine if the touch was within that rectangle
                if (rect.Contains(point))
                {
                    touchId = args.Id;
                    previousPoint = point;
                }
                break;

            case TouchActionType.Moved:
                if (touchId == args.Id)
                {
                    // Adjust the matrix for the new position
                    matrix.TransX += point.X - previousPoint.X;
                    matrix.TransY += point.Y - previousPoint.Y;
                    previousPoint = point;
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = -1;
                break;
        }
    }
    ···
}
```

손가락이 화면에 먼저 닿을 때 형식의 이벤트가 `TouchActionType.Pressed` 발생 합니다. 첫 번째 작업은 손가락이 비트맵과 접촉 하 고 있는지 확인 하는 것입니다. 이러한 작업은 종종 _적중 테스트_라고 합니다. 이 경우에는 `SKRect` 비트맵에 해당 하는 값을 만들고이에 행렬 변환을 적용 한 `MapRect` 다음 터치 점이 변환 된 사각형 내에 있는지 확인 하 여 적중 테스트를 수행할 수 있습니다.

이 경우에는 해당 `touchId` 필드가 TOUCH ID로 설정 되 고 손가락 위치가 저장 됩니다.

이벤트의 경우 `TouchActionType.Moved` 값의 변환 요소는 `SKMatrix` 손가락의 현재 위치와 손가락의 새 위치를 기준으로 조정 됩니다. 새 위치는 다음 번에를 통해 저장 되 고 `SKCanvasView` 가 무효화 됩니다.

이 프로그램을 사용 하 여 시험해 볼 때 비트맵이 비트맵이 표시 되는 영역에 닿을 때만 비트맵을 끌 수 있습니다. 이 프로그램에 대 한 제한이 그다지 중요 하지는 않지만 여러 비트맵을 조작 하는 경우에는 중요 합니다.

## <a name="pinching-and-scaling"></a>집기 및 크기 조정

두 손가락으로 비트맵을 터치 하는 경우 어떤 작업을 수행 하 시겠습니까? 두 손가락을 동시에 이동 하는 경우 비트맵이 손가락과 함께 이동 하는 것이 좋습니다. 두 손가락에서 손가락 또는 확대 작업을 수행 하면 비트맵을 회전 (다음 섹션에서 설명) 하거나 크기를 조정 하는 것이 좋습니다. 비트맵을 확장 하는 경우 두 손가락을 비트맵을 기준으로 동일한 위치에 유지 하 고 비트맵을 적절 하 게 조정 하는 것이 가장 적합 합니다.

한 번에 두 손가락을 처리 하는 것은 복잡 하지만 처리기에서 한 번 `TouchAction` 에 하나의 손가락에 대 한 정보만 수신 한다는 점에 유의 하세요. 두 손가락으로 비트맵을 조작 하는 경우 각 이벤트에 대해 한 손가락의 위치가 변경 되었지만 나머지는 변경 되지 않았습니다. 아래 **비트맵 크기 조정** 페이지 코드에서 위치가 변경 되지 않은 손가락은 해당 점을 기준으로 하기 때문에 _피벗_ 점 이라고 합니다.

이 프로그램과 이전 프로그램의 차이점 중 하나는 여러 touch Id를 저장 해야 한다는 것입니다. 이 용도로는 touch ID가 사전 키이 고 사전 값이 해당 손가락의 현재 위치인 경우 사전이 사용 됩니다.

```csharp
public partial class BitmapScalingPage : ContentPage
{
    ···
    // Touch information
    Dictionary<long, SKPoint> touchDictionary = new Dictionary<long, SKPoint>();
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Find transformed bitmap rectangle
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = matrix.MapRect(rect);

                // Determine if the touch was within that rectangle
                if (rect.Contains(point) && !touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Add(args.Id, point);
                }
                break;

            case TouchActionType.Moved:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    // Single-finger drag
                    if (touchDictionary.Count == 1)
                    {
                        SKPoint prevPoint = touchDictionary[args.Id];

                        // Adjust the matrix for the new position
                        matrix.TransX += point.X - prevPoint.X;
                        matrix.TransY += point.Y - prevPoint.Y;
                        canvasView.InvalidateSurface();
                    }
                    // Double-finger scale and drag
                    else if (touchDictionary.Count >= 2)
                    {
                        // Copy two dictionary keys into array
                        long[] keys = new long[touchDictionary.Count];
                        touchDictionary.Keys.CopyTo(keys, 0);

                        // Find index of non-moving (pivot) finger
                        int pivotIndex = (keys[0] == args.Id) ? 1 : 0;

                        // Get the three points involved in the transform
                        SKPoint pivotPoint = touchDictionary[keys[pivotIndex]];
                        SKPoint prevPoint = touchDictionary[args.Id];
                        SKPoint newPoint = point;

                        // Calculate two vectors
                        SKPoint oldVector = prevPoint - pivotPoint;
                        SKPoint newVector = newPoint - pivotPoint;

                        // Scaling factors are ratios of those
                        float scaleX = newVector.X / oldVector.X;
                        float scaleY = newVector.Y / oldVector.Y;

                        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
                            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
                        {
                            // If something bad hasn't happened, calculate a scale and translation matrix
                            SKMatrix scaleMatrix = 
                                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);

                            SKMatrix.PostConcat(ref matrix, scaleMatrix);
                            canvasView.InvalidateSurface();
                        }
                    }

                    // Store the new point in the dictionary
                    touchDictionary[args.Id] = point;
                }

                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Remove(args.Id);
                }
                break;
        }
    }
    ···
}
```

작업 처리는 `Pressed` ID 및 터치 점이 사전에 추가 된다는 점을 제외 하 고 이전 프로그램과 거의 동일 합니다. `Released`및 `Cancelled` 작업은 사전 항목을 제거 합니다.

그러나 동작에 대 한 처리는 `Moved` 더 복잡 합니다. 하나의 손가락이 관련 되어 있는 경우에는 이전 프로그램과 매우 동일 하 게 처리 됩니다. 두 개 이상의 손가락에 대해 프로그램은 이동 하지 않는 손가락을 포함 하는 사전에서 정보를 가져와야 합니다. 이렇게 하려면 사전 키를 배열로 복사한 다음 첫 번째 키를 이동 중인 핑거의 ID와 비교 합니다. 이를 통해 프로그램은 이동 하지 않는 손가락에 해당 하는 피벗 점을 가져올 수 있습니다.

그런 다음,이 프로그램은 피벗 점을 기준으로 새 손가락 위치의 두 벡터와 피벗 점을 기준으로 하는 이전 손가락 위치를 계산 합니다. 이러한 벡터의 비율은 배율 인수입니다. 0으로 나누기는 가능성이 있으므로 무한 값 또는 NaN (숫자 아님) 값을 확인 해야 합니다. 모두 적절 한 경우에는 필드에 저장 된 값과 크기 조정 변환이 연결 됩니다 `SKMatrix` .

이 페이지를 시험해 보면 비트맵을 하나 또는 두 손가락으로 끌거나 두 손가락으로 크기를 조정할 수 있습니다. 크기 조정은 _이방성_이므로 가로 및 세로 방향으로 크기를 조정할 수 있습니다. 이는 가로 세로 비율을 왜곡 하지만 비트맵을 대칭 이동 하 여 미러 이미지를 만들 수도 있습니다. 비트맵을 0 차원으로 축소 하 고이를 사라지게 할 수도 있습니다. 프로덕션 코드에서이를 방지 하려고 합니다.

## <a name="two-finger-rotation"></a>두 손가락 회전

**비트맵 회전** 페이지에서 회전 또는 isotropic 배율에 두 손가락을 사용할 수 있습니다. 비트맵은 항상 올바른 가로 세로 비율을 유지 합니다. 회전 및 이방성 크기 조정을 위해 두 손가락을 사용 하는 것은 두 작업에서 손가락의 움직임이 매우 비슷하기 때문에 잘 작동 하지 않습니다.

이 프로그램의 가장 큰 차이점은 적중 테스트 논리입니다. 이전 프로그램에서는 `Contains` 의 메서드를 사용 `SKRect` 하 여 터치 지점이 비트맵에 해당 하는 변환 된 사각형 내에 있는지 여부를 확인 했습니다. 그러나 사용자가 비트맵을 조작 하는 경우 비트맵이 회전 될 수 있으며 `SKRect` 회전 된 사각형을 제대로 나타낼 수 없습니다. 이 경우에는 복잡 한 분석 기 하 도형을 구현 하는 데 적중 테스트 논리가 필요할 수 있습니다.

그러나 바로 가기를 사용할 수 있습니다. 즉, 요소가 변환 된 사각형의 경계 내에 있는지 확인 하는 것은 역 변환 된 점이 레이크의 사각형의 경계 내에 있는지 여부를 확인 하는 것과 동일 합니다. 이는 훨씬 손쉬운 계산 이며, 논리는 계속 해 서 편리한 메서드를 사용할 수 있습니다 `Contains` .

```csharp
public partial class BitmapRotationPage : ContentPage
{
    ···
    // Touch information
    Dictionary<long, SKPoint> touchDictionary = new Dictionary<long, SKPoint>();
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!touchDictionary.ContainsKey(args.Id))
                {
                    // Invert the matrix
                    if (matrix.TryInvert(out SKMatrix inverseMatrix))
                    {
                        // Transform the point using the inverted matrix
                        SKPoint transformedPoint = inverseMatrix.MapPoint(point);

                        // Check if it's in the untransformed bitmap rectangle
                        SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);

                        if (rect.Contains(transformedPoint))
                        {
                            touchDictionary.Add(args.Id, point);
                        }
                    }
                }
                break;

            case TouchActionType.Moved:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    // Single-finger drag
                    if (touchDictionary.Count == 1)
                    {
                        SKPoint prevPoint = touchDictionary[args.Id];

                        // Adjust the matrix for the new position
                        matrix.TransX += point.X - prevPoint.X;
                        matrix.TransY += point.Y - prevPoint.Y;
                        canvasView.InvalidateSurface();
                    }
                    // Double-finger rotate, scale, and drag
                    else if (touchDictionary.Count >= 2)
                    {
                        // Copy two dictionary keys into array
                        long[] keys = new long[touchDictionary.Count];
                        touchDictionary.Keys.CopyTo(keys, 0);

                        // Find index non-moving (pivot) finger
                        int pivotIndex = (keys[0] == args.Id) ? 1 : 0;

                        // Get the three points in the transform
                        SKPoint pivotPoint = touchDictionary[keys[pivotIndex]];
                        SKPoint prevPoint = touchDictionary[args.Id];
                        SKPoint newPoint = point;

                        // Calculate two vectors
                        SKPoint oldVector = prevPoint - pivotPoint;
                        SKPoint newVector = newPoint - pivotPoint;

                        // Find angles from pivot point to touch points
                        float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                        float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                        // Calculate rotation matrix
                        float angle = newAngle - oldAngle;
                        SKMatrix touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                        // Effectively rotate the old vector
                        float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                        oldVector.X = magnitudeRatio * newVector.X;
                        oldVector.Y = magnitudeRatio * newVector.Y;

                        // Isotropic scaling!
                        float scale = Magnitude(newVector) / Magnitude(oldVector);

                        if (!float.IsNaN(scale) && !float.IsInfinity(scale))
                        {
                            SKMatrix.PostConcat(ref touchMatrix,
                                SKMatrix.MakeScale(scale, scale, pivotPoint.X, pivotPoint.Y));

                            SKMatrix.PostConcat(ref matrix, touchMatrix);
                            canvasView.InvalidateSurface();
                        }
                    }

                    // Store the new point in the dictionary
                    touchDictionary[args.Id] = point;
                }

                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchDictionary.ContainsKey(args.Id))
                {
                    touchDictionary.Remove(args.Id);
                }
                break;
        }
    }

    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
    ···
}
```

이벤트에 대 한 논리는 `Moved` 이전 프로그램과 같은 방식으로 시작 됩니다. 및 라는 두 벡터는 이동 하는 `oldVector` `newVector` 손가락의 이전 및 현재 지점과 이동 하지 않는 손가락의 피벗 점을 기준으로 계산 됩니다. 그러나 이러한 벡터의 각도는 결정 되며 차이는 회전 각도입니다.

크기 조정이 포함 될 수도 있으므로 이전 벡터는 회전 각도에 따라 회전 됩니다. 이제 두 벡터의 상대적 크기가 배율 인수입니다. `scale`수평 및 수직 크기 조정에 동일한 값이 사용 되므로 크기가 isotropic. `matrix`이 필드는 회전 행렬과 크기 조정 매트릭스에 의해 조정 됩니다.

응용 프로그램에서 단일 비트맵 또는 다른 개체에 대 한 터치 처리를 구현 해야 하는 경우에는 응용 프로그램에 대 한 이러한 세 샘플의 코드를 수정할 수 있습니다. 하지만 여러 비트맵에 대해 터치 처리를 구현 해야 하는 경우에는 이러한 터치 작업을 다른 클래스에 캡슐화 하는 것이 좋습니다.

## <a name="encapsulating-the-touch-operations"></a>터치 작업 캡슐화

**터치 조작** 페이지는 단일 비트맵의 터치 조작을 보여 주지만 위에 표시 된 논리의 대부분을 캡슐화 하는 여러 다른 파일을 사용 합니다. 이러한 파일의 첫 번째는 표시 되는 [`TouchManipulationMode`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) 코드에 의해 구현 되는 다양 한 터치 조작 형식을 나타내는 열거형입니다.

```csharp
enum TouchManipulationMode
{
    None,
    PanOnly,
    IsotropicScale,     // includes panning
    AnisotropicScale,   // includes panning
    ScaleRotate,        // implies isotropic scaling
    ScaleDualRotate     // adds one-finger rotation
}
```

`PanOnly`는 변환을 사용 하 여 구현 되는 단일 손가락 끌기입니다. 모든 후속 옵션에는 패닝도 포함 되지만 두 손가락을 포함 `IsotropicScale` 합니다. 즉, 개체를 가로 및 세로 방향으로 균등 하 게 조정 하는 손가락으로 작업을 수행 합니다. `AnisotropicScale`크기를 다르게 조정할 수 있습니다.

`ScaleRotate`이 옵션은 두 손가락 크기 조정 및 회전에 대 한 옵션입니다. 크기 조정은 isotropic입니다. 앞서 언급 했 듯이, 대부분의 경우에는 손가락 이동이 본질적으로 동일 하기 때문에 이방성 크기 조정을 사용 하는 두 손가락 회전을 구현 하는 것이

이 `ScaleDualRotate` 옵션은 한 손가락 회전을 추가 합니다. 단일 손가락이 개체를 끌 때 끌어온 개체는 중심 주위로 먼저 회전 되어 개체의 중심이 끌기 벡터와 정렬 됩니다.

[**TouchManipulationPage**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) 파일에는 `Picker` 열거형의 멤버가 포함 된이 포함 됩니다 `TouchManipulationMode` .

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             xmlns:local="clr-namespace:SkiaSharpFormsDemos.Transforms"
             x:Class="SkiaSharpFormsDemos.Transforms.TouchManipulationPage"
             Title="Touch Manipulation">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker Title="Touch Mode"
                Grid.Row="0"
                SelectedIndexChanged="OnTouchModePickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type local:TouchManipulationMode}">
                    <x:Static Member="local:TouchManipulationMode.None" />
                    <x:Static Member="local:TouchManipulationMode.PanOnly" />
                    <x:Static Member="local:TouchManipulationMode.IsotropicScale" />
                    <x:Static Member="local:TouchManipulationMode.AnisotropicScale" />
                    <x:Static Member="local:TouchManipulationMode.ScaleRotate" />
                    <x:Static Member="local:TouchManipulationMode.ScaleDualRotate" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                4
            </Picker.SelectedIndex>
        </Picker>
        
        <Grid BackgroundColor="White"
              Grid.Row="1">
            
            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>
    </Grid>
</ContentPage>
```

아래쪽에는 및가 `SKCanvasView` `TouchEffect` 포함 된 단일 셀에 연결 된가 `Grid` 있습니다.

[**TouchManipulationPage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) 코드를 포함 하는 파일에는 `bitmap` 필드가 있지만 형식이 아닙니다 `SKBitmap` . 형식은 `TouchManipulationBitmap` (곧 표시 되는 클래스)입니다.

```csharp
public partial class TouchManipulationPage : ContentPage
{
    TouchManipulationBitmap bitmap;
    ...

    public TouchManipulationPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.MountainClimbers.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            SKBitmap bitmap = SKBitmap.Decode(stream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

생성자는 개체를 인스턴스화하고 `TouchManipulationBitmap` `SKBitmap` 포함 된 리소스에서 가져온 생성자에 전달 합니다. 생성자는 `Mode` `TouchManager` 개체의 속성 속성을 `TouchManipulationBitmap` 열거형의 멤버로 설정 하 여 마칩니다 `TouchManipulationMode` .

`SelectedIndexChanged`또한에 대 한 처리기는 `Picker` 이 속성을 설정 합니다 `Mode` .

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    void OnTouchModePickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (bitmap != null)
        {
            Picker picker = (Picker)sender;
            bitmap.TouchManager.Mode = (TouchManipulationMode)picker.SelectedItem;
        }
    }
    ...
}
```

`TouchAction` `TouchEffect` XAML 파일에서 인스턴스화된의 처리기는 및 라는의 두 메서드를 호출 합니다 `TouchManipulationBitmap` `HitTest` `ProcessTouchEvent` .

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    List<long> touchIds = new List<long>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (bitmap.HitTest(point))
                {
                    touchIds.Add(args.Id);
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    break;
                }
                break;

            case TouchActionType.Moved:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    touchIds.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

메서드가를 반환 하는 경우에는 `HitTest` `true` &mdash; 손가락이 비트맵이 차지 하는 영역 내에서 화면을 작업 했음을 의미 합니다 &mdash; . 그러면 터치 ID가 컬렉션에 추가 됩니다 `TouchIds` . 이 ID는 손가락이 화면에서 리프트 될 때까지 해당 손가락의 터치 이벤트 시퀀스를 나타냅니다. 여러 손가락에서 비트맵을 터치 하는 경우 `touchIds` 각 손가락의 터치 ID가 컬렉션에 포함 됩니다.

`TouchAction`처리기는 `ProcessTouchEvent` 의 클래스도 호출 `TouchManipulationBitmap` 합니다. 실제 터치식 처리가 발생 하는 일부 (전체는 아님)입니다.

[`TouchManipulationBitmap`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs)클래스는 `SKBitmap` 비트맵을 렌더링 하 고 터치 이벤트를 처리 하는 코드를 포함 하는의 래퍼 클래스입니다. 클래스에서 더 일반화 된 코드와 함께 작동 `TouchManipulationManager` 합니다 (곧 표시 될 예정).

`TouchManipulationBitmap`생성자는를 저장 `SKBitmap` 하 고 두 속성, `TouchManager` 형식의 속성 `TouchManipulationManager` 및 형식의 속성을 인스턴스화합니다 `Matrix` `SKMatrix` .

```csharp
class TouchManipulationBitmap
{
    SKBitmap bitmap;
    ...

    public TouchManipulationBitmap(SKBitmap bitmap)
    {
        this.bitmap = bitmap;
        Matrix = SKMatrix.MakeIdentity();

        TouchManager = new TouchManipulationManager
        {
            Mode = TouchManipulationMode.ScaleRotate
        };
    }

    public TouchManipulationManager TouchManager { set; get; }

    public SKMatrix Matrix { set; get; }
    ...
}
```

이 `Matrix` 속성은 모든 터치 활동으로 인해 누적 된 변형입니다. 여기에서 볼 수 있듯이 각 터치 이벤트는 행렬로 확인 된 다음 속성에 저장 된 값과 연결 됩니다 `SKMatrix` `Matrix` .

`TouchManipulationBitmap`개체는 자체의 메서드에 그려집니다 `Paint` . 인수가 `SKCanvas` 개체입니다. 여기 `SKCanvas` 에는 이미 변형이 적용 되어 있을 수 있으므로 메서드는 `Paint` `Matrix` 비트맵과 연결 된 속성을 기존 변환에 연결 하 고 완료 되 면 캔버스를 복원 합니다.

```csharp
class TouchManipulationBitmap
{
    ...
    public void Paint(SKCanvas canvas)
    {
        canvas.Save();
        SKMatrix matrix = Matrix;
        canvas.Concat(ref matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();
    }
    ...
}
```

`HitTest` `true` 사용자가 비트맵 경계 내의 지점에서 화면에 접촉 하는 경우 메서드는를 반환 합니다. 이는 이전에 **비트맵 회전** 페이지에 표시 된 논리를 사용 합니다.

```csharp
class TouchManipulationBitmap
{
    ...
    public bool HitTest(SKPoint location)
    {
        // Invert the matrix
        SKMatrix inverseMatrix;

        if (Matrix.TryInvert(out inverseMatrix))
        {
            // Transform the point using the inverted matrix
            SKPoint transformedPoint = inverseMatrix.MapPoint(location);

            // Check if it's in the untransformed bitmap rectangle
            SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
            return rect.Contains(transformedPoint);
        }
        return false;
    }
    ...
}
```

의 두 번째 공용 메서드 `TouchManipulationBitmap` 는 `ProcessTouchEvent` 입니다. 이 메서드를 호출 하면 터치 이벤트가이 특정 비트맵에 속하도록 이미 설정 된 것입니다. 메서드는 [`TouchManipulationInfo`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) 단순히 이전 지점과 각 손가락의 새 지점인 개체의 사전을 유지 관리 합니다.

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

사전 및 `ProcessTouchEvent` 메서드 자체는 다음과 같습니다.

```csharp
class TouchManipulationBitmap
{
    ...
    Dictionary<long, TouchManipulationInfo> touchDictionary =
        new Dictionary<long, TouchManipulationInfo>();
    ...
    public void ProcessTouchEvent(long id, TouchActionType type, SKPoint location)
    {
        switch (type)
        {
            case TouchActionType.Pressed:
                touchDictionary.Add(id, new TouchManipulationInfo
                {
                    PreviousPoint = location,
                    NewPoint = location
                });
                break;

            case TouchActionType.Moved:
                TouchManipulationInfo info = touchDictionary[id];
                info.NewPoint = location;
                Manipulate();
                info.PreviousPoint = info.NewPoint;
                break;

            case TouchActionType.Released:
                touchDictionary[id].NewPoint = location;
                Manipulate();
                touchDictionary.Remove(id);
                break;

            case TouchActionType.Cancelled:
                touchDictionary.Remove(id);
                break;
        }
    }
    ...
}
```

`Moved`및 이벤트에서 `Released` 메서드는를 호출 합니다 `Manipulate` . 이러한 시간에는 `touchDictionary` 하나 이상의 개체를 포함 `TouchManipulationInfo` 합니다. 에 `touchDictionary` 하나의 항목이 포함 된 경우 `PreviousPoint` 및 값이 같지 않을 수 `NewPoint` 있으며 손가락의 움직임을 나타냅니다. 여러 손가락에서 비트맵을 터치 하는 경우 사전에 항목이 두 개 이상 포함 되지만 이러한 항목 중 하나에는 서로 다른 `PreviousPoint` 및 값이 있습니다 `NewPoint` . 나머지 모든는 동일한 `PreviousPoint` 및 값을 갖습니다 `NewPoint` .

이는 중요 합니다. `Manipulate` 메서드는 한 손가락의 움직임만 처리 하 고 있다고 가정할 수 있습니다. 이 호출이 진행 되는 동안 다른 손가락은 이동 하지 않습니다. 즉, 이동 하는 경우에 대 한 후속 호출에서 해당 이동이 처리 됩니다 `Manipulate` .

`Manipulate`메서드는 편의를 위해 먼저 사전을 배열에 복사 합니다. 처음 두 항목 이외의 항목은 무시 합니다. 두 개 이상의 손가락이 비트맵을 조작 하려고 시도 하는 경우 나머지는 무시 됩니다. `Manipulate`최종 멤버 `TouchManipulationBitmap` :

```csharp
class TouchManipulationBitmap
{
    ...
    void Manipulate()
    {
        TouchManipulationInfo[] infos = new TouchManipulationInfo[touchDictionary.Count];
        touchDictionary.Values.CopyTo(infos, 0);
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();

        if (infos.Length == 1)
        {
            SKPoint prevPoint = infos[0].PreviousPoint;
            SKPoint newPoint = infos[0].NewPoint;
            SKPoint pivotPoint = Matrix.MapPoint(bitmap.Width / 2, bitmap.Height / 2);

            touchMatrix = TouchManager.OneFingerManipulate(prevPoint, newPoint, pivotPoint);
        }
        else if (infos.Length >= 2)
        {
            int pivotIndex = infos[0].NewPoint == infos[0].PreviousPoint ? 0 : 1;
            SKPoint pivotPoint = infos[pivotIndex].NewPoint;
            SKPoint newPoint = infos[1 - pivotIndex].NewPoint;
            SKPoint prevPoint = infos[1 - pivotIndex].PreviousPoint;

            touchMatrix = TouchManager.TwoFingerManipulate(prevPoint, newPoint, pivotPoint);
        }

        SKMatrix matrix = Matrix;
        SKMatrix.PostConcat(ref matrix, touchMatrix);
        Matrix = matrix;
    }
}
```

한 손가락에서 비트맵을 조작 하는 경우는 `Manipulate` `OneFingerManipulate` 개체의 메서드를 호출 합니다 `TouchManipulationManager` . 두 손가락에 대해를 호출 `TwoFingerManipulate` 합니다. 이러한 메서드에 대 한 인수는 동일 합니다. `prevPoint` 및 `newPoint` 인수는 이동 하는 손가락을 나타냅니다. 그러나 `pivotPoint` 두 호출에 대 한 인수는 서로 다릅니다.

한 손가락 조작의 경우는 `pivotPoint` 비트맵의 중심입니다. 이는 단일 손가락 회전을 허용 하기 위한 것입니다. 두 손가락 조작의 경우 이벤트는 한 손가락 으로만 이동 하 여 `pivotPoint` 가 이동 하지 않는 손가락 임을 나타냅니다.

두 경우 모두에서가 `TouchManipulationManager` `SKMatrix` 비트맵을 `Matrix` `TouchManipulationPage` 렌더링 하는 데 사용 하는 현재 속성과 연결 하는 값을 반환 합니다.

`TouchManipulationManager`는 일반화 되며를 제외 하 고 다른 파일은 사용 하지 않습니다 `TouchManipulationMode` . 사용자 고유의 응용 프로그램에서이 클래스를 변경 하지 않고 사용할 수 있습니다. 이 클래스는 `TouchManipulationMode` 형식의 단일 속성을 정의합니다.

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```

그러나이 옵션을 사용 하지 않는 것이 좋습니다 `AnisotropicScale` . 배율 인수 중 하나가 0이 되도록 비트맵을 조작 하는 것이 매우 쉽습니다. 그러면 비트맵이 보이지 않게 되며 반환 되지 않습니다. 이제는 이방성 크기 조정이 필요한 경우 원치 않는 결과를 방지 하기 위해 논리를 향상 시킬 수 있습니다.

`TouchManipulationManager`는 벡터를 사용 하지만 SkiaSharp에는 구조가 없기 때문에 `SKVector` `SKPoint` 대신가 사용 됩니다. `SKPoint`빼기 연산자를 지원 하 고 결과를 벡터로 취급할 수 있습니다. 추가 해야 하는 벡터 특정 논리도 `Magnitude` 계산입니다.

```csharp
class TouchManipulationManager
{
    ...
    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
}
```

회전이 선택 될 때마다 한 손가락 및 2 손가락 조작 메서드는 회전을 먼저 처리 합니다. 회전이 감지 되 면 회전 구성 요소가 효과적으로 제거 됩니다. 남아 있는 사항은 패닝 및 크기 조정으로 해석 됩니다.

메서드는 다음과 같습니다 `OneFingerManipulate` . 한 손가락 회전을 사용 하도록 설정 하지 않은 경우에는 논리는 간단 하 게 &mdash; 이전 지점과 새 점을 사용 하 여 `delta` 변환에 정확 하 게 대응 하는 라는 벡터를 생성 합니다. 한 손가락 회전을 사용 하는 경우이 메서드는 피벗 지점 (비트맵의 중심)의 각도를 이전 지점과 새 점으로 사용 하 여 회전 행렬을 생성 합니다.

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }

    public SKMatrix OneFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        if (Mode == TouchManipulationMode.None)
        {
            return SKMatrix.MakeIdentity();
        }

        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint delta = newPoint - prevPoint;

        if (Mode == TouchManipulationMode.ScaleDualRotate)  // One-finger rotation
        {
            SKPoint oldVector = prevPoint - pivotPoint;
            SKPoint newVector = newPoint - pivotPoint;

            // Avoid rotation if fingers are too close to center
            if (Magnitude(newVector) > 25 && Magnitude(oldVector) > 25)
            {
                float prevAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                // Calculate rotation matrix
                float angle = newAngle - prevAngle;
                touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                // Effectively rotate the old vector
                float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                oldVector.X = magnitudeRatio * newVector.X;
                oldVector.Y = magnitudeRatio * newVector.Y;

                // Recalculate delta
                delta = newVector - oldVector;
            }
        }

        // Multiply the rotation matrix by a translation matrix
        SKMatrix.PostConcat(ref touchMatrix, SKMatrix.MakeTranslation(delta.X, delta.Y));

        return touchMatrix;
    }
    ...
}
```

`TwoFingerManipulate`메서드에서 피벗 점은이 특정 터치 이벤트에서 이동 하지 않는 손가락의 위치입니다. 회전은 한 손가락 회전과 매우 비슷하며, `oldVector` 이전 점을 기반으로 하는 벡터는 회전에 맞게 조정 됩니다. 나머지 이동은 크기 조정으로 해석 됩니다.

```csharp
class TouchManipulationManager
{
    ...
    public SKMatrix TwoFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint oldVector = prevPoint - pivotPoint;
        SKPoint newVector = newPoint - pivotPoint;

        if (Mode == TouchManipulationMode.ScaleRotate ||
            Mode == TouchManipulationMode.ScaleDualRotate)
        {
            // Find angles from pivot point to touch points
            float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
            float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

            // Calculate rotation matrix
            float angle = newAngle - oldAngle;
            touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

            // Effectively rotate the old vector
            float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
            oldVector.X = magnitudeRatio * newVector.X;
            oldVector.Y = magnitudeRatio * newVector.Y;
        }

        float scaleX = 1;
        float scaleY = 1;

        if (Mode == TouchManipulationMode.AnisotropicScale)
        {
            scaleX = newVector.X / oldVector.X;
            scaleY = newVector.Y / oldVector.Y;

        }
        else if (Mode == TouchManipulationMode.IsotropicScale ||
                 Mode == TouchManipulationMode.ScaleRotate ||
                 Mode == TouchManipulationMode.ScaleDualRotate)
        {
            scaleX = scaleY = Magnitude(newVector) / Magnitude(oldVector);
        }

        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
        {
            SKMatrix.PostConcat(ref touchMatrix,
                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y));
        }

        return touchMatrix;
    }
    ...
}
```

이 메서드에는 명시적 변환이 없다는 것을 알 수 있습니다. 그러나 `MakeRotation` 및 메서드는 모두 `MakeScale` 피벗 점을 기반으로 하며 암시적 변환이 포함 되어 있습니다. 비트맵에서 두 손가락을 사용 하 고 동일한 방향으로 끌어 오면는 `TouchManipulation` 두 손가락 사이에서 반복 되는 일련의 터치 이벤트를 가져옵니다. 각 손가락은 다른 크기 조정 또는 회전 결과를 기준으로 이동 하지만 다른 손가락의 이동으로 인해 부정 되며 결과는 변환입니다.

**터치 조작** 페이지의 나머지 부분은 `PaintSurface` `TouchManipulationPage` 코드 숨겨진 파일의 처리기입니다. 이를 통해의 메서드를 호출 하 여 `Paint` `TouchManipulationBitmap` 누적 된 터치 활동을 나타내는 행렬을 적용 합니다.

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    MatrixDisplay matrixDisplay = new MatrixDisplay();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        bitmap.Paint(canvas);

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(bitmap.Matrix);

        matrixDisplay.Paint(canvas, bitmap.Matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));
    }
}
```

`PaintSurface`처리기는 `MatrixDisplay` 누적 된 터치 매트릭스를 표시 하는 개체를 표시 하 여 종료 됩니다.

[![](touch-images/touchmanipulation-small.png "Triple screenshot of the Touch Manipulation page")](touch-images/touchmanipulation-large.png#lightbox "Triple screenshot of the Touch Manipulation page")

## <a name="manipulating-multiple-bitmaps"></a>여러 비트맵 조작

및와 같은 클래스에서 터치 처리 코드를 격리 하는 경우의 이점 중 하나 `TouchManipulationBitmap` `TouchManipulationManager` 는 사용자가 여러 비트맵을 조작할 수 있도록 하는 프로그램에서 이러한 클래스를 다시 사용 하는 기능입니다.

**비트맵 산 보기** 페이지에서는이 작업을 수행 하는 방법을 보여 줍니다. 클래스는 형식의 필드를 정의 `TouchManipulationBitmap` 하는 대신 [`BitmapScatterPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) `List` 비트맵 개체의를 정의 합니다.

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    List<TouchManipulationBitmap> bitmapCollection =
        new List<TouchManipulationBitmap>();
    ...
    public BitmapScatterViewPage()
    {
        InitializeComponent();

        // Load in all the available bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;
        string[] resourceIDs = assembly.GetManifestResourceNames();
        SKPoint position = new SKPoint();

        foreach (string resourceID in resourceIDs)
        {
            if (resourceID.EndsWith(".png") ||
                resourceID.EndsWith(".jpg"))
            {
                using (Stream stream = assembly.GetManifestResourceStream(resourceID))
                {
                    SKBitmap bitmap = SKBitmap.Decode(stream);
                    bitmapCollection.Add(new TouchManipulationBitmap(bitmap)
                    {
                        Matrix = SKMatrix.MakeTranslation(position.X, position.Y),
                    });
                    position.X += 100;
                    position.Y += 100;
                }
            }
        }
    }
    ...
}
```

생성자는 포함 리소스로 사용 가능한 모든 비트맵에서 로드 하 고에 추가 합니다 `bitmapCollection` . `Matrix`속성은 각 개체에 대해 초기화 `TouchManipulationBitmap` 되므로 각 비트맵의 왼쪽 위 모퉁이는 100 픽셀 만큼 오프셋 됩니다.

`BitmapScatterView`또한이 페이지는 여러 비트맵의 터치 이벤트를 처리 해야 합니다. `List`현재 조작 된 개체의 Touch id를 정의 하는 대신 `TouchManipulationBitmap` ,이 프로그램에는 사전이 필요 합니다.

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    Dictionary<long, TouchManipulationBitmap> bitmapDictionary =
       new Dictionary<long, TouchManipulationBitmap>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                for (int i = bitmapCollection.Count - 1; i >= 0; i--)
                {
                    TouchManipulationBitmap bitmap = bitmapCollection[i];

                    if (bitmap.HitTest(point))
                    {
                        // Move bitmap to end of collection
                        bitmapCollection.Remove(bitmap);
                        bitmapCollection.Add(bitmap);

                        // Do the touch processing
                        bitmapDictionary.Add(args.Id, bitmap);
                        bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                        canvasView.InvalidateSurface();
                        break;
                    }
                }
                break;

            case TouchActionType.Moved:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    bitmapDictionary.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

`Pressed`논리가를 역방향으로 반복 하는 방법을 확인 합니다 `bitmapCollection` . 비트맵은 종종 서로 겹칩니다. 컬렉션의 뒷부분에 있는 비트맵이 시각적으로 컬렉션의 앞에 있는 비트맵 위에 배치 됩니다. 화면에서를 누르는 여러 비트맵이 손가락 아래에 있는 경우 맨 위에 있는 비트맵이 해당 손가락에 의해 조작 되는 항목 이어야 합니다.

또한 `Pressed` 논리는이 비트맵을 컬렉션의 끝으로 이동 하 여 다른 비트맵의 기반으로 시각적으로 이동 하도록 합니다.

`Moved`및 이벤트에서 `Released` `TouchAction` 처리기는 `ProcessingTouchEvent` 이전 프로그램과 마찬가지로에서 메서드를 호출 합니다 `TouchManipulationBitmap` .

마지막으로 `PaintSurface` 처리기는 `Paint` 각 개체의 메서드를 호출 합니다 `TouchManipulationBitmap` .

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (TouchManipulationBitmap bitmap in bitmapCollection)
        {
            bitmap.Paint(canvas);
        }
    }
}
```

이 코드는 컬렉션을 반복 하 고 컬렉션의 시작부터 끝 까지의 비트맵 더미를 표시 합니다.

[![](touch-images/bitmapscatterview-small.png "Triple screenshot of the Bitmap Scatter View page")](touch-images/bitmapscatterview-large.png#lightbox "Triple screenshot of the Bitmap Scatter View page")

## <a name="single-finger-scaling"></a>단일 손가락 크기 조정

크기 조정 작업을 수행 하려면 일반적으로 두 손가락을 사용 하는 손가락을 사용 해야 합니다. 그러나 손가락으로 비트맵의 모퉁이를 이동 하도록 하 여 단일 손가락으로 크기 조정을 구현할 수 있습니다.

이는 **단일 손가락 모서리 배율** 페이지에서 보여 줍니다. 이 샘플에서는 클래스에서 구현 된 것과 약간 다른 형식의 크기 조정을 사용 하므로 `TouchManipulationManager` 해당 클래스 또는 클래스를 사용 하지 않습니다 `TouchManipulationBitmap` . 대신 모든 터치 논리는 코드 숨김이 파일에 있습니다. 이는 한 번에 하나의 손가락만 추적 하 고 화면을 터치 하는 모든 보조 손가락을 무시 하기 때문에 일반적으로 약간 간단한 논리입니다.

[**SingleFingerCornerScale**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml) 페이지는 클래스를 인스턴스화하고 `SKCanvasView` `TouchEffect` 터치 이벤트를 추적 하기 위한 개체를 만듭니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.SingleFingerCornerScalePage"
             Title="Single Finger Corner Scale">

    <Grid BackgroundColor="White"
          Grid.Row="1">

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction"   />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

[**SingleFingerCornerScalePage.xaml.cs**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml.cs) 파일은 **미디어** 디렉터리에서 비트맵 리소스를 로드 하 고 필드로 정의 된 개체를 사용 하 여 표시 합니다 `SKMatrix` .

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();
    ···

    public SingleFingerCornerScalePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.SetMatrix(currentMatrix);
        canvas.DrawBitmap(bitmap, 0, 0);
    }
    ···
}
```

이 `SKMatrix` 개체는 아래에 표시 된 터치 논리에 의해 수정 됩니다.

코드 숨겨진 파일의 나머지 부분은 `TouchEffect` 이벤트 처리기입니다. 먼저 손가락의 현재 위치를 값으로 변환 `SKPoint` 합니다. `Pressed`작업 유형에 대해 처리기는 다른 손가락이 화면에 닿아 있지 않은지와 손가락이 비트맵 범위 내에 있는지 확인 합니다.

코드의 중요 한 부분은 `if` 메서드에 대 한 두 호출을 포함 하는 문입니다 `Math.Pow` . 이 수학은 손가락 위치가 비트맵을 채우는 타원 외부에 있는지 확인 합니다. 그렇다면 크기 조정 작업을 수행 합니다. 손가락이 비트맵의 모퉁이 중 하나 근처에 있고 피벗 점이 반대 모퉁이 임을 확인 합니다. 손가락이이 타원 안에 있으면 일반적인 패닝 작업입니다.

```csharp
public partial class SingleFingerCornerScalePage : ContentPage
{
    SKBitmap bitmap;
    SKMatrix currentMatrix = SKMatrix.MakeIdentity();

    // Information for translating and scaling
    long? touchId = null;
    SKPoint pressedLocation;
    SKMatrix pressedMatrix;

    // Information for scaling
    bool isScaling;
    SKPoint pivotPoint;
    ···

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                // Track only one finger
                if (touchId.HasValue)
                    return;

                // Check if the finger is within the boundaries of the bitmap
                SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
                rect = currentMatrix.MapRect(rect);
                if (!rect.Contains(point))
                    return;

                // First assume there will be no scaling
                isScaling = false;

                // If touch is outside interior ellipse, make this a scaling operation
                if (Math.Pow((point.X - rect.MidX) / (rect.Width / 2), 2) +
                    Math.Pow((point.Y - rect.MidY) / (rect.Height / 2), 2) > 1)
                {
                    isScaling = true;
                    float xPivot = point.X < rect.MidX ? rect.Right : rect.Left;
                    float yPivot = point.Y < rect.MidY ? rect.Bottom : rect.Top;
                    pivotPoint = new SKPoint(xPivot, yPivot);
                }

                // Common for either pan or scale
                touchId = args.Id;
                pressedLocation = point;
                pressedMatrix = currentMatrix;
                break;

            case TouchActionType.Moved:
                if (!touchId.HasValue || args.Id != touchId.Value)
                    return;

                SKMatrix matrix = SKMatrix.MakeIdentity();

                // Translating
                if (!isScaling)
                {
                    SKPoint delta = point - pressedLocation;
                    matrix = SKMatrix.MakeTranslation(delta.X, delta.Y);
                }
                // Scaling
                else
                {
                    float scaleX = (point.X - pivotPoint.X) / (pressedLocation.X - pivotPoint.X);
                    float scaleY = (point.Y - pivotPoint.Y) / (pressedLocation.Y - pivotPoint.Y);
                    matrix = SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y);
                }

                // Concatenate the matrices
                SKMatrix.PreConcat(ref matrix, pressedMatrix);
                currentMatrix = matrix;
                canvasView.InvalidateSurface();
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                touchId = null;
                break;
        }
    }
}
```

`Moved`작업 유형은 손가락이 화면을 누른 시간부터이 시간까지 터치 활동에 해당 하는 행렬을 계산 합니다. 이는 손가락이 손가락을 처음 눌렀을 때 적용 되는 행렬을 사용 하 여 행렬을 연결 합니다. 크기 조정 작업은 손가락이 손가락으로 작업 한 것과 반대 되는 모퉁이를 기준으로 합니다.

작은 비트맵 또는 oblong 비트맵의 경우 내부 타원이 대부분의 비트맵을 차지 하 고 작은 영역을 모퉁이에 남겨 두어 비트맵의 크기를 조정할 수 있습니다. 약간 다른 방법을 사용 하는 것 `if` 이 좋습니다 .이 경우를로 설정 하는 전체 블록 `isScaling` 을 `true` 이 코드로 바꿀 수 있습니다.

```csharp
float halfHeight = rect.Height / 2;
float halfWidth = rect.Width / 2;

// Top half of bitmap
if (point.Y < rect.MidY)
{
    float yRelative = (point.Y - rect.Top) / halfHeight;

    // Upper-left corner
    if (point.X < rect.MidX - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Bottom);
    }
    // Upper-right corner
    else if (point.X > rect.MidX + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Bottom);
    }
}
// Bottom half of bitmap
else
{
    float yRelative = (point.Y - rect.MidY) / halfHeight;

    // Lower-left corner
    if (point.X < rect.Left + yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Right, rect.Top);
    }
    // Lower-right corner
    else if (point.X > rect.Right - yRelative * halfWidth)
    {
        isScaling = true;
        pivotPoint = new SKPoint(rect.Left, rect.Top);
    }
}
```

이 코드는 비트맵의 면적을 내부 다이아몬드 모양으로 나누고 네 개의 삼각형을 모퉁이에 배치 합니다. 이렇게 하면 더 큰 영역에서 비트맵을 가져오고 크기를 조정할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [효과로부터 이벤트 호출](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
