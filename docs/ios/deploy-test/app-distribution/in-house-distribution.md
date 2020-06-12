---
title: Xamarin.iOS 앱에 대한 사내 배포
description: 이 문서에서는 Apple Enterprise Developer Program의 구성원으로 애플리케이션을 사내에 배포하는 방법에 대해 간략히 설명합니다.
ms.prod: xamarin
ms.assetid: 9466E51E-303E-466E-85D7-D0525E16BB37
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 854fecd7945c1090b475b3571678388b8e1cf127
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573237"
---
# <a name="in-house-distribution-for-xamarinios-apps"></a>Xamarin.iOS 앱에 대한 사내 배포

_이 문서에서는 Apple Enterprise Developer Program의 구성원으로 애플리케이션을 사내에 배포하는 방법에 대해 간략히 설명합니다._

Xamarin.iOS 앱이 개발되면 소프트웨어 개발 수명 주기의 다음 단계는 사용자에게 앱을 배포하는 것입니다. 독점 앱은 **Apple Developer Enterprise Program**을 통해 *사내*(이전에 엔터프라이즈라고 함)에 배포할 수 있으며 다음과 같은 이점을 제공합니다.

- Apple에서 검토를 위해 애플리케이션을 제출할 필요가 없습니다.
- 애플리케이션을 배포할 수 있는 디바이스의 수에는 제한이 없습니다
  - Apple에서는 사내 애플리케이션을 내부용으로만 사용해야 한다고 명확하게 언급하고 있습니다.

또한 Enterprise Program과 관련하여 주의해야 할 사항은 다음과 같습니다.

- 배포 및 테스트(TestFlight 포함)를 위해 iTunes Connect에 대한 액세스를 제공하지 않습니다.
- 구성원 자격 비용은 연간 299달러입니다.

모든 앱은 여전히 Apple에서 서명해야 합니다.

<a name="testing"></a>

## <a name="testing-your-application"></a>애플리케이션 테스트

애플리케이션 테스트는 임시 배포를 사용하여 수행됩니다. 테스트에 대한 자세한 내용은 [임시 배포](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md) 가이드의 단계를 따릅니다. 최대 100개의 디바이스에서만 테스트할 수 있습니다.

<a name="setup"></a>

## <a name="getting-set-up-for-distribution"></a>배포 설정

다른 Apple Developer Program과 마찬가지로 Apple Developer Enterprise Program에서 팀 관리자와 에이전트만 배포 인증서 및 프로비전 프로필을 만들 수 있습니다.

Apple Developer Enterprise Program 인증서는 3년 동안 지속되며, 프로비전 프로필은 1년 후에 만료됩니다.

