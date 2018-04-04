---
title: Touch ID
description: 터치 ID에는 Apple의 지문 생체 인식 인증 기술입니다.
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 6ec46a5e098ba14925102211a27fcce8c27970e9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="touch-id"></a>Touch ID

Touch ID-암호 비슷한 사용자를 인증 하는 방법으로 iOS 7에서에서 도입 되었습니다. 그러나 장치를 잠금 해제, 앱 스토어를 사용 하 여 iTunes를 사용 하 여 및 iCloud 키 집합에만 인증 개로 제한 되었습니다.

로컬 인증 API를 사용 하 여 iOS 8 응용 프로그램에서 인증 메커니즘으로 Touch ID를 사용 하는 두 가지 됩니다. 현재 수 없으면 원격으로 인증할 수 로컬 인증을 사용 하도록 합니다.

Touch ID 및 해당 가치를 완벽 하 게 이해 하려면 살펴볼 키 집합 서비스 사용자의 데이터에 대 한 의미 이러한 새 변경 합니다. 키 집합 접근도 새로운 액세스 제어 목록 (Acl) 기능을 사용 하 여 iOS 8에에서 따라 확장 되었습니다.

## <a name="keychain--secure-enclave"></a>키 집합 및 보안 영토

키 집합은 하나의 개별 Apple id 암호, 키, 인증서 및 참고 사항에 대 한 보안 저장소를 제공 하는 대용량 데이터베이스 IOS 8에서에서 응용 프로그램는 항상 자체 고유 키 집합 항목에 액세스할 수 하 고 다른 응용 프로그램의 모든 키 집합 항목에 액세스할 수 없습니다. 이 키 집합을 사용 하는 키 집합 서비스 인식 응용 프로그램 키 집합은 단일 암호를 잠금 해제 되어 OS X에서 다릅니다. 이 문서에서 키 집합 iOS 8에서에서 작동 하는 방법에 살펴볼 것입니다.

키 집합은 각 행 이라고 하는 특수 한 데이터베이스는 _키 집합이_합니다. 각 항목 키 집합 특성으로 설명 하 고 암호화 된 값으로 구성 됩니다. 을 키 집합을 효율적으로 사용할 수 있도록 하려면 작은 항목을 위해 최적화 됩니다 또는 _비밀_합니다.
각 키 집합이 사용자 암호 및 고유 장치 암호로 보호 됩니다. 사용자가 자신의 장치를 사용 하지 않는 경우에 키 집합 항목을 보호 해야 합니다. 만 항목을 장치가 잠겨 있을 때 사용할 수 있게 허용 하 여 iOS에서 구현 되었습니다-장치가 잠겨 있는 있습니다 사용할 수 없습니다. 암호화 된 백업에 저장 될 수 있습니다. 액세스 제어를 적용 하는 키 집합의 주요 기능 중 하나 응용 프로그램에서 키 집합의 해당 부분에 액세스할 수 있고 다른 모든 응용 프로그램을 시작 하지 것입니다. 다음 다이어그램에서는 응용 프로그램 키 집합 상호 작용 하는 방법을 보여 줍니다.

