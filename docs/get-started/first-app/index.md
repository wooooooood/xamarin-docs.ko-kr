---
title: 첫 번째 Xamarin.Forms 앱 빌드
description: Visual Studio에서 첫 번째 Xamarin.Forms 애플리케이션을 빌드하는 방법을 보여 주는 비디오 가이드입니다.
zone_pivot_groups: platform-dev16
ms.prod: xamarin
ms.assetid: 72B6AF82-4D98-47E5-AB54-0A35B3253468
ms.technology: xamarin-forms
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 05/23/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: da56bde956a0ff7730ef6737e2802c3723d6d716
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84133484"
---
# <a name="build-your-first-xamarinforms-app"></a>첫 번째 Xamarin.Forms 앱 빌드

_이 비디오를 보고 따라서 Xamarin.Forms로 첫 번째 모바일 앱을 만듭니다._

::: zone pivot="windows"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-Android-App-with-Visual-Studio-2019-and-Xamarin/player]

## <a name="step-by-step-instructions-for-windows"></a>Windows용 단계별 지침

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

위의 비디오와 함께 다음 단계를 수행합니다.

1. **파일 > 새로 만들기 > 프로젝트...** 를 선택하거나 **새 프로젝트 만들기...** 단추를 누릅니다.

    [![새 프로젝트 만들기](images/win-2019/01-sml.png)](images/win-2019/01.png#lightbox)

2. “Xamarin”을 검색하거나 **프로젝트 형식** 메뉴에서 **모바일**을 선택하세요. **모바일 앱(Xamarin.Forms)** 프로젝트 형식을 선택하세요.

    [![Xamarin 프로젝트에 대한 필터](images/win-2019/02-sml.png)](images/win-2019/02.png#lightbox)

3. 예제에서 “AwesomeApp”을 사용하는 프로젝트 이름 &ndash;을(를) 선택합니다.

    [![프로젝트 이름 선택](images/win-2019/03-sml.png)](images/win-2019/03.png#lightbox)

4. **빈** 프로젝트 형식을 클릭하고 **Android** 및 **iOS**가 선택되어 있는지 확인합니다.

    [![.NET Standard가 포함된 Android 및 iOS](images/win-2019/04-sml.png)](images/win-2019/04.png#lightbox)

5. NuGet 패키지가 복원될 때까지 기다립니다(상태 표시줄에 "복원 완료" 메시지가 표시됨).

6. 새 Visual Studio 2019 설치에는 Android Emulator가 구성되어 있지 않습니다. **디버그** 단추에서 드롭다운 화살표를 클릭하고 **Android Emulator 만들기**를 선택하여 에뮬레이터 만들기 화면을 시작합니다.

    ![Android Emulator 드롭다운 만들기](images/win-2019/debug-dropdown.png)

7. 에뮬레이터 만들기 화면에서 기본 설정을 사용하고 **만들기** 단추를 클릭합니다.

    [![Android Emulator 만들기 화면](images/win-2019/create-emulator-sml.png)](images/win-2019/create-emulator.png#lightbox)

8. 에뮬레이터를 만들면 디바이스 관리자 창이 반환됩니다. **시작** 단추를 클릭하여 새 에뮬레이터를 시작합니다.

    ![디바이스 관리자의 Android Emulator](images/win-2019/start-emulator.png)

9. 이제 Visual Studio 2019는 **디버그** 단추에 새 에뮬레이터의 이름을 표시합니다.

    ![디버그 단추의 Android Emulator 이름](images/win-2019/debug-emulator-name.png)

10. **디버그** 단추를 클릭하여 애플리케이션을 Android Emulator에 빌드 및 배포합니다.

    ![애플리케이션을 표시하는 Android Emulator](images/win-2019/android-emulator.png)

## <a name="customize-the-application"></a>애플리케이션 사용자 지정

대화형 기능을 추가하도록 애플리케이션을 사용자 정의할 수 있습니다. 애플리케이션에 사용자 상호 작용을 추가하려면 다음 단계를 수행합니다.

1. **MainPage.xaml**을 편집하여 다음 XAML을 `</StackLayout>`이 종료되기 전에 추가합니다.

    ```xaml
    <Button Text="Click Me" Clicked="Button_Clicked" />
    ```

2. **MainPage.xaml.cs**를 편집하여 다음 코드를 클래스 끝에 추가합니다.

    ```csharp
    int count = 0;
    void Button_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

3. Android에서 앱을 디버그합니다.

    ![Android 앱](images/win/07-sml.png)

> [!NOTE]
> 애플리케이션 예제에는 비디오에서 다루지 않는 추가 대화형 기능이 포함되어 있습니다.

## <a name="build-an-ios-app-in-visual-studio-2019"></a>Visual Studio 2019에서 iOS 앱 빌드

네트워크로 연결된 Mac 컴퓨터를 사용하여 Visual Studio에서 iOS 앱을 빌드하고 디버그할 수 있습니다. 자세한 내용은 [설치 지침](~/ios/get-started/installation/windows/index.md)을 참조하세요.

이 비디오에서는 Windows에서 Visual Studio 2019를 사용하여 iOS 앱을 빌드 및 테스트하는 과정을 다룹니다.

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-iOS-App-with-Visual-Studio-2019-and-Xamarin/player]

::: zone-end
::: zone pivot="win-vs2017"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-Android--iOS-App-in-Visual-Studio-2017/player]

## <a name="step-by-step-instructions-for-windows"></a>Windows용 단계별 지침

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

위의 비디오와 함께 다음 단계를 수행합니다.

1. **파일 > 새로 만들기 > 프로젝트...** 를 선택하거나 **새 프로젝트 만들기...** 단추를 누른 다음 **Visual C# > 플랫폼 간 > 모바일 앱(Xamarin.Forms)** 을 선택합니다.

    [![모바일 앱(Xamarin.Forms)](images/win/01-sml.png)](images/win/01.png#lightbox)

2. **.NET Standard** 코드 공유와 함께 **Android** 및 **iOS**가 선택되었는지 확인합니다.

    [![.NET Standard가 포함된 Android 및 iOS](images/win/02-sml.png)](images/win/02.png#lightbox)

3. NuGet 패키지가 복원될 때까지 기다립니다(상태 표시줄에 "복원 완료" 메시지가 표시됨).

4. 디버그 단추(또는 **디버그 > 디버깅 시작** 메뉴 항목)를 눌러 Android 에뮬레이터를 시작합니다.

5. **MainPage.xaml**을 편집하여 다음 XAML을 `</StackLayout>`이 종료되기 전에 추가합니다.

    ```xaml
    <Button Text="Click Me" Clicked="Button_Clicked" />
    ```

6. **MainPage.xaml.cs**를 편집하여 다음 코드를 클래스 끝에 추가합니다.

    ```csharp
    int count = 0;
    void Button_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

7. Android에서 앱을 디버그합니다.

    ![Android 앱](images/win/07-sml.png)

    > [!TIP]
    > 네트워크로 연결된 Mac 컴퓨터를 사용하여 Visual Studio에서 iOS 앱을 빌드하고 디버그할 수 있습니다. 자세한 내용은 [설치 지침](~/ios/get-started/installation/windows/index.md)을 참조하세요.

::: zone-end
::: zone pivot="macos"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-iOS--Android-App-in-Visual-Studio-for-Mac/player]

## <a name="step-by-step-instructions-for-mac"></a>Mac용 단계별 지침

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

위의 비디오와 함께 다음 단계를 수행합니다.

1. **파일 > 새 솔루션...** 을 선택하거나 **새 프로젝트...** 단추를 누른 다음, **멀티 플랫폼 > 앱 > 빈 Forms 앱**을 선택합니다.

    [![빈 Forms 앱](images/01-sml.png)](images/01.png#lightbox)

2. **.NET Standard** 공유가 포함된 **Android** 및 **iOS**가 선택되었는지 확인합니다.

    [![.NET Standard가 포함된 Android 및 iOS](images/02-sml.png)](images/02.png#lightbox)

3. 솔루션을 마우스 오른쪽 단추로 클릭하여 NuGet 패키지를 복원합니다.

    ![Android 앱](images/03-sml.png)

4. 디버그 단추(또는 **실행 > 디버깅 시작**)를 눌러 Android 에뮬레이터를 시작합니다.

5. **MainPage.xaml**을 편집하여 다음 XAML을 `</StackLayout>`이 종료되기 전에 추가합니다.

    ```xaml
    <Button Text="Click Me" Clicked="Handle_Clicked" />
    ```

6. **MainPage.xaml.cs**를 편집하여 다음 코드를 클래스 끝에 추가합니다.

    ```csharp
    int count = 0;
    void Handle_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

7. Android에서 앱을 디버그합니다.

    ![Android 앱](images/07-sml.png)

8. 마우스 오른쪽 단추를 클릭하여 iOS를 **시작 프로젝트**로 설정합니다.

    [![시작 프로젝트를 iOS로 설정](images/08-sml.png)](images/08.png#lightbox)

9. iOS에서 앱을 디버그합니다.

    ![iOS 앱](images/09-sml.png)

::: zone-end

완료된 코드는 [샘플 갤러리](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)에서 다운로드하거나 [GitHub](https://github.com/xamarin/xamarin-forms-samples/tree/master/GetStarted/FirstApp)에서 볼 수 있습니다.

## <a name="next-steps"></a>다음 단계

- [단일 페이지 빠른 시작](~/get-started/quickstarts/single-page.md) &ndash; 더 기능적인 앱을 빌드하세요.
- [Xamarin.Forms 샘플](~/xamarin-forms/samples/index.md) &ndash; 코드 예제 및 샘플 앱을 다운로드하여 실행합니다.
- [모바일 앱 전자책 만들기](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) &ndash; PDF로 제공되고 수백 개의 추가 샘플을 포함하는 Xamarin.Forms 개발을 설명하는 심층적인 챕터입니다.
