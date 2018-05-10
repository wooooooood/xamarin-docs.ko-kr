---
title: 이식 가능한 클래스 라이브러리 소개
description: 이 문서는 PCL 이식 가능한 클래스 라이브러리 () 프로젝트를 소개 하 고 만들기 및 Mac 및 Visual Studio 용 Visual Studio의 PCL 프로젝트 사용에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: d80a4125d7447b19f001c349aff006dc4744f4a6
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="introduction-to-portable-class-libraries"></a>이식 가능한 클래스 라이브러리 소개

_이 문서는 PCL 이식 가능한 클래스 라이브러리 () 프로젝트를 소개 하 고 만들기 및 Mac 및 Visual Studio 용 Visual Studio의 PCL 프로젝트 사용에 대해 설명 합니다._


플랫폼 간 응용 프로그램 작성의 핵심 구성 요소는 다양 한 플랫폼 관련 프로젝트 간에 코드를 공유할 수 있다는 점입니다. 그러나이 복잡 다양 한 플랫폼 종종 다른 하위 집합의.NET 클래스 라이브러리 BCL (기본)를 사용 하 고 따라서 실제로 빌드됩니다 다른.NET 핵심 라이브러리 프로필을 합니다. 이 각 플랫폼 하나만 있으므로 각 플랫폼에 대해 별도 클래스 라이브러리 프로젝트를 요구 하도록 표시 되는 동일한 프로필을 대상으로 하는 클래스 라이브러리를 사용할 수 있다는 것을 의미 합니다.

다음 3 가지 주요 방법 코드 공유에이 문제를 해결 하는: **표준.NET 프로젝트**, **PCL 이식 가능한 클래스 라이브러리 () 프로젝트**, 및 **공유 자산 프로젝트**.

- **표준.NET 프로젝트** [.NET 표준](~/cross-platform/app-fundamentals/net-standard.md)합니다.
-  **PCL** 프로젝트 대상으로 알려진된 BCL 클래스/기능 집합을 지 원하는 특정 프로필. 그러나 아래쪽 면을 PCL 자신의 라이브러리로 프로필 특정 코드를 분리 하 extra 아키텍처 노력 종종 필요가 없다고입니다. 이 두 가지 방법에 자세한 내용은 참조 하십시오.는 [코드 공유 옵션을 안내](~/cross-platform/app-fundamentals/code-sharing.md) 합니다.
-  **공유 자산 프로젝트** (자세한 내용은 사용 하는 다양 한 플랫폼에 대 한 코드 경로 지정 하는 조건부 컴파일 지시문을 사용 하는 솔루션 내에서 이동 하 고 일반적으로 코드를 공유 하는 빠르고 간편한 방식으로 파일 및 제공 단일 집합 사용 정보 참조는 [프로젝트 공유 문서](~/cross-platform/app-fundamentals/shared-projects.md) 및 [Xamarin 플랫폼 간 솔루션 가이드를 설정](~/cross-platform/app-fundamentals/code-sharing.md) ).


이 페이지를 만드는 방법을 설명는 **PCL** 여러 플랫폼 특정 프로젝트에서 참조할 수 있습니다는 특정 프로필을 대상으로 하는 프로젝트입니다.


## <a name="what-is-a-portable-class-library"></a>이식 가능한 클래스 라이브러리는 무엇입니까?

응용 프로그램 프로젝트 또는 라이브러리 프로젝트를 만들 때 결과 DLL에 대해 생성 되는 특정 플랫폼에서 작업 제한 됩니다. 이렇게 하면에서 Windows 앱에 대 한 어셈블리를 작성 한 다음 다시 Xamarin.iOS 및 Xamarin.Android를 사용 합니다.

그러나 이식 가능한 클래스 라이브러리를 만들 때 코드에서 실행 되도록 하는 플랫폼의 조합을 선택할 수 있습니다. 호환성 선택에 따라 이식 가능한 클래스 라이브러리를 만들 때 라이브러리 지원 플랫폼에 설명 하는 "프로필" 식별자로 변환 됩니다.

다음 표에서.NET 플랫폼에 따라 다른 기능 중 일부를 보여 줍니다. 특정 장치/플랫폼에서 실행 되도록 보장 하는 PCL 어셈블리를 작성할 수 있습니다 선택 하면 프로젝트를 만들 때 어떤 지원이 필요 합니다.

