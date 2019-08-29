---
title: Xamarin을 사용 하 여 watchOS Apps 배포 및 테스트
description: 이 문서에서는 Xamarin을 사용 하 여 빌드한 watchOS apps를 배포 하 고 테스트 하는 방법을 설명 합니다. 배포 검사 목록을 제공 하 고, 명시적 및 와일드 카드 앱 Id에 대해 설명 하 고, 앱 그룹을 살펴봅니다.
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: d950ceb18bd13378ced06ec7257300fc5bf4504b
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120182"
---
# <a name="deploying-and-testing-watchos-apps-with-xamarin"></a>Xamarin을 사용 하 여 watchOS Apps 배포 및 테스트

## <a name="deployment-checklist"></a>배포 검사 목록

테스트 감시에 배포 하거나 앱 스토어에 업로드 하는 경우에는이 페이지의 단계를 완료 해야 합니다.

- **IOS 개발자 센터**에서 다음을 수행 합니다.
  - [앱 id](#App_IDs) 를 만들었습니다.
  - 구성 된 [앱 그룹](#App_Groups) (필요한 경우).
  - 배포 프로 비전 프로필을 만들었습니다.

- 솔루션에서:

  - [번들 id 및 프로젝트 참조가](~/ios/watchos/get-started/installation.md) 설정 되었는지 확인 합니다.
  - 아이콘이 [올바르게 구성](~/ios/watchos/app-fundamentals/icons.md)되어 있는지 확인 합니다.
  - 모든 프로젝트에서 번들 버전 번호 일치를 확인 합니다.
  - 앱 그룹 (필요한 경우)에 대 한 **info.plist** 를 구성 합니다.

- 그런 다음 지침에 따라 다음을 수행 합니다.
  - [테스트를 위해 Apple Watch에 배포](~/ios/watchos/deploy-test/device.md)하거나
  - [앱 스토어에 업로드](~/ios/watchos/deploy-test/appstore.md)합니다.

<a name="App_IDs"/>

## <a name="app-ids"></a>앱 Id

[설치 지침](~/ios/watchos/get-started/installation.md)에 설명 된 것 처럼 조사식 응용 프로그램의 세 프로젝트 모두에는 다음과 같은 관련 번들 id가 있습니다.

- Xamarin.ios 통합 프로젝트-`com.xamarin.WatchKitCatalog`
- WatchKit 확장 프로젝트-`com.xamarin.WatchKitCatalog.watchkitextension`
- 응용 프로그램 프로젝트 보기-`com.xamarin.WatchKitCatalog.watchkitapp`

세 프로젝트 모두에 대해 명시적으로 앱 Id를 사용 하거나 와일드 카드 앱 ID를 사용 하 여 일치 하는 배포 프로 비전 프로필이 필요 합니다.

### <a name="explicit-app-ids"></a>명시적 앱 Id

각 프로젝트의 번들 ID (iOS 개발자 센터에서 다음과 같이 표시 됨)에 대 한 **앱 id** 를 만듭니다.

![IOS 개발자 센터의 번들 Id](images/appids-specific-sml.png)

앱 Id를 만들거나 구성할 때 앱에 필요한 특정 기능을 사용 하도록 설정 해야 합니다. 여기에는 푸시 알림 및 앱 그룹이 포함 될 수 있습니다.

각 앱 ID에 대 한 배포 프로 비전 프로필을 만들어야 합니다.

### <a name="wildcard-app-id"></a>와일드 카드 앱 ID

또는와 `com.xamarin.*`같은 세 개의 프로젝트 모두와 일치 하는 와일드 카드 **앱 ID** 를 만들 수 있습니다.

일부 기능은 와일드 카드 앱 ID (예: 푸시 알림)와 함께 사용할 수 없습니다. 앱이 이러한 기능을 필요로 하는 경우 명시적인 앱 Id를 만들어야 합니다.

배포의 경우 와일드 카드 앱 ID에 대해 하나의 배포 프로 비전 프로필을 만들어야 합니다.

<a name="App_Groups" />

## <a name="app-groups"></a>앱 그룹

앱 그룹을 사용 하 여 iOS 앱과 시청 확장 간에 데이터를 공유할 수 있습니다. 솔루션에 다음이 있는지 확인 해야 합니다.

- Apple 개발자 포털 **인증서, 식별자 & 프로필** 섹션에서 **앱 그룹** 을 구성 했습니다.

- IOS 앱과 시청 확장의 **앱 ID** 및 **Info.plist** *모두* 에서 **앱 그룹** 을 사용 하도록 설정 하 고 **앱 그룹 ID**를 제공 합니다.

### <a name="certificates-identifiers--profiles"></a>인증서, 식별자 & 프로필

앱 그룹을 사용 하려면 **앱 그룹** 화면에서 항목을 만듭니다. 아래 예제에서 그룹은 일반적으로 앱 id에 사용 되는 것과 동일한 역방향 DNS 스타일로 이름이 지정 되지만 `group.` 접두사 (필수)가 사용 됩니다.

![식별자입니다.](images/appgroups-new-sml.png)

그러면 앱 그룹이 목록에 표시 됩니다.

![식별자 목록](images/appgroups-setup-sml.png)

그룹이 만들어지면 **앱 ID** 구성에서 참조할 수 있습니다. IOS 앱과 조사식 확장 **앱 id**를 모두 포함 해야 합니다.

![사용 가능한 구성](images/appgroups-sml.png)

Apple Watch 앱 ID에서 앱 그룹을 사용 하도록 설정 **하지** 마세요. Watch 자체에서 사용 하도록 설정할 필요는 없습니다.

### <a name="entitlementsplist"></a>Entitlements.plist

일부 앱 기능 (예: 앱 그룹) 자격을 설정 해야 합니다.
다음 프로젝트에서 **info.plist** 파일을 편집 하려면 두 번 클릭 합니다.

- iOS 앱 프로젝트
- 확장 프로젝트 감시

을 선택합니다.![Info.plist 편집기](images/entitlements-plist-sml.png)

Watch 앱 프로젝트에서 자격을 사용 하도록 설정 **하지** 마세요. Watch 자체에서 사용 하도록 설정할 필요는 없습니다.

## <a name="related-links"></a>관련 링크

- [Apple WatchKit 제출 가이드](https://developer.apple.com/app-store/watch/)
