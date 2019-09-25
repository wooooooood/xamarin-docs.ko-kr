---
title: Xamarin.ios의 메시지 앱 확장 기본 사항
description: 이 문서에서는 메시지 앱과 통합 되 고 사용자에 게 새로운 기능을 제공 하는 Xamarin.ios 솔루션에 메시지 앱 확장을 포함 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 0CFB494C-376C-449D-B714-9E82644F9DA3
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/02/2017
ms.openlocfilehash: 37f2942c97f7604fbd72a6dd38de518d3668ee9e
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250128"
---
# <a name="message-app-extension-basics-in-xamarinios"></a>Xamarin.ios의 메시지 앱 확장 기본 사항

_이 문서에서는 메시지 앱과 통합 되 고 사용자에 게 새로운 기능을 제공 하는 Xamarin.ios 솔루션에 메시지 앱 확장을 포함 하는 방법을 보여 줍니다._

IOS 10의 새로운 기능으로, 메시지 앱 확장은 **메시지** 앱과 통합 되며 사용자에 게 새로운 기능을 제공 합니다. 확장은 텍스트, 스티커, 미디어 파일 및 대화형 메시지를 보낼 수 있습니다.

## <a name="about-message-app-extensions"></a>메시지 앱 확장 정보

위에서 설명한 것 처럼 메시지 앱 확장은 **메시지** 앱과 통합 되며 사용자에 게 새로운 기능을 제공 합니다. 확장은 텍스트, 스티커, 미디어 파일 및 대화형 메시지를 보낼 수 있습니다. 다음과 같은 두 가지 유형의 메시지 앱 확장을 사용할 수 있습니다.

- **스티커 팩** -사용자가 메시지에 추가할 수 있는 스티커의 컬렉션을 포함 합니다. 스티커 팩은 코드를 작성 하지 않고도 만들 수 있습니다.
- **IMessage apps** -스티커를 선택 하 고, 미디어 파일 (선택적 형식 변환 포함)을 포함 하 여 텍스트를 입력 하 고, 상호 작용 메시지를 만들고 편집 하 고 보내는 메시지 앱 내에 사용자 지정 사용자 인터페이스를 제공할 수 있습니다.

메시지 앱 확장은 세 가지 주요 내용 유형을 제공 합니다.

- **대화형 메시지** -앱이 생성 하는 사용자 지정 메시지 콘텐츠의 형식으로, 사용자가 메시지를 탭 하면 응용 프로그램이 포그라운드에서 시작 됩니다.
- **스티커** -사용자 간에 전송 된 메시지에 포함할 수 있는 앱에 의해 생성 된 이미지입니다.
- **기타 지원 되는 콘텐츠** -앱은 메시지 앱에서 항상 지원 되는 사진, 비디오, 텍스트 또는 다른 콘텐츠 형식에 대 한 링크와 같은 콘텐츠를 제공할 수 있습니다.

IOS 10의 새로운 기능으로, 이제 메시지 앱은 자체의 전용 기본 제공 앱 스토어를 포함 합니다. 메시지 앱 확장을 포함 하는 모든 앱이이 저장소에 표시 되 고 승격 됩니다. 새 메시지 앱 서랍에는 사용자에 게 신속 하 게 액세스할 수 있도록 메시지 앱 스토어에서 다운로드 한 앱이 표시 됩니다.

IOS 10 에서도 새로 추가 된 Apple에는 사용자가 앱을 쉽게 검색할 수 있도록 하는 인라인 앱 특성이 추가 되었습니다. 예를 들어 한 사용자가 두 번째 사용자가 설치 하지 않은 앱에서 다른 사용자에 게 콘텐츠를 전송 하는 경우 (예: 스티커) 보내는 앱의 이름이 메시지 기록의 내용에 나열 됩니다. 사용자가 앱의 이름을 탭 하면 메시지 앱 스토어가 열리고 앱이 스토어에서 선택 됩니다.

메시지 앱 확장은 개발자가 작성 하는 데 익숙한 기존 iOS 앱과 비슷하며 표준 iOS 앱의 모든 표준 프레임 워크 및 기능에 액세스할 수 있습니다. 예:

