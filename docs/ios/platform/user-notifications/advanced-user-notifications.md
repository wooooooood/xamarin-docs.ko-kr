---
title: 고급 사용자 알림
description: 이 문서에서는 활용 전체 Xamarin.iOS 앱에서 하는 방법과 새 사용자 알림을 프레임 워크를 자세히 살펴보겠습니다.
ms.prod: xamarin
ms.assetid: 4E0C60AE-6F54-4098-8FA0-AADF9AC86805
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/03/2018
ms.openlocfilehash: bd8a95afc5bdd5aed958913d63f9b6cfe853677e
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="advanced-user-notifications"></a>고급 사용자 알림

_이 문서에서는 활용 전체 Xamarin.iOS 앱에서 하는 방법과 새 사용자 알림을 프레임 워크를 자세히 살펴보겠습니다._

10, 배달 및 로컬 및 원격 알림 처리를 위한 프레임 워크에서는 사용자 알림 iOS를 처음 사용 합니다. 응용 프로그램 또는 응용 프로그램 확장은이 프레임 워크를 사용 하 여 위치와 같은 조건 집합이 또는 시간을 지정 하 여 로컬 알림 배달을 예약할 수 있습니다.

## <a name="about-user-notifications"></a>사용자 알림 정보

배달 및 로컬 및 원격 알림 처리를 위한 새로운 사용자 알림 프레임 워크 수 있습니다. 응용 프로그램 또는 응용 프로그램 확장은이 프레임 워크를 사용 하 여 위치와 같은 조건 집합이 또는 시간을 지정 하 여 로컬 알림 배달을 예약할 수 있습니다.

또한 응용 프로그램 또는 확장 받을 (하 고 잠재적으로 수정할 수) 알림을 로컬 및 원격 사용자의 iOS 장치에 배달 될 때는 합니다.

새 사용자 알림 UI 프레임 워크에는 응용 프로그램 또는 사용자에 게 제공 하는 경우에 로컬 및 원격 알림 표시를 사용자 지정에 대 한 응용 프로그램 확장 수 있습니다.

이 프레임 워크에서는 앱 사용자에 게 알림을 배달할 수 있는 다음과 같은 방법으로 제공 합니다.

- **Visual 경고** -알림 위치 배너로 화면 맨 위에서부터 세분화 합니다.
- **사운드 및 진동** -알림과 연결 될 수 있습니다.
- **응용 프로그램 아이콘 배지** -새 콘텐츠를 사용할 수 있는지를 보여 주는 배지를 표시 하는 응용 프로그램의 아이콘 경우. 읽지 않은 전자 메일 메시지의 수 예:입니다.

또한 사용자의 현재 컨텍스트에 따라 여러 가지 있는 알림이 표시 됩니다.

- 장치가 잠긴 경우 알림을 공개 아래로 화면 맨 위에서부터 배너로.
- 장치가 잠겨 있으면 사용자의 잠금 화면에 알림을 표시 됩니다.
- 사용자가 알림을 누락, 하는 경우 알림 센터를 열 수 있으며 있는 모든 사용 가능한 대기 알림을 보려면 있습니다.

Xamarin.iOS 앱에 두 가지 유형의 사용자 알림을 보낼 수 있습니다.

- **로컬 알림을** -이러한 사용자가 장치에 로컬로 설치 된 앱으로 전송 됩니다.
- **원격 알림** -에서 원격 서버와 사용자에 게 전송 하거나 응용 프로그램의 콘텐츠 백그라운드 업데이트가 발생 합니다.

자세한 내용은 참조 하십시오 우리의 [향상 된 사용자 알림을](~/ios/platform/user-notifications/enhanced-user-notifications.md) 설명서입니다.

## <a name="the-new-user-notification-interface"></a>새 사용자 알림 인터페이스

사용자 알림 iOS 10에서에서 장치 또는 알림 센터에서 맨 위에 배너로 잠금 화면에서 제목, 부제목 및 표시 될 수 있는 선택적 미디어 첨부 파일 같은 더 많은 콘텐츠를 제공 하는 새 UI 디자인 표시 됩니다.

