---
title: ContentProvider 사용
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 5948d4b5db53db97c4e76cb7568c109b5faf90fe
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762208"
---
# <a name="using-a-contentprovider-with-xamarinandroid"></a>Xamarin Android에서 ContentProvider 사용

CursorAdapters은 ContentProvider의 데이터를 표시 하는 데에도 사용할 수 있습니다.
ContentProviders를 사용 하면 다른 응용 프로그램 (예: 연락처, 미디어 및 일정 정보)에서 제공 하는 데이터에 액세스할 수 있습니다.

ContentProvider에 액세스 하는 기본 방법은 LoaderManager를 사용 하는 CursorLoader를 사용 하는 것입니다. LoaderManager는 주 스레드에서 차단 작업을 이동 하기 위해 Android 3.0 (API 수준 11, Honeycomb)에 도입 되었으며, CursorLoader를 사용 하면 데이터를 표시 하기 위해 ListView에 바인딩하기 전에 스레드에 로드할 수 있습니다.

자세한 내용은 [ContentProviders 소개](~/android/platform/content-providers/index.md) 를 참조 하세요.
