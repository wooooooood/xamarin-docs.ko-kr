---
title: Apple TV App Store에 게시
description: 이 문서에서는 Apple TV 앱 스토어에 앱을 게시 하는 방법을 설명 합니다. Xamarin을 사용 하 여 빌드된 tvOS 응용 프로그램을 구성, 프로 비전, 빌드 및 제출 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 52448C93-DC19-40FA-BF8C-608AE680FF49
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: dd453ab5397e409cc9a7ccef9b4b845d47f32a8b
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573731"
---
# <a name="publishing-to-the-apple-tv-app-store"></a>Apple TV App Store에 게시

Apple은 모든 Apple TV 장치에 응용 프로그램을 배포 하기 위해 apple *Tv 앱 스토어*를 통해 앱을 게시 하도록 요구 하 여 앱이 tvOS apps에 대해 원 중지 쇼핑 위치를 저장 하도록 합니다. 여러 유형의 앱 개발자는이 단일 배포 지점의 대규모 성공에 대해 대문자를 지정할 수 있습니다. Apple TV 앱 스토어는 앱 개발자에 게 배포 및 지불 시스템을 제공 하는 턴키 솔루션입니다.

Apple TV 앱 스토어에 응용 프로그램을 제출 하는 프로세스는 다음을 포함 합니다.

1. *앱 ID* 만들고 *자격*을 선택합니다.
2. 배포 프로 *비전 프로필* 을 만드는 중입니다.
3. 이 프로필을 사용 하 여 앱을 빌드합니다.
4. *ITunes Connect*를 통해 앱을 제출 합니다.

이 문서에서는 Apple TV 앱 스토어 배포용 앱을 프로 비전, 빌드 및 제출 하는 데 필요한 모든 단계를 다룹니다.

<a name="Before_you_Submit"></a>

## <a name="before-you-submit-an-application"></a>애플리케이션을 제출하기 전에

