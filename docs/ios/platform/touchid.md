---
title: Xamarin.ios의 Touch ID
description: 이 문서에서는 Xamarin.ios 앱에서 Touch ID, Apple의 생체 인식 지문 인증 기술을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 54e910d0a4f3301ca441fd18ddb27da930e9415c
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528780"
---
# <a name="touch-id-in-xamarinios"></a>Xamarin.ios의 Touch ID

Touch ID는 암호와 유사 하 게 사용자를 인증 하는 수단으로 iOS 7에서 도입 되었습니다. 그러나 앱 스토어를 사용 하 고 iTunes를 사용 하 고 iCloud 키 집합을 인증 하는 경우에만 장치의 잠금을 해제 하는 것이 제한 되었습니다.

이제 로컬 인증 API를 사용 하 여 iOS 8 응용 프로그램에서 Touch ID를 인증 메커니즘으로 사용 하는 두 가지 방법이 있습니다. 현재 원격으로 인증 하는 데 로컬 인증을 사용할 수 없습니다.

Touch ID와 해당 기능을 완벽 하 게 이해 하려면 키 집합 서비스와 사용자 데이터에 대 한 새로운 변경 사항을 살펴보세요. 또한 키 집합 액세스는 새 Acl (Access Control 목록) 기능을 사용 하 여 iOS 8에서 확장 되었습니다.

## <a name="keychain--secure-enclave"></a>키 집합 & Secure Enclave

키 집합은 하나의 개별 Apple ID에 대 한 암호, 키, 인증서 및 메모에 대 한 보안 저장소를 제공 하는 대량 데이터베이스입니다. IOS 8에서 응용 프로그램은 항상 고유한 키 집합 항목에 액세스할 수 있으며 다른 응용 프로그램의 키 집합 항목에는 액세스할 수 없습니다. 이는 키 집합을 단일 암호로 잠금 해제 하는 OS X와 다르므로 키 집합 서비스 인식 응용 프로그램에서 키 집합을 사용할 수 있습니다. 이 문서에서는 iOS 8에서 키 집합이 작동 하는 방식에 대해 집중적으로 설명 합니다.

키 집합은 각 행을 키 _집합 항목_이라고 하는 특수 한 데이터베이스입니다. 각 항목은 키 집합 특성으로 설명 되며 암호화 된 값으로 구성 됩니다. 키 집합을 효율적으로 사용할 수 있도록 하기 위해 작은 항목 또는 _비밀_에 최적화 되어 있습니다.
각 키 집합 항목은 사용자 암호 및 고유한 장치 암호에 의해 보호 됩니다. 사용자가 장치를 사용 하지 않는 경우에도 키 집합 항목을 보호 해야 합니다. 장치를 잠금 해제 한 경우에만 항목을 사용할 수 있도록 허용 하 여 iOS에서 구현 됩니다. 즉, 장치가 잠겨 있으면 사용할 수 없게 됩니다. 암호화 된 백업에 저장할 수도 있습니다. 키 집합의 주요 기능 중 하나는 액세스 제어를 적용 하는 것입니다. 응용 프로그램은 키 집합의 해당 부분에 액세스할 수 있으며 다른 모든 응용 프로그램은 차단 됩니다. 아래 다이어그램에서는 응용 프로그램이 키 집합을 조작 하는 방법을 보여 줍니다.

