---
title: 배경에서 Xamarin.ios 앱 업데이트
description: 이 문서에서는 지역 모니터링, 백그라운드 페치 및 원격 알림과 같이 배경에 있는 Xamarin.ios 앱을 업데이트 하는 다양 한 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 1d5a227f4acdba319eefc91b4991dead5a036eb9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756322"
---
# <a name="updating-a-xamarinios-app-in-the-background"></a>배경에서 Xamarin.ios 앱 업데이트

백그라운드 새로 고침은 일시 중단 되었거나 실행 되지 않는 응용 프로그램의 절전 모드를 해제 하 고 새 콘텐츠를 사용 하 여 업데이트 하는 프로세스입니다. iOS는 배경에서 콘텐츠를 새로 고치는 세 가지 옵션을 제공 합니다.

1. *지역 모니터링* 및 *중요 한 위치 변경 서비스* -위치 인식 api는 사용자 위치의 변경 내용을 기반으로 백그라운드 업데이트를 트리거합니다. 이러한 Api는 다른 옵션을 사용할 수 없는 위치 기반이 아닌 iOS 6 응용 프로그램에서 콘텐츠를 새로 고치는 데 신중 하 게 사용할 수 있습니다.
1. *백그라운드 페치 (iOS 7 이상)* - *자주* 업데이트 되는 *중요 하지 않은* 콘텐츠를 새로 고치는 임시 방법입니다.
1. *원격 알림 (iOS 7 이상)* -푸시 알림을 받는 응용 프로그램은 알림을 사용 하 여 백그라운드 콘텐츠 새로 고침을 트리거할 수 있습니다. 이 메서드를 사용 하 여 *산발적* 으로 업데이트 되는 *중요 하 고 시간이 중요* 한 콘텐츠를 업데이트할 수 있습니다.

다음 섹션에서는 이러한 옵션의 기본 사항을 다룹니다.

## <a name="region-monitoring-and-significant-location-changes"></a>영역 모니터링 및 중요 한 위치 변경

iOS는 backgrounding 기능을 제공 하는 두 개의 위치 인식 Api를 제공 합니다.

1. *지역 모니터링* 은 경계를 사용 하 여 영역을 설정 하 고 사용자가 지역을 들어가거나 지역을 종료할 때 장치를 절전 모드에서 해제 하는 프로세스입니다. 지역은 원형 이며 다양 한 크기를 가질 수 있습니다. 사용자가 지역 경계를 교차할 때 장치는 일반적으로 알림을 발생 하거나 작업을 시작 하 여 이벤트를 처리 하기 위해 절전 모드를 해제 합니다. 영역 모니터링에는 GPS가 필요 하며 배터리 및 데이터 사용량이 늘어납니다.
1. *중요 한 위치 변경 서비스* 는 셀룰러 라디오를 사용 하는 장치에 사용할 수 있는 보다 간단한 전원 절약 옵션입니다. 장치가 셀 타워로 전환 되 면 중요 한 위치 변경을 수신 대기 하는 응용 프로그램에 알림이 표시 됩니다. 이 서비스는 일시 중단 또는 종료 된 응용 프로그램의 절전 모드를 해제 하는 데 사용할 수 있으며 백그라운드에서 새 콘텐츠를 확인할 수 있는 기회를 제공 합니다. 백그라운드 [작업](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md) 을 사용 하는 경우를 제외 하 고 백그라운드 작업은 약 10 초로 제한 됩니다.

응용 프로그램에는 이러한 위치 인식 `UIBackgroundMode` api를 사용 하는 위치가 필요 하지 않습니다. IOS는 사용자 위치의 변경으로 장치를 해제 때 실행할 수 있는 작업 유형을 추적 하지 않으므로 iOS 6의 백그라운드에서 콘텐츠를 업데이트 하는 데 대 한 해결 방법을 제공 합니다. *위치 기반 api를 사용 하 여 백그라운드 업데이트를 트리거하는 것은 장치 리소스에 표시 되며, 응용 프로그램에서 해당 위치에 액세스 해야 하는 이유를 모르는 사용자를 혼동할 수 있습니다*. 위치 Api를 아직 사용 하지 않는 응용 프로그램에서 백그라운드 처리에 대 한 지역 모니터링 또는 중요 한 위치 변경 사항을 구현할 때는 신중 하 게 사용 합니다.

