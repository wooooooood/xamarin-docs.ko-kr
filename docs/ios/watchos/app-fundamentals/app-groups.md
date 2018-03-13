---
title: "앱 그룹 작업"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 9365bc2707876816419bb5d136a6a1011cf129d7
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="working-with-app-groups"></a>앱 그룹 작업


앱 그룹을 사용하면 서로 다른 응용 프로그램(또는 응용 프로그램과 해당 확장 프로그램)이 공유 파일 저장소 위치에 액세스할 수 있습니다. 앱 그룹은 다음과 같은 데이터에 사용할 수 있습니다.

- Apple Watch [설정을](~/ios/watchos/app-fundamentals/settings.md)합니다.
- 공유 [NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults)합니다.
- 공유 [파일](~/ios/watchos/app-fundamentals/parent-app.md#files)합니다.

## <a name="configure-an-app-group"></a>앱 그룹을 구성

공유 위치를 사용 하 여 구성 된는 [App 그룹](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)에 구성 되어 있는 **인증서, 식별자 및 프로필** 섹션에서 [iOS Dev Center](https://developer.apple.com/devcenter/ios/)합니다. 이 값이 각 프로젝트에서 참조 해야 **Entitlements.plist**합니다.

### <a name="provisioning"></a>프로 비전

앱 그룹 식별자는 일반적으로 번들 ID를 마다와 `group.` 접두사입니다. 예를 들어, 번들 ID를 사용할 수 म `com.xamarin.WatchSettings` 및 응용 프로그램 그룹 `group.com.xamarin.WatchSettings`합니다.

[![](app-groups-images/app-group-sml.png "번들 ID com.xamarin.WatchSettings 및 응용 프로그램 그룹 group.com.xamarin.WatchSettings 사용")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

프로 비전 프로필을 구성할 뿐 아니라 **앱 그룹을 사용 하도록 설정** 에 **Entitlements.plist** 사용자가 선택한 ID 입력:

[![](app-groups-images/entitlements-sml.png "plist를 구성 하 고 ID를 입력 합니다.")](app-groups-images/entitlements.png#lightbox)


### <a name="deployment"></a>배포

앱 그룹을 구성 하면 [배포](~/ios/watchos/deploy-test/index.md#App_Groups) 프로 비전 합니다.


자세한 내용은 참조 하십시오는 [앱 그룹 기능](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 설명서입니다.


## <a name="related-links"></a>관련 링크

- [Apple의 포함 된 앱과 데이터 공유](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Apple의 앱 그룹 문서](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
