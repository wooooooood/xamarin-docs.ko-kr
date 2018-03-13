---
title: "IOS에서 Backgrounding 소개"
ms.topic: article
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c4eed99533ba1aca1bd5ba23078866909330b542
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-backgrounding-in-ios"></a>IOS에서 Backgrounding 소개

iOS는 백그라운드 매우 밀접 하 게 처리를 조정 하 고 구현 하는 세 가지 방법을 제공:

-  **백그라운드 작업을 등록할** -iOS 응용 프로그램을 배경으로 이동 하면 작업을 중단 하지를 요청할 수 있는 응용 프로그램을 중요 한 작업을 완료 하는 경우. 예를 들어 응용 프로그램 사용자를 로그인을 완료 하거나 큰 파일 다운로드를 완료 해야 할 수 있습니다.
-  **배경 필요한 응용 프로그램으로 등록** -응용 프로그램 같은 특정 유형의 응용 프로그램, 알려진 특정 backgrounding 요구 사항으로 등록할 수 있습니다 *오디오* , *VoIP* ,  *외부 액세서리* , *뉴스 스탠드* , 및 *위치* 합니다. 이러한 응용 프로그램으로 등록 된 응용 프로그램 종류의 매개 변수 내에 있는 작업을 수행 하 고 권한을 처리 계속 해 서 백그라운드를 허용 됩니다.
-  **백그라운드 업데이트를 사용 하도록 설정** -응용 프로그램으로 백그라운드 업데이트를 트리거할 수 *영역 모니터링* 또는 대 한 수신 대기 하 여 *중요 한 위치 변경* 합니다. IOS 7 기준으로 응용 프로그램 콘텐츠를 사용 하 여 백그라운드에서 업데이트를 등록할 수도 있습니다 *배경 인출* 또는 *원격 알림* 합니다.


## <a name="application-states-and-application-delegate-methods"></a>응용 프로그램 상태와 응용 프로그램 대리자 메서드

백그라운드 처리에 iOS에 대 한 코드를 살펴보기 전에 backgrounding 어떻게 영향을 줌 iOS 응용 프로그램의 수명 주기를 이해 해야 합니다.

IOS 응용 프로그램 수명 주기는 응용 프로그램 상태와 서로 이동할 메서드가의 컬렉션입니다. 응용 프로그램 사용자의 동작 및 응용 프로그램의 backgrounding 요구에 따라 상태를 전환 합니다. 다음 다이어그램은 이동 보여 줍니다.

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "응용 프로그램 상태 및 대리자 메서드를 응용 프로그램 다이어그램")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

-  **실행 되 고 있지** -응용 프로그램이 장치에 아직 시작 되지 했습니다.
-  **실행/활성** -응용 프로그램의 화면에 표시 되 고 포그라운드에서 코드를 실행 합니다.
-  **비활성** -응용 프로그램 중단 된 걸려오는 전화, 텍스트 또는 기타 중단 합니다.
-  **Backgrounded** -응용 프로그램을 배경으로 이동 하 고 백그라운드 코드 실행을 계속 합니다.
-  **Suspended** -모든 코드가 완료 되는 경우 앱에서 됩니다 또는 응용 프로그램은 백그라운드에서 실행 하는 모든 코드가 없는 경우 *Suspended* 운영 체제에서 합니다. 일시 중단 된 응용 프로그램의 프로세스는 활성 상태로 유지 하지만 응용 프로그램이이 상태 코드를 실행할 수 수 없습니다.
-  **하지 실행/종료 (Rare)를 반환** -경우에 따라서는 응용 프로그램의 프로세스 제거 되 고 응용 프로그램에 반환 된 *실행 되 고 있지* 상태입니다. 메모리 부족 상황에서 이런 또는 사용자 응용 프로그램을 수동으로 종료 합니다.


멀티태스킹 지원 도입 된 이후 iOS 거의 유휴 응용 프로그램을 종료 하 고 대신 해당 프로세스를 계속 *Suspended* 메모리에 있습니다. 응용 프로그램이 다음에 사용자가 열 신속 하 게 실행 되도록 응용 프로그램의 프로세스 활성화 된 상태로 유지 합니다. 또한 응용 프로그램에서 자유롭게 이동할 수 있는 의미는 *Suspended* 다시에 상태는 *Backgrounded* 시스템 리소스에 그리기 없이 상태입니다. iOS 7 장치를 절전 모드, 사용자 상호 작용 없이 백그라운드에서 직접 업데이트 콘텐츠 발생 하는 경우 백그라운드 작업을 일시 중지 하는 응용 프로그램을 사용할 수 있는 새로운 Api와 함께이 기능을 이용 합니다. 새로운 Api를 다룰 예정 [iOS Backgrounding 기술을](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)합니다.

## <a name="application-lifecycle-methods"></a>응용 프로그램 수명 주기 메서드

IOS 응용 프로그램에 게 알리는 이벤트 메서드를 통해 응용 프로그램 상태가 변경 되 면는 `AppDelegate` 클래스:

