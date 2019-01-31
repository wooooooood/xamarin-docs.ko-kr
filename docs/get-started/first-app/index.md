---
title: 첫 번째 Xamarin.Forms 앱 빌드
description: Visual Studio에서 첫 번째 Xamarin.Forms 애플리케이션을 빌드하는 방법을 보여주는 비디오 가이드입니다.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 72B6AF82-4D98-47E5-AB54-0A35B3253468
ms.technology: xamarin-forms
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 09/24/2018
ms.openlocfilehash: d89488e3f6e42f84fc9519eedaa38c99b90ae068
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55293353"
---
# <a name="build-your-first-xamarinforms-app"></a>첫 번째 Xamarin.Forms 앱 빌드

_이 비디오를 보고 따라서 Xamarin.Forms로 첫 번째 모바일 앱을 만듭니다._

::: zone pivot="windows"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-Android--iOS-App-in-Visual-Studio-2017/player]

## <a name="step-by-step-instructions-for-windows"></a>Windows용 단계별 지침

위의 비디오와 함께 다음 단계를 수행합니다.

1. **파일 > 새로 만들기 > 프로젝트...** 를 선택하거나 **새 프로젝트 만들기...** 단추를 누른 다음, **Visual C# > Cross-Platform > 모바일 앱(Xamarin.Forms)** 를 선택합니다.

    [![모바일 앱(Xamarin.Forms)](images/win/01-sml.png)](images/win/01.png#lightbox)

2. **.NET Standard** 코드 공유와 함께 **Android** 및 **iOS**가 선택되었는지 확인합니다.

    [![.NET Standard가 포함된 Android 및 iOS](images/win/02-sml.png)](images/win/02.png#lightbox)

3. NuGet 패키지가 복원될 때까지 기다립니다(상태 표시줄에 "복원 완료" 메시지가 표시됨).

4. 디버그 단추(또는 **디버그 > 디버깅 시작** 메뉴 항목)를 눌러 Android 에뮬레이터를 시작합니다.

5. **MainPage.xaml**을 편집하여 다음 XAML을 `</StackPanel>`이 종료되기 전에 추가합니다.

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

위의 비디오와 함께 다음 단계를 수행합니다.

1. **파일 > 새 솔루션...** 을 선택하거나 **새 프로젝트...** 단추를 누른 다음, **멀티 플랫폼 > 앱 > 빈 Forms 앱**을 선택합니다.

    [![빈 Forms 앱](images/01-sml.png)](images/01.png#lightbox)

2. **.NET Standard** 공유가 포함된 **Android** 및 **iOS**가 선택되었는지 확인합니다.

    [![.NET Standard가 포함된 Android 및 iOS](images/02-sml.png)](images/02.png#lightbox)

3. 솔루션을 마우스 오른쪽 단추로 클릭하여 NuGet 패키지를 복원합니다.

    ![Android 앱](images/03-sml.png)

4. 디버그 단추(또는 **실행 > 디버깅 시작**)를 눌러 Android 에뮬레이터를 시작합니다.

5. **MainPage.xaml**을 편집하여 다음 XAML을 `</StackPanel>`이 종료되기 전에 추가합니다.

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

완료된 코드는 [샘플 갤러리](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/FirstApp/)에서 다운로드하거나 [GitHub](https://github.com/xamarin/xamarin-forms-samples/tree/master/GetStarted/FirstApp)에서 볼 수 있습니다.

## <a name="next-steps"></a>다음 단계

- [빠른 시작 페이지를 단일](~/get-started/quickstarts/single-page.md) &ndash; 더 많은 기능 앱을 빌드합니다.
- [Xamarin.Forms 샘플](~/xamarin-forms/samples/index.yml) &ndash; 코드 예제 및 샘플 앱을 다운로드하여 실행합니다.
- [모바일 앱 전자책 만들기](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) &ndash; PDF로 제공되고 수백 개의 추가 샘플을 포함하는 Xamarin.Forms 개발을 설명하는 심층적인 챕터입니다.
