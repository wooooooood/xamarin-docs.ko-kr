---
title: 1 부입니다. XAML 시작
description: Xamarin.Forms 응용 프로그램에서 XAML 페이지의 시각적 콘텐츠를 정의할 수 주로 사용 됩니다. XAML 파일은 항상는 C# 코드 파일의 태그에 대 한 코드 지원을 제공 하와 연결 합니다. 함께이 두 파일 속성 초기화 및 자식 뷰를 포함 하는 새 클래스 정의에 적용 됩니다. XAML 파일 내에서 XML 요소와 특성, 클래스 및 속성 참조 되 고 태그와 코드 간 연결이 설정 됩니다.
ms.prod: xamarin
ms.assetid: 9073FA0E-BD5A-4492-8A93-54C466F6EDB9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: f8032966b49f6f023642b0d1338e8c5d740b66e0
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="part-1-getting-started-with-xaml"></a>1 부입니다. XAML 시작

_Xamarin.Forms 응용 프로그램에서 XAML 페이지의 시각적 콘텐츠를 정의할 수 주로 사용 됩니다. XAML 파일은 항상는 C# 코드 파일의 태그에 대 한 코드 지원을 제공 하와 연결 합니다. 함께이 두 파일 속성 초기화 및 자식 뷰를 포함 하는 새 클래스 정의에 적용 됩니다. XAML 파일 내에서 XML 요소와 특성, 클래스 및 속성 참조 되 고 태그와 코드 간 연결이 설정 됩니다._

## <a name="creating-the-solution"></a>솔루션 만들기

첫 번째 XAML 파일 편집을 시작 하려면 새 Xamarin.Forms 솔루션을 만들려면 Visual Studio 또는 Mac 용 Visual Studio를 사용 합니다. (환경에 해당 하는이 페이지 맨 위에 있는 탭을 선택 합니다.)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Windows에서 선택 하려면 Visual Studio를 사용 **파일 > 새로 만들기 > 프로젝트** 메뉴에서 합니다. 에 **새 프로젝트** 대화 상자에서 **Visual C# > 크로스 플랫폼** 왼쪽 차례로 **교차 플랫폼 앱 (Xamarin.Forms 또는 네이티브)** 가운데 있는 목록에서. 

![](get-started-with-xaml-images/win/newprojectdialog.png "새 프로젝트 대화 상자")

솔루션에 대 한 위치를 선택의 이름을 지정 **XamlSamples** (또는 원하는 대로)를 누르고 **확인**합니다.

다음 화면에서 선택 된 **비어 있는 앱** 서식 파일을는 **Xamarin.Forms** UI 기술 및 **PCL 이식 가능한 클래스 라이브러리 ()** 전략 코드 공유:

![](get-started-with-xaml-images/win/newcrossplatformapp.png "새 응용 프로그램 대화 상자")

Press **OK**. 

솔루션에 네 개의 프로젝트가 생성 됩니다:는 **XamlSamples** 이식 가능한 클래스 라이브러리 (PCL) **XamlSamples.Android**, **XamlSamples.iOS**, 및 유니버설 Windows 플랫폼 솔루션 **XamlSamples.UWP**합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 용 Visual Studio에서 선택 **파일 > 새 솔루션** 메뉴에서 합니다. 에 **새 프로젝트** 대화 상자에서 **다중 플랫폼 > 앱** 왼쪽 및 **새 Forms 응용 프로그램** (*하지* **Forms 앱** ) 템플릿 목록에서:

![](get-started-with-xaml-images/mac/newprojectdialog1.png "새 프로젝트 대화 상자 1")

키를 눌러 **다음**합니다.

다음 대화 상자에서 프로젝트 이름을 지정의 **XamlSamples** (또는 원하는 대로). 있는지 확인는 **이식 가능한 클래스 라이브러리를 사용 하 여** 라디오 단추를 선택 하 고 **사용자 인터페이스 파일에 대 한 사용 하 여 XAML** 선택:

![](get-started-with-xaml-images/mac/newprojectdialog2.png "새 프로젝트 대화 상자 2")

키를 눌러 **다음**합니다. 

