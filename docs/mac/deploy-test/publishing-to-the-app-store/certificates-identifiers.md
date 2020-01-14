---
title: Xamarin.Mac에서 인증서 및 식별자
description: 이 가이드에서는 Xamarin.Mac 앱을 게시하는 데 필요한 인증서 및 식별자를 만드는 방법을 안내합니다.
ms.prod: xamarin
ms.assetid: 393d0066-7f6f-4ac3-a48d-4b5db65bc4cd
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 12/17/2019
ms.openlocfilehash: 2ea3516c1fb89c8c9b9cc3694d7c95ccd87e9d41
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489645"
---
# <a name="certificates-and-identifiers-in-xamarinmac"></a>Xamarin.Mac에서 인증서 및 식별자

_이 가이드에서는 Xamarin.Mac 앱을 게시하는 데 필요한 인증서 및 식별자를 만드는 방법을 안내합니다._

## <a name="setup"></a>설정

개발용 Mac을 구성하려면 [Apple Developer Member Center](https://developer.apple.com)를 방문하세요. **계정** 링크 및 로그인을 클릭합니다. 주 메뉴는 아래에 나와 있습니다.

> [!div class="mx-imgBorder"]
> [![Apple Developer Member Center](certificates-identifiers-images/devcenter01.png)](certificates-identifiers-images/devcenter01-large.png#lightbox)

**인증서, 식별자 및 프로필** 단추(또는 **인증서** 제목 옆의 더하기 단추)를 클릭합니다.

> [!div class="mx-imgBorder"]
> [![인증서, ID 및 프로필 선택](certificates-identifiers-images/devcenter02.png)](certificates-identifiers-images/devcenter02-large.png#lightbox)

인증서 형식을 선택하고 **계속**을 클릭합니다.

> [!div class="mx-imgBorder"]
> [![인증서 링크 선택](certificates-identifiers-images/devcenter03.png)](certificates-identifiers-images/devcenter03-large.png#lightbox)

필요한 경우 여기서 **중간 인증서**(전 세계 개발자 관계 인증 기관 및 개발자 ID 인증 기관)를 다운로드할 수 있습니다(해당 페이지 아래쪽의 마지막 항목). 그러나 이 부분은 Xcode에서 개발자에 맞게 자동으로 설정합니다.

이 섹션의 나머지 부분에서는 Mac 개발자와 관련된 섹션을 안내합니다.

- **Mac 앱 ID 등록** – 개발자는 자신이 작성하는 각 애플리케이션에 대해 다음 단계를 수행해야 합니다.
- **macOS 시스템 등록** – 테스트할 컴퓨터를 추가할 때에만 필요합니다.
- **인증서 만들기** - 인증서를 설정할 때와 나중에 인증서를 갱신할 때 한 번만 필요합니다.
- **프로비전 프로필 만들** - 개발자는 작성하는 새 애플리케이션마다 그리고 새 시스템을 추가할 때 다음 단계를 수행해야 합니다.

## <a name="register-mac-app-id"></a>Mac 앱 ID 등록

모든 애플리케이션에 대한 앱 ID를 등록해야 합니다. 아래 단계에 따라 항목을 만듭니다.

1. "+" (더하기 기호)를 누르거나 **앱 ID를 등록합니다**.

    > [!div class="mx-imgBorder"]
    > [![앱 ID로 시작](certificates-identifiers-images/appid01.png)](certificates-identifiers-images/appid01-large.png#lightbox)

1. **앱 ID** 선택

    > [!div class="mx-imgBorder"]
    > [![앱 ID로 시작](certificates-identifiers-images/appid02.png)](certificates-identifiers-images/appid02-large.png#lightbox)

1. **설명**을 입력하고 애플리케이션에 필요한 **앱 서비스**를 선택합니다. 플랫폼은 **macOS**이어야 합니다. **설명**(이 포털에서만 사용됨)을 선택합니다. **번들 ID**를 입력합니다.이 ID는 **Info.plist**와 일치해야 합니다. 앱에 필요한 기능을 선택합니다.

    > [!div class="mx-imgBorder"]
    > [![설명 및 앱 서비스 입력](certificates-identifiers-images/appid03.png)](certificates-identifiers-images/appid03-large.png#lightbox)

    **계속**를 눌러 선택 항목을 검토합니다.

1. 정보가 올바르면 **등록**을 클릭하여 설치를 완료합니다.

    > [!div class="mx-imgBorder"]
    > [![입력한 데이터 검토](certificates-identifiers-images/appid04.png)](certificates-identifiers-images/appid04-large.png#lightbox)

1. 정보를 확인하고 **전송** 단추를 클릭합니다.

    > [!div class="mx-imgBorder"]
    > ![정보 확인 중](certificates-identifiers-images/appid05.png)

일부 **앱 서비스**는 추가 구성이 필요할 수 있습니다(예: iCloud). 이 경우 방금 만든 새 앱 ID를 선택하고 **편집** 단추를 클릭합니다.

> [!div class="mx-imgBorder"]
> [![새 앱 ID 편집 중](certificates-identifiers-images/appid06.png)](certificates-identifiers-images/appid06-large.png#lightbox)

iCloud 서비스 등을 구성하려면 **편집** 단추를 클릭합니다.

> [!div class="mx-imgBorder"]
> [![iCloud 서비스 구성 중](certificates-identifiers-images/appid07.png)](certificates-identifiers-images/appid07-large.png#lightbox)

## <a name="register-macos-devices"></a>macOS 디바이스 등록

테스트에 사용할 프로비전 프로필을 만들려면 개발자는 Mac 컴퓨터를 등록해야 합니다. 테스트를 위해 최대 100대의 컴퓨터를 등록할 수 있습니다.

1. Mac 개발자 센터의 **디바이스** 섹션에서 **모두**를 선택하고 **+** 단추를 클릭합니다.

    > [!div class="mx-imgBorder"]
    > [![새 컴퓨터 추가 중](certificates-identifiers-images/device01.png)](certificates-identifiers-images/device01-large.png#lightbox)

1. 추가할 컴퓨터의 **이름** 및 **UUID**를 입력하고 **계속** 단추를 클릭합니다. 정보를 검토하고 **등록** 단추를 클릭합니다.

    > [!div class="mx-imgBorder"]
    > [![새 컴퓨터 정보 입력 중](certificates-identifiers-images/device02.png)](certificates-identifiers-images/device02-large.png#lightbox)

1. 입력한 데이터를 검토하고 확인합니다.

    > [!div class="mx-imgBorder"]
    > [![새 컴퓨터 정보 입력 중](certificates-identifiers-images/device03.png)](certificates-identifiers-images/device03-large.png#lightbox)

## <a name="create-certificates"></a>인증서 만들기

인증서 섹션을 사용하여 Mac 애플리케이션을 서명하는 데 사용할 여러 가지 형식의 인증서를 만듭니다.

> [!div class="mx-imgBorder"]
> [![새 인증서 만들기](certificates-identifiers-images/devcenter04.png)](certificates-identifiers-images/devcenter04-large.png#lightbox)

macOS 개발과 관련된 5가지 주요 유형의 인증서가 있습니다.

- **Mac 개발** – 일반 앱 개발을 위한 선택 사항이지만, 개발자가 iCloud 또는 푸시 알림 같은 기능을 사용하려는 경우에는 필수입니다. 개발자는 이러한 기능을 사용할 수 있는 프로비전 프로필을 만들려면 개발 인증서가 필요합니다.
- **Mac 앱 배포** – 개발자는 앱에 대한 인증서와 설치 관리자에 대한 다른 인증서가 필요합니다.
- **Mac 설치 관리자 스토어** – 개발자는 앱에 대한 인증서와 설치 관리자에 대한 다른 인증서가 필요합니다.
- **개발자 ID 설치 관리자** – 설치 관리자를 Mac App Store 외부에서 배포되기 위한 인증서입니다.
- **개발자 ID 애플리케이션** – 앱을 Mac App Store 외부에서 배포하기 위한 인증서입니다.

다음 섹션에서는 이러한 인증서 형식의 일부를 만드는 예제를 제공합니다.

### <a name="mac-development-certificate"></a>Mac 개발 인증서

위에 언급한 대로, iCloud 또는 푸시 알림 같은 macOS 기능을 사용하지 않는 한 Mac 개발 인증서는 필요 없습니다.

다음 단계에 따라 새 개발 인증서를 만듭니다.

1. **Mac 개발** 라디오 단추를 선택하고 **계속**을 클릭합니다.

    > [!div class="mx-imgBorder"]
    > [![개발 인증서 추가 중](certificates-identifiers-images/certif02.png)](certificates-identifiers-images/certif02-large.png#lightbox)

1. _인증서 서명 요청_을 업로드합니다. 인증서 요청 파일(확장명 `.certSigningRequest`)은 Mac에 로컬로 저장됩니다. **파일 선택**을 클릭하여 인증서 요청을 선택한 다음 **계속**을 누릅니다.

    > [!div class="mx-imgBorder"]
    > [![인증서 서명 요청 업로드](certificates-identifiers-images/certif03.png)](certificates-identifiers-images/certif03-large.png#lightbox)

    **키 집합 액세스**를 사용하여 인증서 요청 파일을 만드는 방법에 대한 지침은 [자세한 정보 >](https://help.apple.com/developer-account/#/devbfa00fef7) 링크를 참조하세요.

1. **다운로드**를 눌러 인증서 파일을 가져온 후 두 번 클릭하여 설치합니다.

    > [!div class="mx-imgBorder"]
    > [![인증서 파일 다운로드](certificates-identifiers-images/certif04.png)](certificates-identifiers-images/certif04-large.png#lightbox)

앞서 언급했듯이, 개발자가 iCloud 및 푸시 알림 같은 macOS 기능을 구현하지 않는 한, 개발자 인증서가 항상 필요하지는 않습니다. Mac 앱 스토어 앱을 테스트하는 데 필요한 **개발 프로비전 프로필**을 만들 때에도 개발자 인증서가 필요합니다.

### <a name="mac-app-store-certificates"></a>Mac 앱 스토어 인증서

App Store에서 앱을 해제하려면 다음의 두 가지 인증서가 필요합니다.

- 애플리케이션에 서명하는 데 사용되는 **Mac 앱 배포** 인증서와 
- 설치 관리자에 서명하기 위한 **Mac 설치 관리자 배포**입니다.

> [!TIP]
> 이러한 키의 인증서 요청 이름을 지정할 때 주의해야 합니다. 나중에 쉽게 구분할 수 있도록 `Application` 및 `Installer` 텍스트를 포함하는 구체적인 이름을 사용해야 합니다.

먼저 설치 관리자 인증서를 만듭니다.

1. 인증서 유형으로 **Mac 설치 관리자 배포**를 선택하고 **계속** 단추를 클릭합니다.

    > [!div class="mx-imgBorder"]
    > [![App Store 인증서 만드는 중](certificates-identifiers-images/certif05.png)](certificates-identifiers-images/certif05-large.png#lightbox)

1. 다음 페이지에서는 **키 집합 액세스**를 사용하여 인증서 요청 파일을 생성하는 방법을 설명합니다. 다음 지침을 따릅니다.

    > [!div class="mx-imgBorder"]
    > [![인증서 요청 업로드](certificates-identifiers-images/certif06.png)](certificates-identifiers-images/certif06-large.png#lightbox)

    **키 집합 액세스**를 사용하여 인증서 요청 파일을 만드는 방법에 대한 지침은 [자세한 정보 >](https://help.apple.com/developer-account/#/devbfa00fef7) 링크를 참조하세요. 인증서(애플리케이션 또는 설치 관리자)의 _유형_을 반영하는 인증서 이름을 선택해야 합니다.

1. **다운로드**를 클릭하여 인증서를 가져온 후 두 번 클릭하여 **키 집합**에 설치합니다.

    > [!div class="mx-imgBorder"]
    > [![App Store 인증서 다운로드](certificates-identifiers-images/certif07.png)](certificates-identifiers-images/certif07-large.png#lightbox)

**Mac 앱 배포 인증서에 대해 동일한 단계를 수행합니다.**

![Mac 앱 배포 인증서](certificates-identifiers-images/certif08.png)

### <a name="developer-id-certificates"></a>개발자 ID 인증서

Xamarin.Mac 애플리케이션을 직접 해제(Apple App Store를 통해 해제하지 않음)하려면 두 가지 인증서가 필요합니다.

- 애플리케이션에 서명하는 데 사용되는 **개발자 ID 설치 관리자** 인증서와 
- 설치 관리자에 서명하기 위한 **개발자 ID 애플리케이션** 인증서입니다.

> [!TIP]
> 이러한 키의 인증서 요청 이름을 지정할 때 주의해야 합니다. 나중에 쉽게 구분할 수 있도록 `Application` 및 `Installer` 텍스트를 포함하는 구체적인 이름을 사용해야 합니다.

인증서를 만들어 다운로드하고 설치하면 인증서가 **키 집합 액세스**에 표시됩니다.

[키 집합 액세스 인증서 목록](certificates-identifiers-images/certif09.png)

## <a name="related-links"></a>관련 링크

- [설치](/visualstudio/mac/installation/)
- [Hello, Mac 샘플](~/mac/get-started/hello-mac.md)
- [Mac 앱 스토어에서 앱 배포](https://developer.apple.com/devcenter/mac/checklist/)
- [개발자 ID 및 게이트키퍼](https://developer.apple.com/resources/developer-id/)
