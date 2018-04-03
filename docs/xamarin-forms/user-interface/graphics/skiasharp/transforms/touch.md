---
title: 터치 조작
description: 터치 끌어, 모으는, 및 회전을 구현 하 사용 하 여 매트릭스 변환
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: 1fbc9826b9edd3d4c8f7e4b47c3ea835d5625343
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2018
---
# <a name="touch-manipulations"></a>터치 조작

_터치 끌어, 모으는, 및 회전을 구현 하 사용 하 여 매트릭스 변환_

모바일 장치에서와 같이 멀티 터치 환경에서는 사용자 화면에 개체를 조작 손가락 자주 사용 합니다. 한 손가락 끌어서 두 손가락 축소 같은 일반적인 제스처 이동 하 고 개체의 크기를 조정 하거나도 회전 수 있습니다. 이러한 제스처는 변환 행렬을 사용 하 여 일반적으로 구현 하 고이 문서에서는 그렇게 하는 방법을 보여 줍니다.

![](touch-images/touchmanipulationsexample.png "비트맵 변환, 크기 조정 및 회전의 대상")

## <a name="manipulating-one-bitmap"></a>한 비트맵을 조작

**터치 조작** 페이지에는 하나의 비트맵에 터치 조작을 보여 줍니다.
이 예제를 사용 하는 문서에 제공 된 터치 추적 효과 [호출 이벤트 효과를](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)합니다.

다른 여러 개의 파일에 대 한 지원을 제공는 **터치 조작** 페이지. 첫 번째는는 [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) 터치 조작 표시 될 수 있습니다는 코드에서 구현 된 다양 한 유형의 나타내는 열거형.

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

`PanOnly` 변환으로 구현 된 한 손가락 끌어서가입니다. 이후의 모든 옵션도 패닝 포함 되지만 두 손가락 포함: `IsotropicScale` 배율 동일 하 게 가로 및 세로 방향에 개체에서 발생 하는 축소 작업. `AnisotropicScale` 동일 하지 않음을 크기 조정을 허용 합니다.

`ScaleRotate` 옵션은 두 손가락 크기 조정 및 회전 합니다. 크기 조정 하는 것은 등방성입니다. 손가락 이동은 기본적으로 동일 하므로 두 손가락 회전으로 이방성 확장을 구현 하는 것은 문제가 있습니다.

`ScaleDualRotate` 옵션 한 손가락 회전을 추가 합니다. 한 손가락 끌면 개체를 끌어 온된 개체 먼저 회전의 중심을 기준 개체의 중심 끌기 벡터와 일치 되도록 합니다.

[ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) 파일에 포함 되어는 `Picker` 의 구성원과 `TouchManipulationMode` 열거형:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
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
            <Picker.Items>
                <x:String>None</x:String>
                <x:String>PanOnly</x:String>
                <x:String>IsotropicScale</x:String>
                <x:String>AnisotropicScale</x:String>
                <x:String>ScaleRotate</x:String>
                <x:String>ScaleDualRotate</x:String>
            </Picker.Items>
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

아래쪽으로 이동 하는 한 `SKCanvasView` 및 `TouchEffect` 단일 셀에 연결 된 `Grid` 포함 하 합니다.

[ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) 코드 숨김 파일에는 `bitmap` 필드 하지 않은 형식의 `SKBitmap`합니다. 형식이 `TouchManipulationBitmap` (곧 알게 되는 클래스).

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            SKBitmap bitmap = SKBitmap.Decode(skStream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

생성자를 인스턴스화하는 `TouchManipulationBitmap` 생성자에 전달 하는 개체는 `SKBitmap` 포함된 리소스에서 가져옵니다. 생성자를 설정 하 여 결론을 내립니다는 `Mode` 속성의는 `TouchManager` 속성은 `TouchManipulationBitmap` 개체의 멤버에는 `TouchManipulationMode` 열거형입니다.

`SelectedIndexChanged` 에 대 한 처리기는 `Picker` 도 설정 하는이 `Mode` 속성:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    void OnTouchModePickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (bitmap != null)
        {
            Picker picker = (Picker)sender;
            TouchManipulationMode mode;
            Enum.TryParse(picker.Items[picker.SelectedIndex], out mode);
            bitmap.TouchManager.Mode = mode;
        }
    }
    ...
}
```

`TouchAction` 의 처리기는 `TouchEffect` 에서 두 개의 메서드를 호출 하 여 XAML 파일에서에서 인스턴스화할 `TouchManipulationBitmap` 라는 `HitTest` 및 `ProcessTouchEvent`:

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

경우는 `HitTest` 메서드 반환 `true` &mdash; 손가락 비트맵을 차지한 영역이 내에서 화면을 touched가 의미 &mdash; 터치 ID에 추가 되는 `TouchIds` 컬렉션입니다. 이 ID는 화면에서 손가락 들어 올릴 때까지 해당 지문에 대 한 터치 이벤트 순서를 나타냅니다. 여러 손가락 터치 비트맵을 하면 `touchIds` 컬렉션에 대 한 각 손가락 터치 ID에 포함 합니다.

`TouchAction` 처리기도 호출는 `ProcessTouchEvent` 클래스 `TouchManipulationBitmap`합니다. 여기에 일부 (모두는 아니지만)의 실제 터치 처리가 발생 합니다.

[ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) 클래스에 대 한 래퍼 클래스는 `SKBitmap` 비트맵을 렌더링 하 고 터치 이벤트를 처리 하는 코드가 들어 있는입니다. 자세히와 함께에서 작동의 코드를 일반화 한 `TouchManipulationManager` 클래스 (곧 표시 됩니다).

`TouchManipulationBitmap` 생성자 저장은 `SKBitmap` 하 고 두 속성을 인스턴스화합니다는 `TouchManager` 형식의 속성이 `TouchManipulationManager` 및 `Matrix` 형식의 속성이 `SKMatrix`:

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

이 `Matrix` 속성은 누적 된 변환으로 인해 모든 터치 활동에서 발생 합니다. 연결 된 다음 행렬에 각 터치 이벤트 해결 되었는지 확인할 수는 `SKMatrix` 하 여 저장 된 값은 `Matrix` 속성입니다.

`TouchManipulationBitmap` 개체 자신을 그릴 해당 `Paint` 메서드. 인수는는 `SKCanvas` 개체입니다. 이 `SKCanvas` 를 적용 하는 변환을 이미 있을 수도 있습니다 하므로 `Paint` 연결 하는 메서드는 `Matrix` 비트맵을 기존 변환와 관련 된 속성과 캔버스를 마쳤을 때 복원:

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

`HitTest` 메서드 반환 `true` 사용자 비트맵의 지점 경계 내에서 화면을 터치 하는 경우. 비트맵을 조작 하는 사용자, 비트맵 회전할 수 나 도형 평행 사변형의 수 (통해서도 이방성 배율 조정 및 회전의 조합). 두려워 수는 `HitTest` 메서드 경우 다소 복잡 한 분석 geometry를 구현 해야 합니다.

그러나 바로 가기를 사용할 수 있는 것입니다.

점이 변환된 사각형의 경계 내에 있는지를 결정 하는 역방향 변환된 점이 되었지만 변형 되지 사각형의 경계 내에 있는지를 결정 하와 같습니다. 보다 쉽게 계산 하 고 편리한를 사용할 수 있습니다 `Contains` 정의한 메서드 `SKRect`:

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

두 번째 공용 메서드에 `TouchManipulationBitmap` 은 `ProcessTouchEvent`합니다. 이 메서드를 호출할 때 권한이 이미 설정 된 터치 이벤트는이 비트맵에 속해 있습니다. 메서드를 유지 관리의 사전 [ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) 개체를 이전 시점 단순히 및 각 손가락 새 지점:

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

다음은 사전 및 `ProcessTouchEvent` 메서드 자체:

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

에 `Moved` 및 `Released` 이벤트, 메서드 호출 `Manipulate`합니다. 다음이 시간에는 `touchDictionary` 하나 이상 포함 `TouchManipulationInfo` 개체입니다. 경우는 `touchDictionary` 하나가 포함 된 항목 될 하는 `PreviousPoint` 및 `NewPoint` 값 다르고 손가락의 움직임을 나타냅니다. 여러 손가락 비트맵을 터치 하, 사전에 둘 이상의 항목이 포함 되어 있지만 이러한 항목 중 하나만 서로 다른 경우 `PreviousPoint` 및 `NewPoint` 값입니다. 나머지 모든가 같은 `PreviousPoint` 및 `NewPoint` 값입니다.

이 중요:는 `Manipulate` 메서드 하나만 손가락 이동을 처리 하 가정할 수 있습니다. 이 호출 시 다른 손가락의 none 이동은 이후 호출에서 해당 이동을 처리은 실제로 이동 (그대로 가능성이), `Manipulate`합니다.

`Manipulate` 메서드는 첫 번째 사전의 편의 위해 배열에 복사 합니다. 처음 두 항목 이외의 값을 무시 합니다. 두 개 이상의 손가락 고 비트맵을 조작 하려고, 다른 항목은 무시 됩니다. `Manipulate` 최종 멤버인 `TouchManipulationBitmap`:

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

한 손가락 비트맵을 조작 하는 경우 `Manipulate` 호출은 `OneFingerManipulate` 의 메서드는 `TouchManipulationManager` 개체입니다. 두 손가락에 대 한 호출 `TwoFingerManipulate`합니다. 이러한 방법에 대 한 인수는 동일한:는 `prevPoint` 및 `newPoint` 인수 이동 하는 손가락을 나타냅니다. 하지만 `pivotPoint` 인수는 다른 두 번의 호출:

한 손가락 조작의 경우는 `pivotPoint` 비트맵 중심입니다. 한 손가락 회전에 대 한 허용 하도록입니다. 이벤트를 두 손가락 조작에 대 한 하나의 손가락 움직임을 나타냅니다 있도록는 `pivotPoint` 이동 하지 않는 손가락 됩니다.

두 경우 모두 `TouchManipulationManager` 반환는 `SKMatrix` 현재의 메서드에 연결 하는 값 `Matrix` 속성은 `TouchManipulationPage` 비트맵 렌더링을 사용 하 여 합니다.

`TouchManipulationManager` 제외한 다른 모든 파일을 사용 하 여 일반화 된 `TouchManipulationMode`합니다. 응용 프로그램에서 변경 없이이 클래스를 사용할 수 있습니다. 형식의 단일 속성 정의 `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


