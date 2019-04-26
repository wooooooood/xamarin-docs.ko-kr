---
title: 백그라운드에서 Xamarin.iOS 앱을 업데이트합니다.
description: 이 문서 영역 모니터링, 백그라운드 페치 및 원격 알림 같은 백그라운드에서 실행 되는 Xamarin.iOS 앱을 업데이트 하는 다양 한 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 835dccaea79467582f56fd4b8b6b3b8f42acd632
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61392354"
---
# <a name="updating-a-xamarinios-app-in-the-background"></a>백그라운드에서 Xamarin.iOS 앱을 업데이트합니다.

백그라운드 새로 고침은 깨울 일시 중단 된 응용 프로그램 및를 실행 하지 않는 새 콘텐츠로 업데이트 프로세스입니다. iOS는 백그라운드에서 콘텐츠를 새로 고치기 위한 세 가지 옵션을 제공 합니다.

1.  *지역 모니터링* 하 고 *중요 한 위치 변경 내용을 서비스* -트리거 백그라운드 위치 인식 Api는 사용자의 위치에 따라 변경 내용을 업데이트 합니다. 이러한 Api 용도 신중 하 게 위치 기반 iOS 6 응용 프로그램에서 콘텐츠를 새로 고칠 옵션도 사용할 수 없는 합니다.
1.  *Fetch (iOS 7 이상)을 백그라운드* -임시 방법 새로 고침 *중요 하지 않은* 업데이트 하는 콘텐츠 *자주* 합니다.
1.  *원격 알림 (iOS 7 이상)* -푸시 알림을 수신 하는 응용 프로그램 알림을 사용 하 여 백그라운드 콘텐츠 새로 고침을 트리거할 수 있습니다. 이 메서드를 사용 하 여 업데이트를 사용할 수 있습니다 *중요 한, 시간에 민감한* 콘텐츠를 업데이트 하는 *산발적* 합니다.


다음 섹션에서 이러한 옵션의 기본 사항을 설명 합니다.

## <a name="region-monitoring-and-significant-location-changes"></a>지역 모니터링 및 중요 한 위치를 변경

iOS backgrounding 기능으로 두 개의 위치 인식 Api를 제공 합니다.

1.  *지역 모니터링* 은 프로세스 경계를 사용 하 여 지역을 설정 하 고 사용자가 입력 하거나 영역을 종료 하는 경우 장치를 깨울입니다. 지역은 순환 되며 다양 한 일 수 있습니다. 사용자 영역 경계를 교차 하는 경우 장치 알림을 발생 하거나 작업을 시작 하 여 일반적으로 이벤트를 처리 하도록 절전 됩니다. 지역 모니터링 GPS 필요 및 배터리 및 데이터 사용이 증가 합니다.
1.  합니다 *중요 한 위치 변경 내용을 서비스* 는 간단 하 고 전원 절약 옵션 셀룰러 라디오를 사용 하 여 장치에서 사용할 수 있습니다. 변경 된 중요 한 위치에 대 한 수신 대기 하는 응용 프로그램이 장치 셀 towers 전환 된 경우 알림을 받게 됩니다. 이 서비스 응용 프로그램을 일시 중단 또는 종료의 절전 모드를 사용할 수 있으며 백그라운드에서 새 콘텐츠에 대 한 기회를 제공 합니다. 함께 사용 하지 않는 한 백그라운드 작업에는 약 10 초 제한 됩니다는 [백그라운드 태스크](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md) 합니다.


