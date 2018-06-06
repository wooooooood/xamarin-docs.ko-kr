---
title: Xamarin.iOS의 핵심 그래픽
description: 핵심 그래픽 iOS 프레임 워크에 설명 합니다. 기 하 도형, 이미지, Pdf를 그리려면 코어 그래픽을 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7d7124c7d09ca4e36ce22d60f578ea4a75d4a05b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786758"
---
# <a name="core-graphics-in-xamarinios"></a>Xamarin.iOS의 핵심 그래픽

_핵심 그래픽 iOS 프레임 워크에 설명 합니다. 기 하 도형, 이미지, Pdf를 그리려면 코어 그래픽을 사용 하는 방법을 보여 줍니다._

iOS에 포함 되어는 [ *코어 그래픽* ](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) 하위 수준 그리기 지원을 제공 하기 위해 프레임 워크입니다. 이러한 프레임 워크 UIKit 내에서 다양 한 그래픽 기능 있게 됩니다. 

코어 그래픽 그리기 장치 독립적인 그래픽을 허용 하는 하위 수준 2 차원 그래픽 프레임 워크입니다. UIKit에 그리기 모든 2D 코어 그래픽을 내부적으로 사용 합니다.

코어 그래픽에서 다양 한 시나리오를 비롯 한 그리기를 지원 합니다.

