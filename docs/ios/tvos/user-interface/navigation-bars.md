---
title: Xamarin에서 tvOS 탐색 모음 사용
description: 이 문서에서는 Xamarin을 사용 하 여 빌드된 tvOS 앱에서 탐색 모음으로 작업 하는 방법을 설명 합니다. 스토리 보드에 탐색 모음을 설정 하 고 이러한 단추에서 이벤트에 응답 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: f33e3f4f27afe7e0b98c4a1f71d77c4a1c7195ac
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289075"
---
# <a name="working-with-tvos-navigation-bars-in-xamarin"></a>Xamarin에서 tvOS 탐색 모음 사용

탐색 모음은 보기의 맨 위에 추가 하 여 제목 및 선택적 탐색 모음 단추를 표시할 수 있습니다. 일반적으로 사용자가 기본 페이지 (예: 테이블 뷰, 컬렉션 또는 메뉴)에서 선택한 항목의 세부 정보를 표시 하는 하위 뷰로 이동할 때 사용 됩니다.

[![](navigation-bars-images/navbar01.png "샘플 탐색 모음")](navigation-bars-images/navbar01.png#lightbox)

가운데에 표시 되는 제목 외에도 탐색 모음에는 막대의 왼쪽과 오른쪽에 하나 이상의 탐색 모음 단추 (`UIBarButtonItem`)가 포함 될 수 있습니다.

> [!IMPORTANT]
> 탐색 모음은 기본적으로 완전히 투명 합니다. 탐색 모음의 콘텐츠는 그 아래에 있는 내용에서 읽을 수 있도록 유지 해야 합니다. 예를 들어, 테이블 뷰나 컬렉션의 내용이 그 아래에 스크롤될 때입니다.

<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>탐색 모음 및 Storyboard

TvOS 앱에서 탐색 모음으로 작업 하는 가장 쉬운 방법은 iOS Designer를 사용 하 여 앱의 UI에 추가 하는 것입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**에서 파일을 두 번 클릭 `Main.storyboard` 하 여 편집용으로 엽니다.
1. **도구 상자** 에서 **탐색 모음** 을 끌어 화면 위쪽의 뷰에 놓습니다.

    [![](navigation-bars-images/navbar02.png "탐색 모음")](navigation-bars-images/navbar02.png#lightbox)
1. 탐색 **모음** 을 두 번 클릭 하 여 **탐색 항목**을 선택 합니다. **Properties Pad**의 **위젯** 탭에서 **제목을**설정할 수 있습니다.

    [![](navigation-bars-images/navbar03.png "제목 설정")](navigation-bars-images/navbar03.png#lightbox)
1. 다음으로 막대의 한쪽 끝에 하나 이상의 **막대 단추 항목** 을 추가할 수 있습니다.

    [![](navigation-bars-images/navbar04.png "막대 단추 항목")](navigation-bars-images/navbar04.png#lightbox)
1. 마지막으로 **속성 탐색기**의 **이벤트** 탭에 있는 작업에 **막대 단추 항목** 을 연결 합니다.

    [![](navigation-bars-images/navbar05.png "막대 단추 항목 동작")](navigation-bars-images/navbar05.png#lightbox)
1. 변경 내용을 저장합니다.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


1. **솔루션 탐색기**에서 파일을 두 번 클릭 `Main.storyboard` 하 여 편집용으로 엽니다.
1. **도구 상자** 에서 **탐색 모음** 을 끌어 화면 위쪽의 뷰에 놓습니다.

    [![](navigation-bars-images/navbar02-vs.png "탐색 모음")](navigation-bars-images/navbar02-vs.png#lightbox)
1. 탐색 **모음** 을 두 번 클릭 하 여 **탐색 항목**을 선택 합니다. **속성 탐색기**의 **위젯** 탭에서 **제목을**설정할 수 있습니다.

    [![](navigation-bars-images/navbar03-vs.png "제목 설정")](navigation-bars-images/navbar03-vs.png#lightbox)
1. 다음으로 막대의 한쪽 끝에 하나 이상의 **막대 단추 항목** 을 추가할 수 있습니다.

    [![](navigation-bars-images/navbar04-vs.png "막대 단추 항목")](navigation-bars-images/navbar04-vs.png#lightbox)
1. 마지막으로 **속성 탐색기**의 **이벤트** 탭에 있는 작업에 **막대 단추 항목** 을 연결 합니다.

    [![](navigation-bars-images/navbar05-vs.png "막대 단추 항목 동작")](navigation-bars-images/navbar05-vs.png#lightbox)
1. 변경 내용을 저장합니다.


-----

> [!IMPORTANT]
> IOS 디자이너에서 uibutton 등의 UI 요소 `TouchUpInside` 에와 같은 이벤트를 할당할 수 있지만, Apple TV에 터치 스크린이 없거나 터치 이벤트가 지원 되기 때문에 호출 되지 않습니다. TvOS 사용자 인터페이스 요소에 `Primary Action` 대 한 이벤트 처리기를 만들 때 항상 이벤트를 사용 해야 합니다.

다음 코드는 `ShowFirstHotel`, `ShowSecondHotel`및 `ShowThirdHotel`의 세 가지 다른 바 buttonitems에 대 한 이벤트 처리기의 예제를 제공 합니다. 각 항목을 클릭 하면 배경 이미지가 `HotelImage` 변경 됩니다. 이는 뷰 컨트롤러 (예제 `ViewController.cs`) 파일에서 편집 됩니다.

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

단추의 `Enabled` 속성이이 `true` 고 다른 컨트롤 또는 뷰에서 포함 되지 않는 한, siri 원격을 사용 하 여 포커스 내 항목으로 만들 수 있습니다.

스토리 보드 사용에 대 한 자세한 내용은 [Hello, tvOS 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)를 참조 하세요.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱 내에서 탐색 모음을 디자인 하 고 사용 하는 방법에 대해 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
