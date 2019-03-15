---
title: Xamarin.iOS에서 자격 사용
description: 자격은 특수 앱 기능이며 올바르게 사용하도록 구성된 애플리케이션에 부여된 보안 권한입니다.
ms.prod: xamarin
ms.assetid: 8A3961A2-02AB-4228-A41D-06CB4108D9D0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/13/2018
ms.openlocfilehash: 5ce778d0e6c2d023362ca5c9c691d77548dd7383
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672601"
---
# <a name="working-with-entitlements-in-xamarinios"></a>Xamarin.iOS에서 자격 사용

_자격은 특수 앱 기능이며 올바르게 사용하도록 구성된 애플리케이션에 부여된 보안 권한입니다._

iOS에서 앱은 애플리케이션과 특정 시스템 리소스 또는 사용자 데이터 사이의 액세스를 제한하는 일련의 규칙을 제공하는 _샌드박스_에서 실행됩니다. _자격_은 시스템이 샌드박스를 확장하여 앱에 추가 기능을 제공하도록 요청하는 데 사용됩니다.

앱의 기능을 확장하려면 앱의 Entitlements.plist 파일에 자격을 제공해야 합니다. 특정 기능만 확장이 가능하며 이러한 기능은 [기능 사용](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드에 나열되어 있고 [아래](#entitlement-key-reference) 설명되어 있습니다. 자격은 시스템에 키/값 쌍으로 전달되며 일반적으로 기능당 하나만 필요합니다. 구체적인 키와 값은 이 가이드의 뒷부분에 나오는 [자격 키 참조](#entitlement-key-reference) 섹션에 설명되어 있습니다.
Mac용 Visual Studio와 Visual Studio는 Entitlements.plist 편집기를 통해 Xamarin.iOS 앱에 자격을 추가할 수 있는 명확한 인터페이스를 제공합니다.
이 가이드에서는 Entitlements.plist 편집기를 소개하고 이 편집기를 사용하는 방법을 소개합니다. 또한 각 기능에 대해 iOS 프로젝트에 추가할 수 있는 모든 자격에 대한 참조를 제공합니다.

## <a name="entitlements-and-provisioning"></a>자격 및 프로비전

Entitlements.plist 파일은 자격을 지정하는 데 사용되며 응용 프로그램 번들에 서명하는 데 사용됩니다.

하지만 앱이 코드로 올바르게 서명되었는지 확인하려면 추가 프로비전이 필요합니다. 사용된 프로비전 프로필에는 필요한 기능이 활성화된 앱 ID가 있어야 합니다. 방법에 대한 자세한 내용은 [기능 사용](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드를 참조하세요.

> [!IMPORTANT]
> Entitlements.plist 파일은 기능을 사용하여 응용 프로그램의 올바른 속성을 입력하는 데 도움이 되지만 Apple 개발자 계정에 연결되어 있지 않기 때문에 프로비저닝 프로필을 생성할 수 없습니다. 애플리케이션을 배포하려면 개발자 포털을 사용하여 프로비전 프로필을 생성해야 합니다.

## <a name="set-entitlements-in-a-xamarinios-project"></a>Xamarin.iOS 프로젝트에서 자격 설정

앱 ID를 정의할 때 필요한 애플리케이션 서비스를 선택하고 구성하는 것 외에도, **Info.plist** 및 **Entitlements.plist** 파일을 편집하여 Xamarin.iOS 프로젝트에서 자격을 구성해야 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac용 Visual Studio에서 자격을 구성하려면 다음을 수행합니다.

1. **솔루션 탐색기**에서 **Info.plist** 파일을 두 번 클릭하여 편집용으로 엽니다.
2. **iOS 애플리케이션 대상** 섹션에서 애플리케이션의 이름을 입력하고, 앱 ID를 정의할 때 생성된 **번들 식별자**를 입력합니다.

  ![](entitlements-images/servicexs01.png "번들 식별자 입력")

3. 변경 내용을 **Info.plist** 파일에 저장합니다.
4. **솔루션 탐색기**에서 **Entitlements.plist** 파일을 두 번 클릭하여 편집용으로 엽니다.

    ![](entitlements-images/servicexs02.png "자격 편집")

5. 앱 ID를 만들 때 정의된 설정과 일치하도록 Xamarin.iOS 애플리케이션에 필요한 자격을 선택하고 구성합니다.
6. 변경 내용을 **Entitlements.plist** 파일에 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 자격을 구성하려면 다음을 수행합니다.

1. **솔루션 탐색기**에서 .**Info.plist** 파일을 마우스 오른쪽 단추로 클릭하고 **연결 프로그램…** 및 **속성 목록 편집기**를 선택하여 편집용으로 엽니다.
2. **iOS 애플리케이션 대상** 섹션에서 애플리케이션의 이름을 입력하고, 앱 ID를 정의할 때 생성된 **번들 식별자**를 입력합니다.

  ![](entitlements-images/servicevs01.png "번들 식별자 설정")

3. 변경 내용을 **Info.plist** 파일에 저장합니다.
4. **솔루션 탐색기**에서 **Entitlements.plist** 파일을 마우스 오른쪽 단추로 클릭하고 **연결 프로그램…** 및 **속성 목록 편집기**를 선택하여 편집용으로 엽니다.

    ![](entitlements-images/servicevs02.png "자격 편집")

    또는 **Entitlements.plist** 파일을 두 번 클릭하면 XML 원본 편집기가 열리고 Entitlement 속성과 키 값을 아래 [자격 키 참조](#entitlement-key-reference) 섹션의 세부 내용대로 설정할 수 있습니다.

5. 앱 ID를 만들 때 정의된 설정과 일치하도록 Xamarin.iOS 애플리케이션에 필요한 자격을 선택하고 구성합니다.
6. 변경 내용을 **Entitlements.plist** 파일에 저장합니다.

-----

## <a name="adding-a-new-entitlementsplist-file"></a>새 Entitlements.plist 파일 추가

자격은 Entitlements.plist 파일을 통해 앱에 추가됩니다. 이 파일은 Xamarin.iOS 파일에 기본적으로 포함되지만 이전 프로젝트에는 없을 수 있습니다.

Xamarin.iOS에 Entitlements.plist 파일을 추가하려면 다음을 수행합니다.

1.  프로젝트 파일을 마우스 오른쪽 단추로 클릭하고 **추가 > 새 파일...** 을 선택합니다.

    ![파일 추가 바로 가기 메뉴](entitlements-images/image1.png)
2.  새 파일 대화 상자에서 **iOS > 속성 목록**을 선택하고 Entitlements라고 이름을 지정합니다.

    ![새 파일 대화 상자](entitlements-images/image2.png)

## <a name="entitlement-key-reference"></a>자격 키 참조

자격 키는 Entitlements.plist 편집기의 원본 패널을 통해 추가할 수 있습니다. 필요한 키는 일반적으로 Entitlements.plist 편집기를 사용할 때 추가되지만 참조용으로 아래에 나열되어 있습니다.

### <a name="wallet"></a>Wallet

*   **설명**: 일반적으로 Passbook으로 알려진 Wallet은 패스를 저장 및 관리하는 앱입니다. 패스는 신용 카드, 상점 카드, 탑승권 또는 티켓일 수 있습니다.

    - **패스 유형 식별자**
        * **키**: com.apple.developer.pass-type-identifiers
        * **문자열**: `$(TeamIdentifierPrefix)*`

* **참고**:
    - 앱에서 모든 패스 유형을 허용할 수 있습니다. 앱을 제한하고 팀 패스 유형의 하위 집합만 허용하려면 문자열 값을 다음과 같이 설정합니다. `$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

    여기서 pass.$(CFBundleIdentifier)는 [위](~/ios/platform/passkit.md)에서 만든 패스 ID입니다.

<a name="icloud" />

### <a name="icloud"></a>iCloud

*   **설명**: iCloud는 iOS 사용자에게 콘텐츠를 저장하고 디바이스 간에 공유할 수 있는 편리하고 간단한 방법을 제공합니다. 개발자가 iCloud를 사용하여 사용자를 위한 스토리지 수단을 제공할 수 있는 네 가지 방법이 있습니다. 키-값 스토리지, UIDocument 스토리지, CoreData 및 CloudKit을 사용하여 개별 파일 및 디렉터리용 스토리지를 직접 제공합니다. 자세한 내용은 iCloud 소개 가이드를 참조하세요.

    - **iCloud 문서 및 CloudKit**
        - **키**: com.apple.developer.ubiquity-container-identifiers
        - **문자열**: `$(TeamIdentifierPrefix)$(CFBundleIdentifier)`
    - **iCloud KeyValue 스토리지**
        - **키**: com.apple.developer.ubiquity-kvstore-identifier
        - **문자열**: `$(TeamIdentifierPrefix)$(CFBundleIdentifier)`

* **참고**:
    - `$(TeamIdentifierPrefix)` 문자열은 developer.apple.com에 로그인하여 **Member Center > Your Account(계정) > Developer Account Summary(개발자 계정 요약)** 로 이동하여 팀 ID(또는 단일 개발자의 개인 ID)를 얻을 수 있습니다. 10자로 된 문자열(예: A93A5CM278)입니다.
    - `$(CFBundleIdentifier)` 문자열은`iCloud`로 시작되며 [기능 사용](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) 가이드의 단계에 따라 iCloud 컨테이너를 만들 때 설정됩니다.
    - $`(TeamIdentifierPrefix)` 및 `$(CFBundleIdentifier)` 자리 표시자를 사용할 수 있으며 빌드 시 올바른 값으로 대체됩니다.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

### <a name="app-groups"></a>앱 그룹

- **설명**: 앱 그룹을 사용하면 서로 다른 애플리케이션(또는 애플리케이션과 해당 확장 프로그램)이 공유 파일 스토리지 위치에 액세스할 수 있습니다.

    - **키**: com.apple.security.application-groups
    - **문자열**: group.$(CFBundleIdentifier)

<a name="apple-pay" />

### <a name="apple-pay"></a>Apple Pay

- **설명**: Apple pay를 사용하면 사용자의 iOS 디바이스를 통해 실제 상품을 구입할 수 있습니다.
    - **키**: com.apple.developer.in-app-payments
    - **문자열**: merchant.your.mechantid

### <a name="push-notifications"></a>푸시 알림

- **키**: aps-environment
- **문자열**: `development` 또는 `production`

### <a name="siri"></a>Siri

- **설명**: SiriKit을 통해 iOS 앱은 앱 확장 및 새로운 인텐트와 인텐트 UI 프레임워크를 사용하여 iOS 디바이스에서 Siri 및 맵 앱에 액세스할 수 있는 서비스를 제공할 수 있습니다. 자세한 내용은 SiriKit 소개 가이드를 참조하세요.
    - **키**: com.apple.developer.siri

### <a name="personal-vpn"></a>개인 VPN

- **키**: com.apple.developer.networking.vpn.api
- **문자열**: allow-vpn

### <a name="keychain-sharing"></a>키 집합 공유

- **설명**: 앱 개발자는 키 집합 공유를 통해 디바이스 키 집합에 저장된 암호를 동일한 팀에서 개발한 다른 앱과 공유할 수 있습니다. 문자열에 키 집합 액세스 그룹 식별자를 전달하여 액세스를 제한할 수 있습니다.
    - **키**: keychain-access-groups
    - **문자열**: $(AppIdentifierPrefix) $(CFBundleIdentifier)

### <a name="inter-app-audio"></a>내부 앱 오디오

- **설명**: 개발자는 내부 앱 오디오를 통해 앱 사이에 오디오를 스트리밍할 수 있습니다.
    - **키**: inter-app-audio
    - **부울:** 예

### <a name="associated-domains"></a>연결된 도메인

- **설명**: 범용 링크로 처리해야 하는 연결된 도메인은 이 자격과 함께 전달해야 합니다. 범용 링크는 앱과 웹 사이트 사이의 딥 링크 설정을 허용하기 위해 구현될 수 있습니다. 앱이 지원하는 각 도메인에 항목을 제공해야 하고 각 항목은 `applinks:`로 시작해야 합니다.
    - **키**: com.apple.developer.associated-domains
    - **문자열**: webcredentials:example.com

### <a name="data-protection"></a>데이터 보호

- **설명**: 데이터 보호를 사용하도록 설정하면 기본 제공된 암호화 하드웨어를 사용하여 앱에 사용되는 중요한 데이터가 암호화된 형식으로 저장됩니다. 기본적으로 보호 수준은 완전 보호(디바이스가 잠금 해제된 경우에만 파일에 액세스 가능)로 설정됩니다.
    - **키**: com.apple.developer.default-data-protection
    - **문자열**: NSFileProtectionComplete

### <a name="homekit"></a>HomeKit

- **설명**: HomeKit 프레임워크는 지원되는 홈 자동화 디바이스(모두 iOS 디바이스)를 설정, 구성 및 관리하기 위한 플랫폼을 제공합니다. HomeKit 사용에 대한 자세한 내용은 HomeKit 소개 가이드를 참조하세요.
    - **키**: com.apple.developer.homekit
    - **부울:** 예

### <a name="healthkit"></a>HealthKit

- **설명**: HealthKit은 건강 관련 정보를 위한 중앙 집중식의 조정된 보안 데이터 저장소를 제공하는 iOS 8에 도입된 프레임워크입니다. HealthKit 사용에 대한 자세한 내용은 HealthKit 소개 가이드를 참조하세요.
    - **키**: com.apple.developer.healthkit
    - **부울:** 예

### <a name="wireless-accessory-configuration"></a>무선 액세서리 구성

- **설명**: 무선 액세서리 구성을 사용하면 앱에서 MFi Wi-Fi 액세서리를 구성할 수 있습니다.
    - **키**: com.apple.external-accessory.wireless-configuration
    - **부울:** 예

### <a name="classkit"></a>ClassKit

- **설명**: ClassKit을 사용하면 교사가 앱에서 할당된 활동에 대한 학생의 진행 상황을 볼 수 있습니다.
    - **키**: com.apple.developer.ClassKit-environment
    - **문자열**: `development` 또는 `production`

## <a name="summary"></a>요약

이 가이드에서는 자격을 소개하고 Mac용 Visual Studio와 Visual Studio에서 사용하는 방법을 설명했습니다. 또한 각 기능에 대한 키/값 쌍 참조를 제공했습니다.