여기서 사용자 알림 iOS 10에에서 표시 되 면에 관계 없이 동일한 모양 및 느낌와 동일한 기능에 표시 됩니다.

IOS 8, Apple 조치 가능한 알림 기능 개발자 수는 알림을 사용자 지정 작업을 연결 하 고 사용자가 알림을에서 응용 프로그램을 실행 하지 않고도 작업을 수행 하도록 허용을 제공 합니다. IOS 9, 사과 텍스트 항목 관련 된 알림을에 응답 하도록 사용자 수 있도록 빠른 응답으로 실행 가능한 알림 향상.

사용자 알림 iOS 10에서에서 사용자 환경의 더 필수적인 부분 이므로, Apple에 필요에 따라 확장 3D 터치 여기서 사용자가 알림을 누를 하 고 사용자 지정 사용자 인터페이스는 다양 한 상호 작용을 제공 하는 디스플레이 지원 하기 위해 조치 가능한 알림 알림에서 합니다.

UI 사용자 지정 사용자 알림이 표시 되 면 알림을에 연결 된 작업을 사용자 상호 작용 하는 경우, 변경 내용에 대 한 의견을 제공 하려면 사용자 지정 UI는 즉시 업데이트할 수 있습니다.

새로운 iOS 10, 사용자 알림 UI API 쉽게 이러한 새 UI 사용자 알림 기능을 사용 하는 Xamarin.iOS 앱을 허용 합니다.

## <a name="adding-media-attachments"></a>미디어 첨부 파일 추가

사용자 간에 공유 되는 보다 일반적인 항목 중 하나는 사진, iOS 10 추가 미디어 항목 (예: 사진)에 연결할 수 있는 됩니다 제시 하 고 알림의 conte의 나머지 부분과 함께 사용자에 게 쉽게 사용할 수는 알림에 직접 nt 합니다.

그러나 전송에 관련 된 크기가 더 작은 이미지에 대 한 원격 알림 페이로드를 연결는 비현실적이 됩니다. 이 상황을 처리 하는 개발자는 (예: CloudKit 데이터 저장소의 경우) 다른 소스에서 이미지를 다운로드 하 고 사용자에 게 표시 되기 전에 알림의 콘텐츠를 연결 하려면 iOS 10에서에서 새 서비스 확장을 사용할 수 있습니다.

서비스 확장에 의해 수정 될 원격 알림에 페이로드 표시 되어야 합니다 변경 가능한 것으로. 예를 들어:

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

다음 프로세스의 개요를 살펴봅니다.

