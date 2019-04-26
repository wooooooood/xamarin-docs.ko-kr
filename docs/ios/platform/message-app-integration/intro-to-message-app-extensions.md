---
title: Xamarin.iOS에서 메시지 앱 확장 기본 사항
description: 이 문서에서는 하는 방법을 보여 줍니다 메시지 앱 통합 및 사용자에 게 새 기능을 제공 하는 Xamarin.iOS 솔루션에서 메시지 앱 확장을을 포함 합니다.
ms.prod: xamarin
ms.assetid: 0CFB494C-376C-449D-B714-9E82644F9DA3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: 7bd4a87843852e940da96f688371ddbecbf7e0b4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61153981"
---
# <a name="message-app-extension-basics-in-xamarinios"></a>Xamarin.iOS에서 메시지 앱 확장 기본 사항

_이 문서에서는 하는 방법을 보여 줍니다 메시지 앱 통합 및 사용자에 게 새 기능을 제공 하는 Xamarin.iOS 솔루션에서 메시지 앱 확장을을 포함 합니다._

새 메시지 앱 확장을 통합 하 여 iOS 10, 합니다 **메시지** 사용자에 게 새로운 기능을 앱 및 표시 합니다. 확장은 텍스트, 스티커, 미디어 파일 및 대화형 메시지를 보낼 수 있습니다.

## <a name="about-message-app-extensions"></a>메시지 앱 확장에 대 한

메시지 앱 확장을 통합 위에서 설명한 대로 합니다 **메시지** 사용자에 게 새로운 기능을 앱 및 표시 합니다. 확장은 텍스트, 스티커, 미디어 파일 및 대화형 메시지를 보낼 수 있습니다. 두 유형의 메시지 앱 확장을 사용할 수 있습니다.

