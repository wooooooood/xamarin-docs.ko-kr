---
title: Xamarin.Forms및 웹 서비스
description: 이 가이드에서는 다른 웹 서비스와 통신 하 여 응용 프로그램에 CRUD (만들기, 읽기, 업데이트 및 삭제) 기능을 제공 하는 방법을 설명 합니다 Xamarin.Forms . 이 항목에서는 ASMX 서비스, WCF 서비스, REST 서비스와의 통신을 다룹니다.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5b2613b94d2c347d9bc6a94086f869b07ab8a55b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84131887"
---
# <a name="xamarinforms-and-web-services"></a>Xamarin.Forms및 웹 서비스

## <a name="introduction"></a>[소개](introduction.md)

이 문서에서는 Xamarin.Forms 다양 한 웹 서비스와 통신 하는 방법을 보여 주는 샘플 응용 프로그램의 연습을 제공 합니다. 응용 프로그램, 페이지, 데이터 모델 및 호출 웹 서비스 작업을 분석 하는 항목을 다룹니다.

## <a name="consume-an-aspnet-web-service-asmx"></a>[ASMX(ASP.NET 웹 서비스) 사용](~/xamarin-forms/data-cloud/web-services/asmx.md)

ASMX (ASP.NET Web Services)는 SOAP (Simple Object Access Protocol)를 사용 하 여 HTTP를 통해 메시지를 보내는 웹 서비스를 빌드하는 기능을 제공 합니다. SOAP는 웹 서비스를 빌드하고 액세스 하기 위한 플랫폼 독립적이 고 언어에 독립적인 프로토콜입니다. ASMX 서비스 소비자는 서비스를 구현 하는 데 사용 되는 플랫폼, 개체 모델 또는 프로그래밍 언어에 대해 알 필요가 없습니다. SOAP 메시지를 송수신 하는 방법을 이해 해야 합니다. 이 문서에서는 응용 프로그램에서 ASMX 웹 서비스를 사용 하는 방법을 보여 줍니다 Xamarin.Forms .

## <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>[WCF (Windows Communication Foundation) 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF는 서비스 지향 응용 프로그램을 빌드하기 위한 Microsoft의 통합 프레임 워크입니다. 이를 통해 개발자는 안전 하 고 신뢰할 수 있으며 트랜잭션 된 분산 응용 프로그램을 빌드할 수 있습니다. ASP.NET 웹 서비스 (ASMX)와 WCF 간에는 차이점이 있지만, WCF는 ASMX에서 제공 하는 것과 동일한 기능 (HTTP를 통한 SOAP 메시지)을 지원 한다는 것을 이해 하는 것이 중요 합니다. 이 문서에서는 응용 프로그램에서 WCF SOAP 서비스를 사용 하는 방법을 보여 줍니다 Xamarin.Forms .

## <a name="consume-a-restful-web-service"></a>[RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/rest.md)

REST (Representational State Transfer)는 웹 서비스를 빌드하기 위한 아키텍처 스타일입니다. REST 요청은 웹 브라우저에서 웹 페이지를 검색 하 고 서버에 데이터를 보내는 데 사용 하는 것과 동일한 HTTP 동사를 사용 하 여 HTTP를 통해 수행 됩니다. 이 문서에서는 응용 프로그램에서 RESTful 웹 서비스를 사용 하는 방법을 보여 줍니다 Xamarin.Forms .
