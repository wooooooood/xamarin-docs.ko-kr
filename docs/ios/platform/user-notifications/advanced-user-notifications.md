---
title: Xamarin.iOS에서 고급 사용자 알림
description: 이 문서에서는 iOS 10에에서 도입 된 사용자 알림 프레임 워크를 자세히 살펴보겠습니다. 사용자 알림, 사용자 알림 인터페이스, 미디어 첨부 파일, 사용자 지정 사용자 인터페이스 및 자세히 설명합니다.
ms.prod: xamarin
ms.assetid: 4E0C60AE-6F54-4098-8FA0-AADF9AC86805
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/03/2018
ms.openlocfilehash: 255603eefb4d7cfd3b906e1744aa19da6a77259a
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865288"
---
# <a name="advanced-user-notifications-in-xamarinios"></a>Xamarin.iOS에서 고급 사용자 알림

새 ios 10 프레임 워크를 제공 하 고 로컬 및 원격 알림이 처리에 대 한 허용 사용자 알림입니다. 앱 또는 앱 확장은이 프레임 워크를 사용 하 여, 위치와 같은 조건 집합 또는 시간을 지정 하 여 로컬 알림 배달을 예약할 수 있습니다.

## <a name="about-user-notifications"></a>사용자 알림 정보

새 사용자 알림 프레임 워크를 제공 하 고 로컬 및 원격 알림 처리할 수 있습니다. 앱 또는 앱 확장은이 프레임 워크를 사용 하 여, 위치와 같은 조건 집합 또는 시간을 지정 하 여 로컬 알림 배달을 예약할 수 있습니다.

또한 앱 또는 확장 수 수신 (및 잠재적으로 수정할) 로컬 및 원격 알림이 사용자의 iOS 장치에 배달 하는 대로 합니다.

새 사용자 알림 UI 프레임 워크에는 앱 또는 앱 확장 사용자에 게 표시 될 때 로컬 및 원격 알림의 모양을 사용자 지정할 수 있습니다.

이 프레임 워크는 앱 사용자에 게 알림을 배달할 수 있는 다음과 같은 방법으로 제공 합니다.

- **시각적 경고가** -알림이 있는 배너로 화면 맨 위에서 세분화 합니다.
- **사운드 및 진동** -알림과 연결할 수 있습니다.
- **앱 아이콘 배지** -앱의 아이콘에는 새 콘텐츠를 사용할 수 있는지를 보여 주는 배지를 표시 하는 경우. 읽지 않은 전자 메일 메시지의 수 예:입니다.

또한 사용자의 현재 컨텍스트에 따라 여러 가지는 알림이 표시 됩니다.

- 장치 잠금 해제 된 경우 알림을 롤 다운 화면 맨 위에서 배너로 합니다.
- 장치가 잠겨 있는 경우 사용자의 잠금 화면에서 알림이 표시 됩니다.
- 사용자가 알림의 놓친 경우 알림 센터를 열 수 있으며 있는 모든 사용 가능한 대기 알림을 봅니다.

Xamarin.iOS 앱에 두 가지 유형의 사용자 알림을 보낼 수 있습니다.

- **로컬 알림** -이러한 사용자가 장치에 로컬로 설치 하는 앱에서 전송 됩니다.
- **원격 알림** -하 고 사용자에 게 표시 하거나 원격 서버에서 보낸 또는 앱의 내용의 백그라운드 업데이트를 트리거합니다.

자세한 내용은 참조 하십시오 우리의 [향상 된 사용자 알림](~/ios/platform/user-notifications/enhanced-user-notifications.md) 설명서.

## <a name="the-new-user-notification-interface"></a>새 사용자 알림 인터페이스

IOS 10에서에서 사용자 알림 장치 또는 알림 센터에서 맨 위에 있는 배너도 잠금 화면에서 제목, 부제목 및 표시할 수 있는 선택적 미디어 첨부 파일 등 더 많은 콘텐츠를 제공 하는 새로운 UI 디자인을 사용 하 여 표시 됩니다.

