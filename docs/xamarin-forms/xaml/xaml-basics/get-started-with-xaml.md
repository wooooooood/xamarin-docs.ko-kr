---
title: 1부. XAML 시작
description: Xamarin.Forms응용 프로그램에서 XAML은 주로 페이지의 시각적 콘텐츠를 정의 하는 데 사용 되며 코드 숨김이 포함 된 파일과 함께 작동 합니다.
ms.prod: xamarin
ms.assetid: 9073FA0E-BD5A-4492-8A93-54C466F6EDB9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/30/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e38080fc9bc4ef0b74eb8c12c3a3f646c4888f53
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198072"
---
# <a name="part-1-getting-started-with-xaml"></a>1부. XAML 시작

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_Xamarin.Forms응용 프로그램에서 XAML은 주로 페이지의 시각적 콘텐츠를 정의 하는 데 사용 되며 c # 코드를 사용 하는 파일과 함께 작동 합니다._

코드를 표시 하는 파일은 태그에 대 한 코드를 지원 합니다. 이러한 두 파일은 모두 자식 뷰와 속성 초기화를 포함 하는 새 클래스 정의에 적용 됩니다. XAML 파일 내에서 클래스와 속성은 XML 요소 및 특성을 사용 하 여 참조 되 고 태그와 코드 간의 링크가 설정 됩니다.

## <a name="creating-the-solution"></a>솔루션 만들기

첫 번째 XAML 파일 편집을 시작 하려면 Visual Studio 또는 Mac용 Visual Studio를 사용 하 여 새 Xamarin.Forms 솔루션을 만듭니다. (아래에서 사용자 환경에 해당 하는 탭을 선택 합니다.)

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Windows에서 Visual Studio 2019를 시작 하 고, 시작 창에서 **새 프로젝트 만들기** 를 클릭 하 여 새 프로젝트를 만듭니다.

![새 솔루션 창](get-started-with-xaml-images/win/new-solution-2019.png)

**새 프로젝트 만들기** 창에서 **프로젝트 형식** 드롭다운의 **모바일**을 선택한 다음 **모바일 앱(Xamarin.Forms)** 템플릿을 선택하고 **다음** 단추를 클릭합니다.

![새 프로젝트 창](get-started-with-xaml-images/win/new-project-2019.png)

**새 프로젝트 구성** 창에서 **프로젝트 이름을** **xamlsamples** (또는 원하는 경우)로 설정 하 고 **만들기** 단추를 클릭 합니다.

**새 플랫폼 간 앱** 대화 상자에서 **비어 있음**을 클릭 하 고 **확인** 단추를 클릭 합니다.

![새 응용 프로그램 대화 상자](get-started-with-xaml-images/win/new-cross-platform-app.png)

솔루션에서 4 개의 프로젝트가 생성 됩니다. **xamlsamples** .NET Standard library, Xamlsamples. **Android** **, Xamlsamples**및 유니버설 Windows 플랫폼 솔루션인 **xamlsamples**.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

Mac용 Visual Studio의 메뉴에서 **파일 > 새 솔루션** 을 선택 합니다. **새 프로젝트** 대화 상자에서 왼쪽에 있는 **다중 플랫폼 > 앱** 을 선택 하 고 템플릿 목록에서*not* **폼 앱**이 아닌 **빈 양식 앱** 을 선택 합니다.

![새 프로젝트 대화 상자 1](get-started-with-xaml-images/mac/newprojectdialog1.png)

**다음**을 누릅니다.

다음 대화 상자에서 프로젝트에 **Xamlsamples** (또는 원하는 대로)의 이름을 지정 합니다. **.NET Standard 사용** 라디오 단추가 선택 되어 있는지 확인 합니다.

![새 프로젝트 대화 상자 2](get-started-with-xaml-images/mac/newprojectdialog2.png)

**다음**을 누릅니다.

다음 대화 상자에서 프로젝트 위치를 선택할 수 있습니다.

![새 프로젝트 대화 상자 3](get-started-with-xaml-images/mac/newprojectdialog3.png)

**만들기** 누르기

솔루션에는 **xamlsamples** .NET Standard Library, **Xamlsamples. Android**및 **xamlsamples. iOS**의 세 가지 프로젝트가 생성 됩니다.

