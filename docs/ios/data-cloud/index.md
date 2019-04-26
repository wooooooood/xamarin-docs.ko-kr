---
title: 데이터 및 Xamarin.iOS 앱에 클라우드 서비스
description: 이 문서는 Xamarin.iOS 앱에 로컬 데이터, iCloud, 및 CloudKit을 사용 하 여 작동 하는 방법에 설명 하는 지침에 연결 합니다.
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2017
ms.openlocfilehash: 21a6c1c0c0ceb5596a056f0818dec39041808504
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61218247"
---
# <a name="data-and-cloud-services-in-xamarinios-apps"></a>데이터 및 Xamarin.iOS 앱에 클라우드 서비스

##  <a name="data-accessiosdata-clouddataindexmd"></a>[데이터 액세스](~/ios/data-cloud/data/index.md)

대부분의 응용 프로그램에는 로컬 장치에서 데이터를 저장 하려면 몇 가지 요구 사항이. 데이터의 양이 일반적으로 작은 아닌 일반적으로 데이터베이스 및 데이터 계층 응용 프로그램 데이터베이스 액세스를 관리 하에 있어야 합니다. iOS에 "기본 제공" Sqlite 데이터베이스 엔진 및 데이터에 대 한 액세스는 SQLite 데이터 공급자와 함께 제공 되는 Xamarin의 플랫폼을 통해 간소화 되었습니다.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

IOS 5에서에서 iCloud 저장소 API에는 응용 프로그램을을 중앙 위치로 사용자 문서 및 응용 프로그램별 데이터를 저장 하 고 모든 사용자 장치에서 해당 항목에 액세스할 수 있습니다.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

CloudKit 프레임 워크는 해당 액세스 iCloud 응용 프로그램 개발을 간소화합니다. 여기에 응용 프로그램 정보를 안전 하 게 저장할 수 뿐만 아니라 응용 프로그램 데이터 및 자산 권한을 검색 합니다. 이 키트는 개인 정보를 공유 하지 않고 Id가 iCloud 사용 하 여 응용 프로그램에 대 한 액세스를 허용 하 여 사용자가 계층 익명성을 제공 합니다.