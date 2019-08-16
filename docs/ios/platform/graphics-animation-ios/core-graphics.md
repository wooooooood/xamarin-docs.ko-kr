---
title: Xamarin.ios의 핵심 그래픽
description: 이 문서에서는 핵심 그래픽 iOS 프레임 워크에 대해 설명 합니다. 핵심 그래픽을 사용 하 여 기 하 도형, 이미지 및 Pdf를 그리는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 0c606e001552f1c4267ffc29bd69b2f38f2ec971
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69527936"
---
# <a name="core-graphics-in-xamarinios"></a>Xamarin.ios의 핵심 그래픽

_이 문서에서는 핵심 그래픽 iOS 프레임 워크에 대해 설명 합니다. 핵심 그래픽을 사용 하 여 기 하 도형, 이미지 및 Pdf를 그리는 방법을 보여 줍니다._

iOS에는 하위 수준의 그리기 지원 기능을 제공 하는 [*핵심 그래픽*](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) 프레임 워크가 포함 되어 있습니다. 이러한 프레임 워크는 UIKit 내에서 풍부한 그래픽 기능을 사용 하도록 설정 합니다. 

핵심 그래픽은 장치 독립적인 그래픽의 그리기를 허용 하는 낮은 수준의 2D 그래픽 프레임 워크입니다. UIKit의 모든 2D 그리기는 내부적으로 핵심 그래픽을 사용 합니다.

핵심 그래픽은 다음을 비롯 한 다양 한 시나리오에서 그리기를 지원 합니다.

