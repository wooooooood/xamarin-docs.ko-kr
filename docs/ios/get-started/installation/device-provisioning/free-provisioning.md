---
title: "무료 프로비전"
description: "Apple의 Xcode 7 릴리스가 발표되면서 모든 iOS 및 Mac 개발자들에게 중요한 변화가 있으니, 그것은 바로 무료 프로비전입니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: A5CE2ECF-8057-49ED-8393-EB0C5977FE4C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 4e93696f8eef44030ffacbdbaa8ebcd860a402f6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="free-provisioning"></a>무료 프로비전

_Apple의 Xcode 7 릴리스가 발표되면서 모든 iOS 및 Mac 개발자들에게 중요한 변화가 있으니, 그것은 바로 무료 프로비전입니다._

개발자는 **Apple 개발자 프로그램**에 참여하지 **않고도** 무료 프로비전을 통해 자신의 iOS 장치에 Xamarin.iOS 응용 프로그램을 배포할 수 있습니다. 이것은 개발자에게 매우 큰 장점입니다. 장치에서 테스트하면 시뮬레이터에서 테스트하는 것보다 메모리, 저장소, 네트워크 연결 등을 포함하여 여러 가지 이점이 있습니다.

Apple 개발자 계정을 사용하지 않는 프로비전은 *서명 ID*(개발자 인증서 및 개인 키 포함)와 *프로비전 프로필*(연결된 iOS 장치의 명시적 앱 ID 및 UDID 포함)을 만드는 Xcode를 통해 수행해야 합니다.

## <a name="requirements"></a>요구 사항

무료 프로비전을 사용하여 장치에 Xamarin.iOS 응용 프로그램을 배포하려면 Xcode 7 이상을 사용해야 합니다.

**사용되는 Apple ID는 어떤 Apple 개발자 프로그램에도 연결되지 않아야 합니다.**

앱에서 사용되는 번들 ID는 고유해야 하며 이전에 다른 앱에서 사용되지 않았어야 합니다. 무료 프로비전에 사용되는 모든 번들 ID는 다시 사용할 수 없습니다. 이미 배포된 앱은 무료 프로비전을 통해 프로비전할 수 없습니다. 

자세한 내용은 [앱 배포 가이드](~/ios/deploy-test/app-distribution/index.md)를 참조하세요.

앱에서 App Services를 사용하는 경우 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md#appservices) 가이드에 설명된 대로 프로비전 프로필을 만들어야 합니다. 제한 사항에 대한 자세한 내용은 아래의 [관련 섹션](#limitations)에서 확인할 수 있습니다.


## <a name="a-namelaunching--launching-your-app"></a><a name="launching" /> 앱 시작

무료 프로비전을 사용하여 장치에 응용 프로그램을 배포하려면 Xcode를 사용하여 서명 ID 및 프로비전 프로필을 만든 후 Mac용 Visual Studio 또는 Visual Studio를 사용하여 앱을 서명할 올바른 프로필을 선택합니다. 이렇게 하려면 아래의 단계별 연습을 수행합니다.

