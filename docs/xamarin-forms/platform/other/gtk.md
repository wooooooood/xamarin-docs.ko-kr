---
title: 'GTK # 플랫폼 설치'
description: 'Xamarin.Forms를 얻었습니다 GTK # 플랫폼에 대 한 미리 보기 지원'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: 6243f7d90b921207f4dd406a1f33f4d7af40ecfb
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668371"
---
# <a name="gtk-platform-setup"></a>GTK # 플랫폼 설치

![미리 보기](~/media/shared/preview.png)

Xamarin.Forms는 GTK# 앱에 대한 제한적인 지원을 하고 있습니다. GTK #는 GTK + toolkit 및 다양 한 GNOME 라이브러리를 연결 하는 그래픽 사용자 인터페이스 도구 키트는 완전 한 네이티브 GNOME 개발 Mono와.NET을 사용 하 여 그래픽 앱. 이 문서에서는 Xamarin.Forms 솔루션에 GTK # 프로젝트를 추가 하는 방법에 설명 합니다.

시작 하기 전이나, 새 Xamarin.Forms 솔루션을 만드는 예를 들어 기존 Xamarin.Forms 솔루션을 사용 하 여 [ **GameOfLife**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/)합니다.

> [!NOTE]
> 이 문서에서는 Mac 용 VS2017 및 Visual Studio에서 Xamarin.Forms 솔루션에 GTK # 앱을 추가 하는 방법, 해당에서 수행할 수도 있습니다 [MonoDevelop](http://www.monodevelop.com/) Linux에 대 한 합니다.

## <a name="adding-a-gtk-app"></a>GTK # 앱을 추가합니다.

GTK # macos 및 Linux의 일부로 설치 됩니다 [Mono](https://www.mono-project.com/download/stable/)합니다. GTK # for.NET에 설치할 수 있습니다 사용 하 여 Windows를 [GTK # Installer](https://www.mono-project.com/download/stable/#download-win)합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

GTK # 앱을 Windows 데스크톱에서 실행 되는 추가 하려면 이러한 지침을 따릅니다.

1. Visual Studio 2017에서 마우스 오른쪽 단추로 클릭에서 솔루션 이름을 **솔루션 탐색기** 선택한 **추가 > 새 프로젝트...** .

2. 에 **새 프로젝트** 창에서 왼쪽된 선택 **Visual C#** 및 **Windows 클래식 바탕 화면**. 프로젝트 형식 목록에서 선택 **클래스 라이브러리 (.NET Framework)**, 있는지 확인 합니다 **Framework** 드롭다운 최소.NET Framework 4.7로 설정 됩니다.

3. 사용 하 여 프로젝트의 이름을 입력 한 **GTK** 예를 들어 확장 **GameOfLife.GTK**합니다. 클릭 합니다 **찾아보기** 단추를 다른 플랫폼을 포함 하는 폴더를 선택 합니다. 프로젝트 및 키를 눌러 **폴더 선택**합니다. GTK 프로젝트 솔루션의 다른 프로젝트와 동일한 디렉터리에 배치 됩니다이 합니다.

    ![새 GTK 프로젝트를 추가](gtk-images/win/add-new-project.png "새 GTK 프로젝트 추가")

    키를 눌러 합니다 **확인** 프로젝트를 만들려면 단추입니다.

4. 에 **솔루션 탐색기**새 GTK 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **NuGet 패키지 관리**합니다. 선택 된 **찾아보기** 탭을 검색할 **Xamarin.Forms** 3.0 이상.

    ![Xamarin.Forms NuGet 패키지를 선택](gtk-images/win/select-forms-nuget-package.png "Xamarin.Forms NuGet 패키지를 선택 합니다.")

    패키지를 선택 하 고 클릭 합니다 **설치** 단추입니다.

