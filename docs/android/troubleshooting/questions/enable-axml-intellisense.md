---
title: "Android.axml 파일에서 Intellisense는 어떻게 사용 합니까?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84850CB2-1CE2-4D3F-BD01-6B3B033F5A4C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: c68ec5a6d369e8673c8764e01943e513489775b0
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="how-do-i-enable-intellisense-in-android-axml-files"></a>Android.axml 파일에서 Intellisense는 어떻게 사용 합니까?

Visual Studio의 android Intellisense에는 두 개의 XML 스키마 파일을 적절 하 게 작업 해야 합니다. 이러한 파일을 찾으려면이 폴더로 이동 합니다.

`C:\Program Files (x86)\Xamarin Studio\AddIns\MonoDevelop.MonoDroid\schemas`*

* (이전에: `C:\Program Files (x86)\MSBuild\Xamarin\Android`)

내부 두.xsd 파일을 찾을 수 있습니다.

1. android-layout-xml.xsd
2. schemas.android.com.apk.res.android.xsd

Visual Studio는 해당 폴더의 모든 XML 스키마 유지:

`C:\Program Files (x86)\Microsoft Visual Studio 12.0\Xml\Schemas`

위의 위치에이 두.xsd 파일을 이동 하거나 Visual Studio 내에서 이러한 스키마를 추가 선택할 수 있습니다. 추가할 수 있습니다""를 통해 Visual Studio 내 이러한 스키마는 **XML > 스키마 > 추가** 대화 상자.






