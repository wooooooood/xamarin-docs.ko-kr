---
title: iOS의 Backgrounding 소개
description: '이 문서에서는 iOS의 backgrounding: 응용 프로그램 상태, 응용 프로그램 수명 주기 방법 및 백그라운드 앱 새로 고침에 대해 설명 합니다.'
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/24/2018
ms.openlocfilehash: 9fe508d5b0f8d15a26f02b110763cc8e3f4a2e25
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292141"
---
# <a name="introduction-to-backgrounding-in-ios"></a>iOS의 Backgrounding 소개

iOS는 백그라운드 처리를 매우 긴밀 하 게 규제 하 고 구현 하는 세 가지 방법을 제공 합니다.

- **백그라운드 작업 등록** -응용 프로그램이 중요 한 작업을 완료 해야 하는 경우 응용 프로그램이 백그라운드에서 이동할 때 iOS가 작업을 중단 하지 않도록 요청할 수 있습니다. 예를 들어 응용 프로그램에서 사용자의 로깅을 완료 하거나 많은 파일 다운로드를 완료 해야 할 수 있습니다.
- **백그라운드에서 필요한 응용 프로그램으로 등록** -앱은 *오디오* , *VoIP* , *외부 액세서리* , *Newsstand* 와 같은 알려진 특정 backgrounding 요구 사항이 있는 특정 유형의 응용 프로그램으로 등록할 수 있습니다. 및 *위치* . 이러한 응용 프로그램은 등록 된 응용 프로그램 유형의 매개 변수 내에 있는 작업을 수행 하는 동안에도 연속 백그라운드 처리 권한이 허용 됩니다.
- **백그라운드 업데이트 사용** -응용 프로그램은 *지역 모니터링* 을 사용 하거나 *중요 한 위치 변경을* 수신 하 여 백그라운드 업데이트를 트리거할 수 있습니다. IOS 7에서 응용 프로그램은 *백그라운드 페치* 또는 *원격 알림을* 사용 하 여 백그라운드에서 콘텐츠를 업데이트 하도록 등록할 수도 있습니다.


## <a name="application-states-and-application-delegate-methods"></a>응용 프로그램 상태 및 응용 프로그램 대리자 메서드

IOS에서 백그라운드 처리에 대 한 코드를 살펴보기 전에 backgrounding iOS 응용 프로그램의 수명 주기에 미치는 영향을 이해 해야 합니다.

IOS 응용 프로그램 수명 주기는 응용 프로그램 상태와 응용 프로그램 간에 이동 하는 방법을 모아 놓은 것입니다. 응용 프로그램은 사용자의 동작과 응용 프로그램의 backgrounding 요구 사항에 따라 상태 사이를 전환 합니다. 이동은 다음 다이어그램에 나와 있습니다.

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "응용 프로그램 상태 및 응용 프로그램 대리자 메서드 다이어그램")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

- **실행 중이 아님** -장치에서 응용 프로그램이 아직 시작 되지 않았습니다.
- **실행 중/활성** -응용 프로그램이 화면에 있고 포그라운드에서 코드를 실행 하 고 있습니다.
- **비활성** -수신 전화 통화, 텍스트 또는 기타 중단에 의해 응용 프로그램이 중단 됩니다.
- **Backgrounded** -응용 프로그램이 백그라운드로 이동 하 여 백그라운드 코드 실행을 계속 합니다.
- **Suspended** -응용 프로그램에 백그라운드에서 실행할 코드가 없거나 모든 코드가 완료 된 경우 OS에 의해 앱이 *일시 중단* 됩니다. 일시 중단 된 응용 프로그램의 프로세스는 활성 상태로 유지 되지만 응용 프로그램은이 상태의 코드를 실행할 수 없습니다.
- **실행 중/종료 안 함 (드물게)** -때로는 응용 프로그램의 프로세스가 소멸 되 고 응용 프로그램이 *실행 중 상태가 아닌* 상태로 돌아갑니다. 이는 메모리 부족 상황에서 발생 하거나 사용자가 수동으로 응용 프로그램을 종료 하는 경우에 발생 합니다.


멀티태스킹을 지원 하므로 iOS는 거의 유휴 응용 프로그램을 종료 하 고 대신 메모리에서 프로세스를 *일시 중단* 된 상태로 유지 합니다. 응용 프로그램의 프로세스를 활성 상태로 유지 하면 다음 번에 사용자가 응용 프로그램을 열 때 응용 프로그램이 빠르게 시작 됩니다. 또한 응용 프로그램이 시스템 리소스를 그리지 않고 *일시 중단* 된 상태에서 *Backgrounded* 상태로 다시 이동할 수 있음을 의미 합니다. iOS 7에서는 장치가 절전 모드로 전환 되 고 사용자 개입 없이 백그라운드에서 직접 콘텐츠를 업데이트할 때 응용 프로그램이 백그라운드 작업을 일시 중지할 수 있게 해 주는 새로운 Api를 통해이 기능을 이용 합니다. [IOS Backgrounding 기술의](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)새로운 api를 살펴보겠습니다.

## <a name="application-lifecycle-methods"></a>응용 프로그램 수명 주기 메서드

앱의 상태가 변경 되 면 iOS는 `AppDelegate` 클래스의 이벤트 메서드를 통해 응용 프로그램에 알립니다.