-----

**Xamlsamples** 솔루션을 만든 후에는 솔루션 시작 프로젝트로 다양 한 플랫폼 프로젝트를 선택 하 여 개발 환경을 테스트 하 고, 휴대폰 에뮬레이터 또는 실제 장치에서 프로젝트 템플릿으로 만든 간단한 응용 프로그램을 빌드 및 배포 하는 것이 좋습니다.

플랫폼별 코드를 작성 해야 하는 경우가 아니면 라이브러리 프로젝트 .NET Standard 공유 **Xamlsamples** 는 거의 모든 프로그래밍 시간을 지출 하 게 됩니다. 이러한 문서는 해당 프로젝트 외부에서 다루지 않습니다.

### <a name="anatomy-of-a-xaml-file"></a>XAML 파일 분석

**Xamlsamples** .NET Standard 라이브러리에는 다음 이름의 파일 쌍이 있습니다.

- **App.xaml, xaml**파일 하거나
- **App.xaml.cs**-xaml 파일과 연결 된 c # *코드 숨겨진* 파일입니다.

코드 숨겨진 파일을 보려면 **app.xaml** 옆의 화살표를 클릭 해야 합니다.

**App.xaml** 과 **App.xaml.cs** `App` 는 모두에서 파생 되는 라는 클래스에 기여 `Application` 합니다. XAML 파일을 사용 하는 대부분의 다른 클래스는에서 파생 되는 클래스에 기여 합니다 `ContentPage` . 이러한 파일은 xaml을 사용 하 여 전체 페이지의 시각적 콘텐츠를 정의 합니다. 다음은 **Xamlsamples** 프로젝트의 다른 두 파일에 적용 되는 것입니다.

- **Mainpage**(xaml 파일) 하거나
- **MainPage.xaml.cs**, c # 코드 숨겨진 파일입니다.

**Mainpage** 파일은 다음과 같습니다 (형식이 약간 다를 수 있음).

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage">

    <StackLayout>
        <!-- Place new controls here -->
        <Label Text="Welcome to Xamarin Forms!"
               VerticalOptions="Center"
               HorizontalOptions="Center" />
    </StackLayout>

</ContentPage>
```

두 XML 네임 스페이스 ( `xmlns` ) 선언은 uri를 참조 하 고, 첫 번째는 Xamarin의 웹 사이트에, 두 번째는 Microsoft의에 있습니다. 해당 Uri가 가리키는 내용을 확인 하지 마십시오. 아무것도 없습니다. 이는 Xamarin 및 Microsoft가 소유 하는 Uri 이며 기본적으로 버전 식별자로 작동 합니다.

첫 번째 XML 네임 스페이스 선언은 접두사가 없는 XAML 파일 내에 정의 된 태그가의 클래스를 참조 함을 의미 Xamarin.Forms 합니다 (예:) `ContentPage` . 두 번째 네임 스페이스 선언은 접두사를 정의 합니다 `x` . 이는 XAML 자체에 내장 되어 있고 다른 XAML 구현에서 지원 되는 여러 요소 및 특성에 사용 됩니다. 그러나 이러한 요소와 특성은 URI에 포함 된 연도에 따라 약간 다릅니다. Xamarin.Forms는 2009 XAML 사양을 지원 하지만 일부는 지원 하지 않습니다.

`local`네임 스페이스 선언을 사용 하면 .NET Standard library 프로젝트에서 다른 클래스에 액세스할 수 있습니다.

첫 번째 태그의 끝에서 `x` 접두사는 이라는 특성에 사용 됩니다 `Class` . 이 접두사를 사용 하는 `x` 것은 xaml 네임 스페이스의 경우 사실상 유니버설 이므로와 같은 xaml 특성 `Class` 은 거의 항상로 참조 됩니다 `x:Class` .

특성은 정규화 `x:Class` 된 .net 클래스 이름 ( `MainPage` 네임 스페이스의 클래스)을 지정 합니다. `XamlSamples` 즉,이 XAML 파일은 `MainPage` `XamlSamples` 에서 파생 되는 네임 스페이스에 이라는 새 클래스 `ContentPage` (특성이 표시 되는 태그)를 정의 합니다 `x:Class` .

`x:Class`특성은 파생 된 c # 클래스를 정의 하기 위해 XAML 파일의 루트 요소에만 나타날 수 있습니다. 이는 XAML 파일에 정의 된 유일한 새 클래스입니다. XAML 파일에 표시 되는 다른 모든 항목은 대신 기존 클래스에서 인스턴스화되고 초기화 됩니다.

**MainPage.xaml.cs** 파일은 사용 하지 않는 지시문을 제외 하 고 다음과 같습니다 `using` .

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
    }
}
```

