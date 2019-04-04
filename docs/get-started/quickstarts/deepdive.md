---
title: Xamarin.Forms 빠른 시작에 대 한 심층 정보
description: 이 문서에서는 Xamarin.Forms를 사용하여 애플리케이션 개발의 기본적인 사항을 검사합니다. Xamarin.Forms 애플리케이션 분석, 아키텍처 및 애플리케이션 기본 사항, 사용자 인터페이스에 대해 다루었습니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 7B2340A1-6883-41D8-860C-0BB6C4E0C316
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/27/2018
ms.openlocfilehash: 67b189254cc08fac0323b7df5fcbab5abd994c05
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58855018"
---
# <a name="xamarinforms-quickstart-deep-dive"></a>Xamarin.Forms 빠른 시작에 대 한 심층 정보

에 [Xamarin.Forms 빠른 시작](~/get-started/index.yml), 정보 응용 프로그램을 빌드 했습니다. 이 문서에서는 Xamarin.Forms 애플리케이션의 핵심 작동 원리를 이해하기 위해 무엇이 빌드되었는지 검토합니다.

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Visual Studio 소개

Visual Studio는 코드를 *솔루션* 및 *프로젝트*로 구성합니다. 솔루션은 하나 이상의 프로젝트를 포함할 수 있는 컨테이너입니다. 프로젝트는 애플리케이션, 지원 라이브러리, 테스트 애플리케이션 등이 될 수 있습니다. 정보 응용 프로그램으로 다음 스크린샷에 표시 된 것 처럼 4 개의 프로젝트를 포함 하는 솔루션 하나로 구성 됩니다.

![](deepdive-images/vs/solution.png "Visual Studio 솔루션 탐색기")

프로젝트는 다음과 같습니다.

- 정보-이 프로젝트는 모든 공유 코드와 공유 UI를 보유 하는.NET Standard 라이브러리 프로젝트.
- Notes.Android-이 프로젝트는 Android 관련 코드를 보관 및 Android 응용 프로그램에 대 한 진입점입니다.
- Notes.iOS-이 프로젝트 iOS 관련 코드를 보유 하 고 iOS 응용 프로그램에 대 한 진입점입니다.
- Notes.UWP –이 프로젝트는 유니버설 Windows 플랫폼 (UWP) 관련 코드 및 UWP 응용 프로그램에 대 한 진입점입니다.

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms 응용 프로그램 분석

다음 스크린샷은 Visual Studio에서 메모.NET Standard 라이브러리 프로젝트의 콘텐츠를 보여줍니다.

![](deepdive-images/vs/net-standard-project.png "Phoneword .NET Standard 프로젝트 콘텐츠")

프로젝트에는 **NuGet** 및 **SDK** 노드를 포함하는 **종속성** 노드가 있습니다.

- **NuGet** &ndash; Xamarin.Forms 및 sqlite-net-pcl NuGet 패키지는 프로젝트에 추가 되었습니다.
- **SDK** &ndash; .NET Standard를 정의하는 NuGet 패키지의 전체 집합을 참조하는 `NETStandard.Library` 메타패키지.

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Mac용 Visual Studio 소개

[Mac용 Visual Studio](/visualstudio/mac/)는 코드를 *솔루션* 및 *프로젝트*로 구성하는 Visual Studio 연습을 따릅니다. 솔루션은 하나 이상의 프로젝트를 포함할 수 있는 컨테이너입니다. 프로젝트는 애플리케이션, 지원 라이브러리, 테스트 애플리케이션 등이 될 수 있습니다. 정보 응용 프로그램으로 다음 스크린샷에 표시 된 대로 세 개의 프로젝트를 포함 하는 솔루션 하나로 구성 됩니다.

![](deepdive-images/vsmac/solution.png "Mac용 Visual Studio 솔루션 창")

프로젝트는 다음과 같습니다.

- 정보-이 프로젝트는 모든 공유 코드와 공유 UI를 보유 하는.NET Standard 라이브러리 프로젝트.
- Notes.Android-이 프로젝트는 Android 관련 코드를 보관 및 Android 응용 프로그램에 대 한 진입점입니다.
- Notes.iOS-이 프로젝트 iOS 관련 코드를 보유 하 고 iOS 응용 프로그램에 대 한 진입점입니다.

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms 응용 프로그램 분석

다음 스크린샷은 Mac 용 Visual Studio에서 메모.NET Standard 라이브러리 프로젝트의 콘텐츠를 보여줍니다.

![](deepdive-images/vsmac/net-standard-project.png "Phoneword .NET Standard 라이브러리 프로젝트 콘텐츠")

프로젝트에는 **NuGet** 및 **SDK** 노드를 포함하는 **종속성** 노드가 있습니다.

