---
title: Xamarin.Forms 빠른 시작
description: 이 문서에서는 Xamarin.Forms를 사용하여 메모를 입력하고 디바이스 스토리지에 저장하는 간단한 플랫폼 간 Notes 애플리케이션을 만드는 방법을 설명합니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E8CF05B1-54B9-428B-8518-D068837BD61E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/21/2018
ms.openlocfilehash: 880fa527d91098cf8c5f8c46cd9c5cce9ec9a489
ms.sourcegitcommit: 744c0a50420bb091fca8b92a84c20e61c741cf9e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52743138"
---
# <a name="xamarinforms-quickstart"></a>Xamarin.Forms 빠른 시작

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)

이 빠른 시작에서는 Xamarin.Forms를 사용하여 메모를 입력하고 디바이스 스토리지에 저장하는 간단한 플랫폼 간 Notes 애플리케이션을 만드는 방법을 설명합니다. 최종 응용 프로그램은 다음과 같습니다.

[![](quickstart-experimental-images/screenshots-sml.png "Notes 애플리케이션")](quickstart-experimental-images/screenshots.png#lightbox "Notes 애플리케이션")

::: zone pivot="windows"

## <a name="get-started-with-visual-studio"></a>Visual Studio 시작

1. **시작** 화면에서 Visual Studio를 시작합니다. 이렇게 하면 시작 페이지가 열립니다.

    ![](quickstart-experimental-images/vs/start-page.png "Visual Studio")

2. Visual Studio에서 새 프로젝트를 만들려면 **새 프로젝트 만들기...** 를 클릭합니다.

    ![](quickstart-experimental-images/vs/new-solution.png "새 프로젝트")

3. **새 프로젝트** 대화 상자에서 **플랫폼 간**을 클릭하고, **모바일 앱(Xamarin.Forms)** 템플릿을 선택하고, 이름을 **Notes**로 설정하고, 프로젝트에 대한 적절한 위치를 선택하고, **확인** 단추를 클릭합니다.

    ![](quickstart-experimental-images/vs/new-project.png "플랫폼 간 프로젝트 템플릿")

    > [!IMPORTANT]
    > 이 빠른 시작의 C# 및 XAML 코드 조각은 솔루션의 이름이 **Notes**이어야 합니다. 이 빠른 시작에서 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

4. **새 플랫폼 간 앱** 대화 상자에서 **비어 있는 앱**을 클릭하고, **.NET Standard**를 코드 공유 전략으로 선택하고, **확인** 단추를 클릭합니다.

    ![](quickstart-experimental-images/vs/new-app.png "새 플랫폼 간 앱")

5. **솔루션 탐색기**의 **Notes** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다.

    ![](quickstart-experimental-images/vs/open-mainpage-xaml.png "MainPage.xaml 열기")

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
            <Editor x:Name="_editor"
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

    이 코드는 텍스트를 표시하는 [`Label`](xref:Xamarin.Forms.Label), 텍스트 입력을 위한 [`Editor`](xref:Xamarin.Forms.Editor), 파일을 저장 또는 삭제하도록 애플리케이션에 지시하는 두 개의 [`Button`](xref:Xamarin.Forms.Button) 인스턴스로 구성된 페이지의 사용자 인터페이스를 선언적으로 정의합니다. 두 개의 `Button` 인스턴스가 [`Grid`](xref:Xamarin.Forms.Grid)에 가로로 배치되고 `Label`, `Editor` 및 `Grid`가 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)에 세로로 배치됩니다.

    **CTRL+S** 키를 눌러 변경 내용을 **MainPage.xaml**에 저장하고 파일을 선택합니다.