`MainPage`클래스는에서 파생 `ContentPage` 되지만 클래스 정의를 확인 합니다 `partial` . 이는에 대 한 다른 partial 클래스 정의가 있어야 `MainPage` 하지만 어디에 있나요? 무엇 `InitializeComponent` 인가요?

Visual Studio는 프로젝트를 빌드할 때 c # 코드 파일을 생성 하도록 XAML 파일을 구문 분석 합니다. **XamlSamples\XamlSamples\obj\Debug** 디렉터리를 살펴보면 이름이 **XamlSamples.MainPage.xaml.g.cs**인 파일이 검색 됩니다. ' G '는 생성 된를 나타냅니다. 이는 `MainPage` 생성자에서 호출 된 메서드의 정의를 포함 하는의 다른 partial 클래스 정의입니다 `InitializeComponent` `MainPage` . 이러한 두 partial `MainPage` 클래스 정의를 함께 컴파일할 수 있습니다. Xaml이 컴파일 되었는지 여부에 따라 xaml 파일이 나 xaml 파일의 이진 형식이 실행 파일에 포함 됩니다.

런타임에 특정 플랫폼 프로젝트의 코드는 메서드를 호출 하 여 `LoadApplication` `App` .NET Standard 라이브러리에 있는 클래스의 새 인스턴스를 전달 합니다. `App`클래스 생성자는를 인스턴스화합니다 `MainPage` . 이 클래스의 생성자는를 호출 하 여 `InitializeComponent` `LoadFromXaml` .NET Standard 라이브러리에서 XAML 파일 (또는 컴파일된 이진)을 추출 하는 메서드를 호출 합니다. `LoadFromXaml`XAML 파일에 정의 된 모든 개체를 초기화 하 고, 모두 부모-자식 관계에서 연결 하 고, 코드에 정의 된 이벤트 처리기를 XAML 파일에 설정 된 이벤트에 연결 하 고, 개체의 결과 트리를 페이지의 콘텐츠로 설정 합니다.

일반적으로 생성 된 코드 파일을 사용 하 여 많은 시간을 소비할 필요는 없지만 생성 된 파일의 코드에서 런타임 예외가 발생 하는 경우가 있으므로이에 대해 잘 알고 있어야 합니다.

이 프로그램을 컴파일하고 실행 하면 XAML에서 제안 하는 `Label` 대로 요소가 페이지의 가운데에 표시 됩니다.

