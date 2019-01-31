---
title: 단일 페이지 Xamarin.Forms 응용 프로그램 만들기
description: 이 문서에는 단일 페이지 플랫폼 간 Xamarin.Forms 응용 프로그램을 만드는, 메모를 입력 하 고 장치 저장소에 저장 하는 방법을 설명 합니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E8CF05B1-54B9-428B-8518-D068837BD61E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/02/2019
ms.openlocfilehash: 7696ece64da28f05bb15866214de4a7f1103d06f
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55293313"
---
# <a name="create-a-single-page-xamarinforms-application"></a>단일 페이지 Xamarin.Forms 응용 프로그램 만들기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)

이 빠른 시작에서는 배웁니다 방법:

- 플랫폼 간 Xamarin.Forms 응용 프로그램을 만듭니다.
- EXtensible Application Markup Language (XAML)을 사용 하는 페이지에 대 한 사용자 인터페이스를 정의 합니다.
- 코드에서 XAML 사용자 인터페이스 요소와 상호 작용 합니다.

빠른 시작을 메모를 입력 하 고 장치 저장소에 유지할 수 있는 플랫폼 간 Xamarin.Forms 응용 프로그램을 만드는 방법을 안내 합니다. 최종 애플리케이션은 다음과 같습니다.

[![](single-page-images/screenshots-sml.png "Notes 애플리케이션")](single-page-images/screenshots.png#lightbox "Notes 애플리케이션")

::: zone pivot="windows"

### <a name="prerequisites"></a>전제 조건

- Visual Studio 2017 (최신 릴리스)를 사용 하 여는 **.NET을 사용한 모바일 개발** 워크 로드가 설치 합니다.
- 기술 자료의 C#입니다.
- (선택 사항) IOS에서 응용 프로그램을 빌드하려면 페어링된 Mac.

이러한 필수 구성이 요소에 대 한 자세한 내용은 참조 하세요. [Xamarin 설치](~/cross-platform/get-started/installation/index.md)합니다. Visual Studio 2017을 Mac 빌드 호스트 연결에 대 한 자세한 내용은 [Xamarin.iOS 개발을 위한 Mac에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md)합니다.

## <a name="get-started-with-visual-studio"></a>Visual Studio 시작

1. Visual Studio를 시작 하 고 시작 페이지에서 클릭 **새 프로젝트 만들기...**  새 프로젝트를 만들려면:

    ![](single-page-images/vs/new-solution.png "새 프로젝트")

2. **새 프로젝트** 대화 상자에서 **플랫폼 간**을 클릭하고, **모바일 앱(Xamarin.Forms)** 템플릿을 선택하고, 이름을 **Notes**로 설정하고, 프로젝트에 대한 적절한 위치를 선택하고, **확인** 단추를 클릭합니다.

    ![](single-page-images/vs/new-project.png "플랫폼 간 프로젝트 템플릿")

    > [!IMPORTANT]
    > 이 빠른 시작의 C# 및 XAML 코드 조각은 솔루션의 이름이 **Notes**이어야 합니다. 이 빠른 시작에서 코드를 솔루션으로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

3. **새 플랫폼 간 앱** 대화 상자에서 **비어 있는 앱**을 클릭하고, **.NET Standard**를 코드 공유 전략으로 선택하고, **확인** 단추를 클릭합니다.

    ![](single-page-images/vs/new-app.png "New Cross Platform App")

    생성 되는.NET Standard 라이브러리에 대 한 자세한 내용은 참조 하세요. [Xamarin.Forms 응용 프로그램 분석](deepdive.md#anatomy-of-a-xamarinforms-application) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

4. **솔루션 탐색기**의 **Notes** 프로젝트에서 **MainPage.xaml**을 두 번 클릭하여 엽니다.

    ![](single-page-images/vs/open-mainpage-xaml.png "MainPage.xaml 열기")

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

    이 코드는 텍스트를 표시하는 [`Label`](xref:Xamarin.Forms.Label), 텍스트 입력을 위한 [`Editor`](xref:Xamarin.Forms.Editor), 파일을 저장 또는 삭제하도록 애플리케이션에 지시하는 두 개의 [`Button`](xref:Xamarin.Forms.Button) 인스턴스로 구성된 페이지의 사용자 인터페이스를 선언적으로 정의합니다. 두 개의 `Button` 인스턴스가 [`Grid`](xref:Xamarin.Forms.Grid)에 가로로 배치되고 `Label`, `Editor` 및 `Grid`가 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)에 세로로 배치됩니다. 사용자 인터페이스를 만드는 방법에 대 한 자세한 내용은 참조 하세요. [사용자 인터페이스](deepdive.md#user-interface) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    **CTRL+S** 키를 눌러 변경 내용을 **MainPage.xaml**에 저장하고 파일을 닫습니다.

6. **솔루션 탐색기**의 **Notes** 프로젝트에서 **MainPage.xaml**을 확장하고 **MainPage.xaml.cs**를 두 번 클릭하여 엽니다.

    ![](single-page-images/vs/open-mainpage-codebehind.png "MainPage.xaml.cs 열기")

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

    이 코드는 애플리케이션의 로컬 애플리케이션 데이터 폴더에 노트 데이터를 저장하는 `notes.txt` 파일을 참조하는 `_fileName` 필드를 정의합니다. 페이지 생성자가 실행될 때 파일이 읽혀지고(있는 경우) [`Editor`](xref:Xamarin.Forms.Editor)에 표시됩니다. **저장** [`Button`](xref:Xamarin.Forms.Button)을 누르면 `OnSaveButtonClicked` 이벤트 처리기가 실행되어 `Editor`의 콘텐츠가 파일에 저장됩니다. **삭제** `Button`을 누르면 `OnDeleteButtonClicked` 이벤트 처리기가 실행되어 파일을 삭제하고(있는 경우) `Editor`에서 모든 텍스트를 제거합니다. 사용자 상호 작용에 대 한 자세한 내용은 참조 하세요. [사용자 상호 작용에 응답할](deepdive.md#responding-to-user-interaction) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    **CTRL+S** 키를 눌러 변경 내용을 **MainPage.xaml.cs**에 저장하고 파일을 닫습니다.

### <a name="building-the-quickstart"></a>빠른 시작 빌드

1. Visual Studio에서 **빌드 > 솔루션 빌드** 메뉴 항목을 선택하거나 F6을 누릅니다. 솔루션이 빌드하고 성공 메시지가 Visual Studio 상태 표시줄에 표시됩니다.

      ![](single-page-images/vs/build-succeeded.png "빌드 성공")

    오류가 있는 경우 이전 단계를 반복하고 솔루션이 성공적으로 빌드할 때까지 실수를 수정합니다.

2. Visual Studio 도구 모음에서 선택한 Android 에뮬레이터의 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](single-page-images/vs/android-start.png "Visual Studio Android 도구 모음")

    [![](single-page-images/vs/notes-android.png "Android 에뮬레이터에서 정보")](single-page-images/vs/notes-android-large.png#lightbox "Notes in the Android Simulator")

    노트를 입력하고 **저장** 단추를 누릅니다.

    각 플랫폼에서 응용 프로그램은 시작 하는 방법에 대 한 자세한 내용은 참조 하십시오 [각 플랫폼에서 응용 프로그램을 시작](deepdive.md#launching-the-application-on-each-platform) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    > [!NOTE]
    > 다음 단계는 Xamarin.Forms 개발을 위한 시스템 요구 사항을 충족하는 [페어링된 Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)이 있는 경우에만 수행해야 합니다.

3. Visual Studio 도구 모음에서 **Notes.iOS** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **시작 프로젝트로 설정**을 선택합니다.

      ![](single-page-images/vs/set-as-startup-project-ios.png "iOS를 시작 프로젝트로 설정")

4. Visual Studio 도구 모음에서 선택한 [iOS 원격 에뮬레이터](~/tools/ios-simulator/index.md)의 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

    ![](single-page-images/vs/ios-start.png "Visual Studio iOS 도구 모음")

    [![](single-page-images/vs/notes-ios.png "IOS 시뮬레이터에서에서 메모")](single-page-images/vs/notes-ios-large.png#lightbox "iOS 시뮬레이터에에서 대 한 정보")

    노트를 입력하고 **저장** 단추를 누릅니다.

    각 플랫폼에서 응용 프로그램은 시작 하는 방법에 대 한 자세한 내용은 참조 하십시오 [각 플랫폼에서 응용 프로그램을 시작](deepdive.md#launching-the-application-on-each-platform) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

::: zone-end
::: zone pivot="macos"

### <a name="prerequisites"></a>전제 조건

- Visual Studio for Mac (최신 릴리스), iOS 및 Android 플랫폼 지원을 설치 합니다.
- Xcode (최신 릴리스)입니다.
- 기술 자료의 C#입니다.

이러한 필수 구성이 요소에 대 한 자세한 내용은 참조 하세요. [Xamarin 설치](~/cross-platform/get-started/installation/index.md)합니다.

## <a name="get-started-with-visual-studio-for-mac"></a>Mac용 Visual Studio 시작

1. Mac용 Visual Studio을 시작하고, 시작 페이지에서 **New Project...** 를 클릭하여 새 프로젝트를 만듭니다.

    ![](single-page-images/vsmac/new-project.png "새 솔루션")

2. **Choose a template for your new project** 대화 상자에서 **Multiplatform > App**을 클릭하여 **Blank Forms App** 템플릿을 선택하고 **Next** 단추를 클릭합니다.

    ![](single-page-images/vsmac/choose-template.png "템플릿 선택")

3. **빈 양식 앱 구성** 대화 상자에서 새 앱 이름을 **Notes**로 설정하고, **.NET Standard** 라디오 단추가 선택되었는지 확인하고, **다음** 단추를 클릭합니다.    

    ![](single-page-images/vsmac/configure-app.png "Forms 애플리케이션 구성")

4. **새 빈 Forms 앱 구성** 대화 상자에서 솔루션 및 프로젝트 이름을 **Notes**로 설정된 채로 두고, 프로젝트에 적절한 위치를 선택하고, **만들기** 단추를 클릭하여 프로젝트를 만듭니다.

    ![](single-page-images/vsmac/configure-project.png "Forms Project 구성")

    > [!IMPORTANT]
    > 이 빠른 시작의 C# 및 XAML 코드 조각은 솔루션과 프로젝의 이름이 모두 **Notes**이어야 합니다. 이 빠른 시작에서 코드를 프로젝트로 복사할 때 다른 이름을 사용하면 빌드 오류가 발생합니다.

    생성 되는.NET Standard 라이브러리에 대 한 자세한 내용은 참조 하세요. [Xamarin.Forms 응용 프로그램 분석](deepdive.md#anatomy-of-a-xamarinforms-application) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

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

    이 코드는 텍스트를 표시하는 [`Label`](xref:Xamarin.Forms.Label), 텍스트 입력을 위한 [`Editor`](xref:Xamarin.Forms.Editor), 파일을 저장 또는 삭제하도록 애플리케이션에 지시하는 두 개의 [`Button`](xref:Xamarin.Forms.Button) 인스턴스로 구성된 페이지의 사용자 인터페이스를 선언적으로 정의합니다. 두 개의 `Button` 인스턴스가 [`Grid`](xref:Xamarin.Forms.Grid)에 가로로 배치되고 `Label`, `Editor` 및 `Grid`가 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)에 세로로 배치됩니다. 사용자 인터페이스를 만드는 방법에 대 한 자세한 내용은 참조 하세요. [사용자 인터페이스](deepdive.md#user-interface) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

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

    이 코드는 애플리케이션의 로컬 애플리케이션 데이터 폴더에 노트 데이터를 저장하는 `notes.txt` 파일을 참조하는 `_fileName` 필드를 정의합니다. 페이지 생성자가 실행될 때 파일이 읽혀지고(있는 경우) [`Editor`](xref:Xamarin.Forms.Editor)에 표시됩니다. **저장** [`Button`](xref:Xamarin.Forms.Button)을 누르면 `OnSaveButtonClicked` 이벤트 처리기가 실행되어 `Editor`의 콘텐츠가 파일에 저장됩니다. **삭제** `Button`을 누르면 `OnDeleteButtonClicked` 이벤트 처리기가 실행되어 파일을 삭제하고(있는 경우) `Editor`에서 모든 텍스트를 제거합니다. 사용자 상호 작용에 대 한 자세한 내용은 참조 하세요. [사용자 상호 작용에 응답할](deepdive.md#responding-to-user-interaction) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **MainPage.xaml.cs**에 저장하고 파일을 닫습니다.

### <a name="building-the-quickstart"></a>빠른 시작 빌드

1. Mac용 Visual Studio에서 **빌드 > 솔루션 빌드** 메뉴 항목을 선택(하거나 **&#8984; + B** 키를 누릅니다). 프로젝트가 빌드되고 성공 메시지가 Mac용 Visual Studio 도구 모음에 표시됩니다.

      ![](single-page-images/vsmac/build-successful.png "빌드 성공")

    오류가 있는 경우 이전 단계를 반복하고 프로젝트가 성공적으로 빌드할 때까지 실수를 수정합니다.

2. 에 **Solution Pad**를 선택 합니다 **Notes.iOS** 프로젝트를 마우스 오른쪽 단추로 **시작 프로젝트로 설정**:

      ![](single-page-images/vsmac/set-startup-project-ios.png "iOS를 시작 프로젝트로 설정")

3. Mac용 Visual Studio 도구 모음에서 선택한 iOS Simulator 안에 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

      ![](single-page-images/vsmac/start.png "Visual Studio for Mac 도구 모음")

      [![](single-page-images/vsmac/notes-ios.png "IOS 시뮬레이터에서에서 메모")](single-page-images/vsmac/notes-ios-large.png#lightbox "iOS 시뮬레이터에에서 대 한 정보")

    노트를 입력하고 **저장** 단추를 누릅니다.

    각 플랫폼에서 응용 프로그램은 시작 하는 방법에 대 한 자세한 내용은 참조 하십시오 [각 플랫폼에서 응용 프로그램을 시작](deepdive.md#launching-the-application-on-each-platform) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

4. 에 **Solution Pad**를 선택 합니다 **Notes.Droid** 프로젝트를 마우스 오른쪽 단추로 **시작 프로젝트로 설정**:

      ![](single-page-images/vsmac/set-startup-project-android.png "Android를 시작 프로젝트로 설정")

5. Mac용 Visual Studio 도구 모음에서 선택한 Android 에뮬레이터 안에 애플리케이션을 시작하려면 **시작** 단추(재생 단추와 비슷한 삼각형 모양의 단추)를 누릅니다.

      [![](single-page-images/vsmac/notes-android.png "Android 에뮬레이터에서 정보")](single-page-images/vsmac/notes-android-large.png#lightbox "Notes in the Android Simulator")

    노트를 입력하고 **저장** 단추를 누릅니다.

    각 플랫폼에서 응용 프로그램은 시작 하는 방법에 대 한 자세한 내용은 참조 하십시오 [각 플랫폼에서 응용 프로그램을 시작](deepdive.md#launching-the-application-on-each-platform) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

::: zone-end

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서 학습 하는 방법.

- 플랫폼 간 Xamarin.Forms 응용 프로그램을 만듭니다.
- EXtensible Application Markup Language (XAML)을 사용 하는 페이지에 대 한 사용자 인터페이스를 정의 합니다.
- 코드에서 XAML 사용자 인터페이스 요소와 상호 작용 합니다.

다중 페이지 응용 프로그램에이 단일 페이지 응용 프로그램을 설정 하려면 다음 빠른 시작을 계속 합니다.

> [!div class="nextstepaction"]
> [다음](multi-page.md)

## <a name="related-links"></a>관련 링크

- [노트(샘플)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)
- [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)
