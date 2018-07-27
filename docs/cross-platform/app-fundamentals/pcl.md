---
title: 이식 가능한 클래스 라이브러리 (PCL) 소개
description: 이 문서에서는 이식 가능한 클래스 라이브러리 (PCL) 프로젝트를 소개 하 고 만들고 Mac 및 Visual Studio 용 Visual Studio에서 PCL 프로젝트를 사용을 안내 합니다.
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 83b1da5cd10a46b8480b0755eeb16bf7434a5906
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270091"
---
# <a name="portable-class-libraries-pcl"></a>이식 가능한 클래스 라이브러리 (PCL)

> [!WARNING]
> 이식 가능한 클래스 라이브러리 (Pcl)는 최신 버전의 Visual Studio에서 사용 되지 않는 것으로 간주 됩니다.
> 여전히, 편집을 열고 컴파일 Pcl을 하는 동안 새 프로젝트에 대 한 것이 좋습니다는 데 [.NET Standard 라이브러리](~/cross-platform/app-fundamentals/net-standard.md)합니다.

플랫폼 간 응용 프로그램을 구축 하는 핵심 구성 요소는 다양 한 플랫폼 특정 프로젝트에서 코드를 공유할 수 있다는 것입니다. 그러나 다양 한 플랫폼의.NET 클래스 라이브러리 BCL (기본), 다른 하위 집합을 자주 사용 하 고 따라서 실제로 기본 제공 되는 다른.NET Core 라이브러리 프로필 한다는 사실에서 작업이 복잡해 집니다. 즉, 각 플랫폼을 대상으로 하는 동일한 프로필 나타나도록는 각 플랫폼에 대해 별도 클래스 라이브러리 프로젝트를 요구 하는 클래스 라이브러리 사용할 수 있습니다.

가지이 문제를 해결 하는 코드를 공유 하는 데 세 가지 주요 방법이 있습니다: **.NET Standard 프로젝트**를 **공유 자산 프로젝트**, 및 **이식 가능한 클래스 라이브러리 (PCL) 프로젝트**.

- **.NET standard 프로젝트** 는.NET 코드를 자세히 공유에 대 한 기본 접근 방법 [.NET Standard 프로젝트 및 Xamarin](~/cross-platform/app-fundamentals/net-standard.md)합니다.
- **공유 자산 프로젝트** (에 대 한 자세한 내용은 사용 하는 다양 한 플랫폼에 대 한 코드 경로 지정 하는 조건부 컴파일 지시문을 사용 하는 솔루션 내에서 이동 하 고 일반적으로 코드를 공유 하는 빠르고 간편한 방식으로 파일 및 제품의 단일 집합 사용 내용은 합니다 [공유 프로젝트 문서](~/cross-platform/app-fundamentals/shared-projects.md)).
- **PCL** 대상은 특정 프로필을 알려진된 BCL 클래스/기능 집합을 지원 합니다. 그러나 아래쪽 면 PCL 자신의 라이브러리에 프로필 특정 코드를 분리 하는 추가 아키텍처 노력 종종 필요 함을입니다.

이 페이지를 만드는 방법을 설명 된 **PCL** 여러 플랫폼별 프로젝트에서 참조할 수 있습니다 특정 프로필을 대상으로 하는 프로젝트입니다.

## <a name="what-is-a-portable-class-library"></a>이식 가능한 클래스 라이브러리 란?

응용 프로그램 프로젝트 또는 라이브러리 프로젝트를 만들면 결과 DLL에 대해 생성 되는 특정 플랫폼에서 작업 제한 됩니다. 이렇게 하면에서 Windows 앱의 경우 어셈블리를 작성 하 고 Xamarin.iOS 및 Xamarin.Android에서 다시 사용 합니다.

그러나 이식 가능한 클래스 라이브러리를 만들 때 실행 하도록 코드를 원하는 플랫폼의 조합을 선택할 수 있습니다. 이식 가능한 클래스 라이브러리를 만들 때 호환성 선택한 라이브러리를 지 원하는 플랫폼을 설명 하는 "프로필" 식별자로 변환 됩니다.

아래 테이블에서는.NET 플랫폼에 따라 달라 지는 기능 중 일부를 보여 줍니다. 특정 장치/플랫폼에서 실행 되도록 보장 되는 PCL 어셈블리를 작성할 수 선택 하기만 하면 프로젝트를 만들 때 어떤 지원이 필요 합니다.

