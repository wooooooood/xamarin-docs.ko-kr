---
title: Xamarin.iOS 앱에 대한 무료 프로비전
description: 이 문서에서는 Xamarin.iOS 개발자가 Apple의 유료 개발자 프로그램에 등록하지 않고도 물리적 장치에서 앱을 테스트할 수 있는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: A5CE2ECF-8057-49ED-8393-EB0C5977FE4C
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 07/16/2018
ms.openlocfilehash: 503dae8253b3c0bb82038dd54b5d97ff632b439b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115044"
---
# <a name="free-provisioning-for-xamarinios-apps"></a>Xamarin.iOS 앱에 대한 무료 프로비전

무료 프로비전을 사용하면 Xamarin.iOS 개발자 **Apple 개발자 프로그램**에 참여하지 **않고도** iOS 장치에서 해당 앱을 배포하고 테스트할 수 있습니다.
시뮬레이터 테스트이 중요하고 편리한 반면 실제 메모리, 저장소 및 네트워크 연결 제약 조건 하에서 제대로 작동하는지 확인하기 위해 실제 iOS 장치에서 앱을 테스트하는 것도 중요합니다.

무료 프로비전을 사용하여 장치에 앱을 배포하려면:

- Xcode를 사용하여 필요한 *서명 ID*(개발자 인증서 및 개인 키) 및 *프로비전 프로필*(명시적 앱 ID 및 연결된 iOS 장치의 UDID 포함)을 만듭니다.
- Mac용 Visual Studio 및 Visual Studio 2017에서 Xcode로 만든 서명 ID 및 프로비전 프로필을 사용하여 Xamarin.iOS 응용 프로그램을 배포합니다.

> [!IMPORTANT]
> [자동 프로비전](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md)을 사용하면 Mac용 Visual Studio 또는 Visual Studio 2017에서 개발자 테스트를 위해 장치를 자동으로 설정할 수 있습니다. 그러나 자동 프로비전은 무료 프로비전과 호환되지 않습니다. 자동 프로비전을 사용하기 위해 유료 Apple Developer Program 계정이 있어야 합니다.

## <a name="requirements"></a>요구 사항

무료 프로비전을 사용하여 장치에 Xamarin.iOS 응용 프로그램을 배포하려면:

- 사용되는 Apple ID는 어떤 Apple 개발자 프로그램에도 연결되지 않아야 합니다.
- Xamarin.iOS 앱은 와일드 카드 앱 ID가 아닌 명시적 앱 ID를 사용해야 합니다.
- Xamarin.iOS 앱에서 사용되는 번들 식별자는 고유해야 하며 이전에 다른 앱에서 사용되지 않았어야 합니다. 무료 프로비전에 사용된 번들 식별자는 다시 사용**할 수 없습니다**.
- 이미 앱을 배포한 경우 무료 프로비전을 사용하여 해당 앱을 배포할 수 없습니다.
- 앱에서 App Services를 사용하는 경우 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md#provisioning-for-application-services) 가이드에 설명된 대로 프로비전 프로필을 만들어야 합니다. 

