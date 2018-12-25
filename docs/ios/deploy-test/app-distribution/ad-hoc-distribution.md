---
title: Xamarin.iOS 앱에 대한 임시 배포
description: 이 문서에서는 다양한 그룹의 사람들과 함께 Xamarin.iOS 응용 프로그램을 테스트하는 데 주로 사용되는 임시 배포 기술에 대해 간략히 설명합니다.
ms.prod: xamarin
ms.assetid: 3B621CAD-103C-478A-97C3-829015F48D1A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 5950143532b2d1d026f73bb254507d7d3022cbf1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112301"
---
# <a name="ad-hoc-distribution-for-xamarinios-apps"></a>Xamarin.iOS 앱에 대한 임시 배포

_이 문서에서는 다양한 그룹의 사람들과 함께 Xamarin.iOS 응용 프로그램을 테스트하는 데 주로 사용되는 임시 배포 기술에 대해 간략히 설명합니다._

Xamarin.iOS 앱이 개발되면 소프트웨어 개발 수명 주기의 다음 단계는 테스트를 위해 사용자에게 앱을 배포하는 것입니다.

iTunes Connect는 앱 테스트를 관리하기 위한 하나의 옵션이며, [TestFlight](~/ios/deploy-test/testflight.md) 가이드에서 자세히 설명합니다. 그러나 Apple Developer Enterprise Program의 구성원은 iTunes Connect에 액세스할 수 없으므로 *임시* 배포가 이러한 앱을 테스트하는 가장 좋은 방법입니다.

Xamarin.iOS 응용 프로그램은 Apple Developer Program 및 Apple Developer Enterprise Program 모두에서 사용할 수 있는 *임시* 배포를 통해 사용자가 테스트할 수 있으며, 최대 100개의 iOS 디바이스를 테스트하도록 허용합니다.

