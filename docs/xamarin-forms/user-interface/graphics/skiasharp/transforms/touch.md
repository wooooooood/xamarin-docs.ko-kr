---
title: 터치 조작
description: 이 문서는 터치, 펴서를 끌어서 회전 구현 매트릭스 변환을 사용 하는 방법에 설명 하 고 샘플 코드를 사용 하 여이 보여 줍니다.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: charlespetzold
ms.author: chape
ms.date: 04/03/2018
ms.openlocfilehash: 2de5b9a3a6bf0d36330212a52ba5c7278b970efc
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130909"
---
# <a name="touch-manipulations"></a>터치 조작

_터치, 펴서를 끌어서 회전 구현 사용 하 여 행렬 변환_

모바일 장치에서 같은 다중 터치 환경에서는 사용자가 화면에서 개체를 조작 손가락이 종종 사용 합니다. 한 손가락 끌어서 두 손가락 축소와 같은 일반적인 제스처 이동 및 개체 크기를 조정 하거나 회전도 수 있습니다. 이러한 제스처는 일반적으로 변환 행렬을 사용 하 여 구현 및이 문서에서는 작업을 수행 하는 방법을 보여 줍니다.

![](touch-images/touchmanipulationsexample.png "변환, 배율 및 회전을 적용 하는 비트맵")

## <a name="manipulating-one-bitmap"></a>하나의 비트맵을 조작

합니다 **터치 조작** 페이지에는 단일 비트맵 터치 조작을 보여 줍니다.
이 샘플에서는 문서에 제공 된 터치 추적 효과 활용 [효과의 이벤트를 호출](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)합니다.

다른 여러 파일에 대 한 지원을 제공 합니다 **터치 조작** 페이지입니다. 첫 번째는 [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) 보이지에서는 코드에서 구현 하는 터치 조작 유형을 나타내는 열거형.

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

`PanOnly` 변환으로 구현 된 한 손가락 끌기가입니다. 모든 후속 옵션 또한 이동을 포함 하지만 두 손가락으로 포함: `IsotropicScale` 는 축소 작업 가로 및 세로 방향으로 동일 하 게 확장 개체의 결과입니다. `AnisotropicScale` 서로 다른 크기 조정 수 있습니다.

`ScaleRotate` 옵션은 두 손가락 크기 조정 및 회전 합니다. 크기 조정은 등방성입니다. 손가락 이동은 기본적으로 동일 하기 때문에 두 손가락 회전 이방성 확장을 구현 하는 것은 문제가 있습니다.

`ScaleDualRotate` 옵션은 한 손가락의 회전을 추가 합니다. 손가락 개체 끌면 끌어 온된 개체 먼저 회전 중심 끌기 벡터를 사용 하 여 개체의 가운데 정렬 되도록 합니다.

합니다 [ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) 파일에 포함는 `Picker` 의 멤버를 사용 하 여는 `TouchManipulationMode` 열거형:

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

아래쪽에는 `SKCanvasView` 와 `TouchEffect` 단일 셀에 연결 된 `Grid` 포함 하는 것입니다.

합니다 [ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) 코드 숨김 파일에는 `bitmap` 필드 하지 않은 형식의 `SKBitmap`합니다. 형식은 `TouchManipulationBitmap` (곧 확인 하겠지만 클래스).

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

생성자를 인스턴스화하는 `TouchManipulationBitmap` 개체를 생성자에 전달는 `SKBitmap` 포함된 리소스에서 가져옵니다. 생성자를 설정 하 여 완료를 `Mode` 의 속성을 `TouchManager` 의 속성을 `TouchManipulationBitmap` 개체의 멤버에는 `TouchManipulationMode` 열거형.

합니다 `SelectedIndexChanged` 에 대 한 처리기는 `Picker` 작업도이 `Mode` 속성:

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

`TouchAction` 처리기는 `TouchEffect` 의 두 메서드를 호출 하 여 XAML 파일에서에서 인스턴스화됩니다 `TouchManipulationBitmap` 라는 `HitTest` 고 `ProcessTouchEvent`:

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

