---
title: Xamarin.ios를 사용 하는 Touch id 및 얼굴 ID
description: 이 문서에서는 iOS의 생체 인식 인증에 대해 간략하게 설명 합니다.
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: profexorgeek
ms.author: jusjohns
ms.date: 12/16/2019
ms.openlocfilehash: 744a07343b9da87f196c0664f57b7d950844ab04
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490429"
---
# <a name="use-touch-id-and-face-id-with-xamarinios"></a>Xamarin.ios에서 Touch ID 및 Face ID 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-faceidsample/)

iOS는 다음과 같은 두 가지 생체 인식 인증 시스템을 지원 합니다.

1. **TOUCH ID** 는 홈 단추 아래에서 지문 센서를 사용 합니다.
1. **얼굴 ID** 는 전면 카메라 센서를 사용 하 여 얼굴 검색을 통해 사용자를 인증 합니다.

Touch ID는 ios 11의 iOS 7 및 얼굴 ID에서 도입 되었습니다.

이러한 인증 시스템은 _Secure Enclave_라는 하드웨어 기반 보안 프로세서를 사용 합니다. Secure Enclave는 얼굴 및 지문 데이터의 수학적 표현을 암호화 하 고이 정보를 사용 하 여 사용자를 인증 합니다. Apple에 따라 얼굴 및 지문 데이터는 장치를 떠나지 않으며 iCloud로 백업 되지 않습니다. 앱은 _로컬 인증_ API를 통해 보안 Enclave 상호 작용 하 고, 얼굴 또는 지문 데이터를 검색 하거나 보안 Enclave에 직접 액세스할 수 없습니다.

Touch ID 및 Face ID는 앱에서 보호 된 콘텐츠에 대 한 액세스를 제공 하기 전에 사용자를 인증 하는 데 사용할 수 있습니다.

## <a name="local-authentication-context"></a>로컬 인증 컨텍스트

IOS의 생체 인식 인증은 `LAContext` 클래스 인스턴스인 _로컬 인증 컨텍스트_ 개체를 사용 합니다. `LAContext` 클래스 수 있습니다.

- 생체 인식 하드웨어의 가용성을 확인 합니다.
- 인증 정책을 평가 합니다.
- 액세스 제어를 평가 합니다.
- 인증 프롬프트를 사용자 지정 하 고 표시 합니다.
- 인증 상태를 다시 사용 하거나 무효화 합니다.
- 자격 증명을 관리 합니다.

## <a name="detect-available-authentication-methods"></a>사용 가능한 인증 방법 검색

샘플 프로젝트에는 `AuthenticationViewController`에 의해 지원 되는 `AuthenticationView` 포함 되어 있습니다. 이 클래스는 `ViewWillAppear` 메서드를 재정의 하 여 사용 가능한 인증 메서드를 검색 합니다.

```csharp
partial class AuthenticationViewController: UIViewController
{
    // ...
    string BiometryType = "";

    public override void ViewWillAppear(bool animated)
    {
        base.ViewWillAppear(animated);
        unAuthenticatedLabel.Text = "";
    
        var context = new LAContext();
        var buttonText = "";

        // Is login with biometrics possible?
        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out var authError1))
        {
            // has Touch ID or Face ID
            if (UIDevice.CurrentDevice.CheckSystemVersion(11, 0))
            {
                context.LocalizedReason = "Authorize for access to secrets"; // iOS 11
                BiometryType = context.BiometryType == LABiometryType.TouchId ? "Touch ID" : "Face ID";
                buttonText = $"Login with {BiometryType}";
            }
            // No FaceID before iOS 11
            else
            {
                buttonText = $"Login with Touch ID";
            }
        }

        // Is pin login possible?
        else if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, out var authError2))
        {
            buttonText = $"Login"; // with device PIN
            BiometryType = "Device PIN";
        }

        // Local authentication not possible
        else
        {
            // Application might choose to implement a custom username/password
            buttonText = "Use unsecured";
            BiometryType = "none";
        }
        AuthenticateButton.SetTitle(buttonText, UIControlState.Normal);
    }
}
```

`ViewWillAppear` 메서드는 UI가 사용자에 게 표시 될 때 호출 됩니다. 이 메서드는 `LAContext`의 새 인스턴스를 정의 하 고 `CanEvaluatePolicy` 메서드를 사용 하 여 생체 인식 인증의 사용 여부를 확인 합니다. 이 경우 시스템 버전 및 `BiometryType` 열거를 확인 하 여 사용할 수 있는 생체 인식 옵션을 확인 합니다.