- [ 를`UIView` 통해 화면에 그립니다](#Drawing_in_a_UIView_Subclass) .
- [메모리 나 화면에 이미지를 그립니다](#Drawing_Images_and_Text).
- PDF 만들기 및 그리기
- 기존 PDF를 읽고 그립니다.


## <a name="geometric-space"></a>기 하 도형 공간

시나리오에 관계 없이 코어 그래픽을 사용 하 여 수행 되는 모든 그리기는 기하학적 공간에서 수행 됩니다. 즉, 픽셀이 아닌 추상 점에서 작동 합니다. Geometry 및 그리기 상태 (예: 색, 선 스타일 등)와 그리기 상태를 사용 하 여 그릴 항목을 설명 하 고, 핵심 그래픽 핸들은 모든 항목을 픽셀로 변환 합니다. 이러한 상태는 복사의 캔버스 처럼 생각할 수 있는 그래픽 컨텍스트에 추가 됩니다.

이 방법에는 다음과 같은 몇 가지 이점이 있습니다.

- 그리기 코드는 동적이 되며 이후에는 런타임에 그래픽을 수정할 수 있습니다.
- 응용 프로그램 번들에서 정적 이미지의 필요성을 줄이면 응용 프로그램 크기를 줄일 수 있습니다.
- 그래픽은 장치에서 해상도를 변경 하는 데 더 탄력적으로 유지 됩니다.

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>UIView 하위 클래스에서 그리기

모든 `UIView` 에 `Draw` 그려야 할 때 시스템에 의해 호출 되는 메서드. 뷰에 그리기 코드를 추가 하려면 하위 클래스 `UIView` 및 재정의: `Draw`

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

Draw는 직접 호출 하면 안 됩니다. 실행된 루프 처리 하는 동안 시스템에 의해 호출 됩니다. 뷰는 뷰 계층 구조에 추가 된 후 실행된 루프를 통해 처음으로 해당 `Draw` 메서드가 호출 됩니다. 에 대 한 후속 호출 `Draw` 뷰를 호출 하 여 그려야 하는 것으로 표시 되어 표시 될 `SetNeedsDisplay` 또는 `SetNeedsDisplayInRect` 뷰.

### <a name="pattern-for-graphics-code"></a>그래픽 코드 패턴

`Draw` 구현에서 코드는 그리려는 항목을 설명 해야 합니다. 그리기 코드는 그리기 상태를 설정 하 고 메서드를 호출 하 여 그리기를 요청 하는 패턴을 따릅니다. 이 패턴은 다음과 같이 일반화 될 수 있습니다.

1. 그래픽 컨텍스트를 가져옵니다.

2. 그리기 특성을 설정 합니다.

3. 그리기 기본 형식에서 일부 기 하 도형을 만듭니다.

4. 그리기 또는 스트로크 메서드를 호출 합니다.

### <a name="basic-drawing-example"></a>기본 그리기 예제

예를 들어 다음 코드 조각을 살펴보십시오.

```csharp
//get graphics context
using (CGContext g = UIGraphics.GetCurrentContext ()) {
            
    //set up drawing attributes
    g.SetLineWidth (10);
    UIColor.Blue.SetFill ();
    UIColor.Red.SetStroke ();

    //create geometry
    var path = new CGPath ();

    path.AddLines (new CGPoint[]{
    new CGPoint (100, 200),
    new CGPoint (160, 100), 
    new CGPoint (220, 200)});

    path.CloseSubpath ();

    //add geometry to graphics context and draw it
    g.AddPath (path);       
    g.DrawPath (CGPathDrawingMode.FillStroke);
}
```

이 코드를 중단 해 보겠습니다.

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
이 줄에서는 먼저 그리기에 사용할 현재 그래픽 컨텍스트를 가져옵니다. 그리기를 수행 하는 캔버스로 그래픽 컨텍스트를 생각할 수 있습니다. 스트로크 및 채우기 색과 같은 그리기에 대 한 모든 상태와 그릴 기 하 도형도 포함 합니다.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

그래픽 컨텍스트를 가져온 후 코드는 위에 표시 된 대로 그릴 때 사용할 일부 특성을 설정 합니다. 이 경우 선 두께, 스트로크 및 채우기 색이 설정 됩니다. 그런 다음, 모든 후속 그리기는 그래픽 컨텍스트의 상태에서 유지 되므로 이러한 특성을 사용 합니다.

기 하 도형을 만들려면 코드에서를 `CGPath`사용 합니다 .이를 통해 선 및 곡선에서 그래픽 경로를 설명할 수 있습니다. 이 경우 경로는 점의 배열을 연결 하는 선을 추가 하 여 삼각형을 구성 합니다. 아래에 표시 된 것 처럼 핵심 그래픽은 뷰 그리기에 좌표계를 사용 합니다. 여기서 원점은 왼쪽 위에 있고, 양수 x는 오른쪽으로, 양수 y는 아래로 이동 합니다.

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

경로를 만든 후에는를 호출 `AddPath` 하 고 `DrawPath` 각각을 그릴 수 있도록 그래픽 컨텍스트에 추가 됩니다.

결과 뷰는 다음과 같습니다.

 ![](core-graphics-images/00-bluetriangle.png "샘플 출력 삼각형")

## <a name="creating-gradient-fills"></a>그라데이션 채우기 만들기

다양 한 형태의 드로잉이 제공 됩니다. 예를 들어 코어 그래픽을 통해 그라데이션 채우기를 만들고 클리핑 패스를 적용할 수 있습니다. 이전 예제의 경로 내부에 그라데이션 채우기를 그리려면 먼저 경로를 클리핑 경로로 설정 해야 합니다.

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

현재 경로를 클리핑 경로로 설정 하면 다음 코드와 같이 선형 그라데이션을 그리는 다음 코드와 같이 패스의 기 하 도형 내에서 이어지는 모든 그리기를 제한 합니다.

```csharp
// the color space determines how Core Graphics interprets color information
    using (CGColorSpace rgb = CGColorSpace.CreateDeviceRGB()) {
        CGGradient gradient = new CGGradient (rgb, new CGColor[] {
        UIColor.Blue.CGColor,
        UIColor.Yellow.CGColor
    });

// draw a linear gradient
    g.DrawLinearGradient (
        gradient, 
        new CGPoint (path.BoundingBox.Left, path.BoundingBox.Top), 
        new CGPoint (path.BoundingBox.Right, path.BoundingBox.Bottom), 
        CGGradientDrawingOptions.DrawsBeforeStartLocation);
    }
```

이러한 변경 내용에 따라 다음과 같이 그라데이션 채우기가 생성 됩니다.

 ![](core-graphics-images/01-gradient-fill.png "그라데이션 채우기가 있는 예제")

## <a name="modifying-line-patterns"></a>선 패턴 수정

선의 그리기 특성도 핵심 그래픽을 사용 하 여 수정할 수 있습니다. 여기에는 다음 코드에 표시 된 대로 선 두께와 스트로크 색 뿐만 아니라 선 패턴 자체도 변경 됩니다.

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

그리기 작업을 수행 하기 전에이 코드를 추가 하면 아래와 같이 파선 사이에 4 개의 간격을 사용 하 여 대시 선이 10 단위 길이입니다.

 ![](core-graphics-images/02-dashed-stroke.png "그리기 작업 앞에이 코드를 추가 하면 대시 선이 생성 됩니다.")
 
Xamarin.ios에서 Unified API를 사용 하는 경우 배열 형식은 이어야 `nfloat`하며, 수학. PI로 명시적으로 캐스팅 해야 합니다.

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>이미지 및 텍스트 그리기

핵심 그래픽은 뷰의 그래픽 컨텍스트에서 경로를 그리는 것 외에도 그리기 이미지 및 텍스트를 지원 합니다. 이미지를 그리려면를 만들어 `CGImage` `DrawImage` 호출에 전달 하면 됩니다.

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

그러나 아래와 같이 뒤집힌 이미지를 생성 합니다.

 ![](core-graphics-images/03-upside-down-monkey.png "상하로 그린 이미지")

이에 대 한 이유는 이미지 그리기에 대 한 핵심 그래픽 원점이 왼쪽 아래에 있고 뷰는 왼쪽 위에서의 원점입니다. 따라서 이미지를 올바르게 표시 하려면 원본을 수정 해야 합니다 .이는 *ctm (* *현재 변환 매트릭스* )를 수정 하 여 수행할 수 있습니다. CTM은 *사용자 공간이*라고도 하는 위치를 정의 합니다. Y 방향으로 CTM을 반전 하 고 음수 y 방향의 범위 ' 높이로 이동 하면 이미지가 대칭 이동 될 수 있습니다.

그래픽 컨텍스트에는 CTM을 변환할 수 있는 도우미 메서드가 있습니다. 이 경우 `ScaleCTM` 아래와 같이 드로잉을 "대칭 이동 `TranslateCTM` " 하 고 왼쪽 위로 이동 합니다.

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
    
    using (CGContext g = UIGraphics.GetCurrentContext ()) {

        // scale and translate the CTM so the image appears upright
        g.ScaleCTM (1, -1);
        g.TranslateCTM (0, -Bounds.Height);
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
}   
```

그러면 결과 이미지가 수직으로 표시 됩니다.

 ![](core-graphics-images/04-upright-monkey.png "위에 표시 된 샘플 이미지")

> [!IMPORTANT]
> 그래픽 컨텍스트를 변경 하면 이후의 모든 그리기 작업에 적용 됩니다. 따라서 CTM이 변환 되 면 추가 그리기에 영향을 줍니다. 예를 들어 CTM 변환 후 삼각형을 그린 경우 거꾸로 표시 됩니다.

### <a name="adding-text-to-the-image"></a>이미지에 텍스트 추가

경로 및 이미지를 사용 하는 것과 마찬가지로, 핵심 그래픽을 사용 하 여 텍스트를 그리는 경우 일부 그래픽 상태를 설정 하 고 그릴 메서드를 호출 하는 것과 동일한 기본 패턴이 텍스트의 경우 텍스트 `ShowText`를 표시 하는 메서드는입니다. 이미지 그리기 예제에 추가 되 면 다음 코드는 핵심 그래픽을 사용 하 여 일부 텍스트를 그립니다.

```csharp
public override void Draw (RectangleF rect)
{
    base.Draw (rect);
    
    // image drawing code omitted for brevity ...

    // translate the CTM by the font size so it displays on screen
    float fontSize = 35f;
    g.TranslateCTM (0, fontSize);

    // set general-purpose graphics state
    g.SetLineWidth (1.0f);
    g.SetStrokeColor (UIColor.Yellow.CGColor);
    g.SetFillColor (UIColor.Red.CGColor);
    g.SetShadow (new CGSize (5, 5), 0, UIColor.Blue.CGColor);

    // set text specific graphics state
    g.SetTextDrawingMode (CGTextDrawingMode.FillStroke);
    g.SelectFont ("Helvetica", fontSize, CGTextEncoding.MacRoman);

    // show the text
    g.ShowText ("Hello Core Graphics");
}
```

여기에서 볼 수 있듯이 텍스트 그리기에 대 한 그래픽 상태 설정은 기 하 도형 그리기와 비슷합니다. 그러나 텍스트 그리기의 경우 텍스트 그리기 모드와 글꼴도 적용 됩니다. 이 경우 그림자를 적용 해도 패스 그리기에 대해 그림자가 동일 하 게 적용 되지만 그림자도 적용 됩니다.

결과 텍스트는 아래와 같이 이미지가 표시 됩니다.

 ![](core-graphics-images/05-text-on-image.png "결과 텍스트가 이미지와 함께 표시 됩니다.")

## <a name="memory-backed-images"></a>메모리 지원 이미지

핵심 그래픽은 뷰의 그래픽 컨텍스트로 그리는 것 외에도 화면에서 그리기 라고도 하는 메모리 지원 이미지 그리기를 지원 합니다. 이렇게 하려면 다음을 수행 해야 합니다.

- 메모리 내 비트맵에 의해 지원 되는 그래픽 컨텍스트 만들기
- 그리기 상태 설정 및 그리기 명령 실행
- 컨텍스트에서 이미지 가져오기
- 컨텍스트 제거


뷰가에서 컨텍스트를 제공 하는 메서드와달리이경우다음두가지방법중하나로컨텍스트를만듭니다.`Draw`

1. (또는 `UIGraphics.BeginImageContext` `BeginImageContextWithOptions`)를 호출 하 여

2. 새을 만들어`CGBitmapContextInstance`

 `CGBitmapContextInstance`는 사용자 지정 이미지 조작 알고리즘을 사용 하는 경우와 같이 이미지 비트로 직접 작업 하는 경우에 유용 합니다. 다른 모든 경우에는 또는 `BeginImageContext` `BeginImageContextWithOptions`를 사용 해야 합니다.

이미지 컨텍스트가 있으면 그리기 코드를 추가 하는 것은 `UIView` 하위 클래스에 있는 것과 같습니다. 예를 들어 앞에서 삼각형을 그리기 위해 사용 된 코드 예제를 사용 하 여 아래와 같이를 사용 하는 대신 `UIView`메모리의 이미지를 그릴 수 있습니다.

```csharp
UIImage DrawTriangle ()
{
    UIImage triangleImage;

    //push a memory backed bitmap context on the context stack
    UIGraphics.BeginImageContext (new CGSize (200.0f, 200.0f));

    //get graphics context
    using(CGContext g = UIGraphics.GetCurrentContext ()){

        //set up drawing attributes
        g.SetLineWidth(4);
        UIColor.Purple.SetFill ();
        UIColor.Black.SetStroke ();

        //create geometry
        path = new CGPath ();

        path.AddLines(new CGPoint[]{
            new CGPoint(100,200),
            new CGPoint(160,100), 
            new CGPoint(220,200)});

        path.CloseSubpath();

        //add geometry to graphics context and draw it
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);

        //get a UIImage from the context
        triangleImage = UIGraphics.GetImageFromCurrentImageContext ();
    }
    
    return triangleImage;
}
```

메모리 지원 비트맵에 그리는 일반적인 용도는에서 `UIView`이미지를 캡처하는 것입니다. 예를 들어 다음 코드는 보기의 계층을 비트맵 컨텍스트에 렌더링 하 고 해당 계층에서 `UIImage` 를 만듭니다.

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Pdf 그리기

이미지 외에도 핵심 그래픽은 PDF 그리기를 지원 합니다. 이미지와 마찬가지로 메모리에 PDF를 렌더링 하 고에서 `UIView`렌더링을 위해 pdf를 읽을 수 있습니다.

### <a name="pdf-in-a-uiview"></a>UIView의 PDF

또한 핵심 그래픽은 파일에서 PDF를 읽고 클래스를 `CGPDFDocument` 사용 하 여 보기에서 PDF를 렌더링 하도록 지원 합니다. 클래스 `CGPDFDocument` 는 코드의 PDF를 나타내며 페이지를 읽고 그리는 데 사용할 수 있습니다.

예를 들어 `UIView` 하위 클래스의 다음 코드는 파일 `CGPDFDocument`에서로 PDF를 읽습니다.

```csharp
public class PDFView : UIView
{
    CGPDFDocument pdfDoc;

    public PDFView ()
    {
        //create a CGPDFDocument from file.pdf included in the main bundle
        pdfDoc = CGPDFDocument.FromFile ("file.pdf");
    }
  
     public override void Draw (Rectangle rect)
    {
        ...
    }
}
```

그런 `Draw` 다음 메서드는 아래와 `CGPDFDocument` 같이를 사용 `CGPDFPage` 하 여 페이지를 읽고를 호출 `DrawPDFPage`하 여 렌더링할 수 있습니다.

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
        
    //flip the CTM so the PDF will be drawn upright
    using (CGContext g = UIGraphics.GetCurrentContext ()) {
        g.TranslateCTM (0, Bounds.Height);
        g.ScaleCTM (1, -1);
        
        // render the first page of the PDF
        using (CGPDFPage pdfPage = pdfDoc.GetPage (1)) {
            
        //get the affine transform that defines where the PDF is drawn
        CGAffineTransform t = pdfPage.GetDrawingTransform (CGPDFBox.Crop, rect, 0, true);
        
        //concatenate the pdf transform with the CTM for display in the view
        g.ConcatCTM (t);
        
        //draw the pdf page
        g.DrawPDFPage (pdfPage);
        }
    }
}
```

### <a name="memory-backed-pdf"></a>메모리 지원 PDF

메모리 내 PDF의 경우를 호출 `BeginPDFContext`하 여 pdf 컨텍스트를 만들어야 합니다. PDF로 그리기는 페이지에 자세히 있습니다. 각 페이지는를 호출 하 `BeginPDFPage` 고를 호출 하 `EndPDFContent`여 시작 되며,의 그래픽 코드와 함께를 호출 하 여 완료 됩니다. 또한 이미지 드로잉과 마찬가지로 메모리 지원 PDF 그리기는 왼쪽 아래에 있는 원본을 사용 합니다 .이는 이미지와 마찬가지로 CTM을 수정 하 여 고려할 수 있습니다.

다음 코드는 PDF에 텍스트를 그리는 방법을 보여 줍니다.

```csharp
//data buffer to hold the PDF
NSMutableData data = new NSMutableData ();

//create a PDF with empty rectangle, which will configure it for 8.5x11 inches
UIGraphics.BeginPDFContext (data, CGRect.Empty, null);

//start a PDF page
UIGraphics.BeginPDFPage ();
       
using (CGContext g = UIGraphics.GetCurrentContext ()) {
    g.ScaleCTM (1, -1);
    g.TranslateCTM (0, -25);      
    g.SelectFont ("Helvetica", 25, CGTextEncoding.MacRoman);
    g.ShowText ("Hello Core Graphics");
    }
    
//complete a PDF page
UIGraphics.EndPDFContent ();
```

결과 텍스트는 PDF에 그려지며,이는 저장 하 고, 업로드 하 `NSData` 고, 전자 메일로 보낼 수 있는에 포함 되어 있습니다.


## <a name="summary"></a>요약

이 문서에서는 *핵심 그래픽* 프레임 워크를 통해 제공 되는 그래픽 기능을 살펴보았습니다. 핵심 그래픽을 사용 하 여 및 메모리 지원 그래픽 컨텍스트의 컨텍스트 `UIView,` 내에서 geometry, 이미지 및 pdf를 그리는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [핵심 그래픽 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/graphicsandanimation)
- [그래픽 및 애니메이션 연습](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [핵심 애니메이션](~/ios/platform/graphics-animation-ios/core-animation.md)
- [핵심 애니메이션 조리법](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