경우는 `HitTest` 메서드가 반환 `true` &mdash; 손가락 비트맵을 차지 하는 영역 내에서 화면을 작업에 의미 &mdash; 터치 ID에 추가 되는 `TouchIds` 컬렉션입니다. 이 ID는 화면에서가 손가락을 뗄 때까지 해당 손가락 터치 이벤트 시퀀스를 나타냅니다. 여러 손가락 터치 비트맵을 해당 `touchIds` 각 손가락 마다 touch ID를 포함 하는 컬렉션입니다.

합니다 `TouchAction` 처리기도 호출 합니다 `ProcessTouchEvent` 클래스의 `TouchManipulationBitmap`합니다. 이 경우 일부 (전부가 아님)의 실제 터치 처리가 발생 합니다.

합니다 [ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) 클래스에 대 한 래퍼 클래스는 `SKBitmap` 비트맵을 렌더링 하 고 터치 이벤트를 처리 하는 코드를 포함 하는 합니다. 자세히와 함께에서 작동에 코드를 일반화를 `TouchManipulationManager` 클래스 (곧 확인 하겠지만).

`TouchManipulationBitmap` 생성자 저장 합니다 `SKBitmap` 하 고 두 개의 속성을 인스턴스화합니다 합니다 `TouchManager` 형식의 속성 `TouchManipulationManager` 및 `Matrix` 형식의 속성 `SKMatrix`:

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

이 `Matrix` 속성은 모든 터치 활동부터 결과 누적된 변환 합니다. 각 터치 이벤트에 연결 된 후 행렬으로 해결 될 볼 수 있듯이 합니다 `SKMatrix` 하 여 저장 된 값을 `Matrix` 속성.

합니다 `TouchManipulationBitmap` 개체 자신을 그릴 해당 `Paint` 메서드. 인수가 `SKCanvas` 개체입니다. 이 `SKCanvas` 변환을 적용 되었을 수 있으므로 `Paint` 메서드 연결는 `Matrix` 속성 연결 된 기존 변환, 비트맵 및 캔버스를 마쳤을 때 복원:

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

합니다 `HitTest` 메서드가 반환 되는 `true` 사용자 비트맵의 지점 경계 내에서 화면을 터치 하는 경우. 비트맵을 조작 하는 사용자, 비트맵, 회전할 수 있습니다 또는 셰이프에 평행 사변형의 수 (통해서도 이방성 크기 조정 및 회전의 조합). 간단한 수는 `HitTest` 메서드 경우 다소 복잡 한 분석 geometry를 구현 해야 합니다.

그러나 바로 가기는 사용할 수 있습니다.

점이 변환된 사각형의 경계 내에 있는지를 결정 하는 역 변환 된 점 변환 되지 않은 사각형의 경계 내에 있는지를 결정 하는와 같습니다. 훨씬 더 쉽게 계산 이며를 편리 하 게 사용할 수 있습니다 `Contains` 메서드를 정의한 `SKRect`:

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

두 번째 공용 메서드도 `TouchManipulationBitmap` 는 `ProcessTouchEvent`합니다. 이 메서드를 호출 하는 경우이 이미 설정 된이 비트맵 속한 터치 이벤트입니다. 메서드를 유지 관리의 사전 [ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) 단순히 이전 시점 및 새 지점의 각 손가락 개체:

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

에 `Moved` 하 고 `Released` 이벤트, 메서드 호출 `Manipulate`합니다. 다음이 시간에는 `touchDictionary` 하나 이상 포함 `TouchManipulationInfo` 개체입니다. 경우는 `touchDictionary` 하나가 포함 되어 있습니다 항목인 것입니다는 `PreviousPoint` 및 `NewPoint` 값 같지 않으며 손가락의 움직임을 나타냅니다. 여러 손가락 비트맵을 터치 하, 사전에 항목이 둘 이상 포함 되어 있지만 이러한 항목 중 하나만 다른 `PreviousPoint` 고 `NewPoint` 값입니다. 모든 rest가 같으면 `PreviousPoint` 고 `NewPoint` 값입니다.

