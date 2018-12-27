---
title: Xamarin.Forms Multiscreen Quickstart
description: 이 문서에서는 응용 프로그램에 대한 통화 기록을 추적하기 위해 두 번째 화면을 추가하여 Phoneword 응용 프로그램을 확장하는 방법을 설명합니다.
zone_pivot_groups: platform
ms.prod: quickstart
ms.assetid: 255d93b9-518c-4e5d-a9cd-4dd8a7945a7f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: 957c3e0d3b0637c8b536d920a05397bc711dfb7d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123481"
---
# <a name="xamarinforms-multiscreen-quickstart"></a>Xamarin.Forms Multiscreen Quickstart

이 빠른 시작에서는 응용 프로그램에 대한 통화 기록을 추적하기 위해 두 번째 화면을 추가하여 Phoneword 응용 프로그램을 확장하는 방법을 보여줍니다. 최종 응용 프로그램은 다음과 같습니다.

[![](quickstart-images/intro-app-examples-sml.png "Phoneword 응용 프로그램")](quickstart-images/intro-app-examples.png#lightbox "Phoneword 응용 프로그램")

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio를 사용하여 앱 업데이트

1. Visual Studio를 실행합니다. 시작 페이지에서 **프로젝트/솔루션 열기...** 를 클릭하고, **프로젝트 열기** 대화 상자에서 Phoneword 프로젝트에 대한 솔루션 파일을 선택합니다.

    ![](quickstart-images/vs/open-solution.png "프로젝트 열기")

2. **솔루션 탐색기**에서 **Phoneword** 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **추가 > 새 항목...** 을 클릭합니다.

    ![](quickstart-images/vs/add-new-item.png "새 항목 추가")

3. **새 항목 추가** 대화 상자에서 **Visual C# 항목 > Xamarin.Forms > 콘텐츠 페이지**를 선택하고, 새 파일의 이름을 **CallHistoryPage**로 지정하고, **추가** 단추를 클릭합니다. 그러면 **CallHistoryPage**란 이름의 페이지가 프로젝트에 추가됩니다.

    ![](quickstart-images/vs/add-callhistorypage-class.png "Xamarin.Forms 프로젝트 템플릿")

4. **CallHistoryPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. 이 코드는 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>
    ```

    **CTRL+S** 키를 눌러 변경 내용을 **CallHistoryPage.xaml**에 저장하고 파일을 닫습니다.

5. **솔루션 탐색기**에서 공유 **Phoneword** 프로젝트의 **App.xaml.cs** 파일을 두 번 클릭하여 엽니다.

    ![](quickstart-images/vs/open-app-class.png "App.xaml.cs 열기")

6. **App.xaml.cs**에서 `System.Collections.Generic` 네임스페이스를 가져와 `PhoneNumbers` 속성의 선언을 추가하고, 속성을 `App` 생성자에서 초기화한 후 [`MainPage`](xref:Xamarin.Forms.Application.MainPage) 속성이 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)가 되도록 초기화합니다. `PhoneNumbers` 컬렉션은 응용 프로그램을 사용해 호출한 각 변환된 전화번호의 목록을 저장하기 위해 사용됩니다.

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public static IList<string> PhoneNumbers { get; set; }

            public App()
            {
                InitializeComponent();
                PhoneNumbers = new List<string>();
                MainPage = new NavigationPage(new MainPage());
            }
            ...
        }
    }
    ```

    **CTRL+S** 키를 눌러 변경 내용을 **App.xaml.cs**에 저장하고 파일을 닫습니다.

7. **솔루션 탐색기**에서 공유 **Phoneword** 프로젝트의 **MainPage.xaml** 파일을 두 번 클릭하여 엽니다.

    ![](quickstart-images/vs/open-mainpage-xaml.png "MainPage.xaml 열기")

8. **MainPage.xaml**에서 [`Button`](xref:Xamarin.Forms.Button) 컨트롤을 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 컨트롤의 끝에 추가합니다. 단추는 호출 기록 페이지로 이동하기 위해 사용됩니다.

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    **CTRL+S** 키를 눌러 변경 내용을 **MainPage.xaml**에 저장하고 파일을 닫습니다.

9. **솔루션 탐색기**에서 **MainPage.xaml.cs**을 두 번 클릭하여 엽니다.

    ![](quickstart-images/vs/open-mainpage-codebehind.png "MainPage.xaml.cs 열기")

10. **MainPage.xaml.cs**에서 `dialer` 변수가 `null`이 아닐 경우 `OnCallHistory` 이벤트 처리기 메서드를 추가하고, 변환된 전화번호를 `App.PhoneNumbers` 컬렉션에 추가하고 `callHistoryButton`을 활성화할 `OnCall` 이벤트 처리기 메서드를 수정합니다.

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            ...

            async void OnCall(object sender, EventArgs e)
            {
                ...
                if (dialer != null) {
                    App.PhoneNumbers.Add (translatedNumber);
                    callHistoryButton.IsEnabled = true;
                    dialer.Dial (translatedNumber);
                }
                ...
            }

            async void OnCallHistory(object sender, EventArgs e)
            {
                await Navigation.PushAsync (new CallHistoryPage ());
            }
        }
    }
    ```

    **CTRL+S** 키를 눌러 변경 내용을 **MainPage.xaml.cs**에 저장하고 파일을 닫습니다.

11. Visual Studio에서 **빌드 > 솔루션 빌드** 메뉴 항목을 선택(또는 **CTRL+SHIFT+B** 키를 누름)합니다. 응용 프로그램이 빌드되고 성공 메시지가 Visual Studio 상태 표시줄에 표시됩니다.

    ![](quickstart-images/vs/build-successful.png "빌드 성공")

    오류가 있는 경우 이전 단계를 반복하고 응용 프로그램이 성공적으로 빌드할 때까지 실수를 수정합니다.

12. Visual Studio 도구 모음에서 응용 프로그램을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](quickstart-images/vs/start.png "Visual Studio 도구 모음")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword 응용 프로그램 UWP")

13. **솔루션 탐색기**에서 **Phoneword.Droid** 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **시작 프로젝트로 설정**을 선택합니다.
14. Visual Studio 도구 모음에서 Android 에뮬레이터 안에 응용 프로그램을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.
15. iOS 디바이스가 있고 Xamarin.Forms 개발에 대한 Mac 시스템 요구 사항을 충족하는 경우 비슷한 기술을 사용하여 앱을 iOS 디바이스에 배포합니다. 또는 [iOS 원격 시뮬레이터](~/tools/ios-simulator/index.md)에 앱을 배포합니다.

    > [!NOTE]
    > 전화 통화는 디바이스 에뮬레이터에서 지원되지 않습니다.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Mac용 Visual Studio를 사용하여 앱 업데이트

1. Mac용 Visual Studio. 시작 페이지에서 **프로젝트 열기...** 를 클릭하고, 대화 상자에서 Phoneword 프로젝트에 대한 솔루션 파일을 선택합니다.

    ![](quickstart-images/xs/open-solution.png "솔루션 열기")

2. **Solution Pad**에서 **Phoneword** 프로젝트를 선택한 다음, **추가 > 새 파일...** 을 선택합니다.

    ![](quickstart-images/xs/add-new-file.png "새 파일 추가")

3. **새 파일** 대화 상자에서 **Forms > Forms ContentPage Xaml**을 선택하고, 새 파일에 **CallHistoryPage**라는 이름을 지정하고 **새로 만들기** 단추를 클릭합니다. 그러면 **CallHistoryPage**란 이름의 페이지가 프로젝트에 추가됩니다.

    ![](quickstart-images/xs/add-callhistorypage-class.png "Add Forms ContentPage")

4. **Solution Pad**에서 **CallHistoryPage.xaml**을 두 번 클릭하여 엽니다.

    ![](quickstart-images/xs/open-callhistorypage-xaml.png "CallHistoryPage.xaml 열기")

5. **CallHistoryPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. 이 코드는 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>      
    ```

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **CallHistoryPage.xaml**에 저장하고 파일을 닫습니다.

