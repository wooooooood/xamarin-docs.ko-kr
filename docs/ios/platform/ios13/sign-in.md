---
title: Xamarin.ios에서 Apple에 로그인 합니다.
description: Apple에서 로그인은 타사 인증 서비스를 사용할 때 id 보호 기능을 제공 합니다.
ms.prod: xamarin
ms.assetid: CDA59BBF-AAE1-4D4F-B4AE-DB37220D41EB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/10/2019
ms.openlocfilehash: ae4cccc900396c7ebd6e737160e38c5e9dcdc74e
ms.sourcegitcommit: cf56d2bae34dc0f8e94c2d3d28d5f460d59807bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70986167"
---
# <a name="sign-in-with-apple-in-xamarinios"></a>Xamarin.ios에서 Apple에 로그인 합니다.

![이 API는 현재 미리 보기로 제공 됩니다.](~/media/shared/preview.png)

Apple로 로그인은 타사 인증 서비스 사용자에 게 id 보호를 제공 하는 새로운 서비스입니다. IOS 13부터 Apple에서는 타사 인증 서비스를 사용 하는 새 앱 에서도 Apple에 로그인을 제공 해야 합니다. 업데이트 되는 기존 앱은 4 월 2020 일까 지 Apple에 로그인을 추가할 필요가 없습니다.

이 문서에서는 Apple에서 로그인을 iOS 13 응용 프로그램에 추가할 수 있는 방법을 소개 합니다.

## <a name="requirements"></a>요구 사항

이 기능에는 다음이 필요 합니다.
* iOS 13
* Xamarin.ios 12.99
* Xcode 11을 지 원하는 visual Studio 2019 또는 Mac 용 Visual Studio 2019. 
 
자세한 내용은 [시작](get-started.md) 을 참조 하세요.

## <a name="apple-developer-setup"></a>Apple developer 설치

Apple에서 로그인을 사용 하 여 앱을 빌드하고 실행 하기 전에 다음 단계를 완료 해야 합니다. [Apple Developer certificate, 식별자 & 프로필][5] 포털에서 다음을 수행 합니다.

1. 새 **앱 id** 식별자를 만듭니다.
2. **설명 필드에** 설명을 설정 합니다.
3. **명시적** 번들 ID를 선택 하 고 `com.xamarin.AddingTheSignInWithAppleFlowToYourApp` 필드에를 설정 합니다.
4. **Apple 기능으로 로그인** 을 사용 하도록 설정 하 고 새 id를 등록 합니다.
5. 새 Id를 사용 하 여 새 프로 비전 프로필을 만듭니다.
6. 장치에 다운로드 하 여 설치 합니다.
7. Visual Studio에서 **info.plist** 파일의 **Apple 기능으로 로그인** 을 사용 하도록 설정 합니다.

## <a name="check-sign-in-status"></a>로그인 상태 확인

앱이 시작 되거나 사용자의 인증 상태를 처음 확인 해야 하는 경우를 인스턴스화하고 `ASAuthorizationAppleIdProvider` 현재 상태를 확인 합니다.

```csharp
var appleIdProvider = new ASAuthorizationAppleIdProvider ();
appleIdProvider.GetCredentialState (KeychainItem.CurrentUserIdentifier, (credentialState, error) => {
    switch (credentialState) {
    case ASAuthorizationAppleIdProviderCredentialState.Authorized:
        // The Apple ID credential is valid.
        break;
    case ASAuthorizationAppleIdProviderCredentialState.Revoked:
        // The Apple ID credential is revoked.
        break;
    case ASAuthorizationAppleIdProviderCredentialState.NotFound:
        // No credential was found, so show the sign-in UI.
        InvokeOnMainThread (() => {
            var storyboard = UIStoryboard.FromName ("Main", null);

            if (!(storyboard.InstantiateViewController (nameof (LoginViewController)) is LoginViewController viewController))
                return;

            viewController.ModalPresentationStyle = UIModalPresentationStyle.FormSheet;
            viewController.ModalInPresentation = true;
            Window?.RootViewController?.PresentViewController (viewController, true, null);
        });
        break;
    }
});
```

`FinishedLaunching` `LoginViewController` 에서 실행 되는이 코드에서, 상태가 일 `NotFound` 때 앱이 처리 하 고 사용자에 게를 표시 합니다. `AppDelegate.cs` 상태에서 또는 `Revoked`를 반환 `Authorized` 하는 경우 다른 작업이 사용자에 게 표시 될 수 있습니다.

## <a name="a-loginviewcontroller-for-sign-in-with-apple"></a>Apple에서 로그인에 대 한 LoginViewController

로그인 `UIViewController` 논리를 구현 하 고 Apple에서 로그인을 제공 하는는 `IASAuthorizationControllerDelegate` 아래 `IASAuthorizationControllerPresentationContextProviding` `LoginViewController` 예제와 같이 및를 구현 해야 합니다.

