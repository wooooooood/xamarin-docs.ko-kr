---
title: Xamarin에서 watchOS 부모 응용 프로그램 작업
description: 이 문서에서는 Xamarin에서 watchOS 부모 응용 프로그램을 사용 하는 방법을 설명 합니다. WatchKit 앱 확장, iOS 앱, 공유 저장소 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 08637fe13b28565e7c6ab1c89b291c6db3b81025
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73001470"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Xamarin에서 watchOS 부모 응용 프로그램 작업

> [!IMPORTANT]
> 아래 예제를 사용 하 여 부모 응용 프로그램에 액세스 하는 것은 watchOS 1 watch 앱 에서만 작동 합니다.

Watch 앱과 함께 제공 되는 iOS 앱 간에 통신 하는 방법에는 여러 가지가 있습니다.

- 조사식 확장은 iPhone의 백그라운드에서 실행 되는 부모 앱에 대해 [메서드를 호출할](#code) 수 있습니다.

- 시청 확장은 부모 iPhone 앱과 함께 [저장소 위치를 공유할](#storage) 수 있습니다.

- 사용자를 앱의 특정 인터페이스 컨트롤러에 보내서 사용자를 Watch 앱에 전달 하기 위해 핸드 오프를 사용 합니다.

부모 앱을 컨테이너 앱이 라고도 하기도 합니다.

<a name="code" />

## <a name="run-code"></a>코드 실행

[GpsWatch 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-gpswatch)에서는 감시 확장과 부모 iPhone 앱 간의 통신을 보여 줍니다.
조사식 확장은 `OpenParentApplication` 메서드를 사용 하 여 부모 iOS 앱을 대신 처리 하도록 요청할 수 있습니다.

이는 장기 실행 작업 (네트워크 요청 포함)에 특히 유용 합니다. 부모 iOS 앱만 백그라운드 처리를 활용 하 여 이러한 작업을 완료 하 고 검색 된 데이터를 조사식 확장에 액세스할 수 있는 위치에 저장할 수 있습니다.

### <a name="watch-kit-app-extension"></a>Watch 키트 앱 확장

Watch 앱 확장에서 `WKInterfaceController.OpenParentApplication`를 호출 합니다. 메서드 요청이 성공적으로 전송 되었는지 여부를 나타내는 `bool`을 반환 합니다. 응답 `Action`의 `error` 매개 변수를 확인 하 여 iPhone 앱에서 실행 되는 메서드 중에 발생 한 내용이 있는지 확인 합니다.

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

Watch 앱 확장의 모든 호출은 iPhone 앱의 `HandleWatchKitExtensionRequest` 메서드를 통해 라우팅됩니다.
Watch 앱에서 다양 한 요청을 수행 하는 경우이 메서드는 `userInfo` 사전을 쿼리하여 요청을 처리 하는 방법을 결정 해야 합니다.

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

[앱 그룹](~/ios/watchos/app-fundamentals/app-groups.md) 을 구성 하는 경우 iOS 8 확장 (감시 확장 포함)에서 부모 앱과 데이터를 공유할 수 있습니다.

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

다음 코드는 일반적인 `NSUserDefaults`집합을 참조할 수 있도록 watch 앱 확장과 부모 iPhone 앱 모두에 작성할 수 있습니다.

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

IOS 앱 및 감시 확장은 공통 파일 경로를 사용 하 여 파일을 공유할 수도 있습니다.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

참고: 경로를 `null` 하는 경우 [앱 그룹 구성을](~/ios/watchos/app-fundamentals/app-groups.md) 확인 하 여 프로 비전 프로필이 올바르게 구성 되 고 개발 컴퓨터에 다운로드/설치 되었는지 확인 합니다.

자세한 내용은 [앱 그룹 기능](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 설명서를 참조 하세요.

## <a name="wormholesharp"></a>WormHoleSharp

WatchOS 1에 대 한 인기 있는 오픈 소스 메커니즘 ( [Mmwormhole 홀](https://github.com/mutualmobile/MMWormhole)기반)은 부모 앱과 시청 앱 간에 데이터 또는 명령을 전달 합니다.

IOS 앱 및 조사식 확장에서 다음과 같은 앱 그룹을 사용 하 여 웜 홀을 구성할 수 있습니다.

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

[WormHoleSharp](https://github.com/Clancey/WormHoleSharp) 버전 C# 을 다운로드합니다.

## <a name="related-links"></a>관련 링크

- [GpsWatch (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [WormHoleSharp (샘플)](https://github.com/Clancey/WormHoleSharp)
- [Apple의 WKInterfaceController 참조](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [포함 하는 앱과의 Apple 공유 데이터](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
