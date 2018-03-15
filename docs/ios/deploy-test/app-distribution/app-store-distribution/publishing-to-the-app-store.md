---
title: "앱 스토어에 게시"
description: "이 문서에서는 앱 스토어를 통해 배포할 Xamarin.iOS 응용 프로그램을 구성, 빌드 및 게시하는 방법을 보여 줍니다. 여기에는 배포를 위해 응용 프로그램을 준비하는 방법, 검토를 위해 Apple 도구를 사용하여 응용 프로그램을 제출하는 방법, 마지막으로 응용 프로그램을 앱 스토어에 게시하는 방법에 대해 설명하는 단계별 지침이 포함되어 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: dfa3d1f89d813f2e57863e615c701cd78c655ac0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="publishing-to-the-app-store"></a>앱 스토어에 게시

_이 문서에서는 앱 스토어를 통해 배포할 Xamarin.iOS 응용 프로그램을 구성, 빌드 및 게시하는 방법을 보여 줍니다. 여기에는 배포를 위해 응용 프로그램을 준비하는 방법, 검토를 위해 Apple 도구를 사용하여 응용 프로그램을 제출하는 방법, 마지막으로 응용 프로그램을 앱 스토어에 게시하는 방법에 대해 설명하는 단계별 지침이 포함되어 있습니다._

Apple은 응용 프로그램을 모든 iOS 장치에 배포하기 위해 *앱 스토어*를 통해 앱을 게시해야 하므로 앱 스토어를 iOS 응용 프로그램을 위한 원 스톱 쇼핑 위치로 만들었습니다. 여러 유형의 응용 프로그램 개발자는 스토어에 있는 50만 개가 넘는 응용 프로그램과 함께 이 단일 배포 지점에서 엄청난 성공을 거두었습니다. 앱 스토어는 턴키 솔루션으로, 앱 개발자에게 배포 및 결제 시스템을 모두 제공합니다.

앱 스토어에 응용 프로그램을 제출하는 프로세스는 다음과 같습니다.

1. **앱 ID** 만들고 **자격**을 선택합니다.
2. **배포 프로비전 프로필**을 만듭니다.
3. 이 프로필을 사용하여 응용 프로그램을 빌드합니다.
4. **iTunes Connect**를 통해 응용 프로그램을 제출합니다.


이 문서에서는 앱 스토어 배포를 위해 응용 프로그램을 프로비전, 빌드 및 제출하는 데 필요한 모든 단계에 대해 설명합니다.

## <a name="before-you-submit-an-application"></a>응용 프로그램을 제출하기 전에

