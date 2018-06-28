---
title: MacOS Mojave 시작
description: 이 문서에서는 빌드 macOS Xamarin.Mac 사용 하 여 Mojave 앱 하도록 설정 하는 방법에 설명 합니다. 10 Xcode를 다운로드 하 고 Mac.에 대 한 Visual Studio를 업데이트 하는 방법에 설명
ms.prod: xamarin
ms.assetid: E9A7B68A-E164-4C5C-86AC-B2A3E7A30DA1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: ee5269bf314401328d0184631e817b37ce091479
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067344"
---
# <a name="getting-started-with-macos-mojave"></a>MacOS Mojave 시작

![미리 보기](~/media/shared/preview.png)

> [!WARNING]
> Xamarin의 macOS Mojave 지원은 현재 미리 보기로, 것 버그를 포함할 수 있습니다, 기능을 완료 하지는 않으며 변경 될 수 있습니다.
> 실험용만 사용 합니다.

> [!NOTE]
> 자세한 내용은 참조는 [릴리스 정보](https://releases.xamarin.com/preview-release-xcode-10-beta/) 미리 보기 릴리스에 Xamarin에 대 한 합니다.

이 문서에서는 빌드 macOS Xamarin.Mac 사용 하 여 Mojave 앱 하도록 설정 하는 방법에 설명 합니다. 10 Xcode를 다운로드 하 고 Mac.에 대 한 Visual Studio를 업데이트 하는 방법에 설명

## <a name="download-and-install"></a>다운로드 및 설치

1. **최신 Xcode 10 베타 설치** – Apple 등록 개발자 다운로드 하 고 최신 버전에서 Xcode 10의 설치 수는 [Apple 개발자 포털](https://developer.apple.com/download/)합니다.

   > [!NOTE]
   > MacOS Mojave SDK는 Xcode 10와 함께 배포 됩니다.

2. **Xcode 10을 실행** – 업데이트 하 고 Mac;에 대 한 Visual Studio를 실행 하기 전에 Xcode 10 실행 Xamarin 필요한 몇 가지 도구를 설치 합니다.

3. **Mac 용 Visual Studio 업데이트** –의 지침에 따라는 [릴리스 정보](https://releases.xamarin.com/preview-release-xcode-10-beta/) Xamarin preview를 설치 하려면.

4. _(선택 사항)_  **Mac 최신 macOS Mojave 베타 설치** – 새로 도입 된 macOS Mojave Api, 등록 된 Apple 개발자 수를 사용 하는 Xamarin.Mac 앱을 테스트 하려면 [다운로드](https://developer.apple.com/download/) 하 고 설치는 최신 macOS Mojave 개발자 베타

   > [!TIP]
   > 앱에서 모든 새 macOS Mojave Api를 사용 하지 않는 경우에 해야 macOS Mojave SDK (Xcode 10과 함께 설치 됨)로 구축 하 고 테스트 하 모든 항목이 예상 대로 작동 하는지 확인 합니다. 응용 프로그램에서 새 Api를 호출 하지 않습니다 macOS Mojave SDK 여 다시 컴파일 하 고 Mac의 운영 체제를 업그레이드 하지 않고 테스트할 수 있습니다.

   > [!IMPORTANT]
   > Mac을 빌드 및 새 macOS Mojave Api를 호출 하는 Xamarin.Mac 응용 프로그램을 테스트할 Mojave macOS 업그레이드 하기 전에:
   > - 읽기 [Apple의 릴리스 정보](https://developer.apple.com/download/) 운영 체제 업데이트에 대 한 합니다.
   > - 읽기는 [릴리스 정보](https://releases.xamarin.com/preview-release-xcode-10-beta/) 미리 보기 릴리스에 Xamarin에 대 한 합니다. 참고가 첫 번째 미리 보기에 새 macOS Mojave AppKit Api (예: 어두운 모드)에 대 한 바인딩을 포함 되지 않습니다.

## <a name="related-links"></a>관련 링크

- [10 Xcode를 다운로드 합니다.](https://developer.apple.com/download/)
- Xamarin 미리 보기 [릴리스 정보](https://releases.xamarin.com/preview-release-xcode-10-beta/)
