---
title: Xamarin.ios의 핵심 이미지
description: 핵심 이미지는 이미지 처리와 라이브 비디오 기능 향상 기능을 제공 하기 위해 iOS 5와 함께 도입 된 새로운 프레임 워크입니다. 이 문서에서는 Xamarin.ios 샘플을 사용 하 여 이러한 기능을 소개 합니다.
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: a537926ab28bc355af5c5c4993ccff4a736b15aa
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70288569"
---
# <a name="core-image-in-xamarinios"></a>Xamarin.ios의 핵심 이미지

_핵심 이미지는 이미지 처리와 라이브 비디오 기능 향상 기능을 제공 하기 위해 iOS 5와 함께 도입 된 새로운 프레임 워크입니다. 이 문서에서는 Xamarin.ios 샘플을 사용 하 여 이러한 기능을 소개 합니다._

핵심 이미지는 얼굴 감지를 포함 하 여 이미지 및 비디오에 적용 되는 다양 한 기본 제공 필터와 효과를 제공 하는 iOS 5에 도입 된 새로운 프레임 워크입니다.

이 문서에는 다음과 같은 간단한 예가 포함 되어 있습니다.

- 얼굴 감지.
- 이미지에 필터 적용
- 사용 가능한 필터를 나열 합니다.


이러한 예제는 Xamarin.ios 응용 프로그램에 핵심 이미지 기능을 통합 하기 시작 하는 데 도움이 됩니다.

## <a name="requirements"></a>요구 사항

최신 버전의 Xcode를 사용 해야 합니다.

## <a name="face-detection"></a>얼굴 감지

핵심 이미지 얼굴 감지 기능은 그림에서 얼굴을 식별 하 고 인식 되는 얼굴의 좌표를 반환 하려고 시도 하는 것을 의미 합니다. 이 정보를 사용 하 여 이미지에 있는 사용자 수를 계산 하 고 이미지에 표시기를 그릴 수 있습니다. 예를 들면 사진의 ' 태깅 ' 사용자의 경우 또는 다른 모든 항목에 대해 생각해 볼 수 있습니다.

CoreImage\SampleCode.cs의이 코드는 포함 된 이미지에서 얼굴 감지를 만들고 사용 하는 방법을 보여 줍니다.

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

기능 배열이 검색 된 경우에는 `CIFaceFeature` 개체로 채워집니다. 각 면 `CIFaceFeature` 에 대 한가 있습니다. `CIFaceFeature`에는 다음과 같은 속성이 있습니다.

- HasMouthPosition –이 면에 대 한 입/군/시가 검색 되었는지 여부입니다.
- HasLeftEyePosition –이 면에 대해 왼쪽 눈이 감지 되었는지 여부입니다.
- HasRightEyePosition –이 얼굴에 대 한 올바른 눈이 감지 되었는지 여부입니다. 
- MouthPosition –이 면에 대 한 입/군/시의 좌표입니다.
- LeftEyePosition –이 면의 왼쪽 눈동자 좌표입니다.
- RightEyePosition –이 면의 올바른 눈동자 좌표입니다.


이러한 모든 속성의 좌표는 왼쪽 위를 원본으로 사용 하는 UIKit와는 달리 왼쪽 아래에 원점이 있습니다. 좌표를 사용 하는 `CIFaceFeature` 경우 해당 좌표를 ' 대칭 이동 ' 해야 합니다. CoreImage\CoreImageViewController.cs의이 매우 기본적인 사용자 지정 이미지 뷰는 이미지에 ' face 표시기 ' 삼각형을 그리는 방법을 보여 줍니다 ( `FlipForBottomOrigin` 메서드 참고).

```csharp
public class FaceDetectImageView : UIView
{
    public Xamarin.iOS.CoreImage.CIFeature[] Features;
    public UIImage Image;
    public FaceDetectImageView (RectangleF rect) : base(rect) {}
    CGPath path;
    public override void Draw (RectangleF rect) {
        base.Draw (rect);
        if (Image != null)
            Image.Draw(rect);

        using (var context = UIGraphics.GetCurrentContext()) {
            context.SetLineWidth(4);
            UIColor.Red.SetStroke ();
            UIColor.Clear.SetFill ();
            if (Features != null) {
                foreach (var feature in Features) { // for each face
                    var facefeature = (CIFaceFeature)feature;
                    path = new CGPath ();
                    path.AddLines(new PointF[]{ // assumes all 3 features found
                        FlipForBottomOrigin(facefeature.LeftEyePosition, 200),
                        FlipForBottomOrigin(facefeature.RightEyePosition, 200),
                        FlipForBottomOrigin(facefeature.MouthPosition, 200)
                    });
                    path.CloseSubpath();
                    context.AddPath(path);
                    context.DrawPath(CGPathDrawingMode.FillStroke);
                }
            }
        }
    }
    /// <summary>
    /// Face recognition coordinates have their origin in the bottom-left
    /// but we are drawing with the origin in the top-left, so "flip" the point
    /// </summary>
    PointF FlipForBottomOrigin (PointF point, int height)
    {
        return new PointF(point.X, height - point.Y);
    }
}
```

