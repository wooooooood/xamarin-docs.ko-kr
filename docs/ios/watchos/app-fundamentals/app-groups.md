---
title: WatchOS에서 Xamarin 앱 그룹 사용
description: 이 문서에서는 앱 그룹 및 watchOS 응용 프로그램에서 사용을 설명 합니다. 요구 사항, Entitlements.plist 고려 사항 및 배포를 프로 비전 하는 앱 그룹을 구성 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 78f6c03f73f0e4d8a74f826dd7bc25bbe325d545
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61411720"
---
# <a name="working-with-watchos-app-groups-in-xamarin"></a>WatchOS에서 Xamarin 앱 그룹 사용


앱 그룹을 사용하면 서로 다른 애플리케이션(또는 애플리케이션과 해당 확장 프로그램)이 공유 파일 저장소 위치에 액세스할 수 있습니다. 앱 그룹은 다음과 같은 데이터에 사용할 수 있습니다.

- Apple Watch [설정을](~/ios/watchos/app-fundamentals/settings.md)합니다.
- 공유 [NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults)합니다.
- 공유 [파일](~/ios/watchos/app-fundamentals/parent-app.md#files)합니다.

## <a name="configure-an-app-group"></a>앱 그룹 구성

공유 위치를 사용 하 여 구성 되어는 [앱 그룹](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)에서 구성 된 합니다 **인증서, 식별자 및 프로필** 섹션에서 [iOS 개발자 센터](https://developer.apple.com/devcenter/ios/)합니다. 이 값을 참조 하 여 각 프로젝트에도 해야 합니다 **Entitlements.plist**합니다.

### <a name="provisioning"></a>프로비전

앱 그룹 식별자는 일반적으로 번들 ID를 해야 합니다. 사용 하 여를 `group.` 접두사입니다. 예를 들어, 번들 ID를 사용할 수 있습니다 `com.xamarin.WatchSettings` 앱 그룹 및 `group.com.xamarin.WatchSettings`합니다.

[![](app-groups-images/app-group-sml.png "번들 ID com.xamarin.WatchSettings 및 앱 그룹 group.com.xamarin.WatchSettings 사용")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

프로비저닝 프로필을 구성 하는 데 뿐만 아니라 **앱 그룹을 사용 하도록 설정** 에 **Entitlements.plist** 선택한 ID를 입력:

[![](app-groups-images/entitlements-sml.png "Plist를 구성 하 고 ID를 입력 합니다.")](app-groups-images/entitlements.png#lightbox)


### <a name="deployment"></a>배포

앱 그룹에 올바르게 구성 해야 하 [배포](~/ios/watchos/deploy-test/index.md#App_Groups) 프로 비전 합니다.


자세한 내용은 참조 하십시오 합니다 [앱 그룹 기능](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 설명서.


## <a name="related-links"></a>관련 링크

- [Apple의 포함 된 앱을 사용 하 여 데이터 공유](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Apple의 앱 그룹 문서](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
