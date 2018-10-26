---
title: Apple TV App Store에 게시
description: 이 문서에서는 Apple TV 앱 스토어에 앱을 게시 하는 방법을 설명 합니다. 구성, 프로 비전, 빌드 및 Xamarin에 내장 된 tvOS 응용 프로그램을 제출 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 52448C93-DC19-40FA-BF8C-608AE680FF49
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: b941bcc8588e7fb0377430cca2829ad72ecbc8c6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122402"
---
# <a name="publishing-to-the-apple-tv-app-store"></a>Apple TV App Store에 게시

모든 Apple TV 장치에 응용 프로그램 배포, Apple에서는 앱이를 통해 게시할 수는 *Apple TV App Store*, 하므로 앱 스토어 tvOS 앱에 대 한 원스톱 쇼핑 위치 합니다. 다양 한 유형의 앱의 개발자가 단일 배포 지점에서 엄청난 성공을 거두었습니다 수 있습니다. Apple TV 앱 스토어는 턴키 솔루션, 앱 개발자 모두 배포 및 결제 시스템을 제공 합니다.

응용 프로그램을 Apple TV App Store에 제출 과정:

1. *앱 ID* 만들고 *자격*을 선택합니다.
2. *배포 프로비전 프로필*을 만듭니다.
3. 이 프로필을 사용 하 여 앱을 빌드합니다.
4. 통해 앱 제출 *iTunes Connect*합니다.


이 문서에서는 프로 비전, 빌드 및 Apple TV App Store 배포를 위한 앱을 제출 하는 데 필요한 모든 단계를 설명 합니다.

<a name="Before_you_Submit" />

## <a name="before-you-submit-an-application"></a>응용 프로그램을 제출하기 전에

