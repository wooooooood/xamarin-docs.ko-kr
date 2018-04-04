---
title: ContentProvider를 사용 하 여
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b9b6340d4aaf386c7b4be8ebf366589582771be2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="using-a-contentprovider"></a>ContentProvider를 사용 하 여

ContentProvider의 데이터를 표시 CursorAdapters는 사용할 수도 있습니다.
다른 응용 프로그램에 의해 노출 되는 데이터를 액세스할 수 있도록 ContentProviders (연락처와 같이 Android 시스템 데이터를 포함 하 여 미디어 및 일정 정보).

LoaderManager를 사용 하 여 CursorLoader와를 ContentProvider 액세스 하는 것이 좋습니다. LoaderManager 주 스레드를 차단 작업을 이동 Android 3.0 (API 수준 11, Honeycomb)에 도입 된 하 고 표시 하기 위해 ListView에 바인딩되어 하기 전에 스레드에 로드 되는 데이터를 허용 하는 CursorLoader를 사용 하 여 합니다.

참조 [ContentProviders 소개](~/android/platform/content-providers/index.md) 자세한 정보에 대 한 합니다.

