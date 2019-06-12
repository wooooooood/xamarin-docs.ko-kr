---
title: 푸시 알림 보내기
description: 푸시 알림은 응용 프로그램 계약 및 사용량을 높이기 위해 백 엔드 시스템에서 모바일 장치의 응용 프로그램으로 메시지와 같은 정보를 전송하는 데 사용됩니다. 알림은 사용자가 알림을 사용하는 대상 응용 프로그램을 활성화 하지 않은 경우에도 언제든지 보낼 수 있습니다.
ms.prod: xamarin
ms.assetid: F58D2D81-FFAF-43DD-8A9B-3684DFEAA99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/29/2019
ms.openlocfilehash: 8d26a582e68fb557d20d7bfa690bbf4acfe307c1
ms.sourcegitcommit: c2bba24233624c2ec0e9ee9827310ca022212a2c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66835299"
---
# <a name="sending-push-notifications"></a>푸시 알림 보내기

_푸시 알림은 응용 프로그램 계약 및 사용량을 높이기 위해 백 엔드 시스템에서 모바일 장치의 응용 프로그램으로 메시지와 같은 정보를 전송하는 데 사용됩니다. 알림은 사용자가 알림을 사용하는 대상 응용 프로그램을 활성화하지 않은 경우에도 언제든지 보낼 수 있습니다._

## <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-xamarinformsazure-notification-hubmd"></a>[Azure Notification Hubs 및 Xamarin.Forms를 사용 하 여 푸시 알림을 보내고 받도록](azure-notification-hub.md)

Azure Notification Hubs를 사용 하면 백 엔드 응용 프로그램은 단일 허브와 통신할 수 있도록 플랫폼 알림을 중앙 집중화할 수 있습니다. Azure Notification Hubs 푸시 알림을 여러 플랫폼 공급자를 배포 하는 주의 해야 합니다. 이 문서에서는 Azure Notification Hubs Xamarin.Forms 응용 프로그램에 통합 하는 방법에 설명 합니다.

## <a name="send-push-notifications-from-azure-mobile-appsazuremd"></a>[Azure 모바일 앱에서 푸시 알림 보내기](azure.md)

Azure Mobile Apps 푸시 알림을 보낼 수 있는 확장 가능한 백 엔드를 제공 하는 Azure Notification Hubs와 통합 합니다. Azure Notification Hubs는 Apple 및 Google 등의 개별 푸시 알림 서비스와 통신의 복잡성을 제거 합니다.