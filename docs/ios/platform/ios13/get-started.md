---
title: IOS 13, iPadOS 13, tvOS 13 및 watchOS 6 시작
description: 이 문서에서는 Xamarin을 사용 하 여 iOS 13, iPadOS 13, tvOS 13 및 watchOS 6 앱을 빌드하기 위해를 설정 하는 방법을 설명 합니다. Xcode 11을 다운로드 하 고 Mac용 Visual Studio를 업데이트 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 97414545-85D2-433C-9246-63B6763F488A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/01/2019
ms.openlocfilehash: 1ab713636e51023653260963378f45637e793d05
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031746"
---
# <a name="get-started-with-ios-13"></a>IOS 13 시작

이 문서에서는 iOS 13에 대해 Xcode 11을 사용 하 여 릴리스된 Api를 호출 하는 Xamarin 앱 빌드를 시작 하는 방법을 설명 합니다. Preview를 사용 하려면 Mojave (macOS 10.14.4) 이상이 필요 합니다.

## <a name="download-and-install"></a>다운로드 및 설치

1. **Xcode 11 설치** – 등록 된 apple 개발자는 [apple 개발자 포털](https://developer.apple.com/download/) 또는 **앱 스토어**에서 최신 버전의 Xcode 11을 다운로드 하 여 설치할 수 있습니다.

2. **Xcode 11 실행** – Xamarin에서 요구 하는 일부 도구를 설치 하기 때문에 Mac용 Visual Studio 또는 Visual Studio 2019을 업데이트 하 고 실행 하기 전에 Xcode 11을 실행 합니다.

3. **업데이트 Mac용 Visual Studio 및 Visual Studio 2019** – 안정적인 최신 버전의 Xamarin이 있는지 확인 합니다.

4. 필드 Ios **장치에 ios 13 설치** – Xcode 11에서 도입 된 api를 사용 하는 앱의 장치 테스트용으로 등록 된 Apple 개발자는 장치에 운영 체제를 [다운로드](https://developer.apple.com/download) 하 여 설치할 수 있습니다. 

   > [!TIP]
   > 앱에서 새 Api를 사용 하지 않더라도 최신 Xcode 11 Sdk를 사용 하 여 빌드하고 모든 것이 예상 대로 작동 하는지 확인 해야 합니다. 앱에서 새 Api를 호출 하지 않는 경우 이러한 새 Sdk를 사용 하 여 다시 컴파일하고 새 운영 체제로 아직 업그레이드 되지 않은 장치에서 테스트할 수 있습니다.
   >
   > Apple에서 Xamarin 앱을 테스트 하기 위해 장치를 최신 운영 체제 릴리스로 업그레이드 하기 전에 다음을 확인 해야 합니다.
   >
   > - 운영 체제 업데이트에 대 한 [Apple의 릴리스 정보](https://developer.apple.com/download/) 를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [다운로드 Xcode](https://developer.apple.com/download/)
- [Xamarin.iOS 릴리스 정보](/xamarin/ios/release-notes/13/13.0)
