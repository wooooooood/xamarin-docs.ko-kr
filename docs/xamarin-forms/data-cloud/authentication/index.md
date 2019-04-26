---
title: 웹 서비스에 대한 액세스 인증
description: 이 가이드만 자신의 데이터에 액세스할 수 있는 백 엔드를 공유할 수 있도록 하려면 Xamarin.Forms 응용 프로그램에 인증 서비스를 통합 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: d598a9b3de31ea6823530f911c3544bf3cebb37f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61331184"
---
# <a name="authenticating-access-to-web-services"></a>웹 서비스에 대한 액세스 인증

_이 가이드만 자신의 데이터에 액세스할 수 있는 백 엔드를 공유할 수 있도록 하려면 Xamarin.Forms 응용 프로그램에 인증 서비스를 통합 하는 방법에 설명 합니다. 다른 공급자에서 제공 하는 기본 제공 인증 메커니즘을 사용 하 여 및 다루는 Xamarin.Auth 구성 요소를 사용 하 여 OAuth id 공급자에 대해 인증 하는 REST 서비스를 사용 하 여 기본 인증을 사용 하 여 포함 됩니다._

## <a name="authenticating-a-restful-web-servicerestmd"></a>[RESTful 웹 서비스를 인증합니다.](rest.md)

HTTP은 리소스에 대 한 액세스를 제어 하는 몇 가지 인증 메커니즘 사용을 지원 합니다. 기본 인증 자격 증명이 있는 클라이언트만를 리소스에 대 한 액세스를 제공 합니다. 이 문서에서는 RESTful 웹 서비스 리소스에 대 한 액세스를 보호 하기 위해 기본 인증을 사용 하는 방법에 설명 합니다.

## <a name="authenticating-users-with-an-identity-provideroauthmd"></a>[Id 공급자를 사용 하 여 사용자 인증](oauth.md)

Xamarin.Auth는 사용자를 인증 하 고 해당 계정에 저장 된 플랫폼 간 SDK입니다. Google, Microsoft, Facebook 및 Twitter와 같은 id 공급자를 사용 하는 것에 대 한 지원을 제공 하는 OAuth 인증자를 포함 합니다. 이 문서에서는 Xamarin.Auth를 사용 하 여 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 방법에 설명 합니다.

## <a name="authenticating-users-with-azure-mobile-appsazuremd"></a>[Azure Mobile Apps 사용 하 여 사용자 인증](azure.md)

Azure Mobile Apps 인증 및 응용 프로그램 사용자 권한 부여를 지원 하기 위해 다양 한 외부 id 공급자를 사용 합니다. 사용 권한은 인증 된 사용자 에게만 액세스를 제한 하려면 테이블에서 설정할 수 있습니다. 이 문서에서는 Azure Mobile Apps를 사용 하 여 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 방법에 설명 합니다.

## <a name="authenticating-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[Azure Active Directory B2C 사용 하 여 사용자 인증](azure-ad-b2c.md)

Azure Active Directory B2C는 소비자 지향 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리 솔루션입니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에 소비자 id 관리를 통합할 Microsoft 인증 라이브러리 (MSAL) 및 Azure Active Directory B2C를 사용 하는 방법을 보여 줍니다.

## <a name="integrating-azure-active-directory-b2c-with-azure-mobile-appsazure-ad-b2c-mobile-appmd"></a>[Azure Mobile Apps와 Azure Active Directory B2C 통합](azure-ad-b2c-mobile-app.md)

Azure Mobile Apps에 대 한 인증 워크플로 관리 하려면 azure Active Directory B2C는 사용할 수 있습니다. 이 방법을 사용 하 여 id 관리 환경을 클라우드에서 완전히 정의 및 모바일 응용 프로그램 코드를 변경 하지 않고 수정할 수 있습니다. 이 문서에서는 Azure Active Directory B2C를 사용 하 여 Xamarin.Forms를 사용 하 여 인증 및 Azure Mobile Apps 인스턴스에 대 한 권한 부여를 제공 하는 방법에 설명 합니다.

## <a name="related-links"></a>관련 링크

- [웹 서비스 소개](~/cross-platform/data-cloud/web-services/index.md)
- [비동기 지원 개요](~/cross-platform/platform/async.md)