사용자 알림을 표시 되는 위치 iOS 10에서에서,에 관계 없이 동일한 모양 및 느낌와 동일한 기능 및 기능을 사용 하 여 표시 됩니다.

IOS 8, Apple 개발자 수 알림 사용자 지정 작업을 연결 하 고 사용자가 앱을 실행 하지 않고 알림 작업을 수행 하도록 허용 하는 실행 가능한 알림을 도입 했습니다. Apple ios 9에서 사용자가 텍스트 입력을 사용 하 여 알림에 응답을 허용 하는 빠른 응답을 사용 하 여 실행 가능한 알림 향상 되었습니다.

사용자 알림 iOS 10에서에서 사용자 환경을 더 핵심 이기 때문에 Apple에 필요에 따라 확장 여기서 알림를 누르면 하 고 사용자 지정 사용자 인터페이스를 다양 한 상호 작용을 제공 하는 표시는 3D 터치를 지원 하기 위해 실행 가능한 알림 알림.

UI 사용자 지정 사용자 알림 표시 되 면 알림 연결 된 모든 작업을 사용 하 여 사용자 상호 작용 하는 경우, 변경 내용에 대 한 피드백을 보내도록 사용자 지정 UI는 즉시 업데이트할 수 있습니다.

새 iOS 10, 사용자 알림 UI API를 쉽게 이러한 새 사용자 알림 UI 기능을 활용 하기 위해 Xamarin.iOS 앱을 허용 합니다.

## <a name="adding-media-attachments"></a>미디어 첨부 파일 추가

사용자 간에 공유 하는 보다 일반적인 항목 중 하나 이므로 사진, 미디어 항목 (예: 사진)를 연결 하는 기능을 추가 하는 iOS 10 알림을 표시 하 고 알림의 conte의 나머지 부분과 함께 사용자가 쉽게 사용할 수 있게 하려면 직접 nt 합니다.

그러나 전송에 관련 된 크기 때문에 작은 이미지를 연결 하는 원격 알림 페이로드 실용적이 지 합니다. 이 상황을 처리 하려면 개발자를 다른 원본 (예: CloudKit 데이터 저장소)에서 이미지를 다운로드 하 여 사용자에 게 표시 되기 전에 알림 내용에 첨부할 iOS 10에서에서 새 서비스 확장을 사용할 수 있습니다.

서비스 확장에서 수정할 원격 알림에 해당 페이로드 표시 되어야 합니다으로 변경할 수 있습니다. 예를 들어:

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

프로세스의 다음 개요를 살펴보십시오.