- **스티커 팩** -스티커 사용자 메시지를 추가할 수 있는 컬렉션을 포함 합니다. 코드 작성 없이 스티커 팩을 만들 수 있습니다.
- **iMessage 앱** -스티커를 선택 하면, 텍스트를 입력, 선택적 형식 변환) (로 미디어 파일을 포함 하 여 및 만들기, 편집 및 상호 작용 메시지를 보내기에 대 한 메시지 앱 내에서 사용자 지정 사용자 인터페이스를 제공할 수 있습니다.

메시지 앱 확장에는 세 가지 기본 콘텐츠 형식을 제공합니다.

- **대화형 메시지** -는 앱을 생성 하는 사용자 지정 메시지 콘텐츠 유형, 메시지를 앱에 사용자가 누를 때 전경에서 시작 됩니다.
- **스티커** -이미지 사용자 간에 전송 된 메시지에 포함 될 수 있는 앱에서 생성 됩니다.
- **기타 지원 콘텐츠** -앱 사진, 비디오, 텍스트 등의 콘텐츠를 제공할 수 있습니다 또는 다른 콘텐츠에 대 한 링크 형식에 항상 메시지 앱에서 지원 됩니다.

새 ios 10, 메시지 앱 이제 자체 전용, 기본 제공 앱 스토어입니다. 메시지 앱 확장을 포함 하는 모든 앱을 표시 하 고이 저장소에서 승격 됩니다. 새 메시지 앱 서랍 사용자에 게 빠르게 액세스할 수 있도록 메시지 앱 스토어에서 다운로드 한 모든 앱에 표시 됩니다.

또한 새 ios 10에서 Apple이 추가 인라인 앱 Attribution 사용자 앱을 쉽게 검색할 수 있습니다. 예를 들어, 하나의 사용자가 두 번째 사용자에 게는 앱에서 다른 콘텐츠를 전송 (예: 스티커 예)를 설치 보내는 앱의 이름 아래 있는지 메시지 기록에서 콘텐츠. 사용자가 앱의 경우 이름, 열 수 것 메시지 앱 스토어 및 스토어에서 선택한 앱.

메시지 앱 확장 기존 iOS 앱을 만들기에 익숙한 개발자는 모든 표준 프레임 워크 및 표준 iOS 앱의 기능에 액세스 해야 하는 것과 비슷합니다. 예를 들어:

- 인 앱 구매에 액세스할을 수 있습니다.
- Apple Pay에 액세스할을 수 있습니다.
- 카메라 같은 장치 하드웨어에 액세스할을 수 있습니다.

그러나 메시지 앱 확장은 iOS 10에서 지원만, 이러한 확장을 전송 하는 콘텐츠는 watchOS 및 macOS 장치에서 볼 수 있습니다. 새 _최근 페이지_ 에서 메시지 앱 확장을 포함 하 여 휴대폰에서 보낸 최신 스티커 표시 되며 사용자 watch에서 해당 스티커를 보낼 수 있도록 watchOS 3에 추가 합니다.

## <a name="about-the-messages-framework"></a>메시지 프레임 워크에 대 한

새 ios 10, 메시지 프레임 워크는 사용자의 iOS 장치에서 메시지 앱 확장 및 메시지 앱 간의 인터페이스를 제공 합니다. 사용자 메시지 앱 내에서 앱이 시작 될 때이 프레임 워크는 앱이 검색 될 수 있게 하 고 데이터와 UI를 레이아웃 하는 데 필요한 컨텍스트를 제공 합니다.

앱이 시작 되 면 새 메시지를 통해 공유할 콘텐츠를 만들려면를 사용 하 여 사용자 상호 작용 합니다. 앱 다음 메시지 프레임 워크를 사용 하 여 처리를 위해 메시지 앱에 새로 만든된 콘텐츠를 전송 하도록 합니다.

메시지 프레임 워크 및 메시지 앱 확장은 기존 iOS 앱 확장 기술 위에 구축 됩니다. 앱 확장에 대 한 자세한 내용은 Apple의를 참조 하세요 [앱 확장의 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)합니다.

시스템 전체에서 Apple이 제공한 다른 확장 점을 달리 개발자는 자체 메시지 앱 컨테이너 역할을 하는 이후가 메시지 앱 확장에 대 한 호스트 앱을 제공 필요가 없습니다. 그러나 개발자가 기존 또는 새 iOS 앱 내에서 메시지 앱 확장을 포함 하 고 번들 함께 전달 합니다.

메시지 앱 확장 되는 iOS 앱의 번들에 포함 하는 경우 장치의 홈 화면에서와 메시지 앱 내에서 메시지 앱 서랍에서 앱의 아이콘이 표시 됩니다. 앱 번들에 포함 되지 않으면, 메시지 앱 확장의 메시지 앱 서랍에서만 표시 됩니다.

메시지 앱 확장 호스트 앱 번들에 포함 되지 않은, 경우에 개발자 제공 해야 메시지 앱 확장의 번들의 앱 아이콘으로 이것이 메시지 앱 서랍 또는 설정과 같은 시스템의 다른 부분에 표시 되는 아이콘 를 확장 합니다.

## <a name="about-stickers"></a>스티커에 대 한

Apple 다른 메시지 내용으로 인라인 보낼 iMessage 스티커를 허용 하 여 통신 하는 사용자에 대 한 새로운 방법으로 스티커를 디자인 하거나 대화 내에서 이전 메시지 거품에 연결할 수 있습니다.

스티커는 무엇 인가요?

- 이들은 메시지 앱 확장을 제공 하는 이미지입니다.
- 애니메이션 또는 정적 이미지 수 있습니다.
- 앱 내에서 이미지 콘텐츠를 공유 하는 새로운 방법을 제공 합니다.

두 가지 방법으로 스티커를 만들려면:

1. 스티커 팩 메시지 앱 확장을 만들 수 있습니다에서 Xcode 내부에서 코드를 포함 하지 않고 있습니다. 스티커 및 앱 아이콘에 대 한 자산 이며 반드시 필요한 것
2. 표준 메시지 앱 확장을 만들어 스티커 메시지 프레임 워크를 통해 코드에서 제공 하는 합니다.

### <a name="creating-sticker-packs"></a>스티커 팩 만들기

스티커 팩 Xcode 내부에서 특별 한 템플릿에서 생성 되 고 스티커도 사용할 수 있는 이미지 자산의 고정 집합을 제공 하기만 하면 됩니다. 위에서 설명한 대로 코드 필요가 없으므로, 개발자 스티커 자산 카탈로그 내에서 스티커 팩 폴더에 이미지 파일을 간단히 끌 합니다.

스티커 팩에 포함할 이미지를 다음 요구 사항을 충족 해야 합니다.

- 이미지는 PNG, APNG, GIF 또는 JPEG 형식 이어야 합니다. Apple에서 스티커 자산을 제공할 때 PNG 및 APNG 형식만 사용 하 여 제안 합니다.
- 애니메이션된 스티커 APNG와 GIF 형식을 지원합니다.
- 사용자가 대화의 메시지 풍선 위에 배치 될 수 있으므로 스티커 이미지에서 투명 한 배경을 제공 해야 합니다.
- 개별 이미지 파일에는 500kb 보다 적은 수 있어야 합니다.
- 이미지 수 없습니다 100 x 100 포인트 보다 작거나 큰 206 x 206 가리키는.

> [!IMPORTANT]
> 스티커 이미지에서 항상 제공 해야 합니다 `@3x` 300 x 300을 618 x 618 픽셀 범위에서 확인 합니다. 시스템에서 자동으로 생성 합니다 `@2x` 및 `@1x` 필요에 따라 런타임에 버전입니다.

Apple에서 제안 가능한 모든 상황에 가장 적합 한지 확인 하기 위해 다양 한 서로 다른 색이 지정 된 배경에 (예: 백서, black, 빨강, 노랑 및 다중 컬러) 및 통해 사진에 대 한 된 스티커 이미지 자산을 테스트 하 게 합니다.

스티커 팩 사용 가능한 세 가지 크기 중 하나에서 스티커를 제공할 수 있습니다.

- **작은** -100 x 100 포인트입니다.
- **중간** -136 x 136 지점입니다. 기본 크기입니다.
- **큰** -206 x 206 지점입니다.

Xcode의 특성 검사기를 사용 하 여 전체 스티커 팩에 대 한 크기를 설정 하 고 최상의 결과 메시지 앱 내에서 스티커 브라우저에서 요청된 된 크기와 일치 하는 이미지 자산 제공.

자세한 내용은 참조 하십시오 우리의 [Ice cream 작성기](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) 앱과 Apple [메시지 참조](https://developer.apple.com/reference/messages)합니다.

## <a name="creating-a-custom-sticker-experience"></a>사용자 지정 스티커 환경 만들기

앱에 더 많은 컨트롤 또는 스티커 팩에서 제공 하는 것 보다 유연 하 게가 필요한 경우 수 메시지 앱 확장을 포함 하 고 사용자 지정 스티커 환경에 대 한 메시지 프레임 워크를 통해 스티커를 제공 합니다.

사용자 지정 스티커 환경을 만드는의 이점은 무엇 인가요?

1. 스티커 앱의 사용자에 게 표시 되는 방식을 사용자 지정할 수 있습니다. 예를 들어, 스티커 또는 표준 표 형태 레이아웃 이외의 형식으로 제공 하는 다른 색이 지정 배경.
2. 스티커를 정적 이미지 자산으로 포함 되는 대신 코드에서 동적으로 만들 수 있습니다.
3. 스티커 이미지 자산을을 앱 스토어에 새 버전을 릴리스할 하지 않고도 개발자 웹 서버에서 동적으로 다운로드할 수 있습니다.
4. 스티커 즉시 만들려면 장치의 카메라에 대 한 액세스를 허용 합니다.
5. 사용자는 앱 내에서 자세한 스티커를 구입할 수 있도록 앱에서 바로 구매 허용 합니다.

사용자 지정 스티커 환경을 만들기를 하려면 다음 절차를 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac 용 Visual Studio를 시작 합니다.
2. 메시지 앱 확장을 추가 하는 솔루션을 엽니다. 
3. 선택 **iOS** > **확장** > **iMessage 확장** 을 클릭 합니다 **다음** 단추: 

    [![](intro-to-message-app-extensions-images/message01.png "IMessage 확장을 선택 합니다.")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. 입력을 **확장 이름** 을 클릭 합니다 **다음** 단추: 

    [![](intro-to-message-app-extensions-images/message02.png "확장 이름 입력")](intro-to-message-app-extensions-images/message02.png#lightbox)
5. 클릭 합니다 **만들기** 확장을 작성 하려면 단추: 

    [![](intro-to-message-app-extensions-images/message03.png "만들기 단추를 클릭 합니다.")](intro-to-message-app-extensions-images/message03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio를 시작합니다.
2. 메시지 앱 확장을 추가 하려면 솔루션을 엽니다.
3. 선택 * * iOS 확장 > iMessage 확장 (iOS) * *을 클릭 합니다 **다음** 단추:

    [![IMessage 확장 (iOS)를 선택 합니다.](intro-to-message-app-extensions-images/message01.w157-sml.png)](intro-to-message-app-extensions-images/message01.w157.png#lightbox)

4. 입력을 **이름을** 을 클릭 합니다 **확인** 단추

-----

기본적으로 `MessagesViewController.cs` 파일이 솔루션에 추가 됩니다. 이 확장의 주 진입점 이며에서 상속 된 `MSMessageAppViewController` 클래스입니다.

사용자에 게 사용 가능한 스티커를 제공 하기 위한 클래스를 제공 하는 메시지 프레임 워크:

- `MSStickerBrowserViewController` -스티커에 표시 됩니다는 뷰를 제어 합니다. 또한 준수 하는 `IMSStickerBrowserViewDataSource` 스티커 개수 및 인덱스의 지정 된 브라우저 스티커 반환 하는 인터페이스입니다.
- `MSStickerBrowserView` -사용 가능한 스티커에 표시 되는 보기입니다.
- `MSStickerSize` -브라우저 보기에서 제공 하는 스티커의 grid에 대 한 개별 셀 크기를 결정 합니다.

### <a name="creating-a-custom-sticker-browser"></a>사용자 지정 스티커 브라우저 만들기

개발자 추가로 사용자 지정할 수 사용자의 스티커 환경을 사용자 지정 스티커 브라우저를 제공 하 여 (`MSMessageAppBrowserViewController`)에서 메시지 앱 확장 합니다. 사용자 지정 스티커 브라우저 메시지 스트림을에 포함할 스티커를 선택 하는 사용자에 게 스티커 표시 되는 방법을 변경 합니다.

다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 에 **Solution Pad**, 확장의 프로젝트 이름을 마우스 오른쪽 단추로 **추가** > **새 파일...**   >  **iOS | Apple Watch** > **컨트롤러 인터페이스**합니다.
2. 입력 `StickerBrowserViewController` 에 대 한 합니다 **이름** 을 클릭 합니다 **새로 만들기** 단추: 

    [![](intro-to-message-app-extensions-images/browser01.png "StickerBrowserViewController 이름 입력")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. 열기는 `StickerBrowserViewController.cs` 편집을 위해 파일입니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 에 **솔루션 탐색기**, 확장의 프로젝트 이름을 마우스 오른쪽 단추로 **추가** > **새 파일...**   >  **iOS | Apple Watch** > **컨트롤러 인터페이스**합니다.
2. 입력 `StickerBrowserViewController` 에 대 한 합니다 **이름** 을 클릭 합니다 **새로 만들기** 단추: 

    [![](intro-to-message-app-extensions-images/browser01.w157-sml.png "StickerBrowserViewController 이름 입력")](intro-to-message-app-extensions-images/browser01.w157.png#lightbox)
3. 열기는 `StickerBrowserViewController.cs` 편집을 위해 파일입니다.

-----

확인 된 `StickerBrowserViewController.cs` 다음과 같습니다.

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MonkeyStickers
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MSStickerSize stickerSize) : base (stickerSize)
        {
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            ...
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();
        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            return Stickers[(int)index];
        }
        #endregion
    }
}
```

세부 정보에서 위의 코드를 살펴보세요. 저장소 확장을 제공 하는 스티커에 대해 생성 됩니다.

```csharp
public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
```

두 메서드를 재정의 하 고는 `MSStickerBrowserViewController` 이 데이터 저장소에서 브라우저에 대 한 데이터를 제공 하는 클래스:

```csharp
public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
{
    return Stickers.Count;
}

public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
{
    return Stickers[(int)index];
}
```

`CreateSticker` 메서드를 확장의 번들에서 이미지 자산의 경로 가져옵니다의 새 인스턴스를 만들고 사용 하는 `MSSticker` 컬렉션에 추가 하는이 자산에서:

```csharp
private void CreateSticker (string assetName, string localizedDescription)
{

    // Get path to asset
    var path = NSBundle.MainBundle.PathForResource (assetName, "png");
    if (path == null) {
        Console.WriteLine ("Couldn't create sticker {0}.", assetName);
        return;
    }

    // Build new sticker
    var stickerURL = new NSUrl (path);
    NSError error = null;
    var sticker = new MSSticker (stickerURL, localizedDescription, out error);
    if (error == null) {
        // Add to collection
        Stickers.Add (sticker);
    } else {
        // Report error
        Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
    }
}
```

합니다 `LoadSticker` 에서 메서드는 `ViewDidLoad` (앱의 번들에 포함) 명명 된 이미지 자산에서 스티커를 만들어 스티커의 컬렉션에 추가 합니다.

사용자 지정 스티커 브라우저를 구현 하려면 편집을 `MessagesViewController.cs` 파일을 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using UIKit;
using Messages;

namespace MonkeyStickers
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public StickerBrowserViewController BrowserViewController { get; set;}
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();


            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);
        }
        #endregion
    }
}
```

이 코드를 자세히 시간이 걸리는 사용자 지정 브라우저에 대 한 저장소를 생성 됩니다.

```csharp
public StickerBrowserViewController BrowserViewController { get; set;}
```

및의 `ViewDidLoad` 메서드를 인스턴스화하고 새 브라우저를 구성 합니다.

```csharp
// Create new browser and configure it
BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
BrowserViewController.View.Frame = View.Frame;
BrowserViewController.ChangeBackgroundColor (UIColor.Gray);
```

다음 뷰를 표시 하려면 브라우저를 추가 합니다.

```csharp
// Add to view
AddChildViewController (BrowserViewController);
BrowserViewController.DidMoveToParentViewController (this);
View.AddSubview (BrowserViewController.View);
```

### <a name="further-sticker-customization"></a>스티커를 자세히 사용자 지정

메시지 앱 확장에 두 개의 클래스를 포함 하 여 추가 스티커 사용자 지정이 가능 합니다.

- `MSStickerView`
- `MSSticker`

위의 메서드를 사용 하 여 확장 표준 스티커 브라우저 방법에 의존 하지 않는 스티커 선택을 지원할 수 있습니다. 또한 두 가지 다른 보기 모드 사이 스티커 표시를 전환할 수 있습니다.

- **Compact** -스티커 보기 메시지 보기의 아래쪽 25% 차지 하는 기본 모드입니다.
- **확장** -스티커 뷰는 전체 메시지 보기를 채웁니다.

이 스티커 보기 전환할 수 있습니다 이러한 모드 간에 수동으로 또는 프로그래밍 방식으로 사용자가.

다음 예제에서는 두 가지 다른 보기 모드 전환 처리를 살펴보세요. 두 명의 서로 다른 뷰 컨트롤러는 각 상태에 대 한 해야 합니다. 합니다 `StickerBrowserViewController` 핸들을 **Compact** 보기와 다음과 같은:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MessageExtension
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set; }
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MessagesViewController messagesAppViewController, MSStickerSize stickerSize) : base (stickerSize)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("add", "Add New Sticker");
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();

        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            // Wanting to add a new sticker?
            if (index == 0) {
                // Yes, ask controller to present add sticker interface
                MessagesAppViewController.AddNewSticker ();
                return null;
            } else {
                // No, return existing sticker
                return Stickers [(int)index];
            }
        }
        #endregion
    }
}
```

합니다 `AddStickerViewController` 처리할 합니다 **확장** 스티커 보기와 다음과 같은 확인:

```csharp
using System;
using Foundation;
using UIKit;
using Messages;