백그라운드 처리에 대 한 위치 모니터링을 사용 하는 앱은 iOS 6의 결함을 노출 합니다. 응용 프로그램의 요구 사항이 백그라운드에 필요한 범주에 맞지 않는 경우 backgrounding 옵션은 제한적입니다. 두 개의 새로운 Api 인 *백그라운드 페치* 및 *원격 알림과*iOS 7 (이상)이 도입 되면서 더 많은 응용 프로그램에 backgrounding 기회를 제공 합니다. 다음 두 섹션에서는 이러한 새 Api를 소개 합니다.

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>백그라운드 인출 (iOS 7 이상)

IOS 6에서 새로운 콘텐츠를 로드 하는 데 필요한 포그라운드를 입력 하는 응용 프로그램은 사용자가 이미 알고 있는 콘텐츠를 간단 하 게 표시 합니다. 백그라운드 페치를 사용 하면 사용자가 응용 프로그램을 시작 *하기 전에* 응용 프로그램에서 새 데이터를 로드 하 고 최신 콘텐츠를 사용자에 게 제공할 수 있습니다.

백그라운드 페치를 구현 하려면 *info.plist* 를 편집 하 고 **백그라운드 모드** 및 **백그라운드 페치** 사용 확인란을 선택 합니다.

 [![](updating-an-application-in-the-background-images/fetch.png "Info.plist를 편집 하 고 백그라운드 모드 및 백그라운드 페치 사용 확인란을 선택 합니다.")](updating-an-application-in-the-background-images/fetch.png#lightbox)

그런 다음에서 `AppDelegate` `FinishedLaunching` 메서드를 재정의 하 여 최소 인출 간격을 설정 합니다. 이 예제에서는 OS가 새 콘텐츠를 가져오는 빈도를 결정 합니다.

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

마지막으로 `PerformFetch` `AppDelegate`,에서 메서드를 재정의 하 고 *완료 처리기*를 전달 하 여 페치를 수행 합니다. 완료 처리기는를 `UIBackgroundFetchResult`사용 하는 대리자입니다.

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

콘텐츠 업데이트가 완료 되 면 적절 한 상태를 사용 하 여 완료 처리기를 호출 하 여 OS를 알 수 있습니다. iOS는 완료 처리기 상태를 위한 세 가지 옵션을 제공 합니다.

1. `UIBackgroundFetchResult.NewData`-새 콘텐츠를 인출 했을 때 호출 되며 응용 프로그램이 업데이트 되었습니다.
1. `UIBackgroundFetchResult.NoData`-새 콘텐츠에 대 한 페치를 통과 했지만 콘텐츠를 사용할 수 없는 경우 호출 됩니다.
1. `UIBackgroundFetchResult.Failed`-오류 처리에 유용 합니다 .이는 반입이 통과할 수 없을 때 호출 됩니다.

백그라운드 페치를 사용 하는 응용 프로그램은를 호출 하 여 백그라운드에서 UI를 업데이트할 수 있습니다. 사용자가 앱을 열면 UI가 최신 상태가 되 고 새 콘텐츠가 표시 됩니다. 응용 프로그램의 응용 프로그램에 새 콘텐츠가 있는 경우 사용자가 볼 수 있도록 응용 프로그램의 앱 전환기 스냅숏이 업데이트 됩니다.

> [!IMPORTANT]
> 가 `PerformFetch` 호출 되 면 응용 프로그램은 약 30 초 동안 새 콘텐츠 다운로드를 시작 하 고 완료 처리기 블록을 호출 합니다. 시간이 너무 오래 걸리면 앱이 종료 됩니다. 미디어 나 기타 많은 파일을 다운로드 하는 경우 _백그라운드 전송 서비스_ 에서 백그라운드 페치를 사용 하는 것이 좋습니다.

### <a name="backgroundfetchinterval"></a>BackgroundFetchInterval

위의 샘플 코드에서 OS는 최소 인출 간격을로 `BackgroundFetchIntervalMinimum`설정 하 여 새 콘텐츠를 가져오는 빈도를 결정 합니다. iOS는 fetch 간격에 대 한 세 가지 옵션을 제공 합니다.

1. `BackgroundFetchIntervalNever`-새 콘텐츠를 인출 하지 않도록 시스템에 지시 합니다. 사용자가 로그인 하지 않은 경우와 같은 특정 상황에서 페치를 해제 하려면이 옵션을 사용 합니다. 이 값은 인출 간격의 기본값입니다. 
1. `BackgroundFetchIntervalMinimum`-시스템에서 사용자 패턴, 배터리 수명, 데이터 사용량 및 다른 응용 프로그램의 요구 사항에 따라 페치할 빈도를 결정 합니다.
1. `BackgroundFetchIntervalCustom`-응용 프로그램의 콘텐츠를 업데이트 하는 빈도를 알고 있는 경우 반입할 때마다 "절전" 간격을 지정할 수 있습니다 .이 기간 동안에는 응용 프로그램에서 새 콘텐츠를 가져올 수 없습니다. 해당 간격이 경과 되 면 시스템은 콘텐츠를 가져올 시기를 결정 합니다.

`BackgroundFetchIntervalMinimum` 및`BackgroundFetchIntervalCustom` 는 모두 시스템을 사용 하 여 페치를 예약 합니다. 이 간격은 개별 사용자의 습관 뿐만 아니라 장치의 요구에 맞게 동적으로 조정 됩니다. 예를 들어 한 사용자가 매일 아침에 응용 프로그램을 확인 하 고 다른 사용자가 매시간 확인 하는 경우 iOS는 응용 프로그램을 열 때마다 두 사용자 모두에 대해 콘텐츠가 최신 상태로 유지 되도록 합니다.

중요 하지 않은 콘텐츠를 자주 업데이트 하는 응용 프로그램에는 백그라운드 페치를 사용 해야 합니다. 중요 업데이트를 사용 하는 응용 프로그램의 경우 원격 알림을 사용 해야 합니다. 원격 알림은 백그라운드 페치를 기반으로 하며 동일한 완료 처리기를 공유 합니다. 다음으로 원격 알림을 살펴보겠습니다.

 <a name="remote_notifications" />

## <a name="remote-notifications-ios-7-and-greater"></a>원격 알림 (iOS 7 이상)

푸시 알림은 *APNs (Apple Push Notification service)* 를 통해 공급자 로부터 장치로 전송 되는 JSON 메시지입니다.

IOS 6에서 들어오는 푸시 알림은 응용 프로그램에서 흥미로운 작업을 사용자에 게 알리는 경고를 시스템에 알려 줍니다. 알림을 클릭 하면 응용 프로그램이 일시 중단 또는 종료 된 상태에서 끌어오고 앱이 콘텐츠 업데이트를 시작 합니다. iOS 7 (이상)은 사용자에 게 알리기 *전에* 응용 프로그램에 백그라운드에서 콘텐츠를 업데이트할 수 있는 기회를 제공 하 여 사용자가 응용 프로그램을 열고 새 콘텐츠를 즉시 제공할 수 있도록 하 여 일반 푸시 알림을 확장 합니다.

원격 알림을 구현 하려면 *info.plist* 를 편집 하 고 **백그라운드 모드** 및 **원격 알림** 사용 확인란을 선택 합니다.

 [![](updating-an-application-in-the-background-images/remote.png "백그라운드 모드 및 원격 알림을 사용 하도록 설정 하는 백그라운드 모드")](updating-an-application-in-the-background-images/remote.png#lightbox)

다음으로 푸시 알림 `content-available` 자체의 플래그를 1로 설정 합니다. 이렇게 하면 경고를 표시 하기 전에 응용 프로그램이 새 콘텐츠를 인출 하는 것을 알 수 있습니다.

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

*AppDelegate*에서 `DidReceiveRemoteNotification` 메서드를 재정의 하 여 사용 가능한 콘텐츠의 알림 페이로드를 확인 하 고 적절 한 완료 처리기 블록을 호출 합니다.

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

응용 프로그램의 기능에 중요 한 콘텐츠를 사용 하는 경우에는 자주 발생 하지 않는 업데이트에 원격 알림을 사용 해야 합니다. 원격 알림에 대 한 자세한 내용은 [iOS의 Xamarin 푸시 알림](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md) 가이드를 참조 하세요.

> [!IMPORTANT]
> 원격 알림의 업데이트 메커니즘은 백그라운드 페치를 기반으로 하기 때문에 응용 프로그램에서 새 콘텐츠 다운로드를 시작 하 고, 알림을 받은 30 초 이내에 완료 처리기 블록을 호출 해야 합니다. 그렇지 않으면 iOS에서 응용 프로그램이 종료 됩니다. 백그라운드에서 미디어 또는 기타 많은 파일을 다운로드 하는 경우 _백그라운드 전송 서비스_ 와 원격 알림을 연결 하는 것이 좋습니다.

### <a name="silent-remote-notifications"></a>자동 원격 알림

원격 알림은 응용 프로그램에 업데이트를 알리는 간단한 방법 이며, 새 콘텐츠를 인출 하는 것은 아니지만 사용자에 게 변경 내용이 있음을 알릴 필요가 없는 경우가 있습니다. 예를 들어 사용자가 동기화를 위해 파일에 플래그를 표시 하는 경우 파일이 업데이트 될 때마다 알림을 받을 필요가 없습니다. 파일 동기화는 놀라운 이벤트가 아니며 사용자의 즉각적인 주의가 필요 하지 않습니다. 사용자는 파일을 열 때 파일을 최신 상태로 유지 하는 것으로 간주 합니다.

위의 경우와 같은 경우에는 iOS에서 푸시 알림을 자동으로 전송할 수 있습니다. 즉, 경고가 발생 하지 않습니다. 정기 알림을 자동으로 전환 하려면 알림 페이로드에서 경고를 제거 하면 됩니다.

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>속도 제한

개발자 관점에서 일반적인 알림과 자동 알림의 가장 큰 차이점은 자동 푸시의 비율이 제한 된다는 것입니다. APNs는 푸시 비율이 너무 높으면 장치에 자동 푸시 배달이 지연 됩니다. 이는 응용 프로그램이 자동 알림이 너무 많은 장치 리소스를 소모 하지 않도록 하기 위한 것입니다.

그러나 APNs는 정상적인 원격 알림 또는 연결 유지 응답과 함께 자동 알림을 "긍정" 할 수 있습니다. 정기 알림은 속도 제한 되지 않으므로 다음 다이어그램과 같이 APNs에서 장치에 저장 된 자동 알림을 푸시하는 데 사용할 수 있습니다.

 [![](updating-an-application-in-the-background-images/silent.png "정기 알림을 사용 하 여이 다이어그램에서 설명한 대로 APNs에서 장치로 저장 된 자동 알림을 푸시할 수 있습니다.")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> Apple에서는 개발자가 응용 프로그램에 필요할 때마다 자동 푸시 알림을 보내고 APNs에서 배달을 예약할 수 있습니다.

이 섹션에서는 백그라운드에서 필요한 범주에 맞지 않는 작업을 실행 하기 위해 백그라운드에서 콘텐츠를 새로 고치는 다양 한 옵션을 설명 했습니다. 이제 이러한 Api 중 일부를 수행 하는 것을 살펴보겠습니다.

 [다음: 4 부-iOS Backgrounding 연습](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