생체 인식 인증을 사용 하지 않는 경우 앱은 PIN 인증으로 대체 하려고 시도 합니다. 생체 인식 또는 PIN 인증을 모두 사용할 수 없는 경우 장치 소유자는 보안 기능을 사용 하도록 설정 하지 않았고 로컬 인증을 통해 콘텐츠를 보호할 수 없습니다.

## <a name="authenticate-a-user"></a>사용자 인증

샘플 프로젝트의 `AuthenticationViewController`에는 사용자 인증을 담당 하는 `AuthenticateMe` 메서드가 포함 되어 있습니다.

```csharp
partial class AuthenticationViewController: UIViewController
{
    // ...
    string BiometryType = "";

    partial void AuthenticateMe(UIButton sender)
    {
        var context = new LAContext();
        NSError AuthError;
        var localizedReason = new NSString("To access secrets");
    
        // Because LocalAuthentication APIs have been extended over time,
        // you must check iOS version before setting some properties
        context.LocalizedFallbackTitle = "Fallback";
    
        if (UIDevice.CurrentDevice.CheckSystemVersion(10, 0))
        {
            context.LocalizedCancelTitle = "Cancel";
        }
        if (UIDevice.CurrentDevice.CheckSystemVersion(11, 0))
        {
            context.LocalizedReason = "Authorize for access to secrets";
            BiometryType = context.BiometryType == LABiometryType.TouchId ? "TouchID" : "FaceID";
        }
    
        // Check if biometric authentication is possible
        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError))
        {
            replyHandler = new LAContextReplyHandler((success, error) =>
            {
                // This affects UI and must be run on the main thread
                this.InvokeOnMainThread(() =>
                {
                    if (success)
                    {
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        unAuthenticatedLabel.Text = $"{BiometryType} Authentication Failed";
                    }
                });
    
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, localizedReason, replyHandler);
        }

        // Fall back to PIN authentication
        else if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, out AuthError))
        {
            replyHandler = new LAContextReplyHandler((success, error) =>
            {
                // This affects UI and must be run on the main thread
                this.InvokeOnMainThread(() =>
                {
                    if (success)
                    {
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        unAuthenticatedLabel.Text = "Device PIN Authentication Failed";
                        AuthenticateButton.Hidden = true;
                    }
                });
    
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, localizedReason, replyHandler);
        }

        // User hasn't configured any authentication: show dialog with options
        else
        {
            unAuthenticatedLabel.Text = "No device auth configured";
            var okCancelAlertController = UIAlertController.Create("No authentication", "This device does't have authentication configured.", UIAlertControllerStyle.Alert);
            okCancelAlertController.AddAction(UIAlertAction.Create("Use unsecured", UIAlertActionStyle.Default, alert => PerformSegue("AuthenticationSegue", this)));
            okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine("Cancel was clicked")));
            PresentViewController(okCancelAlertController, true, null);
        }
    } 
}
```

`AuthenticateMe` 메서드는 **로그인** 단추를 누르는 사용자에 대 한 응답으로 호출 됩니다. 새 `LAContext` 개체가 인스턴스화되고 장치 버전이 검사 되어 로컬 인증 컨텍스트에서 설정할 속성을 결정 합니다.

`CanEvaluatePolicy` 메서드는 생체 인식 인증이 사용 되는지 확인 하기 위해 호출 되 고, 가능한 경우 PIN 인증으로 대체 하 고, 인증을 사용할 수 없는 경우 마지막으로 보안 되지 않은 모드를 제공 합니다. 인증 방법을 사용할 수 있는 경우 `EvaluatePolicy` 메서드를 사용 하 여 UI를 표시 하 고 인증 프로세스를 완료 합니다.

샘플 프로젝트에는 인증에 성공한 경우 데이터를 표시 하기 위한 모의 데이터 및 뷰가 포함 되어 있습니다.

## <a name="related-links"></a>관련 링크

- [Touch ID 또는 얼굴 ID를 사용 하는 로컬 인증 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-faceidsample/)
- Support.apple.com의 [TOUCH ID 정보](https://support.apple.com/en-us/HT204587)
- Support.apple.com의 [얼굴 ID 정보](https://support.apple.com/en-us/HT208108)
