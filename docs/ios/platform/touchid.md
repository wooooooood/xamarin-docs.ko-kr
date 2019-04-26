---
title: Xamarin.iOS에서 touch ID
description: 이 문서에서는 Xamarin.iOS 앱에 Touch ID를 Apple의 지문 생체 인식 인증 기술을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 2d67bc71361e335515cfba8b5a20e157ed6b6b05
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61087856"
---
# <a name="touch-id-in-xamarinios"></a>Xamarin.iOS에서 touch ID

Touch ID는 사용자-암호 인증 수단으로 iOS 7에서에서 도입 되었습니다. 그러나 장치를 잠금 해제, 앱 스토어를 사용 하 여, iTunes를 사용 하 여와 iCloud 키 집합에만 인증으로 제한 되었습니다.

이제 두 가지 로컬 인증 API를 사용 하 여 iOS 8 응용 프로그램의 인증 메커니즘으로 Touch ID를 사용 합니다. 불가능 현재 로컬 인증 원격으로 인증을 사용 합니다.

Touch ID 및 해당 분량을 완벽 하 게 이해 하려면 살펴볼 Keychain 서비스 및 명령을 사용자의 데이터에 대 한 이러한 새로운 변경 내용을 의미 합니다. Keychain Access도 새로운 액세스 제어 목록 (Acl) 기능을 사용 하 여 ios 8 시 확장 되었습니다.

## <a name="keychain--secure-enclave"></a>키 집합 및 보안 Enclave

키 집합은 하나의 개별 Apple id 암호, 키, 인증서 및 정보에 대 한 보안 저장소를 제공 하는 대규모 데이터베이스 IOS 8에서에서 응용 프로그램은 항상 자체 고유 키 집합 항목에 대 한 액세스 권한이 하 고 다른 응용 프로그램의 모든 키 집합 항목에 액세스할 수 없습니다. 이 Osx 키 집합은 단일 암호를 잠금 해제 되어 있으므로 키 집합을 사용 하 여 키 집합 서비스 인식 응용 프로그램에서에서 서로 다릅니다. 이 문서의 keychain iOS 8에서에서 작동 하는 방법을 집중 됩니다.

키 집합은 각 행 이라고 하는 특수 한 데이터베이스를 _Keychain 항목_합니다. 각 항목은 keychain 특성에 의해 설명 되며 암호화 된 값으로 구성 합니다. 작은 항목에 대 한 최적화 되어 효율적으로 사용할 키 집합 있도록 또는 _비밀_합니다.
각 키 집합이 사용자 암호 및 고유 장치 비밀 보호 됩니다. 사용자가 자신의 장치를 사용 하지 않는 경우에 키 집합 항목을 보호 되어야 합니다. 장치가 잠긴 경우 사용할 수 있으려면 항목에만 허용 하 여 iOS에서 구현 되었습니다-사용할 수 없게 되는 장치가 잠겨 있을 경우. 암호화 된 백업에 저장 될 수 있습니다. 액세스 제어를 적용 하는 키 집합의 주요 기능 중 하나 응용 프로그램의 키 집합의 해당 부분에 액세스할 수 있고 다른 모든 응용 프로그램을 주고받을 수 없게 됩니다. 아래 다이어그램에서는 응용 프로그램 키 집합 상호 작용 하는 방법을 보여 줍니다.

