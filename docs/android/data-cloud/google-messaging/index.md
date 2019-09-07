---
title: Google 메시징
description: 이 섹션에는 Google 메시징 서비스를 사용 하 여 Xamarin Android 앱을 구현 하는 방법을 설명 하는 가이드가 포함 되어 있습니다.
ms.prod: xamarin
ms.assetid: 85E8DF92-D160-4763-A7D3-458B4C31635F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/12/2018
ms.openlocfilehash: bfcc526d1787caede4361030f5bf6718a59672b1
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754384"
---
# <a name="google-messaging"></a>Google 메시징

_이 섹션에는 Google 메시징 서비스를 사용 하 여 Xamarin Android 앱을 구현 하는 방법을 설명 하는 가이드가 포함 되어 있습니다._

## <a name="firebase-cloud-messagingfirebase-cloud-messagingmd"></a>[Firebase 클라우드 메시징](firebase-cloud-messaging.md)

FCM (Firebase Cloud Messaging)는 모바일 앱과 서버 응용 프로그램 간의 메시징을 용이 하 게 하는 서비스입니다. FCM Google Cloud Messaging Google의 후속 작업입니다. 이 문서에서는 FCM가 작동 하는 방식에 대 한 개요를 제공 하 고, 앱에서 FCM 서비스를 사용할 수 있도록 자격 증명을 획득 하는 단계별 절차를 제공 합니다.

## <a name="remote-notifications-with-firebase-cloud-messagingremote-notifications-with-fcmmd"></a>[Firebase 클라우드 메시징으로 원격 알림](remote-notifications-with-fcm.md)

이 연습에서는 Firebase 클라우드 메시징을 사용 하 여 Xamarin Android 응용 프로그램에서 원격 알림 (푸시 알림)을 구현 하는 방법에 대 한 단계별 설명을 제공 합니다. FCM (Firebase Cloud Messaging)와 통신 하는 데 필요한 다양 한 클래스를 구현 하는 방법을 보여 주고, FCM에 대 한 액세스를 위해 Android 매니페스트를 구성 하는 방법에 대 한 예제를 제공 하 고, Firebase를 사용 하 여 다운스트림 메시징을 보여줍니다. 콘솔이.

## <a name="google-cloud-messaginggoogle-cloud-messagingmd"></a>[Google Cloud Messaging](google-cloud-messaging.md)

이 섹션에서는 앱과 앱 서버 간에 메시지를 라우팅하 Google Cloud Messaging는 방법에 대 한 개략적인 개요를 제공 하 고, 앱에서 GCM 서비스를 사용할 수 있도록 자격 증명을 획득 하는 단계별 절차를 제공 합니다. GCM은 FCM에 의해 대체 되었습니다.

> [!NOTE]
> GCM은 FCM ( [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) )에 의해 대체 되었습니다.
> GCM 서버 및 클라이언트 Api [는 더 이상 사용](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) 되지 않으며, 2019 년 4 월 11 일에는 더 이상 사용할 수 없습니다.

## <a name="remote-notifications-with-google-cloud-messagingremote-notifications-with-gcmmd"></a>[Google Cloud Messaging로 원격 알림](remote-notifications-with-gcm.md)

이 섹션에서는 Google Cloud Messaging를 사용 하 여 Xamarin.ios에서 원격 알림을 구현 하는 방법에 대 한 단계별 설명을 제공 합니다.
Android 응용 프로그램에서 Google Cloud Messaging를 사용 하도록 설정 하기 위해 활용 해야 하는 다양 한 구성 요소에 대해 설명 합니다.
