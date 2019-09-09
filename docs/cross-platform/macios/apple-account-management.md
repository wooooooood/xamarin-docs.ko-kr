---
title: Apple 계정 관리
description: 이 문서에서는 Mac용 Visual Studio 및 Visual Studio 2019에서 Apple 계정 관리 기능을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: conceptdev
ms.author: crdun
ms.date: 05/06/2018
ms.openlocfilehash: 65945a303375863f7b92b20405aa78e6b2edacda
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766131"
---
# <a name="apple-account-management"></a>Apple 계정 관리

Apple 계정 관리 인터페이스는 Apple ID와 연결 된 모든 개발 팀을 볼 수 있는 방법을 제공 합니다. 또한 컴퓨터에 설치 된 _서명 id_ 및 _프로 비전 프로필_ 의 목록을 표시 하 여 각 팀에 대 한 자세한 정보를 볼 수 있습니다.

Apple ID 인증은 [fastlane](https://fastlane.tools/)을 사용 하는 명령줄에서 수행 됩니다. 성공적으로 인증 하려면 fastlane이 컴퓨터에 설치 되어 있어야 합니다. Fastlane 및 설치 방법에 대 한 자세한 내용은 [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) 가이드에 자세히 설명 되어 있습니다.

Apple 계정 대화 상자에서 다음을 수행할 수 있습니다.

- **인증서 만들기 및 관리**
- **프로 비전 프로필 만들기 및 관리**

이 작업을 수행 하는 방법에 대 한 자세한 내용은이 가이드에 설명 되어 있습니다.

> [!NOTE]
> Apple 계정 관리용 Xamarin 도구는 유료 Apple 개발자 계정에 대 한 정보만 표시 합니다. 유료 Apple 개발자 계정 없이 장치에서 앱을 테스트 하는 방법을 알아보려면 [xamarin.ios 앱에 대 한 무료 프로 비전](~/ios/get-started/installation/device-provisioning/free-provisioning.md) 가이드를 참조 하세요.

IOS 자동 프로 비전 도구를 사용 하 여 서명 Id, 앱 Id 및 프로 비전 프로필을 자동으로 만들고 관리할 수도 있습니다. 이러한 기능을 사용 하는 방법에 대 한 자세한 내용은 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드를 참조 하세요.

## <a name="requirements"></a>요구 사항

Apple 계정 관리는 Mac용 Visual Studio, Visual Studio 2019 및 Visual Studio 2017 (버전 15.7 이상)에서 사용할 수 있습니다.

이 기능을 사용 하려면 Apple Developer 계정이 있어야 합니다. Apple 개발자 계정에 대 한 자세한 내용은 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드에서 확인할 수 있습니다.

- 인터넷에 연결 되어 있는지 확인 합니다. Fastlane은 Apple 개발자 포털과 직접 통신 하기 때문입니다.
- [Fastlane 도구가 설치](~/ios/deploy-test/provisioning/fastlane/index.md#Installation)되어 있는지 확인 합니다.
- 에서 [https://download.fastlane.tools](https://download.fastlane.tools)최신 fastlane 도구를 사용할 수 있는지 확인 합니다.
- 시작 하기 전에 [개발자 포털](https://developer.apple.com/account/)에서 사용자 사용권 계약을 수락 해야 합니다.

## <a name="adding-an-apple-developer-account"></a>Apple 개발자 계정 추가

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 계정 관리 대화 상자를 열려면 **Apple Developer 계정 > Visual Studio > 기본 설정**으로 이동 합니다.

    ![Apple 개발자 계정 옵션](apple-account-management-images/image1.png)

2. 아래와 같이 단추를 클릭 하 여 로그인 대화 상자를 표시 합니다. **+** 

    ![fastlane 대화 상자.](apple-account-management-images/image2.png)

3. Apple ID와 암호를 입력 하 고 **로그인** 단추를 클릭 합니다. 그러면이 컴퓨터의 보안 키 집합에 자격 증명이 저장 됩니다. [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) 은 자격 증명을 안전 하 게 처리 하 고 Apple의 개발자 포털에 전달 하는 데 사용 됩니다.

4. 경고 대화 상자에서 **항상 허용** 을 선택 하 여 Visual Studio에서 자격 증명을 사용할 수 있도록 합니다.

    ![경고 항상 허용 대화 상자](apple-account-management-images/image4.png)

5. 계정이 성공적으로 추가 되 면 apple id 및 Apple ID가 포함 된 모든 팀이 표시 됩니다.

    ![계정이 추가 된 Apple 개발자 계정 대화 상자](apple-account-management-images/image5.png)

6. 팀을 선택 하 고 **세부 정보 보기** ...를 누릅니다. 단추를 선택합니다. 컴퓨터에 설치 된 모든 서명 Id 및 프로 비전 프로필의 목록이 표시 됩니다.

    ![컴퓨터에서 서명 id 및 프로 비전 프로필을 보여 주는 세부 정보 화면 보기](apple-account-management-images/image6.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Apple ID를 Visual Studio 2019에 추가 하기 전에 개발 환경이 [Mac 빌드 호스트](~/ios/get-started/installation/windows/connecting-to-mac/index.md)와 연결 되어 있는지 확인 합니다.

1. 계정 관리 창을 열려면 **Xamarin > Apple 계정 > 도구 > 옵션**으로 이동 합니다.

    ![Apple 계정 옵션 화면](apple-account-management-images/prov1.png)

1. **추가** 단추를 선택 하 고 Apple ID 및 암호를 입력 합니다.

    ![사용자 이름 및 암호 대화 상자](apple-account-management-images/prov1a.png)

1. 계정이 성공적으로 추가 되 면 apple id 및 Apple ID가 포함 된 모든 팀이 표시 됩니다.

1. 팀을 선택 하 고 **세부 정보 보기** ...를 누릅니다. 단추를 선택합니다. 컴퓨터에 설치 된 모든 서명 Id 및 프로 비전 프로필의 목록이 표시 됩니다.

    ![사용자 이름 및 암호 대화 상자](apple-account-management-images/prov2.png)

-----

## <a name="managing-signing-identities-and-provisioning-profiles"></a>서명 Id 및 프로 비전 프로필 관리

팀 세부 정보 대화 상자에는 유형별로 구성 된 서명 Id의 목록이 표시 됩니다. **상태** 열은 다음과 같은 인증서 인지를 알려줍니다. 

- **유효함** – 서명 id (인증서와 개인 키 모두)가 컴퓨터에 설치 되어 있고 만료 되지 않았습니다.

- 키 **집합에 없음** – Apple 서버에 유효한 서명 id가 있습니다. 이를 컴퓨터에 설치 하려면 다른 컴퓨터에서 내보내야 합니다. 개인 키가 포함 되지 않으므로 Apple 개발자 포털에서 서명 id를 다운로드할 수 없습니다.

- **개인 키가 누락 됨** – 개인 키가 없는 인증서가 키 집합에 설치 되어 있습니다.

- **만료** 됨 – 인증서가 만료 됩니다. 키 집합에서이를 제거 해야 합니다.

  ![팀 세부 정보 대화 상자 정보](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>서명 Id 만들기

새 서명 id를 만들려면 **인증서 만들기** 드롭다운 단추를 선택 하 고 필요한 유형을 선택 합니다. 올바른 권한이 있는 경우 몇 초 후에 새 서명 id가 표시 됩니다.

드롭다운의 옵션이 회색으로 표시 되 고 선택 취소 된 경우이 유형의 인증서를 만들 수 있는 올바른 팀 권한이 없다는 것입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![인증서 옵션 만들기](apple-account-management-images/image8.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![인증서 옵션 만들기](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>프로 비전 프로필 다운로드

팀 세부 정보 대화 상자에는 개발자 계정에 연결 된 모든 프로 비전 프로필의 목록도 표시 됩니다. **모든 프로필 다운로드** 단추를 눌러 모든 프로 비전 프로필을 로컬 컴퓨터에 다운로드할 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![프로 비전 프로필 섹션 다운로드](apple-account-management-images/image9.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![프로 비전 프로필 섹션 다운로드](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>iOS 번들 서명

장치에 앱을 배포 하는 방법에 대 한 자세한 내용은 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드를 참조 하세요.

## <a name="troubleshooting"></a>문제 해결

### <a name="view-details-dialog-is-empty"></a>자세히 보기 대화 상자가 비어 있습니다.

이 문제는 현재 버그 [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906)와 관련 된 알려진 문제입니다. 안정적인 최신 버전을 사용 하 고 있는지 확인 Mac용 Visual Studio

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>계정에 로그인 하는 동안 문제가 발생 하는 경우 다음을 시도 하세요.

- 키 집합 응용 프로그램을 열고 범주 아래에서 *암호*를 선택 합니다. `deliver.`모든 항목을 검색 하 고 삭제 합니다.

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"계정을 추가 하는 동안 오류가 발생 했습니다. 앱 특정 암호를 사용 하 여 로그인 하세요. "

계정에 2 단계 인증이 사용 되도록 설정 되어 있기 때문입니다. 안정적인 최신 버전을 사용 하 고 있는지 확인 Mac용 Visual Studio

### <a name="failed-to-create-new-certificate"></a>새 인증서를 만들지 못했습니다.
"이 유형의 인증서에 대 한 제한에 도달 했습니다."

![인증서 제한 대화 상자](apple-account-management-images/image10.png)

허용 되는 최대 인증서 수가 생성 되었습니다. 이 문제를 해결 하려면 [Apple 개발자 센터](https://developer.apple.com/account/ios/certificate/distribution) 로 이동 하 여 프로덕션 인증서 중 하나를 해지 합니다.

## <a name="known-issues"></a>알려진 문제

- 기본적으로 프로비전 프로필 배포는 앱 스토어를 대상으로 합니다. 하우스 또는 임시 프로필은 수동으로 만들어야 합니다.
