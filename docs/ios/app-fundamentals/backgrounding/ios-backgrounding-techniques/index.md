---
title: iOS Backgrounding 기술
description: '이 문서는 iOS의 backgrounding 다양 한 기술을 설명 하는 지침에 연결: 백그라운드 작업, 백그라운드 전송 서비스, 백그라운드 페치 및 원격 알림.'
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 00cca0c75cc79858f6edda5d6fb954611d81161b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119750"
---
# <a name="ios-backgrounding-techniques"></a>iOS Backgrounding 기술

다음 섹션에서는 기존 backgrounding 옵션을 함께 다음과 같은 iOS 기능을 살펴봅니다.

-  [편의적 백그라운드 작업](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -장치가 다른 처리를 위해 절전 모드 해제 때 편의적 청크에서 백그라운드 작업을 실행 하 여 배터리를 유지 합니다.
-  [백그라운드 전송 서비스](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) -안정적으로 업로드 및 네트워크 상태 또는 파일 크기에 관계 없이 파일을 다운로드 합니다.
-  [백그라운드 페치](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -시스템 간격으로 백그라운드에서 응용 프로그램을 새로 고칩니다.
-  [원격 알림](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) -사용 하 여 푸시 알림을 사용자에 게 자동으로 업데이트 하는 옵션을 사용 하 여 사용자가 응용 프로그램, 전에 백그라운드에서 콘텐츠 업데이트를 트리거할 수 있습니다.
-  **UI 업데이트 백그라운드** -사용자에 대 한 응용 프로그램 UI를 준비 하 고 백그라운드에서 모든 응용 프로그램의 스냅숏을 업데이트 합니다.
