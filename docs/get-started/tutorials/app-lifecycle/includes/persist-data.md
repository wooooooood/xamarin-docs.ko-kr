---
ms.openlocfilehash: 16ceaba572ca932777bb366d9f7c58f6dcb24f70
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841385"
---
[`Application`](xref:Xamarin.Forms.Application) 서브클래스에는 수명 주기 상태 변경에 따라 데이터를 저장하는 데 사용할 수 있는 정적 [`Properties`](xref:Xamarin.Forms.Application.Properties) 사전이 있습니다. 이 사전은 `string` 키를 사용하고 `object` 값을 저장합니다. 사전은 자동으로 디바이스에 저장되며, 애플리케이션을 다시 시작하면 다시 채워집니다.

> [!IMPORTANT]
> [`Properties`](xref:Xamarin.Forms.Application.Properties) 사전은 스토리지에 대한 기본 형식만 직렬화할 수 있습니다.

이 연습에서는 백그라운드 시 [`Entry`](xref:Xamarin.Forms.Entry)에서 텍스트를 유지하도록 애플리케이션을 수정하고, 애플리케이션을 다시 시작할 때 텍스트를 `Entry`에 복원합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **솔루션 탐색기**의 **AppLifecycleTutorial** 프로젝트에서 **App.xaml**을 확장하고 **App.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **App.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace AppLifecycleTutorial
    {
        public partial class App : Application
        {
            const string displayText = "displayText";

            public string DisplayText { get; set; }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                Console.WriteLine("OnStart");

                if (Properties.ContainsKey(displayText))
                {
                    DisplayText = (string)Properties[displayText];
                }
            }

            protected override void OnSleep()
            {
                Console.WriteLine("OnSleep");
                Properties[displayText] = DisplayText;
            }

            protected override void OnResume()
            {
                Console.WriteLine("OnResume");
            }
        }
    }
    ```

    이 코드는 `DisplayText` 속성과 `displayText` 상수를 정의합니다. 애플리케이션이 백그라운드되거나 종료될 때 `OnSleep` 메서드 재정의는 `displayText` 키에 대해 `DisplayText` 속성 값을 [`Properties`](xref:Xamarin.Forms.Application.Properties) 사전에 추가합니다. 그런 다음, 애플리케이션이 시작될 때 `Properties` 사전에 `displayText` 키가 포함되어 있으면 키 값이 `DisplayText` 속성으로 복원됩니다.

    > [!IMPORTANT]
    > 예기치 않은 오류를 방지하기 위해 액세스하기 전에 항상 [`Properties`](xref:Xamarin.Forms.Application.Properties) 사전에서 키의 존재 여부를 확인하세요.

    `OnResume` 메서드 오버로드에서 [`Properties`](xref:Xamarin.Forms.Application.Properties) 사전의 데이터를 복원할 필요가 없습니다. 이는 애플리케이션이 백그라운드화되었을 때 애플리케이션과 해당 상태가 여전히 메모리에 있기 때문입니다.

1. **솔루션 탐색기**의 **AppLifecycleTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="AppLifecycleTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="entry"
                   Placeholder="Enter text here"
                   Completed="OnEntryCompleted" />
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 [`Entry`](xref:Xamarin.Forms.Entry)로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. [`Entry.Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) 속성은 `Entry`가 처음 표시될 때 표시되는 자리 표시자 텍스트를 지정하고, `OnEntryCompleted`라는 이벤트 처리기가 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트로 등록됩니다. 또한 `Entry`에는 `x:Name` 특성으로 지정된 이름이 있습니다. 이렇게 하면 코드 숨김 파일이 할당된 이름을 사용하여 `Entry` 개체에 액세스할 수 있습니다.

