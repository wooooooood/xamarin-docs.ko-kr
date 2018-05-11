---
title: 'GTK # 플랫폼 설치 프로그램'
description: 'Xamarin.Forms에 GTK # 플랫폼에 대 한 미리 보기 지원'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: 275ec851a2fd8e96adecfeca5daf6a66add7bd92
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/10/2018
---
# <a name="gtk-platform-setup"></a>GTK # 플랫폼 설치 프로그램

![미리 보기](~/media/shared/preview.png)

Xamarin.Forms는 GTK # 응용 프로그램에 대 한 미리 보기 지원이 되었습니다. GTK # 하는 GTK + toolkit 및 다양 한 GNOME 라이브러리에 연결 하는 그래픽 사용자 인터페이스 도구 키트 모노 및.NET을 사용 하 여 완전 한 네이티브 GNONE 그래픽 앱 개발. 이 문서는 Xamarin.Forms 솔루션을 GTK # 프로젝트를 추가 하는 방법을 보여 줍니다.

시작 하기 전이나, 새 Xamarin.Forms 솔루션을 만드는 예를 들어 기존 Xamarin.Forms 솔루션을 사용 하 여 [ **GameOfLife**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/)합니다.

> [!NOTE]
> 이 문서는 Mac 용 VS2017 및 Visual Studio에서 Xamarin.Forms 솔루션을 GTK # 응용 프로그램을 추가 하는 방법에 중점을 두고 동안도 수행할 수 있습니다에 [MonoDevelop](http://www.monodevelop.com/) Linux 용입니다.

## <a name="adding-a-gtk-app"></a>GTK # 앱 추가

GTK # macOS에 대 한 및 Linux의 일부로 설치 되어 [모노](http://www.mono-project.com/download/stable/)합니다. GTK # for.net에 설치할 수 있는 창에서 [GTK # Installer](http://www.mono-project.com/download/stable/#download-win)합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Windows 바탕 화면에서 실행 되는 GTK # 앱을 추가 하려면 다음이 지침을 따릅니다.

1. 솔루션 이름을 마우스 오른쪽 단추로 Visual Studio 2017에 **솔루션 탐색기** 선택 **추가 > 새 프로젝트...** .

2. 에 **새 프로젝트** 창 왼쪽된 선택에서 **Visual C#** 및 **클래식 Windows 데스크톱**합니다. 프로젝트 형식 목록에서 선택 **클래스 라이브러리 (.NET Framework)**, 되어 있는지 확인 하 고는 **프레임 워크** 드롭 다운.NET Framework 4.7 이상으로 설정 됩니다.

3. 사용 하 여 프로젝트에 대 한 이름을 입력 한 **GTK** 예를 들어 확장 **GameOfLife.GTK**합니다. 클릭는 **찾아보기** 단추를 다른 플랫폼을 포함 하는 폴더를 선택 합니다. 프로젝트 및 키를 눌러 **폴더 선택**합니다. 솔루션의 다른 프로젝트와 동일한 디렉터리에 GTK 프로젝트를 나옵니다.

    ![새로운 GTK 프로젝트에 추가](gtk-images/win/add-new-project.png "새로운 GTK 프로젝트에 추가")

    키를 눌러는 **확인** 단추 프로젝트를 만듭니다.

4. 에 **솔루션 탐색기**새 GTK 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **NuGet 패키지 관리**합니다. 선택은 **찾아보기** 탭을 클릭는 **Include 시험판** 확인란을 검색 한 **Xamarin.Forms** 3.0 이상이 합니다.

    ![Xamarin.Forms NuGet 패키지를 선택](gtk-images/win/select-forms-nuget-package.png "Xamarin.Forms NuGet 패키지를 선택 합니다.")

    패키지를 선택 하 고 클릭는 **설치** 단추입니다.

