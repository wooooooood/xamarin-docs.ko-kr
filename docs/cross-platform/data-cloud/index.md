---
title: 마이크로소프트 Azure와 자마린
description: 이 문서는 Mac용 Visual Studio, Azure 모바일 앱, Active Directory 인증 및 WebAPI에 대한 커넥티드 서비스에 대한 설명서에 연결됩니다.
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: davidortinau
ms.author: daortin
ms.date: 10/09/2017
ms.openlocfilehash: 2b6dfeb0de0fac59556280609dbf870c23a9298b
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388527"
---
# <a name="microsoft-azure-and-xamarin"></a>마이크로소프트 Azure와 자마린

[![](images/evolve-mikej-azure-sml.png "Azure App Services features are easy to add to Xamarin apps, including cloud data storage and cross-platform push notifications")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[진화 2016: Azure 및 Xamarin을 사용 하 여 연결 된 애플 리 케이 션 개발](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Mac용 Visual Studio에서 연결된 서비스

Mac용 Visual Studio의 새로운 [커넥티드 서비스](connected-services.md) 기능은 개발자가 IDE 내에서 모바일 응용 프로그램에 Azure 기능을 빠르고 쉽게 추가할 수 있도록 도와줍니다. 현재 알파 채널에서 테스트할 수 있습니다.

## <a name="azure-app-services"></a>Azure App Services

Azure Mobile 클라이언트를 구현하는 프로세스를 안내하는 [Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) [Apps 설명서](~/cross-platform/data-cloud/mobile-apps.md) 컬렉션이 있습니다.
Xamarin은 또한 플랫폼 간에 푸시 알림을 구현하는 데 도움이 되는 [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) 및 [Android용](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) Azure 메시징 NuGet 패키지를 제공합니다.

[Azure 앱 서비스 포털에서](https://portal.azure.com/) 앱을 구성하여 모바일 앱, 웹 API, 저장소 등에 액세스합니다. 앱 [서비스가 어떻게 다른지](https://azure.microsoft.com/updates/whats-new-with-azure-app-service/) 알아보고 [Microsoft의 이 비디오에서](https://azure.microsoft.com/campaigns/azure-march-announcement/)시청합니다.

## <a name="active-directory-authentication"></a>Active Directory 인증

[Azure Active Directory를](~/cross-platform/data-cloud/active-directory/index.md) 사용하여 사용자를 Xamarin 앱에 로그인할 수 있습니다. 그런 다음 앱은 Office 365와 같은 추가 서비스에 액세스할 수 있습니다.

## <a name="webapi"></a>웹 API

Microsoft의 웹 API는 Xamarin 응용 프로그램에서 쉽게 사용할 수 있는 REST와 유사한 인터페이스를 노출합니다.
[Azure 웹 사이트를](https://trywebsites.azurewebsites.net/) 쉽게 스핀업하고 WebAPI 기반 앱을 빌드하여 Xamarin 앱에 연결할 수 있습니다.

### <a name="introduction-to-web-services"></a>[웹 서비스 소개](~/cross-platform/data-cloud/web-services/index.md)

이 자습서에서는 REST, WCF 및 SOAP 웹 서비스 기술을 Xamarin 모바일 응용 프로그램과 통합하는 방법을 소개합니다. 다양한 서비스 구현을 검사하고, 통합할 사용 가능한 도구 및 라이브러리를 평가하고, 서비스 데이터를 소비하기 위한 샘플 패턴을 제공합니다. 마지막으로 Xamarin 모바일 응용 프로그램과 함께 소비를 위한 RESTful 웹 서비스를 만드는 기본 개요를 제공합니다.

## <a name="samples"></a>샘플

[설명서 샘플](https://github.com/xamarin/mobile-samples/tree/master/Azure)외에도 다음 전체 응용 프로그램은 Xamarin 앱에 통합된 다양한 Azure 기능을 보여 줍니다.

- [스포츠](https://github.com/xamarin/Sport) - 푸시 알림에 & 데이터 저장을 사용하는 친절한 스포츠 리그 추적 응용 프로그램.
- [모멘트](https://github.com/pierceboggan/Moments) – 이미지에 Azure 저장소를 사용하는 인스턴트 사진 공유.
- [Xamarin CRM](https://github.com/xamarin/app-crm) – 백 엔드에 웹 API를 사용합니다.
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) - Azure 모바일 앱.

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) - 전자 책의 [아키텍처 시리즈에](https://www.microsoft.com/net/learn/architecture) 대한 샘플.
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) - 빌드 2016의 Azure + IoT 샘플입니다.

## <a name="related-links"></a>관련 링크

- [Azure PCL 예제(별) @paulbatum(샘플)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure Portal](https://azure.microsoft.com/)
- [자마린을 위한 모바일 클라이언트 (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
