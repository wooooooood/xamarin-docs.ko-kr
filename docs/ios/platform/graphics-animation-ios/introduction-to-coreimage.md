---
title: Xamarin.iOS에 core 이미지
description: Core 이미지는 ios 5 이미지 처리를 제공 하 고 라이브 비디오 향상 기능을 도입 하는 새로운 프레임 워크입니다. 이 문서에서는 Xamarin.iOS 샘플으로 이러한 기능을 소개 합니다.
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 6032554a0ddbda26ff5de94f6035bc4f8c15a22a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786634"
---
# <a name="core-image-in-xamarinios"></a>Xamarin.iOS에 core 이미지

_Core 이미지는 ios 5 이미지 처리를 제공 하 고 라이브 비디오 향상 기능을 도입 하는 새로운 프레임 워크입니다. 이 문서에서는 Xamarin.iOS 샘플으로 이러한 기능을 소개 합니다._

Core 이미지는 다양 한 기본 제공 필터와 이미지 및 비디오, 얼굴 감지를 포함 한 적용할 효과를 제공 하는 iOS 5에에서 도입 된 새로운 프레임 워크입니다.

이 문서에는 간단한 예를를 들어 있습니다.

-  얼굴 감지 합니다.
-  이미지에 필터를 적용합니다.
-  사용 가능한 필터를 나열 합니다.


이 예제에서는 Xamarin.iOS 응용 프로그램에 통합 Core 이미지 기능을 시작 하는 데 도움이 됩니다.

## <a name="requirements"></a>요구 사항

Xcode의 최신 버전을 사용 해야 합니다.

## <a name="face-detection"></a>얼굴 감지

Core 이미지 얼굴 감지 기능이 수행 하는 바로 언급 한 내용을 우수한 사진에 면을 식별 하려고 인식 하는 모든 면의 좌표를 반환 합니다. 이 정보를 이미지에는 사용자의 수를 계산 하 고 이미지에 표시기를 않음 (예: 그리기를 사용할 수 있습니다. '태그 지정' 사용자의 사진)에 대 한 요소 라도 생각할 수 있습니다.

CoreImage\SampleCode.cs에서이 코드에 포함된 된 이미지에 만들고 얼굴 감지 하는 방법을 보여 줍니다.

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

기능 배열 채워집니다 `CIFaceFeature` (모든 면 검색 된) 경우에 개체입니다. 한 `CIFaceFeature` 각 면에 대해 합니다. `CIFaceFeature` 에 다음과 같은 속성이 있습니다.

-  HasMouthPosition –이 글꼴에 대 한 입 발견 되었는지 여부.
-  HasLeftEyePosition –이 글꼴에 대 한 왼쪽된 눈 발견 되었는지 여부.
-  HasRightEyePosition –이 글꼴에 대 한 오른쪽 눈 발견 되었는지 여부. 
-  MouthPosition이이 글꼴에 대 한 입의 좌표입니다.
-  LeftEyePosition이이 글꼴에 대 한 사람 마다 왼쪽된 좌표입니다.
-  RightEyePosition이이 글꼴에 대 한 오른쪽 눈의 좌표입니다.


이러한 모든 속성에 대 한 좌표 아래 왼쪽 – 왼쪽 위를 원본으로 사용 하 여 UIKit 달리 원점을 가진 합니다. 에 좌표를 사용 하는 경우 `CIFaceFeature` '찍은' 해야 합니다. 이 매우 기본적인 사용자 지정 이미지 보기 CoreImage\CoreImageViewController.cs의 이미지에 '얼굴 표시기' 삼각형을 그리는 방법을 보여 줍니다 (참고는 `FlipForBottomOrigin` 메서드):

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

그런 다음 SampleCode.cs 파일에 이미지 및 기능 할당 된 이미지를 다시 그릴 전에:

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

스크린샷은 샘플 출력: 얼굴 기능 검색 위치는 UITextView에 표시 되 고 CoreGraphics를 사용 하 여 원본 이미지에 그려집니다.

작동 하는 안 면 인식 하는 방식 때문에 가끔 검색 (예: 이러한 장난감 원숭이!) 사람이 면 것 외에 일 합니다.

## <a name="filters"></a>필터

50 개 이상의 서로 다른 기본 제공 필터를 있으며 프레임 워크는 확장 가능 하도록 새 필터를 구현할 수 있습니다.

## <a name="using-filters"></a>필터를 사용 하 여

같은 네 가지 단계는 이미지에 필터를 적용할: 이미지를 로드, 필터를 만들어, 필터를 적용 하 고 저장 (또는 표시)는 결과입니다.

첫째,에 이미지를 로드 한 `CIImage` 개체입니다.

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

셋째, 액세스는 `OutputImage` 속성과 호출은 `CreateCGImage` 최종 결과 렌더링 하는 메서드.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

마지막으로 결과를 보려면 보기에 이미지를 할당 합니다. 실제 응용 프로그램에서 파일 시스템, 사진 앨범, 윗 또는 전자 메일에 결과 이미지를 저장할 수도 있습니다.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

결과 표시 하는 이러한 스크린샷을 `CISepia` 및 `CIHueAdjust` 필터는 CoreImage.zip에서 보여 주는 샘플 코드입니다.

참조는 [조정 계약 및 이미지 조리법의 밝기](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) 의 예는 `CIColorControls` 필터입니다.

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

### <a name="listing-filters-and-their-properties"></a>필터 및 속성 나열

CoreImage\SampleCode.cs에서이 코드는 기본 제공 필터의 전체 목록 및 해당 매개 변수를 출력합니다.

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

[CIFilter 클래스 참조](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) 50 기본 제공 필터 및 해당 속성에 설명 합니다. 위의 코드를 사용 하 여 (사용할 수 있는 입력 필터를 적용 하기 전에 유효성을 검사 하려면)는 최대 및 최소 허용 가능한 값 및 매개 변수의 기본값을 포함 하 여 필터 클래스를 쿼리할 수 있습니다.

시뮬레이터 목록 범주 출력은 다음과 같습니다 – 모든 필터 및 매개 변수를 보려면 목록을 통해 스크롤할 수 있습니다.

 [![](introduction-to-coreimage-images/coreimage05.png "시뮬레이터 목록 범주 출력은 다음과 같습니다.")](introduction-to-coreimage-images/coreimage05.png#lightbox)

나열 된 각 필터, Xamarin.iOS 클래스로 표시 된 어셈블리 브라우저 또는 Mac 용 Visual Studio 또는 Visual Studio에서 자동 완성을 사용 하 여 Xamarin.iOS.CoreImage API도 탐색할 수 있도록 합니다. 

## <a name="summary"></a>요약

이 문서는 얼굴 감지 및 이미지에 필터를 적용 하는 같은 새로운 iOS 5 Core 이미지 프레임 워크 기능 중 일부를 사용 하는 방법을 보여 주었습니다. 사용할 수 있는 프레임 워크에서 사용할 수 있는 다른 이미지 필터: 여러 가지가 있습니다.

## <a name="related-links"></a>관련 링크

- [Core 이미지 (샘플)](https://developer.xamarin.com/samples/CoreImage/)
- [계약 및 이미지 조리법의 밝기를 조정 합니다.](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [이미지 필터: 코어를 사용 하 여](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [CIFilter 클래스 참조](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
