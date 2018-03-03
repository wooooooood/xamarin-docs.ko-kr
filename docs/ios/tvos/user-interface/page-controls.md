---
title: "페이지 컨트롤 사용"
description: "이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 페이지 제어를 사용한 작업을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d4da50dac901628b9baf10a07650d232a977a653
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-page-control"></a>페이지 컨트롤 사용

_이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 페이지 제어를 사용한 작업을 설명 합니다._

경우에 따라 Xamarin.tvOS 앱에 일련의 페이지 또는 이미지를 표시 해야 합니다. 페이지 컨트롤 명확 하 게는 사용자가 최대 페이지 수가 초과 하는 페이지를 표시 하도록 설계 되었습니다. 페이지 컨트롤에는 일련의 점 진한 합니다 타원형 배경 모양에 대해 표시 됩니다. 현재 페이지는 채워진된 점이 표시 됩니다, 그리고 빈 점으로 다른 모든 페이지를 표시 합니다. 페이지 컨트롤은 항목이 너무 많아의 배경 영역에 맞지 않을 경우 외부 대부분 점을 자릅니다.

[ ![](page-controls-images/page01.png "페이지 컨트롤 샘플")](page-controls-images/page01.png)

에 사용자에 게 피드백을 제공 하는 비 대화형 요소는 페이지 컨트롤입니다. 현재 페이지 번호 (예: 제스처 또는 단추)를 변경 하려면 다른 컨트롤을 추가 해야 합니다.

Apple는 페이지 컨트롤을 사용 하는 경우 다음 제안에 있습니다.

- **전체 컬렉션 에서만 사용 하 여** -단일 컬렉션에 존재 하는 여러 페이지를 표시 하는 전체 화면 환경에 가장 적합 한 페이지 컨트롤입니다.
- **페이지 수를 제한** -10 개 이하의 페이지가 고 최대 20 (20) 페이지에 페이지 컨트롤 가장 적합 합니다. 20 개 이상의 페이지를 사용 하는 것이 좋습니다는 [컬렉션 뷰](~/ios/tvos/user-interface/collection-views.md) 모눈에 페이지를 표시 하 고 있습니다.

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>페이지 컨트롤 및 스토리 보드

페이지 컨트롤 Xamarin.tvOS 응용 프로그램에서 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 응용 프로그램의 UI를에 추가 하는 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

    
1. 에 **솔루션 패드**, 두 번 클릭은 `Main.storyboard` 파일을 열어 편집 합니다.
1. 끌어서는 **페이지 컨트롤** 에서 **도구 상자** 보기에 놓습니다. 

    [ ![](page-controls-images/page02.png "페이지 컨트롤")](page-controls-images/page02.png)
1. 에 **위젯을 탭** 의 **속성 패드**을 같은 페이지 컨트롤의 몇 가지 속성을 조정할 수 있습니다 해당 **현재 페이지** 및 **페이지#**: 

    [ ![](page-controls-images/page03.png "위젯 탭")](page-controls-images/page03.png)
1. 그런 다음 페이지의 컬렉션을 통해 앞뒤로 이동 하는 보기를 컨트롤 또는 제스처를 추가 합니다.
1. 마지막으로 할당 **이름** 컨트롤에 C# 코드에서에 응답할 수 있도록 합니다. 예: 

    [ ![](page-controls-images/page04.png "컨트롤 이름 지정")](page-controls-images/page04.png)
1. 변경 내용을 저장합니다.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. 에 **솔루션 탐색기**, 두 번 클릭은 `Main.storyboard` 파일을 열어 편집 합니다.
1. 끌어서는 **페이지 컨트롤** 에서 **도구 상자** 보기에 놓습니다. 

    [ ![](page-controls-images/page02-vs.png "페이지 컨트롤")](page-controls-images/page02-vs.png)
1. 에 **위젯을 탭** 의 **속성 탐색기**을 같은 페이지 컨트롤의 몇 가지 속성을 조정할 수 있습니다 해당 **현재 페이지** 및 **페이지번호**: 

    [ ![](page-controls-images/page03-vs.png "위젯 탭")](page-controls-images/page03-vs.png)
1. 그런 다음 페이지의 컬렉션을 통해 앞뒤로 이동 하는 보기를 컨트롤 또는 제스처를 추가 합니다.
1. 마지막으로 할당 **이름** 컨트롤에 C# 코드에서에 응답할 수 있도록 합니다. 예: 

    [ ![](page-controls-images/page04-vs.png "컨트롤 이름 지정")](page-controls-images/page04-vs.png)
1. 변경 내용을 저장합니다.
    

-----

> [!IMPORTANT]
> **참고:** 이벤트를 할당할 수 있지만 `TouchUpInside` 를 UI 요소 (예: UIButton) iOS 디자이너에서에서이 호출 되지 것입니다 있으므로 Apple TV 터치 스크린 또는 터치 이벤트를 지원 하지 않습니다. 항상 사용 해야는 `Primary Action` 이벤트 사용자 인터페이스 요소 tvOS에 대 한 이벤트 처리기를 만들 때.




편집 뷰 컨트롤러 (예제 `ViewController.cs`) 파일을 변경 되는 페이지를 처리 하는 코드를 추가 합니다. 예:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public nint PageNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            PageView.Pages = 6;
            ShowCat ();
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void NextCat (UIBarButtonItem sender) {

            // Display next Cat
            if (++PageNumber > 5) {
                PageNumber = 5;
            }
            ShowCat();

        }

        partial void PreviousCat (UIBarButtonItem sender) {
            // Display previous cat
            if (--PageNumber < 0) {
                PageNumber = 0;
            }
            ShowCat();
        }
        #endregion

        #region Private Methods
        private void ShowCat() {

            // Adjust UI
            PreviousButton.Enabled = (PageNumber > 0);
            NextButton.Enabled = (PageNumber < 5);
            PageView.CurrentPage = PageNumber;

            // Display new cat
            CatView.Image = UIImage.FromFile(string.Format("Cat{0:00}.jpg",PageNumber+1));
        }
        #endregion
    }
}
```

페이지 컨트롤의 두 속성에 자세히 살펴보겠습니다 보겠습니다. 첫째, 최대 페이지 수를 지정 하려면 다음을 사용 합니다.

```csharp
PageView.Pages = 6;
```

현재 페이지 번호를 변경 하려면 다음 코드를 사용 합니다.

```csharp
PageView.CurrentPage = PageNumber;
```

`CurrentPage` 속성은 영 (0) 기반 이므로 첫 번째 페이지 수는 0 및 마지막 페이지의 최대 수는 제외 하나가 됩니다.

스토리 보드를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Hello, tvOS 퀵 스타트 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다. 

<a name="Summary" />

## <a name="summary"></a>요약

이 문서의 디자인 하 고 Xamarin.tvOS 앱 내에서 페이지 제어를 사용한 작업 검사가 수행 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