6. **Solution Pad**에서 **App.xaml.cs**를 두 번 클릭하여 엽니다.

    ![](quickstart-images/xs/open-app-class.png "App.xaml.cs 열기")

7. **App.xaml.cs**에서 `System.Collections.Generic` 네임스페이스를 가져와 `PhoneNumbers` 속성의 선언을 추가하고, 속성을 `App` 생성자에서 초기화한 후 [`MainPage`](xref:Xamarin.Forms.Application.MainPage) 속성이 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)가 되도록 초기화합니다. `PhoneNumbers` 컬렉션은 응용 프로그램을 사용해 호출한 각 변환된 전화번호의 목록을 저장하기 위해 사용됩니다.

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public static IList<string> PhoneNumbers { get; set; }

            public App()
            {
                InitializeComponent();
                PhoneNumbers = new List<string>();
                MainPage = new NavigationPage(new MainPage());
            }
            ...
        }
    }
    ```

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **App.xaml.cs**에 저장하고 파일을 닫습니다.

8. **Solution Pad**에서 **MainPage.xaml**을 두 번 클릭하여 엽니다.

    ![](quickstart-images/xs/open-mainpage-xaml.png "MainPage.xaml 열기")

9. **MainPage.xaml**에서 [`Button`](xref:Xamarin.Forms.Button) 컨트롤을 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 컨트롤의 끝에 추가합니다. 단추는 호출 기록 페이지로 이동하기 위해 사용됩니다.

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **MainPage.xaml**에 저장하고 파일을 닫습니다.

10. **Solution Pad**에서 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다.

    ![](quickstart-images/xs/open-mainpage-codebehind.png "MainPage.xaml.cs 열기")

11. **MainPage.xaml.cs**에서 `dialer` 변수가 `null`이 아닐 경우 `OnCallHistory` 이벤트 처리기 메서드를 추가하고, 변환된 전화번호를 `App.PhoneNumbers` 컬렉션에 추가하고 `callHistoryButton`을 활성화할 `OnCall` 이벤트 처리기 메서드를 수정합니다.

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            ...

            async void OnCall(object sender, EventArgs e)
            {
                ...
                if (dialer != null) {
                    App.PhoneNumbers.Add (translatedNumber);
                    callHistoryButton.IsEnabled = true;
                    dialer.Dial (translatedNumber);
                }
                ...
            }

            async void OnCallHistory(object sender, EventArgs e)
            {
                await Navigation.PushAsync (new CallHistoryPage ());
            }
        }
    }
    ```

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **MainPage.xaml.cs**에 저장하고 파일을 닫습니다.