응용 프로그램에 위치 않아도 `UIBackgroundMode` 이러한 위치 인식 Api를 사용 하도록 합니다. IOS는 유형의 경우 장치에서 사용자의 위치를 변경 하 여 절전 모드에서 실행할 수 있는 작업을 추적 하지 않으므로, 때문에 이러한 Api iOS 6에서 백그라운드로 콘텐츠 업데이트에 대 한 해결책을 제공 합니다. *위치 기반 Api 사용 하 여 백그라운드 업데이트를 트리거하는 장치 리소스 및 응용 프로그램에서 해당 위치에 대 한 액세스를 필요로 하는 이유를 이해 하지 사용자 혼동을 줄 수 있습니다 염두에 둡니다*합니다. 백그라운드 위치 Api를 이미 사용 하지 않는 응용 프로그램에서 처리에 대 한 지역 모니터링 이나 변경 된 중요 한 위치를 구현 하는 경우에 재량에 따라를 사용 합니다.

IOS 6의에서 결함을 노출 하는 위치는 백그라운드 처리에 대 한 모니터링을 사용 하 여 앱: 응용 프로그램의 요구 사항 백그라운드 필요한 범주에 맞지, 경우 제한적으로 backgrounding 옵션입니다. 두 개의 새로운 Api가 도입 *백그라운드 페치* 하 고 *Remote Notifications*, 더 많은 응용 프로그램에 backgrounding 기회를 제공 하는 iOS 7 (이상). 다음 두 섹션에서는 이러한 새 Api를 소개합니다.

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>백그라운드 페치 (iOS 7 이상)

IOS 6, 전경 입력 응용 프로그램 간단 하 게 앞서 설명한 바 콘텐츠를 사용 하 여 사용자를 표시 하는 새 콘텐츠를 로드 하는 데 시간이 필요 합니다. 백그라운드 페치 허용 응용 프로그램을 새 데이터를 로드 *하기 전에* 사용자 응용 프로그램을 시작 하 고 최신 콘텐츠를 사용 하 여 사용자를 제공 합니다.

