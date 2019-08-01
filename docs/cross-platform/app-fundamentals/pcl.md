---
title: PCL (이식 가능한 클래스 라이브러리) 소개
description: 이 문서에서는 PCL (이식 가능한 클래스 라이브러리) 프로젝트를 소개 하 고 Mac용 Visual Studio 및 Visual Studio에서 PCL 프로젝트를 만들고 사용 하는 과정을 안내 합니다.
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: a4ee81f7d59c9fb680dfd371a7aaba7660fb3343
ms.sourcegitcommit: f255aa286bd52e8a80ffa620c2e93c97f069f8ec
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68681069"
---
# <a name="portable-class-libraries-pcl"></a>PCL(이식 가능한 클래스 라이브러리)

> [!TIP]
> PCLs (이식 가능한 클래스 라이브러리)는 최신 버전의 Visual Studio에서 더 이상 사용 되지 않는 것으로 간주 됩니다.
> PCLs를 계속 열고, 편집 하 고, 컴파일할 수는 있지만 새 프로젝트의 경우에는 [.NET Standard 라이브러리](~/cross-platform/app-fundamentals/net-standard.md) 를 사용 하 여 더 큰 API 노출 영역에 액세스 하는 것이 좋습니다.

플랫폼 간 응용 프로그램을 빌드하는 핵심 구성 요소는 다양 한 플랫폼별 프로젝트에서 코드를 공유할 수 있습니다. 그러나이는 다른 플랫폼이 .NET BCL (기본 클래스 라이브러리)의 다른 하위 집합을 사용 하는 경우가 많기 때문에 실제로 다른 .NET Core 라이브러리 프로필에 빌드됩니다. 즉, 각 플랫폼은 동일한 프로필을 대상으로 하는 클래스 라이브러리만 사용할 수 있으므로 각 플랫폼에 대 한 별도의 클래스 라이브러리 프로젝트를 필요로 하는 것으로 나타납니다.

이러한 문제를 해결 하는 코드 공유에는 **.NET Standard 프로젝트**, **공유 자산 프로젝트**및 **PCL (이식 가능한 클래스 라이브러리) 프로젝트**의 세 가지 주요 방법이 있습니다.

- **.NET Standard 프로젝트** 는 .net 코드를 공유 하는 데 선호 되는 방법입니다. [.NET Standard 프로젝트 및 Xamarin](~/cross-platform/app-fundamentals/net-standard.md)에 대해 자세히 알아보세요.
- **공유 자산 프로젝트** 는 단일 파일 집합을 사용 하 고 솔루션 내에서 코드를 공유 하는 빠르고 간단한 방법을 제공 하며, 일반적으로 조건부 컴파일 지시문을 사용 하 여이를 사용 하는 다양 한 플랫폼에 대 한 코드 경로를 지정 합니다. 정보 [공유 프로젝트 문서](~/cross-platform/app-fundamentals/shared-projects.md)를 참조 하세요.
- **PCL** 프로젝트는 알려진 BCL 클래스/기능 집합을 지 원하는 특정 프로필을 대상으로 합니다. 그러나 PCL에서 아래쪽은 프로필 특정 코드를 자체 라이브러리로 분리 하기 위해 추가적인 아키텍처 노력이 필요한 경우가 많습니다.

이 페이지에서는 특정 프로필을 대상으로 하는 **PCL** 프로젝트를 만드는 방법을 설명 합니다 .이 프로젝트는 여러 플랫폼별 프로젝트에서 참조할 수 있습니다.

## <a name="what-is-a-portable-class-library"></a>이식 가능한 클래스 라이브러리는 무엇 인가요?

응용 프로그램 프로젝트 또는 라이브러리 프로젝트를 만들 때 생성 되는 DLL은 생성 된 특정 플랫폼 에서만 작동 하도록 제한 됩니다. 이렇게 하면 Windows 앱에 대 한 어셈블리를 작성 한 후 Xamarin.ios 및 Xamarin.ios에서 다시 사용할 수 없습니다.

