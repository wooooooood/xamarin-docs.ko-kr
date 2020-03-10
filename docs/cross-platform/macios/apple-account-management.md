---
title: Apple 계정 관리
description: 이 문서에서는 Mac용 Visual Studio 및 Visual Studio 2019에서 Apple 계정 관리 기능을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: davidortinau
ms.author: daortin
ms.date: 03/05/2020
ms.openlocfilehash: 17607e09a141fd29cd81cde93d812b20e62a9af8
ms.sourcegitcommit: 60d2243809d8e980fca90b9f771e72f8c0e64d71
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "78946240"
---
# <a name="apple-account-management"></a>Apple 계정 관리

Visual Studio의 Apple 계정 관리 인터페이스는 Apple ID와 연결 된 개발 팀에 대 한 정보를 볼 수 있는 방법을 제공 합니다. 이를 통해 다음을 수행할 수 있습니다.

- **Apple developer 계정 추가**
- **서명 인증서 및 프로 비전 프로필 보기**
- **새 서명 인증서 만들기**
- **기존 프로 비전 프로필 다운로드**

> [!IMPORTANT]
> Apple 계정 관리용 Xamarin 도구는 유료 Apple 개발자 계정에 대 한 정보만 표시 합니다. 유료 Apple 개발자 계정 없이 장치에서 앱을 테스트 하는 방법을 알아보려면 [xamarin.ios 앱에 대 한 무료 프로 비전](~/ios/get-started/installation/device-provisioning/free-provisioning.md) 가이드를 참조 하세요.

## <a name="requirements"></a>요구 사항

Apple 계정 관리는 Mac용 Visual Studio, Visual Studio 2019 및 Visual Studio 2017 (버전 15.7 이상)에서 사용할 수 있습니다. 또한이 기능을 사용 하려면 유료 Apple 개발자 계정이 있어야 합니다. Apple 개발자 계정에 대 한 자세한 내용은 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드에서 확인할 수 있습니다.

