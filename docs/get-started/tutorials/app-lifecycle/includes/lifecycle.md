# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio를 실행하고 **AppLifecycleTutorial**이라는 새로운 빈 Xamarin.Forms 애플리케이션을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET Standard를 사용하는지 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각은 솔루션의 이름이 **AppLifecycleTutorial**이어야 합니다. 이 자습서에서 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET Standard 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)의 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **솔루션 탐색기**의 **AppLifecycleTutorial** 프로젝트에서 **App.xaml**을 확장하고 **App.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **App.xaml.cs**에서 `OnStart`, `OnSleep` 및 `OnResume` 재정의를 다음과 같이 업데이트합니다.

    ```csharp
    protected override void OnStart()
    {
        Console.WriteLine("OnStart");
    }

    protected override void OnSleep()
    {
        Console.WriteLine("OnSleep");
    }

    protected override void OnResume()
    {
        Console.WriteLine("OnResume");
    }
    ```

    이 코드는 각 메서드가 호출된 시기를 나타내는 `Console.WriteLine` 문을 사용하여 애플리케이션 수명 주기 메서드 재정의를 업데이트합니다.

    - `OnStart` 메서드는 애플리케이션이 시작될 때 호출됩니다.
    - `OnSleep` 메서드는 애플리케이션이 백그라운드로 이동할 때 호출됩니다.
    - `OnResume` 메서드는 애플리케이션이 백그라운드에서 다시 시작될 때 호출됩니다.

    > [!NOTE]
    > 애플리케이션 종료 메서드가 없습니다. 정상적인 상황에서는 애플리케이션 종료가 `OnSleep` 메서드에서 발생합니다.

1. Visual Studio 도구 모음에서 선택한 원격 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. 애플리케이션이 시작되면 `OnStart` 메서드가 호출되고 **OnStart**가 Visual Studio **출력** 창에 출력됩니다.

    ```
    [Mono] Found as 'java_interop_jnienv_get_object_array_element'.
    OnStart
    [OpenGLRenderer] HWUI GL Pipeline
    ```

    애플리케이션이 배경으로 설정되면(iOS 또는 Android에서 홈 단추를 눌러서) `OnSleep` 메서드가 호출됩니다.

    ```
    [EGL_emulation] eglMakeCurrent: 0x83ee2920: ver 3 0 (tinfo 0x8357eff0)
    OnSleep
    [Mono] Image addref System.Runtime.Serialization[0x83ee19c0] -> System.Runtime.Serialization.dll[0x83f57b00]: 2
    ```

    그런 다음, 애플리케이션이 백그라운드에서 다시 시작되면(iOS의 애플리케이션 아이콘을 탭하여 Android의 개요 단추를 탭하고 AppLifecycleTutorial 애플리케이션 선택) `OnResume` 메서드가 호출됩니다.

    ```
    Thread finished: <Thread Pool> #5
    OnResume
    [EGL_emulation] eglMakeCurrent: 0x83ee2920: ver 3 0 (tinfo 0x8357eff0)
    ```

    > [!NOTE]
    > 이러한 코드 블록은 Android에서 애플리케이션을 실행할 때 예제 출력을 보여줍니다.

    Xamarin.Forms 앱 수명 주기에 대한 자세한 내용 [Xamarin.Forms 앱 수명 주기](~/xamarin-forms/app-fundamentals/app-lifecycle.md)를 참조하세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac용 Visual Studio를 실행하고 **AppLifecycleTutorial**이라는 새로운 빈 Xamarin.Forms 애플리케이션을 만듭니다. 앱이 공유 코드 메커니즘으로 .NET Standard를 사용하는지 확인합니다.

    > [!IMPORTANT]
    > 이 자습서의 C# 및 XAML 코드 조각은 솔루션의 이름이 **AppLifecycleTutorial**이어야 합니다. 이 자습서에서 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET Standard 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](~/get-started/first-app/index.md)의 [Xamarin.Forms 애플리케이션 분석](~/get-started/first-app/index.md)을 참조하세요.

1. **Solution Pad**의 **AppLifecycleTutorial** 프로젝트에서 **App.xaml**을 확장하고 **App.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **App.xaml.cs**에서 `OnStart`, `OnSleep` 및 `OnResume` 재정의를 다음과 같이 업데이트합니다.

    ```csharp
    protected override void OnStart()
    {
        Console.WriteLine("OnStart");
    }

    protected override void OnSleep()
    {
        Console.WriteLine("OnSleep");
    }

    protected override void OnResume()
    {
        Console.WriteLine("OnResume");
    }
    ```

    이 코드는 각 메서드가 호출된 시기를 나타내는 `Console.WriteLine` 문을 사용하여 애플리케이션 수명 주기 메서드 재정의를 업데이트합니다.

    - `OnStart` 메서드는 애플리케이션이 시작될 때 호출됩니다.
    - `OnSleep` 메서드는 애플리케이션이 백그라운드로 이동할 때 호출됩니다.
    - `OnResume` 메서드는 애플리케이션이 백그라운드에서 다시 시작될 때 호출됩니다.

    > [!NOTE]
    > 애플리케이션 종료 메서드가 없습니다. 정상적인 상황에서는 애플리케이션 종료가 `OnSleep` 메서드에서 발생합니다.

1. Mac용 Visual Studio 도구 모음에서 선택한 iOS 시뮬레이터 또는 Android 에뮬레이터 내에서 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다. 애플리케이션이 시작되면 `OnStart` 메서드가 호출되고 **OnStart**가 Mac용 Visual Studio **애플리케이션 출력** 창에 출력됩니다.

    ```
    2019-02-11 12:05:23.164761+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4089
    2019-02-11 12:05:23.165297+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskCopyDebugDescription: AppLifecycleTuto[4089]/0#-1 LF=0
    2019-02-11 12:05:23.685430+0000 AppLifecycleTutorial.iOS[4089:361037] OnStart
    ```

    애플리케이션이 배경으로 설정되면(iOS 또는 Android에서 홈 단추를 눌러서) `OnSleep` 메서드가 호출됩니다.

    ```
    2019-02-11 12:06:12.394301+0000 AppLifecycleTutorial.iOS[4089:361037] OnSleep
    Thread started: <Thread Pool> #3
    Thread started: <Thread Pool> #4
    Thread started: <Thread Pool> #5
    Thread started: <Thread Pool> #6
    2019-02-11 12:06:13.056968+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4089
    2019-02-11 12:06:13.057264+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskCopyDebugDescription: AppLifecycleTuto[4089]/0#-1 LF=0
    ```

    그런 다음, 애플리케이션이 백그라운드에서 다시 시작되면(iOS의 애플리케이션 아이콘을 탭하여 Android의 개요 단추를 탭하고 AppLifecycleTutorial 애플리케이션 선택) `OnResume` 메서드가 호출됩니다.

    ```
    2019-02-11 12:07:10.321905+0000 AppLifecycleTutorial.iOS[4130:365695] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4130
    2019-02-11 12:07:10.322557+0000 AppLifecycleTutorial.iOS[4130:365695] SecTaskCopyDebugDescription: AppLifecycleTuto[4130]/0#-1 LF=0
    2019-02-11 12:07:17.110938+0000 AppLifecycleTutorial.iOS[4130:365695] OnResume
    ```

    > [!NOTE]
    > 이러한 코드 블록은 iOS에서 애플리케이션을 실행할 때 예제 출력을 보여줍니다.

    Xamarin.Forms 앱 수명 주기에 대한 자세한 내용 [Xamarin.Forms 앱 수명 주기](~/xamarin-forms/app-fundamentals/app-lifecycle.md)를 참조하세요.