그러나 해도 좋을 것 방지 하기 위해는 `AnisotropicScale` 옵션입니다. 배율 인수 중 하나는 0이 됩니다. 하 고 비트맵을 조작 하려면이 옵션으로 매우 쉽습니다. 지도록 비트맵 돌아가려면 하지 못에서 사라집니다. 이방성 크기 조정 기능이 더 필요 않은 경우 원하지 않는 결과 방지 하기 위해 논리를 강화 합니다.

`TouchManipulationManager` 이기 때문 이지만 벡터를 사용 하지 `SKVector` SkiaSharp, 구조 `SKPoint` 대신 사용 됩니다. `SKPoint` 지원 벡터도 빼기 연산자와 결과 처리할 수 있습니다. 추가 하는 데 필요한 벡터 프로그램별 논리는는 `Magnitude` 계산:

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

회전을 선택 될 때마다 한 손가락 및 두 손가락 조작 방법은 모두 먼저 회전을 처리 합니다. 회전 하지 감지 되 면 회전 구성 요소 효과적으로 제거 됩니다. 남은 것은 이동 및 크기 조정으로 해석 됩니다.

다음은 `OneFingerManipulate` 메서드. 한 손가락 회전 설정 되어 있지 않은 경우 논리는 간단한 &mdash; 단순히 라는 벡터를 생성 하는 이전 점과 새 점을 사용 `delta` 정확 하 게 번역에 해당 하는 합니다. 한 손가락 회전을 사용 하도록 설정, 메서드를 사용 하 여 각도 피벗 지점 (비트맵의 센터)에서 이전 지점 및 새 지점을 회전 매트릭스 구성:

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

에 `TwoFingerManipulate` 메서드를 피벗 지점은이 특정 터치 이벤트에서 이동 하지 않는 손가락의 위치입니다. 회전은 한 손가락 회전와 매우 유사 하 고 그 다음 벡터 명명 된 `oldVector` (이전 시점에 기반)는 회전에 대 한 조정 됩니다. 나머지 이동 확장으로 해석 됩니다.

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

