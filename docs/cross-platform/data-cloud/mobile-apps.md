---
title: Microsoft Azure Mobile Apps
description: 이 문서는 Azure에 연결 되어 있는 Xamarin 앱을 빌드하는 방법을 설명 하는 가이드에 연결 합니다. Xamarin Azure 구성 요소, 사용자 및 푸시 알림을 사용 하 여 작업에 설명 합니다.
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
author: conceptdev
ms.author: crdun
ms.date: 04/02/2017
ms.openlocfilehash: a1a0b078659441f0f45af66728a5f37d578d6274
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675093"
---
# <a name="microsoft-azure-mobile-apps"></a>Microsoft Azure Mobile Apps

_Azure portal 설명서에 대 한 샘플 및 코드 다운로드 합니다._

<!--
NOTE TO AUTHORS: this page is referenced from
https://azure.microsoft.com/develop/mobile/xamarin/
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


이러한 링크는에서 제공 되는 Xamarin 설명서의 [Azure Mobile Apps](https://docs.microsoft.com/azure/app-service-mobile/) 웹 사이트입니다.
다운로드 하 여 Xamarin 앱에 Azure 기능을 추가 합니다 [Azure 모바일 클라이언트](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)합니다.

## <a name="working-with-the-xamarin-azure-component"></a>Xamarin Azure 구성 요소 작업

일반 설명서 [Xamarin 클라이언트 라이브러리 (구성 요소)를 사용 하 여 작업](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library) Azure Mobile Apps를 사용 하 여 다양 한 작업을 수행 합니다. 이 페이지에는 많은 샘플 코드 조각 없이 자세한 설명 및 각 아래에 나열 된 연습 문서에서 사용할 수 있는 예제를 포함 합니다.

## <a name="getting-started"></a>시작

이 문서에서는 첫 번째 Xamarin Azure 앱을 실행 하기 위한 단계별 지침을 제공 합니다.
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

구성 및 Azure Mobile Services를 사용 하 여 로그인 화면을 코딩에 대 한 전체 지침을 제공 합니다. 지원 되는 인증 공급자는 Microsoft, Google, Facebook 및 Twitter 포함 합니다.

-  [iOS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>스크립트에서 사용자 권한 부여

Javascript 백 엔드에 대 한 몇 가지 샘플 코드

-  [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>푸시 시작

Apple 및 Google 웹 사이트에서 푸시 알림을 구성 하 고 장치에 Azure Mobile Services에서 푸시 알림을 보내기에 대 한 지침을 완료 합니다.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)


## <a name="get-started-with-notification-hubs"></a>Notification Hubs 시작

Apple 및 Google 웹 사이트의 푸시 알림 구성, Azure 알림 허브를 구성 및 장치에 푸시 알림을 생성 한 다음 지침을 완료 합니다.

-  [iOS](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
-  [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)



## <a name="related-links"></a>관련 링크

- [GettingStarted (샘플)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (샘플)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (샘플)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (샘플)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [NotificationHubs (샘플)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Azure Mobile Apps 학습 경로](https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->