-  `OnActivated` -이 응용 프로그램 시작은 처음으로 이라고 될 때마다 응용 프로그램은 전경으로 반환 됩니다. 에 앱을 열 때마다 실행 하는 코드를 배치할 위치입니다.
-  `OnResignActivation` -사용자가 중단 예: 텍스트 또는 전화 통화를 받을 경우이 메서드가 호출 되 고 응용 프로그램은 일시적으로 비활성화. 사용자는 전화 통화를 허용 해야, 응용 프로그램 백그라운드도 전송 됩니다.
-  `DidEnterBackground` -이 메서드 앱 backgrounded 상태가 될 때 호출 가능한 종료에 대 한 준비 하는 응용 약 5 초를 제공 합니다. 이 시간을 사용 하 여 사용자 데이터 및 작업을 저장 하 고 화면에서 중요 한 정보를 제거 합니다.
-  `WillEnterForeground` -사용자는 backgrounded 또는 일시 중단 된 응용 프로그램에 반환 하 고, 전경에 시작 `WillEnterForeground` 호출 됩니다. 이 시간 중에 저장 된 모든 상태를 리하이드레이션 하 여 전경을 취해야 할 앱을 준비 하는 `DidEnterBackground` 합니다.  `OnActivated` 이 메서드가 완료 된 후 바로 호출 됩니다.
-  `WillTerminate` -응용 프로그램이 종료 되 고 해당 프로세스에서 제거 됩니다. 만이 메서드 또는 사용자는 수동으로 backgrounded 응용 프로그램을 종료 하는 경우 메모리가 부족, 멀티태스킹을 장치 또는 운영 체제 버전에서 사용할 수 없는 경우 호출 됩니다. 일시 중단 된 응용 프로그램 가져오기 종료를 호출 하지 `WillTerminate` 합니다.


다음 다이어그램에서는 방법을 응용 프로그램 상태 및 수명 주기 메서드 종합:

 [![](introduction-to-backgrounding-in-ios-images/image2.png "이 다이어그램에서는 방법을 응용 프로그램 상태와 종합 수명 주기 메서드")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>IOS에서 Backgrounding에 대 한 사용자 정의 컨트롤

iOS 7에는 사용자에 게 제공 backgrounded 응용 프로그램의 상태를 보다 상세하게 몇 가지 기능이 도입 되었습니다. 응용 프로그램 전환기와 백그라운드 응용 프로그램 새로 고침 설정이 응용 프로그램 수명에 영향을 줍니다.

### <a name="app-switcher"></a>응용 프로그램 전환기

응용 프로그램 전환기 iOS 7에에서 도입 된 중요 한 제어 기능은입니다. 두 번 눌러에 의해 시작 되는 **홈** 단추를 선택한 프로세스 연결이 유지 된 응용 프로그램을 표시 합니다.

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "응용 프로그램 전환기를 사용 하 여 앱 간에 이동")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

응용 프로그램 전환기, 사용자가 모든 backgrounded 및 일시 중단 된 응용 프로그램의 스냅숏을 스크롤할 수 있습니다. 응용 프로그램을 눌러 전경으로 시작 합니다. 넘기기가 해당 프로세스를 종료 하 게 백그라운드에서 응용 프로그램을 제거 합니다. 자세히 보기에 응용 프로그램 전환기 해당 메뉴로 이동 합니다는 [iOS 응용 프로그램 수명 주기 데모](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) 다음 섹션에 있습니다.

> [!IMPORTANT]
> **참고**: The 응용 프로그램 전환기 backgrounded 및 일시 중단 된 응용 프로그램 간의 차이 표시 하지 않습니다.



### <a name="background-app-refresh-settings"></a>백그라운드 응용 프로그램 새로 고침 설정

iOS 7 응용 프로그램에 대 한 backgrounding 취소 하기 위해 사용자가 허용 하 여 응용 프로그램 수명 주기에 대 한 사용자 정의 컨트롤을 증가 [백그라운드 처리에 대 한 등록](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md)합니다. *응용 프로그램이 백그라운드 작업을 실행 해도*합니다.

사용자가 이동 하 여이 설정을 변경할 수 <span class="uiitem">설정 > 일반 > 하면서 응용 프로그램 새로 고침</span> 및 선택한 응용 프로그램에 대 한 backgrounding 권한을 편집 합니다. 하면서 응용 프로그램 새로 고침을 off로 설정 하는 경우 응용 프로그램은 백그라운드 들어갈 때 즉시 일시 중단 되며 백그라운드 처리를 수행할 수 없습니다.

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "백그라운드 응용 프로그램 새로 고침 설정")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

개발자와 백그라운드 새로 고침 응용 프로그램 상태를 확인할 수는 `BackgroundRefreshStatus` API입니다. 예를 들어 참조는 [백그라운드 새로 고침 설정을 확인 레시피](https://developer.xamarin.com/recipes/ios/multitasking/check_background_refresh_setting/)합니다.

설명한는 기본적인 iOS 응용 프로그램 수명 주기 및 응용 프로그램 수명 주기를 제어 하기 위한 기능입니다. 다음으로 iOS 응용 프로그램 수명 주기 작업에 살펴보겠습니다.

