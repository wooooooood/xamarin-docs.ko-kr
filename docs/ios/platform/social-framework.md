---
title: Xamarin.ios의 소셜 프레임 워크
description: 소셜 프레임 워크는 Twitter 및 Facebook을 비롯 한 소셜 네트워크와의 상호 작용을 위한 통합 API 뿐만 아니라 중국의 사용자를 위한 SinaWeibo을 제공 합니다.
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/20/2017
ms.openlocfilehash: fd94cd7a6d37e7fa00489e788f232842b319e5d3
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292663"
---
# <a name="social-framework-in-xamarinios"></a>Xamarin.ios의 소셜 프레임 워크

_소셜 프레임 워크는 Twitter 및 Facebook을 비롯 한 소셜 네트워크와의 상호 작용을 위한 통합 API 뿐만 아니라 중국의 사용자를 위한 SinaWeibo을 제공 합니다._

소셜 프레임 워크를 사용 하면 응용 프로그램에서 인증을 관리할 필요 없이 단일 API의 소셜 네트워크와 상호 작용할 수 있습니다. HTTP를 통해 각 소셜 네트워크의 API를 사용할 수 있도록 하는 추상화 뿐만 아니라 게시물을 작성 하기 위한 시스템 제공 보기 컨트롤러를 포함 합니다.

> [!IMPORTANT]
> 플랫폼 간 API를 통해 다양 한 소셜 네트워크에 연결 하려면 Xamarin 구성 요소 저장소의 [xamarin](http://components.xamarin.com/view/xamarin.social/) 구성 요소를 참조 하세요.

## <a name="connecting-to-twitter"></a>Twitter에 연결

### <a name="twitter-account-settings"></a>Twitter 계정 설정

소셜 프레임 워크를 사용 하 여 Twitter에 연결 하려면 아래와 같이 장치 설정에서 계정을 구성 해야 합니다.

 [![](social-framework-images/twitter01.png "Twitter 계정 설정")](social-framework-images/twitter01.png#lightbox)

Twitter를 사용 하 여 계정을 입력 하 고 확인 한 후에는 소셜 프레임 워크 클래스를 사용 하 여 Twitter에 액세스 하는 장치의 모든 응용 프로그램에서이 계정을 사용 합니다.

### <a name="sending-tweets"></a>트 윗 전송

소셜 프레임 워크에는 트 윗을 `SLComposeViewController` 편집 하 고 보내는 시스템 제공 뷰를 제공 하는 라는 컨트롤러가 포함 되어 있습니다. 다음 스크린샷은이 뷰의 예를 보여 줍니다.

 [![](social-framework-images/twitter02.png "이 스크린샷에서는 SLComposeViewController의 예를 보여 줍니다.")](social-framework-images/twitter02.png#lightbox)

Twitter에서를 `SLComposeViewController` 사용 하려면 아래와 같이를 사용 `SLServiceType.Twitter` 하 여 메서드를 `FromService` 호출 하 여 컨트롤러의 인스턴스를 만들어야 합니다.

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

`SLComposeViewController` 인스턴스가 반환 된 후에는 Twitter에 게시할 UI를 제공 하는 데 사용할 수 있습니다. 그러나이 경우에는 다음을 호출 `IsAvailable`하 여 소셜 네트워크 (이 경우 Twitter)의 가용성을 확인 해야 합니다.

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController`사용자 개입 없이 직접 트 윗를 전송 하지 않습니다. 그러나 다음 방법을 사용 하 여 초기화할 수 있습니다.

- `SetInitialText`– 트 윗에서 표시할 초기 텍스트를 추가 합니다.
- `AddUrl`– 트 윗에 Url을 추가 합니다.
- `AddImage`– 이미지를 트 윗에 추가 합니다.


초기화 되 면를 `PresentVIewController` 호출 하면에서 만든 `SLComposeViewController`뷰가 표시 됩니다. 사용자는 필요에 따라 트 윗를 편집 및 전송 하거나 송신을 취소할 수 있습니다. 두 경우 모두에서 컨트롤러를 해제 해야 `CompletionHandler`합니다. 그런 다음 아래와 같이 트 윗를 보내거나 취소 했는지 여부를 확인 하기 위해 결과를 확인할 수도 있습니다.

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>트 윗 예제

다음 코드에서는를 사용 `SLComposeViewController` 하 여 트 윗을 보내는 데 사용 되는 뷰를 표시 하는 방법을 보여 줍니다.

```csharp
using System;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _twitterComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
        #endregion

        #region Computed Properties
        public bool isTwitterAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Twitter); }
        }

        public SLComposeViewController TwitterComposer {
            get { return _twitterComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {

        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            SendTweet.Enabled = isTwitterAvailable;
        }
        #endregion

        #region Actions
        partial void SendTweet_TouchUpInside (UIButton sender)
        {
            // Set initial message
            TwitterComposer.SetInitialText ("Hello Twitter!");
            TwitterComposer.AddImage (UIImage.FromFile ("Icon.png"));
            TwitterComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (TwitterComposer, true, null);
        }
        #endregion
    }
}
```

### <a name="calling-twitter-api"></a>Twitter API 호출

소셜 프레임 워크에는 소셜 네트워크에 대 한 HTTP 요청 만들기 지원도 포함 됩니다. 특정 소셜 네트워크의 API를 `SLRequest` 대상으로 하는 데 사용 되는 클래스의 요청을 캡슐화 합니다.

예를 들어 다음 코드는 위에 지정 된 코드를 확장 하 여 공용 타임 라인을 가져오기 위해 Twitter에 요청을 만듭니다.

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _twitterAccount;
#endregion

#region Computed Properties
public ACAccount TwitterAccount {
    get { return _twitterAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    SendTweet.Enabled = isTwitterAvailable;
    RequestTwitterTimeline.Enabled = false;

    // Initialize Twitter Account access
    var accountStore = new ACAccountStore ();
    var accountType = accountStore.FindAccountType (ACAccountType.Twitter);

    // Request access to Twitter account
    accountStore.RequestAccess (accountType, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestTwitterTimeline.Enabled = true;
            });
        }
    });
}
#endregion

#region Actions
partial void RequestTwitterTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
    var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = TwitterAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

이 코드를 자세히 살펴보겠습니다. 먼저 계정 저장소에 대 한 액세스 권한을 획득 하 고 Twitter 계정의 유형을 가져옵니다.

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

그런 다음 앱에서 Twitter 계정에 액세스할 수 있는지 여부를 묻는 메시지를 표시 하 고 액세스 권한이 부여 되 면 계정이 메모리로 로드 되 고 UI가 업데이트 됩니다.

```csharp
// Request access to Twitter account
accountStore.RequestAccess (accountType, (granted, error) => {
    // Allowed by user?
    if (granted) {
        // Get account
        _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
        InvokeOnMainThread (() => {
            // Update UI
            RequestTwitterTimeline.Enabled = true;
        });
    }
});
```

사용자가 UI의 단추를 탭 하 여 타임 라인 데이터를 요청 하면 앱은 먼저 Twitter에서 데이터에 액세스 하는 요청을 형성 합니다.

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```

