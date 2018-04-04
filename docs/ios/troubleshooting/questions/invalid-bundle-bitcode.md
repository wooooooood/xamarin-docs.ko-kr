---
title: '앱 스토어에 제출 하는 동안 오류가 발생 했습니다: "잘못 된 번들-bitcode에 포함 될 수 없는 옵션에서에서 발견 되 면 전송"'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: cacb9040ddc8582490c68bcfd24e80c4c4679eb4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>앱 스토어에 제출 하는 동안 오류가 발생 했습니다: "잘못 된 번들-bitcode에 포함 될 수 없는 옵션에서에서 발견 되 면 전송"

watchOS 앱과 tvOS _필요_ bitcode 앱 스토어에 제출 되 면입니다. (전자 메일 알림)를 통해 다음과 같은 오류가 발생할 수 있습니다 빌드하고 Xcode 8.3 또는 이전 버전을 사용 하 여 watchOS 및 tvOS 앱을 제출 하는 경우 앱 스토어에 업로드 하려고 할 때:

>잘못 된 번들-bitcode에 포함 될 수 없는 옵션 전송에서 발견 되 면 앱을 처리할 수 없습니다. Xcode에 제공 된 도구 체인으로 응용 프로그램 빌드하지 않는 가능성이 높습니다.

이 문제에 솔루션 Xcode 9, Xamarin.iOS의 최신 버전으로 응용 프로그램을 작성 하는 것입니다.