다음 대화 상자에는 프로젝트의 위치를 선택할 수 있습니다.

![](get-started-with-xaml-images/mac/newprojectdialog3.png "새 프로젝트 대화 상자 3")

키를 눌러 **만들기**

세 개의 프로젝트를 솔루션에 만들:는 **XamlSamples** 이식 가능한 클래스 라이브러리 (PCL) **XamlSamples.Android**, 및 **XamlSamples.iOS**합니다. 

-----

만든 후의 **XamlSamples** 솔루션 솔루션 시작 프로젝트로 다양 한 플랫폼 프로젝트를 선택 하 여 개발 환경을 테스트 하려고 할 수 있습니다 및 빌드하고 간단한 응용 프로그램을 배포 하 여 만든 phone 에뮬레이터 또는 실제 장치에서 사용 되는 프로젝트 템플릿.

공유는 플랫폼별 코드를 작성 하는 제외 **XamlSamples** PCL 프로젝트는 위치 보내는 거의 모든 프로그래밍 시간입니다. 이러한 문서는 해당 프로젝트를 벗어나 모험 되지 않습니다.

### <a name="anatomy-of-a-xaml-file"></a>XAML 파일의 구조

내에서 **XamlSamples** 이식 가능한 클래스 라이브러리는 다음과 같은 이름의 파일 쌍:

- **App.xaml**, XAML 파일; 및
- **App.xaml.cs**, C# *코드 숨김* XAML 파일에 연결 된 파일입니다.

옆에 있는 화살표를 클릭 해야 **App.xaml** 코드 숨김 파일을 볼 수 있습니다. 

둘 다 **App.xaml** 및 **App.xaml.cs** 라는 클래스에 영향을 `App` 에서 파생 된 `Application`합니다. XAML 파일에 다른 대부분의 클래스에서 파생 된 클래스에 기여 `ContentPage`; 해당 파일에서 XAML을 사용 하 여 전체 페이지의 시각적 콘텐츠를 정의할 수 있습니다. 경우 두 개의 다른 파일에는 **XamlSamples** 프로젝트:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- **MainPage.xaml**, XAML 파일; 및
- **MainPage.xaml.cs**, C# 코드 숨김 파일입니다.

**MainPage.xaml** 파일은 다음과 같습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage">

    <Label Text="Welcome to Xamarin Forms!" 
           VerticalOptions="Center" 
           HorizontalOptions="Center" />

</ContentPage>
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- **XamlSamplesPage.xaml**, XAML 파일; 및
- **XamlSamplesPage.xaml.cs**, C# 코드 숨김 파일입니다.

**XamlSamplesPage.xaml** 파일은 다음과 같습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" 
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
             xmlns:local="clr-namespace:XamlSamples" 
             x:Class="XamlSamples.XamlSamplesPage">

    <Label Text="Welcome to Xamarin Forms!" 
           VerticalOptions="Center" 
           HorizontalOptions="Center" />