|기능|.NET Framework|UWP 앱|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|코어|Y|Y|Y|Y|Y|
|LINQ|Y|Y|Y|Y|Y|
|IQueryable|Y|Y|Y|7.5 +|Y|
|Serialization|Y|Y|Y|Y|Y|
|데이터 주석|4.0.3 +|Y|Y||Y|

Xamarin 열 반영 Xamarin.iOS 및 Xamarin.Android Visual Studio와 함께 제공 하는 모든 프로필을 지원 하 고 사용자가 만드는 모든 라이브러리의 기능 가용성을 지원 하도록 선택 하면 다른 플랫폼만 제한 됩니다.

조합 하는 프로필은 다음과 같습니다.

-  .NET 4 또는.NET 4.5
-  Silverlight 5
-  Windows Phone 8
-  UWP 앱

자세히 알아볼 수 있습니다에 서로 다른 프로필의 기능에 대 한 [Microsoft 웹 사이트](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx) 다른 커뮤니티 구성원의를 참조 하 고 [PCL 프로필 요약](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) 포함 된 지원 되는 프레임 워크 정보 및 기타 참고 사항입니다.



만드는 코드를 공유 하는 PCL에 다양 한 파일 연결 대신을 사용 하는 비교 장단점이 있습니다.


**혜택**

1. 중앙 집중식된 코드 공유 – 작성 및 다른 라이브러리 또는 응용 프로그램에서 사용할 수 있는 단일 프로젝트에서 코드를 테스트할 수도 있습니다.
1. 리팩터링 작업이 모든 코드에 영향을 줍니다 (이식 가능한 클래스 라이브러리 및 플랫폼별 프로젝트) 솔루션에 로드 합니다.
1. PCL 프로젝트는 솔루션의 다른 프로젝트에서 쉽게 참조할 수 또는 다른 사용자가 자신의 솔루션에서 참조 하는 출력 어셈블리를 공유할 수 있습니다.


**단점**

1. 동일한 이식 가능한 클래스 라이브러리에서 여러 응용 프로그램 간에 공유 플랫폼 관련 라이브러리를 참조할 수 없습니다 (예:. Community.CsharpSqlite.WP7).
1. 이식 가능한 클래스 라이브러리 하위 집합 MonoTouch 및 모노 for Android (예: DllImport 또는 System.IO.File) 모두에서 사용할 수 있는 클래스를 포함할 수 있습니다.



어느 정도 까지는 공급자 패턴 또는 종속성 주입을 사용 하 여 이식 가능한 클래스 라이브러리에 정의 된 인터페이스 또는 기본 클래스에 대 한 플랫폼 프로젝트의 실제 구현 코드를 작성 하려면 두 단점 우회할 수 있습니다.



이 다이어그램에서는 이식 가능한 클래스 라이브러리를 사용 하 여 코드를 공유할 수 있지만 종속성 주입을 사용 하 여 플랫폼에 종속 된 기능에 전달 하는 플랫폼 간 응용 프로그램의 아키텍처를 보여 줍니다.



