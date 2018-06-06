---
title: Xamarin.iOS에 전달
description: 이 문서에서 Xamarin.iOS 앱으로 전송 하도록 핸드 오프 작업에서는 사용자에 실행 되는 앱 간의 작업을 사용자의 다른 장치입니다.
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ec324e8fb8327b622424311b89567608311a6a19
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787522"
---
# <a name="handoff-in-xamarinios"></a>Xamarin.iOS에 전달

_이 문서에서 Xamarin.iOS 앱으로 전송 하도록 핸드 오프 작업에서는 사용자에 실행 되는 앱 간의 작업을 사용자의 다른 장치입니다._

Apple는 iOS 8 및 OS X Yosemite (10.10) 자신의 장치 중 하나에서 시작 하는 작업을 전송 하도록 사용자에 대 한 일반적인 메커니즘을 제공에서 동일한 응용 프로그램 또는 동일한 작업을 지 원하는 다른 응용 프로그램을 실행 하는 다른 장치에 핸드 오프를 도입 되었습니다.

[![](handoff-images/handoff02.png "핸드 오프 작업 수행의 예")](handoff-images/handoff02.png#lightbox)

이 문서는 빠른 살펴보기 Xamarin.iOS 앱에서 공유 하는 활동을 사용 하도록 설정 하 고 자세히 핸드 오프 프레임 워크를 포함 합니다.

## <a name="about-handoff"></a>핸드 오프 하는 방법에 대 한

핸드 오프 (연속성 라고도 함)는 사용자가 자신의 장치 (iOS 또는 Mac) 중 하나에 활동을 시작 하 고 다른 장치 (사용자의 iClou에서 식별 한 대로 같은 동작에 계속 하는 방법으로 Apple ios 8 및 OS X Yosemite (10.10)을 통해 시작 된 d 계정)입니다.

핸드 오프 향상 된 검색 기능에서 iOS 9도 새로 만들기를 지원 하도록 확장 되었습니다. 자세한 내용은 참조 하십시오 우리의 [검색 향상](~/ios/platform/search/index.md) 설명서입니다.

예를 들어 사용자는 자신의 iPhone에서 전자 메일을 시작 하 고 원활 하 게 동일한 메시지 정보가 채워져 및 iOS에 남아 있습니다 동일한 위치에 커서의 Mac에서 전자 메일을 계속 수 있습니다.

동일한 공유 하는 앱의 _팀 ID_ (엔터프라이즈, Mac 용은 iTunes App Store를 통해 전달 된 핸드 오프를 사용 하 여 작업을 계속 하도록 사용자 응용 프로그램 간에으로 이러한 앱 중 하나에 대 한 적격 또는 등록 된 개발자에 의해 서명 된 또는 임시 응용 프로그램)입니다.

모든 `NSDocument` 또는 `UIDocument` 기반된 앱 자동으로 보유 하는 전달 하기, 즉 기본 제공 지원 하 고 전달 하기를 지원 하려면 최소한의 변경 해야 합니다.

### <a name="continuing-user-activities"></a>지속적인 사용자 동작

`NSUserActivity` 클래스 (일부 변경 된 사항에 함께 `UIKit` 및 `AppKit`) 다른 사용자의 장치에 계속 될 수 있는 사용자의 활동을 정의 하기 위한 지원을 제공 합니다.

다른 사용자의 장치를 통해 전달 되어야 하는 활동에 대 한 것에서 캡슐화 해야 인스턴스 `NSUserActivity`로 표시 됨는 _현재 활동_의 페이로드 집합 (연속 작업을 수행 하는 데 데이터) 및 그런 다음 해당 장치에 작업을 전송 되어야 합니다.

핸드 오프 최소한 계속 될 큰 데이터 패킷을 iCloud를 통해 동기화 되 고 활동을 정의 하는 정보를 전달 합니다.

수신 하는 장치에서 사용자 활동 continuation에 사용할 수 있는 되었다는 알림을 받게 됩니다. (아직 실행 하지 않은) 하는 경우 지정 된 앱이 시작 사용자가 새 장치에서 작업을 계속을 선택 하면 및에서 페이로드는 `NSUserActivity` 작업을 다시 시작 하는 데 사용 됩니다.

[![](handoff-images/handoffinteractions.png "지속적인 사용자 작업의 개요")](handoff-images/handoffinteractions.png#lightbox)

동일한 개발자 팀 ID를 공유 하 고 응답 하는 앱만는 주어진 _활동 유형_ 연속 작업에 적합 합니다. 앱에서 지원 되는 활동 형식을 정의 `NSUserActivityTypes` 키의 해당 **Info.plist** 파일입니다. 이 점을 고려 지속적인 장치 활동 유형 팀 ID에 따라 연속 작업을 수행 하는 앱을 선택 하 고 선택적으로 _작업 제목_합니다.

수신 응용 프로그램 정보를 사용 하는 `NSUserActivity`의 `UserInfo` 사전 해당 사용자 인터페이스를 구성 하 고 최종 사용자에 게 원활한 전환을 표시 되도록 지정된 된 활동의 상태를 복원 합니다.

연속 작업에 효율적으로 통해 보낼 수 있는 추가 정보가 필요한 경우 한 `NSUserActivity`, 원래 응용 프로그램에 대 한 호출을 전송 하 고 필요한 데이터는 전송 스트림을 하나 이상 연결할 수 응용 프로그램을 다시 시작 합니다. 예를 들어 활동 된 이미지가 여러 개 사용 하 여 큰 텍스트 문서를 편집 하는 경우 스트리밍 해야 합니다 수신 장치에서 작업을 계속 하는 데 필요한 정보를 전송 합니다. 자세한 내용은 참조는 [지원 연속 스트림을](#Supporting-Continuation-Streams) 아래 섹션.

위의 설명 대로 `NSDocument` 또는 `UIDocument` 기반된 앱 자동으로 보유 하는 기본 제공 지원으로 전달 합니다. 자세한 내용은 참조는 [문서 기반 앱에서 지 원하는 전달](#Supporting-Handoff-in-Document-Based-Apps) 아래 섹션.

### <a name="the-nsuseractivity-class"></a>NSUserActivity 클래스

`NSUserActivity` 클래스 핸드 오프 교환에는 기본 개체 및 continuation에 사용할 수 있는 사용자 작업의 상태를 캡슐화 하는 데 사용 됩니다. 앱의 복사본을 인스턴스화하고 됩니다 `NSUserActivity` 를 지원 하 고 다른 장치에서 계속 사용 하려는 모든 활동에 대 한 합니다. 예를 들어, 문서 편집기 만들어야 각 문서에 대 한 활동 현재 열려 있습니다. 그러나 앞쪽 문서만 (앞쪽 창이 나 탭으로 표시 됨)는는 _현재 활동_ 이며 따라서 continuation에 사용할 수 있습니다.

인스턴스 `NSUserActivity` 모두에 의해 식별 되는 `ActivityType` 및 `Title` 속성입니다. `UserInfo` 사전 속성은 활동의 상태에 대 한 정보를 전달 하는 데 사용 합니다. 설정의 `NeedsSave` 속성을 `true` 에 지연 하려는 경우이 통해 상태 정보를 로드는 `NSUserActivity`의 위임 합니다. 사용 하 여는 `AddUserInfoEntries` 에 있는 다른 클라이언트에서 새 데이터를 병합 하는 메서드는 `UserInfo` 활동의 상태를 유지 하려면 필요에 따라 사전입니다.

### <a name="the-nsuseractivitydelegate-class"></a>NSUserActivityDelegate 클래스

`NSUserActivityDelegate` 에 정보를 보관 하는 데 사용 되는 `NSUserActivity`의 `UserInfo` 사전 최신으로 그리고 활동의 현재 상태와 동기화 합니다. 시스템 업데이트 될 작업의 정보를 필요로 하는 경우 (와 같은 다른 장치에 대 한 연속 전에)를 호출 하는 `UserActivityWillSave` 대리자의 메서드.

구현 해야 합니다는 `UserActivityWillSave` 메서드와으로 변경한은 `NSUserActivity` (같은 `UserInfo`, `Title`등)는 현재 활동의 상태 여전히 적용 되었는지 확인 하려면. 시스템 호출 하는 경우는 `UserActivityWillSave` 메서드는 `NeedsSave` 플래그 지워질 예정입니다. 설정 해야 활동의 데이터 속성을 수정 하면 `NeedsSave` 를 `true` 다시 합니다.

사용 하는 대신는 `UserActivityWillSave` 위의 메서드를 사용할 수 있습니다 필요에 따라 `UIKit` 또는 `AppKit` 사용자 동작을 자동으로 관리 합니다. 이 위해 응답자 개체의 설정 `UserActivity` 속성과 구현은 `UpdateUserActivityState` 메서드. 참조는 [응답자에 핸드 오프를 지 원하는](#Supporting-Handoff-in-Responders) 자세한 내용은 아래 섹션.

### <a name="app-framework-support"></a>응용 프로그램 프레임 워크 지원

둘 다 `UIKit` (iOS) 및 `AppKit` (OS X)에 전달 하기 위해 기본 제공 지원을 제공는 `NSDocument`, 응답자 (`UIResponder`/`NSResponder`), 및 `AppDelegate` 클래스입니다. 각 운영 체제 핸드 오프를 약간 다르게 구현 하는 동안 기본 메커니즘 및 Api 동일 합니다.

#### <a name="user-activities-in-document-based-apps"></a>문서 기반 앱에서 사용자 동작

문서 기반 iOS 및 OS X 앱에는 기본 제공 핸드 오프 지원 자동으로 합니다. 이 지원을 활성화 하려면 추가 해야 합니다는 `NSUbiquitousDocumentUserActivityType` 키와 값을 각 `CFBundleDocumentTypes` 응용 프로그램의 항목 **Info.plist** 파일입니다.

이 키가 있을 경우, 둘 다 `NSDocument` 및 `UIDocument` 자동으로 만들도록 `NSUserActivity` iCloud 기반 문서에 대 한 지정 된 형식의 인스턴스. 각 문서는 앱에서 지 원하는 형식에 대 한 활동 유형을 제공 해야 합니다 및 여러 문서 형식에는 동일한 활동 형식을 사용할 수 있습니다. 둘 다 `NSDocument` 및 `UIDocument` 자동으로 채울는 `UserInfo` 의 속성은 `NSUserActivity` 와 해당 `FileURL` 속성의 값입니다.

OS x는 `NSUserActivity` 관리 하는 `AppKit` 응답자를 자동으로 연관 및 문서 창과 주 창이 되 면 현재 활동 될 합니다. Ios,에 대 한 `NSUserActivity` 하 여 관리 되는 개체 `UIKit`, 호출 `BecomeCurrent` 메서드 명시적으로 문서의 수도 있고 `UserActivity` 에 속성 집합에 `UIViewController` 응용 프로그램은 전경으로 제공 하는 경우.

`AppKit` 모든 자동으로 복원 `UserActivity` OS X에서 이러한 방식으로 생성 속성입니다. 이 경우에 발생는 `ContinueUserActivity` 메서드 반환 `false` 또는 구현 된 경우. 와 문서를 열이 경우에는 `OpenDocument` 의 메서드는 `NSDocumentController` 다음을 수신 합니다는 `RestoreUserActivityState` 메서드를 호출 합니다.

참조는 [문서 기반 앱에서 지 원하는 전달](#Supporting-Handoff-in-Document-Based-Apps) 자세한 내용은 아래 섹션.

#### <a name="user-activities-and-responders"></a>사용자 활동 및 응답자

둘 다 `UIKit` 및 `AppKit` 응답자 개체의으로 설정 하면 사용자 동작을 자동으로 관리할 수 `UserActivity` 속성입니다. 설정 해야 상태 수정 된 경우는 `NeedsSave` 응답자의 속성 `UserActivity` 를 `true`합니다. 시스템이 자동으로 저장 된 `UserActivity` 호출 하 여 상태를 업데이트 하기 응답자 시간에 부여한 후 필요할 때 해당 `UpdateUserActivityState` 메서드.

여러 응답자는 단일 공유 하는 경우 `NSUserActivity` 수신 인스턴스에 `UpdateUserActivityState` 시스템 사용자 작업 개체를 업데이트 한 경우 콜백 합니다. 응답자 호출 해야 하는 경우는 `AddUserInfoEntries` 업데이트 하는 메서드는 `NSUserActivity`의 `UserInfo` 이 시점에서 현재 작업 상태를 반영 하기 위해 사전입니다. `UserInfo` 전에 각각의 선택을 취소 하면 사전 `UpdateUserActivityState` 호출 합니다.

응답자에서에서 연결을 끊을 자체 작업을 설정할 수는 `UserActivity` 속성을 `null`합니다. 응용 프로그램 프레임 워크는 관리 되는 `NSUserActivity` 인스턴스에 관련된 응답자 더 또는 문서를 자동으로 무효화 합니다.

참조는 [응답자에 핸드 오프를 지 원하는](#Supporting-Handoff-in-Responders) 자세한 내용은 아래 섹션.

#### <a name="user-activities-and-the-appdelegate"></a>사용자 활동 및는 AppDelegate

앱의 `AppDelegate` 핸드 오프 연속 작업을 처리 하는 경우는 해당 주 진입점입니다. 때 사용자의 반응이 핸드 오프 알림에 적절 한 앱을 실행 (없는 경우 이미 실행 중) 및 `WillContinueUserActivityWithType` 의 메서드는 `AppDelegate` 호출 됩니다. 이 시점에서 응용 프로그램에 연속 작업을 시작 하는 사용자를 알려야 합니다.

`NSUserActivity` 인스턴스 때 전달 되는 `AppDelegate`의 `ContinueUserActivity` 메서드를 호출 합니다. 이 시점에서 응용 프로그램의 사용자 인터페이스를 구성 하 고 지정된 된 활동을 계속 해야 합니다.

참조는 [구현 전달](#Implementing-Handoff) 자세한 내용은 아래 섹션.

## <a name="enabling-handoff-in-a-xamarin-app"></a>Xamarin 앱에 대 한 핸드 오프를 사용 하도록 설정

핸드 오프에서 설정한 보안 요구 사항으로 인해 핸드 오프 프레임 워크를 사용 하는 Xamarin.iOS 앱 올바르게 구성 되어야 합니다 Apple 개발자 포털 및 Xamarin.iOS 프로젝트 파일입니다.

다음을 수행합니다.

1. 에 로그인 된 [Apple 개발자 포털](http://developer.apple.com)합니다.
2. 클릭 **인증서, 식별자 및 프로필**합니다.
3. 그렇게 이미 하지 않은 경우 클릭 **식별자** 응용 프로그램에 대 한 ID를 만듭니다 (예: `com.company.appname`), 프로그램 기존 id입니다. 그렇지 않으면 편집
4. 확인은 **iCloud** 서비스가 지정된 된 ID에 대 한 검사 된: 

    [![](handoff-images/provision01.png "지정된 된 ID에 대 한 iCloud 서비스를 사용 하도록 설정")](handoff-images/provision01.png#lightbox)
5. 변경 내용을 저장합니다.
4. 클릭 **프로 비전 프로필** > **개발** 드립니다 프로비저닝 프로필이 새로운 개발 응용 프로그램을 만듭니다. 

    [![](handoff-images/provision02.png "프로비저닝 프로필에서 앱에 대 한 새로운 개발 만들기")](handoff-images/provision02.png#lightbox)
5. 다운로드 및 새 프로비저닝 프로필을 설치 하거나 사용 Xcode를 다운로드 하 여 프로필을 설치 하십시오.
6. Xamarin.iOS 프로젝트 옵션을 편집 하 고 방금 만든 프로 비전 프로필을 사용 하 고 있는지 확인 하십시오. 

    [![](handoff-images/provision03.png "방금 만든 프로 비전 프로필을 선택 합니다.")](handoff-images/provision03.png#lightbox)
7. 다음에 편집 프로그램 **Info.plist** 파일을 프로 비전 프로필을 만드는 데 사용 하는 응용 프로그램 ID를 사용 하 고 있는지 확인 하십시오. 

    [![](handoff-images/provision04.png "앱 ID를 설정 합니다.")](handoff-images/provision04.png#lightbox)
8. 스크롤하여는 **백그라운드 모드** 섹션 및 다음 항목을 확인 합니다. 

    [![](handoff-images/provision05.png "필요한 백그라운드 모드를 사용 하도록 설정")](handoff-images/provision05.png#lightbox)
9. 모든 파일에 변경 내용을 저장 합니다.

현재 위치를이 설정 하 여 응용 프로그램 전달 프레임 워크 Api에 액세스할 준비가 되었습니다. 에 대 한 자세한 내용은 프로 비전을 참조 하십시오 우리의 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 및 [앱을 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드입니다.

## <a name="implementing-handoff"></a>핸드 오프를 구현합니다.

동일한 개발자 팀 ID와 함께 서명 되 고 동일한 작업 유형을 지원 되는 앱 간에 사용자 작업을 계속할 수 있습니다. Xamarin.iOS 앱에서 핸드 오프를 구현 하려면 사용자 작업 개체를 만들 수 있습니다 (에서 `UIKit` 또는 `AppKit`), 작업을 추적할 수 있는 개체의 상태를 업데이트 하 고 받는 장치에 작업을 계속 합니다.

### <a name="identifying-user-activities"></a>사용자 활동을 식별

핸드 오프를 구현 하는 첫 번째 단계는 응용 프로그램을 지 원하는 사용자 동작의 형식을 식별 하는 하 고 다른 장치에서 연속 작업에 대 한 적합 한 후보가 있는 해당 활동을 표시 합니다. 예를 들어: ToDo 앱 편집 항목 하나로 지원할 수 있습니다 _사용자 활동 유형_, 다른 사용 가능한 항목 목록 검색을 지원 합니다.

응용 프로그램 유형을 만들 수 만큼 사용자 활동 필요, 응용 프로그램을 제공 하는 모든 함수에 대 한. 각 사용자 활동 형식에 대 한 응용 프로그램 형식의 작업 시작 및 종료 및 다른 장치에서 해당 작업을 계속 하려면 최신 상태 정보를 유지 관리 해야 하는 시기를 추적 해야 합니다.

송신 및 수신 응용 프로그램 간의 일대일 매핑 없이 동일한 팀 ID로 서명 된 모든 앱에서 사용자 작업을 계속할 수 있습니다. 예를 들어 지정된 된 앱 네 가지 종류의 다른 장치에 다른, 개별 응용 프로그램에서 사용 하는 활동을 만들 수 있습니다. 여기서 각 응용 프로그램은 작은 특정 작업에 초점을 맞추고 (수 있는 많은 기능 및 함수)는 응용 프로그램의 Mac 버전과 iOS 앱 간의 발생 하는 일반적인 문제입니다.

### <a name="creating-activity-type-identifiers"></a>활동 유형 식별자 만들기

_활동 유형 식별자_ 에 추가 하는 짧은 문자열의 `NSUserActivityTypes` 응용 프로그램의 배열을 **Info.plist** 지정된 된 사용자 활동 형식을 고유 하 게 식별 하는 데 사용 되는 파일입니다. 앱에서 지원 되는 각 활동에 대 한 배열에 하나의 항목이 됩니다. Apple 활동 유형 식별자에 대 한 역방향 DNS 스타일 표기법을 사용 하 여 충돌을 피하기 위해 제안 합니다. 예를 들어: `com.company-name.appname.activity` 특정 앱에 대 한 기반 활동 또는 `com.company-name.activity` 여러 앱을 실행할 수 있는 작업에 대 한 합니다.

활동 유형 식별자 만들 때 사용 되는 `NSUserActivity` 활동의 유형을 식별 하는 인스턴스. 활동은 다른 장치에서 계속 되 면 활동 유형 (함께 응용 프로그램의 팀 ID) 작업을 계속 시작 하는 응용 프로그램을 결정 합니다.

예를 들어, 하겠습니다 호출 샘플 앱을 만드는 **MonkeyBrowser** ([여기 다운로드](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)). 이 응용 프로그램 웹 브라우저 보기에서 다른 URL 열려 있는 각 4 개의 탭을 제공 합니다. 사용자 응용 프로그램을 실행 하는 다른 iOS 장치에서 모든 탭을 계속할 수 됩니다.

이 동작을 지원 하도록 필요한 작업 유형 식별자를 만들려면 편집는 **Info.plist** 파일을 전환 하는 **소스** 보기. 추가 `NSUserActivityTypes` 키를 다음과 같은 식별자를 만듭니다.

[![](handoff-images/type01.png "NSUserActivityTypes 키 및 plist 편집기에서 필요한 식별자")](handoff-images/type01.png#lightbox)

4 개의 새 활동 유형 식별자를 각 예제에서에 탭에 대 한 만든 **MonkeyBrowser** 응용 프로그램입니다. 앱을 만들 때의 내용을 대체는 `NSUserActivityTypes` 앱 하 여 특정 활동 유형 식별자와 배열 지원.

### <a name="tracking-user-activity-changes"></a>사용자 작업 변경 내용 추적

새 인스턴스를 만들 때는 `NSUserActivity` 지정는 클래스는 `NSUserActivityDelegate` 인스턴스 활동의 상태 변경 내용을 추적 하도록 합니다. 예를 들어, 상태 변경 내용을 추적 하는 다음 코드를 사용할 수 있습니다.:

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

`UserActivityReceivedData` 메서드는 연속 스트림을 보내는 장치에서 데이터를 받았습니다. 자세한 내용은 참조는 [지원 연속 스트림을](#Supporting-Continuation-Streams) 아래 섹션.

`UserActivityWasContinued` 메서드는 다른 장치에서 현재 장치 활동으로 전송 합니다. 할 일 목록에 새 항목을 추가 하는 등 작업의 유형에 따라 응용 프로그램 보내는 장치에서 작업을 중단 할 수 있습니다.

`UserActivityWillSave` 메서드 활동 변경 내용이 저장 되 고 로컬에서 사용 가능 장치 간에 동기화 전에 호출 됩니다. 에 지난 1 분간 설정을 변경 하려면이 메서드를 사용할 수는 `UserInfo` 의 속성은 `NSUserActivity` 전송 하기 전에 인스턴스.

### <a name="creating-a-nsuseractivity-instance"></a>NSUserActivity 인스턴스 만들기

응용 프로그램의 다른 장치에 계속의 가능성을 제공 하려고 하는 각 활동에서 캡슐화 해야는 `NSUserActivity` 인스턴스. 응용 프로그램 필요에 따라 많은 활동을 만들 수 및 해당 작업의 특성에 앱의 기능을에 따라 달라 집니다. 예를 들어 전자 메일 앱 하 고 메시지를 읽을 관리를 위한 새 메시지를 만들기 위한 하나의 동작을 만들 수 있습니다.

이 예제에서는 앱에 대 한 새 `NSUserActivity` 탭된 웹 브라우저 보기 중 하나에서 새 URL을 입력할 때마다 생성 됩니다. 지정된 된 탭의 상태를 저장 하는 다음 코드:

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

만드는 새 `NSUserActivity` 사용자 활동 유형 중 하나를 사용 하 여 위에서 만든 및 활동에 대 한 이해 하기 쉬운 제목을 제공 합니다. 인스턴스에 연결 된 `NSUserActivityDelegate` 상태가 변경 되 고이 사용자 작업은 현재 작업을 iOS 알립니다에 대 한 감시 하도록 위에서 만든 합니다.

### <a name="populating-the-userinfo-dictionary"></a>사용자 정보 사전 채우기

위에서 살펴본 것 처럼는 `UserInfo` 의 속성은 `NSUserActivity` 클래스는 한 `NSDictionary` 지정된 된 활동의 상태를 정의 하는 데 사용 되는 키-값 쌍의 합니다. 에 저장 된 값 `UserInfo` 다음 유형 중 하나 여야 합니다: `NSArray`, `NSData`, `NSDate`, `NSDictionary`, `NSNull`, `NSNumber`, `NSSet`, `NSString`, 또는 `NSURL`합니다. `NSURL` icloud와 문서를 가리키는 데이터 값을 받는 장치에서 동일한 문서를 가리키는지에 자동으로 조정 됩니다.

위의 예에서 만든는 `NSMutableDictionary` 개체 및 사용자가 지정 된 탭에서 현재 보고 하는 URL을 제공 하는 단일 키를 제공 합니다. `AddUserInfoEntries` 복원 수신 장치에서 작업을 사용 하는 데이터와 활동을 업데이트 하는 사용자 활동의 메서드를 사용 합니다.

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple 활동은 받는 장치를 시기 적절 하 게에서 전송 되도록 하려면 아주 최소 수준으로 전송 되는 정보를 유지 하는 것이 좋습니다. 문서에 연결 된 이미지를 편집할 수와 같은 더 큰 정보가 필요한 경우, 전송에서 사용 해야 연속 스트림을 해야 합니다. 참조는 [지원 연속 스트림을](#Supporting-Continuation-Streams) 자세한 내용을 보려면 아래 섹션.

### <a name="continuing-an-activity"></a>작업을 계속합니다.

핸드 오프는 로컬 iOS 및 OS X 장치를 원래 장치에 물리적으로 접근에 있으며 동일한 iCloud 계정과 비연속적 사용자 동작의 가용성에 로그인에 자동으로 알립니다. 사용자가 새 장치에서 작업을 계속 하도록 하는 경우 실행 되면서 적절 한 응용 프로그램 (팀 ID와 활동 형식에 기반) 및 정보는 `AppDelegate` 연속 발생 해야 합니다.

첫째는 `WillContinueUserActivityWithType` 메서드 호출 되므로 앱에서 연속 작업이 시작 되기 직전에 사용자를 알릴 수 있습니다. 다음 코드를 사용 하 여 우리는 **AppDelegate.cs** 시작 연속 작업을 처리 하는 예제 앱의 파일:

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

위의 예제에서는 각 뷰-컨트롤러에 등록는 `AppDelegate` 공용 되었고 `PreparingToHandoff` 활동 표시기 및 사용자가 활동 현재 장치에 게 전달할 수를 알 수 있도록 메시지를 표시 하는 메서드입니다. 예제:

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

`ContinueUserActivity` 의 `AppDelegate` 실제로 지정된 된 활동을 계속 호출 됩니다. 예제 앱 다시에서:

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

공용 `PerformHandoff` 의 각 뷰-컨트롤러 실제로 인계 preforms 메서드와 현재 장치에서 작업을 복원 합니다. 예제의 사용자가 다른 장치에서 검색 하는 지정 된 탭에서 동일한 URL 표시 합니다. 예제:

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

`ContinueUserActivity` 메서드를 포함 한 `UIApplicationRestorationHandler` 작업 다시 시작 하는 기반으로 문서 또는 응답자에 대해 호출할 수 있습니다. 전달 해야 합니다는 `NSArray` 또는 호출 될 때 복원 처리기에 개체를 복원할 수 있습니다. 예를 들어:

```csharp
completionHandler (new NSObject[]{Tab4});
```

전달 된, 각 개체에 대 한 해당 `RestoreUserActivityState` 메서드가 호출 됩니다. 각 개체의 데이터를 유도할 수 있습니다는 `UserInfo` 자체의 상태를 복원 하는 사전입니다. 예를 들어:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

문서 기반 앱을 구현 하지 않는 경우에 `ContinueUserActivity` 메서드 반환 `false`, `UIKit` 또는 `AppKit` 활동을 자동으로 다시 시작할 수 있습니다. 참조는 [문서 기반 앱에서 지 원하는 전달](#Supporting-Handoff-in-Document-Based-Apps) 자세한 내용은 아래 섹션.

### <a name="failing-handoff-gracefully"></a>핸드 오프를 안전한 오류

핸드 오프 컬렉션 느슨하게 연결 된 iOS 및 OS X 장치 간의 정보의 전송에 의존, 전송 프로세스에서 경우에 따라 실패할 수 있습니다. 이러한 오류를 정상적으로 처리 하 고 발생 하는 경우 모든 사용자에 게 알리는 있는 응용 프로그램을 디자인 해야 합니다.

오류가 발생 한는 `DidFailToContinueUserActivitiy` 의 메서드는 `AppDelegate` 호출 됩니다. 예를 들어:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

제공 된 사용할지 `NSError` 오류에 대 한 사용자에 게 정보를 제공 합니다.

## <a name="native-app-to-web-browser-handoff"></a>웹 브라우저 핸드 오프를 네이티브 응용 프로그램

사용자 활동 원하는 장치에 설치 되어 적절 한 네이티브 앱 필요 없이 계속 하려고 할 수 있습니다. 경우에 따라 웹 기반 인터페이스는 필요한 기능을 제공할 수 있습니다 및 작업을 계속 계속할 수 있습니다. 예를 들어 사용자의 전자 메일 계정 작성 및 메시지 도착 읽기에 대 한 웹 기반 UI를 제공할 수 있습니다.

원래, 네이티브 앱 URL을 알고 있는 경우 웹 인터페이스 (및 계속 되 고 지정된 된 항목을 식별 하는 데 필요한 구문)에 대 한이 정보를 인코딩할 수는 `WebpageURL` 의 속성은 `NSUserActivity` 인스턴스. 받는 장치 없는 경우 적절 한 네이티브 앱을 설치 하는 연속 작업을 처리할 제공 된 웹 인터페이스를 호출할 수 있습니다.

## <a name="web-browser-to-native-app-handoff"></a>네이티브 앱 핸드 오프를 웹 브라우저

사용자가 원래 장치에서 웹 기반 인터페이스를 사용 하는 경우 및 네이티브 응용 프로그램 받는 장치에 클레임의 도메인 부분은 `WebpageURL` 속성을 다음 시스템 핸들 연속 작업이 해당 앱을 사용 합니다. 새 장치를 받게 됩니다는 `NSUserActivity` 활동 유형으로 표시 하는 인스턴스 `BrowsingWeb` 및 `WebpageURL` 사용자가 방문 하는 URL이 포함 됩니다는 `UserInfo` 사전 비어 있게 됩니다.

이 유형의 전달에 참여할 수는 응용 프로그램에 대 한 도메인에 claim 해야는 `com.apple.developer.associated-domains` 형식으로 자격 `<service>:<fully qualified domain name>` (예: `activity continuation:company.com`).

지정된 된 도메인에 일치 하는 경우는 `WebpageURL` 해당 도메인의 웹 사이트에서 승인 된 응용 프로그램 Id의 목록을 다운로드 하는 속성의 값을 전달 합니다. 웹 사이트 라는 JSON 파일에 서명 된 승인 된 Id 목록을 제공 해야 **apple 응용 프로그램-사이트 연결** (예를 들어 `https://company.com/apple-app-site-association`).

이 JSON 파일로 포함 형식에서 응용 프로그램 Id의 목록을 지정 하는 사전 `<team identifier>.<bundle identifier>`합니다. 예를 들어:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

JSON 파일에 서명 (올바른 갖도록 `Content-Type` 의 `application/pkcs7-mime`)를 사용 하 여는 **터미널** 응용 프로그램 및 `openssl` 인증서 및 iOS에서 신뢰할 수 있는 인증 기관에서 발급 하는 키와 함께 명령을 (참조 [ http://support.apple.com/kb/ht5012 ](http://support.apple.com/kb/ht5012) 목록에 대 한). 예를 들어:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

`openssl` 명령 출력에서 웹 사이트에 배치 하는 서명된 된 JSON 파일의 **apple 응용 프로그램-사이트 연결** URL입니다. 예를 들어:

```csharp
https://example.com/apple-app-site-association.
```

앱은 활동을 받지 인 `WebpageURL` 도메인이 해당 `com.apple.developer.associated-domains` 권한. 만 `http` 및 `https` 프로토콜은 지원, 다른 프로토콜에서 예외가 발생 합니다.

## <a name="supporting-handoff-in-document-based-apps"></a>핸드 오프 문서 기반 앱에서 지원

IOS 및 OS X에서 설명한 것 처럼 문서 기반 응용 프로그램 자동으로 지원 하도록 할 iCloud 기반 문서의 인계 응용 프로그램의 **Info.plist** 파일에 포함 되어는 `CFBundleDocumentTypes` 의 키 `NSUbiquitousDocumentUserActivityType`합니다. 예를 들어:

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

이 예에서 문자열은 추가 작업의 이름으로는 역방향 DNS 앱 지정자입니다. 활동 형식 항목에서 반복할 필요가 없습니다 이러한 방식으로 입력 하는 경우는 `NSUserActivityTypes` 배열을 **Info.plist** 파일입니다.

자동으로 만들어진 사용자 활동 개체 (문서의 통해 사용할 수 있는 `UserActivity` 속성) 응용 프로그램의 다른 개체에서 참조할 수 있으며 연속 작업의 상태를 복원 하는 데 사용 합니다. 예를 들어 추적할 항목 선택 및 문서 위치를 지정 합니다. 이 활동을 설정 해야 `NeedsSave` 속성을 `true` 때마다 상태를 변경 하 고 업데이트는 `UserInfo` 사전에는 `UpdateUserActivityState` 메서드.

`UserActivity` 속성 모든 스레드에서 사용할 수 있으며 동기화 상태를 유지 문서 icloud와의 입출력 이동할 때 사용할 수 있도록 키-값 관찰 (KVO) 프로토콜을 준수 합니다. `UserActivity` 속성이 문서를 닫을 때 무효화 됩니다.

자세한 내용은 Apple의를 참조 하십시오 [문서 기반 앱에서 사용자 활동 지원](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5) 설명서입니다.

## <a name="supporting-handoff-in-responders"></a>응답자에서 핸드 오프를 지 원하는

응답자에 연결할 수 있습니다 (중 하나에서 상속 되며, `UIResponder` iOS에서 또는 `NSResponder` OS x)을 설정 하 여 활동의 `UserActivity` 속성입니다. 시스템에서 자동으로 저장 된 `UserActivity` 적절 한 시간, 응답자의 호출에서 속성 `UpdateUserActivityState` 메서드를 사용 하 여 사용자 동작 개체는 현재 데이터는 `AddUserInfoEntriesFromDictionary` 메서드 합니다.

## <a name="supporting-continuation-streams"></a>연속 스트림을 지원합니다.

없는 작업을 계속 하는 데 필요한 정보의 양은 전송할 수 없습니다 효율적으로 초기 핸드 오프 페이로드에 경우가 있을 수 있습니다. 이러한 상황에서 수신 응용 프로그램 자체와 원래 응용 프로그램에서 데이터를 전송 사이의 하나 이상의 스트림을 설정할 수 있습니다.

원래 앱 설정 됩니다는 `SupportsContinuationStreams` 의 속성은 `NSUserActivity` 인스턴스를 `true`합니다. 예를 들어:

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

수신 응용 프로그램이 호출할 수는 `GetContinuationStreams` 의 메서드는 `NSUserActivity` 에 해당 `AppDelegate` 스트림을 설정 하기. 예를 들어:

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

호출 하 여 사용자 활동 대리자 수신 스트림을 원래 장치에서 해당 `DidReceiveInputStream` 메서드 데이터를 제공을 다시 시작 하는 장치에 사용자 작업을 계속 하도록 요청 합니다.

사용 하 여는 `NSInputStream` 스트림 데이터에 대 한 읽기 전용 액세스를 제공 하 고 `NSOutputStream` 쓰기 전용 액세스를 제공 합니다. 요청 및 응답 방식 이며, 여기서 수신 응용 프로그램 더 많은 데이터를 요청 하 고 원래 응용 프로그램에서 제공 하는 스트림은 사용 해야 합니다. 지속적인 장치에서 입력 스트림에서 원래 장치에서 출력 스트림에 기록 된 데이터 읽기 되도록 하거나 그 반대로 합니다.

연속 스트림은 필요한 경우에도 있어야 최소한의 뒤로 및 앞 두 앱 간에 통신 합니다.

자세한 내용은 Apple의을 참조 하십시오. [를 사용 하 여 연속 스트림을](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13) 설명서입니다.

## <a name="handoff-best-practices"></a>핸드 오프에 대 한 유용한 정보

핸드 오프를 통해 사용자 작업의 연속 된 연속 작업의 성공적인 구현을 관련 된 모든 다양 한 구성으로 인해 신중 하 게 디자인을 해야합니다. Apple에서 사용 하도록 설정 하는 전달 응용 프로그램을 위한 다음 모범 사례를 채택 제안:

- 계속 실행 되어야 하는 활동의 상태를 관련 시키기 위해 가능한 작은 페이로드가 필요 하 여 사용자 활동을 디자인 합니다. 클수록 페이로드, 더 긴 시작 하려면 연속을 사용 합니다.
- 많은 양의 성공 연속에 대 한 데이터를 전송 해야 할, 구성 및 네트워크 오버 헤드에 관련 된 비용을 고려해.
- 일반적으로 규모가 큰 Mac 앱에서 처리 하는 몇 가지 작은, iOS 장치에서 작업 관련 응용 프로그램 사용자 활동을 만드는 경우 서로 다른 응용 프로그램 및 운영 체제 버전을 함께 제대로 작동 하거나 실패 하 게 설계 되어야 합니다.
- 활동 유형을 지정할 때는 충돌을 피하기 위해 역방향 DNS 표기법을 사용 합니다. 해당 이름이 형식 정의에 포함 되어야 하는 활동은 지정된 된 앱에만, 하는 경우 (예를 들어 `com.myCompany.myEditor.editing`). 작업에서 여러 앱에서 작업할 수 있는 경우 응용 프로그램 이름 정의에서 삭제 (예를 들어 `com.myCompany.editing`).
- 앱 사용자 작업의 상태를 업데이트 해야 하는 경우 (`NSUserActivity`) 설정의 `NeedsSave` 속성을 `true`합니다. 적절 한 시간에 전달 하기 대리자의 호출 `UserActivityWillSave` 업데이트할 수 있도록 하는 메서드는 `UserInfo` 필요에 따라 사전입니다.
- 전달 프로세스 받는 장치에 즉시 초기화 되지 않는 수 있으므로 구현 해야는 `AppDelegate`의 `WillContinueUserActivity` 및 연속 작업을 시작 하려고 하는 사용자에 게 알립니다.

## <a name="example-handoff-app"></a>예제 핸드 오프 앱

핸드 오프를 사용 하 여 Xamarin.iOS 앱에의 한 예로 나와 [ **MonkeyBrowser** ](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/) 이 가이드에 샘플 응용 프로그램입니다. 응용 프로그램에 사용자 지정 된 활동 형식으로 각 웹 사이트를 탐색 하는 데 사용할 수 있는 4 개의 탭: 날씨, 즐겨찾기, 커피 및 작업.

새 URL과 탭은 사용자가 입력 하는 경우 모든 탭에는 **이동** 단추를 새 `NSUserActivity` 가 현재 사용자를 검색할 URL이 포함 된 해당 탭에 대해 생성 됩니다.

[![](handoff-images/handoff01.png "예제 핸드 오프 앱")](handoff-images/handoff01.png#lightbox)

다른 사용자의 장치에는 **MonkeyBrowser** , 설치 된 응용 프로그램 같은 사용자 계정을 사용 하 여 iCloud에 서명는 동일한 네트워크 위의 장치에 가까이 있는 전달 작업이 홈 표시 됩니다 화면 하단 왼쪽 모서리 쪽) (에:

[![](handoff-images/handoff02.png "홈 화면의 하단 왼쪽 모서리 쪽에 표시 된 전달 활동")](handoff-images/handoff02.png#lightbox)

핸드 오프 아이콘 끌 규모를 증가 하는 경우 응용 프로그램을 시작할 및 사용자 작업에 지정 된는 `NSUserActivity` 새 장치에서 계속 됩니다.

[![](handoff-images/handoff03.png "새 장치에 사용자 작업은 계속")](handoff-images/handoff03.png#lightbox)

경우 사용자 작업에 전송 되었습니다 다른 Apple 장치로 보내는 장치의 `NSUserActivity` 에 대 한 호출을 받게 됩니다는 `UserActivityWasContinued` 메서드를 해당 `NSUserActivityDelegate` 사용자 작업은 다른 성공적으로 전송 된 알립니다. 장치입니다.

## <a name="summary"></a>요약

이 문서에는 여러 사용자의 Apple 장치 간의 사용자 작업을 계속 하는 데 사용 된 핸드 오프 프레임 워크에 간략하게를 설명 했습니다. 다음으로, Xamarin.iOS 앱에서 핸드 오프를 구현 하는 방법에 알아보았습니다. 마지막으로, 다양 한 유형의 핸드 오프 연속 사용 가능한 및 전달 하기 모범 사례를 설명합니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [MonkeyBrowser 샘플](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0에 새로 이란](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper 가이드](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit 사용자 인터페이스 지침](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit 프레임 워크 참조](https://developer.apple.com/library/ios/home_kit_framework_ref)
