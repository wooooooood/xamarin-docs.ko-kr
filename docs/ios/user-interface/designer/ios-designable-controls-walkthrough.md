---
title: "IOS 용 Xamarin 디자이너와 사용자 지정 컨트롤을 사용 하는 연습-"
description: "이 문서에서는 사용자 지정 컨트롤을 만들고 iOS 용 Xamarin 디자이너에서 사용 하는 방법을 보여 주는 단계별 연습을 제공 합니다. 디자이너의 도구 상자에서 사용할 수 있는 컨트롤을 끌어서/삭제 보기로 정도의 만들어야 하는 방법을 보여 줍니다. 또한 디자인 타임에 설정 될 수 있는 속성을 만드는 방법 뿐만 아니라 디자인 타임 및 런타임, 올바르게 렌더링 하도록 컨트롤을 구현 하는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: e78b76a531e9f8ea88adca46fc59b2063fce14cc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---using-custom-controls-with-the-xamarin-designer-for-ios"></a>IOS 용 Xamarin 디자이너와 사용자 지정 컨트롤을 사용 하는 연습-

_이 문서에서는 사용자 지정 컨트롤을 만들고 iOS 용 Xamarin 디자이너에서 사용 하는 방법을 보여 주는 단계별 연습을 제공 합니다. 디자이너의 도구 상자에서 사용할 수 있는 컨트롤을 끌어서/삭제 보기로 정도의 만들어야 하는 방법을 보여 줍니다. 또한 디자인 타임에 설정 될 수 있는 속성을 만드는 방법 뿐만 아니라 디자인 타임 및 런타임, 올바르게 렌더링 하도록 컨트롤을 구현 하는 방법을 보여 줍니다._

## <a name="requirements"></a>요구 사항

IOS 용 Xamarin 디자이너는 Windows에서 Mac 및 Visual Studio 2015 및 2017 용 Visual Studio에서 사용할 수 있습니다.

이 가이드에서 다루는 내용에 대 한 지식이 있다고 가정은 [안내 시작](~/ios/get-started/index.md)합니다.

## <a name="walkthrough"></a>연습

> [!IMPORTANT]
> 사용자 지정 컨트롤 만들어지는 방식은 Xamarin.Studio 5.5 년부터 이전 버전으로 약간 다릅니다. 하거나 사용자 지정 컨트롤을 만들려면는 `IComponent` 인터페이스 (구현 관련된 메서드)와 필요한 또는 클래스는으로 주석을 달아야 `[DesignTimeVisible(true)]`합니다. 다음 연습 예제에서 두 번째 방법을 사용 중입니다.


1. 새 솔루션 만들기는 **iOS > 앱 > 단일 보기 응용 프로그램 > C#** 서식 파일을 이름을 `ScratchTicket`, 새 프로젝트 마법사를 통해 계속:


    [![](ios-designable-controls-walkthrough-images/01new.png "새 솔루션 만들기")](ios-designable-controls-walkthrough-images/01new.png)


1. 라는 새 빈 클래스 파일을 만듭니다 `ScratchTicketView`:


    [![](ios-designable-controls-walkthrough-images/02new.png "새 ScratchTicketView 클래스 만들기")](ios-designable-controls-walkthrough-images/02new.png)