[![](pcl-images/image1.png "이 다이어그램도 종속성 주입을 사용 하 여 플랫폼에 종속 된 기능에 전달할 수 있지만 이식 가능한 클래스 라이브러리를 사용 하 여 코드를 공유 하는 플랫폼 간 응용 프로그램의 아키텍처를 보여 줍니다.")](pcl-images/image1.png#lightbox)



# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)



## <a name="visual-studio-for-mac-walkthrough"></a>Mac용 Visual Studio 연습


이 섹션을 만들고 Mac.에 대 한 Visual Studio를 사용 하 여 이식 가능한 클래스 라이브러리를 사용 하는 방법을 안내합니다 참조는 완전 한 구현에 대 한 PCL 예제 섹션에 있습니다.



### <a name="creating-a-pcl"></a>PCL 만들기


이식 가능한 클래스 라이브러리 솔루션에 추가 일반 라이브러리 프로젝트를 추가 하 매우 비슷합니다.


1. 새 프로젝트 대화 상자에서 선택 된 `Multiplatform > Library > Portable Library` 옵션


    ![](pcl-images/image2.png "다중 플랫폼 > 라이브러리 > 이식 가능한 라이브러리")


1. Visual Studio에서 Mac에 대 한 PCL을 만들 때 자동으로 Xamarin.iOS 및 Xamarin.Android에 사용 중인 프로필로 구성 됩니다. PCL 프로젝트에는이 스크린 샷에 표시 된 것 처럼 표시 됩니다.

    ![](pcl-images/image3.png "이 스크린 샷에 표시 된 것 처럼 PCL 프로젝트가 나타납니다.")

PCL 코드를 추가할 준비가 되었습니다. (응용 프로그램 프로젝트, 라이브러리 프로젝트 및 다른 PCL 프로젝트) 다른 프로젝트에서 참조할 수도 있습니다.



### <a name="editing-pcl-settings"></a>PCL 설정 편집


를 확인 하 고이 프로젝트에 대 한 PCL 설정을 변경 합니다. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **옵션 > 빌드 > 일반** 여기에 표시 된 화면 표시:



[![](pcl-images/image4.png "를 확인 하 고이 프로젝트에 대 한 PCL 설정을 변경 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 여기에 표시 된 화면 표시 옵션 빌드 일반을 선택합니다")](pcl-images/image4.png#lightbox)



이 화면의 설정은이 특정 PCL와 호환 되는 플랫폼을 제어 합니다. 그러면 수 있는 기능을 제어 하는이 PCL에서 사용 하 고 프로필을 변경 하 이러한 옵션 중 하나를 변경 이식 가능한 코드에 사용 합니다.



변경 된 `Target Framework` 옵션을 자동으로 업데이트는 `Current Profile`; 화면에서 호환 되지 않는 옵션을 선택한 경우에 경고를 표시 합니다.



[![](pcl-images/image5.png "현재 프로필 업데이트 대상 프레임 워크 옵션 중 하나를 자동으로 변경 화면에서 호환 되지 않는 옵션을 선택한 경우에 경고를 표시")](pcl-images/image5.png#lightbox)



프로필에는 코드는 PCL에 이미 추가 된 후 변경 되 면 있기 코드 새로 선택한 프로필에 포함 되지 않는 기능을 참조 하는 경우 라이브러리가 더 이상 컴파일되지 것입니다.


## <a name="working-with-a-pcl"></a>PCL 작업


PCL 라이브러리를 코드를 작성 하는 경우 Mac 편집기에 대 한 Visual Studio는 선택한 프로필의 한계를 인식 하 고 자동 완성 옵션을 적절 하 게 조정 됩니다. 예를 들어 왼쪽이 스크린샷에서 자동 완성 옵션을 보여 줍니다 System.IO Mac 용 Visual Studio에서 사용 하는 기본 프로필 (Profile136)를 사용 하 여 워드 스크롤 막대가 표시 되는 사용 가능한 클래스의 절반 정도 나타냅니다 (실제로 14 사용할 수 있는 클래스).



[![](pcl-images/image6.png "Profile136 실제로 발생 표시 되는 사용 가능한 클래스의 절반 정도 나타내는 스크롤 막대는 14 클래스를 사용할 수만 Mac 공지에 대 한 Visual Studio에서 사용 되는 기본 프로필을 사용 하는 IO")](pcl-images/image6.png#lightbox)



Xamarin.iOS 또는 Xamarin.Android 프로젝트 – 자동 완성 System.IO와 같은 클래스에 사용 되는 일반적으로 사용할 수 있는 등 40 클래스는 비교 `File` 및 `Directory` PCL 프로필에 없는 합니다.



[![](pcl-images/image7.png "사용 되는 모든 PCL 프로필에 없는 파일 및 디렉터리와 같은 클래스를 사용할 수 있는 포함 하 여 일반적으로 한 40 클래스")](pcl-images/image7.png#lightbox)



이 반영 PCL을 사용 하 여 원본으로 사용 되는 대신-여러 플랫폼 간에 코드를 원활 하 게 공유 하는 기능 비교 가능한 구현 가능한 모든 플랫폼에서 없기 때문에 특정 Api를 사용할 수 없는 것을 의미 합니다.



### <a name="using-pcl"></a>PCL을 사용 하 여


PCL 프로젝트를 만든 후 일반적으로 참조를 추가 같은 방식으로 호환 되는 모든 응용 프로그램 또는 라이브러리 프로젝트에 대 한 참조를 추가할 수 있습니다. Mac 용 Visual Studio에서 참조 노드를 마우스 오른쪽 단추로 클릭 하 고 선택 `Edit References…` 표시 된 것 처럼 프로젝트 탭으로 전환 합니다.



[![](pcl-images/image8.png "Mac 용 Visual Studio에서 참조 노드를 마우스 오른쪽 단추로 클릭 하 고 선택한 참조 편집 다음 표시 된 것 처럼 프로젝트 탭으로 전환")](pcl-images/image8.png#lightbox)



다음 스크린 샷에서 Xamarin.iOS 프로젝트의 맨 아래에 있고 해당 PCL 라이브러리에 대 한 참조를 PCL 라이브러리를 보여 주는 TaskyPortable 샘플 앱에 대 한 솔루션 패드를 보여 줍니다.



[![](pcl-images/image9.png "TaskyPortable 샘플 응용 프로그램에 대 한 솔루션 채움")](pcl-images/image9.png#lightbox)



출력을 PCL (ie. 결과 어셈블리 DLL) 대부분의 프로젝트에 대 한 참조로 추가할 수도 있습니다. 이렇게 하면 PCL 플랫폼 구성 요소 및 라이브러리를 제공 하기 위한 이상적인 방법입니다.




# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)




## <a name="visual-studio-walkthrough"></a>Visual Studio 연습


이 섹션을 만들고 Visual Studio를 사용 하 여 이식 가능한 클래스 라이브러리를 사용 하는 방법을 안내 합니다. 참조는 완전 한 구현에 대 한 PCL 예제 섹션에 있습니다.



### <a name="creating-a-pcl"></a>PCL 만들기


한 PCL을 Visual Studio에서 솔루션에 추가 하는 것은 일반 프로젝트를 추가 하는 약간 다릅니다.



1. 새 프로젝트 추가 화면에서 선택 된 `Portable Class Library` 옵션


    ![](pcl-images/image10.png "이식 가능한 클래스 라이브러리")


1. Visual Studio 프로필을 구성할 수 있도록 다음과 같은 대화 상자가 표시 되면서 라는 즉시 됩니다.
 지원 하 고 확인을 누르고 필요한 플랫폼을 눈금.


    ![](pcl-images/image11.png "눈금을 지원 하 고 확인을 누르고 해야 플랫폼")


1. PCL 프로젝트는 솔루션 탐색기에 표시 된 대로 표시 됩니다. 참조 노드는 라이브러리 (PCL 프로필에서 정의).NET Framework의 하위 집합을 사용 하는 것을 나타냅니다.

    ![](pcl-images/image12.png "NET Framework PCL 프로필에 정의 된")

PCL 코드를 추가할 준비가 되었습니다. (응용 프로그램 프로젝트, 라이브러리 프로젝트 및 다른 PCL 프로젝트) 다른 프로젝트에서 참조할 수도 있습니다.



### <a name="editing-pcl-settings"></a>PCL 설정 편집


PCL 설정을 확인 및 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 변경할 수 **속성 > 라이브러리** 이 스크린 샷에 표시 된 것 처럼:



[![](pcl-images/image13.png "PCL 설정은 확인 및이 스크린 샷에 표시 된 것 처럼 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 속성 라이브러리를 선택 하 여 변경할 수")](pcl-images/image13.png#lightbox)



프로필에는 코드는 PCL에 이미 추가 된 후 변경 되 면 있기 코드 새로 선택한 프로필에 포함 되지 않는 기능을 참조 하는 경우 라이브러리가 더 이상 컴파일되지 것입니다.



### <a name="working-with-a-pcl"></a>PCL 작업


PCL 라이브러리를 코드를 작성 하는 경우 Visual Studio는 선택한 프로필의 한계를 인식 하 고 Intellisense 옵션을 적절 하 게 조정 됩니다. 예를 들어이 스크린샷은 System.IO에 대 한 자동 완성 옵션의 기본 프로필 (Profile136)를 사용 하 여 워드 스크롤 막대가 표시 되는 사용 가능한 클래스의 절반 정도 나타냅니다 (실제로만 14 클래스 사용 가능한).



[![](pcl-images/image14.png "Profile136 기본 프로필을 사용 하는 IO")](pcl-images/image14.png#lightbox)



– 일반 프로젝트에서 자동 완성 System.IO와 같은 클래스에 사용 되는 일반적으로 사용할 수 있는 등 40 클래스는 비교 `File` 및 `Directory` PCL 프로필에 없는 합니다.



[![](pcl-images/image15.png "일반 프로젝트에서 자동 완성")](pcl-images/image15.png#lightbox)



이 반영 PCL을 사용 하 여 원본으로 사용 되는 대신-여러 플랫폼 간에 코드를 원활 하 게 공유 하는 기능 비교 가능한 구현 가능한 모든 플랫폼에서 없기 때문에 특정 Api를 사용할 수 없는 것을 의미 합니다.



### <a name="using-pcl"></a>PCL을 사용 하 여


PCL 프로젝트를 만든 후 일반적으로 참조를 추가 같은 방식으로 호환 되는 모든 응용 프로그램 또는 라이브러리 프로젝트에 대 한 참조를 추가할 수 있습니다. Visual Studio에서 참조 노드를 마우스 오른쪽 단추로 클릭 하 고 선택 `Add Reference...` 다음 전환 하는 **솔루션: 프로젝트** 표시 된 것 처럼 탭:



[![](pcl-images/image16.png "표시 된 것 처럼 프로젝트 탭")](pcl-images/image16.png#lightbox)



다음 스크린 샷에서 Xamarin.iOS 프로젝트의 맨 아래에 있고 해당 PCL 라이브러리에 대 한 참조를 PCL 라이브러리를 보여 주는 TaskyPortable 샘플 앱에 대 한 솔루션 창을 보여 줍니다.



[![](pcl-images/image17.png "TaskyPortable 샘플 응용 프로그램에 대 한 솔루션 창")](pcl-images/image17.png#lightbox)



출력을 PCL (ie. 결과 어셈블리 DLL) 대부분의 프로젝트에 대 한 참조로 추가할 수도 있습니다.
이렇게 하면 PCL 플랫폼 구성 요소 및 라이브러리를 제공 하기 위한 이상적인 방법입니다.




-----



## <a name="pcl-example"></a>PCL 예제


[TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/) 샘플 응용 프로그램 Xamarin을 사용한 이식 가능한 클래스 라이브러리를 사용할 수 있는 방법을 보여 줍니다.
다음은 iOS, Android 및 Windows Phone 실행 되는 결과 앱의 일부 스크린 샷은입니다.



[![](pcl-images/image18.png "다음은 iOS, Android 및 Windows Phone 실행 되는 결과 앱의 일부 스크린 샷은")](pcl-images/image18.png#lightbox)



다양 한 데이터와 논리 클래스 순수 하 게 이식 가능한 코드를 공유 하 고 SQLite 데이터베이스 구현에 대 한 종속성 주입을 사용 하 여 플랫폼 특정 요구를 통합 하는 방법을 보여 줍니다.




솔루션 구조는 다음과 같습니다 (Mac 및 Visual Studio 용 Visual Studio에서 각각):



[![](pcl-images/image19.png "솔루션 구조가 같습니다. Visual Studio에서 Mac 및 Visual Studio 용 각각")](pcl-images/image19.png#lightbox)



SQLite NET 코드 이식 가능한 클래스 라이브러리로 컴파일될 수 데모 목적으로 추상 개로 리팩터링 되었습니다는 클래스에 대 한 플랫폼 관련 부분이 (각 서로 다른 운영 체제에서 SQLite 구현과 함께 작동할)에 있으므로 및 iOS 및 Android 프로젝트에 서브 클래스로 구현 되는 실제 코드입니다.



### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

이식 가능한 클래스 라이브러리에서 지원할 수 있는.NET 기능 제한 됩니다. 사용할 수 없는 여러 플랫폼에서 실행 되도록 컴파일할 때문에 활용 `[DllImport]` SQLite NET에 사용 되는 기능입니다. 대신 SQLite NET 클래스는 추상 클래스 구현 되며 공유 코드의 나머지 부분을 다음 참조 됩니다. 추상 API의 추출은 다음과 같습니다.


```csharp
public abstract class SQLiteConnection : IDisposable {

    public string DatabasePath { get; private set; }
    public bool TimeExecution { get; set; }
    public bool Trace { get; set; }
    public SQLiteConnection(string databasePath) {
         DatabasePath = databasePath;
    }
    public abstract int CreateTable<T>();
    public abstract SQLiteCommand CreateCommand(string cmdText, params object[] ps);
    public abstract int Execute(string query, params object[] args);
    public abstract List<T> Query<T>(string query, params object[] args) where T : new();
    public abstract TableQuery<T> Table<T>() where T : new();
    public abstract T Get<T>(object pk) where T : new();
    public bool IsInTransaction { get; protected set; }
    public abstract void BeginTransaction();
    public abstract void Rollback();
    public abstract void Commit();
    public abstract void RunInTransaction(Action action);
    public abstract int Insert(object obj);
    public abstract int Update(object obj);
    public abstract int Delete<T>(T obj);

    public void Dispose()
    {
        Close();
    }
    public abstract void Close();

}
```


공유 코드의 나머지 부분에서는 추상 클래스를 사용 하 여 개체를 데이터베이스에서 "검색" 및 "store"입니다. 이 추상 클래스를 사용 하는 모든 응용 프로그램에서 실제 데이터베이스 기능을 제공 하는 완전 한 구현에 전달 해야 합니다.



### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid 및 TaskyiOS


IOS 및 Android 응용 프로그램 프로젝트 사용자 인터페이스 및 유선 전화 접속 여 PCL에서 공유 코드에 사용 되는 다른 플랫폼별 코드를 포함 합니다.



이러한 프로젝트는 또한 추상 데이터베이스 해당 플랫폼에서 사용할 수 있는 API의 구현을 포함 합니다. IOS 및 Android는 Sqlite 데이터베이스 엔진은 운영 체제에 기본 제공 구현을 사용할 수 있도록 `[DllImport]` 데이터베이스 연결의 구체적인 구현을 제공 하기 위해 표시 된 것 처럼 합니다. 플랫폼별 구현 코드의 일부는 다음과 같습니다.


```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```


전체 구현은 샘플 코드에서 볼 수 있습니다.

### <a name="taskywinphone"></a>TaskyWinPhone


Windows Phone 응용 프로그램이 XAML을 사용 하 여 빌드한 UI 며 사용자 인터페이스를 사용 하 여 공유 개체를 연결할 다른 플랫폼별 코드를 포함 합니다.



IOS 및 Android에 사용 되는 구현 달리 Windows Phone 앱을 만들고 해야의 인스턴스를 사용 하 여는 `Community.Sqlite.dll` 추상의 일부로 데이터베이스 API입니다. 사용 하는 대신 `DllImport`, 열기와 같은 메서드 구현에서 참조 되는 Community.Sqlite 어셈블리에 대해는 `TaskWinPhone` 프로젝트. 일부는 iOS 및 Android 버전 위의 비교에 대해 여기에서 표시 됩니다.


```csharp
public static Result Open(string filename, out Sqlite3.sqlite3 db)
{
    db = new Sqlite3.sqlite3();
    return (Result)Sqlite3.sqlite3_open(filename, ref db);
}

public static Result Close(Sqlite3.sqlite3 db)
{
    return (Result)Sqlite3.sqlite3_close(db);
}
```


## <a name="summary"></a>요약


이 문서에 간단히 이점과 문제점 이식 가능한 클래스 라이브러리의 설명, 만들고 PCLs에서 사용 하는 방법을 살펴보았습니다 Mac 및 Visual Studio;에 대 한 Visual Studio 내 마지막으로 실행에서 한 PCL을 보여 주는 전체 샘플 응용 프로그램 – TaskyPortable –를 소개 합니다.


## <a name="related-links"></a>관련 링크

- [TaskyPortable (샘플)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [플랫폼 간 응용 프로그램 빌드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [휴대용 Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)
- [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md)
- [.NET Framework (Microsoft)로 플랫폼 간 개발](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
