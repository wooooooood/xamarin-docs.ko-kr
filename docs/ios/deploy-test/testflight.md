---
title: TestFlight를 사용하여 Xamarin.iOS 앱 배포
description: TestFlight는 현재 Apple에서 소유하고 있으며, Xamarin.iOS 앱을 베타 테스트하는 기본 방법입니다. 이 문서에서는 앱 업로드부터 iTunes Connect 사용에 이르기까지 TestFlight 프로세스의 모든 단계를 안내합니다.
ms.prod: xamarin
ms.assetid: BA880768-2BC8-41E4-B57E-A56F8EED4690
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 87be250bdc425558a8e386a8209596e18f13b3ed
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120523"
---
# <a name="using-testflight-to-distribute-xamarinios-apps"></a>TestFlight를 사용하여 Xamarin.iOS 앱 배포

_TestFlight는 현재 Apple에서 소유하고 있으며, Xamarin.iOS 앱을 베타 테스트하는 기본 방법입니다. 이 문서에서는 앱 업로드부터 iTunes Connect 사용에 이르기까지 TestFlight 프로세스의 모든 단계를 안내합니다._

베타 테스트는 소프트웨어 개발 주기의 핵심 부분이며, 이 프로세스를 간소화하기 위해 제공되는 다양한 플랫폼 간 응용 프로그램(예: [HockeyApp](http://hockeyapp.net/features/), [Applause](http://www.applause.com/mobile-app-testing) 및 Google Play의 Android 앱용 네이티브 앱 베타 테스트)이 있습니다. 이 문서는 Apple의 TestFlight에 중점을 둡니다.

TestFlight는 Apple의 iOS 앱용 베타 테스트 서비스이며, [iTunes Connect](https://itunesconnect.apple.com/)를 통해서만 액세스할 수 있습니다. 현재 iOS 8.0 이상 앱에서 사용할 수 있습니다. TestFlight는 내부 및 외부 사용자 모두와 함께 베타 테스트를 수행할 수 있으며, 외부 사용자에 대한 베타 앱 검토로 인해 앱 스토어에 게시할 때 최종 검토에서 훨씬 쉽게 처리할 수 있습니다.


이전에는 이진 파일이 Mac용 Visual Studio 내에서 생성되고, 테스터에게 배포하기 위해 TestFlightApp 웹 사이트에 업로드되었습니다. 새 프로세스에는 앱 스토어에서 완전하게 테스트된 고품질 응용 프로그램을 사용할 수 있도록 하는 여러 가지 향상된 기능이 있습니다. 예:


- 외부 테스트에 필요한 베타 앱 검토는 최종 앱 스토어 검토에 대한 성공 가능성을 높여주지만, 두 검토 모두 Apple의 지침을 준수해야 합니다.
- 업로드하기 전에 앱을 iTunes Connect에 등록해야 합니다. 이렇게 하면 프로비전 프로필, 이름 및 인증서 간에 불일치가 발생하지 않습니다.
- TestFlight 앱은 이제 실제의 iOS 앱이므로 더 빨리 작동합니다.
- 베타 테스트가 완료되면 검토를 위해 앱을 이동하는 프로세스는 단추 하나만 클릭하므로 빠르고 효율적입니다. 합니다.

## <a name="requirements"></a>요구 사항

iOS 8.0 이상인 앱만 TestFlight를 통해 테스트할 수 있습니다.

모든 테스터는 적어도 iOS 8 디바이스에서 앱을 테스트해야 합니다. 그러나 모범 사례에서는 모든 iOS 버전에서 앱을 테스트해야 한다고 지적하고 있습니다.

## <a name="provisioning"></a>프로비전

TestFlight를 사용하여 빌드를 테스트하려면 새 베타 자격으로 *앱 스토어 배포 프로필*을 만들어야 합니다. 이 자격은 TestFlight를 통한 베타 테스트를 허용하며, **새** 앱 스토어 배포 프로필에 자동으로 포함됩니다. [배포 프로필 만들기](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) 가이드의 단계별 지침에 따라 새 프로필을 생성할 수 있습니다.

아래 그림과 같이 [Xcode에서 빌드의 유효성을 검사](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)할 때 배포 프로필에 베타 자격이 포함되어 있는지 확인할 수 있습니다.

[![](testflight-images/validate-build.png "Apple에 앱 제출")](testflight-images/validate-build.png#lightbox)


## <a name="testflight-workflow"></a>TestFlight 워크플로

다음 워크플로에서는 앱의 베타 테스트를 위해 TestFlight 사용을 시작하는 데 필요한 단계를 설명합니다.

1. 새 앱의 경우 [iTunes Connect 레코드](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)를 만듭니다.
2. 응용 프로그램을 iTunes Connect에 [보관 및 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)합니다.
3. 베타 테스트를 관리합니다.
    - 메타데이터 추가
    - 내부 사용자 추가
        - 최대 25명의 사용자
    - 외부 사용자 추가
        - 최대 1,000명의 사용자
        - Apple 지침을 준수해야 하는 베타 테스트 검토가 필요합니다.
4. 사용자로부터 피드백을 받고, 이에 따라 작업을 수행하고, 2단계로 돌아갑니다.

## <a name="create-an-itunes-connect-record"></a>iTunes Connect 레코드 만들기

1.  Apple 개발자 자격 증명을 사용하여 [iTunes Connect 포털](https://itunesconnect.apple.com/)에 로그인합니다.
2.  **My Apps**를 선택합니다.

    [![](testflight-images/my-apps.png "내 앱 선택")](testflight-images/my-apps.png#lightbox)


3.  **My Apps** 화면의 왼쪽 위 모서리에 있는 **+** 단추를 클릭하여 새 앱을 추가합니다. Mac 및 iOS 개발자 계정이 있는 경우 여기서 새 앱 유형을 선택하라는 메시지가 표시됩니다.

앱의 Info.plist와 정확히 동일한 정보를 포함해야 하는 **새 iOS 앱** 제출 창이 표시됩니다.

새 iTunes Connect 레코드를 만드는 방법에 대한 자세한 내용은 [iTunes Connect 레코드 만들기](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) 가이드를 참조하세요.



###  <a name="completing-the-new-ios-app-submission-form"></a>새 iOS 앱 제출 양식 완성

양식은 아래 그림과 같이 앱의 Info.plist 파일에 있는 정보를 정확히 반영해야 합니다.

[![](testflight-images/infoplist.png "앱의 Info.plist")](testflight-images/infoplist.png#lightbox)
[ ![](testflight-images/newiosapp.png "iTunes Connect의 양식")](testflight-images/newiosapp.png#lightbox)

-  **이름** - 앱 번들을 설정할 때 사용되는 설명이 포함된 이름입니다. `Info.plist`의 **응용 프로그램 이름** 항목과 정확히 일치해야 합니다.
-  **기본 언어** - 앱 내에서 사용되는 기본 언어입니다. 일반적으로 말하는 모든 언어입니다.
-  **번들 ID** - 개발자 계정에 만들어진 모든 앱 ID를 나열하는 드롭다운 메뉴입니다.
    *   **번들 ID 접미사** - 와일드카드 번들 ID(예: 위의 예제와 같이 *로 끝남)를 선택하면 번들 ID 접미사를 묻는 추가 상자가 표시됩니다. 예제에서 **번들 ID**는 `mobi.chkn.*`이고, 접미사는 **PageView**입니다. 이러한 항목 모두는 `Info.plist`에서 **번들 식별자**를 구성합니다.
-  **버전** - 업로드되는 앱의 버전 번호입니다. 이 항목은 개발자가 선택합니다.
-  **SKU** - 사용자에게 표시되지 않는 앱의 고유 ID입니다. 제품 ID와 비슷한 방식으로 생각할 수 있습니다. 위의 예제에서는 해당 날짜에 대한 버전 번호와 함께 날짜를 선택했습니다.


## <a name="upload-your-app"></a>앱 업로드

iTunes Connect 레코드가 만들어지면 새 빌드를 업로드할 수 있습니다. 빌드에는 새 베타 자격이 있어야 합니다.

먼저 IDE에서 [최종 배포 가능한 파일](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)을 빌드한 다음, 응용 프로그램 로더 또는 Xcode의 보관 기능을 통해 [Apple에 앱을 제출](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

###  <a name="create-an-archive"></a>보관 만들기

 Mac용 Visual Studio에서 이진 파일을 빌드하려면 _보관_ 기능을 사용해야 합니다. 프로젝트를 마우스 오른쪽 단추로 클릭하고 아래 그림과 같이 **게시를 위해 보관**을 선택합니다.

 [![](testflight-images/new-archive.png "게시를 위해 보관 선택")](testflight-images/new-archive.png#lightbox)


 자세한 내용은 [배포 가능한 파일 빌드](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) 가이드를 참조하세요.

###  <a name="sign-and-distribute-your-app"></a>앱 서명 및 배포

 보관을 만들면 **보관 보기**가 자동으로 열리고, 보관된 모든 프로젝트가 솔루션별로 그룹화되어 표시됩니다. 앱에 서명하고 배포할 준비를 하려면 아래 그림과 같이 **서명 및 배포...** 를 선택합니다.

[![](testflight-images/archive-view.png "보관을 만들면 자동으로 열리는 보관 보기")](testflight-images/archive-view.png#lightbox)

 그러면 게시 마법사가 열립니다. **앱 스토어** 배포 채널을 선택하여 패키지를 만들고 응용 프로그램 로더를 엽니다. [프로비전 프로필] 화면에서 서명 ID 및 프로비전 프로필을 선택하거나 다른 ID로 다시 서명합니다. 패키지 세부 정보를 확인하고 **게시**를 클릭하여 `.ipa`를 저장합니다.

[![](testflight-images/group.png "서명 ID 및 프로비전 프로필을 선택하거나 다른 ID로 다시 서명")](testflight-images/group.png#lightbox)

 이러한 단계에 대한 자세한 내용은 [Apple에 앱 제출](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) 섹션을 참조하세요.

###  <a name="submitting-your-build"></a>빌드 제출
 게시 마법사에서 응용 프로그램 로더 프로그램을 열어 모든 사용자가 iTunes Connect에 빌드를 업로드합니다. **앱 배달** 옵션을 선택하고 위에서 만든 `.ipa` 파일을 업로드합니다. 응용 프로그램 로더에서 빌드의 유효성을 검사하고 이 빌드를 iTunes Connect에 업로드합니다.

 이러한 단계에 대한 자세한 내용은 [Apple에 앱 제출](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) 섹션을 참조하세요.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

###  <a name="building-your-final-distributable"></a>최종 배포 가능한 파일 빌드
 Visual Studio용 Xamarin 플러그 인은 앱 스토어에 게시하기 위해 Xamarin.iOS 앱을 보관하는 것을 지원하지 않으므로, Visual Studio에서 iOS 응용 프로그램을 게시하는 두 가지 옵션이 있습니다. 이러한 항목은 다음과 같습니다.

1. 임시 IPA 빌드 명령을 통해 만든 IPA를 업로드합니다.
1. 압축된 `.app` 번들을 업로드합니다.

 [배포 가능한 파일 빌드](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) 가이드에는 이러한 두 옵션에 대한 지침이 있습니다.

###  <a name="submitting-your-build"></a>빌드 제출
 앱을 Apple에 제출하려면 빌드 호스트로 이동하고 Xcode의 일부로 설치된 응용 프로그램 로더 프로그램을 사용해야 합니다. 응용 프로그램 로더 액세스에 대한 자세한 내용은 Apple의 [응용 프로그램 로더 액세스](http://help.apple.com/itc/apploader/#/apdATD1E927-D1E1A1303-D1E927A1126) 가이드를 참조하세요.

일단 열리면 **앱 배달** 옵션을 선택하고 위에 만든 zip 또는 `.ipa` 파일을 업로드합니다. 응용 프로그램 로더에서 빌드의 유효성을 검사하고 이 빌드를 iTunes Connect에 업로드합니다.

 이러한 단계에 대한 자세한 내용은 [Apple에 앱 제출](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) 섹션을 참조하세요.

-----


[앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) 가이드에서는 위의 모든 단계를 자세히 설명합니다. 앱 스토어 제출 프로세스에 대한 자세한 내용은 이 문서를 참조하세요.

iTunes Connect의 **My Apps** 섹션으로 돌아가면 응용 프로그램이 성공적으로 업로드되었음을 알 수 있습니다. 이제 몇 가지 베타 테스트를 수행할 준비가 되었습니다!

## <a name="manage-beta-testing"></a>베타 테스트 관리

### <a name="add-metadata"></a>메타데이터 추가

TestFlight를 사용하여 시작하려면 앱의 **시험판** 탭으로 이동합니다. 아래 그림과 같이 빌드, 내부 테스터 및 외부 테스터의 목록을 보여 주는 세 개의 탭이 표시됩니다.

[![](testflight-images/app-uploaded.png "빌드, 내부 테스터 및 외부 테스터 탭")](testflight-images/app-uploaded.png#lightbox)

메타데이터를 앱에 추가하려면 빌드 번호, TestFlight를 차례로 클릭합니다.

[![](testflight-images/metadata.png "메타데이터 추가")](testflight-images/metadata.png#lightbox)

**테스트 정보** 아래에서 테스터에게 앱과 관련된 중요한 정보를 제공할 수 있습니다. 예를 들면 다음과 같습니다.

- 테스트 항목
- 앱에 대한 설명
- 마케팅 URL - 추가하는 앱에 대한 정보를 제공하는 URL입니다.
- 개인 정보 처리 방침 URL - 회사의 개인 정보 처리 방침에 대한 정보를 제공하는 URL입니다.
- 사용자 의견 이메일

이 메타데이터는 내부 테스터에게 **필요하지 않지만**, 외부 테스터에게는 **필요합니다**.

<a name="beta-testing" />

### <a name="enable-beta-testing"></a>베타 테스트 사용

앱 테스트를 시작할 준비가 되면 버전에 대한 **TestFlight 베타 테스트** 스위치를 켭니다.

[![](testflight-images/turn-on-testing.png "TestFlight 베타 테스트 스위치 켜기")](testflight-images/turn-on-testing.png#lightbox)

각 빌드는 TestFlight 베타 스위치를 켠 날짜로부터 **60일** 동안 활성화됩니다. **테스트 정보** 페이지에서 각 빌드에 남아 있는 일 수를 확인할 수 있습니다.

[![](testflight-images/daysleft.png "테스트 정보 페이지")](testflight-images/daysleft.png#lightbox)

테스트는 언제든지 해제할 수 있습니다.

### <a name="internal-testers"></a>내부 테스터

내부 테스터는 iTunes Connect에서 다음 역할 중 하나가 할당된 개발 팀의 구성원입니다.

-  **관리자** – 관리자는 iTunes Connect에서 새 사용자를 추가하고 관리할 책임이 있습니다.
-  **법적** – 팀 에이전트는 법적 역할을 할당받는 유일한 관리 사용자입니다. 법적 계약에 서명할 수 있습니다.
-  **기술** – 기술 사용자는 앱과 관련된 대부분의 속성을 변경할 수 있습니다. 예를 들어 앱 정보를 수정하고, 이진 파일을 업로드한 다음, 검토를 위해 앱을 보냅니다.

각 빌드는 최대 25명의 구성원과 공유할 수 있습니다.

테스터를 추가하려면 iTunes Connect 주 화면에서 **사용자 및 역할**로 이동합니다.

[![](testflight-images/users-and-roles.png "iTunes Connect 주 화면의 사용자 및 역할")](testflight-images/users-and-roles.png#lightbox)

기존 iTunes Connect 사용자가 목록에 표시됩니다. 이러한 사용자를 선택하려면 이름을 클릭하고, **내부 테스터** 스위치를 켜고, **저장**을 클릭합니다.

[![](testflight-images/internal-tester.png "내부 테스터 스위치 켜기")](testflight-images/internal-tester.png#lightbox)

목록에 없는 사용자를 추가하려면 *사용자* 옆에 있는 **+** 단추를 선택하고, 이름, 성 및 이메일 주소를 제공하여 계정을 만듭니다. 사용자는 이메일을 확인하여 계정을 활성화해야 합니다.

[![](testflight-images/add-new-user.png "사용자 추가")](testflight-images/add-new-user.png#lightbox)

**My Apps > 시험판 > 내부 테스터**로 돌아가면 TestFlight 내부 베타 테스트를 위해 추가된 사용자가 표시됩니다.

[![](testflight-images/select-users.png "TestFlight 내부 베타 테스트를 위해 추가된 사용자 목록")](testflight-images/select-users.png#lightbox)

이름을 선택하고 **초대** 단추를 클릭하여 이러한 테스터를 초대할 수 있습니다. 이러한 테스터는 앱을 테스트하기 위한 초대가 포함된 이메일을 받게 됩니다.

내부 테스터 페이지의 상태 열에서 초대 상태를 확인할 수 있습니다.

[![](testflight-images/status-added.png "초대 상태")](testflight-images/status-added.png#lightbox)


### <a name="external-testers"></a>외부 테스터

외부 테스터를 앱 베타 테스트에 초대하기 전에 베타 앱 검토를 거쳐야 하므로 [앱 스토어 검토 지침](https://developer.apple.com/app-store/review/guidelines/)을 준수해야 합니다.

검토를 위해 앱을 제출하려면 아래 그림과 같이 빌드 옆에 있는 **베타 앱 검토 제출** 텍스트를 클릭합니다.

[![](testflight-images/beta-app-review.png "베타 앱 검토 제출")](testflight-images/beta-app-review.png#lightbox)

앱에서 검토를 통과하려면 TestFlight 베타 정보 페이지에 필요한 모든 메타데이터를 입력해야 합니다.

이제 초대 준비를 시작하고, [외부 테스터] 탭을 통해 아래 스크린샷과 같이 이메일, 이름 및 성을 입력하여 최대 2,000명의 외부 테스터를 추가할 수 있습니다. 입력하는 이메일은 Apple ID일 필요는 없습니다. 이는 초대를 받을 이메일일 뿐입니다.

[![](testflight-images/add-external.png "테스터 초대")](testflight-images/add-external.png#lightbox)

외부 테스터가 많은 경우 **파일 가져오기** 링크를 사용하여 줄마다 다음과 같은 형식으로 채워진 `CSV` 파일을 가져올 수 있습니다.

``` 
first name, last name, email address
```

또한 테스터를 체계적으로 유지할 수 있도록 외부 테스터를 여러 그룹에 추가할 수도 있습니다.

외부 테스터의 세부 정보가 입력되었으면 **추가**를 클릭하고 사용자가 초대에 동의했는지 확인합니다.

[![](testflight-images/confirm-consent.png "사용자가 초대에 동의했는지 확인")](testflight-images/confirm-consent.png#lightbox)

베타 앱이 성공적으로 검토되면 외부 테스터에게 초대를 보낼 수 있습니다. 이 시점에서 빌드 페이지의 **외부** 아래에 있는 텍스트가 **초대 보내기**로 변경됩니다. 이 항목을 클릭하여 이미 추가한 모든 테스터에게 초대를 보냅니다.

앱이 거부되면 **Resolution Center**에 표시된 문제를 해결하고, 검토를 위해 업데이트된 이진 파일 전체를 다시 제출해야 합니다.

## <a name="as-a-beta-tester"></a>베타 테스터로서

초대된 테스터는 아래 스크린샷과 비슷한 이메일을 받습니다.

[![](testflight-images/tester-email.png "초대 이메일 예제")](testflight-images/tester-email.png#lightbox)

**TestFlight에서 열기** 단추를 클릭하면 TestFlight 응용 프로그램에서 앱이 열리거나, 아직 다운로드되지 않은 경우 앱 스토어로 이동하여 다운로드할 수 있습니다.

앱이 TestFlight에서 열리면 테스트할 항목에 대한 세부 정보가 표시되고, 테스터가 iOS 8.0 이상 디바이스에 응용 프로그램을 설치하도록 요구하는 메시지가 표시됩니다.

[![](testflight-images/install-app.png "테스트할 항목에 대한 세부 정보를 보여주는 TestFlight")](testflight-images/install-app.png#lightbox)

테스트 빌드는 디바이스의 홈 화면에서 응용 프로그램 이름 앞에 주황색 점으로 표시됩니다.

테스터는 TestFlight 앱을 통해 피드백을 제공할 수 있으며, 이 정보는 메타데이터에 제공된 이메일 주소에서 완화됩니다.

### <a name="beta-testing-complete"></a>베타 테스트 완료

베타 테스트가 완료되면 이제 Apple에 의한 앱 스토어 검토를 위해 앱을 제출할 수 있습니다. 이 프로세스는 아래 그림과 같이 **검토를 위해 제출** 단추를 클릭하여 iTunes Connect에서 매우 간단하게 수행됩니다.

[![](testflight-images/submit-for-review.png "검토를 위해 제출 단추 클릭")](testflight-images/submit-for-review.png#lightbox)

## <a name="summary"></a>요약

이 문서에서는 iTunes Connect를 통해 Apple의 TestFlight 베타 테스트를 사용하는 방법을 살펴보았습니다. 새 빌드를 iTunes Connect에 업로드하는 방법 및 앱을 사용하도록 내부 및 외부 베타 테스터를 초대하는 방법을 설명했습니다.

## <a name="related-links"></a>관련 링크

- [iTunes Connect 레코드 만들기](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [앱 스토어 배포를 위한 앱 프로비전](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#provisioning)
- [Apple TestFlight 베타 사용](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/BetaTestingTheApp.html#//apple_ref/doc/uid/TP40011225-CH35-SW2)