1. 다음 코드에 대 한 추가 `ScratchTicketView` 클래스:

    ```csharp
    using System;
    using System.ComponentModel;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace ScratchTicket
    {
        [Register("ScratchTicketView"), DesignTimeVisible(true)]
        public class ScratchTicketView : UIView
        {
            CGPath path;
            CGPoint initialPoint;
            CGPoint latestPoint;
            bool startNewPath = false;
            UIImage image;
    
            [Export("Image"), Browsable(true)]
            public UIImage Image
            {
                get { return image; }
                set
                {
                    image = value;
                    SetNeedsDisplay();
                }
            }
    
            public ScratchTicketView(IntPtr p)
                : base(p)
            {
                Initialize();
            }
    
            public ScratchTicketView()
            {
                Initialize();
            }
    
            void Initialize()
            {
                initialPoint = CGPoint.Empty;
                latestPoint = CGPoint.Empty;
                BackgroundColor = UIColor.Clear;
                Opaque = false;
                path = new CGPath();
                SetNeedsDisplay();
            }
    
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    initialPoint = touch.LocationInView(this);
                }
            }
    
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    latestPoint = touch.LocationInView(this);
                    SetNeedsDisplay();
                }
            }
    
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                startNewPath = true;
            }
    
            public override void Draw(CGRect rect)
            {
                base.Draw(rect);
    
                using (var g = UIGraphics.GetCurrentContext())
                {
                    if (image != null)
                        g.SetFillColor((UIColor.FromPatternImage(image).CGColor));
                    else
                        g.SetFillColor(UIColor.LightGray.CGColor);
                    g.FillRect(rect);
    
                    if (!initialPoint.IsEmpty)
                    {
                        g.SetLineWidth(20);
                        g.SetBlendMode(CGBlendMode.Clear);
                        UIColor.Clear.SetColor();
    
                        if (path.IsEmpty || startNewPath)
                        {
                            path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                            startNewPath = false;
                        }
                        else
                        {
                            path.AddLineToPoint(latestPoint);
                        }
    
                        g.SetLineCap(CGLineCap.Round);
                        g.AddPath(path);        
                        g.DrawPath(CGPathDrawingMode.Stroke);
                    }
                }
            }
        }
    }
    ```


1. 추가 `FillTexture.png`, `FillTexture2.png` 및 `Monkey.png` 파일 (사용 가능한 [GitHub에서](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true))에 **리소스** 폴더입니다.
    
1. 두 번 클릭 하 여 `Main.storyboard` 디자이너에서 열려는 파일:

    
    [![](ios-designable-controls-walkthrough-images/03new.png "IOS 디자이너")](ios-designable-controls-walkthrough-images/03new.png)



1. 끌기/놓기는 **이미지 보기** 에서 **도구 상자** 스토리 보드에서 보기로 합니다.

    
    [![](ios-designable-controls-walkthrough-images/04new.png "레이아웃에 이미지 뷰 추가")](ios-designable-controls-walkthrough-images/04new.png)


1. 선택 된 **이미지 보기** 변경 하 고 해당 **이미지** 속성을 `Monkey.png`합니다.

    
    [![](ios-designable-controls-walkthrough-images/05new.png "Monkey.png에 이미지 뷰 이미지 속성을 설정합니다.")](ios-designable-controls-walkthrough-images/05new.png)

    
1. 크기 클래스를 사용 하는 것 처럼이 이미지 보기를 제한 해야 합니다. 이미지를 제약 조건 모드로 전환를 두 번 클릭 합니다. Center 고정 핸들을 클릭 하 여 센터로 제한 하 고 세로 및 가로로 정렬 하겠습니다.
    
    
    [![](ios-designable-controls-walkthrough-images/06new.png "이미지를 가운데 맞춤")](ios-designable-controls-walkthrough-images/06new.png)

    
1. 높이 너비를 제한 하려면 크기 고정 핸들 ('본' 모양 핸들) 클릭 하 고 너비와 높이 각각 선택:

    
    [![](ios-designable-controls-walkthrough-images/07new.png "제약 조건 추가")](ios-designable-controls-walkthrough-images/07new.png)


1. 도구 모음에서 [업데이트] 단추를 클릭 하 여 제약 조건에 따라 프레임을 업데이트 합니다.


    [![](ios-designable-controls-walkthrough-images/08new.png "제약 조건 도구 모음")](ios-designable-controls-walkthrough-images/08new.png)


1. 다음으로 프로젝트를 빌드합니다 있도록는 **티켓 보기 스크래치** 아래에 표시 될 **사용자 지정 구성 요소** 도구 상자에서:

    
    [![](ios-designable-controls-walkthrough-images/09new.png "사용자 지정 구성 요소로 도구 상자")](ios-designable-controls-walkthrough-images/09new.png)


