---
title: 기본 리소스
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 20865b71cce16f57b84a1c54986bd84180d3e190
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013217"
---
# <a name="default-resources"></a>기본 리소스

기본 리소스는 항목을 특정 장치나 폼 팩터에 특정 하지 않으며 따라서은 Android OS에서 기본적으로 선택 되지 않은 경우 보다 구체적인 리소스를 찾을 수 있습니다. 이와 같이 가장 일반적인 유형의 리소스를 만드는 일입니다. 하위 디렉터리에 이들 구성 합니다 **리소스** 해당 리소스 종류에 따라 디렉터리:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![기본 리소스 파일](default-resources-images/01-resource-files-vs.png)

위의 이미지를 프로젝트에는 드로어 블 리소스, 레이아웃 및 값 (단순 값이 포함 된 XML 파일)에 대 한 기본값입니다.

리소스 종류의 전체 목록은 아래에 제공 됩니다.

-  **애니메이터** &ndash; 속성 애니메이션을 설명 하는 XML 파일입니다.
   속성 애니메이션 API 수준 (Android 3.0) 11에에서 도입 하 고 개체에 대 한 속성 애니메이션에 대 한 제공 합니다. 속성 애니메이션은 모든 형식의 개체에 애니메이션을 설명 하는 보다 유연 하 고 강력한 방법입니다.

-  **애니메이션** &ndash; 설명 하는 XML 파일 *트윈* 애니메이션 합니다. 트윈 애니메이션은 일련의 애니메이션 이미지 또는 텍스트의 크기를 증가 보기 개체 또는 등 회전의 내용에 대 한 변환을 수행 하는 지침입니다. 트윈 애니메이션 개체를 볼만 제한 됩니다.

-  **색** &ndash; 색 상태 목록에 설명 하는 XML 파일입니다. 색 상태 목록을 이해 하려면 단추와 같은 UI 위젯을 것이 좋습니다.
   다른 상태와 같은 누르거나 비활성화 있 및 단추 각 변경 상태에서를 사용 하 여 색을 변경할 수 있습니다. 목록 상태 목록에 표시 됩니다.

-  **drawable** &ndash; 드로어 블 리소스는 수는 응용 프로그램으로 컴파일되고 및 다음 API 호출에 의해 액세스 되거나 다른 XML 리소스도에서 참조 하는 그래픽에 대 한 일반적인 개념입니다.
   드로어 블의 몇 가지 예는 비트맵 파일 (.png,.gif,.jpg), 라고 하는 특별 한 크기를 조정할 비트맵 [9 패치](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), 상태 목록 등 XML에 정의 된 제네릭 셰이프.
 
-  **레이아웃** &ndash; 활동 또는 목록의 행을 같은 사용자 인터페이스 레이아웃을 설명 하는 XML 파일입니다.

