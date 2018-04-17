---
title: 데이터 및 클라우드 서비스
description: Xamarin.Forms 응용 프로그램이 다양 한 기술 사용 하 여 구현 하는 웹 서비스를 사용할 수 있습니다 하 고이 가이드에는이 작업을 수행 하는 방법을 검사 합니다.
ms.prod: xamarin
ms.assetid: 0601D9D0-C8D2-4C3B-A749-A340BDBF64A4ß
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 6e67820fa83ddea46f934b4eaedde2c6334f9cc6
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
---
# <a name="data--cloud-services"></a>데이터 및 클라우드 서비스

_Xamarin.Forms 응용 프로그램이 다양 한 기술 사용 하 여 구현 하는 웹 서비스를 사용할 수 있습니다 하 고이 가이드에는이 작업을 수행 하는 방법을 검사 합니다._

Xamarin 플랫폼에서 플랫폼 간 웹 서비스 사용에 대 한 소개를 참조 하십시오. [웹 서비스 소개](~/cross-platform/data-cloud/web-services/index.md)합니다.

## <a name="understanding-the-samplexamarin-formsdata-cloudwalkthroughmd"></a>[샘플 이해](~/xamarin-forms/data-cloud/walkthrough.md)

이 문서는 다른 웹 서비스와 통신 하는 방법을 보여 주는 Xamarin.Forms 샘플 응용 프로그램의 연습을 제공 합니다. 응용 프로그램, 페이지, 데이터 모델의 구조를 포함 하는 주제를 다룹니다 및 웹 서비스 작업을 호출 합니다.

## <a name="consuming-web-servicesxamarin-formsdata-cloudconsumingindexmd"></a>[웹 서비스 사용](~/xamarin-forms/data-cloud/consuming/index.md)

이 가이드에 제공 하기 위해 다른 웹 서비스와 통신 하는 방법을 보여 줍니다 생성, 읽기, 업데이트 및 삭제 (CRUD) 기능을 Xamarin.Forms 응용 프로그램입니다. 세션에서 다루게 통신할 [ASMX 서비스](consuming/asmx.md), [WCF 서비스](consuming/wcf.md), [REST 서비스](consuming/rest.md), 및 [Azure 모바일 앱](consuming/azure.md)합니다.

## <a name="authenticating-access-to-web-servicesxamarin-formsdata-cloudauthenticationindexmd"></a>[웹 서비스에 대한 액세스 인증](~/xamarin-forms/data-cloud/authentication/index.md)

이 가이드에서는 사용자가 백 엔드만 자신의 데이터에 대 한 액세스 권한을 보유 함으로써 공유할 수 있도록 Xamarin.Forms 응용 프로그램에 인증 서비스를 통합 하는 방법을 설명 합니다. 주제를 다룹니다 [기본 인증을 사용 하 여 REST 서비스와](authentication/rest.md), [Xamarin.Auth 구성 요소를 사용 하 여 OAuth id 공급자에 대 한 인증](authentication/oauth.md), 기본 제공 된 인증을 사용 하 고 제공 되는 메커니즘 [Azure 모바일 앱](authentication/azure.md)합니다.

## <a name="synchronizing-data-with-web-servicessyncindexmd"></a>[웹 서비스와 데이터 동기화](sync/index.md)

이 문서에서는 Xamarin.Forms 응용 프로그램에 오프 라인 동기화 기능을 추가 하는 방법에 설명 합니다. 오프 라인 동기화에는 사용자가 모바일 응용 프로그램, 보기, 추가, 또는 데이터 수정, 상호 작용할 수 있도록 있더라도 네트워크에 연결 하지 않습니다. 변경 내용이 로컬 데이터베이스에 저장 됩니다 및 장치가 온라인 상태 이면 되 면 변경 내용을 웹 서비스와 동기화 할 수 있습니다.

## <a name="sending-push-notificationspush-notificationsindexmd"></a>[푸시 알림 보내기](push-notifications/index.md)

이 문서에서는 Xamarin.Forms 응용 프로그램에 푸시 알림을 추가 하는 방법을 보여줍니다. Azure 알림 허브가 이와 동시에 다른 플랫폼 알림 시스템 통신할 필요가 백 엔드의 복잡성 원하는 모바일 플랫폼으로 모든 백 엔드에서 모바일 푸시 알림을 보내는 경우에 확장 가능한 푸시 인프라를 제공 합니다.

## <a name="storing-files-in-the-cloudstorageindexmd"></a>[클라우드에 파일 저장](storage/index.md)

이 문서에서는 Azure 저장소에 텍스트 및 이진 데이터를 저장 하려면 Xamarin.Forms를 사용 하는 방법과 데이터에 액세스 하는 방법을 보여줍니다. Azure 저장소에는, 구조화 되지 않은 작업과 구조화 된 데이터를 저장 하는 데 사용할 수 있는 확장 가능한 클라우드 저장소 솔루션입니다.

## <a name="searching-data-in-the-cloudsearchindexmd"></a>[클라우드에서 데이터 검색](search/index.md)

이 문서에서는 Azure 검색 Xamarin.Forms 응용 프로그램에 통합 하는 Microsoft Azure 검색 라이브러리를 사용 하는 방법을 보여줍니다. Azure 검색은 인덱싱 및 쿼리 업로드 된 데이터에 대 한 기능을 제공 하는 클라우드 서비스입니다. 인프라 요구 사항 및 응용 프로그램에서 검색 기능을 구현 하는과 관련 된 검색 알고리즘 복잡성 제거 합니다.

## <a name="storing-data-in-a-document-databasecosmosdbindexmd"></a>[문서 데이터베이스에 데이터 저장](cosmosdb/index.md)

이 가이드에는 Azure Cosmos DB 문서 데이터베이스 Xamarin.Forms 응용 프로그램에 통합 하는 Azure Cosmos DB 표준.NET 클라이언트 라이브러리를 사용 하는 방법을 보여 줍니다. Azure Cosmos DB 문서 데이터베이스는 대기 시간이 짧은 원활한 배율과 전역 복제 해야 하는 응용 프로그램에 대 한 빠르고, 항상 사용 가능한 확장 가능한 데이터베이스 서비스를 제공 하는 JSON 문서에 대 한 액세스를 제공 하는 NoSQL 데이터베이스입니다.

## <a name="adding-intelligence-with-cognitive-servicescognitive-servicesindexmd"></a>[Cognitive Services가 포함된 인텔리전스 추가](cognitive-services/index.md)

이 가이드에서는 Xamarin.Forms 응용 프로그램에서 일부 Microsoft Cognitive 서비스 Api를 사용 하는 방법에 설명 합니다. 인식 서비스는 Api, Sdk 및 안 면 인식, 음성 인식 및 언어 이해 같은 기능을 추가 하 여 응용 프로그램 지능적인을 개발자에 게 사용할 수 있는 서비스의 집합입니다.
