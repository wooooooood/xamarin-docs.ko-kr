---
title: Xamarin.ios의 고급 사용자 알림
description: 이 문서에서는 iOS 10에 도입 된 사용자 알림 프레임 워크를 자세히 살펴봅니다. 사용자 알림, 사용자 알림 인터페이스, 미디어 첨부 파일, 사용자 지정 사용자 인터페이스 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 4E0C60AE-6F54-4098-8FA0-AADF9AC86805
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/03/2018
ms.openlocfilehash: 28734af7c3d9958462e47ff6b11a0f9d0e06bcfb
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655406"
---
# <a name="advanced-user-notifications-in-xamarinios"></a>Xamarin.ios의 고급 사용자 알림

IOS 10의 새로운 기능으로, 사용자 알림 프레임 워크를 사용 하면 로컬 및 원격 알림을 배달 하 고 처리할 수 있습니다. 이 프레임 워크를 사용 하 여 앱 또는 앱 확장은 위치 또는 시간 등의 조건 집합을 지정 하 여 로컬 알림의 배달을 예약할 수 있습니다.

## <a name="about-user-notifications"></a>사용자 알림 정보

새 사용자 알림 프레임 워크를 사용 하면 로컬 및 원격 알림을 배달 하 고 처리할 수 있습니다. 이 프레임 워크를 사용 하 여 앱 또는 앱 확장은 위치 또는 시간 등의 조건 집합을 지정 하 여 로컬 알림의 배달을 예약할 수 있습니다.

또한 앱 또는 확장은 사용자의 iOS 장치에 전달 될 때 로컬 및 원격 알림을 수신 하 고 잠재적으로 수정할 수 있습니다.

새 사용자 알림 UI 프레임 워크를 사용 하면 앱 또는 앱 확장이 사용자에 게 표시 될 때 로컬 및 원격 알림의 모양을 사용자 지정할 수 있습니다.

이 프레임 워크는 앱이 사용자에 게 알림을 제공할 수 있는 다음과 같은 방법을 제공 합니다.

- **시각적 경고** -알림이 화면 위쪽에서 배너로 롤업되는 위치입니다.
- **Sound 및 Vibrations** -알림과 연결할 수 있습니다.
- **앱 아이콘 배지** -앱의 아이콘이 새 콘텐츠를 사용할 수 있음을 보여 주는 배지를 표시 합니다. 읽지 않은 전자 메일 메시지의 수와 같습니다.

또한 사용자의 현재 컨텍스트에 따라 알림이 표시 되는 방법에는 여러 가지가 있습니다.

- 장치의 잠금이 해제 되 면 화면 맨 위에서 배너로 롤오버 됩니다.
- 장치가 잠기면 사용자의 잠금 화면에 알림이 표시 됩니다.
- 사용자가 알림을 누락 하는 경우 알림 센터를 열고 사용 가능한 모든 대기 중인 알림을 볼 수 있습니다.

Xamarin.ios 앱에는 보낼 수 있는 두 가지 유형의 사용자 알림이 있습니다.

- **로컬 알림** -사용자 장치에 로컬로 설치 된 앱에서 전송 됩니다.
- **원격 알림** -원격 서버에서 전송 되 고 사용자에 게 제공 되거나 앱 콘텐츠의 백그라운드 업데이트를 트리거합니다.

자세한 내용은 [향상 된 사용자 알림](~/ios/platform/user-notifications/enhanced-user-notifications.md) 설명서를 참조 하세요.

## <a name="the-new-user-notification-interface"></a>새 사용자 알림 인터페이스

IOS 10에서 사용자에 게 표시 되는 새로운 UI 디자인을 사용 하면 잠금 화면에 표시 될 수 있는 제목, 부제목 및 선택적 미디어 첨부 파일 등의 콘텐츠를 장치 또는 알림 센터의 위쪽에 배너 형태로 제공 합니다.