그런 다음 SampleCode.cs 파일에서 이미지 및 기능이 할당 되어 이미지를 다시 그립니다.

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

스크린 샷에서는 샘플 출력을 보여 줍니다. 검색 된 얼굴 기능의 위치는 UITextView에 표시 되 고 CoreGraphics를 사용 하 여 소스 이미지에 그려집니다.

얼굴 인식이 작동 하는 방식 때문에 가끔 인간 얼굴 외 (예: 이러한 장난감 monkeys!)를 검색 합니다.

## <a name="filters"></a>필터

50 개가 넘는 다른 기본 제공 필터가 있으며 프레임 워크를 확장 하 여 새 필터를 구현할 수 있습니다.

## <a name="using-filters"></a>필터 사용

이미지에 필터를 적용 하면 이미지를 로드 하 고, 필터를 만들고, 필터를 적용 하 고, 결과를 저장 하거나 표시 하는 4 가지 고유한 단계가 있습니다.

먼저 `CIImage` 개체에 이미지를 로드 합니다.

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

둘째, 필터 클래스를 만들고 해당 속성을 설정 합니다.

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

셋째, 속성에 `OutputImage` 액세스 하 고 메서드 `CreateCGImage` 를 호출 하 여 최종 결과를 렌더링 합니다.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

마지막으로 보기에 이미지를 할당 하 여 결과를 확인 합니다. 실제 응용 프로그램에서 결과 이미지는 filesystem, Photo 앨범, 트 윗 또는 email에 저장 될 수 있습니다.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

이러한 스크린샷은 CoreImage 샘플 코드에서 보여 `CISepia` 주는 `CIHueAdjust` 및 필터의 결과를 보여 줍니다.

`CIColorControls` 필터의 예제는 [이미지의 조정 및 밝기 조정 조리법](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) 을 참조 하세요.

```csharp
var uiimage = UIImage.FromFile("photo.JPG");
var ciimage = new CIImage(uiimage);
var hueAdjust = new CIHueAdjust();   // first filter
hueAdjust.Image = ciimage;
hueAdjust.Angle = 2.094f;
var sepia = new CISepiaTone();       // second filter
sepia.Image = hueAdjust.OutputImage; // output from last filter, input to this one
sepia.Intensity = 0.3f;
CIFilter color = new CIColorControls() { // third filter
    Saturation = 2,
    Brightness = 1,
    Contrast = 3,
    Image = sepia.OutputImage    // output from last filter, input to this one
};
var output = color.OutputImage;
var context = CIContext.FromOptions(null);
// ONLY when CreateCGImage is called do all the effects get rendered
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

```csharp
var context = CIContext.FromOptions (null);
```

```csharp
var context = CIContext.FromOptions(new CIContextOptions() {
    UseSoftwareRenderer = true  // CPU
});
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

### <a name="listing-filters-and-their-properties"></a>필터 및 해당 속성 나열

CoreImage\SampleCode.cs의이 코드는 기본 제공 필터 및 해당 매개 변수의 전체 목록을 출력 합니다.

```csharp
var filters = CIFilter.FilterNamesInCategories(new string[0]);
foreach (var filter in filters){
   display.Text += filter +"\n";
   var f = CIFilter.FromName (filter);
   foreach (var key in f.InputKeys){
     var attributes = (NSDictionary)f.Attributes[new NSString(key)];
     var attributeClass = attributes[new NSString("CIAttributeClass")];
     display.Text += "   " + key;
     display.Text += "   " + attributeClass + "\n";
   }
}
```

[Cifilter 클래스 참조](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) 는 50 기본 제공 필터 및 해당 속성을 설명 합니다. 위의 코드를 사용 하 여 매개 변수의 기본값 및 허용 되는 최대값 및 최소값 (필터를 적용 하기 전에 입력의 유효성을 검사 하는 데 사용 될 수 있음)을 포함 하 여 필터 클래스를 쿼리할 수 있습니다.

목록 범주 출력은 시뮬레이터에서 다음과 같이 표시 됩니다. 목록을 스크롤하여 모든 필터 및 해당 매개 변수를 볼 수 있습니다.

 [![](introduction-to-coreimage-images/coreimage05.png "목록 범주 출력은 시뮬레이터에서 다음과 같이 표시 됩니다.")](introduction-to-coreimage-images/coreimage05.png#lightbox)

나열 된 각 필터는 Xamarin.ios에서 클래스로 노출 되었으므로 어셈블리 브라우저에서 CoreImage API를 탐색 하거나 Mac용 Visual Studio 또는 Visual Studio에서 자동 완성 기능을 사용할 수도 있습니다. 

## <a name="summary"></a>요약

이 문서에서는 얼굴 감지와 같은 새로운 iOS 5 핵심 이미지 프레임 워크 기능 중 일부를 사용 하 고 이미지에 필터를 적용 하는 방법을 보여 줍니다. 프레임 워크에서 사용할 수 있는 수십 가지 이미지 필터가 있습니다.

## <a name="related-links"></a>관련 링크

- [핵심 이미지 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/coreimage)
- [이미지 조리법의 계약 및 밝기 조정](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [핵심 이미지 필터 사용](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [CIFilter 클래스 참조](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
