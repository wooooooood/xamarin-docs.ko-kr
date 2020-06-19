---
title: 'GTK # 플랫폼 설정'
description: 'Xamarin.Forms이제 GTK # platform에 대 한 미리 보기 지원이 제공 됩니다.'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a5635da9f7c083609ce1e0f120d0613fff9bd77b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198102"
---
# <a name="gtk-platform-setup"></a>GTK # 플랫폼 설정

![미리 보기](~/media/shared/preview.png)

Xamarin.Forms는 이제 GTK # 앱에 대 한 미리 보기를 지원 합니다. GTK #은 GTK + 도구 키트와 다양 한 GNOME 라이브러리를 연결 하는 그래픽 사용자 인터페이스 도구 키트로, Mono 및 .NET을 사용 하 여 완전 한 네이티브 GNOME 그래픽 앱을 개발할 수 있습니다. 이 문서에서는 솔루션에 GTK # 프로젝트를 추가 하는 방법을 보여 줍니다 Xamarin.Forms .

> [!IMPORTANT]
> Xamarin.FormsGTK #에 대 한 지원은 커뮤니티에서 제공 합니다. 자세한 내용은 [ Xamarin.Forms 플랫폼 지원](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support)을 참조 하세요.

시작 하기 전에 새 솔루션을 만들거나 Xamarin.Forms 기존 Xamarin.Forms 솔루션 (예: [**GameOfLife**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife))을 사용 합니다.

> [!NOTE]
> 이 문서에서는 VS2017 및 Mac용 Visual Studio의 솔루션에 GTK # 앱을 추가 하는 데 중점을 둔 반면 Xamarin.Forms , Linux 용 [MonoDevelop](https://www.monodevelop.com/) 에서 수행할 수도 있습니다.

## <a name="adding-a-gtk-app"></a>GTK # 앱 추가

MacOS 및 Linux 용 GTK #은 [Mono](https://www.mono-project.com/download/stable/)의 일부로 설치 됩니다. Gtk [# 설치 관리자](https://www.mono-project.com/download/stable/#download-win)를 사용 하 여 WINDOWS에 gtk # .net을 설치할 수 있습니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

다음 지침에 따라 Windows 데스크톱에서 실행 되는 GTK # 앱을 추가 합니다.

1. Visual Studio 2019의 **솔루션 탐색기** 에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 프로젝트**...를 선택 합니다.

2. **새 프로젝트** 창의 왼쪽에서 **Visual c #** 및 **Windows 클래식 바탕 화면**을 선택 합니다. 프로젝트 형식 목록에서 **클래스 라이브러리 (.NET Framework)** 를 선택 하 고 **프레임 워크** 드롭다운이 .NET Framework 4.7 이상으로 설정 되어 있는지 확인 합니다.

3. **Gtk** 확장을 사용 하 여 프로젝트의 이름 (예: **GameOfLife**)을 입력 합니다. **찾아보기** 단추를 클릭 하 고 다른 플랫폼 프로젝트를 포함 하는 폴더를 선택한 다음 **폴더 선택**을 누릅니다. 그러면 솔루션의 다른 프로젝트와 동일한 디렉터리에 GTK 프로젝트가 배치 됩니다.

    ![새 GTK 프로젝트 추가](gtk-images/win/add-new-project.png "새 GTK 프로젝트 추가")

    **확인** 단추를 클릭 하 여 프로젝트를 만듭니다.

4. **솔루션 탐색기**에서 새 GTK 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리**를 선택 합니다. **찾아보기** 탭을 선택 하 고 **Xamarin.Forms** 3.0 이상의 값을 검색 합니다.

    ![NuGet 패키지를 선택 합니다. Xamarin.Forms](gtk-images/win/select-forms-nuget-package.png "[!를 선택 합니다. OP. NO-LOC (Xamarin.ios)] NuGet 패키지")

    패키지를 선택 하 고 **설치** 단추를 클릭 합니다.

