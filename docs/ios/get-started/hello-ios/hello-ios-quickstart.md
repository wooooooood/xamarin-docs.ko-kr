---
title: Hello, iOS - 빠른 시작
description: 이 연습에서는 Hello, iOS라는 간단한 Xamarin.iOS 애플리케이션을 빌드하는 방법을 보여줍니다. 이 과정에서 Xamarin.iOS 애플리케이션을 빌드하기 위해 이해해야 하는 기본 도구 및 개념을 소개합니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: D3868F3A-4EED-BDDF-45AA-665102C39634
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: f7319a2c3dd4c3f77873d9b3d2cba74a77f14ae0
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865590"
---
# <a name="hello-ios--quickstart"></a>Hello, iOS - 빠른 시작

이 가이드에서는 사용자가 입력한 영숫자 전화 번호를 숫자 전화 번호로 변환한 다음, 해당 번호로 전화하는 애플리케이션을 만드는 방법을 설명합니다. 최종 애플리케이션은 다음과 같습니다.

 [![](hello-ios-quickstart-images/image1.png "Hello.iOS 빠른 시작 앱")](hello-ios-quickstart-images/image1.png#lightbox)

## <a name="requirements"></a>요구 사항

Xamarin을 사용한 iOS 개발에는 다음이 필요합니다.

- macOS High Sierra(10.13) 이상을 실행하는 Mac
- [앱 스토어](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)에서 설치된 최신 버전의 Xcode 및 iOS SDK

::: zone pivot="macos"

Xamarin.iOS는 다음 설치를 사용하여 작동합니다.

- 위의 사양에 맞는 최신 버전의 Mac용 Visual Studio.

[Xamarin.iOS Mac 설치 가이드](~/ios/get-started/installation/mac.md)에서 단계별 설치 지침을 사용할 수 있습니다.

::: zone-end
::: zone pivot="windows"

Xamarin.iOS는 다음 설치를 사용하여 작동합니다.

- Windows 10의 Visual Studio 2019 또는 Visual Studio 2017 Community, Professional 또는 Enterprise의 최신 버전에서는 위의 사양에 맞는 Mac 빌드 호스트와 페어링됩니다.

[Xamarin.iOS Windows 설치 가이드](~/ios/get-started/installation/windows/index.md)에서 단계별 설치 지침을 사용할 수 있습니다.

::: zone-end

시작하기 전에 [Xamarin 앱 아이콘 집합](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)을 다운로드합니다.

::: zone pivot="macos"

## <a name="visual-studio-for-mac-walkthrough"></a>Mac용 Visual Studio 연습

이 연습에서는 영숫자 전화 번호를 숫자 전화 번호로 변환하는 Phoneword라는 애플리케이션을 만드는 방법을 설명합니다.

1. **애플리케이션** 폴더 또는 **스포트라이트**에서 Mac용 Visual Studio를 시작합니다.

    ![](hello-ios-quickstart-images/image2new.png "시작 화면")

    시작 화면에서 **새 프로젝트...** 을 클릭하여 새 Xamarin.iOS 솔루션을 만듭니다.

    ![](hello-ios-quickstart-images/image3new.png "iOS 솔루션")

2. **새 솔루션 대화 상자**에서 **iOS &gt; 애플리케이션 &gt; 단일 뷰 애플리케이션** 템플릿을 선택하여 C#이 선택되어 있는지 확인합니다. **다음**을 클릭합니다.

    ![](hello-ios-quickstart-images/image4new.png "단일 뷰 애플리케이션 선택")

3. 앱을 구성합니다. **이름**으로 `Phoneword_iOS`를 지정하고, 나머지는 기본값을 유지합니다. **다음**을 클릭합니다.

    ![](hello-ios-quickstart-images/image5new.png "앱 이름 입력")

4. 프로젝트 및 솔루션 이름은 그대로 둡니다. 여기서 프로젝트의 위치를 선택하거나 기본값으로 유지합니다.

    ![](hello-ios-quickstart-images/image6new.png "프로젝트 위치 선택")

5. **만들기**를 클릭하여 **솔루션**을 만듭니다.

6. **솔루션 패드**에서 **Main.storyboard** 파일을 두 번 클릭하여 엽니다. 이렇게 하면 시각적으로 UI를 만들 수 있습니다.

    ![](hello-ios-quickstart-images/image7new.png "iOS 디자이너")

    _크기 클래스_는 기본적으로 사용하도록 설정되어 있습니다. 자세한 내용은 [통합 스토리보드](~/ios/user-interface/storyboards/unified-storyboards.md) 가이드를 참조하세요.

7. **도구 상자 패드**의 검색 표시줄에서 "레이블"을 입력한 다음, **레이블**을 디자인 화면(가운데 영역)으로 끌어 놓습니다.

    ![](hello-ios-quickstart-images/image8new.png "레이블을 가운데에 있는 디자인 화면으로 끌기")

    > [!NOTE]
    > 언제든지 **보기 > 패드**로 이동하여 **Properties Pad** 또는 **도구 상자**를 가져올 수 있습니다.

8. *컨트롤 끌기*(컨트롤 주위에 있는 원)의 핸들을 잡고 레이블을 넓게 만듭니다.

    ![](hello-ios-quickstart-images/image9.png "레이블을 넓게 만들기")

9. 디자인 화면에서 **레이블**을 선택한 채로 **속성 패드**를 사용하여 **레이블**의 **텍스트** 속성을 "Phoneword 입력"으로 변경합니다.

    ![](hello-ios-quickstart-images/image10.png "Phoneword 입력 레이블 설정")

10. 도구 상자 내에서 "텍스트 필드"를 검색하고, **텍스트 필드**를 **도구 상자**에서 디자인 화면으로 끌어서 **레이블** 아래에 놓습니다. **텍스트 필드**가 **레이블**과 같은 너비가 될 때까지 조정합니다.

    ![](hello-ios-quickstart-images/image12new.png "텍스트 필드를 레이블과 같은 너비로 만들기")

11. 디자인 화면에서 **텍스트 필드**를 선택한 채로 **Properties Pad**의 ID 섹션에서 **텍스트 필드**의 **이름** 속성을 `PhoneNumberText`로 변경하고, **텍스트** 속성을 "1-855-XAMARIN"으로 변경합니다.

    ![](hello-ios-quickstart-images/image13new.png "제목 속성을 1-855-XAMARIN으로 변경")

12. **단추**를 **도구 상자**에서 디자인 화면으로 끌어서 **텍스트 필드** 아래에 놓습니다. **단추**가 **텍스트 필드** 및 **레이블**과 같은 너비가 되도록 조정합니다.

    ![](hello-ios-quickstart-images/image14new.png "단추가 텍스트 필드 및 레이블과 같은 너비가 되도록 조정")

13. 디자인 화면에서 **단추**를 선택한 채로 **속성 패드**의 **ID** 섹션에서 **이름** 속성을 `TranslateButton`으로 변경합니다. **제목** 속성을 "변환"으로 변경합니다.

    ![](hello-ios-quickstart-images/image15new.png "제목 속성을 변환으로 변경")

14. 위의 두 단계를 반복하고, **단추**를 **도구 상자**에서 디자인 화면으로 끌어서 첫 번째 **단추** 아래에 놓습니다. **단추**가 첫 번째 **단추**와 같은 너비가 되도록 조정합니다.

    ![](hello-ios-quickstart-images/image16new.png "단추가 첫 번째 단추와 같은 너비가 되도록 조정")

15. 디자인 화면에서 두 번째 **단추**를 선택한 채로 **속성 패드**의 **ID** 섹션에서 **이름** 속성을 `CallButton`으로 변경합니다. **제목** 속성을 "호출"로 변경합니다.

    ![](hello-ios-quickstart-images/image17new.png "제목 속성을 호출로 변경")

    **파일 > 저장**으로 이동하거나 **⌘+s**를 눌러 변경 내용을 저장합니다.

16. 전화 번호를 영숫자에서 숫자로 변환하려면 일부 논리를 앱에 추가해야 합니다. **솔루션 패드**에서 **Phoneword_iOS** 프로젝트를 마우스 오른쪽 단추로 클릭하고, **추가 > 새 파일...** 을 선택하거나 **⌘+n**을 눌러 프로젝트에 새 파일을 추가합니다.

    ![](hello-ios-quickstart-images/image18.png "프로젝트에 새 파일 추가")

17. **새 파일** 대화 상자에서 **일반 > 빈 클래스**를 선택하고 새 파일의 이름을 `PhoneTranslator`로 지정합니다.

    ![](hello-ios-quickstart-images/image19.png "빈 클래스를 선택하고 새 파일의 이름을 PhoneTranslator로 지정")

18. 이렇게 하면 비어 있는 새 C# 클래스가 만들어집니다. 모든 템플릿 코드를 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword_iOS
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

    **PhoneTranslator.cs** 파일을 저장하고 닫습니다.

19. 사용자 인터페이스를 연결하는 코드를 추가합니다. 이렇게 하려면 **솔루션 패드**에서 **ViewController.cs**를 두 번 클릭하여 엽니다.

    ![](hello-ios-quickstart-images/image20new.png "사용자 인터페이스를 연결하는 코드 추가")

20. `TranslateButton`을 연결하여 시작합니다. **ViewController** 클래스에서 `ViewDidLoad` 메서드를 찾고 `base.ViewDidLoad()` 호출 아래에 다음 코드를 추가합니다.

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(
            PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call ", UIControlState.Normal);
            CallButton.Enabled = false;
        } else {
            CallButton.SetTitle ("Call " + translatedNumber,
                UIControlState.Normal);
            CallButton.Enabled = true;
        }
    };
    ```

    파일의 네임스페이스가 다른 경우 `using Phoneword_iOS;`를 포함합니다.

21. `CallButton`이라는 두 번째 단추를 눌러 사용자에게 응답하는 코드를 추가합니다. `TranslateButton`에 대한 코드 아래에 다음 코드를 배치하고, `using Foundation;`을 파일의 맨 위에 추가합니다.

    ```csharp
        CallButton.TouchUpInside += (object sender, EventArgs e) => {
            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl ("tel:" + translatedNumber);

            // ...otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl (url)) {
                var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                PresentViewController (alert, true, null);
            }
        };
    ```

22. 변경 내용을 저장한 다음, **빌드 &gt; 모두 빌드**를 선택하거나 **⌘+B**를 눌러 애플리케이션을 빌드합니다.  애플리케이션이 컴파일되면 IDE의 위쪽에 성공 메시지가 표시됩니다.

    ![](hello-ios-quickstart-images/image21.png "IDE 위쪽에 표시된 성공 메시지")

    오류가 있는 경우 이전 단계를 진행하고, 애플리케이션이 성공적으로 빌드될 때까지 모든 오류를 수정합니다.

23. 마지막으로, **iOS 시뮬레이터**에서 애플리케이션을 테스트합니다. IDE의 왼쪽 위에 있는 첫 번째 드롭다운에서 **디버그**를 선택하고, 두 번째 드롭다운에서 **iPhone XR iOS 12.0**(또는 사용 가능한 다른 시뮬레이터)을 선택하고, **시작**(재생 단추와 비슷한 삼각형 단추)을 누릅니다.

    ![](hello-ios-quickstart-images/image27.png "시뮬레이터를 선택하고 시작을 누릅니다.")

    > [!NOTE]
    > 현재 Apple의 요구 사항에 따라 디바이스 또는 시뮬레이터용 코드를 빌드하는 데 개발 인증서 또는 ‘서명 ID’가 필요할 수 있습니다.  이를 설정하려면 [디바이스 프로비전 가이드](~/ios/get-started/installation/device-provisioning/manual-provisioning.md)의 단계를 수행합니다.

24. 그러면 iOS 시뮬레이터 내에서 애플리케이션이 시작됩니다.

    ![](hello-ios-quickstart-images/image28.png "iOS 시뮬레이터 내에서 실행되는 애플리케이션")

    iOS 시뮬레이터에서는 전화 통화가 지원되지 않습니다. 대신 통화를 시도할 때 경고 대화 상자가 표시됩니다.

    ![](hello-ios-quickstart-images/image29.png "통화를 시도할 때의 경고 대화 상자")

::: zone-end
::: zone pivot="windows"

## <a name="visual-studio-walkthrough"></a>Visual Studio 연습

이 연습에서는 영숫자 전화 번호를 숫자 전화 번호로 변환하는 Phoneword라는 애플리케이션을 만드는 방법을 설명합니다.

> [!NOTE]
> 이 연습에서는 Windows 10 Virtual Machine에서 Visual Studio Enterprise 2017을 사용합니다. 위의 요구 사항을 충족하는 한 설정이 이와 다를 수 있지만, 일부 스크린샷은 설정에 따라 다를 수 있습니다.

> [!NOTE]
> 이 연습을 진행하기 전에 이미 Visual Studio에서 Mac에 연결되어 있어야 합니다. 이는 Xamarin.iOS에서 Apple의 도구를 사용하여 iOS 디자이너 및 애플리케이션을 빌드하고 실행하기 때문입니다. 설정하려면 [Mac에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 가이드의 단계를 수행합니다.

1. **시작** 메뉴에서 Visual Studio를 시작합니다.

    ![](hello-ios-quickstart-images/image001-.png "시작 화면")

    **파일 > 새로 만들기 > 프로젝트... > Visual C# > iPhone 및 iPad > iOS 앱(Xamarin)** 을 선택하여 새로운 Xamarin.iOS 솔루션을 만듭니다.

    ![iOS 앱(Xamarin) 프로젝트 형식 선택](hello-ios-quickstart-images/image002.w157.png "iOS 앱(Xamarin) 프로젝트 형식 선택")

    다음 대화 상자가 나타나면 **단일 보기 앱** 템플릿을 선택하고 **확인**을 눌러 프로젝트를 만듭니다.

    ![단일 보기 프로젝트 템플릿 선택](hello-ios-quickstart-images/image002-2.w157.png "단일 보기 프로젝트 템플릿 선택")

1. 도구 모음에서 Xamarin Mac 에이전트 아이콘이 녹색인지 확인합니다.

    ![도구 모음에서 Xamarin Mac 에이전트 아이콘이 녹색인지 확인](hello-ios-quickstart-images/vs-image4.png)

    그렇지 않은 경우 Mac 빌드 호스트에 연결되지 않았다는 의미이므로 [구성 가이드](~/ios/get-started/installation/windows/connecting-to-mac/index.md)의 단계에 따라 연결합니다.

1. **솔루션 탐색기**에서 **Main.storyboard** 파일을 두 번 클릭하여 iOS 디자이너에서 이 파일을 엽니다.

    ![](hello-ios-quickstart-images/vs-image7.png "iOS 디자이너")

1. **도구 상자** 탭을 열고, 검색 표시줄에서 "레이블"을 입력한 다음, **레이블**을 디자인 화면(가운데 영역)으로 끌어갑니다.

    ![](hello-ios-quickstart-images/vs-image8.png "레이블을 가운데에 있는 디자인 화면으로 끌기")

1. 다음으로, *컨트롤 끌기*의 핸들을 잡고 레이블을 넓게 만듭니다.

    ![](hello-ios-quickstart-images/vs-image9.png "레이블을 넓게 만들기")

1. 디자인 화면에서 **레이블**을 선택한 채로 **속성 창**을 사용하여 **레이블**의 **텍스트** 속성을 "Phoneword 입력"으로 변경합니다.

    ![](hello-ios-quickstart-images/vs-image10.png "레이블의 텍스트 속성을 'Phoneword 입력'으로 변경")

    > [!NOTE]
    > 언제든지 **보기** 메뉴로 이동하여 **속성** 또는 **도구 상자**를 가져올 수 있습니다.

1. 도구 상자 내에서 "텍스트 필드"를 검색하고, **텍스트 필드**를 **도구 상자**에서 디자인 화면으로 끌어서 **레이블** 아래에 놓습니다. **텍스트 필드**가 **레이블**과 같은 너비가 될 때까지 조정합니다.

    ![](hello-ios-quickstart-images/vs-image12.png "텍스트 필드가 레이블과 같은 너비가 될 때까지 조정")

1. 디자인 화면에서 **텍스트 필드**를 선택한 채로 **속성**의 ID 섹션에서 **텍스트 필드**의 **이름** 속성을 `PhoneNumberText`로 변경하고, **텍스트** 속성을 "1-855-XAMARIN"으로 변경합니다.

    ![](hello-ios-quickstart-images/vs-image13.png "텍스트 속성을 1-855-XAMARIN으로 변경")

1. **단추**를 **도구 상자**에서 디자인 화면으로 끌어서 **텍스트 필드** 아래에 놓습니다. **단추**가 **텍스트 필드** 및 **레이블**과 같은 너비가 되도록 조정합니다.

    ![](hello-ios-quickstart-images/vs-image14.png "단추가 텍스트 필드 및 레이블과 같은 너비가 되도록 조정")


1. 디자인 화면에서 **단추**를 선택한 채로 **속성**의 **ID** 섹션에서 **이름** 속성을 `TranslateButton`으로 변경합니다. **제목** 속성을 "변환"으로 변경합니다.

    ![](hello-ios-quickstart-images/vs-image15.png "제목 속성을 변환으로 변경")

1. 앞의 두 단계를 반복하고, **단추**를 **도구 상자**에서 디자인 화면으로 끌어서 첫 번째 **단추** 아래에 놓습니다. **단추**가 첫 번째 **단추**와 같은 너비가 되도록 조정합니다.

    ![](hello-ios-quickstart-images/vs-image16.png "단추가 첫 번째 단추와 같은 너비가 되도록 조정")

1. 디자인 화면에서 두 번째 **단추**를 선택한 채로 **속성**의 **ID** 섹션에서 **이름** 속성을 `CallButton`으로 변경합니다. **제목** 속성을 "호출"로 변경합니다.

    ![](hello-ios-quickstart-images/vs-image17.png "제목 속성을 호출로 변경")

    **파일 > 모두 저장**으로 이동하거나 **Ctrl+s**를 눌러 변경 내용을 저장합니다.

1. 전화 번호를 영숫자에서 숫자로 변환하는 코드를 추가합니다. 이렇게 하려면 먼저 **솔루션 탐색기**에서 **Phoneword** 프로젝트를 마우스 오른쪽 단추로 클릭하고, **추가 > 새 항목...** 을 차례로 선택하거나 **Ctrl+Shift+A**를 눌러 프로젝트에 새 파일을 추가합니다.

    ![](hello-ios-quickstart-images/vs-image18.png "전화 번호를 영숫자에서 숫자로 변환하는 코드 추가")

1. **새 항목 추가** 대화 상자(프로젝트를 마우스 오른쪽 단추로 클릭하고, 추가 > 새 항목... 선택)에서 **Apple > 클래스**를 선택하고 새 파일 이름을 `PhoneTranslator`로 지정합니다.

    ![](hello-ios-quickstart-images/vs-image19.w157.png "PhoneTranslator라는 새 클래스 추가")

    > [!IMPORTANT]
    > 아이콘에 C#이 있는 '클래스' 템플릿을 선택했는지 확인합니다. 그렇지 않으면 이 새 클래스를 참조하지 못할 수 있습니다.

1. 그러면 새 C# 클래스가 만들어집니다. 모든 템플릿 코드를 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

    **PhoneTranslator.cs** 파일을 저장하고 닫습니다.

1. **솔루션 탐색기**에서 **ViewController.cs**를 두 번 클릭하여 단추와의 상호 작용을 처리하는 논리를 추가할 수 있습니다.

    ![](hello-ios-quickstart-images/vs-image20.png "단추와의 상호 작용을 처리하는 논리 추가")


1. `TranslateButton`을 연결하여 시작합니다. **ViewController** 클래스에서 `ViewDidLoad` 메서드를 찾습니다. `ViewDidLoad` 내의 `base.ViewDidLoad()` 호출 아래에 다음 단추 코드를 추가합니다.

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {

        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call", UIControlState.Normal);
            CallButton.Enabled = false;
            }
        else {
            CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
            CallButton.Enabled = true;
            }
    };
    ```
    파일의 네임스페이스가 다른 경우 `using Phoneword;`를 포함합니다.