그러나 이식 가능한 클래스 라이브러리를 만들 때 코드를 실행 하려는 플랫폼의 조합을 선택할 수 있습니다. 이식 가능한 클래스 라이브러리를 만들 때 수행 하는 호환성 선택은 라이브러리에서 지 원하는 플랫폼을 설명 하는 "프로필" 식별자로 변환 됩니다.

다음 표에서는 .NET 플랫폼에 따라 달라 지는 기능 중 일부를 보여 줍니다. 특정 장치/플랫폼에서 실행 되도록 보장 되는 PCL 어셈블리를 작성 하려면 프로젝트를 만들 때 필요한 지원을 선택 하면 됩니다.

|기능|.NET Framework|UWP 앱|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|코어|Y|Y|Y|Y|Y|
|LINQ|Y|Y|Y|Y|Y|
|IQueryable|Y|Y|Y|7.5 +|Y|
|Serialization|Y|Y|Y|Y|Y|
|데이터 주석|4.0.3 +|Y|Y||Y|

Xamarin 열은 Xamarin.ios 및 Xamarin이 Visual Studio와 함께 제공 되는 모든 프로필을 지원 한다는 사실을 반영 하 고 사용자가 만드는 모든 라이브러리의 기능 가용성은 지원 하도록 선택한 다른 플랫폼에 의해서만 제한 됩니다.

여기에는 다음과 같은 조합의 프로필이 포함 됩니다.

- .NET 4 또는 .NET 4.5
- Silverlight 5
- Windows Phone 8
- UWP 앱

