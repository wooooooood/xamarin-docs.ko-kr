---
title: Android 릴리스 빌드를 인터넷에 연결할 수 없는 이유는 무엇인가요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 5996cfa3c0a18fc186ea862a2b3d7910594e1281
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027017"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Android 릴리스 빌드를 인터넷에 연결할 수 없는 이유는 무엇인가요?

## <a name="cause"></a>원인

이 문제의 가장 일반적인 원인은 **INTERNET** 권한이 디버그 빌드에는 자동으로 포함되지만 릴리스 빌드에서는 수동으로 설정되어야 한다는 것입니다. 이는 [여기에서](~/android/deploy-test/building-apps/build-process.md) "DebugSymbols"에 설명된 대로 디버거가 프로세스에 연결할 수 있도록 인터넷 권한이 사용되기 때문입니다.

## <a name="fix"></a>문제 해결

이 문제를 해결하려면 Android 매니페스트에서 인터넷 권한이 필요할 수 있습니다. 매니페스트 편집기 또는 매니페스트의 소스 코드를 통해 이 작업을 수행할 수 있습니다.

- 편집기에서 문제 해결: Android 프로젝트에서 **속성 -> AndroidManifest.xml -> 필요한 권한**으로 이동하여 **인터넷**을 확인합니다.

- 소스 코드에서 문제 해결: 소스 편집기에서 AndroidManifest를 열고 `<Manifest>` 태그 안에 권한 태그를 추가합니다.

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