1. 끌어서 놓기는 **티켓 보기 스크래치** 원숭이 이미지 위에 표시 되도록 합니다. 스크래치 티켓 보기에서는 원숭이 아래와 같이 완전히 끌기 핸들을 조정 합니다.

    
    [![](ios-designable-controls-walkthrough-images/10new.png "이미지 보기를 통해 스크래치 티켓 보기")](ios-designable-controls-walkthrough-images/10new.png)


1. 두 보기를 선택 하는 경계 사각형을 그려 스크래치 티켓 보기 이미지 보기를 제한 합니다. 아래와 같이 제약 조건에 따라 너비, 높이, 가운데 및 중간 및 업데이트 프레임을 제한 하는 옵션을 선택 합니다.
 
    
    [![](ios-designable-controls-walkthrough-images/11new.png "가운데 맞춤 및 제약 조건 추가")](ios-designable-controls-walkthrough-images/11new.png)


1. 응용 프로그램을 실행 하 고 "스크래치"는 원숭이 표시할 이미지입니다.


 [ ![](ios-designable-controls-walkthrough-images/10-app.png "샘플 응용 프로그램 실행")](ios-designable-controls-walkthrough-images/10-app.png)

## <a name="adding-design-time-properties"></a>디자인 타임 속성 추가

디자이너 속성 유형 숫자, 열거형, string, bool, CGSize, UIColor, 및 UIImage의 사용자 지정 컨트롤에 대 한 디자인 타임 지원을 포함 됩니다. 을 설명 하기 위해 추가 속성을는 `ScratchTicketView` "긁혔을 해제 합니다." 이미지를 설정 하려면

다음 코드를 추가 하는 `ScratchTicketView` 속성에 대 한 클래스:

```csharp
[Export("Image"), Browsable(true)]
public UIImage Image 
{
    get { return image; }
    set { 
            image = value;
            SetNeedsDisplay ();
        }
}
```

Null 검사를 추가 해야 할 수도 `Draw` 메서드를 다음과 같이 합니다.

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (var g = UIGraphics.GetCurrentContext())
    {
        if (image != null)
            g.SetFillColor ((UIColor.FromPatternImage (image).CGColor));
        else
            g.SetFillColor (UIColor.LightGray.CGColor);
            
        g.FillRect(rect);

        if (!initialPoint.IsEmpty)
        {
             g.SetLineWidth(20);
             g.SetBlendMode(CGBlendMode.Clear);
             UIColor.Clear.SetColor();

             if (path.IsEmpty || startNewPath)
             {
                 path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                 startNewPath = false;
             }
             else
             {
                 path.AddLineToPoint(latestPoint);
             }

             g.SetLineCap(CGLineCap.Round);
             g.AddPath(path);       
             g.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

포함 하는 `ExportAttribute` 및 `BrowsableAttribute` 인수를로 설정 `true` 디자이너에 표시 되는 속성으로 인해 **속성** 패널입니다. 다른 이미지와 같은 프로젝트에 포함 된 속성을 변경 `FillTexture2.png`, 아래와 같이 디자인 타임에 컨트롤 업데이트 발생:

 [ ![](ios-designable-controls-walkthrough-images/11-customproperty.png "디자인 타임 속성 편집")](ios-designable-controls-walkthrough-images/10-app.png)

## <a name="summary"></a>요약

이 문서에서 사용자 지정 컨트롤을 만들 수 있을 뿐 아니라 iOS 디자이너를 사용 하 여 iOS 응용 프로그램에서 사용 하는 방법을 단계별로 진행할 합니다. 컨트롤 디자이너에서 응용 프로그램에 사용할 수 있도록을 작성 하는 방법에 대해 살펴보았습니다 **도구 상자**합니다. 또한 디자이너에서 사용자 지정 컨트롤 속성을 노출 하는 방법 뿐 아니라 디자인 타임 및 런타임 모두에서 제대로 렌더링 되도록 컨트롤을 구현 하는 방법을 검토 합니다.



## <a name="related-links"></a>관련 링크

- [ScratchTicket (샘플)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [필요한 이미지 (샘플)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
