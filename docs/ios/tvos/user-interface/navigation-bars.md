---
title: "탐색 컨트롤러 작업"
description: "이 문서에서는 디자인과 Xamarin.tvOS 앱 내에서 탐색 모음 작업을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 0f59e8f5e732a45f7e6148a08de80fffc56dbb26
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-navigation-controllers"></a>탐색 컨트롤러 작업

_이 문서에서는 디자인과 Xamarin.tvOS 앱 내에서 탐색 모음 작업을 설명 합니다._

제목과 선택적 탐색 모음 단추를 표시 하는 보기의 맨 위에 탐색 모음을 추가할 수 있습니다. 일반적으로 테이블 뷰, 컬렉션 또는 하위 선택한 항목의 세부 정보를 보여 주는 뷰 메뉴와 같은 기본 페이지에서 사용자가 탐색 하는 경우 사용 됩니다.

[![](navigation-bars-images/navbar01.png "샘플 탐색 모음")](navigation-bars-images/navbar01.png#lightbox)

또한 제목 (가운데에 표시)에 탐색 모음에 하나 이상의 탐색 모음 단추가 포함 될 수 있습니다 (`UIBarButtonItem`) 막대의 왼쪽과 오른쪽에 있습니다.

> [!IMPORTANT]
> 탐색 모음은 기본적으로 완전히 투명 합니다. 유지 하 고 탐색 모음의 내용을 읽을 수 있는 그 아래에 있는 내용을 주의 해야 합니다. 예를 들어 때 표 보기 또는 컬렉션의 콘텐츠 스크롤 그 아래에서입니다.




<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>스토리 보드와 탐색 모음

탐색 모음 Xamarin.tvOS 응용 프로그램에서 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 응용 프로그램의 UI를에 추가 하는 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


1. 에 **솔루션 패드**를 두 번 클릭 `Main.storyboard` 파일을 열어 편집 합니다.
1. 끌어서는 **탐색 모음** 에서 **도구 상자** 화면 맨 위에 있는 보기에 놓습니다. 

    [![](navigation-bars-images/navbar02.png "탐색 모음")](navigation-bars-images/navbar02.png#lightbox)
1. 두 번 클릭은 **탐색 모음** 하기 위해 선택 **탐색 항목**합니다. 에 **위젯** 탭은 **속성 패드**를 설정할 수 있습니다는 **제목**: 

    [![](navigation-bars-images/navbar03.png "제목 설정")](navigation-bars-images/navbar03.png#lightbox)
1. 하나 이상을 추가할 수는 다음으로 **표시줄 단추 항목** 막대의 양쪽 끝에: 

    [![](navigation-bars-images/navbar04.png "막대 단추 항목 A")](navigation-bars-images/navbar04.png#lightbox)
1. 마지막으로, 유선 접속은 **표시줄 단추 항목** 작업에 **이벤트** 탭은 **속성 탐색기**: 

    [![](navigation-bars-images/navbar05.png "단추 항목 동작 가로 막대")](navigation-bars-images/navbar05.png#lightbox)
1. 변경 내용을 저장합니다.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. 에 **솔루션 탐색기**를 두 번 클릭 `Main.storyboard` 파일을 열어 편집 합니다.
1. 끌어서는 **탐색 모음** 에서 **도구 상자** 화면 맨 위에 있는 보기에 놓습니다. 

    [![](navigation-bars-images/navbar02-vs.png "탐색 모음")](navigation-bars-images/navbar02-vs.png#lightbox)
1. 두 번 클릭은 **탐색 모음** 하기 위해 선택 **탐색 항목**합니다. 에 **위젯** 탭은 **속성 탐색기**를 설정할 수 있습니다는 **제목**: 

    [![](navigation-bars-images/navbar03-vs.png "제목 설정")](navigation-bars-images/navbar03-vs.png#lightbox)
1. 하나 이상을 추가할 수는 다음으로 **표시줄 단추 항목** 막대의 양쪽 끝에: 

    [![](navigation-bars-images/navbar04-vs.png "단추 항목 가로 막대")](navigation-bars-images/navbar04-vs.png#lightbox)
1. 마지막으로, 유선 접속은 **표시줄 단추 항목** 작업에 **이벤트** 탭은 **속성 탐색기**: 

    [![](navigation-bars-images/navbar05-vs.png "단추 항목 동작 가로 막대")](navigation-bars-images/navbar05-vs.png#lightbox)
1. 변경 내용을 저장합니다.


-----

> [!IMPORTANT]
> 이벤트를 할당할 수 있지만 `TouchUpInside` 를 UI 요소 (예: UIButton) iOS 디자이너에서에서이 호출 되지 것입니다 있으므로 Apple TV 터치 스크린 또는 터치 이벤트를 지원 하지 않습니다. 항상 사용 해야는 `Primary Action` 이벤트 사용자 인터페이스 요소 tvOS에 대 한 이벤트 처리기를 만들 때.




다음 코드는 세 가지 다른 BarButtonItems에 이벤트 처리기의 예를 제공: `ShowFirstHotel`, `ShowSecondHotel`, 및 `ShowThirdHotel`합니다. 각 항목을 클릭할 때, 배경 이미지 `HotelImage` 변경 됩니다. 이 뷰 컨트롤러에서 편집 (예제 `ViewController.cs`) 파일:

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

단추의 같으면 `Enabled` 속성은 `true` 다른 컨트롤이 나 보기에서 다루지 않는 하 고, 포커스 항목 Siri 원격을 사용 하 여 만들 수 있습니다.

스토리 보드를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Hello, tvOS 퀵 스타트 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다. 

<a name="Summary" />

## <a name="summary"></a>요약

이 문서 디자인 Xamarin.tvOS 앱 내에서 탐색 모음을 사용 하 고 검사가 수행 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
