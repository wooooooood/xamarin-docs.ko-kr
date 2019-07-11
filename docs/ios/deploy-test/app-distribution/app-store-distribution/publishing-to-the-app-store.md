---
title: App Store에 Xamarin.iOS 앱 게시
description: 이 문서에서는 App Store에 배포할 Xamarin.iOS 애플리케이션을 구성, 빌드 및 게시하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/25/2018
ms.openlocfilehash: c81c84b8b32bdde6949918f3a31f171983007f39
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675216"
---
# <a name="publishing-xamarinios-apps-to-the-app-store"></a>App Store에 Xamarin.iOS 앱 게시

앱을 [앱 스토어](https://www.apple.com/ios/app-store/)에 게시하려면, 앱 개발자는 먼저 Apple에 검토를 위해 앱과 함께 스크린샷, 설명, 아이콘 및 기타 정보를 제출해야 합니다. 앱이 승인되면 Apple은 사용자가 구매하고 iOS 디바이스에서 직접 설치할 수 있도록 앱 스토어에 앱을 배치합니다.

이 가이드에서는 앱 스토어를 위해 앱을 준비하고 검토를 위해 Apple에 전송하는 단계를 설명합니다. 특히, 다음과 같은 내용을 설명합니다.

> [!div class="checklist"]
> - 앱 스토어 검토 지침 따르기
> - 앱 ID 및 자격 설정
> - 앱 스토어 아이콘 및 앱 아이콘 제공
> - 앱 스토어 프로비전 프로필 설정
> - **릴리스** 빌드 구성 업데이트
> - iTunes Connect에서 앱 구성
> - 앱을 빌드하고 Apple에 제출

> [!IMPORTANT]
> Apple은 2019년 3월부터 App Store에 제출된 모든 앱과 업데이트가 iOS 12.1 SDK 이상에서 빌드되어 Xcode 10.1 이상에 포함된다고 [발표했습니다](https://developer.apple.com/ios/submit/).
> 앱은 iPhone XS 및 12.9인치 iPad Pro 화면 크기도 지원해야 합니다.

## <a name="app-store-guidelines"></a>앱 스토어 지침

앱 스토어 게시를 위해 앱을 제출하기 전에 Apple [앱 스토어 검토 지침](https://developer.apple.com/appstore/resources/approval/guidelines.html)에 정의된 표준을 충족하는지 확인합니다.
앱 스토어에 앱을 제출하면 Apple은 이러한 요구 사항을 충족하는지 검토합니다. 충족하지 않는 경우 Apple은 해당 앱을 반려하며 귀하는 언급된 문제를 해결하여 다시 제출해야 합니다.
따라서 개발 프로세스에서 최대한 빨리 지침에 익숙해지는 것이 좋습니다.

앱을 제출할 때 주의해야 할 몇 가지 사항은 다음과 같습니다.

1. 앱 설명이 해당 기능과 일치해야 합니다.
2. 정상적인 사용에서 앱이 충돌하지 않는지 테스트합니다. 여기에는 지원하는 모든 iOS 디바이스에서의 사용이 포함됩니다.

또한 Apple에서 제공하는 [앱 스토어 관련 리소스](https://developer.apple.com/app-store/resources/)를 살펴봅니다.

## <a name="set-up-an-app-id-and-entitlements"></a>앱 ID 및 자격 설정

모든 iOS 앱에는 *자격*이라고 하는 연관된 애플리케이션 서비스 집합이 있는 고유한 앱 ID가 있습니다. HealthKit 등과 같은 푸시 알림 수신, iOS 액세스 기능과 같은 다양한 작업을 수행하도록 앱을 허용하는 자격을 부여합니다.

앱 ID를 만들고 필요한 자격을 선택하려면 [Apple Developer 포털](https://developer.apple.com/account/)을 방문하여 다음 단계를 수행합니다:

1. **인증서, 식별자 및 프로필** 섹션에서 **식별자 > 앱 ID**를 차례로 선택합니다.
2. **+** 단추를 클릭하고 새 애플리케이션에 대한 **이름** 및 **번들 ID**를 제공합니다.
3. 화면 아래쪽으로 스크롤하여 Xamarin.iOS 애플리케이션에 필요한 모든 **App Services**를 선택합니다. App Services에 대한 자세한 내용은 [Xamarin.iOS에서 기능을 사용하여 작업하기](~/ios/deploy-test/provisioning/capabilities/index.md)에 나와있습니다.
4. **계속** 단추를 클릭하고 화면의 지시에 따라 새 앱 ID를 만듭니다.

앱 ID를 정의할 때 필요한 애플리케이션 서비스를 선택하고 구성하는 것 외에도, **Info.plist** 및 **Entitlements.plist** 파일을 편집하여 Xamarin.iOS 프로젝트에서 자격 및 앱 ID를 구성해야 합니다. 자세한 내용은 [Xamarin.iOS에서 자격 작업하기](~/ios/deploy-test/provisioning/entitlements.md) 가이드를 살펴봅니다. 해당 가이드는 **Entitlements.plist** 파일 생성 방법 및 다양한 권한 설정의 의미에 대한 설명을 포함합니다.

## <a name="include-an-app-store-icon"></a>앱 스토어 아이콘을 포함합니다.

Apple에 앱을 제출할 때 앱 스토어 아이콘이 들어 있는 자산 카탈로그를 포함해야 합니다. 이 작업을 수행 하는 방법에 알아보려면 [Xamarin.iOS 내의 앱 스토어 아이콘](~/ios/app-fundamentals/images-icons/app-store-icon.md) 가이드를 참조합니다.

## <a name="set-the-apps-icons-and-launch-screens"></a>앱 아이콘 및 시작 화면 설정

Apple이 IOS 앱을 앱 스토어에서 사용할 수 있도록 하려면, 앱이 실행되는 모든 iOS 디바이스에 대해 적절한 아이콘 및 시작 화면이 있어야 합니다. 앱 아이콘 및 시작 화면을 설정하는 방법에 대한 자세한 내용은 다음 가이드를 읽어보세요.

- [Xamarin.iOS 애플리케이션 아이콘](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Xamarin.iOS 앱에 대한 시작 화면](~/ios/app-fundamentals/images-icons/launch-screens.md)

## <a name="create-and-install-an-app-store-provisioning-profile"></a>앱 스토어 프로비전 프로필 생성 및 설치

iOS는 *프로비전 프로필*을 사용하여 특정 애플리케이션 빌드를 배포하는 방법을 제어합니다. 이러한 파일은 앱 서명에 사용된 인증서, 앱 ID 및 앱을 설치할 수 있는 위치에 대한 정보가 포함된 파일입니다. 개발 및 임시 배포의 경우 프로비전 프로필에는 앱이 배포될 수 있도록 허용되는 디바이스 목록도 포함됩니다. 그러나 앱 스토어 배포의 경우 앱 스토어를 통한 공개 배포만이 유일한 메커니즘이므로 인증서 및 앱 ID 정보만 포함됩니다.

앱 스토어 프로비전 프로필을 만들고 설치 하려면 다음 단계를 수행합니다.

1. [Apple 개발자 포털](https://developer.apple.com/account/) 로그인
2. **인증서, ID 및 프로필**에서 **프로비전 프로필 > 배포**를 선택합니다.
3. **+** 단추를 클릭하고 **앱 스토어**를 선택한 후, **계속**을 클릭합니다.
4. 목록에서 앱의 **앱 ID**를 선택하고 **계속**을 클릭합니다.
5. 서명 인증서를 선택하고 **계속**을 클릭합니다.
6. **프로필 이름**을 입력하고 **계속**을 클릭하여 프로필을 생성합니다.
7. Xamarin의 [Apple 계정 관리](~/cross-platform/macios/apple-account-management.md) 도구를 사용하여 Mac에 새로 만든 프로비전 프로필을 다운로드합니다. Mac을 사용하는 경우 Apple Developer 포털에서 직접 프로비전 프로필을 다운로드할 수 있으며 더블 클릭하여 설치할 수 있습니다.

자세한 지침은 [배포 프로필 만들기](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) 및 [Xamarin.iOS 프로젝트에서 배포 프로필 선택하기](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile)를 참조하세요.

## <a name="update-the-release-build-configuration"></a>릴리스 빌드 구성 업데이트

새 Xamarin.iOS 프로젝트는 자동으로 **디버그** 및 **릴리스**_빌드 구성_을 설정합니다. **릴리스** 빌드를 올바르게 구성하려면, 다음 단계를 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**에서 **Info.plist**를 엽니다. **수동 프로비저닝**을 선택합니다. 파일을 저장한 후 닫습니다.
2. **Solution Pad**에서 **프로젝트 이름**을 마우스 오른쪽 단추로 클릭하고 **옵션**을 선택한 후, **iOS 빌드** 탭으로 이동합니다.
3. **구성**을 **릴리스**로 설정하고 **플랫폼**을 **iPhone**으로 설정합니다.
4. 특정 iOS SDK를 빌드하려면 **SDK 버전** 목록에서 선택합니다. 그렇지 않으면 이 값을 **기본**으로 둡니다.
5. 링크하면 사용되지 않는 코드를 제거하여 애플리케이션의 전체 크기를 줄일 수 있습니다. 대부분의 경우 **링커 동작**은 기본값인 **프레임 워크 SDK만 링크**로 설정해야 합니다. 일부 타사 라이브러리를 사용할 때와 같은 일부 상황에서는 필요한 코드가 제거되지 않도록 이 값을 **연결 안 함**으로 설정할 필요가 있습니다. 자세한 내용은 [Xamarin.iOS 앱 연결하기](~/ios/deploy-test/linker.md) 가이드를 참조하세요.
6. **PNG 이미지 최적화**를 확인하여 추가로 애플리케이션의 크기를 줄일 수 있는지 알아봅니다.
7. 디버깅은 빌드를 쓸데없이 크게 만들므로 _사용하지 않도록_ 설정해야 합니다.
8. iOS 11의 경우 **ARM64**를 지원하는 디바이스 아키텍처 중 하나를 선택합니다. 64비트 iOS 디바이스 빌드에 대한 자세한 내용은 [32/64비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md) 설명서의 **Xamarin.iOS 앱의 64비트 빌드 활성화** 섹션을 참조하세요.
9. 더 작고 빠른 코드를 빌드하기 위해 **LLVM** 컴파일러를 사용하고자 할 수 있습니다. 그러나 이 옵션은 컴파일 시간을 증가시킵니다.
10. 애플리케이션의 요구에 따라 사용할 **가비지 수집**의 형식 및 **국제화**에 대한 설정을 조정할 수도 있습니다.

    위에서 설명하는 옵션을 설정한 다음, 빌드 설정은 다음과 유사하게 됩니다.

    ![iOS 빌드 설정](publishing-to-the-app-store-images/build-m157.png "iOS 빌드 설정")

    또한 빌드 설정에 대한 추가 설명이 있는 [iOS 빌드 메커니즘](~/ios/deploy-test/ios-build-mechanics.md) 가이드를 참조합니다.

11. **iOS 번들 서명** 탭으로 이동합니다. 여기서 옵션을 편집할 수 없으면 **Info.plist** 파일에서 **수동 프로비전**이 선택되었는지 확인합니다.
12. **구성**이 **릴리스**로 설정되고 **플랫폼**이 **iPhone**으로 설정되었는지 확인합니다.
13. **서명 ID**를 **배포(자동)** 로 설정합니다.
14. **프로비전 프로필**은 [위에서 만든](#create-and-install-an-app-store-provisioning-profile) 앱 스토어 프로비전 프로필을 선택합니다.

    프로젝트의 번들 서명 옵션은 이제 다음과 유사 하게 표시됩니다.

    ![iOS 번들 서명](publishing-to-the-app-store-images/bundleSigning-m157.png "iOS 번들 서명")

15. **OK**를 클릭하여 변경 내용을 프로젝트 속성에 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio 2019 또는 Visual Studio 2017이 [Mac 빌드 호스트에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md)되었는지 확인합니다.
2. **솔루션 탐색기**에서 **프로젝트 이름**을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.
3. **iOS 빌드** 탭으로 이동하고 **구성**을 **릴리스**로 설정하고 **플랫폼**을 **iPhone**으로 설정합니다.
4. 특정 iOS SDK를 빌드하려면 **SDK 버전** 목록에서 선택합니다. 그렇지 않으면 이 값을 **기본**으로 둡니다.
5. 링크하면 사용되지 않는 코드를 제거하여 애플리케이션의 전체 크기를 줄일 수 있습니다. 대부분의 경우 **링커 동작**은 기본값인 **프레임 워크 SDK만 링크**로 설정해야 합니다. 일부 타사 라이브러리를 사용할 때와 같은 일부 상황에서는 필요한 코드가 제거되지 않도록 이 값을 **연결 안 함**으로 설정할 필요가 있습니다. 자세한 내용은 [Xamarin.iOS 앱 연결하기](~/ios/deploy-test/linker.md) 가이드를 참조하세요.
6. **PNG 이미지 최적화**를 확인하여 추가로 애플리케이션의 크기를 줄일 수 있는지 알아봅니다.
7. 디버깅은 빌드를 쓸데없이 크게 만들므로 사용하지 않도록 설정해야 합니다.
8. iOS 11의 경우 **ARM64**를 지원하는 디바이스 아키텍처 중 하나를 선택합니다. 64비트 iOS 디바이스 빌드에 대한 자세한 내용은 [32/64비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md) 설명서의 **Xamarin.iOS 앱의 64비트 빌드 활성화** 섹션을 참조하세요.
9. 더 작고 빠른 코드를 빌드하기 위해 **LLVM** 컴파일러를 사용하고자 할 수 있습니다. 그러나 이 옵션은 컴파일 시간을 증가시킵니다.
10. 애플리케이션의 요구에 따라 사용할 **가비지 수집**의 형식 및 **국제화**에 대한 설정을 조정할 수도 있습니다.

    위에서 설명하는 옵션을 설정한 다음, 빌드 설정은 다음과 유사하게 됩니다.

    ![iOS 빌드 설정](publishing-to-the-app-store-images/build-w157.png "iOS 빌드 설정")

    또한 빌드 설정에 대한 추가 설명이 있는 [iOS 빌드 메커니즘](~/ios/deploy-test/ios-build-mechanics.md) 가이드를 참조합니다.

11. **iOS 번들 서명** 탭으로 이동합니다. **구성**이 **릴리스**로 설정되고 **플랫폼**이 **iPhone**으로 설정되었는지 확인하고 **수동 프로비저닝**이 선택되었는지 확인합니다.
12. **서명 ID**를 **배포(자동)** 로 설정합니다.
13. **프로비전 프로필**은 [위에서 만든](#create-and-install-an-app-store-provisioning-profile) 앱 스토어 프로비전 프로필을 선택합니다.

    프로젝트의 번들 서명 옵션은 이제 다음과 유사 하게 표시됩니다.

    ![iOS 번들 서명 설정](publishing-to-the-app-store-images/bundleSigning-w157.png "iOS 번들 서명 설정")

14. **iOS IPA 옵션** 탭으로 이동합니다.
15. **구성**이 **릴리스**로 설정되고 **플랫폼**이 **iPhone**으로 설정되었는지 확인합니다.
16. **IPA(iTunes Package Archive) 빌드** 체크박스를 체크합니다. 이 설정은 각 **릴리스** 빌드가(선택한 구성이므로) .ipa 파일 생성을 생성하도록 합니다. 이 파일은 앱 스토어에서 릴리스하기 위해 Apple에 제출될 수 있습니다.

    > [!NOTE]
    > **iTunes 메타 데이터** 및 **iTunesArtwork**는 앱 스토어 릴리스에 필요하지 않습니다. 자세한 내용은 [Xamarin.iOS 앱 내 iTunesMetadata.plist 파일](~/ios/deploy-test/app-distribution/itunesmetadata.md) 및 [iTunes 아트 워크](~/ios/app-fundamentals/images-icons/app-icons.md#itunes-artwork)를 참조합니다.

17. Xamarin.iOS 프로젝트 이름과 다른.ipa 파일이름을 지정하려면 **패키지 이름** 필드에 입력합니다.

    ![iOS 번들 서명 설정](publishing-to-the-app-store-images/ipaOptions-w157.png "iOS 번들 서명 설정")

18. 빌드 구성을 저장하고 닫습니다.

-----

## <a name="configure-your-app-in-itunes-connect"></a>iTunes Connect에서 앱 구성

[iTunes Connect](https://itunesconnect.apple.com)는 앱 스토어에서 iOS 애플리케이션을 관리하는 웹 기반 도구 모음입니다. 먼저 iTunes Connect에서 Xamarin.iOS 애플리케이션을 제대로 구성한 후에, Apple 검토 및 앱 스토어 릴리스를 위해 제출해야 합니다.

이 작업을 수행하는 방법에 대해 알아보려면, [iTunes Connect에서에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) 가이드를 참조합니다.

## <a name="build-and-submit-your-app"></a>앱 빌드 및 제출

빌드 설정을 올바르게 구성하고 iTunes Connect가 제출 대기 중인 경우, 이제 앱을 빌드하고 Apple에 제출할 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac용 Visual Studio에서 빌드 구성 및 빌드할 디바이스(시뮬레이터는 해당사항 없음) **릴리스**를 선택합니다.

    ![빌드 구성 및 플랫폼 선택](publishing-to-the-app-store-images/chooseConfig-m157.png "빌드 구성 및 플랫폼 선택")

2. **빌드** 메뉴에서 **게시를 위해 보관**을 선택합니다.
3. 보관이 만들어지면 **보관** 보기가 표시됩니다.

    ![보관 보기](publishing-to-the-app-store-images/archives-m157.png "보관 보기")

    > [!NOTE]
    > 기본적으로는 **보관** 보기는 열려 있는 솔루션의 보관만 표시합니다. 보관이 있는 솔루션을 모두 보려면 **모든 보관 표시** 체크박스를 체크합니다. 포함한 디버그 정보가 필요한 경우 충돌 보고서를 기호화하여 사용될 수 있도록 이전 보관을 보관하는 것이 좋습니다.

4. **서명 및 배포**를 클릭하여 게시 마법사를 엽니다.
5. **앱 스토어** 배포 채널을 선택합니다. **다음**을 클릭합니다.

    ![배포 채널 선택](publishing-to-the-app-store-images/distChannel-m157.png "배포 채널 선택")

6. **프로비전 프로필** 창에서 서명 ID, 앱 및 프로비전 프로필을 선택합니다. **다음**을 클릭합니다.

    ![프로비전 프로필 선택](publishing-to-the-app-store-images/provProfileSelect-m157.png "프로비전 프로필 선택")

7. 패키지 세부 정보를 확인하고 **게시**를 클릭하여 앱을 위해 .ipa 파일을 저장합니다.

    ![앱 세부 정보 확인](publishing-to-the-app-store-images/publish-m157.png "앱 세부 정보 확인")

8. .ipa 파일이 저장되면 앱이 iTunes Connect에 업로드될 준비가 됩니다.

    ![제출 준비됨](publishing-to-the-app-store-images/readyToGo-m157.png "제출 준비됨")

9. **애플리케이션 로더 열기**를 클릭하여 로그인합니다(반드시 Apple ID에 대한 [앱 특정 암호 만들기](https://support.apple.com/ht204397)를 해야 함).

    > [!NOTE]
    > 도구에 대한 자세한 내용은 [애플리케이션 로더에 대한 Apple 문서](https://help.apple.com/itc/apploader/#/apdS673accdb)를 참조합니다.

10. **앱 배달**을 선택하고 **선택** 단추를 클릭합니다.

    ![앱 배달 선택](publishing-to-the-app-store-images/publishvs01.png "앱 배달 선택")

11. 위에서 만든 .ipa 파일을 선택하고 **확인** 버튼을 클릭합니다.
12. 애플리케이션 로더에서 파일의 유효성을 검사합니다.

    ![유효성 검사 화면](publishing-to-the-app-store-images/publishvs02.png "유효성 검사 화면")

13. **다음** 단추를 클릭합니다. 그러면 앱 스토어에 대한 애플리케이션의 유효성이 검사됩니다.

    ![앱 스토어에 대한 유효성 검사](publishing-to-the-app-store-images/publishvs03.png "앱 스토어에 대한 유효성 검사")

14. **보내기** 단추를 클릭하여 검토를 위해 애플리케이션을 Apple에 보냅니다.
15. 파일이 성공적으로 업로드되면 애플리케이션 로더에서 알려줍니다.

    > [!NOTE]
    > Apple은 .ipa 파일에 포함된 **iTunesMetadata.plist**가 있는 앱을 거부할 수 있으며 다음과 같은 오류가 발생합니다.
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > 이 오류의 해결 방법은 [Xamarin 포럼의 이 게시물](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1)을 참조합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

> [!NOTE]
> Visual Studio 2017은 현재 mac용 Visual Studio에서 찾은 워크플로 **게시를 위해 보관**을 지원하지 않습니다.

1. Visual Studio 2019 또는 Visual Studio 2017이 [Mac 빌드 호스트에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md)되었는지 확인합니다.
2. Visual Studio 2017 **솔루션 구성** 드롭다운에서 **릴리스**를 선택하고 **솔루션 플랫폼**에서 **iPhone**을 선택합니다.

    ![빌드 구성 및 플랫폼 선택](publishing-to-the-app-store-images/chooseConfig-w157.png "빌드 구성 및 플랫폼 선택")

3. 프로젝트를 빌드합니다. .ipa 파일이 만들어집니다.

    > [!NOTE]
    > 이 문서의[릴리스 빌드 구성 업데이트](#update-the-release-build-configuration) 섹션에서는 각각의 **릴리스** 빌드에 대한.ipa 파일을 만들기 위한 앱의 빌드 설정을 구성했습니다.

4. Windows 머신에서 .ipa 파일을 찾으려면 Visual Studio 2019 또는 Visual Studio 2017 **솔루션 탐색기**에서 Xamarin.iOS 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 **파일 탐색기에서 폴더 열기**를 선택합니다. 그런 다음, 방금 연 Windows **파일 탐색기**에서 **bin/iPhone/Release** 하위 디렉터리로 이동합니다. [사용자 지정한 .ipa 파일 출력 위치](#customize-the-ipa-location)가 없다면, 이 디렉터리에 있어야 합니다.
5. 대신 Mac 빌드 호스트에서 .ipa 파일을 보려면 Visual Studio 2019 또는 Visual Studio 2017 **솔루션 탐색기**(Windows에서) Xamarin.iOS 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고  **빌드 서버에 IPA 파일 표시**를 선택합니다. 이렇게 하면 .ipa 파일이 선택된 Mac 빌드 호스트에서 **Finder** 창이 열립니다.
6. Mac 빌드 호스트에서 **애플리케이션 로더**를 엽니다. Xcode에서 **Xcode &gt; 개발자 도구 열기 &gt; 애플리케이션 로더**를 선택합니다.

    > [!NOTE]
    > 도구에 대한 자세한 내용은 [애플리케이션 로더에 대한 Apple 문서](https://help.apple.com/itc/apploader/#/apdS673accdb)를 참조합니다.

7. 애플리케이션 로더에 로그인합니다(Apple ID에 대해 [앱별 암호를 생성](https://support.apple.com/ht204397)해야 함).
8. **앱 배달**을 선택하고 **선택** 단추를 클릭합니다.

    ![앱 배달 선택](publishing-to-the-app-store-images/publishvs01.png "앱 배달 선택")

9. 위에서 만든 .ipa 파일을 선택하고 **확인**을 클릭합니다.
10. 애플리케이션 로더에서 파일의 유효성을 검사합니다.

    ![유효성 검사 화면](publishing-to-the-app-store-images/publishvs02.png "유효성 검사 화면")

11. **다음** 단추를 클릭합니다. 그러면 앱 스토어에 대한 애플리케이션의 유효성이 검사됩니다.

    ![앱 스토어에 대한 유효성 검사](publishing-to-the-app-store-images/publishvs03.png "앱 스토어에 대한 유효성 검사")

12. **보내기** 단추를 클릭하여 검토를 위해 애플리케이션을 Apple에 보냅니다.
13. 파일이 성공적으로 업로드되면 애플리케이션 로더에서 알려줍니다.

    > [!NOTE]
    > Apple은 .ipa 파일에 포함된 **iTunesMetadata.plist**가 있는 앱을 거부할 수 있으며 다음과 같은 오류가 발생합니다.
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > 이 오류의 해결 방법은 [Xamarin 포럼의 이 게시물](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1)을 참조합니다.

-----

## <a name="itunes-connect-status"></a>iTunes Connect 상태

앱 등록 상태를 보려면 iTunes Connect에 로그인하고 앱을 선택합니다. 초기 상태는 **검토 대기 중**이지만 처리 중에는 **수신 업로드**로 일시적으로 나타날 수 있습니다.

![검토 대기 중](publishing-to-the-app-store-images/image21.png "검토 대기 중")

## <a name="tips-and-tricks"></a>팁과 요령

### <a name="customize-the-ipa-location"></a>IPA 위치 사용자 지정

**MSBuild** 속성으로서`IpaPackageDir` .ipa 파일 출력 위치를 사용자 지정할 수 있습니다. `IpaPackageDir`을 사용자 지정 위치로 설정하면 .ipa 파일이 기본 타임스탬프가 적용된 하위 디렉터리 대신 해당 위치에 배치됩니다. 이는 CI(지속적인 통합) 빌드에 사용되는 것과 같이 특정 디렉터리 경로를 올바르게 사용하도록 자동화된 빌드를 만들 때 유용할 수 있습니다.

새 속성을 사용하는 방법에는 여러 가지가 있습니다. 예를 들어 .ipa 파일을 이전의 기본 디렉터리(Xamarin.iOS 9.6 이하)로 출력하려면 다음 방법 중 하나를 사용하여 `IpaPackageDir` 속성을 `$(OutputPath)`로 설정할 수 있습니다. 두 방법은 모두 IDE 빌드 및 **msbuild** 또는 **mdtool**을 사용하는 명령줄 빌드를 포함하여 모든 Unified API Xamarin.iOS 빌드와 호환됩니다.

- 첫 번째 옵션은 **MSBuild** 파일의 `<PropertyGroup>` 요소 내에 `IpaPackageDir` 속성을 설정하는 것입니다. 예를 들어 다음 `<PropertyGroup>`을 .csproj iOS 앱 프로젝트 파일의 아래쪽(닫은 `</Project>` 태그 바로 앞)에 추가할 수 있습니다.

    ```xml
    <PropertyGroup>
      <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- .ipa 파일을 빌드하는 데 사용되는 구성에 해당하는 기존 `<PropertyGroup>`의 아래쪽에 `<IpaPackageDir>` 요소를 추가하는 것이 더 좋습니다. 이는 iOS IPA 옵션 프로젝트 속성 페이지에서 계획된 설정과의 향후 호환성을 위해 프로젝트를 준비하기 때문입니다. 현재 `Release|iPhone` 구성을 사용하여 .ipa 파일을 빌드하는 경우 업데이트된 속성 그룹 전체는 다음과 비슷할 수 있습니다.

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone'">
       <Optimize>true</Optimize>
       <OutputPath>bin\iPhone\Release</OutputPath>
       <ErrorReport>prompt</ErrorReport>
       <WarningLevel>4</WarningLevel>
       <ConsolePause>false</ConsolePause>
       <CodesignKey>iPhone Developer</CodesignKey>
       <MtouchUseSGen>true</MtouchUseSGen>
       <MtouchUseRefCounting>true</MtouchUseRefCounting>
       <MtouchFloat32>true</MtouchFloat32>
       <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
       <MtouchLink>SdkOnly</MtouchLink>
       <MtouchArch>;ARMv7, ARM64</MtouchArch>
       <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
       <MtouchTlsProvider>Default</MtouchTlsProvider>
       <PlatformTarget>x86&</PlatformTarget>
       <BuildIpa>true</BuildIpa>
       <IpaPackageDir>$(OutputPath</IpaPackageDir>
    </PropertyGroup>
    ```

**msbuild** 명령줄 빌드에 대한 또 다른 방법은 `/p:` 명령줄 인수를 추가하여 `IpaPackageDir` 속성을 설정하는 것입니다. 이 경우 **msbuild**는 명령줄에서 전달된 `$()` 식을 확장하지 않으므로 `$(OutputPath)` 구문을 사용할 수 없습니다. 대신 전체 경로 이름을 제공해야 합니다.

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

또는 Mac에서 다음과 같습니다.

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

배포 빌드를 만들고 보관했으므로 이제 iTunes Connect에 애플리케이션을 제출할 준비가 되었습니다.

## <a name="summary"></a>요약

이 문서에서는 구성, 빌드 및 릴리스하기 위해 앱 스토어에 iOS 앱을 제출하는 방법을 설명합니다.

## <a name="related-links"></a>관련 링크

- [Apple 개발자 포털(Apple)](https://developer.apple.com/account/)
- [iTunes Connect(Apple)](https://itunesconnect.apple.com)
- [앱 스토어 검토 지침(Apple)](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [일반적인 앱 거부(Apple)](https://developer.apple.com/app-store/review/rejections/)
- [Xamarin.iOS에서 기능 사용](~/ios/deploy-test/provisioning/capabilities/index.md)
- [Xamarin.iOS에서 자격 사용](~/ios/deploy-test/provisioning/entitlements.md)
- [iTunes Connect에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Xamarin.iOS 애플리케이션 아이콘](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Xamarin.iOS 앱에 대한 시작 화면](~/ios/app-fundamentals/images-icons/launch-screens.md)
- [애플리케이션 로더 설명서(Apple)](https://help.apple.com/itc/apploader/#/apdS673accdb)