Apple TV App Store에 게시할 앱을 제출 하면 이동 검토 프로세스를 통해 apple 품질 및 내용에 대 한 Apple의 지침을 충족 하는지 확인 합니다. 응용 프로그램이 이러한 지침을 충족하지 못하는 경우 Apple에서 이를 거부합니다. 이 경우 Apple에서 언급한 부적합 사항을 처리하고 다시 제출해야 합니다.
따라서 이러한 지침을 숙지하고 프로그램을 응용 프로그램에 적용하여 Apple 검토를 통해 최상의 기회를 얻을 수 있습니다. Apple의 지침에서 제공 됩니다 [앱 스토어 검토 지침](https://developer.apple.com/appstore/resources/approval/guidelines.html) 하 고 [에 새 Apple TV 프로그램 앱 제출 준비](https://developer.apple.com/tvos/submit/)합니다.

앱을 제출할 때 주의해야 할 몇 가지 사항은 다음과 같습니다.

1. 앱의 설명에는 앱에 포함 된 기능 일치 해야 합니다.
2. 정상적인 사용에서 앱이 충돌하지 않는지 테스트합니다. 여기에 지 원하는 모든 Apple TV 장치에서 사용 합니다.


Apple에는 Apple TV 앱 스토어 제출 팁의 목록을 유지 관리합니다. 이러한 목록은 [앱 스토어에서 배포](https://developer.apple.com/appstore/resources/submission/tips.html)에서 참조할 수 있습니다.

<a name="Configuring_your_Application_in_iTunes_Connect" />

## <a name="configuring-your-application-in-itunes-connect"></a>iTunes Connect에서 응용 프로그램 구성

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) 은 무엇 보다도, Apple TV App Store에서 tvOS 앱 관리에 대 한 웹 기반 도구 모음입니다. Xamarin.tvOS 앱 해야 제대로 설치 하 고 검토를 위해 Apple에 제출할 수 있습니다 하 고 궁극적으로 판매 하거나 Apple TV 앱 스토어에서 무료 앱으로 릴리스할 수 있기 전에 iTunes Connect에서에서 구성 합니다.

다음을 수행합니다.

1. iTunes Connect의 **계약, 세금 및 뱅킹** 섹션에서 적절한 계약이 적용되고 최신 상태인지 확인하여 iOS 응용 프로그램을 무료 또는 판매용으로 릴리스합니다.
2. 새 **iTunes Connect 레코드** 응용 프로그램에 대 한 지정 하 고 해당 **표시 이름** (처럼 Apple TV App Store에서).
3. **판매 가격**을 선택하거나 응용 프로그램이 무료로 릴리스되도록 지정합니다.
4. 제공을 **앱 스토어 아이콘** (큰 아이콘) 및 작업을 지 원하는 Apple TV 장치에 응용 프로그램의 스크린샷입니다. 참조 우리의 [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) 자세한 가이드입니다.
5. Clear, 제공 간결한 **설명** 앱의 해당 기능을 포함 하 고 최종 사용자에 게 도움이 됩니다.
6. 제공 **범주**, **하위 범주**, 및 **키워드** Apple TV App Store에서 앱을 찾는 사용자 수 있도록 합니다.
7. Apple에서 요구하는 웹 사이트에 대한 **연락처** 및 **지원** URL을 제공합니다.
8. 응용 프로그램 설정 **등급**, 자녀 Apple TV App Store에서 사용 됩니다.
9. **Game Center** 및 **인앱 구매**와 같은 선택적인 앱 스토어 기술을 구성합니다.

자세한 내용은 참조 하십시오 우리의 [iTunes Connect에서에서 tvOS 앱 구성](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) 설명서.

<a name="Preparing_for_App_Store_Distribution" />

## <a name="preparing-for-app-store-distribution"></a>앱 스토어 배포 준비

Apple TV 앱 스토어에 앱을 게시 하려면 먼저 배포의 경우 여러 단계를 포함 하는 작성 해야 합니다. 다음 섹션에서는 빌드할 수 있습니다를 검토 및 릴리스 Apple TV App Store에 제출 되도록 게시 Xamarin.tvOS 앱을 준비 하는 데 필요한 모든 내용을 다룹니다.

<a name="Provisioning_for_Application_Services" />

### <a name="provisioning-for-application-services"></a>응용 프로그램 서비스 프로비전

Apple에 대 한 고유 ID를 만들면 tvOS 앱에 대 한 활성화 될 수 있는 자격이 라고도 하는 특별 한 응용 프로그램 서비스를 제공 합니다. 여부 사용자 지정 자격을 사용 하는 경우, Apple TV App Store에 게시 하기 전에 Xamarin.tvOS 앱에 대 한 고유 ID를 만들려면 해야 계속 합니다.

앱 ID를 만들고 필요에 따라 자격을 선택하려면 Apple의 웹 기반 iOS 프로비전 포털을 사용하여 다음 단계를 수행합니다.

1. 선택 **프로 비전** > **개발**합니다.
2. **+** 단추를 클릭하고 새 응용 프로그램에 대한 **이름** 및 **번들 ID**를 제공합니다.
3. 화면 아래쪽으로 스크롤하여 선택 **App Services** Xamarin.tvOS 앱에서 해야 합니다.
4. **계속** 단추를 클릭하고 화면의 지시에 따라 새 앱 ID를 만듭니다.

선택한 앱 ID를 정의할 때 필요한 응용 프로그램 서비스를 구성 하는 것 외에도 해야 모두 편집 하 여 Xamarin.tvOS 프로젝트의 앱 ID와 자격을 구성 합니다 `Info.plist` 고 `Entitlements.plist` 파일입니다.

Mac 용 Visual Studio에서 다음을 수행 합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 `Info.plist` 파일을 두 번 클릭하여 엽니다.
2. 에 **tvOS 응용 프로그램 대상** 섹션에서 응용 프로그램에 대 한 이름을 입력 하 고 입력 합니다 **번들 식별자** 앱 ID를 정의할 때 만든
3. 변경 내용을 `Info.plist` 파일에 저장합니다.
4. 편집하기 위해 **솔루션 탐색기**에서 `Entitlements.plist` 파일을 두 번 클릭하여 엽니다.
5. 선택한 앱 ID를 정의할 때 위에서 수행한 설정과 일치 하도록 하면 Xamarin.tvOS 앱에 필요한 자격을 구성
6. 변경 내용을 `Entitlements.plist` 파일에 저장합니다.

자세한 지침은 [응용 프로그램 서비스 프로비전](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) 설명서를 참조하세요. 이 문서는 iOS 용으로 작성 된 경우 동일한 단계를 Xamarin.tvOS 앱 프로 비전에 사용 됩니다.

<a name="Setting_the_Apps_Icons_and_Launch_Screens" />

### <a name="setting-the-apps-icons-launch-image-and-top-shelf-image"></a>앱 아이콘, 시작 이미지 및 인기 코너 이미지 설정

Apple에서 Apple TV App Store에 포함 되도록 허용할 tvOS 앱에 대해 적절 한 아이콘, 시작 및 인기 코너 이미지 모두에서 실행 되는 Apple TV 장치에 대 한 필요 합니다. 컴파일되는 필요한 이미지 자산 추가 해야는 `Assets.car` 파일을 iTunes Connect에 업로드 되기 전에 Xamarin.tvOS 앱의 번들에 포함 합니다.

자세한 내용은 참조 하십시오 우리의 [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) 설명서.

<a name="Creating_and_Installing_a_Distribution_Profile" />

### <a name="creating-and-installing-a-distribution-profile"></a>배포 프로필 만들기 및 설치

사용 하 여 tvOS *프로 비전 프로필* 특정 응용 프로그램 빌드를 배포 하는 방법을 제어 합니다. 이러한 파일은 앱 서명에 사용된 인증서, *응용 프로그램 ID* 및 앱을 설치할 수 있는 위치에 대한 정보가 포함된 파일입니다. 개발 및 임시 배포의 경우 프로비전 프로필에는 앱이 배포될 수 있도록 허용되는 장치 목록도 포함됩니다. 그러나 Apple TV App Store 배포에 대 한 인증서 및 앱 ID 정보만 포함 됩니다, Apple TV App Store를 통해 공개 배포 하는 유일한 메커니즘 이므로 합니다.

프로비전에는 Apple의 웹 기반 iOS 프로비전 포털을 사용하는 다음 단계가 포함됩니다.

1.  **프로비전** > **배포**를 차례로 선택합니다.
2.  클릭 합니다 **+** 으로 만들려는 배포 프로필 유형을 선택 하 고 단추 **Apple TV App Store**합니다.
3.  드롭다운 목록에서 배포 프로필을 만들려는 **앱 ID**를 선택합니다.
4.  응용 프로그램 서명에 필요한 인증서를 선택 합니다.
5.  새 **배포 프로필**에 대한 **이름**을 입력하고 해당 프로필을 생성합니다.
6.  Xcode에서 사용 가능한 프로필의 목록을 새로 고칩니다.
7.  프로 비전 프로필에 대 한 Visual Studio에서 배포를 선택 합니다 **앱 스토어** _빌드 구성_합니다.

자세한 지침은 [배포 프로필 만들기](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#creatingprofile) 및 [Xamarin.iOS 프로젝트에서 배포 프로필 선택](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile)을 참조하세요. 마찬가지로 이러한 문서 모두 iOS에 특정 하 하지만 동일한 기법 tvOS 앱에 대 한 사용 됩니다.


<a name="Setting_the_Build_Configuration_for_your_Application" />

### <a name="setting-the-build-configuration-for-your-application"></a>응용 프로그램에 대한 빌드 구성 설정

기본적으로 새 Xamarin.tvOS 앱을 만들 때 _빌드 구성_ 둘 다에 대해 자동으로 생성 됩니다 **디버그** 하 고 **릴리스** 배포 합니다. Apple에 제출할 앱의 최종 빌드를 수행 하기 전에 자료에 있도록 해야 하는 몇 가지 수정 **릴리스** 구성 합니다.

다음을 수행합니다.

1. 마우스 오른쪽 단추로 클릭 합니다 **프로젝트 이름** 에 **솔루션 탐색기** 및 선택 **옵션** 열어서 편집 합니다.
2. TvOS의 특정 버전을 대상으로 하는 경우에서 선택할 **tvOS 빌드** > **iOS SDK 버전**합니다. TvOS 지원의 미리 보기 릴리스의 경우로이 값을 남겨 주세요 **기본**입니다.
3. 사용 되지 않는 메서드, 속성, 클래스를 제거 하 여 전체 크기의 배포 가능을 줄이고 연결, 등 및 대부분에서의 경우의 기본값을 두어야 **링크 프레임 워크 SDK만**합니다. 때와 같이 일부 상황에서 일부 특정을 사용 하 여 타사 라이브러리를이 값을 설정 해야 할 수 있습니다 **연결 하지 않으면** 제거 되 고 필요한 요소를 유지 하 합니다.
4. Xamarin.tvOS 앱을 전달할 LLVM 최적화 컴파일러 사용 해야 합니다. 있는지 확인 합니다 **LLVM 최적화 컴파일러 사용** 아래에 있는 확인란를 **릴리스** 구성 합니다.
5. Apple는 tvOS 앱 bitcode를 사용 하는 데도 필요 합니다. 아래는 **릴리스** 구성을 추가 `--bitcode=asmonly` 에 **추가 mtouch 인수** 상자입니다.
6. 합니다 **PNG 이미지 파일 최적화 iOS 용** 앱의 결과물 크기를 더 줄일 데는이 확인란을 선택 해야 합니다.
7. 디버깅은 *되지* 빌드를 불필요 하 게 큰 확인 됩니다 것 처럼 사용할 수 있습니다.


<a name="Building_and_Submitting_the_Distributable" />

## <a name="building-and-submitting-the-distributable"></a>배포 가능한 파일 빌드 및 제출

올바르게 구성 Xamarin.tvOS 앱 준비가 이제 검토 및 릴리스 Apple에 제출할 최종 배포 빌드를 수행 합니다.

#### <a name="build-your-archive"></a>보관 빌드

1. Mac용 Visual Studio에서 **릴리스 | 장치** 구성을 선택합니다:

    ![](app-store-publishing-images/buildxs01new.png "릴리스 구성을 선택 합니다.")
2. **빌드** 메뉴에서 **게시를 위해 보관**을 선택합니다.

    [![](app-store-publishing-images/buildxs02new.png "게시를 위해 보관 선택")](app-store-publishing-images/buildxs02new.png#lightbox)
3. 보관이 만들어지면 **보관** 보기가 표시됩니다.

    [![](app-store-publishing-images/buildxs03new.png "보관 보기")](app-store-publishing-images/buildxs03new.png#lightbox)

### <a name="sign-and-distribute-your-app"></a>앱 서명 및 배포

보관용 응용 프로그램을 빌드할 때마다 *보관 보기*가 자동으로 열리고, 보관된 모든 프로젝트가 솔루션별로 그룹화되어 표시됩니다. 이 보기에는 기본적으로 현재 열려 있는 솔루션만 표시됩니다. 보관이 있는 솔루션을 모두 보려면 **모든 보관 표시** 옵션을 클릭합니다.

나중에 생성된 모든 디버그 정보를 기호로 나타낼 수 있도록 고객에게 배포된 보관 파일(앱 스토어 또는 엔터프라이즈 배포)을 유지하는 것이 좋습니다.

앱에 서명하고 배포할 준비를 하려면 다음을 수행합니다.

1. 선택 된 **서명 및 배포 하는 중...** , 아래 그림 참조:

    [![](app-store-publishing-images/buildxs04new.png "를 theSign 및 배포를 선택 하는 중...")](app-store-publishing-images/buildxs04new.png#lightbox)
2. 그러면 게시 마법사가 열립니다. **앱 스토어** 배포 채널을 선택하여 패키지를 만들고 응용 프로그램 로더를 엽니다.

    [![](app-store-publishing-images/distribute01.png "앱 스토어 배포 채널 선택")](app-store-publishing-images/distribute01.png#lightbox)
3. 프로 비전 프로필 화면에서 서명 id 및 프로 비전 프로필을 해당 선택 하거나 다른 id로 다시 서명:

    [![](app-store-publishing-images/distribute02.png "서명 id 및 해당 프로 비전 프로필 선택")](app-store-publishing-images/distribute02.png#lightbox)
4. 패키지 세부 정보를 확인하고 **게시**를 클릭하여 `.ipa` 패키지를 저장합니다.

    [![](app-store-publishing-images/distribute03.png "패키지의 세부 정보를 확인 합니다.")](app-store-publishing-images/distribute03.png#lightbox)
5. `.ipa`가 저장되면 앱이 응용 프로그램 로더를 통해 iTunes Connect에 업로드될 준비가 됩니다.

    [![](app-store-publishing-images/distribute04.png "응용 프로그램 로더를 통해 iTunes Connect에 업로드")](app-store-publishing-images/distribute04.png#lightbox)

배포 빌드를 만들고 보관했으므로 이제 iTunes Connect에 응용 프로그램을 제출할 준비가 되었습니다.

<a name="Submitting_Your_App_to_Apple" />

## <a name="submitting-your-app-to-apple"></a>Apple에 앱 제출

배포 빌드가 완료되면 iOS 응용 프로그램을 Apple에 제출하여 앱 스토어에서 검토하고 릴리스할 준비가 됩니다.


Mac 용 Visual Studio의 보관 워크플로에서 응용 프로그램 로더 열기 자동으로 저장 한 후의 `.ipa`:

2. *앱 배달*을 선택하고 *선택* 단추를 클릭합니다.

    [![](app-store-publishing-images/publishvs01.png "앱 배달 선택")](app-store-publishing-images/publishvs01.png#lightbox)

3. 위에서 만든 zip 또는 IPA 파일을 선택하고 **확인** 단추를 클릭합니다.
4. 응용 프로그램 로더에서 파일의 유효성을 검사합니다.

    [![](app-store-publishing-images/publishvs02.png "응용 프로그램 로더 유효성 검사 화면")](app-store-publishing-images/publishvs02.png#lightbox)
5. *다음* 단추를 클릭합니다. 그러면 앱 스토어에 대한 응용 프로그램의 유효성이 검사됩니다.

    [![](app-store-publishing-images/publishvs03.png "앱 스토어에 대 한 유효성을 검사할 응용 프로그램")](app-store-publishing-images/publishvs03.png#lightbox)
6. **보내기** 단추를 클릭하여 검토를 위해 응용 프로그램을 Apple에 보냅니다.
7. 파일이 성공적으로 업로드되면 응용 프로그램 로더에서 알려줍니다.

<a name="iTunes_Connect_Status" />

### <a name="itunes-connect-status"></a>iTunes Connect 상태

ITunes Connect에 다시 로그인 하 고 사용할 수 있는 앱 목록에서 앱을 선택 하는 경우 iTunes Connect에서에서 상태는 이제 임을 **검토 대기** (일시적으로 읽을 수 있습니다 **업로드 수신** 처리 하는 동안).

[![](app-store-publishing-images/image21.png "ITunes에서 상태를 검토를 위해 대기를 보여 주는 연결")](app-store-publishing-images/image21.png#lightbox)

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>문제 해결

Xamarin.tvOS 앱을 Apple TV App Store에 제출 하는 데 문제가 있는 경우를 참조 하십시오 우리의 [문제 해결](~/ios/tvos/troubleshooting.md) 가이드입니다. Xamarin.tvOS 해결 방법과 발생할 수 있는 몇 가지 알려진된 문제가 포함 됩니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 구성, 빌드 및 Apple TV App Store 게시에 대 한 앱을 제출 하는 단계별 가이드를 제공 합니다. 먼저, 배포 프로비전 프로필을 만들고 설치하는 데 필요한 단계에 대해 설명했습니다. 다음으로, 배포 빌드를 만들려면 Mac 용 Visual Studio를 사용 하는 방법을 단계별로 설명 했습니다. 마지막으로, 해당 하는 방법을 살펴보았습니다 Apple TV 앱 스토어에 응용 프로그램을 제출 하려면 iTunes Connect 및 Xcode 보관 도구를 사용 합니다.


## <a name="related-links"></a>관련 링크

- [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md)
- [새 Apple TV 프로그램 앱 제출 준비](https://developer.apple.com/tvos/submit/)
- [앱 스토어 제출 팁](https://developer.apple.com/appstore/resources/submission/tips.html)
- [일반적인 앱 거부](https://developer.apple.com/app-store/review/rejections/)
- [앱 스토어 검토 지침](https://developer.apple.com/appstore/resources/approval/guidelines.html)