무료 프로비전과 관련된 제한 사항에 대한 자세한 내용은 이 문서의 [제한 사항](#limitations) 섹션을 살펴보고, iOS 응용 프로그램을 배포하는 방법에 대한 자세한 내용은 [앱 배포 가이드](~/ios/deploy-test/app-distribution/index.md)를 참조하세요.

## <a name="testing-on-device-with-free-provisioning"></a>무료 프로비전을 사용하여 장치 테스트

무료 프로비전을 사용하여 Xamarin.iOS 앱을 테스트하려면 아래 단계를 따릅니다.

### <a name="use-xcode-to-create-a-signing-identity-and-provisioning-profile"></a>Xcode를 사용하여 서명 ID 및 프로비전 프로필 만들기

1. Apple ID가 없는 경우 [만듭니다](https://appleid.apple.com).
2. Xcode를 열고 **Xcode > 기본 설정**으로 이동합니다.
3. **계정** 아래에서 **+** 단추를 사용하여 기존 Apple ID를 추가합니다. 이래 스크린샷과 비슷한 화면이 보일 것입니다.

    ![Xcode 기본 설정 – 계정](free-provisioning-images/launchapp1.png "Xcode 기본 설정 – 계정")

4. Xcode 기본 설정을 닫습니다.
5. 앱을 배포하려는 iOS 장치에 플러그 인합니다.
6. Xcode에서 새 프로젝트를 만듭니다. **파일 > 새로 만들기 > 프로젝트**를 선택하고, **단일 보기 앱**을 선택합니다.
7. 새 프로젝트 대화 상자에서 **팀**을 방금 추가한 Apple ID로 설정합니다. 드롭다운 목록에서 **사용자 이름(개인 팀)** 과 유사해야 합니다.

    ![새 앱 만들기](free-provisioning-images/launchapp2.png "새 앱 만들기")

8. 새 프로젝트를 만들면 시뮬레이터가 아닌 iOS 장치를 대상으로 지정하는 Xcode 빌드 체계를 선택합니다.

    ![Xcode 빌드 체계 선택](free-provisioning-images/xcodescheme.png "Xcode 빌드 체계 선택")

9. Xcode의 **프로젝트 탐색기**에서 최상위 노드를 선택하여 앱의 프로젝트 설정을 엽니다.
10. **일반 > ID** 아래에서 **번들 식별자**가 Xamarin.iOS 앱의 번들 식별자와 _정확히 일치_하는지 확인합니다.

    ![번들 식별자 설정](free-provisioning-images/launchapp5.png "번들 식별자 설정")

    > [!IMPORTANT]
    > Xcode는 명시적 앱 ID에 대한 프로비전 프로필만을 만듭니다. 해당 프로필은 Xamarin.iOS 앱의 앱 ID와 동일해야 합니다.
    > 다른 경우 무료 프로비전을 사용하여 Xamarin.iOS 앱을 배포할 수 없습니다.

11. **배포 정보** 아래에서 배포 대상이 연결된 iOS 장치에 설치된 iOS 버전 이하인지 확인합니다.
12. **서명** 아래에서 **자동으로 서명 관리**를 선택하고 드롭다운 목록에서 팀을 선택합니다.

    ![자동으로 서명 관리](free-provisioning-images/launchapp6.png "자동으로 서명 관리")

    Xcode는 자동으로 프로비전 프로필 및 서명 ID를 생성합니다. 프로비전 프로필 옆에 있는 정보 아이콘을 클릭하면 볼 수 있습니다.

    ![프로비전 프로필 보기](free-provisioning-images/launchapp7.png "프로비전 프로필 보기")

    > [!TIP]
    > Xcode가 프로비전 프로필을 생성하려고 할 때 오류가 발생하는 경우 Xcode에서 현재 선택한 빌드 체계가 시뮬레이터가 아닌 연결된 iOS 장치를 대상으로 지정하는지 확인합니다.

13. Xcode를 테스트하려면 [실행] 단추를 클릭하여 장치에 새 응용 프로그램을 배포합니다.

### <a name="deploy-your-xamarinios-app"></a>Xamarin.iOS 앱 배포

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. USB를 통하거나 [무선](~/ios/deploy-test/wireless-deployment.md)으로 Mac 빌드 호스트에 iOS 장치를 연결합니다.
2. Mac용 Visual studio **Solution Pad**에서 **Info.plist**를 두 번 클릭합니다.
3. **서명**에서 **수동 프로비전**을 선택합니다.
4. **iOS 번들 서명**을 클릭합니다. 클릭합니다.
5. **구성**에서 **디버그**를 선택합니다.
6. **플랫폼**에서 **iPhone**을 선택합니다.
7. Xcode에서 생성한 **서명 ID**를 선택합니다.
8. Xcode에서 생성한 **프로비전 프로필**을 선택합니다.

    ![서명 ID 및 프로비전 프로필 설정](free-provisioning-images/launchapp8.png "서명 ID 및 프로비전 프로필 설정")

    > [!TIP]
    > 서명 ID 또는 올바른 프로비전 프로필을 찾을 수 없는 경우 Mac용 Visual Studio를 다시 시작해야 할 수도 있습니다.

9. **확인**을 클릭하여 **프로젝트 옵션**을 저장하고 닫습니다.
10. iOS 장치를 선택하고 앱을 실행합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio 2017이 [Mac 빌드 호스트와 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md)되었는지 확인합니다.
2. USB를 통하거나 [무선](~/ios/deploy-test/wireless-deployment.md)으로 Mac 빌드 호스트에 iOS 장치를 연결합니다.
3. Visual Studio 2017의 **솔루션 탐색기**에서 Xamarin.iOS 프로젝트를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.
4. **iOS 번들 서명**으로 이동합니다.
5. **구성**에서 **디버그**를 선택합니다.
6. **플랫폼**에서 **iPhone**을 선택합니다.
7. **수동 프로비저닝**을 선택합니다.
8. Xcode에서 생성한 **서명 ID**를 선택합니다.
9. Xcode에서 생성한 **프로비전 프로필**을 선택합니다.
    
    ![서명 ID 및 프로비전 프로필 설정](free-provisioning-images/setprofile-w157.png "서명 ID 및 프로비전 프로필 설정")

    > [!TIP]
    > Xcode는 이 서명 ID 및 프로비전 프로필을 만들고 Mac 빌드 호스트에 저장했습니다. Mac 빌드 호스트에 [페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md)되었으므로 Visual Studio 2017에 액세스할 수 있습니다. 나열되지 않은 경우 Visual Studio 2017을 다시 시작해야 합니다.

10. 프로젝트 속성을 저장하고 닫습니다.
11. iOS 장치를 선택하고 앱을 실행합니다.

-----

## <a name="limitations"></a>제한 사항

Apple은 개발자가 *자신의* 장치에만 배포할 수 있도록 보장하기 위해 무료 프로비전을 사용하여 iOS 장치에서 응용 프로그램을 실행할 수 있는 시기 및 방법에 대한 여러 제한을 두었습니다.

- iTunes Connect에 대한 액세스가 제한되며, 따라서 응용 프로그램을 무료로 프로비전하는 개발자들에게는 앱 스토어 및 TestFlight에 게시와 같은 서비스가 제공되지 않습니다. 임시 및 사내 수단을 통해 배포하려면 Apple 개발자 계정(엔터프라이즈 또는 개인)이 필요합니다.
- 무료 프로비전을 사용하여 만든 프로비전 프로필은 일주일 후 만료되고, 서명 ID는 1년 후 만료됩니다. 
- Xcode가 명시적 앱 ID에 대한 프로비전 프로필만을 만들므로 설치하려는 모든 앱에서 [위의 지침](#testing-on-device-with-free-provisioning)을 따라야 합니다.
- 대부분의 응용 프로그램 서비스에 대한 프로비전은 무료 프로비전으로는 불가능합니다. 여기에는 Apple Pay, Game Center, iCloud, 앱 내 구매, 푸시 알림 및 전자 지갑이 포함됩니다. Apple에서는 [지원되는 기능(iOS)](https://help.apple.com/developer-account/#/dev21218dfd6) 가이드에 있는 기능의 전체 목록을 제공합니다. 응용 프로그램 서비스에 사용할 앱을 프로비전하려면 [기능 작업](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드를 참조하세요.

## <a name="summary"></a>요약

이 가이드에서는 무료 프로비전을 사용하여 iOS 장치에 응용 프로그램을 설치하는 방법의 장점과 제한 사항에 대해 알아보았습니다. 무료 프로비전을 사용하여 Xamarin.iOS 앱을 설치하는 방법을 설명하는 단계별 연습을 제공했습니다.

## <a name="related-links"></a>관련 링크

- [장치 프로비전](~/ios/get-started/installation/device-provisioning/index.md)
- [응용 프로그램 서비스 프로비전](~/ios/get-started/installation/device-provisioning/index.md#provisioning-for-application-services)