[![](advanced-user-notifications-images/extension02.png "추가 미디어 첨부 파일 처리")](advanced-user-notifications-images/extension02.png#lightbox)

서비스 확장에 필요한 모든 방법을 통해 필요한 이미지를 다운로드할 수 있습니다 (APNs)를 통해 장치에는 원격 알림이 배달 되 면 (예는 `NSURLSession`) 알림 및 표시의 콘텐츠를 수정할 수 있는 이미지를 수신한 후 사용자에 해당 합니다.

다음은이 프로세스 코드에서 처리 될 수 있습니다 하는 방법의 예입니다.

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyNotification
{
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Constructors
        public NotificationService ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            // Get file URL
            var attachementPath = request.Content.UserInfo.ObjectForKey (new NSString ("my-attachment"));
            var url = new NSUrl (attachementPath.ToString ());

            // Download the file
            var localURL = new NSUrl ("PathToLocalCopy");

            // Create attachment
            var attachmentID = "image";
            var options = new UNNotificationAttachmentOptions ();
            NSError err;
            var attachment = UNNotificationAttachment.FromIdentifier (attachmentID, localURL, options , out err);

            // Modify contents
            var content = request.Content.MutableCopy() as UNMutableNotificationContent;
            content.Attachments = new UNNotificationAttachment [] { attachment };

            // Display notification
            contentHandler (content);
        }

        public override void TimeWillExpire ()
        {
            // Handle service timing out
        }
        #endregion
    }
}
```

APNs에서 알림을 받으면 콘텐츠에서 이미지의 사용자 지정 주소를 읽고 파일 서버에서 다운로드 됩니다. 다음 `UNNotificationAttachement` 고유 ID 및 이미지의 로컬 위치를 사용 하 여 만들어집니다 (으로 `NSUrl`). 알림 콘텐츠를 변경할 수 있는 복사본이 생성 됩니다 하 고 미디어 첨부 파일 추가 됩니다. 호출 하 여 사용자에 게 알림이 표시 됩니다는 마지막으로 `contentHandler`합니다.

알림 첨부 파일에 추가 되 면 시스템을 이동 및 파일의 관리를 인계 받습니다.

위의 원격 알림을 외에도 미디어 첨부 파일은 로컬 알림에서 지원 또한 여기서는 `UNNotificationAttachement` 만들어지고 해당 콘텐츠와 함께 알림을 연결할입니다.

IOS 10에서에서 알림 이미지의 미디어 첨부 파일 지원 (정적 및 Gif), 오디오 또는 비디오와 시스템 자동으로 표시 됩니다 올바른 사용자 지정 UI 각 이러한 유형의 첨부 파일에 대 한 알림을 사용자에 게 표시 되 면 합니다.

> [!NOTE]
> 미디어 크기를 최적화 하기 위해 주의 해야 하 고 시스템으로 원격 서버에서 미디어를 다운로드 하려면 (또는 로컬 알림을 위한 미디어를 조합 하)는 시간 제한을 엄격한 둘 다에 app Service 확장을 실행 하는 경우. 예를 들어, 이미지의 간소한 버전 또는 알림에 표시할 비디오의 약식 클립을 전송 하는 것이 좋습니다.

## <a name="creating-custom-user-interfaces"></a>사용자 지정 사용자 인터페이스 만들기

해당 사용자 알림에 대 한 사용자 지정 사용자 인터페이스를 만들려면 개발자는 앱의 솔루션에 새 ios 10 알림 콘텐츠 확장을 추가 해야 합니다.

알림 콘텐츠 확장에는 개발자를 자신의 뷰 알림 UI를 추가 하 고 원하는 모든 내용을 그리는 수 있습니다. 알림 콘텐츠 확장 iOS 12부터 단추 및 슬라이더와 같은 대화형 UI 컨트롤을 지원 합니다. 자세한 내용은 참조는 [iOS 12에서에서 대화형 알림을](~/ios/platform/introduction-to-ios12/notifications/interactive.md) 설명서.

사용자 알림을 사용 하 여 사용자 상호 작용을 지원 하려면 사용자 지정 작업 생성, 시스템에 등록 된 있고 시스템을 사용 하 여 예약 하기 전에 알림을에 연결 합니다. 이러한 작업을 처리 하는 알림 콘텐츠 확장 호출 됩니다. 참조를 [알림 작업을 사용 하 여 작업](~/ios/platform/user-notifications/enhanced-user-notifications.md) 섹션을 [향상 된 사용자 알림](~/ios/platform/user-notifications/enhanced-user-notifications.md) 사용자 지정 작업에 대 한 자세한 내용은 문서.

사용자 지정 UI를 사용 하 여 사용자 알림을 사용자에 게 표시 되 면 다음 요소를 갖습니다.

[![](advanced-user-notifications-images/customui01.png "사용자 지정 UI 요소를 사용 하 여 사용자 알림")](advanced-user-notifications-images/customui01.png#lightbox)

(알림을 아래 제시 된) 사용자 지정 작업을 사용 하 여 사용자 상호 작용을 하는 경우 지정 된 작업을 호출할 때 발생 하는 대상으로 사용자 피드백을 제공 하려면 사용자 인터페이스를 업데이트할 수 있습니다.

### <a name="adding-a-notification-content-extension"></a>알림 콘텐츠 확장을 추가합니다.

Xamarin.iOS 앱에 사용자 지정 사용자 알림 UI를 구현 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac 용 Visual Studio에서 앱의 솔루션을 열으십시오
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭 합니다 **Solution Pad** 선택한 **추가** > **새 프로젝트 추가**합니다.
3. 선택 **iOS** > **확장** > **알림 콘텐츠 확장** 을 클릭 합니다 **다음** 단추: 

    [![](advanced-user-notifications-images/notify01.png "알림 콘텐츠 확장 선택")](advanced-user-notifications-images/notify01.png#lightbox)
4. 입력을 **이름을** 확장을 클릭 합니다 **다음** 단추: 

    [![](advanced-user-notifications-images/notify02.png "확장의 이름을 입력 합니다.")](advanced-user-notifications-images/notify02.png#lightbox)
5. 조정 합니다 **프로젝트 이름** 및/또는 **솔루션 이름** 필요 하 고 클릭 합니다 **만들기** 단추: 

    [![](advanced-user-notifications-images/notify03.png "프로젝트 이름 및/또는 솔루션 이름을 조정합니다")](advanced-user-notifications-images/notify03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Mac 용 Visual Studio에서 앱의 솔루션을 열으십시오
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** 선택한 **추가 > 새 프로젝트...** .
3. 선택 **시각적 C# > iOS 확장 > 알림 콘텐츠 확장**:

    [![](advanced-user-notifications-images/notify01.w157-sml.png "알림 콘텐츠 확장 선택")](advanced-user-notifications-images/notify01.w157.png#lightbox)
4. 입력을 **이름을** 확장을 클릭 합니다 **확인** 단추입니다.

-----

알림 콘텐츠 확장 솔루션에 추가 되 면 확장의 프로젝트에 세 개의 파일이 만들어집니다.

1. `NotificationViewController.cs` -알림 콘텐츠 확장에 대 한 기본 보기 컨트롤러입니다.
2. `MainInterface.storyboard` -여기서 개발자 레이아웃 표시 되는 UI iOS 디자이너에 있는 알림 콘텐츠 확장에 대 한 합니다.
3. `Info.plist` -알림 콘텐츠 확장의 구성을 제어합니다.

기본 `NotificationViewController.cs` 파일은 다음과 같습니다.

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

        }
        #endregion
    }
}
```