이 예제에서는 반환 된 결과를 URL에를 포함 `?count=10` 하 여 마지막 10 개 항목으로 제한 합니다. 마지막으로,이 요청을 Twitter 계정 (위에서 로드 됨)에 연결 하 고 Twitter에 대 한 호출을 수행 하 여 데이터를 인출 합니다.

```csharp
// Request data
request.Account = TwitterAccount;
request.PerformRequest ((data, response, error) => {
    // Was there an error?
    if (error == null) {
        // Was the request successful?
        if (response.StatusCode == 200) {
            // Yes, display it
            InvokeOnMainThread (() => {
                Results.Text = data.ToString ();
            });
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", response.StatusCode);
            });
        }
    } else {
        // No, display error
        InvokeOnMainThread (() => {
            Results.Text = string.Format ("Error: {0}", error);
        });
    }
});
```

데이터가 성공적으로 로드 되 면 원시 JSON 데이터가 표시 됩니다 (아래 예제 출력과 같이).

[![](social-framework-images/twitter03.png "원시 JSON 데이터 표시의 예")](social-framework-images/twitter03.png#lightbox)

실제 앱에서는 JSON 결과를 정상적으로 구문 분석 하 고 결과를 사용자에 게 제공할 수 있습니다. JSON을 구문 분석 하는 방법에 대 한 자세한 내용은 [소개 웹 서비스](~/cross-platform/data-cloud/web-services/index.md) 를 참조 하세요.

## <a name="connecting-to-facebook"></a>Facebook에 연결

### <a name="facebook-account-settings"></a>Facebook 계정 설정

소셜 프레임 워크를 사용 하 여 Facebook에 연결 하는 것은 위에 표시 된 Twitter에 사용 되는 프로세스와 거의 동일 합니다. 아래와 같이 장치 설정에서 Facebook 사용자 계정을 구성 해야 합니다.

[![](social-framework-images/facebook01.png "Facebook 계정 설정")](social-framework-images/facebook01.png#lightbox)

구성 된 후 소셜 프레임 워크를 사용 하는 장치의 모든 응용 프로그램은이 계정을 사용 하 여 Facebook에 연결 합니다.

### <a name="posting-to-facebook"></a>Facebook에 게시

소셜 프레임 워크는 여러 소셜 네트워크에 액세스 하도록 디자인 된 통합 API 이므로 사용 중인 소셜 네트워크에 관계 없이 코드는 거의 동일 하 게 유지 됩니다.

예를 들어 앞 `SLComposeViewController` 에서 설명한 Twitter 예제와 똑같이를 사용할 수 있으며, 다른 유일한 설정은 Facebook 특정 설정 및 옵션으로 전환 하는 것입니다. 예를 들어:

```csharp
using System;
using Foundation;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _facebookComposer = SLComposeViewController.FromService (SLServiceType.Facebook);
        #endregion

        #region Computed Properties
        public bool isFacebookAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Facebook); }
        }

        public SLComposeViewController FacebookComposer {
            get { return _facebookComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {

        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            PostToFacebook.Enabled = isFacebookAvailable;
        }
        #endregion

        #region Actions
        partial void PostToFacebook_TouchUpInside (UIButton sender)
        {
            // Set initial message
            FacebookComposer.SetInitialText ("Hello Facebook!");
            FacebookComposer.AddImage (UIImage.FromFile ("Icon.png"));
            FacebookComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (FacebookComposer, true, null);
        }
        #endregion
    }
}
```

Facebook과 함께 사용 하는 `SLComposeViewController` 경우은 Twitter 예제와 거의 동일 하 게 표시 되는 보기를 표시 합니다 .이 경우에는 **facebook** 을 제목으로 표시 합니다.

[![](social-framework-images/facebook02.png "SLComposeViewController 표시")](social-framework-images/facebook02.png#lightbox)

### <a name="calling-facebook-graph-api"></a>Facebook Graph API 호출

Twitter 예제와 마찬가지로 소셜 프레임 워크의 `SLRequest` 개체는 Facebook의 graph API와 함께 사용할 수 있습니다. 예를 들어 다음 코드는 위에 지정 된 코드를 확장 하 여 Xamarin 계정에 대 한 graph API의 정보를 반환 합니다.

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _facebookAccount;
#endregion

#region Computed Properties
public ACAccount FacebookAccount {
    get { return _facebookAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    PostToFacebook.Enabled = isFacebookAvailable;
    RequestFacebookTimeline.Enabled = false;

    // Initialize Facebook Account access
    var accountStore = new ACAccountStore ();
    var options = new AccountStoreOptions ();
    var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
    accountType = accountStore.FindAccountType (ACAccountType.Facebook);

    // Request access to Facebook account
    accountStore.RequestAccess (accountType, options, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _facebookAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestFacebookTimeline.Enabled = true;
            });
        }
    });

}
#endregion

