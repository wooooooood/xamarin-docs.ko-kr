---
title: 기본 리소스
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: a1f2016af3bcac338f47b7315a26fe50ae76fee7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755097"
---
# <a name="default-resources"></a>기본 리소스

기본 리소스는 특정 장치나 폼 팩터에 한정 되지 않는 항목 이므로 더 이상 특정 리소스를 찾을 수 없는 경우 Android OS에서 기본 선택입니다. 따라서 가장 일반적인 형식의 리소스를 만들 수 있습니다. 리소스 종류에 따라 **Resources** 디렉터리의 하위 디렉터리로 구성 됩니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![기본 리소스 파일](default-resources-images/01-resource-files-vs.png)

위의 이미지에서 프로젝트에는 그릴 때 사용할 리소스, 레이아웃 및 값 (단순 값을 포함 하는 XML 파일)의 기본값이 있습니다.

리소스 종류의 전체 목록은 아래에 제공 되어 있습니다.

- **애니메이터** &ndash; 속성 애니메이션을 설명 하는 XML 파일입니다.
   속성 애니메이션은 API 수준 11 (Android 3.0)에서 도입 되었으며 개체의 속성에 대 한 애니메이션을 제공 합니다. 속성 애니메이션은 모든 형식의 개체에 대 한 애니메이션을 설명 하는 보다 유연 하 고 강력한 방법입니다.

- **anim** 트윈 애니메이션을 설명 하는 XML 파일입니다. &ndash; 트윈 애니메이션은 뷰 개체의 내용에 대 한 변환을 수행 하거나 이미지를 회전 하거나 텍스트 크기를 확대 하는 일련의 애니메이션 명령입니다. 트윈 애니메이션은 뷰 개체로만 제한 됩니다.

- **색** &ndash; 색의 상태 목록을 설명 하는 XML 파일입니다. 색 상태 목록을 이해 하려면 단추와 같은 UI 위젯을 고려 합니다.
   눌러져 있거나 사용 하지 않도록 설정 된 상태와 같은 서로 다른 상태를 가질 수 있으며,이 단추는 상태 변화 마다 색을 변경할 수 있습니다. 목록은 상태 목록으로 표시 됩니다.

- **그릴** 때 &ndash; 그릴 수 있는 리소스는 응용 프로그램으로 컴파일된 다음 API 호출을 통해 액세스 하거나 다른 XML 리소스에서 참조할 수 있는 그래픽의 일반적인 개념입니다.
   Drawables의 몇 가지 예로는 비트맵 파일 (.png, .gif, .jpg), [9 개 패치](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), 상태 목록, XML로 정의 된 일반 도형 등의 크기 조정 가능한 특수 한 비트맵이 있습니다.

- **레이아웃** &ndash; 작업 또는 목록의 행과 같은 사용자 인터페이스 레이아웃을 설명 하는 XML 파일입니다.