12. Mac용 Visual Studio에서 **빌드 > 솔루션 빌드** 메뉴 항목을 선택(또는 **&#8984; + B** 키를 누름)합니다. 응용 프로그램이 빌드되고 성공 메시지가 Mac용 Visual Studio 상태 표시줄에 표시됩니다.

    ![](quickstart-images/xs/build-successful.png "빌드 성공")

    오류가 있는 경우 이전 단계를 반복하고 응용 프로그램이 성공적으로 빌드할 때까지 실수를 수정합니다.

13. Mac용 Visual Studio 도구 모음에서 iOS Simulator 안의 응용 프로그램을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](quickstart-images/xs/start.png "Mac용 Visual Studio 도구 모음")
    ![](quickstart-images/xs/phone-result-ios.png "iOS Simulator")

    주의: 전화 통화는 iOS Simulator에서 지원되지 않습니다.

14. **Solution Pad**에서 **Phoneword.Droid** 프로젝트를 선택한 다음, **시작 프로젝트로 설정**을 마우스 오른쪽 단추로 클릭하여 선택합니다.

    ![](quickstart-images/xs/set-startup-project.png "시작 프로젝트로 설정")

15. Mac용 Visual Studio 도구 모음에서 Android 에뮬레이터 안의 응용 프로그램을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](quickstart-images/xs/phone-result-android.png "Android Emulator")

    > [!NOTE]
    > 전화 통화는 디바이스 에뮬레이터에서 지원되지 않습니다.

::: zone-end

멀티 스크린 Xamarin.Forms 응용 프로그램을 완료한 것을 축하합니다. 이 가이드의 [다음 항목](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/deepdive.md)은 Xamarin.Forms를 사용하여 페이지 탐색 및 데이터 바인딩에 대한 이해를 갖추기 위해 이 연습에서 수행된 단계를 검토합니다.

## <a name="related-links"></a>관련 링크

- [Phoneword(샘플)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
- [PhonewordMultiscreen(샘플)](https://developer.xamarin.com/samples/xamarin-forms/PhonewordMultiscreen/)
