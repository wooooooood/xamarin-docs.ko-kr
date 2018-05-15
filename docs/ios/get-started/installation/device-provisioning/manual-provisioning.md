---
title: 수동 프로비전
description: Xamarin.iOS가 성공적으로 설치된 후 iOS 개발의 다음 단계는 iOS 장치를 프로비전하는 것입니다. 이 가이드는 개발 인증서 요청하기, 앱 서비스를 사용해 작업하기, 앱을 장치로 배포하기 등을 자세히 다룹니다.
ms.prod: xamarin
ms.assetid: E26ACC94-F4A5-4FF5-B7D4-BE596745A665
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 07/15/2017
ms.openlocfilehash: f604d41990a7a592a3d5207e7a12075c35ae661f
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="manual-provisioning"></a>수동 프로비전

_Xamarin.iOS가 성공적으로 설치된 후 iOS 개발의 다음 단계는 iOS 장치를 프로비전하는 것입니다. 이 가이드는 개발 인증서 요청하기, 앱 서비스를 사용해 작업하기, 앱을 장치로 배포하기 등을 자세히 다룹니다._

<a name="signingidentity" />

## <a name="creating-a-signing-identity"></a>서명 ID 만들기

개발 장치를 설정하는 첫 번째 단계는 서명 ID를 만드는 것입니다. 서명 ID는 다음 두 가지로 구성됩니다.

- 개발 인증서
- 개인 키

