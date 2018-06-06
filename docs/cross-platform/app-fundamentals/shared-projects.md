---
title: 공유 된 코드를 공유 하는 프로젝트를 사용 하 여
description: 공유 프로젝트의 여러 다른 응용 프로그램 프로젝트에서 참조 되는 일반적인 코드를 작성할 수 있습니다. 코드 참조 하는 각 프로젝트의 일부로 컴파일되고 공유 코드 베이스에 플랫폼 관련 기능을 통합할 수 있도록 컴파일러 지시문을 포함할 수 있습니다.
ms.prod: xamarin
ms.assetid: 191c71fb-44a4-4e6c-af4b-7b1107dce6af
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 749daac36cce26c9d24d04b89e933aab994791b4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781805"
---
# <a name="using-shared-projects-to-share-code"></a>공유 된 코드를 공유 하는 프로젝트를 사용 하 여

_공유 프로젝트의 여러 다른 응용 프로그램 프로젝트에서 참조 되는 일반적인 코드를 작성할 수 있습니다. 코드 참조 하는 각 프로젝트의 일부로 컴파일되고 공유 코드 베이스에 플랫폼 관련 기능을 통합할 수 있도록 컴파일러 지시문을 포함할 수 있습니다._

공유 프로젝트도 (공유 자산 프로젝트)를 통해 Xamarin 응용 프로그램을 비롯 한 여러 대상 프로젝트 간에 공유 되는 코드를 작성할 수 있습니다.

공유 프로젝트를 참조 하는 프로젝트를 컴파일할 수를 플랫폼별 코드를 조건에 따라 포함할 수 있도록 컴파일러 지시문을 지원 합니다. 컴파일러 지시문을 관리 하 고 각 응용 프로그램에서 코드의 모양을 시각화할 수 있도록 IDE 지원 이기도 합니다.

이전에 파일 링크를 프로젝트 간에 코드 공유를 사용한 경우 공유 프로젝트 매우 향상 된 IDE 지원 되지만 비슷한 방식으로 작동 합니다.



## <a name="what-is-a-shared-project"></a>공유 프로젝트는 무엇입니까?

대부분의 다른 프로젝트 형식을 달리 공유 프로젝트 출력 (형태로 DLL), 참조 하는 각 프로젝트에 코드를 컴파일하 되는 대신 합니다. 아래 다이어그램에 설명 되어-공유 프로젝트의 전체 내용을 개념적 측면 "에 복사" 각 참조 하는 프로젝트 및 그 중 일부 처럼 컴파일됩니다.

 ![](shared-projects-images/sharedassetproject.png "공유 프로젝트 아키텍처")

공유 프로젝트의 코드에 사용할 수 있거나 어떤 응용 프로그램에 따라 프로젝트를 사용 하는 코드는 다이어그램에서 색이 지정 된 플랫폼 상자에는 제안 된 코드의 섹션을 사용 하지 않도록 설정 하는 컴파일러 지시문을 포함할 수 있습니다.

공유 프로젝트는 자체적으로 컴파일되지 않습니다 않습니다, 그리고 다른 프로젝트에 포함 될 수 있는 소스 코드 파일을 그룹화로 순수 하 게 존재 합니다. 다른 프로젝트에서 참조 되는 코드로 효과적으로 컴파일된 *부분* 해당 프로젝트의 합니다. 공유 프로젝트 (다른 공유 프로젝트 포함) 다른 프로젝트 형식을 참조할 수 없습니다.

