---
title: Xamarin.iOS에서 소셜 프레임 워크
description: 소셜 프레임 워크 SinaWeibo 뿐만 아니라 Twitter 및 Facebook을 포함 하 여 중국의 사용자에 대 한 소셜 네트워크와 상호 작용 하기 위한 통합된 API를 제공 합니다.
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 08ccd5b5ac78e82bf745764d70e59d2db9ec6776
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115707"
---
# <a name="social-framework-in-xamarinios"></a>Xamarin.iOS에서 소셜 프레임 워크

_소셜 프레임 워크 SinaWeibo 뿐만 아니라 Twitter 및 Facebook을 포함 하 여 중국의 사용자에 대 한 소셜 네트워크와 상호 작용 하기 위한 통합된 API를 제공 합니다._

소셜 프레임 워크를 사용 하면 응용 프로그램을 인증을 관리할 필요 없이 단일 API에서 소셜 네트워크와 상호 작용할 수 있습니다. HTTP를 통해 각 소셜 네트워크의 API 사용을 허용 하는 추상화 뿐만 아니라 게시물을 작성 하는 것에 대 한 뷰 컨트롤러를 제공 하는 시스템을 포함 합니다.

> [!IMPORTANT]
> 다양 한 소셜 네트워크에 연결 하는 플랫폼 간 API에 대 한 참조를 [Xamarin.Social](http://components.xamarin.com/view/xamarin.social/) Xamarin Component Store에서 구성 요소입니다.

## <a name="connecting-to-twitter"></a>Twitter에 연결

### <a name="twitter-account-settings"></a>Twitter 계정 설정

소셜 프레임 워크를 사용 하 여 Twitter에 연결 하려면 계정 아래와 같이 장치 설정에서 구성 해야 합니다.

 [![](social-framework-images/twitter01.png "Twitter 계정 설정")](social-framework-images/twitter01.png#lightbox)

계정에 입력 하 고 Twitter를 사용 하 여 확인할 소셜 프레임 워크 클래스를 사용 하 여 Twitter에 액세스 하는 장치에서 모든 응용 프로그램에는이 계정을 사용 합니다.

### <a name="sending-tweets"></a>트 윗 보내기

소셜 프레임 워크 라는 컨트롤러가 `SLComposeViewController` 는 트 윗을 보내고 편집에 대 한 제공 하는 시스템 뷰를 제공 합니다. 다음 스크린샷은이 보기의 예를 보여 줍니다.

 [![](social-framework-images/twitter02.png "이 스크린샷에서 SLComposeViewController의 예를 보여 줍니다.")](social-framework-images/twitter02.png#lightbox)

사용 하는 `SLComposeViewController` Twitter를 사용 하 여 컨트롤러의 인스턴스를 호출 하 여 생성 해야 합니다 `FromService` 메서드를 `SLServiceType.Twitter` 아래와 같이:

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

이후에 `SLComposeViewController` 인스턴스가 반환 됩니다, Twitter에 게시 하는 UI를 표시 하도록 사용할 수 있습니다. 그러나 가장 먼저 할 일을 호출 하 여이 예제의 경우 Twitter 소셜 네트워크의 가용성을 확인 하는 `IsAvailable`:

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController` 사용자 개입 없이 직접 트 윗을 전송 하지 않습니다. 그러나 다음 방법을 사용 하 여 초기화할 수 있습니다.

-   `SetInitialText` -트 윗에 표시할 초기 텍스트를 추가 합니다. 
-  `AddUrl` -트 윗에 Url을 추가합니다.
-  `AddImage` -트 윗에 이미지를 추가합니다.


초기화 되 면 호출 `PresentVIewController` 에서 만든 보기를 표시 합니다 `SLComposeViewController`합니다. 사용자 수 다음 필요에 따라 편집 및 트 윗을 보내거나 보내기 취소 합니다. 두 경우 모두에서 컨트롤러를 해제 합니다 `CompletionHandler`아래와 같이 트 윗 전송 되었거나 취소 하는 경우를 확인 하려면 결과 또한 확인 수는:

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>트 윗 예제

다음 코드를 사용 하는 방법을 보여 줍니다는 `SLComposeViewController` 트 윗을 보내는 데 사용 하는 뷰를 제공 합니다.

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

### <a name="calling-twitter-api"></a>Twitter API를 호출합니다.

소셜 프레임 워크에는 소셜 네트워크에 대 한 HTTP 요청에 대 한 지원이 포함 됩니다. 요청을 캡슐화 하는 `SLRequest` 특정 소셜 네트워크의 API를 대상으로 사용 되는 클래스입니다.

예를 들어, 다음 코드에서 공용 타임 라인 (위의 코드에서 확장)에서 가져오려는 Twitter에 요청 합니다.

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

이 코드를 자세하게 살펴보겠습니다. 먼저 계정 저장소에 액세스할 수 하 고 Twitter 계정 유형을 가져옵니다.

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

다음으로,이 경우 앱에 Twitter 계정에 액세스할 수와 계정 업데이트 UI 및 메모리에 로드 됩니다 액세스가 허용 된 경우 사용자 요청:

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

사용자 타임 라인 데이터 (UI의 단추를 눌러)를 요청 하면 앱에서 Twitter 데이터에 액세스 하는 요청을 먼저 forms:

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```
이 예제에서는 최근 열 개의 항목에 반환된 된 결과 포함 하 여 제한 `?count=10` url에서입니다. 마지막으로, 요청 (위의 로드)는 Twitter 계정에 연결 하 고 데이터를 가져오기 위한 Twitter에 대 한 호출을 수행 합니다.

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

데이터를 성공적으로 로드 하는 경우 원시 JSON 데이터 (예: 아래 예제에서는 출력) 표시 됩니다.

[![](social-framework-images/twitter03.png "원시 JSON 데이터 표시의 예")](social-framework-images/twitter03.png#lightbox)

실제 앱에서는 JSON 결과 다음 구문 분석 될 수 일반 및 사용자에 게 표시 되는 결과입니다. 참조 [웹 서비스 소개](~/cross-platform/data-cloud/web-services/index.md) JSON 구문 분석 하는 방법에 대 한 정보에 대 한 합니다.

## <a name="connecting-to-facebook"></a>Facebook에 연결

### <a name="facebook-account-settings"></a>Facebook 계정 설정

소셜 프레임 워크를 사용 하 여 Facebook에 연결 하는 것은 위에 나와 있는 Twitter에 사용 되는 프로세스에 거의 동일 합니다. Facebook 사용자 계정 아래와 같이 장치 설정에서 구성 해야 합니다.

[![](social-framework-images/facebook01.png "Facebook 계정 설정")](social-framework-images/facebook01.png#lightbox)

일단 구성 되 면 소셜 프레임 워크를 사용 하는 장치의 모든 응용 프로그램은 Facebook에 연결 하려면이 계정을 사용 합니다.

### <a name="posting-to-facebook"></a>Facebook에 게시

소셜 프레임 워크가 여러 소셜 네트워크에 액세스 하도록 하는 통합된 API 인 코드 유지 되는 소셜 네트워크에 관계 없이 거의 동일 합니다.

예를 들어를 `SLComposeViewController` 만 다른 모드로 전환 하는 Facebook 관련 설정과 옵션을 이전에 표시 된 Twitter 예제와 같이 정확 하 게 사용할 수 있습니다. 예를 들어:

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

Facebook을 사용 하는 경우는 `SLComposeViewController` 표시를 보여 주는 Twitter 예제와 거의 동일한 보이는 **Facebook** 이 경우 제목으로:

[![](social-framework-images/facebook02.png "SLComposeViewController 표시")](social-framework-images/facebook02.png#lightbox)

### <a name="calling-facebook-graph-api"></a>Facebook Graph API 호출

예제와 비슷한 Twitter, 소셜 프레임 워크의 `SLRequest` Facebook의 graph API 사용 하 여 개체를 사용할 수 있습니다. 예를 들어, 다음 코드 (위의 코드에서 확장)에서 Xamarin 계정에 대 한 graph API에서에서 정보를 반환 합니다.

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

이 코드 위에 표시 된 Twitter 버전 간의 유일한 차이점은 개발자/앱 특정 id (Facebook의 개발자 포털에서 생성할 수 있습니다)는 요청을 수행할 때 옵션으로 설정 해야 하는 Facebook의 요구 사항:

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

이 옵션 (또는 잘못 된 키를 사용 하 여)을 설정 하지 못했습니다 오류 또는 반환할 데이터가 발생 합니다.

## <a name="summary"></a>요약

이 문서에서는 Facebook 및 Twitter와 상호 작용 하는 소셜 프레임 워크를 사용 하는 방법을 보여 주었습니다. 장치 설정에서 각 소셜 네트워크에 대 한 계정을 구성 하는 위치에 알아보았습니다. 사용 하는 방법을 설명 합니다 `SLComposeViewController` 소셜 네트워크에 게시 하기 위한 통합 된 뷰를 제공 합니다. 또한 조사 하 고는 `SLRequest` 각 소셜 네트워크의 API를 호출 하는 데 사용 되는 클래스입니다.


## <a name="related-links"></a>관련 링크

- [SocialFrameworkDemo (샘플)](https://developer.xamarin.com/samples/SocialFrameworkDemo/)
- [웹 서비스 소개](~/cross-platform/data-cloud/web-services/index.md)
