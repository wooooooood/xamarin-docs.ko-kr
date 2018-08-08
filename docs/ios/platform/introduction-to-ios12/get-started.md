---
title: IOS 12, 12, tvOS 및 watchOS 5 시작
description: 이 문서에는 최대 빌드 iOS 12, 12, tvOS 및 Xamarin 사용한 5 watchOS 앱을 설정 하는 방법을 설명 합니다. Xcode 10을 다운로드 하 고 Mac 및 Visual Studio 2017 용 Visual Studio를 업데이트 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 6C0F0133-1A5F-408B-8BCA-BDCA313A55C2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/07/2018
ms.openlocfilehash: cb84ddc646933d253ca72fe8f9f581364aab698b
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615176"
---
# <a name="getting-started-with-ios-12-tvos-12-and-watchos-5"></a>IOS 12, 12, tvOS 및 watchOS 5 시작

![미리 보기](~/media/shared/preview.png)

> [!WARNING]
> Xamarin iOS 12, 12, tvOS 및 watchOS 5 Sdk Xcode 10을 사용 하 여 배포에 대 한 지원은 현재 미리 보기로 제공, 의미 없는 버그를 포함할 수 있습니다는 기능을 완료 하 고 변경 될 수 있습니다. 실험 용도로 사용 합니다.

이 문서에는 Xcode 10을 사용 하 여 출시 하는 Api를 호출 하는 Xamarin 앱 빌드를 설정 하는 방법을 설명 합니다. Xcode 10을 다운로드 하 고 Mac 및 Visual Studio 2017 용 Visual Studio를 업데이트 하는 방법을 설명 합니다.

## <a name="download-and-install"></a>다운로드 및 설치

1. **최신 Xcode 10 베타 버전을 설치** -등록 된 Apple 개발자 다운로드 하 고 최신 버전의 Xcode 10의 설치 수를 [Apple Developer 포털](https://developer.apple.com/download/)합니다.

2. **Xcode 10을 실행** – Xcode 10 실행 전에 Xamarin 도구를 업데이트 하 고 일부를 설치 하는 대로 Mac 또는 Visual Studio 2017, Visual Studio를 실행 해야 합니다.

3. **Mac 및 Visual Studio 2017 용 Visual Studio 업데이트** –의 지침에 따라 합니다 [블로그를 릴리스](https://releases.xamarin.com/preview-release-xcode-10-beta-5/) Xamarin 미리 보기를 설치 하려면.

4. _(선택 사항)_  **IOS 장치에 최신 iOS 베타를 설치** – Xcode 10, 등록 된 Apple 개발자 수를 사용 하 여 도입 된 Api를 사용 하는 앱의 장치 테스트용 [다운로드](https://developer.apple.com/download) 최신 설치 장치에 개발자 베타 버전입니다.

   > [!TIP]
   > 앱에서 새 Api를 사용 하지 않는 경우에 최신 Xcode 10 Sdk를 사용 하 여 빌드 및 테스트 하 여 모든 항목이 예상 대로 작동 하는지 확인 해야 합니다. 앱 새 Api를 호출 하지 않으면, 이러한 새 Sdk를 사용 하 여 다시 컴파일할 수 있으며 새 운영 체제에 아직 업그레이드 하지 않은 장치에서 테스트 하 고 있습니다.
   >
   > Xamarin 앱을 테스트 하려면 Apple에서 릴리스는 최신 운영 체제에 장치를 업그레이드 하기 전에 야 합니다.
   >
   > - 읽기 [Apple 릴리스](https://developer.apple.com/download/) 운영 체제 업데이트에 대 한 합니다.
   > - Xamarin 미리 보기를 읽을 [블로그 게시물을 릴리스](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)합니다.

## <a name="related-links"></a>관련 링크

- [Xcode 10 다운로드](https://developer.apple.com/download/)
- Xamarin 미리 보기 [릴리스 블로그 게시물](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
