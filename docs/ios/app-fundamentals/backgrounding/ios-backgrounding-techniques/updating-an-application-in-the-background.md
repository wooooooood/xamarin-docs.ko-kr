---
title: "백그라운드에서 응용 프로그램 업데이트"
ms.topic: article
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f4a18bf8f35d1a6c615c819ea90433d1eb123422
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="updating-an-application-in-the-background"></a>백그라운드에서 응용 프로그램 업데이트

백그라운드 새로 고침을 일시 중단 하는 응용 프로그램을 다시 시작 또는 실행 중이 고를 새 내용으로 업데이트는 프로세스입니다. iOS는 백그라운드에서 콘텐츠를 새로 고치기 위한 세 가지 옵션을 제공 합니다.

1.  *지역 모니터링* 및 *상당한 위치 변경 내용을 서비스* -위치 인식 Api 트리거 배경 사용자의 위치에 대 한 변경에 따라 업데이트 합니다. 이러한 Api 수 신중 하 게 사용 위치 기반 iOS 6 응용 프로그램의 콘텐츠를 새로 고치려면 다른 옵션도 사용할 수 없는 합니다.
1.  *Fetch (iOS 7 이상) 백그라운드* -대 한 임시 액세스를 새로 고치 *중요 하지 않은* 콘텐츠를 업데이트 하는 *자주* 합니다.
1.  *원격 알림 (iOS 7 이상)* -푸시 알림을 수신 하는 응용 프로그램이 백그라운드 콘텐츠 새로 고침을 트리거하는 알림을 사용할 수 있습니다. 으로 업데이트 하려면이 메서드를 사용할 수 *중요, 시간에 민감한* 콘텐츠를 업데이트 하는 *산발적으로 장애가* 합니다.


다음 섹션에서는 이러한 옵션의 기본 사항을 설명합니다.

## <a name="region-monitoring-and-significant-location-changes"></a>지역 모니터링 및 중요 한 위치를 변경

iOS backgrounding 기능으로 두 개의 위치 인식 Api를 제공 합니다.

1.  *지역 모니터링* 경계를 포함 하는 영역을 설정 하 고 사용자가 입력 하거나 여 영역을 종료 하는 경우 장치를 다시 시작 하는 프로세스입니다. 지역 순환 되며 다양 한 크기의 일 수 있습니다. 사용자 영역 경계를 교차 하는 경우 장치는 알림을 시작 하 고 작업을 시작한 또는 일반적으로 이벤트를 처리 하려면 절전 됩니다. 지역 모니터링 GPS를 차지 하며 배터리 및 데이터 사용량 증가 합니다.
1.  *상당한 위치 변경 내용을 서비스* 는 간단 하 고 전원 관리 옵션 셀룰러 라디오를 사용 하 여 장치에 사용할 수 있습니다. 중요 한 위치를 변경에 대 한 수신 대기 하는 응용 프로그램 전환 된 경우 장치가 셀 towers 알림을 받게 됩니다. 이 서비스는 일시 중단 또는 종료 된 응용 프로그램의 절전 모드를 사용할 수 하 고 백그라운드에서 새 콘텐츠 있는지 검사 하는 기회를 제공 합니다. 함께 사용 하지 않는 한 백그라운드 작업은 약 10 초 제한는 [백그라운드 작업](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md) 합니다.


응용 프로그램에서 위치 하지 않아도 `UIBackgroundMode` 위치 인식 이러한 Api를 사용 하도록 합니다. IOS 장치 사용자의 위치에 있는 변경 내용의 절전 모드 해제 하는 경우에 실행할 수 있는 작업의 종류를 추적 하지 않습니다, 때문에 이러한 Api iOS 6에서 백그라운드에서 콘텐츠를 업데이트 하기 위한 해결 제공 합니다. *위치 기반 Api 사용 하 여 백그라운드 업데이트를 트리거하지 장치 리소스에 그려집니다 하며 사용자에 게 응용 프로그램의 위치에 대 한 액세스를 필요로 하는 이유를 이해 하지 혼동할 수 있으므로 유의*합니다. 백그라운드 위치 Api를 이미 사용 하지 않는 응용 프로그램에서 처리에 대 한 영역 모니터링 또는 중요 한 변경의 위치를 구현할 때 신중 하 게를 사용 합니다.