-  [통해 화면 그리기는 `UIView` ](#Drawing_in_a_UIView_Subclass) 합니다.
-  [메모리에 또는 화면에 이미지를 그리기](#Drawing_Images_and_Text)합니다.
-  만들기 및 PDF로 드로잉 합니다.
-  읽기 및 기존 PDF 그리기


## <a name="geometric-space"></a>기하학적 공간

시나리오에 관계 없이 모든 그리기 코어 그래픽 완료 한 후에 갖고 있으므로 픽셀 아니라 추상 지점에서 사용할 기하학적 공간에서 수행 됩니다. 핵심 그래픽 픽셀으로 변환 하는 모든 처리 및 그려지는 기 하 도형 및 색, 스타일 등의 상태를 그리기 측면에서 원하는 작업을 설명 합니다. 이러한 상태는 상상할 수 있는 복사의 캔버스와 같은 그래픽 컨텍스트에 추가 됩니다.

이 방법에 몇 가지 이점이 있습니다.

-  그리기 코드 동적, 되 고 런타임에 그래픽 ׂ 이후에 수도 있습니다.
-  응용 프로그램 번들의 정적 이미지에 대 한 필요성을 줄이고 응용 프로그램 크기를 줄일 수 있습니다.
-  장치에서 그래픽 해상도 변경 복원 력이 향상 됩니다.

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>UIView 하위 클래스에 그리기

모든 `UIView` 에 `Draw` 을 그릴 수 있도록 해야 하는 경우 시스템에서 호출 되어 메서드. 보기에 그리기 코드를 추가 하위 클래스 `UIView` 재정의 `Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

그리기 호출 되 면 안 직접 합니다. 실행된 루프 처리 하는 동안 시스템에 의해 호출 됩니다. 뷰 계층 구조 보기에 추가 된 후 실행된 루프를 통해 처음으로 해당 `Draw` 메서드를 호출 합니다. 에 대 한 후속 호출 `Draw` 뷰 중 하나를 호출 하 여 그려야 하는 것으로 표시 하면 발생 `SetNeedsDisplay` 또는 `SetNeedsDisplayInRect` 보기.

### <a name="pattern-for-graphics-code"></a>그래픽 코드에 대 한 패턴

코드는 `Draw` 구현은 원하는 항목 그려지는 특징을 기술 해야 합니다. 그리기 코드 것 그리기 일부 상태를 설정 하 고 있는를 그릴 수 요청 메서드를 호출 패턴을 따릅니다. 이 패턴은 다음과 같이 일반화할 수 수 있습니다.:

1. 그래픽 컨텍스트를 가져옵니다.

2. 그리기 특성을 설정 합니다.

3. 일부 기 하 도형 그리기 기본 형식에서 만듭니다.

4. 그리기 또는 스트로크 메서드를 호출 합니다.

### <a name="basic-drawing-example"></a>기본 그리기 예제

예를 들어 다음 코드 조각을 참조 하십시오.

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

이 코드 분리 해 보겠습니다.

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
이 줄으로 드로잉에 사용할 현재 그래픽 컨텍스트와를 먼저 가져옵니다. 생각할 수 있습니다 그래픽 컨텍스트의 캔버스로 드로잉 실행 된다는, 선 및 채우기 색으로 기 하 도형을 그리는 데 같은 그리기에 대 한 모든 상태를 포함 하 합니다.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

그래픽 컨텍스트를 받은 후 위에 표시를 그릴 때 사용할 코드에서는 일부 특성을 설정 합니다. 이 경우 줄 너비, 선 및 채우기 색 설정 됩니다. 모든 후속 드로잉은 그래픽 컨텍스트 상태에서 유지 되기 때문에 이러한 특성을 사용 합니다.

사용 하 여 코드를 만들려면 기 하 도형은 `CGPath`를 허용 하는 그래픽 경로 선 및 곡선에서 이해 해야 합니다. 이 경우 경로 삼각형을 구성 하는 지점의 배열에 연결 하는 선을 추가 합니다. 그리기 보기는 좌표계 코어 그래픽 사용 하 여 아래 표시, 여기서 원점은 왼쪽 위, 오른쪽 및 아래쪽 양수 y 방향 양수 x direct 사용 하 여:

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

호출 되도록 그래픽 컨텍스트에 추가 됩니다 경로 만든 후 `AddPath` 및 `DrawPath` 각각 그릴 수 있습니다.

결과 보기는 다음과 같습니다.

 ![](core-graphics-images/00-bluetriangle.png "샘플 출력 삼각형")

## <a name="creating-gradient-fills"></a>그라데이션 채우기 만들기

그리기의 다양 한 형태를 사용할 수 있습니다. 예를 들어, 그라데이션 채우기를 만들고 클리핑 경로 적용 코어 그래픽 수 있습니다. 그리려면 경로 안에 그라데이션 채우기 이전 예제의 먼저 경로 해야를 클리핑 패스도 설정할 수 있습니다:

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

선형 그라데이션을 그립니다 클리핑 패스 다음 코드 처럼 경로 기 하 도형 내 모든 후속 그리기를 제한 하는 대로 현재 경로 설정입니다.

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

이러한 변경 내용은 아래와 같이 그라데이션 채우기를 생성 합니다.

 ![](core-graphics-images/01-gradient-fill.png "그라데이션 채우기가 적용 된 예제")

## <a name="modifying-line-patterns"></a>선 패턴을 수정합니다.

핵심 그래픽 줄 그리기 특성을 수정할 수도 있습니다. 여기에 다음 코드와 같이 자체 선 패턴 뿐만 아니라 선 두께 선 색을 변경 합니다.

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

아래와 같이 단위로 파선된 선 10 long, 대시 사이의 간격 단위 4와 함께 모든 그리기 작업 결과 앞이 코드를 추가 합니다.

 ![](core-graphics-images/02-dashed-stroke.png "파선에 그리기 작업 결과 앞에 다음이 코드를 추가합니다.")
 
통합 API를 사용 하 여 Xamarin.iOS에서, 배열 형식 되도록 필요는 `nfloat`, Math.PI를 명시적으로 캐스팅 해야 합니다.

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>그리기 이미지 및 텍스트

보기의 그래픽 컨텍스트에서 경로 그리는 것 외에 코어 그래픽 그리기 이미지 및 텍스트에도 지원 합니다. 이미지를 그리려면 만들기만 `CGImage` 에 전달 된 `DrawImage` 호출:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

그러나 180도, 아래와 같이 그려지는 이미지 생성 합니다.

 ![](core-graphics-images/03-upside-down-monkey.png "180도 회전 그려지는 이미지")

이 대 한 원인은 보기의 원점이 홈페이지에서 이미지 그리기에 대 한 핵심 그래픽 원점이 왼쪽 아래에 있고입니다. 따라서 이미지를 올바르게 표시 하려면 시작을 수정할 필요가, 수정 하 여 수행할 수 있습니다는 *현재 변환 매트릭스* *(CTM)* 합니다. CTM 정의 지점 거주 라고도 *사용자 공간*합니다. Y 방향의 CTM 반전 경계 높이 음수 y 방향에서으로 이동 하는 이미지 대칭 이동 합니다.

그래픽 컨텍스트에는 CTM 변환 하는 도우미 메서드가 있습니다. 이 경우 `ScaleCTM` "대칭 이동" 그리기 및 `TranslateCTM` 아래와 같이 왼쪽 위의으로 이동 합니다.

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

결과 이미지가 표시 됩니다 수직:

 ![](core-graphics-images/04-upright-monkey.png "샘플 이미지 표시 수직")

> [!IMPORTANT]
> 그래픽 컨텍스트의 변경 내용은 이후의 모든 그리기 작업에 적용 합니다. 따라서는 CTM 변환 될 때 영향을 주므로 드로잉 추가 합니다. 예를 들어 CTM 변환 후 삼각형을 그린 하는 경우에 표시 180도 회전 합니다.

### <a name="adding-text-to-the-image"></a>이미지에 텍스트 추가

경로 및 이미지를 사용 하 코어 그래픽 텍스트 그리기 동일한 기본 패턴의 일부 그래픽 상태를 설정 하 고 그리는 데 메서드를 호출 해야 합니다. 텍스트의 경우 텍스트를 표시 하는 메서드는 `ShowText`합니다. 예제에서는 그리기 이미지에 추가 하는 경우 다음 코드 코어 그래픽을 사용 하 여 일부 텍스트를 그립니다.

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

볼 수 있듯이 텍스트 그리기에 대 한 그래픽 상태로 설정 하는 기 하 도형 그리기 비슷합니다. 그러나 텍스트를 그리기 위한 텍스트 모드와 글꼴 그리기도 적용 됩니다. 이 경우 그리기 경로 대 한 동일 하 게 작동 그림자를 적용 하지만 그림자가 적용도 합니다.

아래와 같이 결과 텍스트 이미지와 함께 표시 됩니다.

 ![](core-graphics-images/05-text-on-image.png "결과 텍스트 이미지와 함께 표시 됩니다.")

## <a name="memory-backed-images"></a>메모리 기반 이미지

보기의 그래픽 컨텍스트에 드로잉에 외에도 코어 그래픽 지원 메모리 그리기 화면 밖에 그리는 라고도 이미지를 백업 합니다. 이렇게 하면 필요:

-  그래픽 컨텍스트를 만들기는 뒷받침 되며 메모리 내 비트맵
-  그리기 상태를 설정 하 고 그리기 명령 실행
-  컨텍스트를 이미지 가져오기
-  컨텍스트를 제거합니다.


와 달리는 `Draw` 메서드 컨텍스트를 보기에서 제공 하는 경우이 경우 컨텍스트를 만들 두 가지 방법 중 하나에서:

1. 호출 하 여 `UIGraphics.BeginImageContext` (또는 `BeginImageContextWithOptions`)

2. 새 `CGBitmapContextInstance`

 `CGBitmapContextInstance` 사용 하는 직접 이미지 비트와 같은 경우 여기서 사용자 지정 이미지 조작 알고리즘을 사용 하는 경우 유용 합니다. 다른 모든 경우에 사용 해야 `BeginImageContext` 또는 `BeginImageContextWithOptions`합니다.

이미지 컨텍스트를 만든 후 추가 중인 것 처럼이 그리기 코드는 `UIView` 하위 클래스입니다. 예를 들어 삼각형을 그리는 데 이전 코드 예제에서는 수 그리는 데 사용할 이미지에 대신 메모리에는 `UIView`다음과 같이 합니다.

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

메모리 기반 비트맵 그리기의 일반적인 용도에서 이미지를 캡처하는 `UIView`합니다. 예를 들어 다음 코드에서는 비트맵 컨텍스트에 보기의 계층을 렌더링 하 고 만듭니다는 `UIImage` 여기에서:

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Pdf 그리기

이미지, 아니라 코어 그래픽 PDF 그리기를 지원합니다. 이미지와 마찬가지로 있습니다 수 메모리에서 PDF 렌더링으로 PDF 렌더링에 대 한 읽기는 `UIView`합니다.

### <a name="pdf-in-a-uiview"></a>UIView에서 PDF

PDF 파일에서 읽기 및 사용 하 여 보기에서 렌더링 코어 그래픽 지원는 `CGPDFDocument` 클래스입니다. `CGPDFDocument` 클래스 코드에서 PDF를 나타내며 읽고 페이지를 그리는 데 사용할 수 있습니다.

예를 들어 다음 코드에서는 `UIView` 하위 클래스는 PDF 파일에서에서 읽을에 `CGPDFDocument`:

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

`Draw` 메서드를 유도할 수 있습니다는 `CGPDFDocument` 에 페이지를 읽으려는 `CGPDFPage` 하 고 호출 하 여 렌더링 `DrawPDFPage`다음과 같이:

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

### <a name="memory-backed-pdf"></a>메모리 기반 PDF

메모리에서 PDF에 대 한 호출 하 여 PDF 컨텍스트를 만들어야 할 `BeginPDFContext`합니다. PDF로 드로잉 페이지로 세분화 된입니다. 각 페이지를 호출 하 여 시작 `BeginPDFPage` 호출 하 여 완료 및 `EndPDFContent`, graphics 코드 사이 있습니다. 또한 이미지 그리기와 메모리 백업 된 것으로 사용 하 여 그리기 PDF는 CTM만 수정 하 여 고려할 수 있는 왼쪽 아래에 원본 이미지와 같은입니다.

다음 코드를 pdf 텍스트를 그리는 방법을 보여 줍니다.

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

결과 텍스트를 다음에 포함 된 PDF 그려집니다는 `NSData` 를 저장할 수 있습니다, 업로드, 전자 메일을 보낸 등입니다.


## <a name="summary"></a>요약

이 문서에서 보이는 것 처럼 그래픽 기능을 통해 제공 되는 *코어 그래픽* 프레임 워크입니다. 핵심 그래픽의 컨텍스트 내에서 기 하 도형, 이미지 및 Pdf를 그리는 데 사용 하는 방법에 살펴보았습니다는 `UIView,` 뿐만 아니라 메모리 기반 그래픽 컨텍스트.

## <a name="related-links"></a>관련 링크

- [코어 그래픽 예제](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [그래픽 및 애니메이션 연습](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [핵심 애니메이션](~/ios/platform/graphics-animation-ios/core-animation.md)
- [코어 애니메이션 레시피](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
