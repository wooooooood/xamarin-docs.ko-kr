---
title: '앱 스토어에 제출 하는 동안 오류 발생: "잘못 된 번들에 포함 될 수 없는 옵션은 전송에서 검색 됩니다."'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: c6a03fa3327e81f577f85b05a033d9c97495eb2e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031071"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>앱 스토어에 제출 하는 동안 오류 발생: "잘못 된 번들에 포함 될 수 없는 옵션은 전송에서 검색 됩니다."

watchOS apps 및 tvOS apps는 앱 스토어에 전송 될 때 bitcode를 _요구_ 합니다. Xcode 8.3 이전 버전을 사용 하 여 watchOS 및 tvOS 앱을 빌드하고 제출할 때 앱 스토어에 업로드 하려고 할 때 전자 메일 알림을 통해 다음과 같은 오류가 발생할 수 있습니다.

>잘못 된 번들-bitcode에 포함 될 수 있는 옵션이 전송에서 검색 되었으므로 앱을 처리할 수 없습니다. Xcode에 제공 된 도구 체인를 사용 하 여 앱을 빌드하지 않는 것 같습니다.

이 문제에 대 한 해결 방법은 Xcode 9 및 최신 버전의 Xamarin.ios를 사용 하 여 응용 프로그램을 빌드하는 것입니다.
