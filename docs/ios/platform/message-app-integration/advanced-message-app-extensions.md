---
title: 고급 메시지 응용 프로그램 확장
description: 이 문서에서는 메시지 앱 뿐 아니라 통합 하 고 사용자에 게 새로운 기능을 제공 하는 Xamarin.iOS 솔루션에서 메시지 앱 확장을 사용 하기 위한 고급 기술을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: cd2cabf98c83bba7502e8533e482713a9c43f67a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="advanced-message-app-extensions"></a>고급 메시지 응용 프로그램 확장

_이 문서에서는 메시지 앱 뿐 아니라 통합 하 고 사용자에 게 새로운 기능을 제공 하는 Xamarin.iOS 솔루션에서 메시지 앱 확장을 사용 하기 위한 고급 기술을 보여 줍니다._


새로운와 통합 되어 메시지 앱 확장 iOS 10에는 **메시지** 사용자에 게 새 기능을 응용 프로그램 및 표시 합니다. 확장은 텍스트 스티커, 미디어 파일, 대화형 메시지를 보낼 수 있습니다.

## <a name="about-message-app-extensions"></a>메시지가 응용 프로그램 확장에 대 한

위에서 설명 했 듯이 메시지 앱 확장와 통합 하는 **메시지** 사용자에 게 새 기능을 응용 프로그램 및 표시 합니다. 확장은 텍스트 스티커, 미디어 파일, 대화형 메시지를 보낼 수 있습니다. 메시지 앱 확장의 두 종류 사용할 수 있습니다.

- **스티커 팩** -사용자는 메시지에 추가할 수 있는 스티커의 컬렉션을 포함 합니다. 코드를 작성 하지 않고 스티커 팩을 만들 수 있습니다.
- **응용 프로그램 iMessage** -스티커 선택, 텍스트를 입력, 미디어 파일 (선택 사항 형식 변환)를 포함 하 여 및 만들기, 편집 및 상호 작용 메시지를 보내는 메시지 앱 내에서 사용자 지정 사용자 인터페이스를 표시할 수 있습니다.

메시지 앱 확장 세 가지 기본 콘텐츠 형식을 제공합니다.

- **대화형 메시지** -한 유형의 응용 프로그램을 생성 하는 사용자 지정 메시지 내용, 사용자가 응용 프로그램 메시지를 누를 때 전경에서 시작 됩니다.
- **스티커** -는 사용자 간에 보낸 메시지에 포함 될 수 있는 앱에서 생성 되는 이미지입니다. 참조 우리의 [아이스크림 작성기](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) 스티커 팩 응용 프로그램의 예제 구현은 용 예제 응용 프로그램입니다.
- **기타 지원 콘텐츠** -응용 프로그램에서 사진, 동영상, 텍스트 또는 메시지가 응용 프로그램에서 항상 지원 형식의 링크와 같은 콘텐츠를 제공할 수 있습니다.

새로운 iOS 10에 메시지 앱 이제 포함 자체 전용, 기본 제공 앱 스토어입니다. 메시지 앱 확장을 포함 하는 모든 앱을 표시 하 고이 저장소에서 승격 됩니다. 새 메시지 앱 서랍에 빠르게 액세스할 수는 사용자에 게 메시지 앱 스토어에서 다운로드 한 모든 앱에 표시 됩니다.

또한 새로운 iOS 10, 사과 추가 했습니다 인라인 앱 Attribution 사용자를 쉽게 응용 프로그램을 검색할 수 있도록 합니다. 예를 들어 한 사용자는 두 번째 사용자가 앱에서 다른 콘텐츠를 전송 하는 경우 (예: 스티커 예:), 설치 보내는 응용 프로그램의 이름 아래에 있는지 메시지 기록의 콘텐츠입니다. 사용자가 응용 프로그램의 경우 이름, 열 수 있습니다 메시지 앱 저장소 및 저장소에 선택한 앱입니다.

메시지 응용 프로그램 확장은 기존 iOS 앱 만들기에 익숙한 개발자는 하 고 모든 표준 프레임 워크와 표준 iOS 앱의 기능에 액세스 한 유사 합니다. 예를 들어:

- 앱에서 바로 구매에 대 한 액세스를 갖습니다.
- Apple Pay 액세스를 권한이 있습니다.
- 카메라 같은 장치 하드웨어에 액세스할을 수 있습니다.

메시지 응용 프로그램 확장은 10 iOS에만 지원, 이러한 확장을 전송 하는 콘텐츠를 watchOS와 macOS 장치에서 볼 수 있는 반면 합니다. 새 _제외할지 페이지_ 에서 메시지 앱 확장을 포함 하 여 폰에서 보낸 최신 스티커 표시 되며 사용자 시계에서 이러한 스티커를 보낼 수 있도록 watchOS 3에 추가 합니다.

## <a name="about-interactive-messages"></a>대화형 메시지에 대 한

대화형 메시지 사용자 지정 메시지 거품을 표시 하 고 메시지 응용 프로그램 확장에서 제공 됩니다. 메시지 입력 필드에 삽입 하 고 보냅니다 대화형 메시지 콘텐츠를 만드는 데 사용할 수 있습니다.

[![](advanced-message-app-extensions-images/interactive01.png "대화형 메시지 콘텐츠 만들기")](advanced-message-app-extensions-images/interactive01.png#lightbox)

받는 사용자를 만든 메시지 앱 확장을 로드 하 고 메시지 기록에 해당 메시지 거품을 탭 하 여 대화형 메시지에 회신할 수 있습니다. 확장 전체 화면 시작된 되며 사용자 응답을 작성 하 고 다시 원래 사용자에 게 보낼 수 있도록 합니다.

[![](advanced-message-app-extensions-images/interactive02.png "확장 전체 화면 시작")](advanced-message-app-extensions-images/interactive02.png#lightbox)


다음 항목 아래에 자세히 설명 합니다.

- 메시지 API 개요
- 확장 프로그램 수명 주기
- 메시지 작성
- 메시지 보내기

## <a name="messages-api-overview"></a>메시지 API 개요

사용자가 호출 되 면 메시지 앱 확장의 간단한 보기 모드에서 메시지 기록 맨 아래에 표시 됩니다.

[![](advanced-message-app-extensions-images/interactive03.png "메시지 API 개요")](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. `MSMessageAppViewController` 메시지 앱 확장 개체가 확장의 뷰가 사용자에 게 표시 될 때 호출 되는 주 클래스입니다.
2. 사용자에 게 표시 되는 대화는 `MSConversation` 개체 인스턴스입니다.
3. `MSMessage` 클래스 대화에 지정 된 메시지 거품을 나타냅니다.
4. `MSSession` 메시지가 전송 되는 방식을 제어 합니다.
5. `MSMessageTemplateLayout` 메시지가 표시 되는 방식을 제어 합니다.

## <a name="the-extension-lifecycle"></a>확장 프로그램 수명 주기

활성화 되기 메시지 앱 확장의 과정을 살펴보겠습니다.

[![](advanced-message-app-extensions-images/interactive04.png "활성화 되기 메시지 앱 확장의 프로세스")](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. 확장 (예: 앱 서랍)에서 시작 되 면 메시지 응용 프로그램 프로세스를 시작 됩니다.
2. `DidBecomeActive` 메서드를 호출 하 고 전달 되는 `MSConversation` 메시지 앱 확장에서 실행 되는 대화를 나타내는입니다.
3. 확장을 기반으로 하기 때문에 `UIViewController` 둘 다 `ViewWillAppear` 및 `ViewDidAppear` 호출 됩니다.

다음으로 비활성화 되 고 메시지 앱 확장의 프로세스를 살펴보세요.

[![](advanced-message-app-extensions-images/interactive05.png "비활성화 되 고 메시지 앱 확장의 프로세스")](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. 메시지 앱 확장을 비활성화 하는 중 때는 `ViewWillDisappear` 메서드가 먼저 호출 됩니다.
2. 그런 다음 `ViewDidDisappear` 메서드가 호출 됩니다.
3. `WillResignActive` 메서드를 호출 하 고 전달 되는 `MSConversation` 메시지 앱 확장에서 실행 되는 대화를 나타내는입니다. 이 시점에서 메시지 앱 및 확장 간의 연결을 해제 합니다.
4. 나중에 메시지가 응용 프로그램에서 프로세스가 종료 됩니다.

확장 수명이 짧은 프로세스 이므로, 처리 및 배터리 전원을 절약 하기 위해 시스템에서 적극적으로 종료 됩니다. 개발자가 디자인 하 고 메시지 앱 확장을 구현 하는 경우 점을 명심 해야 합니다.

## <a name="composing-a-message"></a>메시지 작성

메시지 앱 확장 프로세스에서 실행 하 고 표시 하는 사용자 인터페이스를 새 메시지를 작성 하려면 다음 코드를 사용할 수 있습니다.

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

이 코드에서는 새 `MSMessage` 몇 속성을 설정 하 고 (같은 `Url`). IOS에만 메시지를 만들 수, 하는 동안 표시할 iOS 및 macOS에 보낼 수 있습니다.

사용자가 macOS에서 대화의 메시지 거품을 클릭 하면 Mac 웹 브라우저에서 URL에 지정 된 주소를 열려고 시도 합니다. 결과적으로, 개발자의 웹 사이트 macOS에서 웹 브라우저에서 메시지의 일부 표시 컴퓨터 기반 표시 할 수 있어야 합니다.

`AccessibilityLabel` 속성은 화면 판독기가 사용자에 게 대화의 대화 참여자를 읽는 데 사용 됩니다. `Layout` 속성 지정 방법을 메시지가 표시 됩니다, 현재만 `MSMessageTemplateLayout` 를 지원 하며 다음과 같습니다.

[![](advanced-message-app-extensions-images/interactive06.png "MSMessageTemplateLayout 서식 파일")](advanced-message-app-extensions-images/interactive06.png#lightbox)

`Image` 의 속성은 `MSMessageTemplateLayout` 화면의 MessageBubble의 본문에 콘텐츠를 제공 합니다. `MediaFileUrl` 속성 메시지 거품의 본문에 콘텐츠를 제공 하지만 지원 하지 않습니다는 콘텐츠에 대 한 허용 `UIImage` (예: 백그라운드에서 루프는 비디오 파일). 모두는 `Image` 및 `MediaFileUrl` 속성이 제공 됩니다는 `Image` 속성 우선 적용 됩니다. `MediaFileUrl` PNG, JPEG, GIF 및 모든 형식의 미디어 플레이어 프레임 워크에서 재생할 수 있는 비디오를 지 원하는 미디어 형식입니다.

권장된 미디어 크기는 3 x 해상도에서 300 x 300 픽셀입니다. 약간 장기 및 단기 자산도 사용할 수 하며, 사과 최상의 결과 얻으려면 몇 가지 다양 한 크기를 사용 하 여 테스트 합니다. 메시지 앱 다운 샘플 되며 필요에 따라이 미디어의 크기를 조정 합니다.

자산을 수신기로 보내면 연결 된 모든 미디어를 자동으로 코드는 네트워크를 통해 전송에서 최적화 하기 위해 메시지 앱으로 변환 됩니다 합니다. 이 인해 Apple 것을 권장 하지 미디어 축소 되 고 전송 하기 위해 압축 때문에 메시지에 첨부 하는 미디어의 텍스트를 포함 하 여 잠재적으로 렌더링이 텍스트 읽을 수 있습니다.

`ImageTitle` 및 `ImageSubtitle` 속성 메시지 거품에 표시 된 미디어에 대 한 설명을 제공 합니다. 이러한 속성 파일로 보내집니다 텍스트 받는 장치에 있는 또렷하게 보입니다 렌더링 됩니다 이미지의 왼쪽 아래에 있습니다.

`Caption`, `SubCaption`, `TrailingCaption` 및 `TrailingSubcaption` 속성에서 더욱 이미지를 설명 하 고 이미지 아래 섹션에서 렌더링 됩니다. 이러한 모든 속성을 설정 `null` 캡션 영역 없이 메시지 거품을 만듭니다.

[![](advanced-message-app-extensions-images/interactive07.png "캡션 영역의 없이 메시지 거품형")](advanced-message-app-extensions-images/interactive07.png#lightbox)

마지막은 메시지 응용 프로그램에서 메시지 앱 확장의 아이콘 메시지 거품의 왼쪽 위 모서리에는 그립니다.

## <a name="sending-a-message"></a>메시지 보내기

한 번는 `MSMessage` 다음 코드를 사용 하 여 전송할 수 구성 되었습니다.

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

`ActiveConversation` 속성은 `MSMessagesAppViewController` 현재 대화에서 메시지 앱 확장 실행 된 저장 됩니다.

호출는 `InsertMessage` 의 `MSConversation` 대화에서 메시지를 포함 하 여 발생할 수 있는 오류를 처리 합니다. 메시지가 성공적으로 포함 되어 있으면 메시지 거품 입력 필드에 표시 됩니다.

또한 확장와 같은 대화에 서로 다른 유형의 데이터를 보낼 수 있습니다.

- **텍스트** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **첨부 파일** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **스티커**  -  `ActiveConversation.InsertSticker (sticker, (obj) => {...});` 여기서 `sticker` 는 `MSSticker`합니다.

사용자는 파란색을 탭 하 여 메시지를 보낼 수는 입력 필드에 새 콘텐츠를 **보낼** 단추 (에서처럼 일반적인 메시지). 콘텐츠를 자동으로 보낼 메시지 응용 프로그램 확장에 대 한 방법이 있으면,이 과정은 완전히 사용자 제어 합니다.

## <a name="handling-the-compact-and-expanded-modes"></a>압축 및 확장 된 모드 처리

두 가지 다른 보기 모드 중 하나에서 메시지 앱 확장을 표시할 수 있습니다.

[![](advanced-message-app-extensions-images/interactive08.png "두 가지 다른 보기 모드에 표시 되는 메시지 응용 프로그램 확장: 압축 및 확장")](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** -메시지 보기의 아래쪽 25%를 메시지 앱 확장 사용 되는 기본 모드입니다. 축소 모드에서는 응용 프로그램 키보드, 가로 스크롤 또는 살짝 제스처 인식기에 액세스할 수 없는지 않습니다. 응용 프로그램 입력 필드에 액세스 권한이 및에 대 한 호출이 `InsertMessage` 없는 사용자에 게 즉시 표시 됩니다.
- **확장** -메시지 앱 확장의 전체 메시지 보기를 채웁니다. 키보드, 가로 스크롤 및 살짝 제스처 인식기에 액세스 하지 않는 하지만 입력 필드에 대 한 액세스 없는 합니다.

메시지 앱 확장 전환 될 수 있습니다 이러한 모드 간에 프로그래밍 방식으로 또는 수동으로 사용자가 언제 든 지 고 모든 응답 모드를 변경 하 보기에 바로 있어야 합니다.

두 가지 다른 보기 모드 간에 전환이 처리의 다음 예제를 살펴보십시오. 두 명의 서로 다른 뷰 컨트롤러는 각 상태에 대 한 요구 됩니다. `StickerBrowserViewController` 핸들의 **Compact** 보기 및 `AddStickerViewController` 처리할는 **확장** 보기:

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

`DidTransition` 메서드는 두 가지 모드 간 전환 처리 하도록 재정의 됩니다.

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

필요에 따라 응용 프로그램 사용 수는 `WillTransition` 으로 위의 코드 예에서 Icecream 작성기 사용자에 게 표시 되기 전에 보기 모드 변경을 처리 하는 메서드. 자세한 내용은 참조 하십시오 우리의 [추가로 스티커 사용자 지정](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md) 설명서입니다.

## <a name="replying-to-a-message"></a>메시지에 회신 하기

메시지 앱 확장 메시지에 회신 하는 경우를 처리 해야 하는 두 가지 경우에는

[![](advanced-message-app-extensions-images/interactive09.png "비활성 및 활성 모드에서 메시지 앱 확장")](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **확장은 비활성** -확장을 활성화 하는 대화형 대화 계속를 탭 하는 메시지 대화 내용에서 메시지 앱 확장의 메시지 거품 중 하나가 있습니다.
- **확장은 활성** -확장 된 보기 모드를 입력 하 고 중단 한 위치에서 대화형 프로세스를 계속 하려면 메시지 대화 내용에서 메시지 앱 확장의 메시지 거품형을 탭 합니다.

### <a name="the-extension-is-inactive"></a>확장은 비활성 상태입니다.

메시지 거품 메시지 기록에서 사용자가 탭 됩니다 때 메시지 앱 확장 아니기 때문에 다음 프로세스가 발생 합니다.

[![](advanced-message-app-extensions-images/interactive10.png "처리는 비활성 메시지 거품형")](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. 사용자가 확장의 메시지 거품을 탭합니다.
2. 확장 프로그램을 시작할 때 메시지가 응용 프로그램 프로세스를 시작 됩니다.
3. `DidBecomeActive` 메서드를 호출 하 고 전달 되는 `MSConversation` 메시지 앱 확장에서 실행 되는 대화를 나타내는입니다.
4. 확장을 기반으로 하기 때문에 `UIViewController` 둘 다 `ViewWillAppear` 및 `ViewDidAppear` 호출 됩니다.

프로세스가 완료 되 면 확장 된 보기 모드에서 메시지 앱 확장 표시 됩니다.

### <a name="the-extension-is-active"></a>확장 활성 상태입니다.

메시지 거품 메시지 기록에서 사용자가 탭이 수행 되 고 메시지 앱 확장이 활성 상태 인지를 다음 프로세스가 발생 합니다.

[![](advanced-message-app-extensions-images/interactive11.png "처리는 활성 메시지 거품형")](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. 사용자가 확장의 메시지 거품을 탭합니다.
2. 메시지 앱 확장을 활성화 이미 이므로 `WillTransition` 의 메서드는 `MSMessagesAppViewController` 압축에서 확장 된 보기 모드로 전환 처리 하기 위해 호출 됩니다.
3. `DidSelectMessage` 의 메서드는 `MSMessagesAppViewController` 호출 되 고 전달 되는 `MSMessage` 및 `MSConversation` 메시지 거품에 속하는 합니다.
4. `DidTransition` 의 메서드는 `MSMessagesAppViewController` 압축에서 확장 된 보기 모드로 전환 처리 하기 위해 호출 됩니다.

다시, 프로세스가 완료 되 면 확장 된 보기 모드에서 메시지 앱 확장 표시 됩니다.

### <a name="accessing-the-selected-message"></a>선택한 메시지에 액세스

두 경우 모두 사용자 메시지 앱 확장에 속한 메시지 거품을 누르면 해야 합니다 액세스 하는 `MSMessage` 를 사용 하 여 눌렀는지는 `SelectedMessage` 속성은 `MSConversation`합니다.

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

선택한 메시지에서 메시지 앱 확장의 UI에 표시 되어야 합니다 및 응답을 작성 하는 사용자가 요청을 수 있습니다.

## <a name="removing-partially-completed-messages"></a>메시지 완료 부분적으로 제거

두 개의 사용자 대화에서 간에 대화형 대화의 다른 단계를 보내는 과정에서 부분적으로 완료 된 메시지 거품 채울 메시지 대 본을 시작할 수 있습니다.

[![](advanced-message-app-extensions-images/interactive12.png "부분적으로 완료 된 메시지 거품 메시지 기록을 복잡 하 게 만들지 수 있습니다.")](advanced-message-app-extensions-images/interactive12.png#lightbox)

대신, 메시지 앱 확장 간결한 설명 메시지 대 본에 이전 메시지 거품을 축소 해야:

[![](advanced-message-app-extensions-images/interactive13.png "메시지 기록의 이전 메시지 거품을 축소")](advanced-message-app-extensions-images/interactive13.png#lightbox)

이 사용 하 여 처리 됩니다는 `MSSession` 을 기존 단계를 모두 축소 합니다. 하므로 `DidSelectMessage` 의 메서드는 `MSMessagesAppViewController` 클래스 다음과 같이 변경할 수 있습니다.

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

선택한 메시지 이미 있으면는 기존 `MSSession`, 그렇지 않으면 새 사용 `MSSession` 만들어집니다. `SummaryText` 의 속성은 `MSMessage` 캡션을 축소 된 이전 단계를 추가 하는 데 사용 됩니다. 경우는 `SummaryText` 속성이 `null`, 대화 대 본에서 대화의 이전 단계를 완전히 제거 됩니다.

## <a name="advanced-message-api-features"></a>고급 메시지 API 기능

위의 자세히 설명 하는 새로운 메시지 API의 기본 기능으로 다음에 검사 Apple 내장 프레임 워크에 대 한 고급 기능 중 일부입니다.

첫째, 몇 가지가 다른 재정의에 `MSMessagesAppViewController` 대화에 대 한 깊은 액세스를 제공 하는 클래스:

- `DidStartSendingMessage` -이 사용자가 보내기 단추를 누를 때 호출 됩니다. 메시지 배달 송신 프로세스가 시작 된 정당한 받는 사람에 게 실제로 되었음을 것은 아닙니다.
- `DidCancelSendingMessage` -사용자가 누를 때 발생 하는이 *X* 대화 대화 참여자에서 메시지 거품의 오른쪽 위 모퉁이의 단추입니다.
- `DidReceiveMessage` -이 메서드는 메시지 앱 확장이 활성 상태 인지는 대화의 참가자 중 하나에서 새 메시지를 받았습니다.

### <a name="group-conversations"></a>그룹 대화

사용자 (3 명 이상인 개인용)와 대화 그룹에에서 포함 된 제품 및 고려해 야 디자인 및 메시지 앱 확장을 구현 하는 경우에 아무런 동안 메시지 앱 확장을 사용할 수 있습니다.

3 명의 사용자와 그룹의 대화에 상호 작용을 살펴보세요.

[![](advanced-message-app-extensions-images/interactive14.png "3 명의 사용자와 그룹의 대화에 대 한 상호 작용")](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. 1 사용자가 그룹 대화형 메시지를 보낼 요청 사용자 2와 사용자 3 햄버거 토핑이 선택할 수 있습니다.
2. 사용자 2 tomatoes를 선택합니다.
3. 사용자 3 pickles를 선택합니다.
4. 사용자 2, 사용자 3을 모두 선택 사용자 1로 다시 거의 동시에 도착합니다. 결과적으로, 사용자 2 선택 요약 선으로 축소 및를 사용할 수 없습니다. 이 경우 수는 또한 대칭 이동 된, 사용자 2 항목으로 표시 되 고 사용자 3의 되 고 축소 합니다.

두 경우 모두이 동작은 없습니다 원하는 대로 사용자 1 사용자 2 및 3의 사용자를 모두 선택 항목에 액세스할 수 있어야 합니다. Apple 메시지 앱 확장 클라우드에서 메시지 상태를 저장 하 고 URL 속성을 사용이 상황을 처리 하려면 제안 된 `MSMessage` (하는 사용자 간의 사용 된다면)이이 상태 액세스할 수 있습니다.

사용자가 메시지를 보내면 세션 토큰 생성 되 고 현재 메시지 상태와 함께 클라우드 밀어넣습니다. 사용자 탭에 메시지 거품 대화 대화 내용에서 세션 토큰 클라우드에서 현재 세션 상태를 검색 하지 사용 됩니다.

### <a name="sender-identifiers"></a>보낸 사람 식별자

메시지 보낸 사람 식별자에 액세스 토론 하 위에 제시한 그룹 대화를 예로 들어를 보겠습니다.

[![](advanced-message-app-extensions-images/interactive15.png "그룹 대화 식별자를 전송 합니다.")](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. 사용자 1 그룹 대화형 메시지를 전송 하는 다시 요청 사용자 2와 사용자 3 햄버거 토핑이 선택할 수 있습니다.
2. 사용자 3 pickles를 선택합니다.
3. 사용자 3의 선택 사용자 1로 다시 나타나고 사용자 2가 아직 회신 하지 않습니다.
4. 고유 식별자에 대 한 메시지 앱 확장만 알고 Apple 사용자 개인 정보 보호 최선을 다하고 이기 때문에 (으로 `NSUUID`) 각 참가자는 대화에 할당 된 합니다. 로컬 장치에서 현재 사용자의 식별자만 알려져 있습니다.
5. `MSMessage` 에 `SenderIdentifier` 사용자 중 하 나와 일치 하는 속성의 참가자가 목록에서에 알려진 확장 합니다.
6. 각 사용자 장치에 로컬 사용자의 식별자만 알고 다시 있는 참여자 목록 자체 복사본이 있습니다.
7. 메시지를 보낼 때 해당 `SenderIdentifier` 로컬 사용자의 속성이 라고도 합니다.

보낸 사람 식별자는 다음과 같은 방법으로 사용할 수 있습니다.

- 참여자 목록을 확인 하 여 확장 대화에서 사용자의 수를 확인할 수 있습니다.
- 확장 사용자 로부터 메시지를 받으면 것의 추적할 수 보낸 사람 식별자입니다. 보낸 사람 식별자가 같은 다른 메시지를 수신 하는 경우 확장 같은 사용자 임을 알고 있습니다.
- 대화에서 특정 사용자를 식별 하는 데 사용할 수 있습니다.

보낸 사람 식별자의 텍스트 필드에 사용할 수는 `MSMessageTemplateLayout` 달러 기호를 접두사로 (`$`). 예를 들어:

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

이러한 종류의 서식 지정와 메시지 거품을 표시 하는 메시지 앱, 하면 대체 됩니다는 `$uuid...` 메시지를 보낸 대화의 참가자의 연락처 이름으로 합니다.

보낸 사람 식별자는 대화의 각 참가자에 대 한 다른 고유 보낸 사람 식별자가 해당 사용자 1 장치 및 사용자 3 장치 이번에 위의 다이어그램을 살펴보면 하므로, 각 장치에서 고유 합니다.

보낸 사람 식별자 메시지 앱 확장의 설치 범위가 지정 됩니다. 따라서 사용자를 제거 하 고 메시지 앱 확장을 다시 설치 하는 경우 새 설치에 대 한 새 보낸 사람 식별자 생성 됩니다.

보낸 사람 식별자에 액세스 하려면 확장 다음 코드를 사용할 수 있습니다.

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

다음 Apple 플랫폼에서 메시지 응용 프로그램 확장에 의해 생성 되는 대화형 메시지 배달 됩니다.

- watchOS 3
- macOS Sierra
- iOS 10

세 플랫폼 iOS 10에는 대화형 메시지를 생성 하 사용자 수 있게 됩니다. 시에라 macOS에서 사용자가 대화형 메시지 거품을 클릭할 경우 URL에 연결 된는 `MSMessage` Safari에서 열 수 있는 메시지 표현을 표시 되지 않도록 합니다.

WatchOS, 메시지 앱 핸드 오프를 대화형 메시지는 사용자 응답을 구성할 수 있는 연결 된 iOS 장치를 수행할 수 있습니다.

새 메시지 API 이전 Apple 플랫폼에서 대화형 메시지를 수신 하는 경우 대체 (fallback)에 대 한 지원을 있습니다.

- watchOS 2 +
- OS X 10.11 +
- iOS 9 +

두 개의 별도 메시지로 대체 (fallback) 형식으로 전달 됩니다.

- 하나는 이미지는 됩니다 템플릿 레이아웃에서 제공 합니다.
- 제공 되는 URL 됩니다 다른는 `MSMessage`합니다.

## <a name="summary"></a>요약

이 문서와 통합 된 Xamarin.iOS 솔루션에서 메시지 앱 확장을 사용 하기 위한 고급 기술을 제시한는 **메시지** 응용 프로그램 및 사용자에 게 새로운 기능을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [아이스크림 작성기 (샘플)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [메시지 참조](https://developer.apple.com/reference/messages)
- [응용 프로그램 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
