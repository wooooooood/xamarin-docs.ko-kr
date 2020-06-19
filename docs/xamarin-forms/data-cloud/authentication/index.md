---
title: Xamarin.Forms웹 서비스 인증
description: 이 가이드에서는 인증 서비스를 응용 프로그램에 통합 하 여 Xamarin.Forms 사용자가 자신의 데이터에만 액세스할 수 있도록 하는 동시에 백 엔드를 공유할 수 있도록 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 59130739617f38ce32e0d241f8e068cf077997ac
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136086"
---
# <a name="xamarinforms-web-service-authentication"></a>Xamarin.Forms웹 서비스 인증

## <a name="authenticate-a-restful-web-service"></a>[RESTful 웹 서비스 인증](rest.md)

HTTP에서는 여러 인증 메커니즘을 사용 하 여 리소스에 대 한 액세스를 제어할 수 있습니다. 기본 인증은 올바른 자격 증명이 있는 클라이언트에만 리소스에 대 한 액세스를 제공 합니다. 이 문서에서는 기본 인증을 사용 하 여 RESTful 웹 서비스 리소스에 대 한 액세스를 보호 하는 방법을 설명 합니다.

## <a name="authenticate-users-with-azure-active-directory-b2c"></a>[Azure Active Directory B2C로 사용자 인증](azure-ad-b2c.md)

Azure Active Directory B2C는 소비자 지향 웹 및 모바일 애플리케이션을 위한 클라우드 ID 관리 솔루션입니다. 이 문서에서는 MSAL (Microsoft 인증 라이브러리) 및 Azure Active Directory B2C를 사용 하 여 소비자 id 관리를 응용 프로그램에 통합 하는 방법을 설명 합니다 Xamarin.Forms .

## <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinformsazure-cosmosdb-authmd"></a>[Azure Cosmos DB 문서 데이터베이스를 사용 하 여 사용자 인증Xamarin.Forms](azure-cosmosdb-auth.md)

Azure Cosmos DB 문서 데이터베이스는 분할 된 컬렉션을 지원 하며,이는 여러 서버 및 파티션에 걸쳐 있을 수 있으며 저장소와 처리량을 무제한으로 지원 합니다. 이 문서에서는 사용자가 응용 프로그램에서 자신의 문서에만 액세스할 수 있도록 분할 된 컬렉션과 액세스 제어를 결합 하는 방법을 설명 합니다 Xamarin.Forms .