IOS 6의에서 결함을 노출 하는 위치는 백그라운드 처리에 대 한 모니터링을 사용 하 여 앱: backgrounding 옵션을 제한적 배경 필요한 범주에 응용 프로그램의 요구 사항을 적용할 수 없는, 하는 경우. 두 개의 새로운 Api의 도입으로 *배경 인출* 및 *원격 알림*, iOS 7 (및 큰) backgrounding 기회를 더 많은 응용 프로그램을 제공 합니다. 다음 두 섹션에서는 이러한 새 Api를 소개합니다.

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>백그라운드 가져오기 (iOS 7 이상)

IOS 6, 전경 입력 응용 프로그램에 간단 하 게 사용자 콘텐츠를 이미 확인 한 상태로 표시 하는 새 콘텐츠를 로드 하는 데 시간이 필요 합니다. 백그라운드 가져오기를 사용 하면 새 데이터를 로드 하려면 응용 프로그램 *전에* 사용자는 응용 프로그램을 시작 하 고 사용자에 게 가장 최신 콘텐츠가 제공 합니다.

백그라운드 가져오기의 구현 하려면 편집 *Info.plist* 확인 하 고는 **백그라운드 모드를 사용 하도록 설정** 및 **백그라운드 가져오기를** 확인란:

 [![](updating-an-application-in-the-background-images/fetch.png "Info.plist 편집 하 고 백그라운드 모드를 사용 하도록 설정 하 고 백그라운드 인출 확인란")](updating-an-application-in-the-background-images/fetch.png#lightbox)

다음에 `AppDelegate`, 재정의 `FinishedLaunching` 최소 인출 간격을 설정 하는 메서드. 이 예제에서는 새 콘텐츠를 인출 하는 빈도 결정 하는 OS를 사용 했습니다.

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

마지막으로 재정의 하 여 인출이 수행는 `PerformFetch` 에서 메서드는 `AppDelegate`, 전달 및는 *완료 처리기*합니다. 완료 처리기가 사용 하는 대리자는 `UIBackgroundFetchResult`:

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

업데이트 콘텐츠를 완료 하는 경우 적절 한 상태와 함께 완료 처리기를 호출 하 여 알고 OS 하도록 합니다. iOS 완료 처리기 상태에 대 한 세 가지 옵션이 있습니다.

1.  `UIBackgroundFetchResult.NewData` -새 콘텐츠를 가져온 하 고 응용 프로그램이 업데이트 될 때 호출 됩니다.
1.  `UIBackgroundFetchResult.NoData` -새 내용에 대 한 인출 진행 하지만 콘텐츠를 사용할 수 있는 때 호출 됩니다.
1.  `UIBackgroundFetchResult.Failed` -유용한 오류 처리를 위해,이 때 호출 됩니다는 페치를 통과 하지 못했습니다.


배경 Fetch를 사용 하 여 응용 프로그램 백그라운드에서 UI를 업데이트 하는 호출을 수행할 수 있습니다. 사용자가 앱을 열면 UI 최신이 고 새 콘텐츠를 표시 됩니다. 또한 사용자 때 응용 프로그램에 새 콘텐츠를 볼 수 있도록 응용 프로그램의 응용 프로그램 전환기 스냅숏을 업데이트 합니다.

> [!IMPORTANT]
> **참고**: 한 번 `PerformFetch` 는 호출 응용 프로그램은 약 30 초 새 콘텐츠의 다운로드를 시작 하 고 완료 처리기 블록을 호출 합니다. 이 시간이 너무 오래 걸리는 경우 앱이 종료 됩니다. 배경 함께 가져올를 사용 하는 것이 좋습니다는 _백그라운드 전송 서비스_ 미디어 또는 기타 큰 파일을 다운로드할 때.


### <a name="backgroundfetchinterval"></a>BackgroundFetchInterval

위의 샘플 코드에 최소 인출 간격을 설정 하 여 새 콘텐츠를 인출 하는 빈도 결정 하는 운영 체제에 게 `BackgroundFetchIntervalMinimum`합니다. iOS 인출 간격에 대 한 세 가지 옵션이 있습니다.

1.  `BackgroundFetchIntervalNever` -하지 새 콘텐츠를 인출 하는 시스템에 알립니다. 이 때 사용자가 로그인 되지 않은 같은 특수 한 상황에서 페치 하지 않으려면 사용 합니다. Fetch 간격에 대 한 기본 값입니다. 
1.  `BackgroundFetchIntervalMinimum` -사용자의 패턴, 배터리 수명, 데이터 사용량 및 다른 응용 프로그램의 요구에 따라 시스템을 인출 하는 빈도 결정 하도록 합니다.
1.  `BackgroundFetchIntervalCustom` -응용 프로그램의 콘텐츠 업데이트 빈도 알고 있는 경우는 응용 프로그램을 새 콘텐츠를 가져올 없게 됩니다 "sleep" 간격 마다 인출 후 지정할 수 있습니다. 해당 간격이 되 면 시스템 내용을 가져올 때 결정 됩니다.


둘 다 `BackgroundFetchIntervalMinimum` 및 `BackgroundFetchIntervalCustom` 인출을 예약 하는 시스템에 의존 합니다. 이 간격은 동적 장치의 요구 사항 뿐만 아니라 개별 사용자의 환경에 맞게 조정 합니다. 예를 들어, 한 명의 사용자 매일 아침에 응용 프로그램을 확인 하 고 다른 검사 1 시간 마다 iOS는 콘텐츠를 확인 하는 경우 최신 상태 두 사용자 모두에 대 한 응용 프로그램을 열 때마다 합니다.

중요 하지 않은 콘텐츠 자주 업데이트 하는 응용 프로그램에 대 한 배경 Fetch는 사용 해야 합니다. 응용 프로그램의 중요 업데이트가 있는 경우 원격 알림은 사용 해야 합니다. 원격 알림 백그라운드 인출에 기반 하 고 동일한 완료 처리기를 공유 합니다. 것에 대해 살펴보기 원격 알림 다음 합니다.

 <a name="remote_notifications" />


## <a name="remote-notifications-ios-7-and-greater"></a>원격 알림 (iOS 7 이상)

푸시 알림은 통해 장치에는 공급자에서 전송 하는 JSON 메시지는 *Apple 푸시 알림 서비스 (APNs)*합니다.

IOS 6에에서는 들어오는 푸시 알림을 응용 프로그램에서 발생 하는 흥미로운 사용자에 게 알리도록 하려면 시스템에 알려줍니다. 알림 클릭 응용 프로그램 일시 중단 또는 종료 된 상태에서 끌어와 앱 콘텐츠 업데이트를 시작 합니다. iOS 7 (및 큰) 응용 프로그램을 백그라운드에서 콘텐츠를 업데이트할 수를 지정 하 여 일반 푸시 알림을 확장 *전에* 사용자 응용 프로그램을 열 수 고를 새 내용으로 표시할 수 있도록 사용자에 게 알리는 즉시 합니다.

원격 알림을 구현 하려면 편집 *Info.plist* 확인 하 고는 **백그라운드 모드를 사용 하도록 설정** 및 **원격 알림** 확인란:

 [![](updating-an-application-in-the-background-images/remote.png "백그라운드 모드를 사용 하도록 설정 및 원격 알림 백그라운드 모드 설정")](updating-an-application-in-the-background-images/remote.png#lightbox)

다음으로 설정 된 `content-available` 1로 푸시 알림 자체에 대 한 플래그입니다. 이 응용 프로그램을 경고를 표시 하기 전에 새 내용을 가져올 알 수 있습니다.

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

에 *AppDelegate*, 재정의 `DidReceiveRemoteNotification` 메서드를 사용할 수 있는 내용에 대 한 알림 페이로드를 확인 하 고 적절 한 완료 처리기 블록을 호출 합니다.

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

콘텐츠 응용 프로그램의 기능에 중요 한 이유가 드물게 업데이트에 대 한 원격 알림을 사용 해야 합니다. 원격 알림에 대 한 자세한 내용은 참조는 Xamarin [iOS에서 푸시 알림을](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md) 가이드입니다.

> [!IMPORTANT]
> **참고**: 원격 알림에 업데이트 메커니즘은 백그라운드 인출 기반, 응용 프로그램의 새 콘텐츠 다운로드를 시작 하 고는 알림을 받으면 30 초 안에 완료 처리기 블록을 호출 해야 또는 iOS 됩니다 때문에 응용 프로그램을 종료 합니다. 사용 하 여 원격 알림을 쌍으로 연결 하는 것이 좋습니다. _백그라운드 전송 서비스_ 미디어 또는 백그라운드에서 기타 큰 파일을 다운로드할 때.


### <a name="silent-remote-notifications"></a>자동 원격 알림

원격 알림은 응용 프로그램 업데이트에 알리는 새 콘텐츠를 페치를 시작 하는 간단한 방법을 하지만 여기서 변경 된 내용으로 사용자에 게 알릴 필요가 없습니다. 예를 들어 사용자는 동기화 중에 대 한 파일 플래그, 하는 경우 파일이 업데이트 될 때마다 알림을 받도록 필요 하지 않습니다. 파일 동기화 놀라운 이벤트 되지 않으며 사용자의 즉각적인 주의가 필요 하지 않습니다. 사용자만 파일을 열면 최신 상태로 유지할 수를 예상 합니다.

위와 같은 경우, iOS 자동-즉, 없이 전송 경고에 푸시 알림을 허용 됩니다. 자동으로 일반 알림 기능을 설정 하려면 알림 페이로드에서 경고를 제거 하면:

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>속도 제한

일반-자동 알림 개발자의 관점에서 가장 큰 차이점은 자동 푸시 속도 제한 않는다는 것입니다. APNs 푸시 속도 너무 높은 경우 장치에 푸시 자동 배달을 지연 됩니다. 이 응용 프로그램 너무 많은 자동 알림 장치 리소스를 드레이닝 하지 않는 것입니다.

그러나 APNs에서 "피기백" 일반 원격 알림 또는 유지 응답와 함께 자동 알림 수입니다. 정기적으로 알림이 속도 제한 되지 않으므로 다음 다이어그램에 표시 된 것 처럼 장치에 APNs에서 저장된 한 자동 알림을 데 다시 사용할 수 있습니다.

 [![](updating-an-application-in-the-background-images/silent.png "이 다이어그램에서와 같이 정기적으로 알림이 장치에 APNs에서 자동 저장된 알림 데 사용할 수 수 있습니다.")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> **참고**: Apple 응용 프로그램에서 필요로 하 고 let APNs의 배달을 예약할 때마다 자동 푸시 알림을 보내려면 개발자가 권장 합니다.


이 섹션에서는 배경 필요한 범주에 적합 하지 않은 작업을 실행 하려면 백그라운드에서 콘텐츠를 새로 고치기 위한 다양 한 옵션에 설명한 합니다. 이제 일부의 작업에서 이러한 Api를 확인해 보겠습니다.

 [다음: 4 부-iOS Backgrounding 연습](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
