---
title: TvOS Xamarin에서 페이지 컨트롤 사용
description: 이 문서에서는 Xamarin을 사용 하 여 빌드한 앱에서 tvOS 페이지 컨트롤을 사용 하는 방법을 설명 합니다. 페이지 컨트롤의 개괄적으로 설명, 스토리 보드에서 설정 하는 방법에 설명 하 고 페이지 변경 이벤트에 응답 하는 방법을 검사 합니다.
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 173bc7713b5b8c330d4d4c5863bef24be8bdcb52
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107471"
---
# <a name="working-with-tvos-page-controls-in-xamarin"></a>TvOS Xamarin에서 페이지 컨트롤 사용

경우에 따라 Xamarin.tvOS 앱에 일련의 페이지 또는 이미지를 표시 해야 합니다. 페이지 컨트롤을 명확 하 게 최대 페이지 수 중에서 사용자가 페이지를 표시 하도록 설계 되었습니다. 페이지 컨트롤에는 일련의 점으로 진한, oval 백그라운드 모양에 대해 표시 됩니다. 현재 페이지는 채워진된 점이 표시 됩니다, 그리고 다른 모든 페이지 속이 빈 점으로 표시 됩니다. 페이지 컨트롤은 해당 배경 영역에 맞게 너무 많이 있는 경우 외부 대부분의 점을 자릅니다.

[![](page-controls-images/page01.png "샘플 페이지 컨트롤")](page-controls-images/page01.png#lightbox)

만 사용자에 게 피드백을 제공 하도록 설계 된 비 대화형 요소에는 페이지 컨트롤입니다. 현재 페이지 번호 (예: 제스처 또는 단추)를 변경 하려면 다른 컨트롤을 추가 해야 합니다.

Apple는 페이지 컨트롤을 사용 하는 경우 다음 제안에 있습니다.

- **전체 컬렉션 에서만 사용 하 여** -페이지 컨트롤을 단일 컬렉션에 존재 하는 여러 페이지를 표시 하는 전체 화면 환경에서 가장 잘 작동 합니다.
- **페이지 수를 제한** -페이지 컨트롤에 가장 적합 한 10 개 이하의 페이지 및 20 (20) 페이지의 최대입니다. 20 개 이상의 페이지를 사용 하는 것이 좋습니다는 [컬렉션 뷰](~/ios/tvos/user-interface/collection-views.md) 모눈에 페이지를 표시 하 고 있습니다.

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>페이지 컨트롤 및 스토리 보드

Xamarin.tvOS 앱에서 페이지 컨트롤을 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 앱의 UI에 추가할 경우

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

    
1. 에 **Solution Pad**를 두 번 클릭 합니다 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 끌어서를 **페이지 컨트롤** 에서 **도구 상자** 보기에 놓습니다. 

    [![](page-controls-images/page02.png "페이지 컨트롤")](page-controls-images/page02.png#lightbox)
1. 에 **위젯을 탭** 의 합니다 **Properties Pad**와 같은 페이지 컨트롤의 여러 속성을 조정할 수 있습니다 해당 **현재 페이지** 및 **페이지&#40;**: 

    [![](page-controls-images/page03.png "위젯 탭")](page-controls-images/page03.png#lightbox)
1. 다음으로 페이지의 컬렉션을 통해 앞뒤로 이동 보기로 컨트롤 또는 제스처를 추가 합니다.
1. 마지막으로 할당 **이름을** 컨트롤에 대응할 수 있도록 C# 코드입니다. 예를 들어: 

    [![](page-controls-images/page04.png "컨트롤 이름 지정")](page-controls-images/page04.png#lightbox)
1. 변경 내용을 저장합니다.
    

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    
1. 에 **솔루션 탐색기**를 두 번 클릭 합니다 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 끌어서를 **페이지 컨트롤** 에서 **도구 상자** 보기에 놓습니다. 

    [![](page-controls-images/page02-vs.png "페이지 컨트롤")](page-controls-images/page02-vs.png#lightbox)
1. 에 **위젯을 탭** 의 **속성 탐색기**, 같은 페이지 컨트롤의 여러 속성을 조정할 수 있습니다 해당 **현재 페이지** 및 **페이지수**: 

    [![](page-controls-images/page03-vs.png "위젯 탭")](page-controls-images/page03-vs.png#lightbox)
1. 다음으로 페이지의 컬렉션을 통해 앞뒤로 이동 보기로 컨트롤 또는 제스처를 추가 합니다.
1. 마지막으로 할당 **이름을** 컨트롤에 대응할 수 있도록 C# 코드입니다. 예를 들어: 

    [![](page-controls-images/page04-vs.png "컨트롤 이름 지정")](page-controls-images/page04-vs.png#lightbox)
1. 변경 내용을 저장합니다.
    

-----

> [!IMPORTANT]
> 와 같은 이벤트를 할당할 수 있지만 `TouchUpInside` UI 요소에 (예: UIButton) iOS 디자이너에서에서이 호출 되지 것입니다 Apple TV 화면 또는 터치 이벤트를 지 원하는 터치 없기 때문입니다. 항상 사용 해야 하는 `Primary Action` 이벤트 사용자 인터페이스 요소 tvOS에 대 한 이벤트 처리기를 만들 때.

편집 보기 컨트롤러 (예제 `ViewController.cs`) 파일을 변경할 페이지를 처리 하는 코드를 추가 합니다. 예를 들어:

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

페이지 컨트롤의 두 속성에 대해 좀 더 자세히 살펴보겠습니다. 먼저 최대 페이지 수를 지정 하려면 다음을 사용 합니다.

```csharp
PageView.Pages = 6;
```

현재 페이지 번호를 변경 하려면 다음 코드를 사용 합니다.

```csharp
PageView.CurrentPage = PageNumber;
```

`CurrentPage` 속성은 영 (0) 기반 이므로 첫 번째 페이지는 0이 된 후 마지막 페이지의 최대 수를 뺀 하나가 됩니다.

스토리 보드를 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Tvos 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다. 

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 페이지 컨트롤을 사용 하 여 작업 검사가 수행 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
