---
title: Xamarin.ios의 포함 프레임 워크
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 포함 된 프레임 워크를 사용 하 여 코드를 공유 하는 방법을 설명 합니다. Mtouch 도구나 네이티브 참조를 사용 하 여이 작업을 수행할 수 있습니다.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: 6287dca8660c1147455beb22304b7f8637ac7fa5
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292760"
---
# <a name="embedded-frameworks-in-xamarinios"></a>Xamarin.ios의 포함 프레임 워크

_이 문서에서는 응용 프로그램 개발자가 자신의 앱에 사용자 프레임 워크를 포함 하는 방법을 설명 합니다._

IOS 8.0 Apple을 사용 하면 포함 된 프레임 워크를 만들어 앱 확장과 Xcode의 주 앱 간에 코드를 공유할 수 있습니다.

Xamarin.ios 9.0은 Xamarin.ios 앱에서 이러한 포함 프레임 워크 (Xcode로 만듦)를 사용 하기 위한 지원을 추가 합니다. *모든 형식의 Xamarin.ios 프로젝트에서 포함 된 프레임 워크를 만들 수는 **없으며** 기존 네이티브 (목적-C) 프레임 워크도 사용 합니다.*

Xamarin.ios에서 프레임 워크를 사용 하는 방법에는 두 가지가 있습니다.

- 프로젝트의 **IOS 빌드** 옵션에서 추가 mtouch 인수에 다음을 추가 하 여 프레임 워크를 mtouch 도구에 전달 합니다.

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  각 프로젝트 구성에 대해 설정 해야 합니다.

- 상황에 맞는 메뉴에서 네이티브 참조 추가

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

프로젝트를 마우스 오른쪽 단추로 클릭 하 고 기본 참조를 찾아 추가 합니다.

![](embedded-frameworks-images/xam-native-refs.png "Mac용 Visual Studio에서 네이티브 참조 추가를 선택 합니다.")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

프로젝트를 마우스 오른쪽 단추로 클릭 하 고 기본 참조를 찾아 추가 합니다.

![](embedded-frameworks-images/vs-native-refs.png "Visual Studio에서 네이티브 참조 추가를 선택 합니다.")

-----

  모든 구성에 적용 됩니다.

이후 버전의 Mac용 Visual Studio 및 Visual Studio 용 Xamarin 도구를 사용 하면 프로젝트 파일을 수동으로 편집 하지 않고도 IDE 내에서 프레임 워크를 사용할 수 있습니다.

[Github](https://github.com/rolfbjarne/embedded-frameworks) 에서 몇 가지 샘플 프로젝트를 찾을 수 있습니다.

## <a name="limitations"></a>제한 사항

- 포함 프레임 워크는 [통합](~/cross-platform/macios/unified/index.md) 프로젝트 에서만 지원 됩니다.
- 포함 프레임 워크는 최소한 iOS 8.0의 배포 대상이 있는 프로젝트 에서만 지원 됩니다.
- 확장에 포함 된 프레임 워크가 필요한 경우 컨테이너 앱에도 프레임 워크에 대 한 참조가 있어야 합니다. 그렇지 않으면 프레임 워크가 앱 번들에 포함 되지 않습니다.

## <a name="the-mono-runtime"></a>Mono 런타임

내부적으로 Xamarin.ios는 Mono 런타임을 각 확장 및 컨테이너 앱에 정적으로 연결 하는 대신이 기능을 활용 하 여 Mono 런타임과 프레임 워크로 연결 합니다.

컨테이너 앱이 통합 앱이 고, 확장을 포함 하 고, 대상 배포가 iOS 8.0 이상인 경우 자동으로 수행 됩니다.

확장이 없는 앱은 프레임 워크를 참조 하는 앱이 하나만 있는 경우 프레임 워크를 사용 하는 데 크기 제한이 있으므로 정적으로 Mono 런타임과 계속 연결 됩니다.

이 동작은 프로젝트의 iOS 빌드 옵션에 다음을 추가 mtouch 인수로 추가 하 여 앱 개발자가 재정의할 수 있습니다.

- `--mono:static`: 정적으로 Mono 런타임에 연결 합니다.
- `--mono:framework`: Mono 런타임에 프레임 워크로 링크 합니다.

확장이 없는 앱의 경우에도 Mono 런타임을 프레임 워크로 연결 하는 한 가지 시나리오는 실행 파일 크기를 줄여 실행 파일에 대해 Apple에서 적용 하는 크기 제한을 극복 하는 것입니다. 참조를 위해 Mono 런타임은 아키텍처 당 약 1.7 MB를 추가 합니다 (Xamarin.ios 8.12는 물론 릴리스 간에는 다르며 앱 사이에도 다름). Mono 프레임 워크는 아키텍처 당 약 2.3 MB를 추가 합니다. 즉, 확장이 없는 단일 아키텍처 앱의 경우 앱 링크를 Mono 런타임에 프레임 워크로 사용 하면 실행 파일이 ~ 1.7 MB를 축소 하 고, 결과는 ~ 2.3 MB 프레임 워크를 추가 합니다. ~ 0.6 MB의 큰 앱은 모두 함께 작동 합니다.

