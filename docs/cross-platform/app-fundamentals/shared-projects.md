---
title: 공유 코드를 공유 하는 프로젝트 사용
description: 공유 프로젝트의 여러 다른 응용 프로그램 프로젝트에서 참조 되는 일반적인 코드를 작성할 수 있습니다. 코드를 참조 하는 각 프로젝트의 일부로 컴파일되고 공유 코드 베이스에 플랫폼 특정 기능을 통합 하는 데 컴파일러 지시문을 포함할 수 있습니다.
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 61096635cd94d0fdd0abe6fda59c4efa41eeceb1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61193235"
---
# <a name="shared-projects-code-sharing"></a>공유 프로젝트 코드 공유

_공유 프로젝트의 여러 다른 응용 프로그램 프로젝트에서 참조 되는 일반적인 코드를 작성할 수 있습니다. 코드를 참조 하는 각 프로젝트의 일부로 컴파일되고 공유 코드 베이스에 플랫폼 특정 기능을 통합 하는 데 컴파일러 지시문을 포함할 수 있습니다._

(또한 공유 자산 프로젝트 라고도) 하는 공유 프로젝트를 통해 Xamarin 응용 프로그램을 포함 하 여 여러 대상 프로젝트 간에 공유 되는 코드를 작성할 수 있습니다.

공유 프로젝트 참조는 프로젝트의 하위 집합으로 컴파일하려 플랫폼별 코드를 조건부로 포함할 수 있습니다 있도록 컴파일러 지시문을 지원 합니다. 각 응용 프로그램에서 코드를 어떻게 나타나는지를 시각화 하 고 컴파일러 지시문을 관리 하려면 IDE 지원 이기도 합니다.

이전에 파일 링크 프로젝트 간에 코드 공유를 사용한 하는 경우 공유 프로젝트에 훨씬 향상 된 IDE 지원 하지만 비슷한 방식으로 작동 합니다.

## <a name="what-is-a-shared-project"></a>공유 프로젝트 란?

대부분의 다른 프로젝트 형식과 달리 공유 프로젝트에 모든 출력이 없습니다 (형태로 DLL)를 참조 하는 각 프로젝트에 코드를 컴파일할 하는 대신 합니다. 이 코드는 아래 다이어그램에 설명 되어 있습니다-공유 프로젝트의 전체 내용을 개념적으로 "복사" 각 참조 하는 프로젝트 및 그 중 일부 처럼 컴파일됩니다.

![](shared-projects-images/sharedassetproject.png "공유 프로젝트 아키텍처")

공유 프로젝트의 코드는 사용 하도록 설정 하거나 프로젝트에 다이어그램에서 색이 지정 된 플랫폼 상자에서 제안한 하는 코드를 사용 하는 점에 응용 프로그램에 따라 코드의 섹션을 사용 하지 않도록 설정 하는 컴파일러 지시문을 포함할 수 있습니다.

자체적으로 공유 프로젝트를 컴파일되지 않습니다 가져오기, 다른 프로젝트에 포함 될 수 있는 소스 코드 파일의 그룹화를 전적으로 존재 합니다. 다른 프로젝트에서 참조 하는 경우 코드로 효과적으로 컴파일된 *파트* 해당 프로젝트의 합니다. 공유 프로젝트 (다른 공유 프로젝트 포함) 다른 프로젝트 형식을 참조할 수 없습니다.