Apple TV 앱 스토어에 게시 하기 위해 앱을 제출한 후 apple에서 품질 및 콘텐츠에 대 한 Apple의 지침을 충족 하는지 확인 하는 검토 프로세스를 진행 합니다. 애플리케이션이 이러한 지침을 충족하지 못하는 경우 Apple에서 이를 거부합니다. 이 경우 Apple에서 언급한 부적합 사항을 처리하고 다시 제출해야 합니다.
따라서 이러한 지침을 숙지하고 프로그램을 애플리케이션에 적용하여 Apple 검토를 통해 최상의 기회를 얻을 수 있습니다. Apple의 지침은 [앱 스토어 검토 지침](https://developer.apple.com/appstore/resources/approval/guidelines.html) 및 [새 apple TV에 대 한 앱 제출 준비](https://developer.apple.com/tvos/submit/)에서 사용할 수 있습니다.

앱을 제출할 때 주의해야 할 몇 가지 사항은 다음과 같습니다.

1. 앱의 설명이 앱에 포함 된 기능과 일치 하는지 확인 합니다.
2. 정상적인 사용에서 앱이 충돌하지 않는지 테스트합니다. 여기에는 지원 되는 모든 Apple TV 장치에 대 한 사용이 포함 됩니다.

Apple은 Apple TV 앱 스토어 제출 팁의 목록도 유지 관리 합니다. 이러한 목록은 [앱 스토어에서 배포](https://developer.apple.com/appstore/resources/submission/tips.html)에서 참조할 수 있습니다.

<a name="Configuring_your_Application_in_iTunes_Connect"></a>

## <a name="configuring-your-application-in-itunes-connect"></a>iTunes Connect에서 애플리케이션 구성

[ITunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) 는 특히 Apple TV 앱 스토어에서 tvOS apps를 관리 하는 데 사용할 수 있는 웹 기반 도구 모음입니다. TvOS 앱을 검토를 위해 Apple에 제출 하기 전에 iTunes Connect에서 적절 하 게 설정 하 고 구성 해야 하며, 궁극적으로는 Apple TV 앱 스토어에서 판매 또는 무료 앱으로 릴리스할 수 있습니다.

다음을 수행합니다.

1. iTunes Connect의 **계약, 세금 및 뱅킹** 섹션에서 적절한 계약이 적용되고 최신 상태인지 확인하여 iOS 애플리케이션을 무료 또는 판매용으로 릴리스합니다.
2. 응용 프로그램에 대 한 새 **ITunes Connect 레코드** 를 만들고 해당 **표시 이름을** 지정 합니다 (Apple TV 앱 스토어에 표시 됨).
3. **판매 가격**을 선택하거나 애플리케이션이 무료로 릴리스되도록 지정합니다.
4. 지원 되는 Apple TV 장치에서 작동 중인 응용 프로그램의 **앱 스토어 아이콘** (큼 아이콘) 및 스크린샷을 제공 합니다. 자세한 내용은 [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) 가이드를 참조 하세요.
5. 최종 사용자에 게 기능 및 혜택을 포함 하 여 앱에 대 한 명확 하 고 간결한 **설명을** 제공 합니다.
6. 사용자가 Apple TV 앱 스토어에서 앱을 찾는 데 도움이 되는 **범주**, **하위 범주**및 **키워드** 를 제공 합니다.
7. Apple에서 요구 하는 웹 사이트에 대 한 **연락처** 및 **지원** url을 제공 합니다.
8. Apple TV 앱 스토어에서 자녀 보호에 사용 되는 응용 프로그램의 **등급**을 설정 합니다.
9. **Game Center** 및 **앱 내 구매**와 같은 선택적인 앱 스토어 기술을 구성 합니다.

자세한 내용은 [ITunes Connect에서 TvOS 앱 구성](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) 설명서를 참조 하세요.

<a name="Preparing_for_App_Store_Distribution"></a>

## <a name="preparing-for-app-store-distribution"></a>앱 스토어 배포 준비

Apple TV 앱 스토어에 앱을 게시 하려면 먼저 배포를 위해 빌드해야 합니다. 여기에는 여러 단계가 포함 됩니다. 다음 섹션에서는 게시를 위해 tvOS 앱을 준비 하 고 검토 및 릴리스할 수 있도록 Apple TV 앱 스토어에 제출할 수 있도록 준비 하는 데 필요한 모든 것을 설명 합니다.

<a name="Provisioning_for_Application_Services"></a>

### <a name="provisioning-for-application-services"></a>애플리케이션 서비스 프로비전

Apple은 고유 ID를 만들 때 tvOS 앱에 대해 활성화할 수 있는 특수 한 애플리케이션 서비스 (권한 라고도 함)를 선택 합니다. 사용자 지정 자격을 사용 하는지 여부에 관계 없이 Apple TV 앱 스토어에 게시 하기 전에 tvOS 앱에 대 한 고유 ID를 만들어야 합니다.

앱 ID를 만들고 필요에 따라 자격을 선택하려면 Apple의 웹 기반 iOS 프로비전 포털을 사용하여 다음 단계를 수행합니다.

1. **프로 비전**  >  **개발**을 선택 합니다.
2. 단추를 클릭 **+** 하 고 새 응용 프로그램의 **이름** 및 **번들 ID** 를 제공 합니다.
3. 화면 아래쪽으로 스크롤하고 tvOS 앱에 필요한 모든 **App Services** 를 선택 합니다.
4. **계속** 단추를 클릭하고 화면의 지시에 따라 새 앱 ID를 만듭니다.

앱 ID를 정의할 때 필요한 애플리케이션 서비스를 선택 하 고 구성 하는 것 외에도, 및 파일을 모두 편집 하 여 tvOS 프로젝트에서 앱 ID 및 자격을 구성 해야 합니다 `Info.plist` `Entitlements.plist` .

Mac용 Visual Studio에서 다음을 수행 합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 `Info.plist` 파일을 두 번 클릭하여 엽니다.
2. **TvOS 응용 프로그램 대상** 섹션에서 응용 프로그램의 이름을 입력 하 고 앱 ID를 정의할 때 만든 **번들 식별자** 를 입력 합니다.
3. 변경 내용을 `Info.plist` 파일에 저장합니다.
4. 편집하기 위해 **솔루션 탐색기**에서 `Entitlements.plist` 파일을 두 번 클릭하여 엽니다.
5. 앱 ID를 정의할 때 위에서 수행한 설정과 일치 하도록 tvOS 앱에 필요한 자격을 선택 하 고 구성 합니다.
6. 변경 내용을 `Entitlements.plist` 파일에 저장합니다.

자세한 지침은 [애플리케이션 서비스 프로비전](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning-for-application-services) 설명서를 참조하세요. 이 문서를 iOS 용으로 작성 하는 동안에는 동일한 단계를 사용 하 여 tvOS 앱을 프로 비전 합니다.

<a name="Setting_the_Apps_Icons_and_Launch_Screens"></a>

### <a name="setting-the-apps-icons-launch-image-and-top-shelf-image"></a>앱 아이콘 설정, 이미지 및 상위 선반 이미지 시작

Apple TV 앱 스토어에 포함 하기 위해 Apple에서 tvOS 앱을 수락 하려면 해당 앱이 실행 되는 모든 Apple TV 장치에 대 한 적절 한 아이콘, 시작 및 인기 이미지가 필요 합니다. `Assets.car`ITunes Connect에 업로드 되기 전에 파일에 컴파일되고 tvOS 앱의 번들에 포함 되는 필수 이미지 자산을 추가 해야 합니다.

자세한 지침은 [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) 설명서를 참조 하세요.

<a name="Creating_and_Installing_a_Distribution_Profile"></a>

### <a name="creating-and-installing-a-distribution-profile"></a>배포 프로필 만들기 및 설치

tvOS는 *프로 비전 프로필* 을 사용 하 여 특정 응용 프로그램 빌드를 배포할 수 있는 방법을 제어 합니다. 이러한 파일은 앱 서명에 사용된 인증서, *애플리케이션 ID* 및 앱을 설치할 수 있는 위치에 대한 정보가 포함된 파일입니다. 개발 및 임시 배포의 경우 프로비전 프로필에는 앱이 배포될 수 있도록 허용되는 디바이스 목록도 포함됩니다. 그러나 Apple TV 앱 스토어 배포의 경우 공용 배포에 대 한 유일한 메커니즘이 Apple TV 앱 스토어를 통해서만 발생 하므로 인증서 및 앱 ID 정보만 포함 됩니다.

프로비전에는 Apple의 웹 기반 iOS 프로비전 포털을 사용하는 다음 단계가 포함됩니다.

1. **프로 비전**  >  **배포**를 선택 합니다.
2. 단추를 클릭 **+** 하 고 **Apple TV 앱 스토어로**만들 배포 프로필의 유형을 선택 합니다.
3. 드롭다운 목록에서 배포 프로필을 만들려는 **앱 ID**를 선택합니다.
4. 응용 프로그램에 서명 하는 데 필요한 인증서를 선택 합니다.
5. 새 **배포 프로필**에 대한 **이름**을 입력하고 해당 프로필을 생성합니다.
6. Xcode에서 사용 가능한 프로필 목록을 새로 고칩니다.
7. Visual Studio에서 **앱 스토어** _빌드 구성_에 대 한 배포 프로 비전 프로필을 선택 합니다.

자세한 지침은 [배포 프로필 만들기](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#creatingprofile) 및 [Xamarin.iOS 프로젝트에서 배포 프로필 선택](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile)을 참조하세요. 이 두 문서는 모두 iOS에만 적용 되지만 동일한 기술이 tvOS apps에 사용 됩니다.

<a name="Setting_the_Build_Configuration_for_your_Application"></a>

### <a name="setting-the-build-configuration-for-your-application"></a>애플리케이션에 대한 빌드 구성 설정

기본적으로 새 tvOS 앱을 만들 때 **디버그** 및 **릴리스** 배포 모두에 대해 _빌드 구성이_ 자동으로 만들어집니다. Apple에 제출할 응용 프로그램의 최종 빌드를 수행 하기 전에 기본 **릴리스** 구성에 대 한 몇 가지 사항을 수정 해야 합니다.

다음을 수행합니다.

1. **솔루션 탐색기** 및 선택 **옵션** 에서 **프로젝트 이름을** 마우스 오른쪽 단추로 클릭 하 여 편집용으로 엽니다.
2. 특정 버전의 tvOS를 대상으로 하는 경우 **tvOS 빌드**  >  **iOS SDK 버전**에서 선택 합니다. TvOS 지원의 미리 보기 릴리스에서는이 값을 **기본값으로**설정 해 두십시오.
3. 링크를 사용 하면 사용 되지 않는 메서드, 속성, 클래스 등을 제거 하 여 앱의 배포 전 전체 크기를 줄일 수 있습니다. 대부분의 경우에는 대부분의 경우에는 **연결 프레임 워크 SDK**의 기본값을 그대로 두어야 합니다. 특정 타사 라이브러리를 사용 하는 경우와 같은 일부 경우에는 필요한 요소가 제거 되지 않도록이 값을 **연결 안 함** 으로 설정 해야 할 수 있습니다.
4. TvOS 앱을 배송 하려면 LLVM 최적화 컴파일러를 사용 해야 합니다. **릴리스** 구성에서 **LLVM 최적화 컴파일러 사용** 확인란이 선택 되어 있는지 확인 합니다.
5. 또한 tvOS apps에서 bitcode를 사용 해야 합니다. **릴리스** 구성 아래에서 `--bitcode=asmonly` **추가 mtouch 인수** 상자에를 추가 합니다.
6. **IOS에 대 한 PNG 이미지 파일 최적화** 확인란을 선택 하면 앱의 결과물 크기를 더 줄일 수 있습니다.
7. 빌드를 불필요 하 게 더 크게 만들기 때문에 디버깅을 사용 하도록 *설정 하면 안* 됩니다.

<a name="Building_and_Submitting_the_Distributable"></a>

## <a name="building-and-submitting-the-distributable"></a>배포 가능한 파일 빌드 및 제출

TvOS 앱이 올바르게 구성 되 면 이제 검토 및 릴리스를 위해 Apple에 제출할 최종 배포 빌드를 수행할 준비가 되었습니다.

#### <a name="build-your-archive"></a>보관 빌드

1. Mac용 Visual Studio에서 **릴리스 | 디바이스** 구성을 선택합니다:

    ![](app-store-publishing-images/buildxs01new.png "Select the Release configuration")
2. **빌드** 메뉴에서 **게시를 위해 보관**을 선택합니다.

    [![](app-store-publishing-images/buildxs02new.png "Select Archive for Publishing")](app-store-publishing-images/buildxs02new.png#lightbox)
3. 보관이 만들어지면 **보관** 보기가 표시됩니다.

    [![](app-store-publishing-images/buildxs03new.png "The Archives view")](app-store-publishing-images/buildxs03new.png#lightbox)

### <a name="sign-and-distribute-your-app"></a>앱 서명 및 배포

보관용 애플리케이션을 빌드할 때마다 *보관 보기*가 자동으로 열리고, 보관된 모든 프로젝트가 솔루션별로 그룹화되어 표시됩니다. 이 보기에는 기본적으로 현재 열려 있는 솔루션만 표시됩니다. 보관이 있는 솔루션을 모두 보려면 **모든 보관 표시** 옵션을 클릭합니다.

나중에 생성된 모든 디버그 정보를 기호로 나타낼 수 있도록 고객에게 배포된 보관 파일(앱 스토어 또는 엔터프라이즈 배포)을 유지하는 것이 좋습니다.

앱에 서명하고 배포할 준비를 하려면 다음을 수행합니다.

1. 아래 그림과 같이 **서명 및 배포 ...** 를 선택 합니다.

    [![](app-store-publishing-images/buildxs04new.png ", Select theSign and Distribute...")](app-store-publishing-images/buildxs04new.png#lightbox)
2. 그러면 게시 마법사가 열립니다. **앱 스토어** 배포 채널을 선택 하 여 패키지를 만들고 응용 프로그램 로더를 엽니다.

    [![](app-store-publishing-images/distribute01.png "Select the App Store distribution channel")](app-store-publishing-images/distribute01.png#lightbox)
3. 프로 비전 프로필 화면에서 서명 id 및 해당 프로 비전 프로필을 선택 하거나 다른 id로 다시 서명 합니다.

    [![](app-store-publishing-images/distribute02.png "Select the signing identity and corresponding provisioning profile")](app-store-publishing-images/distribute02.png#lightbox)
4. 패키지 세부 정보를 확인하고 **게시**를 클릭하여 `.ipa` 패키지를 저장합니다.

    [![](app-store-publishing-images/distribute03.png "Verify the details of the package")](app-store-publishing-images/distribute03.png#lightbox)
5. `.ipa`가 저장되면 앱이 애플리케이션 로더를 통해 iTunes Connect에 업로드될 준비가 됩니다.

    [![](app-store-publishing-images/distribute04.png "Uploaded to iTunes Connect via the Application Loader")](app-store-publishing-images/distribute04.png#lightbox)

배포 빌드를 만들고 보관했으므로 이제 iTunes Connect에 애플리케이션을 제출할 준비가 되었습니다.

<a name="Submitting_Your_App_to_Apple"></a>

## <a name="submitting-your-app-to-apple"></a>Apple에 앱 제출

배포 빌드가 완료되면 iOS 애플리케이션을 Apple에 제출하여 앱 스토어에서 검토하고 릴리스할 준비가 됩니다.

Mac용 Visual Studio의 보관 워크플로는 다음을 저장 하면 응용 프로그램 로더가 자동으로 열립니다 `.ipa` .

1. *앱 배달*을 선택하고 *선택* 단추를 클릭합니다.

    [![](app-store-publishing-images/publishvs01.png "Select Deliver Your App")](app-store-publishing-images/publishvs01.png#lightbox)

2. 위에서 만든 zip 또는 IPA 파일을 선택하고 **확인** 단추를 클릭합니다.
3. 애플리케이션 로더에서 파일의 유효성을 검사합니다.

    [![](app-store-publishing-images/publishvs02.png "The Application Loader validation screen")](app-store-publishing-images/publishvs02.png#lightbox)
4. *다음* 단추를 클릭합니다. 그러면 앱 스토어에 대한 애플리케이션의 유효성이 검사됩니다.

    [![](app-store-publishing-images/publishvs03.png "The application being validated against the App Store")](app-store-publishing-images/publishvs03.png#lightbox)
5. **보내기** 단추를 클릭하여 검토를 위해 애플리케이션을 Apple에 보냅니다.
6. 파일이 성공적으로 업로드되면 애플리케이션 로더에서 알려줍니다.

<a name="iTunes_Connect_Status"></a>

### <a name="itunes-connect-status"></a>iTunes Connect 상태

ITunes Connect에 다시 로그인 하 고 사용 가능한 앱 목록에서 앱을 선택 하는 경우 iTunes Connect의 상태에서 **검토 대기** 중임을 표시 합니다 (처리 중에 **업로드 수신** 됨을 일시적으로 읽을 수 있음).

[![](app-store-publishing-images/image21.png "The status in iTunes Connect showing Waiting for Review")](app-store-publishing-images/image21.png#lightbox)

<a name="Troubleshooting"></a>

## <a name="troubleshooting"></a>문제 해결

Apple TV 앱 스토어에 tvOS 앱을 제출 하는 데 문제가 있는 경우 [문제 해결](~/ios/tvos/troubleshooting.md) 가이드를 참조 하세요. 여기에는 발생할 수 있는 몇 가지 알려진 문제 및 tvOS에서 문제를 해결 하는 방법이 포함 되어 있습니다.

<a name="Summary"></a>

## <a name="summary"></a>요약

이 문서에서는 Apple TV 앱 스토어 게시용 앱을 구성, 빌드 및 제출 하는 단계별 가이드를 제공 합니다. 먼저, 배포 프로비전 프로필을 만들고 설치하는 데 필요한 단계에 대해 설명했습니다. 다음으로 Mac용 Visual Studio를 사용 하 여 배포 빌드를 만드는 방법을 살펴보았습니다. 마지막으로, iTunes Connect 및 Xcode Archive 도구를 사용 하 여 Apple TV 앱 스토어에 응용 프로그램을 제출 하는 방법을 보여 주었습니다.

## <a name="related-links"></a>관련 링크

- [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md)
- [새 Apple TV에 대 한 앱 제출 준비](https://developer.apple.com/tvos/submit/)
- [앱 스토어 제출 팁](https://developer.apple.com/appstore/resources/submission/tips.html)
- [일반적인 앱 거부](https://developer.apple.com/app-store/review/rejections/)
- [앱 스토어 검토 지침](https://developer.apple.com/appstore/resources/approval/guidelines.html)