5. 이제를 검색 ** Xamarin.Forms 합니다. Platform.object** 3.0 패키지 이상.

    ![을 선택 Xamarin.Forms 합니다. Platform.object NuGet 패키지](gtk-images/win/select-forms-platform-nuget-package.png "[!를 선택 합니다. OP. NO-LOC (Xamarin.ios)]. Platform.object NuGet 패키지")

    패키지를 선택 하 고 **설치** 단추를 클릭 합니다.

6. **솔루션 탐색기**에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 **솔루션용 NuGet 패키지 관리**를 선택 합니다. **업데이트** 탭과 패키지를 선택 합니다 **Xamarin.Forms** . 모든 프로젝트를 선택 하 고 Xamarin.Forms GTK 프로젝트에서 사용 하는 것과 동일한 버전으로 업데이트 합니다.

7. **솔루션 탐색기**에서 GTK 프로젝트의 **참조** 를 마우스 오른쪽 단추로 클릭 합니다. **참조 관리자** 대화 상자에서 왼쪽에 있는 **프로젝트** 를 선택 하 고 .NET Standard 또는 공유 프로젝트 옆에 있는 확인란을 선택 합니다.

    ![공유 프로젝트 참조](gtk-images/win/reference-shared-project.png "공유 프로젝트 참조")