5. 이제 검색 된 **Xamarin.Forms.Platform.GTK** 3.0 패키지 이상.

    ![Xamarin.Forms.Platform.GTK NuGet 패키지를 선택](gtk-images/win/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet 패키지를 선택 합니다.")

    패키지를 선택 하 고 클릭 합니다 **설치** 단추입니다.

6. 에 **솔루션 탐색기**, 솔루션 이름을 마우스 오른쪽 단추로 **솔루션용 NuGet 패키지 관리**합니다. 선택 된 **업데이트** 탭 및 **Xamarin.Forms** 패키지 있습니다. 모든 프로젝트를 선택 하 고 GTK 프로젝트에서 사용 되는 동일한 Xamarin.Forms 버전으로 업데이트 합니다.

7. 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 **참조** GTK 프로젝트에서. 에 **참조 관리자** 대화 상자에서 **프로젝트** 왼쪽 및 확인 하려면.NET Standard 또는 공유 프로젝트 옆의 확인란:

    ![공유 프로젝트 참조](gtk-images/win/reference-shared-project.png "공유 프로젝트 참조")

8. 에 **참조 관리자** 대화 상자에서 키를 누릅니다를 **찾아보기** 단추를 이동 하는 **C:\Program Files (x86)\GtkSharp\2.12\lib** 폴더를 선택은  **atk sharp.dll**, **gdk sharp.dll**합니다 **glade sharp.dll**를 **glib sharp.dll**, **gtk-dotnet.dll**하십시오 **gtk sharp.dll** 파일입니다.

    ![GTK # 라이브러리를 참조할](gtk-images/win/reference-gtk-libraries.png "GTK # 라이브러리 참조")

    키를 눌러 합니다 **확인** 참조를 추가 하는 단추입니다.

9. GTK 프로젝트에서 이름 바꾸기 **Class1.cs** 하 **Program.cs**합니다.

10. GTK 프로젝트에서 편집 합니다 **Program.cs** 파일에 다음 코드는 유사 합니다.

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    이 코드 GTK # 및 Xamarin.Forms를 초기화 합니다. 응용 프로그램 창을 만들고 앱을 실행 합니다.

11. 에 **솔루션 탐색기**GTK 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **속성**합니다.

12. 에 **속성** 창에서를 **응용 프로그램** 탭을 변경 합니다 **출력 형식** 드롭다운을 **Windows 응용 프로그램**.

    ![프로젝트 출력 형식을 변경](gtk-images/win/change-project-output-type.png "프로젝트 출력 형식 변경")

13. 에 **솔루션 탐색기**GTK 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **시작 프로젝트로 설정**합니다. F5 키를 눌러 Visual Studio 디버거를 사용 하 여 Windows 바탕 화면에서 프로그램을 실행 하려면:

    ![GTK # 게임 수명의](gtk-images/win/gtk-gameoflife.png "GTK # 게임 수명")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

앱을 추가 하려면 GTK # Mac 데스크톱에서 실행 되는 이러한 지침을 따릅니다.

1. Mac 용 Visual Studio에서 Xamarin.Forms 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 프로젝트 추가...** .

2. 에 **새 프로젝트** 창 **기타 >.NET > Gtk # 2.0 프로젝트** 키를 누릅니다 **다음**합니다.

3. 사용 하 여 프로젝트의 이름을 입력 한 **GTK** 예를 들어 확장 **GameOfLife.GTK**, 키를 누릅니다 **만들기**.

4. 에 **Solution Pad**를 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가...**  는 GTK에 대 한 Xamarin.Forms 3.0 시험판 NuGet 패키지를 추가한 프로젝트를 큽니다.

    ![Xamarin.Forms NuGet 패키지를 선택](gtk-images/mac/select-forms-nuget-package.png "Xamarin.Forms NuGet 패키지를 선택 합니다.")

5. 에 **Solution Pad**를 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가...**  는 GTK에 대 한 프로젝트를 마우스 Xamarin.Forms.Platform.GTK 3.0 시험판 NuGet 패키지 추가 이상.

    ![Xamarin.Forms.Platform.GTK NuGet 패키지를 선택](gtk-images/mac/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet 패키지를 선택 합니다.")

6. GTK 프로젝트에서 사용 되는 동일한 Xamarin.Forms 버전을 사용 하도록 다른 플랫폼 프로젝트를 업데이트 합니다.

7. 에 **Solution Pad**를 마우스 오른쪽 단추로 클릭 **참조 > 참조 편집...**  는 GTK에 대 한 프로젝트 및 Xamarin.Forms 프로젝트 (.NET Standard 또는 공유 프로젝트)에 대 한 참조를 추가 합니다.

    ![공유 프로젝트 참조](gtk-images/mac/reference-shared-project.png "공유 프로젝트 참조")

8. 편집 된 **Program.cs** 한다는는 다음 코드와 유사 하므로 GTK 프로젝트의 파일:

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    이 코드 GTK # 및 Xamarin.Forms를 초기화 합니다. 응용 프로그램 창을 만들고 앱을 실행 합니다.

9. 에 **Solution Pad**GTK 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **시작 프로젝트로 설정**합니다.

10. 키를 눌러 Mac 도구 모음에 대 한 Visual studio에서 **시작** 단추 (재생 단추와 비슷한 삼각형 모양의 단추) 앱을 시작 합니다.

    ![GTK # 게임 수명의](gtk-images/mac/gtk-gameoflife.png "GTK # 게임 수명")

-----

## <a name="next-steps"></a>다음 단계

### <a name="platform-specifics"></a>플랫폼별

XAML 또는 코드에서 Xamarin.Forms 응용 프로그램에서 실행 중인 플랫폼을 확인할 수 있습니다. 이 옵션을 사용 하면 GTK #에서 실행 되는 경우 프로그램 특성을 변경할 수 있습니다. 코드에서의 값을 비교 `Device.RuntimePlatform` 사용 하 여를 `Device.GTK` 상수 ("GTK" 문자열 같음). 일치 하는 경우 응용 프로그램 GTK #에서 실행 됩니다.

XAML을 사용할 수 있습니다는 `OnPlatform` 플랫폼 특정 속성 값을 선택 하는 태그:

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="GTK" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="application-icon"></a>애플리케이션 아이콘

시작 시 앱 아이콘을 설정할 수 있습니다.

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>테마

GTK #에서 사용할 수 있는 다양 한 테마 가지 및 Xamarin.Forms 앱에서 사용할 수 있습니다.

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>네이티브 양식

네이티브 양식 허용 Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-GTK # 프로젝트를 비롯 한 네이티브 프로젝트에서 사용할 수 있도록 페이지를 파생 합니다. 인스턴스를 만들어이 수행할 수 있습니다 합니다 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-파생 페이지 및 네이티브 GTK # 형식을 사용 하 여 변환 된 `CreateContainer` 확장 메서드:

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

기본 형식에 대 한 자세한 내용은 참조 하세요. [네이티브 Forms](~/xamarin-forms/platform/native-forms.md)합니다.

## <a name="issues"></a>문제

이것이 미리 하므로 해야 프로덕션을 준비 하는 것은 아닙니다. 현재 구현 상태를 참조 하세요 [상태](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md), 및 현재 알려진된 문제를 참조 하세요. [보류 중 및 알려진 문제](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md)합니다.
