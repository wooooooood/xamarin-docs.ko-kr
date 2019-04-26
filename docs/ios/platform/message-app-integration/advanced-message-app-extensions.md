---
title: Xamarin.iOS에서 고급 메시지 앱 확장
description: 이 문서에는 메시지 앱 통합 및 사용자에 게 새 기능을 제공 하는 Xamarin.iOS 솔루션에서 메시지 앱 확장을 사용 하 여 작업에 대 한 고급 기술을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: baceb59116dd907918b34eca4f44293051190954
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61155653"
---
# <a name="advanced-message-app-extensions-in-xamarinios"></a>Xamarin.iOS에서 고급 메시지 앱 확장

_이 문서에는 메시지 앱 통합 및 사용자에 게 새 기능을 제공 하는 Xamarin.iOS 솔루션에서 메시지 앱 확장을 사용 하 여 작업에 대 한 고급 기술을 보여 줍니다._


새 메시지 앱 확장을 통합 하 여 iOS 10, 합니다 **메시지** 사용자에 게 새로운 기능을 앱 및 표시 합니다. 확장은 텍스트, 스티커, 미디어 파일 및 대화형 메시지를 보낼 수 있습니다.

## <a name="about-message-app-extensions"></a>메시지 앱 확장에 대 한

메시지 앱 확장을 통합 위에서 설명한 대로 합니다 **메시지** 사용자에 게 새로운 기능을 앱 및 표시 합니다. 확장은 텍스트, 스티커, 미디어 파일 및 대화형 메시지를 보낼 수 있습니다. 두 유형의 메시지 앱 확장을 사용할 수 있습니다.

