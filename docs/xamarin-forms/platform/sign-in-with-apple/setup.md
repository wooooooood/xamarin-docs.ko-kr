---
title: 설치 단계-Apple을 사용 하 여 로그인Xamarin.Forms
description: Apple 설치 프로그램을 사용 하 여 로그인 하면 모바일 응용 프로그램이 대상으로 하는 다양 한 플랫폼에 따라 달라 집니다.
ms.prod: xamarin
ms.assetid: 8F712802-395B-469B-B5BE-C927AD1A8391
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 09/10/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 95fc2547dd2f17f7aa2b2e8ca4c70915c6542318
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198038"
---
# <a name="setup-sign-in-with-apple-for-xamarinforms"></a>Apple을 사용 하 여 로그인 설정Xamarin.Forms

이 가이드에서는 Apple을 사용 하 여 로그인 하기 위해 플랫폼 간 응용 프로그램을 설정 하는 데 필요한 일련의 단계를 다룹니다. Apple 개발자 포털에서 Apple 설정이 바로 진행 되는 동안에는 Android와 Apple 간에 보안 관계를 만들기 위해 추가 단계가 필요 합니다. 

## <a name="apple-developer-setup"></a>Apple developer 설치

응용 프로그램에서 Apple에 로그인을 사용 하려면 Apple 개발자 포털의 [인증서, 식별자 & 프로필](https://developer.apple.com/account/resources/) 섹션에서 일부 설정 단계를 해결 해야 합니다.

### <a name="apple-sign-in-domain"></a>Apple 로그인 도메인

*인증서, 식별자 & 프로필* 섹션의 [자세히](https://developer.apple.com/account/resources/services/list) 섹션에서 도메인 이름을 등록 하 고 Apple을 사용 하 여 확인 합니다.

![추가 섹션](sign-in-images/readme-signin-domain-configure.png)

도메인을 추가 하 고 **등록**을 클릭 합니다.

![도메인 등록 양식](sign-in-images/readme-signin-domain-more.png)

> [!NOTE]
> SPF 규격이 아닌 도메인에 대 한 오류가 표시 되 면 SPF DNS TXT 레코드를 도메인에 추가 하 고 계속 하기 전에 전파 될 때까지 기다려야 합니다. SPF TXT는 다음과 같이 보일 수 있습니다.`v=spf1 a a:myapp.com -all`

다음에는 **다운로드** 를 클릭 하 여 `apple-developer-domain-association.txt` 파일을 검색 하 고 도메인의 웹 사이트 폴더에 업로드 하 여 도메인 소유권을 확인 해야 합니다 `.well-known` .

`.well-known/apple-developer-domain-association.txt`파일이 업로드 되 고 연결할 수 있게 되 면 **확인** 을 클릭 하 여 Apple에서 도메인 소유권을 확인할 수 있습니다.

> [!NOTE]
> Apple은를 사용 하 여 소유권을 확인 `https://` 합니다. SSL 설정이 필요 하 고 파일에 보안 URL을 통해 액세스할 수 있는지 확인 합니다.

계속 하기 전에이 프로세스를 완료 하십시오.

## <a name="setup-your-app-id"></a>앱 ID 설정

[식별자](https://developer.apple.com/account/resources/identifiers/list) 섹션에서 새 식별자를 만들고 **앱 id**를 선택 합니다. 앱 ID가 이미 있는 경우 편집을 대신 선택 합니다.

![새 앱 ID 만들기](sign-in-images/readme-appid-create.png)

**Apple에서 로그인**을 사용 하도록 설정 합니다. **기본 앱 ID로 사용** 옵션을 사용 하는 것이 좋습니다.

![Apple에서 로그인 사용](sign-in-images/readme-appid-signin.png)

앱 ID 변경 내용을 저장 합니다.

## <a name="create-a-service-id"></a>서비스 ID 만들기

[식별자](https://developer.apple.com/account/resources/identifiers/list/serviceId) 섹션에서 새 식별자를 만들고 **서비스 id**를 선택 합니다.

![새 서비스 ID 만들기](sign-in-images/readme-serviceid-create.png)

서비스 ID에 설명 및 식별자를 제공 합니다.  이 식별자는 `ServerId` 입니다.  **Apple에서 로그인**을 사용 하도록 설정 해야 합니다.

계속 하기 전에 Apple을 사용 하 _여 로그인_ 옵션 옆에 있는 **구성** 을 클릭 합니다.

구성 패널에서 올바른 **기본 앱 ID** 가 선택 되어 있는지 확인 합니다.

다음으로, 이전에 구성한 **웹 도메인** 을 선택 합니다.

마지막으로 하나 이상의 **반환 url**을 추가 합니다.  나중에 사용 하는 모든 `redirect_uri` 사용자를 사용 하는 경우 정확히 여기에 등록 해야 합니다.  `http://` `https://` URL을 입력할 때를 URL에 포함 해야 합니다.

> [!NOTE]
> 테스트 목적으로는 `127.0.0.1` 또는를 사용할 수 `localhost` 없지만와 같은 다른 도메인은 사용할 수 있습니다 `local.test` .  이 작업을 수행 하도록 선택 하는 경우 컴퓨터의 파일을 편집 `hosts` 하 여이 가상 도메인을 로컬 IP 주소로 확인할 수 있습니다.

![Apple 로그인 구성](sign-in-images/readme-serviceid-configure.png)

완료 되 면 변경 내용을 저장 합니다.

## <a name="create-a-key-for-your-services-id"></a>서비스 ID의 키 만들기

[키](https://developer.apple.com/account/resources/authkeys/list) 섹션에서 새 **키**를 만듭니다.

키에 이름을 지정 하 고 **Apple에서 로그인**을 사용 하도록 설정 합니다.

![새 키 만들기](sign-in-images/readme-key-create.png)

_Apple에 로그인_옆에 있는 **구성** 을 클릭 합니다.

올바른 **기본 앱 ID** 가 선택 되어 있는지 확인 하 고 **저장**을 클릭 합니다.

**계속** 을 클릭 한 다음 **등록** 을 클릭 하 여 새 키를 만듭니다.

다음에는 방금 생성 한 키를 다운로드할 수 있는 기회가 하나 뿐입니다.  **다운로드**를 클릭합니다.

![다운로드 키](sign-in-images/readme-key-download.png)

또한이 단계에서 **키 ID** 를 기록해 둡니다. 이는 나중에 사용 됩니다 `KeyId` .

`.p8`키 파일이 다운로드 됩니다.  메모장 또는 VSCode에서이 파일을 열어 텍스트 내용을 볼 수 있습니다.  다음과 유사 하 게 표시 됩니다.

```
-----BEGIN PRIVATE KEY-----
MIGTAgEAMBMGBasGSM49AgGFCCqGSM49AwEHBHkwdwIBAQQg3MX8n6VnQ2WzgEy0
Skoz9uOvatLMKTUIPyPCAejzzUCgCgYIKoZIzj0DAQehRANCAARZ0DoM6QPqpJxP
JKSlWz0AohFhYre10EXPkjrih4jTm+b0AeG2BGuoIWd18i8FimGDgK6IzHHPsEqj
DHF5Svq0
-----END PRIVATE KEY-----
```

이 키의 이름을로 하 `P8FileContents` 고 안전한 장소에 보관 합니다. 모바일 응용 프로그램에이 서비스를 통합할 때 사용 합니다.

## <a name="summary"></a>요약

이 문서에서는 응용 프로그램에서 사용 하기 위해 Apple에 로그인을 설정 하는 데 필요한 단계에 대해 설명 Xamarin.Forms 했습니다.

## <a name="related-links"></a>관련 링크

- [Apple 지침을 사용 하 여 로그인](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)
  