8. **참조 관리자** 대화 상자에서 **찾아보기** 단추를 누르고 **C:\Program files (x86) \GtkSharp\2.12\lib** 폴더로 이동한 다음 **atk-sharp.dll**, **gdk-sharp.dll**, **glade-sharp.dll**, **glib-sharp.dll**, **gtk-dotnet.dll**, **gtk-sharp.dll** 파일을 선택 합니다.

    ![GTK # 라이브러리 참조](gtk-images/win/reference-gtk-libraries.png "GTK # 라이브러리 참조")

    **확인** 단추를 클릭 하 여 참조를 추가 합니다.

9. GTK 프로젝트에서 **Class1.cs** 의 이름을 **Program.cs**로 바꿉니다.

10. GTK 프로젝트에서 다음 코드와 같이 **Program.cs** 파일을 편집 합니다.

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

    이 코드는 GTK # 및를 초기화 하 고 Xamarin.Forms 응용 프로그램 창을 만든 다음 응용 프로그램을 실행 합니다.

11. **솔루션 탐색기**에서 GTK 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.

12. **속성** 창에서 **응용 프로그램** 탭을 선택 하 고 **출력 형식** 드롭다운을 **Windows 응용 프로그램**으로 변경 합니다.

    ![프로젝트 출력 형식 변경](gtk-images/win/change-project-output-type.png "프로젝트 출력 형식 변경")

13. **솔루션 탐색기**에서 GTK 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **시작 프로젝트로 설정**을 선택 합니다. Windows 바탕 화면에서 Visual Studio 디버거를 사용 하 여 프로그램을 실행 하려면 F5 키를 누릅니다.

    ![GTK # 수명의 게임](gtk-images/win/gtk-gameoflife.png "GTK # 수명의 게임")

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

Mac 데스크톱에서 실행 되는 GTK # 앱을 추가 하려면 다음 지침을 따르세요.

1. Mac용 Visual Studio에서 솔루션을 마우스 오른쪽 단추로 클릭 Xamarin.Forms 하 고 **추가 > 새 프로젝트 추가**...를 선택 합니다.

2. **새 프로젝트** 창에서 **기타 > .net > Gtk # 2.0 프로젝트** 를 선택 하 고 **다음**을 누릅니다.

3. **Gtk** 확장을 사용 하 여 프로젝트의 이름 (예: **GameOfLife**)을 입력 하 고 **만들기**를 누릅니다.

4. **Solution Pad**에서 **패키지 > 패키지 추가** ...를 마우스 오른쪽 단추로 클릭 하 고, GTK 프로젝트에 대해 Xamarin.Forms 3.0 시험판 NuGet 패키지 이상을 추가 합니다.

    ![NuGet 패키지를 선택 합니다. Xamarin.Forms](gtk-images/mac/select-forms-nuget-package.png "[!를 선택 합니다. OP. NO-LOC (Xamarin.ios)] NuGet 패키지")

5. **Solution Pad**에서 패키지를 마우스 오른쪽 단추로 클릭 하 **> 패키지 추가** ...를 마우스 오른쪽 단추로 클릭 하 고를 추가 합니다. Xamarin.Forms Platform.object 3.0 시험판 NuGet 패키지 이상

    ![을 선택 Xamarin.Forms 합니다. Platform.object NuGet 패키지](gtk-images/mac/select-forms-platform-nuget-package.png "[!를 선택 합니다. OP. NO-LOC (Xamarin.ios)]. Platform.object NuGet 패키지")

6. GTK 프로젝트에서 사용 하는 것과 동일한 버전을 사용 하도록 다른 플랫폼 프로젝트를 업데이트 합니다 Xamarin.Forms .

7. **Solution Pad**에서 참조를 마우스 오른쪽 단추로 클릭 하 **> 참조 편집** ...을 마우스 오른쪽 단추로 클릭 하 고 Xamarin.Forms 프로젝트 (.NET Standard 또는 공유 프로젝트)에 대 한 참조를 추가 합니다.

    ![공유 프로젝트 참조](gtk-images/mac/reference-shared-project.png "공유 프로젝트 참조")

8. 다음 코드와 비슷하게 GTK 프로젝트의 **Program.cs** 파일을 편집 합니다.

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

    이 코드는 GTK # 및를 초기화 하 고 Xamarin.Forms 응용 프로그램 창을 만든 다음 응용 프로그램을 실행 합니다.

9. **Solution Pad**에서 GTK 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **시작 프로젝트로 설정**을 선택 합니다.

10. Mac용 Visual Studio 도구 모음에서 **시작** 단추 (재생 단추와 비슷한 삼각형 모양의 단추)를 눌러 앱을 시작 합니다.

    ![GTK # 수명의 게임](gtk-images/mac/gtk-gameoflife.png "GTK # 수명의 게임")

-----

## <a name="next-steps"></a>다음 단계

### <a name="platform-specifics"></a>플랫폼별

Xamarin.FormsXAML 또는 코드에서 응용 프로그램이 실행 되는 플랫폼을 확인할 수 있습니다. 이를 통해 GTK #에서 실행 되는 프로그램 특성을 변경할 수 있습니다. 코드에서의 값을 `Device.RuntimePlatform` `Device.GTK` 상수 ("GTK" 문자열)와 비교 합니다. 일치 하는 항목이 있으면 응용 프로그램이 GTK #에서 실행 됩니다.

XAML에서 태그를 사용 하 여 `OnPlatform` 플랫폼과 관련 된 속성 값을 선택할 수 있습니다.

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

GTK #에 사용할 수 있는 다양 한 테마가 있으며 앱에서 사용할 수 있습니다 Xamarin.Forms .

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>네이티브 양식

네이티브 폼을 사용 하면 Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) 네이티브 프로젝트 (GTK # 프로젝트 포함)에서 파생 된 페이지를 사용할 수 있습니다. 이렇게 하려면 파생 페이지의 인스턴스를 만든 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 다음 확장 메서드를 사용 하 여 네이티브 GTK # 형식으로 변환 합니다 `CreateContainer` .

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

네이티브 폼에 대 한 자세한 내용은 [Native forms](~/xamarin-forms/platform/native-forms.md)를 참조 하세요.

## <a name="issues"></a>문제

이는 미리 보기 이므로 프로덕션이 준비 되지 않은 것으로 간주 됩니다. 현재 구현 상태에 대해서는 [상태](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)를 참조 하 고, 현재 알려진 문제는 [보류 중인 & 알려진 문제](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md)를 참조 하세요.