namespace MessageExtension
{
    public class AddStickerViewController : UIViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set;}
        public MSSticker NewSticker { get; set;}
        #endregion

        #region Constructors
        public AddStickerViewController (MessagesViewController messagesAppViewController)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Override Method
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Build interface to create new sticker
            var cancelButton = new UIButton (UIButtonType.RoundedRect);
            cancelButton.TouchDown += (sender, e) => {
                // Cancel add new sticker
                MessagesAppViewController.CancelAddNewSticker ();
            };
            View.AddSubview (cancelButton);

            var doneButton = new UIButton (UIButtonType.RoundedRect);
            doneButton.TouchDown += (sender, e) => {
                // Add new sticker to collection
                MessagesAppViewController.AddStickerToCollection (NewSticker);
            };
            View.AddSubview (doneButton);
            
            ...
        }
        #endregion
    }
}
```

`MessageViewController` 이 뷰 컨트롤러 요청 된 상태 드라이브를 구현 합니다.

```csharp
using System;
using UIKit;
using Messages;

namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public bool IsAddingSticker { get; set;}
        public StickerBrowserViewController BrowserViewController { get; set; }
        public AddStickerViewController AddStickerController { get; set;}
        #endregion

        #region Constructors
        protected MessagesViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Public Methods
        public void PresentStickerBrowser ()
        {
            // Is the Add sticker view being displayed?
            if (IsAddingSticker) {
                // Yes, remove it from view
                AddStickerController.RemoveFromParentViewController ();
                AddStickerController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);

            // Save mode
            IsAddingSticker = false;
        }

        public void PresentAddSticker ()
        {
            // Is the sticker browser being displayed?
            if (!IsAddingSticker) {
                // Yes, remove it from view
                BrowserViewController.RemoveFromParentViewController ();
                BrowserViewController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (AddStickerController);
            AddStickerController.DidMoveToParentViewController (this);
            View.AddSubview (AddStickerController.View);

            // Save mode
            IsAddingSticker = true;
        }

        public void AddNewSticker ()
        {
            // Switch to expanded view mode
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void CancelAddNewSticker ()
        {
            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }

        public void AddStickerToCollection (MSSticker sticker)
        {
            // Add sticker to collection
            BrowserViewController.Stickers.Add (sticker);

            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (this, MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Create new Add controller and configure it as well
            AddStickerController = new AddStickerViewController (this);
            AddStickerController.View.Frame = View.Frame;

            // Initially present the sticker browser
            PresentStickerBrowser ();
        }

        public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            base.DidTransition (presentationStyle);

            // Take action based on style
            switch (presentationStyle) {
            case MSMessagesAppPresentationStyle.Compact:
                PresentStickerBrowser ();
                break;
            case MSMessagesAppPresentationStyle.Expanded:
                PresentAddSticker ();
                break;
            }
        }
        #endregion
    }
}
```

사용자가 사용할 수 있는 컬렉션에 새에 스티커 추가 요청 하면 새 `AddStickerViewController` 표시 컨트롤러와 스티커 뷰 이루어집니다 입력 합니다 **확장** 보기:

```csharp
// Switch to expanded view mode
Request (MSMessagesAppPresentationStyle.Expanded);
```

사용자 선택 추가할 스티커를 사용할 수 있는 해당 컬렉션에 추가 됩니다 및 **Compact** 보기를 요청 합니다.

```csharp
public void AddStickerToCollection (MSSticker sticker)
{
    // Add sticker to collection
    BrowserViewController.Stickers.Add (sticker);

    // Switch to compact view mode
    Request (MSMessagesAppPresentationStyle.Compact);
}
```

`DidTransition` 메서드는 두 가지 모드를 전환 처리 하도록 재정의 됩니다.

```csharp
public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
{
    base.DidTransition (presentationStyle);

    // Take action based on style
    switch (presentationStyle) {
    case MSMessagesAppPresentationStyle.Compact:
        PresentStickerBrowser ();
        break;
    case MSMessagesAppPresentationStyle.Expanded:
        PresentAddSticker ();
        break;
    }
}
``` 

## <a name="summary"></a>요약

이 문서에서는 이러한 부분이 메시지 앱 확장을 사용 하 여 통합 하는 Xamarin.iOS 솔루션에 포함 된 **메시지** 앱 및 사용자에 게 새로운 기능을 제공 합니다. 확장을 사용 하 여 텍스트, 스티커, 미디어 파일 및 대화형 메시지를 보내는 것을 설명 합니다.



## <a name="related-links"></a>관련 링크

- [아이스크림 작성기 (샘플)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [메시지 참조](https://developer.apple.com/reference/messages)
- [앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
