---
title: Xamarin.iOS에 대한 수동 프로비전
description: Xamarin.iOS가 성공적으로 설치된 후 iOS 개발의 다음 단계는 iOS 디바이스를 프로비전하는 것입니다. 이 가이드에서는 수동 프로비저닝을 사용하여 개발 인증서와 프로필을 설정하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: E26ACC94-F4A5-4FF5-B7D4-BE596745A665
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/06/2020
ms.openlocfilehash: 9333750432395d008a5454e293648f4e594ae112
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571690"
---
# <a name="manual-provisioning-for-xamarinios"></a>Xamarin.iOS에 대한 수동 프로비전

_Xamarin.iOS가 성공적으로 설치된 후 iOS 개발의 다음 단계는 iOS 디바이스를 프로비전하는 것입니다. 이 가이드에서는 수동 프로비저닝을 사용하여 개발 인증서와 프로필을 설정하는 방법을 설명합니다._

> [!NOTE]
> 이 페이지에서 지침은 Apple 개발자 프로그램에 대한 유료 액세스 권한을 가진 개발자와 관련이 있습니다. 무료 계정이 있는 경우 디바이스 테스트에 대한 자세한 내용은 [무료 프로 비전](~/ios/get-started/installation/device-provisioning/free-provisioning.md) 가이드를 살펴보세요.

## <a name="create-a-development-certificate"></a>개발 인증서 만들기

개발 디바이스를 설정하는 첫 번째 단계는 서명 인증서를 만드는 것입니다. 서명 인증서는 다음과 같은 두 가지 요소로 구성됩니다.

- 개발 인증서
- 프라이빗 키