</ContentPage>
```

-----

두 개의 XML 네임 스페이스 ( `xmlns`) 선언은 Uri, Xamarin 웹 사이트에 보이는 첫 번째 및 두 번째에 Microsoft의를 참조 합니다. 확인 하려면 필요한 해당 Uri 지점 이름을 바꾸지 마십시오. 아무 것도 없는 합니다. Xamarin와 Microsoft가 소유 하는 Uri 단순히 이며 기본적으로 버전 식별자로 작동 합니다.

첫 번째 XML 네임 스페이스 선언을 접두사가 없는 XAML 파일에 정의 된 태그 xamarin.forms에 클래스에 예를 들어 참조 것을 의미 `ContentPage`합니다. 접두사를 정의 하는 두 번째 네임 스페이스 선언 `x`합니다. 이 옵션은 사용 XAML의 다른 구현에서 자체와 여러 가지 요소 및 XAML에 내장 된 특성을 지원 합니다. 그러나 이러한 요소와 특성은 URI에 포함 된 연도 따라 약간 다릅니다. Xamarin.Forms는 2009 XAML 사양 있지만 그 중 일부만 지원합니다.

`local` 네임 스페이스 선언을 PCL 프로젝트에서 다른 클래스에 액세스할 수 있습니다.

해당 첫 번째 태그의 끝에는 `x` 이라는 특성에 대해 사용은 접두사 `Class`합니다. 때문에이 사용 하 여 `x` 접두사는 XAML 네임 스페이스에서 XAML 속성에 대 한 유니버설 거의 같은 `Class` 거의 항상 라고 `x:Class`합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

`x:Class` 특성 정규화 된.NET 클래스 이름을 지정:는 `MainPage` 클래스에 `XamlSamples` 네임 스페이스입니다. 즉,이 XAML 파일 라는 새 클래스를 정의 함을 `MainPage` 에 `XamlSamples` 에서 파생 되는 네임 스페이스 `ContentPage`-는 태그는 `x:Class` 특성이 나타납니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

`x:Class` 특성 정규화 된.NET 클래스 이름을 지정:는 `XamlSamplesPage` 클래스에 `XamlSamples` 네임 스페이스입니다. 즉,이 XAML 파일 라는 새 클래스를 정의 함을 `XamlSamplesPage` 에 `XamlSamples` 에서 파생 되는 네임 스페이스 `ContentPage`-는 태그는 `x:Class` 특성이 나타납니다.

-----

`x:Class` 특성 파생된 C# 클래스를 정의 하는 XAML 파일의 루트 요소에만 나타날 수 있습니다. XAML 파일에 정의 된 새 클래스입니다. XAML 파일에 표시 되는 모든 항목은 대신 단순히 기존 클래스에서 인스턴스화되고 초기화 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**MainPage.xaml.cs** 파일은 다음과 같습니다 (사용 되지 않는 외 `using` 지시문):

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

`MainPage` 클래스에서 파생 `ContentPage`, 여기서 하지만 `partial` 클래스 정의 합니다. 이 인해 다른 partial 클래스 정의가 없어야 `MainPage`, 여기서는? 이며 `InitializeComponent` 메서드? 

Visual Studio에서 프로젝트를 빌드할 때 C# 코드 파일을 생성 하려면 XAML 파일 구문 분석 합니다. 보면는 **XamlSamples\XamlSamples\obj\Debug** 디렉터리 라는 파일을 찾을 수 있습니다 **XamlSamples.MainPage.xaml.g.cs**합니다. 'G'는 생성을 나타냅니다. 이 다른 partial 클래스 정의의 `MainPage` 의 정의 포함 하는 `InitializeComponent` 에서 호출 하는 메서드는 `MainPage` 생성자입니다. 이러한 두 부분 `MainPage` 클래스 정의 함께 컴파일될 수 있습니다. XAML은 컴파일 따라 여부, 실행 파일에 XAML 파일이 나 XAML 파일의 이진 형식이 포함 됩니다.

런타임 시 호출 하 여 특정 플랫폼 프로젝트에서에서 코드는 `LoadApplication` 의 새 인스턴스를 전달 하는 메서드를는 `App` PCL에는 클래스입니다. `App` 클래스 생성자를 인스턴스화하 `MainPage`합니다. 해당 클래스의 생성자를 호출 `InitializeComponent`, 호출의 `LoadFromXaml` PCL에서 XAML 파일 (또는 해당 컴파일된 이진 파일)를 추출 하는 메서드입니다. `LoadFromXaml` XAML 파일에 정의 된 모든 개체를 초기화, 부모-자식 관계에서 모두 함께 연결, XAML 파일에서 설정 하는 이벤트에는 코드에 정의 된 이벤트 처리기를 연결 및 개체의 결과 트리 페이지의 내용으로 설정 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**XamlSamplesPage.xaml.cs** 파일은 다음과 같습니다.

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class XamlSamplesPage : ContentPage
    {
        public XamlSamplesPage()
        {
            InitializeComponent();
        }
    }
}
```

`XamlSamplesPage` 클래스에서 파생 `ContentPage`, 여기서 하지만 `partial` 클래스 정의 합니다. 다른 C# 파일을 다른 partial 클래스 정의가 없어야 있습니다 제안 `XamlSamplesPage`, 여기서는? 이며 `InitializeComponent` 메서드?

