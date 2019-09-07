---
title: Android 릴리스 빌드를 인터넷에 연결할 수 없는 이유는 무엇인가요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 162378c00f3e20574d04dc373fcc492a9407b88d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761039"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Android 릴리스 빌드를 인터넷에 연결할 수 없는 이유는 무엇인가요?

## <a name="cause"></a>원인

이 문제의 가장 일반적인 원인은 **인터넷** 권한이 디버그 빌드에 자동으로 포함 되지만 릴리스 빌드에 대해 수동으로 설정 해야 한다는 것입니다. 이는 "DebugSymbols"에 설명 된 대로 디버거가 프로세스에 연결할 수 있도록 하기 위해 인터넷 권한이 사용 되기 때문 [입니다.](~/android/deploy-test/building-apps/build-process.md)

## <a name="fix"></a>문제 해결

이 문제를 해결 하려면 Android 매니페스트에서 인터넷 권한이 필요할 수 있습니다. 매니페스트 편집기나 매니페스트의 sourcecode을 통해이 작업을 수행할 수 있습니다.

- 편집기의 수정: Android 프로젝트에서 **속성-> AndroidManifest .xml-> 필요한 권한** 으로 이동 하 여 **인터넷** 을 확인 합니다.

- Sourcecode의 수정: 소스 편집기에서 androidmanifest를 열고 권한 태그를 `<Manifest>` 태그 안에 추가 합니다.

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