참고 Android 응용 프로그램 프로젝트 다른 Android 응용 프로그램 프로젝트를 참조할 수 없습니다.-예를 들어, Android를 단위 테스트 프로젝트는 Android 응용 프로그램 프로젝트를 참조할 수 없습니다. 이 제한에 대 한 자세한 내용은 참조 [포럼 토론](http://forums.xamarin.com/discussion/comment/98092/)합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Mac용 Visual Studio 연습

이 섹션을 만들고 mac 용 Visual Studio를 사용 하는 공유 프로젝트를 사용 하는 방법을 안내합니다 참조는 하 [프로젝트 예제에서는 공유](#Shared_Project_Example) 전체 예제에 대 한 섹션입니다.

## <a name="creating-a-shared-project"></a>공유 프로젝트 만들기

새 공유 프로젝트를 이동할 만들려면 **파일 > 새 솔루션...**  (또는 기존 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 프로젝트 추가...** ):

[![새 공유 프로젝트](shared-projects-images/xs-newsolution-sml.png "새 솔루션")](shared-projects-images/xs-newsolution.png#lightbox)

다음 화면에서 프로젝트 이름을 선택 하 고 클릭 **만들기**합니다.

새 공유 프로젝트는 다음과 같습니다.-개의 참조가 없는 또는 구성 요소 노드 공유 프로젝트에 대 한 지원 되지 않습니다.

![빈 프로젝트를 공유](shared-projects-images/xs-empty.png "빈 공유 프로젝트")

유용 하 게 공유 프로젝트에 대해 하나 이상의 빌드 가능한 프로젝트 (예: iOS 또는 Android 응용 프로그램 또는 라이브러리 또는 PCL 프로젝트를) 참조할 수 해야 합니다. 공유 프로젝트에 대해서는, 따라서 구문 (또는 다른)를 참조 하는 경우 컴파일된 가져오기지 않습니다 오류 강조 표시 되지 것입니다까지 다른 작업에 의해 참조 되었습니다.

공유 프로젝트에 대 한 참조를 추가 하면 일반 라이브러리 프로젝트 참조와 동일한 방식으로 수행 됩니다. 이 스크린샷에서 Xamarin.iOS 프로젝트를 공유 프로젝트 참조를 보여 줍니다.

![](shared-projects-images/xs-reference.png "공유 프로젝트에 대 한 프로젝트 참조")

다른 라이브러리 또는 응용 프로그램에서 공유 프로젝트 참조 되 면 솔루션을 빌드할 수 있으며 코드에서 오류를 봅니다. 공유 프로젝트에서 참조 하는 경우 _둘 이상의_ 다른 프로젝트 메뉴 나타나는 소스 코드 편집기의 왼쪽 위를 보여 줍니다가이 파일을 참조 하는 프로젝트를 선택 합니다.

## <a name="shared-project-options"></a>공유 프로젝트 옵션

공유 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하는 경우 **옵션** 다른 보다 더 적은 설정이 프로젝트 형식 있습니다. 공유 프로젝트 (자체적으로) 컴파일되므로 출력 또는 컴파일러 옵션, 프로젝트 구성, 어셈블리 서명, 또는 사용자 지정 명령을 설정할 수 없습니다. 공유 프로젝트의 코드는 효과적으로 참조 하는 모든 항목에서 이러한 값을 상속 합니다.



합니다 **옵션** 화면이 프로젝트 아래에 표시 됩니다 **이름** 및 **Default Namespace** 은 일반적으로 변경 하는 두 개의 설정 합니다.


![](shared-projects-images/xs-sharedprojectoptions.png "공유 프로젝트 옵션")



# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)



## <a name="visual-studio-walkthrough"></a>Visual Studio 연습


이 섹션은 만들고 Visual Studio를 사용 하 여 공유 프로젝트를 사용 하는 방법을 안내 합니다. 참조는 하 [프로젝트 예제에서는 공유](#Shared_Project_Example) 완전 한 구현에 대 한 섹션입니다.

### <a name="creating-a-shared-project"></a>공유 프로젝트 만들기

새 공유 프로젝트를 이동할 만들려면 **파일 > 새 솔루션...**  프로젝트 및 솔루션 이름을 선택 합니다.

![](shared-projects-images/vs-newsolution.png "새 솔루션")

솔루션 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 새 공유 프로젝트를 솔루션에 추가할 수도 있습니다 **추가 > 새 프로젝트...** . 새 공유 프로젝트 같습니다 (클래스 파일을 추가한 후에)-참조 되지 않는 알 수 있습니다. 또는 구성 요소 노드 공유 프로젝트에 대 한 지원 되지 않습니다.

![](shared-projects-images/vs-empty.png "빈 공유 프로젝트")

유용 하 게 공유 프로젝트에 대해 하나 이상의 빌드 가능한 프로젝트 (예: iOS 또는 Android 응용 프로그램 또는 라이브러리 또는 PCL 프로젝트를) 참조할 수 해야 합니다. 공유 프로젝트에 대해서는, 따라서 구문 (또는 다른)를 참조 하는 경우 컴파일된 가져오기지 않습니다 오류 강조 표시 되지 것입니다까지 다른 작업에 의해 참조 되었습니다.

공유 프로젝트에 대 한 참조를 추가 하면 일반 라이브러리 프로젝트 참조와 동일한 방식으로 수행 됩니다. 이 스크린샷에서 Xamarin.iOS 프로젝트를 공유 프로젝트 참조를 보여 줍니다.

![](shared-projects-images/vs-reference.png "공유 프로젝트에 대 한 프로젝트 참조")

다른 라이브러리 또는 응용 프로그램에서 공유 프로젝트 참조 되 면 솔루션을 빌드할 수 있으며 코드에서 오류를 봅니다. 공유 프로젝트에서 참조 하는 경우 _둘 이상의_ 다른 프로젝트를 현재 코드 파일을 참조 하는 프로젝트를 소스 코드 편집기의 왼쪽 위에 나타나는 메뉴.


### <a name="shared-project-properties"></a>공유 프로젝트 속성


선택 하면 공유 프로젝트에 있는 설정은 더 적습니다 다른 프로젝트 유형에 비해 속성 패널에서. 공유 프로젝트 (자체적으로) 컴파일되므로 출력 또는 컴파일러 옵션, 프로젝트 구성, 어셈블리 서명, 또는 사용자 지정 명령을 설정할 수 없습니다. 공유 프로젝트의 코드는 효과적으로 참조 하는 모든 항목에서 이러한 값을 상속 합니다.

합니다 **속성** -아래 패널에 표시 됩니다는 **루트 Namespace** 변경할 수 있는 유일한 설정입니다.

![](shared-projects-images/vs-sharedprojectproperties.png "공유 프로젝트 속성")

-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>공유 프로젝트 예제

합니다 [Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) 예제를 사용 하 여 일반적인 코드를 포함 하는 공유 프로젝트에서 사용 하는 iOS, Android 및 Windows Phone 응용 프로그램입니다. 모두를 `SQLite.cs` 고 `TaskRepository.cs` 소스 코드 파일에는 컴파일러 지시문 (예: 매월 `#if __ANDROID__`)를 각 참조 하는 응용 프로그램에 대 한 다른 출력을 생성 합니다.

전체 솔루션 구조는 다음과 같습니다 (Visual Studio 및 Mac 용 Visual Studio에서 각각).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](shared-projects-images/xs-examplesolution.png "솔루션 Mac 용 visual Studio")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](shared-projects-images/vs-examplesolution.png "Visual Studio 솔루션")

-----

Windows Phone 프로젝트에서 탐색할 수 있습니다 Visual Studio 내에서 mac의 경우 mac 용 Visual Studio에서 컴파일을 위해 해당 프로젝트 형식을 사용할 수 없습니다 하는 경우에

실행 중인 응용 프로그램은 다음과 같습니다.

![](shared-projects-images/example.png "iOS, Android, Windows Phone 예제")

## <a name="summary"></a>요약

이 문서 공유 프로젝트의 작동 방식을 어떻게 이러한 수 생성 되어 Mac 용 Visual Studio 및 Visual Studio에 사용을 설명 하 고 작업에서 공유 프로젝트를 보여 주는 간단한 샘플 응용 프로그램을 도입 했습니다.

## <a name="related-links"></a>관련 링크

- [Tasky 샘플 앱](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [이식 가능한 클래스 라이브러리 (샘플)](~/cross-platform/app-fundamentals/pcl.md)
- [코드 공유 옵션 (샘플)](~/cross-platform/app-fundamentals/code-sharing.md)
