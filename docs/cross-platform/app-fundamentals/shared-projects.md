---
title: 공유 프로젝트를 사용 하 여 코드 공유
description: 공유 프로젝트를 사용 하면 다양 한 응용 프로그램 프로젝트에서 참조 하는 공통 코드를 작성할 수 있습니다. 코드는 각 참조하는 프로젝트의 일부로 컴파일되며 플랫폼 특정 기능을 공유 코드 베이스에 통합하는 데 도움이 되는 컴파일러 지시문을 포함할 수 있습니다.
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: ed58b0810d3c4fd3a3dd99cddd16227f9ac30273
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "68739054"
---
# <a name="shared-projects-code-sharing"></a>공유 프로젝트 코드 공유

_공유 프로젝트를 사용 하면 다양 한 응용 프로그램 프로젝트에서 참조 하는 공통 코드를 작성할 수 있습니다. 이 코드는 각 참조 하는 프로젝트의 일부로 컴파일되며 플랫폼 특정 기능을 공유 코드 베이스에 통합 하는 데 도움이 되는 컴파일러 지시문을 포함할 수 있습니다._

공유 프로젝트 (공유 자산 프로젝트 라고도 함)를 사용 하면 Xamarin 응용 프로그램을 비롯 한 여러 대상 프로젝트 간에 공유 되는 코드를 작성할 수 있습니다.

공유 프로젝트를 참조 하는 프로젝트의 하위 집합으로 컴파일되도록 플랫폼별 코드를 조건부로 포함할 수 있도록 컴파일러 지시문을 지원 합니다. 컴파일러 지시문을 관리 하 고 각 응용 프로그램에서 코드가 표시 되는 방식을 시각화 하는 데 도움이 되는 IDE 지원도 있습니다.

과거에 파일 연결을 사용 하 여 프로젝트 간에 코드를 공유 하는 경우 공유 프로젝트는 비슷한 방식으로 작동 하지만 IDE 지원이 크게 향상 되었습니다.

## <a name="what-is-a-shared-project"></a>공유 프로젝트 란?

대부분의 다른 프로젝트 형식과 달리, 공유 프로젝트는 DLL 형식으로 출력을 포함 하지 않고 코드를 참조 하는 각 프로젝트로 컴파일됩니다. 이는 아래 다이어그램에 나와 있습니다. 개념적으로 공유 프로젝트의 전체 콘텐츠는 참조 하는 각 프로젝트에 "복사" 되 고 그 일부인 것 처럼 컴파일됩니다.

![](shared-projects-images/sharedassetproject.png "공유 프로젝트 아키텍처")

공유 프로젝트의 코드는 코드를 사용 하는 응용 프로그램 프로젝트에 따라 코드 섹션을 활성화 하거나 비활성화 하는 컴파일러 지시문을 포함할 수 있습니다. 코드는 다이어그램의 색이 지정 된 플랫폼 상자에서 제안 합니다.

공유 프로젝트는 자체적으로 컴파일되지 않으며 다른 프로젝트에 포함 될 수 있는 소스 코드 파일의 그룹 으로만 존재 합니다. 다른 프로젝트에서 참조 하는 경우 코드는 해당 프로젝트의 *일부로* 효과적으로 컴파일됩니다. 공유 프로젝트는 다른 프로젝트 형식 (다른 공유 프로젝트 포함)을 참조할 수 없습니다.