1. Apple ID가 없는 경우 [appleid.apple.com](https://appleid.apple.com/account)에서 만듭니다.
2. Xcode를 열고 **Xcode > 기본 설정**으로 이동합니다.
3. **계정** 아래에서 **+** 단추를 사용하여 기존 Apple ID를 추가합니다. 이래 스크린샷과 비슷한 화면이 보일 것입니다.

  [![](free-provisioning-images/launchapp1.png "Xcode 기본 계정")](free-provisioning-images/launchapp1.png#lightbox)

4. 배포하려는 iOS 장치를 연결하고 Xcode에서 비어 있는 새 단일 보기 iOS 프로젝트를 만듭니다. **팀** 드롭다운을 방금 추가한 Apple ID로 설정합니다. `your name (Personal Team - your Apple ID)`와 비슷한 형식이어야 합니다.

  [![](free-provisioning-images/launchapp2.png "서명 ID 만들기")](free-provisioning-images/launchapp2.png#lightbox)

5. **일반 > ID** 섹션 아래에서 번들 식별자가 Xamarin.iOS 앱의 번들 식별자와 _정확히_ 일치하는지 확인하고 배포 대상이 연결된 iOS 장치보다 낮거나 일치하는지 확인합니다. Xcode는 명시적 앱 ID로만 프로비전 프로필을 만들기 때문에 이 단계가 매우 중요합니다.

  [![](free-provisioning-images/launchapp5.png "명시적 앱 ID로 프로비전 프로필 만들기")](free-provisioning-images/launchapp5.png#lightbox)

6. 서명 섹션에서 **자동으로 서명 관리**를 선택하고 드롭다운 목록에서 팀을 선택합니다.

  [![](free-provisioning-images/launchapp6.png "자동으로 서명 관리를 선택하고 드롭다운 목록에서 팀 선택")](free-provisioning-images/launchapp6.png#lightbox)

7. 이전 단계에서는 자동으로 프로비전 프로필 및 서명 ID가 생성됩니다. 프로비전 프로필 옆에 있는 정보 아이콘을 클릭하면 볼 수 있습니다.

  [![](free-provisioning-images/launchapp7.png "프로비전 프로필 보기")](free-provisioning-images/launchapp7.png#lightbox)

8. Xcode를 테스트하려면 [실행] 단추를 클릭하여 장치에 새 응용 프로그램을 배포합니다.

9. IDE로 돌아가서, 동일한 장치가 연결된 상태에서 Xamarin.iOS 프로젝트 이름을 마우스 오른쪽 단추로 클릭하여 **프로젝트 옵션** 대화 상자를 엽니다. IOS 번들 서명 섹션을 찾아서 서명 ID 및 프로비전 프로필을 명시적으로 설정합니다.

  [![](free-provisioning-images/launchapp8.png "서명 ID 및 프로비전 프로필 설정")](free-provisioning-images/launchapp8.png#lightbox)

IDE에서 서명 ID 또는 올바른 프로비전 프로필을 찾을 수 없는 경우 IDE를 다시 시작해야 할 수도 있습니다.


## <a name="a-namelimitations-limitations"></a><a name="limitations" />제한 사항

Apple은 개발자가 *자신의* 장치에만 배포할 수 있도록 보장하기 위해 무료 프로비전을 사용하여 iOS 장치에서 응용 프로그램을 실행할 수 있는 시기 및 방법에 대한 여러 제한을 두었습니다. 이 단원에는 이러한 제한 사항이 나열되어 있습니다.

ITunes Connect에 대한 액세스 역시 제한되며, 따라서 응용 프로그램을 무료로 프로비전하는 개발자들에게는 앱 스토어 및 TestFlight에 게시와 같은 서비스가 제공되지 않습니다. 임시 및 사내 수단을 통해 배포하려면 Apple 개발자 계정(엔터프라이즈 또는 개인)이 필요합니다.

이 방법으로 만든 프로비전 프로필은 일주일 후, 서명 ID는 1년 후 만료됩니다. 또한 프로비전 프로필은 명시적 앱 ID를 통해서만 만들어지므로 설치하려는 모든 앱과 관련하여 [위의](#launching) 지침을 따라야 합니다.

대부분의 응용 프로그램 서비스에 대한 프로비전 역시 무료 프로비전으로는 불가능합니다. 여기에는 다음이 포함됩니다.

- Apple Pay
- Game Center
- iCloud
- 앱에서 바로 구매
- 푸시 알림
- Wallet(이전에는 Passbook)

전체 목록은 Apple에서 제공하는 [지원되는 기능](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/SupportedCapabilities/SupportedCapabilities.html#//apple_ref/doc/uid/TP40012582-CH38-SW1) 가이드에서 확인할 수 있습니다. 응용 프로그램 서비스에 사용할 앱을 프로비전하려면 [기능 작업](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드를 참조하세요.


## <a name="summary"></a>요약

이 가이드에서는 무료 프로비전을 사용하여 iOS 장치에 응용 프로그램을 설치하는 방법의 장점과 제한 사항에 대해 알아보았습니다. 무료 프로비전을 사용하여 Xamarin.iOS 앱을 설치하는 방법도 단계별로 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [장치 프로비전](~/ios/get-started/installation/device-provisioning/index.md)
- [응용 프로그램 서비스 프로비전](~/ios/get-started/installation/device-provisioning/index.md#appservices)
