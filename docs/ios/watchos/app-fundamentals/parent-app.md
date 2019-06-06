---
title: WatchOS에서 Xamarin 부모 응용 프로그램 사용
description: 이 문서에서는 Xamarin에서 watchOS 부모 응용 프로그램을 사용 하는 방법을 설명 합니다. WatchKit 앱 확장 및 iOS 앱, 공유 저장소 등을 설명합니다.
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 6b3a6f45d78c0febb2aacf4f7693bc6e328c3ec0
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66740954"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>WatchOS에서 Xamarin 부모 응용 프로그램 사용

> [!IMPORTANT]
> WatchOS 1 watch 앱에서 작동만 아래 예제를 사용 하 여 부모 응용 프로그램에 액세스 합니다.


Watch 앱 및 번들로 제공 되는 iOS 앱 간에 통신 하는 방법은 여러 가지:

- 조사식 확장 수 있습니다 [메서드를 호출](#code) iPhone의 백그라운드에서 실행 되는 부모 앱에 대 한 합니다.

- 조사식 확장 수 있습니다 [저장소 위치를 공유](#storage) 부모 iPhone 앱을 사용 하 여 합니다.

- 핸드 오프를 사용 하 여 전달할 데이터 보기 또는 알림 Watch 앱을 사용자 앱에서 특정 인터페이스 컨트롤러에 전송 합니다.

부모 앱 되기도 컨테이너 앱 이라고 합니다.


<a name="code" />

## <a name="run-code"></a>코드를 실행 합니다.

에 설명 된 부모 iPhone 앱 및 watch 확장 간의 통신을 [GpsWatch 샘플](https://developer.xamarin.com/samples/monotouch/WatchKit/GpsWatch/)합니다.
조사식 확장 상위 iOS 앱을 사용 하 여 해당 대신 일부 처리를 요청할 수는 `OpenParentApplication` 메서드.

IOS 앱 작업 (포함 하 여 네트워크 요청)-부모만을 장기 실행 활용할 수 있습니다 이러한 작업을 완료 하 고 조사식 확장에 액세스할 수 있는 위치에서 검색된 된 데이터를 저장 하기 위해 백그라운드 처리에 대 한 특히 유용 합니다.



### <a name="watch-kit-app-extension"></a>조사식 키트 앱 확장

호출 된 `WKInterfaceController.OpenParentApplication` watch 앱 확장에 있습니다. 반환 된 `bool` 메서드 요청을 성공적으로 보낸 여부를 나타내는입니다. 확인 합니다 `error` 응답에서 매개 변수 `Action` iPhone 앱에서 실행 되는 메서드 동안 발생 한 경우를 결정 합니다.

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

Watch 앱 확장의 모든 호출은 iPhone 앱을 통해 라우팅됩니다 `HandleWatchKitExtensionRequest` 메서드.
Watch 앱에서 다른 요청을 변경 하는 경우이 메서드를 쿼리할 필요는 `userInfo` 사전 요청을 처리 하는 방법을 결정 합니다.


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

구성 하는 경우는 [앱 그룹](~/ios/watchos/app-fundamentals/app-groups.md) iOS 8 확장 (조사식 확장 포함) 부모 앱을 사용 하 여 데이터를 공유할 수 있습니다.

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

다음 코드를 작성할 수 있습니다 watch 앱 확장 및 부모 iPhone 앱의 공통 집합을 참조할 수 있도록 `NSUserDefaults`:

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

IOS 앱 및 watch 확장을 공통 파일 경로 사용 하 여 파일을 공유할 수도 있습니다.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

참고: 경로가 `null` 확인 합니다 [앱 그룹 구성](~/ios/watchos/app-fundamentals/app-groups.md) 프로 비전 프로필을 올바르게 구성 되었는지 및 개발 컴퓨터에 다운로드/설치 되었습니다.

자세한 내용은 참조 하십시오 합니다 [앱 그룹 기능](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 설명서.

## <a name="wormholesharp"></a>WormHoleSharp

WatchOS 1에 대 한 인기 있는 오픈 소스 메커니즘 (기반 [MMWormHole](https://github.com/mutualmobile/MMWormhole)) 부모 앱 및 watch 앱 간의 데이터 또는 명령을 전달 합니다.

IOS 앱에서 다음과 같은 앱 그룹을 사용 하 여 웜 홀을 구성 하 고 확장 보기 수 있습니다.

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

다운로드는 C# 버전 [WormHoleSharp](https://github.com/Clancey/WormHoleSharp)합니다.



## <a name="related-links"></a>관련 링크

- [GpsWatch (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WormHoleSharp (샘플)](https://github.com/Clancey/WormHoleSharp)
- [Apple의 WKInterfaceController 참조](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple의 포함 된 앱을 사용 하 여 데이터 공유](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
