---
title: Xamarin.ios의 핸드 오프
description: 이 문서에서는 사용자의 다른 장치에서 실행 되는 앱 간에 사용자 활동을 전송 하기 위해 Xamarin.ios 앱에서의 핸드 오프 작업을 설명 합니다.
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: 9fa0d51e02382458535b065377af55542d87913a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290752"
---
# <a name="handoff-in-xamarinios"></a>Xamarin.ios의 핸드 오프

_이 문서에서는 사용자의 다른 장치에서 실행 되는 앱 간에 사용자 활동을 전송 하기 위해 Xamarin.ios 앱에서의 핸드 오프 작업을 설명 합니다._

Apple은 iOS 8 및 OS X Yosemite (10.10)에 전달 하 여 사용자가 장치 중 하나에서 시작 된 활동을 동일한 앱을 실행 하는 다른 장치 또는 동일한 활동을 지 원하는 다른 앱으로 전송 하는 일반적인 메커니즘을 제공 합니다.

[![](handoff-images/handoff02.png "핸드 오프 작업을 수행 하는 예제")](handoff-images/handoff02.png#lightbox)

이 문서에서는 Xamarin.ios 앱에서 활동 공유를 사용 하도록 설정 하는 방법에 대해 간략히 살펴보고 핸드 오프 프레임 워크를 자세히 다룹니다.

## <a name="about-handoff"></a>핸드 오프 정보

사용자가 장치 (iOS 또는 Mac) 중 하나에서 활동을 시작 하 고 사용자의 iClou로 식별 된 다른 장치에서 동일한 활동을 계속 진행 하는 방식으로 iOS 8 및 OS X Yosemite (10.10)에서 Apple에 의해 전달 (연속성이 라고도 함)이 도입 되었습니다. d 계정).

핸드 오프는 새로운 향상 된 검색 기능을 지원 하기 위해 iOS 9에서 확장 되었습니다. 자세한 내용은 [향상 된 검색 기능](~/ios/platform/search/index.md) 설명서를 참조 하세요.

예를 들어 사용자는 iPhone에서 전자 메일을 시작 하 고 Mac에서 전자 메일을 원활 하 게 계속할 수 있습니다. 여기에는 동일한 메시지 정보와 iOS에서 남겨진 동일한 위치에 커서가 있습니다.

동일한 _팀 ID_ 를 공유 하는 앱은 해당 앱이 ITunes 앱 스토어를 통해 제공 되거나 등록 된 개발자 (Mac, 엔터프라이즈 또는 임시 앱 용)에서 서명 된 경우에만 앱 전체에서 사용자 활동을 계속 하는 데 사용할 수 있습니다.

모든 `NSDocument` 또는`UIDocument` 기반 앱은 자동으로 전달 지원을 기본 제공 하며, 전달 기능을 지원 하기 위해 최소한의 변경만 필요 합니다.

### <a name="continuing-user-activities"></a>사용자 활동 계속

클래스 `NSUserActivity` ( `UIKit` 및 의일부작은변경내용포함)는사용자의다른장치에서잠재적으로계속될수있는사용자활동을정의하는기능을제공합니다.`AppKit`

활동을 사용자의 다른 장치에 전달 하려면 _현재 활동_으로 표시 되 고 페이로드 집합 (연속 작업을 수행 `NSUserActivity`하는 데 사용 되는 데이터)이 있어야 하 고 활동을 수행 해야 하는 인스턴스에 캡슐화 해야 합니다. 해당 장치에 전송 됩니다.

전달은 작업을 계속 하도록 정의 하기 위해 최소 정보를 전달 하 고, iCloud를 통해 더 큰 데이터 패킷을 동기화 합니다.

수신 장치에서 사용자는 활동을 연속으로 사용할 수 있다는 알림을 받게 됩니다. 사용자가 새 장치에서 작업을 계속 하도록 선택 하면 지정 된 앱이 시작 되 고 (아직 실행 중이 아닌 경우)의 `NSUserActivity` 페이로드가 활동을 다시 시작 하는 데 사용 됩니다.

