---
title: '앱 스토어에 제출 하는 동안 오류가 발생 했습니다: "잘못 된 번들-bitcode에 포함 될 수 없는 옵션이 제출에서 감지 됩니다."'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: 867ad29abfa6a38971b60ac9ebf181905949dafd
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106990"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>앱 스토어에 제출 하는 동안 오류가 발생 했습니다: "잘못 된 번들-bitcode에 포함 될 수 없는 옵션이 제출에서 감지 됩니다."

watchOS 앱 및 tvOS 앱 _필요_ bitcode는 App Store에 제출 되 면 합니다. 빌드하고 Xcode 8.3 또는 이전 버전을 사용 하 여 watchOS 및 tvOS 앱을 제출 하는 경우 다음 오류가 나타날 수 있습니다 (전자 메일 알림)를 통해 앱 스토어에 업로드 하려고 할 때:

>잘못 된 번들-bitcode에 포함 될 수 없는 옵션이 제출에서 감지는 앱을 처리할 수 없습니다. Xcode에서 제공 하는 도구 체인을 사용 하 여 앱을 빌드하지 않는 것입니다.

이 문제를 해결 하려면 Xcode 9 및 최신 버전의 Xamarin.iOS 사용 하 여 응용 프로그램을 구축 하는 것입니다.
