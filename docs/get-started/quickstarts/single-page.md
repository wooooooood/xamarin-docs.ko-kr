---
title: 단일 페이지 Xamarin.Forms 애플리케이션 만들기
description: 이 문서에서는 노트를 입력하고 디바이스 스토리지에 유지할 수 있는 단일 페이지 플랫폼 간의 Xamarin.Forms 애플리케이션 생성 방법을 설명합니다.
zone_pivot_groups: platform-dev16
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E8CF05B1-54B9-428B-8518-D068837BD61E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: c1d7aa1535fe979df222aaedc6ba2cf3bae0d51c
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79303632"
---
# <a name="create-a-single-page-xamarinforms-application"></a>단일 페이지 Xamarin.Forms 애플리케이션 만들기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)

이 빠른 시작에서 다음과 같은 작업을 수행하는 방법을 알아봅니다.

- 플랫폼 간 Xamarin.Forms 애플리케이션을 만듭니다.
- XAML(eXtensible Application Markup Language)을 사용하는 페이지에 대해 사용자 인터페이스를 정의합니다.
- 코드에서 XAML 사용자 인터페이스 요소와 상호작용합니다.

빠른 시작에서는 노트를 입력하고 디바이스 스토리지에 유지할 수 있는 단일 페이지 플랫폼 간 Xamarin.Forms 애플리케이션을 만드는 방법을 안내합니다. 최종 애플리케이션은 다음과 같습니다.