-  **메뉴** &ndash; 와 같은 응용 프로그램 메뉴를 설명 하는 XML 파일 *옵션 메뉴*에 *상황에 맞는 메뉴*, 및 *메뉴가*합니다. 메뉴의 예제를 참조 합니다 [팝업 메뉴 데모](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) 또는 [표준 컨트롤](https://developer.xamarin.com/samples/mobile/StandardControls/) 샘플입니다.

-  **원시** &ndash; 원시 이진 형식의 저장 된 임의 파일입니다. 이러한 파일은 Android 응용 프로그램 이진 형식에서으로 컴파일됩니다.

-  **값** &ndash; 간단한 값이 포함 된 XML 파일입니다. 값 디렉터리에 XML 파일을 단일 리소스를 정의 하지 않습니다 하지만 대신 여러 리소스를 정의할 수 있습니다. 예를 들어 다른 XML 파일 색 값의 목록을 보유 하는 동안 하나의 XML 파일에 문자열 값의 목록을 보유할 수 있습니다.

-  **xml** &ndash; .NET 구성 파일에 비슷한 방식에서으로 작동 하는 XML 파일입니다. 이들은 응용 프로그램에서 런타임 시 읽을 수 있는 임의의 XML입니다.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![기본 리소스 파일](default-resources-images/01-resource-files-xs.png)

위의 이미지를 프로젝트에는 드로어 블 리소스, 레이아웃 및 값 (단순 값이 포함 된 XML 파일)에 대 한 기본값입니다.

리소스 종류의 전체 목록은 아래에 제공 됩니다.

-  **애니메이터** &ndash; 속성 애니메이션을 설명 하는 XML 파일입니다.
   속성 애니메이션 API 수준 (Android 3.0) 11에에서 도입 하 고 개체에 대 한 속성 애니메이션에 대 한 제공 합니다. 속성 애니메이션은 모든 형식의 개체에 애니메이션을 설명 하는 보다 유연 하 고 강력한 방법입니다.

-  **애니메이션** &ndash; 설명 하는 XML 파일 *트윈* 애니메이션 합니다. 트윈 애니메이션은 일련의 애니메이션 이미지 또는 텍스트의 크기를 증가 보기 개체 또는 등 회전의 내용에 대 한 변환을 수행 하는 지침입니다. 트윈 애니메이션 개체를 볼만 제한 됩니다.

-  **색** &ndash; 색 상태 목록에 설명 하는 XML 파일입니다. 색 상태 목록을 이해 하려면 단추와 같은 UI 위젯을 것이 좋습니다.
   다른 상태와 같은 누르거나 비활성화 해야 하 고 단추를 각 변경 상태에서를 사용 하 여 색을 변경할 수 있습니다. 목록 상태 목록에 표시 됩니다.

-  **글꼴** &ndash; API 수준 26부터 Android 응용 프로그램 리소스로 글꼴을 포함할 수는 있습니다. 지원 라이브러리 26 API 수준 14 글꼴 백 포팅 됩니다. 글꼴 포함 응용 프로그램을을 사용 하기 전에 자산으로 가져올 필요 없이 XML 레이아웃에서 직접 사용자 지정 글꼴을 로드할 수 있습니다.

-  **mipmap** &ndash; 드로어 블 리소스는 수는 응용 프로그램으로 컴파일되고 및 다음 API 호출에 의해 액세스 되거나 다른 XML 리소스도에서 참조 하는 그래픽에 대 한 일반적인 개념입니다.
   드로어 블의 몇 가지 예는 비트맵 파일 (.png,.gif,.jpg), 라고 하는 특별 한 크기를 조정할 비트맵 [9 패치](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), 상태 목록 등 XML에 정의 된 제네릭 셰이프.

-  **레이아웃** &ndash; 활동 또는 목록의 행을 같은 사용자 인터페이스 레이아웃을 설명 하는 XML 파일입니다.

-  **메뉴** &ndash; 와 같은 응용 프로그램 메뉴를 설명 하는 XML 파일 *옵션 메뉴*에 *상황에 맞는 메뉴*, 및 *메뉴가*합니다. 메뉴의 예제를 참조 합니다 [팝업 메뉴 데모](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) 또는 [표준 컨트롤](https://developer.xamarin.com/samples/mobile/StandardControls/) 샘플입니다.

-  **원시** &ndash; 원시 이진 형식의 저장 된 임의 파일입니다. 이러한 파일은 Android 응용 프로그램 이진 형식에서으로 컴파일됩니다.

-  **값** &ndash; 간단한 값이 포함 된 XML 파일입니다. 값 디렉터리에 XML 파일을 단일 리소스를 정의 하지 않습니다 하지만 대신 여러 리소스를 정의할 수 있습니다. 예를 들어 다른 XML 파일 색 값의 목록을 보유 하는 동안 하나의 XML 파일에 문자열 값의 목록을 보유할 수 있습니다.

-  **xml** &ndash; .NET 구성 파일에 비슷한 방식에서으로 작동 하는 XML 파일입니다. 이 응용 프로그램에서 런타임 시 읽을 수 있는 임의의 XML

-----
