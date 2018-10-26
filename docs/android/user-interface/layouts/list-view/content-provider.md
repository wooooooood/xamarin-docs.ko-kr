---
title: ContentProvider 사용
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: d1ec628de3481820f320a5a8e6ef88fcbaab75a6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122116"
---
# <a name="using-a-contentprovider"></a>ContentProvider 사용

CursorAdapters는 ContentProvider에서 데이터를 표시 하려면 데도 사용할 수 있습니다.
ContentProviders 허용 다른 응용 프로그램에서 제공 하는 데이터에 액세스할 수 있습니다 (연락처와 같이 Android 시스템 데이터를 포함 하 여 미디어 및 일정 정보).

ContentProvider에 액세스 하는 기본 방법은 LoaderManager를 사용 하 여를 CursorLoader 된 합니다. LoaderManager 주 스레드나 해제 차단 작업을 이동 (API 수준 11, Honeycomb) 한 Android 3.0에서 도입 되었습니다 하 고 표시 하기 위해 ListView에 바인딩되는 스레드에서 로드 될 데이터를 허용 하는 CursorLoader를 사용 하 여 합니다.

가리킵니다 [ContentProviders 소개](~/android/platform/content-providers/index.md) 자세한 내용은 합니다.