개발 인증서 및 연결된 [키](#keypairs)는 iOS 개발자에게 중요합니다. Apple의 ID를 설정하여 응용 프로그램에 디지털 서명을 넣는 것처럼 개발용 특정 장치 및 프로필에 연결합니다. Apple은 인증서를 확인하여 배포하도록 허용된 장치에 대한 액세스를 제어합니다.

개발 팀, 인증서 및 프로필의 Apple Members Center의 [Certificates, Identifiers & Profiles](https://developer.apple.com/account/overview.action)(인증서, 식별자 및 프로필) 섹션에 액세스하여 관리할 수 있습니다. 장치 또는 시뮬레이터용 코드를 빌드하려면 Apple에 서명 ID가 필요합니다.  

> [!IMPORTANT]
> 한 번에 iOS 개발 인증서 두 개만 가질 수 있다는 점에 유의해야 합니다. 더 만들어야 하는 경우에는 기존 인증서를 해지해야 합니다. 해지된 인증서를 사용하는 모든 컴퓨터는 앱에 서명할 수 없습니다.

서명 ID를 생성하려면 다음을 수행합니다.

1. [개발자 포털의 Certificates, Identifiers, and Profiles(인증서, 식별자 및 프로필) 섹션](https://developer.apple.com/account/overview.action)에 로그인하여 **iOS 앱** 열에서 **인증서** 섹션을 선택합니다. 그런 다음, **+** 를 눌러서 새 인증서를 만듭니다.

    [![](manual-provisioning-images/cert-plus.png "+를 클릭하여 새 인증서 만들기")](manual-provisioning-images/cert-plus.png#lightbox)

2. 인증서 유형에 **iOS App Development**(iOS 앱 개발) 옵션을 선택하고 **계속**을 클릭합니다. 이 화면은 계정 권한에 따라 다르게 보일 수 있습니다.

    [![](manual-provisioning-images/cert-first.png "인증서 유형에 iOS 앱 개발 옵션 선택")](manual-provisioning-images/cert-first.png#lightbox)

3. 인증서를 수동으로 생성하기 위해 업로드할 인증서 서명 요청을 요청합니다. 이렇게 하려면 Mac에서 **Keychain Access**(키 집합 액세스)를 시작합니다. 주 메뉴로 이동하여 아래 그림과 같이 **Certificate Assistant**(인증서 도우미) 및 **Request a Certificate from a Certificate Authority...**(인증 기관의 인증서 요청)를 선택합니다.

      [![](manual-provisioning-images/key-first.png "인증서 서명 요청 요청하기")](manual-provisioning-images/key-first.png#lightbox)

4. 정보를 입력하고 **디스크에 저장** 옵션을 선택합니다.

    [![](manual-provisioning-images/key-second.png "정보 입력")](manual-provisioning-images/key-second.png#lightbox)

5. 쉽게 찾을 수 있는 위치에 CSR을 저장합니다.

    [![](manual-provisioning-images/cert-third.png "CSR 저장")](manual-provisioning-images/cert-third.png#lightbox)

6. 프로비전 포털로 돌아가서 포털에 인증서를 업로드하고 제출합니다.

    [![](manual-provisioning-images/cert-second.png "포털에 인증서 업로드")](manual-provisioning-images/cert-second.png#lightbox)

    관리자 권한이 없는 경우에는 관리자 또는 팀 에이전트가 인증서를 승인해야 합니다.

7. 인증서가 승인되면 프로비전 포털에서 다운로드합니다.

    [![](manual-provisioning-images/status-dev.png "프로비전 포털에서 인증서 다운로드")](manual-provisioning-images/status-dev.png#lightbox)

8. 다운로드한 인증서를 두 번 클릭하여 키 집합 액세스를 시작하고 **My Certificates**(내 인증서) 패널을 열어서 새 인증서 및 연결된 개인 키를 표시합니다.

    [![](manual-provisioning-images/keychain.png "키 집합 액세스의 인증서")](manual-provisioning-images/keychain.png#lightbox)

<a name="keypairs" />

### <a name="understanding-certificate-key-pairs"></a>인증서 키 쌍 이해

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

개발자 프로필에는 인증서, 이와 연결된 키, 계정과 연결된 프로비전 프로필에 포함됩니다. 실제로 개발자 프로필에는 두 가지 버전이 있습니다. 한 가지는 개발자 포털에 있고 나머지는 로컬 Mac에 있습니다. 이 두 가지의 차이점은 포함된 키의 유형입니다. _포털의 프로필에는 인증서와 연결된 모든 공개 키가 있지만 로컬 Mac의 복사본에는 모든 개인 키가 포함됩니다_. 인증서가 유효하려면 키 쌍이 일치해야 합니다. 로컬 Mac에 개발자 프로필의 백업을 유지해야 합니다. 개인 키가 손실되면 모든 인증서와 프로비전 프로필을 다시 생성해야 하기 때문입니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

개발자 프로필에는 인증서, 이와 연결된 키, 계정과 연결된 프로비전 프로필에 포함됩니다. 실제로 개발자 프로필에는 두 가지 버전이 있습니다. 한 가지는 개발자 포털에 있고 나머지는 Mac에 있습니다. 이 두 가지의 차이점은 포함된 키의 유형입니다. _포털의 프로필에는 인증서와 연결된 모든 공개 키가 있지만 Mac의 복사본에는 모든 개인 키가 포함됩니다_. 인증서가 유효하려면 키 쌍이 일치해야 합니다. Xamarin 빌드 호스트의 Mac에 개발자 프로필의 백업을 유지해야 합니다. 개인 키가 손실되면 모든 인증서와 프로비전 프로필을 다시 생성해야 하기 때문입니다.

-----

> [!WARNING]
> 인증서 및 연결된 키가 손실되면 엄청난 혼란을 겪을 수 있습니다. 기존 인증서를 해지해야 하고 연결된 모든 장치(임의 배포용으로 등록된 장치 포함)를 다시 프로비전해야 하기 때문입니다. 개발 인증서 설정이 완료된 후에는 백업 복사본을 내보내서 안전한 곳에 보관하십시오. 이 작업을 수행하는 방법에 대한 자세한 내용은 Apple 설명서 [Maintaining Certificates](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html)(인증서 유지 관리) 가이드에서 Exporting and Importing Certificates and Profiles(인증서 및 프로필 내보내기 및 가져오기) 섹션을 참조하세요.

<a name="provisioning" />

## <a name="provisioning-an-ios-device-for-development"></a>개발용 iOS 장치 프로비전

Apple의 ID를 설정했고 개발 인증서가 준비되었으면 Apple 장치에 앱을 배포할 수 있도록 프로비전 프로필 및 필요한 엔터티를 설정해야 합니다. Xcode에서 지원하는 iOS 버전을 장치에서 실행해야 합니다. 장치, Xcode 또는 둘 다를 업데이트해야 할 수도 있습니다.

<a name="adddevice" />

## <a name="add-a-device"></a>장치 추가

개발용 프로비전 프로필을 만드는 경우 어떤 장치가 응용 프로그램을 실행할 수 있는지 명시해야 합니다. 이를 위해서는 연간 최대 100개의 장치를 개발자 포털에 추가할 수 있으며 이 곳에서 특정 프로비전 프로필에 추가할 장치를 선택할 수 있습니다. 개발자 포털에 장치를 추가하려면 Mac에서 다음 단계를 수행합니다.

1. Xcode를 시작합니다.
2. 프로비전할 장치를 제공된 USB 케이블을 사용하여 Mac에 연결합니다.
2. **창** 메뉴에서 **장치**를 선택합니다.

  [![](manual-provisioning-images/add01.png "창 메뉴에서 장치 선택")](manual-provisioning-images/add01.png#lightbox)

3. 장치 창 왼쪽의 **장치** 목록에서 원하는 iOS 장치를 선택합니다.
4. **식별자** 문자열을 선택하여 클립보드로 복사합니다.

  [![](manual-provisioning-images/add02.png "식별자 문자열 선택")](manual-provisioning-images/add02.png#lightbox)

5. Safari에서 [Apple Developer Center](https://developer.apple.com/membercenter/index.action)로 이동하여 로그인합니다.
6. **Certificates, Identifiers & Profiles**(인증서, 식별자 및 프로필) 링크를 클릭합니다.

  [![](manual-provisioning-images/add03.png "인증서, 식별자 및 프로필 링크 클릭")](manual-provisioning-images/add03.png#lightbox)

7. **장치** 링크를 클릭합니다.

  [![](manual-provisioning-images/add04.png "장치 링크 클릭")](manual-provisioning-images/add04.png#lightbox)

8. **+** 단추를 클릭합니다.

  [![](manual-provisioning-images/add05.png "+ 단추 클릭")](manual-provisioning-images/add05.png#lightbox)

9. 새 장치의 이름을 입력하고 위에서 복사한 장치 **식별자** 를 **UUID** 필드에 붙여 넣습니다.

  [![](manual-provisioning-images/add06.png "새 장치의 이름 및 장치 식별자 입력")](manual-provisioning-images/add06.png#lightbox)

10. **계속** 단추를 클릭합니다.
11. 마지막으로, 정보를 검토하고 **등록** 단추를 클릭합니다.

  [![](manual-provisioning-images/add07.png "정보 검토")](manual-provisioning-images/add07.png#lightbox)

Xamarin.iOS 응용 프로그램을 테스트하거나 디버그하는 데 사용할 iOS 장치에 위의 단계를 반복합니다.

개발자 포털에 장치를 추가한 후에는 프로비전 프로필을 만들어서 이 프로필에 장치를 추가해야 합니다.


<a name="provisioningprofile" />

## <a name="creating-a-development-provisioning-profile"></a>개발 프로비전 프로필 만들기

개발 인증서와 마찬가지로 프로비전 프로필은 Apple Members Center의 [Certificates, Identifiers & Profiles](https://developer.apple.com/account/overview.action)(인증서, 식별자 및 프로필) 섹션을 통해 수동으로 생성할 수 있습니다.

프로비전 프로필을 만들기 전에는 *앱 ID*를 만들어야 합니다. 앱 ID는 응용 프로그램을 고유하게 식별하는 역방향 DNS 스타일 문자열입니다. 아래 단계는 대부분의 응용 프로그램을 빌드하고 설치하는 데 사용할 수 있는 **와일드 카드 앱 ID**를 만드는 방법을 보여줍니다. **Explicit App ID**(명시적 앱 ID)는 응용 프로그램(일치하는 번들 ID 포함)을 하나만 설치할 수 있으며 일반적으로 Apple Pay 및 HealthKit와 같은 특정 iOS 기능에 사용됩니다. 명시적 앱 ID를 만드는 방법에 대한 자세한 내용은 [기능 사용](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드를 참조하세요.

### <a name="app-id"></a>앱 ID

1. [개발자 포털](https://developer.apple.com/account/overview.action)에서 Apple Developer Center의 *Certificate, Identifiers and Profiles*(인증서, 식별자 및 프로필) 섹션으로 이동합니다. **식별자** 아래에서 **앱 ID**를 선택합니다.
2. **+** 단추를 클릭하고 **이름**을 지정합니다.

    [![](manual-provisioning-images/appid05a.png "이름 입력")](manual-provisioning-images/appid05a.png#lightbox)
3. App 접두사를 미리 설정해야 합니다. 앱 접미사에 대한 **Wildcard App ID**(와일드카드 앱 ID)를 선택합니다. 번들 ID를 `com.[DomainName].*` 형식으로 입력합니다.

  [![](manual-provisioning-images/appid05b.png "번들 ID 입력")](manual-provisioning-images/appid05b.png#lightbox)

3. **계속** 단추를 클릭하고 화면의 지침에 따라 새 앱 ID를 만듭니다.

### <a name="provisioning-profile"></a>프로비전 프로필

앱 ID를 만든 후에는 프로비전 프로필을 생성할 수 있습니다. 프로비전 프로필에는 *어떤* 앱(와일드카드 앱 ID인 경우 여러 앱)이 프로필과 연결되어 있는지, *누가* 프로필을 사용할 수 있는지(추가된 개발자 인증서에 따라), *어떤* 장치에 앱을 설치할 수 있는지에 대한 정보가 포함됩니다.

개발용 프로비전 프로필을 수동으로 만들려면 다음을 수행합니다.

1. Safari를 사용하여 [Apple Developers Member Center](https://developer.apple.com/membercenter/index.action)로 이동하고 *Certificates, Identifiers & Profiles*(인증서, 식별자 및 프로필) 섹션에서 프로비전 프로필을 선택합니다.
2. 오른쪽 위 모서리에 있는 **+** 단추를 눌러서 새 프로필을 만듭니다.
3. **개발** 섹션에서 **iOS App Development**(iOS 앱 개발) 옆에 있는 라디오 단추를 선택하고 **계속**을 누릅니다.

    [![](manual-provisioning-images/provisioning-profile01.png "만들 프로필 형식 선택")](manual-provisioning-images/provisioning-profile01.png#lightbox)
4. 드롭다운 메뉴에서 사용할 앱 ID를 선택합니다.

    [![](manual-provisioning-images/provisioning-profile02.png "사용할 앱 ID 선택")](manual-provisioning-images/provisioning-profile02.png#lightbox)
5. 프로비전 프로필에 포함할 인증서를 선택하고 **계속**을 누릅니다.

    [![](manual-provisioning-images/provisioning-profile03.png "프로비전 프로필에 포함할 인증서 선택")](manual-provisioning-images/provisioning-profile03.png#lightbox)
6. 앱을 설치할 모든 장치를 선택합니다.

    [![](manual-provisioning-images/provisioning-profile04.png "앱을 설치할 모든 장치 선택")](manual-provisioning-images/provisioning-profile04.png#lightbox)
7. 프로비전 프로필에 식별 가능한 이름을 제공하고 **계속**을 눌러서 프로필을 만듭니다.

    [![](manual-provisioning-images/provisioning-profile05.png "프로비전 프로필에 식별 가능한 이름 입력")](manual-provisioning-images/provisioning-profile05.png#lightbox)
8. **다운로드**를 눌러서 Mac에 프로비전 프로필을 다운로드합니다.

    [![](manual-provisioning-images/provisioning-profile06.png "프로비전 프로필 다운로드")](manual-provisioning-images/provisioning-profile06.png#lightbox)
9. 파일을 두 번 클릭하여 Xcode에 프로비전 프로필을 설치합니다. 여는 것을 제외하고 Xcode에는 프로필이 설치되었다는 시각적인 단서가 표시되지 않을 수 있습니다. 이것은 **Xcode > 기본 설정 > 계정**으로 이동하여 확인할 수 있습니다. Apple ID를 선택하고 **자세히 보기...** 를 클릭합니다. 아래 그림과 같이 새 프로비전 프로필이 나열됩니다.

      [![](manual-provisioning-images/provisioning-profile07.png "Xcode에서 프로필 보기")](manual-provisioning-images/provisioning-profile07.png#lightbox)

프로비전 프로필 만들기가 완료되면 Mac용 Visual Studio 및 Visual Studio에서 모든 개발 인증서를 사용할 수 있도록 Xcode를 새로 고쳐야 할 수도 있습니다.

<a name="download" />

## <a name="downloading-profiles-and-certificates-in-xcode"></a>Xcode에서 프로필 및 인증서 다운로드

Apple Developer 포털에서 만든 인증서 및 프로비전 프로필은 Xcode에 자동으로 나타나지 않을 수 있습니다. 따라서 Mac용 Visual Studio 및 Visual Studio에서 액세스할 수 있도록 다운로드할 필요가 있습니다. Apple Developer 포털에서 만든 인증서를 업데이트하고 다운로드하려면 다음을 수행합니다.

1.   Mac용 Visual Studio 또는 Visual Studio를 종료합니다.
2.   Xcode를 시작합니다.
3.   **Xcode 메뉴 > 기본 설정...** 을 선택합니다.
4.   **계정** 탭을 클릭합니다.
5.   팀을 선택하고 **수동 프로필 다운로드** 단추를 선택합니다. [![](manual-provisioning-images/selectteam1.png "수동 프로필 다운로드")](manual-provisioning-images/selectteam1.png#lightbox)

6.   Xcode를 종료합니다.
7.  Mac용 Visual Studio 또는 Visual Studio를 시작합니다.

Mac용 Visual Studio 또는 Visual Studio에 새 인증서 또는 프로비전 프로필이 보이고 사용할 준비가 됩니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

> [!IMPORTANT]
> Xcode에서 업데이트된 새로운 인증서나 수정된 인증서를 보려면 Mac용 Visual Studio를 중지하고 다시 시작해야 할 수도 있습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!IMPORTANT]
> Xcode에서 업데이트된 새로운 인증서나 수정된 인증서를 보려면 Visual Studio를 중지하고 다시 시작해야 할 수도 있습니다.

-----

<a name="appservices" />

## <a name="provisioning-for-application-services"></a>응용 프로그램 서비스 프로비전

Apple은 Xamarin.iOS 응용 프로그램에 활성화할 수 있는 다양한 응용 프로그램 서비스(다른 이름: 기능)를 제공합니다. 이러한 응용 프로그램 서비스는 **앱 ID**를 만들 때 iOS 프로비전 포털에서 구성하고 Xamarin.iOS 응용 프로그램 프로젝트에 속하는 **Entitlements.plist** 파일에서도 구성해야 합니다. 앱에 응용 프로그램 서비스를 추가하는 방법에 대한 자세한 내용은 [기능 소개](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드 및 [자격 사용](~/ios/deploy-test/provisioning/entitlements.md) 가이드를 참조하세요.

* 필요한 앱 서비스를 포함하는 앱 ID를 만듭니다.
* 앱 ID를 포함하는 새로운 [프로비전 프로필](#provisioningprofile)을 만듭니다.
* Xamarin.iOS 프로젝트에서 자격 설정

<a name="deploy" />

## <a name="deploying-to-a-device"></a>장치에 배포

이 시점에서 프로비전은 완료되고, 장치에 앱을 배포할 준비가 되어 있어야 합니다. 이렇게 하려면 아래 단계를 수행합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

> [!IMPORTANT]
> 시작하기 전에 **Info.plist**에서 **수동 프로비저닝**을 선택해야 합니다.

1. 장치를 Mac에 연결합니다.
2. 프로젝트의 **Info.plist**에서 번들 식별자가 앱 ID와 일치하도록 합니다(앱 ID가 와일드카드인 경우 제외).

  ![](manual-provisioning-images/deploydevice01xs.png "식별자 입력")

3. 프로젝트를 마우스 오른쪽 단추로 클릭하여 프로젝트 옵션 대화 상자를 표시하고 **빌드 > iOS 번들 서명**으로 이동합니다. **서명 ID** 및 **프로비전 프로필** 옆에 있는 드롭다운 목록에서 Mac용 Visual Studio에 올바른 프로필이 표시되는지 확인하고 특정 ID와 프로필을 선택합니다.

  ![](manual-provisioning-images/deploydevice02xs.png "특정 ID와 프로필 선택")

**자동**으로 설정되어 있는 경우 2단계에서 설정한 번들 ID를 기반으로 Mac용 Visual Studio에서 ID와 프로필이 선택됩니다.

4. 빌드 구성이 시뮬레이터가 아닌 **iPhone** / **iPad**로 설정되어 있는지 확인합니다.
5. Mac용 Visual Studio에서 **실행**을 클릭하여 장치에서 실행되는 앱을 봅니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!IMPORTANT]
> 시작하기 전에 **프로젝트 > 속성 프로비전...** 에서 **수동 프로비전**을 선택해야 합니다.

1. 장치를 Mac 빌드 호스트에 플러그 인합니다.
2. 프로젝트의 **Info.plist**에서 번들 식별자가 앱 ID와 일치하도록 합니다.

  ![](manual-provisioning-images/servicevs01.png "식별자 입력")

3. 프로젝트를 마우스 오른쪽 단추로 클릭하여 프로젝트 옵션 대화 상자를 표시하고 **빌드 > iOS 번들 서명**으로 이동합니다. **서명 ID** 및 **프로비전 프로필** 옆에 있는 드롭다운 목록에서 Visual Studio에 올바른 프로필이 표시되는지 확인하고 특정 ID와 프로필을 선택합니다.

    **자동**으로 설정되어 있는 경우 2단계에서 설정한 번들 ID를 기반으로 Visual Studio에서 ID와 프로필이 선택됩니다.

4. 빌드 구성이 시뮬레이터가 아닌 **iPhone** 또는 **iPad**로 설정되어 있는지 확인합니다.
5. Visual Studio에서 **실행**을 클릭하여 장치에서 실행되는 앱을 봅니다.


-----

## <a name="summary"></a>요약

이 가이드에서는 Xamarin.iOS용 개발 환경을 설정하는 데 필요한 단계를 설명했습니다. 개발자, 개발자 팀, 앱을 실행할 수 있는 장치 및 개별 앱 ID에 대한 정보로 응용 프로그램을 코드 서명하는 방법을 알아보았습니다.


## <a name="related-links"></a>관련 링크

- [무료 프로비전](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [앱 배포](~/ios/deploy-test/app-distribution/index.md)
- [문제 해결](~/ios/deploy-test/troubleshooting.md)
- [Apple - 앱 배포 가이드](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