임시 배포는 앱 스토어 승인을 요구하지 않는 이점이 있으며, 웹 서버 또는 iTunes를 통해 무선으로 설치할 수 있습니다. 그러나 개발 및 배포 모두에 대해 **100**개 디바이스로 제한되며 이러한 디바이스는 Member Center에서 UDID를 통해 수동으로 추가해야 합니다. 디바이스 추가에 대한 자세한 내용은 [디바이스 프로비전](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#adddevice) 가이드를 참조하세요.

임시 배포를 사용하려면 응용 프로그램 ID 및 이 응용 프로그램을 설치할 수 있는 디바이스뿐만 아니라 코드 서명 정보가 포함된 임시 *프로비전 프로필*도 사용하여 해당 응용 프로그램을 프로비전해야 합니다.

이 가이드에서는 임시 배포를 위한 프로비전 정보 및 Xamarin.iOS 앱을 배포하는 방법에 대한 정보를 제공합니다.

<a name="setup" />

## <a name="setting-up-for-distribution"></a>배포 설정

사내 배포를 위해 Xamarin.iOS 응용 프로그램을 릴리스하려는 경우에도 테스트용 특정 임시 배포 프로비전 프로필을 작성해야 합니다. 이 프로필을 사용하면 응용 프로그램을 iOS 디바이스에 설치할 수 있도록 디지털 서명하여 릴리스할 수 있습니다.

다음 섹션에서는 배포 인증서 및 배포 프로비전 프로필을 사용하여 설정하는 방법에 대해 설명합니다.

> [!NOTE]
> 팀 에이전트 및 관리자만 배포 인증서 및 프로비저닝 프로필을 만들 수 있습니다.

<a name="createcertificate" />

## <a name="create-a-distribution-certificate"></a>배포 인증서 만들기


1. Apple Developer Member Center의 *인증서, 식별자 및 프로필* 섹션으로 이동합니다.
2. *인증서* 아래에서 **프로덕션**을 선택합니다.
3. 새 인증서를 만들기 위해 **+** 단추를 클릭합니다.
4. *프로덕션* 제목 아래에서 프로그램 구성원 자격에 따라 **사내 및 임시** 또는 **앱 스토어 및 임시**를 선택합니다.

  [![](ad-hoc-distribution-images/cert-first-small.png "사내 및 임시 선택 또는 앱 스토어 및 임시 선택")](ad-hoc-distribution-images/cert-first-large.png#lightbox)

5. [계속]을 클릭하고, 지시에 따라 키 집합 액세스를 통해 CSR(인증서 서명 요청)을 만듭니다.

  [![](ad-hoc-distribution-images/createcertmanually02.png "키 집합 액세스를 통해 CSR(인증서 서명 요청) 만들기")](ad-hoc-distribution-images/createcertmanually02.png#lightbox)

6. 지시한 대로 CSR을 만들었으면 [계속]을 클릭하고 CSR을 Member Center에 업로드합니다.

  [![](ad-hoc-distribution-images/createcertmanually03.png "Member Center에 CSR 업로드")](ad-hoc-distribution-images/createcertmanually03.png#lightbox)

7. [생성]을 클릭하여 인증서를 만듭니다.
8. 마지막으로 완성된 인증서를 다운로드하고 파일을 두 번 클릭하여 설치합니다.
9. 이 시점에서 인증서가 시스템에 설치되지만, Xcode에서 볼 수 있도록 [프로필을 새로 고쳐야](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) 할 수도 있습니다.

또는 Xcode의 [기본 설정] 대화 상자를 통해 인증서를 요청할 수도 있습니다. 이렇게 하려면 아래 단계를 수행합니다.

1.   팀을 선택하고 **인증서 관리...** 를 클릭합니다. [![](ad-hoc-distribution-images/selectteam.png "팀 선택")](ad-hoc-distribution-images/selectteam.png#lightbox)

2.   다음으로 **더하기(+)** 단추를 클릭하고 **iOS 앱 스토어**를 선택합니다. [![](ad-hoc-distribution-images/selectcert.png "iOS 앱 스토어 선택")](ad-hoc-distribution-images/selectcert.png#lightbox)

<a name="createprofile" />

## <a name="create-a-distribution-provisioning-profile"></a>배포 프로비전 프로필 만들기

<a name="createappid" />

### <a name="create-an-app-id"></a>앱 ID 만들기
만든 다른 프로비전 프로필과 마찬가지로 앱 ID는 사용자의 디바이스에 배포되는 앱을 식별하는 데 필요합니다. 앱 ID를 아직 만들지 않았으면 다음 단계에 따라 만듭니다.


1. [Apple Developer Center](https://developer.apple.com/account/overview.action)에서 *인증서, 식별자 및 프로필* 섹션으로 이동합니다. **식별자** 아래에서 **앱 ID**를 선택합니다.
2. **+** 단추를 클릭하고 포털에서 식별할 수 있는 **이름**을 제공합니다.
3. 앱 접두사는 이미 팀 ID로 설정되어 있으며 변경할 수 없습니다. 명시적 또는 와일드카드 앱 ID를 선택하고, 다음과 같이 번들 ID를 역방향 DNS 형식으로 입력합니다.
    - **명시적 앱 ID**: `com.[DomainName].[AppName]`
    - **와일드카드 앱 ID**: `com.[DomainName].*`
4. 앱에 필요한 [App Services](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices)를 선택합니다.
5. **계속** 단추를 클릭하고 화면의 지시에 따라 새 앱 ID를 만듭니다.

배포 프로필을 만드는 데 필요한 필수 구성 요소가 있으면 아래 단계에 따라 해당 배포 프로필을 만듭니다.

1. Apple 프로비전 포털로 돌아가서 **프로비전 > 배포**를 선택합니다. [![](ad-hoc-distribution-images/distribute01.png "프로비전 > 배포 선택")](ad-hoc-distribution-images/distribute01.png#lightbox)

2. **+** 단추를 클릭하고 만들려는 배포 프로필 유형을 **임시**로 선택합니다.

    [![](ad-hoc-distribution-images/distribute02.png "임시 배포 형식 만들기")](ad-hoc-distribution-images/distribute02.png#lightbox)

3. **계속** 단추를 클릭하고 드롭다운 목록에서 배포 프로필을 만들려는 앱 ID를 선택합니다.

    [![](ad-hoc-distribution-images/distribute03.png "드롭다운 목록에서 앱 ID 선택")](ad-hoc-distribution-images/distribute03.png#lightbox)

4. **계속** 단추를 클릭하고 응용 프로그램에 서명하는 데 필요한 배포 인증서를 선택합니다.

    [![](ad-hoc-distribution-images/distribute04.png "응용 프로그램 서명에 필요한 배포 인증서 선택")](ad-hoc-distribution-images/distribute04.png#lightbox)

6. **계속** 단추를 클릭하고 새 배포 프로필에 대한 **이름**을 입력합니다.

    [![](ad-hoc-distribution-images/distribute06.png "새 배포 프로필에 대한 이름 입력")](ad-hoc-distribution-images/distribute06.png#lightbox)

7. **생성** 단추를 클릭하여 새 프로필을 만들고 프로세스를 완료합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac용 Visual Studio에서 새 배포 프로필을 사용하려면, 먼저 Mac용 Visual Studio를 종료한 다음, [Xcode에서 프로필 및 인증서 다운로드](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) 섹션의 지침에 따라 Xcode에서 사용 가능한 서명 ID 및 프로비전 프로필의 목록을 새로 고쳐야 할 수도 있습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 새 배포 프로필을 사용하려면, 먼저 Visual Studio를 종료한 다음, [Xcode에서 프로필 및 인증서 다운로드](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) 섹션의 지침에 따라 빌드 호스트의 Mac에 있는 Xcode에서 사용 가능한 서명 ID 및 프로비전 프로필의 목록을 새로 고쳐야 할 수도 있습니다.

-----

<a name="selectprofile" />

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Xamarin.iOS 프로젝트에서 배포 프로필 선택

Xamarin.iOS 응용 프로그램의 최종 빌드를 수행할 준비가 되면 위에서 만든 배포 프로필을 선택합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 Mac용 Visual Studio에서 다음을 수행합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 프로젝트 이름을 두 번 클릭하여 엽니다.
2. **구성** 드롭다운에서 **iOS 번들 서명** 및 빌드 형식을 선택합니다.

    ![](ad-hoc-distribution-images/releasexs01.png "구성 드롭다운에서 빌드 형식 선택")
3. 대부분의 경우 **서명 ID** 및 **프로비전 프로필**은 기본값(**자동**)으로 그대로 둘 수 있으며, Mac용 Visual Studio에서는 Info.plist의 번들 식별자에 따라 올바른 프로필을 선택합니다.

    ![](ad-hoc-distribution-images/releasexs02.png "기본값(자동)으로 설정된 서명 ID 및 프로비전 프로필")
4. 필요한 경우 드롭다운에서 서명 ID 및 배포 프로필(위에서 만든 항목)을 선택합니다.

    ![](ad-hoc-distribution-images/releasexs03.png "서명 ID 및 배포 프로필 선택")
5. **확인** 단추를 클릭하여 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
 Visual Studio에서 다음을 수행합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택하여 엽니다.
2. **구성** 드롭다운에서 **iOS 번들 서명** 및 빌드 형식을 선택합니다.

    ![](ad-hoc-distribution-images/releasevs01.png "구성 드롭다운에서 빌드 형식 선택")
3. 대부분의 경우 **서명 ID** 및 **프로비전 프로필**은 기본값(**자동**)으로 그대로 둘 수 있으며, Visual Studio에서는 Info.plist의 번들 식별자에 따라 올바른 프로필을 선택합니다.

    ![](ad-hoc-distribution-images/releasevs02.png "기본값(자동)으로 설정된 서명 ID 및 프로비전 프로필")
4. 필요한 경우 드롭다운에서 서명 ID 및 배포 프로필(위에서 만든 항목)을 선택합니다.

    ![](ad-hoc-distribution-images/releasevs03.png "서명 ID 및 배포 프로필 선택")
5. 프로젝트의 속성에 대한 변경 내용을 저장합니다.

-----

<a name="adhoc" />

## <a name="ad-hoc-distribution"></a>임시 배포

[TestFlight](~/ios/deploy-test/testflight.md)는 인기 있는 베타 테스트 및 배포 수단이지만, iTunes Connect의 일부이므로 **Apple Developer Enterprise Program**의 구성원은 사용할 수 없습니다.

iTunes Connect가 옵션이 아닌 경우 임시 배포를 사용하면 개발자가 다양한 디바이스에서 앱에 대한 베타 테스트를 수행할 수 있습니다. 임시 배포는 사내 배포와 비슷한 방식으로 작동하며 IPA를 만들어야 합니다. 그러면 무선으로 배포하거나 iTunes를 통해 수동으로 배포할 수 있습니다.

<a name="IPA_Creation" />

### <a name="ipa-support-for-ad-hoc-deployment"></a>임시 배포를 위한 IPA 지원

프로비전된 응용 프로그램은 *IPA*라는 파일로 패키지할 수 있습니다. 이는 추가 메타데이터 및 아이콘과 함께 응용 프로그램이 포함된 Zip 파일입니다. IPA는 프로비전 프로필에 포함된 디바이스에 직접 동기화할 수 있도록 응용 프로그램을 iTunes에 로컬로 추가하는 데 사용됩니다.

IPA 만들기에 대한 자세한 내용은 [IPA 지원](~/ios/deploy-test/app-distribution/ipa-support.md) 가이드를 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.iOS 응용 프로그램을 테스트하는 데 필요한 임시 배포 메커니즘에 대해 설명했습니다.


## <a name="related-links"></a>관련 링크

- [App Store 배포](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [사내 배포](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [iTunesMetadata.plist 파일](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA 지원](~/ios/deploy-test/app-distribution/ipa-support.md)
