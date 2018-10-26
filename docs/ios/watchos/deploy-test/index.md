---
title: 배포 및 Xamarin 사용 하 여 watchOS 앱 테스트
description: 이 문서에서는 배포 및 Xamarin에 내장 된 watchOS 앱을 테스트 하는 방법을 설명 합니다. 배포 검사 목록에서는 명시적 설명 및 와일드 카드 앱 Id 및 앱 그룹을 살펴봅니다.
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: a0738b03c4fa0ad975b872307bb17f387b1c5fd5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120959"
---
# <a name="deploying-and-testing-watchos-apps-with-xamarin"></a>배포 및 Xamarin 사용 하 여 watchOS 앱 테스트

## <a name="deployment-checklist"></a>배포 검사 목록

Watch 테스트 환경에 배포 하거나 앱 스토어에 업로드,이 페이지에서 단계를 완료 해야 합니다.

- 에 **iOS 개발자 센터**:
  - [앱 Id](#App_IDs) 만들어졌습니다.
  - [앱 그룹](#App_Groups) (필요한 경우)를 구성 합니다.
  - 배포 프로 비전 프로필 생성

- 솔루션:

  - 확인 합니다 [번들 Id와 프로젝트 참조](~/ios/watchos/get-started/installation.md) 설정 됩니다.
  - 확인 아이콘 [올바르게](~/ios/watchos/app-fundamentals/icons.md)합니다.
  - 모든 프로젝트에서 번들 버전 번호 일치 항목을 확인 합니다.
  - 구성 된 **Entitlements.plist** 앱 그룹 (필요한 경우)에 대 한 합니다.

* 다음 지침을 수행 합니다.
  - [테스트에 대 한 배포는 Apple Watch](~/ios/watchos/deploy-test/device.md), 또는
  - [앱 스토어에 업로드](~/ios/watchos/deploy-test/appstore.md)합니다.

<a name="App_IDs"/>

## <a name="app-ids"></a>앱 Id

에 설명 된 대로 합니다 [설치 지침](~/ios/watchos/get-started/installation.md), Watch 앱의 모든 세 가지 프로젝트와 관련 된 번들 Id와 같은:

- Xamarin.iOS 통합 프로젝트 `com.xamarin.WatchKitCatalog`
- WatchKit 확장 프로젝트 `com.xamarin.WatchKitCatalog.watchkitextension`
- Watch 앱 프로젝트 `com.xamarin.WatchKitCatalog.watchkitapp`

세 개의 프로젝트 모두 일치 하는 배포 프로 비전 프로필이 필요한 중 하나를 사용 하 여 명시적으로 각각에 대 한 앱 Id 또는 와일드 카드 앱 id입니다.

### <a name="explicit-app-ids"></a>명시적 앱 Id

만들기는 **앱 ID** 각 각 프로젝트의 번들 ID (iOS 개발자 센터에서 다음과 같이 표시 됩니다)에 대 한 합니다.

![IOS 개발자 센터의에서 번들 Id](images/appids-specific-sml.png)

을 만들거나 앱 Id를 구성 하는 경우 앱에 필요한 특정 기능을 사용 하도록 설정 해야 합니다. 이 푸시 알림 및 앱 그룹에 포함할 수 있습니다.

각 앱 ID에 대 한 배포 프로 비전 프로필을 만드는 해야 합니다.

### <a name="wildcard-app-id"></a>와일드 카드 앱 ID

또는 와일드 카드를 만들 수 있습니다 **앱 ID** 와 일치 하는 세 개의 프로젝트 모두 같은 `com.xamarin.*`합니다.

참고는 일부 기능 (예: 푸시 알림) 와일드 카드 앱 ID를 사용 하 여 사용할 수 없습니다. 앱에 이러한 기능에 필요한 경우 명시적 앱 Id를 만들어야 합니다.

배포의 경우만 해야 배포 프로 비전 프로필 만들 와일드 카드 앱 id입니다.

<a name="App_Groups" />

## <a name="app-groups"></a>앱 그룹

IOS 앱 및 Watch 확장 간에 데이터를 공유 하는 앱 그룹을 사용할 수 있습니다. 솔루션에 있는지를 확인 해야 합니다.

- 구성 된 **앱 그룹** Apple Developer 포털의 **인증서, 식별자 및 프로필** 섹션입니다.

- 사용 하도록 설정 **앱 그룹** (제공 된 **앱 그룹 ID**)에서 *둘 다* iOS 앱 및 Watch 확장의 **앱 ID** 및  **Entitlements.plist**합니다.

### <a name="certificates-identifiers--profiles"></a>인증서, 식별자 및 프로필

앱 그룹을 사용 하려면에 항목을 생성 합니다 **앱 그룹** 화면. 아래 예제에서 앱 Id에 대 한 하지만 일반적으로 사용 되는 동일한 역방향 DNS 스타일을 사용 하 여 그룹 이름은 `group.` 접두사 (필수)입니다.

![식별자](images/appgroups-new-sml.png)

앱 그룹 목록에 나타납니다.

![식별자 목록](images/appgroups-setup-sml.png)

참조 될 수는 그룹을 만든 후에 **앱 ID** 구성 합니다. IOS 앱 및 Watch 확장을 포함 해야 **앱 Id**합니다.

![사용 가능한 consifurations](images/appgroups-sml.png)

수행할 **되지** Apple Watch 앱 ID에 앱 그룹을 사용 하도록 설정 Watch 자체에서 사용 하도록 설정 하는 데 필요한 것입니다.

### <a name="entitlementsplist"></a>Entitlements.plist

일부 앱 기능 (예: 앱 그룹)를 사용 하면 자격을 설정 해야 합니다.
편집 하려면 두 번 클릭 합니다 **Entitlements.plist** 이러한 프로젝트에서 파일:

- iOS 앱 프로젝트
- 조사식 확장 프로젝트

.![Entitlements.plist 편집기](images/entitlements-plist-sml.png)

수행할 **되지** Watch 앱 프로젝트에서 자격을 사용 하도록 설정 합니다. Watch 자체에서 사용 하도록 설정 하는 데 필요한 것입니다.

## <a name="related-links"></a>관련 링크

- [Apple WatchKit 제출 가이드](https://developer.apple.com/app-store/watch/)
