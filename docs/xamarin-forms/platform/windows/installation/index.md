---
제목: "Windows 프로젝트 설정" 설명: "이전 Xamarin.Forms 솔루션 (또는 macOS에서 만든)은 유니버설 Windows 플랫폼 프로젝트를 포함 하지 않으므로이 문서에서는 기존 솔루션에 새 UWP 프로젝트를 추가 하는 방법을 설명 Xamarin.Forms 합니다."
assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320: xamarin-forms author: davidbritch: dabritch:: 04/10/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="setup-windows-projects"></a>Windows 프로젝트 설정

_기존 솔루션에 새 Windows 프로젝트 추가 Xamarin.Forms_

이전 Xamarin.Forms 솔루션 (또는 macOS에서 만든)은 UWP (유니버설 Windows 플랫폼) 앱 프로젝트를 포함 하지 않습니다. 따라서 Windows 10 (UWP) 앱을 빌드하려면 UWP 프로젝트를 수동으로 추가 해야 합니다.

## <a name="add-a-universal-windows-platform-app"></a>유니버설 Windows 플랫폼 앱 추가

**Windows 10** 의 **Visual STUDIO 2019** 은 UWP 앱을 빌드하는 데 권장 됩니다. 유니버설 Windows 플랫폼에 대 한 자세한 내용은 [유니버설 Windows 플랫폼 소개](/windows/uwp/get-started/universal-application-platform-guide/)를 참조 하세요.

UWP는 Xamarin.Forms 2.1 이상 및에서 사용할 수 있습니다 Xamarin.Forms . 맵은 2.2 이상에서 지원 됩니다 Xamarin.Forms .

<a href="#troubleshooting">문제 해결</a> 섹션에서 유용한 팁을 확인 하세요.

다음 지침에 따라 Windows 10 휴대폰, 태블릿 및 데스크톱에서 실행 되는 UWP 앱을 추가 합니다.

 1(sp1). 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 프로젝트 ...** 를 선택 하 고 **비어 있는 앱 (유니버설 Windows)** 프로젝트를 추가 합니다.

  ![](universal-images/add-wu.png "Add New Project Dialog")

 sr-2. **새 유니버설 Windows 플랫폼 프로젝트** 대화 상자에서 앱이 실행 되는 최소 및 대상 버전의 Windows 10을 선택 합니다.

  ![](universal-images/target-version.png "New Universal Windows Platform Project Dialog")

 3. UWP 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리 ...** 를 선택 하 고 패키지를 추가 합니다. **Xamarin.Forms** 솔루션의 다른 프로젝트도 동일한 버전의 패키지로 업데이트 되는지 확인 Xamarin.Forms 합니다.

 3-4. 새 UWP 프로젝트가 **빌드 > Configuration Manager** 창에 빌드 되었는지 확인 합니다 .이는 기본적으로 발생 하지 않습니다. 유니버설 프로젝트에 대 한 **빌드** 및 **배포** 상자의 틱:

  [![](universal-images/configuration-sml.png "Configuration Manager Window")](universal-images/configuration.png#lightbox "Configuration Manager Window")

 5. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가 > 참조** 를 선택 하 고 Xamarin.Forms 응용 프로그램 프로젝트 (.NET Standard 또는 공유 프로젝트)에 대 한 참조를 만듭니다.

  ![](universal-images/addref-sml.png "Reference Manager Dialog")

 6@@. UWP 프로젝트에서 **App.xaml.cs** 를 편집 하 여 메서드 `Init` 내에 메서드 호출을 `OnLaunched` 52 줄에 포함 시킵니다.

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 일. UWP 프로젝트에서 요소 내에 포함 된를 제거 하 여 **mainpage** 을 편집 합니다. `Grid` `Page`

 20cm(8. **Mainpage**에서 `xmlns` 다음에 대 한 새 항목을 추가 합니다. `Xamarin.Forms.Platform.UWP`

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 되었는지. **Mainpage**에서 root 요소를로 변경 합니다. `<Page` `<forms:WindowsPage`

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 5-10. UWP 프로젝트에서 **MainPage.xaml.cs** 를 편집 하 여 `: Page` 클래스 이름에 대 한 상속 지정자를 제거 합니다 .이 지정자는 이제 `WindowsPage` 이전 단계에서 변경한 내용 때문에에서 상속 됩니다.

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 pt. **MainPage.xaml.cs**에서 `LoadApplication` 생성자에 호출을 추가 `MainPage` 하 여 앱을 시작 합니다 Xamarin.Forms .

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

> [!NOTE]
> 메서드에 대 한 인수는 `LoadApplication` `Xamarin.Forms.Application` .net standard 프로젝트에 정의 된 인스턴스입니다.

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

10. 로컬 리소스를 추가 합니다 (예: 이미지 파일)을 입력 합니다.

## <a name="troubleshooting"></a>문제 해결

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>".NET 네이티브 도구 체인으로 컴파일"을 사용 하는 경우 "대상 호출 예외"

UWP 앱이 여러 어셈블리를 참조 하는 경우 (예: 타사 컨트롤 라이브러리 또는 앱 자체가 여러 라이브러리로 분할 된 경우)는 Xamarin.Forms 이러한 어셈블리 (예: 사용자 지정 렌더러)에서 개체를 로드 하지 못할 수 있습니다.

프로젝트에 대 한 **속성 > 빌드 > 일반** 창에서 UWP 앱에 대 한 옵션인 **.NET 네이티브 도구 체인으로 컴파일** 을 사용 하는 경우이 문제가 발생할 수 있습니다.

아래 코드에 표시 된 것 처럼 App.xaml.cs에서 호출의 UWP 관련 오버 로드를 사용 하 여이 문제를 해결할 수 있습니다 `Forms.Init` (를 코드에서 참조 하는 실제 클래스로 대체 해야 함 **App.xaml.cs** `ClassInOtherAssembly` ).

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

직접 참조 또는 NuGet을 통해 솔루션 탐색기에 참조로 추가한 각 어셈블리에 대 한 항목을 추가 합니다.

#### <a name="dependency-services-and-net-native-compilation"></a>종속성 서비스 및 .NET 네이티브 컴파일

.NET 네이티브 컴파일을 사용 하는 릴리스 빌드는 주 앱 실행 파일 (예: 별도의 프로젝트 또는 라이브러리) 외부에 정의 된 종속성 서비스를 확인 하지 못할 수 있습니다.

메서드를 사용 `DependencyService.Register<T>()` 하 여 종속성 서비스 클래스를 수동으로 등록 합니다. 위의 예제에 따라 register 메서드를 다음과 같이 추가 합니다.

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
