---
title: 설치 Windows 프로젝트
description: 기존 Xamarin.Forms 솔루션에 새 Windows 프로젝트를 추가합니다.
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 071239abc71a41798a240128f8339687026296f9
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="setup-windows-projects"></a>설치 Windows 프로젝트

_기존 Xamarin.Forms 솔루션에 새 Windows 프로젝트를 추가합니다._

이전 Xamarin.Forms 솔루션 (또는 macOS에서 만든) 유니버설 Windows 플랫폼 (UWP) 응용 프로그램 프로젝트를 갖지 않습니다. 따라서 Windows 10 (UWP) 앱을 빌드하기 위해 UWP 프로젝트를 수동으로 추가 해야 합니다.

<a name="pcl" />

## <a name="update-the-pcl-profile"></a>PCL 프로필 업데이트

기존 Xamarin.Forms 앱 PCL 이식 가능한 클래스 라이브러리 () 템플릿을 사용 하는 경우 해당 프로필을 업데이트 해야 합니다.

1. **마우스 오른쪽 단추로 클릭 > 속성** (기존 설정이 다를 수 있습니다)

  ![](images/targets.png "PCL 대상")

2. 클릭는 **변경 중...**  단추

3. 확인 된 **Windows 8** 및 **Windows Phone 8.1** 옵션을 선택 (및 **Windows Phone Silveright** 은 *선택 취소*):

  ![](images/pcl.png "PCL 대상 옵션")

4. 키를 눌러 **확인** 변경 내용을 저장 합니다.

이 **프로필 111** 구성 하는 경우 프로그램 PCL Visual Studio에서 드롭 다운 목록을 사용 하 여 Mac에 대 한 합니다.

  ![](images/pcl-xs.png "PCL 프로필 111")

## <a name="add-a-universal-windows-platform-app"></a>추가 유니버설 Windows 플랫폼 앱

실행 해야 **Visual Studio 2017** 에 **Windows 10** UWP 앱을 빌드할 수 있습니다. 유니버설 Windows 플랫폼에 대 한 자세한 내용은 참조 [유니버설 Windows 플랫폼 소개](/windows/uwp/get-started/universal-application-platform-guide/)합니다.

UWP Xamarin.Forms 2.1에서 사용할 수 있으며 나중에 있으며 Xamarin.Forms.Maps Xamarin.Forms 2.2 이상에 지원 됩니다.

확인 된 <a href="#troubleshooting">문제 해결</a> 유용한 팁에 대 한 섹션.

Windows 10 휴대폰, 태블릿 및 데스크톱에서 실행 되는 UWP 앱을 추가 하려면 다음이 지침을 따릅니다.

 1 . 선택한 솔루션을 마우스 오른쪽 단추로 클릭 **추가 > 새 프로젝트...**  추가 **비어 있는 앱 (유니버설 Windows)** 프로젝트:

  ![](universal-images/add-wu.png "새 프로젝트 추가 대화 상자")

 2 . 에 **새 유니버설 Windows 플랫폼 프로젝트** 대화 상자에서 응용 프로그램에서 실행 되는 Windows 10의 최소값 및 대상 버전을 선택 합니다.

  ![](universal-images/target-version.png "새 유니버설 Windows 플랫폼 프로젝트 대화 상자")

 3 . UWP 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **NuGet 패키지 관리...**  추가 **Xamarin.Forms** 패키지 합니다. 다른 프로젝트는 솔루션의 Xamarin.Forms 패키지의 동일한 버전으로 업데이트를 확인 합니다.

 4 . 있는지 확인 새 UWP 프로젝트 생성 됩니다는 **빌드 > 구성 관리자** 창 (이 아마도 않습니다 나타나므로 기본적으로). 눈금의 **빌드** 및 **배포** 유니버설 프로젝트에 대 한 상자:

  [![](universal-images/configuration-sml.png "구성 관리자 창")](universal-images/configuration.png#lightbox "구성 관리자 창")

 5 . 선택한 프로젝트를 마우스 오른쪽 단추로 클릭 **추가 > 참조** Xamarin.Forms 응용 프로그램 프로젝트 (PCL,.NET 표준 또는 공유 프로젝트)에 대 한 참조를 만듭니다.

  ![](universal-images/addref-sml.png "참조 관리자 대화 상자")

 6 . UWP 프로젝트에서 편집 **App.xaml.cs** 포함 하도록는 `Init` 메서드 호출 내에서 `OnLaunched` 줄 52 메서드:

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7 . UWP 프로젝트에서 편집 **MainPage.xaml** 제거 하 여는 `Grid` 내에 포함 된는 `Page` 요소입니다.

 8 . **MainPage.xaml**, 새 추가 `xmlns` 에 대 한 항목 `Xamarin.Forms.Platform.UWP`:

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

 10입니다. UWP 프로젝트에서 편집 **MainPage.xaml.cs** 제거 하는 `: Page` 클래스 이름에 대 한 상속 지정자 (이제에서 상속 하므로 `WindowsPage` 이전 단계에서의 변경으로 인해):

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11 . **MainPage.xaml.cs**, 추가 `LoadApplication` 에서 호출 된 `MainPage` Xamarin.Forms 응용 프로그램을 시작 하 고 생성자:

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

12입니다. 로컬 리소스 (예: 추가할. 이미지 파일)는 필요한 기존 플랫폼 프로젝트에서.

## <a name="troubleshooting"></a>문제 해결

<a name="target-invocation-exception" />

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>"호출 예외 대상" ".NET 네이티브 도구 체인으로 컴파일"을 사용 하는 경우

UWP 앱은 여러 어셈블리 (세 번째 파티 라이브러리를 제어 하는 예를 들어 또는 자체 응용 프로그램은 여러 라이브러리도 분할)를 참조 하는 경우 Xamarin.Forms 개체 (예: 사용자 지정 렌더러) 해당 어셈블리에서 로드할 수 없습니다.

이 사용 하는 경우 발생할 수 있습니다는 **.NET 네이티브 도구 체인을 사용 하 여 컴파일** 의 UWP 앱 용 옵션인는 **속성 > 빌드 > 일반** 프로젝트에 대 한 창.

UWP 특정 오버 로드를 사용 하 여이 문제를 해결할 수 있습니다는 `Forms.Init` 에서 호출 **App.xaml.cs** 아래 코드에 나와 있는 것 처럼 (바꿔야 `ClassInOtherAssembly` 실제 클래스와 함께 코드 참조):

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

응용 프로그램에서 참조 되는 각 어셈블리에 대 한 참조를 추가 합니다.

#### <a name="dependency-services-and-net-native-compilation"></a>종속성 서비스 및.NET 네이티브 컴파일

기본 응용 프로그램 실행 파일 (예: 별도 프로젝트 또는 라이브러리) 외부에 정의 된 종속성 서비스를 해결 하려면 릴리스 빌드 컴파일.NET 네이티브를 사용 하 여 실패할 수 있습니다.

사용 하 여는 `DependencyService.Register<T>()` 수동으로 종속성 서비스 클래스를 등록 합니다. 위 예제 based, 다음과 같이 register 메서드를 추가 합니다.

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