백그라운드 페치를 구현 하려면 편집 *Info.plist* 하 고 확인 합니다 **Enable Background Modes** 하 고 **백그라운드 페치** 확인란:

 [![](updating-an-application-in-the-background-images/fetch.png "Info.plist를 편집 하 고 Enable Background Modes 및 백그라운드 가져오기 확인 확인란을 선택")](updating-an-application-in-the-background-images/fetch.png#lightbox)

다음으로 `AppDelegate`를 재정의 합니다 `FinishedLaunching` 최소 인출 간격을 설정 하는 방법. 이 예제에서는 OS 새 콘텐츠를 인출 하는 빈도 결정 하도록 합니다.

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

마지막으로 인출 재정의 하 여 수행 합니다 `PerformFetch` 에서 메서드를 `AppDelegate`, 전달를 *완료 처리기*합니다. 완료 처리기가 사용 하는 대리자를 `UIBackgroundFetchResult`:

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

콘텐츠 업데이트를 마치면 알고 있어야 적절 한 상태를 완료 처리기를 호출 하 여 OS 하도록 합니다. iOS는 완료 처리기 상태에 대 한 세 가지 옵션을 제공합니다.

1.  `UIBackgroundFetchResult.NewData` -응용 프로그램이 업데이트 되어 새 콘텐츠를 가져온 경우 호출 됩니다.
1.  `UIBackgroundFetchResult.NoData` -새 콘텐츠에 대 한 인출에서 단계를 진행 하지만 없는 콘텐츠를 사용할 수 하는 경우 호출 됩니다.
1.  `UIBackgroundFetchResult.Failed` -오류 처리에 대 한 유용한,이 때 호출 됩니다 fetch를 이동할 수 없습니다.


백그라운드 페치를 사용 하 여 응용 프로그램을 백그라운드에서 UI를 업데이트 하는 호출 가능 합니다. 사용자가 앱을 열면 UI 최신이 고 새 콘텐츠를 표시 됩니다. 응용 프로그램에서 새 콘텐츠를에 하는 경우 사용자가 볼 수 있도록 응용 프로그램의 응용 프로그램 전환기 스냅숏을 업데이트 됩니다.

> [!IMPORTANT]
> 한 번 `PerformFetch` 는 호출 응용 프로그램은 약 30 초 동안 새 콘텐츠의 다운로드를 시작 및 완료 처리기 블록을 호출 합니다. 이 시간이 너무 오래 걸리는 경우 앱이 종료 됩니다. 사용 하 여 백그라운드 페치를 사용 하는 것이 좋습니다 합니다 _백그라운드 전송 서비스_ 미디어 또는 기타 대형 파일을 다운로드 하는 경우.


### <a name="backgroundfetchinterval"></a>BackgroundFetchInterval

OS 최소 인출 간격을 설정 하 여 새 콘텐츠를 인출 하는 빈도 결정 하도록 위의 샘플 코드에서 `BackgroundFetchIntervalMinimum`합니다. iOS는 인출 간격에 대 한 세 가지 옵션을 제공합니다.

1.  `BackgroundFetchIntervalNever` -시스템에 새 콘텐츠를 페치 하지 지시 합니다. 가져오는 경우 사용자가 로그인 하지 않은 같은 특정 상황에서는 사용 하지 않으려면이 사용 합니다. Fetch 간격에 대 한 기본 값입니다. 
1.  `BackgroundFetchIntervalMinimum` -사용자 패턴, 배터리, 데이터 사용량 및 다른 응용 프로그램의 요구에 따라 시스템 인출 하는 빈도 결정 하도록 합니다.
1.  `BackgroundFetchIntervalCustom` -응용 프로그램의 콘텐츠 업데이트 빈도 알고 있는 경우는 응용 프로그램에서 하지 못합니다 새 콘텐츠를 가져오는 중 "절전" 간격 후 모든 인출 지정할 수 있습니다. 해당 간격 인 시스템 내용을 가져올 때 결정 됩니다.


둘 다 `BackgroundFetchIntervalMinimum` 고 `BackgroundFetchIntervalCustom` 인출을 예약 하려면 시스템에 의존 합니다. 이 간격은 장치의 요구 사항 뿐만 아니라 개별 사용자의 습관에 적응할 수 동적입니다. 예를 들어 한 사용자가 매일 아침 응용 프로그램 및 다른 검사 매시간 iOS는 콘텐츠를 확인 하는 경우 최신 상태 모두 사용자에 대 한 응용 프로그램을 열 때마다 합니다.

중요 하지 않은 콘텐츠를 사용 하 여 자주 업데이트 하는 응용 프로그램에 대 한 백그라운드 페치를 사용 해야 합니다. 중요 업데이트를 사용 하 여 응용 프로그램의 경우 원격 알림은 사용 해야 합니다. 원격 알림 백그라운드 페치를 기반으로 하 고 동일한 완료 처리기를 공유 합니다. 알아보겠습니다 원격 알림으로 다음입니다.

 <a name="remote_notifications" />


## <a name="remote-notifications-ios-7-and-greater"></a>원격 알림 (iOS 7 이상)

푸시 알림은의 방식으로 장치에는 공급자 로부터 전송 된 JSON 메시지를 *Apple Push Notification 서비스 (APNs)* 합니다.

IOS 6에 들어오는 푸시 알림을 응용 프로그램에서 발생 한 흥미로운 사용자 경고 시스템에 알려줍니다. 앱 콘텐츠 업데이트 시작를 일시 중단 또는 종료 상태에서 응용 프로그램을 가져와서 알림을 클릭 합니다. iOS 7 (및 큰) 응용 프로그램이 백그라운드에서 콘텐츠를 업데이트할 수 있는 기회를 제공 하 여 일반적인 푸시 알림 확장 *하기 전에* 사용자 응용 프로그램을 열 수 있습니다 하 고 새 콘텐츠로 표시 되도록 사용자에 게 알리지 즉시 합니다.

원격 알림을 구현 하려면 편집 *Info.plist* 확인 합니다 **Enable Background Modes** 하 고 **Remote notifications** 확인란:

 [![](updating-an-application-in-the-background-images/remote.png "백그라운드 모드 Enable Background Modes 및 원격 알림 설정")](updating-an-application-in-the-background-images/remote.png#lightbox)

다음으로 설정 된 `content-available` 플래그가 1로 푸시 알림을 자체입니다. 이렇게 하면 응용 프로그램을 알립니다 경고를 표시 하기 전에 새 콘텐츠를 가져올 수 있습니다.

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

에 *AppDelegate*, 재정의 `DidReceiveRemoteNotification` 메서드를 사용할 수 있는 콘텐츠에 대 한 알림 페이로드를 확인 하 고 적절 한 완료 처리기 블록 호출:

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

응용 프로그램의 기능에 중요 한 콘텐츠를 사용 하 여 자주 업데이트에 사용할 원격 알림. 원격 알림에 대 한 자세한 내용은 참조는 Xamarin [iOS에 푸시 알림](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md) 가이드입니다.

> [!IMPORTANT]
> 원격 알림을 업데이트 메커니즘을 기반으로 백그라운드 가져오기 때문에 응용 프로그램의 새 콘텐츠 다운로드를 시작 하 고 알림을 받는 30 초 이내 완료 처리기 블록을 호출 해야 하거나 iOS 응용 프로그램을 종료 합니다. 사용 하 여 원격 알림 페어링 하는 것이 좋습니다 _백그라운드 전송 서비스_ 미디어 또는 기타 대형 파일 백그라운드에서 다운로드 하는 경우.


### <a name="silent-remote-notifications"></a>자동 원격 알림

원격 알림 응용 프로그램 업데이트를 알리고 새 콘텐츠를 페치를 시작 하는 간단한 방법이 되지만 내용이 변경 되었음을 나타내는 사용자에 게 알릴 필요가 없는 경우가 있습니다. 예를 들어 사용자 플래그 동기화 중에 대 한 파일에서는 파일 업데이트 될 때마다 사용자에 게 알려야 필요 없습니다. 파일 동기화 중 이벤트를 놀라운 되지 않으며 사용자의 즉각적인 주의가 필요 하지 않습니다. 사용자는 방금 파일을 열면 최신 상태인 것으로 기대 합니다.

위와 같은 경우, iOS 푸시 알림이 전송 되도록 자동으로-즉, 경고 없이 허용 합니다. 자동으로 정기 알림의 설정 하려면 알림 페이로드가에서 경고를 제거 하면 됩니다.

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>속도 제한

개발자의 관점에서 일반 및 자동 알림 간의 가장 큰 차이 자동 푸시 속도가 제한 합니다. APNs 푸시 속도가 너무 높은 경우 자동 푸시를 장치에 배달을 지연 됩니다. 이 응용 프로그램이 너무 많은 자동 알림 사용 하 여 장치 리소스를 고갈 되지 않도록 하는 것입니다.

단, APNs와 함께 일반 원격 알림 또는 유지 응답 "피기백" 자동 알림을 수 있습니다. 일반 알림 속도 제한 되지 않으므로 다음 다이어그램에 설명 된 대로 APNs에서 자동 알림을 등록 저장된 장치에 적용할 다시 사용할 수 있습니다 이러한.

 [![](updating-an-application-in-the-background-images/silent.png "일반 알림 수 APNs에서 저장된 한 자동 알림을 장치에 적용할이 다이어그램에 표시 된 것과 같이")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> Apple 응용 프로그램에 필요한 때마다 자동 푸시 알림을 전송 하도록 권장 하 고 및 해당 배달 예약 APNs를 사용 합니다.


이 섹션에서는 백그라운드 필요한 범주에 맞지 않는 작업을 실행 하려면 백그라운드로 콘텐츠를 새로 고치기 위한 다양 한 옵션에 설명 했습니다. 이제 작업에 이러한 Api의 일부를 확인해 보겠습니다.

 [다음: 파트 4-iOS Backgrounding 연습](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
