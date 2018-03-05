---
title: "Wallet 기능"
description: "응용 프로그램에 기능을 추가하려면 흔히 추가 프로비전 설정이 필요합니다. 이 가이드에서는 Wallet 기능에 필요한 설정을 설명합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: BD9475E6-F586-488C-93D4-8A2A1629B99B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 952fa7dfa93c1dcb45eafe4eec08c73d2a8571eb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="wallet-capabilities"></a>Wallet 기능

_응용 프로그램에 기능을 추가하려면 흔히 추가 프로비전 설정이 필요합니다. 이 가이드에서는 Wallet 기능에 필요한 설정을 설명합니다._

Wallet은 사용자가 티켓, 탑승권 및 쿠폰을 장치에서 바로 표시할 수 있는 바코드 및 기타 콘텐츠를 저장하고 표시하는 앱입니다. 이 정보는 _패스_에 저장됩니다. 예를 들어, 탑승권이나 단일 티켓은 단일 패스입니다. 

개발자는 다양한 방법으로 Wallet을 사용할 수 있습니다.

*   패스를 만들기 위해 응용 프로그램을 빌드할 필요가 없습니다. Passfile은 JSON 파일과 선택적 메타데이터 파일을 포함하는 압축된 아카이브입니다. 이것을 준비하려면 [Pass Type ID](~/ios/platform/passkit.md)(패스 유형 ID) 및 [Pass certificate](~/ios/platform/passkit.md)(패스 인증서)가 필요합니다. 이 정보는 JSON 파일에서 선언됩니다. Passfile 프로비전에 대한 자세한 내용은 [PassKit 소개](~/ios/platform/passkit.md) 가이드를 참조하세요.

*   패스를 배포하도록 도우미 앱을 작성합니다. 이러한 앱에는 패스를 생성, 편집 및 업데이트한 다음, Wallet 앱에 추가하는 기능이 포함됩니다. 이런 종류의 앱의 좋은 예는 영화 앱입니다. 사용자가 앱을 통해 티켓을 구입하면 티켓을 앱에서 Wallet으로 직접 추가할 수 있습니다. 도우미 앱을 사용하려면 프로비전 프로필에 Wallet 기능이 있는 앱 ID가 포함되어야 하며, 아래 단계에 따라 설정할 수 있습니다. 또한 필요한 자격이 앱에 포함되어야 합니다.

*   통로 앱은 패스를 직접 조작하지 않는 앱입니다. 패스를 받아서 Wallet에 추가하는 옵션을 제공하는 것 이상의 최소 상호 작용이 포함됩니다. 이러한 앱에는 특별한 프로비전 또는 자격이 필요하지 않지만 PassKit Framework의 메서드가 사용됩니다.

## <a name="developer-center"></a>개발자 센터

Wallet에 사용할 새 프로비전 프로필을 만들려면 다음을 수행합니다.

1.  Apple Developer 포털의 [Certificates, Identifiers, and Profiles](https://developer.apple.com/account/ios/certificate/)(인증서, 식별자 및 프로필) 섹션으로 이동합니다.
2.  **식별자** 아래에서 **앱 ID**로 이동합니다. 
    
    ![앱 ID 선택](wallet-capabilities-images/image17.png)

3.  페이지 오른쪽 위에서 **+** 아이콘을 클릭합니다.
4.  새로운 앱 ID에 **이름**과 번들 식별자를 지정하여 등록합니다. (이 번들 식별자는 프로젝트의 번들 ID와 일치해야 합니다.)
   
    ![앱 ID 추가 세부 정보](wallet-capabilities-images/image18.png)

5.  서비스 목록에서 **Wallet** 앱 서비스를 선택합니다.
    
    ![서비스 선택 화면](wallet-capabilities-images/image19.png)

6.  **계속**을 누른 다음, **등록**을 눌러서 앱 ID를 만듭니다.

필요한 경우 기존 앱 ID를 편집하여 Wallet 기능을 추가할 수 있습니다.

[기능 사용](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드의 설명에 따라, 이 앱 ID를 사용하여 새 프로비전 프로필을 생성하거나 다시 생성할 수 있습니다.

![새로 만든 앱 ID를 사용하여 프로비전 프로필 만들기](wallet-capabilities-images/image20.png)


Wallet 사용에 대한 자세한 내용은 다음 가이드를 참조하세요.

*   [PassKit 소개](~/ios/platform/passkit.md)
 
## <a name="next-steps"></a>다음 단계
 
아래 목록에는 필요할 수도 있는 추가 단계가 설명되어 있습니다.

* 앱에서 프레임워크 네임스페이스를 사용합니다.
* 앱에 필요한 자격을 추가합니다. 필요한 자격 및 추가 방법에 대한 자세한 내용은 [자격 사용](~/ios/deploy-test/provisioning/entitlements.md) 가이드를 참조하세요.
* 앱의 **iOS 번들 서명**에서 **사용자 지정 자격**이 **Entitlements.plist**로 설정되어 있는지 확인합니다. 이 설정은 디버그 및 iOS 시뮬레이터 빌드에 대한 기본 설정이 _아닙니다_.

앱 서비스에 문제가 발생하면 주 가이드의 [문제 해결](~/ios/deploy-test/provisioning/capabilities/index.md) 섹션을 참조하세요.