Android 응용 프로그램 프로젝트는 다른 Android 응용 프로그램 프로젝트를 참조할 수 없습니다. 예를 들어 android 단위 테스트 프로젝트는 Android 응용 프로그램 프로젝트를 참조할 수 없습니다. 이 제한 사항에 대 한 자세한 내용은이 [포럼 토론](http://forums.xamarin.com/discussion/comment/98092/)을 참조 하세요.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Mac용 Visual Studio 연습

이 섹션에서는 Mac용 Visual Studio를 사용 하 여 공유 프로젝트를 만들고 사용 하는 방법을 안내 합니다. 전체 예제는 [공유 프로젝트 예제](#Shared_Project_Example) 섹션을 참조 하세요.

## <a name="creating-a-shared-project"></a>공유 프로젝트 만들기

새 공유 프로젝트를 만들려면 **새 솔루션 > 파일로** 이동 합니다. 또는 기존 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 추가 새 프로젝트 추가**...)를 선택 합니다.

[![새 공유 프로젝트](shared-projects-images/xs-newsolution-sml.png "새 솔루션")](shared-projects-images/xs-newsolution.png#lightbox)

다음 화면에서 프로젝트 이름을 선택 하 고 **만들기**를 클릭 합니다.

새 공유 프로젝트가 아래에 표시 됩니다. 참조 또는 구성 요소 노드가 없다는 점에 유의 하십시오. 공유 프로젝트에 대해서는 지원 되지 않습니다.

![빈 공유 프로젝트](shared-projects-images/xs-empty.png "빈 공유 프로젝트")

공유 프로젝트를 유용 하 게 만들려면 하나 이상의 빌드 가능 프로젝트 (예: iOS 또는 Android 응용 프로그램, 라이브러리 또는 PCL 프로젝트)에서 참조 해야 합니다. 공유 프로젝트는 참조 하는 항목이 없는 경우에는 컴파일되지 않으므로 구문 (또는 기타) 오류는 다른 항목에서 참조 될 때까지 강조 표시 되지 않습니다.

공유 프로젝트에 대 한 참조 추가는 일반 라이브러리 프로젝트를 참조 하는 것과 동일한 방식으로 수행 됩니다. 이 스크린샷에서는 공유 프로젝트를 참조 하는 Xamarin.ios 프로젝트를 보여 줍니다.

![](shared-projects-images/xs-reference.png "공유 프로젝트에 대 한 프로젝트 참조")

다른 라이브러리나 응용 프로그램에서 공유 프로젝트를 참조 하면 솔루션을 빌드하고 코드에서 오류를 볼 수 있습니다. 공유 프로젝트를 _둘_ 이상의 다른 프로젝트에서 참조 하는 경우 소스 코드 편집기의 왼쪽 위에이 파일을 참조 하는 프로젝트를 선택 하는 메뉴가 표시 됩니다.

## <a name="shared-project-options"></a>공유 프로젝트 옵션

공유 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **옵션** 을 선택 하면 다른 프로젝트 형식 보다 설정이 줄어듭니다. 공유 프로젝트는 자체적으로 컴파일되지 않으므로 출력 또는 컴파일러 옵션, 프로젝트 구성, 어셈블리 서명 또는 사용자 지정 명령을 설정할 수 없습니다. 공유 프로젝트의 코드는 이러한 값을 참조 하는 모든 항목에서 효과적으로 상속 합니다.

**옵션** 화면은 아래와 같이 표시 됩니다. 프로젝트 **이름과** **기본 네임 스페이스** 는 일반적으로 변경 하는 두 가지 설정 뿐입니다.

![](shared-projects-images/xs-sharedprojectoptions.png "공유 프로젝트 옵션")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-walkthrough"></a>Visual Studio 연습

이 섹션에서는 Visual Studio를 사용 하 여 공유 프로젝트를 만들고 사용 하는 방법을 안내 합니다. 전체 구현을 보려면 [공유 프로젝트 예제](#Shared_Project_Example) 섹션을 참조 하세요.

### <a name="creating-a-shared-project"></a>공유 프로젝트 만들기

새 공유 프로젝트를 만들려면 **파일** > **새로 만들기** > **프로젝트**로 이동 합니다.

Visual Studio 2019의 **새 프로젝트 만들기** 페이지에서 검색 상자에 **공유** 를 입력 합니다. **공유 프로젝트** 템플릿을 선택 하 고 **다음**을 선택 합니다. 프로젝트의 이름을 입력 하 고 **만들기**를 선택 합니다.

Visual Studio 2017에서 **공유 프로젝트** 템플릿을 선택 하 고 프로젝트의 이름을 선택 합니다.

![Visual Studio 2017의 공유 프로젝트 템플릿](shared-projects-images/vs-newsolution.png)

솔루션 파일을 마우스 오른쪽 단추로 클릭 하 고 **새 프로젝트 > 추가**를 선택 하 여 기존 솔루션에 새 공유 프로젝트를 추가할 수도 있습니다. 새 공유 프로젝트는 클래스 파일이 추가 된 후 아래와 같이 표시 됩니다. 참조 또는 구성 요소 노드가 없다는 점에 유의 하십시오. 공유 프로젝트에 대해서는 지원 되지 않습니다.

![](shared-projects-images/vs-empty.png "빈 공유 프로젝트")

공유 프로젝트를 유용 하 게 만들려면 하나 이상의 빌드 가능 프로젝트 (예: iOS 또는 Android 응용 프로그램, 라이브러리 또는 PCL 프로젝트)에서 참조 해야 합니다. 공유 프로젝트는 참조 하는 항목이 없는 경우에는 컴파일되지 않으므로 구문 (또는 기타) 오류는 다른 항목에서 참조 될 때까지 강조 표시 되지 않습니다.

공유 프로젝트에 대 한 참조 추가는 일반 라이브러리 프로젝트를 참조 하는 것과 동일한 방식으로 수행 됩니다. 이 스크린샷에서는 공유 프로젝트를 참조 하는 Xamarin.ios 프로젝트를 보여 줍니다.

![](shared-projects-images/vs-reference.png "공유 프로젝트에 대 한 프로젝트 참조")

다른 라이브러리나 응용 프로그램에서 공유 프로젝트를 참조 하면 솔루션을 빌드하고 코드에서 오류를 볼 수 있습니다. 공유 프로젝트를 _둘_ 이상의 다른 프로젝트에서 참조 하는 경우 소스 코드 편집기의 왼쪽 위에 메뉴가 표시 되어 현재 코드 파일을 참조 하는 프로젝트가 표시 됩니다.

### <a name="shared-project-properties"></a>공유 프로젝트 속성

공유 프로젝트를 선택 하면 속성 패널에서 다른 프로젝트 형식 보다 설정이 줄어듭니다. 공유 프로젝트는 자체적으로 컴파일되지 않으므로 출력 또는 컴파일러 옵션, 프로젝트 구성, 어셈블리 서명 또는 사용자 지정 명령을 설정할 수 없습니다. 공유 프로젝트의 코드는 이러한 값을 참조 하는 모든 항목에서 효과적으로 상속 합니다.

**속성** 패널은 아래와 같습니다. **루트 네임 스페이스** 는 변경할 수 있는 유일한 설정입니다.

![](shared-projects-images/vs-sharedprojectproperties.png "공유 프로젝트 속성")

-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>공유 프로젝트 예제

[Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) 예제에서는 공유 프로젝트를 사용 하 여 IOS, Android 및 Windows Phone 응용 프로그램 모두에서 사용 되는 공통 코드를 포함 합니다. `SQLite.cs` 및`TaskRepository.cs` 소스 코드 파일 모두 컴파일러 지시문을 제품 (예: `#if __ANDROID__`)를 통해 해당 항목을 참조 하는 각 응용 프로그램에 대해 서로 다른 출력을 생성할 수 있습니다.

전체 솔루션 구조는 아래에 표시 됩니다 (각각 Mac용 Visual Studio 및 Visual Studio).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](shared-projects-images/xs-examplesolution.png "Mac용 Visual Studio 솔루션")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](shared-projects-images/vs-examplesolution.png "Visual Studio 솔루션")

-----

프로젝트 형식이 Mac용 Visual Studio에서 컴파일을 지원 하지 않더라도 Mac용 Visual Studio 내에서 Windows Phone 프로젝트를 탐색할 수 있습니다.

실행 중인 응용 프로그램은 다음과 같습니다.

![](shared-projects-images/example.png "iOS, Android Windows Phone 예제")

## <a name="summary"></a>요약

이 문서에서는 공유 프로젝트의 작동 방식, Mac용 Visual Studio와 Visual Studio 모두에서 이러한 프로젝트를 만들고 사용 하는 방법에 대해 설명 하 고, 작업에서 공유 프로젝트를 보여 주는 간단한 예제 응용 프로그램을 소개 했습니다.

## <a name="related-links"></a>관련 링크

- [Tasky 샘플 앱](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [이식 가능한 클래스 라이브러리 (샘플)](~/cross-platform/app-fundamentals/pcl.md)
- [코드 공유 옵션 (샘플)](~/cross-platform/app-fundamentals/code-sharing.md)
