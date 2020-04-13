---
title: 자마린.양식 및 웹 서비스
description: 이 가이드에서는 Xamarin.Forms 응용 프로그램에 CRUD()(를) 만드는 기능을 제공하기 위해 다른 웹 서비스와 통신하는 방법을 설명합니다. 주제는 ASMX 서비스, WCF 서비스, REST 서비스와의 통신을 포함합니다.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: 799a0a97f7c1a212b10dc62f8b3e7bd2cf2060c8
ms.sourcegitcommit: bf3b5925018e1bd6a00505e9f37500761b66809d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80395452"
---
# <a name="xamarinforms-and-web-services"></a>자마린.양식 및 웹 서비스

## <a name="introduction"></a>[소개](introduction.md)

이 문서에서는 다른 웹 서비스와 통신하는 방법을 보여 주는 Xamarin.Forms 샘플 응용 프로그램의 연습을 제공합니다. 응용 프로그램의 해부학, 페이지, 데이터 모델 및 웹 서비스 작업을 호출하는 항목이 다룹니다.

## <a name="consume-an-aspnet-web-service-asmx"></a>[ASMX(ASP.NET 웹 서비스) 사용](~/xamarin-forms/data-cloud/web-services/asmx.md)

ASP.NET 웹 서비스(ASMX)는 SOAP(단순 개체 액세스 프로토콜)를 사용하여 HTTP를 통해 메시지를 보내는 웹 서비스를 빌드하는 기능을 제공합니다. SOAP는 웹 서비스를 구축하고 액세스하기 위한 플랫폼 독립적이고 언어 독립적인 프로토콜입니다. ASMX 서비스의 소비자는 서비스를 구현하는 데 사용되는 플랫폼, 개체 모델 또는 프로그래밍 언어에 대해 알 필요가 없습니다. SOAP 메시지를 보내고 받는 방법만 이해해야 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 ASMX 웹 서비스를 사용하는 방법을 보여 줍니다.

## <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>[WCF(Windows 통신 기반) 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF는 서비스 지향 응용 프로그램을 빌드하기 위한 Microsoft의 통합 프레임워크입니다. 이를 통해 개발자는 안전하고 신뢰할 수 있으며 트랜잭션이 가능하며 상호 운용 가능한 분산 응용 프로그램을 구축할 수 있습니다. ASP.NET 웹 서비스(ASMX)와 WCF 간에는 차이가 있지만 WCF는 ASMX가 제공하는 것과 동일한 기능을 지원한다는 것을 이해하는 것이 중요합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 WCF SOAP 서비스를 사용하는 방법을 보여 줍니다.

## <a name="consume-a-restful-web-service"></a>[RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/rest.md)

대리 상태 전송(REST)은 웹 서비스를 빌드하기 위한 아키텍처 스타일입니다. REST 요청은 웹 브라우저가 웹 페이지를 검색하고 서버로 데이터를 보내는 데 사용하는 것과 동일한 HTTP 동사를 사용하여 HTTP를 통해 이루어집니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 RESTful 웹 서비스를 사용하는 방법을 보여 줍니다.