- **메뉴** 옵션 메뉴, *상황에 맞는 메뉴*및 *하위 메뉴*와 같은 응용 프로그램 메뉴를 설명 하는 XML 파일입니다. &ndash; 메뉴의 예제는 [팝업 메뉴 데모](https://docs.microsoft.com/samples/xamarin/monodroid-samples/popupmenudemo) 또는 [표준 컨트롤](https://docs.microsoft.com/samples/xamarin/mobile-samples/standardcontrols/) 샘플을 참조 하세요.

- **원시** &ndash; 원시, 이진 형식으로 저장 된 임의 파일 이러한 파일은 Android 응용 프로그램에 이진 형식으로 컴파일됩니다.

- **값** &ndash; 단순 값을 포함 하는 XML 파일입니다. Values 디렉터리의 XML 파일은 단일 리소스를 정의 하지 않고, 대신 여러 리소스를 정의할 수 있습니다. 예를 들어 하나의 XML 파일에는 문자열 값 목록이 포함 될 수 있지만 다른 XML 파일에는 색 값 목록이 포함 될 수 있습니다.

- .net 구성 파일과 유사 하 게 작동 하는 **xml** &ndash; xml 파일입니다. 응용 프로그램에서 런타임에 읽을 수 있는 임의 XML입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![기본 리소스 파일](default-resources-images/01-resource-files-xs.png)

위의 이미지에서 프로젝트에는 그릴 때 사용할 리소스, 레이아웃 및 값 (단순 값을 포함 하는 XML 파일)의 기본값이 있습니다.

리소스 종류의 전체 목록은 아래에 제공 되어 있습니다.

- **애니메이터** &ndash; 속성 애니메이션을 설명 하는 XML 파일입니다.
   속성 애니메이션은 API 수준 11 (Android 3.0)에서 도입 되었으며 개체의 속성에 대 한 애니메이션을 제공 합니다. 속성 애니메이션은 모든 형식의 개체에 대 한 애니메이션을 설명 하는 보다 유연 하 고 강력한 방법입니다.

- **anim** 트윈 애니메이션을 설명 하는 XML 파일입니다. &ndash; 트윈 애니메이션은 뷰 개체의 내용에 대 한 변환을 수행 하거나 이미지를 회전 하거나 텍스트 크기를 확대 하는 일련의 애니메이션 명령입니다. 트윈 애니메이션은 뷰 개체로만 제한 됩니다.

- **색** &ndash; 색의 상태 목록을 설명 하는 XML 파일입니다. 색 상태 목록을 이해 하려면 단추와 같은 UI 위젯을 고려 합니다.
   이 작업은 누름 또는 사용 안 함과 같은 다른 상태를 가질 수 있으며, 상태 변경 마다 단추가 색을 변경할 수 있습니다. 목록은 상태 목록으로 표시 됩니다.

- **글꼴** &ndash; API 수준 26부터 Android 응용 프로그램의 리소스로 글꼴을 포함할 수 있습니다. 지원 라이브러리 26은 포트 글꼴을 API 수준 14로 백 포팅하 글꼴을 포함 하면 응용 프로그램에서 사용 하기 전에 자산으로 가져올 필요 없이 XML 레이아웃에서 직접 사용자 지정 글꼴을 로드할 수 있습니다.

- **밉 맵** &ndash; 그릴 수 있는 리소스는 응용 프로그램으로 컴파일된 다음 API 호출을 통해 액세스 하거나 다른 XML 리소스에서 참조할 수 있는 그래픽의 일반적인 개념입니다.
   Drawables의 몇 가지 예로는 비트맵 파일 (.png, .gif, .jpg), [9 개 패치](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), 상태 목록, XML로 정의 된 일반 도형 등의 크기 조정 가능한 특수 한 비트맵이 있습니다.

- **레이아웃** &ndash; 작업 또는 목록의 행과 같은 사용자 인터페이스 레이아웃을 설명 하는 XML 파일입니다.

- **메뉴** 옵션 메뉴, *상황에 맞는 메뉴*및 *하위 메뉴*와 같은 응용 프로그램 메뉴를 설명 하는 XML 파일입니다. &ndash; 메뉴의 예제는 [팝업 메뉴 데모](https://docs.microsoft.com/samples/xamarin/monodroid-samples/popupmenudemo) 또는 [표준 컨트롤](https://docs.microsoft.com/samples/xamarin/mobile-samples/standardcontrols/) 샘플을 참조 하세요.

- **원시** &ndash; 원시, 이진 형식으로 저장 된 임의 파일 이러한 파일은 Android 응용 프로그램에 이진 형식으로 컴파일됩니다.

- **값** &ndash; 단순 값을 포함 하는 XML 파일입니다. Values 디렉터리의 XML 파일은 단일 리소스를 정의 하지 않고, 대신 여러 리소스를 정의할 수 있습니다. 예를 들어 하나의 XML 파일에는 문자열 값 목록이 포함 될 수 있지만 다른 XML 파일에는 색 값 목록이 포함 될 수 있습니다.

- .net 구성 파일과 유사 하 게 작동 하는 **xml** &ndash; xml 파일입니다. 응용 프로그램에서 런타임에 읽을 수 있는 임의 XML입니다.

-----
