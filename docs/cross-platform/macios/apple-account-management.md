---
title: Apple 계정 관리
description: 이 문서에서는 Mac 및 Visual Studio 2019 용 Visual Studio에서 Apple 계정 관리 기능을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 8617d6e0c0930f581c45dbb461dfcb5d85a2becc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61206368"
---
# <a name="apple-account-management"></a>Apple 계정 관리

Apple 계정 관리 인터페이스 id입니다. Apple 연관 된 모든 개발 팀을 볼 수가 있습니다. 또한의 목록을 표시 하 여 각 팀에 대 한 자세한 정보를 볼 수 있습니다 _서명 Id_ 하 고 _프로 비전 프로필_ 컴퓨터에 설치 되어 있는 합니다.

Apple ID의 인증을 사용 하 여 명령줄에서 수행 됩니다 [fastlane](https://fastlane.tools/)합니다. fastlane은 정상적으로 인증할 수에 대 한 컴퓨터에 설치 되어야 합니다. 자세한 내용은 fastlane 다운로드 및 설치 하는 방법에 자세히 설명 되어는 [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) 가이드입니다.

Apple 계정 대화 상자를 사용 하면 다음을 수행할 수 있습니다.

* **인증서 만들기 및 관리**
* **만들기 및 프로 비전 프로필 관리**

이 작업을 수행 하는 방법에 대 한 정보는이 가이드에 설명 되어 있습니다.

> [!NOTE]
> Apple 계정 관리를 위해 Xamarin 도구만 유료 Apple 개발자 계정에 대 한 정보를 표시합니다. 유료 Apple 개발자 계정 사용 하지 않고 장치에서 앱을 테스트 하는 방법에 알아보려면 하세요 합니다 [Xamarin.iOS 앱에 대 한 무료 프로 비전](~/ios/get-started/installation/device-provisioning/free-provisioning.md) 가이드입니다.

또한 자동으로 만들고 서명 Id, 앱 Id 및 프로 비전 프로필을 관리 하려면 iOS 자동 프로 비전 도구를 사용할 수 있습니다. 이러한 기능을 사용 하 여에 대 한 자세한 내용은 참조는 [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) 가이드입니다.

## <a name="requirements"></a>요구 사항

Apple 계정 관리에서 Mac, Visual Studio 2019 및 Visual Studio 2017 (버전 15.7 이상) 용 Visual Studio에서 제공 됩니다.

이 기능을 사용 하는 Apple 개발자 계정이 있어야 합니다. 자세한 내용은 Apple 개발자 계정에서 사용할 수는 [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) 가이드입니다.

- 인터넷에 연결 되어 있어야 합니다. Fastlane은 Apple Developer 포털와 직접 통신 때문입니다.
- 했는지 [fastlane 도구 설치](~/ios/deploy-test/provisioning/fastlane/index.md#Installation)합니다.
- fastlane 도구를 최신 했는지 [ https://download.fastlane.tools ](https://download.fastlane.tools)합니다.
- 시작 하기 전에 확인에서 모든 사용자 사용권 계약에 동의 해야 합니다 [개발자 포털](https://developer.apple.com/account/)합니다.

## <a name="adding-an-apple-developer-account"></a>Apple 개발자 계정 추가

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 계정 관리 대화 상자를 열려면로 이동 **Visual Studio > 기본 설정 > Apple 개발자 계정**:

    ![Apple 개발자 계정 옵션](apple-account-management-images/image1.png)

2. 키를 눌러 합니다 **+** 아래와 같이 대화 상자에서 기호를 표시 하려면 단추: 

    ![fastlane 대화 상자입니다.](apple-account-management-images/image2.png)

4. Apple ID와 암호를 입력 하 고 클릭 합니다 **로그인** 단추입니다. 이 컴퓨터에 이렇게에 자격 증명 보안 키 집합에 저장 됩니다. [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) 자격 증명을 안전 하 게 처리 하 고 Apple 개발자 포털에 전달 하는 데 사용 됩니다.
 
5. 선택 **항상 허용** 자격 증명을 사용 하도록 Visual Studio 수 있도록 경고 대화 상자에서:

    ![경고 대화 상자를 항상 허용](apple-account-management-images/image4.png)

6. 계정이 성공적으로 추가 되 면 Apple ID 및 Apple ID의 일부인 모든 팀 표시 됩니다.

    ![계정 추가 사용 하 여 Apple 개발자 계정 대화 상자](apple-account-management-images/image5.png)

7. 모든 팀 및 키를 눌러 선택 된 **세부 정보 보기...** 클릭합니다. 이 모든 서명 Id 및 컴퓨터에 설치 된 프로 비전 프로필의 목록이 표시 됩니다.

    ![서명 id 및 프로 비전 프로필 컴퓨터 보기 세부 정보 화면 표시](apple-account-management-images/image6.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio 2019에 Apple ID 추가 시작 하기 전에 개발 환경 인지 확인 [Mac 빌드 호스트에 쌍을 이루는](~/ios/get-started/installation/windows/connecting-to-mac/index.md)합니다.

1. 로 계정 관리 창을 열려면 **도구 > 옵션 > Xamarin > Apple 계정**:

    ![Apple 계정 옵션 화면](apple-account-management-images/prov1.png)

1. 선택 된 **추가** 단추 및 Apple ID와 암호를 입력 합니다.

    ![사용자 이름 및 암호 대화 상자](apple-account-management-images/prov1a.png)

1. 계정이 성공적으로 추가 되 면 Apple ID 및 Apple ID의 일부인 모든 팀 표시 됩니다.
 
1. 모든 팀 및 키를 눌러 선택 된 **세부 정보 보기...** 클릭합니다. 이 모든 서명 Id 및 컴퓨터에 설치 된 프로 비전 프로필의 목록이 표시 됩니다.

    ![사용자 이름 및 암호 대화 상자](apple-account-management-images/prov2.png)

-----


## <a name="managing-signing-identities-and-provisioning-profiles"></a>서명 Id를 관리 및 프로 비전 프로필

팀 정보 대화 상자에는 서명 Id를 유형별로 구성의 목록을 표시 합니다. 합니다 **상태** 열 라는 인증서가 하는 경우: 

* **유효한** – 서명 id (인증서 및 개인 키)는 컴퓨터에 설치 되 고 만료 되지 않았는지입니다.

* **키 집합에 없는** – Apple 서버에서 유효한 서명 id를 있습니다. 이 컴퓨터에 설치 하려면 다른 컴퓨터에서 내보낼 수 있어야 합니다. 개인 키 포함 되어 있지는 Apple Developer 포털에서 서명 id를 다운로드할 수 없습니다.

* **개인 키가 없습니다.** – 개인 키를 사용 하 여는 인증서가 keychain에 설치 합니다.

* **만료** –은 인증서가 만료 되었습니다. 키 집합에서이 제거 해야 합니다.

  ![팀 정보 대화 상자 정보](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>서명 Id 만들기

새 서명 id를 만들려면 선택 합니다 **Create Certificate** 드롭다운 단추 및 필요한 유형 선택 합니다. 새 서명 권한이 있는 경우에 몇 초 후 identity가 나타납니다.

옵션 드롭다운 목록에서 회색을 선택 하지 않은 경우이 유형의 인증서를 만드는 데 올바른 팀 권한 없는 것을 의미 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![인증서 옵션 만들기](apple-account-management-images/image8.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![인증서 옵션 만들기](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>프로 비전 프로필 다운로드

팀 정보 대화 상자는 또한 개발자 계정에 연결 하는 모든 프로 비전 프로필의 목록을 표시 합니다. 키를 눌러 로컬 컴퓨터에 모든 프로 비전 프로필을 다운로드할 수 있습니다 합니다 **모든 프로필 다운로드** 단추

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![프로 비전 프로필 섹션 다운로드](apple-account-management-images/image9.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![프로 비전 프로필 섹션 다운로드](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>iOS 번들 서명

장치에 앱 배포에 대 한 자세한 내용은 참조는 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드입니다.

## <a name="troubleshooting"></a>문제 해결

### <a name="view-details-dialog-is-empty"></a>자세히 보기 대화 상자 비어 있습니다.

이 현재 버그와 관련 된 알려진된 문제 [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906)합니다. Mac 용 Visual Studio의 안정적인 최신 버전 사용 중인지 확인

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>계정에 로그인 하는 문제가 발생 하는 경우 다음을 시도 하세요.

* 키 집합 응용 프로그램을 열고 범주 아래에서 선택 *암호*합니다. 검색할 `deliver.`, 모든 항목을 삭제 합니다.

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"계정 추가 오류입니다. 앱 별 암호를 "로그인 하세요

2 단계 인증을 사용 하 여 계정의 때문입니다. Mac 용 Visual Studio의 안정적인 최신 버전 사용 중인지 확인

### <a name="failed-to-create-new-certificate"></a>새 인증서를 만들지 못했습니다.
"이 유형의 인증서에 대 한 제한을 도달 했습니다"

![인증서 한도 대화 상자](apple-account-management-images/image10.png)

최대 허용 되는 인증서 생성 되었습니다. 이 문제를 해결 하려면로 이동 합니다 [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution) 및 프로덕션 인증서 중 하나를 취소 합니다.

## <a name="known-issues"></a>알려진 문제

* 기본적으로 프로비전 프로필 배포는 앱 스토어를 대상으로 합니다. 하우스 또는 임시 프로필은 수동으로 만들어야 합니다.