- 앱 내 구매에 액세스할 수 있습니다.
- Apple Pay에 대 한 액세스 권한이 있습니다.
- 카메라와 같은 장치 하드웨어에 액세스할 수 있습니다.

메시지 앱 확장은 iOS 10 에서만 지원 됩니다. 그러나 이러한 확장에서 전송 하는 콘텐츠는 watchOS 및 macOS 장치에서 볼 수 있습니다. WatchOS 3에 추가 된 새 _최근 페이지_ 에는 메시지 앱 확장의 항목을 비롯 하 여 휴대폰에서 전송 된 최근 스티커가 표시 되 고 사용자가이 스티커를 시청에서 보낼 수 있습니다.

## <a name="about-the-messages-framework"></a>메시지 프레임 워크 정보

IOS 10의 새로운 기능으로, 메시지 프레임 워크는 사용자의 iOS 장치에서 메시지 앱 확장 및 메시지 앱 간의 인터페이스를 제공 합니다. 사용자가 메시지 앱 내에서 앱을 시작 하면이 프레임 워크는 앱을 검색할 수 있도록 허용 하 고 UI를 레이아웃 하는 데 필요한 데이터 및 컨텍스트를 제공 합니다.

앱이 시작 되 면 사용자는이를 조작 하 여 메시지를 통해 공유할 새 콘텐츠를 만듭니다. 그러면 앱에서 메시지 프레임 워크를 사용 하 여 새로 만든 콘텐츠를 처리를 위해 메시지 앱에 전송 합니다.

메시지 프레임 워크 및 메시지 앱 확장은 기존 iOS 앱 확장 기술 위에 빌드됩니다. 앱 확장에 대 한 자세한 내용은 Apple의 [앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)를 참조 하세요.

Apple에서 시스템을 통해 제공 하는 다른 확장 지점과 달리 개발자는 메시지 앱 자체가 컨테이너 역할을 수행 하기 때문에 메시지 앱 확장에 호스트 앱을 제공할 필요가 없습니다. 그러나 개발자는 새 iOS 앱 또는 기존 iOS 앱 내에 메시지 앱 확장을 포함 하 고 번들과 함께 배송할 수 있습니다.

메시지 앱 확장이 iOS 앱의 번들에 포함 된 경우 앱의 아이콘이 메시지 앱 내에서 장치의 홈 화면과 메시지 앱 서랍에 모두 표시 됩니다. 앱 번들에 포함 되지 않은 경우 메시지 앱 확장은 메시지 앱 서랍에만 표시 됩니다.

메시지 앱 확장이 호스트 앱 번들에 포함 되지 않은 경우에도 개발자는 메시지 앱 확장 프로그램의 번들에 앱 아이콘을 제공 해야 합니다 .이 아이콘은 메시지 앱 서랍 또는 설정과 같은 시스템의 다른 부분에 표시 되는 아이콘입니다. 확장의 경우입니다.

## <a name="about-stickers"></a>스티커 정보

Apple에서 만든 스티커는 다른 메시지 내용으로 스티커를 인라인으로 전송 하거나 대화 내부의 이전 메시지 거품에 연결할 수 있도록 하 여 사용자가 통신할 수 있도록 하는 새로운 방법으로 스티커 iMessage.

스티커 란?

- 메시지 앱 확장에서 제공 하는 이미지입니다.
- 애니메이션 또는 정적 이미지 중 하나를 사용할 수 있습니다.
- 앱 내부에서 이미지 콘텐츠를 공유 하는 새로운 방법을 제공 합니다.

스티커를 만드는 방법에는 두 가지가 있습니다.

1. 스티커 팩 메시지 앱 확장은 코드를 포함 하지 않고 Xcode 내부에서 만들 수 있습니다. 스티커와 앱 아이콘에 대 한 자산만 있으면 됩니다.
2. 메시지 프레임 워크를 통해 코드에서 스티커를 제공 하는 표준 메시지 앱 확장을 만듭니다.

### <a name="creating-sticker-packs"></a>스티커 팩 만들기