Mac 용 Visual Studio에서 프로젝트를 빌드할 때 C# 코드 파일을 생성 하려면 XAML 파일 구문 분석 합니다. 보면는 **XamlSamples\XamlSamples\obj\Debug** 디렉터리 라는 파일을 찾을 수 있습니다 **XamlSamples.XamlSamplesPage.xaml.g.cs**합니다. 'G'는 생성을 나타냅니다. 이 다른 partial 클래스 정의의 `XamlSamplesPage` 의 정의 포함 하는 `InitializeComponent` 에서 호출 하는 메서드는 `XamlSamplesPage` 생성자입니다.  이러한 두 부분 `XamlSamplesPage` 클래스 정의 함께 컴파일될 수 있습니다. XAML은 컴파일 따라 여부, 실행 파일에 XAML 파일이 나 XAML 파일의 이진 형식이 포함 됩니다.

런타임 시 호출 하 여 특정 플랫폼 프로젝트에서에서 코드는 `LoadApplication` 의 새 인스턴스를 전달 하는 메서드를는 `App` PCL에는 클래스입니다. `App` 클래스 생성자를 인스턴스화하 `XamlSamplesPage`합니다. 해당 클래스의 생성자를 호출 `InitializeComponent`, 호출의 `LoadFromXaml` PCL에서 XAML 파일 (또는 해당 컴파일된 이진 파일)를 추출 하는 메서드입니다. `LoadFromXaml` XAML 파일에 정의 된 모든 개체를 초기화, 부모-자식 관계에서 모두 함께 연결, XAML 파일에서 설정 하는 이벤트에는 코드에 정의 된 이벤트 처리기를 연결 및 개체의 결과 트리 페이지의 내용으로 설정 합니다.

-----

정상적으로 생성 된 코드 파일에 많은 시간을 보낼 필요가 없습니다, 있지만 때로는 런타임 예외 발생 생성 된 파일의 코드에서 작업할 수 있도록 합니다.

컴파일 및이 프로그램을 실행 하는 경우는 `Label` XAML 알 수 있듯이 요소는 페이지의 가운데에 표시 합니다. 왼쪽에서 오른쪽에는 세 플랫폼은 iOS, Android, 및 UWP:

