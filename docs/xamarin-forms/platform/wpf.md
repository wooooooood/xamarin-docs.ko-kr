---
title: WPF 플랫폼 설치 프로그램
description: Xamarin.Forms에 WPF 플랫폼에 대 한 미리 보기 지원
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/05/2018
ms.openlocfilehash: 51aad1643709a96c56ccad8187a53f47a65a9dac
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
---
# <a name="wpf-platform-setup"></a>WPF 플랫폼 설치 프로그램

![미리 보기](~/media/shared/preview.png)

Xamarin.Forms에는 Windows Presentation Foundation (WPF)에 대 한 미리 보기 지원이 되었습니다. 이 문서는 Xamarin.Forms 솔루션에 WPF 프로젝트를 추가 하는 방법을 보여 줍니다.

시작 하기 전이나, Visual Studio 2017에서 새 Xamarin.Forms 솔루션을 만들어 예를 들어 기존 Xamarin.Forms 솔루션을 사용 하 여 [ **BoxViewClock**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/)합니다. Windows에서 Xamarin.Forms 솔루션에 WPF 앱만 추가할 수 있습니다.

## <a name="adding-a-wpf-app"></a>WPF 응용 프로그램 추가

Windows 7, 8 및 10 데스크톱에서 실행 되는 WPF 응용 프로그램을 추가 하려면 다음이 지침을 따릅니다.

1. 솔루션 이름을 마우스 오른쪽 단추로 Visual Studio 2017 년에 **솔루션 탐색기** 선택 **추가 > 새 프로젝트...** .

2. 에 **새 프로젝트** 창 왼쪽된 선택에서 **Visual C#** 및 **클래식 Windows 데스크톱**합니다. 프로젝트 형식 목록에서 선택 **WPF 응용 프로그램 (.NET Framework)**합니다. 

3. 사용 하 여 프로젝트에 대 한 이름을 입력 한 **WPF** 확장, 예를 들어 **BoxViewClock.WPF**합니다. 클릭는 **찾아보기** 단추를 선택는 **BoxViewClock** 폴더 및 키를 눌러 **폴더 선택**합니다. 솔루션의 다른 프로젝트와 동일한 디렉터리에 WPF 프로젝트를 나옵니다.

    ![새 WPF 프로젝트에 추가](wpf-images/add-new-project.png "새 WPF 프로젝트에 추가")

    프로젝트를 만들려면 확인 키를 누릅니다.

4. 에 **솔루션 탐색기**, 오른쪽 새를 클릭 **BoxViewClock.WPF** 프로젝트를 마우스 선택 **NuGet 패키지 관리**합니다. 선택 된 **찾아보기** 탭을 클릭는 **시험판 포함** 확인란을 검색 한 **Xamarin.Forms**합니다.

    ![NuGet 패키지를 선택](wpf-images/select-nuget-package.png "NuGet 패키지를 선택 합니다.")

    클릭 하 고 패키지를 선택은 **설치** 단추입니다.

5. 지금 검색 **Xamarin.Forms.Platform.WPF** 패키지 및 해당 한도 설치 합니다. Microsoft 패키지 인지 확인 하십시오!

6. 솔루션 이름을 마우스 오른쪽 단추로 클릭는 **솔루션 탐색기** 선택 **솔루션에 대 한 NuGet 패키지 관리**합니다. 선택 된 **업데이트** 탭 및 **Xamarin.Forms** 패키지 합니다. 모든 프로젝트를 선택 하 고 동일한 Xamarin.Forms 버전으로 업데이트할:

    ![NuGet 패키지를 업데이트](wpf-images/update-nuget-package.png "NuGet 패키지를 업데이트 합니다.") 

7. WPF 프로젝트에서 마우스 오른쪽 단추로 클릭 **참조**합니다. 에 **참조 관리자** 대화 상자에서 **프로젝트** 왼쪽과 확인 하려면 옆의 확인란에는 **BoxViewClock** 프로젝트:

    ![공유 프로젝트 참조](wpf-images/reference-shared-project.png "공유 프로젝트 참조")

8. 편집 된 **MainWindow.xaml** WPF 프로젝트의 파일입니다. 에 `Window` 태그에 대 한 XML 네임 스페이스 선언을 추가 **Xamarin.Forms.Platform.WPF** 어셈블리 및 네임 스페이스:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    이제 변경 하는 `Window` 태그를 `wpf:FormsApplcationPage`합니다. 변경 된 `Title` 를 응용 프로그램의 예를 들어 이름으로 설정 **BoxViewClock**합니다. 완료 된 XAML 파일은 다음과 같이 표시 됩니다.

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

9. 편집 된 **MainWindow.xaml.cs** WPF 프로젝트의 파일입니다. 새로운 두 개의 추가 `using` 지시문:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    기본 클래스를 변경 `MainWindow` 에서 `Window` 를 `FormsApplicationPage`합니다. 다음은 `InitializeComponent` 호출, 다음 두 문을 추가 합니다.

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    설명을 제외 하 고 사용 하지 않는 `using` 지시문, 전체 **MainWindows.xaml.cs** 파일은 다음과 같이 표시 됩니다.

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

10. WPF 프로젝트를 마우스 오른쪽 단추로 클릭는 **솔루션 탐색기** 선택 **시작 프로젝트로 설정**합니다. F5 키를 눌러 Visual Studio 디버거와 함께 Windows 바탕 화면에서 프로그램을 실행 하려면:

    ![WPF BoxView 클록](wpf-images/wpf-boxviewclock.png "WPF BoxView 클록" )

## <a name="next-steps"></a>다음 단계

### <a name="platform-specifics"></a>플랫폼 세부 사항

Xamarin.Forms 응용 프로그램은 코드 또는 XAML에서 실행 중인 플랫폼을 확인할 수 있습니다. 이 옵션을 사용 하면 WPF에서 실행 되는 프로그램 특성을 변경할 수 있습니다. 코드에서의 값을 비교 `Device.RuntimePlatform` 와 `Device.WPF` 상수 (= "WPF" 문자열)입니다. WPF에서 응용 프로그램 실행 되는 일치 하는 경우.

XAML에서 사용할 수 있습니다는 `OnPlatform` 태그 플랫폼에 특정 속성 값을 선택 합니다.

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

WPF에서 창의 처음 크기를 조정할 수 **MainWindow.xaml** 파일:

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>문제

이것은 하지를 프로덕션 준비가 될 때 이므로 미리 보기입니다. Xamarin.Forms에 대 한 NuGet 패키지를 일부 WPF에 대 한 준비가 되었습니다. 및 일부 기능이 완벽 하 게 작동 하는지 확인 합니다.