- **NuGet** &ndash; Xamarin.Forms 및 sqlite-net-pcl NuGet 패키지는 프로젝트에 추가 되었습니다.
- **SDK** &ndash; .NET Standard를 정의하는 NuGet 패키지의 전체 집합을 참조하는 `NETStandard.Library` 메타패키지.

::: zone-end

프로젝트는 또한 다음과 같은 여러 파일로 구성됩니다.

- **Data\NoteDatabase.cs** –이 클래스에는 데이터베이스 만들기, 데이터를 읽고에서 데이터를 쓸 것에서 데이터를 삭제 하는 코드가 포함 되어 있습니다.
- **Models\Note.cs** –이 클래스 정의 `Note` 모델 인스턴스가 응용 프로그램에서 각 메모에 대 한 데이터를 저장 합니다.
- **App.xaml** - 응용 프로그램의 리소스 사전을 정의하는 `App` 클래스에 대한 XAML 태그입니다.
- **App.xaml.cs** – `App` 클래스의 코드 숨김으로, 각 플랫폼에서 응용 프로그램이 표시할 첫 번째 페이지를 인스턴스화하고 응용 프로그램 수명 주기 이벤트를 처리하는 역할을 담당합니다.
- **AssemblyInfo.cs** –이 파일에 어셈블리 수준에서 적용 되는 프로젝트에 대 한 응용 프로그램 특성을 포함 합니다.
- **NotesPage.xaml** – The XAML 태그에 대 한는 `NotesPage` 응용 프로그램이 시작 될 때 표시 되는 페이지의 UI를 정의 하는 클래스입니다.
- **NotesPage.xaml.cs** –에 대 한 코드 숨김의 `NotesPage` 페이지와 상호 작용할 때 실행 되는 비즈니스 논리를 포함 하는 클래스입니다.
- **NoteEntryPage.xaml** – The XAML 태그를 `NoteEntryPage` 메모를 입력할 때 표시 되는 페이지의 UI를 정의 하는 클래스입니다.
- **NoteEntryPage.xaml.cs** –에 대 한 코드 숨김의 `NoteEntryPage` 페이지와 상호 작용할 때 실행 되는 비즈니스 논리를 포함 하는 클래스입니다.

Xamarin.iOS 애플리케이션에 대한 자세한 내용은 [Xamarin.iOS 애플리케이션 분석](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application)을 참조하세요. Xamarin.Android 애플리케이션에 대한 자세한 내용은 [Xamarin.Android 애플리케이션 분석](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy)을 참조하세요.

## <a name="architecture-and-application-fundamentals"></a>아키텍처 및 응용 프로그램 기본 사항

Xamarin.Forms 애플리케이션은 기존의 플랫폼 간 애플리케이션과 같은 방식으로 설계됩니다. 공유 코드는 일반적으로 .NET 표준 라이브러리에 배치되고 플랫폼 관련 애플리케이션은 공유 코드를 사용합니다. 다음 다이어그램은 정보 응용 프로그램에 대 한이 관계의 개요를 보여줍니다.

::: zone pivot="windows"

![](deepdive-images/vs/architecture.png "정보 아키텍처")

::: zone-end
::: zone pivot="macos"

![](deepdive-images/vsmac/architecture.png "정보 아키텍처")

::: zone-end

다음 코드 예제와 같이, 시작 코드의 재사용을 최대화하기 위해 Xamarin.Forms 애플리케이션에는 각 플랫폼에서 애플리케이션이 표시할 첫 번째 페이지를 인스턴스화하는 역할을 담당하는 `App`이라는 단일 클래스가 있습니다.

```csharp
using Xamarin.Forms;

namespace Notes
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new NavigationPage(new NotesPage());
        }
        ...
    }
}
```

설정 하는이 코드는 `MainPage` 의 속성을 `App` 클래스를 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 내용이 인스턴스를 `NotesPage` 인스턴스.

또한 합니다 **AssemblyInfo.cs** 파일 어셈블리 수준에서 적용 되는 단일 응용 프로그램 특성을 포함 합니다.

```csharp
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
```

합니다 [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) XAML 중간 언어로 직접 컴파일될 수 있도록 특성 XAML 컴파일러를 켭니다. 자세한 내용은 [XAML 컴파일](~/xamarin-forms/xaml/xamlc.md)을 참조하세요.

## <a name="launching-the-application-on-each-platform"></a>각 플랫폼에서 응용 프로그램 시작

### <a name="ios"></a>iOS

IOS에서 초기 Xamarin.Forms 페이지를 시작 하려면 Notes.iOS 프로젝트는 다음과 같이 정의 됩니다. 합니다 `AppDelegate` 에서 상속 된 클래스는 `FormsApplicationDelegate` 클래스:

```csharp
namespace Notes.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init();
            LoadApplication(new App());
            return base.FinishedLaunching(app, options);
        }
    }
}
```

`FinishedLaunching` 재정의는 `Init` 메서드를 호출하여 Xamarin.Forms 프레임워크를 초기화합니다. 이렇게 하면 루트 뷰 컨트롤러가 `LoadApplication` 메서드에 대한 호출로 설정되기 전에 Xamarin.Forms의 iOS 특정 구현이 응용 프로그램에 로드됩니다.

### <a name="android"></a>Android

Android에서 초기 Xamarin.Forms 페이지를 시작 하려면 Notes.Android 프로젝트는 만드는 코드를 포함를 `Activity` 사용 하 여는 `MainLauncher` 에서 상속 하는 활동을 사용 하 여 특성을 `FormsAppCompatActivity` 클래스:

```csharp
namespace Notes.Droid
{
    [Activity(Label = "Notes",
              Icon = "@mipmap/icon",
              Theme = "@style/MainTheme",
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(savedInstanceState);
            global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
            LoadApplication(new App());
        }
    }
}
```

`OnCreate` 재정의는 `Init` 메서드를 호출하여 Xamarin.Forms 프레임워크를 초기화합니다. 이로 인해 Xamarin.Forms 애플리케이션이 로드되기 전에 Xamarin.Forms의 Android 특정 구현이 애플리케이션에 로드됩니다.

::: zone pivot="windows"

### <a name="universal-windows-platform"></a>UWP

UWP(유니버설 Windows 플랫폼) 애플리케이션에서 Xamarin.Forms 프레임워크를 초기화하는 `Init` 메서드가 `App` 클래스에서 호출됩니다.

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

이렇게 하면 Xamarin.Forms의 UWP 특정 구현이 애플리케이션에 로드됩니다. 초기 Xamarin.Forms 페이지에 의해 시작 되는 `MainPage` 클래스:

```csharp
namespace Notes.UWP
{
    public sealed partial class MainPage
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.LoadApplication(new Notes.App());
        }
    }
}
```

Xamarin.Forms 애플리케이션은 `LoadApplication` 메서드를 사용해 로드합니다.

> [!NOTE]
> 유니버설 Windows 플랫폼 앱만 될 수 있습니다 Xamarin.Forms를 사용 하 여 빌드된 Windows에서 Visual Studio를 사용 합니다.

::: zone-end

## <a name="user-interface"></a>사용자 인터페이스

Xamarin.Forms 응용 프로그램의 사용자 인터페이스를 만드는 데 사용 되는 4 개의 주 제어 그룹이 가지가 있습니다.

1. **페이지** – Xamarin.Forms 페이지는 플랫폼 간 모바일 애플리케이션 화면을 나타냅니다. 정보 응용 프로그램을 사용 합니다 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 단일 화면을 표시 하는 클래스입니다. 페이지에 대한 자세한 내용은 [Xamarin.Forms 페이지](~/xamarin-forms/user-interface/controls/pages.md)를 참조하세요.
1. **뷰** – Xamarin.Forms 뷰는 레이블, 버튼 및 텍스트 입력 상자 등의 사용자 인터페이스에 표시되는 컨트롤입니다. 완성된 된 정보 응용 프로그램을 사용 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView)를 [ `Editor` ](xref:Xamarin.Forms.Editor), 및 [ `Button` ](xref:Xamarin.Forms.Button) 뷰. 뷰에 대한 자세한 내용은 [Xamarin.Forms 뷰](~/xamarin-forms/user-interface/controls/views.md)를 참조하세요.
1. **레이아웃** – Xamarin.Forms 레이아웃은 뷰를 논리 구조로 구성하는 데 사용되는 컨테이너입니다. 정보 응용 프로그램을 사용 합니다 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 뷰를 세로로 정렬 하는 클래스 및 [ `Grid` ](xref:Xamarin.Forms.Grid) 단추를 가로로 정렬 하는 클래스입니다. 레이아웃에 대한 자세한 내용은 [Xamarin.Forms 레이아웃](~/xamarin-forms/user-interface/controls/layouts.md)을 참조하세요.
1. **셀** – Xamarin.Forms 셀은 목록에 있는 항목에 사용되는 특수한 요소이며, 목록의 각 항목이 어떻게 그려져야 하는지를 설명합니다. 정보 응용 프로그램을 사용 합니다 [ `TextCell` ](xref:Xamarin.Forms.TextCell) 목록에서 각 행에 대해 두 개의 항목을 표시 합니다. 셀에 대한 자세한 내용은 [Xamarin.Forms 셀](~/xamarin-forms/user-interface/controls/cells.md)을 참조하세요.