5. 지금 검색는 **Xamarin.Forms.Platform.GTK** 3.0 패키지 큽니다.

    ![Xamarin.Forms.Platform.GTK NuGet 패키지를 선택](gtk-images/win/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet 패키지를 선택 합니다.")

    패키지를 선택 하 고 클릭는 **설치** 단추입니다.

6. 에 **솔루션 탐색기**솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **솔루션에 대 한 NuGet 패키지 관리**합니다. 선택 된 **업데이트** 탭 및 **Xamarin.Forms** 패키지 합니다. 모든 프로젝트를 선택 하 고 GTK 프로젝트에서 사용 된 것과 동일한 Xamarin.Forms 버전으로 업데이트할 키를 누릅니다.

7. 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 **참조** GTK 프로젝트에 있습니다. 에 **참조 관리자** 대화 상자에서 **프로젝트** 왼쪽과 확인 하려면.NET 표준 또는 공유 프로젝트 옆의 확인란에:

    ![공유 프로젝트 참조](gtk-images/win/reference-shared-project.png "공유 프로젝트 참조")

8. 에 **참조 관리자** 대화 상자에서 키를 눌러는 **찾아보기** 을 찾아서 단추는 **C:\Program Files (x86)\GtkSharp\2.12\lib** 폴더를 선택은  **atk sharp.dll**, **gdk sharp.dll**, **글레이드 sharp.dll**, **glib sharp.dll**, **gtk-dotnet.dll**, **gtk sharp.dll** 파일입니다.

    ![GTK # 라이브러리 참조](gtk-images/win/reference-gtk-libraries.png "GTK # 라이브러리 참조")

    키를 눌러는 **확인** 참조를 추가 하는 단추입니다.

9. GTK 프로젝트 이름 바꾸기 **Class1.cs** 를 **Program.cs**합니다.

10. GTK 프로젝트에서 편집 된 **Program.cs** 다음 코드와 같이 파일:

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

    이 코드 GTK # 및 Xamarin.Forms를 초기화 합니다. 응용 프로그램 창 만들고 응용 프로그램을 실행 합니다.

11. 에 **솔루션 탐색기**GTK 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **속성**합니다.

12. 에 **속성** 창에서는 **응용 프로그램** 탭 하 고 변경는 **출력 형식이** 드롭다운을 **Windows 응용 프로그램**합니다.

    ![프로젝트 출력 형식을 변경](gtk-images/win/change-project-output-type.png "프로젝트 출력 형식 변경")

13. 에 **솔루션 탐색기**WPF 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **시작 프로젝트로 설정**합니다. F5 키를 눌러 Visual Studio 디버거와 함께 Windows 바탕 화면에서 프로그램을 실행 하려면:

    ![수명 GTK # 게임](gtk-images/win/gtk-gameoflife.png "GTK # 게임 수명")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 데스크톱에서 실행 되는 GTK # 앱을 추가 하려면 다음이 지침을 따릅니다.

1. Mac 용 Visual Studio에서 Xamarin.Forms 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 프로젝트 추가...** .

2. 에 **새 프로젝트** 창 선택 **다른 >.NET > Gtk # 2.0 프로젝트** 누릅니다 **다음**합니다.

3. 사용 하 여 프로젝트에 대 한 이름을 입력 한 **GTK** 예를 들어 확장 **GameOfLife.GTK**, 한 키를 누릅니다 **만들기**합니다.

4. 에 **솔루션 패드**를 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가 중...**  는 GTK에 대 한 프로젝트를 마우스 Xamarin.Forms 3.0 시험판 NuGet 패키지 추가 큽니다.

    ![Xamarin.Forms NuGet 패키지를 선택](gtk-images/mac/select-forms-nuget-package.png "Xamarin.Forms NuGet 패키지를 선택 합니다.")

5. 에 **솔루션 패드**를 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가 중...**  는 GTK에 대 한 프로젝트를 마우스 Xamarin.Forms.Platform.GTK 3.0 시험판 NuGet 패키지 추가 큽니다.

    ![Xamarin.Forms.Platform.GTK NuGet 패키지를 선택](gtk-images/mac/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet 패키지를 선택 합니다.")

6. GTK 프로젝트에서 사용 된 것과 동일한 Xamarin.Forms 버전을 사용 하도록 다른 플랫폼 프로젝트를 업데이트 합니다.

7. 에 **솔루션 패드**를 마우스 오른쪽 단추로 클릭 **참조 > 참조 편집...**  는 GTK에 대 한 프로젝트를 마우스 Xamarin.Forms 프로젝트 (.NET 표준 또는 공유 프로젝트)에 대 한 참조를 추가 합니다.

    ![공유 프로젝트 참조](gtk-images/mac/reference-shared-project.png "공유 프로젝트 참조")

8. 편집 된 **Program.cs** 한다는 다음 코드와 유사 하므로 GTK 프로젝트의 파일:

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

    이 코드 GTK # 및 Xamarin.Forms를 초기화 합니다. 응용 프로그램 창 만들고 응용 프로그램을 실행 합니다.

9. 에 **솔루션 패드**GTK 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **시작 프로젝트로 설정**합니다.

10. Mac의 도구 모음에 대 한 Visual Studio에서 키를 누릅니다는 **시작** 단추 (재생 단추 유사한 삼각형 모양의 단추)를 응용 프로그램을 실행 합니다.

    ![수명 GTK # 게임](gtk-images/mac/gtk-gameoflife.png "GTK # 게임 수명")

-----

## <a name="next-steps"></a>다음 단계

### <a name="platform-specifics"></a>플랫폼 세부 사항

XAML 또는 코드에서 Xamarin.Forms 응용 프로그램을 실행 하는 대상 플랫폼을 확인할 수 있습니다. 이 옵션을 사용 하면 GTK #에서 실행 되는 프로그램 특성을 변경할 수 있습니다. 코드에서의 값을 비교 `Device.RuntimePlatform` 와 `Device.GTK` 상수 (= "GTK" 문자열)입니다. GTK #에서 응용 프로그램이 실행 되는 일치 하는 경우.

XAML에서 사용할 수 있습니다는 `OnPlatform` 태그 플랫폼에 특정 속성 값을 선택 합니다.

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

### <a name="application-icon"></a>응용 프로그램 아이콘

시작 시 응용 프로그램 아이콘을 설정할 수 있습니다.

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>테마

테마의 다양 한 GTK #에서 사용할 수 있으며 Xamarin.Forms 앱에서 사용할 수 있습니다.

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>기본 형식

기본 형식 Xamarin.Forms를 사용 하면 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-GTK # 프로젝트를 포함 하는 네이티브 프로젝트에서 사용할 수 있도록 페이지를 파생 합니다. 인스턴스를 만들어이 작업을 수행할 수 있습니다는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-파생 된 페이지 및 네이티브 GTK # 형식을 사용 하 여 변환 된 `CreateContainer` 확장 메서드:

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

기본 형식에 대 한 자세한 내용은 참조 [네이티브 Forms](~/xamarin-forms/platform/native-forms.md)합니다.

## <a name="issues"></a>문제

이것은 하지를 프로덕션 준비가 될 때 이므로 미리 보기입니다. 현재 구현 상태를 참조 하십시오. [상태](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md), 현재 알려진된 문제에 대 한 참조 및 [보류 중 및 알려진 문제](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md)합니다.
