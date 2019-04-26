---
title: Windows 프로젝트 설정
description: 이전 Xamarin.Forms 솔루션 (또는 macOS에서 만든)는 유니버설 Windows 플랫폼 프로젝트에 없습니다 하 고 있으므로이 문서에서는 기존 Xamarin.Forms 솔루션에 새 UWP 프로젝트를 추가 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: b0f06cf15d3a3ec7eae4742d5d037e233be46d08
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60857615"
---
# <a name="setup-windows-projects"></a>Windows 프로젝트 설정

_기존 Xamarin.Forms 솔루션에 새 Windows 프로젝트를 추가합니다._

이전 Xamarin.Forms 솔루션 (또는 macOS에서 만든) 유니버설 Windows 플랫폼 (UWP) 앱 프로젝트를 갖지 않습니다. 따라서 Windows 10 (UWP) 앱을 빌드하는 UWP 프로젝트를 수동으로 추가 해야 합니다.

## <a name="add-a-universal-windows-platform-app"></a>추가 유니버설 Windows 플랫폼 앱

**Visual Studio 2019** 대 **Windows 10** UWP 앱을 빌드할 것이 좋습니다. 유니버설 Windows 플랫폼에 대 한 자세한 내용은 참조 하세요. [유니버설 Windows 플랫폼 소개](/windows/uwp/get-started/universal-application-platform-guide/)합니다.

UWP Xamarin.Forms 2.1에서 제공 되며 나중에 및 Xamarin.Forms 2.2 이상 Xamarin.Forms.Maps 지원 됩니다.

확인 합니다 <a href="#troubleshooting">문제 해결</a> 유용한 팁에 대 한 섹션입니다.

Windows 10 휴대폰, 태블릿 및 데스크톱에서 실행 되는 UWP 앱을 추가 하려면 이러한 지침을 따릅니다.

 1 . 선택한 솔루션을 마우스 오른쪽 단추로 클릭 **추가 > 새 프로젝트...**  추가 된 **비어 있는 앱 (유니버설 Windows)** 프로젝트:

  ![](universal-images/add-wu.png "새 프로젝트 대화 상자를 추가 합니다.")

 2 . 에 **새 유니버설 Windows 플랫폼 프로젝트** 대화 상자에서 Windows 10 앱에서 실행 되는 최소 및 대상 버전 선택:

  ![](universal-images/target-version.png "새 유니버설 Windows 플랫폼 프로젝트 대화 상자")

 3 . UWP 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **NuGet 패키지 관리...**  추가한 합니다 **Xamarin.Forms** 패키지 있습니다. 또한 솔루션의 다른 프로젝트는 Xamarin.Forms 패키지의 동일한 버전으로 업데이트 됩니다 확인 합니다.

 4 . 해야 새 UWP 프로젝트에서 빌드할 수는 합니다 **빌드 > 구성 관리자** 창 (이 아마도 않습니다 발생 했을 기본적으로). 눈금은 **빌드** 하 고 **배포** 유니버설 프로젝트에 대 한 상자:

  [![](universal-images/configuration-sml.png "구성 관리자 창")](universal-images/configuration.png#lightbox "구성 관리자 창")

 5 . 프로젝트를 마우스 오른쪽 단추로 클릭 **추가 > 참조** Xamarin.Forms 응용 프로그램 프로젝트 (.NET Standard 또는 공유 프로젝트)에 대 한 참조를 만듭니다.

  ![](universal-images/addref-sml.png "참조 관리자 대화 상자")

 6 . UWP 프로젝트에서 편집 **App.xaml.cs** 포함 하는 `Init` 메서드 호출 내에서 `OnLaunched` 줄 52 메서드:

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7 . UWP 프로젝트에서 편집 **MainPage.xaml** 제거 하 여 합니다 `Grid` 내에 포함 된를 `Page` 요소입니다.

 8 . **MainPage.xaml**, 새 `xmlns` 에 대 한 항목 `Xamarin.Forms.Platform.UWP`:

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 9 . **MainPage.xaml**, 루트 변경 `<Page` 요소를 `<forms:WindowsPage`:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 10 . UWP 프로젝트에서 편집 **MainPage.xaml.cs** 제거할 합니다 `: Page` 클래스 이름에 대 한 상속 지정자 (이제에서 상속 하므로 `WindowsPage` 이전 단계에서 수행 된 변경으로 인해):

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11 . **MainPage.xaml.cs**를 추가 합니다 `LoadApplication` 에서 호출을 `MainPage` Xamarin.Forms 앱을 시작 하는 생성자:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

12 . 모든 로컬 리소스 (예: 추가 이미지 파일)는 필요한 기존 플랫폼 프로젝트에서.

## <a name="troubleshooting"></a>문제 해결

<a name="target-invocation-exception" />

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>"대상 호출 예외" ".NET 네이티브 도구 체인을 사용 하 여 컴파일"을 사용 하는 경우

UWP 앱에서 여러 어셈블리 (타사 라이브러리를 제어 하는 예를 들어 또는 자체 앱은 여러 라이브러리도 분할)를 참조 하는 경우 Xamarin.Forms 개체 (예: 사용자 지정 렌더러) 해당 어셈블리에서 로드 하지 못할 수 있습니다.

사용 하는 경우 발생할 수 있습니다는 **.NET 네이티브 도구 체인을 사용 하 여 컴파일합니다** 에서 UWP 앱에 대 한 옵션을 **속성 > 빌드 > 일반** 프로젝트에 대 한 창.

UWP 특정 오버 로드를 사용 하 여 수정할 수 있습니다 합니다 `Forms.Init` 에서 호출 **App.xaml.cs** 아래 코드에 표시 된 대로 (바꿔야 `ClassInOtherAssembly` 코드에서 참조 하는 실제 클래스를 사용 하 여):

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

솔루션 탐색기에서 직접 참조 또는 NuGet을 통해 참조로 추가 된 각 어셈블리에 대 한 항목을 추가 합니다.

#### <a name="dependency-services-and-net-native-compilation"></a>종속성 서비스 및.NET 네이티브 컴파일

릴리스 빌드가.NET 네이티브 컴파일을 사용 하는 기본 앱 실행 파일 (예: 라이브러리 또는 별도 프로젝트를) 외부에서 정의 되는 종속성 서비스 확인에 실패할 수 있습니다.

사용 된 `DependencyService.Register<T>()` 수동으로 종속성 서비스 클래스를 등록 하는 방법입니다. 위의 예제에 based, 다음과 같은 register 메서드를 추가 합니다.

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
