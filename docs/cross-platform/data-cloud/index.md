---
title: Microsoft Azure 및 Xamarin
description: 이 문서는 Mac, Azure Mobile Apps, Active Directory 인증 및 WebAPI에 대 한 Visual Studio에서 연결 된 서비스에 대 한 설명서를 링크 합니다.
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: asb3993
ms.author: amburns
ms.date: 10/09/2017
ms.openlocfilehash: 723970a4ad7e2ced85147dbcc6c22f9a45519121
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864608"
---
# <a name="microsoft-azure-and-xamarin"></a>Microsoft Azure 및 Xamarin

[![](images/evolve-mikej-azure-sml.png "Azure App Services 기능은 클라우드 데이터 저장소 및 플랫폼 간 푸시 알림을 포함 하 여 Xamarin 앱을 쉽게 추가할 수")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Evolve 2016: Azure 및 Xamarin을 사용 하 여 연결 된 앱 개발](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Mac용 Visual Studio에서 연결된 서비스

새 [연결 된 서비스](connected-services.md) Mac 용 Visual Studio의 기능 개발자가 쉽고 신속 하 게 IDE 내에서 모바일 응용 프로그램에 Azure 기능을 추가 하는 데 도움이 됩니다. 알파 채널에서 테스트를 위해 현재 사용할 수 있습니다.

## <a name="azure-app-services"></a>Azure App Service

컬렉션이 있기 [Azure Mobile Apps 설명서](~/cross-platform/data-cloud/mobile-apps.md) 구현 하는 과정 안내 하는 [Azure 모바일 클라이언트](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)합니다.
Xamarin에 대 한 Azure 메시징 NuGet 패키지도 제공 [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) 하 고 [Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) 플랫폼에서 푸시 알림을 구현할 수 있도록 합니다.

앱을 구성 합니다 [Azure App Services 포털](https://portal.azure.com/) Mobile Apps, Web Api, 저장소 및 등을 액세스 하 합니다. 에 대 한 자세한 [앱 서비스는 다른 방법을](https://azure.microsoft.com/updates/whats-new-with-azure-app-service/) 의 기능과 [Microsoft에서 이러한 비디오](https://azure.microsoft.com/campaigns/azure-march-announcement/)합니다.

## <a name="active-directory-authentication"></a>Active Directory 인증

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md) 를 통해 Xamarin 앱에서 사용자가 로그인 할 수는 [Xamarin.Auth 구성 요소](https://www.nuget.org/packages/Xamarin.Auth/)합니다.
앱에서 Office 365와 같은 추가 서비스를 액세스할 수 있습니다.

## <a name="webapi"></a>WebAPI

Microsoft의 웹 API에는 Xamarin 응용 프로그램에서 쉽게 사용할 수 있는 REST 유형의 인터페이스를 노출 합니다.
있습니다 수 쉽게 스핀업을 [Azure 웹 사이트](https://trywebsites.azurewebsites.net/) Xamarin 앱에 연결할 WebAPI 기반 앱을 빌드하고 있습니다.


### <a name="introduction-to-web-servicescross-platformdata-cloudweb-servicesindexmd"></a>[웹 서비스 소개](~/cross-platform/data-cloud/web-services/index.md)

이 자습서에서는 REST를 통합 하는 방법 소개 WCF SOAP 웹 서비스 기술 Xamarin 모바일 응용 프로그램을 사용 하 여 합니다. 다양 한 서비스 구현 검사, 사용 가능한 도구 및 라이브러리를 통합 하면 평가 하 고 서비스 데이터를 사용 하기 위한 샘플 패턴을 제공 합니다. 마지막으로, Xamarin 모바일 응용 프로그램을 사용 하 여 소비에 대 한 RESTful 웹 서비스를 만드는 기본 개요를 제공 합니다.

## <a name="samples"></a>샘플

외에 [설명서 샘플](https://github.com/xamarin/mobile-samples/tree/master/Azure), 다음 전체 응용 프로그램에서 Xamarin 앱에 통합 하는 다양 한 Azure 기능을 보여 줍니다.

- [Sport](https://github.com/xamarin/Sport) – 데이터 저장소 및 푸시 알림을 사용 하는 친숙 한 스포츠 리그 추적 앱.
- [잠시 후](https://github.com/pierceboggan/Moments) – 공유를 인스턴트 사진 이미지에 대 한 Azure Storage를 사용 합니다.
- [Xamarin CRM](https://github.com/xamarin/app-crm) -백 엔드에 대 한 Web API를 사용 합니다.
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) -Azure Mobile Apps입니다.

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) 에 대 한 샘플 – 합니다 [아키텍처 시리즈](https://www.microsoft.com/net/learn/architecture) 전자책의 합니다.
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) – Build 2016에서 Azure + IoT 샘플입니다.


## <a name="related-links"></a>관련 링크

- [Azure PCL 예제 (여 @paulbatum) (샘플)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure portal](https://azure.microsoft.com/)
- [Xamarin (NuGet)에 대 한 모바일 클라이언트](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