#region Actions
partial void RequestFacebookTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl ("https://graph.facebook.com/283148898401104");
    var request = SLRequest.Create (SLServiceKind.Facebook, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = FacebookAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

위에서 언급 한이 코드와 Twitter 버전의 유일한 차이점은 요청 시 옵션으로 설정 해야 하는 개발자/앱 관련 ID (Facebook의 개발자 포털에서 생성할 수 있음)를 가져오기 위해 Facebook의 요구 사항입니다.

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

이 옵션을 설정 하지 않거나 잘못 된 키를 사용 하면 오류가 발생 하거나 반환 되는 데이터가 반환 되지 않습니다.

## <a name="summary"></a>요약

이 문서에서는 소셜 프레임 워크를 사용 하 여 Twitter 및 Facebook과 상호 작용 하는 방법을 살펴보았습니다. 장치 설정에서 각 소셜 네트워크에 대 한 계정을 구성 하는 위치를 살펴보았습니다. 또한를 `SLComposeViewController` 사용 하 여 소셜 네트워크에 게시 하기 위한 통합 된 보기를 제공 하는 방법에 대해서도 설명 합니다. 또한 각 소셜 네트워크의 `SLRequest` API를 호출 하는 데 사용 되는 클래스를 검사 했습니다.


## <a name="related-links"></a>관련 링크

- [SocialFrameworkDemo (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/socialframeworkdemo)
- [웹 서비스 소개](~/cross-platform/data-cloud/web-services/index.md)
