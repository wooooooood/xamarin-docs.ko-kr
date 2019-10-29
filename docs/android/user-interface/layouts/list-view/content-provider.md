---
title: ContentProvider 사용
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: c99b3d64f4f5033240c086af8677d31485a44f01
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028929"
---
# <a name="using-a-contentprovider-with-xamarinandroid"></a>Xamarin Android에서 ContentProvider 사용

CursorAdapters은 ContentProvider의 데이터를 표시 하는 데에도 사용할 수 있습니다.
ContentProviders를 사용 하면 다른 응용 프로그램 (예: 연락처, 미디어 및 일정 정보)에서 제공 하는 데이터에 액세스할 수 있습니다.

ContentProvider에 액세스 하는 기본 방법은 LoaderManager를 사용 하는 CursorLoader를 사용 하는 것입니다. LoaderManager는 주 스레드에서 차단 작업을 이동 하기 위해 Android 3.0 (API 수준 11, Honeycomb)에 도입 되었으며, CursorLoader를 사용 하면 데이터를 표시 하기 위해 ListView에 바인딩하기 전에 스레드에 로드할 수 있습니다.

자세한 내용은 [ContentProviders 소개](~/android/platform/content-providers/index.md) 를 참조 하세요.
