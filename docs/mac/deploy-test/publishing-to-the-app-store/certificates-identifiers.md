---
title: Xamarin.Mac에서 인증서 및 식별자
description: 이 가이드에서는 Xamarin.Mac 앱을 게시하는 데 필요한 인증서 및 식별자를 만드는 방법을 안내합니다.
ms.prod: xamarin
ms.assetid: 393d0066-7f6f-4ac3-a48d-4b5db65bc4cd
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 5ba09afc477ddaadc07aa415376860eea3c8c28d
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671470"
---
# <a name="certificates-and-identifiers-in-xamarinmac"></a>Xamarin.Mac에서 인증서 및 식별자

_이 가이드에서는 Xamarin.Mac 앱을 게시하는 데 필요한 인증서 및 식별자를 만드는 방법을 안내합니다._

## <a name="certificates-and-identifiers"></a>인증서 및 식별자

개발용 Mac을 구성하려면 [Apple Developer Member Center](https://developer.apple.com)를 방문하세요. 주 메뉴는 아래에 나와 있습니다.

[![Apple Developer Member Center](certificates-identifiers-images/devcenter01.png "Apple Developer Member Center")](certificates-identifiers-images/devcenter01-large.png#lightbox)

**인증서, 식별자 및 프로필** 링크를 클릭합니다.

[![인증서, 식별자 및 프로필 선택](certificates-identifiers-images/devcenter02.png "인증서, 식별자 및 프로필 선택")](certificates-identifiers-images/devcenter02-large.png#lightbox)

다음으로, **Mac 앱** 섹션에서 **인증서 링크**를 클릭합니다.

[![인증서 링크 선택](certificates-identifiers-images/devcenter03.png "인증서 링크 선택")](certificates-identifiers-images/devcenter03-large.png#lightbox)

**모두** 링크를 클릭하고 **+** 단추를 클릭합니다.

[![모두 선택하고 새 항목 추가](certificates-identifiers-images/certif01.png "모두 선택하고 새 항목 추가")](certificates-identifiers-images/certif01-large.png#lightbox)

필요한 경우 여기서 **중간 인증서**를 다운로드합니다(전 세계 개발자 관계 인증 기관 및 개발자 ID 인증 기관). 그러나 이 부분은 Xcode에서 개발자에 맞게 자동으로 설정합니다.

이 섹션의 나머지 부분에서는 Mac 개발자 계정 설정을 완료하는 4개 섹션을 하나씩 안내합니다.

-   **Mac 앱 ID 등록** – 개발자는 자신이 작성하는 각 애플리케이션에 대해 다음 단계를 수행해야 합니다.
-   **macOS 시스템 등록** – 테스트할 컴퓨터를 추가할 때에만 필요합니다.
-   **인증서 만들기** - 인증서를 설정할 때 그리고 나중에 인증서를 갱신할 때 한 번만 필요합니다.
-   **프로비전 프로필 만들** - 개발자는 작성하는 새 애플리케이션마다 그리고 새 시스템을 추가할 때 다음 단계를 수행해야 합니다.

언제든지 페이지 왼쪽 상단의 **개요** 링크를 클릭하여 이 메뉴로 돌아올 수 있습니다.

### <a name="register-mac-app-id"></a>Mac 앱 ID 등록

개발자는 작성하는 각 애플리케이션의 앱 ID를 등록해야 합니다. 아래 단계를 사용하여 "MacWriter"라고 하는 기본 샘플 앱에 대한 항목을 만듭니다.

1. **앱 ID 설명**을 입력하고 애플리케이션에 필요한 **앱 서비스**를 선택합니다. 

    [![설명 및 앱 서비스 입력](certificates-identifiers-images/devcenter04.png "설명 및 앱 서비스 입력")](certificates-identifiers-images/devcenter04-large.png#lightbox)
2. 앱의 **번들 ID**를 입력하고 **계속** 단추를 클릭합니다. 

    [![번들 ID 입력](certificates-identifiers-images/devcenter05.png "번들 ID 입력")](certificates-identifiers-images/devcenter05-large.png#lightbox)
3. 정보를 확인하고 **전송** 단추를 클릭합니다. 

    [![정보 확인](certificates-identifiers-images/devcenter06.png "정보 확인")](certificates-identifiers-images/devcenter06-large.png#lightbox)

일부 **앱 서비스**는 추가 구성이 필요할 수 있습니다(예: iCloud). 이 경우 방금 만든 새 앱 ID를 선택하고 **편집** 단추를 클릭합니다.

[![새 앱 ID 편집](certificates-identifiers-images/devcenter07.png "새 앱 ID 편집")](certificates-identifiers-images/devcenter07-large.png#lightbox)

iCloud 서비스를 구성하려면 **편집** 단추를 클릭합니다.

[![iCloud 서비스 구성](certificates-identifiers-images/devcenter08.png "iCloud 서비스 구성")](certificates-identifiers-images/devcenter08-large.png#lightbox)

여기서 개발자는 사용할 데이터베이스를 구성할 수 있습니다.

[![데이터베이스 구성](certificates-identifiers-images/devcenter09.png "데이터베이스 구성")](certificates-identifiers-images/devcenter09-large.png#lightbox)

### <a name="register-macos-systems"></a>macOS 시스템 등록

테스트에 사용할 프로비전 프로필을 만들려면 개발자는 Mac 컴퓨터를 등록해야 합니다. 개발자는 Mac 앱을 테스트하기 위해 최대 100대의 컴퓨터를 등록할 수 있습니다.

Mac 개발자 센터의 **디바이스** 섹션에서 **모두**를 선택하고 **+** 단추를 클릭합니다.

[![새 컴퓨터 추가](certificates-identifiers-images/devcenter10.png "새 컴퓨터 추가")](certificates-identifiers-images/devcenter10-large.png#lightbox)

추가할 컴퓨터의 **이름** 및 **UUID**를 입력하고 **계속** 단추를 클릭합니다. 정보를 검토하고 **등록** 단추를 클릭합니다.

[![새 컴퓨터 정보 입력](certificates-identifiers-images/devcenter11.png "새 컴퓨터 정보 입력")](certificates-identifiers-images/devcenter11-large.png#lightbox)

### <a name="create-certificates"></a>인증서 만들기

인증서 섹션을 사용하여 Mac 애플리케이션을 서명하는 데 사용할 여러 가지 형식의 인증서를 만듭니다.

[![새 인증서 만들기](certificates-identifiers-images/certif01.png "새 인증서 만들기")](certificates-identifiers-images/certif01-large.png#lightbox)

크게 나눠서 세 가지 형식의 인증서가 있습니다.

-   **Mac 개발 인증서** – 일반 앱 개발을 위한 선택 사항이지만, 개발자가 iCloud 또는 푸시 알림 같은 기능을 사용할 계획이라면 필수입니다. 개발자는 이러한 기능을 사용할 수 있는 프로비전 프로필을 만들려면 개발 인증서가 필요합니다.
-   **Mac 앱 스토어** – 개발자는 앱에 대한 인증서와 설치 관리자에 대한 인증서가 필요합니다.
-   **개발자 ID** – Mac 앱 스토어 외부에 배포하기로 선택하는 경우 앱 및 설치 프로그램에 대한 인증서입니다.

다음 섹션에서는 위의 각 인증서 형식을 만드는 예제를 제공합니다.

#### <a name="mac-development-certificate"></a>Mac 개발 인증서

앞서 언급했듯이, iCloud 또는 푸시 알림 같은 macOS 기능을 사용하지 않는 이상, Mac 앱 개발 인증서가 필요 없습니다.

다음 단계에 따라 새 개발 인증서를 만듭니다.

1. **Mac 개발** 라디오 단추를 선택하고 **계속**을 클릭합니다. 

     [![개발 인증서 추가](certificates-identifiers-images/certif02.png "개발 인증서 추가")](certificates-identifiers-images/certif02-large.png#lightbox)
2. 다음 화면에서는 키 집합 액세스를 사용하여 업로드할 인증서 서명 요청 파일을 만드는 방법을 설명합니다. 

    [![키 집합 액세스 업로드 화면](certificates-identifiers-images/certif03.png "키 집합 액세스 업로드 화면")](certificates-identifiers-images/certif03-large.png#lightbox)
3. 나중에 최종 인증서를 만들었을 때 쉽게 인식할 수 있도록 의미 있는 일반적인 이름을 선택합니다. 다음 단계에서 찾을 수 있도록 파일 저장 위치를 기억해 둡니다. 

     ![인증서 내보내기](certificates-identifiers-images/image12.png "인증서 내보내기")
4. 인증서 요청 파일(확장명 `.certSigningRequest`)은 Mac에 로컬로 저장됩니다. 다음 단계에서 선택해야 하므로 어디에 저장되는지(기본 위치는 바탕 화면) 기억해야 합니다. 

     [![인증서 파일 업로드](certificates-identifiers-images/image13.png "인증서 파일 업로드")](certificates-identifiers-images/image13-large.png#lightbox)
5. **다운로드**를 클릭하여 인증서를 가져온 후 두 번 클릭하여 **키 집합**에 설치합니다. 

     [![개발 인증서 다운로드](certificates-identifiers-images/image15.png "개발 인증서 다운로드")](certificates-identifiers-images/image15-large.png#lightbox)
6. **다운로드**를 클릭하여 인증서를 가져온 후 두 번 클릭하여 **키 집합**에 설치합니다. **개발자 인증서 유틸리티**는 다음과 같이 인증서를 표시합니다. 

     [![개발자 인증서 유틸리티](certificates-identifiers-images/image16.png "개발자 인증서 유틸리티")](certificates-identifiers-images/image16-large.png#lightbox)
7. **키 집합**에 다음과 같이 표시됩니다. 

     ![키 집합 액세스의 인증서](certificates-identifiers-images/image17.png "키 집합 액세스의 인증서")

앞서 언급했듯이, 개발자가 iCloud 및 푸시 알림 같은 macOS 기능을 구현하지 않는 한, 개발자 인증서가 항상 필요하지는 않습니다. Mac 앱 스토어 앱을 테스트하는 데 필요한 **개발 프로비전 프로필**을 만들 때에도 개발자 인증서가 필요합니다.

#### <a name="mac-app-store-certificates"></a>Mac 앱 스토어 인증서

앱 스토어에 앱을 릴리스하려면 애플리케이션 및 Mac 설치 관리자 패키지를 서명하는 데 사용할 **Mac 앱 스토어** 인증서를 만들어야 합니다.

1. 인증서 유형으로 **Mac 앱 스토어**를 선택하고 **계속** 단추를 클릭합니다. 

    [![앱 스토어 인증서 만들기](certificates-identifiers-images/certif04.png "앱 스토어 인증서 만들기")](certificates-identifiers-images/certif04-large.png#lightbox)
2. 만들려는 인증서 유형을 선택합니다(앱 스토어에 릴리스할 각 유형 중 하나가 필요). 

    [![인증서 종류 선택](certificates-identifiers-images/certif05.png "인증서 종류 선택")](certificates-identifiers-images/certif05-large.png#lightbox)
3. 다음 페이지에서는 **키 집합 액세스**를 사용하여 인증서 요청 파일을 생성하는 방법을 설명합니다. 다음 지침을 따릅니다. 

     [![키 집합 요청 생성](certificates-identifiers-images/certif06.png "키 집합 요청 생성")](certificates-identifiers-images/certif06-large.png#lightbox)
4. 구체적인 **일반 이름**을 선택합니다. 예를 들어 이름에 "앱 스토어 애플리케이션"이라는 텍스트를 사용합니다. 

     ![설명이 포함된 이름 입력](certificates-identifiers-images/image20.png "설명이 포함된 이름 입력")
5. 인증서 요청 파일(확장명 `.certSigningRequest`)은 Mac에 로컬로 저장됩니다. 저장된 위치를 기억해 둡니다(기본 위치는 바탕 화면). 

     [![인증서 저장](certificates-identifiers-images/image21.png "인증서 저장")](certificates-identifiers-images/image21-large.png#lightbox)
6. **다운로드**를 클릭하여 인증서를 가져온 후 두 번 클릭하여 **키 집합**에 설치합니다. 

      [![앱 스토어 인증서 다운로드](certificates-identifiers-images/image23.png "앱 스토어 인증서 다운로드")](certificates-identifiers-images/image23-large.png#lightbox)
7. **계속**을 클릭하고 동일한 단계에 따라 이번에는 *설치 관리자*에 대한 다른 인증서를 다운로드합니다. 

     [![설치 관리자 선택](certificates-identifiers-images/image24.png "설치 관리자 선택")](certificates-identifiers-images/image24-large.png#lightbox)
8. 구체적인 **일반 이름**을 선택합니다. 예를 들어 이름에 "앱 스토어 설치 관리자"라는 텍스트를 사용합니다. 

     ![인증서 이름 설정](certificates-identifiers-images/image25.png "인증서 이름 설정")
9. 인증서 요청 파일(확장명 `.certSigningRequest`)은 Mac에 로컬로 저장됩니다. 저장된 위치를 기억해 둡니다(기본 위치는 바탕 화면). 

     [![인증서 업로드](certificates-identifiers-images/image26.png "인증서 업로드")](certificates-identifiers-images/image26-large.png#lightbox) 

     [![배포 인증서 다운로드](certificates-identifiers-images/image28.png "배포 인증서 다운로드")](certificates-identifiers-images/image28-large.png#lightbox)
10. **다운로드**를 클릭하여 인증서를 가져온 후 두 번 클릭하여 **키 집합**에 설치합니다. 개발자 인증서 유틸리티는 다음과 같이 인증서를 표시합니다. 

     [![개발자 인증서 유틸리티](certificates-identifiers-images/image29.png "개발자 인증서 유틸리티")](certificates-identifiers-images/image29-large.png#lightbox)
11. 이제 **키 집합**에 다음과 같은 두 개의 새 인증서가 나타납니다. 

     [![키 집합 액세스의 인증서](certificates-identifiers-images/image30.png "키 집합 액세스의 인증서")](certificates-identifiers-images/image30-large.png#lightbox)

#### <a name="developer-id-certificates"></a>개발자 ID 인증서

Xamarin.Mac 애플리케이션을 셀프 릴리스하려면(Apple 앱 스토어를 통해 릴리스하지 않고) 개발자는 릴리스 및 설치할 앱을 서명하기 위한 개발자 ID 인증서가 필요합니다.

다음을 수행합니다.

1. **인증서** 섹션에서 **+** 단추를 클릭한 후 **개발자 ID** 라디오 단추를 선택합니다. 

    [![개발자 ID 추가](certificates-identifiers-images/certif07.png "개발자 ID 추가")](certificates-identifiers-images/certif07-large.png#lightbox)
2. **계속** 단추를 클릭하고 만들려는 개발자 ID 유형을 선택합니다. 

    [![개발자 ID 유형 선택](certificates-identifiers-images/certif08.png "개발자 ID 유형 선택")](certificates-identifiers-images/certif08-large.png#lightbox)
3. 두 개가 필요합니다. 하나는 애플리케이션 자체를 서명하는 데 필요하고, 다른 하나는 애플리케이션의 설치 관리자를 서명하는 데 필요합니다. 이러한 키의 인증서 요청 이름을 지정할 때 주의해야 합니다. 나중에 쉽게 구분할 수 있도록 `Application` 및 `Installer` 텍스트를 포함하는 구체적인 이름을 사용해야 합니다.
4. 다음 화면은 인증서를 만드는 방법에 대한 자세한 지침을 제공합니다. **계속** 단추를 클릭하세요. 

    [![인증서를 만드는 방법](certificates-identifiers-images/certif09.png "인증서를 만드는 방법")](certificates-identifiers-images/certif09-large.png#lightbox)
5. 구체적인 **일반 이름**을 선택합니다. 예를 들어 이름에 "개발자 ID 애플리케이션"이라는 텍스트를 사용합니다. 

     ![인증서 이름 입력](certificates-identifiers-images/image33.png "인증서 이름 입력")
6. 인증서 요청 파일(확장명 `.certSigningRequest`)은 Mac에 로컬로 저장됩니다. 저장된 위치를 기억해 둡니다(기본 위치는 바탕 화면). 

     [![인증서 업로드](certificates-identifiers-images/certif10.png "인증서 업로드")](certificates-identifiers-images/certif10-large.png#lightbox) 

     [![개발자 ID 다운로드](certificates-identifiers-images/certif11.png "개발자 ID 다운로드")](certificates-identifiers-images/certif11-large.png#lightbox)
7. **다운로드**를 클릭하여 인증서를 가져온 후 두 번 클릭하여 **키 집합**에 설치합니다.
8. **계속**을 클릭하고 동일한 단계에 따라 이번에는 *설치 관리자*에 대한 다른 인증서를 다운로드합니다.
9. 구체적인 **일반 이름**을 선택합니다. 예를 들어 이름에 "개발자 ID 설치 관리자"라는 텍스트를 사용합니다. 

     ![일반 이름 입력](certificates-identifiers-images/image38.png "일반 이름 입력")
10. 인증서 요청 파일(확장명 `.certSigningRequest`)은 Mac에 로컬로 저장됩니다. 저장된 위치를 기억해 둡니다(기본 위치는 바탕 화면). 

     [![인증서 업로드](certificates-identifiers-images/certif10.png "인증서 업로드")](certificates-identifiers-images/certif10-large.png#lightbox)
11. 이제 인증서를 다운로드할 수 있습니다. **다운로드** 단추를 클릭한 후 **완료**를 클릭합니다. 

     [![인증서 다운로드](certificates-identifiers-images/certif11.png "인증서 다운로드")](certificates-identifiers-images/certif11-large.png#lightbox)
12. **다운로드**를 클릭하여 인증서를 가져온 후 두 번 클릭하여 **키 집합**에 설치합니다. **개발자 인증서 유틸리티**는 다음과 같이 인증서를 표시합니다. 

     [![개발자 인증서 유틸리티](certificates-identifiers-images/certif12.png "개발자 인증서 유틸리티")](certificates-identifiers-images/certif12-large.png#lightbox)
13. 다음 항목이 **키 집합**에 표시됩니다. 

     [![키 집합 액세스의 인증서](certificates-identifiers-images/image43.png "키 집합 액세스의 인증서")](certificates-identifiers-images/image43-large.png#lightbox)


## <a name="related-links"></a>관련 링크

- [설치](/visualstudio/mac/installation/)
- [Hello, Mac 샘플](~/mac/get-started/hello-mac.md)
- [Mac 앱 스토어에서 앱 배포](https://developer.apple.com/devcenter/mac/checklist/)
- [개발자 ID 및 게이트키퍼](https://developer.apple.com/resources/developer-id/)
