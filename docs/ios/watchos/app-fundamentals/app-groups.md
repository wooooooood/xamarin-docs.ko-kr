---
title: Xamarin에서 watchOS 앱 그룹 작업
description: 이 문서에서는 watchOS 응용 프로그램에서 앱 그룹 및 해당 그룹의 용도에 대해 설명 합니다. 앱 그룹, 프로 비전 요구 사항, info.plist 고려 사항 및 배포를 구성 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: e117fce77e9cdc8d9e9dc8b9ed7b3aa22eca4e39
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73001704"
---
# <a name="working-with-watchos-app-groups-in-xamarin"></a>Xamarin에서 watchOS 앱 그룹 작업

앱 그룹을 사용하면 서로 다른 애플리케이션(또는 애플리케이션과 해당 확장 프로그램)이 공유 파일 스토리지 위치에 액세스할 수 있습니다. 앱 그룹은 다음과 같은 데이터에 사용할 수 있습니다.

- Apple Watch [설정](~/ios/watchos/app-fundamentals/settings.md).
- 공유 [Nsuserdefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- 공유 [파일](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>앱 그룹 구성

공유 위치는 [IOS 개발자 센터](https://developer.apple.com/devcenter/ios/)의 **인증서, 식별자 & 프로필** 섹션에서 구성 된 [앱 그룹](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)을 사용 하 여 구성 됩니다. 이 값은 각 프로젝트의 **info.plist**에서도 참조 되어야 합니다.

### <a name="provisioning"></a>프로비전

앱 그룹에는 식별자가 포함 됩니다. 일반적으로 번들 ID는 `group.` 접두사가 있습니다. 예를 들어 번들 ID `com.xamarin.WatchSettings`와 앱 그룹 `group.com.xamarin.WatchSettings`를 사용할 수 있습니다.

[![](app-groups-images/app-group-sml.png "Use the Bundle ID com.xamarin.WatchSettings and the app group   group.com.xamarin.WatchSettings")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

프로 비전 프로필을 구성 하는 것 외에도 **info.plist** 에서 **앱 그룹을 사용 하도록 설정** 하 고 선택한 ID를 입력 합니다.

[![](app-groups-images/entitlements-sml.png "Configure the plist and enter the ID")](app-groups-images/entitlements.png#lightbox)

### <a name="deployment"></a>배포

[배포](~/ios/watchos/deploy-test/index.md#App_Groups) 프로 비전에서 앱 그룹을 올바르게 구성 했는지 확인 합니다.

자세한 내용은 [앱 그룹 기능](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 설명서를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [포함 하는 앱과의 Apple 공유 데이터](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Apple의 앱 그룹 문서](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