[Microsoft 웹 사이트](https://msdn.microsoft.com/library/gg597391(v=vs.110).aspx) 에서 다양 한 프로필 기능에 대 한 자세한 내용을 읽고 지원 되는 프레임 워크 정보 및 기타 메모를 포함 하는 다른 커뮤니티 구성원의 [PCL 프로필 요약](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) 을 확인할 수 있습니다.

**이점**

1. 중앙 집중식 코드 공유 – 다른 라이브러리나 응용 프로그램에서 사용할 수 있는 단일 프로젝트에서 코드를 작성 하 고 테스트 합니다.
2. 리팩터링 작업은 솔루션에 로드 된 모든 코드 (이식 가능한 클래스 라이브러리 및 플랫폼별 프로젝트)에 영향을 줍니다.
3. PCL 프로젝트는 솔루션의 다른 프로젝트에서 쉽게 참조할 수 있으며, 다른 사용자가 해당 솔루션에서 참조할 수 있도록 출력 어셈블리를 공유할 수 있습니다.

**단점**

1. 동일한 이식 가능한 클래스 라이브러리는 여러 응용 프로그램 간에 공유 되므로 플랫폼별 라이브러리를 참조할 수 없습니다 (예: Community.CsharpSqlite.WP7).
2. 이식 가능한 클래스 라이브러리 하위 집합에는 Android 용 Monotouch.dialog 및 Mono (예: DllImport 또는 System.object)에서 사용할 수 있는 클래스가 포함 되지 않을 수 있습니다.

> [!NOTE]
> 이식 가능한 클래스 라이브러리는 최신 버전의 Visual Studio에서 더 이상 사용 되지 않으며 [.NET Standard 라이브러리](net-standard.md) 를 대신 사용 하는 것이 좋습니다.

일부 익스텐트는 공급자 패턴 또는 종속성 주입을 사용 하 여 이식 가능한 클래스 라이브러리에 정의 된 인터페이스 또는 기본 클래스에 대해 플랫폼 프로젝트의 실제 구현을 코딩 하는 것을 우회할 수 있습니다.

이 다이어그램에서는 이식 가능한 클래스 라이브러리를 사용 하 여 코드를 공유 하 고 종속성 주입을 사용 하 여 플랫폼 종속 기능을 전달 하는 플랫폼 간 응용 프로그램의 아키텍처를 보여 줍니다.

[![](pcl-images/image1.png "이 다이어그램에서는 이식 가능한 클래스 라이브러리를 사용 하 여 코드를 공유 하 고 종속성 주입을 사용 하 여 플랫폼 종속 기능을 전달 하는 플랫폼 간 응용 프로그램의 아키텍처를 보여 줍니다.")](pcl-images/image1.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Mac용 Visual Studio 연습

이 섹션에서는 Mac용 Visual Studio를 사용 하 여 이식 가능한 클래스 라이브러리를 만들고 사용 하는 방법을 안내 합니다. 전체 구현은 PCL 예제 섹션을 참조 하세요.

### <a name="creating-a-pcl"></a>PCL 만들기

이식 가능한 클래스 라이브러리를 솔루션에 추가 하는 것은 일반 라이브러리 프로젝트를 추가 하는 것과 매우 비슷합니다.

1. **새 프로젝트** 대화 상자에서 **다중 플랫폼 > 라이브러리 > 이식 가능한 라이브러리** 옵션을 선택 합니다.

    ![새 PCL 프로젝트 만들기](pcl-images/image2.png)

1. Mac용 Visual Studio에서 PCL을 만들 때 Xamarin.ios 및 Xamarin.ios에 대해 작동 하는 프로필을 사용 하 여 자동으로 구성 됩니다. PCL 프로젝트는 다음 스크린샷에 표시 된 것 처럼 표시 됩니다.

    ![Solution pad의 PCL 프로젝트](pcl-images/image3.png)

이제 PCL에서 코드를 추가할 준비가 되었습니다. 다른 프로젝트 (응용 프로그램 프로젝트, 라이브러리 프로젝트 및 기타 PCL 프로젝트) 에서도 참조할 수 있습니다.

### <a name="editing-pcl-settings"></a>PCL 설정 편집

이 프로젝트에 대 한 PCL 설정을 확인 하 고 변경 하려면 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **옵션 > >** 선택 하 여 다음과 같은 화면을 표시 합니다.

[![프로필을 설정 하는 PCL 프로젝트 옵션](pcl-images/image4-sml.png)](pcl-images/image4.png#lightbox)

**변경 ...** 을 클릭 하 여이 이식 가능한 클래스 라이브러리의 대상 프로필을 변경 합니다.

코드가 PCL에 이미 추가 된 후 프로필이 변경 되 면 코드에서 새로 선택한 프로필에 포함 되지 않은 기능을 참조 하는 경우 라이브러리가 더 이상 컴파일되지 않을 수 있습니다.

## <a name="working-with-a-pcl"></a>PCL 사용

PCL 라이브러리에서 코드를 작성 하면 Mac용 Visual Studio 편집기에서 선택한 프로필의 제한 사항을 인식 하 여 자동 완성 옵션을 적절 하 게 조정 합니다. 예를 들어이 스크린샷은 Mac용 Visual Studio에 사용 된 기본 프로필 (Profile136)을 사용 하 여 System.IO에 대 한 자동 완성 옵션을 보여 줍니다. 사용 가능한 클래스의 절반을 나타내는 스크롤 막대가 표시 됩니다 (실제로는 14 개만 있음). 사용 가능한 클래스).

[![PCL의 System.IO 클래스에 있는 14 개 클래스의 Intellisense 목록](pcl-images/image6.png)](pcl-images/image6.png#lightbox)

Xamarin.ios 또는 xamarin.ios 프로젝트에서 System.IO 자동 완성과 비교 – PCL 프로필에 없는와 `File` `Directory` 같은 일반적으로 사용 되는 클래스를 포함 하 여 사용할 수 있는 40 클래스가 있습니다.

[![.NET Framework System.IO 네임 스페이스에서 40 클래스의 Intellisense 목록](pcl-images/image7.png)](pcl-images/image7.png#lightbox)

이는 PCL 사용의 기본 절충을 반영 합니다. 많은 플랫폼에서 코드를 원활 하 게 공유 하는 기능을 사용 하면 가능한 모든 플랫폼에서 비슷한 구현이 없으므로 특정 Api를 사용할 수 없습니다.

### <a name="using-pcl"></a>PCL 사용

PCL 프로젝트를 만든 후에는 일반적으로 참조를 추가 하는 것과 같은 방식으로 호환 되는 응용 프로그램 또는 라이브러리 프로젝트에서 해당 프로젝트에 대 한 참조를 추가할 수 있습니다. Mac용 Visual Studio에서 참조 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 편집** ...을 선택 하 고 다음과 같이 **프로젝트** 탭으로 전환 합니다.

[![참조 편집 옵션을 통해 PCL에 대 한 참조 추가](pcl-images/image8.png)](pcl-images/image8.png#lightbox)

다음 스크린샷에는 TaskyPortable 샘플 앱에 대 한 Solution pad가 나와 있습니다. 여기에는 PCL 프로젝트의 pcl 라이브러리와 해당 PCL 라이브러리에 대 한 참조가 표시 됩니다.

[![PCL 프로젝트를 보여 주는 TaskyPortable 샘플 솔루션](pcl-images/image9.png)](pcl-images/image9.png#lightbox)

PCL의 출력 (즉, 결과 어셈블리 DLL)을 대부분의 프로젝트에 대 한 참조로 추가할 수도 있습니다. 따라서 PCL은 플랫폼 간 구성 요소 및 라이브러리를 제공 하는 이상적인 방법입니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-walkthrough"></a>Visual Studio 연습

이 섹션에서는 Visual Studio를 사용 하 여 이식 가능한 클래스 라이브러리를 만들고 사용 하는 방법을 안내 합니다. 전체 구현은 PCL 예제 섹션을 참조 하세요.

### <a name="creating-a-pcl"></a>PCL 만들기

Visual Studio에서 솔루션에 PCL을 추가 하는 것은 일반 프로젝트를 추가 하는 것과 약간 다릅니다.

1. **새 프로젝트 추가** 화면에서 **클래스 라이브러리 (레거시 이식 가능)** 옵션을 선택 합니다. 이 프로젝트 형식이 더 이상 사용 되지 않는 것을 확인 하는 것이 적절 합니다.

    [![이식 가능한 클래스 라이브러리를 만들기 위한 새 프로젝트 창](pcl-images/image10-sml.png "이식 가능한 클래스 라이브러리")](pcl-images/image10.png#lightbox)

2. Visual Studio는 프로필을 구성할 수 있도록 다음 대화 상자를 즉시 표시 합니다.
 지원 해야 하는 플랫폼을 확인 하 고 확인을 누릅니다.

    [![라이브러리에 대 한 대상 플랫폼을 선택 합니다] . (pcl-images/image11-sml.png "지원 해야 하는 플랫폼을 확인 하 고 확인을 누릅니다") .](pcl-images/image11.png#lightbox)

3. Pcl 프로젝트는 프로젝트 이름 옆에 표시 되는 &ndash; 텍스트 **(이식 가능)** 가 pcl 임을 나타내는 솔루션 탐색기 표시 된 대로 표시 됩니다.

    ![PCL 프로필에서 정의 된 NET Framework](pcl-images/image12.png "PCL 프로필에서 정의 된 NET Framework")

이제 PCL에서 코드를 추가할 준비가 되었습니다. 다른 프로젝트 (응용 프로그램 프로젝트, 라이브러리 프로젝트 및 기타 PCL 프로젝트) 에서도 참조할 수 있습니다.

### <a name="editing-pcl-settings"></a>PCL 설정 편집

다음 스크린샷에 표시 된 것 처럼 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성 > 라이브러리** 를 선택 하 여 PCL 설정을 보고 변경할 수 있습니다.

[![플랫폼 대상 편집](pcl-images/image13-sml.png)](pcl-images/image13.png#lightbox)

코드가 PCL에 이미 추가 된 후 프로필이 변경 되 면 코드에서 새로 선택한 프로필에 포함 되지 않은 기능을 참조 하는 경우 라이브러리가 더 이상 컴파일되지 않을 수 있습니다.

> [!TIP]
> 또한이에 대해 알려주는 메시지도 있습니다 **. NETStandard는 코드를 공유 하는 데 권장 되는 방법입니다**. 이는 PCLs가 여전히 지원 되지만 .NET Standard로 업그레이드 하는 것을 나타내는 것입니다.

### <a name="working-with-a-pcl"></a>PCL 사용

PCL 라이브러리에서 코드를 작성할 때 Visual Studio는 선택한 프로필의 제한 사항을 인식 하 고 Intellisense 옵션을 적절 하 게 조정 합니다. 예를 들어이 스크린샷에서는 기본 프로필 (Profile136)을 사용 하 여 System.IO에 대 한 자동 완성 옵션을 보여 줍니다. 사용 가능한 클래스의 절반을 나타내는 스크롤 막대가 표시 됩니다 (실제로는 14 개의 클래스만 사용할 수 있음).

[![PCL에서 사용할 수 있는 IO 클래스의 수 감소](pcl-images/image14.png)](pcl-images/image14.png#lightbox)

일반 프로젝트에서 System.IO 자동 완성을 사용 하 여 비교 합니다. 예 `File` 를 들어, `Directory` PCL 프로필에 없는 일반적으로 사용 되는 클래스를 포함 하 여 40 클래스를 사용할 수 있습니다.

[![.NET Framework에서 사용할 수 있는 많은 IO 클래스](pcl-images/image15.png)](pcl-images/image15.png#lightbox)

이는 PCL 사용의 기본 절충을 반영 합니다. 많은 플랫폼에서 코드를 원활 하 게 공유 하는 기능을 사용 하면 가능한 모든 플랫폼에서 비슷한 구현이 없으므로 특정 Api를 사용할 수 없습니다.

> [!TIP]
> .NET Standard 2.0은 System.IO 네임 스페이스를 포함 하 여 PCLs 보다 훨씬 더 큰 API 노출 영역을 나타냅니다. 새 프로젝트의 경우 PCL을 통해 .NET Standard 하는 것이 좋습니다.

### <a name="using-pcl"></a>PCL 사용

PCL 프로젝트를 만든 후에는 일반적으로 참조를 추가 하는 것과 같은 방식으로 호환 되는 응용 프로그램 또는 라이브러리 프로젝트에서 해당 프로젝트에 대 한 참조를 추가할 수 있습니다. Visual Studio에서 참조 노드를 마우스 오른쪽 단추로 클릭 하 고 `Add Reference...` 다음과 같이 **솔루션 > 프로젝트** 탭으로 전환 합니다.

[![참조 프로젝트 추가 탭을 통해 PCL에 참조를 추가 합니다.](pcl-images/image16.png)](pcl-images/image16.png#lightbox)

다음 스크린샷에는 TaskyPortable 샘플 앱에 대 한 솔루션 창이 나와 있습니다. 여기에는 PCL 프로젝트의 pcl 라이브러리와 해당 PCL 라이브러리에 대 한 참조가 표시 됩니다.

[![PCL 라이브러리를 보여 주는 TaskyPortable 샘플 솔루션](pcl-images/image17.png)](pcl-images/image17.png#lightbox)

PCL의 출력 (즉, 결과 어셈블리 DLL)을 대부분의 프로젝트에 대 한 참조로 추가할 수도 있습니다.
따라서 PCL은 플랫폼 간 구성 요소 및 라이브러리를 제공 하는 이상적인 방법입니다.

-----

## <a name="pcl-example"></a>PCL 예

[Taskyportable](https://docs.microsoft.com/samples/xamarin/mobile-samples/taskyportable/) 샘플 응용 프로그램은 Xamarin에서 이식 가능한 클래스 라이브러리를 사용할 수 있는 방법을 보여 줍니다.
IOS 및 Android에서 실행 되는 응용 프로그램의 몇 가지 스크린샷:

[![](pcl-images/image18.png "IOS, Android 및 Windows Phone에서 실행 되는 응용 프로그램의 몇 가지 스크린샷은 다음과 같습니다.")](pcl-images/image18.png#lightbox)

이 클래스는 단순히 이식 가능한 코드 인 많은 데이터 및 논리 클래스를 공유 하며, SQLite 데이터베이스 구현에 대 한 종속성 주입을 사용 하 여 플랫폼별 요구 사항을 통합 하는 방법도 보여 줍니다.

솔루션 구조는 아래에 표시 됩니다 (각각 Mac용 Visual Studio 및 Visual Studio).

[![](pcl-images/image19.png "솔루션 구조는 Mac용 Visual Studio 및 Visual Studio 각각에 나와 있습니다.")](pcl-images/image19.png#lightbox)

SQLite-NET 코드에는 각 운영 체제에서 각각 다른 운영 체제의 SQLite 구현에 사용할 수 있는 플랫폼별 부분이 있으므로, 이식 가능한 클래스 라이브러리로 컴파일될 수 있는 추상 클래스로 리팩터링 되었습니다. iOS 및 Android 프로젝트에서 서브 클래스로 구현 되는 실제 코드입니다.

### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

이식 가능한 클래스 라이브러리는 지원할 수 있는 .NET 기능에서 제한 됩니다. 이는 여러 플랫폼에서 실행 되도록 컴파일되므로 SQLite-NET에서 사용 되는 `[DllImport]` 기능을 사용할 수 없습니다. 대신 SQLite-NET은 추상 클래스로 구현 된 다음 나머지 공유 코드를 통해 참조 됩니다. 추상 API의 추출은 다음과 같습니다.

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

나머지 공유 코드는 추상 클래스를 사용 하 여 데이터베이스에서 개체를 "저장" 하 고 "검색" 합니다. 이 추상 클래스를 사용 하는 응용 프로그램에서는 실제 데이터베이스 기능을 제공 하는 완전 한 구현을 전달 해야 합니다.

### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid 및 Taskyandroid

IOS 및 Android 응용 프로그램 프로젝트에는 PCL의 공유 코드를 연결 하는 데 사용 되는 사용자 인터페이스 및 기타 플랫폼별 코드가 포함 되어 있습니다.

이러한 프로젝트에는 해당 플랫폼에서 작동 하는 추상 데이터베이스 API의 구현도 포함 되어 있습니다. IOS 및 Android에서 Sqlite 데이터베이스 엔진은 운영 체제에 기본 제공 되므로 구현을 사용 `[DllImport]` 하 여 데이터베이스 연결의 구체적인 구현을 제공할 수 있습니다. 플랫폼별 구현 코드의 발췌는 다음과 같습니다.

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```

전체 구현은 샘플 코드에서 볼 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 이식 가능한 클래스 라이브러리의 이점 및 문제에 대해 간략하게 설명 하 고, Mac용 Visual Studio 및 Visual Studio 내부에서 PCLs를 만들고 사용 하는 방법을 보여 줍니다. 그리고 마지막으로 작업 중인 PCL을 보여 주는 완전 한 샘플 응용 프로그램 – TaskyPortable이 도입 되었습니다.

## <a name="related-links"></a>관련 링크

- [TaskyPortable (샘플)](https://docs.microsoft.com/samples/xamarin/mobile-samples/taskyportable/)
- [플랫폼 간 애플리케이션 빌드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [이식 가능한 Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)
- [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md)
- [.NET Framework를 사용한 크로스 플랫폼 개발 (Microsoft)](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)