이 방법에서는 명시적 변환이 없습니다 것을 알 수 있습니다. 그러나 두는 `MakeRotation` 및 `MakeScale` 메서드 피벗 지점에 기반 하며 암시적 변환이 포함 된 합니다. 비트맵과 같은 방향에 끌어에 두 손가락을 사용 중인 경우 `TouchManipulation` 일련의 두 손가락을 교대로 터치 이벤트를 발생 합니다. 각 손가락 다른 크기 조정 또는 회전 결과 기준으로 하지만 다른 손가락 움직이면 부정 이동한 결과 변환 합니다.

나머지 부분에서 **터치 조작** 페이지는는 `PaintSurface` 의 처리기는 `TouchManipulationPage` 코드 숨김 파일입니다. 이 호출의 `Paint` 의 메서드는 `TouchManipulationBitmap`, 적용 되는 누적된 터치 활동을 나타내는 매트릭스:

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

`PaintSurface` 처리기가 표시 하 여 종료는 `MatrixDisplay` 누적된 터치 매트릭스를 보여 주는 개체:

[![](touch-images/touchmanipulation-small.png "터치 조작을 페이지의 삼중 스크린샷")](touch-images/touchmanipulation-large.png#lightbox "터치 조작 페이지의 삼중 스크린샷")

## <a name="manipulating-multiple-bitmaps"></a>여러 비트맵을 조작

와 같은 클래스에 터치 처리 코드를 격리의 장점 중 하나 `TouchManipulationBitmap` 및 `TouchManipulationManager` 여러 비트맵을 조작할 수 있도록 하는 프로그램에서 이러한 클래스를 다시 사용 하는 기능입니다.

**비트맵 분산형 보기** 페이지 이것은 수행 하는 방법을 보여 줍니다. 형식의 필드를 정의 하는 대신 `TouchManipulationBitmap`, [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) 클래스 정의 `List` 비트맵 개체:

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
                using (SKManagedStream skStream = new SKManagedStream(stream))
                {
                    SKBitmap bitmap = SKBitmap.Decode(skStream);
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

생성자는 모든 페이지의 포함 리소스로 사용할 수 있는 비트맵에서 로드 하 고에 추가 된 `bitmapCollection`합니다. 에 `Matrix` 각 속성은 초기화 `TouchManipulationBitmap` 100 픽셀로 오프셋 하는 각 비트맵의 왼쪽 위 모서리 개체입니다.

`BitmapScatterView` 페이지도 여러 비트맵에 대 한 터치 이벤트를 처리 해야 합니다. 정의 하는 대신 한 `List` 터치의 Id를 현재 조작 `TouchManipulationBitmap` 개체의 경우이 프로그램에는 사전 필요:

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

알림 방법을 `Pressed` 논리 반복는 `bitmapCollection` 역순에서입니다. 비트맵의 서로 겹칩니다. 컬렉션의 뒷부분에 나오는 비트맵 시각적으로 컬렉션의 앞부분에 나오는 비트맵 맨 위에 배치 합니다. 여러 비트맵이 화면에서 손가락 아래에 있는 경우 최상위 개체는 해당 손가락으로 조작 하는 것 이어야 합니다.

또한는 `Pressed` 논리 시각적으로 다른 비트맵 더미의 맨 위로 이동 하는 컬렉션의 끝에 해당 비트맵을 이동 합니다.

에 `Moved` 및 `Released` 이벤트는 `TouchAction` 처리기 호출의 `ProcessingTouchEvent` 에서 메서드 `TouchManipulationBitmap` 이전 프로그램 동일 하 게 합니다.

마지막으로 `PaintSurface` 처리기 호출의 `Paint` 각 메서드 `TouchManipulationBitmap` 개체:

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

코드는 컬렉션을 반복 하 고 컬렉션의 처음부터 끝에 비트맵의 누적을 표시 합니다.

[![](touch-images/bitmapscatterview-small.png "비트맵 분산형 보기 페이지의 삼중 스크린샷")](touch-images/bitmapscatterview-large.png#lightbox "비트맵 분산형 보기 페이지의 삼중 스크린 샷")


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [효과의 이벤트를 호출합니다.](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
