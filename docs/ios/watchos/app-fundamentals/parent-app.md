---
title: "부모 응용 프로그램 사용"
description: "WatchOS 1에서에서 iOS 및 조사식 앱 간에 데이터 공유"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 82f85808da6776f2f718b21b2e87ff6d4d8087fd
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-the-parent-application"></a>부모 응용 프로그램 사용

_WatchOS 1에서에서 iOS 및 조사식 앱 간에 데이터 공유_

> [!IMPORTANT]
> 아래 예제를 사용 하 여만 부모 응용 프로그램에 액세스 watchOS 1 조사식 앱에서 작동 합니다.


여러 가지 방법으로 watch 앱 및 라이브러리와 함께 제공 하는 iOS 앱 간에 통신 합니다.

- 조사식 확장 수 [메서드를 호출](#code) iPhone의 백그라운드에서 실행 하는 부모 응용 프로그램에 대 한 합니다.

- 조사식 확장 수 [저장소 위치를 공유](#storage) 부모 iPhone 앱.

- 핸드 오프를 사용 하 여 사용자를 응용 프로그램에서 특정 인터페이스 컨트롤러로 보내 조사식 앱을 눈 또는 알림에서 데이터를 전달 하 합니다.

부모 앱 되기도 컨테이너 응용 프로그램 이라고 합니다.


<a name="code" />

## <a name="run-code"></a>코드 실행

에 설명 된 조사식 확장 프로그램과 부모 iPhone 앱 간의 통신은 [GpsWatch 샘플](https://developer.xamarin.com/samples/GpsWatch)합니다.
조사식 확장에서 사용 하 여 해당를 대신 하 여 한 처리를 수행 하려면 부모 iOS 앱을 요청할 수는 `OpenParentApplication` 메서드.

이것은 iOS 앱 (포함 하 여 네트워크 요청)-작업의 부모를 장기 실행 활용할 수 있습니다 이러한 작업을 완료 하 고 조사식 확장에서 액세스할 수 있는 위치에 검색된 된 데이터를 저장 하기 위해 백그라운드 처리에 특히 유용 합니다.



### <a name="watch-kit-app-extension"></a>조사식 키트 응용 프로그램 확장

호출 된 `WKInterfaceController.OpenParentApplication` 조사식 응용 프로그램 확장에서 합니다. 반환 된 `bool` 메서드 요청이 성공적으로 전송 여부를 나타내는입니다. 확인 된 `error` 응답에서 매개 변수 `Action` 여부를 확인 하는 iPhone 앱에서 실행 되는 메서드가 동안 발생 합니다.

```csharp
WKInterfaceController.OpenParentApplication (new NSDictionary (), (replyInfo, error) => {
    if(error != null) {
        Console.WriteLine (error);
        return;
    }
    Console.WriteLine ("parent app responded");
    // do something with replyInfo[] dictionary
});
```


### <a name="ios-app"></a>iOS 앱

IPhone 앱을 통해 watch 앱 확장의 모든 호출은 라우팅됩니다 `HandleWatchKitExtensionRequest` 메서드.
Watch 앱에서 다른 요청을 하는 경우이 메서드는 쿼리를 해야 합니다는 `userInfo` 요청을 처리 하는 방법을 결정 하는 사전입니다.


```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    // ... other AppDelegate methods
    public override void HandleWatchKitExtensionRequest
        (UIApplication application, NSDictionary userInfo, Action<NSDictionary> reply)
    {
        var status = 2;
        // do something in the background, and respond
        reply (new NSDictionary (
            "count", NSNumber.FromInt32 ((int)status),
            "value2", new NSString("some-info")
            ));
    }
}
```


<a name="storage" />

## <a name="shared-storage"></a>공유 저장소

구성 하는 경우는 [app 그룹](~/ios/watchos/app-fundamentals/app-groups.md) 다음 iOS 8 확장 (조사식 확장 포함) 부모 응용 프로그램 데이터를 공유할 수 있습니다.

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

공통 집합을 참조할 수 있도록 조사식 응용 프로그램 확장와 부모 iPhone 앱에서 다음 코드를 작성할 수 있습니다 `NSUserDefaults`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
        "group.com.your-company.watchstuff",
        NSUserDefaultsType.SuiteName);

// set values
shared.SetInt (2, "count");
shared.Synchronize ();

// get values
shared.Synchronize ();
var count = shared.IntForKey ("count");
```

<a name="files" />

### <a name="files"></a>파일

IOS 앱 및 조사식 확장에서 공통 파일 경로 사용 하 여 파일을 공유할 수도 있습니다.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

참고: 경로 이면 `null` 다음 확인은 [앱 그룹 구성](~/ios/watchos/app-fundamentals/app-groups.md) 프로 비전 프로필 제대로 구성 하 고 개발 컴퓨터에 다운로드/설치 되었습니다.

자세한 내용은 참조 하십시오는 [앱 그룹 기능](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 설명서입니다.

## <a name="wormholesharp"></a>WormHoleSharp

WatchOS 1에 대 한 인기 있는 오픈 소스 메커니즘 (기반 [MMWormHole](https://github.com/mutualmobile/MMWormhole)) 부모 앱와 watch 앱 간에 데이터 나 명령을 전달 하 합니다.

IOS 앱에 다음과 같은 앱 그룹을 사용 하 여 웜 구성 및 확장을 볼 수 있습니다.

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

C# 버전을 다운로드 [WormHoleSharp](https://github.com/Clancey/WormHoleSharp)합니다.



## <a name="related-links"></a>관련 링크

- [GpsWatch (샘플)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WormHoleSharp (샘플)](https://github.com/Clancey/WormHoleSharp)
- [Apple의 WKInterfaceController 참조](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple의 포함 된 앱과 데이터 공유](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