[![](touchid-images/image1.png "이 다이어그램에서는 응용 프로그램 키 집합 상호 작용 하는 방법을 보여 줍니다.")](touchid-images/image1.png#lightbox)

### <a name="secure-enclave"></a>보안 Enclave

키 집합 자체에서 키 집합 항목을 해독할 수 없습니다. 수행 하는 대신 합니다 *보안 Enclave*합니다. 보안 enclave는 등록 된 인쇄에 대해 Touch ID 센서에서 지문 데이터에서 일치 하는 항목을 결정 하는 일을 담당 하는 A7 칩 내에서 공동 프로세서. 그런 다음 키 집합 항목의 암호를 해독 하 고 키 집합 암호 해독 된 암호를 반환 합니다.

### <a name="working-with-keychain"></a>키 집합 사용

먼저 응용 프로그램 암호가 있는지 확인 하려면 키 집합에 쿼리해야 합니다. 존재 하지 않는 경우 메시지를 표시 하려면 암호를 묻는 계속 되지 않습니다. 암호를 업데이트 해야 하는 경우 새 암호를 묻는 메시지를 표시 하 고 키 집합에 업데이트 된 값을 전달 합니다.

> [!NOTE]
> 암호를 사용 하 여 키 집합에서 검색 된 후 메모리에서 데이터에 대 한 모든 참조를 제거 해야 합니다. 전역 변수를 할당 하지 않습니다.

## <a name="keychain-acl-and-touch-id"></a>키 집합 ACL 및 터치 ID

액세스 제어 목록에는 특정 작업이 발생 되도록 허용 하려면 어떻게 해야 하는 것에 대 한 정보를 설명 하는 iOS 8에서에서 새 키 집합 항목 특성입니다. 이 형식의 경고 대화 상자를 표시 하거나 암호를 요청할 수 있습니다. ACL을 사용 하면 접근성 및 항목 키 집합에 대 한 인증을 설정할 수 있습니다. 아래 다이어그램에서는 키 집합 항목의 나머지 부분을 사용 하 여이 새 특성을 연결 하는 방법을 보여 줍니다.

[![](touchid-images/image2.png "이 다이어그램에서는 키 집합 항목의 나머지 부분을 사용 하 여이 새 특성을 연결 하는 방법을 보여 줍니다.")](touchid-images/image2.png#lightbox)

IOS 8 기준으로 이제 새 사용자 현재 상태 정책이 되어, `SecAccessControl`는 iPhone 5 초 이상 보안 enclave에 의해 적용 됩니다. 방금 어떻게 장치 구성을 영향을 줄 수 정책 평가 아래 표에서 볼 수 있습니다.

|장치 구성|정책 평가|백업 메커니즘|
|--- |--- |--- |
|암호 없이 장치|액세스 권한 없음|없음|
|암호를 사용 하 여 장치|암호 필요|없음|
|Touch ID 사용 하 여 장치|Touch ID를 선호합니다.|암호를 허용합니다.|

보안 Enclave 내 모든 작업에 서로 신뢰할 수 있습니다. 즉, Touch ID 인증 결과 키 집합 항목 암호 해독 권한을 부여 하 사용할 수 있습니다. 또한 보안 Enclave 실패 한 Touch ID 일치 하는 경우 사용자는 암호를 사용 하 여 되돌려야 하는 카운터를 유지 합니다.
호출 iOS 8의에서 새로운 프레임 _Local Authentication_, 인증 된 장치 내에서이 프로세스를 지원 합니다. 다음 섹션에서 살펴보겠습니다.

## <a name="local-authentication"></a>로컬 인증

이전 섹션에서 설정 했습니다 응용 프로그램 로컬 인증 장치에서 설정 된 보안 정책 사용 하 여 준수에서 사용자를 인증에 사용할 수 있습니다.

현재 API 두 기능을 제공 합니다. 첫째, 새 키 집합 액세스 제어 목록 (Acl)를 사용 하 여 기존 키 집합 서비스를 지원 합니다. 키 집합 데이터는 사용자가 지문을 성공적인 인증을 사용 하 여 잠금 수 있습니다.

둘째, LocalAuthentication 로컬로 응용 프로그램을 인증 하는 두 가지 방법 제공 합니다. 개발자 사용할지 `CanEvaluatePolicy` Touch ID를 받아들일 수 있는 장치 인지 확인 하 고 `EvaluatePolicy` 인증 작업을 시작 합니다.

로컬 인증을 제공 하는 두 기능을 하는 동안 원격 서버에 인증 하는 응용 프로그램 또는 사용자에 대 한 메커니즘을 제공 하지 않습니다.
Local Authentication 인증을 위해 새 표준 사용자 인터페이스를 제공합니다. Touch ID의 경우이 아래 그림과 같이 두 개의 단추를 사용 하 여 경고 뷰입니다. [취소]를 인증 – 암호의 대체 방법을 사용 하 여 하나 이상의 단추입니다. 설정 해야 하는 사용자 지정 메시지를 이기도 합니다. Touch ID 인증이 필요한 사용자에 게 설명 하는 데 사용 하는 것이 좋습니다.

[![](touchid-images/image12.png "Touch ID 인증 경고")](touchid-images/image12.png#lightbox)

### <a name="with-keychain-services"></a>키 집합 서비스를 사용 하 여

키 집합 항목을가 하는 방법에 약간 앞서 살펴본이 암호를 확인 하려면 보안 enclave를 사용 하 여 해독 합니다. IOS 8, 로컬 인증 대체 (fallback) 메커니즘을 또는 암호의 구현을 제공 하는 액세스 제어 목록 기능을 함께 Touch ID 검증을 요청에 사용할 수 있습니다.
에서는 사용 해야 하는 ACL을 사용 하 여 `SecAccessControl` 정책 및 사용 하 여 장치 상태를 확인 한 다음 `SecAccessible.WhenPasscodeSetThisDeviceOnly` 또는 `SecAccessible.WhenUnlocked`합니다.

#### <a name="considerations-with-acl"></a>ACL 사용 시 고려 사항

여러 가지 방법으로 키 집합을 사용 하 여 ACL을 사용 하는 경우 염두 해야 했습니다 하 고 그 중 일부는 다음과 같습니다.

-   만 사용 하 여 포그라운드 응용 프로그램을 사용 하 여 키 집합 작업 호출이 실패 하는 백그라운드 스레드에서 호출 하는 경우.
-   추가 하 고 키 집합 항목을 업데이트 하는 인증이 필요할 수 있습니다.
-   키 집합에 여러 일치 하는 항목을 반환 하는 요청을 인증 해야 합니다.
-   보호 된 항목 ACL 장치 전용 및 따라서 동기화 않거나 백업 됩니다.

### <a name="using-local-authentication-without-keychain-services"></a>키 집합 서비스 없이 로컬 인증을 사용 하 여

로컬 인증 암호 또는 터치 ID에 같은 자격 증명을 수집 하 고 사용자 인증을 완료 하려면 보안 Enclave를 사용 하는 방법으로 만들었습니다. 응용 프로그램 및 서로 직접 통신할 수 있는 보안 Enclave 사이의 연결 고리로 생각 합니다. 응용 프로그램에 대 한 정책 평가 위한 데도 수 있습니다.

이 응용 프로그램을 위해 보안 Enclave 내에서 작업을 시작 하는 로컬 인증 내에서 정책 평가 호출 합니다. 직접 쿼리/에 액세스 하지 않고 보안 Enclave를 앱에 인증을 제공 하기 위해 활용할 수 있습니다.

[![](touchid-images/image13a.png "키 집합 서비스 없이 로컬 인증을 사용 하 여")](touchid-images/image13a.png#lightbox)

예를 들어 개인에 대 한 도움이 자녀 또는 은행 응용 프로그램 같은 장치 소유자의 눈에만 기능을 잠금 해제 하려면 사용자가 확인을 구현 하는 간단한 방법을 제공 응용 프로그램에서 로컬 인증 사용 응용 프로그램입니다. 이미 존재 하는 인증을 확장 하는 방법으로도 사용할 수 있습니다 – 사용자가 해당 정보를 안전한 비슷하지만 옵션이 하려는 인.

로컬 인증의 보안 키 집합의 다릅니다. 예를 들어, 키 집합을 사용 하는 경우 운영 체제와 보안 Enclave 간에 트러스트가 됩니다. 로컬 인증을 사용 하 여 응용 프로그램과 운영 체제 보안 Enclave를 하지 보안 Enclave 자체의 결과에 대 한 액세스 권한만 있는지 즉 것입니다.

보안 주제에 대 것도 매우 중요 한지 알고 **액세스할 수 없는** 등록된 손가락 또는 지문 이미지입니다. 보안 Enclave이 정보의 소유자 이며 따라서 없는 다른 시스템 구성 요소에 액세스할 수 합니다.

로컬 인증 API를 활용 하 여 키 집합 없이 Touch ID를 사용 하려면 사용할 수 있는 몇 가지 함수가 있습니다. 이러한 아래에서 자세히 설명 합니다.

*   `CanEvaluatePolicy` –이 확인 하는 장치를 터치 ID를 받아들일 수 있는 경우
*   `EvaluatePolicy` 인증 작업을 시작 – 및 UI를 표시 하 고 반환 된 `true` 또는 `false` 응답 합니다.
*   `DeviceOwnerAuthenticationWithBiometrics` – Touch ID 화면 표시를 사용할 수 있는 정책입니다. 암호 대체 메커니즘이 여기에 대신 Touch ID 인증을 건너뛸 수 있도록 응용 프로그램에서이 대체 (fallback이)를 구현 해야 주목할 가치가 있습니다.

아래 나열 되어 있는 로컬 인증을 사용 하 여 몇 가지 주의 사항이 있습니다.

*   Keychain에 마찬가지로 실행할 수 있습니다 포그라운드에서 합니다. 백그라운드 스레드에서 호출 하면 실패 합니다.
*   정책 평가 실패할 수 있는 점을 염두에 두십시오. 암호 단추를 대신 구현 해야 합니다.
*   제공 해야 합니다는 `localizedReason` 인증이 필요한 이유를 설명 합니다. 이렇게 하면 사용자와 신뢰를 구축 합니다.

다음으로, 아래 섹션에서 살펴보겠습니다 다음이 사항을 고려 하는 API를 구현 하는 방법입니다.

## <a name="adding-touch-id-to-your-application"></a>응용 프로그램에 Touch ID 추가

이전 섹션에서 액세스 및 인증 키 집합 및 로컬 인증을 사용 하 여 이론에 살펴보았습니다. 이제 살펴보겠습니다를 어떻게 응용 프로그램에서 Touch ID를 통합할 수 있습니다.

### <a name="walkthrough"></a>연습

응용 프로그램에 일부 Touch ID 인증을 추가 살펴보도록 하겠습니다. 이 연습에서는 업데이트 하려고 합니다 [스토리 보드 테이블](https://developer.xamarin.com/samples/StoryboardTable/) 샘플에서는 다음과 같이 작동 하도록 로컬 인증 추가 합니다 [스토리 보드 테이블 – 로컬 인증](https://developer.xamarin.com/samples/monotouch/StoryboardTable_LocalAuthentication/) 만 허용 하는 샘플 작업 목록에 추가할 사용자를 인증 합니다.

1. 샘플을 다운로드 하 고 mac 용 Visual Studio에서 실행
2. 두 번 클릭 `MainStoryboard.Storyboard` 를 iOS 디자이너에서에서 샘플을 엽니다. 이 샘플을 사용 하 여 인증을 제어 하는 응용 프로그램을 새 화면을 추가 하려고 합니다. 현재 이동이 `MasterViewController`합니다.
3. 새 끌어 **뷰 컨트롤러** 에서 **도구 상자** 에 **디자인 화면**합니다. 로 설정 합니다 **루트 뷰 컨트롤러** 하 여 **Ctrl + 드래그** 에서 합니다 **탐색 컨트롤러**:

    [![](touchid-images/image4.png "루트 뷰 컨트롤러를 설정 합니다.")](touchid-images/image4.png#lightbox)
4.  새 뷰 컨트롤러 이름을 `AuthenticationViewController`입니다.
5. 다음으로 단추를 끌어서에 배치 된 `AuthenticationViewController`합니다. 이 메서드를 호출 `AuthenticateButton`에 텍스트를 제공 하 고 `Add a Chore`입니다.
6. 이벤트를 만들 합니다 `AuthenticateButton` 호출 `AuthenticateMe`합니다.
7. 수동 만들기에서 segue `AuthenticationViewController` 아래쪽에 있는 검은색 표시줄을 클릭 하 여 및 **Ctrl + 드래그** 표시줄에서 합니다 `MasterViewController` 선택 하 고 **푸시** (또는 **표시** size 클래스 사용):

    [![](touchid-images/image5.png "표시줄에서를 MasterViewController 푸시를 선택 하 고 끌거나 표시")](touchid-images/image6.png#lightbox)
8. 클릭 새로 만든 segue 및 식별자를 제공 `AuthenticationSegue`아래 그림과 같이:

    [![](touchid-images/image7.png "AuthenticationSegue segue 식별자로")](touchid-images/image7.png#lightbox)
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

로컬 인증을 사용 하 여 Touch ID 인증을 구현 해야 하는 모든 코드입니다. 아래 이미지에서 강조 표시 된 줄은 로컬 인증 사용을 보여 줍니다.

[![](touchid-images/image8.png "강조 표시 된 줄은 로컬 인증의 사용을 보여 줍니다.")](touchid-images/image8.png#lightbox)

먼저, 장치가 Touch ID를 사용 하 여 입력을 받아들일 수 있는 경우 설정 해야 합니다 `CanEvaluatePolicy` 정책에 전달 하 고 `DeviceOwnerAuthenticationWithBiometrics`입니다. 이 true 이면 Touch ID UI를 사용 하 여 표시할 수 있습니다 `EvaluatePolicy`합니다. 세 가지 정보를 전달 해야 하며 가지 `EvaluatePolicy` – 자체 정책, 인증이 필요한 이유를 설명 하는 문자열 및 회신 처리기입니다. 응답 처리기 수행할 성공 또는 실패 하면 인증의 경우 응용 프로그램에 알려줍니다. 응답 처리기를 자세히 살펴보겠습니다.

[![](touchid-images/image9.png "응답 처리기")](touchid-images/image9.png#lightbox)

응답 처리기 형식 지정 `LAContextReplyHandler`, 매개 변수 성공-를 사용 하는 `bool` 값 및 `NSError` 호출 `error`합니다. 성공한 경우 이것이 있는 것을 실제로 수행 무엇이 든 해당 인증 – 하고자이 경우 새 작업을 추가 해는 화면을 표시 합니다. 전경에서 실행 되 고 사용 해야 해야 하는 로컬 인증의 제한 사항 중 `InvokeOnMainThread`:

[![](touchid-images/image10.png "로컬 인증을 위해 사용 하 여 InvokeOnMainThread")](touchid-images/image10.png#lightbox)

마지막으로 인증에 성공 했을 때 원하는 전환의 `MasterViewController`합니다. `PerformSegue` 이렇게 하려면 메서드를 사용할 수 있습니다.

[![](touchid-images/image11.png "MasterViewController 전환할 PerformSegue 메서드 호출")](touchid-images/image11.png#lightbox)

## <a name="summary"></a>요약
이 가이드의 키 집합 및 iOS에서 작동 하는 방법을 살펴보았습니다. ACL에 키 집합에도 살펴보았습니다 및 iOS에서이를 변경 합니다. 다음으로, iOS 8의에서 새로운 응용 프로그램에서 Touch ID 인증을 구현할 살펴보고 있으며 로컬 인증 프레임 워크에서 확인을 했습니다.

## <a name="related-links"></a>관련 링크

- [테이블 – 로컬 인증 스토리 보 딩](https://developer.xamarin.com/samples/monotouch/StoryboardTable_LocalAuthentication/) 
- [키 집합 WWDC 샘플](https://developer.xamarin.com/samples/KeychainTouchID/)
- [키 집합 (샘플)](https://developer.xamarin.com/samples/Keychain/)