앱 스토어에 게시할 앱이 제출되면 Apple에서 품질 및 내용에 대한 Apple의 지침을 충족하는지 확인하는 검토 프로세스를 거칩니다. 응용 프로그램이 이러한 지침을 충족하지 못하는 경우 Apple에서 이를 거부합니다. 이 경우 Apple에서 언급한 부적합 사항을 처리하고 다시 제출해야 합니다.
따라서 이러한 지침을 숙지하고 프로그램을 응용 프로그램에 적용하여 Apple 검토를 통해 최상의 기회를 얻을 수 있습니다. Apple 지침은 [앱 스토어 검토 지침](https://developer.apple.com/appstore/resources/approval/guidelines.html)에서 사용할 수 있습니다.

앱을 제출할 때 주의해야 할 몇 가지 사항은 다음과 같습니다.

1. 응용 프로그램의 설명이 응용 프로그램에 포함된 기능과 일치하는지 확인합니다.
2. 정상적인 사용에서 응용 프로그램이 충돌하지 않는지 테스트합니다. 여기에는 지원하는 모든 iOS 장치에서의 사용이 포함됩니다.


또한 Apple은 앱 스토어 제출 팁의 목록도 유지합니다. 이러한 목록은 [앱 스토어에서 배포](https://developer.apple.com/appstore/resources/submission/tips.html)에서 참조할 수 있습니다.

## <a name="configuring-your-application-in-itunes-connect"></a>iTunes Connect에서 응용 프로그램 구성

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa)는 특히 앱 스토어에서 iOS 응용 프로그램을 관리하는 웹 기반 도구 모음입니다. 먼저 iTunes Connect에서 Xamarin.iOS 응용 프로그램을 제대로 설정하고 구성한 후에, 이를 검토하여 궁극적으로 앱 스토어에서 판매하거나 무료 앱으로 릴리스할 수 있도록 Apple에 제출해야 합니다.

다음을 수행합니다.

1. iTunes Connect의 **계약, 세금 및 뱅킹** 섹션에서 적절한 계약이 적용되고 최신 상태인지 확인하여 iOS 응용 프로그램을 무료 또는 판매용으로 릴리스합니다.
2. 응용 프로그램에 대한 새 **iTunes Connect 레코드**를 만들고, **표시 이름**(앱 스토어에 표시됨)을 지정합니다.
3. **판매 가격**을 선택하거나 응용 프로그램이 무료로 릴리스되도록 지정합니다.
5. 최종 사용자에게 제공되는 기능 및 혜택을 포함하여 응용 프로그램에 대한 명확하고 간결한 **설명**을 제공합니다.
6. 사용자가 앱 스토어에서 앱을 찾는 데 도움이 되는 **범주**, **하위 범주** 및 **키워드**를 제공합니다.
7. Apple에서 요구하는 웹 사이트에 대한 **연락처** 및 **지원** URL을 제공합니다.
8. 앱 스토어의 자녀 보호 기능에 사용되는 앱의 **등급**을 설정합니다.
9. **Game Center** 및 **인앱 구매**와 같은 선택적인 앱 스토어 기술을 구성합니다.

자세한 내용은 [iTunes Connect에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) 설명서를 참조하세요.

## <a name="preparing-for-app-store-distribution"></a>앱 스토어 배포 준비

응용 프로그램을 앱 스토어에 게시하려면 먼저 배포용으로 빌드해야 하며, 여기에는 여러 단계가 포함됩니다. 다음 섹션에서는 검토 및 릴리스를 위해 빌드하고 앱 스토어에 제출할 수 있도록 Xamarin.iOS 응용 프로그램을 준비하는 데 필요한 모든 항목에 대해 설명합니다.

### <a name="provisioning-for-application-services"></a>응용 프로그램 서비스 프로비전

Apple은 iOS 응용 프로그램에 대한 고유 ID를 만들 때 활성화될 수 있는 자격이라고도 하는 특별한 응용 프로그램 서비스에 대한 선택 항목을 제공합니다. 사용자 지정 자격의 사용 여부에 관계없이 여전히 앱 스토어에 게시하기 전에 Xamarin.iOS 응용 프로그램에 대한 고유 ID를 만들어야 합니다.

앱 ID를 만들고 필요에 따라 자격을 선택하려면 Apple의 웹 기반 iOS 프로비전 포털을 사용하여 다음 단계를 수행합니다.

1. **인증서, 식별자 및 프로필** 섹션에서 **식별자** > **앱 ID**를 차례로 선택합니다.
2. **+** 단추를 클릭하고 새 응용 프로그램에 대한 **이름** 및 **번들 ID**를 제공합니다.
3. 화면 아래쪽으로 스크롤하여 Xamarin.iOS 응용 프로그램에 필요한 모든 **App Services**를 선택합니다.
4. **계속** 단추를 클릭하고 화면의 지시에 따라 새 앱 ID를 만듭니다.

앱 ID를 정의할 때 필요한 응용 프로그램 서비스를 선택하고 구성하는 것 외에도, `Info.plist` 및 `Entitlements.plist` 파일을 모두 편집하여 Xamarin.iOS 프로젝트의 앱 ID 및 자격을 구성해야 합니다.

다음을 수행합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 `Info.plist` 파일을 두 번 클릭하여 엽니다.
2. **iOS 응용 프로그램 대상** 섹션에서 응용 프로그램의 이름을 입력하고, 앱 ID를 정의할 때 만든 **번들 식별자**를 입력합니다.
3. 변경 내용을 `Info.plist` 파일에 저장합니다.
4. 편집하기 위해 **솔루션 탐색기**에서 `Entitlements.plist` 파일을 두 번 클릭하여 엽니다.
5. 앱 ID를 정의할 때 위에서 수행한 설정과 일치하도록 Xamarin.iOS 응용 프로그램에 필요한 자격을 선택하고 구성합니다.
6. 변경 내용을 `Entitlements.plist` 파일에 저장합니다.

자세한 지침은 [응용 프로그램 서비스 프로비전](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) 설명서를 참조하세요.

### <a name="setting-the-store-icons"></a>스토어 아이콘 설정

앱 스토어 아이콘은 이제 자산 카탈로그를 통해 제공됩니다. 앱 스토어 아이콘을 추가하려면 먼저 프로젝트의 **Assets.xcassets** 파일에 있는 **AppIcon** 이미지 집합을 찾습니다.

자산 카탈로그에 필요한 아이콘의 이름은 **앱 스토어**이며, 크기는 **1024x1024**여야 합니다. Apple은 자산 카탈로그의 앱 스토어 아이콘이 투명하거나 알파 채널을 포함할 수 없다고 주장했습니다.

스토어 아이콘 설정에 대한 내용은 [앱 스토어 아이콘](~/ios/app-fundamentals/images-icons/app-store-icon.md) 가이드를 참조하세요.

### <a name="setting-the-apps-icons-and-launch-screens"></a>앱 아이콘 및 시작 화면 설정

Apple에서 iOS 응용 프로그램을 앱 스토어에 포함하도록 승인하려면 응용 프로그램이 실행될 모든 iOS 장치에 적합한 아이콘과 시작 화면이 필요합니다. 앱 아이콘은 **Assets.xcassets** 파일에 설정된 **AppIcon** 이미지를 통해 자산 카탈로그의 프로젝트에 추가 됩니다. 시작 화면은 스토리보드를 통해 추가됩니다.

앱 아이콘을 만들고 화면을 실행하는 방법에 대한 자세한 내용은 [응용 프로그램 아이콘](~/ios/app-fundamentals/images-icons/app-icons.md) 및 [시작 화면](~/ios/app-fundamentals/images-icons/launch-screens.md) 가이드를 참조하세요.

### <a name="creating-and-installing-a-distribution-profile"></a>배포 프로필 만들기 및 설치

iOS는 *프로비전 프로필*을 사용하여 특정 응용 프로그램 빌드를 배포하는 방법을 제어합니다. 이러한 파일은 앱 서명에 사용된 인증서, *응용 프로그램 ID* 및 앱을 설치할 수 있는 위치에 대한 정보가 포함된 파일입니다. 개발 및 임시 배포의 경우 프로비전 프로필에는 앱이 배포될 수 있도록 허용되는 장치 목록도 포함됩니다. 그러나 앱 스토어 배포의 경우 앱 스토어를 통한 공개 배포만이 유일한 메커니즘이므로 인증서 및 앱 ID 정보만 포함됩니다.

프로비전에는 Apple의 웹 기반 iOS 프로비전 포털을 사용하는 다음 단계가 포함됩니다.

1.  **프로비전** > **배포**를 차례로 선택합니다.
2.  **+** 단추를 클릭하고 만들려는 배포 프로필 유형을 **앱 스토어**로 선택합니다.
3.  드롭다운 목록에서 배포 프로필을 만들려는 **앱 ID**를 선택합니다.
4.  응용 프로그램에 서명할 유효한 프로덕션(배포) 인증서를 선택합니다.
5.  새 **배포 프로필**에 대한 **이름**을 입력하고 해당 프로필을 생성합니다.
6.  Mac에서 Xcode를 열고 **기본 설정 > [Apple ID 선택]> 세부 정보 보기...**로 차례로 이동합니다. Mac의 Xcode에서 사용 가능한 모든 프로필을 다운로드합니다.
7.  IDE로 돌아가서 **iOS 번들 서명** 옵션 아래에서 올바른 _빌드 구성_(**앱 스토어** 또는 **릴리스** 중 하나)에 대한 배포 프로비전 프로필을 선택합니다.

자세한 지침은 [배포 프로필 만들기](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) 및 [Xamarin.iOS 프로젝트에서 배포 프로필 선택](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile)을 참조하세요.

## <a name="setting-the-build-configuration-for-your-application"></a>응용 프로그램에 대한 빌드 구성 설정

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

다음을 수행합니다.

1. 편집할 수 있도록 **Solution Pad**에서 **프로젝트 이름**을 마우스 오른쪽 단추로 클릭하고 **옵션**을 선택하여 엽니다.
2. **구성** 드롭다운에서 **iOS 빌드**를 선택하고 **릴리스 | iPhone**을 선택합니다.

    ![](publishing-to-the-app-store-images/configurevs01.png "구성 드롭다운에서 AppStore 선택")

3. 대상으로 하는 특정 iOS 버전이 있는 경우 **SDK 버전** 드롭다운 목록에서 선택합니다. 그렇지 않으면 이 값을 기본 설정으로 그대로 둡니다.
4. 연결하면 사용되지 않는 메서드, 속성, 클래스 등을 제거하여 응용 프로그램의 배포 가능한 전체 크기를 줄일 수 있으며, 대부분의 경우 **SDK 어셈블리만 연결**의 기본값으로 유지해야 합니다. 일부 특정 타사 라이브러리를 사용할 때와 같이 일부 상황에서는 이 값을 **연결하지 않음**으로 설정하여 필수 요소가 제거되지 않도록 강제할 수 있습니다. 자세한 내용은 [iOS 빌드 메커니즘](~/ios/deploy-test/ios-build-mechanics.md) 가이드를 참조하세요.
5. **iOS용 PNG 이미지 파일 최적화** 확인란을 선택하면 응용 프로그램의 결과물 크기를 더 줄일 수 있습니다.
6. 디버깅은 빌드를 쓸데없이 크게 만들므로 _사용하지 않도록_ 설정해야 합니다.
8. iOS 11의 경우 **ARM64**를 지원하는 장치 아키텍처 중 하나를 선택해야 합니다. 64비트 iOS 장치 빌드에 대한 자세한 내용은 [32/64비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md) 설명서의 **Xamarin.iOS 앱의 64비트 빌드 활성화** 섹션을 참조하세요.
9. 필요에 따라 더 작고 더 빠른 코드를 만드는 **LLVM** 컴파일러를 사용할 수도 있지만 컴파일하는 데 시간이 더 오래 걸립니다.
10. 응용 프로그램의 요구에 따라 사용할 **가비지 수집**의 형식 및 **국제화**에 대한 설정을 조정할 수도 있습니다.
11. 변경 내용을 빌드 구성에 저장합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

기본적으로 Visual Studio에서 새 Xamarin.iOS 응용 프로그램을 만들 때 **임시** 및 **앱 스토어** 배포 둘 다에 대한 _빌드 구성_이 자동으로 만들어집니다. Apple에 제출할 응용 프로그램의 최종 빌드를 수행하기 전에 수정해야 하는 몇 가지 기본 구성이 있습니다.

다음을 수행합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 **프로젝트 이름**을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택하여 엽니다.
2. **구성** 드롭다운에서 **iOS 빌드** 및 **AppStore**(또는 AppStore를 사용할 수 없는 경우 **릴리스**)를 선택합니다.

    ![](publishing-to-the-app-store-images/configurevs01.png "구성 드롭다운에서 AppStore 선택")

3. 대상으로 하는 특정 iOS 버전이 있는 경우 **SDK 버전** 드롭다운 목록에서 선택합니다. 그렇지 않으면 이 값을 기본 설정으로 그대로 둡니다.
4. 연결하면 사용되지 않는 메서드, 속성, 클래스 등을 제거하여 응용 프로그램의 배포 가능한 전체 크기를 줄일 수 있으며, 대부분의 경우 **SDK 어셈블리만 연결**의 기본값으로 유지해야 합니다. 일부 특정 타사 라이브러리를 사용할 때와 같이 일부 상황에서는 이 값을 **연결하지 않음**으로 설정하여 필수 요소가 제거되지 않도록 강제할 수 있습니다. 자세한 내용은 [iOS 빌드 메커니즘](~/ios/deploy-test/ios-build-mechanics.md) 가이드를 참조하세요.
5. **iOS용 PNG 이미지 파일 최적화** 확인란을 선택하면 응용 프로그램의 결과물 크기를 더 줄일 수 있습니다.
6. 디버깅은 빌드를 쓸데없이 크게 만들므로 _사용하지 않도록_ 설정해야 합니다.
7. **고급** 탭을 클릭합니다.

    ![](publishing-to-the-app-store-images/configurevs02.png "고급 탭")

8. Xamarin.iOS 응용 프로그램이 iOS 8 및 64비트 iOS 장치를 대상으로 하는 경우 **ARM64**를 지원하는 장치 아키텍처 중 하나를 선택해야 합니다. 64비트 iOS 장치 빌드에 대한 자세한 내용은 [32/64비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md) 설명서의 **Xamarin.iOS 앱의 64비트 빌드 활성화** 섹션을 참조하세요.
9. 필요에 따라 더 작고 더 빠른 코드를 만드는 **LLVM** 컴파일러를 사용할 수도 있지만 컴파일하는 데 시간이 더 오래 걸립니다.
10. 응용 프로그램의 요구에 따라 사용할 **가비지 수집**의 형식 및 **국제화**에 대한 설정을 조정할 수도 있습니다.
11. 변경 내용을 빌드 구성에 저장합니다.

-----

## <a name="building-and-submitting-the-distributable"></a>배포 가능한 파일 빌드 및 제출

Xamarin.iOS 응용 프로그램을 올바르게 구성했으므로 검토 및 릴리스를 위해 Apple에 제출할 최종 배포 빌드를 수행할 준비가 되었습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="build-your-archive"></a>보관 빌드

1. Mac용 Visual Studio에서 **릴리스 | 장치** 구성을 선택합니다:

    ![](publishing-to-the-app-store-images/buildxs01new.png "릴리스 | 장치 구성 선택")
1. **빌드** 메뉴에서 **게시를 위해 보관**을 선택합니다.

    ![](publishing-to-the-app-store-images/buildxs02new.png "게시를 위해 보관 선택")

1. 보관이 만들어지면 **보관** 보기가 표시됩니다.

    ![](publishing-to-the-app-store-images/buildxs03new.png "표시된 보관 보기")


> [!NOTE]
> 참고: 이전의 _앱 스토어_ 및 _임시_ 구성은 이제 모든 Mac용 Visual Studio 템플릿 프로젝트에서 제거되었지만, 이전 프로젝트에는 이러한 구성이 여전히 포함되어 있습니다. 이 경우 위 목록의 1단계에서 **앱 스토어 | 장치** 구성을 계속 사용할 수 있습니다.

### <a name="sign-and-distribute-your-app"></a>앱 서명 및 배포

 보관용 응용 프로그램을 빌드할 때마다 **보관 보기**가 자동으로 열리고, 보관된 모든 프로젝트가 솔루션별로 그룹화되어 표시됩니다. 이 보기에는 기본적으로 현재 열려 있는 솔루션만 표시됩니다. 보관이 있는 솔루션을 모두 보려면 **모든 보관 표시** 옵션을 클릭합니다.

 나중에 생성된 모든 디버그 정보를 기호로 나타낼 수 있도록 고객에게 배포된 보관 파일(앱 스토어 또는 엔터프라이즈 배포)을 유지하는 것이 좋습니다.

 앱에 서명하고 배포할 준비를 하려면 다음을 수행합니다.


1. 아래 그림과 같이 **서명 및 배포...**를 선택합니다.

    ![](publishing-to-the-app-store-images/buildxs04new.png "서명 및 배포... 선택")

1. 그러면 게시 마법사가 열립니다. **앱 스토어** 배포 채널을 선택하여 패키지를 만들고 응용 프로그램 로더를 엽니다.

    ![](publishing-to-the-app-store-images/distribute01.png "응용 프로그램 로더 열기")

1. [프로비전 프로필] 화면에서 서명 ID 및 해당 프로비전 프로필을 선택하거나 다른 ID로 다시 서명합니다.

    ![](publishing-to-the-app-store-images/distribute02.png "서명 ID 및 해당 프로비전 프로필 선택")

1. 패키지 세부 정보를 확인하고 **게시**를 클릭하여 `.ipa` 패키지를 저장합니다.

    ![](publishing-to-the-app-store-images/distribute03.png "패키지 세부 정보 확인")

1. `.ipa`가 저장되면 앱이 응용 프로그램 로더를 통해 iTunes Connect에 업로드될 준비가 됩니다.

    ![](publishing-to-the-app-store-images/distribute04.png "게시 성공 화면")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio용 Xamarin 플러그 인은 현재 iOS 응용 프로그램을 앱 스토어에 게시하기 위한 보관 워크플로를 지원하지 않습니다. 이에 따라 아래에 설명된 **임시 IPA 빌드** 명령을 통해 만든 IPA를 업로드했습니다.


## <a name="build-an-ipa"></a>IPA 빌드

 이 섹션에서는 임시 또는 엔터프라이즈 배포를 사용하는 경우 워크플로와 비슷한 IPA 빌드에 대해 설명합니다. 그러나 이 빌드는 위에서 만든 앱 스토어 프로비전 프로필을 사용하여 서명됩니다.

 다음을 수행합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 Xamarin.iOS 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택하여 엽니다.

    ![](publishing-to-the-app-store-images/imagevs01.png "속성 선택")
1. **iOS 번들 서명**을 선택하고 프로비전 프로필을 앱 스토어 프로비전 프로필로 변경합니다.

    ![](publishing-to-the-app-store-images/ipa01.png "iOS 번들 서명을 선택하고 프로비전 프로필을 앱 스토어 프로비전 프로필로 변경")
1. **iOS IPA 옵션**을 선택하고 **구성** 드롭다운 목록에서 **임시**를 선택합니다(임시가 표시되지 않으면 대신 **릴리스** 선택).

    ![](publishing-to-the-app-store-images/imagevs02.png "구성 드롭다운 목록에서 임시 선택")

1. 필요에 따라 IPA에 대해 **패키지 이름**을 지정할 수 있습니다. 지정하지 않으면 Xamarin.iOS 프로젝트와 동일한 이름이 적용됩니다.
1. 변경 내용을 프로젝트 속성에 저장합니다.
1. Mac용 Visual Studio **빌드 구성** 드롭다운에서 **임시**를 선택합니다.

    ![](publishing-to-the-app-store-images/imagevs05.png "빌드 구성 드롭다운에서 임시 선택")
1. 프로젝트를 빌드하여 IPA 패키지를 만듭니다.
1. IPA는 `Bin` > _iOS 장치_ > `Ad Hoc` 폴더에 빌드됩니다.

    ![](publishing-to-the-app-store-images/imagevs06.png "파일 탐색기에 표시된 IPA")

-----


## <a name="customizing-the-ipa-location"></a>IPA 위치 사용자 지정

`.ipa` 파일 출력 위치를 쉽게 사용자 지정할 수 있도록 새로운 `IpaPackageDir` **MSBuild** 속성이 추가되었습니다. `IpaPackageDir`을 사용자 지정 위치로 설정하면 `.ipa` 파일이 기본 타임스탬프가 적용된 하위 디렉터리 대신 해당 위치에 배치됩니다. 이는 CI(지속적인 통합) 빌드에 사용되는 것과 같이 특정 디렉터리 경로를 올바르게 사용하도록 자동화된 빌드를 만들 때 유용할 수 있습니다.

새 속성을 사용하는 방법에는 여러 가지가 있습니다.

예를 들어 `.ipa` 파일을 이전의 기본 디렉터리(Xamarin.iOS 9.6 이하)로 출력하려면 다음 방법 중 하나를 사용하여 `IpaPackageDir` 속성을 `$(OutputPath)`로 설정할 수 있습니다. 두 방법 모두는 IDE 빌드 및 `xbuild`, `msbuild` 또는 `mdtool`을 사용하는 명령줄 빌드를 포함하여 모든 통합 API Xamarin.iOS 빌드와 호환됩니다.

  - 첫 번째 옵션은 **MSBuild** 파일의 `<PropertyGroup>` 요소 내에 `IpaPackageDir` 속성을 설정하는 것입니다. 예를 들어 다음 `<PropertyGroup>`을 `.csproj` iOS 앱 프로젝트 파일의 아래쪽(닫은 `</Project>` 태그 바로 앞)에 추가할 수 있습니다.

      ```xml
        <PropertyGroup>
            <IpaPackageDir>$(OutputPath)</IpaPackageDir>
        </PropertyGroup>
      ```
  - `.ipa` 파일을 빌드하는 데 사용되는 구성에 해당하는 기존 `<PropertyGroup>`의 아래쪽에 `<IpaPackageDir>` 요소를 추가하는 것이 더 좋습니다. 이는 iOS IPA 옵션 프로젝트 속성 페이지에서 계획된 설정과의 향후 호환성을 위해 프로젝트를 준비하기 때문입니다. 현재 `Release|iPhone` 구성을 사용하여 `.ipa` 파일을 빌드하는 경우 업데이트된 속성 그룹 전체는 다음과 비슷할 수 있습니다.

      ```xml
      <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' )
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
`msbuild` 또는 `xbuild` 명령줄 빌드에 대한 또 다른 방법은 `/p:` 명령줄 인수를 추가하여 `IpaPackageDir` 속성을 설정하는 것입니다. 이 경우 `msbuild`는 명령줄에서 전달된 `$()` 식을 확장하지 않으므로 `$(OutputPath)` 구문을 사용할 수 없습니다. 대신 전체 경로 이름을 제공해야 합니다. Mono의 `xbuild` 명령은 `$()` 식을 확장하지만, 이후 릴리스에서는 [플랫폼 간 버전의 `msbuild`](http://www.mono-project.com/docs/about-mono/releases/4.4.0/#msbuild-preview-for-os-x)를 지지하여 `xbuild`는 결국 사용 중단될 예정이므로 전체 경로 이름을 사용하는 것이 좋습니다. 이 방법을 사용하는 전체 예제는 Windows에서 다음과 비슷하게 보일 수 있습니다.

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

또는 Mac에서 다음과 같습니다.

```bash
xbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

배포 빌드를 만들고 보관했으므로 이제 iTunes Connect에 응용 프로그램을 제출할 준비가 되었습니다.

### <a name="automatically-copy-app-bundles-back-to-windows"></a>.app 번들을 Windows로 다시 자동 복사

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="submitting-your-app-to-apple"></a>Apple에 앱 제출

> [!NOTE]
> 참고: Apple은 최근에 iOS 응용 프로그램에 대한 확인 프로세스를 변경했으며 `iTunesMetadata.plist`가 IPA에 포함된 앱을 거부할 수 있습니다. `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"` 오류가 발생하면 [여기](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1)서 설명하는 해결 방법으로 문제를 해결해야 합니다.

배포 빌드가 완료되면 iOS 응용 프로그램을 Apple에 제출하여 앱 스토어에서 검토하고 릴리스할 준비가 됩니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 다음을 수행합니다.

1. **Xcode**를 시작합니다.
2. **창** 메뉴에서 **구성 도우미**를 선택합니다.
3. **보관** 탭을 클릭하고 위에서 빌드한 보관을 선택합니다.

    ![](publishing-to-the-app-store-images/publishxs01.png "제출할 보관 선택")
4. **유효성 검사...** 단추를 클릭합니다.
5. 확인할 계정을 선택하고 **선택** 단추를 클릭합니다.

    ![](publishing-to-the-app-store-images/publishxs02.png "확인할 계정 선택")
6. **유효성 검사** 단추를 클릭합니다.

    ![](publishing-to-the-app-store-images/publishxs03.png "파일 요약 대화 상자")
7. 번들에 문제가 있으면 해당 문제가 표시됩니다.
8. 모든 문제를 해결하고 Mac용 Visual Studio에서 보관 파일을 다시 빌드합니다.
9. **제출...** 단추를 클릭합니다.
10. 확인할 계정을 선택하고 **선택** 단추를 클릭합니다.

    ![](publishing-to-the-app-store-images/publishxs04.png "확인할 계정 선택")
11. **제출** 단추를 클릭합니다.

    ![](publishing-to-the-app-store-images/publishxs05.png "파일 요약 대화 상자")
12. iTunes Connect에 파일이 업로드되면 Xcode에서 알려줍니다.


_.ipa_가 저장되면 Mac용 Visual Studio의 보관 워크플로에서 응용 프로그램 로더가 자동으로 열립니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

응용 프로그램을 Apple에 제출하면 응용 프로그램 로더 앱을 사용하여 검토할 수 있습니다. 이러한 단계는 Mac 빌드 호스트에서 수행해야 합니다. 응용 프로그램 로더의 복사본은 [여기](https://itunesconnect.apple.com/apploader/ApplicationLoader_3.0.dmg)서 다운로드할 수 있습니다.

-----

1. *앱 배달*을 선택하고 *선택* 단추를 클릭합니다.

    [![](publishing-to-the-app-store-images/publishvs01.png "앱 배달 선택")](publishing-to-the-app-store-images/publishvs01.png#lightbox)

2. 위에서 만든 zip 또는 IPA 파일을 선택하고 **확인** 단추를 클릭합니다.

3. 응용 프로그램 로더에서 파일의 유효성을 검사합니다.

    [![](publishing-to-the-app-store-images/publishvs02.png "유효성 검사 화면")](publishing-to-the-app-store-images/publishvs02.png#lightbox)
4. *다음* 단추를 클릭합니다. 그러면 앱 스토어에 대한 응용 프로그램의 유효성이 검사됩니다.

    [![](publishing-to-the-app-store-images/publishvs03.png "앱 스토어에 대한 유효성 검사")](publishing-to-the-app-store-images/publishvs03.png#lightbox)
5. **보내기** 단추를 클릭하여 검토를 위해 응용 프로그램을 Apple에 보냅니다.
6. 파일이 성공적으로 업로드되면 응용 프로그램 로더에서 알려줍니다.

## <a name="itunes-connect-status"></a>iTunes Connect 상태

iTunes Connect에 다시 로그인하고 사용 가능한 앱 목록에서 응용 프로그램을 선택하면, iTunes Connect의 상태가 **검토 대기 중**임을 나타냅니다(처리 중에 **업로드 수신됨**을 일시적으로 읽을 수 있음).

[![](publishing-to-the-app-store-images/image21.png "검토 대기 중임을 나타내는 iTunes Connect 상태")](publishing-to-the-app-store-images/image21.png#lightbox)

## <a name="summary"></a>요약

이 문서에서는 앱 스토어 게시를 위한 응용 프로그램 구성, 빌드 및 제출에 대한 단계별 가이드를 제공했습니다. 먼저, 배포 프로비전 프로필을 만들고 설치하는 데 필요한 단계에 대해 설명했습니다. 다음으로, Visual Studio 및 Mac용 Visual Studio를 사용하여 배포 빌드를 만드는 방법을 살펴보았습니다. 마지막으로, iTunes Connect 및 도구를 사용하여 응용 프로그램을 앱 스토어에 제출하는 방법을 보여 주었습니다.


## <a name="related-links"></a>관련 링크

- [이미지 작업](~/ios/app-fundamentals/images-icons/index.md)
- [iOS 앱 개발 워크플로 가이드: 응용 프로그램 배포](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/35-Distributing_Applications/distributing_applications.html)
- [앱 스토어 제출 팁](https://developer.apple.com/appstore/resources/submission/tips.html)
- [일반적인 앱 거부](https://developer.apple.com/app-store/review/rejections/)
- [앱 스토어 검토 지침](https://developer.apple.com/appstore/resources/approval/guidelines.html)
