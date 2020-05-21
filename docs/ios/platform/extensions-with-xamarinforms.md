---
title: IOS 확장에서 Xamarin.ios 페이지 다시 사용
description: 이 문서에서는 iOS 확장에 대해 설명 하 고이 확장에서 Xamarin.ios 페이지를 사용 하는 방법을 설명 합니다. 확장을 만들고, Xamarin.ios를 통합 하 고, 간단한 ContentPage를 iOS 확장 프로젝트에 기본적으로 렌더링 하는 방법에 대 한 연습이 포함 되어 있습니다.
ms.prod: xamarin
ms.assetid: 50076FFD-3EF6-41CC-BC7E-210DE3958F5B
ms.technology: xamarin-ios
author: alexeystrakh
ms.author: alstrakh
ms.date: 05/13/2020
ms.openlocfilehash: 4006bb3ef82264b5a7a6d764719bee81a8ce78a1
ms.sourcegitcommit: 5bc37b384706bfbf5bf8e5b1288a34ddd0f3303b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/15/2020
ms.locfileid: "83418075"
---
# <a name="reuse-xamarinforms-pages-in-an-ios-extension"></a>IOS 확장에서 Xamarin.ios 페이지 다시 사용

iOS 확장을 사용 하면 [ios 및 macOS 확장 지점이](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW2)나 사용자 지정 컨텍스트 작업, 암호 자동 채우기, 들어오는 통화 필터, 알림 콘텐츠 한정자 등의 추가 기능을 추가 하 여 기존 시스템 동작을 사용자 지정할 수 있습니다. Xamarin.ios는 확장을 지원 하 고 [이 가이드](https://docs.microsoft.com/xamarin/ios/platform/extensions) 에서는 xamarin 도구를 사용 하 여 iOS 확장을 만드는 과정을 안내 합니다.

확장은 컨테이너 앱의 일부로 배포 되 고 호스트 앱의 특정 확장 지점에서 활성화 됩니다. 컨테이너 앱은 일반적으로 사용자에 게 확장에 대 한 정보를 제공 하 고, 활성화 하 고 사용 하는 방법을 제공 하는 간단한 iOS 응용 프로그램입니다. 확장 및 컨테이너 앱 간에 코드를 공유 하는 세 가지 주요 방법이 있습니다.

1. 공용 iOS 프로젝트.

    컨테이너와 확장 사이의 모든 공유 코드를 공유 iOS 라이브러리에 배치 하 고 두 프로젝트에서 라이브러리를 참조할 수 있습니다. 일반적으로 공유 라이브러리는 네이티브 UIViewControllers를 포함 하며 Xamarin.ios 라이브러리 여야 합니다.

1. 파일 링크.

    경우에 따라 컨테이너 앱은 확장에서 단일를 렌더링 해야 하는 동안 대부분의 기능을 제공 합니다 `UIViewController` . 공유할 파일이 적은 경우 컨테이너 앱에 있는 파일에서 확장명 앱에 파일 링크를 추가 하는 것이 일반적입니다.

1. 일반적인 Xamarin.ios 프로젝트입니다.

    앱 페이지가 Android 등의 다른 플랫폼과 이미 공유 된 경우에는 Xamarin.ios 프레임 워크를 사용 하 여 iOS 확장이 네이티브 UIViewControllers에서 작동 하 고 Xamarin.ios 페이지가 아니라 확장 프로젝트에서 기본적으로 필요한 페이지를 다시 구현 하는 것이 일반적인 방법입니다. IOS 확장에서 Xamarin.ios를 사용 하려면 추가 단계를 수행 해야 합니다 .이에 대해서는 아래에서 설명 합니다.

## <a name="xamarinforms-in-an-ios-extension-project"></a>IOS 확장 프로젝트의 xamarin.ios

네이티브 프로젝트에서 Xamarin.ios를 사용 하는 기능은 [네이티브 폼](~/xamarin-forms/platform/native-forms.md)을 통해 제공 됩니다. `ContentPage`파생 된 페이지를 네이티브 xamarin.ios 프로젝트에 직접 추가할 수 있습니다. `CreateViewController`확장 메서드는 xamarin.ios 페이지의 인스턴스를 `UIViewController` 일반 컨트롤러로 사용 하거나 수정할 수 있는 네이티브로 변환 합니다. IOS 확장은 특수 한 종류의 네이티브 iOS 프로젝트 이므로 여기에서 **네이티브 폼** 을 사용할 수도 있습니다.

> [!IMPORTANT]
> IOS 확장에 대 한 [알려진 제한 사항은](https://docs.microsoft.com/xamarin/ios/platform/extensions#limitations) 많이 있습니다. IOS 확장에서 Xamarin.ios를 사용할 수 있지만 메모리 사용 및 시작 시간을 매우 신중 하 게 모니터링 해야 합니다. 그렇지 않으면이를 정상적으로 처리할 수 없는 방식으로 확장을 iOS에서 종료할 수 있습니다.

## <a name="walkthrough"></a>연습

이 연습에서는 Xamarin.ios 응용 프로그램, Xamarin.ios 확장을 만들고 확장 프로젝트에서 공유 코드를 다시 사용 합니다.

1. Mac용 Visual Studio를 열고 **빈 Forms 앱** 템플릿을 사용 하 여 새 xamarin.ios 프로젝트를 만든 다음 이름을 Forms **shareextension**으로 만듭니다.

    ![프로젝트 만들기](extensions-xf-images/1.walkthrough-createproject.png)

1. **양식 Shareextension/mainpage**에서 콘텐츠를 다음 레이아웃으로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <ContentPage
        x:Class="FormsShareExtension.MainPage"
        xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:d="http://xamarin.com/schemas/2014/forms/design"
        xmlns:local="clr-namespace:FormsShareExtension"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        x:DataType="local:MainPageViewModel"
        BackgroundColor="Orange"
        mc:Ignorable="d">
        <ContentPage.BindingContext>
            <local:MainPageViewModel Message="Hello from Xamarin.Forms!" />
        </ContentPage.BindingContext>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <Label
                Margin="20"
                Text="{Binding Message}"
                VerticalOptions="CenterAndExpand" />
            <Button Command="{Binding DoCommand}" Text="Do the job!" />
        </StackLayout>
    </ContentPage>
    ```

1. **Mainpageviewmode** 라는 새 클래스를 **양식 shareextension** 프로젝트에 추가 하 고 클래스의 내용을 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.ComponentModel;
    using System.Windows.Input;
    using Xamarin.Forms;

    namespace FormsShareExtension
    {
        public class MainPageViewModel : INotifyPropertyChanged
        {
            public event PropertyChangedEventHandler PropertyChanged;

            private string _message;
            public string Message
            {
                get { return _message; }
                set
                {
                    if (_message != value)
                    {
                        _message = value;
                        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Message)));
                    }
                }
            }

            private ICommand _doCommand;
            public ICommand DoCommand
            {
                get { return _doCommand; }
                set
                {
                    if(_doCommand != value)
                    {
                        _doCommand = value;
                        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(DoCommand)));
                    }
                }
            }

            public MainPageViewModel()
            {
                DoCommand = new Command(OnDoCommandExecuted);
            }

            private void OnDoCommandExecuted(object state)
            {
                Message = $"Job {Environment.TickCount} has been completed!";
            }
        }
    }
    ```

    코드는 모든 플랫폼에서 공유 되며 iOS 확장에도 사용 됩니다.

1. Solution pad에서 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 프로젝트 추가를 선택 하 > iOS > 확장 > 작업 확장**을 선택 하 고 이름을 **Myaction** 으로 입력 하 고 **만들기**를 누릅니다.

    ![확장 만들기](extensions-xf-images/2.walkthrough-createextension.png)

1. IOS 확장 및 공유 코드에서 Xamarin.ios를 사용 하려면 필요한 참조를 추가 해야 합니다.

    - IOS 확장을 마우스 오른쪽 단추로 클릭 하 > 참조를 선택 하 고, **양식 shareextension > 참조 > 프로젝트를 추가** 하 고 **확인**을 누릅니다.

    - IOS 확장을 마우스 오른쪽 단추로 클릭 하 고 **패키지 > NuGet 패키지 관리 ...** 를 선택 하 > xamarin.ios를 선택 하 고 **패키지 추가**를 누릅니다.

1. 확장 프로젝트를 확장 하 고 진입점을 수정 하 여 Xamarin.ios를 초기화 하 고 페이지를 만듭니다. IOS 요구 사항에 따라 확장은 **info.plist** 에서 또는로 진입점을 정의 해야 합니다 `NSExtensionMainStoryboard` . `NSExtensionPrincipalClass` 진입점이 활성화 되 면이 경우에는 `ActionViewController.ViewDidLoad` xamarin.ios 페이지의 인스턴스를 만들고 사용자에 게 표시할 수 있습니다. 따라서 진입점을 열고 `ViewDidLoad` 메서드를 다음 구현으로 바꿉니다.

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        // Initialize Xamarin.Forms framework
        global::Xamarin.Forms.Forms.Init();
        // Create an instance of XF page with associated View Model
        var xfPage = new MainPage();
        var viewModel = (MainPageViewModel)xfPage.BindingContext;
        viewModel.Message = "Welcome to XF Page created from an iOS Extension";
        // Override the behavior to complete the execution of the Extension when a user press the button
        viewModel.DoCommand = new Command(() => DoneClicked(this));
        // Convert XF page to a native UIViewController which can be consumed by the iOS Extension
        var newController = xfPage.CreateViewController();
        // Present new view controller as a regular view controller
        this.PresentModalViewController(newController, false);
    }
    ```

    는 `MainPage` 표준 생성자를 사용 하 여 인스턴스화되고 확장에서 사용 하기 전에 확장 메서드를 사용 하 여 네이티브로 변환 합니다 `UIViewController` `CreateViewController` . 
    
    응용 프로그램을 빌드하고 실행 합니다.

    ![확장 만들기](extensions-xf-images/3.walkthrough-runapp.png)

    확장을 활성화 하려면 Safari 브라우저로 이동 하 고, 웹 주소 (예: [microsoft.com](https://microsoft.com))를 입력 하 고, 탐색을 클릭 한 후 페이지 맨 아래에 있는 **공유** 아이콘을 눌러 사용 가능한 작업 확장을 확인 합니다. 사용 가능한 확장 목록에서 **Myaction** 확장을 선택 합니다.

    ![확장 만들기](extensions-xf-images/4.walkthrough-run1.png) ![확장 만들기](extensions-xf-images/5.walkthrough-run2.png) ![확장 만들기](extensions-xf-images/6.walkthrough-run3.png)

    확장이 활성화 되 고 Xamarin Forms 페이지가 사용자에 게 표시 됩니다. 모든 바인딩과 명령은 컨테이너 앱에서와 동일 하 게 작동 합니다.

1. 원본 진입점 뷰 컨트롤러는 iOS에서 만들어지고 활성화 되기 때문에 표시 됩니다. 이 문제를 해결 하려면 `UIModalPresentationStyle.FullScreen` 호출 바로 앞에 다음 코드를 추가 하 여 새 컨트롤러에 대 한 모달 프레젠테이션 스타일을로 변경 합니다 `PresentModalViewController` .

    ```csharp
    newController.ModalPresentationStyle = UIModalPresentationStyle.FullScreen;
    ```

    IOS 시뮬레이터 또는 장치에서 빌드 및 실행:

    ![IOS 확장의 Xamarin 양식](extensions-xf-images/7.walkthrough-result.png)

    > [!IMPORTANT]
    > 장치 빌드의 경우 [여기에 설명](~/iOS/platform/extensions.md#debug-and-release-versions-of-extensions)된 대로 적절 한 빌드 설정과 **릴리스** 구성을 사용 해야 합니다.

## <a name="related-links"></a>관련 링크

- [Xamarin.iOS의 iOS 확장](~/iOS/platform/extensions.md)
- [Xamarin 네이티브 프로젝트의 xamarin.ios](~/xamarin-forms/platform/native-forms.md)
- [IOS 앱 확장의 효율성 및 성능 최적화](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/ExtensionCreation.html#//apple_ref/doc/uid/TP40014214-CH5-SW7)
- [샘플 소스 코드](https://github.com/xamcat/xamarin-forms-ios-extension)
