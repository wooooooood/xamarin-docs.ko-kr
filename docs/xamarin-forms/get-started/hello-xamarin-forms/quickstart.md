---
title: Xamarin.Forms 빠른 시작
ms.topic: article
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: 5ee36d63e2751eb5d09ee526755d62dda4ef537e
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2018
---
# <a name="xamarinforms-quickstart"></a>Xamarin.Forms 빠른 시작

이 연습에서는 사용자가 입력한 영숫자 전화 번호를 숫자 전화 번호로 변환하고 그 번호로 전화하는 응용 프로그램을 만드는 방법을 보여줍니다. 최종 응용 프로그램은 다음과 같습니다.

[![](quickstart-images/intro-app-examples-sml.png "Phoneword 응용 프로그램")](quickstart-images/intro-app-examples.png#lightbox "Phoneword 응용 프로그램")

Phoneword 응용 프로그램을 다음과 같이 만듭니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **시작** 화면에서 Visual Studio를 시작합니다. 이렇게 하면 시작 페이지가 열립니다.

    ![](quickstart-images/vs/start-page.png "Visual Studio")

2. Visual Studio에서 새 프로젝트를 만들려면 **새 프로젝트 만들기...** 를 클릭합니다.

    ![](quickstart-images/vs/new-solution.png "새 프로젝트")

3. **새 프로젝트** 대화 상자에서 **플랫폼 간**을 클릭하고, **플랫폼 간 앱(Xamarin.Forms)** 템플릿을 선택하고, 이름 및 솔루션 이름을 `Phoneword`로 설정하고, 프로젝트에 대한 적절한 위치를 선택하고, **확인** 단추를 클릭합니다.

    ![](quickstart-images/vs/new-project.png "플랫폼 간 프로젝트 템플릿")

4. **새 플랫폼 간 앱** 대화 상자에서 **Blank App**을 클릭하고, **Xamarin.Forms**를 UI 기술로 선택하고, **.NET Standard**를 코드 공유 전략으로 선택하고, **확인** 단추를 클릭합니다.

    ![](quickstart-images/vs/new-app.png "새 플랫폼 간 앱")

5. **솔루션 탐색기**의 **Phoneword** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다.

    ![](quickstart-images/vs/open-mainpage-xaml.png "MainPage.xaml 열기")

6. **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. 이 코드는 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, WinPhone, Windows" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    **CTRL+S** 키를 눌러 변경 내용을 **MainPage.xaml**에 저장하고 파일을 선택합니다.

7. **솔루션 탐색기**에서 **MainPage.xaml**을 확장한 다음, **MainPage.xaml.cs**를 두 번 클릭하여 엽니다.

    ![](quickstart-images/vs/open-mainpage-codebehind.png "MainPage.xaml.cs 열기")

8. **MainPage.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. `OnTranslate`와 `OnCall` 메서드는 사용자 인터페이스에서 **변환** 및 **호출** 단추가 각각 클릭될 때 그에 대한 응답으로 실행됩니다.

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            string translatedNumber;

            public MainPage ()
            {
                InitializeComponent ();
            }

            void OnTranslate (object sender, EventArgs e)
            {
                translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
                if (!string.IsNullOrWhiteSpace (translatedNumber)) {
                    callButton.IsEnabled = true;
                    callButton.Text = "Call " + translatedNumber;
                } else {
                    callButton.IsEnabled = false;
                    callButton.Text = "Call";
                }
            }

            async void OnCall (object sender, EventArgs e)
            {
                if (await this.DisplayAlert (
                        "Dial a Number",
                        "Would you like to call " + translatedNumber + "?",
                        "Yes",
                        "No")) {
                    var dialer = DependencyService.Get<IDialer> ();
                    if (dialer != null)
                        dialer.Dial (translatedNumber);
                }
            }
        }
    }
    ```

    > [!NOTE]
    > 이 때 응용 프로그램을 빌드하려고 하면 나중에 수정될 오류가 발생합니다.

    **CTRL+S** 키를 눌러 변경 내용을 **MainPage.xaml.cs**에 저장하고 파일을 선택합니다.

9. **솔루션 탐색기**에서 **App.xaml**을 확장한 다음 **App.xaml.cs**를 두 번 클릭하여 엽니다.

    ![](quickstart-images/vs/open-app-class.png "App.xaml.cs 열기")

10. **App.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. `App` 생성자는 `MainPage` 클래스를 응용 프로그램이 시작될 때 표시될 페이지로 설정합니다.

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public App()
            {
                InitializeComponent();
                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    **CTRL+S** 키를 눌러 변경 내용을 **App.xaml.cs**에 저장하고 파일을 선택합니다.

11. **솔루션 탐색기**에서 **Phoneword** 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **추가 > 새 항목...**을 클릭합니다.

    ![](quickstart-images/vs/add-new-item.png "새 항목 추가")

12. **새 항목 추가** 대화 상자에서 **Visual C# > Code > Class**를 선택하고, 새 파일에 **PhoneTranslator**라는 이름을 지정하고 **추가** 단추를 클릭합니다.

    ![](quickstart-images/vs/add-translator-class.png "새 클래스 추가")

13. **PhoneTranslator.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. 이 코드는 전화 단어를 전화번호로 변환합니다.

    ```csharp
    using System.Text;

    namespace Core
    {
        public static class PhonewordTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw))
                    return null;

                raw = raw.ToUpperInvariant();

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c))
                        newNumber.Append(c);
                    else
                    {
                        var result = TranslateToNumber(c);
                        if (result != null)
                            newNumber.Append(result);
                        // Bad character?
                        else
                            return null;
                    }
                }
                return newNumber.ToString();
            }

            static bool Contains(this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static readonly string[] digits = {
                "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
            };

            static int? TranslateToNumber(char c)
            {
                for (int i = 0; i < digits.Length; i++)
                {
                    if (digits[i].Contains(c))
                        return 2 + i;
                }
                return null;
            }
        }
    }
    ```

    **CTRL+S** 키를 눌러 변경 내용을 **PhoneTranslator.cs**에 저장하고 파일을 선택합니다.

14. **솔루션 탐색기**에서 **Phoneword** 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **추가 > 새 항목...**을 클릭합니다.

    ![](quickstart-images/vs/add-new-item.png "새 항목 추가")

15. **새 항목 추가** 대화 상자에서 **Visual C# > Code > Interface**를 선택하고, 새 파일에 **IDialer**라는 이름을 지정하고 **추가** 단추를 클릭합니다.

    ![](quickstart-images/vs/add-idialer-interface.png "새 인터페이스 추가")

16. **IDialer.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. 이 코드는 변환된 전화번호로 전화를 걸기 위해 각 플랫폼에 구현되어야 하는 `Dial` 메서드를 정의합니다.

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```

    **CTRL+S** 키를 눌러 변경 내용을 **IDialer.cs**에 저장하고 파일을 선택합니다.

    > [!NOTE]
    > 응용 프로그램에 대한 공통 코드가 이제 완료되었습니다. 플랫폼 특정 전화 걸기 코드는 이제 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)로 구현됩니다.