7. **솔루션 탐색기**의 **Notes** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다.

    ![](quickstart-experimental-images/vs/open-mainpage-codebehind.png "MainPage.xaml.cs 열기")

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
                    _editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, _editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                _editor.Text = string.Empty;
            }
        }
    }
    ```

    이 코드는 애플리케이션의 로컬 애플리케이션 데이터 폴더에 노트 데이터를 저장하는 `notes.txt` 파일을 참조하는 `_fileName` 필드를 정의합니다. 페이지 생성자가 실행될 때 파일이 읽혀지고(있는 경우) [`Editor`](xref:Xamarin.Forms.Editor)에 표시됩니다. **저장** [`Button`](xref:Xamarin.Forms.Button)을 누르면 `OnSaveButtonClicked` 이벤트 처리기가 실행되어 `Editor`의 콘텐츠가 파일에 저장됩니다. **삭제** `Button`을 누르면 `OnDeleteButtonClicked` 이벤트 처리기가 실행되어 파일을 삭제하고(있는 경우) `Editor`에서 모든 텍스트를 제거합니다.

    **CTRL+S** 키를 눌러 변경 내용을 **MainPage.xaml.cs**에 저장하고 파일을 선택합니다.

### <a name="building-the-quickstart"></a>빠른 시작 빌드

1. Visual Studio에서 **빌드 > 솔루션 빌드** 메뉴 항목을 선택하거나 F6을 누릅니다. 솔루션이 빌드하고 성공 메시지가 Visual Studio 상태 표시줄에 표시됩니다.

      ![](quickstart-experimental-images/vs/build-succeeded.png "빌드 성공")

    오류가 있는 경우 이전 단계를 반복하고 솔루션이 성공적으로 빌드할 때까지 실수를 수정합니다.

2. Visual Studio 도구 모음에서 선택한 Android 에뮬레이터의 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](quickstart-experimental-images/vs/android-start.png "Visual Studio Android 도구 모음")

    ![](quickstart-experimental-images/vs/notes-android.png "Android 에뮬레이터의 노트")

    노트를 입력하고 **저장** 단추를 누릅니다.

    > [!NOTE]
    > 다음 단계는 Xamarin.Forms 개발을 위한 시스템 요구 사항을 충족하는 [페어링된 Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)이 있는 경우에만 수행해야 합니다.

3. Visual Studio 도구 모음에서 **Notes.iOS** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **시작 프로젝트로 설정**을 선택합니다.

      ![](quickstart-experimental-images/vs/set-as-startup-project-ios.png "iOS를 시작 프로젝트로 설정")

4. Visual Studio 도구 모음에서 선택한 [iOS 원격 에뮬레이터](~/tools/ios-simulator/index.md)의 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](quickstart-experimental-images/vs/ios-start.png "Visual Studio iOS 도구 모음")

    ![](quickstart-experimental-images/vs/notes-ios.png "iOS 시뮬레이터의 노트")

    노트를 입력하고 **저장** 단추를 누릅니다.

::: zone-end
::: zone pivot="macos"

## <a name="get-started-with-visual-studio-for-mac"></a>Mac용 Visual Studio 시작

1. Mac용 Visual Studio를 시작하고, 시작 페이지에서 **새 프로젝트...** 를 클릭하여 새 프로젝트를 만듭니다.

    ![](quickstart-experimental-images/vsmac/new-project.png "새 솔루션")

2. **새 프로젝트에 대한 템플릿 선택** 대화 상자에서 **다중 플랫폼 > 앱**을 클릭하여 **빈 Forms 앱** 템플릿을 선택하고 **다음** 단추를 클릭합니다.

    ![](quickstart-experimental-images/vsmac/choose-template.png "템플릿 선택")

3. **빈 양식 앱 구성** 대화 상자에서 새 앱 이름을 **Notes**로 설정하고, **.NET Standard** 라디오 단추가 선택되었는지 확인하고, **다음** 단추를 클릭합니다.    

    ![](quickstart-experimental-images/vsmac/configure-app.png "Forms 응용프로그램 구성")

4. **새 빈 Forms 앱 구성** 대화 상자에서 솔루션 및 프로젝트 이름을 **Notes**로 설정된 채로 두고, 프로젝트에 적절한 위치를 선택하고, **만들기** 단추를 클릭하여 프로젝트를 만듭니다.

    ![](quickstart-experimental-images/vsmac/configure-project.png "Forms Project 구성")

    > [!IMPORTANT]
    > 이 빠른 시작의 C# 및 XAML 코드 조각은 솔루션과 프로젝의 이름이 모두 **Notes**이어야 합니다. 이 빠른 시작에서 코드를 프로젝트로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

5. **Solution Pad**의 **Notes** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다.

    ![](quickstart-experimental-images/vsmac/mainpage-xaml.png "MainPage.xaml")

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
            <Editor x:Name="_editor"
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

    이 코드는 텍스트를 표시하는 [`Label`](xref:Xamarin.Forms.Label), 텍스트 입력을 위한 [`Editor`](xref:Xamarin.Forms.Editor), 파일을 저장 또는 삭제하도록 애플리케이션에 지시하는 두 개의 [`Button`](xref:Xamarin.Forms.Button) 인스턴스로 구성된 페이지의 사용자 인터페이스를 선언적으로 정의합니다. 두 개의 `Button` 인스턴스가 [`Grid`](xref:Xamarin.Forms.Grid)에 가로로 배치되고 `Label`, `Editor` 및 `Grid`가 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)에 세로로 배치됩니다.

    **File > Save**를 선택하거나(또는 **&#8984; + S**를 눌러) 변경 내용을 **MainPage.xaml**에 저장하고 파일을 닫습니다.

7. **Solution Pad**의 **Notes** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다.

    ![](quickstart-experimental-images/vsmac/mainpage-xaml-cs.png "MainPage.xaml.cs")

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
                    _editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, _editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                _editor.Text = string.Empty;
            }
        }
    }
    ```

    이 코드는 애플리케이션의 로컬 애플리케이션 데이터 폴더에 노트 데이터를 저장하는 `notes.txt` 파일을 참조하는 `_fileName` 필드를 정의합니다. 페이지 생성자가 실행될 때 파일이 읽혀지고(있는 경우) [`Editor`](xref:Xamarin.Forms.Editor)에 표시됩니다. **저장** [`Button`](xref:Xamarin.Forms.Button)을 누르면 `OnSaveButtonClicked` 이벤트 처리기가 실행되어 `Editor`의 콘텐츠가 파일에 저장됩니다. **삭제** `Button`을 누르면 `OnDeleteButtonClicked` 이벤트 처리기가 실행되어 파일을 삭제하고(있는 경우) `Editor`에서 모든 텍스트를 제거합니다.

    **File > Save**를 선택하거나(또는 **&#8984; + S**를 눌러) 변경 내용을 **MainPage.xaml.cs**에 저장하고 파일을 닫습니다.

### <a name="building-the-quickstart"></a>빠른 시작 빌드

1. Mac용 Visual Studio에서 **빌드 > 솔루션 빌드** 메뉴 항목을 선택(하거나 **&#8984; + B** 키를 누릅니다). 프로젝트가 빌드되고 성공 메시지가 Mac용 Visual Studio 도구 모음에 표시됩니다.

      ![](quickstart-experimental-images/vsmac/build-successful.png "빌드 성공")

    오류가 있는 경우 이전 단계를 반복하고 프로젝트가 성공적으로 빌드할 때까지 실수를 수정합니다.

2. **Solution Pad**에서 **Notes.iOS** 프로젝트를 선택하고 **시작 프로젝트로 설정**을 마우스 오른쪽 단추로 클릭하여 선택합니다.

      ![](quickstart-experimental-images/vsmac/set-startup-project-ios.png "iOS를 시작 프로젝트로 설정")

3. Mac용 Visual Studio 도구 모음에서 선택한 iOS Simulator 안에 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

      ![](quickstart-experimental-images/vsmac/start.png "Visual Studio for Mac 도구 모음")

      ![](quickstart-experimental-images/vsmac/notes-ios.png "iOS 시뮬레이터의 노트")

    노트를 입력하고 **저장** 단추를 누릅니다.

4. **Solution Pad**에서 **Notes.Droid** 프로젝트를 선택하고 **시작 프로젝트로 설정**을 마우스 오른쪽 단추로 클릭하여 선택합니다.

      ![](quickstart-experimental-images/vsmac/set-startup-project-android.png "Android를 시작 프로젝트로 설정")

5. Mac용 Visual Studio 도구 모음에서 선택한 Android 에뮬레이터 안에 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

      ![](quickstart-experimental-images/vsmac/notes-android.png "Android Emulator의 노트")

    노트를 입력하고 **저장** 단추를 누릅니다.

::: zone-end

## <a name="related-links"></a>관련 링크

- [노트(샘플)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)
