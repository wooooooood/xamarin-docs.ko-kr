---
title: Xamarin.Forms 웹 서비스 인증
description: 이 가이드에서는 인증 서비스를 Xamarin.Forms 응용 프로그램에 통합하여 사용자가 자신의 데이터에만 액세스하면서 백 엔드를 공유할 수 있도록 하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: c3ad061953b08ef12bea7b6a63b7c39bbd89a5f6
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388579"
---
# <a name="xamarinforms-web-service-authentication"></a>Xamarin.Forms 웹 서비스 인증

## <a name="authenticate-a-restful-web-service"></a>[RESTful 웹 서비스 인증](rest.md)

HTTP는 리소스에 대한 액세스를 제어하기 위해 여러 인증 메커니즘의 사용을 지원합니다. 기본 인증은 올바른 자격 증명을 가진 클라이언트에만 리소스에 대한 액세스를 제공합니다. 이 문서에서는 기본 인증을 사용하여 RESTful 웹 서비스 리소스에 대한 액세스를 보호하는 방법을 설명합니다.

## <a name="authenticate-users-with-azure-active-directory-b2c"></a>[Azure Active Directory B2C로 사용자 인증](azure-ad-b2c.md)

Azure Active Directory B2C는 소비자 지향 웹 및 모바일 애플리케이션을 위한 클라우드 ID 관리 솔루션입니다. 이 문서에서는 MSAL(Microsoft 인증 라이브러리) 및 Azure Active Directory B2C를 사용하여 소비자 ID 관리를 Xamarin.Forms 응용 프로그램에 통합하는 방법을 설명합니다.

## <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>[Azure 코스모스 DB 문서 데이터베이스 및 Xamarin.Forms를 사용하여 사용자를 인증합니다.](azure-cosmosdb-auth.md)

Azure Cosmos DB 문서 데이터베이스는 여러 서버와 파티션에 걸쳐 무제한 저장소 및 처리량을 지원하면서 분할된 컬렉션을 지원합니다. 이 문서에서는 사용자가 Xamarin.Forms 응용 프로그램에서만 자신의 문서에 액세스할 수 있도록 액세스 제어를 분할된 컬렉션과 결합하는 방법을 설명합니다.