17. **솔루션 탐색기**에서 **Phoneword.iOS** 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **추가 > 새 항목...**을 선택합니다.

    ![](quickstart-images/vs/add-new-item-ios.png "새 항목 추가")

18. **새 항목 추가** 대화 상자에서 **Apple > Code > Class**를 선택하고, 새 파일에 **PhoneDialer**라는 이름을 지정하고 **추가** 단추를 클릭합니다.

    ![](quickstart-images/vs/new-phone-dialer-ios.png "새 클래스 추가")

19. **PhoneDialer.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. 이 코드는 변환된 전화번호로 전화를 걸기 위해 iOS 플랫폼에서 사용될 <code>Dial</code> 메서드를 만듭니다.

    ```csharp
    using Foundation;
    using Phoneword.iOS;
    using UIKit;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.iOS
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                return UIApplication.SharedApplication.OpenUrl (
                    new NSUrl ("tel:" + number));
            }
        }
    }
    ```

    **CTRL+S** 키를 눌러 변경 내용을 **PhoneDialer.cs**에 저장하고 파일을 선택합니다.

20. **솔루션 탐색기**에서 **Phoneword.Android** 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **추가 > 새 항목...**을 클릭합니다.

    ![](quickstart-images/vs/add-new-item-android.png "새 항목 추가")

21. **새 항목 추가** 대화 상자에서 **Visual C# > Android > Class**를 선택하고, 새 파일에 **PhoneDialer**라는 이름을 지정하고 **추가** 단추를 클릭합니다.

    ![](quickstart-images/vs/new-phone-dialer-android.png "새 클래스 추가")

