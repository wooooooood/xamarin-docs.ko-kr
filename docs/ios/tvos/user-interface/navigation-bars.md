---
title: TvOS 탐색 모음에서 Xamarin 사용 하 여 작업
description: 이 문서에서는 Xamarin에 내장 된 tvOS 앱에서 탐색 모음을 사용 하는 방법을 설명 합니다. 스토리 보드의 탐색 모음을 설정 하 고 이러한 단추에서 이벤트에 응답에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 81e6cfe1e532bcfa7616e35adb28b314587bafc8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61161783"
---
# <a name="working-with-tvos-navigation-bars-in-xamarin"></a>TvOS 탐색 모음에서 Xamarin 사용 하 여 작업

제목과 선택적 탐색 표시줄 단추에 표시할 보기의 맨 위에 탐색 모음을 추가할 수 있습니다. 일반적으로 테이블 뷰, 컬렉션 또는 하위 뷰는 선택한 항목의 세부 정보를 보여 주는 메뉴와 같은 기본 페이지에서 사용자가 탐색 하는 경우 사용 됩니다.

[![](navigation-bars-images/navbar01.png "샘플 탐색 모음")](navigation-bars-images/navbar01.png#lightbox)

제목에 (가운데에 표시 됨) 또한 탐색 모음에 하나 이상의 탐색 모음 단추의 포함 될 수 있습니다 (`UIBarButtonItem`) 막대의 왼쪽과 오른쪽에 있습니다.

> [!IMPORTANT]
> 탐색 막대는 기본적으로 완전히 투명 하 게 됩니다. 콘텐츠의 탐색 모음 아래에 있는 콘텐츠에 대 읽을 수 있는 유지 되도록 주의 해야 합니다. 예를 들어 경우 테이블 뷰 또는 컬렉션의 콘텐츠 스크롤 아래입니다.

<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>탐색 모음 및 스토리 보드

Xamarin.tvOS 앱에서 탐색 모음을 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 앱의 UI에 추가할 경우

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 에 **Solution Pad**를 두 번 클릭 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 끌어서를 **탐색 모음** 에서 **도구 상자** 화면 맨 위에 있는 보기에 놓습니다. 

    [![](navigation-bars-images/navbar02.png "탐색 모음")](navigation-bars-images/navbar02.png#lightbox)
1. 두 번 클릭 합니다 **탐색 모음** 하기 위해 선택할 **탐색 항목**합니다. 에 **위젯** 탭의 **Properties Pad**, 설정할 수 있습니다는 **제목**: 

    [![](navigation-bars-images/navbar03.png "제목 설정")](navigation-bars-images/navbar03.png#lightbox)
1. 다음으로, 하나 이상의 추가할 수 있습니다 **막대 단추 항목** 막대의 양쪽 끝에: 

    [![](navigation-bars-images/navbar04.png "막대가 표시 단추 항목입니다.")](navigation-bars-images/navbar04.png#lightbox)
1. 마지막으로 통신 하는 **막대 단추 항목** 작업를 **이벤트** 탭의 **속성 탐색기**: 

    [![](navigation-bars-images/navbar05.png "막대가 표시 단추 항목 작업")](navigation-bars-images/navbar05.png#lightbox)
1. 변경 내용을 저장합니다.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


1. 에 **솔루션 탐색기**를 두 번 클릭 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 끌어서를 **탐색 모음** 에서 **도구 상자** 화면 맨 위에 있는 보기에 놓습니다. 

    [![](navigation-bars-images/navbar02-vs.png "탐색 모음")](navigation-bars-images/navbar02-vs.png#lightbox)
1. 두 번 클릭 합니다 **탐색 모음** 하기 위해 선택할 **탐색 항목**합니다. 에 **위젯** 탭의 **속성 탐색기**, 설정할 수 있습니다는 **제목**: 

    [![](navigation-bars-images/navbar03-vs.png "제목 설정")](navigation-bars-images/navbar03-vs.png#lightbox)
1. 다음으로, 하나 이상의 추가할 수 있습니다 **막대 단추 항목** 막대의 양쪽 끝에: 

    [![](navigation-bars-images/navbar04-vs.png "막대가 표시 단추 항목")](navigation-bars-images/navbar04-vs.png#lightbox)
1. 마지막으로 통신 하는 **막대 단추 항목** 작업를 **이벤트** 탭의 **속성 탐색기**: 

    [![](navigation-bars-images/navbar05-vs.png "막대가 표시 단추 항목 작업")](navigation-bars-images/navbar05-vs.png#lightbox)
1. 변경 내용을 저장합니다.


-----

> [!IMPORTANT]
> 와 같은 이벤트를 할당할 수 있지만 `TouchUpInside` UI 요소에 (예: UIButton) iOS 디자이너에서에서이 호출 되지 것입니다 Apple TV 화면 또는 터치 이벤트를 지 원하는 터치 없기 때문입니다. 항상 사용 해야 하는 `Primary Action` 이벤트 사용자 인터페이스 요소 tvOS에 대 한 이벤트 처리기를 만들 때.

다음 코드는 세 가지 다른 BarButtonItems에서 이벤트 처리기의 예제를 제공 합니다: `ShowFirstHotel`, `ShowSecondHotel`, 및 `ShowThirdHotel`합니다. 각 항목은 클릭 하면, 배경 이미지 `HotelImage` 변경 됩니다. 이 뷰 컨트롤러에서 편집 (예제 `ViewController.cs`) 파일:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void ShowFirstHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel01.jpg");
        }

        partial void ShowSecondHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel02.jpg");
        }

        partial void ShowThirdHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel03.jpg");
        }
        #endregion
    }
}
```

단추의 오랫동안 `Enabled` 속성은 `true` 다른 컨트롤이 나 보기에서 다루지 않은, 포커스 내 항목 Siri 원격을 사용 하 여 만들 수 있습니다.

스토리 보드를 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Tvos 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다. 

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 탐색 모음을 사용 하 여 작업 검사가 수행 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