|기능|.NET Framework|UWP 앱|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|코어|Y|Y|Y|Y|Y|
|LINQ|Y|Y|Y|Y|Y|
|IQueryable|Y|Y|Y|7.5 +|Y|
|Serialization|Y|Y|Y|Y|Y|
|데이터 주석|4.0.3 +|Y|Y||Y|

Xamarin 열에는 Xamarin.iOS 및 Xamarin.Android 지원 Visual Studio와 함께 제공 되는 모든 프로필을 만드는 모든 라이브러리의 기능 가용성을 지원 하려는 다른 플랫폼만 제한 됩니다 사실을 반영 합니다.

프로필의 조합에 포함 됩니다.

- .NET 4 또는.NET 4.5
- Silverlight 5
- Windows Phone 8
- UWP 앱

알아볼 수 있습니다에 다양 한 프로필 기능에 대 한 [Microsoft 웹 사이트](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx) 다른 커뮤니티 멤버의 내용과 [PCL 프로필 요약](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) 지원 되는 프레임 워크 정보 및 기타 정보를 포함 하는 합니다.

**혜택**

1. 중앙 집중식된 코드 공유-작성 및 다른 라이브러리 또는 응용 프로그램에서 사용할 수 있는 단일 프로젝트에서 코드를 테스트 합니다.
2. 모든 코드는 영향이 리팩터링 작업 (이식 가능한 클래스 라이브러리 및 플랫폼 특정 프로젝트) 솔루션에 로드 합니다.
3. 솔루션에서 다른 프로젝트에서 PCL 프로젝트를 쉽게 참조할 수 있습니다 또는 다른 사용자가 솔루션에서 참조를 위해 출력 어셈블리를 공유할 수 있습니다.

**단점**

1. 동일한 이식 가능한 클래스 라이브러리에서 여러 응용 프로그램 간에 공유 되므로 플랫폼별 라이브러리를 참조할 수 없습니다 (예: Community.CsharpSqlite.WP7).
2. 이식 가능한 클래스 라이브러리 하위 집합 MonoTouch 및 (예: DllImport 또는 System.IO.File) Android 용 Mono에서 사용할 수 있는 클래스 포함 되지 않을 수 있습니다.

> [!NOTE]
> Visual Studio의 최신 버전의 되지 이식 가능한 클래스 라이브러리 및 [.NET 표준 라이브러리](net-standard.md) 대신 것이 좋습니다.

어느 정도 까지는 공급자 패턴 또는 종속성 주입을 사용 하 여 이식 가능한 클래스 라이브러리에 정의 된 인터페이스 또는 기본 클래스에 대해 플랫폼 프로젝트의 실제 구현 코드를 모두 단점을 우회할 수도 있습니다.

이 다이어그램에서는 이식 가능한 클래스 라이브러리를 사용 하 여 코드를 공유할 수 있지만 또한 종속성 주입을 사용 하 여 플랫폼에 종속 된 기능에 전달 하는 플랫폼 간 응용 프로그램의 아키텍처를 보여 줍니다.