- `OnActivated`-응용 프로그램이 처음 시작 될 때와 앱이 포그라운드로 다시 들어올 때마다 호출 됩니다. 앱을 열 때마다 실행 되어야 하는 코드를 저장 하는 위치입니다.
- `OnResignActivation`-사용자가 텍스트 또는 전화 통화와 같은 중단을 수신 하는 경우이 메서드가 호출 되 고 앱이 일시적으로 비활성화 됩니다. 사용자가 전화 통화를 수락 하면 앱이 백그라운드에 전송 됩니다.
- `DidEnterBackground`-앱이 backgrounded 상태로 전환 될 때 호출 됩니다 .이 메서드는 가능한 종료를 준비 하는 데 5 초 정도 응용 프로그램을 제공 합니다. 이 시간을 사용 하 여 사용자 데이터 및 작업을 저장 하 고 화면에서 중요 한 정보를 제거 합니다.
- `WillEnterForeground`-사용자가 backgrounded 또는 suspended 응용 프로그램으로 반환 하 여 포그라운드로 `WillEnterForeground` 시작 하면가 호출 됩니다. 이는 중 `DidEnterBackground` 에 저장 된 모든 상태를 리하이드레이션 하 여 포그라운드를 수행 하도록 응용 프로그램을 준비 하는 시간입니다.  `OnActivated`는이 메서드가 완료 된 직후에 호출 됩니다.
- `WillTerminate`-응용 프로그램이 종료 되 고 해당 프로세스가 제거 됩니다. 이 메서드는 장치 또는 OS 버전에서 멀티태스킹를 사용할 수 없는 경우, 즉 메모리가 부족 하거나 사용자가 backgrounded 응용 프로그램을 수동으로 종료 하는 경우에만 호출 됩니다. 종료 된 일시 중단 된 응용 프로그램은를 호출 `WillTerminate` 하지 않습니다.


다음 다이어그램에서는 응용 프로그램의 상태 및 수명 주기 방법이 함께 어떻게 연결 되어 있음을 보여 줍니다.

 [![](introduction-to-backgrounding-in-ios-images/image2.png "이 다이어그램에서는 응용 프로그램의 상태 및 수명 주기 방법이 함께 작동 하는 방법을 보여 줍니다.")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>IOS의 Backgrounding에 대 한 사용자 정의 컨트롤

iOS 7에는 사용자에 게 응용 프로그램의 backgrounded 상태를 보다 효과적으로 제어할 수 있는 몇 가지 기능이 도입 되었습니다. 앱 전환기 및 백그라운드 앱 새로 고침 설정은 모두 응용 프로그램 수명 주기에 영향을 줍니다.

### <a name="app-switcher"></a>앱 전환기

앱 전환기는 iOS 7에 도입 된 중요 한 제어 기능입니다. **홈** 단추를 두 번 눌러 시작 되며 프로세스가 활성 상태인 응용 프로그램을 표시 합니다.

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "앱 전환기를 사용 하 여 앱 간 이동")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

사용자는 앱 전환기를 사용 하 여 모든 backgrounded 및 일시 중단 된 응용 프로그램의 스냅숏을 스크롤할 수 있습니다. 응용 프로그램을 누르면 포그라운드로 시작 됩니다. 위로 살짝 밀기는 백그라운드에서 응용 프로그램을 제거 하 고 프로세스를 종료 합니다. 다음 섹션의 [IOS 응용 프로그램 수명 주기 데모](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) 에서 앱 전환기를 좀 더 자세히 살펴보겠습니다.

> [!IMPORTANT]
> 앱 전환기는 backgrounded 응용 프로그램과 일시 중단 된 응용 프로그램의 차이를 표시 하지 않습니다.



### <a name="background-app-refresh-settings"></a>백그라운드 응용 프로그램 새로 고침 설정

iOS 7은 사용자가 [백그라운드 처리를 위해 등록 된](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md)응용 프로그램에 대해 backgrounding를 옵트아웃 (opt out) 할 수 있도록 하 여 응용 프로그램 수명 주기에 대 한 사용자 제어 *응용 프로그램에서 백그라운드 작업을 실행 하는 것을 방지 하는 것은 아닙니다*.

사용자는 **설정 > 일반 > 백그라운드 앱 새로 고침** 으로 이동 하 고 선택한 응용 프로그램에 대 한 backgrounding 권한을 편집 하 여이 설정을 변경할 수 있습니다. 백그라운드 앱 새로 고침이 off로 설정 되 면 응용 프로그램이 백그라운드에서 시작 될 때 즉시 일시 중단 되 고 백그라운드 처리를 수행할 수 없습니다.

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "백그라운드 응용 프로그램 새로 고침 설정")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

개발자는 `BackgroundRefreshStatus` API를 사용 하 여 백그라운드 새로 고침 응용 프로그램 상태를 확인할 수 있습니다. 예제를 보려면 [백그라운드 새로 고침 설정 확인 조리법](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/check_background_refresh_setting)을 참조 하십시오.

IOS 응용 프로그램 수명 주기의 기본 사항과 응용 프로그램 수명 주기를 제어 하는 기능에 대해 설명 했습니다. 다음으로, 작동 중인 iOS 응용 프로그램 수명 주기를 살펴보겠습니다.

