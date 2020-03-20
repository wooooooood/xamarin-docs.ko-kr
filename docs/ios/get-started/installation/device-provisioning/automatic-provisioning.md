---
title: Xamarin.iOS에 대한 자동 프로비저닝
description: Xamarin.iOS가 성공적으로 설치된 후 iOS 개발의 다음 단계는 iOS 디바이스를 프로비전하는 것입니다. 이 가이드에서는 자동 서명을 사용하여 개발 인증서와 프로필을 요청하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.custom: video
ms.date: 03/05/2020
ms.openlocfilehash: 069c40b74876bea1d3a0c8fca23b3d90c4b91635
ms.sourcegitcommit: 997f7b6a1a1bc50b98c3ca5bbc75d6875ba2ae9a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79510681"
---
# <a name="automatic-provisioning-for-xamarinios"></a>Xamarin.iOS에 대한 자동 프로비저닝

_Xamarin.iOS가 성공적으로 설치된 후 iOS 개발의 다음 단계는 iOS 디바이스를 프로비전하는 것입니다. 이 가이드에서는 자동 프로비저닝을 사용하여 개발 인증서와 프로필을 요청하는 방법을 설명합니다._

## <a name="requirements"></a>요구 사항

자동 프로비저닝은 Mac용 Visual Studio, Visual Studio 2019 및 Visual Studio 2017(버전 15.7 이상)에서 사용할 수 있습니다. 

> [!NOTE]
> 이 기능을 사용하려면 유료 Apple 개발자 계정도 필요합니다. Apple 개발자 계정에 대한 자세한 내용은 [디바이스 프로비저닝](~/ios/get-started/installation/device-provisioning/index.md) 가이드를 참조하세요.
> 유료 Apple 개발자 계정이 없는 경우 [Xamarin.iOS에 대한 무료 프로비저닝](~/ios/get-started/installation/device-provisioning/free-provisioning.md) 가이드를 참조하세요.