[![](touchid-images/image1.png "이 다이어그램에서는 응용 프로그램이 키 집합을 조작 하는 방법을 보여 줍니다.")](touchid-images/image1.png#lightbox)

### <a name="secure-enclave"></a>Secure Enclave

키 집합에서 키 집합 항목을 해독할 수 없습니다. 대신, *Secure Enclave*에서 수행 됩니다. Secure enclave는 등록 된 인쇄에 대해 터치식 ID 센서의 지문 데이터와 성공적인 일치를 확인 하는 A7 칩 내의 공동 프로세서입니다. 그런 다음 키 집합 항목의 암호를 해독 하 고 해독 된 암호를 키 집합으로 반환 합니다.

### <a name="working-with-keychain"></a>키 집합 작업

먼저 응용 프로그램에서 키 집합을 쿼리하여 암호가 있는지 확인 해야 합니다. 존재 하지 않는 경우 사용자에 게 묻지 않도록 암호를 요청 해야 할 수 있습니다. 암호를 업데이트 해야 하는 경우 사용자에 게 새 암호를 입력 하 라는 메시지를 표시 하 고 업데이트 된 값을 키 집합에 전달 합니다.

> [!NOTE]
> 키 집합에서 검색 된 비밀을 사용한 후에는 메모리에서 데이터에 대 한 모든 참조를 제거 해야 합니다. 전역 변수에 할당 하지 않습니다.

## <a name="keychain-acl-and-touch-id"></a>키 집합 ACL 및 Touch ID

Access Control 목록은 특정 작업을 수행 하도록 허용 하기 위해 수행 해야 하는 작업에 대 한 정보를 설명 하는 iOS 8의 새로운 키 집합 항목 특성입니다. 이는 경고 대화 상자를 표시 하거나 암호를 요청 하는 형식입니다. ACL을 사용 하면 키 집합 항목에 대 한 액세스 가능성 및 인증을 설정할 수 있습니다. 아래 다이어그램에서는이 새 특성이 키 집합 항목의 나머지 부분과 연결 되는 방법을 보여 줍니다.

[![](touchid-images/image2.png "이 다이어그램에서는이 새 특성이 키 집합 항목의 나머지와 연결 되는 방법을 보여 줍니다.")](touchid-images/image2.png#lightbox)

IOS 8을 기준으로 이제 iPhone 5 초 이상에서 보안 enclave 적용 되 `SecAccessControl`는 새로운 사용자의 현재 상태 정책이 있습니다. 아래 표에서는 장치 구성이 정책 평가에 영향을 주는 방법만 확인할 수 있습니다.

|장치 구성|정책 평가|백업 메커니즘|
|--- |--- |--- |
|암호 없는 장치|액세스 권한 없음|없음|
|암호를 사용 하는 장치|암호 필요|없음|
|Touch ID를 사용 하는 장치|Touch ID 선호|암호 허용|

Secure Enclave 내의 모든 작업은 서로를 신뢰할 수 있습니다. 즉, Touch ID 인증 결과를 사용 하 여 키 집합 항목 암호 해독에 권한을 부여할 수 있습니다. 또한 Secure Enclave는 실패 한 터치 ID와 일치 하는 카운터를 유지 합니다 .이 경우 사용자가 암호를 사용 하 여 되돌려야 합니다.
_로컬 인증_이라고 하는 iOS 8의 새 프레임 워크는 장치 내에서이 인증 프로세스를 지원 합니다. 이에 대해서는 다음 섹션에서 살펴보겠습니다.

## <a name="local-authentication"></a>로컬 인증

이전 섹션에서 설명한 대로 응용 프로그램은 로컬 인증을 사용 하 여 장치에 설정 된 보안 정책을 준수 하는 사용자를 인증할 수 있습니다.

현재 API는 다음과 같은 두 가지 기능만 제공 합니다. 첫째, 새 키 집합 Access Control 목록 (Acl)을 사용 하 여 기존 키 집합 서비스를 지원 합니다. 사용자 지문을 성공적으로 인증 하면 키 집합 데이터의 잠금을 해제할 수 있습니다.

두 번째로 LocalAuthentication은 응용 프로그램을 로컬로 인증 하는 두 가지 방법을 제공 합니다. 개발자는를 `CanEvaluatePolicy` 사용 하 여 장치에서 Touch ID를 수락할 수 있는지 확인 한 다음 `EvaluatePolicy` 인증 작업을 시작할 수 있습니다.

두 기능 모두 로컬 인증을 제공 하지만 응용 프로그램이 나 사용자가 원격 서버에 인증 하는 메커니즘을 제공 하지 않습니다.
로컬 인증은 인증을 위한 새로운 표준 사용자 인터페이스를 제공 합니다. Touch ID의 경우 아래와 같이 두 개의 단추가 있는 경고 보기입니다. 하나는 취소 하 고 다른 하나는 인증 방법 (암호)을 사용 하는 단추입니다. 또한 설정 해야 하는 사용자 지정 메시지도 있습니다. Touch ID 인증이 필요한 이유를 설명 하는 데이 방법을 사용 하는 것이 좋습니다.

[![](touchid-images/image12.png "Touch ID 인증 경고")](touchid-images/image12.png#lightbox)

### <a name="with-keychain-services"></a>키 집합 서비스 사용

키 집합 항목의 암호를 해독 하는 방법에 대 한 자세한 내용은 보안 enclave을 사용 하 여 암호를 확인 하는 방법을 살펴보았습니다. IOS 8에서 로컬 인증을 사용 하 여 대체 메커니즘의 구현이 나 암호를 제공 하는 Access Control 목록 기능과 함께 Touch ID 확인을 요청할 수 있습니다.
ACL을 사용 하려면 `SecAccessControl` 정책을 사용 하 고 또는 `SecAccessible.WhenUnlocked`을 사용 하 여 `SecAccessible.WhenPasscodeSetThisDeviceOnly` 장치의 상태를 확인 해야 합니다.

#### <a name="considerations-with-acl"></a>ACL에 대 한 고려 사항

키 집합에 ACL을 사용 하는 경우 다음과 같은 여러 가지 사항을 염두에 두어야 합니다.

- 포그라운드 응용 프로그램 에서만 사용 – 백그라운드 스레드에서 키 집합 작업을 호출 하면 호출이 실패 합니다.
- 키 집합 항목을 추가 하 고 업데이트 하려면 인증이 필요할 수 있습니다.
- 요청에서 키 집합에 일치 하는 항목이 여러 개 반환 되는 경우 인증이 필요할 수 있습니다.
- ACL로 보호 된 항목은 장치 전용 이므로 동기화 되거나 백업 되지 않습니다.

### <a name="using-local-authentication-without-keychain-services"></a>키 집합 서비스 없이 로컬 인증 사용

로컬 인증은 암호 또는 터치 ID와 같은 자격 증명을 수집 하 고, 보안 Enclave 작업 하 여 사용자 인증을 완료 하는 방법으로 생성 되었습니다. 응용 프로그램과 보안 Enclave 간의 브리지로 간주 하 여 서로 직접 통신할 수 없습니다. 응용 프로그램에 대 한 정책 평가에도 사용할 수 있습니다.

이렇게 하기 위해 응용 프로그램은 로컬 인증 내에서 정책 평가를 호출 하 여 Secure Enclave 내에서 작업을 시작 합니다. 이를 활용 하 여 보안 Enclave을 직접 쿼리하거나 액세스 하지 않고도 앱에 대 한 인증을 제공할 수 있습니다.

[![](touchid-images/image13a.png "키 집합 서비스 없이 로컬 인증 사용")](touchid-images/image13a.png#lightbox)

응용 프로그램에서 로컬 인증을 사용 하면 사용자 확인을 간편 하 게 구현할 수 있습니다. 예를 들어, 은행 응용 프로그램 같은 장치 소유자의 눈에만 기능을 잠금 해제 하거나 개별 사용자를 위한 도움이 될 자녀 보호를 사용할 수 있습니다. 프로그램별. 이미 존재 하는 인증을 확장 하는 방법으로 사용할 수도 있습니다. 즉, 사용자의 정보를 안전 하 게 보호 하는 것은 물론 옵션도 있습니다.

로컬 인증의 보안은 키 집합의 보안과 다릅니다. 예를 들어 키 집합을 사용 하는 경우 운영 체제와 보안 Enclave 간에 트러스트가 설정 됩니다. 로컬 인증을 사용 하는 경우 응용 프로그램과 운영 체제 사이에서 보안 Enclave 자체가 아닌 보안 Enclave 결과에만 액세스할 수 있습니다.

보안 주체에서 등록 된 손가락 또는 지문 이미지에 대 한 **액세스 권한이** 없다는 것을 알고 있어야 합니다. Secure Enclave는이 정보의 소유자 이므로 다른 시스템 구성 요소에 액세스할 수 없습니다.

로컬 인증 API를 활용 하 여 키 집합 없이 Touch ID를 사용 하려면 몇 가지 기능을 사용할 수 있습니다. 이러한 내용은 아래에 자세히 설명 되어 있습니다.

*   `CanEvaluatePolicy`– 장치에서 Touch ID를 받아들일 수 있는지만 확인 합니다.
*   `EvaluatePolicy`– 인증 작업을 시작 하 고 UI를 표시 하며 `true` 또는 `false` 대답을 반환 합니다.
*   `DeviceOwnerAuthenticationWithBiometrics`– Touch ID 화면을 표시 하는 데 사용할 수 있는 정책입니다. 여기에는 암호 대체 메커니즘이 없다는 것에 주목 해야 합니다. 대신 사용자가 Touch ID 인증을 건너뛸 수 있도록 응용 프로그램에서이 대체 (fallback)를 구현 해야 합니다.

로컬 인증을 사용 하는 경우 다음과 같은 몇 가지 주의 사항이 있습니다.

*   키 집합을 사용 하는 것 처럼 포그라운드에서 실행할 수 있습니다. 이를 백그라운드 스레드에서 호출 하면 오류가 발생 합니다.
*   정책 평가가 실패할 수 있다는 점에 유의 하세요. 암호 단추를 대체 방법으로 구현 해야 합니다.
*   인증이 필요한 이유를 `localizedReason` 설명 하기 위해를 제공 해야 합니다. 이렇게 하면 사용자와의 트러스트를 만들 수 있습니다.

다음 섹션에서는 이러한 주의 사항을 고려 하 여 API를 구현 하는 방법을 살펴보겠습니다.

## <a name="adding-touch-id-to-your-application"></a>응용 프로그램에 Touch ID 추가

이전 섹션에서는 키 집합 및 로컬 인증을 사용 하는 액세스 및 인증에 대 한 이론적 근거를 살펴보았습니다. 이제 Touch ID를 응용 프로그램에 통합 하는 방법을 살펴보겠습니다.

### <a name="walkthrough"></a>연습

응용 프로그램에 일부 Touch ID 인증을 추가 하는 방법을 살펴보겠습니다. 이 연습에서는 인증 된 사용자가 목록에 작업을 추가할 수 있도록 해 주는 [Storyboard 테이블 – 로컬 인증](https://docs.microsoft.com/samples/xamarin/ios-samples/storyboardtable-localauthentication) 샘플과 같이 작동 하도록 로컬 인증을 추가 하 여 [스토리 보드 테이블](https://docs.microsoft.com/samples/xamarin/ios-samples/data/storyboardtable/) 샘플을 업데이트 하겠습니다.

1. 샘플을 다운로드 하 고 Mac용 Visual Studio에서 실행 합니다.
2. 을 두 번 `MainStoryboard.Storyboard` 클릭 하 여 iOS 디자이너에서 샘플을 엽니다. 이 샘플에서는 인증을 제어 하는 새 화면을 응용 프로그램에 추가 하려고 합니다. 이는 현재 `MasterViewController`의 앞에 있습니다.
3. **도구 상자** 에서 **Design Surface**새 **뷰 컨트롤러** 를 끌어 옵니다. **탐색 컨트롤러**에서 **Ctrl + Drag** 를 수행 하 여이를 **루트 뷰 컨트롤러로** 설정 합니다.

    [![](touchid-images/image4.png "루트 뷰 컨트롤러 설정")](touchid-images/image4.png#lightbox)
4. 새 뷰 컨트롤러 `AuthenticationViewController`의 이름을로 표시 합니다.
5. 그런 다음 단추를 끌어서에 놓습니다 `AuthenticationViewController`. 이 `AuthenticateButton`를 호출 하 고 텍스트 `Add a Chore`를 지정 합니다.
6. `AuthenticateButton` 호출`AuthenticateMe`된에서 이벤트를 만듭니다.
7. 아래쪽에 있는 검은색 표시줄 `AuthenticationViewController` 을 클릭 하 고 **Ctrl + 끌어서** 를 선택 하 고 **푸시** (또는 size 클래스를 `MasterViewController` 사용 하는 경우 **표시** )를 선택 하 여에서 수동 segue을 만듭니다.

    [![](touchid-images/image5.png "막대에서 MasterViewController로 끌고 푸시 또는 표시를 선택 합니다.")](touchid-images/image6.png#lightbox)
8. 새로 만든 segue를 클릭 하 고 아래 그림과 같이 식별자 `AuthenticationSegue`를 제공 합니다.

    [![](touchid-images/image7.png "Segue 식별자를 AuthenticationSegue로 설정 합니다.")](touchid-images/image7.png#lightbox)
9. 다음 코드를 `AuthenticationViewController`에 추가합니다.

    ```csharp
    partial void AuthenticateMe (UIButton sender)
    {
        var context = new LAContext();
        NSError AuthError;
        var myReason = new NSString("To add a new chore");

        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError)){
            var replyHandler = new LAContextReplyHandler((success, error) => {
                this.InvokeOnMainThread(()=> {
                    if(success)
                    {
                        Console.WriteLine("You logged in!");
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        // Show fallback mechanism here
                    }
                });
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, myReason, replyHandler);
        };
    }
    ```

로컬 인증을 사용 하 여 Touch ID 인증을 구현 하는 데 필요한 모든 코드입니다. 아래 이미지의 강조 표시 된 줄은 로컬 인증의 사용을 보여 줍니다.

[![](touchid-images/image8.png "강조 표시 된 줄은 로컬 인증의 사용을 보여 줍니다.")](touchid-images/image8.png#lightbox)

먼저를 사용 하 `CanEvaluatePolicy` 고 정책 `DeviceOwnerAuthenticationWithBiometrics`에 전달 하 여 장치에서 Touch ID 입력을 허용할 수 있는지 여부를 설정 해야 합니다. True 이면를 사용 하 `EvaluatePolicy`여 Touch ID UI를 표시할 수 있습니다. 전달 `EvaluatePolicy` 해야 하는 정보에는 정책 자체, 인증이 필요한 이유를 설명 하는 문자열 및 회신 처리기의 세 가지가 있습니다. 회신 처리기는 인증에 성공 하거나 실패 한 경우에 수행 해야 하는 작업을 응용 프로그램에 알려 줍니다. 회신 처리기에 대해 자세히 살펴보겠습니다.

[![](touchid-images/image9.png "회신 처리기")](touchid-images/image9.png#lightbox)

`LAContextReplyHandler`회신 처리기는 success `bool` 매개 변수, 값, `NSError` 호출 `error`된를 사용 하는 형식으로 지정 됩니다. 성공적으로 완료 되 면 인증 하려는 모든 항목을 실제로 수행할 것입니다 .이 경우에는 새 chore를 추가할 수 있는 화면이 표시 됩니다. 로컬 인증의 주의 사항 중 하나는 포그라운드에서 실행 해야 하기 때문에 다음을 사용 `InvokeOnMainThread`해야 합니다.

[![](touchid-images/image10.png "로컬 인증에 InvokeOnMainThread 사용")](touchid-images/image10.png#lightbox)

마지막으로, 인증이 성공적으로 완료 되 면로 `MasterViewController`전환 하려고 합니다. `PerformSegue` 메서드를 사용 하 여이 작업을 수행할 수 있습니다.

[![](touchid-images/image11.png "PerformSegue 메서드를 호출 하 여 MasterViewController로 전환 합니다.")](touchid-images/image11.png#lightbox)

## <a name="summary"></a>요약

이 가이드에서는 키 집합 및이를 iOS에서 작동 하는 방법을 살펴보았습니다. 또한 키 집합 ACL을 탐색 하 고이를 iOS에서 변경 했습니다. 다음으로, iOS 8의 새로운 로컬 인증 프레임 워크를 살펴보고 응용 프로그램에서 Touch ID 인증을 구현 하는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [스토리 보드 테이블-로컬 인증](https://docs.microsoft.com/samples/xamarin/ios-samples/storyboardtable-localauthentication)
- [키 집합 WWDC 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-keychaintouchid/)
- [키 집합 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/keychain/)