스티커 팩은 Xcode 내의 특수 한 템플릿에서 만들어지며 스티커로 사용할 수 있는 고정 이미지 자산 집합을 제공 하기만 하면 됩니다. 위에서 설명한 것 처럼 코드는 필요 하지 않습니다. 개발자는 이미지 파일을 스티커 자산 카탈로그 내부의 스티커 팩 폴더로 끌어 놓기만 하면 됩니다.

스티커 팩에 이미지를 포함 하려면 다음 요구 사항을 충족 해야 합니다.

- 이미지는 PNG, APNG, GIF 또는 JPEG 형식 이어야 합니다. Apple은 스티커 자산을 제공할 때 PNG 및 APNG 형식만 사용 하는 것을 제안 합니다.
- 애니메이션 스티커는 APNG 및 GIF 형식만 지원 합니다.
- 스티커 이미지는 사용자의 대화에서 메시지 거품 위에 배치 될 수 있으므로 투명 한 배경을 제공 해야 합니다.
- 개별 이미지 파일은 500kb 미만 이어야 합니다.
- 이미지는 100x100 점이 나 206 x 206 점이 클 때 보다 작을 수 없습니다.

> [!IMPORTANT]
> 스티커 이미지는 항상 300 x 300에서 `@3x` 618 x 618 픽셀 범위의 해상도로 제공 되어야 합니다. 시스템은 필요에 따라 런타임에 `@2x` 및 `@1x` 버전을 자동으로 생성 합니다.

Apple은 다양 한 색이 지정 된 배경 (흰색, 검정, 빨강, 노랑, 색 등) 및 사진에 대해 스티커 이미지 자산을 테스트 하 여 가능한 모든 상황에서 가장 잘 보이도록 합니다.

스티커 팩은 사용 가능한 세 가지 크기 중 하나로 스티커를 제공할 수 있습니다.

- 100 x 100 점이 있습니다.
- **중간** -136 x 136 점이 있습니다. 기본 크기입니다.
- **큼** -206 x 206 점이 있습니다.

Xcode의 Attributes Inspector를 사용 하 여 전체 스티커 팩의 크기를 설정 하 고 요청 된 크기와 일치 하는 이미지 자산만 제공 하 여 메시지 앱 내부의 스티커 브라우저에서 최상의 결과를 얻을 수 있습니다.

