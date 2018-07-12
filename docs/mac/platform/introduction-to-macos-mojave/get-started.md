---
title: MacOS Mojave 시작
description: 이 문서에는 최대 빌드 macOS Mojave Xamarin.Mac 앱을 설정 하는 방법을 설명 합니다. Xcode 10을 다운로드 하 고 mac 용 Visual Studio를 업데이트 하는 방법에 설명
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: ee5269bf314401328d0184631e817b37ce091479
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831347"
---
# <a name="getting-started-with-macos-mojave"></a>MacOS Mojave 시작

![미리 보기](~/media/shared/preview.png)

> [!WARNING]
> Xamarin의 macOS Mojave 지원은 현재 미리 보기에서 해당 버그를 포함할 수 있습니다, 기능이 완전 하지는 않으며 변경 될 수 있습니다.
> 실험 용도로 사용 합니다.

> [!NOTE]
> 자세한 내용은 합니다 [릴리스](https://releases.xamarin.com/preview-release-xcode-10-beta/) Xamarin에 대 한 미리 보기 릴리스 합니다.

이 문서에는 최대 빌드 macOS Mojave Xamarin.Mac 앱을 설정 하는 방법을 설명 합니다. Xcode 10을 다운로드 하 고 mac 용 Visual Studio를 업데이트 하는 방법에 설명

## <a name="download-and-install"></a>다운로드 및 설치

1. **최신 Xcode 10 베타 버전을 설치** -등록 된 Apple 개발자 다운로드 하 고 최신 버전의 Xcode 10의 설치 수를 [Apple Developer 포털](https://developer.apple.com/download/)합니다.

   > [!NOTE]
   > MacOS Mojave SDK는 Xcode 10을 사용 하 여 배포 됩니다.

2. **Xcode 10을 실행** –; Mac 용 Visual Studio를 실행 하 고 업데이트 하기 전에 Xcode 10 실행 Xamarin 필요한 몇 가지 도구를 설치 합니다.

3. **Mac 용 Visual Studio 업데이트** –의 지침에 따라 합니다 [릴리스](https://releases.xamarin.com/preview-release-xcode-10-beta/) Xamarin 미리 보기를 설치 하려면.

4. _(선택 사항)_  **Mac에서 최신 macOS Mojave 베타를 설치** – 새로 도입 된 macOS 등록 된 Apple 개발자 수 Mojave Api를 사용 하는 Xamarin.Mac 앱을 테스트 하려면 [다운로드](https://developer.apple.com/download/) 하 고 설치 합니다 최신 macOS Mojave 개발자 베타 버전입니다.

   > [!TIP]
   > 앱에서 모든 새 macOS Mojave Api를 사용 하지 않는 경우에 macOS Mojave SDK (Xcode 10와 함께 설치 됨)를 사용 하 여 빌드 및 테스트 하 여 모든 항목이 예상 대로 작동 하는지 확인 해야 합니다. 앱 새 Api를 호출 하지 않으면, macOS Mojave SDK를 사용 하 여 다시 컴파일할 수 있으며 Mac의 운영 체제를 업그레이드 하지 않고 테스트할 수 있습니다.

   > [!IMPORTANT]
   > Mac을 빌드 및 새 macOS Mojave Api를 호출 하는 Xamarin.Mac 응용 프로그램을 테스트할 Mojave macOS에 업그레이드 하기 전에:
   > - 읽기 [Apple 릴리스](https://developer.apple.com/download/) 운영 체제 업데이트에 대 한 합니다.
   > - 읽기를 [릴리스](https://releases.xamarin.com/preview-release-xcode-10-beta/) Xamarin에 대 한 미리 보기 릴리스 합니다. 참고가 첫 번째 미리 보기에 새 macOS Mojave AppKit Api (예: 어두운 모드)에 대 한 바인딩을 포함 되지 않습니다.

## <a name="related-links"></a>관련 링크

- [Xcode 10 다운로드](https://developer.apple.com/download/)
- Xamarin 미리 보기 [릴리스 정보](https://releases.xamarin.com/preview-release-xcode-10-beta/)