- **스티커 팩** -스티커 사용자 메시지를 추가할 수 있는 컬렉션을 포함 합니다. 코드 작성 없이 스티커 팩을 만들 수 있습니다.
- **iMessage 앱** -스티커를 선택 하면, 텍스트를 입력, 선택적 형식 변환) (로 미디어 파일을 포함 하 여 및 만들기, 편집 및 상호 작용 메시지를 보내기에 대 한 메시지 앱 내에서 사용자 지정 사용자 인터페이스를 제공할 수 있습니다.

메시지 앱 확장에는 세 가지 기본 콘텐츠 형식을 제공합니다.

- **대화형 메시지** -는 앱을 생성 하는 사용자 지정 메시지 콘텐츠 유형, 메시지를 앱에 사용자가 누를 때 전경에서 시작 됩니다.
- **스티커** -이미지 사용자 간에 전송 된 메시지에 포함 될 수 있는 앱에서 생성 됩니다. 참조 우리의 [Ice cream 작성기](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) 스티커 팩 응용 프로그램의 구현 예제 용 샘플 앱입니다.
- **기타 지원 콘텐츠** -앱 사진, 비디오, 텍스트 또는 메시지 앱에서 항상 지원 되는 형식의 링크와 같은 콘텐츠를 제공할 수 있습니다.

새 ios 10, 메시지 앱 이제 자체 전용, 기본 제공 앱 스토어입니다. 메시지 앱 확장을 포함 하는 모든 앱을 표시 하 고이 저장소에서 승격 됩니다. 새 메시지 앱 서랍 사용자에 게 빠르게 액세스할 수 있도록 메시지 앱 스토어에서 다운로드 한 모든 앱에 표시 됩니다.

또한 새 ios 10에서 Apple이 추가 인라인 앱 Attribution 사용자 앱을 쉽게 검색할 수 있습니다. 예를 들어, 하나의 사용자가 두 번째 사용자에 게는 앱에서 다른 콘텐츠를 전송 (예: 스티커 예)를 설치 보내는 앱의 이름 아래 있는지 메시지 기록에서 콘텐츠. 사용자가 앱의 경우 이름, 열 수 것 메시지 앱 스토어 및 스토어에서 선택한 앱.

메시지 앱 확장은 기존 iOS 앱을 만드는 친숙 한 개발자는 모든 표준 프레임 워크 및 표준 iOS 앱의 기능에 대 한 액세스를 갖습니다 유사 합니다. 예를 들어:

- 인 앱 구매에 액세스할을 수 있습니다.
- Apple Pay에 액세스할을 수 있습니다.
- 카메라 같은 장치 하드웨어에 액세스할을 수 있습니다.

그러나 메시지 앱 확장은 iOS 10에서 지원만, 이러한 확장을 전송 하는 콘텐츠는 watchOS 및 macOS 장치에서 볼 수 있습니다. 새 _최근 페이지_ 에서 메시지 앱 확장을 포함 하 여 휴대폰에서 보낸 최신 스티커 표시 되며 사용자 watch에서 해당 스티커를 보낼 수 있도록 watchOS 3에 추가 합니다.

## <a name="about-interactive-messages"></a>대화형 메시지에 대 한

대화형 메시지 사용자 지정 메시지 거품을 표시 하 고 메시지 앱 확장에서 제공 됩니다. 메시지 입력 필드에 삽입 하 고 보냅니다 대화형 메시지 콘텐츠를 만드는 데 사용할 수 있습니다.

[![](advanced-message-app-extensions-images/interactive01.png "대화형 메시지 콘텐츠 만들기")](advanced-message-app-extensions-images/interactive01.png#lightbox)

받는 사용자는 생성 된 메시지 앱 확장을 로드 하 고 메시지 기록에 해당 메시지 거품을 탭 하 여 대화형 메시지에 회신할 수 있습니다. 확장 전체 화면 시작된 되며 사용자 응답을 작성 하 고 다시 원래 사용자에 게 보낼 수 있도록 합니다.

[![](advanced-message-app-extensions-images/interactive02.png "확장 전체 화면 시작")](advanced-message-app-extensions-images/interactive02.png#lightbox)


다음 항목 아래에서 자세히 다룰:

- 메시지 API 개요
- 확장 수명 주기
- 메시지를 작성합니다.
- 메시지 보내기

## <a name="messages-api-overview"></a>메시지 API 개요

사용자가 호출 되 면 메시지 앱 확장을 간단히 보기 모드에서 메시지 기록의 맨 아래에 표시 됩니다.

[![](advanced-message-app-extensions-images/interactive03.png "메시지 API 개요")](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. `MSMessageAppViewController` 메시지 앱 확장의 개체는 사용자에 게 확장의 보기가 표시 되 면 호출 되는 주 클래스입니다.
2. 대화와 사용자에 게 표시 되는 `MSConversation` 개체 인스턴스입니다.
3. `MSMessage` 클래스 대화에 지정 된 메시지 거품을 나타냅니다.
4. `MSSession` 메시지가 전송 되는 방식을 제어 합니다.
5. `MSMessageTemplateLayout` 메시지가 표시 되는 방식을 제어 합니다.

## <a name="the-extension-lifecycle"></a>확장 수명 주기

활성화 메시지 앱 확장의 프로세스를 살펴보십시오.

[![](advanced-message-app-extensions-images/interactive04.png "활성화 메시지 앱 확장의 프로세스")](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. 확장 (예: 앱 서랍)에서 시작 되 면 메시지 앱 프로세스를 시작 됩니다.
2. `DidBecomeActive` 메서드가 호출 되어 전달 되는 `MSConversation` 메시지 앱 확장에서 실행 되는 대화를 나타내는입니다.
3. 확장을 기반으로 하기 때문에 `UIViewController` 둘 다 `ViewWillAppear` 고 `ViewDidAppear` 라고 합니다.

다음으로 비활성화 되 고 메시지 앱 확장의 프로세스를 살펴보십시오.

[![](advanced-message-app-extensions-images/interactive05.png "비활성화 되 고 메시지 앱 확장의 프로세스")](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. 메시지 앱 확장은 비활성화 되 면를 `ViewWillDisappear` 메서드가 먼저 호출 됩니다.
2. 그런 다음 `ViewDidDisappear` 메서드가 호출 됩니다.
3. `WillResignActive` 메서드가 호출 되어 전달 되는 `MSConversation` 메시지 앱 확장에서 실행 되는 대화를 나타내는입니다. 이 시점에서 메시지 앱 확장 사이의 연결을 해제 합니다.
4. 나중에 메시지 앱에서 프로세스가 종료 됩니다.

확장 단기적인된 프로세스 이므로 처리 및 배터리 전원을 절약 하기 위해 시스템에서 적극적으로 종료 됩니다. 개발자가 디자인 및 메시지 앱 확장을 구현할 때 염두에서에 유지 해야 합니다.

## <a name="composing-a-message"></a>메시지를 작성합니다.

메시지 앱 확장을 프로세스에서 실행 되 고 해당 사용자 인터페이스를 표시 하는 새 메시지를 작성 하려면 다음 코드를 사용할 수 있습니다.

```csharp
MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
{
    var components = new NSUrlComponents {
        QueryItems = iceCream.QueryItems
    };

    var layout = new MSMessageTemplateLayout {
        Image = iceCream.RenderSticker (true),
        Caption = caption
    };

    var message = new MSMessage (session ?? new MSSession()) {
        Url = components.Url,
        Layout = layout
    };

    return message;
}
```

이 코드를 만듭니다 `MSMessage` 여러 속성을 설정 하 고 (같은 `Url`). 메시지 iOS에만 만들 수 있습니다, 있지만 표시할 iOS 및 macOS에 보낼 수 있습니다.

사용자가 macOS에서 대화의 메시지 거품을 클릭 하면 Mac 웹 브라우저에서 URL에 지정 된 주소를 열려고 시도 합니다. 결과적으로, 개발자 웹 사이트는 macOS에서 웹 브라우저에서 메시지의 일부 표현이 기반 컴퓨터를 표시할 수 있어야 합니다.

`AccessibilityLabel` 속성은 화면 읽기 프로그램 사용자에 게 대화의 내용 읽기를 사용 합니다. `Layout` 속성 어떻게 메시지 표시 되도록 지정, 현재만 `MSMessageTemplateLayout` 은 지원 되며 다음과 같습니다.

[![](advanced-message-app-extensions-images/interactive06.png "MSMessageTemplateLayout 템플릿")](advanced-message-app-extensions-images/interactive06.png#lightbox)

합니다 `Image` 의 속성을 `MSMessageTemplateLayout` 화면의 MessageBubble의 본문에 콘텐츠를 제공 합니다. 합니다 `MediaFileUrl` 속성 또한 메시지 풍선의 본문에 콘텐츠를 제공 하지만에서 지원 되지 않는 콘텐츠에 대 한 허용 `UIImage` (예:는 백그라운드에서 반복 되는 비디오 파일). 모두를 `Image` 및 `MediaFileUrl` 속성을 제공 합니다 `Image` 속성 우선 적용 됩니다. `MediaFileUrl` PNG, JPEG, GIF 및 비디오 (Media Player 프레임 워크에서 재생 될 수 있는 모든 형식)를에서 지 원하는 미디어 형식입니다.

권장 되는 미디어 크기 3 x 해상도 300 x 300 픽셀입니다. 약간 크고 작은 자산도 허용 됩니다 하 고 Apple에서 최상의 결과 얻기 위해 몇 가지 다양 한 크기를 사용 하 여 테스트를 제안 합니다. 메시지 앱을 다운 샘플링 되며 필요에 따라이 미디어 크기 조정 합니다.

자산 수신자에 게 전송 될 때 연결 된 모든 미디어를 자동으로 네트워크를 통해 전송에서 최적화 하기 위해 메시지 앱에서 트랜스 됩니다 합니다. 이 때문에 Apple 따라서 잠재적으로 텍스트를 렌더링 불법적인 미디어 축소 되며 전송에 대 한 압축 때문에 메시지에 첨부 된 미디어의 텍스트를 포함 하 여 방지 합니다.

합니다 `ImageTitle` 고 `ImageSubtitle` 속성 메시지 거품에 표시 되는 미디어에 대 한 설명을 제공 합니다. 이러한 속성 전송할 텍스트로 수신 장치에 있는 또렷하게 보입니다 렌더링 됩니다 이미지의 왼쪽 위 모서리에 있습니다.

`Caption`, `SubCaption`, `TrailingCaption` 고 `TrailingSubcaption` 속성에서 추가 이미지를 설명 하 고 이미지 아래 섹션에서 렌더링 됩니다. 이러한 속성을 일부만 설정 `null` 캡션 영역 없이 메시지 거품 만들어집니다.

[![](advanced-message-app-extensions-images/interactive07.png "캡션 영역 없이 메시지 거품")](advanced-message-app-extensions-images/interactive07.png#lightbox)

마지막 점은 메시지 앱 메시지 풍선의 왼쪽 위 모서리에서 메시지 앱 확장 아이콘을 그릴 됩니다.

## <a name="sending-a-message"></a>메시지 보내기

한 번을 `MSMessage` 다음 코드를 전송할 수로 구성 되었습니다.

```csharp
public void SendMessage (MSMessage message)
{
    // Insert the message into the conversation
    ActiveConversation.InsertMessage (message, (error) => {
        // Did the message send successfully?
        if (error == null) {
            // Handle successful send
        } else {
            // Report Error
            Console.WriteLine ("Error: {0}", error);
        }
    };

}
```

`ActiveConversation` 의 속성을 `MSMessagesAppViewController` 메시지 앱 확장에서 실행 된 현재 대화 보유할 합니다.

호출을 `InsertMessage` 의 `MSConversation` 대화의 메시지를 포함 하 여 발생할 수 있는 오류를 처리 합니다. 메시지를 성공적으로 포함 하는 경우 메시지 거품 입력 필드에 표시 됩니다.

또한 확장와 같은 대화에 서로 다른 유형의 데이터를 보낼 수 있습니다.

- **텍스트** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **첨부 파일** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **스티커**  -  `ActiveConversation.InsertSticker (sticker, (obj) => {...});` 여기서 `sticker` 되는 `MSSticker`합니다.

입력 필드에서 새 콘텐츠를 사용자가 파란색을 탭 하 여 메시지를 보낼 수 있으려면 **보낼** (에서처럼 모든 일반적인 메시지) 단추입니다. 콘텐츠를 자동으로 보낼 메시지 앱 확장에 대 한 방법은 없습니다,이 프로세스는 완전히 사용자의 제어 합니다.

## <a name="handling-the-compact-and-expanded-modes"></a>Compact 및 확장 모드 처리

메시지 앱 확장을 두 가지 다른 보기 모드 중 하나로 표시할 수 있습니다.

[![](advanced-message-app-extensions-images/interactive08.png "두 가지 다른 보기 모드에서 표시 하는 메시지 앱 확장: Compact 및 확장")](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** -메시지 앱 확장 메시지 보기의 아래쪽 25% 차지 하는 기본 모드입니다. 축소 모드에서는 앱이 없습니다 키보드, 가로 스크롤 또는 살짝 밀기 제스처 인식기에 대 한 액세스. 앱에서 입력 필드에 액세스할 수 및 호출 `InsertMessage` 사용자에 게 즉시 표시 됩니다.
- **확장** -메시지 앱 확장은 전체 메시지 보기를 채웁니다. 입력 필드에 액세스 하지 못합니다 않지만 키보드, 가로 스크롤 및 살짝 밀기 제스처 인식기에 액세스할 수 있습니다.

메시지 앱 확장을 전환할 수 있습니다 이러한 모드 간에 수동으로 또는 프로그래밍 방식으로 사용자가 언제 든 지 고 즉시 응답에 뷰 모드 변경 해야 합니다.

다음 예제에서는 두 가지 다른 보기 모드 전환 처리를 살펴보세요. 두 명의 서로 다른 뷰 컨트롤러는 각 상태에 대 한 해야 합니다. `StickerBrowserViewController` 핸들을 **Compact** 보기 및 `AddStickerViewController` 처리할 합니다 **확장** 보기:

```csharp
using System;

using Messages;
using Foundation;
using UIKit;

namespace MessagesExtension {
    public partial class MessagesViewController : MSMessagesAppViewController, IIceCreamsViewControllerDelegate, IBuildIceCreamViewControllerDelegate {
        public MessagesViewController (IntPtr handle) : base (handle)
        {
        }

        public override void WillBecomeActive (MSConversation conversation)
        {
            base.WillBecomeActive (conversation);

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, PresentationStyle);
        }

        public override void WillTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected an active converstation");

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, presentationStyle);
        }

        void PresentViewController (MSConversation conversation, MSMessagesAppPresentationStyle presentationStyle)
        {
            // Determine the controller to present.
            UIViewController controller;

            if (presentationStyle == MSMessagesAppPresentationStyle.Compact) {
                // Show a list of previously created ice creams.
                controller = InstantiateIceCreamsController ();
            } else {
                var iceCream = new IceCream (conversation.SelectedMessage);
                controller = iceCream.IsComplete ? InstantiateCompletedIceCreamController (iceCream) : InstantiateBuildIceCreamController (iceCream);
            }

            foreach (var child in ChildViewControllers) {
                child.WillMoveToParentViewController (null);
                child.View.RemoveFromSuperview ();
                child.RemoveFromParentViewController ();
            }

            AddChildViewController (controller);
            controller.View.Frame = View.Bounds;
            controller.View.TranslatesAutoresizingMaskIntoConstraints = false;
            View.AddSubview (controller.View);

            controller.View.LeftAnchor.ConstraintEqualTo (View.LeftAnchor).Active = true;
            controller.View.RightAnchor.ConstraintEqualTo (View.RightAnchor).Active = true;
            controller.View.TopAnchor.ConstraintEqualTo (View.TopAnchor).Active = true;
            controller.View.BottomAnchor.ConstraintEqualTo (View.BottomAnchor).Active = true;

            controller.DidMoveToParentViewController (this);
        }

        UIViewController InstantiateIceCreamsController ()
        {
            // Instantiate a `IceCreamsViewController` and present it.
            var controller = Storyboard.InstantiateViewController (IceCreamsViewController.StoryboardIdentifier) as IceCreamsViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an IceCreamsViewController from the storyboard");

            controller.Builder = this;
            return controller;
        }

        UIViewController InstantiateBuildIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (BuildIceCreamViewController.StoryboardIdentifier) as BuildIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an BuildIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            controller.Builder = this;

            return controller;
        }

        public UIViewController InstantiateCompletedIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (CompletedIceCreamViewController.StoryboardIdentifier) as CompletedIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an CompletedIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            return controller;
        }

        public void DidSelectAdd (IceCreamsViewController controller)
        {
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void Build (BuildIceCreamViewController controller, IceCreamPart iceCreamPart)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected a conversation");

            var iceCream = controller.IceCream;
            if (iceCream == null)
                throw new Exception ("Expected the controller to be displaying an ice cream");

            string messageCaption = string.Empty;
            var b = iceCreamPart as Base;
            var s = iceCreamPart as Scoops;
            var t = iceCreamPart as Topping;

            if (b != null) {
                iceCream.Base = b;
                messageCaption = "Let's build an ice cream";
            } else if (s != null) {
                iceCream.Scoops = s;
                messageCaption = "I added some scoops";
            } else if (t != null) {
                iceCream.Topping = t;
                messageCaption = "Our finished ice cream";
            } else {
                throw new Exception ("Unexpected type of ice cream part selected.");
            }

            // Create a new message with the same session as any currently selected message.
            var message = ComposeMessage (iceCream, messageCaption, conversation.SelectedMessage?.Session);

            // Add the message to the conversation.
            conversation.InsertMessage (message, null);

            // If the ice cream is complete, save it in the history.
            if (iceCream.IsComplete) {
                var history = IceCreamHistory.Load ();
                history.Append (iceCream);
                history.Save ();
            }

            Dismiss ();
        }

        MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
        {
            var components = new NSUrlComponents {
                QueryItems = iceCream.QueryItems
            };

            var layout = new MSMessageTemplateLayout {
                Image = iceCream.RenderSticker (true),
                Caption = caption
            };

            var message = new MSMessage (session ?? new MSSession()) {
                Url = components.Url,
                Layout = layout
            };

            return message;
        }
    }
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

필요에 따라 앱을 사용할 수도 있습니다는 `WillTransition` (에서 같이 위의 Icecream 작성기 예제) 사용자에 게 표시 되기 전에 보기 모드 변경을 처리 하는 방법입니다. 자세한 내용은 참조 하십시오 우리의 [스티커를 자세히 사용자 지정](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md) 설명서.

## <a name="replying-to-a-message"></a>메시지에 회신

메시지 앱 확장을 메시지에 회신 하는 경우를 처리 해야 하는 두 가지 경우가 있습니다.

[![](advanced-message-app-extensions-images/interactive09.png "비활성 및 활성 모드에서 메시지 앱 확장")](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **확장은 비활성** -확장을 활성화 하 고 대화형 대화를 계속 하려면 사용자 탭 할 수 있는 메시지 대 본에서 메시지 앱 확장의 메시지 거품 중 하나가 있습니다.
- **확장은 활성** -에서 사용자가 확장 된 보기 모드를 입력 하 고 중단 된 위치에서 대화형 프로세스를 계속 메시지 기록을에서 메시지 앱 확장의 메시지 거품을 탭 합니다.

### <a name="the-extension-is-inactive"></a>확장은 비활성 상태입니다.

메시지 기록에서 사용자가 메시지 거품을 탭 할 때 메시지 앱 확장이 활성화 됩니다. 다음 프로세스가 발생 합니다.

[![](advanced-message-app-extensions-images/interactive10.png "비활성 메시지 거품을 처리합니다.")](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. 사용자 확장의 메시지 거품을 탭 합니다.
2. 확장 시작 되 면 메시지 앱 프로세스를 시작 됩니다.
3. `DidBecomeActive` 메서드가 호출 되어 전달 되는 `MSConversation` 메시지 앱 확장에서 실행 되는 대화를 나타내는입니다.
4. 확장을 기반으로 하기 때문에 `UIViewController` 둘 다 `ViewWillAppear` 고 `ViewDidAppear` 라고 합니다.

프로세스가 완료 되 면 확장 된 보기 모드에서 메시지 앱 확장 표시 됩니다.

### <a name="the-extension-is-active"></a>확장 활성 상태입니다.

메시지 기록에서 사용자가 메시지 거품을 탭 할 때 메시지 앱 확장이 활성화 됩니다 다음 프로세스가 발생 합니다.

[![](advanced-message-app-extensions-images/interactive11.png "활성 메시지 거품을 처리합니다.")](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. 사용자 확장의 메시지 거품을 탭 합니다.
2. 메시지 앱 확장을 이미 활성화, 때문에 합니다 `WillTransition` 메서드는 `MSMessagesAppViewController` 압축에서 확장 된 보기 모드로 전환 처리 하기 위해 호출 됩니다.
3. `DidSelectMessage` 메서드를 `MSMessagesAppViewController` 가 호출 되 고 전달 합니다 `MSMessage` 및 `MSConversation` 메시지 거품에 속하는.
4. 합니다 `DidTransition` 메서드는 `MSMessagesAppViewController` 압축에서 확장 된 보기 모드로 전환 처리 하기 위해 호출 됩니다.

마찬가지로 프로세스가 완료 되 면 확장 된 보기 모드에서 메시지 앱 확장 표시 됩니다.

### <a name="accessing-the-selected-message"></a>선택한 메시지에 액세스

두 경우 모두 사용자가 메시지 앱 확장에 속하는 메시지 거품을 탭 하면 필요에 액세스할 수는 `MSMessage` 를 사용 하 여 누른를 `SelectedMessage` 의 속성은 `MSConversation`.

예를 들어:

```csharp
using System;
using Foundation;
using Messages;
using UIKit;


namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        ...

        #region Override Methods
        public override void DidSelectMessage (MSMessage message, MSConversation conversation)
        {
            base.DidSelectMessage (message, conversation);

            // Get selected message
            var selected = conversation.SelectedMessage;

            // Present the user interface to continue editing
            // the conversation
            ...
        }
        #endregion
    }
}
```

선택한 메시지 메시지 앱 확장의 UI에 표시 하 고 응답을 작성 하는 사용자가 수 있습니다.

## <a name="removing-partially-completed-messages"></a>메시지 완료 부분적으로 제거

두 사용자는 대화에서 간에 대화형 대화의 다른 단계를 보내는 중 부분적으로 완료 된 메시지 풍선 어지럽히는 메시지 기록을 시작할 수 있습니다.

[![](advanced-message-app-extensions-images/interactive12.png "부분적으로 완료 된 메시지 풍선을 메시지 대 본을 어지럽히지 수 있습니다.")](advanced-message-app-extensions-images/interactive12.png#lightbox)

대신 메시지 앱 확장 메시지 대 본의 간결한 설명에 이전 메시지 풍선을 축소 해야 합니다.

[![](advanced-message-app-extensions-images/interactive13.png "메시지 기록의 이전 메시지 거품 축소")](advanced-message-app-extensions-images/interactive13.png#lightbox)

이 사용 하 여 처리 됩니다는 `MSSession` 기존 단계를 모두 축소 하려면. 하므로 `DidSelectMessage` 메서드는 `MSMessagesAppViewController` 클래스는 다음과 같이 수정 될 수 있습니다.

```csharp
public override void DidSelectMessage (MSMessage message, MSConversation conversation)
{
    base.DidSelectMessage (message, conversation);

    MSSession session;
    var summary = "";

    // Get selected message
    var selected = conversation.SelectedMessage;

    // Does the selected message already have a session?
    if (selected.Session == null) {
        // No, create a new session
        session = new MSSession ();
        summary = "Let's build an ice cream";
    } else {
        // Yes, use the existing session
        session = selected.Session;
        summary = "I added some scoops";
    }

    // Create an instance of the message being composed
    var existingMessage = new MSMessage (session);
    message.SummaryText = summary;
    ...

    // Present the user interface to continue editing
    // the conversation
    ...
}
```

선택한 메시지는 기존 이미 있는 경우 `MSSession`에 사용 되는 다른 새 `MSSession` 만들어집니다. 합니다 `SummaryText` 의 속성을 `MSMessage` 캡션을 축소 된 이전 단계를 추가할 때 사용 됩니다. 경우는 `SummaryText` 속성이 `null`, 대화 기록에서 대화의 이전 단계를 완전히 제거 됩니다.

## <a name="advanced-message-api-features"></a>고급 메시지 API 기능

위의 자세히 설명 하는 새 메시지 API의 기본 기능을 사용 하 여 Apple 프레임 워크에 빌드되어 있는 고급 기능 중 일부를 검사한 다음입니다.

먼저 여러 가지 다른 재정의 방법에는 `MSMessagesAppViewController` 대화에 대 한 심층적인 액세스를 제공 하는 클래스:

- `DidStartSendingMessage` -이 사용자가 보내기 단추를 누를 때 호출 됩니다. 메시지 배달 송신 프로세스 시작 것만 받는 사람에 게 실제로 되었는지 것은 아닙니다.
- `DidCancelSendingMessage` -이 사용자가 누를 때 발생 합니다 *X* 대화 기록에서 메시지 풍선의 오른쪽 위 모서리에 있는 단추입니다.
- `DidReceiveMessage` -이 메서드는 메시지 앱 확장 활성 상태일 때는 대화의 참가자 중 하나에서 새 메시지를 받았습니다.

### <a name="group-conversations"></a>그룹 대화

사용자 (사용 하 여 3 개 이상의 개인) 그룹 대화와 관련 된 디자인 및 메시지 앱 확장을 구현할 때 고려해 야 할이 하는 동안 메시지 앱 확장을 사용할 수 있습니다.

3 명의 사용자와 그룹의 대화에 다음 상호 작용을 살펴보십시오.

[![](advanced-message-app-extensions-images/interactive14.png "3 명의 사용자와 그룹의 대화에 대 한 상호 작용")](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. 1 사용자가 그룹 대화형 메시지를 보내면 사용자 2와 사용자 3 햄버거 토핑이 선택 하 게 합니다.
2. 사용자 2 tomatoes를 선택합니다.
3. 사용자 3 피를 선택합니다.
4. 사용자 2와 사용자 3 선택 사용자 1로 다시 거의 동시에 도착합니다. 결과적으로, 사용자 2의 선택한 요약 줄으로 축소 되 고를 사용할 수 없습니다. 이 경우 수 있는 대칭 이동 된, 사용자 2를 선택 하 여 표시 되 고 사용자 3의 되 고 축소 합니다.

두 경우 모두이 동작은 아닙니다 원하는 사용자 1 사용자 2와 3 사용자의 선택 항목을 액세스할 수 있어야 합니다. Apple 메시지 앱 확장 클라우드에서 메시지 상태를 저장 하 고의 URL 속성을 사용 하 여이 상황을 처리 하려면 제안 된 `MSMessage` (하는 전송 사용자 간의)이이 상태에 액세스 해야 합니다.

사용자가 메시지를 보내면 세션 토큰이 생성 되 고 현재 메시지 상태를 사용 하 여 클라우드로 푸시 합니다. 사용자 대화 기록에서 메시지 거품에 누르면 클라우드에서 현재 세션 상태를 검색 하려면 세션 토큰이 사용 됩니다.

### <a name="sender-identifiers"></a>보낸 사람 식별자

토론 메시지의 보낸 사람 식별자에 액세스 하려면 위에 지정 된 그룹 대화의 예제를 수행 합니다.

[![](advanced-message-app-extensions-images/interactive15.png "그룹 대화 식별자를 전송 합니다.")](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. 사용자 1 그룹 대화형 메시지를 전송 하는 다시 사용자 2와 사용자 3 햄버거 토핑이 선택 하 게 합니다.
2. 사용자 3 피를 선택합니다.
3. 사용자 1로 다시 도착 하는 사용자 3의 선택 및 사용자 2가 아직 응답 하지 않습니다.
4. 메시지 앱 확장 고유 식별자만 인식 Apple 사용자 개인 정보 보호를 사용 하 여 많은 주의 기울이고 이기 때문에 (으로 `NSUUID`) 각 참가자는 대화에 할당 된 합니다. 로컬 장치에만 현재 사용자의 식별자 라고 합니다.
5. 합니다 `MSMessage` 에 `SenderIdentifier` 사용자 중 하 나와 일치 하는 속성의 참가자 목록에 알려진 확장 합니다.
6. 각 사용자 장치에 다시 로컬 사용자의 식별자만 알려져 있는 참가자 목록 자체 복사본이 있습니다.
7. 메시지를 보내면 해당 `SenderIdentifier` 로컬 사용자의 속성 이라고 합니다.

보낸 사람 식별자는 다음과 같은 방법으로 사용할 수 있습니다.

- 참가자 목록 확인 하 여 확장 대화의 사용자 수를 가져올 수 있습니다.
- 확장에서 사용자 로부터 메시지를 수신 하는 경우이 추적할 수 보낸 사람 식별자입니다. 보낸 사람 id가 동일한 다른 메시지를 수신 하는 경우 확장을 같은 사용자 임을 알고 있습니다.
- 대화에서 특정 사용자를 식별 하는 데 사용할 수 있습니다.

보낸 사람 식별자의 텍스트 필드에 사용할 수는 `MSMessageTemplateLayout` 달러 기호를 접두사로 (`$`). 예를 들어:

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

대체할 메시지 앱이이 유형의 서식 지정을 사용 하 여 메시지 거품을 표시 합니다 `$uuid...` 메시지를 전송 하는 대화의 참가자의 연락처 이름입니다.

보낸 사람 식별자 대화의 각 참가자에 대 한 다른 고유 보낸 사람 식별자가 사용자 1의 장치 및 사용자 3의 장치 이번에 위의 다이어그램을 살펴보면 이므로 각 장치에서 고유 합니다.

보낸 사람 식별자는 메시지 앱 확장의 설치에 범위가 지정 됩니다. 따라서 사용자를 제거 하 고 메시지 앱 확장을 다시 설치 하는 경우 새 설치에 대 한 새 보낸 사람 식별자 생성 됩니다.

보낸 사람 식별자에 액세스 하려면 확장에 다음 코드를 따르면 됩니다.

```csharp
public override void DidStartSendingMessage (MSMessage message, MSConversation conversation)
{
    base.DidStartSendingMessage (message, conversation);

    // Get the sender's participant ID
    var senderID = message.SenderParticipantIdentifier;

    // Get the local participant ID
    var localID = conversation.LocalParticipantIdentifier;

    // Get the remote participant IDs
    var remoteIDs = conversation.RemoteParticipantIdentifiers;
}
```

## <a name="supported-platforms"></a>지원되는 플랫폼

메시지 앱 확장을 생성 하는 대화형 메시지는 다음 Apple 플랫폼에 제공 됩니다.

- watchOS 3
- macOS Sierra
- iOS 10

IOS 10의 세 가지 플랫폼에서 대화형 메시지를 생성 하려면 사용자가 수. 있습니다 MacOS Sierra에서 대화형 메시지 거품을 클릭할 경우 URL에 연결 합니다 `MSMessage` Safari에서 열릴 메시지 표현의 표시 해야 하 고 있습니다.

WatchOS에서 메시지 앱에 사용자 응답을 구성할 수 있는 연결 된 iOS 장치에 대화형 메시지 전달 수 있습니다.

새 메시지 API에 오래 된 Apple 플랫폼에서 대화형 메시지를 수신 하는 경우 대체 (fallback)에 대 한 지원이 있습니다.

- watchOS 2 +
- OS X 10.11 +
- iOS 9 +

별도 2 개 메시지로 대체 (fallback) 형식으로 전달 됩니다.

- 하나는 이미지가 됩니다 템플릿 레이아웃에서 제공 합니다.
- 다른은 URL에서 제공 되는 `MSMessage`합니다.

## <a name="summary"></a>요약

이 문서와 통합 되는 Xamarin.iOS 솔루션에서 메시지 앱 확장을 사용 하 여 작업에 대 한 고급 기술을 제시 했지만 합니다 **메시지** 앱 및 사용자에 게 새로운 기능을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [아이스크림 작성기 (샘플)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [메시지 참조](https://developer.apple.com/reference/messages)
- [앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