런타임 시 각 컨트롤은 렌더링될 해당 고유 장치에 매핑됩니다.

### <a name="layout"></a>레이아웃

정보 응용 프로그램을 사용 합니다 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 화면 크기에 관계 없이 화면에는 뷰를 자동으로 정렬 하 여 플랫폼 간 응용 프로그램 개발을 간소화 하기 위해. 각 자식 요소는 가로 또는 세로로 추가된 순서대로 차례로 위치가 지정됩니다. 얼마나 큰 공간을 `StackLayout`이 사용할지는 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 [`VerticalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 속성을 어떻게 설정했는지에 달려 있지만, 기본적으로는 `StackLayout`은 전체 화면을 사용합니다.

다음 XAML 코드를 사용 하 여 예제를 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 레이아웃에는 `NoteEntryPage`:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.NoteEntryPage"
             Title="Note Entry">
    ...    
    <StackLayout Margin="{StaticResource PageMargin}">
        <Editor Placeholder="Enter your note"
                Text="{Binding Text}"
                HeightRequest="100" />
        <Grid>
            ...
        </Grid>
    </StackLayout>    
</ContentPage>
```

기본적으로는 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 세로 방향으로 가정 합니다. 그러나 변경할 수 있습니다 가로 방향으로 설정 하 여 합니다 [ `StackLayout.Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) 속성을 합니다 [ `StackOrientation.Horizontal` ](xref:Xamarin.Forms.StackOrientation.Horizontal) 열거형 멤버입니다.

> [!NOTE]
> 보기의 크기를 통해 설정할 수 있습니다 합니다 `HeightRequest` 및 `WidthRequest` 속성입니다.

[`StackLayout`](xref:Xamarin.Forms.StackLayout) 클래스에 대한 자세한 내용은 [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)을 참조하세요.

### <a name="responding-to-user-interaction"></a>사용자 상호 작용에 응답

XAML에 정의된 개체는 코드 숨김 파일에서 처리되는 이벤트를 발생시킬 수 있습니다. 다음 코드 예제에서는 `OnSaveButtonClicked` 에 대 한 코드 숨김의 메서드를 `NoteEntryPage` 대 한 응답으로 실행 되는 클래스는 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) 대 한 이벤트가 발생 합니다 *저장* 단추 .

```csharp
async void OnSaveButtonClicked(object sender, EventArgs e)
{
    var note = (Note)BindingContext;
    note.Date = DateTime.UtcNow;
    await App.Database.SaveNoteAsync(note);
    await Navigation.PopAsync();
}
```

`OnSaveButtonClicked` 메서드는 데이터베이스에 메모를 저장 하 고 이전 페이지로 다시 이동 합니다.

> [!NOTE]
> XAML 클래스의 코드 숨김 파일은 `x:Name` 특성으로 할당된 이름을 사용하여 XAML에서 정의된 개체에 접근할 수 있습니다. 이 특성에 할당된 값은 C# 변수와 동일한 규칙을 가지며, 따라서 문자 또는 밑줄로 시작해야 하고 공백을 포함하면 안 됩니다.

저장의 연결 단추를 `OnSaveButtonClicked` 의 XAML 태그에서 메서드가 발생를 `NoteEntryPage` 클래스:

```xaml
<Button Text="Save"
        Clicked="OnSaveButtonClicked" />
```

### <a name="lists"></a>목록

합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 는 목록에서 항목의 컬렉션을 세로로 표시 하는 일을 담당 합니다. 각 항목에는 `ListView` 단일 셀에 포함 됩니다.

다음 코드 예제는 [ `ListView` ](xref:Xamarin.Forms.ListView) 에서 `NotesPage`:

```xaml
<ListView x:Name="listView"
          Margin="{StaticResource PageMargin}"
          ItemSelected="OnListViewItemSelected">
    <ListView.ItemTemplate>
        <DataTemplate>
            <TextCell Text="{Binding Text}"
                      Detail="{Binding Date}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

각 행의 레이아웃을 [ `ListView` ](xref:Xamarin.Forms.ListView) 내에 정의 되어는 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 요소 및 사용에 데이터 바인딩 응용 프로그램에서 검색 되는 모든 메모를 표시 합니다. 합니다 [ `ListView.ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 속성이 데이터 원본에 `NotesPage.xaml.cs`:

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    listView.ItemsSource = await App.Database.GetNotesAsync();
}
```    

이 코드는 [ `ListView` ](xref:Xamarin.Forms.ListView) 데이터베이스에 저장 된 모든 정보를 사용 하 여 합니다.

행을 선택 하는 경우는 [ `ListView` ](xref:Xamarin.Forms.ListView)서 [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트가 발생 합니다. 명명 된 이벤트 처리기를 `OnListViewItemSelected`, 이벤트가 발생할 때 실행 됩니다.

```csharp
async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        ...
    }
}
```

합니다 [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트를 통해 셀과 연결 된 개체에 액세스할 수 합니다 [ `e.SelectedItem` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) 속성입니다.

에 대 한 자세한 내용은 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 클래스를 참조 하십시오 [ListView](~/xamarin-forms/user-interface/listview/index.md)합니다.

## <a name="navigation"></a>탐색

Xamarin.Forms는 사용된 [`Page`](xref:Xamarin.Forms.Page) 형식에 따라 여러 다른 페이지 탐색 환경을 제공합니다. 에 대 한 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 계층적 또는 모달 인스턴스 탐색 될 수 있습니다. 모달 탐색에 대 한 정보를 참조 하세요 [Xamarin.Forms 모달 페이지](~/xamarin-forms/app-fundamentals/navigation/modal.md)합니다.

> [!NOTE]
> [`CarouselPage`](xref:Xamarin.Forms.CarouselPage), [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 및 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 클래스는 대체 탐색 환경을 제공합니다. 자세한 내용은 [탐색](~/xamarin-forms/app-fundamentals/navigation/index.md)을 참조하세요.

계층적 탐색에 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 클래스의 스택을 탐색 하는 데 사용 됩니다 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 앞으로 및 뒤로 원하는 대로 개체입니다. 이 모델은 탐색을 [`Page`](xref:Xamarin.Forms.Page) 개체의 LIFO(Last-In, First-Out, 후입선출) 스택으로 구현합니다. 한 페이지에서 다른 페이지로 이동하려면 애플리케이션은 새 페이지를 탐색 스택으로 푸시하여 활성 페이지가 되게 합니다. 이전 페이지로 돌아가기 위해 응용 프로그램은 탐색 스택에서 현재 페이지를 빼(pop)고 맨 위에 있는 새 페이지는 활성 페이지가 됩니다.

`NavigationPage`클래스는 또한 제목을 표시하는 페이지의 맨 위에 탐색 모음을 추가하고 이전 페이지로 돌아가게 하는 플랫폼에 적절한 **뒤로** 단추를 추가합니다. 

탐색 스택에 추가 된 첫 번째 페이지 라고 합니다 *루트* 응용 프로그램 및 다음 코드 예제에서는 페이지 정보 응용 프로그램에서이 작업을 수행 하는 방법을 보여 줍니다.

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new NotesPage ());
}
```

모든 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스에는 페이지 스택을 수정하기 위해 메서드를 노출하는 [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성이 있습니다. 이들 메서드는 애플리케이션에 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)가 포함되는 경우에만 호출해야합니다. `NoteEntryPage`로 이동하려면 아래 코드 예제에서 설명한 것처럼 [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) 메서드를 호출해야 합니다.

```csharp
await Navigation.PushAsync(new NoteEntryPage());
```

그러면 새 `NoteEntryPage` 개체를 탐색 스택으로 푸시할 수 있는 활성 페이지가 됩니다.

활성 페이지는 장치의 *뒤로* 단추를 눌러 탐색 스택에서 뺄(pop) 수 있습니다. 이때 단추는 장치의 물리적 단추든 화면상 단추든 상관없습니다. 프로그래밍 방식으로 원래 페이지로 돌아가려면 `NoteEntryPage` 개체는 아래 코드 예제에서 설명한 것처럼 [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) 메서드를 호출해야 합니다.

```csharp
await Navigation.PopAsync();
```

계층적 탐색에 대한 자세한 내용은 [계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)을 참조하세요.

## <a name="data-binding"></a>데이터 바인딩

데이터 바인딩은 Xamarin.Forms 애플리케이션이 데이터를 나타내고 해당 데이터와 상호 작용하는 방법을 단순화하기 위해 사용됩니다. 그것을 통해 사용자 인터페이스와 기본 애플리케이션 간에 연결이 설정됩니다. [`BindableObject`](xref:Xamarin.Forms.BindableObject) 클래스에는 데이터 바인딩을 지원하는 인프라의 대부분이 담겨 있습니다.

데이터 바인딩은 *source* 및 *target*이라는 두 개의 개체를 연결합니다. *source* 개체는 데이터를 제공합니다. *target* 개체는 원본 개체의 데이터를 사용(하고 종종 표시)합니다. 예를 들어, 한 [ `Editor` ](xref:Xamarin.Forms.Editor) (*대상* 개체)는 일반적으로 바인딩합니다 해당 [ `Text` ](xref:Xamarin.Forms.Editor.Text) 속성을 공용 `string` 속성에는 *원본* 개체입니다. 다음 다이어그램은 바인딩 관계를 보여 줍니다.

![](deepdive-images/data-binding.png "데이터 바인딩")

데이터 바인딩의 주요 장점은 더 이상 뷰와 데이터 원본 간의 데이터 동기화에 대해 걱정할 필요가 없다는 것입니다. *source* 개체의 변경 사항은 바인딩 프레임워크가 화면 뒤에서 *target* 개체에 자동으로 푸시하며, 대상(target) 개체의 변경 내용은 필요한 경우 *source* 개체로 다시 푸시될 수 있습니다.

데이터 바인딩 설정은 이루어집니다.

- *target* 개체의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 속성은 *source*에 설정되어야 합니다.
- 바인딩이 *source*와 *target* 간에 설정되어야 합니다. XAML의 경우 [`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension) 태그 확장을 사용하여 이루어집니다.

바인딩 대상의 정보 응용 프로그램에는 [ `Editor` ](xref:Xamarin.Forms.Editor) 메모를 표시 하는 동안를 `Note` 인스턴스로는 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 의 `NoteEntryPage` 바인딩 소스입니다.

합니다 `BindingContext` 의 `NoteEntryPage` 다음 코드 예제 에서처럼 페이지 탐색 하는 동안 설정 됩니다.

```csharp
async void OnNoteAddedClicked(object sender, EventArgs e)
{
    await Navigation.PushAsync(new NoteEntryPage
    {
        BindingContext = new Note()
    });
}

async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        await Navigation.PushAsync(new NoteEntryPage
        {
            BindingContext = e.SelectedItem as Note
        });
    }
}
```

에 `OnNoteAddedClicked` 응용 프로그램에 새 메모 추가 될 때 실행 되는 메서드를를 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 의 `NoteEntryPage` 새로 설정 된 `Note` 인스턴스. 에 `OnListViewItemSelected` 에서 기존 메모를 선택할 때 실행 되는 메서드를를 [ `ListView` ](xref:Xamarin.Forms.ListView)의 `BindingContext` 의 `NoteEntryPage` 선택한로 설정 되어 `Note` 인스턴스를 통해 액세스할 수 있는 [ `e.SelectedItem` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) 속성입니다.

> [!IMPORTANT]
> 각 *target* 개체의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 속성은 개별적으로 설정할 수 있으나, 그럴 필요는 없습니다. `BindingContext` 모든 자식에서 상속 되는 특별 한 속성이입니다. 따라서 때를 `BindingContext` 에 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 로 설정 됩니다는 `Note` 자식의 모든 인스턴스를 `ContentPage` 동일 `BindingContext`, 합니다 의공용속성에바인딩될수있습니다`Note`개체입니다.

[ `Editor` ](xref:Xamarin.Forms.Editor) 에서 `NoteEntryPage` 에 바인딩됩니다 합니다 `Text` 의 속성을 `Note` 개체:

```xaml
<Editor Placeholder="Enter your note"
        Text="{Binding Text}"
        ... />
```

*source* 개체의 [`Editor.Text`](xref:Xamarin.Forms.Editor.Text) 속성과 `Text` 속성 간의 바인딩이 설정됩니다. 변경 합니다 `Editor` 자동으로 전파 됩니다는 `Note` 개체입니다. 마찬가지로, 변경 사항을 적용 하는 경우는 `Note.Text` 속성인 Xamarin.Forms 바인딩 엔진은 내용도 업데이트의 `Editor`합니다. 이것을 *양방향(two-way) 바인딩*이라고 합니다.

데이터 바인딩에 대한 자세한 내용은 [Xamarin.Forms 데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)을 참조하세요.

## <a name="styling"></a>스타일 지정

Xamarin.Forms 응용 프로그램은 종종 동일한 모양이 여러 시각적 요소를 포함 합니다. 반복 될 수 있습니다 각 시각적 요소의 모양을 설정 하 고 오류가 발생 하기 쉽습니다. 대신, 스타일 만들 수 있습니다는 모양을 정의 하 고 그런 다음 필요한 시각적 요소에 적용 합니다.

합니다 [ `Style` ](xref:Xamarin.Forms.Style) 다음 여러 시각적 요소 인스턴스에 적용할 수 있는 하나의 개체에 속성 값의 컬렉션을 그룹화 하는 클래스입니다. 스타일에 저장 되는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), 응용 프로그램 수준, 페이지 수준 또는 수준 보기에서. 정의할 위치를 선택는 `Style` 사용할 수 있는 영향을 줍니다.

- [`Style`](xref:Xamarin.Forms.Style) 응용 프로그램 수준에서 정의 된 인스턴스는 응용 프로그램에 걸쳐 적용할 수 있습니다.
- [`Style`](xref:Xamarin.Forms.Style) 페이지 수준에서 정의 된 인스턴스 및 해당 자식 페이지에 적용할 수 있습니다.
- [`Style`](xref:Xamarin.Forms.Style) 뷰 수준에서 정의 된 인스턴스 및 해당 자식 뷰에 적용할 수 있습니다.

> [!IMPORTANT]
> 응용 프로그램 전체에서 사용 되는 모든 스타일 중복을 피하기 위해 응용 프로그램의 리소스 사전에 저장 됩니다. 그러나 XAML 페이지에 관련 된 리소스는 구문 분석 대신 응용 프로그램 시작 시 페이지에서 필요한 경우에 따라 응용 프로그램의 리소스 사전에 포함 해서는 안 됩니다.

각 [ `Style` ](xref:Xamarin.Forms.Style) 하나 이상의 컬렉션을 포함 하는 인스턴스 [ `Setter` ](xref:Xamarin.Forms.Setter) 개체와 각 `Setter` 필요는 [ `Property` ](xref:Xamarin.Forms.Setter.Property) 및 [`Value`](xref:Xamarin.Forms.Setter.Value). 합니다 `Property` 스타일에 적용 되는 요소의 바인딩 가능한 속성 이름 및 `Value` 속성에 적용 되는 값입니다. 다음 코드 예제에서 스타일을 보여 줍니다. `NoteEntryPage`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.NoteEntryPage"
             Title="Note Entry">
    <ContentPage.Resources>
        <!-- Implicit styles -->
        <Style TargetType="{x:Type Editor}">
            <Setter Property="BackgroundColor"
                    Value="{StaticResource AppBackgroundColor}" />
        </Style>
        ...
    </ContentPage.Resources>
    ...
</ContentPage>
```

이 스타일에 적용 됩니다 [ `Editor` ](xref:Xamarin.Forms.Editor) 페이지에서 인스턴스.

만들 때를 [ `Style` ](xref:Xamarin.Forms.Style)서 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) 속성은 항상 필요 합니다.

> [!NOTE]
> Xamarin.Forms 응용 프로그램 스타일 지정 XAML 스타일을 사용 하 여 일반적으로 수행 됩니다. 그러나 Xamarin.Forms는 또한 다음과 같은 스타일 (CSS 스타일 시트)를 사용 하는 시각적 요소를 지원 합니다. 자세한 내용은 [(CSS 스타일 시트)를 사용 하 여 스타일 지정 Xamarin.Forms 앱](~/xamarin-forms/user-interface/styles/css/index.md)합니다.

XAML 스타일에 대 한 자세한 내용은 참조 하세요. [XAML 스타일을 사용 하 여 Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/xaml/index.md)합니다.

### <a name="providing-platform-specific-styles"></a>플랫폼 특정 스타일을 제공합니다.

`OnPlatform` 태그 확장을 사용 하면 플랫폼별 기준 UI 모양을 사용자 지정할 수 있도록 합니다.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.App">
    <Application.Resources>
        ...
        <Color x:Key="iOSNavigationBarColor">WhiteSmoke</Color>
        <Color x:Key="AndroidNavigationBarColor">#2196F3</Color>
        <Color x:Key="iOSNavigationBarTextColor">Black</Color>
        <Color x:Key="AndroidNavigationBarTextColor">White</Color>

        <Style TargetType="{x:Type NavigationPage}">
            <Setter Property="BarBackgroundColor"
                    Value="{OnPlatform iOS={StaticResource iOSNavigationBarColor},
                                       Android={StaticResource AndroidNavigationBarColor}}" />
             <Setter Property="BarTextColor"
                    Value="{OnPlatform iOS={StaticResource iOSNavigationBarTextColor},
                                       Android={StaticResource AndroidNavigationBarTextColor}}" />           
        </Style>
        ...
    </Application.Resources>
</Application>
```

이 [ `Style` ](xref:Xamarin.Forms.Style) 다양 한 설정 [ `Color` ](xref:Xamarin.Forms.Color) 에 대 한 값을 [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) 하 고 [ `BarTextColor` ](xref:Xamarin.Forms.NavigationPage.BarTextColor) 속성을 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)사용 중인 플랫폼에 따라 합니다.

XAML 태그 확장에 대한 자세한 내용은 [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md)을 참조하세요. 에 대 한 자세한 합니다 `OnPlatform` 태그 확장 참조 [OnPlatform 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)합니다.

## <a name="testing-and-deployment"></a>테스트 및 배포

Visual Studio와 Mac용 Visual Studio는 응용 프로그램을 테스트하고 배포하기 위한 다양한 옵션을 제공합니다. 애플리케이션 디버그는 애플리케이션 개발 주기에서 일반적인 과정이며 코드 문제를 진단하는 데 도움이 됩니다. 자세한 내용은 [중단점 설정](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [단계별 코드 실행](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code) 및 [로그 창에 정보 출력](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)을 참조하세요.

시뮬레이터는 응용 프로그램 배포 및 테스트를 시작하기에 좋은 곳이며, 응용 프로그램을 테스트하는 유용한 기능을 제공합니다. 그러나 사용자가 최종 응용 프로그램을 시뮬레이터에서 사용하지는 않으므로, 초기에 자주 실제 장치에서 응용 프로그램을 테스트해야 합니다. iOS 디바이스 프로비전에 대한 자세한 내용은 [디바이스 프로비전](~/ios/get-started/installation/device-provisioning/index.md)을 참조하세요. Android 디바이스 프로비전에 대한 자세한 내용은 [개발용 디바이스 설정](~/android/get-started/installation/set-up-device-for-development.md)을 참조하세요.

## <a name="next-steps"></a>다음 단계

이 심층 분석에 Xamarin.Forms를 사용 하 여 응용 프로그램 개발의 기본적인 사항을 살펴보았습니다. 제안된 다음 단계로는 다음 기능에 대한 내용이 있습니다.

- Xamarin.Forms 애플리케이션의 사용자 인터페이스를 만드는 데 사용되는 4개의 주된 제어 그룹이 있습니다. 자세한 내용은 [컨트롤 참조](~/xamarin-forms/user-interface/controls/index.md)를 참조하세요.
- 데이터 바인딩은 두 개체의 속성을 연결하여 한 속성의 변경 내용이 다른 속성에 자동으로 반영되도록 하는 기술입니다. 자세한 내용은 [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)을 참조하세요.
- Xamarin.Forms는 사용되는 페이지 형식에 따라 다양한 페이지 탐색 환경을 제공합니다. 자세한 내용은 [탐색](~/xamarin-forms/app-fundamentals/navigation/index.md)을 참조하세요.
- 스타일을 통해 반복 태그를 줄이고, 애플리케이션 모양을 쉽게 변경할 수 있습니다. 자세한 내용은 [Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/index.md)을 참조하세요.
- XAML 태그 확장은 요소 특성을 리터럴 텍스트 문자열 이외의 원본으로 설정할 수 있도록 하여 XAML의 성능과 유연성을 확장합니다. 자세한 내용은 [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md)을 참조하세요.
- 데이터 템플릿은 지원되는 보기에서 데이터 표현을 정의하는 기능을 제공합니다. 자세한 내용은 [데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.
- 각 페이지, 레이아웃 및 보기는 차례로 네이티브 컨트롤을 만들고, 화면에 정렬하고, 공유 코드에 지정된 동작을 추가하는 `Renderer` 클래스를 사용하여 각 플랫폼에서 다르게 렌더링됩니다. 개발자는 컨트롤의 모양 및/또는 동작을 사용자 지정하기 위해 자신 만의 사용자 지정 `Renderer` 클래스를 구현할 수 있습니다. 자세한 내용은 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)를 참조하세요.
- 각 플랫폼의 네이티브 컨트롤을 사용자 지정할 수 있는 효과가 있습니다. 효과의 경우, 플랫폼별 프로젝트에서 [`PlatformEffect`](xref:Xamarin.Forms.PlatformEffect`2) 클래스를 하위 클래스로 지정하여 만들어지고, 적절한 Xamarin.Forms 컨트롤에 연결하여 사용됩니다. 자세한 내용은 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)를 참조하세요.
- 공유 코드는 [`DependencyService`](xref:Xamarin.Forms.DependencyService) 클래스를 통해 네이티브 기능에 액세스할 수 있습니다. 자세한 내용은 [DependencyService를 사용한 네이티브 기능 액세스](~/xamarin-forms/app-fundamentals/dependency-service/index.md)를 참조하세요.

또는 Charles Petzold의 책인 [_Creating Mobile Apps with Xamarin.Forms_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)(Xamarin.Forms를 사용하여 모바일 앱 만들기)는 Xamarin.Forms에 대해 더 배울 수 있는 좋은 자료입니다. 이 책은 PDF 또는 다양한 전자책 형식으로 제공됩니다.

## <a name="related-links"></a>관련 링크

- [XAML(eXtensible Application Markup Language)](~/xamarin-forms/xaml/index.md)
- [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [컨트롤 참조](~/xamarin-forms/user-interface/controls/index.md)
- [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [시작 샘플](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/)
- [Xamarin.Forms API 참조](xref:Xamarin.Forms)
- [무료 사용자 진행 방식 학습 (비디오)](https://university.xamarin.com/self-guided/)