개발 인증서 및 연결된 [키](#understanding-certificate-key-pairs)는 iOS 개발자에게 중요합니다. Apple의 ID를 설정하여 애플리케이션에 디지털 서명을 넣는 것처럼 개발용 특정 디바이스 및 프로필에 연결합니다. Apple은 인증서를 확인하여 배포하도록 허용된 디바이스에 대한 액세스를 제어합니다.

개발 팀, 인증서 및 프로필의 Apple Members Center의 [인증서, 식별자 및 프로필](https://developer.apple.com/account/resources/certificates/list)(로그인 필수) 섹션에 액세스하여 관리할 수 있습니다. 디바이스 또는 시뮬레이터용 코드를 빌드하려면 Apple에 서명 ID가 필요합니다.  

> [!IMPORTANT]
> 한 번에 iOS 개발 인증서 두 개만 가질 수 있다는 점에 유의해야 합니다. 더 만들어야 하는 경우에는 기존 인증서를 해지해야 합니다. 해지된 인증서를 사용하는 모든 컴퓨터는 앱에 서명할 수 없습니다.

수동 프로비저닝 프로세스를 시작하기 전에 [Apple 계정 관리](~/cross-platform/macios/apple-account-management.md) 가이드에 설명된 대로 Visual Studio에 Apple 개발자 계정을 추가했는지 확인해야 합니다. Apple 개발자 계정을 추가했으면 다음을 수행하여 서명 인증서를 생성합니다.

1. Visual Studio에서 Apple 개발자 계정 창으로 이동합니다.
    1. Mac: **Visual Studio > 기본 설정 > Apple 개발자 계정**
    2. Windows: **도구 > 옵션 > Xamarin > Apple 계정**

2. 팀을 선택하고 **세부 정보 보기…** 를 클릭합니다.
3. **인증서 만들기**를 클릭하고 **Apple 개발** 또는 **iOS 개발**을 선택합니다. 올바른 권한이 있는 경우 몇 초 후에 새 서명 ID가 표시됩니다.

### <a name="understanding-certificate-key-pairs"></a>인증서 키 쌍 이해

개발자 프로필에는 인증서, 이와 연결된 키, 계정과 연결된 프로비전 프로필에 포함됩니다. 실제로 개발자 프로필에는 두 가지 버전이 있습니다. 한 가지는 개발자 포털에 있고 나머지는 로컬 Mac에 있습니다. 이 두 가지의 차이점은 포함된 키의 유형입니다. _포털의 프로필에는 인증서와 연결된 모든 퍼블릭 키가 있지만 로컬 Mac의 복사본에는 모든 프라이빗 키가 포함됩니다_. 인증서가 유효하려면 키 쌍이 일치해야 합니다.

> [!WARNING]
> 인증서 및 연결된 키가 손실되면 엄청난 혼란을 겪을 수 있습니다. 기존 인증서를 해지해야 하고 연결된 모든 디바이스(임시 배포용으로 등록된 디바이스 포함)를 다시 프로비저닝해야 하기 때문입니다. 개발 인증서 설정이 완료된 후에는 백업 복사본을 내보내서 안전한 곳에 보관하십시오. 이 작업을 수행하는 방법에 대한 자세한 내용은 Apple 설명서 [Maintaining Certificates](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html)(인증서 유지 관리) 가이드에서 Exporting and Importing Certificates and Profiles(인증서 및 프로필 내보내기 및 가져오기) 섹션을 참조하세요.

<a name="provisioning"></a>

## <a name="provision-an-ios-device-for-development"></a>개발용 iOS 디바이스 프로비저닝

Apple의 ID를 설정했고 개발 인증서가 준비되었으면 Apple 디바이스에 앱을 배포할 수 있도록 프로비저닝 프로필 및 필요한 엔터티를 설정해야 합니다. Xcode에서 지원하는 iOS 버전을 디바이스에서 실행해야 합니다. 디바이스, Xcode 또는 둘 다를 업데이트해야 할 수도 있습니다.

<a name="adddevice"></a>

## <a name="add-a-device"></a>디바이스 추가

개발용 프로비전 프로필을 만드는 경우 어떤 디바이스가 애플리케이션을 실행할 수 있는지 명시해야 합니다. 이를 위해서는 연간 최대 100개의 디바이스를 개발자 포털에 추가할 수 있으며 이 곳에서 특정 프로비전 프로필에 추가할 디바이스를 선택할 수 있습니다. 개발자 포털에 디바이스를 추가하려면 Mac에서 다음 단계를 수행합니다.

1. 프로비전할 디바이스를 제공된 USB 케이블을 사용하여 Mac에 연결합니다.
2. Xcode를 열고 **창 > 디바이스 및 시뮬레이터**로 이동합니다.
3. **디바이스** 탭의 왼쪽 메뉴에서 디바이스를 선택합니다.
4. **식별자** 문자열을 선택하여 클립보드로 복사합니다.

   ![iOS 식별자 문자열 위치가 강조 표시된 Xcode 디바이스 및 시뮬레이터 창](manual-provisioning-images/xcode-devices.png)

5. 웹 브라우저에서 [개발자 포털의 디바이스 섹션](https://developer.apple.com/account/resources/devices/list)으로 이동하여 **+** 단추를 클릭합니다.

   ![추가 단추가 강조 표시된 Apple 개발자 사이트의 디바이스 페이지 스크린샷](manual-provisioning-images/developer-portal-devices.png)

6. 올바른 **플랫폼**을 설정하고 새 디바이스의 이름을 지정합니다. 앞에서 복사한 식별자를 **디바이스 ID** 필드에 붙여넣습니다.

    ![필드가 올바르게 입력된 새 디바이스 등록 페이지 스크린샷](manual-provisioning-images/new-device-info.png)

7. **Continue(계속)** 를 클릭합니다.
8. 정보를 검토하고 **등록**을 클릭합니다.

Xamarin.iOS 애플리케이션을 테스트하거나 디버그하는 데 사용할 iOS 디바이스에 위의 단계를 반복합니다.

<a name="provisioningprofile"></a>

## <a name="create-a-development-provisioning-profile"></a>개발 프로비저닝 프로필 만들기

개발자 포털에 디바이스를 추가한 후에는 프로비전 프로필을 만들어서 이 프로필에 디바이스를 추가해야 합니다. 

프로비전 프로필을 만들기 전에는 *앱 ID*를 만들어야 합니다. 앱 ID는 애플리케이션을 고유하게 식별하는 역방향 DNS 스타일 문자열입니다. 아래 단계는 대부분의 애플리케이션을 빌드하고 설치하는 데 사용할 수 있는 **와일드 카드 앱 ID**를 만드는 방법을 보여줍니다. **Explicit App ID**(명시적 앱 ID)는 애플리케이션(일치하는 번들 ID 포함)을 하나만 설치할 수 있으며 일반적으로 Apple Pay 및 HealthKit와 같은 특정 iOS 기능에 사용됩니다. 명시적 앱 ID를 만드는 방법에 대한 자세한 내용은 [기능 사용](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드를 참조하세요.

### <a name="new-wildcard-app-id"></a>새 와일드카드 앱 ID

1. [개발자 포털의 식별자 섹션](https://developer.apple.com/account/resources/identifiers/list)으로 이동하여 **+** 단추를 클릭합니다.
2. **앱 ID**를 선택하고 **계속**을 클릭합니다.
3. **설명**을 입력합니다. 그런 다음 **번들 ID**를 **와일드카드**로 설정하고 `com.[DomainName].*` 형식으로 ID를 입력합니다.

   ![필수 필드가 입력된 새 앱 ID 등록 페이지 스크린샷](manual-provisioning-images/new-app-id.png)

4. **Continue(계속)** 를 클릭합니다.
5. 정보를 검토하고 **등록**을 클릭합니다.

### <a name="new-provisioning-profile"></a>새 프로비저닝 프로필

앱 ID를 만든 후에는 프로비저닝 프로필을 만들 수 있습니다. 프로비저닝 프로필에는 ‘어떤’ 앱(와일드카드 앱 ID인 경우 여러 앱)이 프로필과 연결되어 있는지, ‘누가’ 프로필을 사용할 수 있는지(추가된 개발자 인증서에 따라), ‘어떤’ 디바이스에 앱을 설치할 수 있는지에 대한 정보가 포함됩니다.   

개발용 프로비저닝 프로필을 수동으로 만들려면 다음을 수행합니다.

1. [개발자 포털의 프로필 섹션](https://developer.apple.com/account/resources/profiles/list)으로 이동하여 **+** 단추를 클릭합니다.

2. **개발** 아래에서 **iOS 앱 개발**을 선택하고 **계속**을 클릭합니다.

3. 드롭다운 메뉴에서 사용할 앱 ID를 선택하고 **계속**을 클릭합니다.

4. 프로비저닝 프로필에 포함할 인증서를 선택하고 **계속**을 클릭합니다.

5. 앱을 설치할 모든 디바이스를 선택하고 **계속**을 클릭합니다.

6. **프로비저닝 프로필 이름**을 입력하고 **생성**을 클릭합니다.

7. 필요한 경우 다음 페이지에서 **다운로드**를 클릭하여 Mac에 프로비저닝 프로필을 다운로드할 수 있습니다.

<a name="download"></a>

## <a name="download-provisioning-profiles-in-visual-studio"></a>Visual Studio에서 프로비저닝 프로필 다운로드

Apple 개발자 포털에서 새 프로비저닝 프로필을 만든 후에는 앱에서 번들 서명에 사용할 수 있도록 Visual Studio를 사용하여 다운로드합니다.

1. Visual Studio에서 Apple 개발자 계정 창으로 이동합니다.
    1. Mac: **Visual Studio > 기본 설정 > Apple 개발자 계정**
    2. Windows: **도구 > 옵션 > Xamarin > Apple 계정**

2. 팀을 선택하고 **세부 정보 보기...** 를 클릭합니다.
3. 새 프로필이 **프로비저닝 프로필** 목록에 표시되는지 확인합니다. 목록을 새로 고치려면 Visual Studio를 다시 시작해야 할 수 있습니다. 
4. **모든 프로필 다운로드**를 클릭합니다.

Visual Studio에서 새 프로비저닝 프로필을 사용할 준비가 됩니다.

## <a name="deploy-to-a-device"></a>디바이스에 배포

이 시점에서 프로비전은 완료되고, 디바이스에 앱을 배포할 준비가 되어 있어야 합니다. 이를 수행하려면 다음 단계를 따르십시오.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

1. 디바이스를 Mac에 연결합니다.
2. **Info.plist**를 열고 **번들 식별자**가 앞에서 만든 앱 ID와 일치하는지 확인합니다(앱 ID가 와일드카드인 경우 제외).
3. **서명** 섹션에서 **체계**로 **수동 프로비저닝**을 선택합니다.

    ![수동 프로비저닝이 선택된 Mac용 Visual Studio의 Info.plist 스크린샷](manual-provisioning-images/vsm-info-plist.png)

4. **번들 서명 옵션...** 을 클릭합니다.
5. 빌드 구성이 **디버그|iPhone**으로 설정되어 있는지 확인합니다. **서명 ID** 및  **프로필** 드롭다운 메뉴를 둘 다 열어서 올바른 인증서와 프로비저닝 프로필이 표시되는지 확인합니다. 

   ![열려 있는 프로비저닝 프로필 드롭다운 메뉴에 앱에서 사용 가능한 모든 프로비저닝 프로필이 표시된 iOS 번들 서명 속성 페이지](manual-provisioning-images/vsm-bundle-signing.png)

6. 사용할 특정 ID와 프로필을 선택하거나 **자동**으로 둡니다. **자동**으로 설정할 경우 Mac용 Visual Studio가 **Info.plist**의 **번들 식별자**에 따라 ID와 프로필을 선택합니다. 
7. **확인**을 클릭합니다.
8. **실행**을 클릭하여 앱을 디바이스에 배포합니다.


# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. 디바이스를 Mac 빌드 호스트에 연결합니다.
2. **Info.plist**를 열고 **번들 식별자**가 앞에서 만든 앱 ID와 일치하는지 확인합니다(앱 ID가 와일드카드인 경우 제외).
3. **솔루션 탐색기**에서 iOS 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택한 후 **iOS 번들 서명** 탭으로 이동합니다.
4. 빌드 구성이 **디버그|iPhone**으로 설정되어 있는지 확인합니다. **번들 서명**에서 **체계**로 **수동 프로비저닝**을 선택합니다.

    ![수동 프로비저닝이 선택된 Mac용 Visual Studio의 Info.plist 스크린샷](manual-provisioning-images/vs-bundle-signing.png)

5. **서명 ID** 및 **프로비저닝 프로필** 드롭다운 메뉴를 둘 다 열어서 올바른 인증서와 프로비저닝 프로필이 표시되는지 확인합니다.
6. 사용할 특정 ID와 프로필을 선택하거나 **자동**으로 둡니다. **자동**으로 설정할 경우 Visual Studio가 **Info.plist**의 **번들 식별자**에 따라 ID와 프로필을 선택합니다. 
7. **실행**을 클릭하여 앱을 디바이스에 배포합니다.

-----

## <a name="provisioning-for-application-services"></a>애플리케이션 서비스 프로비전

Apple은 Xamarin.iOS 애플리케이션에 활성화할 수 있는 다양한 애플리케이션 서비스(다른 이름: 기능)를 제공합니다. 이러한 애플리케이션 서비스는 **앱 ID**를 만들 때 iOS 프로비전 포털에서 구성하고 Xamarin.iOS 애플리케이션 프로젝트에 속하는 **Entitlements.plist** 파일에서도 구성해야 합니다. 앱에 애플리케이션 서비스를 추가하는 방법에 대한 자세한 내용은 [기능 소개](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드 및 [자격 사용](~/ios/deploy-test/provisioning/entitlements.md) 가이드를 참조하세요.

- 필요한 앱 서비스를 포함하는 앱 ID를 만듭니다.
- 앱 ID를 포함하는 새로운 [프로비전 프로필](#provisioningprofile)을 만듭니다.
- Xamarin.iOS 프로젝트에서 자격 설정

## <a name="related-links"></a>관련 링크

- [무료 프로비전](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [앱 배포](~/ios/deploy-test/app-distribution/index.md)
- [문제 해결](~/ios/deploy-test/troubleshooting.md)
- [Apple - 앱 배포 가이드](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
