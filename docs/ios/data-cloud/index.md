---
title: "데이터 및 클라우드 서비스"
description: "안정화 및 배포 가이드"
ms.topic: article
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: b5e250c7c505958c8293970321b6173e6086e7b1
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="data-and-cloud-services"></a>데이터 및 클라우드 서비스


##  <a name="data-accessiosdata-clouddataindexmd"></a>[데이터 액세스](~/ios/data-cloud/data/index.md)

대부분의 응용 프로그램에는 로컬 장치에서 데이터를 저장 하려면 몇 가지 요구를 사항이 있습니다. 많으면 데이터의 양이 다르거나 일반적으로,이 일반적으로 필요 데이터베이스 및 데이터 계층 응용 프로그램에 데이터베이스 액세스를 관리 합니다. iOS는 "기본 제공" Sqlite 데이터베이스 엔진이 및 데이터에 대 한 액세스 SQLite 데이터 공급자와 함께 제공 되는 Xamarin 플랫폼을 통해 간소화 되었습니다.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

IOS 5에서에서 iCloud 저장소 API 응용 프로그램을 사용자 문서 및 응용 프로그램 관련 데이터를 중앙 위치를 저장 하는 사용자의 모든 장치에서 해당 항목에 액세스할 수 있습니다.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

CloudKit 프레임 워크는 해당 액세스 iCloud 응용 프로그램 개발을 간소화합니다. 여기에 응용 프로그램 데이터 및 자산 권한으로 응용 프로그램 정보를 안전 하 게 저장할 수를 검색 합니다. 이 키트 개인 정보를 공유 하지 않고 자신의 iCloud Id와 응용 프로그램에 대 한 액세스를 허용 하 여 사용자에 게 익명의 계층을 제공 합니다.