[![](single-page-images/screenshots-sml.png "Notes Application")](single-page-images/screenshots.png#lightbox "Notes Application")

::: zone pivot="windows"

### <a name="prerequisites"></a>사전 요구 사항

- **.NET을 사용한 모바일 개발** 워크로드가 설치된 Visual Studio 2019(최신 릴리스)
- C# 관련 지식
- (선택 사항) iOS상의 애플리케이션 빌드를 위해 페어링된 Mac

이 사전 요구 사항에 대한 자세한 내용은 [Xamarin 설치](~/get-started/installation/index.md)를 참조하세요. Visual Studio 2019를 Mac 빌드 호스트에 연결하는 방법에 대한 자세한 내용은 [Xamarin.iOS 개발을 위해 Mac에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md)을 참조하세요.

## <a name="get-started-with-visual-studio-2019"></a>Visual Studio 2019 시작

1. Visual Studio 2019를 시작하고 시작 창에서 **새 프로젝트 만들기**를 클릭하여 새 프로젝트를 만듭니다.

    ![](single-page-images/vs/new-solution-2019.png "New Project")

2. **새 프로젝트 만들기** 창에서 **프로젝트 형식** 드롭다운의 **모바일**을 선택한 다음 **모바일 앱(Xamarin.Forms)** 템플릿을 선택하고 **다음** 단추를 클릭합니다.

    ![](single-page-images/vs/new-project-2019.png "Cross-Platform Project Templates")

3. **새 프로젝트 구성** 창에서 **프로젝트 이름**을 **Notes**로 지정하고 적절한 프로젝트 위치를 선택한 다음 **만들기** 단추를 클릭합니다.

    ![](single-page-images/vs/configure-project.png "Configure your Project")

    > [!IMPORTANT]
    > 이 빠른 시작의 C# 및 XAML 코드 조각은 솔루션의 이름이 **Notes**이어야 합니다. 이 빠른 시작에서 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

4. **새 플랫폼 간 앱** 대화 상자에서 **빈 앱**을 클릭한 후 **확인** 단추를 클릭합니다.

    ![](single-page-images/vs/new-app-2019.png "New Cross-Platform App")

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [Xamarin.Forms 애플리케이션 분석](deepdive.md#anatomy-of-a-xamarinforms-application)을 참조하세요.

5. **솔루션 탐색기**의 **Notes** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다.

    ![](single-page-images/vs/open-mainpage-xaml-2019.png "Open MainPage.xaml")

6. **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 텍스트를 표시하는 [`Label`](xref:Xamarin.Forms.Label), 텍스트 입력을 위한 [`Editor`](xref:Xamarin.Forms.Editor), 파일을 저장 또는 삭제하도록 애플리케이션에 지시하는 두 개의 [`Button`](xref:Xamarin.Forms.Button) 인스턴스로 구성된 페이지의 사용자 인터페이스를 선언적으로 정의합니다. 두 개의 `Button` 인스턴스가 [`Grid`](xref:Xamarin.Forms.Grid)에 가로로 배치되고 `Label`, `Editor` 및 `Grid`가 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 세로로 배치됩니다. 사용자 인터페이스를 만드는 방법에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)의 [사용자 인터페이스](deepdive.md#user-interface)를 참조하세요.

    **CTRL+S** 키를 눌러 변경 내용을 **MainPage.xaml**에 저장하고 파일을 닫습니다.

7. **솔루션 탐색기**의 **Notes** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다.

    ![](single-page-images/vs/open-mainpage-codebehind-2019.png "Open MainPage.xaml.cs")

8. **MainPage.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    이 코드는 애플리케이션의 로컬 애플리케이션 데이터 폴더에 노트 데이터를 저장하는 `notes.txt` 파일을 참조하는 `_fileName` 필드를 정의합니다. 페이지 생성자가 실행될 때 파일이 읽혀지고(있는 경우) [`Editor`](xref:Xamarin.Forms.Editor)에 표시됩니다. **저장** [`Button`](xref:Xamarin.Forms.Button)을 누르면 `OnSaveButtonClicked` 이벤트 처리기가 실행되어 `Editor`의 콘텐츠가 파일에 저장됩니다. **삭제** `Button`을 누르면 `OnDeleteButtonClicked` 이벤트 처리기가 실행되어 파일을 삭제하고(있는 경우) `Editor`에서 모든 텍스트를 제거합니다. 사용자 상호 작용에 대한 자세한 내용은[Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)의 [사용자 상호 작용에 응답](deepdive.md#responding-to-user-interaction)을 참조하세요.

    **CTRL+S** 키를 눌러 변경 내용을 **MainPage.xaml.cs**에 저장하고 파일을 닫습니다.

### <a name="building-the-quickstart"></a>빠른 시작 빌드

1. Visual Studio에서 **빌드 > 솔루션 빌드** 메뉴 항목을 선택하거나 F6을 누릅니다. 솔루션이 빌드하고 성공 메시지가 Visual Studio 상태 표시줄에 표시됩니다.

      ![](single-page-images/vs/build-succeeded.png "Build Succeeded")

    오류가 있는 경우 이전 단계를 반복하고 솔루션이 성공적으로 빌드할 때까지 실수를 수정합니다.

2. Visual Studio 도구 모음에서 선택한 Android 에뮬레이터의 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](single-page-images/vs/android-start.png "Visual Studio Android Toolbar")

    [![](single-page-images/vs/notes-android.png "Notes in the Android Emulator")](single-page-images/vs/notes-android-large.png#lightbox "Notes in the Android Simulator")

    노트를 입력하고 **저장** 단추를 누릅니다.

    각 플랫폼에서의 애플리케이션을 시작 방법에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)의 [각 플랫폼에서 애플리케이션 시작](deepdive.md#launching-the-application-on-each-platform)을 참조하세요.

    > [!NOTE]
    > 다음 단계는 Xamarin.Forms 개발을 위한 시스템 요구 사항을 충족하는 [페어링된 Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)이 있는 경우에만 수행해야 합니다.

3. Visual Studio 도구 모음에서 **Notes.iOS** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **시작 프로젝트로 설정**을 선택합니다.

      ![](single-page-images/vs/set-as-startup-project-ios.png "Set iOS as Startup Project")

4. Visual Studio 도구 모음에서 선택한 [iOS 원격 에뮬레이터](~/tools/ios-simulator/index.md)의 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](single-page-images/vs/ios-start.png "Visual Studio iOS Toolbar")

    [![](single-page-images/vs/notes-ios.png "Notes in the iOS Simulator")](single-page-images/vs/notes-ios-large.png#lightbox "Notes in the iOS Simulator")

    노트를 입력하고 **저장** 단추를 누릅니다.

    각 플랫폼에서의 애플리케이션을 시작 방법에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)의 [각 플랫폼에서 애플리케이션 시작](deepdive.md#launching-the-application-on-each-platform)을 참조하세요.

::: zone-end
::: zone pivot="win-vs2017"

### <a name="prerequisites"></a>사전 요구 사항

- **.NET을 사용한 모바일 개발** 워크로드가 설치된 Visual Studio 2017
- C# 관련 지식
- (선택 사항) iOS상의 애플리케이션 빌드를 위해 페어링된 Mac

이 사전 요구 사항에 대한 자세한 내용은 [Xamarin 설치](~/get-started/installation/index.md)를 참조하세요. Visual Studio 2019를 Mac 빌드 호스트에 연결하는 방법에 대한 자세한 내용은 [Xamarin.iOS 개발을 위해 Mac에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md)을 참조하세요.

## <a name="get-started-with-visual-studio-2017"></a>Visual Studio 2017 시작

1. Visual Studio 2017을 시작하고 시작 창에서 **새 프로젝트 만들기**를 클릭하여 새 프로젝트를 만듭니다.

    ![](single-page-images/vs/new-solution.png "New Project")

2. **새 프로젝트** 대화 상자에서 **플랫폼 간**을 클릭하고, **모바일 앱(Xamarin.Forms)** 템플릿을 선택하고, 이름을 **Notes**로 설정하고, 프로젝트에 대한 적절한 위치를 선택하고, **확인** 단추를 클릭합니다.

    ![](single-page-images/vs/new-project.png "Cross-Platform Project Templates")

    > [!IMPORTANT]
    > 이 빠른 시작의 C# 및 XAML 코드 조각은 솔루션의 이름이 **Notes**이어야 합니다. 이 빠른 시작에서 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

3. **새 플랫폼 간 앱** 대화 상자에서 **비어 있는 앱**을 클릭하고, **.NET Standard**를 코드 공유 전략으로 선택하고, **확인** 단추를 클릭합니다.

    ![](single-page-images/vs/new-app.png "New Cross-Platform App")

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [Xamarin.Forms 애플리케이션 분석](deepdive.md#anatomy-of-a-xamarinforms-application)을 참조하세요.

4. **솔루션 탐색기**의 **Notes** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다.

    ![](single-page-images/vs/open-mainpage-xaml.png "Open MainPage.xaml")

5. **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 텍스트를 표시하는 [`Label`](xref:Xamarin.Forms.Label), 텍스트 입력을 위한 [`Editor`](xref:Xamarin.Forms.Editor), 파일을 저장 또는 삭제하도록 애플리케이션에 지시하는 두 개의 [`Button`](xref:Xamarin.Forms.Button) 인스턴스로 구성된 페이지의 사용자 인터페이스를 선언적으로 정의합니다. 두 개의 `Button` 인스턴스가 [`Grid`](xref:Xamarin.Forms.Grid)에 가로로 배치되고 `Label`, `Editor` 및 `Grid`가 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 세로로 배치됩니다. 사용자 인터페이스를 만드는 방법에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)의 [사용자 인터페이스](deepdive.md#user-interface)를 참조하세요.

    **CTRL+S** 키를 눌러 변경 내용을 **MainPage.xaml**에 저장하고 파일을 닫습니다.

6. **솔루션 탐색기**의 **Notes** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다.

    ![](single-page-images/vs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

7. **MainPage.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    이 코드는 애플리케이션의 로컬 애플리케이션 데이터 폴더에 노트 데이터를 저장하는 `notes.txt` 파일을 참조하는 `_fileName` 필드를 정의합니다. 페이지 생성자가 실행될 때 파일이 읽혀지고(있는 경우) [`Editor`](xref:Xamarin.Forms.Editor)에 표시됩니다. **저장** [`Button`](xref:Xamarin.Forms.Button)을 누르면 `OnSaveButtonClicked` 이벤트 처리기가 실행되어 `Editor`의 콘텐츠가 파일에 저장됩니다. **삭제** `Button`을 누르면 `OnDeleteButtonClicked` 이벤트 처리기가 실행되어 파일을 삭제하고(있는 경우) `Editor`에서 모든 텍스트를 제거합니다. 사용자 상호 작용에 대한 자세한 내용은[Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)의 [사용자 상호 작용에 응답](deepdive.md#responding-to-user-interaction)을 참조하세요.

    **CTRL+S** 키를 눌러 변경 내용을 **MainPage.xaml.cs**에 저장하고 파일을 닫습니다.

### <a name="building-the-quickstart"></a>빠른 시작 빌드

1. Visual Studio에서 **빌드 > 솔루션 빌드** 메뉴 항목을 선택하거나 F6을 누릅니다. 솔루션이 빌드하고 성공 메시지가 Visual Studio 상태 표시줄에 표시됩니다.

      ![](single-page-images/vs/build-succeeded.png "Build Succeeded")

    오류가 있는 경우 이전 단계를 반복하고 솔루션이 성공적으로 빌드할 때까지 실수를 수정합니다.

2. Visual Studio 도구 모음에서 선택한 Android 에뮬레이터의 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](single-page-images/vs/android-start.png "Visual Studio Android Toolbar")

    [![](single-page-images/vs/notes-android.png "Notes in the Android Emulator")](single-page-images/vs/notes-android-large.png#lightbox "Notes in the Android Simulator")

    노트를 입력하고 **저장** 단추를 누릅니다.

    각 플랫폼에서의 애플리케이션을 시작 방법에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)의 [각 플랫폼에서 애플리케이션 시작](deepdive.md#launching-the-application-on-each-platform)을 참조하세요.

    > [!NOTE]
    > 다음 단계는 Xamarin.Forms 개발을 위한 시스템 요구 사항을 충족하는 [페어링된 Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)이 있는 경우에만 수행해야 합니다.

3. Visual Studio 도구 모음에서 **Notes.iOS** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **시작 프로젝트로 설정**을 선택합니다.

      ![](single-page-images/vs/set-as-startup-project-ios.png "Set iOS as Startup Project")

4. Visual Studio 도구 모음에서 선택한 [iOS 원격 에뮬레이터](~/tools/ios-simulator/index.md)의 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](single-page-images/vs/ios-start.png "Visual Studio iOS Toolbar")

    [![](single-page-images/vs/notes-ios.png "Notes in the iOS Simulator")](single-page-images/vs/notes-ios-large.png#lightbox "Notes in the iOS Simulator")

    노트를 입력하고 **저장** 단추를 누릅니다.

    각 플랫폼에서의 애플리케이션을 시작 방법에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)의 [각 플랫폼에서 애플리케이션 시작](deepdive.md#launching-the-application-on-each-platform)을 참조하세요.

::: zone-end
::: zone pivot="macos"

### <a name="prerequisites"></a>사전 요구 사항

- iOS 및 Android 플랫폼 지원이 설치된 Mac용 Visual Studio(최신 릴리스)
- Xcode(최신 릴리스)
- C# 관련 지식

이 사전 요구 사항에 대한 자세한 내용은 [Xamarin 설치](~/get-started/installation/index.md)를 참조하세요.

## <a name="get-started-with-visual-studio-for-mac"></a>Mac용 Visual Studio 시작

1. Mac용 Visual Studio를 시작하고 시작 창에서 **새로 만들기**를 클릭하여 새 프로젝트를 만듭니다.

    ![](single-page-images/vsmac/new-project.png "New Solution")

2. **Choose a template for your new project** 대화 상자에서 **Multiplatform > App**을 클릭하여 **Blank Forms App** 템플릿을 선택하고 **Next** 단추를 클릭합니다.

    ![](single-page-images/vsmac/choose-template.png "Choose a Template")

3. **빈 양식 앱 구성** 대화 상자에서 새 앱 이름을 **Notes**로 설정하고, **.NET Standard** 라디오 단추가 선택되었는지 확인하고, **다음** 단추를 클릭합니다.    

    ![](single-page-images/vsmac/configure-app.png "Configure the Forms Application")

4. **새 빈 Forms 앱 구성** 대화 상자에서 솔루션 및 프로젝트 이름을 **Notes**로 설정된 채로 두고, 프로젝트에 적절한 위치를 선택하고, **만들기** 단추를 클릭하여 프로젝트를 만듭니다.

    ![](single-page-images/vsmac/configure-project.png "Configure the Forms Project")

    > [!IMPORTANT]
    > 이 빠른 시작의 C# 및 XAML 코드 조각은 솔루션과 프로젝의 이름이 모두 **Notes**이어야 합니다. 이 빠른 시작에서 코드를 프로젝트로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성된 .NET 표준 라이브러리에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [Xamarin.Forms 애플리케이션 분석](deepdive.md#anatomy-of-a-xamarinforms-application)을 참조하세요.

5. **Solution Pad**의 **Notes** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다.

    ![](single-page-images/vsmac/mainpage-xaml.png "MainPage.xaml")

6. **MainPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    이 코드는 텍스트를 표시하는 [`Label`](xref:Xamarin.Forms.Label), 텍스트 입력을 위한 [`Editor`](xref:Xamarin.Forms.Editor), 파일을 저장 또는 삭제하도록 애플리케이션에 지시하는 두 개의 [`Button`](xref:Xamarin.Forms.Button) 인스턴스로 구성된 페이지의 사용자 인터페이스를 선언적으로 정의합니다. 두 개의 `Button` 인스턴스가 [`Grid`](xref:Xamarin.Forms.Grid)에 가로로 배치되고 `Label`, `Editor` 및 `Grid`가 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 세로로 배치됩니다. 사용자 인터페이스를 만드는 방법에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)의 [사용자 인터페이스](deepdive.md#user-interface)를 참조하세요.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **MainPage.xaml**에 저장하고 파일을 닫습니다.

7. **Solution Pad**의 **Notes** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다.

    ![](single-page-images/vsmac/mainpage-xaml-cs.png "MainPage.xaml.cs")

8. **MainPage.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    이 코드는 애플리케이션의 로컬 애플리케이션 데이터 폴더에 노트 데이터를 저장하는 `notes.txt` 파일을 참조하는 `_fileName` 필드를 정의합니다. 페이지 생성자가 실행될 때 파일이 읽혀지고(있는 경우) [`Editor`](xref:Xamarin.Forms.Editor)에 표시됩니다. **저장** [`Button`](xref:Xamarin.Forms.Button)을 누르면 `OnSaveButtonClicked` 이벤트 처리기가 실행되어 `Editor`의 콘텐츠가 파일에 저장됩니다. **삭제** `Button`을 누르면 `OnDeleteButtonClicked` 이벤트 처리기가 실행되어 파일을 삭제하고(있는 경우) `Editor`에서 모든 텍스트를 제거합니다. 사용자 상호 작용에 대한 자세한 내용은[Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)의 [사용자 상호 작용에 응답](deepdive.md#responding-to-user-interaction)을 참조하세요.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **MainPage.xaml.cs**에 저장하고 파일을 닫습니다.

### <a name="building-the-quickstart"></a>빠른 시작 빌드

1. Mac용 Visual Studio에서 **빌드 > 솔루션 빌드** 메뉴 항목을 선택(하거나 **&#8984; + B** 키를 누릅니다). 프로젝트가 빌드되고 성공 메시지가 Mac용 Visual Studio 도구 모음에 표시됩니다.

      ![](single-page-images/vsmac/build-successful.png "Build Successful")

    오류가 있는 경우 이전 단계를 반복하고 프로젝트가 성공적으로 빌드할 때까지 실수를 수정합니다.

2. **Solution Pad**에서 **Notes.iOS** 프로젝트를 선택해 마우스 오른쪽 단추로 클릭하고 **시작 프로젝트로 설정**을 선택합니다.

      ![](single-page-images/vsmac/set-startup-project-ios.png "Set iOS as Startup Project")

3. Mac용 Visual Studio 도구 모음에서 선택한 iOS Simulator 안에 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

      ![](single-page-images/vsmac/start.png "Visual Studio for Mac Toolbar")

      [![](single-page-images/vsmac/notes-ios.png "Notes in the iOS Simulator")](single-page-images/vsmac/notes-ios-large.png#lightbox "Notes in the iOS Simulator")

    노트를 입력하고 **저장** 단추를 누릅니다.

    각 플랫폼에서의 애플리케이션을 시작 방법에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)의 [각 플랫폼에서 애플리케이션 시작](deepdive.md#launching-the-application-on-each-platform)을 참조하세요.

4. **Solution Pad**에서 **Notes.Droid** 프로젝트를 선택해 마우스 오른쪽 단추로 클릭하고 **시작 프로젝트로 설정**을 선택합니다.

      ![](single-page-images/vsmac/set-startup-project-android.png "Set Android as Startup Project")

5. Mac용 Visual Studio 도구 모음에서 선택한 Android 에뮬레이터 안에 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

      [![](single-page-images/vsmac/notes-android.png "Notes in the Android Emulator")](single-page-images/vsmac/notes-android-large.png#lightbox "Notes in the Android Simulator")

    노트를 입력하고 **저장** 단추를 누릅니다.

    각 플랫폼에서의 애플리케이션을 시작 방법에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)의 [각 플랫폼에서 애플리케이션 시작](deepdive.md#launching-the-application-on-each-platform)을 참조하세요.

::: zone-end

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 다음과 같은 방법을 배웠습니다.

- 플랫폼 간 Xamarin.Forms 애플리케이션을 만듭니다.
- XAML(eXtensible Application Markup Language)을 사용하는 페이지에 대해 사용자 인터페이스를 정의합니다.
- 코드에서 XAML 사용자 인터페이스 요소와 상호작용합니다.

이 단일 페이지 애플리케이션을 다중 페이지 애플리케이션으로 전환하려면 다음 빠른 시작 안내를 계속 진행하세요.

> [!div class="nextstepaction"]
> [다음](multi-page.md)

## <a name="related-links"></a>관련 링크

- [노트(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)
- [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)
