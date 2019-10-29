---
title: Microsoft Azure 및 Xamarin
description: 이 문서는 Mac용 Visual Studio, Azure Mobile Apps, Active Directory 인증 및 WebAPI의 연결된 서비스에 대 한 설명서에 연결 되어 있습니다.
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: davidortinau
ms.author: daortin
ms.date: 10/09/2017
ms.openlocfilehash: 273a1a8fec4cf40893ff94fef4b1394065a8547b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016618"
---
# <a name="microsoft-azure-and-xamarin"></a>Microsoft Azure 및 Xamarin

[![](images/evolve-mikej-azure-sml.png "Azure App Services features are easy to add to Xamarin apps, including cloud data storage and cross-platform push notifications")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[진화 2016: Azure 및 Xamarin을 사용 하 여 연결 된 앱 개발](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Mac용 Visual Studio에서 연결된 서비스

개발자는 Mac용 Visual Studio의 새로운 [연결된 서비스](connected-services.md) 기능을 사용 하 여 IDE 내에서 모바일 응용 프로그램에 Azure 기능을 빠르고 쉽게 추가할 수 있습니다. 현재 알파 채널에서 테스트에 사용할 수 있습니다.

## <a name="azure-app-services"></a>Azure 앱 서비스

Azure [모바일 클라이언트](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)를 구현 하는 과정을 안내 하는 [azure Mobile Apps 설명서](~/cross-platform/data-cloud/mobile-apps.md) 모음이 있습니다.
또한 Xamarin은 플랫폼 간에 푸시 알림을 구현 하는 데 도움이 되는 [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) 및 [Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) 용 Azure Messaging NuGet 패키지를 제공 합니다.

[Azure 앱 Services 포털](https://portal.azure.com/) 에서 Mobile Apps, Web Api, Storage 등에 액세스할 수 있는 앱을 구성 합니다. [App services가 어떻게 다르고](https://azure.microsoft.com/updates/whats-new-with-azure-app-service/) [Microsoft의 이러한 비디오](https://azure.microsoft.com/campaigns/azure-march-announcement/)를 시청 하는지 알아보세요.

## <a name="active-directory-authentication"></a>Active Directory 인증

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md) 는 Xamarin.ios [구성 요소](https://www.nuget.org/packages/Xamarin.Auth/)를 통해 xamarin 앱에서 사용자를 로그인 하는 데 사용할 수 있습니다.
그러면 앱에서 Office 365와 같은 추가 서비스에 액세스할 수 있습니다.

## <a name="webapi"></a>webAPI

Microsoft Web API는 Xamarin 응용 프로그램에서 쉽게 사용할 수 있는 REST와 유사한 인터페이스를 제공 합니다.
[Azure 웹 사이트](https://trywebsites.azurewebsites.net/) 를 쉽게 스핀 하 고 WebAPI 기반 앱을 빌드하여 Xamarin 앱에 연결할 수 있습니다.

### <a name="introduction-to-web-servicescross-platformdata-cloudweb-servicesindexmd"></a>[웹 서비스 소개](~/cross-platform/data-cloud/web-services/index.md)

이 자습서에서는 REST, WCF 및 SOAP 웹 서비스 기술을 Xamarin mobile 응용 프로그램과 통합 하는 방법을 소개 합니다. 다양 한 서비스 구현을 검사 하 고, 사용 가능한 도구 및 라이브러리를 평가 하 여 통합 하 고, 서비스 데이터를 사용 하기 위한 샘플 패턴을 제공 합니다. 마지막으로, Xamarin 모바일 응용 프로그램을 사용 하기 위해 RESTful 웹 서비스를 만드는 기본적인 개요를 제공 합니다.

## <a name="samples"></a>샘플

다음 전체 응용 프로그램은 [설명서 샘플](https://github.com/xamarin/mobile-samples/tree/master/Azure)외에도 Xamarin 앱에 통합 된 다양 한 Azure 기능을 보여 줍니다.

- [스포츠](https://github.com/xamarin/Sport) – & 푸시 알림을 사용 하는 데이터 저장소를 사용 하는 리그 추적 앱입니다.
- [분](https://github.com/pierceboggan/Moments) -이미지에 대 한 Azure Storage를 사용 하는 인스턴트 사진 공유
- [XAMARIN CRM](https://github.com/xamarin/app-crm) – 백 엔드에 웹 API를 사용 합니다.
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) -Azure Mobile Apps.

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) – Ebooks의 [아키텍처 시리즈](https://www.microsoft.com/net/learn/architecture) 에 대 한 샘플입니다.
- [Mydriving](https://azure.microsoft.com/campaigns/mydriving/) – Azure + IoT 샘플 (2016 빌드)

## <a name="related-links"></a>관련 링크

- [Azure PCL 예제 (@paulbatum) (샘플)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure Portal](https://azure.microsoft.com/)
- [Xamarin 용 모바일 클라이언트 (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
