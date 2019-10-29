---
title: Xamarin.ios에서 사용 되지 않는 알림 기술
description: 이 문서에서는 iOS 10에 도입 된 사용자 알림 프레임 워크를 위해 더 이상 사용 되지 않는 iOS 알림 기술에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 20C4F6E5-56DF-4A85-BBF0-E38C88586307
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/07/2016
ms.openlocfilehash: de9a46587a5d1de6f12dd54122b27e53694cdeb8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031397"
---
# <a name="deprecated-notification-technologies-in-xamarinios"></a>Xamarin.ios에서 사용 되지 않는 알림 기술

이 섹션에서는 Xamarin.ios에서 로컬 및 푸시 알림을 구현 하는 방법을 보여 줍니다. IOS 알림의 다양 한 UI 요소에 대해 설명 하 고 알림을 만들고 표시 하는 것과 관련 된 API에 대해 설명 합니다.

> [!IMPORTANT]
> 이 섹션의 정보는 iOS 9 및 이전 버전과 관련이 있으며 이전 iOS 버전을 지원 하기 위해 여기에 남아 있습니다. IOS 10 이상에서는 iOS 장치에서 로컬 알림과 원격 알림을 모두 지원 하기 위한 [사용자 알림 프레임 워크 가이드](~/ios/platform/user-notifications/index.md) 를 참조 하세요.

## <a name="sections"></a>섹션

<a name="Local Notifications In iOS" />

## <a name="local-notifications-in-ioslocal-notifications-in-iosmd"></a>[IOS의 로컬 알림](local-notifications-in-ios.md)

이 섹션에서는 Xamarin.ios에서 로컬 알림을 구현 하는 방법에 대해 설명 합니다. IOS 알림의 다양 한 UI 요소에 대해 설명 하 고 알림을 만들고 표시 하는 것과 관련 된 API에 대해 설명 합니다.

<a name="Local Notifications Walkthrough" />

## <a name="walkthrough---using-local-notifications-in-xamarinioslocal-notifications-in-ios-walkthroughmd"></a>[연습 - Xamarin.iOS에서 로컬 알림 사용](local-notifications-in-ios-walkthrough.md)

이 섹션에서는 Xamarin.ios 응용 프로그램에서 로컬 알림을 사용 하는 방법을 안내 합니다. 앱에서 수신 될 때 경고를 표시 하는 알림을 만들고 게시 하는 기본 사항을 설명 합니다.

<a name="Remote Notifications In iOS" />

## <a name="remote-notifications-in-iosremote-notifications-in-iosmd"></a>[IOS의 원격 알림](remote-notifications-in-ios.md)

이 섹션에서는 iOS의 푸시 알림을 다룹니다. APNS (Apple Push Notification Gateway Service) 및 iOS 응용 프로그램에 알림 게시에서 재생 하는 역할을 소개 합니다. 푸시 알림 및 토론을 사용 하도록 설정 하는 데 필요한 보안 인증서를 만드는 방법을 설명 합니다. 마지막으로,이 섹션에서는 응용 프로그램 서버가 클라이언트 모바일 장치를 추적 하기 위해 수행 해야 하는 몇 가지 정리 작업을 설명 합니다.

## <a name="related-links"></a>관련 링크

- [알림 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/notifications)
