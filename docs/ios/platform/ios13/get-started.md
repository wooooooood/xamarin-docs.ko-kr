---
title: IOS 13, iPadOS 13, 13, tvOS 및 watchOS 6 시작
description: 이 문서에는 최대 빌드 iOS 13, iPadOS 13, 13, tvOS 및 Xamarin 사용 하 여 6 watchOS 앱을 설정 하는 방법을 설명 합니다. Xcode 11을 다운로드 하 고 Mac 및 Visual Studio 2019 용 Visual Studio를 업데이트 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 97414545-85D2-433C-9246-63B6763F488A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/01/2019
ms.openlocfilehash: d2bb69394d8e9bfeb949a734291179a3e6f5a495
ms.sourcegitcommit: a6ba6ed086bcde4f52fb05f83c59c68e8aa5e436
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67540439"
---
# <a name="get-started-with-ios-13"></a>13 iOS 시작

![미리 보기 기능](~/media/shared/preview.png)

이 문서에는 13 iOS 용 Xcode 11과 함께 출시 되는 Api를 호출 하는 Xamarin 앱 빌드를 시작 하는 방법을 설명 합니다. MacOS 10.14.4 (Mojave) 미리 보기를 사용 하려면 이상.

## <a name="download-and-install"></a>다운로드 및 설치

1. **Xcode 11 beta를 설치** -등록 된 Apple 개발자 다운로드 하 고 최신 버전의 Xcode 11 beta의 설치 수를 [Apple Developer 포털](https://developer.apple.com/download/) 또는 **앱 스토어**합니다.

2. **Xcode 11 beta를 실행** – 실행 Xcode 11 해당 Xamarin 도구를 업데이트 하 고 일부를 설치 하는 대로 Mac 용 Visual Studio를 실행 하기 전에 필요 합니다.

3. Mac 용 Visual Studio에서 선택 **Visual Studio > 업데이트 확인...** 를 선택 합니다 **Xcode 11 미리 보기** 채널을 열고, 사용 가능한 업데이트를 설치 합니다.

4. Mac 용 Visual Studio에서 선택 **Visual Studio > 기본 설정 > 프로젝트 > SDK 위치 > Apple** 선택한 **Xcode beta.app**합니다.

5. (선택 사항) 이 미리 보기를 사용 하 여 평가 하는 경우 _Xcode 11 beta 3_, 링크를 사용 해야 합니다. 프로젝트를 마우스 오른쪽 단추로 클릭, 이동할 **옵션 > iOS 빌드 > 링커 동작** 링커 동작 값을 설정 하 고 **프레임 워크 Sdk를 전용에 링크**합니다. 이 해결 방법은 향후 미리 보기에 필요한 되지 않습니다.

6. (선택 사항) **IOS 장치에서 iOS 13 설치** – Xcode 11, 등록 된 Apple 개발자 수를 사용 하 여 도입 된 Api를 사용 하는 앱의 테스트 장치에 대 한 [다운로드](https://developer.apple.com/download) 하 고 장치의 운영 체제를 설치 합니다. IOS 베타 단계에서 이므로 기본 장치에 설치 하 고 주의 해야 합니다.

   > [!TIP]
   > 앱에서 새 Api를 사용 하지 않는 경우에 최신 Xcode 11 Sdk를 사용 하 여 빌드 및 테스트 하 여 모든 항목이 예상 대로 작동 하는지 확인 해야 합니다. 앱 새 Api를 호출 하지 않으면, 이러한 새 Sdk를 사용 하 여 다시 컴파일할 수 있으며 새 운영 체제에 아직 업그레이드 하지 않은 장치에서 테스트 하 고 있습니다.
   >
   > Xamarin 앱을 테스트 하려면 Apple에서 릴리스는 최신 운영 체제에 장치를 업그레이드 하기 전에 야 합니다.
   >
   > - 읽기 [Apple 릴리스](https://developer.apple.com/download/) 운영 체제 업데이트에 대 한 합니다.

## <a name="related-links"></a>관련 링크

- [Xcode를 다운로드 합니다.](https://developer.apple.com/download/)