이 중요:는 `Manipulate` 메서드 하나만 손가락의 움직임 처리 하는 고 가정할 수 있습니다. 이 호출 시 다른 손가락 모두가 이동 하 고는 실제로 이동 (가능성이)으로, 이러한 움직임에 대 한 이후 호출에서 처리 됩니다 `Manipulate`합니다.

`Manipulate` 메서드는 먼저 사전 편의 위해 배열에 복사 합니다. 처음 두 항목은 이외의 무시 됩니다. 다른 두 개 이상의 손가락 비트맵을 조작 하려고, 무시 됩니다. `Manipulate` 최종 멤버의 `TouchManipulationBitmap`:

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

한 손가락 비트맵을 조작 하는 경우 `Manipulate` 호출을 `OneFingerManipulate` 메서드를 `TouchManipulationManager` 개체입니다. 두 손가락에 대 한 호출 `TwoFingerManipulate`합니다. 이러한 메서드를 인수는 동일: 합니다 `prevPoint` 및 `newPoint` 인수를 이동 하는 손가락을 나타냅니다. 하지만 `pivotPoint` 인수가 두 번의 호출 다릅니다.

한 손가락 조작의 경우는 `pivotPoint` 중앙 비트맵입니다. 이 한 손가락의 회전을 허용 하는 것입니다. 이벤트를 두 손가락 조작에 대 한 하나의 손가락의 움직임을 나타냅니다. 있도록는 `pivotPoint` 손가락 이동 하지 않는 됩니다.

두 경우 모두 `TouchManipulationManager` 반환을 `SKMatrix` 현재 메서드를 연결 하는 값을 `Matrix` 속성은 `TouchManipulationPage` 비트맵을 렌더링 하기 위해 사용 합니다.

