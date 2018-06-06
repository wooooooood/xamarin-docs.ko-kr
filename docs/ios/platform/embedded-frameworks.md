---
title: Xamarin.iOS에서 포함 된 프레임 워크
description: 이 문서에 포함 된 프레임 워크 Xamarin.iOS 응용 프로그램에서 코드를 공유 하는 방법을 설명 합니다. Mtouch 도구 또는 네이티브 참조와이 수행할 수 있습니다.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: e42f0940fe3fc132c9d381907aad5afbe474c4ad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787294"
---
# <a name="embedded-frameworks-in-xamarinios"></a>Xamarin.iOS에서 포함 된 프레임 워크

_이 문서에서는 응용 프로그램 개발자가 자신의 앱에서 사용자 프레임 워크를 포함할 수는 방법을 설명 합니다._

Ios 8.0 Apple 아무런 상관이 응용 프로그램 확장과 Xcode에서 주 앱 간에 코드를 공유 하는 포함 된 프레임 워크를 만들 수 있습니다.

Xamarin.iOS 9.0 Xamarin.iOS 앱에서 이러한 포함 된 프레임 워크 (Xcode를 사용 하 여 만든)를 사용 하기 위한 지원을 추가 합니다. *됩니다 **하지** Xamarin.iOS 프로젝트의 모든 형식에서 포함 된 프레임 워크를 만들고, 기존 네이티브 (Objective-c) 프레임 워크만 사용할 수 있습니다.*

Xamarin.iOS에서 프레임 워크를 사용 하는 방법은 두 가지가 있습니다.

- 다음 프로젝트의 추가 mtouch 인수를 추가 하 여 프레임 워크 mtouch 도구에 전달할 **iOS 빌드** 옵션:

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  이 설정은 각 프로젝트 구성에 대해 설정 됩니다.

- 상황에 맞는 메뉴에서 네이티브 참조를 추가 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

프로젝트 및 찾아보기 네이티브 참조를 추가 하려면 마우스 오른쪽 단추로 클릭

![](embedded-frameworks-images/xam-native-refs.png "Mac 용 Visual Studio에서 네이티브 참조 추가 선택")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

프로젝트 및 찾아보기 네이티브 참조를 추가 하려면 마우스 오른쪽 단추로 클릭

![](embedded-frameworks-images/vs-native-refs.png "Visual Studio에서 추가 기본 참조를 선택 합니다.")

-----

  이 모든 구성에 대해 작동 합니다.

Mac 용 Visual Studio와 Xamarin Tools for Visual Studio의 이후 버전에서 프로젝트 파일을 수동으로 편집) (없이 IDE 내에서 프레임 워크를 사용할 수 됩니다.

몇 가지 예제 프로젝트에서 확인할 수 있습니다 [github](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>제한 사항

- 포함 된 프레임 워크는 에서만 지원 [통합](~/cross-platform/macios/unified/index.md) 프로젝트.
- 포함 된 프레임 워크는 에서만 지원 프로젝트의 배포 대상과 이상 iOS 8.0 합니다.
- 확장 된 포함 된 프레임 워크를 필요한 경우 컨테이너 응용 프로그램에는 프레임 워크, 그렇지 않으면 프레임 워크 앱 번들에 포함 되지 것입니다에 대 한 참조도 있어야 합니다.

## <a name="the-mono-runtime"></a>모노 런타임

내부적으로 Xamarin.iOS에서는이 기능 모노 런타임을 각 확장명과 컨테이너 응용 프로그램에 정적으로 연결 하는 대신 프레임 워크로 모노 런타임 연결을 활용 합니다.

이 컨테이너 응용 프로그램은 통합 앱 확장을 포함 하 고 대상 배포는 iOS 8.0 이상 자동으로 수행 됩니다.

확장명 없이 앱 됩니다 여전히 모노 런타임 정적으로 하므로 연결할 하나의 앱만 참조 하는 경우에 프레임 워크를 사용 하기 위한 크기 저하 됩니다.

이 동작은 iOS 프로젝트의 빌드 옵션에서에서 추가 mtouch 인수로 추가 하 여 응용 프로그램 개발자가 재정의할 수 있습니다.

- `--mono:static`: 링크 모노 런타임 정적으로 합니다.
- `--mono:framework`: 모노 런타임 프레임 워크와 연결 되어 있습니다.

확장명 없이 앱에도 프레임 워크로 모노 런타임 연결을 위한 한 가지 시나리오는 Apple 실행 파일에 적용 된 크기 제한을 해결 하기 위해 실행 파일 크기를 줄일 것입니다. 그러나 참조용으로 모노 런타임 아키텍처 당 약 1.7 MB (변경 됨에 따라 Xamarin.iOS 8.12의 his 릴리스 간에 앱 간에)를 추가 합니다. 모노 프레임 워크 추가 약 2.3 MB 확장명 없이 단일 아키텍처 응용 프로그램에 대 한 즉 아키텍처별 앱의 링크를 만드는 Mono 런타임 프레임 워크는 ~1.7MB, 하 여 실행 파일을 축소 하지만 ~2.3MB 프레임 워크를 추가, 결과 ~0.6MB 큰 앱 alltogether에.