[![](handoff-images/handoffinteractions.png "사용자 활동을 계속 하는 개요")](handoff-images/handoffinteractions.png#lightbox)

동일한 개발자 팀 ID를 공유 하 고 지정 된 _활동 유형에_ 응답 하는 앱만 연속으로 사용할 수 있습니다. 앱은 `NSUserActivityTypes` **info.plist** 파일의 키 아래에서 지 원하는 활동 유형을 정의 합니다. 이 경우 계속 해 서 장치는 팀 ID, 활동 유형 및 _작업 제목_(옵션)에 따라 연속 작업을 수행 하도록 앱을 선택 합니다.

수신 앱은의 `NSUserActivity` `UserInfo` 사전에 있는 정보를 사용 하 여 사용자 인터페이스를 구성 하 고, 지정 된 활동의 상태를 복원 하 여 최종 사용자에 게 원활한 전환이 표시 되도록 합니다.

연속 작업에를 `NSUserActivity`통해 효율적으로 보낼 수 있는 것 보다 많은 정보가 필요한 경우 다시 시작 하는 앱은 원래 앱에 대 한 호출을 보내고 필요한 데이터를 전송 하는 스트림을 하나 이상 설정할 수 있습니다. 예를 들어 작업이 여러 이미지를 포함 하는 많은 텍스트 문서를 편집 하는 경우 수신 장치에서 작업을 계속 하는 데 필요한 정보를 전송 하려면 스트리밍이 필요 합니다. 자세한 내용은 아래의 [연속 스트림 지원](#supporting-continuation-streams) 섹션을 참조 하세요.

위에서 `NSDocument` 설명한 것 처럼 또는 `UIDocument` 기반 앱은 자동으로 전달 지원을 기본 제공 합니다. 자세한 내용은 아래의 [문서 기반 앱에서 전달 지원](#supporting-handoff-in-document-based-apps) 섹션을 참조 하세요.

### <a name="the-nsuseractivity-class"></a>NSUserActivity 클래스

클래스 `NSUserActivity` 는 전달 교환의 주 개체 이며 연속에 사용할 수 있는 사용자 활동의 상태를 캡슐화 하는 데 사용 됩니다. 앱은 지원 되는 모든 활동 `NSUserActivity` 에 대 한의 복사본을 인스턴스화하고 다른 장치에서 계속 진행 하려고 합니다. 예를 들어 문서 편집기는 현재 열려 있는 각 문서에 대 한 활동을 만듭니다. 그러나 맨 앞의 창이 나 탭에 표시 되는 맨 앞의 문서만 _현재 작업_ 이며 연속에 사용할 수 있는 줄임으로써.

인스턴스 `NSUserActivity` 는 해당 `ActivityType` 및 `Title` 속성으로 식별 됩니다. `UserInfo` 사전 속성은 활동의 상태에 대 한 정보를 전달 하는 데 사용 됩니다. 의 대리자 `NeedsSave` 를 통해 `true` 상태 정보를 지연 로드 하려는 경우 속성을로 설정 합니다. `NSUserActivity` 작업 상태 `AddUserInfoEntries` 를 유지 하기 위해 필요에 따라 메서드를 사용 `UserInfo` 하 여 다른 클라이언트의 새 데이터를 사전에 병합 합니다.

### <a name="the-nsuseractivitydelegate-class"></a>NSUserActivityDelegate 클래스

는 `NSUserActivityDelegate` `NSUserActivity`의 사전`UserInfo` 정보를 최신 상태로 유지 하 고 활동의 현재 상태와 동기화 하는 데 사용 됩니다. 시스템에서 업데이트할 활동의 정보 (예: 다른 장치에서 계속 하기 전에)가 필요한 경우 대리자의 메서드를 `UserActivityWillSave` 호출 합니다.

`UserActivityWillSave` 메서드를 구현 하 고 `NSUserActivity` (예: `UserInfo`, `Title`등)를 변경 하 여 현재 활동의 상태를 계속 반영 해야 합니다. 시스템에서 `UserActivityWillSave` 메서드 `NeedsSave` 를 호출 하면 플래그가 지워집니다. 활동의 데이터 속성을 수정할 경우를 `NeedsSave` `true` 다시로 설정 해야 합니다.

위에서 설명한 `UserActivityWillSave` 방법을 사용 하는 대신 선택적으로 사용자 활동을 자동 `AppKit` 으로 보유 `UIKit` 하거나 관리할 수 있습니다. 이렇게 하려면 응답자 개체의 `UserActivity` 속성을 설정 하 고 메서드를 `UpdateUserActivityState` 구현 합니다. 자세한 내용은 아래의 [응답자의 지원](#supporting-handoff-in-responders) 전달 섹션을 참조 하세요.

### <a name="app-framework-support"></a>앱 프레임 워크 지원

( `UIKit` IOS) 및 `AppKit` (OS X)는 `NSDocument`모두, 응답자 (`NSResponder``UIResponder`/) 및 `AppDelegate` 클래스에 대 한 기본 제공 지원을 제공 합니다. 각 OS는 더 약간 다르게 이송을 구현 하지만 기본 메커니즘과 Api는 동일 합니다.

#### <a name="user-activities-in-document-based-apps"></a>문서 기반 앱의 사용자 활동

문서 기반 iOS 및 OS X 앱은 자동으로 전달 지원을 기본 제공 합니다. 이 지원을 활성화 하려면 앱의 `NSUbiquitousDocumentUserActivityType` **info.plist** 파일에서 각 `CFBundleDocumentTypes` 항목에 대 한 키 및 값을 추가 해야 합니다.

이 키가 있는 `NSDocument` 경우 및 `UIDocument` 는 지정 된 유형의 `NSUserActivity` iCloud 기반 문서에 대 한 인스턴스를 자동으로 만듭니다. 앱에서 지 원하는 각 문서 형식에 대 한 활동 유형을 제공 해야 하며 여러 문서 유형은 동일한 활동 유형을 사용할 수 있습니다. 및 `NSDocument` `UserInfo` `FileURL` 는 모두`NSUserActivity` 의 속성을 해당 속성 값으로 자동으로 채웁니다. `UIDocument`

OS X에서는 문서 창이 `NSUserActivity` 주 창이 `AppKit` 될 때 응답자와 관련 하 여 관리 되는 및가 자동으로 현재 활동이 됩니다. IOS에서는에서 `UIKit`관리 `NSUserActivity` 하는 개체에 대해 메서드를 명시적 `BecomeCurrent` 으로 호출 하거나 앱이 포그라운드로 들어올 `UserActivity` `UIViewController` 때에 문서의 속성을 설정 해야 합니다.

`AppKit`는 OS X에서 `UserActivity` 이런 방식으로 만들어진 모든 속성을 자동으로 복원 합니다. 이는 메서드가를 `ContinueUserActivity` 반환 `false` 하거나 구현 되지 않은 경우에 발생 합니다. 이 경우 `OpenDocument` `NSDocumentController` 의 메서드를 사용 하 여 문서를 열고 메서드 호출을 `RestoreUserActivityState` 받습니다.

자세한 내용은 아래의 [문서 기반 앱에서 전달 지원](#supporting-handoff-in-document-based-apps) 섹션을 참조 하세요.

#### <a name="user-activities-and-responders"></a>사용자 활동 및 응답자

`UserActivity` 및 `UIKit` 는`AppKit` 모두 응답자 개체의 속성으로 설정 하는 경우 사용자 활동을 자동으로 관리할 수 있습니다. 상태가 수정 된 경우 `NeedsSave` `UserActivity` 응답자의 속성을로 `true`설정 해야 합니다. 필요한 경우 해당 `UpdateUserActivityState` 메서드를 호출 `UserActivity` 하 여 상태를 업데이트 하는 응답자 시간을 제공한 후 시스템에서 자동으로를 저장 합니다.

여러 응답 자가 단일 `NSUserActivity` 인스턴스를 공유 하는 경우 시스템에서 사용자 활동 개체를 업데이트할 때 `UpdateUserActivityState` 콜백을 받게 됩니다. 이 시점에서 현재 활동 상태 `AddUserInfoEntries` 를 반영 하도록 `NSUserActivity`의 `UserInfo` 사전을 업데이트 하려면 응답자는 메서드를 호출 해야 합니다. `UserInfo` 각`UpdateUserActivityState` 호출 전에 사전이 지워집니다.

활동에서 자신을 분리 하기 위해 응답자는 `UserActivity` 속성을로 `null`설정할 수 있습니다. 앱 프레임 워크 관리 되 `NSUserActivity` 는 인스턴스에 더 이상 연결 된 응답자 또는 문서가 없으면 자동으로 무효화 됩니다.

자세한 내용은 아래의 [응답자의 지원](#supporting-handoff-in-responders) 전달 섹션을 참조 하세요.

#### <a name="user-activities-and-the-appdelegate"></a>사용자 활동 및 AppDelegate

앱 `AppDelegate` 은 핸드 오프 연속을 처리할 때 기본 진입점입니다. 사용자가 핸드 오프 알림에 응답 하면 적절 한 앱이 시작 되 고 `WillContinueUserActivityWithType` (아직 실행 되지 않은 경우) `AppDelegate` 의 메서드가 호출 됩니다. 이 시점에서 앱은 연속 작업이 시작 되 고 있음을 사용자에 게 알려 주어 야 합니다.

의 메서드가 호출 되 `NSUserActivity` 면`ContinueUserActivity`인스턴스가전달됩니다. `AppDelegate` 이때 앱의 사용자 인터페이스를 구성 하 고 지정 된 작업을 계속 합니다.

자세한 내용은 아래의 [핸드 오프](#implementing-handoff) 전달 섹션을 참조 하세요.

## <a name="enabling-handoff-in-a-xamarin-app"></a>Xamarin 앱에서 핸드 오프 사용

전달에 의해 적용 되는 보안 요구 사항으로 인해 핸드 오프 프레임 워크를 사용 하는 Xamarin.ios 앱은 Apple Developer 포털과 Xamarin.ios 프로젝트 파일 모두에서 제대로 구성 해야 합니다.

다음을 수행합니다.

1. [Apple 개발자 포털](https://developer.apple.com)에 로그인 합니다.
2. **인증서, 식별자 & 프로필**을 클릭 합니다.
3. 아직 수행 하지 않은 경우 **식별자** 를 클릭 하 고 앱에 대 한 id (예: `com.company.appname`)를 만든 다음 기존 id를 편집 합니다.
4. 지정 된 ID에 대해 **iCloud** 서비스를 확인 했는지 확인 합니다.

    [![](handoff-images/provision01.png "지정 된 ID에 대해 iCloud 서비스를 사용 하도록 설정")](handoff-images/provision01.png#lightbox)
5. 변경 내용을 저장합니다.
6. 프로 **비전 프로필** > **개발** 을 클릭 하 고 앱에 대 한 새 개발 프로 비전 프로필을 만듭니다.

    [![](handoff-images/provision02.png "앱에 대 한 새 개발 프로 비전 프로필 만들기")](handoff-images/provision02.png#lightbox)
7. 새 프로 비전 프로필을 다운로드 하 고 설치 하거나 Xcode를 사용 하 여 프로필을 다운로드 하 고 설치 합니다.
8. Xamarin.ios 프로젝트 옵션을 편집 하 고 방금 만든 프로 비전 프로필을 사용 하 고 있는지 확인 합니다.

    [![](handoff-images/provision03.png "앞서 만든 프로 비전 프로필을 선택 합니다.")](handoff-images/provision03.png#lightbox)
9. 그런 다음 **info.plist** 파일을 편집 하 고 프로 비전 프로필을 만드는 데 사용 된 앱 ID를 사용 하 고 있는지 확인 합니다.

    [![](handoff-images/provision04.png "앱 ID 설정")](handoff-images/provision04.png#lightbox)
10. **배경 모드** 섹션으로 스크롤하고 다음 항목을 확인 합니다.

    [![](handoff-images/provision05.png "필요한 백그라운드 모드 사용")](handoff-images/provision05.png#lightbox)
11. 모든 파일의 변경 내용을 저장 합니다.

이러한 설정이 적용 되 면 응용 프로그램은 이제 전달 프레임 워크 Api에 액세스할 준비가 되었습니다. 프로 비전에 대 한 자세한 내용은 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 및 [앱 가이드 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 을 참조 하세요.

## <a name="implementing-handoff"></a>핸드 오프 구현

동일한 개발자 팀 ID로 서명 된 앱 간에 사용자 활동을 계속 하 고 동일한 활동 유형을 지원할 수 있습니다. Xamarin.ios 앱에 전달 기능을 구현 하려면 또는 `UIKit` `AppKit`에서 사용자 활동 개체를 만들고, 활동을 추적 하기 위해 개체의 상태를 업데이트 하 고, 수신 장치에서 활동을 계속 해야 합니다.

### <a name="identifying-user-activities"></a>사용자 활동 식별

핸드 오프를 구현 하는 첫 번째 단계는 앱이 지 원하는 사용자 활동의 유형을 식별 하 고 다른 장치에서 연속으로 사용할 수 있는 활동을 확인 하는 것입니다. 예: ToDo 앱은 한 _사용자 활동 유형_으로 항목 편집을 지원 하 고 사용 가능한 항목 목록을 다른 항목으로 탐색할 수 있습니다.

앱은 앱에서 제공 하는 모든 함수에 대해 필요한 만큼의 사용자 작업 유형을 만들 수 있습니다. 각 사용자 활동 유형에 대해 앱은 유형 활동의 시작 및 종료 시간을 추적 하 고, 다른 장치에서 해당 작업을 계속 하기 위해 최신 상태 정보를 유지 관리 해야 합니다.

사용자 활동은 보내고 받는 앱 간에 일대일 매핑 없이 동일한 팀 ID로 서명 된 모든 앱에서 계속할 수 있습니다. 예를 들어, 지정 된 앱은 다른 장치에서 다른 개별 앱이 사용 하는 네 가지 유형의 활동을 만들 수 있습니다. 이는 앱의 Mac 버전 (기능 및 기능이 다를 수 있음)과 iOS 앱 (각 앱이 작고 특정 작업에 집중 하는 iOS 앱) 사이에서 발생 하는 일반적인 작업입니다.

### <a name="creating-activity-type-identifiers"></a>활동 유형 식별자 만들기

_활동 유형 식별자_ 는 지정 된 사용자 활동 유형을 고유 하 `NSUserActivityTypes` 게 식별 하는 데 사용 되는 앱의 **info.plist** 파일 배열에 추가 된 짧은 문자열입니다. 앱에서 지 원하는 각 작업에 대 한 항목은 배열에 하나씩 있습니다. Apple은 충돌을 방지 하기 위해 활동 형식 식별자에 대 한 역방향 DNS 스타일 표기법을 사용 하는 것을 제안 합니다. 예를 들어 `com.company-name.appname.activity` 특정 앱 기반 활동의 경우 `com.company-name.activity` 또는 여러 앱에서 실행할 수 있는 활동의 경우입니다.

활동 유형 식별자는 활동 유형을 식별 하는 `NSUserActivity` 인스턴스를 만들 때 사용 됩니다. 활동이 다른 장치에서 계속 되 면 활동 유형 (앱의 팀 ID와 함께)은 활동을 계속 하기 위해 시작할 앱을 결정 합니다.

예를 들어 **Monkeybrowser** 라는 샘플 앱을 만들어 보겠습니다 ([여기에서 다운로드](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser)). 이 앱에는 웹 브라우저 보기에서 각각 다른 URL이 열려 있는 4 개의 탭이 표시 됩니다. 사용자는 앱을 실행 하는 다른 iOS 장치에서 탭을 계속 사용할 수 있습니다.

이 동작을 지원 하기 위해 필요한 활동 형식 식별자를 만들려면 **info.plist** 파일을 편집 하 고 **원본** 뷰로 전환 합니다. 키를 `NSUserActivityTypes` 추가 하 고 다음 식별자를 만듭니다.

[![](handoff-images/type01.png "Info.plist 편집기의 NSUserActivityTypes 키 및 필수 식별자")](handoff-images/type01.png#lightbox)

예제 **Monkeybrowser** 앱의 각 탭에 대해 하나씩 4 개의 새 활동 형식 식별자를 만들었습니다. 사용자 고유의 앱을 만들 때 `NSUserActivityTypes` 배열의 내용을 앱이 지 원하는 활동에 특정 한 활동 유형 식별자로 바꿉니다.

### <a name="tracking-user-activity-changes"></a>사용자 활동 변경 내용 추적

`NSUserActivity` 클래스의 새 인스턴스를 만들 때 활동의 상태에 대 한 변경 `NSUserActivityDelegate` 내용을 추적 하는 인스턴스를 지정 합니다. 예를 들어 다음 코드를 사용 하 여 상태 변경을 추적할 수 있습니다.

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;

namespace MonkeyBrowse
{
    public class UserActivityDelegate : NSUserActivityDelegate
    {
        #region Constructors
        public UserActivityDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void UserActivityReceivedData (NSUserActivity userActivity, NSInputStream inputStream, NSOutputStream outputStream)
        {
            // Log
            Console.WriteLine ("User Activity Received Data: {0}", userActivity.Title);
        }

        public override void UserActivityWasContinued (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity Was Continued: {0}", userActivity.Title);
        }

        public override void UserActivityWillSave (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity will be Saved: {0}", userActivity.Title);
        }
        #endregion
    }
}
```

연속 스트림이 송신 장치에서 데이터를 수신 하면 메서드가호출됩니다.`UserActivityReceivedData` 자세한 내용은 아래의 [연속 스트림 지원](#supporting-continuation-streams) 섹션을 참조 하세요.

메서드 `UserActivityWasContinued` 는 다른 장치가 현재 장치에서 작업을 수행할 때 호출 됩니다. 활동의 유형에 따라 할 일 목록에 새 항목을 추가 하는 것과 같은 앱이 보내는 장치에서 활동을 중단 해야 할 수 있습니다.

메서드 `UserActivityWillSave` 는 활동에 대 한 변경 내용이 저장 되 고 로컬에서 사용 가능한 장치에 동기화 되기 전에 호출 됩니다. 이 메서드를 사용 하 여 `UserInfo` `NSUserActivity` 인스턴스를 보내기 전에 인스턴스의 속성에 대 한 마지막 분 변경 작업을 수행할 수 있습니다.

### <a name="creating-a-nsuseractivity-instance"></a>NSUserActivity 인스턴스 만들기

앱에서 다른 장치를 계속 진행할 수 있는 가능성을 제공 하고자 하는 각 활동은 `NSUserActivity` 인스턴스에 캡슐화 되어야 합니다. 앱은 필요한 만큼의 작업을 만들 수 있으며 해당 활동의 특성은 해당 앱의 기능 및 기능에 따라 달라 집니다. 예를 들어 전자 메일 앱은 새 메시지를 만들기 위한 활동 하 나와 메시지를 읽는 다른 활동을 만들 수 있습니다.

예제 앱의 경우 사용자가 탭 `NSUserActivity` 웹 브라우저 보기 중 하나에 새 URL을 입력할 때마다 새이 만들어집니다. 다음 코드는 지정 된 탭의 상태를 저장 합니다.

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSUserActivity UserActivity { get; set; }
...

UserActivity = new NSUserActivity (UserActivityTab1);
UserActivity.Title = "Weather Tab";
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

위에서 만든 사용자 활동 `NSUserActivity` 유형 중 하나를 사용 하 여 새를 만들고 활동에 대해 사람이 읽을 수 있는 제목을 제공 합니다. 위의 `NSUserActivityDelegate` 만든 인스턴스에 연결 하 여 상태 변경을 감시 하 고이 사용자 활동이 현재 활동 임을 iOS에 알립니다.

### <a name="populating-the-userinfo-dictionary"></a>UserInfo 사전 채우기

위에서 `UserInfo` 설명한 것 처럼 `NSUserActivity` 클래스 `NSDictionary` 의 속성은 지정 된 작업의 상태를 정의 하는 데 사용 되는 키-값 쌍의입니다. `UserInfo` 에 저장 된 값은 ,`NSSet`,, ,,,,`NSNull` 또는`NSURL`형식 중 하나 여야 합니다. `NSDictionary` `NSDate` `NSData` `NSArray` `NSNumber` `NSString` `NSURL`iCloud 문서를 가리키는 데이터 값은 수신 장치에서 동일한 문서를 가리키도록 자동으로 조정 됩니다.

위의 예제에서는 개체를 `NSMutableDictionary` 만들고 사용자가 지정 된 탭에서 현재 보고 있는 URL을 제공 하는 단일 키로 채웁니다. 사용자 `AddUserInfoEntries` 활동의 메서드는 수신 장치에서 활동을 복원 하는 데 사용 되는 데이터로 활동을 업데이트 하는 데 사용 됩니다.

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple은 작업이 적시에 수신 장치에 전송 되도록 하기 위해 분위기 최소값으로 전송 되는 정보를 유지 하는 것을 제안 합니다. 문서를 편집 하는 데 연결 된 이미지를 보내야 하는 것과 같은 정보를 더 많이 필요로 하는 경우 연속 스트림을 사용 해야 합니다. 자세한 내용은 아래의 [연속 스트림 지원](#supporting-continuation-streams) 섹션을 참조 하세요.

### <a name="continuing-an-activity"></a>활동 계속

핸드 오프는 비연속적 사용자 활동의 가용성을 위해 원래 장치에 물리적으로 근접 하 고 동일한 iCloud 계정에 로그인 한 로컬 iOS 및 OS X 장치를 자동으로 알립니다. 사용자가 새 장치에서 작업을 계속 하도록 선택 하는 경우 시스템은 적절 한 앱 (팀 ID 및 활동 유형에 따라)을 시작 하 고 해당 연속 작업 `AppDelegate` 을 수행 해야 한다는 정보를 설정 합니다.

`WillContinueUserActivityWithType` 먼저 메서드가 호출 되어 사용자에 게 연속 작업이 시작 되는 것을 알릴 수 있습니다. 예제 앱의 **AppDelegate.cs** 파일에서 다음 코드를 사용 하 여 연속 시작을 처리 합니다.

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSString UserActivityTab2 = new NSString ("com.xamarin.monkeybrowser.tab2");
public NSString UserActivityTab3 = new NSString ("com.xamarin.monkeybrowser.tab3");
public NSString UserActivityTab4 = new NSString ("com.xamarin.monkeybrowser.tab4");
...

public FirstViewController Tab1 { get; set; }
public SecondViewController Tab2 { get; set;}
public ThirdViewController Tab3 { get; set; }
public FourthViewController Tab4 { get; set; }
...

public override bool WillContinueUserActivity (UIApplication application, string userActivityType)
{
    // Report Activity
    Console.WriteLine ("Will Continue Activity: {0}", userActivityType);

    // Take action based on the user activity type
    switch (userActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Inform view that it's going to be modified
        Tab1.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Inform view that it's going to be modified
        Tab2.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Inform view that it's going to be modified
        Tab3.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Inform view that it's going to be modified
        Tab4.PreparingToHandoff ();
        break;
    }

    // Inform system we handled this
    return true;
}
```

위의 예제에서 각 뷰 컨트롤러는에 `AppDelegate` 등록 하 고 활동 표시기를 표시 하는 공용 `PreparingToHandoff` 메서드와 활동을 현재 장치에 전달 하려고 한다는 사실을 사용자에 게 알리는 메시지를 포함 합니다. 예제:

```csharp
private void ShowBusy(string reason) {

    // Display reason
    BusyText.Text = reason;

    //Define Animation
    UIView.BeginAnimations("Show");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0.5f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PreparingToHandoff() {
    // Inform caller
    ShowBusy ("Continuing Activity...");
}
```

의는 `ContinueUserActivity`실제로지정 된작업을계속하기위해호출됩니다.`AppDelegate` 이번에는 앱 예제에서 다음을 수행 합니다.

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

각 뷰 `PerformHandoff` 컨트롤러의 공용 메서드는 실제로 전달를 미리 형성 하 고 현재 장치에서 활동을 복원 합니다. 예제의 경우 지정 된 탭에서 사용자가 다른 장치에서 검색 한 것과 동일한 URL을 표시 합니다. 예제:

```csharp
private void HideBusy() {

    //Define Animation
    UIView.BeginAnimations("Hide");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PerformHandoff(NSUserActivity activity) {

    // Hide busy indicator
    HideBusy ();

    // Extract URL from dictionary
    var url = activity.UserInfo ["Url"].ToString ();

    // Display value
    URL.Text = url;

    // Display the give webpage
    WebView.LoadRequest(new NSUrlRequest(NSUrl.FromString(url)));

    // Save activity
    UserActivity = activity;
    UserActivity.BecomeCurrent ();

}
```

메서드에는 문서 또는 `UIApplicationRestorationHandler` 응답자 기반 활동 다시 시작에 대해 호출할 수 있는가 포함 되어 있습니다. `ContinueUserActivity` 호출 될 때 복원 처리기에 `NSArray` 또는 복원 가능한 개체를 전달 해야 합니다. 예를 들어:

```csharp
completionHandler (new NSObject[]{Tab4});
```

전달 된 `RestoreUserActivityState` 각 개체에 대해 메서드가 호출 됩니다. 그런 다음 각 개체는 `UserInfo` 사전의 데이터를 사용 하 여 자체 상태를 복원할 수 있습니다. 예를 들어:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

문서 기반 앱 `ContinueUserActivity` 의 경우 메서드를 구현 하지 않거나 반환 하거나를 `UIKit` 반환 `false`하거나 `AppKit` 작업을 자동으로 다시 시작할 수 있습니다. 자세한 내용은 아래의 [문서 기반 앱에서 전달 지원](#supporting-handoff-in-document-based-apps) 섹션을 참조 하세요.

### <a name="failing-handoff-gracefully"></a>정상적으로 전달 실패

전달은 느슨하게 연결 된 iOS 및 OS X 장치 컬렉션 간에 정보를 전송 하기 때문에 경우에 따라 전송 프로세스가 실패할 수 있습니다. 이러한 오류를 정상적으로 처리 하 고 발생 하는 상황을 사용자에 게 알리려면 앱을 디자인 해야 합니다.

오류가 `DidFailToContinueUserActivitiy` 발생할 경우 `AppDelegate` 의 메서드가 호출 됩니다. 예:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

제공 `NSError` 된를 사용 하 여 오류에 대 한 정보를 사용자에 게 제공 해야 합니다.

## <a name="native-app-to-web-browser-handoff"></a>네이티브 앱에서 웹 브라우저 전달

사용자가 원하는 장치에 적절 한 네이티브 앱을 설치 하지 않고도 작업을 계속할 수 있습니다. 경우에 따라 웹 기반 인터페이스를 사용 하 여 필요한 기능을 제공할 수 있으며 작업을 계속 계속할 수 있습니다. 예를 들어 사용자의 전자 메일 계정으로 메시지를 작성 하 고 읽을 수 있는 웹 기반 UI를 제공할 수 있습니다.

원래 앱이 웹 인터페이스 (및 지정 된 항목을 계속 진행 하는 데 필요한 구문)의 URL을 알고 있는 경우 `WebpageURL` `NSUserActivity` 인스턴스의 속성에서이 정보를 인코딩할 수 있습니다. 수신 장치에 연속 작업을 처리할 적절 한 네이티브 앱이 설치 되어 있지 않은 경우 제공 된 웹 인터페이스를 호출할 수 있습니다.

## <a name="web-browser-to-native-app-handoff"></a>웹 브라우저에서 네이티브 앱 전달

사용자가 원래 장치에서 웹 기반 인터페이스를 사용 하 고 있고 수신 장치의 네이티브 앱이 `WebpageURL` 속성의 도메인 부분을 클레임 하는 경우 시스템은 해당 앱을 사용 하 여 연속 작업을 처리 합니다. 새 장치는 활동 유형을로 `NSUserActivity` `BrowsingWeb` 표시 하는 인스턴스를 수신 하 고, `WebpageURL` 는 사용자가 방문한 `UserInfo` URL을 포함 하 게 됩니다. 사전이 비어 있게 됩니다.

앱이이 형식의 전달에 참여 하려면 형식 `com.apple.developer.associated-domains` `<service>:<fully qualified domain name>` (예: `activity continuation:company.com`)을 사용 하 여 도메인에 자격을 요청 해야 합니다.

지정 된 도메인이 `WebpageURL` 속성의 값과 일치 하면 핸드 오프는 해당 도메인의 웹 사이트에서 승인 된 앱 id 목록을 다운로드 합니다. 웹 사이트는 **apple 앱 사이트-연결** (예: `https://company.com/apple-app-site-association`) 이라는 서명 된 JSON 파일에 승인 된 id 목록을 제공 해야 합니다.

이 JSON 파일에는 형식의 `<team identifier>.<bundle identifier>`앱 id 목록을 지정 하는 사전이 포함 되어 있습니다. 예를 들어:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

의 `Content-Type` [https://support.apple.com/kb/ht5012](https://support.apple.com/kb/ht5012) `openssl` 올바른를 갖도록 JSON 파일에 서명 하려면 터미널 앱 및 iOS에서 신뢰 하는 인증 기관에서 발급 한 키 (목록 참조)를 사용 하 여 명령을 사용 합니다. `application/pkcs7-mime` 예를 들어:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

이 명령은 웹 사이트에 입력 한 서명 된 JSON 파일을 **apple 앱 사이트-연결** URL에 출력 합니다. `openssl` 예를 들어:

```csharp
https://example.com/apple-app-site-association.
```

앱은 해당 `com.apple.developer.associated-domains` 도메인의 자격을 `WebpageURL` 가진 모든 활동을 수신 합니다. `http` 및`https` 프로토콜만 지원 되며 다른 프로토콜은 예외를 발생 시킵니다.

## <a name="supporting-handoff-in-document-based-apps"></a>문서 기반 앱에서 핸드 오프 지원

위에서 설명한 것 처럼 iOS 및 OS X에서 문서 기반 앱은 앱의 **info.plist** 파일에의 `CFBundleDocumentTypes` `NSUbiquitousDocumentUserActivityType`키가 포함 된 경우 iCloud 기반 문서의 전달 기능을 자동으로 지원 합니다. 예를 들어:

```xml
<key>CFBundleDocumentTypes</key>
<array>
    <dict>
        <key>CFBundleTypeName</key>
        <string>NSRTFDPboardType</string>
        . . .
        <key>LSItemContentTypes</key>
        <array>
        <string>com.myCompany.rtfd</string>
        </array>
        . . .
        <key>NSUbiquitousDocumentUserActivityType</key>
        <string>com.myCompany.myEditor.editing</string>
    </dict>
</array>
```

이 예제에서 문자열은 활동의 이름이 추가 된 역방향 DNS 앱 지정자입니다. 이러한 방식으로 입력 한 경우 `NSUserActivityTypes` **info.plist** 파일의 배열에서 작업 형식 항목을 반복할 필요가 없습니다.

자동으로 생성 된 사용자 활동 개체 (문서의 `UserActivity` 속성을 통해 사용 가능)는 앱의 다른 개체에서 참조 하 고 연속 시 상태를 복원 하는 데 사용할 수 있습니다. 예를 들어 항목 선택 및 문서 위치를 추적 합니다. 상태를 변경 하 고 `NeedsSave` `UpdateUserActivityState` 메서드에서 `true` 사전을 업데이트할 때마다이 활동 속성을로 설정 해야 합니다. `UserInfo`

속성 `UserActivity` 은 모든 스레드에서 사용할 수 있으며 KVO (키-값 관찰) 프로토콜을 준수 하므로 iCloud에서 이동할 때 문서를 동기화 상태로 유지 하는 데 사용할 수 있습니다. 문서를 닫으면 속성이 무효화 됩니다. `UserActivity`

자세한 내용은 [문서 기반 앱의 Apple 사용자 활동 지원](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5) 설명서를 참조 하세요.

## <a name="supporting-handoff-in-responders"></a>응답기에서 전달 지원

속성을 설정 하 여 응답자 (iOS `UIResponder` 또는 `NSResponder` OS X에서 상속 됨)를 활동에 연결할 수 있습니다. `UserActivity` 시스템은 적절 한 시간 `UserActivity` 에 속성을 자동으로 저장 하 고, `AddUserInfoEntriesFromDictionary` 메서드 `UpdateUserActivityState` 를 사용 하 여 현재 데이터를 사용자 동작 개체에 추가 하는 응답자의 메서드를 호출 합니다.

## <a name="supporting-continuation-streams"></a>연속 스트림 지원

는 작업을 계속 하는 데 필요한 정보의 양이 초기 전달 페이로드에 의해 효율적으로 전송 되지 않는 경우가 있을 수 있습니다. 이러한 상황에서 수신 앱은 데이터를 전송 하기 위해 자신과 원래 앱 간에 하나 이상의 스트림을 설정할 수 있습니다.

원본 응용 프로그램은 `SupportsContinuationStreams` `NSUserActivity` 인스턴스의 속성을로 `true`설정 합니다. 예를 들어:

```csharp
// Create a new user Activity to support this tab
UserActivity = new NSUserActivity (ThisApp.UserActivityTab1){
    Title = "Weather Tab",
    SupportsContinuationStreams = true
};
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

그러면 받는 앱에서의 메서드 `GetContinuationStreams` `NSUserActivity` `AppDelegate` 를 호출 하 여 스트림을 설정할 수 있습니다. 예를 들어:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

원래 장치에서 사용자 활동 대리자는 해당 `DidReceiveInputStream` 메서드를 호출 하 여 다시 시작 장치에서 사용자 활동을 계속 하는 데 필요한 데이터를 제공 함으로써 스트림을 받습니다.

을 사용 `NSInputStream` 하 여 스트림 데이터에 읽기 전용 액세스를 제공 하 `NSOutputStream` 고 쓰기 전용 액세스를 제공 합니다. 스트림은 수신 앱에서 더 많은 데이터를 요청 하 고 원래 앱에서 제공 하는 요청 및 응답 방식으로 사용 해야 합니다. 따라서 원래 장치의 출력 스트림에 기록 된 데이터를 계속 해 서 장치의 입력 스트림에서 읽고 그 반대의 경우도 마찬가지입니다.

연속 스트림이 필요한 경우에도 두 앱 간에는 최소한의 통신이 있어야 합니다.

자세한 내용은 Apple의 [연속 스트림 사용](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13) 설명서를 참조 하세요.

## <a name="handoff-best-practices"></a>핸드 오프 모범 사례

전달을 통해 사용자 활동의 원활한 연속을 구현 하려면 관련 된 모든 다양 한 구성 요소가 있으므로 신중 하 게 설계 해야 합니다. Apple은 전달이 설정 된 앱에 대 한 다음 모범 사례를 채택 하는 것을 제안 합니다.

- 작업 상태를 계속 하는 데 사용할 수 있는 가장 작은 페이로드를 요구 하도록 사용자 활동을 설계 합니다. 페이로드가 클수록 연속 작업이 시작 되는 데 걸리는 시간이 길어집니다.
- 성공적인 연속을 위해 많은 양의 데이터를 전송 해야 하는 경우 구성 및 네트워크 오버 헤드와 관련 된 비용을 고려 합니다.
- 일반적으로 많은 수의 Mac 앱은 iOS 장치에서 몇 가지 작고 작업별 앱에 의해 처리 되는 사용자 활동을 만듭니다. 서로 다른 앱 및 OS 버전은 함께 작동 하거나 정상적으로 실패 하도록 설계 되어야 합니다.
- 활동 유형을 지정할 때 충돌을 방지 하려면 역방향 DNS 표기법을 사용 합니다. 활동이 지정 된 앱에만 해당 되는 경우 해당 이름이 형식 정의에 포함 되어야 합니다 (예: `com.myCompany.myEditor.editing`). 활동이 여러 앱에서 작동할 수 있는 경우 정의에서 앱 이름을 삭제 합니다 (예: `com.myCompany.editing`).
- 앱에서 사용자 활동의 상태 (`NSUserActivity`)를 업데이트 해야 하는 경우 `NeedsSave` 속성을로 `true`설정 합니다. 적절 한 시간에 전달은 대리자의 `UserActivityWillSave` 메서드를 호출 하므로 필요에 따라 `UserInfo` 사전을 업데이트할 수 있습니다.
- 전달 프로세스는 수신 장치에서 즉시 초기화 되지 않을 수 있기 때문에를 `AppDelegate` `WillContinueUserActivity` 구현 하 고 연속 작업이 시작 됨을 사용자에 게 알립니다.

## <a name="example-handoff-app"></a>핸드 오프 앱 예제

Xamarin.ios 앱에서 핸드 오프를 사용 하는 예로이 가이드에 [**Monkeybrowser**](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser) 샘플 앱이 포함 되어 있습니다. 앱에는 사용자가 웹을 탐색 하는 데 사용할 수 있는 4 개의 탭이 있으며 각 탭에는 지정 된 작업 유형이 있습니다. 날씨, 즐겨찾기, 커피 및 작업.

모든 탭에서 사용자가 새 url을 입력 하 고 **이동** 단추를 누르면 사용자가 현재 검색 `NSUserActivity` 중인 url을 포함 하는 해당 탭에 대 한 새가 만들어집니다.

[![](handoff-images/handoff01.png "핸드 오프 앱 예제")](handoff-images/handoff01.png#lightbox)

사용자의 장치 중 다른 사용자의 장치에 **Monkeybrowser** 앱이 설치 되어 있는 경우, 동일한 사용자 계정을 사용 하 여 iCloud에 로그인 하 고, 동일한 네트워크에 있고, 위 장치와 근접 하 게 됩니다. 전달 활동은 홈 화면에 표시 됩니다 (아래쪽). 왼쪽 모서리):

[![](handoff-images/handoff02.png "왼쪽 아래 모퉁이의 홈 화면에 표시 되는 핸드 오프 활동")](handoff-images/handoff02.png#lightbox)

사용자가 핸드 위로 아이콘을 끌면 앱이 시작 되 고에 `NSUserActivity` 지정 된 사용자 활동이 새 장치에서 계속 됩니다.

[![](handoff-images/handoff03.png "새 장치에서 계속 되는 사용자 작업")](handoff-images/handoff03.png#lightbox)

사용자 활동이 다른 Apple 장치에 성공적으로 전송 되 면 송신 장치 `NSUserActivity` 는 해당 `NSUserActivityDelegate` 에 대 한 `UserActivityWasContinued` 메서드에 대 한 호출을 수신 하 여 사용자 활동이 다른 사용자에 게 전송 되었음을 알 수 있습니다. 장치.

## <a name="summary"></a>요약

이 문서에는 사용자의 Apple 장치 간에 사용자 활동을 계속 하는 데 사용 되는 핸드 오프 프레임 워크에 대 한 소개를 제공 합니다. 다음으로 Xamarin.ios 앱에서 핸드 오프를 사용 하도록 설정 하 고 구현 하는 방법을 살펴보았습니다. 마지막으로, 다양 한 유형의 전달 연속 사용 가능 및 전달 모범 사례에 대해 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [MonkeyBrowser 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0의 새로운 기능](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper 가이드](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit 사용자 인터페이스 지침](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit Framework 참조](https://developer.apple.com/library/ios/home_kit_framework_ref)
