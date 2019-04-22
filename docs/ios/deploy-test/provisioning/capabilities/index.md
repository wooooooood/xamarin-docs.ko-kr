---
title: Xamarin.iOS에서 기능 사용
description: 애플리케이션에 기능을 추가하려면 흔히 추가 프로비전 설정이 필요합니다. 이 가이드는 모든 기능에 필요한 설정을 설명합니다.
ms.prod: xamarin
ms.assetid: 98A4676F-992B-4593-8D38-6EEB2EB0801C
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 7049cc36f5f661152e027beb53180d793078beff
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58855031"
---
# <a name="working-with-capabilities-in-xamarinios"></a>Xamarin.iOS에서 기능 사용

_애플리케이션에 기능을 추가하려면 흔히 추가 프로비전 설정이 필요합니다. 이 가이드는 모든 기능에 필요한 설정을 설명합니다._

Apple은 기능을 확장하고 iOS 앱이 수행할 수 있는 범위를 넓히는 수단으로 _앱 서비스_라는 _기능_을 개발자에게 제공합니다. 이런 기능을 통해 개발자는 앱에서 시작된 금전 거래 기능, Siri와 같은 추가 장치 서비스 등과 같은 플랫폼 기능을 애플리케이션에 더 깊이 통합할 수 있습니다.
이러한 기능은 Xamarin.iOS 프로젝트에 사용할 수 있습니다. 전체 서비스 목록은 아래에 설명되어 있습니다.

* 앱 그룹
* 연결된 도메인
* 데이터 보호
* Game Center
* HealthKit
* HomeKit
* 무선 액세서리 구성
* iCloud
* 앱에서 바로 구매
* 내부 앱 오디오
* Apple Pay
* Wallet
* 푸시 알림
* 개인 VPN
* Siri
* 맵
* 백그라운드 모드
* 키 집합 공유
* 네트워크 확장
* 핫스폿 구성
* 다중 경로
* NFC 태그 읽기

기능은 Mac용 Visual Studio 및 Visual Studio 2019를 통해 사용하도록 설정하거나 Apple Developer Portal에서 수동으로 사용하도록 설정할 수 있습니다. Wallet, Apple Pay 및 iCloud와 같은 특정 기능을 사용하려면 앱 ID를 추가로 구성해야 합니다.

이 가이드에서는 Visual Studio의 애플리케이션에서 각 App Services를 수동으로 사용하도록 설정하는 방법과 개발자 센터를 통해 수동으로 설정하는 방법 및 필요한 추가 설정을 설명합니다. 

## <a name="adding-app-services"></a>App Services 추가

기능을 사용하려면 앱에 올바른 서비스가 활성화되어 있고 앱 ID가 포함된 유효한 프로비전 프로필이 있어야 합니다. 프로비전 프로필은 Mac용 Visual Studio 및 Visual Studio 2019에서 자동으로 만들거나 Apple Developer Center에서 수동으로 만들 수 있습니다.

이 섹션에서는 Visual Studio의 자동 프로비전 또는 개발자 센터를 사용하여 대부분의 기능을 사용하도록 설정하는 방법에 대해 설명합니다. Wallet, iCloud, Apple Pay 및 앱 그룹과 같은 일부 기능은 추가 설정이 필요합니다. 이 내용은 인접한 가이드에 자세히 설명되어 있습니다.

