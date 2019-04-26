---
title: Xamarin.Android에서 알림
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: b98ee5afbd65d5cf32bc6e3151284678e248cf47
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61014253"
---
# <a name="notifications-in-xamarinandroid"></a>Xamarin.Android에서 알림


## <a name="overview"></a>개요

이 섹션에서는 Xamarin.Android에서 알림을 구현 하는 방법에 설명 합니다. 여러 UI 요소에는 Android 알림을 설명 하 고 API에 설명의 만들기 및 알림을 표시를 사용 하 여 관련 됩니다.


## <a name="sections"></a>섹션

### <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Android에서 로컬 알림](local-notifications.md)

이 섹션에서는 Xamarin.Android에서 로컬 알림을 구현 하는 방법에 설명 합니다. 여러 UI 요소에는 Android 알림을 설명 하 고 API에 설명의 만들기 및 알림을 표시를 사용 하 여 관련 됩니다. 

### <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[연습-Xamarin.Android에서 로컬 알림 사용](local-notifications-walkthrough.md)  
 
이 연습에서는 Xamarin.Android 응용 프로그램에서 로컬 알림을 사용 하는 방법에 설명 합니다. 만들기 및 알림을 게시의 기본 사항을 보여 줍니다. 알림 서랍에서 알림을 클릭할 때 두 번째 작업을 시작 합니다. 


## <a name="for-further-reading"></a>추가 정보

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; FCM Firebase Cloud Messaging ()는 모바일 앱 및 서버 응용 프로그램 간의 메시징 원활 하 게 하는 서비스입니다. Firebase Cloud Messaging 사용할 수 있습니다 (푸시 알림 라고도 함) 원격 알림을 구현 하려면 Xamarin.Android 응용 프로그램.

[알림을](https://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; 이 Android 개발자 항목은 Android 알림에 대 한 최선의 가이드입니다. Android 사용자 인터페이스의 지침을 적절 하 게 도움이 되는 고려 사항 섹션에 알림을 디자인 디자인을 포함 합니다. 활동을 시작할 때 preserviing 탐색에 대 한 자세한 배경 정보를 제공 하 고 잠금 화면에서 알림 및 제어 미디어 재생 진행률을 표시 하는 방법을 설명 합니다. 

[NotificationListenerService](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) &ndash; 이 Android 서비스를 사용 하면 수신 대기할 (및 상호 작용) 앱에 대 한 Android 장치에서 앱에 등록 된 알림 뿐 아니라 게시 된 모든 알림을 수신 합니다. 사용자 장치에 대 한 알림을 수신할 수 있도록 앱에 권한을 명시적으로 제공 해야 하는 참고 합니다.





## <a name="related-links"></a>관련 링크

- [로컬 알림 (샘플)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [원격 알림 (샘플)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications/)