만료된 인증서는 갱신할 수 없으며, 대신 [아래](#certificate)에서 설명한 대로 만료된 인증서를 새 인증서로 교체해야 합니다.

<a name="certificate"></a>

## <a name="creating-a-distribution-certificate"></a>배포 인증서 만들기

1. Apple Developer Member Center의 *인증서, 식별자 및 프로필* 섹션으로 이동합니다.
2. *인증서* 아래에서 **프로덕션**을 선택합니다.
3. 새 인증서를 만들기 위해 **+** 단추를 클릭합니다.
4. *프로덕션* 제목 아래에서 **사내 및 임시**를 선택합니다.

   [![](in-house-distribution-images/createcertmanually01.png "Select In-House and Ad Hoc")](in-house-distribution-images/createcertmanually01.png#lightbox)

5. [계속]을 클릭하고, 지시에 따라 키 집합 액세스를 통해 CSR(인증서 서명 요청)을 만듭니다.

   [![](in-house-distribution-images/createcertmanually02.png "Create a Certificate Signing Request via Keychain Access")](in-house-distribution-images/createcertmanually02.png#lightbox)

6. 지시한 대로 CSR을 만들었으면 [계속]을 클릭하고 CSR을 Member Center에 업로드합니다.

   [![](in-house-distribution-images/createcertmanually03.png "Upload the CSR to the Member Center")](in-house-distribution-images/createcertmanually03.png#lightbox)

7. [생성]을 클릭하여 인증서를 만듭니다.
8. 완성된 인증서를 다운로드하고 파일을 두 번 클릭하여 설치합니다.
9. 이 시점에서 인증서가 시스템에 설치되지만, Xcode에서 볼 수 있도록 프로필을 새로 고쳐야 할 수도 있습니다.

또는 Xcode의 [기본 설정] 대화 상자를 통해 인증서를 요청할 수도 있습니다. 이렇게 하려면 다음 단계를 수행합니다.

1. 팀을 선택하고 *세부 정보 보기*를 클릭합니다.

   [![](in-house-distribution-images/selectteam.png "Select your team")](in-house-distribution-images/selectteam.png#lightbox)

2. 다음으로, **iOS 배포 인증서** 옆에 있는 **만들기** 단추를 클릭합니다.

   [![](in-house-distribution-images/selectcert.png "Create the iOS Distribution Certificate")](in-house-distribution-images/selectcert.png#lightbox)

3. 다음으로, **더하기(+)** 단추를 클릭하고 **iOS 앱 스토어**를 선택합니다.

   [![](in-house-distribution-images/selectcert.png "Select iOS App Store")](in-house-distribution-images/selectcert.png#lightbox)

<a name="profile"></a>

## <a name="creating-a-distribution-provisioning-profile"></a>배포 프로비전 프로필 만들기

<a name="appid"></a>

### <a name="creating-an-app-id"></a>앱 ID 만들기

만든 다른 프로비전 프로필과 마찬가지로 앱 ID는 사용자의 디바이스에 배포되는 앱을 식별하는 데 필요합니다. 앱 ID를 아직 만들지 않았으면 다음 단계에 따라 만듭니다.

1. [Apple Developer Center](https://developer.apple.com/account/overview.action)에서 *인증서, 식별자 및 프로필* 섹션으로 이동합니다. **식별자** 아래에서 **앱 ID**를 선택합니다.
2. **+** 단추를 클릭하고 포털에서 식별할 수 있는 **이름**을 제공합니다.
3. 앱 접두사는 이미 팀 ID로 설정되어 있으며 변경할 수 없습니다. 명시적 또는 와일드카드 앱 ID를 선택하고, 다음과 같이 번들 ID를 역방향 DNS 형식으로 입력합니다. **명시적 앱**: com.[DomainName].[AppName] **Wildcard**:com.[DomainName].*
4. 앱에 필요한 [App Services](~/ios/get-started/installation/device-provisioning/index.md#provisioning-for-application-services)를 선택합니다.
5. **계속** 단추를 클릭하고 화면의 지시에 따라 새 앱 ID를 만듭니다.

배포 프로필을 만드는 데 필요한 필수 구성 요소가 있으면 아래 단계에 따라 해당 배포 프로필을 만듭니다.

1. Apple 프로비전 포털로 돌아가서 **프로비전** > **배포**를 차례로 선택합니다.

   [![](in-house-distribution-images/distribute01.png "Select Provisioning > Distribution")](in-house-distribution-images/distribute01.png#lightbox)

2. **+** 단추를 클릭하고 만들려는 배포 프로필 유형을 **사내**로 선택합니다.

   [![](in-house-distribution-images/distribute02.png "Create an In-House Distribution Profile")](in-house-distribution-images/distribute02.png#lightbox)

3. **계속** 단추를 클릭하고 드롭다운 목록에서 배포 프로필을 만들려는 앱 ID를 선택합니다.

   [![](in-house-distribution-images/distribute03.png "Select App ID from the dropdown list")](in-house-distribution-images/distribute03.png#lightbox)

4. **계속** 단추를 클릭하고 애플리케이션에 서명하는 데 필요한 배포 인증서를 선택합니다.

   [![](in-house-distribution-images/distribute04.png "Select distribution certificate required to sign the application")](in-house-distribution-images/distribute04.png#lightbox)

5. **계속** 단추를 클릭하고 새 배포 프로필에 대한 **이름**을 입력합니다.

   [![](in-house-distribution-images/distribute06.png "Enter a Name for the new Distribution Profile")](in-house-distribution-images/distribute06.png#lightbox)

6. **생성** 단추를 클릭하여 새 프로필을 만들고 프로세스를 완료합니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

 Mac용 Visual Studio에서 새 배포 프로필을 사용하려면, 먼저 Mac용 Visual Studio를 종료한 다음, [서명 ID 요청](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) 섹션의 지침에 따라 Xcode에서 사용 가능한 서명 ID 및 프로비전 프로필의 목록을 새로 고쳐야 할 수도 있습니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio에서 새 배포 프로필을 사용하려면, 먼저 Visual Studio를 종료한 다음, [서명 ID 요청](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) 섹션의 지침에 따라 빌드 호스트의 Mac에 있는 Xcode에서 사용 가능한 서명 ID 및 프로비전 프로필의 목록을 새로 고쳐야 할 수도 있습니다.

-----

<a name="inhouse"></a>

## <a name="distributing-your-app-in-house"></a>사내 앱 배포

Apple Developer Enterprise Program에서 정식 사용자는 애플리케이션을 배포하고 Apple에서 설정한 [지침](https://developer.apple.com/programs/enterprise/)을 준수해야 하는 사람입니다.

앱은 다음과 같은 다양한 수단을 사용하여 안전하게 배포할 수 있습니다.

- iTunes를 통해 로컬로
- MDM 서버
- 내부 보안 웹 서버
- 전자 메일

이러한 방법 중 하나를 사용하여 앱을 배포하려면 다음 섹션에서 설명하는 대로 먼저 IPA 파일을 만들어야 합니다.

### <a name="creating-an-ipa-for-in-house-deployment"></a>사내 배포를 위한 IPA 만들기

프로비전된 애플리케이션은 *IPA*라는 파일로 패키지할 수 있습니다. 이는 추가 메타데이터 및 아이콘과 함께 애플리케이션이 포함된 Zip 파일입니다. IPA는 프로비전 프로필에 포함된 디바이스에 직접 동기화할 수 있도록 애플리케이션을 iTunes에 로컬로 추가하는 데 사용됩니다.

IPA 만들기에 대한 자세한 내용은 [IPA 지원](~/ios/deploy-test/app-distribution/ipa-support.md) 가이드를 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.iOS 애플리케이션을 사내에 배포하는 방법에 대해 간략히 설명했습니다.

## <a name="related-links"></a>관련 링크

- [App Store 배포](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [임시 배포](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist 파일](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA 지원](~/ios/deploy-test/app-distribution/ipa-support.md)