> [!IMPORTANT]
> 일부 기능은 자동 프로비저닝에서 추가하고 관리할 수 없습니다. 다음 목록에는 지원되는 기능이 포함되어 있습니다.
>
>* HealthKit 
>* HomeKit 
>* 개인 VPN 
>* 무선 액세서리 구성 
>* 내부 앱 오디오 
>* SiriKit 
>* 핫스폿 
>* 네트워크 확장 
>* NFC 태그 읽기
>* 다중 경로 
>
>푸시 알림, Game Center, 앱에서 바로 구매, 맵, 키 집합 공유, 연결된 도메인 및 데이터 보호 기능은 현재 지원되지 않습니다. 이러한 기능을 추가하려면 수동 프로비전을 사용하고 [개발자 센터](#devcenter) 섹션의 단계를 수행합니다.

## <a name="using-the-ide"></a>IDE 사용

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

기능은 Mac용 Visual Studio에서 **Entitlements.plist**에 추가됩니다. 기능을 추가하려면 다음 단계를 사용합니다.

1. iOS 애플리케이션의 **Info.plist** 파일을 열고, 콤보 상자에서 **자동 프로비저닝** 구성표 및 **팀**을 선택합니다. 도움이 필요하면 [자동 프로비저닝](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) 가이드의 단계를 수행합니다.

    ![자동으로 서명 관리 옵션](images/manage-signing.png)

2. **Entitlements.plist** 파일을 열고 추가할 기능을 선택합니다.

    ![entitlements.plist 파일에 기능 추가](images/image17.png)

    기능을 선택하면 두 가지 작업이 수행됩니다.
    * 해당 기능이 앱 ID에 추가됩니다.
    * 자격 키/값 쌍이 Entitlements.plist 파일에 추가됩니다.

    이러한 작업이 수행되면 Mac용 Visual Studio에 다음과 같이 성공 메시지가 표시됩니다.

    ![entitlements.plist 파일에 기능 추가](images/image18.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

기능은 **Entitlements.plist**에 추가됩니다. Visual Studio 2019에서 기능을 추가하려면 다음 단계를 사용합니다.

1. [Mac에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 가이드에 설명된 대로 Mac에 Visual Studio 2019를 페어링합니다.

2. **프로젝트 > 속성 프로비전...** 을 선택하여 프로비전 옵션을 엽니다.

3. 콤보 상자에서 **자동으로 프로비전** 구성표 및 **팀**을 선택합니다. 도움이 필요하면 [자동 프로비저닝](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) 가이드의 단계를 수행합니다.

    ![자동으로 서명 관리 옵션](images/manage-signing-vs.png)

4. **Entitlements.plist** 파일을 열고 추가할 기능을 선택합니다. 파일을 저장합니다.

    **Entitlement.plist**를 저장하면 다음과 같은 두 작업을 수행합니다.

    * 해당 기능이 앱 ID에 추가됩니다.
    * 자격 키/값 쌍이 Entitlements.plist 파일에 추가됩니다.

-----


<a name="devcenter" />

## <a name="using-the-developer-center"></a>개발자 센터 사용

개발자 센터를 사용하려면 앱 ID를 만든 다음, 해당 앱 ID를 사용하여 프로비전 프로필을 작성하는 두 단계의 프로세스가 필요합니다. 이러한 단계는 아래에서 자세히 설명합니다.

### <a name="creating-an-app-id-with-an-app-service"></a>앱 서비스로 앱 ID 생성하기

1.  Mac에서 [Apple Developer Center](https://developer.apple.com/account)(Windows 컴퓨터를 사용하는 경우 빌드 호스트 mac)로 이동하여 로그인합니다.
2.  **인증서, 식별자 및 프로필**을 선택합니다.

    ![Apple Developer Center](images/image5.png)

3.  **식별자** 아래에서 **앱 ID**를 선택합니다.

    ![개발자 센터에서 앱 ID 선택](images/image6.png)

4.  오른쪽 위 모서리에 있는 **+** 단추를 눌러서 새 앱 ID를 만듭니다.
5.  App ID 설명을 입력하고 Explicit App ID(명시적 앱 ID)를 선택하고 번들 ID를 `com.domain.appname` 형식으로 입력합니다. 이 번들 ID는 프로젝트의 번들 ID와 일치해야 합니다.

    ![앱 ID 추가 세부 정보](images/image7.png)

6.  **App Services** 아래에서 앱에 필요한 서비스를 선택합니다.

    ![App Services 선택 페이지](images/image8.png)

7.  **계속**을 누릅니다.
8.  앱 ID를 확인합니다. 각 서비스는 다음 상태 중 하나에 있습니다. 아래 그림과 같이 **사용**, **사용 안 함** 또는 **구성 가능**. **사용**이면 프로비전 프로필에 사용할 준비가 된 상태입니다. **구성 가능**이면 해당 기능에 대해 추가 설정이 필요합니다. 추가 단계는 뒷부분에 나오는 섹션에 자세히 설명되어 있습니다.

    ![앱 ID 확인](images/image9.png)

9.  **등록** 및 **완료**를 차례로 클릭합니다. 새로 생성된 앱 ID가 iOS 앱 ID 목록에 표시됩니다.


<a name="provisioningprofile" />

### <a name="creating-a-provisioning-profile"></a>프로비전 프로필 만들기

이제 이 앱 ID가 포함된 프로비전 프로필을 만듭니다. 아래의 단계를 수행합니다.

1.  Apple Developer Center에서 **프로비전 프로필 > 모두**로 이동합니다.

    ![프로비전 프로필 섹션](images/image10.png)

2.  오른쪽 위 모서리에 있는 **+** 단추를 눌러서 새 프로비전 프로필을 만듭니다.
3.  필요한 프로비전 프로필 유형을 선택하고 **계속**을 클릭합니다.

    ![프로비전 프로필 선택](images/image11.png)

4.  드롭다운 목록에서 위 단계에서 생성된 앱 ID를 선택하고 **계속**을 누릅니다.

    ![앱 ID 선택](images/image12.png)

5.  앱에 서명하는 데 사용되는 인증서를 선택하고 **계속**을 누릅니다.

    ![인증서 선택](images/image13.png)

6.  이 프로필에 포함할 디바이스를 선택하고 **계속**을 누릅니다.

    ![프로비전 프로필에 대한 디바이스 선택](images/image14.png)

7.  프로필을 식별할 수 있도록 이름을 지정하고 **계속**을 눌러서 프로필을 생성합니다.

    ![프로비전 프로필 이름 지정](images/image15.png)

8.  **다운로드** 단추를 눌러서 다운로드하고 파인더에서 파일을 두 번 클릭하여 프로비전 프로필을 설치합니다.

9. Visual Studio를 사용하는 경우 **수동 프로비전** 옵션을 선택해야 합니다.

10. Mac용 Visual Studio/Visual Studio에서 **프로젝트 옵션 > 번들 서명**으로 이동하여 프로비전 프로필을 방금 생성한 항목으로 설정합니다.

    ![Mac용 Visual Studio 프로젝트 옵션](images/image16.png)

> [!IMPORTANT]
> Entitlement.plist 파일에 자격 키를 설정하고 Info.plist 파일에 개인 키를 설정해야 할 수도 있습니다. 자격에 대한 자세한 내용은 [자격 사용](~/ios/deploy-test/provisioning/entitlements.md) 가이드를 참조하세요.

<a name="nextsteps" />

## <a name="next-steps"></a>다음 단계

서버 쪽에서 기능을 사용하도록 설정한 후에도 앱에서 해당 기능을 사용하려면 추가로 작업을 수행해야 합니다. 아래 목록에는 필요할 수도 있는 추가 단계가 설명되어 있습니다.

*   앱에서 프레임워크 네임스페이스를 사용합니다.
*   앱에 필요한 자격을 추가합니다. 필요한 자격에 및 추가 방법에 대한 자세한 내용은 [자격 소개](~/ios/deploy-test/provisioning/entitlements.md) 가이드를 참조하세요.

<a name="troubleshooting" />

## <a name="troubleshooting-capabilities"></a>기능 문제 해결

아래 목록에는 앱 서비스를 사용하도록 설정한 상태에서 앱을 개발할 때 장애물을 만들 수 있는 가장 일반적인 문제 중 일부가 자세히 나열되어 있습니다.

-   Apple Developer 포털의  **Certificates, IDs & Profiles**(인증서, ID 및 프로필) 섹션에 올바른 ID가 제대로 생성되고 등록되어 있는지 확인합니다.
-   서비스가 앱(또는 확장) ID에 추가되어 있고 Apple Developer 포털의  **Certificates, IDs & Profiles** (인증서, ID 및 프로필)에서 생성된 앱 그룹/가맹점 ID/컨테이너를 사용하도록 구성되어 있는지 확인합니다.
-   프로비전 프로필과 App ID가 설치되어 있는지, 앱의  **Info.plist** (Xamarin 프로젝트)가 위에서 구성한 앱 ID 중 하나를 사용 중인지 확인합니다.
-   앱의  **Entitlements.plist**  파일(Xamarin 프로젝트)에 올바른 서비스를 사용하도록 설정되어 있는지 확인합니다.
-   info.plist에 적절한 개인 키가 설정되어 있는지 확인합니다.
-   앱의  **iOS 번들 서명**에서  **사용자 지정 자격**이 **Entitlements.plist**로 설정되어 있는지 확인합니다. 이 설정은 디버그 및 iOS 시뮬레이터 빌드에 대한 기본 설정이  _아닙니다_ .

<a name="summary" />

## <a name="summary"></a>요약

이 가이드에서는 기능 또는 _앱 서비스_를 설명하고 이 기능을 Visual Studio와 Apple Developer Center에서 사용하도록 설정하는 방법을 설명했습니다. 또한 Wallet, iCloud, Apple Pay 및 앱 그룹과 같은 복잡한 서비스를 설정하는 방법을 자세히 설명했습니다. 마지막으로 설정 및 간단한 문제 해결 옵션에 대한 다음 단계를 설명했습니다.