[![기본 Xamarin.Forms 표시](get-started-with-xaml-images/xamlsamples.png)](get-started-with-xaml-images/xamlsamples-large.png#lightbox)

더 흥미로운 시각적 개체의 경우 더 흥미로운 XAML이 필요 합니다.

## <a name="adding-new-xaml-pages"></a>새 XAML 페이지 추가

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

다른 XAML 기반 `ContentPage` 클래스를 프로젝트에 추가 하려면 **xamlsamples** .NET Standard library 프로젝트를 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음 **> 새 항목 추가**...를 선택 합니다. **새 항목 추가** 대화 상자에서 **Visual c # 항목 > Xamarin.Forms > 콘텐츠 페이지** (콘텐츠 **페이지 (c #)** 가 아닌 코드 전용 페이지 또는 **콘텐츠 뷰**)를 선택 합니다. 페이지에 이름을 지정 합니다 (예: **HelloXamlPage**).

![새 항목 추가 대화 상자](get-started-with-xaml-images/win/add-new-item-dialog-2019.png)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

다른 XAML 기반 `ContentPage` 클래스를 프로젝트에 추가 하려면 **xamlsamples** .NET Standard library 프로젝트를 선택 하 고 **파일 > 새 파일** 메뉴 항목을 호출 합니다. **새 파일** 대화 상자 왼쪽의 왼쪽에서 **폼** 을 선택 하 고, 폼 **contentpage Xaml** (코드 전용 페이지를 만드는 **폼 콘텐츠 페이지**또는 페이지가 아닌 **콘텐츠 뷰**)을 선택 합니다. 페이지에 이름을 지정 합니다 (예: **HelloXamlPage**).

![새 파일 대화 상자](get-started-with-xaml-images/mac/newfiledialog.png)

-----

프로젝트에는 두 개의 파일 ( **HelloXamlPage** 및 코드 숨김이 파일 **HelloXamlPage.xaml.cs**)이 추가 됩니다.

## <a name="setting-page-content"></a>페이지 콘텐츠 설정

**HelloXamlPage** 파일을 편집 하 여 및에 대 한 태그만 갖도록 합니다 `ContentPage` `ContentPage.Content` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage">
    <ContentPage.Content>

    </ContentPage.Content>
</ContentPage>
```

`ContentPage.Content`태그는 고유한 XAML 구문의 일부입니다. 처음에는 잘못 된 XML로 표시 될 수 있지만 올바른 XML로 표시 될 수 있습니다. 마침표가 XML에서 특수 문자가 아닙니다.

`ContentPage.Content`태그를 *속성 요소* 태그 라고 합니다. `Content`는의 속성 이며 `ContentPage` 일반적으로 단일 뷰나 자식 뷰가 있는 레이아웃으로 설정 됩니다. 일반적으로 속성은 XAML에서 특성이 되지만 특성을 복합 개체로 설정 하기가 어렵습니다 `Content` . 이러한 이유로 속성은 마침표로 구분 된 클래스 이름 및 속성 이름으로 구성 된 XML 요소로 표현 됩니다. 이제 `Content` 다음과 같이 태그 사이에 속성을 설정할 수 있습니다 `ContentPage.Content` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage"
             Title="Hello XAML Page">
    <ContentPage.Content>

        <Label Text="Hello, XAML!"
               VerticalOptions="Center"
               HorizontalTextAlignment="Center"
               Rotation="-15"
               IsVisible="true"
               FontSize="Large"
               FontAttributes="Bold"
               TextColor="Blue" />

    </ContentPage.Content>
</ContentPage>
```

또한 `Title` 루트 태그에 특성이 설정 되어 있는지 확인 합니다.

이 때 클래스, 속성 및 XML 간의 관계는 명확 해야 합니다. Xamarin.Forms 클래스 (예: `ContentPage` 또는 `Label` )는 XAML 파일에 XML 요소로 표시 됩니다. 설정 및 7 개의 속성을 포함 하 여 해당 클래스의 속성 `Title` `ContentPage` `Label` 은 일반적으로 XML 특성으로 표시 됩니다.

이러한 속성의 값을 설정 하는 데 많은 단축키가 있습니다. 일부 속성은 기본 데이터 형식입니다. 예를 들어 및 속성은 형식이 고, `Title` `Text` 는 형식이 며, `String` `Rotation` `Double` `IsVisible` `true` 기본적으로이 고 여기서는 그림에만 설정 됨은 형식입니다 `Boolean` .

`HorizontalTextAlignment`속성은 열거형 인 형식입니다 `TextAlignment` . 열거형 형식의 속성의 경우 모든 사용자에 게 멤버 이름을 제공 해야 합니다.

그러나 더 복잡 한 형식의 속성에 대 한 변환기는 XAML을 구문 분석 하는 데 사용 됩니다. 이러한 클래스는에서 Xamarin.Forms 파생 되는의 클래스입니다 `TypeConverter` . 대부분은 public 클래스 이지만 일부는 그렇지 않습니다. 이 특정 XAML 파일의 경우 이러한 클래스 중 일부는 내부적으로 역할을 수행 합니다.

- `LayoutOptionsConverter`속성의 경우 `VerticalOptions`
- `FontSizeConverter`속성의 경우 `FontSize`
- `ColorTypeConverter`속성의 경우 `TextColor`

이러한 변환기는 속성 설정의 허용 되는 구문을 제어 합니다.

는 `ThicknessTypeConverter` 쉼표를 구분 하 여 한 개, 두 개 또는 네 개의 숫자를 처리할 수 있습니다. 한 번호를 제공 하는 경우 네 면 모두에 적용 됩니다. 두 숫자를 사용 하는 경우 첫 번째 값은 왼쪽과 오른쪽 안쪽 여백이 고 두 번째 숫자는 top 및 bottom입니다. 네 개의 숫자는 왼쪽, 위쪽, 오른쪽 및 아래쪽 순서로 정렬 됩니다.

는 `LayoutOptionsConverter` 구조체의 public 정적 필드 이름을 `LayoutOptions` 형식의 값으로 변환할 수 있습니다 `LayoutOptions` .

는 `FontSizeConverter` `NamedSize` 멤버 또는 숫자 글꼴 크기를 처리할 수 있습니다.

는 `ColorTypeConverter` `Color` 숫자 기호 (#) 뒤에 알파 채널을 사용 하거나 사용 하지 않고 구조체 또는 16 진수 RGB 값의 공용 정적 필드 이름을 허용 합니다. 알파 채널이 없는 구문은 다음과 같습니다.

 `TextColor="#rrggbb"`

각 문자는 16 진수입니다. 알파 채널을 포함 하는 방법은 다음과 같습니다.

 `TextColor="#aarrggbb">`

알파 채널의 경우 FF는 완전히 불투명 하 고 00은 완전히 투명 함을 명심 해야 합니다.

두 가지 다른 형식을 사용 하면 각 채널에 대해 단일 16 진수를 지정할 수 있습니다.

 `TextColor="#rgb"` `TextColor="#argb"`

이 경우 숫자를 반복 하 여 값을 구성 합니다. 예를 들어 #CF3는 RGB 색 CC-FF-33입니다.

## <a name="page-navigation"></a>페이지 탐색

**Xamlsamples** 프로그램을 실행 하면 `MainPage` 이 표시 됩니다. 새를 표시 하려면 `HelloXamlPage` **App.xaml.cs** 파일에서 새 시작 페이지로 설정 하거나에서 새 페이지로 이동 합니다 `MainPage` .

탐색을 구현 하려면 먼저 **App.xaml.cs** 생성자에서 코드를 변경 하 여 `NavigationPage` 개체를 만듭니다.

```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

**MainPage.xaml.cs** 생성자에서 간단 하 게 만들고 `Button` 이벤트 처리기를 사용 하 여로 이동할 수 있습니다 `HelloXamlPage` .

```csharp
public MainPage()
{
    InitializeComponent();

    Button button = new Button
    {
        Text = "Navigate!",
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };

    button.Clicked += async (sender, args) =>
    {
        await Navigation.PushAsync(new HelloXamlPage());
    };

    Content = button;
}
```

`Content`페이지의 속성을 설정 하면 `Content` XAML 파일의 속성 설정이 대체 됩니다. 이 프로그램의 새 버전을 컴파일하고 배포할 때 화면에 단추가 표시 됩니다. 키를 누르면로 이동 `HelloXamlPage` 합니다. IPhone, Android 및 UWP의 결과 페이지는 다음과 같습니다.

[![회전 된 레이블 텍스트](get-started-with-xaml-images/helloxaml1.png)](get-started-with-xaml-images/helloxaml1-large.png#lightbox)

`MainPage`IOS에서 **< 뒤로** 단추를 사용 하거나, 페이지 맨 위에 있는 왼쪽 화살표를 사용 하거나, Android의 휴대폰 아래쪽에 있는 왼쪽 화살표를 사용 하거나, Windows 10의 페이지 맨 위에 있는 왼쪽 화살표를 사용 하 여 다시 이동할 수 있습니다.

을 렌더링 하는 다양 한 방법에 대해 XAML을 자유롭게 시험해 보세요 `Label` . 텍스트에 유니코드 문자를 포함 해야 하는 경우 표준 XML 구문을 사용할 수 있습니다. 예를 들어 인사말을 스마트 부호로 넣으려면 다음을 사용 합니다.

 `<Label Text="&#x201C;Hello, XAML!&#x201D;" … />`

모양은 다음과 같습니다.

[![유니코드 문자를 사용 하 여 회전 된 레이블 텍스트](get-started-with-xaml-images/helloxaml2.png)](get-started-with-xaml-images/helloxaml2-large.png#lightbox)

## <a name="xaml-and-code-interactions"></a>XAML 및 코드 상호 작용

**HelloXamlPage** 샘플에는 페이지의 단일만 포함 되어 `Label` 있지만 매우 드문 경우입니다. 대부분의 `ContentPage` 파생은 `Content` 속성을와 같은 일종의 레이아웃으로 설정 합니다 `StackLayout` . `Children`의 속성은 `StackLayout` 형식으로 정의 되어 `IList<View>` 있지만 실제로는 형식의 개체 `ElementCollection<View>` 이며,이 컬렉션은 여러 뷰나 다른 레이아웃으로 채워질 수 있습니다. XAML에서 이러한 부모-자식 관계는 일반 XML 계층 구조를 사용 하 여 설정 됩니다. **XamlPlusCodePage**라는 새 페이지에 대 한 XAML 파일은 다음과 같습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

이 XAML 파일은 구문상 완전 하며, 다음과 같습니다.

[![페이지의 여러 컨트롤](get-started-with-xaml-images/xamlpluscode1.png)](get-started-with-xaml-images/xamlpluscode1-large.png#lightbox)

그러나이 프로그램을 기능적으로 판별 하는 것이 좋습니다. 아마도는가 `Slider` `Label` 현재 값을 표시 하도록 하 고,는 `Button` 프로그램 내에서 작업을 수행 하기 위한 것일 수 있습니다.

4 부에서 볼 수 있습니다 [. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)입니다 `Slider` .를 사용 하 여 값을 표시 하는 작업은 `Label` 완전히 XAML에서 데이터 바인딩을 사용 하 여 처리할 수 있습니다. 그러나 코드 솔루션을 먼저 확인 하는 것이 유용 합니다. 마찬가지로 클릭을 처리 하는 경우에도 코드를 명확 하 게 `Button` 해야 합니다. 즉,의 코드 숨김이 파일에는의 `XamlPlusCodePage` 이벤트 및의 이벤트에 대 한 처리기가 포함 되어야 합니다 `ValueChanged` `Slider` `Clicked` `Button` . 추가 해 보겠습니다.

```csharp
namespace XamlSamples
{
    public partial class XamlPlusCodePage
    {
        public XamlPlusCodePage()
        {
            InitializeComponent();
        }

        void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
        {

        }

        void OnButtonClicked(object sender, EventArgs args)
        {

        }
    }
}
```

이러한 이벤트 처리기는 public 일 필요가 없습니다.

XAML 파일로 돌아가서 `Slider` 및 `Button` 태그는 `ValueChanged` `Clicked` 이러한 처리기를 참조 하는 및 이벤트에 대 한 특성을 포함 해야 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Clicked="OnButtonClicked" />
    </StackLayout>
</ContentPage>
```

이벤트에 처리기를 할당 하는 것은 속성에 값을 할당 하는 것과 동일한 구문을 갖습니다.

의 이벤트 처리기가를 `ValueChanged` `Slider` 사용 하 여 현재 값을 표시 하는 경우 `Label` 처리기는 코드에서 해당 개체를 참조 해야 합니다. 에는 `Label` 특성으로 지정 된 이름이 필요 합니다 `x:Name` .

```xaml
<Label x:Name="valueLabel"
       Text="A simple Label"
       Font="Large"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand" />
```

`x`특성의 접두사는 `x:Name` 이 특성이 XAML에 내장 되어 있음을 나타냅니다.

특성에 할당 하는 이름에는 `x:Name` c # 변수 이름과 동일한 규칙이 있습니다. 예를 들어 문자 또는 밑줄로 시작 해야 하 고 공백을 포함 하지 않아야 합니다.

이제 `ValueChanged` 이벤트 처리기에서를 설정 `Label` 하 여 새 값을 표시할 수 있습니다 `Slider` . 새 값은 이벤트 인수에서 사용할 수 있습니다.

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

또는 처리기가 `Slider` 인수에서이 이벤트를 생성 하는 개체를 가져오고 `sender` 해당 개체에서 속성을 가져올 수 있습니다 `Value` .

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

프로그램을 처음 실행할 때 `Label` `Slider` 이벤트는 아직 발생 하지 않았기 때문에에서 값을 표시 하지 않습니다 `ValueChanged` . 그러나를 조작 하면 `Slider` 값이 표시 됩니다.

[![표시 된 슬라이더 값](get-started-with-xaml-images/xamlpluscode2.png)](get-started-with-xaml-images/xamlpluscode2-large.png#lightbox)

이제 `Button` . `Clicked`단추의를 사용 하 여 경고를 표시 하 여 이벤트에 대 한 응답을 시뮬레이트할 수 있습니다 `Text` . 이벤트 처리기는 인수를로 안전 하 게 캐스팅 한 `sender` `Button` 다음 해당 속성에 액세스할 수 있습니다.

```csharp
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!",
        "The button labeled '" + button.Text + "' has been clicked",
        "OK");
}
```

메서드는 메서드가 `async` 비동기 이므로 메서드가 `DisplayAlert` `await` 완료 될 때를 반환 하는 연산자를 사용 하 여 앞에와 야 하므로로 정의 됩니다. 이 메서드는 `Button` 인수에서 이벤트를 발생 시키는 `sender` 것을 가져오므로 여러 단추에 대해 동일한 처리기를 사용할 수 있습니다.

XAML로 정의 된 개체는 코드를 사용 하는 파일에서 처리 되는 이벤트를 발생 시킬 수 있으며, 코드 숨김이 특성을 사용 하 여 할당 된 이름을 사용 하 여 XAML에 정의 된 개체에 액세스할 수 있습니다 `x:Name` . 다음은 코드와 XAML이 상호 작용 하는 두 가지 기본적인 방법입니다.

새로 생성 된 **XamlPlusCode.xaml.g.cs 파일**을 검사 하 여 XAML이 작동 하는 방법에 대 한 몇 가지 추가 정보를 통해 모든 특성에 전용 필드로 할당 된 이름이 포함 되어 통해 수집할 수 있습니다 `x:Name` . 이 파일의 단순화 된 버전은 다음과 같습니다.

```csharp
public partial class XamlPlusCodePage : ContentPage {

    private Label valueLabel;

    private void InitializeComponent() {
        this.LoadFromXaml(typeof(XamlPlusCodePage));
        valueLabel = this.FindByName<Label>("valueLabel");
    }
}
```

이 필드를 선언 하면 `XamlPlusCodePage` 사법권의 partial 클래스 파일 내에서 자유롭게 변수를 사용할 수 있습니다. 런타임에는 XAML이 구문 분석 된 후에 필드가 할당 됩니다. 즉,이 `valueLabel` 필드는 `null` 생성자가 시작 될 때 `XamlPlusCodePage` 이지만가 호출 된 후에는 유효 `InitializeComponent` 합니다.

가 `InitializeComponent` 다시 생성자에 게 컨트롤을 반환 하면 페이지의 시각적 개체가 코드에서 인스턴스화되어 초기화 된 것 처럼 생성 됩니다. XAML 파일은 더 이상 클래스에서 아무 역할도 수행 하지 않습니다. 예를 들어에 뷰를 추가 `StackLayout` 하거나 `Content` 페이지의 속성을 완전히 다른 항목으로 설정 하는 등 원하는 방식으로 페이지에서 이러한 개체를 조작할 수 있습니다. `Content`페이지의 속성과 레이아웃 컬렉션의 항목을 검사 하 여 "트리를 탐색" 할 수 있습니다 `Children` . 이러한 방식으로 액세스 하는 뷰에 대해 속성을 설정 하거나 동적으로 이벤트 처리기를 할당할 수 있습니다.

무료입니다. 사용자의 페이지 이며 XAML은 콘텐츠를 빌드하기 위한 도구 일 뿐입니다.

## <a name="summary"></a>요약

이 소개를 통해 XAML 파일 및 코드 파일이 클래스 정의에 기여 하는 방법과 XAML 및 코드 파일이 상호 작용 하는 방법을 살펴보았습니다. 그러나 XAML은 매우 유연한 방식으로 사용할 수 있도록 하는 고유한 구문 기능을 제공 합니다. 2 부에서 탐색을 시작할 수 있습니다 [. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)입니다.

## <a name="related-links"></a>관련 링크

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [2 부. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [3 부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [4 부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [5 부. MVVM에 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