IOS 10에서 사용자 알림이 표시 되는 위치에 관계 없이 동일한 모양과 느낌을 가진 동일한 기능 및 기능을 제공 합니다.

IOS 8에서 Apple은 개발자가 알림에 사용자 지정 작업을 연결 하 고 사용자가 앱을 시작 하지 않고도 알림에 대해 조치를 취할 수 있는 조치 가능한 알림을 도입 했습니다. IOS 9에서 사용자가 텍스트 입력을 사용 하 여 알림에 응답할 수 있도록 하는 빠른 회신을 통해 Apple의 조치 가능한 알림이 사용 됩니다.

사용자 알림은 iOS 10의 사용자 환경에서 더 중요 한 부분 이므로, Apple은 3D 터치를 지원 하기 위해 사용자가 알림을 누르고 사용자 지정 사용자 인터페이스를 표시 하 여 풍부한 상호 작용을 제공 하는 3D 터치를 지원 합니다. 알림을 사용 합니다.

사용자 지정 사용자 알림 UI가 표시 되 면 사용자가 알림에 연결 된 작업과 상호 작용 하는 경우 변경 된 내용에 대 한 피드백을 제공 하도록 사용자 지정 UI를 즉시 업데이트할 수 있습니다.

IOS 10의 새로운 기능인 사용자 알림 UI API를 사용 하면 Xamarin.ios 앱에서 이러한 새로운 사용자 알림 UI 기능을 쉽게 활용할 수 있습니다.

## <a name="adding-media-attachments"></a>미디어 첨부 파일 추가

사용자 간에 공유 되는 더 일반적인 항목 중 하나는 사진 이므로 iOS 10은 알림에 직접 미디어 항목 (예: 사진)을 연결 하는 기능을 추가 하 여 사용자가 알림 메시지를 표시 하 고 사용자에 게 즉시 제공 하는 기능을 제공 합니다. 4.0.

그러나 작은 이미지를 전송 하는 것과 관련 된 크기 때문에 원격 알림 페이로드에 연결 하는 것은 실용적이 지 않습니다. 이러한 상황을 처리 하기 위해 개발자는 iOS 10의 새로운 서비스 확장을 사용 하 여 다른 원본 (예: CloudKit 데이터 저장소)에서 이미지를 다운로드 하 고 사용자에 게 표시 하기 전에 알림 콘텐츠에 연결할 수 있습니다.

서비스 확장에서 원격 알림을 수정 하려면 해당 페이로드를 변경 가능으로 표시 해야 합니다. 예를 들어:

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

프로세스에 대 한 다음 개요를 살펴보겠습니다.

