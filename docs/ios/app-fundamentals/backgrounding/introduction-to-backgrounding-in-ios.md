---
title: IOS의 Backgrounding 소개
description: '이 문서에서는 iOS의 backgrounding 설명: 응용 프로그램 상태, 응용 프로그램 수명 주기 메서드 및 백그라운드 앱 새로 고침 합니다.'
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 804a99817f664989bbac67a4c662357f4ee628c5
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242279"
---
# <a name="introduction-to-backgrounding-in-ios"></a>IOS의 Backgrounding 소개

iOS는 백그라운드 처리를 매우 긴밀 하 게 규제 및 구현 하는 세 가지 방법을 제공 합니다.

-  **백그라운드 작업을 등록할** -iOS 응용 프로그램을 이동할 때 작업을 중단할 필요가 요청할 수 있는 응용 프로그램을 중요 한 작업을 완료 하는 경우. 예를 들어, 응용 프로그램 사용자 로그인을 완료 하거나 대용량 파일 다운로드를 완료 해야 할 수 있습니다.
-  **백그라운드 필요한 응용 프로그램으로 등록** -앱과 같은 특정 유형의 응용 프로그램을 알려진 특정 backgrounding 요구 사항으로 등록할 수 있습니다 *오디오* 하십시오 *VoIP* ,  *외부 액세서리* 하십시오 *Newsstand* , 및 *위치* 합니다. 이러한 응용 프로그램에 등록 된 응용 프로그램 종류의 매개 변수 내에 있는 작업을 수행 하는 이러한 권한을 처리 하는 연속 백그라운드를 허용 됩니다.
-  **백그라운드 업데이트를 사용 하도록 설정** -응용 프로그램에서 사용 하 여 백그라운드 업데이트를 트리거할 수 있습니다 *지역 모니터링* 에 대 한 수신 대기 하거나 *중요 한 위치를 변경* 합니다. IOS 7 기준으로 응용 프로그램을 사용 하 여 백그라운드에서 콘텐츠를 업데이트 하려면 등록할 수도 있습니다 *백그라운드 페치* 하거나 *Remote Notifications* 합니다.


## <a name="application-states-and-application-delegate-methods"></a>응용 프로그램 상태 및 응용 프로그램 대리자 메서드

IOS에서 처리 하는 백그라운드에 대 한 코드를 살펴보기 전에 iOS 응용 프로그램의 수명 주기 backgrounding 어떻게 영향을 이해 해야 합니다.

IOS 응용 프로그램 수명 주기는 응용 프로그램 상태 및 이들 간의 이동에 대 한 메서드 컬렉션. 응용 프로그램 사용자의 동작 및 응용 프로그램의 backgrounding 요구에 따라 상태 간에 전환 됩니다. 이동은 다음 다이어그램에 나와 있습니다.

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "응용 프로그램 상태 및 응용 프로그램 대리자 메서드 다이어그램")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

-  **실행 되 고 있지** -응용 프로그램이 장치에 아직 시작 되지에 있습니다.
-  **실행/활성** -응용 프로그램 화면에서 이며 전경에서 코드를 실행 합니다.
-  **비활성** -응용 프로그램은 들어오는 전화 통화, 텍스트 또는 기타 중단에 의해 중단 됩니다.
-  **Backgrounded** -응용 프로그램이 백그라운드로 이동 하 고 계속 백그라운드 코드를 실행 합니다.
-  **Suspended** 백그라운드에서 실행 하는 코드를 응용 프로그램에 없는 경우 앱은 모든 코드가 완료 된 경우 수- *일시 중단 된* OS에서. 일시 중단 된 응용 프로그램의 프로세스를 활성 상태로 유지 하지만 응용 프로그램에서이 상태 코드를 실행할 수 없는.
-  **하지 실행/종료 (Rare)를 반환** -경우에 따라 응용 프로그램의 프로세스 파괴 되 고 응용 프로그램에 반환 합니다 *실행 되 고 있지* 상태입니다. 메모리 부족 상황에서 발생 합니다. 아니면 사용자는 수동으로 응용 프로그램을 종료 합니다.


멀티태스킹 지원이 도입 된 이후 iOS 거의 유휴 응용 프로그램을 종료 하 고 대신 해당 프로세스를 유지 *Suspended* 메모리에 있습니다. 응용 프로그램의 프로세스를 활성 상태로 유지 하면 응용 프로그램이 다음에 사용자가 신속 하 게 시작 합니다. 응용 프로그램에서 자유롭게 이동할 수 있는 의미는 *일시 중단* 다시 상태로 합니다 *Backgrounded* 시스템 리소스에 그리지 않고 상태. iOS 7 장치가 절전 모드로 전환 등 사용자 상호 작용 없이 백그라운드에서 직접 콘텐츠를 업데이트 하는 경우 백그라운드 작업을 일시 중지 하려면 응용 프로그램을 사용 하도록 설정 하는 새 Api 사용 하 여이 기능을 이용 합니다. 새 Api를 다루지 [iOS Backgrounding 기술](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)합니다.

## <a name="application-lifecycle-methods"></a>응용 프로그램 수명 주기 메서드

