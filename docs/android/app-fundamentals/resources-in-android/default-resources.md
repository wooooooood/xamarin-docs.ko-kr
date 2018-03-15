---
title: "기본 리소스"
ms.topic: article
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 5cf5bd38612f0f763e30456b0dd42198a3c0ff06
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="default-resources"></a>기본 리소스

기본 리소스는 많으므로 Android 운영 체제에서 기본적으로 선택 되지 않은 경우 보다 구체적인을 특정 장치나 폼 팩터에 한정 되지 않는 항목을 들 수 있습니다. 따라서 가장 일반적인 유형의 리소스를 만드는 일입니다. 하위 디렉터리를 구성 하는 **리소스** 해당 리소스 종류에 따라 디렉터리:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![기본 리소스 파일](default-resources-images/01-resource-files-vs.png)

위의 그림에서 프로젝트에 그릴 수 있는 리소스, 레이아웃 및 값 (단순 값이 포함 된 XML 파일)에 대 한 기본 값이 있습니다.

리소스 종류의 전체 목록은 아래에 제공 됩니다.

-  **애니메이터** &ndash; 속성 애니메이션을 설명 하는 XML 파일입니다.
   속성 애니메이션 API 수준 (Android 3.0) 11에에서 도입 된 고 애니메이션 개체에서 속성을 제공 합니다. 속성 애니메이션은 모든 형식의 개체에 애니메이션을 설명 하는 보다 유연 하 고 강력한 방법입니다.

-  **애니메이션** &ndash; 설명 하는 XML 파일 *트윈* 애니메이션 합니다. 트윈 애니메이션은 일련의 이미지 또는 텍스트의 크기 증가 보기 개체 또는 예제에서는 회전의 내용에 대 한 변환을 수행 하는 애니메이션 명령입니다. 트윈 애니메이션 개체를 볼만 제한 됩니다.

-  **색** &ndash; 색 상태 목록을 설명 하는 XML 파일입니다. 상태 목록 색을 이해 하려면 단추와 같은 UI 위젯을 것이 좋습니다.
   와 같은 눌려 지거나 사용 하지 않도록 설정, 다른 상태에 있을 수 있습니다 하 고 단추 각 변경 상태에서 색을 변경할 수 있습니다. 목록 상태 목록에 표시 됩니다.

-  **그릴** &ndash; 그릴 수 있는 리소스는 그래픽 또는 될 수 있는 응용 프로그램으로 컴파일된 하 고 다음 API 호출에 의해 액세스 다른 XML 리소스도에서 참조에 대 한 일반 개념입니다.
   Drawables의 몇 가지 예는 비트맵 파일 (.png,.gif,.jpg), 라고 하는 특별 한 크기 조정 가능한 비트맵 [9 패치](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), 상태 목록 등 XML에 정의 된 제네릭 셰이프입니다.
 
-  **레이아웃** &ndash; 활동 목록에서 행과 같은 사용자 인터페이스 레이아웃을 설명 하는 XML 파일입니다.