22. **PhoneDialer.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. 이 코드는 변환된 전화번호로 전화를 걸기 위해 Android 플랫폼에서 사용될 `Dial` 메서드를 만듭니다.

    ```csharp
    using Android.Content;
    using Android.Telephony;
    using Phoneword.Droid;
    using System.Linq;
    using Xamarin.Forms;
    using Uri = Android.Net.Uri;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.Droid
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                var context = MainActivity.Instance;
                if (context == null)
                    return false;

                var intent = new Intent (Intent.ActionCall);
                intent.SetData (Uri.Parse ("tel:" + number));

                if (IsIntentAvailable (context, intent)) {
                    context.StartActivity (intent);
                    return true;
                }

                return false;
            }

            public static bool IsIntentAvailable(Context context, Intent intent)
            {
                var packageManager = context.PackageManager;

                var list = packageManager.QueryIntentServices (intent, 0)
                    .Union (packageManager.QueryIntentActivities (intent, 0));

                if (list.Any ())
                    return true;

                var manager = TelephonyManager.FromContext (context);
                return manager.PhoneType != PhoneType.None;
            }
        }
    }
    ```

    **CTRL+S** 키를 눌러 변경 내용을 **PhoneDialer.cs**에 저장하고 파일을 선택합니다.

23. **솔루션 탐색기**의 **Phoneword.Android** 프로젝트에서 **MainActivity.cs**를 두 번 클릭하여 열고, 모든 템플릿 코드를 제거한 후 다음 코드로 바꿉니다.

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
        {
            internal static MainActivity Instance { get; private set; }

            protected override void OnCreate(Bundle bundle)
            {
                TabLayoutResource = Resource.Layout.Tabbar;
                ToolbarResource = Resource.Layout.Toolbar;

                base.OnCreate(bundle);

                Instance = this;
                global::Xamarin.Forms.Forms.Init(this, bundle);
                LoadApplication(new App());
            }
        }
    }  
    ```

    **CTRL+S** 키를 눌러 변경 내용을 **MainActivity.cs** 파일에 저장하고 파일을 닫습니다.

24. **솔루션 탐색기**의 **Phoneword.Android** 프로젝트에서 **속성**을 두 번 클릭한 다음 **Android Manifest** 탭을 선택합니다.

    ![](quickstart-images/vs/android-manifest.png "빌드 > Android Manifest 열기")

25. **필요한 권한** 섹션에서 **CALL_PHONE** 권한을 사용하도록 설정합니다. 그러면 응용 프로그램에 전화를 거는 권한이 주어집니다.

    ![](quickstart-images/vs/android-manifest-changed.png "CallPhone 권한을 사용하도록 설정")

    **CTRL+S** 키를 눌러 변경 내용을 매니페스트에 저장하고 파일을 닫습니다.

26. **솔루션 탐색기**에서 **Phoneword.UWP** 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **추가 > 새 항목...**을 클릭합니다.

    ![](quickstart-images/vs/add-new-item-uwp.png "새 항목 추가")

27. **새 항목 추가** 대화 상자에서 **Visual C# > Code > Class**를 선택하고, 새 파일에 **PhoneDialer**라는 이름을 지정하고 **추가** 단추를 클릭합니다.

    ![](quickstart-images/vs/new-phone-dialer-uwp.png "새 클래스 추가")

28. **PhoneDialer.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. 이 코드는 변환된 전화번호로 전화를 걸기 위해 유니버설 Windows 플랫폼에서 사용될 `Dial` 메서드와 도우미 메서드를 만듭니다.

    ```csharp
    using Phoneword.UWP;
    using System;
    using System.Threading.Tasks;
    using Windows.ApplicationModel.Calls;
    using Windows.UI.Popups;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.UWP
    {
        public class PhoneDialer : IDialer
        {
            bool dialled = false;

            public bool Dial(string number)
            {
                DialNumber(number);
                return dialled;
            }

            async Task DialNumber(string number)
            {
                var phoneLine = await GetDefaultPhoneLineAsync();
                if (phoneLine != null)
                {
                    phoneLine.Dial(number, number);
                    dialled = true;
                }
                else
                {
                    var dialog = new MessageDialog("No line found to place the call");
                    await dialog.ShowAsync();
                    dialled = false;
                }
            }

            async Task<PhoneLine> GetDefaultPhoneLineAsync()
            {
                var phoneCallStore = await PhoneCallManager.RequestStoreAsync();
                var lineId = await phoneCallStore.GetDefaultLineAsync();
                return await PhoneLine.FromIdAsync(lineId);
            }
        }
    }
    ```

    **CTRL+S** 키를 눌러 변경 내용을 **PhoneDialer.cs**에 저장하고 파일을 선택합니다.

29. **솔루션 탐색기**의 **Phoneword.UWP**에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가**를 선택합니다.

    ![](quickstart-images/vs/uwp-add-reference.png "참조 추가")

30. **참조 관리자** 대화 상자에서 **유니버설 Windows > 확장 > UWP에 대한 Windows 모바일 확장**을 클릭하고 **확인** 단추를 클릭합니다.

    ![](quickstart-images/vs/uwp-add-reference-extensions.png "UWP에 대한 Windows 모바일 확장 추가")

31. **솔루션 탐색기**의 **Phoneword.UWP** 프로젝트에서, **Package.appxmanifest**를 두 번 클릭합니다.

    ![](quickstart-images/vs/uwp-manifest.png "UWP Manifest 열기")

31. **기능** 페이지에서 **전화 통화** 기능을 사용하도록 설정합니다. 그러면 응용 프로그램에 전화를 거는 권한이 주어집니다.

    ![](quickstart-images/vs/uwp-manifest-changed.png "전화 통화 기능 활성화")

    **CTRL+S** 키를 눌러 변경 내용을 매니페스트에 저장하고 파일을 닫습니다.

32. Visual Studio에서 **빌드 > 솔루션 빌드** 메뉴 항목을 선택(하거나 **CTRL+SHIFT+B** 키를 누릅니다). 응용 프로그램이 빌드하고 성공 메시지가 Visual Studio 상태 표시줄에 표시됩니다.

    ![](quickstart-images/vs/build-successful.png "빌드 성공")

    오류가 있는 경우 이전 단계를 반복하고 응용 프로그램이 성공적으로 빌드할 때까지 실수를 수정합니다.

33. **솔루션 탐색기**에서 **Phoneword.UWP** 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **시작 프로젝트로 설정**을 선택합니다.

    ![](quickstart-images/vs/uwp-set-as-startup-project.png "시작 프로젝트로 설정")

34. Visual Studio 도구 모음에서 응용 프로그램을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](quickstart-images/vs/start.png "Visual Studio 도구 모음")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword 응용 프로그램 UWP")

35. **솔루션 탐색기**에서 **Phoneword.Android** 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **시작 프로젝트로 설정**을 선택합니다.
36. Visual Studio 도구 모음에서 Android 에뮬레이터 안에 응용 프로그램을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.
37. iOS 장치가 있고 Xamarin.Forms 개발에 대한 Mac 시스템 요구 사항을 충족하는 경우 비슷한 기술을 사용하여 앱을 iOS 장치에 배포합니다. 또는 [iOS 원격 시뮬레이터](~/tools/ios-simulator.md)에 앱을 배포합니다.

    주의: 전화 통화는 모든 시뮬레이터에서 지원되지는 않습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac용 Visual Studio를 시작하고, 시작 페이지에서 **새 프로젝트...** 를 클릭하여 새 프로젝트를 만듭니다.

    ![](quickstart-images/xs/new-solution.png "새 솔루션")

2. **새 프로젝트에 대한 템플릿 선택** 대화 상자에서 **다중 플랫폼 > 앱**을 클릭하여 **빈 Forms 앱** 템플릿을 선택하고 **다음** 단추를 클릭합니다.

    ![](quickstart-images/xs/choose-template.png "템플릿 선택")

3. **빈 Forms 앱 구성** 대화 상자에서 새 앱 `Phoneword`의 이름을 지정하고, **이식 가능한 클래스 라이브러리 사용** 라디오 단추가 선택되었는지 확인하고, **사용자 인터페이스 파일에 대해 XAML 사용** 확인란이 선택되었는지 확인한 후 **다음** 단추를 클릭합니다.

    ![](quickstart-images/xs/configure-app.png "Forms 응용프로그램 구성")

4. **새 빈 Forms 앱 구성** 대화 상자에서 솔루션 및 프로젝트 이름을 `Phoneword`으로 설정된 채로 두고, 프로젝트에 적절한 위치를 선택한 다음, **만들기** 단추를 클릭하여 프로젝트를 만듭니다.

    ![](quickstart-images/xs/configure-project.png "Forms Project 구성")

5. **Solution Pad**에서 **Phoneword** 프로젝트를 선택한 다음, **추가 > 새 파일...**을 선택합니다.

    ![](quickstart-images/xs/add-new-file.png "새 파일 추가")

6. **새 파일** 대화 상자에서 **Forms > Forms ContentPage Xaml**을 선택하고, 새 파일에 **MainPage**라는 이름을 지정하고 **새로 만들기** 단추를 클릭합니다. 그러면 **MainPage**란 이름의 페이지가 프로젝트에 추가됩니다.

    ![](quickstart-images/xs/add-mainpage-class.png "새 ContentPage 추가")

7. **Solution Pad**에서 **MainPage.xaml**을 두 번 클릭하여 엽니다.

    ![](quickstart-images/xs/open-mainpage-xaml.png "MainPage.xaml 열기")

8. **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. 이 코드는 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, WinPhone, Windows" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    **File > Save**를 선택하거나(또는 **&#8984; + S**를 눌러) 변경 내용을 **MainPage.xaml**에 저장하고 파일을 닫습니다.

9. **Solution Pad**에서 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다.

    ![](quickstart-images/xs/open-mainpage-codebehind.png "MainPage.xaml.cs 열기")

10. **MainPage.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. `OnTranslate`과 `OnCall` 메서드는 사용자 인터페이스에서 **변환** 및 **호출** 단추가 각각 클릭될 때 그에 대한 응답으로 실행됩니다.

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            string translatedNumber;

            public MainPage ()
            {
                InitializeComponent ();
            }

            void OnTranslate (object sender, EventArgs e)
            {
                translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
                if (!string.IsNullOrWhiteSpace (translatedNumber)) {
                    callButton.IsEnabled = true;
                    callButton.Text = "Call " + translatedNumber;
                } else {
                    callButton.IsEnabled = false;
                    callButton.Text = "Call";
                }
            }

            async void OnCall (object sender, EventArgs e)
            {
                if (await this.DisplayAlert (
                        "Dial a Number",
                        "Would you like to call " + translatedNumber + "?",
                        "Yes",
                        "No")) {
                    var dialer = DependencyService.Get<IDialer> ();
                    if (dialer != null)
                        dialer.Dial (translatedNumber);
                }
            }
        }
    }
    ```

    > [!NOTE]
    > 이 때 응용 프로그램을 빌드하려고 하면 나중에 수정될 오류가 발생합니다.

    **File > Save**를 선택하거나(또는 **&#8984; + S**를 눌러) 변경 내용을 **MainPage.xaml.cs**에 저장하고 파일을 닫습니다.

11. **Solution Pad**에서 **App.xaml.cs**를 두 번 클릭하여 엽니다.

    ![](quickstart-images/xs/open-app-class.png "App.xaml.cs 열기")

12. **App.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. `App` 생성자는 `MainPage` 클래스를 응용 프로그램이 시작될 때 표시될 페이지로 설정합니다.

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public App()
            {
                InitializeComponent();
                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    **File > Save**를 선택하거나(또는 **&#8984; + S**를 눌러) 변경 내용을 **Phoneword.cs**에 저장하고 파일을 닫습니다.

13. **Solution Pad**에서 **Phoneword** 프로젝트를 선택한 다음, **추가 > 새 파일...**을 선택합니다.

    ![](quickstart-images/xs/add-new-translator-file.png "새 파일 추가")

14. **새 파일** 대화 상자에서 **General > Empty Class**를 선택하고, 새 파일에 **PhoneTranslator**라는 이름을 지정하고 **새로 만들기** 단추를 클릭합니다.

    ![](quickstart-images/xs/add-translator-class.png "새 클래스 추가")

15. **PhoneTranslator.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. 이 코드는 전화 단어를 전화번호로 변환합니다.

    ```csharp
    using System.Text;

    namespace Core
    {
        public static class PhonewordTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw))
                    return null;

                raw = raw.ToUpperInvariant();

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c))
                        newNumber.Append(c);
                    else
                    {
                        var result = TranslateToNumber(c);
                        if (result != null)
                            newNumber.Append(result);
                        // Bad character?
                        else
                            return null;
                    }
                }
                return newNumber.ToString();
            }

            static bool Contains(this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static readonly string[] digits = {
                "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
            };

            static int? TranslateToNumber(char c)
            {
                for (int i = 0; i < digits.Length; i++)
                {
                    if (digits[i].Contains(c))
                        return 2 + i;
                }
                return null;
            }
        }
    }
    ```

    **File > Save**를 선택하거나(또는 **&#8984; + S**를 눌러) 변경 내용을 **PhoneTranslator.cs**에 저장하고 파일을 닫습니다.

16. **Solution Pad**에서 **Phoneword** 프로젝트를 선택한 다음, **추가 > 새 파일...**을 선택합니다.

    ![](quickstart-images/xs/add-new-interface.png "새 파일 추가")

17. **새 파일** 대화 상자에서 **General > Empty Interface**를 선택하고, 새 파일에 **IDialer**라는 이름을 지정하고 **새로 만들기** 단추를 클릭합니다.

    ![](quickstart-images/xs/add-idialer-interface.png "새 인터페이스 추가")

18. **IDialer.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. 이 코드는 변환된 전화번호로 전화를 걸기 위해 각 플랫폼에 구현되어야 하는 `Dial` 메서드를 정의합니다.

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```
    **File > Save**를 선택하거나(또는 **&#8984; + S**를 눌러) 변경 내용을 **IDialer.cs**에 저장하고 파일을 닫습니다.

    > [!NOTE]
    > 응용 프로그램에 대한 공통 코드가 이제 완료되었습니다. 플랫폼 특정 전화 걸기 코드는 이제 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)로 구현됩니다.

19. **Solution Pad**에서 **Phoneword.iOS** 프로젝트를 선택한 다음, **추가 > 새 파일...**을 마우스 오른쪽 단추로 클릭하여 선택합니다.

    ![](quickstart-images/xs/add-new-file-ios.png "새 파일 추가")

20. **새 파일** 대화 상자에서 **General > Empty Class**를 선택하고, 새 파일에 **PhoneDialer**라는 이름을 지정하고 **새로 만들기** 단추를 클릭합니다.

    ![](quickstart-images/xs/new-phonedialer-ios.png "새 클래스 추가")

21. **PhoneDialer.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. 이 코드는 변환된 전화번호로 전화를 걸기 위해 iOS 플랫폼에서 사용될 `Dial` 메서드를 만듭니다.

    ```csharp
    using Foundation;
    using Phoneword.iOS;
    using UIKit;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.iOS
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                return UIApplication.SharedApplication.OpenUrl (
                    new NSUrl ("tel:" + number));
            }
        }
    }
    ```

    **File > Save**를 선택하거나(또는 **&#8984; + S**를 눌러) 변경 내용을 **PhoneDialer.cs**에 저장하고 파일을 닫습니다.

22. **Solution Pad**에서 **Phoneword.Droid** 프로젝트를 선택한 다음, **추가 > 새 파일...**을 마우스 오른쪽 단추로 클릭하여 선택합니다.

    ![](quickstart-images/xs/add-new-file-android.png "새 파일 추가")

23. **새 파일** 대화 상자에서 **General > Empty Class**를 선택하고, 새 파일에 **PhoneDialer**라는 이름을 지정하고 **새로 만들기** 단추를 클릭합니다.

    ![](quickstart-images/xs/new-phonedialer-android.png "새 클래스 추가")

24. **PhoneDialer.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다. 이 코드는 변환된 전화번호로 전화를 걸기 위해 Android 플랫폼에서 사용될 `Dial` 메서드를 만듭니다.

    ```csharp
    using Android.Content;
    using Android.Telephony;
    using Phoneword.Droid;
    using System.Linq;
    using Xamarin.Forms;
    using Uri = Android.Net.Uri;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.Droid
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                var context = MainActivity.Instance;
                if (context == null)
                    return false;

                var intent = new Intent (Intent.ActionCall);
                intent.SetData (Uri.Parse ("tel:" + number));

                if (IsIntentAvailable (context, intent)) {
                    context.StartActivity (intent);
                    return true;
                }

                return false;
            }

            public static bool IsIntentAvailable(Context context, Intent intent)
            {
                var packageManager = context.PackageManager;

                var list = packageManager.QueryIntentServices (intent, 0)
                    .Union (packageManager.QueryIntentActivities (intent, 0));

                if (list.Any ())
                    return true;

                var manager = TelephonyManager.FromContext (context);
                return manager.PhoneType != PhoneType.None;
            }
        }
    }
    ```

    **File > Save**를 선택하거나(또는 **&#8984; + S**를 눌러) 변경 내용을 **PhoneDialer.cs**에 저장하고 파일을 닫습니다.

25. **Solution Pad**의 **Phoneword.Droid** 프로젝트에서 **MainActivity.cs**를 두 번 클릭하여 열고, 모든 템플릿 코드를 제거한 후 다음 코드로 바꿉니다.

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
        {
            internal static MainActivity Instance { get; private set; }

            protected override void OnCreate(Bundle bundle)
            {
                TabLayoutResource = Resource.Layout.Tabbar;
                ToolbarResource = Resource.Layout.Toolbar;

                base.OnCreate(bundle);

                Instance = this;
                global::Xamarin.Forms.Forms.Init(this, bundle);
                LoadApplication(new App());
            }
        }
    }        
    ```

    **파일 > 저장**을 선택하여(또는 **&#8984; + S**를 눌러) 변경 내용을 **MainPage.xaml.cs** 파일에 저장하고 파일을 닫습니다.

26. **Solution Pad**에서 **속성** 폴더를 확장한 다음 **AndroidManifest.xml** 파일을 두 번 클릭합니다.

    ![](quickstart-images/xs/android-manifest.png "빌드 > Android Manifest 열기")

27. **필요한 권한** 섹션에서 **CallPhone** 권한을 사용하도록 설정합니다. 그러면 응용 프로그램에 전화를 거는 권한이 주어집니다.

    ![](quickstart-images/xs/android-manifest-changed.png "CallPhone 권한을 사용하도록 설정")

    **File > Save**를 선택하거나(또는 **&#8984; + S**를 눌러) 변경 내용을 **AndroidManifest.xml**에 저장하고 파일을 닫습니다.

28. **Solution Pad**에서 **PhonewordPage** 클래스를 **Phoneword** 프로젝트에서 제거합니다. 이 페이지는 프로젝트가 만들어졌을 때 자동으로 추가되었으며 더 이상 필요하지 않습니다.
29. Mac용 Visual Studio에서 **빌드 > 솔루션 빌드** 메뉴 항목을 선택(하거나 **&#8984; + B** 키를 누릅니다). 응용 프로그램이 빌드하고 성공 메시지가 Mac용 Visual Studio 상태 표시줄에 표시됩니다.

    ![](quickstart-images/xs/build-successful.png "빌드 성공")

30. 오류가 있는 경우 이전 단계를 반복하고 응용 프로그램이 성공적으로 빌드할 때까지 실수를 수정합니다.
31. Mac용 Visual Studio 도구 모음에서 iOS Simulator 안에 응용 프로그램을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](quickstart-images/xs/start.png "Mac용 Visual Studio 도구 모음")
    ![](quickstart-images/xs/phoneword-result-ios.png "iOS Simulator")

    주의: 전화 통화는 iOS Simulator에서 지원되지 않습니다.

32. **Solution Pad**에서 **Phoneword.Droid** 프로젝트를 선택한 다음, **시작 프로젝트로 설정**을 마우스 오른쪽 단추로 클릭하여 선택합니다.

    ![](quickstart-images/xs/set-startup-project.png "시작 프로젝트로 설정")

33. Mac용 Visual Studio 도구 모음에서 Android 에뮬레이터 안에 응용 프로그램을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](quickstart-images/xs/phoneword-result-android.png "Android Emulator")

    주의: 전화 통화는 Android Simulator에서 지원되지 않습니다.

-----

Xamarin.Forms 응용 프로그램을 완료한 것을 축하합니다. 이 가이드의 [다음 항목](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md)은 Xamarin.Forms를 사용하여 응용 프로그램 개발의 기본에 대한 이해를 갖추기 위해 이 연습에서 수행된 단계를 검토합니다.


## <a name="related-links"></a>관련 링크

- [DependencyService를 통한 네이티브 기능에 액세스](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword(샘플)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
