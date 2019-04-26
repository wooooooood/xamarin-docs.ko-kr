---
title: WPF 플랫폼 설치
description: Xamarin.Forms를 얻었습니다 WPF 플랫폼에 대 한 미리 보기 지원
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2018
ms.openlocfilehash: 2bef13e7f465dd213649f88deb572eb661895250
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61246339"
---
# <a name="wpf-platform-setup"></a>WPF 플랫폼 설치

![미리 보기](~/media/shared/preview.png)

Xamarin.Forms는 Windows Presentation Foundation (WPF)에 대 한 미리 보기 지원이 되었습니다. 이 문서에서는 Xamarin.Forms 솔루션에 WPF 프로젝트를 추가 하는 방법에 설명 합니다.

시작 하기 전이나, Visual Studio 2019에서 새 Xamarin.Forms 솔루션을 만드는 예를 들어 기존 Xamarin.Forms 솔루션을 사용 하 여 [ **BoxViewClock**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/)합니다. Windows에서 Xamarin.Forms 솔루션에 WPF 앱만 추가할 수 있습니다.

## <a name="add-a-wpf-project-to-a-xamarinforms-app-with-xamarinuniversity"></a>WPF 프로젝트 Xamarin.University 사용 하 여 Xamarin.Forms 앱에 추가

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms 3.0 WPF 지원이, [Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-wpf-app"></a>WPF 앱 추가

Windows 7, 8 및 10 데스크톱에서 실행 되는 WPF 앱을 추가 하려면 이러한 지침을 따릅니다.

1. 솔루션 이름을 마우스 오른쪽 단추로 Visual Studio 2019에는 **솔루션 탐색기** 선택한 **추가 > 새 프로젝트...** .

2. 에 **새 프로젝트** 창에서 왼쪽된 선택 **Visual C#** 및 **Windows 클래식 바탕 화면**. 프로젝트 형식 목록에서 선택 **WPF 앱 (.NET Framework)** 합니다. 

3. 사용 하 여 프로젝트의 이름을 입력 한 **WPF** 확장명을 예를 들어 **BoxViewClock.WPF**합니다. 클릭 합니다 **찾아보기** 단추를 선택 합니다 **BoxViewClock** 폴더를 누릅니다 **폴더 선택**합니다. WPF 프로젝트를 솔루션의 다른 프로젝트와 동일한 디렉터리에 배치 됩니다이 합니다.

    ![새 WPF 프로젝트에 추가](wpf-images/add-new-project.png "새 WPF 프로젝트에 추가")

    프로젝트를 만들려면 확인 키를 누릅니다.

4. 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 새를 클릭 **BoxViewClock.WPF** 프로젝트를 마우스 **NuGet 패키지 관리**합니다. 선택 합니다 **찾아보기** 탭을 클릭 합니다 **시험판 포함** 확인란을 검색 **Xamarin.Forms**합니다.

    ![NuGet 패키지를 선택](wpf-images/select-nuget-package.png "NuGet 패키지를 선택 합니다.")

    패키지 클릭을 선택 합니다 **설치** 단추입니다.

5. 이제 검색할 **Xamarin.Forms.Platform.WPF** 패키징하고 하나를 설치 합니다. Microsoft에서 패키지를 해야 합니다.

6. 솔루션 이름을 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** 선택한 **솔루션용 NuGet 패키지 관리**합니다. 선택 된 **업데이트** 탭 및 **Xamarin.Forms** 패키지 있습니다. 모든 프로젝트를 선택 하 고 동일한 Xamarin.Forms 버전으로 업데이트 합니다.

    ![NuGet 패키지를 업데이트할](wpf-images/update-nuget-package.png "NuGet 패키지를 업데이트 합니다.") 

7. WPF 프로젝트에서 마우스 오른쪽 단추로 클릭 **참조가**합니다. 에 **참조 관리자** 대화 상자에서 **프로젝트** 왼쪽 및 확인 하려면 옆의 확인란을 **BoxViewClock** 프로젝트:

    ![공유 프로젝트 참조](wpf-images/reference-shared-project.png "공유 프로젝트 참조")

8. 편집 된 **MainWindow.xaml** WPF 프로젝트의 파일입니다. 에 `Window` 태그에 대 한 XML 네임 스페이스 선언을 추가 합니다 **Xamarin.Forms.Platform.WPF** 어셈블리 및 네임 스페이스:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    이제 변경 합니다 `Window` 태그를 `wpf:FormsApplicationPage`입니다. 변경 된 `Title` 응용 프로그램의 예를 들어 이름으로 설정 **BoxViewClock**합니다. 완료 된 XAML 파일은 다음과 같이 표시 됩니다.

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

9. 편집 된 **MainWindow.xaml.cs** WPF 프로젝트의 파일입니다. 새로 추가 하는 두 개의 `using` 지시문:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    기본 클래스를 변경 `MainWindow` 에서 `Window` 에 `FormsApplicationPage`입니다. 다음의 `InitializeComponent` 호출, 다음 두 명령문을 추가 합니다.

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    주석을 제외 하 고 사용 하지 않는 `using` 지시문, 전체 **MainWindows.xaml.cs** 파일 같이 표시 됩니다.

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

10. WPF 프로젝트를 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** 선택한 **시작 프로젝트로 설정**합니다. F5 키를 눌러 Visual Studio 디버거를 사용 하 여 Windows 바탕 화면에서 프로그램을 실행 하려면:

    ![WPF BoxView 시계](wpf-images/wpf-boxviewclock.png "WPF BoxView 시계" )

## <a name="next-steps"></a>다음 단계

### <a name="platform-specifics"></a>플랫폼별

Xamarin.Forms 응용 프로그램 코드 또는 XAML에서에서 실행 중인 플랫폼을 확인할 수 있습니다. 이 옵션을 사용 하면 WPF에서 실행 되는 경우 프로그램 특성을 변경할 수 있습니다. 코드에서의 값을 비교 `Device.RuntimePlatform` 사용 하 여를 `Device.WPF` 상수 ("WPF" 문자열 같음). 일치 하는 경우 응용 프로그램은 WPF에서 실행 됩니다.

XAML을 사용할 수 있습니다는 `OnPlatform` 플랫폼 특정 속성 값을 선택 하는 태그:

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

WPF 창의 초기 크기를 조정할 수 있습니다 **MainWindow.xaml** 파일:

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>문제

이것이 미리 하므로 해야 프로덕션을 준비 하는 것은 아닙니다. Xamarin.Forms에 대 한 일부 NuGet 패키지는 WPF에 대 한 준비 하 고 일부 기능이 완벽 하 게 작동 하지 않을 수 있습니다.