1. `CallButton`이라는 두 번째 단추를 눌러 사용자에게 응답하는 코드를 추가합니다. `TranslateButton`에 대한 코드 아래에 다음 코드를 배치하고, `using Foundation;`을 파일의 맨 위에 추가합니다.

    ```csharp
    CallButton.TouchUpInside += (object sender, EventArgs e) => {
        var url = new NSUrl ("tel:" + translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app,
            // otherwise show an alert dialog

        if (!UIApplication.SharedApplication.OpenUrl (url)) {
                        var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                        alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        PresentViewController (alert, true, null);
                    }
    };
    ```

1. 변경 내용을 저장한 다음, **빌드 &gt; 솔루션 빌드**를 선택하거나 **Ctrl+Shift+B**를 눌러 애플리케이션을 빌드합니다.  애플리케이션이 컴파일되면 IDE의 아래쪽에 성공 메시지가 표시됩니다.

    ![](hello-ios-quickstart-images/vs-image21.png "IDE 아래쪽에 표시된 성공 메시지")

    오류가 있는 경우 이전 단계를 진행하고, 애플리케이션이 성공적으로 빌드될 때까지 모든 오류를 수정합니다.

1. 마지막으로, **원격 iOS 시뮬레이터**에서 애플리케이션을 테스트합니다. IDE 도구 모음의 드롭다운 메뉴에서 **디버그** 및 **iPhone 8 Plus iOS x.x**를 선택하고, **시작**(재생 단추와 비슷한 녹색 삼각형)을 누릅니다.

    ![](hello-ios-quickstart-images/vs-image27.png "시작 누르기")

1. 그러면 iOS 시뮬레이터 내에서 애플리케이션이 시작됩니다.

    ![](hello-ios-quickstart-images/vs-image28.png "iOS 시뮬레이터 내에서 실행되는 애플리케이션")

    iOS 시뮬레이터에서는 전화 통화가 지원되지 않습니다. 대신 통화를 시도할 때 경고 대화 상자가 표시됩니다.

    ![](hello-ios-quickstart-images/vs-image29.png "통화를 시도할 때 표시되는 경고 대화 상자")

::: zone-end

첫 번째 Xamarin.iOS 애플리케이션을 완성한 것을 축하합니다!

이제 이 가이드의 도구와 기술을 [Hello, iOS 심층 분석](~/ios/get-started/hello-ios/hello-ios-deepdive.md)에서 분석할 시간입니다.

## <a name="related-links"></a>관련 링크

- [Xamarin 앱 아이콘 및 시작 이미지(샘플)](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)
- [Hello, iOS(샘플)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS 휴먼 인터페이스 지침](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS 프로비전 포털](https://developer.apple.com/ios/manage/overview/index.action)
