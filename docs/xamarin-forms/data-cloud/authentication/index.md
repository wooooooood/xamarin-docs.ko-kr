---
title: Xamarin.Forms 웹 서비스 인증
description: 이 가이드만 자신의 데이터에 액세스할 수 있는 백 엔드를 공유할 수 있도록 하려면 Xamarin.Forms 응용 프로그램에 인증 서비스를 통합 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: a36dfba7aa07de1633ca9620674ddb4887902ba9
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67650421"
---
# <a name="xamarinforms-web-service-authentication"></a>Xamarin.Forms 웹 서비스 인증

## <a name="authenticate-a-restful-web-servicerestmd"></a>[RESTful 웹 서비스를 인증 합니다.](rest.md)

HTTP은 리소스에 대 한 액세스를 제어 하는 몇 가지 인증 메커니즘 사용을 지원 합니다. 기본 인증 자격 증명이 있는 클라이언트만를 리소스에 대 한 액세스를 제공 합니다. 이 문서에서는 RESTful 웹 서비스 리소스에 대 한 액세스를 보호 하기 위해 기본 인증을 사용 하는 방법에 설명 합니다.

## <a name="authenticate-users-with-an-identity-provideroauthmd"></a>[Id 공급자를 사용 하 여 사용자 인증](oauth.md)

Xamarin.Auth는 사용자를 인증 하 고 해당 계정에 저장 된 플랫폼 간 SDK입니다. Google, Microsoft, Facebook 및 Twitter와 같은 id 공급자를 사용 하는 것에 대 한 지원을 제공 하는 OAuth 인증자를 포함 합니다. 이 문서에서는 Xamarin.Auth를 사용 하 여 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 방법에 설명 합니다.

## <a name="authenticate-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[Azure Active Directory B2C 사용 하 여 사용자 인증](azure-ad-b2c.md)

Azure Active Directory B2C는 소비자 지향 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리 솔루션입니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에 소비자 id 관리를 통합할 Microsoft 인증 라이브러리 (MSAL) 및 Azure Active Directory B2C를 사용 하는 방법에 설명 합니다.

## <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinformsazure-cosmosdb-authmd"></a>[Azure Cosmos DB 문서 데이터베이스 및 Xamarin.Forms를 사용 하 여 사용자 인증](azure-cosmosdb-auth.md)

Azure Cosmos DB 문서 데이터베이스 무제한 저장소 및 처리량을 지원 하면서 여러 서버 및 파티션에 걸칠 수 있는 분할 된 컬렉션을 지원 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 자신의 문서에만 액세스할 수 있도록 분할 된 컬렉션을 사용 하 여 액세스 제어를 결합 하는 방법을 설명 합니다.