[![](get-started-with-xaml-images/xamlsamples.png "Xamarin.Forms 디스플레이 기본")](get-started-with-xaml-images/xamlsamples-large.png#lightbox "Xamarin.Forms 기본 표시")

더 흥미로운 시각적 개체에 대 한 필요한 것은 더 많은 XAML 흥미로운 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="preliminaries"></a>준비 단계

파일 이름에에서 하려면 Visual Studio Mac Windows에서 실행 되는 Visual Studio에서 만든 파일와 일치, 이름 바꾸기 **XamlSamplesPage.xaml** 를 **MainPage.xaml**, 및  **XamlSamplesPage.xaml.cs** 를 **MainPage.xaml.cs**합니다. 내에서 **XamlSamplesPage.xaml** 파일에서 변경 `XamlSamplesPage` 를 `MainPage`합니다. 내에서 **XamlSamplesPage.xaml.cs** 파일에서 두 개 변경 `XamlSamplesPage` 를 `MainPage`합니다. 내에서 **App.xaml.cs** 파일에서 문을 변경 합니다.

```csharp
MainPage = new XamlSamplesPage();
```

다음과 같이 변경합니다.

```csharp
MainPage = new MainPage();
```

-----

테스트 프로그램 계속 컴파일하고 계속 하기 전에 배포입니다.

## <a name="adding-new-xaml-pages"></a>새 XAML 페이지 추가

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

다른 XAML 기반 추가 하려면 `ContentPage` 프로젝트에 클래스를 선택는 **XamlSamples** PCL 프로젝트를 마우스 호출는 **프로젝트 > 새 항목 추가** 메뉴 항목입니다. 왼쪽에는 **새 항목 추가** 대화 상자에서 **Visual C#** 및 **Xamarin.Forms**합니다. 목록에서 선택 **콘텐츠 페이지** (하지 **콘텐츠 페이지 (C#)** 를 만드는 코드 전용 페이지 또는 **콘텐츠 보기**, 페이지 않습니다). 예를 들어 페이지에 이름을 지정 **HelloXamlPage.xaml**:

![](get-started-with-xaml-images/win/addnewitemdialog.png "새 항목 추가 대화 상자")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

다른 XAML 기반 추가 하려면 `ContentPage` 프로젝트에 클래스를 선택는 **XamlSamples** PCL 프로젝트를 마우스 호출는 **파일 > 새 파일** 메뉴 항목입니다. 왼쪽에는 **새 파일** 대화 상자에서 **Forms** 왼쪽 및 **Forms ContentPage Xaml** (하지 **Forms ContentPage**이며 코드 전용 페이지를 만듭니다. 또는 **콘텐츠 보기**, 페이지 않습니다). 예를 들어 페이지에 이름을 지정 **HelloXamlPage**:

![](get-started-with-xaml-images/mac/newfiledialog.png "새 파일 대화 상자")

-----

두 개의 파일을 프로젝트에 추가 됩니다 **HelloXamlPage.xaml** 및 코드 숨김 파일 **HelloXamlPage.xaml.cs**합니다. 

## <a name="setting-page-content"></a>페이지 콘텐츠를 설정합니다.

편집 된 **HelloXamlPage.xaml** 파일에 대 한 유일한 태그는 `ContentPage` 및 `ContentPage.Content`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage">
    <ContentPage.Content>
        
    </ContentPage.Content>
</ContentPage>
```

`ContentPage.Content` 태그는 XAML의 고유 구문의 일부입니다. 처음에 잘못 된 xml에 표시 될 수 있지만 사용할 합니다. XML에서 특수 문자 상태인 아닙니다.

`ContentPage.Content` 태그 라고 *속성 요소* 태그입니다. `Content` 속성인 `ContentPage`, 일반적으로 단일 뷰 또는 자식 뷰가 포함 된 레이아웃을 설정 하 고 있습니다. 일반적으로 속성 XAML에서 특성 되지만 설정할 어려울는 `Content` 특성을 복잡 한 개체입니다. 따라서 속성은 클래스 이름과 마침표로 구분 된 속성 이름으로 구성 된 XML 요소로 표현 됩니다. 이제는 `Content` 간에 속성을 설정할 수 있습니다는 `ContentPage.Content` 다음과 같은 태그:

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

또한 한 `Title` 특성이 루트 태그에 설정 되어 있습니다.

이때 클래스, 속성 및 XML 간의 관계 분명해 집니다: A Xamarin.Forms 클래스 (같은 `ContentPage` 또는 `Label`) XML 요소는 XAML 파일에 나타납니다. 해당 클래스의 속성-포함 하 여 `Title` 에 `ContentPage` 및의 7 개의 속성과 `Label`-일반적으로 XML 특성으로 표시 합니다.

대부분의 바로 가기 이러한 속성의 값을 설정 하기 위해 존재 합니다. 일부 속성은 기본 데이터 형식: 예를 들어는 `Title` 및 `Text` 형식의 속성은 `String`, `Rotation` 유형의 `Double`, 및 `IsVisible` (변수인 `true` 기본적으로 여기에 대해서만 설정 됩니다 그림)은 형식의 `Boolean`합니다.

`HorizontalTextAlignment` 속성은 형식이 `TextAlignment`를 열거형입니다. 모든 열거형 형식의 속성, 공급 가장 필요한 것은 멤버 이름입니다.

그러나 더 복잡 한 형식의 속성에 대 한 변환기는 XAML을 구문 분석에 사용 됩니다. 클래스에서 파생 되는 xamarin.forms 이들은 `TypeConverter`합니다. 공용 클래스 대부분은 있지만 일부 않습니다. 이 특정 XAML 파일에 대 한 이러한 클래스 중 몇 가지 중요 한 역할 원리를 자세히 파악할수록:

-  `LayoutOptionsConverter` 에 대 한는 `VerticalOptions` 속성
-  `FontSizeConverter` 에 대 한는 `FontSize` 속성
-  `ColorTypeConverter` 에 대 한는 `TextColor` 속성

이러한 변환기 속성 설정의 허용 가능한 구문으로 제어합니다.

`ThicknessTypeConverter` 하나, 2 또는 4 개의 번호를 쉼표로 구분 하 여 처리할 수 있습니다. 한 숫자가 제공 되는 경우 네 면 모두에 적용 됩니다. 두 개의 숫자와 왼쪽 및 오른쪽 안쪽 여백을, 첫 번째는이 고 두 번째 위쪽 및 아래쪽입니다. 4 개의 번호 순서 왼쪽, 위쪽, 오른쪽 및 아래쪽에는입니다.

`LayoutOptionsConverter` 의 공용 정적 필드의 이름으로 변환할 수는 `LayoutOptions` 종류의 값에는 구조 `LayoutOptions`합니다.

`FontSizeConverter` 처리할 수는 `NamedSize` 멤버나 숫자 글꼴 크기입니다.

`ColorTypeConverter` 의 공용 정적 필드의 이름을 허용는 `Color` 구조 또는 알파 채널이 유무 16 진 RGB 값 앞에 숫자 기호 (#). 알파 채널 없이 구문을 다음과 같습니다.

 `TextColor="#rrggbb"`

각각의 작은 문자는 16 진수 숫자입니다. 알파 채널이 포함 된 방법을 다음과 같습니다.

 `TextColor="#aarrggbb">`

알파 채널에 대 한 FF은 완전히 불투명 한 00은 완전히 투명 염두에 둬야 합니다.

다른 두 가지 형식을 사용 하면 각 채널에 대 한 단일 16 진수만 지정할 수 있도록:

 `TextColor="#rgb"` `TextColor="#argb"`

이러한 경우에 그 숫자 값을 구성 하도록 반복 됩니다. 예를 들어 #CF3 CC-FF-33 RGB 색입니다.

## <a name="page-navigation"></a>페이지 탐색

실행 하는 경우는 **XamlSamples** 프로그램은 `MainPage` 표시 됩니다. 새 보려면 `HelloXamlPage` 새 시작 화면으로 페이지에 설정 하거나는 **App.xaml.cs** 파일 또는 페이지로 이동 하 여 새에서 `MainPage`합니다.

탐색을 구현 하려면 먼저에서 코드 변경는 **App.xaml.cs** 생성자 있도록는 `NavigationPage` 개체가 만들어집니다.

```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

에 **MainPage.xaml.cs** 생성자는 간단한 만들 수 있습니다 `Button` 이벤트 처리기를 사용 하 여 탐색 및 `HelloXamlPage`:

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

설정의 `Content` 속성 페이지의 설정을 대체는 `Content` XAML 파일에는 속성입니다. 컴파일하고이 프로그램의 새 버전을 배포 하는 경우 단추가 화면에 나타납니다. 이동 누르지 `HelloXamlPage`합니다. IPhone, Android 및 UWP에서 결과 페이지는 다음과 같습니다.

[![](get-started-with-xaml-images/helloxaml1.png "레이블 텍스트를 회전")](get-started-with-xaml-images/helloxaml1-large.png#lightbox "레이블 텍스트를 회전")

다시 탐색할 수 `MainPage` 를 사용 하 여는 **< 다시** iOS, android에서는 전화 맨 아래에 또는 페이지 맨 위에 있는 왼쪽된 화살표를 사용 하 여 또는 왼쪽된 화살표를 사용 하 여 Windows 10에서 페이지의 위쪽에 단추입니다.

렌더링 하 여 다양 한 방법에 대 한 XAML과 원하는 대로 볼 수는 `Label`합니다. 텍스트에 모든 유니코드 문자를 포함 해야 할 경우 표준 XML 구문을 사용할 수 있습니다. 예를 들어 따옴표로 묶어야 인사말 스마트를 사용 합니다.

 `<Label Text="&#x201C;Hello, XAML!&#x201D;" … />`

창의 모양은 다음과 같습니다.

[![](get-started-with-xaml-images/helloxaml2.png "유니코드 문자로 레이블 텍스트를 회전")](get-started-with-xaml-images/helloxaml2-large.png#lightbox "유니코드 문자로 레이블 텍스트를 회전")

## <a name="xaml-and-code-interactions"></a>XAML 및 코드 상호 작용

**HelloXamlPage** 샘플에는 단일 `Label` 페이지 있지만 매우 일반적인 것은 아닙니다. 대부분 `ContentPage` 파생 항목 집합의 `Content` 와 같은 속성의 일부 레이아웃을 정렬 한 `StackLayout`합니다. `Children` 속성의는 `StackLayout` 형식으로 정의 된 `IList<View>` 형식의 개체가 실제로 이지만 `ElementCollection<View>`, 및 컬렉션 다중 뷰 또는 다른 레이아웃으로 채울 수 있습니다. Xaml에서는 이러한 부모-자식 관계는 일반 XML 계층 구조에서 설정 됩니다. 다음은 명명 된 새 페이지에 대 한 XAML 파일 **XamlPlusCodePage**:

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

이 XAML 파일에는 구문상 완료 되며 다음과 같이 표시:

[![](get-started-with-xaml-images/xamlpluscode1.png "페이지에 있는 여러 컨트롤")](get-started-with-xaml-images/xamlpluscode1-large.png#lightbox "페이지에 있는 여러 컨트롤")

그러나 해야이 프로그램의 기능적 불완전 한 수를 고려해 야 할 가능성이 높습니다. 아마도 `Slider` 발생 하도록 되어는 `Label` 현재 값을 표시 하 고 `Button` 프로그램 내에서 작업을 수행 하기 위한 용도가 것입니다.

볼 수 있듯이 [4 부입니다. 데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md), 표시 작업은 `Slider` 를 사용 하 여 값을 `Label` 데이터 바인딩으로 XAML을 처리할 수 있습니다. 하지만 코드 솔루션을 먼저 참조 하는 것이 유용 합니다. 그럼에도 처리는 `Button` 클릭 하 여 코드를 반드시 있어야 합니다. 즉, 코드 숨김 파일에 대 한 `XamlPlusCodePage` 에 대 한 처리기를 포함 해야 합니다는 `ValueChanged` 의 이벤트는 `Slider` 및 `Clicked` 의 이벤트는 `Button`합니다. 추가 해 보겠습니다.

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

이러한 이벤트 처리기를 public 필요가 없습니다.

XAML 파일에서 다시는 `Slider` 및 `Button` 태그에 대 한 특성을 포함할 필요는 `ValueChanged` 및 `Clicked` 이러한 처리기를 참조 하는 이벤트:

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

이벤트에 대 한 처리기를 할당 속성 값을 할당 하는 구문은 동일에 있는지 확인 합니다.

경우에 대 한 처리기는 `ValueChanged` 의 이벤트는 `Slider` 사용는 `Label` 현재 값을 표시 하려면 처리기 코드에서 해당 개체를 참조 해야 합니다. `Label` 가 지정 된 이름이 필요는 `x:Name` 특성입니다.

```xaml
<Label x:Name="valueLabel"
       Text="A simple Label"
       Font="Large"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand" />
```

`x` 의 접두사는 `x:Name` 특성이이 특성은 XAML로 내장 함수를 나타냅니다.

지정 하는 이름은 `x:Name` 특성은 C# 변수 이름으로 한 동일한 규칙입니다. 예를 들어 문자 또는 밑줄로 시작 고 공백 포함 하지 않음 포함 해야 합니다.

이제는 `ValueChanged` 이벤트 처리기는 `Label` 표시할 새 `Slider` 값입니다. 새 값이 이벤트 인수에서 사용할 수 있습니다.

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

처리기를 얻을 수 또는 `Slider` 에서이 이벤트를 생성 하는 개체는 `sender` 인수 가져오고,는 `Value` 에서:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

프로그램을 처음 실행할 때는 `Label` 표시 되지 않습니다는 `Slider` 때문에 값의 `ValueChanged` 이벤트가 아직 실행 되지 않았습니다. 하지만 조작 하는 `Slider` 인해 값이 표시 될 수 있습니다.

[![](get-started-with-xaml-images/xamlpluscode2.png "표시 되는 슬라이더 값")](get-started-with-xaml-images/xamlpluscode2-large.png#lightbox "슬라이더 값 표시")

이제는 `Button`합니다. 보겠습니다에 대 한 응답을 시뮬레이션 한 `Clicked` 로 경고를 표시 하 여 이벤트는 `Text` 단추의 합니다. 이벤트 처리기 안전 하 게 캐스팅 수는 `sender` 인수에는 `Button` 다음 해당 속성에 액세스:

```csharp
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!",
        "The button labeled '" + button.Text + "' has been clicked",
        "OK");
}
```

메서드가으로 정의 된 `async` 때문에 `DisplayAlert` 메서드는 비동기적 이며 앞에 추가 해야는 `await` 메서드가 완료 될 때 반환 하는 연산자입니다. 하므로이 메서드를 가져옵니다는 `Button` 에서 이벤트를 발생 시키기는 `sender` 인수를 여러 단추에 대 한 동일한 처리기를 사용할 수 없습니다.

XAML에서 정의 되는 개체 코드 숨김 파일에서 처리 되는 이벤트를 발생 시킬 수 및 코드 숨김 파일에 사용 하 여 할당 된 이름을 사용 하 여 XAML에서 정의 되는 개체에 액세스할 수 있는지를 살펴 보았으며는 `x:Name` 특성입니다. 코드 및 XAML 상호 작용 하는 두 가지 기본 방법은 다음과 같습니다.

새로 생성 된 검사 하 여 XAML works를 수집할 수 방식에 몇 가지 추가 정보 **XamlPlusCode.xaml.g.cs 파일**, 이제 포함 하나에 할당 된 모든 이름은 `x:Name` 전용 필드로 특성입니다. 해당 파일의 단순화 된 버전 다음과 같습니다.

```csharp
public partial class XamlPlusCodePage : ContentPage {

    private Label valueLabel;

    private void InitializeComponent() {
        this.LoadFromXaml(typeof(XamlPlusCodePage));
        valueLabel = this.FindByName<Label>("valueLabel");
    }
}
```

이 필드의 선언에서는 자유롭게 모든 위치에서 사용할 변수는 `XamlPlusCodePage` 관할지에서 partial 클래스 파일입니다. 런타임 시 XAML을 구문 분석할 필드 지정 됩니다. 즉는 `valueLabel` 필드는 `null` 때는 `XamlPlusCodePage` 생성자 시작 후에 유효 하지만 `InitializeComponent` 호출 됩니다.

후 `InitializeComponent` 반환 생성자에 게 제어 하 고, 한 경우 처럼 된 인스턴스화 되었고 코드에서 초기화 페이지의 시각적 개체가 생성 되었습니다. 더 이상 XAML 파일의 클래스에서 모든 역할을 수행합니다. 원하는 예를 들어 뷰를 추가 하 여 어떤 방식으로든 페이지에서 이러한 개체를 조작할 수는 `StackLayout`, 또는 설정의 `Content` 다른 페이지의 속성 완전히 합니다. 연습할 수 있습니다"트리" 검사 하 여는 `Content` 페이지에 있는 항목의 속성은 `Children` 레이아웃의 컬렉션입니다. 이 방식으로 액세스 하는 뷰의 속성을 설정 하거나을 이벤트 처리기를 동적으로 할당할 수 있습니다.

됩니다. 페이지에서 사용 되며 XAML은 해당 콘텐츠를 빌드하는 도구만.

## <a name="summary"></a>요약

이 소개와 XAML 파일 및 코드 파일에 주는 영향 클래스 정의 및 XAML 및 코드 파일이 상호 작용 하는 방법을 살펴보았습니다. 하지만 XAML 또한 자체 고유 구문 기능을 사용 하는 매우 유연한 방식으로 사용할 수 있습니다. 이러한 탐색을 시작할 수 있습니다 [2 부 합니다. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)합니다.



## <a name="related-links"></a>관련 링크

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [2부. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [3부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [4부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [5부. MVVM에 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