1. **솔루션 탐색기**의 **AppLifecycleTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnAppearing` 메서드에 대한 재정의 및 `OnEntryCompleted` 이벤트 처리기를 클래스에 추가합니다.

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();

        entry.Text = (Application.Current as App).DisplayText;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        (Application.Current as App).DisplayText = entry.Text;
    }
    ```

    `OnAppearing` 메서드는 `App.DisplayText` 속성의 값을 검색하여 [`Entry`](xref:Xamarin.Forms.Entry)의 [`Text`](xref:Xamarin.Forms.Entry.Text) 속성 값으로 설정합니다.

    > [!NOTE]
    > `OnAppearing` 메서드 재정의는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)가 배치된 후에 실행되지만 표시되기 직전에 실행됩니다. 따라서 Xamarin.Forms 뷰 콘텐츠를 설정하는 것이 좋습니다.

    [`Entry`](xref:Xamarin.Forms.Entry)에서 텍스트가 확정되면 반환 키를 사용하여 `OnEntryCompleted` 메서드가 실행되고 `Entry` 텍스트가 `App.DisplayText` 속성에 저장됩니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [`Entry`](xref:Xamarin.Forms.Entry)에 일부 텍스트를 입력하고 반환 키를 누릅니다. 그런 다음, 홈 단추를 눌러 `OnSleep` 메서드를 호출하여 애플리케이션의 배경을 지정합니다.

    마지막으로 Visual Studio에서 애플리케이션을 다시 시작하면 이전에 [`Entry`](xref:Xamarin.Forms.Entry)에 입력한 텍스트가 복원됩니다.

    [![iOS 및 Android에서 수명 주기 상태 변경에 따라 텍스트 속성이 유지되는 항목의 스크린샷](../images/persist-data.png "수명 주기 상태 변경에 따라 텍스트 속성이 유지되는 항목")](../images/persist-data-large.png#lightbox "수명 주기 상태 변경에 따라 텍스트 속성이 유지되는 항목")

    속성 사전에 데이터를 유지하는 방법에 대한 자세한 내용은 [Xamarin.Forms 앱 클래스](~/xamarin-forms/app-fundamentals/application-class.md) 가이드의 [속성 사전](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary)을 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad**의 **AppLifecycleTutorial** 프로젝트에서 **App.xaml**을 확장하고 **App.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **App.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace AppLifecycleTutorial
    {
        public partial class App : Application
        {
            const string displayText = "displayText";

            public string DisplayText { get; set; }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                Console.WriteLine("OnStart");

                if (Properties.ContainsKey(displayText))
                {
                    DisplayText = (string)Properties[displayText];
                }
            }

            protected override void OnSleep()
            {
                Console.WriteLine("OnSleep");
                Properties[displayText] = DisplayText;
            }

            protected override void OnResume()
            {
                Console.WriteLine("OnResume");
            }
        }
    }
    ```

    이 코드는 `DisplayText` 속성과 `displayText` 상수를 정의합니다. 애플리케이션이 백그라운드되거나 종료될 때 `OnSleep` 메서드 재정의는 `displayText` 키에 대해 `DisplayText` 속성 값을 [`Properties`](xref:Xamarin.Forms.Application.Properties) 사전에 추가합니다. 그런 다음, 애플리케이션이 시작될 때 `Properties` 사전에 `displayText` 키가 포함되어 있으면 키 값이 `DisplayText` 속성으로 복원됩니다.

    > [!IMPORTANT]
    > 예기치 않은 오류를 방지하기 위해 액세스하기 전에 항상 [`Properties`](xref:Xamarin.Forms.Application.Properties) 사전에서 키의 존재 여부를 확인하세요.

    `OnResume` 메서드 오버로드에서 [`Properties`](xref:Xamarin.Forms.Application.Properties) 사전의 데이터를 복원할 필요가 없습니다. 이는 애플리케이션이 백그라운드화되었을 때 애플리케이션과 해당 상태가 여전히 메모리에 있기 때문입니다.

1. **Solution Pad**의 **AppLifecycleTutorial** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="AppLifecycleTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="entry"
                   Placeholder="Enter text here"
                   Completed="OnEntryCompleted" />
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 [`Entry`](xref:Xamarin.Forms.Entry)로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. [`Entry.Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) 속성은 `Entry`가 처음 표시될 때 표시되는 자리 표시자 텍스트를 지정하고, `OnEntryCompleted`라는 이벤트 처리기가 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트로 등록됩니다. 또한 `Entry`에는 `x:Name` 특성으로 지정된 이름이 있습니다. 이렇게 하면 코드 숨김 파일이 할당된 이름을 사용하여 `Entry` 개체에 액세스할 수 있습니다.

1. **Solution Pad**의 **AppLifecycleTutorial** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **MainPage.xaml.cs**에서 `OnAppearing` 메서드에 대한 재정의 및 `OnEntryCompleted` 이벤트 처리기를 클래스에 추가합니다.

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();

        entry.Text = (Application.Current as App).DisplayText;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        (Application.Current as App).DisplayText = entry.Text;
    }
    ```

    `OnAppearing` 메서드는 `App.DisplayText` 속성의 값을 검색하여 [`Entry`](xref:Xamarin.Forms.Entry)의 [`Text`](xref:Xamarin.Forms.Entry.Text) 속성 값으로 설정합니다.

    > [!NOTE]
    > `OnAppearing` 메서드 재정의는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)가 배치된 후에 실행되지만 표시되기 직전에 실행됩니다. 따라서 Xamarin.Forms 뷰 콘텐츠를 설정하는 것이 좋습니다.

    [`Entry`](xref:Xamarin.Forms.Entry)에서 텍스트가 확정되면 반환 키를 사용하여 `OnEntryCompleted` 메서드가 실행되고 `Entry` 텍스트가 `App.DisplayText` 속성에 저장됩니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    [`Entry`](xref:Xamarin.Forms.Entry)에 일부 텍스트를 입력하고 반환 키를 누릅니다. 그런 다음, 홈 단추를 눌러 `OnSleep` 메서드를 호출하여 애플리케이션의 배경을 지정합니다.

    마지막으로 Mac용 Visual Studio에서 애플리케이션을 다시 시작하면 이전에 [`Entry`](xref:Xamarin.Forms.Entry)에 입력한 텍스트가 복원됩니다.

    [![iOS 및 Android에서 수명 주기 상태 변경에 따라 텍스트 속성이 유지되는 항목의 스크린샷](../images/persist-data.png "수명 주기 상태 변경에 따라 텍스트 속성이 유지되는 항목")](../images/persist-data-large.png#lightbox "수명 주기 상태 변경에 따라 텍스트 속성이 유지되는 항목")

    속성 사전에 데이터를 유지하는 방법에 대한 자세한 내용은 [Xamarin.Forms 앱 클래스](~/xamarin-forms/app-fundamentals/application-class.md) 가이드의 [속성 사전](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary)을 참조하세요.