> [!NOTE]
> 시작하기에 앞서, [Apple 개발자 포털](https://developer.apple.com/account/) 또는 [App Store Connect](https://appstoreconnect.apple.com/)에서 모든 라이선스 계약에 동의하시기 바랍니다.


## <a name="enable-automatic-provisioning"></a>자동 프로비저닝 사용

자동 서명 프로세스를 시작하기 전에 [Apple 계정 관리](~/cross-platform/macios/apple-account-management.md) 가이드에 설명된 대로 Visual Studio에 Apple ID를 추가했는지 확인해야 합니다. 

Apple ID를 추가했다면 모든 관련 _팀_을 사용할 수 있습니다. 따라서 팀에 대해 인증서, 프로필 및 다른 ID를 만들 수 있습니다. 팀 ID는 프로비저닝 프로필에 포함될 앱 ID의 접두사를 만들 때도 사용됩니다. 이 요소가 있으면 Apple이 신원을 확인할 수 있습니다.

IOS 디바이스에 배포할 앱에 자동으로 서명하려면 다음을 수행합니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

1. Mac용 Visual Studio에서 iOS 프로젝트를 엽니다.

2. **Info.plist** 파일을 엽니다.

3. **서명** 섹션에서 **자동 프로비저닝**을 선택합니다.

    ![팀 선택기 드롭다운](automatic-provisioning-images/image2.png)

4. **팀** 드롭다운에서 팀을 선택합니다.

5. 몇 초 후 서명 인증서 및 프로비저닝 프로필이 생성됩니다.

    ![성공적으로 생성된 인증서와 프로필](automatic-provisioning-images/image5.png)

    자동 서명이 실패하면 **자동 서명 패드**에 오류의 원인이 표시됩니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

> [!NOTE]
> Visual Studio 2017 또는 Visual Studio 2019(버전 16.4 이상)를 사용하는 경우 계속하기 전에 [Mac 빌드 호스트에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md)되어 있어야 합니다.

1. **솔루션 탐색기**에서 iOS 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. 그런 다음 **iOS 번들 서명** 탭으로 이동합니다.

    ![iOS 속성의 번들 서명 페이지 스크린샷.](automatic-provisioning-images/bundle-signing-win.png)

2. **자동 프로비저닝** 구성표를 선택합니다.

3. **팀** 드롭다운 메뉴에서 팀을 선택하여 자동 서명 프로세스를 시작합니다. Visual Studio에 프로세스가 성공적으로 완료되었는지 표시됩니다.

    ![“자동 프로비저닝을 완료했습니다” 메시지가 강조 표시된 번들 서명 페이지 스크린샷.](automatic-provisioning-images/signing-success-win.png)

-----

## <a name="run-automatic-provisioning"></a>자동 프로비저닝 실행

자동 프로비저닝이 사용하도록 설정되면 다음과 같은 상황이 발생할 경우 필요에 따라 Visual Studio가 이 프로세스를 다시 실행합니다.

- iOS 디바이스가 mac에 플러그 인됩니다.
  - 디바이스가 Apple Developer Portal에 등록되었는지 자동으로 검사합니다. 등록되지 않은 경우 디바이스를 추가하고 이를 포함하는 새 프로비저닝 프로필을 생성합니다.
- 앱의 번들 ID가 변경된 경우
  - 앱 ID를 업데이트합니다. 이 앱 ID를 포함하는 새 프로비저닝 프로필이 생성됩니다.
- 지원되는 기능은 Entitlements.plist 파일에 활성화됩니다.
  - 이 기능은 앱 ID에 추가되고, 업데이트된 앱 ID가 포함된 새 프로비저닝 프로필이 생성됩니다.
  - 일부 기능은 현재 지원되지 않습니다. 지원되는 기능에 대한 자세한 내용은 [기능 사용](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드를 참조하세요.

## <a name="wildcard-app-ids"></a>와일드카드 앱 ID

Mac용 Visual Studio 및 Visual Studio 2019(버전 16.5 이상)에서, 자동 프로비저닝은 기본적으로 명시적 앱 ID 대신 **Info.plist**에 지정된 **번들 식별자**를 기반으로 와일드카드 앱 ID 및 프로비저닝 프로필을 만들고 사용하려고 시도합니다. 와일드카드 앱 ID는 프로필 및 Apple Developer Portal에서 유지 관리하는 ID 수를 줄입니다.

경우에 따라 앱의 자격에는 명시적인 앱 ID가 필요합니다. 다음 자격은 와일드카드 앱 ID를 지원하지 않습니다.

- 앱 그룹
- 연결된 도메인
- Apple Pay
- Game Center
- HealthKit
- HomeKit
- 핫스폿
- 앱에서 바로 구매
- 다중 경로
- NFC
- 개인 VPN
- 푸시 알림
- 무선 액세서리 구성

앱에서 해당 자격 중 하나를 사용하는 경우 Visual Studio는 와일드카드 앱 ID 대신 명시적인 앱 ID를 만들려고 시도합니다.

## <a name="troubleshoot"></a>문제 해결 

- 새 Apple 개발자 계정이 승인되기까지 몇 시간 정도 걸릴 수 있습니다. 계정이 승인되기 전까지는 자동 프로비저닝을 사용할 수 없습니다.
- 자동 프로비저닝 프로세스가 `Authentication Service Is Unavailable` 오류 메시지와 함께 실패할 경우, [App Store Connect](https://appstoreconnect.apple.com/) 또는 [appleid.apple.com](https://appleid.apple.com)에 로그인하여 최신 서비스 계약에 동의했는지 확인하세요.
- `Authentication Error: Xcode 7.3 or later is required to continue developing with your Apple ID.` 오류 메시지가 표시된다면 선택한 팀에 Apple Developer Program에 대한 유효한 유료 멤버 자격이 있는지 확인하세요. 유료 Apple 개발자 계정을 사용하려면 [Xamarin.iOS 앱에 대한 체험 프로비저닝](~/ios/get-started/installation/device-provisioning/free-provisioning.md) 가이드를 참조하세요.

## <a name="related-links"></a>관련 링크

- [무료 프로비전](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [앱 배포](~/ios/deploy-test/app-distribution/index.md)
- [문제 해결](~/ios/deploy-test/troubleshooting.md)
- [Apple - 앱 배포 가이드](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