합니다 `DidReceiveNotification` 알림을 알림 콘텐츠 확장의 내용 지정 하 여 사용자 지정 UI를 채울 수 있도록 사용자가 확장 되 면 메서드는 `UNNotification`합니다. 위의 예제에 대 한 레이블을 추가한 이름 사용 하 여 코드에 노출 보기로 `label` 알림의 본문을 표시 하는 데 사용 됩니다.

### <a name="setting-the-notification-content-extensions-categories"></a>알림 콘텐츠 확장의 범주를 설정합니다.

시스템에 응답할 때 특정 범주를 기반으로 앱의 알림 콘텐츠 확장을 찾는 방법에 숙지 해야 합니다. 다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 확장의 두 번 클릭 `Info.plist` 파일을 **Solution Pad** 을 편집용으로 엽니다.
2. 으로 전환 합니다 **원본** 보기.
3. 확장 된 `NSExtension` 키입니다.
4. 추가 된 `UNNotificationExtensionCategory` 형식으로 키 **문자열** 확장 속한 범주의 값을 사용 하 여 (이 예제의 ' 이벤트 초대): 

    [![](advanced-user-notifications-images/customui02.png "UNNotificationExtensionCategory 키 추가")](advanced-user-notifications-images/customui02.png#lightbox)
5. 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 확장의 두 번 클릭 `Info.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다.
2. 확장 된 `NSExtension` 키입니다.
3. 추가 된 `UNNotificationExtensionCategory` 형식으로 키 **문자열** 확장 속한 범주의 값을 사용 하 여 (이 예제의 ' 이벤트 초대): 

    [![](advanced-user-notifications-images/customui02w.png "UNNotificationExtensionCategory 키 추가")](advanced-user-notifications-images/customui02w.png#lightbox)
4. 변경 내용을 저장합니다.

-----

알림 콘텐츠 확장 범주 (`UNNotificationExtensionCategory`) 알림 작업을 등록 하는 데 사용 되는 동일한 범주 값을 사용 합니다. 전환 하는 경우 앱 여러 범주에 대 한 동일한 UI를 사용 합니다는 `UNNotificationExtensionCategory` 형식으로 **배열** 모든 필요한 범주를 제공 합니다. 예를 들어:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](advanced-user-notifications-images/customui03.png "알림 콘텐츠 확장 범주")](advanced-user-notifications-images/customui03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui03w.png "알림 콘텐츠 확장 범주")](advanced-user-notifications-images/customui03w.png#lightbox)

-----

### <a name="hiding-the-default-notification-content"></a>기본 알림 내용 숨기기

여기서 사용자 지정 알림 UI를 표시할 수 동일한 콘텐츠를 기본 알림 (제목, 부제목 및 본문 알림 UI의 맨 아래에 자동으로 표시)으로 상황에이 기본 정보를 를추가하여숨길수있습니다`UNNotificationExtensionDefaultContentHidden`키를 `NSExtensionAttributes` 형식으로 키 **부울** 값을 사용 하 여 `YES` 확장의 `Info.plist` 파일:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](advanced-user-notifications-images/customui04.png "기본 정보 찾기")](advanced-user-notifications-images/customui04.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui04w.png "기본 정보 찾기")](advanced-user-notifications-images/customui04w.png#lightbox)

-----

### <a name="designing-the-custom-ui"></a>사용자 지정 UI를 디자인합니다.

알림 콘텐츠 확장의 사용자 지정 사용자 인터페이스를 디자인 하려면 두 번 클릭 합니다 `MainInterface.storyboard` iOS 디자이너에서에서 편집 하기 위해 열려는 파일을 끌어서 원하는 인터페이스를 작성 해야 하는 요소 (같은 `UILabels` 고 `UIImageViews`).

> [!NOTE]
> IOS 12 일부 터 알림 콘텐츠 확장 단추 및 텍스트 필드와 같은 대화형 컨트롤을 포함할 수 있습니다. 자세한 내용은 참조는 [iOS 12에서에서 대화형 알림을](~/ios/platform/introduction-to-ios12/notifications/interactive.md) 설명서.

UI에 배치 하 고 필요한 컨트롤에 노출 되 면 C# 코드를 열고는 `NotificationViewController.cs` 편집에 대 한 수정 및는 `DidReceiveNotification` 알림을 확장할 때 UI를 채우는 방법입니다. 예:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

        }
        #endregion
    }
}
```

### <a name="setting-the-content-area-size"></a>콘텐츠 영역 크기를 설정합니다.

사용자에 게 표시 하는 콘텐츠 영역의 크기를 조정 하려면 아래 코드를 설정 하는 것을 `PreferredContentSize` 속성에는 `ViewDidLoad` 메서드 원하는 크기를 합니다. IOS 디자이너에서에서 보기 제약 조건을 적용 하 여이 크기를 조정할 수도, 개발자에 게 가장 적합 한 방법을 선택 하도록 하는 것이 그대로 있습니다.

알림 콘텐츠 확장은 호출 알림 콘텐츠 영역 전 시스템 이미 실행 되 고 먼저 때문에 전체 크기 조정 및 사용자에 게 표시 될 때 요청된 된 크기까지 애니메이션 효과 줄.

이 효과 제거 하려면 편집를 `Info.plist` 파일에 대 한 확장을 `UNNotificationExtensionInitialContentSizeRatio` 의 키를 `NSExtensionAttributes` 키를 입력 **수** 원하는 비율을 나타내는 값을 사용 하 여 합니다. 예:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](advanced-user-notifications-images/customui05.png "UNNotificationExtensionInitialContentSizeRatio 키")](advanced-user-notifications-images/customui05.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui05w.png "UNNotificationExtensionInitialContentSizeRatio 키")](advanced-user-notifications-images/customui05w.png#lightbox)

-----

### <a name="using-media-attachments-in-custom-ui"></a>사용자 지정 UI에서 미디어 첨부 파일을 사용 하 여

때문에 미디어 첨부 파일 (에서처럼 합니다 [미디어 첨부 파일 추가](#adding-media-attachments) 위의 섹션) 알림 페이로드가의 일부인, 액세스 및 기본에서 것 처럼 알림 콘텐츠 확장에 표시 된 수 알림 UI입니다.

예를 들어, 위의 사용자 지정 UI를 포함 하는 경우는 `UIImageView` 에 노출 된는 C# 코드를 다음 코드는 미디어 첨부 파일을 사용 하 여에서 채우는 데 사용할 수 없습니다.

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;

namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }
        #endregion
    }
}
```

있기 때문에 미디어 첨부 파일 시스템에 의해 관리 되는 앱의 샌드박스 외부입니다. 호출 하 여 파일에 액세스 한다는 것을 시스템에 알리기 위해 해야 하는 확장 된 `StartAccessingSecurityScopedResource` 메서드. 확장 파일을 사용 하 여 완료 되 면 호출 해야 하는 `StopAccessingSecurityScopedResource` 해당 연결을 해제 합니다.

### <a name="adding-custom-actions-to-a-custom-ui"></a>사용자 지정 UI를 사용자 지정 작업 추가

사용자 지정 알림 UI에 대화형 작업을 추가 하려면 사용자 지정 작업 단추를 사용할 수 있습니다. 참조를 [알림 작업을 사용 하 여 작업](~/ios/platform/user-notifications/enhanced-user-notifications.md) 섹션을 [향상 된 사용자 알림](~/ios/platform/user-notifications/enhanced-user-notifications.md) 사용자 지정 작업에 대 한 자세한 내용은 문서.

사용자 지정 작업을 하는 것 외에도 알림 콘텐츠 확장도 다음과 같은 기본 제공 작업에 응답할 수 있습니다.

- **기본 작업** -사용자가 앱을 열고 지정 된 알림의 세부 정보를 표시 하는 알림을 탭 하는 경우입니다.
- **작업 해제** -이 작업 사용자 지정된 알림을 해제 하는 경우 앱에 전송 됩니다.

알림 콘텐츠 확장 수도 사용자 사용자 지정 작업 중 하나를 호출 하는 경우 해당 UI를 업데이트 하는 기능, 사용자가 수락 하는 경우 날짜를 표시 합니다 **Accept** 사용자 지정 작업 단추입니다. 또한 알림 콘텐츠 확장 시스템 알림을 닫히기 전에 사용자가 작업의 효과 볼 수 있도록 알림 UI의 사건의 지연에 알 수 있습니다.

두 번째 버전을 구현 하 여 이렇게는 `DidReceiveNotification` 는 완료 처리기를 포함 하는 메서드. 예:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;
using CoreGraphics;

namespace myApp {
    public class NotificationViewController : UIViewController, UNNotificationContentExtension {

        public override void ViewDidLoad() {
            base.ViewDidLoad();

            // Adjust the size of the content area
            var size = View.Bounds.Size
            PreferredContentSize = new CGSize(size.Width, size.Width/2);
        }

        public void DidReceiveNotification(UNNotification notification) {

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = Content.UserInfo["location"] as string;
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments[0];
                if (attachment.Url.StartAccessingSecurityScopedResource()) {
                    EventImage.Image = UIImage.FromFile(attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch(response.ActionIdentifier){
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
    }
}
```