-  **메뉴** &ndash; 와 같은 응용 프로그램 메뉴를 설명 하는 XML 파일 *옵션 메뉴*, *상황에 맞는 메뉴*, 및 *하위*합니다. 예를 보려면 메뉴 참조는 [팝업 메뉴 데모](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) 또는 [표준 컨트롤](https://developer.xamarin.com/samples/mobile/StandardControls/) 샘플.

-  **원시** &ndash; 원시, 이진 형식으로 저장 된 임의 파일입니다. 이러한 파일은 이진 형식에서 Android 응용 프로그램으로 컴파일됩니다.

-  **값** &ndash; 단순 값이 포함 된 XML 파일입니다. 단일 리소스를 정의 하는 대신 여러 리소스를 정의할 수 하는 값 디렉터리에 XML 파일. 예를 들어 XML 파일 한 개는 다른 XML 파일 색 값의 목록을 보유 하는 동안 문자열 값의 목록을 보유할 수 있습니다.

-  **xml** &ndash; .NET 구성 파일에 사용 되는 XML 파일입니다. 이 응용 프로그램에서 런타임에 읽을 수 있는 임의의 XML


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![기본 리소스 파일](default-resources-images/01-resource-files-xs.png)

위의 그림에서 프로젝트에 그릴 수 있는 리소스, 레이아웃 및 값 (단순 값이 포함 된 XML 파일)에 대 한 기본 값이 있습니다.

리소스 종류의 전체 목록은 아래에 제공 됩니다.

-  **애니메이터** &ndash; 속성 애니메이션을 설명 하는 XML 파일입니다.
   속성 애니메이션 API 수준 (Android 3.0) 11에에서 도입 된 고 애니메이션 개체에서 속성을 제공 합니다. 속성 애니메이션은 모든 형식의 개체에 애니메이션을 설명 하는 보다 유연 하 고 강력한 방법입니다.

-  **애니메이션** &ndash; 설명 하는 XML 파일 *트윈* 애니메이션 합니다. 트윈 애니메이션은 일련의 이미지 또는 텍스트의 크기 증가 보기 개체 또는 예제에서는 회전의 내용에 대 한 변환을 수행 하는 애니메이션 명령입니다. 트윈 애니메이션 개체를 볼만 제한 됩니다.

-  **색** &ndash; 색 상태 목록을 설명 하는 XML 파일입니다. 상태 목록 색을 이해 하려면 단추와 같은 UI 위젯을 것이 좋습니다.
   와 같은 눌려 지거나 사용 하지 않도록 설정, 다른 상태를 갖도록 해야 하 고 단추 각 변경 상태에서 색을 변경할 수 있습니다. 목록 상태 목록에 표시 됩니다.

-  **글꼴** &ndash; API 수준 26 년부터 Android 응용 프로그램에 글꼴 한 리소스로 포함할 수는 있습니다. 지원 라이브러리 26 backport 글꼴 14 API 수준으로 됩니다. 응용 프로그램을 사용 하면 글꼴 포함

-  **mip 맵** &ndash; 그릴 수 있는 리소스는 그래픽 또는 될 수 있는 응용 프로그램으로 컴파일된 하 고 다음 API 호출에 의해 액세스 다른 XML 리소스도에서 참조에 대 한 일반 개념입니다.
   Drawables의 몇 가지 예는 비트맵 파일 (.png,.gif,.jpg), 라고 하는 특별 한 크기 조정 가능한 비트맵 [9 패치](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), 상태 목록 등 XML에 정의 된 제네릭 셰이프입니다.

-  **레이아웃** &ndash; 활동 목록에서 행과 같은 사용자 인터페이스 레이아웃을 설명 하는 XML 파일입니다.

-  **메뉴** &ndash; 와 같은 응용 프로그램 메뉴를 설명 하는 XML 파일 *옵션 메뉴*, *상황에 맞는 메뉴*, 및 *하위*합니다. 예를 보려면 메뉴 참조는 [팝업 메뉴 데모](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) 또는 [표준 컨트롤](https://developer.xamarin.com/samples/mobile/StandardControls/) 샘플.

-  **원시** &ndash; 원시, 이진 형식으로 저장 된 임의 파일입니다. 이러한 파일은 이진 형식에서 Android 응용 프로그램으로 컴파일됩니다.

-  **값** &ndash; 단순 값이 포함 된 XML 파일입니다. 단일 리소스를 정의 하는 대신 여러 리소스를 정의할 수 하는 값 디렉터리에 XML 파일. 예를 들어 XML 파일 한 개는 다른 XML 파일 색 값의 목록을 보유 하는 동안 문자열 값의 목록을 보유할 수 있습니다.

-  **xml** &ndash; .NET 구성 파일에 사용 되는 XML 파일입니다. 이 응용 프로그램에서 런타임에 읽을 수 있는 임의의 XML

-----