IOS 앱 상태를 변경할 때의 이벤트 메서드를 통해 응용 프로그램에 알리는 `AppDelegate` 클래스:

-  `OnActivated` 응용 프로그램 시작은 처음으로 호출 됩니다-앱이 전경에 다시 들어올 때마다 및 합니다. 앱을 열 때마다 실행 해야 하는 코드를 삽입할 위치입니다.
-  `OnResignActivation` -사용자가 텍스트 또는 전화 통화와 같은 작업을 중단을 수신 하는 경우이 메서드가 호출 되 고 앱을 일시적으로 비활성화 됩니다. 사용자는 전화 통화를 허용 해야, 백그라운드에 앱 전송 됩니다.
-  `DidEnterBackground` -이 메서드 종료 준비를 약 5 초 응용 프로그램에 제공 앱 backgrounded 상태가 될 때 호출 됩니다. 사용자 데이터 및 작업을 저장 하 고 화면에서 중요 한 정보를 제거 하려면이 시간을 사용 합니다.
-  `WillEnterForeground` 사용자 backgrounded 또는 일시 중단 된 응용 프로그램에 반환 하 고, 전경으로 시작 하는- `WillEnterForeground` 호출 합니다. 리하이드레이션 중에 저장 된 상태에서 포그라운드를 되려면 앱을 준비 하는 시간이이 `DidEnterBackground` 합니다.  `OnActivated` 이 메서드가 완료 된 후에 즉시 호출 됩니다.
-  `WillTerminate` -응용 프로그램이 종료 되 고 해당 프로세스에서 제거 됩니다. 만이 메서드는 사용자는 수동으로 backgrounded 응용 프로그램을 종료 하는 경우 또는 메모리가 부족 하면 멀티태스킹 장치 OS 버전에 사용할 수 없는 경우에 호출 됩니다. 종료 하는 일시 중단 된 응용 프로그램은 호출 하지 `WillTerminate` 합니다.


다음 다이어그램은 어떻게 응용 프로그램 상태 및 수명 주기 메서드를 서로 맞추는:

 [![](introduction-to-backgrounding-in-ios-images/image2.png "이 다이어그램에 표시 하는 방법을 응용 프로그램 상태 및 수명 주기 메서드 연동")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>IOS의 Backgrounding에 대 한 사용자 정의 컨트롤

iOS 7 사용자 backgrounded 응용 프로그램의 상태를 더 많이 제어할 수 있도록 몇 가지 기능이 도입 되었습니다. 응용 프로그램 전환기와 백그라운드 앱 새로 고침 설정을 응용 프로그램 수명에 영향을 줍니다.

### <a name="app-switcher"></a>응용 프로그램 전환기

응용 프로그램 전환기에는 iOS 7에에서 도입 된 중요 한 제어 기능입니다. 두 번 탭 하 여 시작 합니다 **홈** 단추 및 프로세스는 활성 응용 프로그램을 표시:

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "응용 프로그램 전환기를 사용 하 여 앱 간에 이동")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

응용 프로그램 전환기를 사용 하 여 사용자가 스크롤할 수 backgrounded 및 일시 중단 된 모든 응용 프로그램의 스냅숏. 응용 프로그램 탭 전경으로 시작 합니다. 살짝 해당 프로세스를 종료 백그라운드에서 응용 프로그램을 제거 합니다. 응용 프로그램 전환기를 자세히 살펴보면 수행 됩니다는 [iOS 응용 프로그램 수명 주기 데모](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) 다음 섹션에 있습니다.

> [!IMPORTANT]
> 응용 프로그램 전환기 backgrounded 및 일시 중단 된 응용 프로그램 간의 차이 표시 하지 않습니다.



### <a name="background-app-refresh-settings"></a>백그라운드 앱 새로 고침 설정

iOS 7 응용 프로그램에 대 한 backgrounding 옵트아웃 하는 사용자를 허용 하 여 응용 프로그램 수명 주기를 통해 사용자 정의 컨트롤을 증가 [백그라운드 처리에 대 한 등록](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md)합니다. *이 응용 프로그램에서 백그라운드 작업을 실행 해도*합니다.

사용자로 이동 하 여이 설정을 변경할 수 있습니다 <span class="uiitem">설정 > 일반 > 하면서 앱 새로 고침</span> 및 선택한 응용 프로그램에 대 한 backgrounding 권한을 편집 합니다. 백그라운드 앱 새로 고침을 off로 설정 되어 응용 프로그램 백그라운드를 입력 하면 즉시 일시 중단 되며 모든 백그라운드 처리를 수행 하지 못했습니다.

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "백그라운드 앱 새로 고침 설정")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

개발자가 사용 하 여 백그라운드 응용 프로그램 새로 고침 상태를 확인할 수는 `BackgroundRefreshStatus` API. 예를 들어 참조를 [배경 새로 고침 설정 확인 레](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/check_background_refresh_setting)합니다.

IOS 응용 프로그램 수명 주기 및 응용 프로그램 수명 주기를 제어 하기 위한 기능을의 기본 사항을 살펴보았습니다. 다음으로, 실행 중인 iOS 응용 프로그램 수명 주기를 확인해 보겠습니다.

