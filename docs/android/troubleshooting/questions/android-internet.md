---
title: Android 릴리스 빌드를 인터넷에 연결할 수 없는 이유는 무엇인가요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: cd27d5c884086cd0fade4364851039fd0cd915a0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60945463"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Android 릴리스 빌드를 인터넷에 연결할 수 없는 이유는 무엇인가요?

## <a name="cause"></a>원인

이 문제의 가장 일반적인 원인은 합니다 **인터넷** 권한은 디버그 빌드에서 자동으로 포함 되어 있지만 릴리스 빌드를 수동으로 설정 해야 합니다. "DebugSymbols"에 설명 된 대로 디버거 프로세스에 연결할 수 있도록 인터넷 권한 사용 되기 때문에 이것이 [여기](~/android/deploy-test/building-apps/build-process.md)합니다.


## <a name="fix"></a>문제 해결

문제를 해결 하려면 Android 매니페스트에서 인터넷 사용 권한을 요구할 수 있습니다. 이 매니페스트의 sourcecode 또는 매니페스트 편집기를 통해 수행할 수 있습니다.

-   편집기에서 수정 합니다. Android 프로젝트에서로 이동 **속성-AndroidManifest.xml >-> 필요한 권한** 확인 하 고 **인터넷**

-   Sourcecode에서 수정 합니다. AndroidManifest 원본 편집기에서 열고 내 사용 권한 태그를 추가 합니다 `<Manifest>` 태그:

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