[![](pcl-images/image1.png "이 다이어그램 이식 가능한 클래스 라이브러리를 사용 하 여 코드를 공유할 수 있지만 또한 종속성 주입을 사용 하 여 플랫폼에 종속 된 기능에 전달 하는 플랫폼 간 응용 프로그램의 아키텍처를 보여 줍니다.")](pcl-images/image1.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>연습에서는 Mac 용 visual Studio

이 섹션을 만들고 mac 용 Visual Studio를 사용 하 여 이식 가능한 클래스 라이브러리를 사용 하는 방법을 안내합니다 참조를 완전 한 구현에 대 한 PCL 예제 섹션에 있습니다.

### <a name="creating-a-pcl"></a>PCL 만들기

이식 가능한 클래스 라이브러리 솔루션에 추가 일반 라이브러리 프로젝트를 추가 하 매우 비슷합니다.

1. 에 **새 프로젝트** 대화 상자를 선택 합니다 **다중 플랫폼 > 라이브러리 > 이식 가능한 라이브러리** 옵션:

    ![새 PCL 프로젝트 만들기](pcl-images/image2.png)

1. Mac 용 Visual Studio에서 PCL을 만들면 Xamarin.iOS 및 Xamarin.Android에 대해 작동 하는 프로필을 사용 하 여 자동으로 구성 됩니다. PCL 프로젝트에는이 스크린샷에 표시 된 것 처럼 표시 됩니다.

    ![Solution pad에서 PCL 프로젝트](pcl-images/image3.png)

PCL 코드를 추가할 준비가 되었습니다. 또한 다른 프로젝트 (응용 프로그램 프로젝트에서 라이브러리 프로젝트 및 다른 PCL 프로젝트)에서 참조 될 수도 있습니다.

### <a name="editing-pcl-settings"></a>PCL 설정 편집

살펴보고이 프로젝트에 대 한 PCL 설정을 변경 하려면 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **옵션 > 빌드 > 일반** 여기에 표시 된 화면을 보려면:

[![프로필을 설정 하는 PCL 프로젝트 옵션](pcl-images/image4-sml.png)](pcl-images/image4.png#lightbox)

클릭 **변경 하는 중...**  이 이식 가능한 클래스 라이브러리에 대 한 대상 프로필을 변경 합니다.

프로필에는 코드를 PCL에 이미 추가 된 후 변경 되 면 경우 코드는 새로 선택한 프로필에 포함 되지 않는 기능을 참조 하는 경우 라이브러리를 더 이상 컴파일되지 것입니다.

## <a name="working-with-a-pcl"></a>PCL을 사용 하 여 작업

PCL 라이브러리의 코드를 작성 하는 경우 편집기 Mac 용 Visual Studio는 선택한 프로필의 한계를 인식 하 고 자동 완성 옵션을 적절 하 게 조정 됩니다. 예를 들어이 스크린샷에서 자동 완성 옵션을 보여 줍니다 System.IO Mac 용 Visual Studio에서 사용 하는 기본 프로필 (Profile136)를 사용 하 여 – 표시 되는 사용 가능한 클래스의 절반 정도 나타내는 스크롤 막대를 확인할 수 있습니다 (실제로 14 사용할 수 있는 클래스).

[![PCL의 System.IO 클래스에 14 클래스의 Intellisense 목록이](pcl-images/image6.png)](pcl-images/image6.png#lightbox)

Xamarin.iOS 또는 Xamarin.Android 프로젝트-자동 완성 System.IO를 사용 하 여 사용할 수 있는 등 일반적으로 사용 하는 클래스와 같은 40 클래스는 비교 `File` 및 `Directory` PCL 프로필에 없는 합니다.

[![.NET Framework System.IO 네임 스페이스의 40 클래스의 Intellisense 목록이](pcl-images/image7.png)](pcl-images/image7.png#lightbox)

PCL을 사용 하 여 기본 절충 반영 – 여러 플랫폼 간에 코드를 원활 하 게 공유 하는 기능이 모든 가능한 플랫폼 구현 비교할 수 없기 때문에 특정 Api를 사용할 수 없는 것을 의미 합니다.

### <a name="using-pcl"></a>PCL을 사용 하 여

PCL 프로젝트를 만든 후에 일반적으로 참조를 추가 하면 동일한 방식 호환 되는 모든 응용 프로그램 또는 라이브러리 프로젝트에 대 한 참조를 추가할 수 있습니다. Mac 용 Visual Studio에서 참조 노드를 마우스 오른쪽 단추로 클릭 하 고 선택 **참조 편집...**  다음으로 전환 합니다 **프로젝트** 표시 된 것 처럼 탭:

[![참조 편집 옵션을 통해 PCL에 대 한 참조를 추가 합니다.](pcl-images/image8.png)](pcl-images/image8.png#lightbox)

다음 스크린샷에서 Xamarin.iOS 프로젝트에서 PCL 라이브러리 맨 아래에 있고 해당 PCL 라이브러리에 대 한 참조를 보여 주는 TaskyPortable 샘플 앱에 대 한 Solution pad를 보여줍니다.

[![TaskyPortable 샘플 솔루션 보여 주는 PCL 프로젝트](pcl-images/image9.png)](pcl-images/image9.png#lightbox)

PCL에서 출력 (ie. 결과 어셈블리 DLL) 대부분의 프로젝트에 대 한 참조로 추가할 수도 있습니다. 이렇게 하면 PCL 플랫폼 구성 요소 및 라이브러리를 제공 하는 이상적인 방법입니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-walkthrough"></a>Visual Studio 연습

이 섹션을 만들고 Visual Studio를 사용 하 여 이식 가능한 클래스 라이브러리를 사용 하는 방법을 안내 합니다. 참조를 완전 한 구현에 대 한 PCL 예제 섹션에 있습니다.

### <a name="creating-a-pcl"></a>PCL 만들기

Visual Studio에서 솔루션에 PCL을 추가 하는 것은 일반 프로젝트를 추가 하는 약간 다릅니다.

1. 에 **새 프로젝트 추가** 화면에서 선택 합니다 **클래스 라이브러리 (레거시 이식 가능)** 옵션. 오른쪽에 대 한 설명을 참고가 프로젝트 형식에 사용 되지 않는다는 것을 권고 합니다.

    [![이식 가능한 클래스 라이브러리를 만드는 새 프로젝트 창](pcl-images/image10-sml.png "이식 가능한 클래스 라이브러리")](pcl-images/image10.png#lightbox)

2. Visual Studio 프로필을 구성할 수 있도록 즉시 다음 대화 상자를 사용 하 여 묻습니다.
 확인을 누릅니다 지원 해야 하는 플랫폼을 선택 합니다.

    [![라이브러리에 대 한 대상 플랫폼을 선택할](pcl-images/image11-sml.png "확인을 누릅니다 지원 해야 하는 플랫폼 선택")](pcl-images/image11.png#lightbox)

3. PCL 프로젝트는 솔루션 탐색기에 표시 된 대로 나타납니다 &ndash; 텍스트 **(이식 가능)** PCL 것을 나타내기 위해 프로젝트 이름 옆에 나타납니다.

    ![PCL 프로필에서 정의 된.NET Framework](pcl-images/image12.png "PCL 프로필에서 정의 된.NET Framework")

PCL 코드를 추가할 준비가 되었습니다. 또한 다른 프로젝트 (응용 프로그램 프로젝트, 라이브러리 프로젝트 및 다른 PCL 프로젝트)에서 참조 될 수도 있습니다.

### <a name="editing-pcl-settings"></a>PCL 설정 편집

PCL 설정을 확인 하 고 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 변경할 수 있습니다 **속성 > 라이브러리** 이 스크린샷에 표시 된 것 처럼:

[![플랫폼 대상 편집](pcl-images/image13-sml.png)](pcl-images/image13.png#lightbox)

프로필에는 코드를 PCL에 이미 추가 된 후 변경 되 면 경우 코드는 새로 선택한 프로필에 포함 되지 않는 기능을 참조 하는 경우 라이브러리를 더 이상 컴파일되지 것입니다.

> [!TIP]
> 또한 하다 고 알리는 메시지 **합니다. NETStandard는 코드를 공유 하는 좋은 방법은**합니다. 이것은 Pcl 하는 동안 계속 지원 되는 표시,.NET Standard로 업그레이드 하는 것이 좋습니다.

### <a name="working-with-a-pcl"></a>PCL을 사용 하 여 작업

PCL 라이브러리의 코드를 작성 하는 경우 Visual Studio는 선택한 프로필의 한계를 인식 하 고 Intellisense 옵션을 적절 하 게 조정 됩니다. 예를 들어이 스크린샷은 System.IO에 대 한 자동 완성 옵션의 기본 프로필 (Profile136)를 사용 하 여 – 표시 되는 사용 가능한 클래스의 절반 정도 나타내는 스크롤 막대를 확인할 수 있습니다 (실제로만 14 클래스 사용할 수 있습니다).

[![PCL에 사용할 수 있는 IO 클래스의 수가 감소](pcl-images/image14.png)](pcl-images/image14.png#lightbox)

일반 프로젝트-자동 완성 System.IO를 사용 하 여 사용할 수 있는 등 일반적으로 사용 하는 클래스와 같은 40 클래스는 비교 `File` 고 `Directory` PCL 프로필에 없는 합니다.

[![자세한 IO의 많은 클래스는.NET Framework에서 사용할 수](pcl-images/image15.png)](pcl-images/image15.png#lightbox)

PCL을 사용 하 여 기본 절충 반영 – 여러 플랫폼 간에 코드를 원활 하 게 공유 하는 기능이 모든 가능한 플랫폼 구현 비교할 수 없기 때문에 특정 Api를 사용할 수 없는 것을 의미 합니다.

> [!TIP]
> .NET standard 2.0에는 훨씬 더 큰 API 노출 영역을 Pcl에 System.IO 네임 스페이스를 포함 하 여 보다 나타냅니다. 새 프로젝트에 대 한.NET Standard은 PCL 보다 권장 됩니다.

### <a name="using-pcl"></a>PCL을 사용 하 여

PCL 프로젝트를 만든 후에 일반적으로 참조를 추가 하면 동일한 방식 호환 되는 모든 응용 프로그램 또는 라이브러리 프로젝트에 대 한 참조를 추가할 수 있습니다. Visual Studio에서 참조 노드를 마우스 오른쪽 단추로 클릭 하 고 선택 `Add Reference...` 다음으로 전환 합니다 **솔루션 > 프로젝트** 표시 된 것 처럼 탭:

[![참조 프로젝트 추가 탭을 통해 PCL에 대 한 참조를 추가 합니다.](pcl-images/image16.png)](pcl-images/image16.png#lightbox)

다음 스크린 샷에서 Xamarin.iOS 프로젝트에서 PCL 라이브러리 맨 아래에 있고 해당 PCL 라이브러리에 대 한 참조를 보여 주는 TaskyPortable 샘플 앱에 대 한 솔루션 창을 보여 줍니다.

[![PCL 라이브러리를 보여 주는 TaskyPortable 샘플 솔루션](pcl-images/image17.png)](pcl-images/image17.png#lightbox)

PCL에서 출력 (ie. 결과 어셈블리 DLL) 대부분의 프로젝트에 대 한 참조로 추가할 수도 있습니다.
이렇게 하면 PCL 플랫폼 구성 요소 및 라이브러리를 제공 하는 이상적인 방법입니다.

-----

## <a name="pcl-example"></a>PCL 예제

합니다 [TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/) 샘플 응용 프로그램 Xamarin을 사용 하 여 이식 가능한 클래스 라이브러리를 사용할 수 있는 방법을 보여 줍니다.
IOS 및 Android에서 실행 되는 결과 앱의 일부 스크린 샷은 다음과 같습니다.

[![](pcl-images/image18.png "IOS, Android 및 Windows Phone 실행 되는 결과 앱의 일부 스크린 샷은 다음과 같습니다.")](pcl-images/image18.png#lightbox)

다양 한 데이터 및 논리 클래스 순수 하 게 이식 가능한 코드를 공유 하 고 종속성 주입을 사용 하 여 SQLite 데이터베이스 구현에 대 한 플랫폼 특정 요구 사항을 통합 하는 방법을 보여 줍니다.

솔루션 구조는 다음과 같습니다 (Visual Studio 및 Mac 용 Visual Studio에서 각각).

[![](pcl-images/image19.png "솔루션 구조는 다음과 같습니다 Visual Studio에서 Mac 및 Visual Studio 용 각각")](pcl-images/image19.png#lightbox)

SQLite-NET 코드 데모 목적으로 추상 개로 리팩터링 되었습니다.이 클래스는 이식 가능한 클래스 라이브러리로 컴파일될 수에 대 한 플랫폼별 부분이 (각 다른 운영 체제에서 SQLite 구현을 사용 하 여 작동)에 있으므로 및 iOS 및 Android 프로젝트의 서브 클래스로 구현 되는 실제 코드입니다.

### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

이식 가능한 클래스 라이브러리를 지원할 수 있는.NET 기능에 제한 됩니다. 사용할 수 없는 여러 플랫폼에서 실행 되도록 컴파일할 때문에 사용할 `[DllImport]` SQLite-.NET에서 사용 되는 기능. 대신 SQLite NET은 추상 클래스로 구현 되 고 나머지 공유 코드를 통해 참조 됩니다. 추상 API의 추출은 다음과 같습니다.

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

공유 코드의 나머지 추상 클래스를 사용 하 여 개체를 데이터베이스에서 "검색" 및 "저장" 합니다. 이 추상 클래스를 사용 하는 모든 응용 프로그램에서 실제 데이터베이스 기능을 제공 하는 전체 구현에 전달 해야 합니다.

### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid 및 TaskyiOS

IOS 및 Android 응용 프로그램 프로젝트 사용자 인터페이스 및 유선 전화 접속 PCL의 공유 코드에 사용 되는 기타 플랫폼별 코드를 포함 합니다.

이러한 프로젝트는 또한 추상 데이터베이스 플랫폼에서 작동 하는 API의 구현을 포함 합니다. IOS 및 Android는 Sqlite 데이터베이스 엔진은 운영 체제에 기본 제공 구현을 사용할 수 있도록 `[DllImport]` 같이 데이터베이스 연결의 구체적인 구현을 제공 합니다. 플랫폼 전용 구현 코드의 일부는 다음과 같습니다.

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```

완벽 하 게 구현 샘플 코드에서 볼 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 간단 하 게 이점 및 이식 가능한 클래스 라이브러리의 문제를 설명에 만들고에서 Pcl을 사용 하는 방법을 보여 줍니다. Visual Studio 및 Mac 용 Visual Studio 내에서 마지막 작업에서 PCL을 보여 주는 전체 샘플 응용 프로그램 – TaskyPortable –를 소개 합니다.

## <a name="related-links"></a>관련 링크

- [TaskyPortable (샘플)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [플랫폼 간 응용 프로그램 빌드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [이식 가능한 Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)
- [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md)
- [.NET Framework (Microsoft)를 사용 하 여 플랫폼 간 개발](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)