자세한 내용은 [아이스크림 작성기](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder) 앱 및 Apple의 [메시지 참조](https://developer.apple.com/reference/messages)를 참조 하세요.

## <a name="creating-a-custom-sticker-experience"></a>사용자 지정 스티커 환경 만들기

앱에 스티커 팩에서 제공 하는 것 보다 더 많은 컨트롤이 나 유연성이 필요한 경우 메시지 앱 확장을 포함 하 고 사용자 지정 스티커 환경을 위해 메시지 프레임 워크를 통해 스티커를 제공할 수 있습니다.

사용자 지정 스티커 환경을 만들 때의 이점은 무엇 인가요?

1. 앱이 앱 사용자에 게 스티커를 표시 하는 방법을 사용자 지정할 수 있습니다. 예를 들어 스티커를 표준 그리드 레이아웃이 나 다른 색이 지정 된 배경에 표시 하려면입니다.
2. 스티커를 정적 이미지 자산으로 포함 하는 대신 코드에서 동적으로 만들 수 있습니다.
3. 새 버전을 앱 스토어에 릴리스할 필요 없이 스티커 이미지 자산이 개발자의 웹 서버에서 동적으로 다운로드 될 수 있습니다.
4. 장치 카메라에 대 한 액세스를 통해 스티커를 즉석에서 만들 수 있습니다.
5. 사용자가 앱 내에서 더 많은 스티커를 구입할 수 있도록 앱 내 구매를 허용 합니다.

사용자 지정 스티커 환경을 만들려면 다음을 수행 합니다.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac용 Visual Studio를 시작합니다.
2. 솔루션을 열어에 메시지 앱 확장을 추가 합니다.
3. **IOS** > **확장** iMessage 확장을 선택 하 고 다음 단추를 클릭 합니다. > 

    [![](intro-to-message-app-extensions-images/message01.png "IMessage 확장 선택")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. **확장 이름을** 입력 하 고 **다음** 단추를 클릭 합니다.

    [![](intro-to-message-app-extensions-images/message02.png "확장 이름 입력")](intro-to-message-app-extensions-images/message02.png#lightbox)
5. **만들기** 단추를 클릭 하 여 확장을 빌드합니다.

    [![](intro-to-message-app-extensions-images/message03.png "만들기 단추를 클릭 합니다.")](intro-to-message-app-extensions-images/message03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio를 시작합니다.
2. 솔루션을 열어 메시지 앱 확장을 추가 합니다.
3. Ios **(Ios 확장 > IMessage Extension)** 를 선택 하 고 **다음** 단추를 클릭 합니다.

    [![IMessage 확장 (iOS)을 선택 합니다.](intro-to-message-app-extensions-images/message01.w157-sml.png)](intro-to-message-app-extensions-images/message01.w157.png#lightbox)

4. **이름을** 입력 하 고 **확인** 단추를 클릭 합니다.

-----

기본적으로이 파일 `MessagesViewController.cs` 은 솔루션에 추가 됩니다. 이는 확장에 대 한 주 진입점 이며 `MSMessageAppViewController` 클래스에서 상속 됩니다.

메시지 프레임 워크는 사용자에 게 사용 가능한 스티커를 제공 하기 위한 클래스를 제공 합니다.

- `MSStickerBrowserViewController`-스티커가 표시 될 뷰를 제어 합니다. 또한 `IMSStickerBrowserViewDataSource` 인터페이스를 준수 하 여 지정 된 브라우저 인덱스의 스티커 개수와 스티커를 반환 합니다.
- `MSStickerBrowserView`-사용 가능한 스티커가 표시 되는 뷰입니다.
- `MSStickerSize`-브라우저 보기에 표시 되는 스티커 표의 개별 셀 크기를 결정 합니다.

### <a name="creating-a-custom-sticker-browser"></a>사용자 지정 스티커 브라우저 만들기

개발자는 메시지 앱 확장에서 사용자 지정 스티커 브라우저 (`MSMessageAppBrowserViewController`)를 제공 하 여 사용자의 스티커 경험을 추가로 사용자 지정할 수 있습니다. 사용자 지정 스티커 브라우저는 메시지 스트림에 포함할 스티커를 선택할 때 사용자에 게 스티커가 표시 되는 방식을 변경 합니다.

다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**에서 확장의 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다.  >  **iOS |**  > **인터페이스 컨트롤러**를 Apple Watch 합니다.
2. 이름 `StickerBrowserViewController` 으로를 입력 하 고 **새로 만들기** 단추를 클릭 합니다.

    [![](intro-to-message-app-extensions-images/browser01.png "이름에 StickerBrowserViewController를 입력 합니다.")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. 편집할 파일 `StickerBrowserViewController.cs` 을 엽니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기**에서 확장의 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다.  >  **iOS |**  > **인터페이스 컨트롤러**를 Apple Watch 합니다.
2. 이름 `StickerBrowserViewController` 으로를 입력 하 고 **새로 만들기** 단추를 클릭 합니다.

    [![](intro-to-message-app-extensions-images/browser01.w157-sml.png "이름에 StickerBrowserViewController를 입력 합니다.")](intro-to-message-app-extensions-images/browser01.w157.png#lightbox)
3. 편집할 파일 `StickerBrowserViewController.cs` 을 엽니다.

-----

다음과 같이 `StickerBrowserViewController.cs` 확인 합니다.

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

위의 코드를 자세히 살펴보세요. 확장에서 제공 하는 스티커의 저장소를 만듭니다.

```csharp
public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
```

및은 `MSStickerBrowserViewController` 클래스의 두 메서드를 재정의 하 여이 데이터 저장소의 브라우저에 대 한 데이터를 제공 합니다.

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

메서드 `CreateSticker` 는 확장의 번들에서 이미지 자산의 경로를 가져온 다음이 자산 `MSSticker` 에서 컬렉션에 추가 하는의 새 인스턴스를 만드는 데 사용 합니다.

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

메서드 `LoadSticker` 는에서 `ViewDidLoad` 호출 되어 명명 된 이미지 자산 (앱의 번들에 포함)에서 스티커를 만들고 스티커의 컬렉션에 추가 합니다.

사용자 지정 스티커 브라우저를 구현 하려면 `MessagesViewController.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

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

이 코드를 자세히 살펴보면 사용자 지정 브라우저에 대 한 저장소를 만듭니다.

```csharp
public StickerBrowserViewController BrowserViewController { get; set;}
```

`ViewDidLoad` 또한 메서드에서는 새 브라우저를 인스턴스화하고 구성 합니다.

```csharp
// Create new browser and configure it
BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
BrowserViewController.View.Frame = View.Frame;
BrowserViewController.ChangeBackgroundColor (UIColor.Gray);
```

그런 다음 보기에 브라우저를 추가 하 여 표시 합니다.

```csharp
// Add to view
AddChildViewController (BrowserViewController);
BrowserViewController.DidMoveToParentViewController (this);
View.AddSubview (BrowserViewController.View);
```

### <a name="further-sticker-customization"></a>추가 스티커 사용자 지정

메시지 앱 확장에 두 개의 클래스만 포함 하 여 추가 스티커 사용자 지정이 가능 합니다.

- `MSStickerView`
- `MSSticker`

위의 방법을 사용 하 여 확장은 표준 스티커 브라우저 방법을 사용 하지 않는 스티커 선택을 지원할 수 있습니다. 또한 스티커 표시는 서로 다른 두 가지 보기 모드 간에 전환할 수 있습니다.

- **Compact** -스티커 보기가 메시지 보기의 하위 25%를 차지 하는 기본 모드입니다.
- **확장** 됨-스티커 보기가 전체 메시지 보기를 채웁니다.

이 스티커 보기는 프로그래밍 방식으로 또는 사용자가 수동으로 이러한 모드 간에 전환할 수 있습니다.

두 가지 서로 다른 보기 모드 간의 전환을 처리 하는 다음 예를 살펴보겠습니다. 각 상태에는 두 개의 서로 다른 뷰 컨트롤러가 필요 합니다. 는 `StickerBrowserViewController` **압축** 뷰를 처리 하 고 다음과 같습니다.

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

는 `AddStickerViewController` **확장** 된 스티커 보기를 처리 하 고 다음과 같습니다.

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

는 `MessageViewController` 이러한 뷰 컨트롤러를 구현 하 여 요청 된 상태를 구동 합니다.

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

사용자가 사용 가능한 컬렉션에 새 스티커를 추가 하도록 요청 하는 경우 새 `AddStickerViewController` 가 표시 되는 컨트롤러가 되 고 스티커 보기가 **확장** 된 보기로 전환 됩니다.

```csharp
// Switch to expanded view mode
Request (MSMessagesAppPresentationStyle.Expanded);
```

사용자가 추가할 스티커를 선택 하면 사용할 수 있는 컬렉션에 추가 되 고 **압축** 보기가 요청 됩니다.

```csharp
public void AddStickerToCollection (MSSticker sticker)
{
    // Add sticker to collection
    BrowserViewController.Stickers.Add (sticker);

    // Switch to compact view mode
    Request (MSMessagesAppPresentationStyle.Compact);
}
```

메서드 `DidTransition` 는 두 가지 모드 간의 전환을 처리 하도록 재정의 됩니다.

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

이 문서에서는 **메시지** 앱과 통합 되 고 사용자에 게 새로운 기능을 제공 하는 xamarin.ios 솔루션에 메시지 앱 확장을 포함 합니다. 여기서는 확장을 사용 하 여 텍스트, 스티커, 미디어 파일 및 대화형 메시지를 보냈습니다.

## <a name="related-links"></a>관련 링크

- [아이스크림 빌더 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-icecreambuilder)
- [메시지 참조](https://developer.apple.com/reference/messages)
- [앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
