---
title: 데이터 및 클라우드 서비스
description: Xamarin.Forms 응용 프로그램은 다양 한 기술 사용 하 여 구현한 웹 서비스를 사용할 수 있으며,이 가이드에는이 작업을 수행 하는 방법을 살펴봅니다.
ms.prod: xamarin
ms.assetid: 0601D9D0-C8D2-4C3B-A749-A340BDBF64A4ß
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 6e67820fa83ddea46f934b4eaedde2c6334f9cc6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61327508"
---
# <a name="data--cloud-services"></a>데이터 및 클라우드 서비스

_Xamarin.Forms 응용 프로그램은 다양 한 기술 사용 하 여 구현한 웹 서비스를 사용할 수 있으며,이 가이드에는이 작업을 수행 하는 방법을 살펴봅니다._

Xamarin 플랫폼에서 플랫폼 간 웹 서비스 사용에 대 한 소개를 참조 하세요 [웹 서비스 소개](~/cross-platform/data-cloud/web-services/index.md)합니다.

## <a name="understanding-the-samplexamarin-formsdata-cloudwalkthroughmd"></a>[샘플 이해](~/xamarin-forms/data-cloud/walkthrough.md)

이 문서에서는 Xamarin.Forms 샘플 응용 프로그램이 다른 웹 서비스와 통신 하는 방법을 보여 주는 연습을 제공 합니다. 다룹니다 응용 프로그램 페이지, 데이터 모델을 분석 하 고 웹 서비스 작업을 호출 합니다.

## <a name="consuming-web-servicesxamarin-formsdata-cloudconsumingindexmd"></a>[웹 서비스 사용](~/xamarin-forms/data-cloud/consuming/index.md)

이 가이드에 제공 하는 다른 웹 서비스와 통신 하는 방법을 보여 줍니다. 만들기, 읽기, 업데이트 및 삭제 (CRUD) Xamarin.Forms 응용 프로그램에는 기능입니다. 다룹니다 통신할 [ASMX 서비스](consuming/asmx.md), [WCF services](consuming/wcf.md)를 [REST 서비스](consuming/rest.md), 및 [Azure Mobile Apps](consuming/azure.md)합니다.

## <a name="authenticating-access-to-web-servicesxamarin-formsdata-cloudauthenticationindexmd"></a>[웹 서비스에 대한 액세스 인증](~/xamarin-forms/data-cloud/authentication/index.md)

이 가이드만 자신의 데이터에 액세스할 수 있는 백 엔드를 공유할 수 있도록 하려면 Xamarin.Forms 응용 프로그램에 인증 서비스를 통합 하는 방법에 설명 합니다. 다루는 [기본 인증을 사용 하 여 REST 서비스를 사용 하 여](authentication/rest.md), [Xamarin.Auth 구성 요소를 사용 하 여 OAuth id 공급자에 대 한 인증](authentication/oauth.md), 기본 제공된 인증을 사용 하 고 제공 하는 메커니즘 [Azure Mobile Apps](authentication/azure.md)합니다.

## <a name="synchronizing-data-with-web-servicessyncindexmd"></a>[웹 서비스와 데이터 동기화](sync/index.md)

이 문서에서는 Xamarin.Forms 응용 프로그램에 오프 라인 동기화 기능을 추가 하는 방법에 설명 합니다. 오프 라인 동기화 하면 모바일 응용 프로그램, 보기, 추가, 또는 데이터 수정, 상호 작용할 수 없는 네트워크에 연결 하는 경우에 합니다. 변경 내용은 로컬 데이터베이스에 저장 됩니다 하 고 장치가 온라인 상태가 되 면 웹 서비스를 사용 하 여 변경 내용을 동기화 할 수 있습니다.

## <a name="sending-push-notificationspush-notificationsindexmd"></a>[푸시 알림 보내기](push-notifications/index.md)

이 문서에서는 Xamarin.Forms 응용 프로그램에 푸시 알림 추가 하는 방법에 설명 합니다. Azure 알림 허브는 다양한 플랫폼 알림 시스템과 통신해야 하는 백 엔드의 복잡성을 제거하면서 모든 백 엔드에서 모든 모바일 플랫폼으로 모바일 푸시 알림을 전송할 수 있는 확장 가능한 푸시 인프라를 제공합니다.

## <a name="storing-files-in-the-cloudstorageindexmd"></a>[클라우드에 파일 저장](storage/index.md)

이 문서에서는 Xamarin.Forms를 사용 하 여 Azure Storage에 텍스트 및 이진 데이터를 저장 하는 방법을 데이터에 액세스 하는 방법을 보여 줍니다. Azure Storage는 비 구조화 및 구조화 된 데이터를 저장 하는 데 사용할 수 있는 확장 가능한 클라우드 저장소 솔루션입니다.

## <a name="searching-data-in-the-cloudsearchindexmd"></a>[클라우드에서 데이터 검색](search/index.md)

이 문서에서는 Microsoft Azure Search 라이브러리를 사용 하 여 Xamarin.Forms 응용 프로그램에 Azure Search를 통합 하는 방법에 설명 합니다. Azure Search는 인덱싱 및 쿼리 업로드 된 데이터에 대 한 기능을 제공 하는 클라우드 서비스입니다. 인프라 요구 사항 및 응용 프로그램에 검색 기능을 구현 하 여 일반적으로 관련 된 검색 알고리즘 복잡성이 제거 됩니다.

## <a name="storing-data-in-a-document-databasecosmosdbindexmd"></a>[문서 데이터베이스에 데이터 저장](cosmosdb/index.md)

이 가이드에는 Azure Cosmos DB.NET Standard 클라이언트 라이브러리를 사용 하 여 Xamarin.Forms 응용 프로그램에 Azure Cosmos DB 문서 데이터베이스를 통합 하는 방법을 보여 줍니다. Azure Cosmos DB 문서 데이터베이스는 원활한 확장 및 전역 복제가 필요한 응용 프로그램에 대 한 빠른, 고가용성, 확장성 있는 데이터베이스 서비스를 제공 하는 JSON 문서에 대 한 대기 시간이 짧은 액세스를 제공 하는 NoSQL 데이터베이스.

## <a name="adding-intelligence-with-cognitive-servicescognitive-servicesindexmd"></a>[Cognitive Services가 포함된 인텔리전스 추가](cognitive-services/index.md)

이 가이드에서는 Xamarin.Forms 응용 프로그램에서 Microsoft Cognitive Services Api 중 일부를 사용 하는 방법을 설명 합니다. Cognitive Services는 Api, Sdk 및 얼굴 인식, 음성 인식 및 언어 이해와 같은 기능을 추가 하 여 더 지능적인 응용 프로그램을 확인 하는 개발자에 게 사용할 수 있는 서비스의 집합입니다.
