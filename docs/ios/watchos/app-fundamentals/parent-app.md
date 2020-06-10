---
title: Xamarin에서 watchOS 부모 응용 프로그램 작업
description: 이 문서에서는 Xamarin에서 watchOS 부모 응용 프로그램을 사용 하는 방법을 설명 합니다. WatchOS 앱 확장, iOS 앱, 공유 저장소 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 49f2bdf63c286464073308cd1f17239692aa2395
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84567335"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Xamarin에서 watchOS 부모 응용 프로그램 작업

Watch 앱과 함께 제공 되는 iOS 앱 간에 통신 하는 방법에는 여러 가지가 있습니다.

- Watch 앱은 iPhone의 부모 앱에서 [코드를 실행할](#run-code) 수 있습니다.

- 시청 확장은 부모 iPhone 앱과 함께 [저장소 위치를 공유할](#shared-storage) 수 있습니다.

- 사용자를 앱의 특정 인터페이스 컨트롤러에 전송 하 여 알림 앱에 데이터를 전달 하려면 핸드 오프를 사용 합니다.

부모 앱을 컨테이너 앱이 라고도 하기도 합니다.

## <a name="run-code"></a>코드 실행

이 두 샘플에서는를 사용 하 여 `WCSession` 코드를 실행 하 고 watch 앱과 쌍을 이루는 iPhone 간에 메시지를 보내는 방법을 보여 줍니다.

- [연결 보기](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchconnectivity/)
- [SimpleWatchConnectivity](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-simplewatchconnectivity/) 

## <a name="shared-storage"></a>공유 스토리지

[앱 그룹](~/ios/watchos/app-fundamentals/app-groups.md) 을 구성 하는 경우 iOS 8 확장 (감시 확장 포함)에서 부모 앱과 데이터를 공유할 수 있습니다.

### <a name="nsuserdefaults"></a>NSUserDefaults

다음 코드는 일반적인 집합을 참조할 수 있도록 watch 앱 확장과 부모 iPhone 앱 모두에 작성할 수 있습니다 `NSUserDefaults` .

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

<a name="files"></a>

### <a name="files"></a>Files

IOS 앱 및 감시 확장은 공통 파일 경로를 사용 하 여 파일을 공유할 수도 있습니다.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

참고: 경로에서 `null` 프로 비전 프로필이 올바르게 구성 되 고 개발 컴퓨터에 다운로드/설치 되었는지 확인 하려면 [앱 그룹 구성을](~/ios/watchos/app-fundamentals/app-groups.md) 확인 합니다.

자세한 내용은 [앱 그룹 기능](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 설명서를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [Apple의 WKInterfaceController 참조](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [포함 하는 앱과의 Apple 공유 데이터](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