[![](advanced-user-notifications-images/extension02.png "추가 미디어 첨부 파일 프로세스")](advanced-user-notifications-images/extension02.png#lightbox)

서비스 확장 (APNs)를 통해 장치에 원격 알림이 배달 되 면 원하는 모든 수단을 통해 필요한 이미지 다운로드할 수 있습니다 (예:는 `NSURLSession`) 및 이미지를 수신한 후 알림 및 표시 폴더의 내용을 수정할 수 있습니다 사용자에 게 것입니다.

다음은 코드에서이 프로세스를 어떻게 처리할 수 있는지의 예입니다.

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

APNs에서 알림을 받으면 내용에서 이미지의 사용자 지정 주소는 읽기 하 고 파일 서버에서 다운로드 됩니다. `UNNotificationAttachement` 의 고유 ID와 이미지의 로컬 위치를 사용 하 여 만들어집니다 (으로 `NSUrl`). 알림 콘텐츠의 변경 가능한 복사본을 만들고 미디어 첨부 파일을 추가 합니다. 호출 하 여 사용자에 게 알림이 표시 되는 마지막으로,는 `contentHandler`합니다.

첨부 파일에는 알림을 추가 되 면 시스템은 이동 및 파일의 관리가 됩니다.

위에서 설명한 원격 알림, 외에도 미디어 첨부 파일 에서도 지원 됩니다 로컬 알림에서 여기서는 `UNNotificationAttachement` 생성 되어 해당 콘텐츠와 함께 알림에 연결 합니다.

IOS 10에서에서 알림은 미디어 첨부 파일의 이미지를 지원 (정적 및 Gif), 오디오 또는 비디오와 시스템 자동으로 표시 됩니다 올바른 사용자 지정 UI 이러한 유형의 첨부 파일의 각 사용자에 게 알림이 제공 될 때입니다.

> [!NOTE]
> 미디어 크기를 최적화 하기 위해 주의 해야 하 고 응용 프로그램의 서비스 확장을 실행 하는 경우 원격 서버에서 미디어를 다운로드 하려면 (또는 로컬 알림에 대 한 미디어를 어셈블하기) 시스템으로 걸리는 시간이 적용 둘 다에 엄격 하 게 제한 합니다. 예를 들어 이미지의 아래쪽 크기 조정 된 버전 또는 알림을에 표시 되는 비디오의 작은 클립 전송 하는 것이 좋습니다.

## <a name="creating-custom-user-interfaces"></a>사용자 지정 사용자 인터페이스 만들기

해당 사용자 알림에 대 한 사용자 지정 사용자 인터페이스를 만들려면 개발자 (iOS 10에 새 항목) 알림 콘텐츠 확장 응용 프로그램의 솔루션에 추가 해야 합니다.

알림 콘텐츠 확장을 사용 하 고 개발자에 게 자신의 뷰 알림 UI를 추가 하 고 원하는 모든 콘텐츠를 그립니다. 그러나 대화형 UI 위젯 (예: 단추 또는 텍스트 필드)는 **하지** 알림 UI에서 지원 되 고 사용자 지정 UI 디자인에 추가할 수 없습니다.

사용자 알림과 함께 사용자 상호 작용을 지원 하려면 사용자 지정 작업 처리를, 시스템에 등록 된 만들고 시스템으로 예약 하기 전에 알림을에 연결 합니다. 알림 콘텐츠 확장 이러한 작업의 처리를 처리 하도록 호출 됩니다. 참조는 [알림 작업을 사용](~/ios/platform/user-notifications/enhanced-user-notifications.md) 섹션은 [향상 된 사용자 알림을](~/ios/platform/user-notifications/enhanced-user-notifications.md) 문서에 대 한 자세한 내용은 사용자 지정 작업입니다.

사용자 지정 UI를 사용한 사용자 알림을 사용자에 게 표시 되 면 다음과 같은 요소가 미치게 될:

[![](advanced-user-notifications-images/customui01.png "사용자 지정 UI 요소와 사용자 알림")](advanced-user-notifications-images/customui01.png#lightbox)

사용자 지정 작업 (알림 아래 제시 된)와 상호 작용할 하는 경우는 발생 하는 결과 지정된 된 동작을 호출할 때로 사용자 피드백을 제공할 사용자 인터페이스를 업데이트할 수 있습니다.

### <a name="adding-a-notification-content-extension"></a>추가 알림 내용 확장

Xamarin.iOS 앱에서 UI는 사용자 지정 사용자 알림을 구현 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac.에 대 한 Visual Studio에서 응용 프로그램의 솔루션을 열으십시오
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭는 **솔루션 패드** 선택 **추가** > **새 프로젝트 추가**합니다.
3. 선택 **iOS** > **확장** > **알림 콘텐츠 확장이** 클릭는 **다음** 단추: 

    [![](advanced-user-notifications-images/notify01.png "알림 콘텐츠 확장 선택")](advanced-user-notifications-images/notify01.png#lightbox)
4. 입력 한 **이름** 확장과 클릭에 대 한는 **다음** 단추: 

    [![](advanced-user-notifications-images/notify02.png "확장에 대 한 이름을 입력 합니다.")](advanced-user-notifications-images/notify02.png#lightbox)
5. 조정 된 **프로젝트 이름** 및/또는 **솔루션 이름** 필요 하 고 클릭는 **만들기** 단추: 

    [![](advanced-user-notifications-images/notify03.png "프로젝트 이름 및/또는 솔루션 이름 조정")](advanced-user-notifications-images/notify03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Mac.에 대 한 Visual Studio에서 응용 프로그램의 솔루션을 열으십시오
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭는 **솔루션 탐색기** 선택 **추가 > 새 프로젝트...** .
3. 선택 **Visual C# > 확장 iOS > 알림 콘텐츠 확장**:

    [![](advanced-user-notifications-images/notify01.w157-sml.png "알림 콘텐츠 확장 선택")](advanced-user-notifications-images/notify01.w157.png#lightbox)
4. 입력 한 **이름** 확장과 클릭에 대 한는 **확인** 단추입니다.

-----

알림 콘텐츠 확장 솔루션에 추가 되 면 3 개의 파일 확장명의 프로젝트에서 만들어집니다.

1. `NotificationViewController.cs` -이 알림 내용 확장에 대 한 주 보기 컨트롤러입니다.
2. `MainInterface.storyboard` -여기서 개발자 레이아웃 표시 되는 UI iOS 디자이너에에서 알림 콘텐츠 확장 합니다.
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

`DidReceiveNotification` 메서드는 알림을 알림 내용 확장 내용으로 사용자 지정 UI를 채울 수 있도록 사용자가 확장 되는 `UNNotification`합니다. 위의 예제에 대 한 레이블을 추가한 이름의 코드에 노출 하는 보기와 `label` 는 알림 메시지의 본문을 표시 하는 데 사용 됩니다.

### <a name="setting-the-notification-content-extensions-categories"></a>알림 콘텐츠 확장 범주를 설정합니다.

시스템에 응답 하 고 특정 범주에 따라 응용 프로그램의 알림 내용 확장을 찾는 방법에 대 한 정보가 제공 해야 합니다. 다음을 수행합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 확장의 두 번 클릭 `Info.plist` 파일에 **솔루션 패드** 를 편집 하기 위해 엽니다.
2. 전환 하는 **소스** 보기.
3. 확장 된 `NSExtension` 키입니다.
4. 추가 `UNNotificationExtensionCategory` 형식으로 키 **문자열** 확장에 속한 범주 값 (이 예제의 ' 이벤트 초대): 

    [![](advanced-user-notifications-images/customui02.png "UNNotificationExtensionCategory 키 추가")](advanced-user-notifications-images/customui02.png#lightbox)
5. 변경 내용을 저장합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 확장의 두 번 클릭 `Info.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
3. 확장 된 `NSExtension` 키입니다.
4. 추가 `UNNotificationExtensionCategory` 형식으로 키 **문자열** 확장에 속한 범주 값 (이 예제의 ' 이벤트 초대): 

    [![](advanced-user-notifications-images/customui02w.png "UNNotificationExtensionCategory 키 추가")](advanced-user-notifications-images/customui02w.png#lightbox)
5. 변경 내용을 저장합니다.

-----

알림 콘텐츠 확장 범주 (`UNNotificationExtensionCategory`) 알림 작업을 등록 하는 데 사용 되는 동일한 범주 값을 사용 합니다. 전환 하는 경우 앱 여러 범주에 대 한 동일한 UI를 사용 합니다는 `UNNotificationExtensionCategory` 유형에 **배열** 모든 필요한 범주를 제공 합니다. 예를 들어:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](advanced-user-notifications-images/customui03.png "알림 콘텐츠 확장 범주")](advanced-user-notifications-images/customui03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui03w.png "알림 콘텐츠 확장 범주")](advanced-user-notifications-images/customui03w.png#lightbox)

-----

### <a name="hiding-the-default-notification-content"></a>기본 알림 콘텐츠 숨기기

여기서 사용자 지정 알림 UI을 나타내려면 동일한 콘텐츠를 기본 알림 (제목, 부제목 및 알림 UI의 맨 아래에 자동으로 표시 하는 본문) 상황에서이 기본 정보를 숨길 수 있습니다는 를추가하여`UNNotificationExtensionDefaultContentHidden`키를 `NSExtensionAttributes` 형식으로 키 **부울** 값 `YES` 는 확장에서 `Info.plist` 파일:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](advanced-user-notifications-images/customui04.png "기본 정보 찾기")](advanced-user-notifications-images/customui04.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui04w.png "기본 정보 찾기")](advanced-user-notifications-images/customui04w.png#lightbox)

-----

### <a name="designing-the-custom-ui"></a>사용자 지정 UI 디자인

알림 콘텐츠 확장의 사용자 지정 사용자 인터페이스를 디자인 하려면 두 번 클릭는 `MainInterface.storyboard` iOS 디자이너에서에서 편집을 위해 열 파일을 끌어 원하는 인터페이스를 빌드하는 데 필요한 요소 (같은 `UILabels` 및 `UIImageViews`).

> [!NOTE]
> 알림 UI는 _하지_ 알림 콘텐츠 확장에서 텍스트 필드 또는 단추와 같은 대화형 컨트롤을 지원 합니다. 스토리 보드에 추가할 수 있습니다, 하는 동안 사용자 상호 작용할 수 없습니다. 사용자 상호 작용에는 사용자 지정 알림 UI를 추가 하려면 사용자 지정 작업을 대신 사용 합니다.

UI에 배치 된 하 고 C# 코드에 노출 하는 데 필요한 컨트롤을 열고는 `NotificationViewController.cs` 편집을 위해 및 수정는 `DidReceiveNotification` 메서드 알림을 확장할 때 UI를 채울 수 있습니다. 예를 들어:

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

사용자에 게 표시 하는 콘텐츠 영역의 크기를 조정 하려면 아래 코드 설정 하 고는 `PreferredContentSize` 속성에는 `ViewDidLoad` 메서드를 원하는 크기로 만듭니다. 보기에는 iOS 디자이너에 제약 조건을 적용 하 여이 크기를 조정할 수 또한, 가장 잘 작동 하는 메서드를 선택 하도록 개발자에 게 유지 됩니다.

콘텐츠 확장이 호출 알림 콘텐츠 영역 전 시스템 이미 실행 되 고 알림 시작 되기 때문에 전체 크기 조정 및 사용자에 게 표시 하는 경우 요청 된 크기로 애니메이션을 적용할 수 있습니다.

이 효과 제거 하려면 편집는 `Info.plist` 확장을 설정 하는 파일은 `UNNotificationExtensionInitialContentSizeRatio` 의 키는 `NSExtensionAttributes` 키를 입력 **번호** 원하는 비율을 나타내는 값을 사용 합니다. 예를 들어:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](advanced-user-notifications-images/customui05.png "UNNotificationExtensionInitialContentSizeRatio 키")](advanced-user-notifications-images/customui05.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui05w.png "UNNotificationExtensionInitialContentSizeRatio 키")](advanced-user-notifications-images/customui05w.png#lightbox)

-----

### <a name="using-media-attachments-in-custom-ui"></a>미디어 첨부 파일을 사용 하 여 사용자 지정 UI

때문에 미디어 첨부 파일 (에 표시 되는 [미디어 첨부 파일 추가](#Adding-Media-Attachments) 위의 섹션) 알림 페이로드의 일부인, 액세스 및 기본에 예상 되는 것 처럼 알림 내용 확장에 표시 된 수 UI 알림입니다.

예를 들어, 위의 사용자 지정 UI를 포함 하는 경우는 `UIImageView` C# 코드에 노출 된 하는, 다음 코드는 미디어 첨부 파일과 함께 채우는 데 사용할 수 없습니다.

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

되기 때문에 미디어 첨부 파일 시스템에 의해 관리 되는 응용 프로그램의 샌드박스 외부에서. 확장 파일에 대 한 액세스를 호출 하 여가 시스템에 알리기 위해 필요한는 `StartAccessingSecurityScopedResource` 메서드. 호출 해야 하는 확장 된 파일 작업을 마치면는 `StopAccessingSecurityScopedResource` 의 연결을 해제 합니다.

### <a name="adding-custom-actions-to-a-custom-ui"></a>사용자 지정 UI 사용자 지정 작업 추가

위에서 설명 했 듯이 텍스트 필드 또는 단추와 같은 컨트롤의 UI 디자인에서 지원 되지 않는 하므로 알림 UI 사용자 상호 작용을 지원 하지 않습니다.

대신, 사용자 지정 알림 UI에 대화형 작업 추가를 표준 사용자 지정 작업 메커니즘 사용 됩니다. 참조는 [알림 작업을 사용](~/ios/platform/user-notifications/enhanced-user-notifications.md) 섹션은 [향상 된 사용자 알림을](~/ios/platform/user-notifications/enhanced-user-notifications.md) 문서에 대 한 자세한 내용은 사용자 지정 작업입니다.

사용자 지정 동작 외에도 알림 콘텐츠 확장은 기본 제공 작업에도 응답할 수 있습니다.

- **기본 동작이** -사용자가 앱을 열고 지정 된 알림 세부 정보를 표시에 대 한 알림을 누르면입니다.
- **동작을 해제** -사용자 지정된 알림을 해제할 때이 작업 응용 프로그램에 전송 됩니다.

알림 콘텐츠 확장이 레이블에도 사용자는 사용자 지정 작업 중 하나를 호출 하는 경우의 UI를 업데이트 하는 기능, 사용자가 수락 하는 경우 날짜를 표시 하는 등의 **Accept** 사용자 지정 작업 단추입니다. 또한 알림 콘텐츠 확장이 사용자 알림을 닫기 전에 해당 작업의 효과 볼 수 있도록 알림 UI 기를 지연 하도록 시스템을 알 수 있습니다.

두 번째 버전을 구현 하 여 이렇게는 `DidReceiveNotification` 완료 처리기를 포함 하는 메서드입니다. 예를 들어:

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

추가 하 여는 `Server.PostEventResponse` 처리기에는 `DidReceiveNotification` 알림 콘텐츠 확장 확장의 메서드 *해야* 모든 사용자 지정 작업을 처리 합니다. 변경 하 여 포함 된 응용 프로그램에 로그온 하는 사용자 지정 동작 확장에 전달할 수도 있습니다는 `UNNotificationContentExtensionResponseOption`합니다. 예를 들어:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>사용자 지정 UI 텍스트 입력된 작업 사용

응용 프로그램의 성능과 알림을 디자인에 따라 사용자를 알림 (예: 메시지에 회신)에 텍스트를 입력 해야 하는 횟수가 있을 수 있습니다. 알림 콘텐츠 확장 표준 알림 않습니다 것 처럼 기본 제공 텍스트 입력된 작업에 대 한 액세스를에 있습니다.

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

이 코드를 새 텍스트 입력된 작업을 만들고 확장의 범주에 추가 (에 `MakeExtensionCategory`) 메서드. 에 `DidReceive` 메서드를 재정의 다음 코드로 텍스트를 입력 하는 사용자 처리:

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

텍스트 입력 필드에 사용자 지정 단추를 추가 하는 것에 대 한 디자인 호출을 하는 경우 데이터를 포함 다음 코드를 추가 합니다.

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

주석 작업, 트리거될 때 사용자가을 활성화할 수 보기 컨트롤러 컴퓨터와 사용자 지정 텍스트 입력된 필드 모두 필요 합니다.

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

<a name="Summary" />

## <a name="summary"></a>요약

이 문서는 Xamarin.iOS 앱의 새로운 사용자 알림 프레임 워크를 사용 하는 고급 확인을 수행 했습니다. 로컬 및 원격 알림 모두에 미디어 첨부 파일 추가 설명 것 있으며 새 사용자 알림 UI를 사용 하 여 사용자 지정 알림 Ui를 만들 대상 것 있습니다.


## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications 프레임 워크 참조](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [로컬 및 원격 알림 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
