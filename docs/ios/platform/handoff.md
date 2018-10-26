---
title: Xamarin.iOS에서 핸드 오프
description: 이 문서에서는 전송할 Xamarin.iOS 앱에 핸드 오프를 사용 하 여 작업 사용자를 실행 하는 앱 간에 사용자 활동의 다른 장치입니다.
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.openlocfilehash: 4b19d060bd8adf1c2b09bb18b7ff608381a35231
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116669"
---
# <a name="handoff-in-xamarinios"></a>Xamarin.iOS에서 핸드 오프

_이 문서에서는 전송할 Xamarin.iOS 앱에 핸드 오프를 사용 하 여 작업 사용자를 실행 하는 앱 간에 사용자 활동의 다른 장치입니다._

Apple는 iOS 8 및 OS X Yosemite(10.10 () 장치 중 하나에서 시작 하는 작업을 전송 하기 위해 사용자에 대 한 일반적인 메커니즘을 제공 하려면 동일한 앱 또는 동일한 동작을 지 원하는 다른 응용 프로그램을 실행 하는 다른 장치에 핸드 오프를 도입 했습니다.

[![](handoff-images/handoff02.png "핸드 오프 작업을 수행 하는 예제")](handoff-images/handoff02.png#lightbox)

이 문서는 빠른 살펴봅니다 Xamarin.iOS 앱을 공유 하는 활동을 사용 하도록 설정 하 고 핸드 오프 프레임 워크를 자세히 설명 됩니다.

## <a name="about-handoff"></a>핸드 오프 하는 방법에 대 한

핸드 오프 (라고도: 연속성) 장치 (iOS 또는 Mac) 중 하나에 활동을 시작 하 고 다른 장치 (사용자의 iClou에서 식별 한 대로 동일한 작업을 계속 하려면 사용자에 대 한 방법으로 Apple ios 8 및 OS X Yosemite(10.10 ()에 의해 정의 된 d 계정)입니다.

핸드 오프 검색 기능 향상 된 새로운 기능도 9 iOS에서 확장 되었습니다. 자세한 내용은 참조 하십시오 우리의 [검색 기능 향상](~/ios/platform/search/index.md) 설명서.

예를 들어, 사용자는 자신의 iPhone에서 전자 메일을 시작 하 고 모든 입력 같은 메시지 정보 및 iOS에서 왼쪽은 동일한 위치에 커서를 사용 하 여 해당 Mac에서 전자 메일을 계속 원활 하 게 수 있습니다.

동일한 공유 되는 앱의 모든 _팀 ID_ (Mac Enterprise 용으로 이러한 앱은 앱에서 사용자 활동을 계속 하려면 핸드 오프를 사용 하 여 iTunes 앱 스토어를 통해 배달에 대 한 적격 또는 개발자로 등록된 하 여 서명 됩니다 또는 임시 앱)입니다.

모든 `NSDocument` 또는 `UIDocument` 기반된 앱 기본 제공 지원 및 핸드 오프를 지원 하기 위해 최소한의 변경 내용이 필요한 핸드 오프를 자동으로 포함 합니다.

### <a name="continuing-user-activities"></a>계속 사용자 활동

합니다 `NSUserActivity` 클래스 (일부 작은 변경 내용과 함께 `UIKit` 고 `AppKit`) 다른 사용자의 장치에서 계속 될 수 있는 사용자의 활동을 정의 하는 것에 대 한 지원을 제공 합니다.

인스턴스에 캡슐화 되어야 해당 활동의 다른 사용자의 장치를 통해 전달할 경우 `NSUserActivity`으로 표시 된, 합니다 _현재 활동_, 집합이 페이로드 (연속 작업을 수행 하는 데 데이터) 및 그런 다음 해당 장치에 작업을 전송 되어야 합니다.

핸드 오프 정보와 iCloud를 통해 동기화 되는 보다 큰 데이터 패킷을 계속 될 활동을 정의 하는 최소 사항만 전달 합니다.

수신 장치에 사용자 작업을 연속 작업에 사용할 수 있음을 알림을 받게 됩니다. (아직 실행) 하는 경우 지정 된 앱이 시작 사용자는 새 장치 작업을 계속 하도록 선택 하는 경우 및에서 페이로드를 `NSUserActivity` 작업을 다시 시작 하는 데 사용 됩니다.

[![](handoff-images/handoffinteractions.png "계속 사용자 작업의 개요")](handoff-images/handoffinteractions.png#lightbox)

동일한 개발자 팀 ID를 공유 하 고 응답 하는 앱만을 지정 _활동 유형_ 연속 작업에 적합 합니다. 앱에서 지원 되는 작업 유형을 정의 합니다 `NSUserActivityTypes` 키에 해당 **Info.plist** 파일입니다. 이 점을 고려 계속 장치 활동 유형 팀 ID를 기반으로 연속 작업을 수행 하려면 앱을 선택 하 고 선택적으로 _작업 제목_합니다.

수신 응용 프로그램의 정보를 사용 합니다 `NSUserActivity`의 `UserInfo` 사용자 인터페이스를 구성 하 고 전환을 원활 하 게 최종 사용자에 게 표시 되도록 지정된 된 활동의 상태를 복원 하는 사전입니다.

연속 통해 효율적으로 전송할 수 있는 추가 정보가 필요한 경우는 `NSUserActivity`, 원래 앱에 대 한 호출을 전송 하 고 필요한 데이터를 전송 하려면 하나 이상의 스트림을 연결할 수 앱을 다시 시작 합니다. 예를 들어, 활동 된 여러 이미지를 사용 하 여 큰 텍스트 문서를 편집 하는 경우 스트리밍는 해야 수신 장치에서 작업을 계속 하는 데 필요한 정보를 전송 합니다. 자세한 내용은 참조는 [지원 연속 스트림을](#Supporting-Continuation-Streams) 섹션 아래.

위에서 설명한 것 처럼 `NSDocument` 또는 `UIDocument` 기반된 앱 기본 제공 지원 핸드 오프를 자동으로 포함 합니다. 자세한 내용은 참조는 [문서 기반 앱에서 지 원하는 핸드 오프](#Supporting-Handoff-in-Document-Based-Apps) 섹션 아래.

### <a name="the-nsuseractivity-class"></a>NSUserActivity 클래스

`NSUserActivity` 클래스 핸드 오프 exchange의 주요 개체 이며 연속 작업에 사용할 수 있는 사용자 활동의 상태를 캡슐화 하는 데 사용 됩니다. 앱의 복사본을 인스턴스화하는 `NSUserActivity` 지원 하 고 다른 장치에서 계속 사용 하려는 모든 작업에 대 한 합니다. 예를 들어, 문서 편집기가 만듭니다 각 문서에 대 한 활동 현재 열려 있습니다. 그러나만 앞쪽 (앞쪽 창 또는 탭에 표시) 문서가 합니다 _현재 작업_ 따라서 continuation에 사용할 수 있습니다.

인스턴스의 `NSUserActivity` 모두에 의해 식별 되는 해당 `ActivityType` 및 `Title` 속성입니다. `UserInfo` 사전 속성 작업의 상태에 대 한 정보를 전달 하는 데 사용 됩니다. 설정 합니다 `NeedsSave` 속성을 `true` 지연 하려는 경우를 통해 상태 정보를 로드 합니다 `NSUserActivity`대리자의 합니다. 사용 하 여는 `AddUserInfoEntries` 에 다른 클라이언트에서 새 데이터를 병합 하는 메서드를 `UserInfo` 활동의 상태를 유지 하려면 필요에 따라 사전.

### <a name="the-nsuseractivitydelegate-class"></a>NSUserActivityDelegate 클래스

`NSUserActivityDelegate` 의 정보를 유지 하는 데 사용 되는 `NSUserActivity`의 `UserInfo` 사전 최신으로 그리고 작업의 현재 상태와 동기화 합니다. 시스템 업데이트 작업의 정보를 필요로 하는 경우 (같은 다른 장치에서 연속 작업 전에)를 호출 합니다 `UserActivityWillSave` 대리자의 메서드.

구현 해야 합니다는 `UserActivityWillSave` 메서드와으로 변경한 합니다 `NSUserActivity` (같은 `UserInfo`, `Title`등) 현재 활동의 상태는 여전히 반영 되도록 합니다. 시스템 호출 하는 경우는 `UserActivityWillSave` 메서드는 `NeedsSave` 플래그를 지웁니다. 설정 해야 모든 작업의 데이터 속성을 수정 하면 `NeedsSave` 에 `true` 다시 합니다.

사용 하는 대신 합니다 `UserActivityWillSave` 위의 메서드를 필요에 따라 있습니다 `UIKit` 또는 `AppKit` 사용자 활동을 자동으로 관리 합니다. 이 위해 응답자 개체를 설정 `UserActivity` 속성을 구현 합니다 `UpdateUserActivityState` 메서드. 참조 된 [응답자에서 핸드 오프를 지 원하는](#Supporting-Handoff-in-Responders) 자세한 내용은 아래 섹션입니다.

### <a name="app-framework-support"></a>응용 프로그램 프레임 워크 지원

둘 다 `UIKit` (iOS) 및 `AppKit` (OS X)에 게 전달 하기 위해 기본 제공 지원을 제공 합니다 `NSDocument`, 응답자 (`UIResponder`/`NSResponder`), 및 `AppDelegate` 클래스입니다. 각 OS 핸드 오프를 약간 다르게 구현 하는 동안 기본 메커니즘은 및 Api 동일 합니다.

#### <a name="user-activities-in-document-based-apps"></a>문서 기반 앱에서 사용자 활동

문서 기반 iOS 및 OS X 앱 핸드 오프 지원을 기본 제공 자동으로 가집니다. 이 지원은 활성화 하려면 추가 해야는 `NSUbiquitousDocumentUserActivityType` 키와 각각에 대해 값 `CFBundleDocumentTypes` 앱의 항목 **Info.plist** 파일입니다.

이 키가 있는 경우 둘 다 `NSDocument` 하 고 `UIDocument` 자동으로 만들 `NSUserActivity` iCloud 기반 문서에 대 한 지정 된 형식의 인스턴스. 각 앱을 지 원하는 문서 형식에 대 한 활동 유형을 제공 해야 합니다 및 다중 문서 형식에는 동일한 작업 형식을 사용할 수 있습니다. 둘 다 `NSDocument` 및 `UIDocument` 자동 채우기는 `UserInfo` 의 속성을 `NSUserActivity` 사용 하 여 해당 `FileURL` 속성의 값.

OS X에서 합니다 `NSUserActivity` 에서 관리 하는 `AppKit` 응답기를 사용 하 여 자동으로 연결 하 고 문서 창이 주 창 하는 경우 현재 작업 수 있습니다. Ios의 경우에 대 한 `NSUserActivity` 관리 하는 개체 `UIKit`를 호출 해야 합니다 `BecomeCurrent` 메서드 명시적으로 문서의 있거나 `UserActivity` 속성 집합을 `UIViewController` 전경으로 앱을 제공 하는 경우.

`AppKit` 하나를 자동으로 복원 된다는 `UserActivity` OS X에서 이러한 방식으로 생성 하는 속성입니다. 이 경우에 발생 합니다 `ContinueUserActivity` 메서드가 반환 되는 `false` 구현 하지 않으면. 이 경우 문서를 사용 하 여 열을 `OpenDocument` 메서드의 `NSDocumentController` 다음을 수신 합니다를 `RestoreUserActivityState` 메서드 호출 합니다.

참조 된 [문서 기반 앱에서 지 원하는 핸드 오프](#Supporting-Handoff-in-Document-Based-Apps) 자세한 내용은 아래 섹션입니다.

#### <a name="user-activities-and-responders"></a>사용자 활동 및 응답자

둘 다 `UIKit` 하 고 `AppKit` 응답자를 설정한 경우 사용자 작업을 자동으로 관리할 수 `UserActivity` 속성입니다. 설정 해야 상태 수정 하는 경우는 `NeedsSave` 응답자의 속성 `UserActivity` 에 `true`입니다. 시스템이 자동으로 저장 합니다 `UserActivity` 를 호출 하 여 상태를 업데이트 하려면 응답자 시간이 지난 후 필요한 경우 해당 `UpdateUserActivityState` 메서드.

여러 응답 자가 단일 공유 하는 경우 `NSUserActivity` 받게 인스턴스는 `UpdateUserActivityState` 시스템 사용자 활동 개체를 업데이트 하는 경우 콜백 합니다. 응답자를 호출 해야 합니다.를 `AddUserInfoEntries` 메서드를 업데이트 합니다 `NSUserActivity`의 `UserInfo` 이 시점에서 현재 작업 상태를 반영 하기 위해 사전. 합니다 `UserInfo` 각 하기 전에 사전 지워집니다 `UpdateUserActivityState` 호출 합니다.

응답기를 설정할 수의 연결을 끊을 자체 작업을 해당 `UserActivity` 속성을 `null`입니다. 앱 프레임 워크를 관리 하는 경우 `NSUserActivity` 인스턴스 연결된 응답자 더 없거나 문서에 자동으로 유효성이 검사 된 것입니다.

참조 된 [응답자에서 핸드 오프를 지 원하는](#Supporting-Handoff-in-Responders) 자세한 내용은 아래 섹션입니다.

#### <a name="user-activities-and-the-appdelegate"></a>사용자 활동 및 AppDelegate

앱의 `AppDelegate` 핸드 오프 연속 작업을 처리 하는 경우는 해당 주 진입점입니다. 사용자 응답 하면 핸드 오프 알림, 적절 한 앱을 실행 (하는 경우 이미 실행 중이 아닌) 및 `WillContinueUserActivityWithType` 메서드는 `AppDelegate` 호출 됩니다. 이 시점에서 앱에 연속 작업이 시작 되는 사용자를 알려야 합니다.

`NSUserActivity` 때 인스턴스가 배달 되는지 합니다 `AppDelegate`의 `ContinueUserActivity` 메서드가 호출 됩니다. 이 시점에서 앱의 사용자 인터페이스를 구성 하 고 지정된 된 활동을 계속 해야 합니다.

참조 된 [핸드 오프 구현](#Implementing-Handoff) 자세한 내용은 아래 섹션.

## <a name="enabling-handoff-in-a-xamarin-app"></a>Xamarin 앱에서 사용 하도록 설정 하면 핸드 오프

핸드 오프에 따른 보안 요구 사항으로 인해 핸드 오프 프레임 워크를 사용 하 여 Xamarin.iOS 앱을 제대로 구성 해야 Xamarin.iOS 프로젝트 파일에서 Apple 개발자 포털에 합니다.

다음을 수행합니다.

1. 에 로그인 합니다 [Apple Developer 포털](http://developer.apple.com)합니다.
2. 클릭할 **인증서, 식별자 및 프로필**합니다.
3. 이미 않았다면 클릭할 **식별자** 앱 ID를 만듭니다 (예: `com.company.appname`), 다른 기존 ID를 편집
4. 있는지 확인 합니다 **iCloud** 서비스에 지정된 된 ID에 대 한 확인: 

    [![](handoff-images/provision01.png "지정된 된 ID에 대 한 iCloud 서비스를 사용 하도록 설정")](handoff-images/provision01.png#lightbox)
5. 변경 내용을 저장합니다.
4. 클릭할 **프로 비전 프로필** > **개발** 를 프로 비전 프로필을 새로 개발 앱을 만듭니다. 

    [![](handoff-images/provision02.png "새 개발 프로 비전 앱에 대 한 프로필 만들기")](handoff-images/provision02.png#lightbox)
5. 다운로드 및 새 프로 비전 프로필을 설치 또는 Xcode를 사용 하 여 다운로드 하 고 프로필을 설치 합니다.
6. Xamarin.iOS 프로젝트 옵션을 편집 하 고 방금 만든 프로 비전 프로필을 사용 하 고 있는지 확인 합니다. 

    [![](handoff-images/provision03.png "방금 만든 프로 비전 프로필 선택")](handoff-images/provision03.png#lightbox)
7. 다음으로, 편집 하 **Info.plist** 파일을 프로 비전 프로필을 만드는 데 사용 된 앱 ID를 사용 하 고 있는지 확인: 

    [![](handoff-images/provision04.png "앱 ID를 설정 합니다.")](handoff-images/provision04.png#lightbox)
8. 스크롤하여 합니다 **백그라운드 모드** 섹션 및 다음 항목을 확인 합니다. 

    [![](handoff-images/provision05.png "필수 백그라운드 모드를 사용 하도록 설정")](handoff-images/provision05.png#lightbox)
9. 모든 파일에 변경 내용을 저장 합니다.

현재 위치에서 이러한 설정을 사용 하 여 응용 프로그램 전달 프레임 워크 Api에 액세스할 준비가 되었습니다. 프로 비전에 대 한 자세한 내용은 참조 하십시오 우리의 [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) 하 고 [앱 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드입니다.

## <a name="implementing-handoff"></a>핸드 오프를 구현합니다.

동일한 개발자 팀 ID를 사용 하 여 서명 되 고 동일한 작업 유형을 지원 되는 앱 간에 사용자 작업을 계속할 수 있습니다. Xamarin.iOS 앱에 핸드 오프를 구현 하려면 사용자 작업 개체를 만들 수 있습니다 (에 있거나 `UIKit` 또는 `AppKit`), 활동을 추적 하는 개체의 상태를 업데이트 하 고 수신 장치에 작업을 계속할 수 없습니다.

### <a name="identifying-user-activities"></a>사용자 활동을 식별

핸드 오프를 구현 하는 첫 번째 단계는 앱을 지 원하는 사용자 작업의 형식을 식별 하는 및 다른 장치에서 연속 작업에 대 한 적합 한 후보가 이러한 작업을 표시 합니다. 예를 들어: 할 일 앱을 하나로 편집 항목을 지원할 수 있습니다 _사용자 활동 유형_, 및 다른 사용 가능한 항목 목록 검색을 지원 합니다.

앱 형식을 만들 수 만큼 사용자 활동은 필요한 앱 제공 하는 모든 함수에 대 한 하나. 각 사용자 활동 형식에 대해 앱 형식의 작업 시작 및 종료 및 다른 장치에서 해당 작업을 계속 하려면 최신 상태 정보를 유지 해야 하는 시기를 추적 해야 합니다.

송신 및 수신 앱 간의 일대일 매핑 없이 동일한 팀 ID로 서명 된 앱에 사용자 작업을 계속할 수 있습니다. 예를 들어, 지정된 된 앱에 네 가지 유형의 다른 장치에서 다른 개별 앱에서 사용 되는 활동을 만들면 됩니다. 이 주로 Mac 버전 (있는 많은 기능 및 함수 포함) 앱을 iOS 앱 간의 각 앱 작고 특정 작업에 집중 합니다.

### <a name="creating-activity-type-identifiers"></a>활동 유형 식별자 만들기

_활동 형식 식별자_ 에 추가 하는 간단한 문자열을 `NSUserActivityTypes` 앱의 배열 **Info.plist** 지정된 된 사용자 활동 형식에 고유 하 게 식별 하는 데 사용 되는 파일. 앱에서 지 원하는 각 작업에 대 한 배열에 하나의 항목이 됩니다. Apple에서 충돌을 방지 하려면 활동 형식 식별자에 대 한 역방향 DNS 스타일 표기법을 사용 하 여 제안 합니다. 예를 들어: `com.company-name.appname.activity` 특정 앱에 대 한 작업 기반 또는 `com.company-name.activity` 여러 앱에서 실행할 수 있는 작업에 대 한 합니다.

활동 유형 식별자를 만들 때 사용 되는 `NSUserActivity` 활동의 type을 식별 하는 인스턴스. 활동은 다른 장치에서 계속 되 면 활동 유형 (앱의 팀 ID)와 함께 작업을 계속 시작 하는 앱을 결정 합니다.

예를 들어 것 이라는 샘플 앱을 만듭니다 **MonkeyBrowser** ([에서 다운로드할](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)). 이 앱에는 각각 웹 브라우저 뷰를 다른 URL이 열려 있는 4 개의 탭에 표시 됩니다. 사용자는 앱을 실행 하는 다른 iOS 장치에서 모든 탭 계속 됩니다.

편집이 동작을 지 원하는 데 필요한 활동 형식 식별자를 만들려면 합니다 **Info.plist** 파일을 전환할 합니다 **원본** 보기. 추가 된 `NSUserActivityTypes` 다음 식별자를 만들고 키:

[![](handoff-images/type01.png "Plist 편집기에서 필요한 식별자 고 NSUserActivityTypes 키")](handoff-images/type01.png#lightbox)

4 개의 새 활동 형식 식별자를 각 예제에 탭에 대 한 하나를 만들었습니다 **MonkeyBrowser** 앱. 자신의 앱을 만들 때의 내용을 바꿉니다는 `NSUserActivityTypes` 앱 특정 활동에 활동 형식 식별자를 사용 하 여 배열을 지원 합니다.

### <a name="tracking-user-activity-changes"></a>사용자 활동 변경 사항 추적

새 인스턴스를 만들 때 합니다 `NSUserActivity` 지정 클래스를 `NSUserActivityDelegate` 활동의 상태 변경 내용을 추적 하는 인스턴스. 예를 들어, 상태 변경 내용을 추적 하려면 다음 코드를 사용할 수 있습니다.:

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

`UserActivityReceivedData` 연속 Stream에 보내는 장치에서 데이터를 수신 하는 메서드가 호출 됩니다. 자세한 내용은 참조는 [지원 연속 스트림을](#Supporting-Continuation-Streams) 섹션 아래.

`UserActivityWasContinued` 메서드 다른 장치는 현재 장치에서 작업 동안 수행 된 경우 호출 됩니다. 할 일 목록에 새 항목을 추가 하는 등의 작업의 유형에 따라 앱에서 보내는 장치 작업을 중단 필요 될 수 있습니다.

`UserActivityWillSave` 활동의 변경 내용이 저장 되 고 로컬로 사용할 수 있는 장치 간에 동기화 전에 메서드를 호출 합니다. 이 메서드를 사용 하 여 마지막 변경에 `UserInfo` 속성의는 `NSUserActivity` 전송 되기 전에 인스턴스.

### <a name="creating-a-nsuseractivity-instance"></a>NSUserActivity 인스턴스 만들기

앱에서 다른 장치에서 계속의 가능성을 제공 하고자 하는 각 활동에서 캡슐화 해야 합니다는 `NSUserActivity` 인스턴스. 앱 필요에 따라 많은 활동을 만들 수 및 해당 작업 하는 기능 및 해당 앱의 기능에 따라 달라 집니다. 예를 들어 메일 앱을 만드는 새 메시지 및 메시지를 읽는 다른 하나의 동작을 만들 수 있습니다.

이 예제에서는 앱에 대 한 새 `NSUserActivity` 탭된 웹 브라우저 뷰 중 하나에서 새 URL을 입력할 때마다 생성 됩니다. 지정 된 탭의 상태를 저장 하는 다음 코드:

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

만드는 새 `NSUserActivity` 사용자 활동 유형 중 하나를 사용 하 여 위에서 만든 및 활동에 대 한 알기 쉬운 제목을 제공 합니다. 인스턴스에 연결 된 `NSUserActivityDelegate` 상태 변경 및 사용자 활동에이 작업이 현재 iOS 알립니다를 시청 하려면 위에서 만든 합니다.

### <a name="populating-the-userinfo-dictionary"></a>UserInfo 사전 채우기

위에서 살펴본 것 처럼를 `UserInfo` 의 속성을 `NSUserActivity` 클래스는를 `NSDictionary` 지정된 된 활동의 상태를 정의 하는 데 사용 되는 키-값 쌍의 합니다. 저장 된 값 `UserInfo` 다음 형식 중 하나 여야 합니다: `NSArray`, `NSData`, `NSDate`, `NSDictionary`를 `NSNull`, `NSNumber`를 `NSSet`, `NSString`, 또는 `NSURL`합니다. `NSURL` iCloud 문서를 가리키는 데이터 값을 받는 장치에서 동일한 문서를 가리키도록 자동으로 조정 됩니다.

위의 예제에서 만든를 `NSMutableDictionary` 개체 및 사용자 지정된 탭에서 현재 표시 된 URL을 제공 하는 단일 키를 사용 하 여 채워집니다. `AddUserInfoEntries` 사용자 활동의 메서드를 수신 하는 장치에서 작업을 복원 하는 데 사용할 데이터를 사용 하 여 활동을 업데이트 사용:

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple 활동 수신 장치에 시기 적절 한 방식으로 전송 되도록 분위기 최소값으로 보내는 정보를 유지 하는 것이 좋습니다. 문서에 연결 하는 이미지를 편집할 수와 같은 큰 정보가 필요한 경우를 사용 해야 연속 스트림을 전송 되어야 합니다. 참조 된 [지원 연속 스트림을](#Supporting-Continuation-Streams) 대 한 자세한 내용은 아래 섹션입니다.

### <a name="continuing-an-activity"></a>작업을 계속할 수 없습니다.

핸드 오프 로컬 iOS 및 OS X 장치에에서 있는 원래 장치에 대 한 물리적 근접성 건의 사용자 활동의 가용성과 동일한 iCloud 계정에 로그인 자동으로 제공 합니다. (팀 ID 및 작업 형식에 따라) 적절 한 앱 및 정보 시스템 사용자가 새 장치에서 작업을 계속 하기로 하는 경우 시작 됩니다 해당 `AppDelegate` 연속 발생 해야 합니다.

첫 번째는 `WillContinueUserActivityWithType` 앱 연속 작업이 시작 되려고 하는 사용자에 게 알릴 수 있도록 호출 됩니다. 다음 코드를 사용 합니다 **AppDelegate.cs** 연속 시작을 처리 하는 예제 앱의 파일:

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

위의 예제에서 각 보기 컨트롤러에 등록 합니다 `AppDelegate` 있고 공용 `PreparingToHandoff` 활동 표시기 및 활동 현재 장치로 전달 될를 사용자에 게 알리는 메시지를 표시 하는 메서드. 예제:

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

합니다 `ContinueUserActivity` 의 `AppDelegate` 실제로 지정된 된 활동을 계속 호출 됩니다. 예제 앱 다시에서:

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

공용 `PerformHandoff` 각 뷰 컨트롤러의 메서드에 실제로 전달을 수행 하 고 현재 장치에서 작업을 복원 합니다. 예의 경우 사용자가 다른 장치에서 검색 하는 지정 된 탭에 동일한 URL을 표시 합니다. 예제:

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

`ContinueUserActivity` 메서드를 포함 한 `UIApplicationRestorationHandler` 작업 다시 시작 하는 기반으로 문서 또는 응답자 호출할 수 있습니다. 전달 해야 합니다.는 `NSArray` 또는 호출 될 때 복원 처리기에 가능한 개체입니다. 예를 들어:

```csharp
completionHandler (new NSObject[]{Tab4});
```

에 전달 하는 각 개체에 대 한 해당 `RestoreUserActivityState` 메서드가 호출 됩니다. 각 개체의 데이터를 사용할 수는 `UserInfo` 자체 상태를 복원 하는 사전입니다. 예를 들어:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

문서 기반 앱을 구현 하는 경우는 `ContinueUserActivity` 메서드 또는 해당 반환 `false`를 `UIKit` 또는 `AppKit` 자동으로 작업을 재개할 수 있습니다. 참조 된 [문서 기반 앱에서 지 원하는 핸드 오프](#Supporting-Handoff-in-Document-Based-Apps) 자세한 내용은 아래 섹션입니다.

### <a name="failing-handoff-gracefully"></a>핸드 오프를 정상적으로 실패

핸드 오프 컬렉션 느슨하게 연결 된 iOS 및 OS X 장치 간에 정보의 전송에 의존 하므로 전송 프로세스를 따라 실패할 수 있습니다. 이러한 오류를 정상적으로 처리 하 고 발생 하는 경우 모든 사용자의 앱을 디자인 해야 합니다.

오류가 발생 합니다 `DidFailToContinueUserActivitiy` 메서드는 `AppDelegate` 호출 됩니다. 예를 들어:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

제공 된 사용 해야 `NSError` 오류에 대 한 사용자에 게 정보를 제공 합니다.

## <a name="native-app-to-web-browser-handoff"></a>네이티브 앱-웹 브라우저 핸드 오프

사용자는 원하는 장치에 설치 하는 적절 한 네이티브 앱 필요 없이 작업을 계속 하려고 합니다. 상황에 따라 웹 기반 인터페이스에 필요한 기능을 제공할 수 있습니다 하 고 작업을 계속 계속할 수 있습니다. 예를 들어, 사용자의 전자 메일 계정 메시지를 읽고 작성에 대 한 웹 기반 UI를 제공할 수 있습니다.

원래, 네이티브 앱의 URL을 알고 있는 경우 웹 인터페이스 (및 계속 되 고 지정된 된 항목을 식별 하는 데 필요한 구문)에 대 한이 정보를 인코딩할 수는 `WebpageURL` 의 속성을 `NSUserActivity` 인스턴스. 수신 장치에 설치 하 여 연속 작업을 처리할 적절 한 네이티브 앱에 없는 경우에 제공 된 웹 인터페이스를 호출할 수 있습니다.

## <a name="web-browser-to-native-app-handoff"></a>네이티브 앱 핸드 오프를 웹 브라우저

사용자가 원래 장치에서 웹 기반 인터페이스를 사용 하는 경우 및 수신 장치에서 네이티브 앱의 도메인 부분을 클레임을 `WebpageURL` 속성을 시스템 핸들 연속 작업이 해당 앱을 사용 합니다. 새 장치 받을 `NSUserActivity` 활동 유형으로 표시 하는 인스턴스 `BrowsingWeb` 및 `WebpageURL` 사용자를 방문한 URL이 포함 됩니다는 `UserInfo` 사전 비어 있게 됩니다.

이 유형의 핸드 오프에 참여 하는 앱에 대 한 도메인에 claim 해야 합니다는 `com.apple.developer.associated-domains` 형식 사용 하 여 자격 `<service>:<fully qualified domain name>` (예: `activity continuation:company.com`).

지정된 된 도메인 일치 하는 경우는 `WebpageURL` 해당 도메인에서 웹 사이트에서 승인 된 앱 Id의 목록을 다운로드 하는 속성의 값을 전달 합니다. 웹 사이트 라는 JSON 파일에 서명 된 승인 된 Id 목록을 제공 해야 합니다 **apple-앱-사이트-연결** (예를 들어 `https://company.com/apple-app-site-association`).

JSON 파일에 폼의 앱 Id의 목록을 지정 하는 사전 `<team identifier>.<bundle identifier>`합니다. 예를 들어:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

JSON 파일에 서명 (올바른 갖도록 `Content-Type` 의 `application/pkcs7-mime`)를 사용 합니다 **터미널** 앱 및 `openssl` 인증서 및 iOS에서 신뢰할 수 있는 인증 기관에서 발급 한 키를 사용 하 여 명령을 (참조 [ http://support.apple.com/kb/ht5012 ](http://support.apple.com/kb/ht5012) 목록은). 예를 들어:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

합니다 `openssl` 명령에서 웹 사이트에 배치 하는 부호 있는 JSON 파일을 출력 합니다 **apple-앱-사이트-연결** URL입니다. 예를 들어:

```csharp
https://example.com/apple-app-site-association.
```

앱 활동을 받지는 해당 `WebpageURL` 도메인이 해당 `com.apple.developer.associated-domains` 자격. 만 `http` 고 `https` 프로토콜 지원, 기타 프로토콜 예외가 발생 합니다.

## <a name="supporting-handoff-in-document-based-apps"></a>문서 기반 앱에서 전달 지원

IOS 및 OS X에서 설명한 것 처럼 문서 기반 앱 자동으로 지원 iCloud 기반 문서 핸드 오프 앱의 **Info.plist** 파일에는 `CFBundleDocumentTypes` 의 키 `NSUbiquitousDocumentUserActivityType`합니다. 예를 들어:

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

이 예제에서 문자열은 추가 작업의 이름으로는 역방향 DNS 앱 지정자. 이러한 방식으로 입력 하는 경우 활동 형식 항목에서 반복할 필요 하지 않습니다 합니다 `NSUserActivityTypes` 배열을 합니다 **Info.plist** 파일.

자동으로 만들어진 사용자 활동 개체 (문서를 통해 사용할 수 있는 `UserActivity` 속성) 앱의 다른 개체에서 참조 하 고 연속 작업의 상태를 복원 하는 데 있습니다. 예를 들어 추적 항목 선택 및 문서를 놓습니다. 이 작업을 설정 해야 `NeedsSave` 속성을 `true` 상태를 변경 하 고 업데이트 될 때마다 합니다 `UserInfo` 사전에는 `UpdateUserActivityState` 메서드.

`UserActivity` 속성 모든 스레드에서 사용할 수와 icloud와의 입출력 이동함에 따라 문서를를 동기화 상태로 유지할 수 있도록 키-값 관찰 (KVO) 프로토콜을 준수 합니다. `UserActivity` 문서를 닫을 때 속성 무효화 됩니다.

자세한 내용은 Apple의를 참조 하세요 [문서 기반 앱에서 사용자 작업 지원](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5) 설명서.

## <a name="supporting-handoff-in-responders"></a>응답자의 전달 지원

응답자를 연결할 수 있습니다 (에서 상속 `UIResponder` iOS에서 또는 `NSResponder` OS X에서)을 설정 하 여 작업에 해당 `UserActivity` 속성입니다. 시스템에서 자동으로 저장 합니다 `UserActivity` 적절 한 시간, 응답자의 호출에서 속성 `UpdateUserActivityState` 현재 데이터를 사용 하 여 사용자 활동 개체를 추가 하는 메서드를 `AddUserInfoEntriesFromDictionary` 메서드.

## <a name="supporting-continuation-streams"></a>연속 스트림 지원

활동을 계속 해야 하는 정보의 양이 없습니다 효율적으로 전송할 초기 핸드 오프 페이로드에 경우가 있을 수 있습니다. 이러한 상황에서 수신 응용 프로그램 자체와 데이터를 전송 하려면 원래 앱 간에 하나 이상의 스트림을 설정할 수 있습니다.

원래 앱 설정이 적용 합니다 `SupportsContinuationStreams` 의 속성을 `NSUserActivity` 인스턴스를 `true`. 예를 들어:

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

수신 응용 프로그램을 호출할 수 있습니다는 `GetContinuationStreams` 메서드를 `NSUserActivity` 에서 해당 `AppDelegate` 스트림을 연결할. 예를 들어:

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

사용자 작업 대리자를 호출 하 여 스트림을 받는 원본 장치에서 해당 `DidReceiveInputStream` 데이터를 제공 하는 방법을 다시 시작 장치에서 사용자 작업을 계속 하도록 요청 합니다.

사용 하 여를 `NSInputStream` 스트림 데이터에 대 한 읽기 전용 액세스를 제공 하 고 `NSOutputStream` 쓰기 전용 액세스를 제공 합니다. 요청 및 응답 방식 이며, 여기서 수신 앱에서 더 많은 데이터를 요청 하 고 원래 앱 제공 스트림은 사용 해야 합니다. 원래 장치에서 출력 스트림에 기록 된 데이터는 계속 장치의 입력 스트림에서 읽을 있도록 그 반대로 가능 합니다.

연속 Stream 필요한 있는 경우에도 있어야는 최소한의 뒤로 및 명시 두 앱 간에 통신 합니다.

자세한 내용은 Apple의을 참조 하세요 [사용 하 여 연속 스트림을](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13) 설명서.

## <a name="handoff-best-practices"></a>핸드 오프에 대 한 유용한 정보

핸드 오프를 통해 사용자 작업을 원활 하 게 연속의 성공적인 구현을 관련 된 모든 다양 한 구성으로 인해 신중 하 게 디자인을 해야합니다. Apple에서 제안 핸드 오프 사용 앱에 대 한 다음 모범 사례를 채택 하 게 합니다.

- 계속 실행 되어야 하는 활동의 상태를 연결할 가능한 가장 작은 페이로드가 필요 하도록 사용자 활동을 디자인 합니다. 클수록 페이로드, 더 긴 시작 하려면 연속을 걸립니다.
- 성공한 연속에 대 한 데이터의 대용량을 전송 해야 하는 경우 비용 관련 구성 및 네트워크 오버 헤드를 고려 합니다.
- 일반적으로 대형 Mac 앱의 사용자 활동에서 처리 되는 몇 가지 작은 iOS 장치에서 작업 관련 앱을 만드는 것입니다. 서로 다른 응용 프로그램 및 OS 버전을 함께 사용 하거나 적절 하 게 실패 설계 되어야 합니다.
- 활동 형식에서 지정 하는 경우 충돌을 방지 하려면 역방향 DNS 표기법을 사용 합니다. 형식 정의에 해당 이름이 포함 되어야 지정된 된 앱 관련 활동 인 경우 (예를 들어 `com.myCompany.myEditor.editing`). 정의에서 작업에서 여러 앱에서 작업할 수 있는 경우 drop 앱 이름 (예를 들어 `com.myCompany.editing`).
- 앱 사용자 동작 상태를 업데이트 해야 하는 경우 (`NSUserActivity`)을 설정 합니다 `NeedsSave` 속성을 `true`입니다. 핸드 오프를 대리자의 적절 한 시간에 호출 `UserActivityWillSave` 업데이트할 수 있도록 하는 메서드는 `UserInfo` 필요에 따라 사전입니다.
- 전달 프로세스 수신 장치에 즉시 초기화 되지 않는 수 있으므로 구현 해야 합니다 `AppDelegate`의 `WillContinueUserActivity` 및 연속 작업을 시작 하는 사용자에 게 알립니다.

## <a name="example-handoff-app"></a>핸드 오프 앱의 예

포함 된 핸드 오프를 사용 하 여 Xamarin.iOS 앱에서의 예로 [ **MonkeyBrowser** ](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/) 이 가이드를 사용 하 여 샘플 앱입니다. 앱에 사용자 지정된 작업 형식을 사용 하 여 각 웹을 검색 하는 데 사용할 수 있는 네 개의 탭이 있습니다: 날씨, 즐겨찾기, 휴식 시간 및 작업.

새 URL 및 탭을 입력할 때 모든 탭 합니다 **이동** 단추를 새 `NSUserActivity` 현재 사용자를 검색 하는 URL이 포함 된 해당 탭에 대해 생성 됩니다.

[![](handoff-images/handoff01.png "핸드 오프 앱의 예")](handoff-images/handoff01.png#lightbox)

다른 사용자의 장치에는 **MonkeyBrowser** 앱 설치가 동일한 사용자 계정을 사용 하 여 iCloud에 로그인는 동일한 네트워크 및 위의 장치에 가까운 근접성에 전달 작업에 표시할 홈 (왼쪽 아래 모서리에) 화면:

[![](handoff-images/handoff02.png "홈 화면의 왼쪽 아래 모서리에 표시 되는 핸드 오프 동작")](handoff-images/handoff02.png#lightbox)

전달 아이콘 끌 상향 하는 경우 앱을 시작할 수는 및에 지정 된 사용자 활동은 `NSUserActivity` 새 장치에서 계속 됩니다.

[![](handoff-images/handoff03.png "새 장치에서 계속 사용자 활동")](handoff-images/handoff03.png#lightbox)

경우 사용자 작업에 성공적으로 보낸 다른 Apple 장치에서 보내는 장치 `NSUserActivity` 에 대 한 호출을 받을 합니다 `UserActivityWasContinued` 메서드를 해당 `NSUserActivityDelegate` 사용자 활동 간에 성공적으로 전송 된 것을 장치입니다.

## <a name="summary"></a>요약

이 문서에서는 여러 사용자의 Apple 장치 간의 사용자 작업을 계속 하는 데 핸드 오프 framework 소개를 제공 했습니다. 다음으로 사용 하도록 설정 하 고 Xamarin.iOS 앱에 핸드 오프를 구현 하는 방법에 알아보았습니다. 마지막으로, 다양 한 유형의 사용 가능한 핸드 오프 연속 및 핸드 오프 모범 사례를 설명합니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [MonkeyBrowser 샘플](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [새로운 iOS 9.0에서](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper 가이드](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit 사용자 인터페이스 지침](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit 프레임 워크 참조](https://developer.apple.com/library/ios/home_kit_framework_ref)
