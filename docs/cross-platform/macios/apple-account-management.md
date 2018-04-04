---
title: Apple 계정 관리
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/05/2017
ms.openlocfilehash: 21af0ef09644f39f9be42788b3d8f4977a2143d3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="apple-account-management"></a>Apple 계정 관리

Apple 계정 관리 인터페이스 id입니다. Apple와 관련 된 모든 개발 팀을 볼 수가 있습니다. 수의 목록을 표시 하 여 각 팀에 대 한 자세한 정보를 볼 수도 있습니다 _서명 Id_ 및 _프로 비전 프로필_ 컴퓨터에 설치 되어 있습니다.

Apple ID의 인증을 사용 하 여 명령줄에서 수행 [fastlane](https://fastlane.tools/)합니다. fastlane는 성공적으로 인증 될 수 있습니다에 대 한 컴퓨터에 설치 되어야 합니다. 자세한 fastlane 및 설치 하는 방법에 대 한 자세한 내용은에 [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) 가이드입니다.

Mac 용 Visual Studio에서 Apple 계정 대화 상자를 사용 하면 다음을 수행할 수 있습니다.

* **만들기 및 관리 인증서** 
* **만들기 및 프로 비전 프로필 관리** 

이 작업을 수행 하는 방법에 대 한 정보는이 가이드에 설명 되어 있습니다.

또한 다음을 수행할 iOS 번들 서명 도구를 사용할 수 있습니다.

* **기존 프로필을 새 서명 id를 추가 합니다.** 
* **새 장치를 프로 비전** 

이러한 기능을 사용 하 여에 대 한 자세한 내용은 참조는 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드입니다.
️
## <a name="requirements"></a>요구 사항

Apple 계정 관리를 Mac.에 대 한 Visual Studio에서 사용할 수 이 현재 사용할 수 없는 경우 Visual Studio for Windows에서

이 기능을 사용 하는 Apple 개발자 계정이 있어야 합니다. 자세한 내용은 Apple 개발자 계정에는에서 사용할 수는 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드입니다.

- 인터넷에 연결 되어 있어야 합니다. Apple 개발자 포털와 직접 통신 하며 fastlane 때문입니다.
- 해야 [fastlane 도구가 설치 되어](~/ios/deploy-test/provisioning/fastlane/index.md#Installation)합니다.
- 최신 fastlane 도구 해야 [ https://download.fastlane.tools ](https://download.fastlane.tools)합니다.
- 시작 하기 전에 확인에서 모든 사용자 사용권 계약에 동의 하는 [개발자 포털](https://developer.apple.com/account/)합니다.

## <a name="adding-an-apple-developer-account"></a>Apple 개발자 계정 추가

1. 로 이동 계정 관리 대화 상자를 열려면 **Visual Studio > 기본 설정 > Apple 개발자 계정**:

    ![Apple 개발자 계정 옵션](apple-account-management-images/image1.png)

2. 키를 눌러는 **+** 아래와 같이 로그인 대화 상자에서 표시 하려면 단추: 

    ![fastlane 대화 상자입니다.](apple-account-management-images/image2.png)

4. Apple ID와 암호를 입력 하 고 클릭는 **로그인** 단추입니다. 이 컴퓨터에 자격 증명 보안 키 집합에 저장 합니다. [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) 자격 증명을 안전 하 게 처리 하 고 Apple 개발자 포털에 전달 하는 데 사용 됩니다.
 
5. 선택 **항상 허용** 자격 증명을 사용 하도록 Visual Studio를 허용 하도록 경고 대화 상자에서:

    ![](apple-account-management-images/image4.png)

6. 계정을 성공적으로 추가 되 면 사용자의 Apple ID와 팀의 구성원 인 사용자의 Apple ID가 표시 됩니다.

    ![](apple-account-management-images/image5.png)

7. 모든 팀 및 키를 눌러 선택 된 **세부 정보 보기...** 클릭합니다. 그러면 모든 서명 Id 및 컴퓨터에 설치 된 프로비저닝 프로필의 목록이 표시 됩니다.

    ![](apple-account-management-images/image6.png)


<a name="managing" />


## <a name="managing-signing-identities-and-provisioning-profiles"></a>서명 Id를 관리 하 고 프로 비전 프로필

팀 세부 정보 대화 상자에는 유형별로 구성 된 서명 Id의 목록이 표시 됩니다. **상태** 열 라는 해당 인증서가 있습니다. 

* **유효한** – 서명 id (인증서와 개인 키)가 컴퓨터에 설치 되어 있고 만료 되지 않은 합니다.

* **키 집합에 없는** – Apple 서버에서 유효한 서명 id가 있습니다. 이 컴퓨터에 설치 하려면 다른 컴퓨터에서 내보낼 수 있어야 합니다. 개인 키 포함 되지 않습니다 Apple 개발자 포털에서 서명 id를 다운로드할 수 없습니다.

* **개인 키가 없는** – A 인증서 개인 키와 키 집합에 설치 됩니다.

* **만료** – 인증서가 만료 되었습니다. 이 키 체인에서 제거 해야 합니다.

  ![](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>서명 Id 만들기

새 서명 id를 만들려면 선택 된 **새 인증서 만들기** 드롭다운 단추 및 필요한 유형 선택 합니다. 새 서명 올바른 사용 권한이 있는 경우에 몇 초 후 identity가 나타납니다.

드롭다운 목록에서 옵션을 회색으로 표시 하 고 하지 않은 경우 아래 그림과 같이, 이러한 종류의 인증서를 만들 수 있는 올바른 팀 권한을 않았는지 의미 합니다.

![](apple-account-management-images/image8.png)

## <a name="download-provisioning-profiles"></a>프로 비전 프로필을 다운로드 합니다.

팀 세부 정보 대화 상자에는 개발자 계정에 연결 된 모든 프로 비전 프로필의 목록이 표시 됩니다. 키를 눌러 로컬 컴퓨터에 모든 프로 비전 프로필을 다운로드할 수 있습니다는 **모든 프로필을 다운로드** 단추

![](apple-account-management-images/image9.png)

## <a name="ios-bundle-signing"></a>iOS 번들 서명

장치에 앱 배포에 대 한 자세한 내용은 참조는 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드입니다.

## <a name="troubleshooting"></a>문제 해결

### <a name="view-details-dialog-is-empty"></a>자세히 보기 대화 상자 비어 있습니다.

이 버그와 관련 된 알려진된 문제로 현재 [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906)합니다. Mac에 대 한 안정적인 최신 버전의 Visual Studio를 사용 하 고 있는지 확인

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>계정에 로그인 하는 데 문제가 있는 경우 다음을 시도 하십시오.

* 키 집합 응용 프로그램을 열고 범주 아래 선택 *암호*합니다. 검색할 `deliver.`, 모든 항목을 삭제 합니다.

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"계정을 추가 하는 오류가 발생 했습니다. 앱 별 암호를 사용 하 여 로그인 하세요. "

계정에서 2 단계 인증을 사용 하는 때문입니다. Mac에 대 한 안정적인 최신 버전의 Visual Studio를 사용 하 고 있는지 확인

### <a name="failed-to-create-new-certificate"></a>새 인증서를 만들지 못했습니다.
"이러한 종류의 인증서에 대 한 제한을 도달 했습니다"

![](apple-account-management-images/image10.png)

허용 되는 인증서의 최대 수 생성 되었습니다. 이 해결 하려면로 이동 된 [Apple 개발자 센터](https://developer.apple.com/account/ios/certificate/distribution) 및 프로덕션 인증서 중 하나를 취소 합니다.

## <a name="known-issues"></a>알려진 문제

* 경우에 따라 세부 정보 보기 대화 상자는 엄청난 서명 id 및 프로필을 인출 하는 시간이 걸릴 수 있습니다.
* 종종 포커스 수를 반환 하지 Visual Studio Mac 용 일으키는를 추가할 계정 세부 정보를 입력 한 후. 이 경우 프로세스를 다시 시도 하십시오.
* Mac용 Visual Studio에서 만든 프로비전 프로필은 프로젝트에서 선택한 계정 자격(Entitlements.plist)을 고려하지 않습니다. 이 기능은 향후 버전의 IDE에 추가됩니다.
* 기본적으로 프로비전 프로필 배포는 앱 스토어를 대상으로 합니다. 하우스 또는 임시 프로필은 수동으로 만들어야 합니다.
