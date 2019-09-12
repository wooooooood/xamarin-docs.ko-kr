---
title: 앱 스토어 배포
description: 이 문서에서는 App Store에 Xamarin.iOS 애플리케이션을 배포하는 방법을 설명합니다. 배포 인증서를 만드는 방법, 배포 프로비저닝 프로필을 만드는 방법 및 iTunes Connect를 구성하고 앱을 제출하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: B07E2C1F-A6DF-43CB-BFB0-0252A5558467
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 08/23/2017
ms.openlocfilehash: 05034989c60868f8bff8164da7da90a7ff8788a3
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70763214"
---
# <a name="app-store-distribution"></a>앱 스토어 배포

Xamarin.iOS 앱이 개발되면 소프트웨어 개발 수명 주기의 다음 단계는 iTunes 앱 스토어를 사용하여 사용자에게 앱을 배포하는 것입니다. 이는 애플리케이션을 배포하는 가장 일반적인 방법입니다. Apple의 앱 스토어에 애플리케이션을 게시하면 전 세계의 소비자가 해당 애플리케이션을 사용할 수 있습니다.

> [!IMPORTANT]
> Apple은 2019년 3월부터 App Store에 제출된 모든 앱과 업데이트가 iOS 12.1 SDK 이상에서 빌드되어 Xcode 10.1 이상에 포함된다고 [발표했습니다](https://developer.apple.com/ios/submit/).
> 앱은 iPhone XS 및 12.9인치 iPad Pro 화면 크기도 지원해야 합니다.

애플리케이션을 배포하는 경우와 마찬가지로 애플리케이션을 배포하려면 적절한 *프로비전 프로필*을 사용하여 애플리케이션을 프로비전해야 합니다. 프로비전 프로필은 애플리케이션 ID 및 의도된 배포 메커니즘뿐만 아니라 코드 서명 정보도 포함된 파일입니다. 앱 스토어 배포가 아닌 경우 앱을 배포할 수 있는 디바이스에 대한 정보도 포함되어 있습니다.

> [!IMPORTANT]
> iTunes Connect를 사용하기 위해 앱 스토어에 앱을 게시하려면 사용자가 개인 또는 조직의 Apple Developer Program에 **반드시** 속해야 합니다. Apple Developer **Enterprise** Program의 구성원인 경우 이 페이지의 단계를 수행할 수 없습니다.

<a name="provisioning" />

## <a name="provisioning-an-app-for-app-store-distribution"></a>앱 스토어 배포를 위한 앱 프로비전

Xamarin.iOS 애플리케이션을 릴리스하려는 방법에 관계없이 특정 *배포 프로비전 프로필*을 작성해야 합니다. 이 프로필을 사용하면 애플리케이션을 iOS 디바이스에 설치할 수 있도록 디지털 서명하여 릴리스할 수 있습니다. 개발 프로비전 프로필과 마찬가지로 배포 프로필에는 다음 항목이 포함됩니다.

- 앱 ID
- 배포 인증서

개발 프로비전 프로필에 사용한 것과 동일한 **앱 ID** 및 **디바이스**를 선택할 수 있지만, 아직 없는 경우 앱 스토어에 앱을 제출할 때 조직을 식별하기 위한 배포 인증서를 만들어야 합니다. 배포 인증서를 만드는 방법에 대한 단계는 아래 섹션에서 설명합니다.

> [!NOTE]
> 팀 에이전트 및 관리자만 배포 인증서 및 프로비저닝 프로필을 만들 수 있습니다.

<a name="creatingcertificate" />

## <a name="creating-a-distribution-certificate"></a>배포 인증서 만들기

1. Apple Developer Member Center의 *인증서, 식별자 및 프로필* 섹션으로 이동합니다.
2. *인증서* 아래에서 **프로덕션**을 선택합니다.
3. 새 인증서를 만들기 위해 **+** 단추를 클릭합니다.
4. *프로덕션* 제목 아래에서 **앱 스토어 및 임시**를 선택합니다.

    [![](images/createcertmanually01.png "앱 스토어 및 임시 선택")](images/createcertmanually01.png#lightbox)
5. **계속**을 클릭하고, 지시에 따라 키 집합 액세스를 통해 CSR(인증서 서명 요청)을 만듭니다.

    [![](images/createcertmanually02.png "키 집합 액세스를 통해 CSR(인증서 서명 요청) 만들기")](images/createcertmanually02.png#lightbox)
6. 지시한 대로 CSR을 만들었으면 **계속**을 클릭하고 CSR을 Member Center에 업로드합니다.

    [![](images/createcertmanually03.png "Member Center에 CSR 업로드")](images/createcertmanually03.png#lightbox)

7. **생성**을 클릭하여 인증서를 만듭니다.
8. 마지막으로 완성된 인증서를 **다운로드**하고 파일을 두 번 클릭하여 설치합니다.
9. 이 시점에서 인증서가 시스템에 설치되지만, Xcode에서 볼 수 있도록 [프로필을 새로 고쳐야](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) 할 수도 있습니다.

또는 Xcode의 [기본 설정] 대화 상자를 통해 인증서를 요청할 수도 있습니다. 이렇게 하려면 다음 단계를 수행합니다.

1. 팀을 선택하고 **인증서 관리...** 를 클릭합니다.  [![](images/selectteam.png "팀을 선택하고 세부 정보 보기")](images/selectteam.png#lightbox)

2. 다음으로, **iOS 배포 인증서** 옆에 있는 **만들기** 단추를 클릭합니다.  [![](images/selectcert.png "iOS 배포 인증서 만들기")](images/selectcert.png#lightbox)

3. 팀 권한에 따라 아래와 같이 서명 ID가 생성되거나 팀 에이전트 또는 관리자가 승인할 때까지 기다려야 할 수 있습니다.  [ ![](images/generated.png "서명 ID 생성 및 대화 상자 표시")](images/generated.png#lightbox)

<a name="creatingprofile" />

## <a name="creating-a-distribution-profile"></a>배포 프로필 만들기

<a name="creatingappid" />

### <a name="creating-an-app-id"></a>앱 ID 만들기

만든 다른 프로비전 프로필과 마찬가지로 앱 ID는 사용자의 디바이스에 배포되는 앱을 식별하는 데 필요합니다. 앱 ID를 아직 만들지 않았으면 다음 단계에 따라 만듭니다.

1. [Apple Developer Center](https://developer.apple.com/account/overview.action)에서 *인증서, 식별자 및 프로필* 섹션으로 이동합니다. **식별자** 아래에서 **앱 ID**를 선택합니다.
2. **+** 단추를 클릭하고 포털에서 식별할 수 있는 **이름**을 제공합니다.
3. 앱 접두사는 이미 팀 ID로 설정되어 있으며 변경할 수 없습니다. 명시적 또는 와일드카드 앱 ID를 선택하고, 다음과 같이 번들 ID를 역방향 DNS 형식으로 입력합니다.
    - **명시적 앱 ID**: com.[DomainName].[AppName]
    - **와일드카드 앱 ID**: com.[DomainName].*
4. 앱에 필요한 [App Services](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices)를 선택합니다.
5. **계속** 단추를 클릭하고 화면의 지침에 따라 새 앱 ID를 만듭니다.

### <a name="creating-a-provisioning-profile"></a>프로비전 프로필 만들기

배포 프로필을 만드는 데 필요한 필수 구성 요소가 있으면 아래 단계에 따라 해당 배포 프로필을 만듭니다.

1. Apple 프로비전 포털로 돌아가서 **프로비전** > **배포**를 차례로 선택합니다.

    [![](images/distribute01.png "프로비전 > 배포 선택")](images/distribute01.png#lightbox)

2. **+** 단추를 클릭하고 만들려는 배포 프로필 유형을 **앱 스토어**로 선택합니다.

    [![](images/distribute02.png "앱 스토어 배포 프로필 만들기")](images/distribute02.png#lightbox)

3. **계속** 단추를 클릭하고 드롭다운 목록에서 배포 프로필을 만들려는 앱 ID를 선택합니다.

    [![](images/distribute03.png "드롭다운 목록에서 앱 ID 선택")](images/distribute03.png#lightbox)

4. **계속** 단추를 클릭하고 애플리케이션에 서명하는 데 필요한 인증서를 선택합니다.

    [![](images/distribute04.png "애플리케이션 서명에 필요한 인증서 선택")](images/distribute04.png#lightbox)

5. **계속** 단추를 클릭하고 Xamarin.iOS 애플리케이션이 실행될 수 있는 iOS 디바이스를 선택합니다.

    [![](images/distribute05.png "앱이 실행될 수 있는 iOS 디바이스 선택")](images/distribute05.png#lightbox)

6. **계속** 단추를 클릭하고 새 배포 프로필에 대한 **이름**을 입력합니다.

    [![](images/distribute06.png "새 배포 프로필에 대한 이름 입력")](images/distribute06.png#lightbox)

7. **생성** 단추를 클릭하여 새 프로필을 만들고 프로세스를 완료합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 Mac용 Visual Studio에서 새 배포 프로필을 사용하려면, 먼저 Mac용 Visual Studio를 종료한 다음, [서명 ID 요청](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) 섹션의 지침에 따라 Xcode에서 사용 가능한 서명 ID 및 프로비전 프로필의 목록을 새로 고쳐야 할 수도 있습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 Visual Studio에서 새 배포 프로필을 사용하려면, 먼저 Visual Studio를 종료한 다음, [서명 ID 요청](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) 섹션의 지침에 따라 빌드 호스트의 Mac에 있는 Xcode에서 사용 가능한 서명 ID 및 프로비전 프로필의 목록을 새로 고쳐야 할 수도 있습니다.

-----

<a name="selectprofile" />

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Xamarin.iOS 프로젝트에서 배포 프로필 선택

iTunes 앱 스토어에서 판매할 Xamarin.iOS 애플리케이션의 최종 빌드를 수행할 준비가 되면 위에서 만든 배포 프로필을 선택합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 Mac용 Visual Studio에서 다음을 수행합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 프로젝트 이름을 두 번 클릭하여 엽니다.
2. **구성** 드롭다운에서 **iOS 번들 서명** 및 **릴리스 | iPhone**을 선택합니다.

    ![](images/releasexs01.png "구성 드롭다운에서 릴리스 | iPhone 선택")
3. 대부분의 경우 **서명 ID** 및 **프로비전 프로필**은 기본값(**자동**)으로 그대로 둘 수 있으며, Mac용 Visual Studio에서는 Info.plist의 번들 식별자에 따라 올바른 프로필을 선택합니다.

    ![](images/releasexs02.png "기본값(자동)으로 설정된 서명 ID 및 프로비전 프로필")
4. 필요한 경우 드롭다운에서 서명 ID 및 배포 프로필(위에서 만든 항목)을 선택합니다.

    ![](images/releasexs03.png "서명 ID 및 배포 프로필 선택")
5. **확인** 단추를 클릭하여 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 Visual Studio에서 다음을 수행합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택하여 엽니다.
2. **구성** 드롭다운에서 **iOS 번들 서명** 및 **릴리스 | iPhone**을 선택합니다.

    ![](images/releasevs01.png "구성 드롭다운에서 릴리스 | iPhone 선택")
3. 대부분의 경우 **서명 ID** 및 **프로비전 프로필**은 기본값(**자동**)으로 그대로 둘 수 있으며, Visual Studio에서는 Info.plist의 번들 식별자에 따라 올바른 프로필을 선택합니다.

    ![](images/releasevs02.png "기본값(자동)으로 설정된 서명 ID 및 프로비전 프로필")
4. 필요한 경우 드롭다운에서 서명 ID 및 배포 프로필(위에서 만든 항목)을 선택합니다.

    ![](images/releasevs03.png "서명 ID 및 배포 프로필 선택")
5. 프로젝트의 속성에 대한 변경 내용을 저장합니다.

-----

<a name="itunesconnect" />

## <a name="configuring-your-application-in-itunes-connect"></a>iTunes Connect에서 애플리케이션 구성

애플리케이션이 성공적으로 프로비전되면, 다음 단계는 앱 스토어에서 iOS 애플리케이션을 관리하는 웹 기반 도구 모음인 [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa)에서 앱을 구성하는 것입니다.

먼저 iTunes Connect에서 Xamarin.iOS 애플리케이션을 제대로 설정하고 구성한 후에, 이를 검토하여 궁극적으로 앱 스토어에서 판매하거나 무료 앱으로 릴리스할 수 있도록 Apple에 제출해야 합니다.

자세한 내용은 [iTunes Connect에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) 설명서를 참조하세요.

<a name="submitting" />

## <a name="submitting-an-app-to-itunes-connect"></a>iTunes Connect에 앱 제출

애플리케이션이 배포 프로비전 프로필을 사용하여 서명되고 iTunes Connect에서 앱이 만들어지면, 검토를 위해 애플리케이션 이진 파일이 Apple에 업로드됩니다. Apple에서 성공적으로 검토되면 앱 스토어에서 사용할 수 있습니다.

앱 스토어에 애플리케이션을 게시하는 방법에 대한 자세한 내용은 [앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)를 참조하세요.

<a name="windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>.app 번들을 Windows로 다시 자동 복사

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="summary"></a>요약

이 문서에서는 앱 스토어에 배포하기 위해 Xamarin.iOS 애플리케이션을 준비하는 주요 구성 요소에 대해 설명했습니다.

## <a name="related-links"></a>관련 링크

- [iTunes Connect에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [사내 배포](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [임시 배포](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist 파일](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA 지원](~/ios/deploy-test/app-distribution/ipa-support.md)