> [!NOTE]
> 시작 하기 전에 먼저 [Apple 개발자 포털](https://developer.apple.com/account/)에서 사용자 사용권 계약을 수락 해야 합니다.

## <a name="add-an-apple-developer-account"></a>Apple 개발자 계정 추가

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

1. **Apple 개발자 계정 > Visual Studio > 기본 설정** 으로 이동 하 고 **+** 단추를 클릭 하 여 로그인 대화 상자를 엽니다.

    ![Mac용 Visual Studio 기본 설정의 Apple 개발자 계정 페이지입니다.](apple-account-management-images/add-account-vsm.png)

2. Apple ID와 암호를 입력 한 다음 **로그인**을 클릭 합니다. 그러면이 컴퓨터의 보안 키 집합에 자격 증명이 저장 됩니다.

3. 경고 대화 상자에서 **항상 허용** 을 선택 하 여 Visual Studio에서 자격 증명을 사용할 수 있도록 합니다.

    ![경고 항상 허용 대화 상자](apple-account-management-images/image4.png)

4. 계정이 성공적으로 추가 되 면 apple ID 및 Apple ID가 포함 된 모든 팀이 표시 됩니다.

    ![계정이 추가 된 Apple 개발자 계정 대화 상자](apple-account-management-images/image5.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

> [!NOTE]
> Visual Studio 2017 또는 Visual Studio 2019 (버전 16.4 및 이전 버전)를 사용 하는 경우 계속 하기 전에 [Mac 빌드 호스트와 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 해야 합니다.

1. **도구 > 옵션 > Xamarin > Apple 계정** 으로 이동한 후 **추가**를 클릭 합니다.

    ![Visual Studio 옵션에서 Apple 계정 페이지의 스크린샷](apple-account-management-images/add-account-vsw.png)

2. Apple ID와 암호를 입력 하 고 **로그인**을 클릭 합니다.

3. 계정이 성공적으로 추가 되 면 apple id 및 Apple ID가 포함 된 모든 팀이 표시 됩니다.

    ![계정이 추가 된 개발자 계정 페이지의 스크린샷](apple-account-management-images/accounts-vsw.png)

-----

## <a name="view-signing-certificates-and-provisioning-profiles"></a>서명 인증서 및 프로 비전 프로필 보기

팀을 선택 하 고 **자세히 보기** ...를 클릭 합니다. 컴퓨터에 설치 된 서명 id 및 프로 비전 프로필의 목록을 표시 하는 대화 상자를 엽니다.

팀 세부 정보 대화 상자에는 유형별로 구성 된 서명 Id의 목록이 표시 됩니다. **상태** 열은 다음과 같은 인증서 인지를 알려줍니다. 

- **유효함** – 서명 id (인증서와 개인 키 모두)가 컴퓨터에 설치 되어 있고 만료 되지 않았습니다.

- 키 **집합에 없음** – Apple 서버에 유효한 서명 id가 있습니다. 이를 컴퓨터에 설치 하려면 다른 컴퓨터에서 내보내야 합니다. 개인 키가 포함 되지 않으므로 Apple 개발자 포털에서 서명 id를 다운로드할 수 없습니다.

- **개인 키가 누락 됨** – 개인 키가 없는 인증서가 키 집합에 설치 되어 있습니다.

- **만료** 됨 – 인증서가 만료 됩니다. 키 집합에서이를 제거 해야 합니다.

  ![팀 세부 정보 대화 상자 정보](apple-account-management-images/image7.png)

## <a name="create-a-signing-certificate"></a>서명 인증서 만들기

새 서명 id를 만들려면 **인증서 만들기** 를 클릭 하 여 드롭다운 메뉴를 열고 만들려는 [인증서 종류](https://help.apple.com/xcode/mac/current/#/dev80c6204ec) 를 선택 합니다. 올바른 권한이 있는 경우 몇 초 후에 새 서명 id가 표시 됩니다.

드롭다운의 옵션이 회색으로 표시 되 고 선택 취소 된 경우이 유형의 인증서를 만들 수 있는 올바른 팀 권한이 없다는 것입니다.

## <a name="download-provisioning-profiles"></a>프로 비전 프로필 다운로드

팀 세부 정보 대화 상자에는 개발자 계정에 연결 된 모든 프로 비전 프로필의 목록도 표시 됩니다. **모든 프로필 다운로드**를 클릭 하 여 모든 프로 비전 프로필을 로컬 컴퓨터에 다운로드할 수 있습니다.


## <a name="troubleshoot"></a>문제 해결

- 새 Apple 개발자 계정을 승인 하는 데 몇 시간 정도 걸릴 수 있습니다. 계정이 승인 될 때까지 자동 프로 비전을 사용 하도록 설정할 수 없습니다.

- `Authentication Error: Xcode 7.3 or later is required to continue developing with your Apple ID.`메시지가 표시 되 고 Apple developer 계정 추가에 실패 하는 경우 사용 중인 Apple ID에 Apple 개발자 프로그램에 대 한 활성 유료 구성원이 있는지 확인 합니다. 유료 Apple 개발자 계정을 사용 하려면 [xamarin.ios 앱에 대 한 무료 프로 비전](~/ios/get-started/installation/device-provisioning/free-provisioning.md) 가이드를 참조 하세요.

- `You have reached the limit for certificates of this type`오류가 발생 하 여 새 서명 인증서를 만들지 못한 경우 허용 되는 최대 인증서 수가 생성 됩니다. 이 문제를 해결 하려면 [Apple 개발자 센터](https://developer.apple.com/account/ios/certificate/distribution) 로 이동 하 여 프로덕션 인증서 중 하나를 해지 합니다.

- Mac용 Visual Studio에서 계정에 로그인 하는 데 문제가 있는 경우 키 집합 응용 프로그램을 열고 **Category** 에서 **암호**를 선택 합니다. `deliver.`를 검색 하 고 찾은 항목을 모두 삭제 합니다.

## <a name="known-issues"></a>알려진 문제

- 기본적으로 프로비전 프로필 배포는 앱 스토어를 대상으로 합니다. 하우스 또는 임시 프로필은 수동으로 만들어야 합니다.