추가 하 여를 `Server.PostEventResponse` 처리기는 `DidReceiveNotification` 알림 콘텐츠 확장, 확장 메서드의 *해야* 모든 사용자 지정 작업을 처리 합니다. 확장에 포함 된 앱에 사용자 지정 동작을 변경 하 여도 전달할 수는 `UNNotificationContentExtensionResponseOption`합니다. 예를 들어:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>사용자 지정 UI에 텍스트 입력된 작업을 사용 하 여 작업

앱의 알림을 디자인에 따라 (예: 메시지에 회신) 알림을에 텍스트를 입력 해야 할 시간 있을 수 있습니다. 알림 콘텐츠 확장을 표준 알림을 않습니다 것 처럼 기본 제공 텍스트 입력된 작업에 액세스할 수 있습니다.

예를 들어:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Computed Properties
        // Allow to take input
        public override bool CanBecomeFirstResponder {
            get { return true; }
        }

        // Return the custom created text input view with the
        // required buttons and return here
        public override UIView InputAccessoryView {
            get { return InputView; }
        }
        #endregion

        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Private Methods
        private UNNotificationCategory MakeExtensionCategory ()
        {

            // Create Accept Action
            ...

            // Create decline Action
            ...

            // Create Text Input Action
            var commentID = "comment";
            var commentTitle = "Comment";
            var textInputButtonTitle = "Send";
            var textInputPlaceholder = "Enter comment here...";
            var commentAction = UNTextInputNotificationAction.FromIdentifier (commentID, commentTitle, UNNotificationActionOptions.None, textInputButtonTitle, textInputPlaceholder);

            // Create category
            var categoryID = "event-invite";
            var actions = new UNNotificationAction [] { acceptAction, declineAction, commentAction };
            var intentIDs = new string [] { };
            var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);

            // Return new category
            return category;

        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Is text input?
            if (response is UNTextInputNotificationResponse) {
                var textResponse = response as UNTextInputNotificationResponse;
                Server.Send (textResponse.UserText, () => {
                    // Close Notification
                    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
                });
            }

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch (response.ActionIdentifier) {
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
        #endregion
    }
}
```

이 코드 새 텍스트 입력된 작업을 만들고 확장의 범주에 추가 (에 `MakeExtensionCategory`) 메서드. `DidReceive` 메서드를 재정의 다음 코드를 사용 하 여 텍스트를 입력 하는 사용자를 처리 합니다.

```csharp
// Is text input?
if (response is UNTextInputNotificationResponse) {
    var textResponse = response as UNTextInputNotificationResponse;
    Server.Send (textResponse.UserText, () => {
        // Close Notification
        completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
    });
}
```

텍스트 입력 필드에 사용자 지정 단추를 추가 하는 것에 대 한 디자인 호출을 하는 경우 다음 코드를 포함 시킬 추가 합니다.

```csharp
// Allow to take input
public override bool CanBecomeFirstResponder {
    get {return true;}
}

// Return the custom created text input view with the
// required buttons and return here
public override UIView InputAccessoryView {
    get {return InputView;}
}
```

주석 작업을 사용자에 의해 트리거되면 뷰 컨트롤러 및 사용자 지정 텍스트 입력된 필드를 모두 활성화할 필요 합니다.

```csharp
// Update UI when the user interacts with the
// Notification
Server.PostEventResponse += (response) {
    // Take action based on the response
    switch(response.ActionIdentifier){
    ...
    case "comment":
        BecomeFirstResponder();
        TextField.BecomeFirstResponder();
        break;
    }

    // Close Notification
    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);

};
```

## <a name="summary"></a>요약

이 문서에서는 새 사용자 알림 프레임 워크를 사용 하 여 Xamarin.iOS 앱에는 고급 확인을 수행 했습니다. 미디어 첨부 파일을 로컬 및 원격 알림이 추가 설명 및 사용자 지정 알림 Ui를 만드는 새 사용자 알림 UI를 사용 하는 방법을 다루었습니다.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications 프레임 워크 참조](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
