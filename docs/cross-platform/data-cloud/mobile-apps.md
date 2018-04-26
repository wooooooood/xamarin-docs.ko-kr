---
title: Microsoft Azure 모바일 앱
description: Azure 포털 문서에 대 한 샘플 및 코드를 다운로드 합니다.
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
ms.technology: xamarin-cross-platform
author: conceptdev
ms.author: crdun
ms.date: 04/02/2017
ms.openlocfilehash: 4a6211ac218d914089bf1d0dbcb9956800dde700
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2018
---
# <a name="microsoft-azure-mobile-apps"></a>Microsoft Azure 모바일 앱

_Azure 포털 문서에 대 한 샘플 및 코드를 다운로드 합니다._

<!--
NOTE TO AUTHORS: this page is referenced from
http://azure.microsoft.com/develop/mobile/xamarin/
as https://developer.xamarin.com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started  http://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data   http://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push   http://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication http://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs  http://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data    http://go.microsoft.com/fwlink/p/?LinkId=331330
-->


이러한 링크는에서 사용할 수 있는 Xamarin 설명서의 [Azure 모바일 앱](https://docs.microsoft.com/azure/app-service-mobile/) 웹 사이트입니다.
Azure 기능을 Xamarin 앱을 다운로드 하 여 추가 된 [Azure 모바일 클라이언트](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)합니다.

## <a name="working-with-the-xamarin-azure-component"></a>Xamarin Azure 구성 요소 작업

일반 설명서 [Xamarin 클라이언트 라이브러리 (구성 요소)를 사용](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library) Azure 모바일 앱으로 다양 한 작업을 수행 합니다. 이 페이지에 대 한 세부 정보 및 예 각 아래 나열 된 연습 문서에서 사용 하지 않고 샘플 코드 조각 많이 포함 되어 있습니다.

## <a name="getting-started"></a>시작

이 문서에서는 첫 Xamarin Azure 응용 프로그램 시작 및 실행 하기 위한 단계별 지침을 제공 합니다.
포털에서 새 Azure 모바일 앱을 만드는 다음 다운로드 및 미리 구성 된 앱을 실행 하는 내용을 다룹니다.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started)

<!--
## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

-  [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-->

<!--
## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

-  [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-->

## <a name="get-started-with-users"></a>사용자 시작

구성 및 Azure 모바일 서비스를 사용 하 여 로그인 화면 코딩을 위한 전체 지침을 제공 합니다. 지원 되는 인증 공급자는 Microsoft, Google, Facebook 및 Twitter에 포함 됩니다.

-  [iOS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>사용자 스크립트에서 권한 부여

Javascript 백 엔드에 대 한 몇 가지 샘플 코드

-  [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>강제 시작

Apple 및 Google 웹 사이트에 푸시 알림을 구성 하 고 장치에 Azure 모바일 서비스에서 푸시 알림을 보내야 하는 지침을 완료 합니다.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)


## <a name="get-started-with-notification-hubs"></a>알림 허브 시작

하 Apple 및 Google 웹 사이트에서 푸시 알림을 구성 하 고, Azure 알림 허브를 구성 하 고, 장치에 푸시 알림을 생성 한 다음 지침을 완료 합니다.

-  [iOS](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
-  [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)



## <a name="related-links"></a>관련 링크

- [GettingStarted (샘플)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (샘플)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (샘플)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (샘플)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [NotificationHubs (샘플)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure 모바일 클라이언트](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Azure 모바일 앱 학습 경로](https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->