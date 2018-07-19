---
title: 내 Android 릴리스 빌드는 인터넷에 연결할 수 없는 이유는?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 7f956defd0243e1927746a53e6b3b1b05d98f8d2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762086"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>내 Android 릴리스 빌드는 인터넷에 연결할 수 없는 이유는?

## <a name="cause"></a>원인

이 문제의 가장 일반적인 이유는 **인터넷** 권한은 자동으로 디버그 빌드에 연결 되어 있지만 릴리스 빌드에 대해 수동으로 설정 해야 합니다. "DebugSymbols"에 대 한 설명 된 대로 디버거가 프로세스에 연결할 수 있도록 인터넷 권한 사용 되기 때문에 이것이 [여기](~/android/deploy-test/building-apps/build-process.md)합니다.


## <a name="fix"></a>문제 해결

이 문제를 해결 하려면 Android 매니페스트에 인터넷 권한을 요구할 수 있습니다. 이 매니페스트의 sourcecode 또는 매니페스트 편집기를 통해 수행할 수 있습니다.

-   편집기에서 수정: Android 프로젝트에서로 이동 **속성 AndroidManifest.xml-> 필요한 권한->** 확인 **인터넷**

-   Sourcecode에서 수정:는 AndroidManifest 소스 편집기에서 열고 권한 태그 내에 추가 `<Manifest>` 태그:

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