[![](touchid-images/image1.png "이 다이어그램에서는 응용 프로그램 키 집합 상호 작용 하는 방법을 보여 줍니다.")](touchid-images/image1.png#lightbox)

### <a name="secure-enclave"></a>보안 영토

키 집합 자체; 키 집합이 해독할 수 없음 수행 하는 대신는 *Secure 영토*합니다. 보안 영토 A7 칩 지문 데이터 등록 된 인쇄에 대해 Touch ID 센서에서 성공적으로 일치 하는 결정을 내에서 보조 프로세서입니다. 그런 다음 키 집합이 해독 되며 키 집합에 암호 해독 된 암호를 반환 합니다.

### <a name="working-with-keychain"></a>키 집합 사용

먼저 응용 프로그램 암호가 있는지 확인 키 집합에 쿼리해야 합니다. 존재 하지 않는 경우 메시지를 표시 하려면 암호는 사용자 요청 계속 되지 않습니다. 새 암호를 묻는 메시지 암호를 업데이트 하는 경우에 키 집합에 업데이트 된 값에 전달 합니다.

> [!NOTE]
> 키 집합에서 검색 되는 암호를 사용 하 여 메모리에서 데이터에 대 한 모든 참조를 제거 해야 합니다. 전역 변수를 할당 하지 않습니다.

## <a name="keychain-acl-and-touch-id"></a>키 집합 ACL과 터치 ID

액세스 제어 목록에는 특정 작업이 발생 되도록 허용 하기 위해 어떤 발생 해야 하는지에 대 한 정보를 설명 하는 iOS 8에에서는 새 키 집합 항목 속성입니다. 이 경고 대화 상자를 표시 하거나 암호를 요청의 형태로 수 있습니다. ACL을 사용 하면의 내게 필요한 옵션 및 인증 키 항목을 설정할 수 있습니다. 아래 다이어그램에서는 키 집합 항목의 나머지 부분을 사용 하 여이 새 특성을 연결 하는 방법을 보여 줍니다.

[![](touchid-images/image2.png "이 다이어그램에서는 키 집합 항목의 나머지 부분을 사용 하 여이 새 특성을 연결 하는 방법을 보여 줍니다.")](touchid-images/image2.png#lightbox)

현재 iOS 8 있습니다는 이제 새 사용자 상태 정책 `SecAccessControl`, 하는 iPhone 5 이상에서 보안 영토에 의해 적용 됩니다. 방금 어떻게 장치 구성 수에 영향을 줄 정책 평가 아래 테이블에서 볼 수 있습니다.

|장치 구성|정책 평가|백업 메커니즘|
|--- |--- |--- |
|암호 없이 장치|권한 없음|없음|
|암호와 함께 장치|암호 필요|없음|
|Touch ID 사용 하 여 장치|Touch ID를 선호합니다.|암호를 허용합니다.|

보안 영토 내 모든 작업에 서로 신뢰할 수 있습니다. 즉, 키 집합 항목 암호 해독 권한을 부여 하려면 Touch ID 인증 결과 사용할 수 있습니다. 또한 Secure 영토 있는 경우 사용자는으로 되돌려야 할 암호를 사용 하 여 실패 한 Touch ID 검색 작업의 카운터를 유지 합니다.
새로운 프레임 워크 8, iOS에서 호출 _로컬 인증_, 장치 내에서 인증 하는이 과정을 지원 합니다. 다음 섹션에서 살펴보겠습니다.

## <a name="local-authentication"></a>로컬 인증

이전 섹션에서 설정, 응용 프로그램 로컬 인증 준수 장치에 설정 된 보안 정책 사용 하 여 사용자를 인증할 수 사용할 수 있습니다.

API는 두 개의 기능을 제공 하는 현재: 첫째, 새로운 키 집합 액세스 제어 목록 (Acl)를 사용 하 여 기존 키 집합 서비스를 지원 합니다. 키 집합 데이터는 사용자가 지문의 성공적인 인증을 사용 하 여 잠금 수 있습니다.

둘째, LocalAuthentication 응용 프로그램을 로컬로 인증 하는 두 가지 방법을 제공 합니다. 사용 해야 `CanEvaluatePolicy` Touch ID를 받아들일 수 있는 장치 인지 확인 하려면 다음 `EvaluatePolicy` 인증 작업을 시작 합니다.

기능이 모두 로컬 인증을 제공 하는 동안 응용 프로그램이 나 사용자는 원격 서버를 인증 하는 메커니즘을 제공 하지 않습니다.
로컬 인증 인증에 대 한 새 표준 사용자 인터페이스를 제공합니다. Touch ID의 경우 아래 그림과 같이 두 개의 단추가 포함 된 경고 보기 이것이입니다. 인증-암호의 대체 방법을 사용 하 여 취소를와 한 개의 단추입니다. 설정 해야 하는 사용자 지정 메시지 이기도 합니다. Touch ID 인증이 필요한 사용자에 게 설명 하는 데 사용 하는 것이 좋습니다.

[![](touchid-images/image12.png "Touch ID 인증 경고")](touchid-images/image12.png#lightbox)

### <a name="with-keychain-services"></a>키 집합 서비스

살펴보았습니다 약간 앞서 키 집합이 어떤가요 우리의 암호를 확인 하려면 보안 영토를 사용 하 여 해독 합니다. IOS 8, 암호 또는 대체 메커니즘의 구현을 제공 하는 액세스 제어 목록 기능을 함께 Touch ID 확인을 요청 하려면 로컬 인증을 사용할 수 있습니다.
에서는 사용 해야 하는 ACL을 사용 하는 `SecAccessControl` 정책 및 사용 하 여 장치 상태를 확인 하는 다음 `SecAccessible.WhenPasscodeSetThisDeviceOnly` 또는 `SecAccessible.WhenUnlocked`합니다.

#### <a name="considerations-with-acl"></a>ACL로 고려 사항

여러 가지 방법으로 키 집합으로 ACL을 사용 하는 경우 유념 해야 म 및이 중 일부는 다음과 같습니다.

-   사용할 포그라운드 응용 프로그램 – 호출이 실패 하는 백그라운드 스레드에서 모든 키 집합 작업을 호출 하는 경우.
-   추가 하 고 키 집합 항목을 업데이트 하는 인증이 필요할 수 있습니다.
-   키 집합에 여러 일치 하는 항목을 반환 하는 요청을 인증 해야 합니다.
-   ACL 항목을 보호 되는 장치 전용 및 따라서 not 동기화 또는 백업 합니다.

### <a name="using-local-authentication-without-keychain-services"></a>서비스 키 집합 없이 로컬 인증을 사용 하 여

로컬 인증 암호 또는 터치 ID에 같은 자격 증명을 수집 하 고 보안 영토 사용자 인증을 완료 하는 데 사용 하는 방법으로 만들었습니다. 응용 프로그램 및 서로 직접 통신할 수 있는 보안 영토 사이의 연결 고리로 생각 합니다. 또한 응용 프로그램에 대 한 정책 평가에 사용할 수 있습니다.

이 응용 프로그램을 수행 하려면 보안 영토 내부 작업을 시작 하는 로컬 인증 내부 정책 평가 호출 합니다. 직접 액세스 하지 않고 쿼리/Secure 영토 응용 프로그램에 인증을 제공 하려면이 옵션을 활용할 수 있습니다.

[![](touchid-images/image13a.png "서비스 키 집합 없이 로컬 인증을 사용 하 여")](touchid-images/image13a.png#lightbox)

예: 장치 소유자는 은행 응용 프로그램 같은 또는 자녀 도움이 될 보호에 눈에 대해서만 기능은 개인에 대 한 잠금을 해제 하려면 사용자가 확인을 구현 하는 간단한 방법을 제공 응용 프로그램에서 로컬 인증을 사용 하 여 응용 프로그램입니다. 이미 존재 하는 인증을 확장 하는 방법으로도 사용할 수 있습니다-사용자 같은 보안 정보 했으나도 옵션이 있습니다.

로컬 인증 보안 키 집합은와 다릅니다. 예를 들어, 키 집합을 사용할 경우 트러스트 운영 체제와 Secure 영토 사이입니다. 로컬 인증을 사용 하면 응용 프로그램에서을 의미 하지는 Secure 영토 자체 보안 영토의 결과에 액세스할 수 있다고 운영 체제 사이입니다.

보안 주제의 이기도 있다는 것을 매우 중요 **에 액세스할 수 없는** 등록된 손가락 또는 지문 이미지입니다. 보안 영토는 이러한 정보의 소유자가 고 다른 시스템 구성 요소가 없으면에 액세스할 수 있으므로 합니다.

로컬 인증 API를 활용 하 여 키 집합 없이 Touch ID를 사용 하려면 사용할 수 있는 몇 가지 함수도 있습니다. 이 아래에서 자세히:

*   `CanEvaluatePolicy` -단순히 장치가 터치 ID를 수용할 수 있음을 의미 인지를 확인 합니다 이것
*   `EvaluatePolicy` -인증 작업을 시작이 고, UI를 표시 하 고 반환 된 `true` 또는 `false` 응답 합니다.
*   `DeviceOwnerAuthenticationWithBiometrics` – Touch ID 화면 표시를 사용할 수 있는 정책입니다. 이므로 암호 대체 메커니즘이 여기, 대신 사용자가을 Touch ID 인증을 건너뛸 수 있도록 응용 프로그램에서이 대체 (fallback이)를 구현 해야 합니다.

아래 나열 되어 있는 로컬 인증을 사용 하 여 몇 가지 제한이 있습니다.

*   키 집합와 마찬가지로 것만 실행할 수 있습니다 포그라운드에서. 백그라운드 스레드에서 호출 하면 실패 합니다.
*   정책 평가 오류가 발생할 수 있는 점을 염두에 두십시오. 암호 단추를 대신 구현 해야 합니다.
*   제공 해야 합니다는 `localizedReason` 인증이 필요한 이유를 설명 합니다. 이렇게 하면 사용자와 신뢰 관계를 구축 합니다.

다음으로 아래 섹션에 살펴보도록 하겠습니다 고려 이러한 제한 사항을이 적용 하는 API를 구현 하는 방법입니다.

## <a name="adding-touch-id-to-your-application"></a>응용 프로그램에 Touch ID를 추가합니다.

이전 섹션에서 액세스 및 인증 키 집합 및 로컬 인증을 사용 하 여 이론에서 찾았습니다. 이제 응용 프로그램에 Touch ID를 통합 하는 방법을 살펴보겠습니다 해당 메뉴로 이동 합니다.

### <a name="walkthrough"></a>연습

따라서 살펴보겠습니다 Touch ID 인증 일부 응용 프로그램에 추가 합니다. 이 연습에서 하겠습니다 사용 하 여 [스토리 보드 테이블](https://developer.xamarin.com/samples/StoryboardTable/) 샘플. 장치 소유자는이 목록에 추가할 수 있습니다만 않으려는 모든 항목을 추가 하도록 하 여 불필요 하 고 있는지 확인 하려고!

1.  이 샘플을 다운로드 하 고 Mac. 용 Visual Studio 실행
2.  두 번 클릭 하면 `MainStoryboard.Storyboard` 를 iOS 디자이너에서에서이 샘플을 엽니다. 이 샘플에는 인증을 제어 하는 샘플 응용 프로그램에서는 새 화면 추가 하려고 합니다. 이것은 현재 이동 `MasterViewController`합니다.
3.  새 끌어 **뷰-컨트롤러** 에서 **도구 상자** 에 **디자인 화면**합니다. 설정으로이 **뷰-컨트롤러 루트** 여 **Ctrl + 드래그** 에서 **탐색 컨트롤러**:

    [![](touchid-images/image4.png "루트 뷰 컨트롤러 설정")](touchid-images/image4.png#lightbox)
4.  새 보기 컨트롤러 이름을 `AuthenticationViewController`합니다.
5.  그런 다음 단추를 끌어에 배치 하는 `AuthenticationViewController`합니다. 이 메서드를 호출 `AuthenticateButton`, 텍스트를 지정 하 고 `Add a Chore`합니다.
6.  에 이벤트를 만들려면는 `AuthenticateButton` 호출 `AuthenticateMe`합니다.
7.  설명서와 함께 만들기에서 segue `AuthenticationViewController` 아래쪽의 검은색 표시줄을 클릭 하 여 및 **Ctrl + 드래그** 표시줄에서는 `MasterViewController` 선택한 **푸시** (또는 **표시** 크기 클래스 사용):

    [![](touchid-images/image5.png "도구 모음에서 푸시를 선택 하 고 MasterViewController 끌거나 표시")](touchid-images/image6.png#lightbox)
8.  클릭는 새로 만든 segue 하 고 식별자 `AuthenticationSegue`아래 그림과 같이,:

    [![](touchid-images/image7.png "AuthenticationSegue를 segue 식별자를 설정 합니다.")](touchid-images/image7.png#lightbox)
9.  다음 코드를 `AuthenticationViewController`에 추가합니다.

    ```
    partial void AuthenticateMe (UIButton sender)
        {
            var context = new LAContext();
            NSError AuthError;
            var myReason = new NSString("To add a new chore");


            if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError)){
                var replyHandler = new LAContextReplyHandler((success, error) => {

                    this.InvokeOnMainThread(()=>{
                        if(success){
                            Console.WriteLine("You logged in!");
                            PerformSegue("AuthenticationSegue", this);
                        }
                        else{
                            //Show fallback mechanism here
                        }
                    });

                });
                context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, myReason, replyHandler);
            };
        }
    ```

Touch ID 인증 로컬 인증을 사용 하 여 구현에 필요한 모든 코드입니다. 아래 그림에 강조 표시 된 줄 로컬 인증의 사용을 보여 줍니다.

[![](touchid-images/image8.png "강조 표시 된 줄 로컬 인증의 사용을 보여 줍니다.")](touchid-images/image8.png#lightbox)

먼저 장치가 Touch ID를 사용 하 여 입력을 받아들일 수 있는 인지를 설정 하는 `CanEvaluatePolicy` 정책에 전달 하 고 `DeviceOwnerAuthenticationWithBiometrics`합니다. 이 true 이면 Touch ID UI를 사용 하 여 표시할 수 `EvaluatePolicy`합니다. 세 가지로 전달 해야 하는 정보는 `EvaluatePolicy` – 정책 자체, 인증이 필요한 이유를 설명 하는 문자열 및 회신 처리기입니다. 회신 처리기 수행할 성공 또는 실패, 인증의 경우 응용 프로그램을 지시 합니다. 회신 처리기에서 자세히 살펴보겠습니다.

[![](touchid-images/image9.png "응답 처리기")](touchid-images/image9.png#lightbox)

회신 처리기 형식의 지정 `LAContextReplyHandler`, 매개 변수 성공-를 사용 하는 `bool` 값 및 `NSError` 호출 `error`합니다. 성공한 경우이 여기서 실제로 수행 합니다 무엇이 든 – 인증 하려는 경우 새 작업을 추가 해는 화면을 표시 합니다. 로컬 인증의 제한 사항 중 하나는 전경에서 실행을 사용 해야 커야 함을 `InvokeOnMainThread`:

[![](touchid-images/image10.png "로컬 인증에 대 한 InvokeOnMainThread를 사용 하 여")](touchid-images/image10.png#lightbox)

마지막으로, 인증에 성공한 경우 원하는 전환의 `MasterViewController`합니다. `PerformSegue` 이 메서드를 사용할 수 있습니다.

[![](touchid-images/image11.png "전환에 MasterViewController PerformSegue 메서드 호출")](touchid-images/image11.png#lightbox)

## <a name="summary"></a>요약
이 가이드의 키 집합 및 iOS에서 작동 원리를 살펴보았습니다. 또한 ACL, 키 집합에서 살펴본 및 iOS에이 변경 합니다. 다음으로 iOS 8의에서 새로운 기능 이므로 Touch ID 인증 응용 프로그램의 구현에 검토 한 다음 로컬 인증 프레임 워크를 확인을 했습니다.

## <a name="related-links"></a>관련 링크

- [Touch ID 샘플](https://developer.xamarin.com/samples/StoryboardTable_LocalAuthentication)
- [키 집합 WWDC 샘플](https://developer.xamarin.com/samples/KeychainTouchID/)
- [키 집합 (샘플)](https://developer.xamarin.com/samples/Keychain/)
