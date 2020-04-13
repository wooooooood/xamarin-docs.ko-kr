---
title: WPF 플랫폼 설정
description: Xamarin.Forms는 이제 WPF 플랫폼에 대한 미리 보기 지원을 제공합니다.
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/09/2020
ms.openlocfilehash: c9ec9fec2391d7d7a24f97f2ec20208a7d69dbc1
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80992330"
---
# <a name="wpf-platform-setup"></a>WPF 플랫폼 설정

![미리 보기](~/media/shared/preview.png)

Xamarin.Forms는 이제 WPF(Windows 프레젠테이션 파운데이션)에 대한 미리 보기 지원을 제공합니다. 이 문서에서는 Xamarin.Forms 솔루션에 WPF 프로젝트를 추가하는 방법을 보여 줍니다.

> [!IMPORTANT]
> Xamarin.Forms WPF에 대 한 지원은 지역 사회에 의해 제공 됩니다. 자세한 내용은 [Xamarin.Forms 플랫폼 지원](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support)을 참조하십시오.

시작하기 전에 Visual Studio 2019에서 새 Xamarin.Forms 솔루션을 만들거나 기존 Xamarin.Forms 솔루션(예: [**BoxViewClock)을**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock)사용합니다. Windows의 Xamarin.Forms 솔루션에만 WPF 앱을 추가할 수 있습니다.

## <a name="add-a-wpf-application"></a>WPF 응용 프로그램 추가

다음 지침을 따라 Windows 7, 8 및 10개의 데스크톱에서 실행되는 WPF 응용 프로그램을 추가합니다.

1. Visual Studio 2019에서 **솔루션 탐색기에서** 솔루션 이름을 마우스 오른쪽 단추로 클릭하고 **새 프로젝트 추가를**> 선택합니다.

2. 새 **프로젝트 추가** 창에서 **언어** 드롭다운에서 **C#을** **선택하고, 플랫폼에서** **Windows를** 선택하고, **프로젝트 유형** 드롭다운에서 **데스크톱을** 선택합니다. 프로젝트 유형 목록에서 **WPF 앱(.NET 프레임워크)을 선택합니다.**

    ![새 WPF 프로젝트 추가](wpf-images/add-project.png "새 WPF 프로젝트 추가")

    **다음** 버튼을 누릅니다.

3. 새 **프로젝트 구성** 창에서 **WPF** 확장이 있는 프로젝트의 이름을 입력합니다(예: **BoxViewClock.WPF)** **찾아보기** 단추를 클릭하고 **BoxViewClock** 폴더를 선택하고 **폴더 선택을** 눌러 WPF 프로젝트를 솔루션의 다른 프로젝트와 동일한 디렉토리에 넣습니다.

    ![새 WPF 프로젝트 추가](wpf-images/configure-project.png "새 WPF 프로젝트 추가")

    **만들기** 버튼을 눌러 프로젝트를 만듭니다.

4. 솔루션 **탐색기에서**새 **BoxViewClock.WPF** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리를 선택합니다.** **찾아보기** 탭을 선택하고 **Xamarin.Forms.Platform.WPF를**검색합니다.

    ![NuGet 패키지 선택](wpf-images/select-nuget-package.png "NuGet 패키지 선택")

    패키지를 선택하고 **설치** 버튼을 클릭합니다.

5. **솔루션 탐색기에서** 솔루션 이름을 마우스 오른쪽 단추로 클릭하고 **솔루션에 대한 NuGet 패키지 관리를 선택합니다.**. . **업데이트** 탭을 선택한 다음 **Xamarin.Forms** 패키지를 선택합니다. 모든 프로젝트를 선택하고 동일한 Xamarin.Forms 버전으로 업데이트합니다.

    ![NuGet 패키지 업데이트](wpf-images/update-nuget-package.png "NuGet 패키지 업데이트")

6. WPF 프로젝트에서 **참조를** 마우스 오른쪽 단추로 클릭하고 **참조 추가를 선택합니다.**. . 참조 **관리자** 대화 상자에서 왼쪽의 **프로젝트를** 선택하고 **BoxViewClock** 프로젝트 옆에 있는 확인란을 선택합니다.

    ![공유 프로젝트 참조](wpf-images/reference-shared-project.png "공유 프로젝트 참조")

    **확인** 버튼을 누릅니다.

7. WPF 프로젝트의 **MainWindow.xaml** 파일을 편집합니다. `Window` 태그에서 **Xamarin.Forms.Platform.WPF** 어셈블리 및 네임스페이스에 대한 XML 네임스페이스 선언을 추가합니다.

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    이제 `Window` 태그를 로 `wpf:FormsApplicationPage`변경합니다. `Title` 응용 프로그램 이름으로 설정을 변경합니다(예: **BoxViewClock**. 완성된 XAML 파일은 다음과 같아야 합니다.

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"            
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>

        </Grid>
    </wpf:FormsApplicationPage>
    ```

8. WPF 프로젝트의 **MainWindow.xaml.cs** 파일을 편집합니다. 두 개의 `using` 새 지시문을 추가합니다.

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    의 `MainWindow` 기본 클래스를 `Window` `FormsApplicationPage`에서 로 변경합니다. 호출 `InitializeComponent` 후 다음 두 문을 추가합니다.

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```

    주석 및 사용되지 `using` 않는 지시문을 제외하고 전체 **MainWindows.xaml.cs** 파일은 다음과 같아야 합니다.

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

9. **솔루션 탐색기에서** WPF 프로젝트를 마우스 오른쪽 단추로 클릭하고 **시작 프로젝트로 설정을**선택합니다. F5를 눌러 Windows 데스크톱에서 Visual Studio 디버거를 사용하여 프로그램을 실행합니다.

    ![WPF 박스뷰 클럭](wpf-images/wpf-boxviewclock.png "WPF 박스뷰 클럭" )

## <a name="platform-specifics"></a>플랫폼 세부 사항

Xamarin.Forms 응용 프로그램이 코드 또는 XAML에서 실행 중인 플랫폼을 확인할 수 있습니다. 이렇게 하면 WPF에서 실행 중인 프로그램 특성을 변경할 수 있습니다. 코드에서 `Device.WPF` 상수의 `Device.RuntimePlatform` 값을 비교합니다(문자열 "WPF"와 동일). 일치하는 응용 프로그램이 WPF에서 실행되고 있습니다.

XAML에서 `OnPlatform` 태그를 사용하여 플랫폼과 관련된 속성 값을 선택할 수 있습니다.

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

## <a name="window-size"></a>창 크기

WPF **MainWindow.xaml** 파일에서 창의 초기 크기를 조정할 수 있습니다.

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>문제

미리 보기이므로 모든 것이 프로덕션 준비가 된 것은 아닙니다. Xamarin.Forms의 모든 NuGet 패키지가 WPF에 사용할 수 있는 것은 아니며 일부 기능이 완전히 작동하지 않을 수 있습니다.

## <a name="related-video"></a>관련 동영상

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms 3.0 WPF 지원 비디오**
