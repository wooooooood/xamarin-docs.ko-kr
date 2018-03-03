---
title: "소셜 프레임 워크"
description: "소셜 프레임 워크 SinaWeibo 뿐만 아니라 Twitter 및 Facebook, 중국에서 사용자에 대 한 포함 한 소셜 네트워크와 상호 작용 하기 위한 통합된 API를 제공 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: da096c8575896bc9f522a92b3fb94b81f9e772df
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="social-framework"></a>소셜 프레임 워크

_소셜 프레임 워크 SinaWeibo 뿐만 아니라 Twitter 및 Facebook, 중국에서 사용자에 대 한 포함 한 소셜 네트워크와 상호 작용 하기 위한 통합된 API를 제공 합니다._


소셜 프레임 워크를 사용 하 여 응용 프로그램을을 인증을 관리 하지 않고도 단일 API에서 소셜 네트워크와 상호 작용할 수 있습니다. HTTP를 통해 각 소셜 네트워크 API를 사용한 허용 하는 추상적 표현 뿐만 아니라 게시물을 작성 하기 위한 뷰-컨트롤러를 제공 하는 시스템을 포함 합니다.

> [!IMPORTANT]
> **참고:** 다양 한 소셜 네트워크에 연결 하는 플랫폼 간 API에 대 한 참조는 [Xamarin.Social](http://components.xamarin.com/view/xamarin.social/) Xamarin 구성 요소 저장소에서 구성 요소입니다.

## <a name="connecting-to-twitter"></a>Twitter에 연결

### <a name="twitter-account-settings"></a>Twitter 계정 설정

소셜 프레임 워크를 사용 하 여 Twitter에 연결할 계정의을 아래와 같이 장치 설정에서 구성할 수 있어야 합니다.

 [ ![](social-framework-images/twitter01.png "Twitter 계정 설정")](social-framework-images/twitter01.png)

계정을 입력 하 고 Twitter를 사용 하 여 확인 된 모든 응용 프로그램에서 소셜 프레임 워크 클래스를 사용 하 여 Twitter를 액세스 하는 장치에이 계정을 사용 합니다.

### <a name="sending-tweets"></a>트 윗을 보낼

소셜 프레임 워크 라는 컨트롤러가 포함 `SLComposeViewController` 를 편집 하 고 여 트 윗을 보낼 제공 하는 시스템 뷰를 제공 하는 합니다. 다음 스크린샷은이 보기의 예를 보여 줍니다.

 [ ![](social-framework-images/twitter02.png "이 스크린샷에서 SLComposeViewController 예를 보여 줍니다.")](social-framework-images/twitter02.png)

사용 하는 `SLComposeViewController` Twitter와 컨트롤러의 인스턴스가 호출 하 여 생성 해야 합니다는 `FromService` 메서드 `SLServiceType.Twitter` 아래와 같이:

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

이후에 `SLComposeViewController` 인스턴스가 반환 됩니다, Twitter에 게시 하도록 UI를 표시 하도록 사용할 수 있습니다. 그러나 가장 먼저 할 일을 호출 하 여이 경우 Twitter 소셜 네트워크의 가용성을 확인 하는 `IsAvailable`:

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController` 사용자 상호 작용 하지 않고 직접 여 트 윗을 보내지 않습니다. 그러나 다음 방법을 사용 하 여 초기화할 수 있습니다.

-   `SetInitialText` –는 트 윗에 표시할 초기 텍스트를 추가 합니다. 
-  `AddUrl` –는 트 윗에 Url을 추가합니다.
-  `AddImage` –는 트 윗에 이미지를 추가합니다.


초기화 된 호출 `PresentVIewController` 로 생성 된 뷰에 표시 됩니다는 `SLComposeViewController`합니다. 그런 다음 필요에 따라 편집 및는 트 윗을 보낼 하거나 수 보내는 취소 합니다. 두 경우 모두에서 컨트롤러를 해제는 `CompletionHandler`여기서 결과 수 또한 여부를 확인 하기는 트 윗 전송 또는 취소 하는 경우 아래와 같이,:

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>트 윗 예제

다음 코드에서는 사용 하 여는 `SLComposeViewController` 는 트 윗을 보낼 사용 되는 뷰를 표시 하도록:

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

소셜 프레임 워크에는 소셜 네트워크에 대 한 HTTP 요청에 대 한 지원이 포함 되어 있습니다. 요청에 캡슐화 되는 `SLRequest` 특정 소셜 네트워크의 API를 대상으로 사용 되는 클래스입니다.

예를 들어 다음 코드 (나와 있는 코드에서 확장)에서 공용 타임 라인을 얻으려고 Twitter 요청 하면:

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

이 코드를 자세히 살펴보겠습니다. 먼저, 계정 저장소에 액세스 하 고 Twitter 계정 유형을 가져옵니다.

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

다음으로, 것 사용자 인지 묻고 응용 프로그램 들이 Twitter 계정에 액세스할 수 있습니다, 계정 UI를 업데이트 하 고 메모리에 로드 됩니다 액세스가 허용 되 면:

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

사용자가 일정 데이터 (단추를 눌러 UI에서)를 요청 하는 경우 응용 프로그램은 먼저 Twitter에서 데이터에 액세스 하는 요청 forms:

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```
이 예에서는 제한 하는 반환 된 결과 마지막 10 개의 항목에 포함 하 여 `?count=10` url에서입니다. 마지막으로, 요청 Twitter 계정 (위에서 로드)에 연결 하 고 데이터를 인출할 수 Twitter에 대 한 호출을 수행 합니다.

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

데이터를 로드 했습니다 경우 원시 JSON 데이터 (아래 예제의 출력)에서 같이 표시 됩니다.

[ ![](social-framework-images/twitter03.png "원시 JSON 데이터 표시의 예")](social-framework-images/twitter03.png)

실제 앱에서 JSON 결과 다음 구문 분석 될 수 표준 및 사용자에 게 결과입니다. 참조 [소개 웹 서비스](~/cross-platform/data-cloud/web-services/index.md) JSON을 구문 분석 하는 방법에 대 한 내용은 합니다.

## <a name="connecting-to-facebook"></a>Facebook에 연결

### <a name="facebook-account-settings"></a>Facebook 계정 설정

Facebook 소셜 프레임 워크에 연결 하는 것은 위에 표시 된 Twitter에 사용 되는 프로세스에 거의 동일 합니다. Facebook 사용자 계정은 아래와 같이 장치 설정에서 구성 해야 합니다.

[ ![](social-framework-images/facebook01.png "Facebook 계정 설정")](social-framework-images/facebook01.png)

구성 되 면 모든 응용 프로그램에서 소셜 프레임 워크를 사용 하는 장치는 Facebook에 연결할이 계정을 사용 합니다.

### <a name="posting-to-facebook"></a>Facebook에 게시

소셜 프레임 워크의 여러 소셜 네트워크에 액세스 하도록 하는 통합된 API 이기 코드를 사용 하 고 해당 소셜 네트워크에 관계 없이 거의 동일 합니다.

예를 들어는 `SLComposeViewController` 만 다른 모드로 전환 하는 facebook 설정 및 옵션을 앞에서 살펴본 Twitter 예제와 같이 정확 하 게 사용할 수 있습니다. 예:

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

Facebook과 함께 사용할 경우의 `SLComposeViewController` 뷰를 표시 하 고 Twitter 예제 거의 똑같은 표시 **Facebook** 이 사례에서 제목으로:

[ ![](social-framework-images/facebook02.png "SLComposeViewController 표시")](social-framework-images/facebook02.png)

### <a name="calling-facebook-graph-api"></a>Facebook Graph API를 호출합니다.

예제와 비슷한 Twitter, 소셜 프레임 워크의 `SLRequest` 개체를 Facebook의 graph API와 함께 사용할 수 있습니다. 예를 들어 다음 코드 (나와 있는 코드에서 확장)에서 Xamarin 계정에 대 한 graph API에서에서 정보를 반환:

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

이 코드와, 위에서 설명한 Twitter 버전 간의 차이점은 ID를 가져오는 개발자/응용 프로그램 특정 (있음 Facebook의 개발자 포털에서 생성할 수 있습니다) 요청을 만드는 경우 옵션으로 설정할 수 있는 Facebook의 요구 사항:

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

오류 또는 반환 되는 데이터가 없는이 옵션 (또는 잘못 된 키 사용)을 설정 하지 못하면 발생 합니다.

## <a name="summary"></a>요약

이 문서는 Facebook 및 Twitter와 상호 작용 하는 소셜 프레임 워크를 사용 하는 방법을 배웠습니다. 장치 설정에서 각 소셜 네트워크에 대 한 계정을 구성 하는 위치에 알아보았습니다. 사용 하는 방법을 설명는 `SLComposeViewController` 소셜 네트워크에 게시 하기 위한 통합된 보기를 제공 합니다. 또한 검사는 `SLRequest` 각 소셜 네트워크 API를 호출 하는 데 사용 되는 클래스입니다.


## <a name="related-links"></a>관련 링크

- [SocialFrameworkDemo (샘플)](https://developer.xamarin.com/samples/SocialFrameworkDemo/)
- [웹 서비스 소개](~/cross-platform/data-cloud/web-services/index.md)
