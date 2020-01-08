---
title: Xamarin.ios 앱의 데이터 및 Cloud Services
description: 이 문서는 Xamarin.ios 앱에서 로컬 데이터, iCloud 및 CloudKit를 사용 하는 방법을 설명 하는 가이드로 연결 됩니다.
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/13/2017
ms.openlocfilehash: e9f910804f3da9d173206d51c59e1d7ddd9b90a6
ms.sourcegitcommit: bad1ab3f78d7f94d48511666626b54f8ba155689
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75663514"
---
# <a name="data-and-cloud-services-in-xamarinios-apps"></a>Xamarin.ios 앱의 데이터 및 Cloud Services

## <a name="data-accessiosdata-clouddataindexmd"></a>[데이터 액세스](~/ios/data-cloud/data/index.md)

대부분의 응용 프로그램에는 장치에 로컬로 데이터를 저장 하기 위한 몇 가지 요구 사항이 있습니다. 데이터 양이 일반적으로 작은 경우에는 데이터베이스 액세스를 관리 하기 위해 응용 프로그램에서 데이터베이스와 데이터 계층이 필요 합니다. iOS에는 sqlite 데이터베이스 엔진 "기본 제공"이 있으며, SQLite Data Provider와 함께 제공 되는 Xamarin 플랫폼에 의해 데이터 액세스가 간소화 됩니다.

## <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

ICloud 저장소 API를 사용 하면 응용 프로그램에서 사용자 문서 및 응용 프로그램별 데이터를 중앙 위치에 저장 하 고 모든 사용자의 장치에서 이러한 항목에 액세스할 수 있습니다.

## <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

CloudKit 프레임 워크는 iCloud에 액세스 하는 응용 프로그램 개발을 간소화 합니다. 여기에는 응용 프로그램 데이터를 검색 하 고 응용 프로그램 정보를 안전 하 게 저장할 수 있는 권한이 포함 됩니다. 이 키트는 개인 정보를 공유 하지 않고 iCloud Id로 응용 프로그램에 대 한 액세스를 허용 하 여 사용자에 게 익명의 계층을 제공 합니다.
