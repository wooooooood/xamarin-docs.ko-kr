---
title: iOS Backgrounding 기술
description: 이 문서는 iOS에서 백그라운드 작업, 백그라운드 전송 서비스, 백그라운드 페치 및 원격 알림과 같은 다양 한 backgrounding 기술을 설명 하는 가이드로 연결 됩니다.
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: e438ed1a2367dee722a0129fce2e44f5083b757a
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521292"
---
# <a name="ios-backgrounding-techniques"></a>iOS Backgrounding 기술

다음 섹션에서는 기존 backgrounding 옵션과 함께 다음 iOS 기능을 살펴봅니다.

- [Oplocks 백그라운드 작업](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -다른 처리를 위해 장치를 절전 모드에서 해제 하는 경우 백그라운드 작업을 실행 하 여 배터리 수명을 유지 합니다.
- [백그라운드 전송 서비스](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) -네트워크 상태 또는 파일 크기에 관계 없이 안정적으로 파일을 업로드 하 고 다운로드 합니다.
- [백그라운드 페치](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -시스템에서 결정 된 간격으로 백그라운드에서 응용 프로그램을 새로 고칩니다.
- [원격 알림](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) -푸시 알림을 사용 하 여 사용자가 응용 프로그램을 열기 전에 백그라운드에서 콘텐츠 업데이트를 트리거하고 사용자에 게 알리거나 자동으로 업데이트 하는 옵션을 사용 합니다.
- **백그라운드 UI 업데이트** -사용자에 대 한 응용 프로그램 UI를 준비 하 고 백그라운드에서 응용 프로그램의 스냅숏을 업데이트 합니다.