```csharp
public partial class LoginViewController : UIViewController, IASAuthorizationControllerDelegate, IASAuthorizationControllerPresentationContextProviding {
    public LoginViewController (IntPtr handle) : base (handle)
    {
    }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        // Perform any additional setup after loading the view, typically from a nib.

        SetupProviderLoginView ();
    }

    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        PerformExistingAccountSetupFlows ();
    }

    void SetupProviderLoginView ()
    {
        var authorizationButton = new ASAuthorizationAppleIdButton (ASAuthorizationAppleIdButtonType.Default, ASAuthorizationAppleIdButtonStyle.White);
        authorizationButton.TouchUpInside += HandleAuthorizationAppleIDButtonPress;
        loginProviderStackView.AddArrangedSubview (authorizationButton);
    }

    // Prompts the user if an existing iCloud Keychain credential or Apple ID credential is found.
    void PerformExistingAccountSetupFlows ()
    {
        // Prepare requests for both Apple ID and password providers.
        ASAuthorizationRequest [] requests = {
            new ASAuthorizationAppleIdProvider ().CreateRequest (),
            new ASAuthorizationPasswordProvider ().CreateRequest ()
        };

        // Create an authorization controller with the given requests.
        var authorizationController = new ASAuthorizationController (requests);
        authorizationController.Delegate = this;
        authorizationController.PresentationContextProvider = this;
        authorizationController.PerformRequests ();
    }

    private void HandleAuthorizationAppleIDButtonPress (object sender, EventArgs e)
    {
        var appleIdProvider = new ASAuthorizationAppleIdProvider ();
        var request = appleIdProvider.CreateRequest ();
        request.RequestedScopes = new [] { ASAuthorizationScope.Email, ASAuthorizationScope.FullName };

        var authorizationController = new ASAuthorizationController (new [] { request });
        authorizationController.Delegate = this;
        authorizationController.PresentationContextProvider = this;
        authorizationController.PerformRequests ();
    }
}
```

![Apple에서 로그인을 사용 하는 샘플 앱 애니메이션](sign-in-images/sign-in-flow.png)

이 예제 코드는에서 `PerformExistingAccountSetupFlows` 현재 로그인 상태를 확인 하 고 현재 뷰에 대리자로 연결 합니다. 기존 iCloud 키 집합 자격 증명 또는 Apple ID 자격 증명이 있는 경우 사용자에 게 해당 자격 증명을 사용 하 라는 메시지가 표시 됩니다.

Apple은 `ASAuthorizationAppleIdButton`이러한 용도로만 사용할 단추를 제공 합니다. 작업을 수행 하는 경우 단추는 메서드에서 `HandleAuthorizationAppleIDButtonPress`처리 된 워크플로를 트리거합니다.

## <a name="handling-authorization"></a>권한 부여 처리

에서 사용자 계정을 저장 하는 사용자 지정 논리를 구현합니다.`IASAuthorizationController` 아래 예제에서는 사용자의 계정을 키 집합 Apple의 자체 저장소 서비스에 저장 합니다.

```csharp
#region IASAuthorizationController Delegate

[Export ("authorizationController:didCompleteWithAuthorization:")]
public void DidComplete (ASAuthorizationController controller, ASAuthorization authorization)
{
    if (authorization.GetCredential<ASAuthorizationAppleIdCredential> () is ASAuthorizationAppleIdCredential appleIdCredential) {
        var userIdentifier = appleIdCredential.User;
        var fullName = appleIdCredential.FullName;
        var email = appleIdCredential.Email;

        // Create an account in your system.
        // For the purpose of this demo app, store the userIdentifier in the keychain.
        try {
            new KeychainItem ("com.example.apple-samplecode.juice", "userIdentifier").SaveItem (userIdentifier);
        } catch (Exception) {
            Console.WriteLine ("Unable to save userIdentifier to keychain.");
        }

        // For the purpose of this demo app, show the Apple ID credential information in the ResultViewController.
        if (!(PresentingViewController is ResultViewController viewController))
            return;

        InvokeOnMainThread (() => {
            viewController.UserIdentifierText = userIdentifier;
            viewController.GivenNameText = fullName?.GivenName ?? "";
            viewController.FamilyNameText = fullName?.FamilyName ?? "";
            viewController.EmailText = email ?? "";

            DismissViewController (true, null);
        });
    } else if (authorization.GetCredential<ASPasswordCredential> () is ASPasswordCredential passwordCredential) {
        // Sign in using an existing iCloud Keychain credential.
        var username = passwordCredential.User;
        var password = passwordCredential.Password;

        // For the purpose of this demo app, show the password credential as an alert.
        InvokeOnMainThread (() => {
            var message = $"The app has received your selected credential from the keychain. \n\n Username: {username}\n Password: {password}";
            var alertController = UIAlertController.Create ("Keychain Credential Received", message, UIAlertControllerStyle.Alert);
            alertController.AddAction (UIAlertAction.Create ("Dismiss", UIAlertActionStyle.Cancel, null));

            PresentViewController (alertController, true, null);
        });
    }
}

[Export ("authorizationController:didCompleteWithError:")]
public void DidComplete (ASAuthorizationController controller, NSError error)
{
    Console.WriteLine (error);
}

#endregion
```

## <a name="authorization-controller"></a>권한 부여 컨트롤러

이 구현 `ASAuthorizationController` 에서 최종 조각은 공급자에 대 한 권한 부여 요청을 관리 하는입니다.

```csharp
#region IASAuthorizationControllerPresentation Context Providing

public UIWindow GetPresentationAnchor (ASAuthorizationController controller) => View.Window;

#endregion
```

## <a name="summary"></a>요약

이 문서에서는 iOS 용 Apple에 로그인 했습니다. 

## <a name="related-links"></a>관련 링크

* [Apple 지침을 사용 하 여 로그인](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)
* [Apple 자격으로 로그인 합니다.][2]
* [WWDC 2019 세션 706: Apple에 로그인 소개][3]
* [Xamarin을 사용 하 여 Apple에 로그인 설정][4]

[1]: https://developer.apple.com/documentation/authenticationservices/adding_the_sign_in_with_apple_flow_to_your_app
[2]: https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_applesignin
[3]: https://developer.apple.com/videos/play/wwdc19/706/
[4]: ~/xamarin-forms/platform/sign-in-with-apple/setup.md
[5]: https://developer.apple.com/account/resources/identifiers/list