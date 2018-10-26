---
title: Xamarin.iOS에서 포함 된 프레임 워크
description: 이 문서에서는 Xamarin.iOS 응용 프로그램에 포함 된 프레임 워크를 사용 하 여 코드를 공유 하는 방법을 설명 합니다. Mtouch 도구 또는 네이티브 참조를 사용 하 여이 수행할 수 있습니다.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2018
ms.openlocfilehash: b59fd7c1a9e5f528878b90e1a76fabe5a79bab81
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108244"
---
# <a name="embedded-frameworks-in-xamarinios"></a>Xamarin.iOS에서 포함 된 프레임 워크

_이 문서는 응용 프로그램 개발자는 앱에서 사용자 프레임 워크를 포함 하는 방법에 대해 설명 합니다._

Ios 8.0 Apple 있게 앱 확장 및 Xcode에서 기본 앱 간에 코드 공유 하는 포함 된 프레임 워크를 만들 수 있습니다.

Xamarin.iOS 9.0 Xamarin.iOS 앱에 (Xcode를 사용 하 여 만든)이 포함 된 프레임 워크를 사용 하는 것에 대 한 지원을 추가 합니다. *됩니다 **되지** Xamarin.iOS 프로젝트의 모든 형식에서 포함 된 프레임 워크를 만들어, 기존 네이티브 (Objective-c) 프레임 워크만 사용할 수 있습니다.*

Xamarin.iOS에서 프레임 워크를 사용 하는 방법은 두 가지 있습니다.

- 프로젝트의 추가 mtouch 인수에 다음을 추가 하 여 mtouch 도구의 프레임 워크 전달할 **iOS 빌드** 옵션:

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  이 각 프로젝트 구성에 대해 설정 해야 합니다.

- 상황에 맞는 메뉴에서 네이티브 참조를 추가 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

프로젝트 및 찾아보기 네이티브 참조를 추가 하려면 마우스 오른쪽 단추로 클릭

![](embedded-frameworks-images/xam-native-refs.png "Mac 용 Visual Studio에서 네이티브 참조 추가 선택")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

프로젝트 및 찾아보기 네이티브 참조를 추가 하려면 마우스 오른쪽 단추로 클릭

![](embedded-frameworks-images/vs-native-refs.png "Visual Studio에서 네이티브 참조 추가 선택 합니다.")

-----

  이 모든 구성에 대해 작동 합니다.

Mac 용 Visual Studio 및 Xamarin Tools for Visual Studio의 향후 버전에서 프로젝트 파일을 수동으로 편집) 하는 경우 (없이 IDE 내에서 프레임 워크를 사용할 수 됩니다.

몇 가지 샘플 프로젝트에서 찾을 수 있습니다 [github](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>제한 사항

- 포함 된 프레임 워크 에서만 지원 됩니다 [통합](~/cross-platform/macios/unified/index.md) 프로젝트입니다.
- 포함 된 프레임 워크는 프로젝트 에서만 지원의 배포 대상 사용 하 여 적어도 iOS 8.0입니다.
- 확장 하는 포함 된 프레임 워크에 필요한 경우 컨테이너 앱에는 프레임 워크, 그렇지 않으면 프레임 워크 앱 번들에 포함 되지 것입니다에 대 한 참조도 있어야 합니다.

## <a name="the-mono-runtime"></a>Mono 런타임

내부적으로 Xamarin.iOS는 Mono 런타임이 각 확장 프로그램 및 컨테이너 앱에 정적으로 연결 하는 대신 프레임 워크는 Mono 런타임과 함께 연결 하려면이 기능을 사용 합니다.

컨테이너 앱은 통합 앱 확장을 포함 하 고 대상 배포를 iOS 8.0 이상 자동으로 수행 됩니다.

앱 확장 하지 않고 계속 연결는 Mono 런타임과 함께 정적으로 하나의 앱만 참조 하는 경우에 프레임 워크를 사용 하는 크기 저하 있기 때문에.

프로젝트의 iOS 빌드 옵션에서에서 추가 mtouch 인수도 다음을 추가 하 여 앱 개발자가이 동작을 재정의할 수 있습니다.

- `--mono:static`: 링크는 Mono 런타임과 함께 정적으로 합니다.
- `--mono:framework`: 프레임 워크는 Mono 런타임과 함께 연결 합니다.

확장 없이 앱에 대해서도 프레임 워크로 Mono 런타임과 함께 연결 하는 것에 대 한 하나의 시나리오, Apple가 실행 파일에 강제로 적용 하는 모든 크기 제한을 극복 하기 위해 실행 파일 크기를 줄일 것입니다. 하지만 참조에 대 한 Mono 런타임 (Xamarin.iOS 8.12의 자신의 다름 릴리스에서 앱 간에) 하는 대로 1.7MB 아키텍처 당를 추가 합니다. 약 2.3MB 즉 확장 하지 않고 앱의 단일 아키텍처, 아키텍처 당 앱 링크를 만드는 추가 하는 Mono 프레임 워크는 Mono 런타임과 함께 프레임 워크는 ~1.7MB, 하 여 실행 파일을 축소 하지만 ~2.3MB 프레임 워크를 추가, 생성 ~0.6MB 더 큰 앱 alltogether에서.

