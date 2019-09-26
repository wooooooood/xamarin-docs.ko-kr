---
title: WPF 플랫폼 설정
description: 이제 xamarin.ios는 WPF 플랫폼에 대 한 미리 보기를 지원 합니다.
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2018
ms.openlocfilehash: 8fcad4799cd53892106b3e221cff0dfbc737e10d
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "70760048"
---
# <a name="wpf-platform-setup"></a>WPF 플랫폼 설정

![미리 보기](~/media/shared/preview.png)

이제 xamarin.ios는 WPF (Windows Presentation Foundation)에 대 한 미리 보기를 지원 합니다. 이 문서에서는 Xamarin.ios 솔루션에 WPF 프로젝트를 추가 하는 방법을 보여 줍니다.

시작 하기 전에 Visual Studio 2019에서 새 Xamarin.ios 솔루션을 만들거나 기존 Xamarin.ios 솔루션 (예: [**Boxviewclock**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock))을 사용 합니다. WPF 앱은 Windows의 Xamarin.ios 솔루션에만 추가할 수 있습니다.

## <a name="add-a-wpf-project-to-a-xamarinforms-app-with-xamarinuniversity"></a>Xamarin.ios를 사용 하 여 Xamarin.ios 앱에 WPF 프로젝트 추가

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.ios 3.0 WPF 지원 비디오**

## <a name="adding-a-wpf-app"></a>WPF 앱 추가

Windows 7, 8 및 10 데스크톱에서 실행 되는 WPF 앱을 추가 하려면 다음 지침을 따르세요.

1. Visual Studio 2019의 **솔루션 탐색기** 에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 프로젝트**...를 선택 합니다.

2. **새 프로젝트** 창의 왼쪽에서 **비주얼 C#**  및 **Windows 클래식 바탕 화면**을 선택 합니다. 프로젝트 형식 목록에서 **WPF 앱 (.NET Framework)** 을 선택 합니다. 

3. **Wpf** 확장명을 사용 하 여 프로젝트의 이름을 입력 합니다 (예: **BOXVIEWCLOCK. wpf**). **찾아보기** 단추를 클릭 하 고 **boxviewclock** 폴더를 선택 하 고 **폴더 선택**을 누릅니다. 이렇게 하면 WPF 프로젝트가 솔루션의 다른 프로젝트와 동일한 디렉터리에 배치 됩니다.

    ![새 WPF 프로젝트 추가](wpf-images/add-new-project.png "새 WPF 프로젝트 추가")

    확인을 눌러 프로젝트를 만듭니다.

4. **솔루션 탐색기**에서 새 **BOXVIEWCLOCK. WPF** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리**를 선택 합니다. **찾아보기** 탭을 선택 하 고 **시험판 포함** 확인란을 클릭 한 다음 **xamarin.ios**를 검색 합니다.

    ![NuGet 패키지를 선택 합니다] . (wpf-images/select-nuget-package.png "NuGet 패키지를 선택 합니다") .

    해당 패키지를 선택 하 고 **설치** 단추를 클릭 합니다.

5. 이제 **xamarin.ios** 패키지를 검색 하 여 설치 합니다. 패키지가 Microsoft에 있는지 확인 하세요.

6. **솔루션 탐색기** 에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 **솔루션용 NuGet 패키지 관리**를 선택 합니다. **업데이트** 탭과 **Xamarin Forms** 패키지를 선택 합니다. 모든 프로젝트를 선택 하 고 동일한 Xamarin.ios 버전으로 업데이트 합니다.

    ![NuGet 패키지 업데이트](wpf-images/update-nuget-package.png "NuGet 패키지 업데이트") 

7. WPF 프로젝트에서 **참조**를 마우스 오른쪽 단추로 클릭 합니다. **참조 관리자** 대화 상자의 왼쪽에서 **프로젝트** 를 선택 하 고 **boxviewclock** 프로젝트 옆에 있는 확인란을 선택 합니다.

    ![공유 프로젝트 참조](wpf-images/reference-shared-project.png "공유 프로젝트 참조")

8. WPF 프로젝트의 **mainwindow.xaml** 파일을 편집 합니다. 태그에서 xamarin.ios 어셈블리와 네임 스페이스에 대 한 XML 네임 스페이스 선언을 추가 합니다. `Window`

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    이제 태그를 `Window` 로 `wpf:FormsApplicationPage`변경 합니다. 설정을 응용 `Title` 프로그램의 이름 (예: **boxviewclock**)으로 변경 합니다. 완성 된 XAML 파일은 다음과 같습니다.

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>
        
        </Grid>
    </wpf:FormsApplicationPage>
    ```

9. WPF 프로젝트의 **MainWindow.xaml.cs** 파일을 편집 합니다. 두 개의 새 `using` 지시문을 추가 합니다.

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    의 `MainWindow` 기본 클래스를에서 `Window` 로 `FormsApplicationPage`변경 합니다. `InitializeComponent` 호출을 수행 하 고 다음 두 문을 추가 합니다.

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    주석과 사용 하지 않는 `using` 지시문을 제외 하 고 전체 **MainWindows.xaml.cs** 파일은 다음과 같습니다.

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;

    namespace BoxViewClock.WPF
    {
        public partial class MainWindow : FormsApplicationPage
        {
            public MainWindow()
            {
                InitializeComponent();

                Forms.Init();
                LoadApplication(new BoxViewClock.App());
            }
        }
    }
    ```

10. **솔루션 탐색기** 에서 WPF 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **시작 프로젝트로 설정**을 선택 합니다. Windows 바탕 화면에서 Visual Studio 디버거를 사용 하 여 프로그램을 실행 하려면 F5 키를 누릅니다.

    ![WPF BoxView 클록] (wpf-images/wpf-boxviewclock.png "WPF BoxView 클록" )

## <a name="next-steps"></a>다음 단계

### <a name="platform-specifics"></a>플랫폼별

코드 또는 XAML에서 Xamarin Forms 응용 프로그램이 실행 되는 플랫폼을 확인할 수 있습니다. 이렇게 하면 WPF에서 실행 될 때 프로그램 특성을 변경할 수 있습니다. 코드에서의 `Device.RuntimePlatform` 값을 `Device.WPF` 상수 ("WPF" 문자열)와 비교 합니다. 일치 하는 항목이 있으면 응용 프로그램이 WPF에서 실행 됩니다.

XAML에서 `OnPlatform` 태그를 사용 하 여 플랫폼과 관련 된 속성 값을 선택할 수 있습니다.

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="WPF" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="window-size"></a>창 크기

WPF **mainwindow.xaml** 파일에서 창의 초기 크기를 조정할 수 있습니다.

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>문제

이는 미리 보기 이므로 프로덕션이 준비 되지 않은 것으로 간주 됩니다. Xamarin에 대 한 모든 NuGet 패키지는 WPF에 대해 준비 되는 것이 아니라 일부 기능이 제대로 작동 하지 않을 수 있습니다.
