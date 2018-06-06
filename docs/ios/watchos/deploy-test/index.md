---
title: 배포 및 Xamarin 사용한 watchOS 앱 테스트
description: 이 문서에서는 배포 및 Xamarin을 사용 하 여 빌드한 watchOS 앱을 테스트 하는 방법에 설명 합니다. 배포 검사 목록에서는 명시적 설명 및 와일드 카드 응용 프로그램 Id, 앱 그룹을 참조 하며 합니다.
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 778583456e74bb7ed3a85dce96bcdbc487aef57a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790944"
---
# <a name="deploying-and-testing-watchos-apps-with-xamarin"></a>배포 및 Xamarin 사용한 watchOS 앱 테스트

## <a name="deployment-checklist"></a>배포 검사 목록

해야 할지 여부를 테스트 조사식에 배포 하거나 앱 스토어에 업로드 하는이 페이지에서 단계를 완료 하려면:

- 에 **iOS Dev Center**:
  - [앱 Id](#App_IDs) 만들어졌습니다.
  - [앱 그룹](#App_Groups) (필요한 경우)를 구성 합니다.
  - 배포에 프로 비전 프로필 파일을 만든

- 솔루션:

  - 확인 된 [프로젝트 참조 및 번들 Id](~/ios/watchos/get-started/installation.md) 설정 됩니다.
  - 아이콘이 확인 [올바르게 구성](~/ios/watchos/app-fundamentals/icons.md)합니다.
  - 모든 프로젝트에서 번들 버전 번호가 일치 항목을 확인 합니다.
  - 구성에서 **Entitlements.plist** (필요한 경우) 응용 프로그램 그룹에 대 한 합니다.

* 다음에 지침을 따릅니다.
  - [Apple Watch 테스트를 위한 배포](~/ios/watchos/deploy-test/device.md), 또는
  - [앱 스토어에 업로드](~/ios/watchos/deploy-test/appstore.md)합니다.

<a name="App_IDs"/>

## <a name="app-ids"></a>응용 프로그램 Id

에 설명 된 대로 [설정 지침](~/ios/watchos/get-started/installation.md), Watch 앱에서 모든 세 가지 프로젝트와 관련 된 번들 Id와 같은:

- Xamarin.iOS 통합 프로젝트- `com.xamarin.WatchKitCatalog`
- -WatchKit 확장 프로젝트 `com.xamarin.WatchKitCatalog.watchkitextension`
- 조사식 응용 프로그램 프로젝트- `com.xamarin.WatchKitCatalog.watchkitapp`

세 프로젝트 모두 일치 하는 배포 프로 비전 프로필을 사용 하거나 명시적으로 각 항목에 대해 응용 프로그램 Id 또는 앱 id입니다. 와일드 카드 필요

### <a name="explicit-app-ids"></a>명시적 응용 프로그램 Id

만들기는 **앱 ID** 각 각 프로젝트의 번들 ID (iOS 개발자 센터에서 다음과 같이 표시 됩니다)에 대 한:

![IOS 개발자 센터의에서 번들 Id](images/appids-specific-sml.png)

만들거나의 앱 Id를 구성 하는 경우 앱에 필요한 특정 기능을 사용 하도록 설정 해야 합니다. 여기에 푸시 알림과 앱 그룹 포함 될 수 있습니다.

각 응용 프로그램 id. 배포 프로 비전 프로필을 만드는 해야 합니다.

### <a name="wildcard-app-id"></a>와일드 카드 응용 프로그램 ID

또는 와일드 카드를 만들 수 있습니다 **앱 ID** 세 프로젝트 모두를 같은 일치 하는 `com.xamarin.*`합니다.

참고 일부 기능 (예: 푸시 알림) 와일드 카드 응용 프로그램 ID 함께 사용할 수 없습니다. 이러한 기능을 필요로 하는 응용 프로그램 명시적의 앱 Id를 만들어야 합니다.

배포에 대 한 하면 됩니다. 앱 ID 와일드 카드에 대 한 하나의 배포 프로 비전 프로필을 만들려면

<a name="App_Groups" />

## <a name="app-groups"></a>앱 그룹

조사식 확장 하 고 iOS 앱 간의 데이터를 공유 하는 응용 프로그램 그룹을 사용할 수 있습니다. 솔루션에 있는지 확인 해야 합니다.

- 구성에서 **App 그룹** Apple 개발자 포털에서 **인증서, 식별자 및 프로필** 섹션.

- 활성화 **앱 그룹** (제공 하 고는 **앱 그룹 ID**)에서 *둘 다* iOS 앱 및 조사식 확장 **앱 ID** 및  **Entitlements.plist**합니다.

### <a name="certificates-identifiers--profiles"></a>인증서, 식별자 및 프로필

에 항목을 생성 하는 응용 프로그램 그룹을 사용 하려면는 **앱 그룹** 화면입니다. 아래 예제에서는 그룹의 이름이 하지만, 응용 프로그램 Id에 대해 일반적으로 사용 되는 동일한 역방향 DNS 스타일으로 된 `group.` 접두사 (필요 함):

![식별자](images/appgroups-new-sml.png)

앱 그룹 목록에 나타납니다.

![식별자 목록](images/appgroups-setup-sml.png)

그룹을 만든 후에서 참조할 수 있습니다 프로그램 **앱 ID** 구성 합니다. 두 iOS 앱과 조사식 확장을 포함 해야 **의 앱 Id**합니다.

![사용 가능한 consifurations](images/appgroups-sml.png)

수행 **하지** Apple Watch 응용 프로그램 ID에 응용 프로그램 그룹을 사용 하도록 설정 조사식 자체에서 사용 되려면 필요 하지 않습니다.

### <a name="entitlementsplist"></a>Entitlements.plist

일부 응용 프로그램 기능 (예:. 응용 프로그램 그룹의 경우)에 자격을 설정 해야 합니다.
편집 하려면 두 번 클릭은 **Entitlements.plist** 이러한 프로젝트 파일:

- iOS 앱 프로젝트
- 조사식 확장 프로젝트

이어야 합니다.![Entitlements.plist 편집기](images/entitlements-plist-sml.png)

수행 **하지** Watch 앱 프로젝트에서 자격을 설정 합니다. 조사식 자체에서 사용 되려면 필요 하지 않습니다.

## <a name="related-links"></a>관련 링크

- [Apple WatchKit 제출 가이드](https://developer.apple.com/app-store/watch/)