참고 Android 응용 프로그램 프로젝트에서 다른 Android 응용 프로그램 프로젝트를 참조할 수 없습니다-예를 들어 Android 단위 테스트 프로젝트는 Android 응용 프로그램 프로젝트를 참조할 수 없습니다. 이이 대 한 자세한 내용은 참조 [포럼 토론](http://forums.xamarin.com/discussion/comment/98092/)합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)



## <a name="visual-studio-for-mac-walkthrough"></a>Mac용 Visual Studio 연습


이 섹션을 만들고 Mac.에 대 한 Visual Studio를 사용 하는 공유 프로젝트를 사용 하는 방법을 안내합니다 참조는를 [공유 프로젝트 예제](#Shared_Project_Example) 전체 예제는 섹션입니다.


## <a name="creating-a-shared-project"></a>공유 프로젝트 만들기


새 공유 프로젝트를 만들려면로 이동 **파일 > 새 솔루션 중...**  이름을 선택 합니다.


![](shared-projects-images/xs-newsolution.png "새 솔루션")


솔루션 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 솔루션에 새 공유 프로젝트를 추가할 수도 있습니다 **추가 > 새 프로젝트 추가...** . 새 공유 프로젝트 찾습니다-아래와 같이 확인할 참조가 없는 또는 구성 요소 노드; 공유 프로젝트에 대해 지원 되지 않습니다.


![](shared-projects-images/xs-empty.png "비어 있는 공유 프로젝트")


유용 하 게 되려면 공유 프로젝트에 대 한 하나 이상의 빌드 수 프로젝트 (예: iOS 또는 Android 응용 프로그램 또는 라이브러리 또는 PCL 프로젝트)에서 참조 하도록 해야 합니다. 공유 프로젝트에 대해서는 것, 따라서 구문 (또는 다른)를 참조 하는 경우 컴파일된 가져올지 않습니다 오류 다른 작업에 의해 참조 될 때까지 강조 표시 되지 것입니다.



공유 프로젝트에 대 한 참조를 추가 합니다. 일반 라이브러리 프로젝트에 있는 참조 동일한 방식으로 수행 됩니다. 이 스크린샷은 Xamarin.iOS 프로젝트 공유 프로젝트 참조를 보여 줍니다.


![](shared-projects-images/xs-reference.png "공유 프로젝트에 프로젝트 참조")


다른 라이브러리 또는 응용 프로그램에서 공유 프로젝트 참조 되 면 솔루션 빌드하고 코드에서 모든 오류를 볼 수 있습니다. 공유 프로젝트에서 참조 되는 경우 _2 이상의_ 다른 프로젝트에에서 메뉴가 나타납니다 소스 코드 편집기의 왼쪽 위에 표시 되는 프로젝트는이 파일을 참조할을 선택 합니다.



## <a name="shared-project-options"></a>공유 프로젝트 옵션


공유 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **옵션** 다른 보다 더 적은 설정이 프로젝트 형식 발생 합니다. 공유 프로젝트 (자체적으로) 컴파일되므로 출력 또는 컴파일러 옵션, 프로젝트 구성, 서명, 어셈블리 또는 사용자 지정 명령을 설정할 수 없습니다. 공유 프로젝트의 코드는 효과적으로 참조 하는 무엇이 든에서 이러한 값을 상속 합니다.



**옵션** 화면-프로젝트 아래에 표시 됩니다 **이름** 및 **Default Namespace** 일반적으로 변경 하는 두 가지 설정이 있습니다.


![](shared-projects-images/xs-sharedprojectoptions.png "공유 프로젝트 옵션")



# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)



## <a name="visual-studio-walkthrough"></a>Visual Studio 연습


이 섹션을 만들고 Visual Studio를 사용 하 여 공유 프로젝트를 사용 하는 방법을 안내 합니다. 참조는를 [공유 프로젝트 예제](#Shared_Project_Example) 완전 한 구현에 대 한 섹션.


### <a name="creating-a-shared-project"></a>공유 프로젝트 만들기


새 공유 프로젝트를 만들려면로 이동 **파일 > 새 솔루션 중...**  프로젝트 및 솔루션에 대 한 이름을 선택 합니다.


![](shared-projects-images/vs-newsolution.png "새 솔루션")


솔루션 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 솔루션에 새 공유 프로젝트를 추가할 수도 있습니다 **추가 > 새 프로젝트...** . 새 공유 프로젝트 항목이 (클래스 파일을 추가한 후에) 아래 표시 된 대로-참조 되지 않는 표시 또는 구성 요소 노드; 공유 프로젝트에 대해 지원 되지 않습니다.


![](shared-projects-images/vs-empty.png "비어 있는 공유 프로젝트")


유용 하 게 되려면 공유 프로젝트에 대 한 하나 이상의 빌드 수 프로젝트 (예: iOS 또는 Android 응용 프로그램 또는 라이브러리 또는 PCL 프로젝트)에서 참조 하도록 해야 합니다. 공유 프로젝트에 대해서는 것, 따라서 구문 (또는 다른)를 참조 하는 경우 컴파일된 가져올지 않습니다 오류 다른 작업에 의해 참조 될 때까지 강조 표시 되지 것입니다.



공유 프로젝트에 대 한 참조를 추가 합니다. 일반 라이브러리 프로젝트에 있는 참조 동일한 방식으로 수행 됩니다. 이 스크린샷은 Xamarin.iOS 프로젝트 공유 프로젝트 참조를 보여 줍니다.


![](shared-projects-images/vs-reference.png "공유 프로젝트에 프로젝트 참조")


다른 라이브러리 또는 응용 프로그램에서 공유 프로젝트 참조 되 면 솔루션 빌드하고 코드에서 모든 오류를 볼 수 있습니다. 공유 프로젝트에서 참조 되는 경우 _2 이상의_ 현재 코드 파일을 참조 하는 프로젝트를 소스 코드 편집기의 왼쪽 위에에서 다른 프로젝트는 메뉴가 나타납니다.


### <a name="shared-project-properties"></a>공유 프로젝트 속성


때 프로젝트를 선택 하면 공유 있습니다 별도 설정이 다른 프로젝트 형식에 비해 속성 패널에 있습니다. 공유 프로젝트 (자체적으로) 컴파일되므로 출력 또는 컴파일러 옵션, 프로젝트 구성, 서명, 어셈블리 또는 사용자 지정 명령을 설정할 수 없습니다. 공유 프로젝트의 코드는 효과적으로 참조 하는 무엇이 든에서 이러한 값을 상속 합니다.



**속성** -아래 패널에 표시 됩니다는 **루트 Namespace** 변경할 수 있는 유일한 설정 합니다.


![](shared-projects-images/vs-sharedprojectproperties.png "공유 프로젝트 속성")



-----

<a name="Shared_Project_Example"/>

## <a name="shared-project-example"></a>공유 프로젝트 예제

[Tasky](https://github.com/xamarin/mobile-samples/tree/master/Tasky) 예제 응용 프로그램 모두는 iOS, Android 및 Windows Phone 공통 코드를 포함 하도록 프로젝트 공유 사용 하 여 사용 합니다. 둘 다는 `SQLite.cs` 및 `TaskRepository.cs` 소스 코드 파일 utilise 컴파일러 지시문 (예:. `#if __ANDROID__`)를 각 참조 하는 응용 프로그램에 대 한 다른 출력을 생성 합니다.

완벽 한 솔루션 구조는 다음과 같습니다 (Mac 및 Visual Studio 용 Visual Studio에서 각각):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 ![](shared-projects-images/xs-examplesolution.png "Mac 솔루션에 대 한 visual Studio")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 ![](shared-projects-images/vs-examplesolution.png "Visual Studio 솔루션")

-----

Windows Phone 프로젝트 탐색에서 Visual Studio 내에서 Mac 용 있지만 Mac.에 대 한 Visual Studio에서 컴파일을 위해 해당 프로젝트 형식을 지원 되지 않습니다.

실행 중인 응용 프로그램은 다음과 같습니다.

 ![](shared-projects-images/example.png "iOS, Android, Windows Phone 예제")



## <a name="summary"></a>요약

이 문서는 공유 프로젝트의 작동 방식을 어떻게 있습니다 만들고 사용할 수 있는 Mac 용 Visual Studio와 Visual Studio 모두에서 사용할 설명 하 고 공유 프로젝트에 동작을 보여 주는 간단한 샘플 응용 프로그램을 도입 했습니다.

## <a name="related-links"></a>관련 링크

- [Tasky 샘플 응용 프로그램](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [이식 가능한 클래스 라이브러리 (샘플)](~/cross-platform/app-fundamentals/pcl.md)
- [코드 공유 옵션 (샘플)](~/cross-platform/app-fundamentals/code-sharing.md)