`TouchManipulationManager` 제외 하 고 다른 모든 파일을 사용 하 여 일반화 `TouchManipulationMode`합니다. 응용 프로그램에서 변경 없이이 클래스를 사용할 수 있습니다. 형식의 단일 속성 정의 `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


그러나 원할 것을 방지 하는 `AnisotropicScale` 옵션입니다. 0 요소 중 하나는 크기 조정 되도록 비트맵을 조작 하려면이 옵션을 사용 하 여 매우 쉽습니다. 비트맵을에서 반환 하는 적용 되지 않고 sight 사라질 수 있습니다. 실제로 필요가 없는 경우 이방성 크기 조정, 바람직하지 않은 결과 방지 하려면 논리를 강화 합니다.

`TouchManipulationManager` 벡터의 되지만 있기 때문에 사용 되지 않습니다 `SKVector` SkiaSharp, 구조 `SKPoint` 대신 사용 됩니다. `SKPoint` 지원 벡터도 빼기 연산자 및 결과 처리할 수 있습니다. 추가 하는 데 필요한 벡터 별 논리를 `Magnitude` 계산:

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

회전을 선택한 후 때마다 한 손가락 및 두 손가락 조작 방법은 모두 먼저 회전을 처리 합니다. 모든 회전 감지 되 면 회전 구성 요소 효과적으로 제거 됩니다. 남은 이동 및 크기 조정으로 해석 됩니다.

다음은 `OneFingerManipulate` 메서드. 한 손가락의 회전 설정 되지 않은 경우 논리는 간단한 &mdash; 단순히 사용 하 여 이전 시점 및 새 지점 라는 벡터를 생성 하려면 `delta` 정확 하 게 번역에 해당 하는 합니다. 사용 하도록 설정 하는 한 손가락 회전을 사용 하 여 메서드를 사용 하 여 각도 피벗 지점 (비트맵의 센터)에서 새 지점과 이전 시점 회전 행렬을 생성 하려면:

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

에 `TwoFingerManipulate` 메서드를 피벗 점은이 특정 터치 이벤트에서 이동 하지 않는 손가락의 위치입니다. 회전은 한 손가락 회전을 매우 유사 하며 벡터 명명할 `oldVector` (이전 시점에 따라) 회전 조정 됩니다. 나머지 이동 크기를 조정으로 해석 됩니다.

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

이 메서드에서 명시적 변환이 알 수 있습니다. 그러나 두 합니다 `MakeRotation` 및 `MakeScale` 메서드 피벗 점의 기반한 및 암시적 변환을 포함 하는 합니다. 비트맵 및 동일한 방향으로 끌어 두 손가락을 사용 하는 경우 `TouchManipulation` 일련의 돌려 두 손가락 사이의 교대로 반복 되는 터치 이벤트를 받게 됩니다. 다른 크기 조정 또는 회전 결과 기준으로 각 손가락 이동 하지만 다른 손가락 이동 하 여 부정 됩니다 하 고 결과 변환입니다.

나머지 부분을 **터치 조작** 페이지를 `PaintSurface` 처리기에서는 `TouchManipulationPage` 코드 숨김 파일. 이 호출을 `Paint` 메서드는 `TouchManipulationBitmap`, 누적된 터치 작업을 나타내는 매트릭스를 적용 되는:

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

합니다 `PaintSurface` 처리기가 표시 하 여 종료를 `MatrixDisplay` 터치 누적 된 매트릭스를 보여 주는 개체:

[![](touch-images/touchmanipulation-small.png "터치 조작 페이지 스크린샷 삼중")](touch-images/touchmanipulation-large.png#lightbox "삼중 터치 조작 페이지 스크린샷")

## <a name="manipulating-multiple-bitmaps"></a>여러 비트맵을 조작

클래스에서 터치 처리 코드와 같은 격리의 장점 중 하나 `TouchManipulationBitmap` 고 `TouchManipulationManager` 여러 비트맵을 조작할 수 있도록 하는 프로그램에서 이러한 클래스를 다시 사용 하는 기능입니다.

합니다 **비트맵 분산형 보기** 페이지 이렇게 하는 방법을 보여 줍니다. 형식의 필드를 정의 하는 대신 `TouchManipulationBitmap`서 [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) 클래스 정의 `List` 비트맵 개체:

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

생성자에 추가 합니다 포함 리소스로 사용할 수 있는 비트맵의 모든 로드는 `bitmapCollection`합니다. 있음을 합니다 `Matrix` 각 속성은 초기화 `TouchManipulationBitmap` 개체 이므로 각 비트맵의 왼쪽 위 모퉁이 100 픽셀로 오프셋 됩니다.

`BitmapScatterView` 페이지는 또한 여러 비트맵에 대 한 터치 이벤트를 처리 해야 합니다. 정의 하는 대신를 `List` 터치 Id의 현재 조작 `TouchManipulationBitmap` 개체에이 프로그램을 사전에 필요 합니다.

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

알림 방법을 `Pressed` 을 반복 하는 논리는 `bitmapCollection` 역순에서입니다. 비트맵의 서로 겹칩니다. 컬렉션의 뒷부분에 나오는 비트맵 컬렉션의 앞부분에서 비트맵을 기반으로 시각적으로 배치 합니다. 화면에서를 누르는 손가락 아래에 있는 여러 비트맵의 경우 최상위 개체는 손가락으로 조작 되는 것 이어야 합니다.

또한는 `Pressed` 논리 다른 비트맵의 누적의 맨 위에 시각적으로 이동할 수 있도록 컬렉션의 끝에 해당 비트맵을 이동 합니다.

에 `Moved` 및 `Released` 이벤트를 `TouchAction` 처리기 호출을 `ProcessingTouchEvent` 에서 메서드 `TouchManipulationBitmap` 마찬가지로 이전 프로그램입니다.

마지막으로, 합니다 `PaintSurface` 처리기 호출을 `Paint` 메서드의 각 `TouchManipulationBitmap` 개체:

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

[![](touch-images/bitmapscatterview-small.png "비트맵 분산형 보기 페이지의 3 배가 스크린 샷")](touch-images/bitmapscatterview-large.png#lightbox "삼중 비트맵 분산형 보기 페이지 스크린샷")

## <a name="single-finger-scaling"></a>한 손가락 크기 조정

크기 조정 작업에는 일반적으로 두 손가락을 사용한 축소 제스처에 필요 합니다. 그러나 손가락 비트맵의 모퉁이 이동 하 여 손가락을 사용 하 여 크기 조정을 구현 하는 것이 같습니다.

에 설명 되어이 **단일 손가락 모퉁이 확장** 페이지입니다. 이 샘플에는에서 구현 하는 크기 조정 약간 다른 형식을 사용 하기 때문에 합니다 `TouchManipulationManager` 클래스는 클래스를 사용 하지 않습니다 또는 `TouchManipulationBitmap` 클래스입니다. 대신 모든 터치 논리의 코드 숨김 파일. 한 번에 하나의 손가락을 추적 하 고 화면을 터치 수 있는 모든 보조 손가락을 무시 하므로 이것이 평소 보다 좀 더 간단 논리입니다.

합니다 [ **SingleFingerCornerScale.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml) 페이지를 인스턴스화하는 `SKCanvasView` 만들고 클래스를 `TouchEffect` 터치 이벤트를 추적 하는 것에 대 한 개체:

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

합니다 [ **SingleFingerCornerScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/SingleFingerCornerScalePage.xaml.cs) 파일에서 비트맵 리소스를 로드 합니다 **미디어** 디렉터리 사용 하 여 표시를 `SKMatrix` 으로 정의 된 개체를 필드:

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

이 `SKMatrix` 개체 아래에 표시 된 터치 논리에 의해 수정 됩니다.

코드 숨김 파일의 나머지 부분은 `TouchEffect` 이벤트 처리기입니다. 손가락으로의 현재 위치를 변환 하 여 시작을 `SKPoint` 값입니다. 에 대 한는 `Pressed` 없는 다른 손가락 화면에 접촉 되어 있는 작업 유형 처리기를 확인 및 손가락 비트맵의 범위 내에 있는 합니다.

코드의 중요 한 부분은 `if` 를 두 번 호출 문에서 `Math.Pow` 메서드. 이 수학 비트맵을 채우는 타원 외부 손가락 위치 인지 확인 합니다. 따라서 하는 경우 크기 조정 작업을 합니다. 비트맵의 모서리 중 하나의 근처 손가락 이며 반대쪽 모퉁이 피벗 점을 결정 됩니다. 이 타원 안에서 손가락을 인지 일반 이동 작업을 합니다.

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

`Moved` 손가락이 이번까지 화면을 누를 때부터 touch 작업에 해당 하는 행렬을 계산 하는 작업 유형입니다. 손가락 비트맵을 처음 누를 때 행렬을 사용 하 여 해당 매트릭스를 적용 연결 합니다. 크기 조정 작업은 항상 손가락 터치는는 달리 모퉁이 기준으로 합니다.

소규모 또는 장방형 비트맵에 대 한 내부 타원 비트맵의 대부분을 차지 하 고 매우 작은 영역 크기를 조정할 비트맵 모퉁이에 그대로 수 있습니다. 약간 다른 방법을 선호할이 경우에 전체를 대체할 수 있습니다 `if` 설정 하는 블록 `isScaling` 에 `true` 이 코드를 사용 하 여:

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

이 코드는 내부 다이아몬드 모양으로 비트맵의 영역 및 모서리에 있는 삼각형 네 개에 효과적으로 나눕니다. 이렇게 하면 훨씬 더 큰 영역을 잡고 비트맵 크기를 조정 하려면 모퉁이에 있습니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [효과의 이벤트를 호출합니다.](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
