---
title: Xamarin에서 tvOS 페이지 컨트롤 사용
description: 이 문서에서는 Xamarin을 사용 하 여 빌드된 앱에서 tvOS 페이지 컨트롤을 사용 하는 방법을 설명 합니다. 페이지 컨트롤에 대 한 개략적인 설명을 제공 하 고 스토리 보드를 설정 하는 방법에 대해 설명 하며 페이지 변경 이벤트에 응답 하는 방법을 검사 합니다.
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 8bb517eaa549567ae92695fbad300d055f42771f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769055"
---
# <a name="working-with-tvos-page-controls-in-xamarin"></a>Xamarin에서 tvOS 페이지 컨트롤 사용

TvOS 앱에 일련의 페이지 또는 이미지를 표시 해야 하는 경우가 있습니다. 페이지 컨트롤은 사용자가 최대 페이지 수를 벗어난 페이지를 명확 하 게 표시 하도록 디자인 되었습니다. 페이지 컨트롤은 진한 타원 모양의 배경에 대해 일련의 점을 표시 합니다. 현재 페이지에는 채워진 점이 표시 되 고 다른 모든 페이지는 빈 점으로 표시 됩니다. 페이지 컨트롤이 너무 많아 배경 영역에 맞출 수 없는 경우 가장 바깥쪽 점이 잘립니다.

[![](page-controls-images/page01.png "샘플 페이지 컨트롤")](page-controls-images/page01.png#lightbox)

사용자에 게 피드백을 제공 하도록 디자인 된 비 대화형 요소의 페이지 컨트롤입니다. 현재 페이지 번호 (예: 제스처 또는 단추)를 변경 하는 다른 컨트롤을 추가 해야 합니다.

페이지 컨트롤을 사용할 때 Apple에는 다음과 같은 제안이 있습니다.

- **전체 컬렉션에만 사용** -페이지 컨트롤은 전체 화면 환경에서 가장 잘 작동 하 여 단일 컬렉션에 존재 하는 여러 페이지를 표시 합니다.
- 페이지 페이지 컨트롤 **수 제한은** 10 개 이상의 페이지에서 가장 잘 작동 하 고 최대 20 개의 페이지에 대해 가장 잘 작동 합니다. 20 페이지 이상에서는 [컬렉션 뷰](~/ios/tvos/user-interface/collection-views.md) 를 사용 하 여 페이지를 모눈에 표시 하는 것이 좋습니다.

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>페이지 컨트롤 및 Storyboard

TvOS 앱에서 페이지 컨트롤을 사용 하는 가장 쉬운 방법은 iOS Designer를 사용 하 여 앱의 UI에 추가 하는 것입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 편집용으로 엽니다.
1. **도구 상자** 에서 **페이지 컨트롤** 을 끌어 뷰에 놓습니다.

    [![](page-controls-images/page02.png "페이지 컨트롤")](page-controls-images/page02.png#lightbox)
1. **Properties Pad**의 **위젯 탭** 에서 **현재 페이지** **및 페이지 수**와 같은 페이지 컨트롤의 몇 가지 속성을 조정할 수 있습니다.

    [![](page-controls-images/page03.png "위젯 탭")](page-controls-images/page03.png#lightbox)
1. 다음으로, 컨트롤 또는 제스처를 뷰에 추가 하 여 페이지의 컬렉션에서 앞뒤로 이동 합니다.
1. 마지막으로, 코드에서 C# 이에 응답할 수 있도록 컨트롤에 이름을 할당 합니다. 예를 들어:

    [![](page-controls-images/page04.png "컨트롤 이름")](page-controls-images/page04.png#lightbox)
1. 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 편집용으로 엽니다.
1. **도구 상자** 에서 **페이지 컨트롤** 을 끌어 뷰에 놓습니다.

    [![](page-controls-images/page02-vs.png "페이지 컨트롤")](page-controls-images/page02-vs.png#lightbox)
1. **속성 탐색기**의 **위젯 탭** 에서 **현재 페이지** **및 페이지 수**와 같은 페이지 컨트롤의 여러 속성을 조정할 수 있습니다.

    [![](page-controls-images/page03-vs.png "위젯 탭")](page-controls-images/page03-vs.png#lightbox)
1. 다음으로, 컨트롤 또는 제스처를 뷰에 추가 하 여 페이지의 컬렉션에서 앞뒤로 이동 합니다.
1. 마지막으로, 코드에서 C# 이에 응답할 수 있도록 컨트롤에 이름을 할당 합니다. 예:

    [![](page-controls-images/page04-vs.png "컨트롤 이름")](page-controls-images/page04-vs.png#lightbox)
1. 변경 내용을 저장합니다.

-----

> [!IMPORTANT]
> IOS 디자이너에서 uibutton 등의 UI 요소 `TouchUpInside` 에와 같은 이벤트를 할당할 수 있지만, Apple TV에 터치 스크린이 없거나 터치 이벤트가 지원 되기 때문에 호출 되지 않습니다. TvOS 사용자 인터페이스 요소에 `Primary Action` 대 한 이벤트 처리기를 만들 때 항상 이벤트를 사용 해야 합니다.

뷰 컨트롤러 (예제 `ViewController.cs`) 파일을 편집 하 고 변경 되는 페이지를 처리 하는 코드를 추가 합니다. 예를 들어:

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

`CurrentPage` 속성은 0부터 시작 하므로 첫 번째 페이지는 0이 고 마지막 페이지는 최대 페이지 수에서 1을 뺀 값이 됩니다.

스토리 보드 사용에 대 한 자세한 내용은 [Hello, tvOS 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)를 참조 하세요.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱 내에서 페이지 컨트롤을 디자인 하 고 작업 하는 방법에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
