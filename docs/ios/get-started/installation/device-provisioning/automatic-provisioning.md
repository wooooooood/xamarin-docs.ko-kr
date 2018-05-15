---
title: 자동 프로비저닝
description: Xamarin.iOS가 성공적으로 설치된 후 iOS 개발의 다음 단계는 iOS 장치를 프로비전하는 것입니다. 이 가이드에서는 자동 서명을 사용하여 개발 인증서와 프로필을 요청하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 0e2ce758da2951efa0508e76cdf4eaac5384fa6b
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="automatic-provisioning"></a>자동 프로비저닝

_Xamarin.iOS가 성공적으로 설치된 후 iOS 개발의 다음 단계는 iOS 장치를 프로비전하는 것입니다. 이 가이드에서는 자동 서명을 사용하여 개발 인증서와 프로필을 요청하는 방법을 설명합니다._

## <a name="requirements"></a>요구 사항

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- Mac용 Visual Studio 7.3 이상
- Xcode 9 이상

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- Visual Studio 2017 버전 15.7 (이상)

또한 다음 항목을 포함하는 Mac 빌드 호스트에 페어링해야 합니다.

- Xcode 9 이상

-----

## <a name="enabling-automatic-signing"></a>자동 서명 사용

자동 서명 프로세스를 시작하기 전에 [Apple 계정 관리](~/cross-platform/macios/apple-account-management.md) 가이드에 설명된 대로 Visual Studio에 추가된 Apple ID가 있는지 확인해야 합니다. Apple ID를 추가했다면 모든 관련 _팀_을 사용할 수 있습니다. 따라서 팀에 대해 인증서, 프로필 및 다른 ID를 만들 수 있습니다. 팀 ID는 프로비저닝 프로필에 포함될 앱 ID에 대한 접두사를 만들 때도 사용됩니다. 이 요소가 있으면 Apple이 신원을 확인할 수 있습니다.

IOS 장치에 배포할 앱에 자동으로 서명하려면 다음을 수행합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac용 Visual Studio에서 iOS 프로젝트를 엽니다.

2. **Info.plist** 파일을 엽니다.

3. **서명** 섹션에서 **자동 프로비저닝**을 선택합니다.

    ![팀 선택기 드롭다운](automatic-provisioning-images/image2.png)

4. **팀** 드롭다운에서 팀을 선택합니다.

6. 몇 초 후 서명 인증서 및 프로비저닝 프로필이 생성됩니다.

    ![성공적으로 생성된 인증서와 프로필](automatic-provisioning-images/image5.png)

    자동 서명이 실패하면 **자동 서명 패드**에 오류의 원인이 표시됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. [Mac에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 가이드에 설명된 대로 Mac에 Visual Studio 2017을 페어링합니다.

2. **프로젝트 > 속성 프로비전...** 을 선택하여 프로비전 옵션을 엽니다.

3. **자동 프로비저닝** 구성표를 선택합니다.

    ![자동 구성표 선택](automatic-provisioning-images/prov4.png)

4. **팀** 콤보 상자에서 팀을 선택하여 자동 서명 프로세스를 시작합니다.

    ![팀 선택](automatic-provisioning-images/prov3.png)

4. 그러면 자동 서명 프로세스를 시작합니다. 그런 다음, Visual Studio는 서명에 이러한 아티팩트를 사용하도록 앱 ID, 프로비전 프로필 및 서명 ID를 생성하려고 합니다. 빌드 출력에서 생성 프로세스를 확인할 수 있습니다.

    ![아티팩트의 생성을 보여주는 빌드 출력](automatic-provisioning-images/prov5.png)

-----

## <a name="triggering-automatic-provisioning"></a>자동 프로비저닝 트리거

자동 서명이 활성화되면 다음과 같은 상황이 발생할 경우 필요에 따라 Mac용 Visual Studio에서 이러한 아티팩트를 업데이트합니다.

* iOS 장치가 mac에 플러그 인됩니다.
    - 장치가 Apple Developer Portal에 등록되었는지 자동으로 검사합니다. 그렇지 않다면 추가하고 이를 포함하는 새 프로비저닝 프로필을 생성합니다.
* 앱의 번들 ID가 변경된 경우
    - 앱 ID를 업데이트합니다. 이 앱 ID를 포함하는 새 프로비저닝 프로필이 생성됩니다.
* 지원되는 기능은 Entitlements.plist 파일에 활성화됩니다.
    - 이 기능은 앱 ID에 추가되고, 업데이트된 앱 ID가 포함된 새 프로비저닝 프로필이 생성됩니다.
    - 일부 기능은 현재 지원되지 않습니다. 지원되는 기능에 대한 자세한 내용은 [기능 사용](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드를 참조하세요.


## <a name="related-links"></a>관련 링크

- [무료 프로비전](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [앱 배포](~/ios/deploy-test/app-distribution/index.md)
- [문제 해결](~/ios/deploy-test/troubleshooting.md)
- [Apple - 앱 배포 가이드](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