[![](advanced-user-notifications-images/extension02.png "미디어 첨부 파일 추가 프로세스")](advanced-user-notifications-images/extension02.png#lightbox)

원격 알림이 장치 (APNs를 통해)에 전달 되 면 서비스 확장에서 원하는 수단 (예: `NSURLSession`)을 통해 필요한 이미지를 다운로드 하 고 이미지를 받은 후에 알림 콘텐츠를 수정 하 고 표시할 수 있습니다. 사용자에 게 있습니다.

다음은 코드에서이 프로세스를 처리 하는 방법의 예입니다.

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

APNs에서 알림을 받으면 이미지의 사용자 지정 주소를 콘텐츠에서 읽고 파일이 서버에서 다운로드 됩니다. 그런 다음 `UNNotificationAttachement` 고유한 ID 및 이미지의 로컬 위치 ( `NSUrl`)를 사용 하 여를 만듭니다. 변경 가능한 알림 콘텐츠의 복사본이 만들어지고 미디어 첨부 파일이 추가 됩니다. 마지막으로를 호출 `contentHandler`하 여 사용자에 게 알림을 표시 합니다.

알림에 첨부 파일이 추가 되 면 시스템은 파일의 이동 및 관리를 수행 합니다.

위에서 설명한 원격 알림 외에도, 미디어 첨부 파일은 로컬 알림에서 지원 됩니다. 여기서는 `UNNotificationAttachement` 를 만들고 해당 콘텐츠와 함께 알림에 첨부 합니다.

IOS 10의 알림은 이미지 (정적 및 Gif)의 미디어 첨부 파일을 지원 하며, 오디오 또는 비디오를 사용 하면 사용자에 게 알림이 제공 될 때 이러한 각 첨부 파일 형식에 대 한 올바른 사용자 지정 UI가 자동으로 표시 됩니다.

> [!NOTE]
> 시스템에서 앱의 서비스 확장을 실행할 때 둘 다에 대해 엄격한 제한을 적용 하므로 미디어 크기와 원격 서버에서 미디어를 다운로드 하는 데 걸리는 시간 (또는 로컬 알림에 대 한 미디어를 조합 하는 데 걸리는 시간)을 모두 최적화 해야 합니다. 예를 들어, 화면에 표시 되는 축소 된 버전의 이미지 또는 작은 비디오 클립을 전송 하는 것이 좋습니다.

## <a name="creating-custom-user-interfaces"></a>사용자 지정 사용자 인터페이스 만들기

사용자 알림에 대 한 사용자 지정 사용자 인터페이스를 만들려면 개발자가 앱의 솔루션에 알림 콘텐츠 확장 (iOS 10에 새로 추가)을 추가 해야 합니다.

알림 콘텐츠 확장을 사용 하면 개발자가 알림 UI에 자신의 보기를 추가 하 고 원하는 콘텐츠를 그릴 수 있습니다. IOS 12부터 알림 콘텐츠 확장 프로그램은 단추 및 슬라이더와 같은 대화형 UI 컨트롤을 지원 합니다. 자세한 내용은 [iOS 12 설명서의 대화형 알림](~/ios/platform/introduction-to-ios12/notifications/interactive.md) 을 참조 하세요.

사용자 알림과의 사용자 상호 작용을 지원 하려면 사용자 지정 작업을 만들어 시스템에 등록 하 고 시스템에 예약 하기 전에 알림에 연결 해야 합니다. 알림 콘텐츠 확장은 이러한 동작의 처리를 처리 하기 위해 호출 됩니다. 사용자 지정 작업에 대 한 자세한 내용은 [고급 사용자 알림](~/ios/platform/user-notifications/enhanced-user-notifications.md) 문서의 [알림 작업으로 작업](~/ios/platform/user-notifications/enhanced-user-notifications.md) 섹션을 참조 하세요.

사용자에 게 사용자 지정 UI가 있는 사용자 알림이 표시 되 면 다음 요소를 갖게 됩니다.

[![](advanced-user-notifications-images/customui01.png "사용자 지정 UI 요소를 사용 하는 사용자 알림")](advanced-user-notifications-images/customui01.png#lightbox)

사용자가 알림 아래에 표시 되는 사용자 지정 작업과 상호 작용 하는 경우 사용자가 지정 된 작업을 호출 했을 때 발생 하는 상황으로 사용자 의견을 제공 하도록 사용자 인터페이스를 업데이트할 수 있습니다.

### <a name="adding-a-notification-content-extension"></a>알림 콘텐츠 확장 추가

Xamarin.ios 앱에서 사용자 지정 사용자 알림 UI를 구현 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac용 Visual Studio에서 앱 솔루션을 엽니다.
2. **Solution Pad** 에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 프로젝트 추가**를 선택 합니다.
3. **IOS** > **확장** 알림 콘텐츠 확장을 선택 하 고 다음 단추를 클릭 합니다. >  

    [![](advanced-user-notifications-images/notify01.png "알림 콘텐츠 확장 선택")](advanced-user-notifications-images/notify01.png#lightbox)
4. 확장의 **이름을** 입력 하 고 **다음** 단추를 클릭 합니다. 

    [![](advanced-user-notifications-images/notify02.png "확장의 이름 입력")](advanced-user-notifications-images/notify02.png#lightbox)
5. 필요한 경우 **프로젝트 이름** 및/또는 **솔루션 이름을** 조정 하 고 **만들기** 단추를 클릭 합니다. 

    [![](advanced-user-notifications-images/notify03.png "프로젝트 이름 및/또는 솔루션 이름 조정")](advanced-user-notifications-images/notify03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Mac용 Visual Studio에서 앱 솔루션을 엽니다.
2. **솔루션 탐색기** 에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 프로젝트**...를 선택 합니다.
3. **C# Visual > IOS 확장 > 알림 콘텐츠 확장**을 선택 합니다.

    [![](advanced-user-notifications-images/notify01.w157-sml.png "알림 콘텐츠 확장 선택")](advanced-user-notifications-images/notify01.w157.png#lightbox)
4. 확장의 **이름을** 입력 하 고 **확인** 단추를 클릭 합니다.

-----

솔루션에 알림 콘텐츠 확장이 추가 되 면 확장의 프로젝트에 세 개의 파일이 생성 됩니다.

1. `NotificationViewController.cs`-알림 콘텐츠 확장에 대 한 주 뷰 컨트롤러입니다.
2. `MainInterface.storyboard`-개발자가 iOS 디자이너에서 알림 콘텐츠 확장에 대해 표시 되는 UI를 레이아웃 합니다.
3. `Info.plist`-알림 콘텐츠 확장의 구성을 제어 합니다.

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

알림 콘텐츠 확장이 사용자 지정 UI를 `UNNotification`의 콘텐츠로 채울 수 있도록 사용자가 알림을 확장할 때 메서드가호출됩니다.`DidReceiveNotification` 위의 예제에서 레이블은 이름 `label` 으로 코드에 노출 되 고 알림의 본문을 표시 하는 데 사용 되는 뷰에 추가 되었습니다.

### <a name="setting-the-notification-content-extensions-categories"></a>알림 콘텐츠 확장의 범주 설정

응답 하는 특정 범주에 따라 앱의 알림 콘텐츠 확장을 찾는 방법에 대 한 정보가 시스템에 있어야 합니다. 다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. `Info.plist` **Solution Pad** 에서 확장 파일을 두 번 클릭 하 여 편집용으로 엽니다.
2. **원본** 뷰로 전환 합니다.
3. 키를 `NSExtension` 확장 합니다.
4. 확장이 속하는 범주의 값을 사용 하 여 키를형식문자열로추가합니다(이예제에서는'이벤트-초대'`UNNotificationExtensionCategory` ). 

    [![](advanced-user-notifications-images/customui02.png "UNNotificationExtensionCategory 키 추가")](advanced-user-notifications-images/customui02.png#lightbox)
5. 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. `Info.plist` **솔루션 탐색기** 에서 확장 파일을 두 번 클릭 하 여 편집용으로 엽니다.
2. 키를 `NSExtension` 확장 합니다.
3. 확장이 속하는 범주의 값을 사용 하 여 키를형식문자열로추가합니다(이예제에서는'이벤트-초대'`UNNotificationExtensionCategory` ). 

    [![](advanced-user-notifications-images/customui02w.png "UNNotificationExtensionCategory 키 추가")](advanced-user-notifications-images/customui02w.png#lightbox)
4. 변경 내용을 저장합니다.

-----

알림 콘텐츠 확장 범주 (`UNNotificationExtensionCategory`)는 알림 작업을 등록 하는 데 사용 되는 것과 동일한 범주 값을 사용 합니다. 앱에서 여러 범주에 대해 동일한 UI를 사용 하는 경우를 형식 **배열로** 전환 `UNNotificationExtensionCategory` 하 고 필요한 모든 범주를 제공 합니다. 예를 들어:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](advanced-user-notifications-images/customui03.png "알림 콘텐츠 확장 범주")](advanced-user-notifications-images/customui03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui03w.png "알림 콘텐츠 확장 범주")](advanced-user-notifications-images/customui03w.png#lightbox)

-----

### <a name="hiding-the-default-notification-content"></a>기본 알림 내용 숨기기

사용자 지정 알림 UI가 기본 알림과 동일한 콘텐츠를 표시 하는 경우 (제목, 부제목 및 본문은 알림 UI의 맨 아래에 자동으로 표시 됨), 다음을 추가 `UNNotificationExtensionDefaultContentHidden`하여이기본정보를숨길수있습니다.확장 `NSExtensionAttributes` `YES` 파일의 값을 사용 하는 부울 형식의 키에 대 한 키입니다. `Info.plist`

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](advanced-user-notifications-images/customui04.png "기본 정보 찾기")](advanced-user-notifications-images/customui04.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui04w.png "기본 정보 찾기")](advanced-user-notifications-images/customui04w.png#lightbox)

-----

### <a name="designing-the-custom-ui"></a>사용자 지정 UI 디자인

알림 콘텐츠 확장의 사용자 지정 사용자 인터페이스를 디자인 하려면 `MainInterface.storyboard` 파일을 두 번 클릭 하 여 편집용으로 엽니다. iOS 디자이너에서 편집 하려면 원하는 인터페이스를 작성 하는 데 필요한 요소 ( `UILabels` 예: 및 `UIImageViews`)를 끌어 놓습니다.

> [!NOTE]
> IOS 12부터 알림 콘텐츠 확장에는 단추와 텍스트 필드와 같은 대화형 컨트롤이 포함 될 수 있습니다. 자세한 내용은 [iOS 12 설명서의 대화형 알림](~/ios/platform/introduction-to-ios12/notifications/interactive.md) 을 참조 하세요.

Ui가 배치 되 고 필요한 컨트롤이 코드에 C# 노출 되 면 편집용으로를 `NotificationViewController.cs` 열고 사용자가 알림을 확장할 때 ui를 채우도록 메서드를 `DidReceiveNotification` 수정 합니다. 예를 들어:

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

### <a name="setting-the-content-area-size"></a>콘텐츠 영역 크기 설정

사용자에 게 표시 되는 콘텐츠 영역의 크기를 조정 하려면 아래 코드에서 `PreferredContentSize` `ViewDidLoad` 메서드의 속성을 원하는 크기로 설정 합니다. IOS 디자이너에서 뷰에 제약 조건을 적용 하 여이 크기를 조정할 수도 있습니다 .이 크기는 개발자에 게 가장 적합 한 방법을 선택 하는 것입니다.

알림 콘텐츠 확장을 호출 하기 전에 알림 시스템이 이미 실행 되 고 있기 때문에 콘텐츠 영역은 전체 크기를 시작 하 고 사용자에 게 표시 될 때 요청 된 크기에 애니메이션을 적용 합니다.

이 효과를 제거 하려면 확장에 `Info.plist` 대 한 파일을 편집 하 고 `UNNotificationExtensionInitialContentSizeRatio` `NSExtensionAttributes` 키의 키를 원하는 비율을 나타내는 값으로 형식 **Number** 로 설정 합니다. 예:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](advanced-user-notifications-images/customui05.png "UNNotificationExtensionInitialContentSizeRatio 키")](advanced-user-notifications-images/customui05.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](advanced-user-notifications-images/customui05w.png "UNNotificationExtensionInitialContentSizeRatio 키")](advanced-user-notifications-images/customui05w.png#lightbox)

-----

### <a name="using-media-attachments-in-custom-ui"></a>사용자 지정 UI에서 미디어 첨부 파일 사용

위의 미디어 첨부 [파일 추가](#adding-media-attachments) 섹션에 표시 된 미디어 첨부 파일은 알림 페이로드의 일부 이므로 기본 알림 UI에 있는 것 처럼 알림 콘텐츠 확장에서 액세스 하 고 표시할 수 있습니다.

예를 들어 위의 사용자 지정 UI가 코드에 `UIImageView` C# 노출 된를 포함 하는 경우 다음 코드를 사용 하 여 미디어 첨부 파일에서로 채울 수 있습니다.

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

미디어 첨부 파일은 시스템에 의해 관리 되기 때문에 앱의 샌드박스 외부에 있습니다. 확장은 메서드를 `StartAccessingSecurityScopedResource` 호출 하 여 파일에 대 한 액세스를 필요로 함을 시스템에 알려야 합니다. 확장명이 파일을 사용 하 여 수행 되는 경우를 호출 `StopAccessingSecurityScopedResource` 하 여 연결을 해제 해야 합니다.

### <a name="adding-custom-actions-to-a-custom-ui"></a>사용자 지정 UI에 사용자 지정 작업 추가

사용자 지정 동작 단추를 사용 하 여 사용자 지정 알림 UI에 대화형 작업을 추가할 수 있습니다. 사용자 지정 작업에 대 한 자세한 내용은 [고급 사용자 알림](~/ios/platform/user-notifications/enhanced-user-notifications.md) 문서의 [알림 작업으로 작업](~/ios/platform/user-notifications/enhanced-user-notifications.md) 섹션을 참조 하세요.

사용자 지정 작업 외에도 알림 콘텐츠 확장 프로그램은 다음과 같은 기본 제공 작업에 응답할 수 있습니다.

- **기본 작업** -사용자가 알림을 탭 하 여 앱을 열고 지정 된 알림의 세부 정보를 표시 하는 경우입니다.
- **작업 해제** -이 작업은 사용자가 지정 된 알림을 해제할 때 앱에 전송 됩니다.

사용자가 사용자 지정 작업 **수락** 단추를 탭 할 때 수락 된 날짜를 표시 하는 것과 같은 사용자 지정 작업 중 하나를 호출 하는 경우 알림 콘텐츠 확장에도 UI를 업데이트할 수 있습니다. 또한 알림 콘텐츠 확장은 알림이 종결 되기 전에 사용자가 작업의 영향을 확인할 수 있도록 알림 UI의 해제 지연 하도록 시스템에 지시할 수 있습니다.

완료 처리기를 포함 하는 `DidReceiveNotification` 메서드의 두 번째 버전을 구현 하 여이 작업을 수행 합니다. 예:

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

알림 콘텐츠 확장 `Server.PostEventResponse` 의 `DidReceiveNotification` 메서드에 처리기를 추가 하 여 확장은 모든 사용자 지정 작업을 처리 *해야* 합니다. 또한 확장은를 `UNNotificationContentExtensionResponseOption`변경 하 여 포함 하는 앱에 사용자 지정 작업을 전달할 수 있습니다. 예를 들어:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>사용자 지정 UI에서 텍스트 입력 작업 사용

앱 및 알림 디자인에 따라 사용자가 알림 (예: 메시지에 회신)에 텍스트를 입력 해야 하는 경우가 있을 수 있습니다. 알림 콘텐츠 확장은 표준 알림과 마찬가지로 기본 제공 텍스트 입력 작업에 액세스할 수 있습니다.

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

이 코드는 새 텍스트 입력 작업을 만들어 확장의 범주 ( `MakeExtensionCategory`) 메서드에 추가 합니다. `DidReceive` Override 메서드에서는 다음 코드를 사용 하 여 텍스트를 입력 하는 사용자를 처리 합니다.

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

디자인에서 텍스트 입력 필드에 사용자 지정 단추를 추가 하기 위해를 호출 하는 경우 다음 코드를 추가 하 여 포함 합니다.

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

사용자가 주석 작업을 트리거한 경우 보기 컨트롤러와 사용자 지정 텍스트 입력 필드를 모두 활성화 해야 합니다.

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

이 문서에서는 Xamarin.ios 앱에서 새로운 사용자 알림 프레임 워크를 사용 하는 방법을 자세히 살펴봅니다. 로컬 및 원격 알림에 미디어 첨부 파일을 추가 하 고 새 사용자 알림 UI를 사용 하 여 사용자 지정 알림 ui를 만드는 방법에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [UserNotifications 프레임 워크 참조](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
