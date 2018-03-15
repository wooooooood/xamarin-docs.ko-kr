---
title: Hello, iOS
description: "두 부분으로 구성된 이 가이드에서는 Mac용 Visual Studio 또는 Visual Studio를 사용하여 기본 Xamarin.iOS 응용 프로그램을 빌드하고, Xamarin을 사용하여 iOS 응용 프로그램 개발에 대한 기본 사항을 이해하는 방법을 설명합니다. Xamarin.iOS 응용 프로그램을 빌드 및 배포하는 데 필요한 도구, 개념 및 단계를 소개합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: D3868F3A-4EED-BDDF-45AA-665102C39634
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/23/2017
ms.openlocfilehash: 7b1d56c62fe54d5b1e196e20e1a6989b542da1be
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="helloios-quickstart"></a>Hello.iOS 빠른 시작

이 가이드에서는 사용자가 입력한 영숫자 전화 번호를 숫자 전화 번호로 변환한 다음, 해당 번호로 전화하는 응용 프로그램을 만드는 방법을 설명합니다. 최종 응용 프로그램은 다음과 같습니다.

 [![](hello-ios-quickstart-images/image1.png "Hello.iOS 빠른 시작 앱")](hello-ios-quickstart-images/image1.png#lightbox)


<a name="Requirements" />

## <a name="requirements"></a>요구 사항

Xamarin을 사용한 iOS 개발에는 다음이 필요합니다.

-  macOS Sierra 10.12 이상을 실행하는 Mac
-  [앱 스토어](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)에서 설치된 최신 버전의 Xcode 및 iOS SDK

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Xamarin.iOS는 다음 설치 중 하나를 사용하여 작동합니다.

-  위의 사양에 맞는 최신 버전의 Mac용 Visual Studio.

[Xamarin.iOS Mac 설치 가이드](~/ios/get-started/installation/mac.md)에서 단계별 설치 지침을 사용할 수 있습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.iOS는 다음 설치 중 하나를 사용하여 작동합니다.

-  Windows 7 이상에서 위의 사양에 맞는 Mac 빌드 호스트와 연결되는 최신 버전의 Visual Studio 2015 또는 2017 Professional 이상

[Xamarin.iOS Windows 설치 가이드](~/ios/get-started/installation/windows/index.md)에서 단계별 설치 지침을 사용할 수 있습니다.

-----

시작하기 전에 [Xamarin 앱 아이콘 집합](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)을 다운로드합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="visual-studio-for-mac-walkthrough"></a>Mac용 Visual Studio 연습

이 연습에서는 영숫자 전화 번호를 숫자 전화 번호로 변환하는 Phoneword라는 응용 프로그램을 만드는 방법을 설명합니다.

1. **응용 프로그램** 폴더 또는 **스포트라이트**에서 Mac용 Visual Studio를 시작하여 [시작] 화면을 가져옵니다.

  ![](hello-ios-quickstart-images/image2new.png "시작 화면")

시작 화면에서 **새 프로젝트...**을 클릭하여 새 Xamarin.iOS 솔루션을 만듭니다.

![](hello-ios-quickstart-images/image3new.png "iOS 솔루션")


2. **새 솔루션 대화 상자**에서 **iOS > 응용 프로그램 > 단일 뷰 응용 프로그램** 템플릿을 선택하여 C#이 선택되어 있는지 확인합니다. **다음**을 클릭합니다.

  ![](hello-ios-quickstart-images/image4new.png "단일 뷰 응용 프로그램 선택")

3. 앱을 구성합니다. **이름**으로 `Phoneword_iOS`를 지정하고, 나머지는 기본값을 유지합니다. **다음**을 클릭합니다.

  ![](hello-ios-quickstart-images/image5new.png "앱 이름 입력")

4. 프로젝트 및 솔루션 이름은 그대로 둡니다. 여기서 프로젝트의 위치를 선택하거나 기본값으로 유지합니다.

  ![](hello-ios-quickstart-images/image6new.png "프로젝트 위치 선택")

5. **만들기**를 클릭하여 **솔루션**을 만듭니다.

6. **솔루션 패드**에서 **Main.storyboard** 파일을 두 번 클릭하여 엽니다. 이렇게 하면 시각적으로 UI를 만들 수 있습니다.

  ![](hello-ios-quickstart-images/image7new.png "iOS 디자이너")

  _크기 클래스_는 기본적으로 사용하도록 설정되어 있습니다. 자세한 내용은 [통합 스토리보드](~/ios/user-interface/storyboards/unified-storyboards.md) 가이드를 참조하세요.

8. **도구 상자 패드**의 검색 표시줄에서 "레이블"을 입력한 다음, **레이블**을 디자인 화면(가운데 영역)으로 끌어 놓습니다.

  ![](hello-ios-quickstart-images/image8new.png "레이블을 가운데에 있는 디자인 화면으로 끌기")

  > [!NOTE]
> **참고:** 언제든지 **보기 > 패드**로 이동하여 **Properties Pad** 또는 **도구 상자**를 가져올 수 있습니다.

9. *컨트롤 끌기*(컨트롤 주위에 있는 원)의 핸들을 잡고 레이블을 넓게 만듭니다.

  ![](hello-ios-quickstart-images/image9.png "레이블을 넓게 만들기")


10. 디자인 화면에서 **레이블**을 선택한 채로 **속성 패드**를 사용하여 **레이블**의 **텍스트** 속성을 "Phoneword 입력"으로 변경합니다.

  ![](hello-ios-quickstart-images/image10.png "Phoneword 입력 레이블 설정")

11. 도구 상자 내에서 "텍스트 필드"를 검색하고, **텍스트 필드**를 **도구 상자**에서 디자인 화면으로 끌어서 **레이블** 아래에 놓습니다. **텍스트 필드**가 **레이블**과 같은 너비가 될 때까지 조정합니다.

  ![](hello-ios-quickstart-images/image12new.png "텍스트 필드를 레이블과 같은 너비로 만들기")


12. 디자인 화면에서 **텍스트 필드**를 선택한 채로 **Properties Pad**의 ID 섹션에서 **텍스트 필드**의 **이름** 속성을 `PhoneNumberText`로 변경하고, **텍스트** 속성을 "1-855-XAMARIN"으로 변경합니다.

  ![](hello-ios-quickstart-images/image13new.png "제목 속성을 1-855-XAMARIN으로 변경")


13. **단추**를 **도구 상자**에서 디자인 화면으로 끌어서 **텍스트 필드** 아래에 놓습니다. **단추**가 **텍스트 필드** 및 **레이블**과 같은 너비가 되도록 조정합니다.

  ![](hello-ios-quickstart-images/image14new.png "단추가 텍스트 필드 및 레이블과 같은 너비가 되도록 조정")


14. 디자인 화면에서 **단추**를 선택한 채로 **속성 패드**의 **ID** 섹션에서 **이름** 속성을 `TranslateButton`으로 변경합니다. **제목** 속성을 "변환"으로 변경합니다.

  ![](hello-ios-quickstart-images/image15new.png "제목 속성을 변환으로 변경")


15. 위의 두 단계를 반복하고, **단추**를 **도구 상자**에서 디자인 화면으로 끌어서 첫 번째 **단추** 아래에 놓습니다. **단추**가 첫 번째 **단추**와 같은 너비가 되도록 조정합니다.

  ![](hello-ios-quickstart-images/image16new.png "단추가 첫 번째 단추와 같은 너비가 되도록 조정")


16. 디자인 화면에서 두 번째 **단추**를 선택한 채로 **속성 패드**의 **ID** 섹션에서 **이름** 속성을 `CallButton`으로 변경합니다. **제목** 속성을 "호출"로 변경합니다.

  ![](hello-ios-quickstart-images/image17new.png "제목 속성을 호출로 변경")

  **파일 > 저장**으로 이동하거나 **⌘+s**를 눌러 변경 내용을 저장합니다.


17. 전화 번호를 영숫자에서 숫자로 변환하려면 일부 논리를 앱에 추가해야 합니다. **솔루션 패드**에서 **Phoneword_iOS** 프로젝트를 마우스 오른쪽 단추로 클릭하고, **추가 > 새 파일...**을 선택하거나 **⌘+n**을 눌러 프로젝트에 새 파일을 추가합니다.

  ![](hello-ios-quickstart-images/image18.png "프로젝트에 새 파일 추가")


18. **새 파일** 대화 상자에서 **일반 > 빈 클래스**를 선택하고 새 파일의 이름을 `PhoneTranslator`로 지정합니다.

  ![](hello-ios-quickstart-images/image19.png "빈 클래스를 선택하고 새 파일의 이름을 PhoneTranslator로 지정")


19. 이렇게 하면 비어 있는 새 C# 클래스가 만들어집니다. 모든 템플릿 코드를 제거하고 다음 코드로 바꿉니다.

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

20. 사용자 인터페이스를 연결하는 코드를 추가합니다. 이렇게 하려면 **솔루션 패드**에서 **ViewController.cs**를 두 번 클릭하여 엽니다.

  ![](hello-ios-quickstart-images/image20new.png "사용자 인터페이스를 연결하는 코드 추가")


21. `TranslateButton`을 연결하여 시작합니다. **ViewController** 클래스에서 `ViewDidLoad` 메서드를 찾고 `base.ViewDidLoad()` 호출 아래에 다음 코드를 추가합니다.

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

22. `CallButton`이라는 두 번째 단추를 눌러 사용자에게 응답하는 코드를 추가합니다. `TranslateButton`에 대한 코드 아래에 다음 코드를 배치하고, `using Foundation;`을 파일의 맨 위에 추가합니다.

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


23. 변경 내용을 저장한 다음, **빌드 > 모두 빌드**를 선택하거나 **⌘+B**를 눌러 응용 프로그램을 빌드합니다.  응용 프로그램이 컴파일되면 IDE의 위쪽에 성공 메시지가 표시됩니다.

  ![](hello-ios-quickstart-images/image21.png "IDE 위쪽에 표시된 성공 메시지")

  오류가 있는 경우 이전 단계를 진행하고, 응용 프로그램이 성공적으로 빌드될 때까지 모든 오류를 수정합니다.

27. 마지막으로, **iOS 시뮬레이터**에서 응용 프로그램을 테스트합니다. IDE의 왼쪽 위에 있는 첫 번째 드롭다운에서 **디버그**를 선택하고, 두 번째 드롭다운에서 **iPhone 8 Plus iOS x.x**를 선택하고, **시작**(재생 단추와 비슷한 삼각형 단추)을 누릅니다.

  ![](hello-ios-quickstart-images/image27new.png "시작 누르기")


  > [!NOTE]
> **참고:** 현재 Apple의 요구 사항에 따라 장치 또는 시뮬레이터용 코드를 빌드하는 데 개발 인증서 또는 *서명 ID*가 필요할 수 있습니다. 이를 설정하려면 [장치 프로비전 가이드](~/ios/get-started/installation/device-provisioning/manual-provisioning.md)의 단계를 수행합니다.

28. 그러면 iOS 시뮬레이터 내에서 응용 프로그램이 시작됩니다.

  ![](hello-ios-quickstart-images/image28.png "iOS 시뮬레이터 내에서 실행되는 응용 프로그램")

  iOS 시뮬레이터에서는 전화 통화가 지원되지 않습니다. 대신 통화를 시도할 때 경고 대화 상자가 표시됩니다.

  ![](hello-ios-quickstart-images/image29.png "통화를 시도할 때의 경고 대화 상자")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-walkthrough"></a>Visual Studio 연습

이 연습에서는 영숫자 전화 번호를 숫자 전화 번호로 변환하는 Phoneword라는 응용 프로그램을 만드는 방법을 설명합니다.

**참고**: 이 연습에서는 Windows 10 Virtual Machine에서 Visual Studio Enterprise 2017을 사용합니다. 위의 요구 사항을 충족하는 한 설정이 이와 다를 수 있지만, 일부 스크린샷은 설정에 따라 다를 수 있습니다.

> [!NOTE]
> **이 연습을 진행하기 전에** 이미 Visual Studio에서 Mac에 _연결되어 있어야 합니다_. 이는 Xamarin.iOS에서 Apple의 도구를 사용하여 iOS 디자이너 및 응용 프로그램을 빌드하고 실행하기 때문입니다. 설정하려면 [Mac에 연결](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 가이드의 단계를 수행합니다.

1. **시작** 메뉴에서 Visual Studio를 시작합니다.

  ![](hello-ios-quickstart-images/image001-.png "시작 화면")

  **새 솔루션** 아래의 검색 상자에서 _단일 뷰 앱_을 입력하고, 새 Xamarin.iOS 솔루션을 만들기 위해 **단일 뷰 앱(iPhone)**을 선택합니다.

  ![](hello-ios-quickstart-images/image002-.png "단일 뷰 앱 추가")


2. 아래 그림과 같이 프로젝트 및 솔루션의 이름을 `Phoneword`로 지정합니다.

  ![](hello-ios-quickstart-images/vs-image3.png "프로젝트 이름을 PhonewordiOS로, 새 솔루션 이름을 Phoneword로 지정")


3. **확인**을 눌러 새 프로젝트를 만듭니다.

4. 도구 모음에서 Xamarin Mac 에이전트 아이콘이 녹색인지 확인합니다.

    ![도구 모음에서 Xamarin Mac 에이전트 아이콘이 녹색인지 확인](hello-ios-quickstart-images/vs-image4.png)

    그렇지 않은 경우 Mac 빌드 호스트에 연결되지 않았다는 의미이므로 [구성 가이드](~/ios/get-started/installation/windows/connecting-to-mac/index.md)의 단계에 따라 연결합니다.


5. **솔루션 탐색기**에서 **Main.storyboard** 파일을 두 번 클릭하여 iOS 디자이너에서 이 파일을 엽니다.

  ![](hello-ios-quickstart-images/vs-image7.png "iOS 디자이너")

6. **도구 상자** 탭을 열고, 검색 표시줄에서 "레이블"을 입력한 다음, **레이블**을 디자인 화면(가운데 영역)으로 끌어갑니다.

  ![](hello-ios-quickstart-images/vs-image8.png "레이블을 가운데에 있는 디자인 화면으로 끌기")


7. 다음으로, *컨트롤 끌기*의 핸들을 잡고 레이블을 넓게 만듭니다.

  ![](hello-ios-quickstart-images/vs-image9.png "레이블을 넓게 만들기")


8. 디자인 화면에서 **레이블**을 선택한 채로 **속성 창**을 사용하여 **레이블**의 **텍스트** 속성을 "Phoneword 입력"으로 변경합니다.

  ![](hello-ios-quickstart-images/vs-image10.png "레이블의 텍스트 속성을 'Phoneword 입력'으로 변경")

  > [!NOTE]
> **참고:** 언제든지 **보기** 메뉴로 이동하여 **속성** 또는 **도구 상자**를 가져올 수 있습니다.


9. 도구 상자 내에서 "텍스트 필드"를 검색하고, **텍스트 필드**를 **도구 상자**에서 디자인 화면으로 끌어서 **레이블** 아래에 놓습니다. **텍스트 필드**가 **레이블**과 같은 너비가 될 때까지 조정합니다.

  ![](hello-ios-quickstart-images/vs-image12.png "텍스트 필드가 레이블과 같은 너비가 될 때까지 조정")


10. 디자인 화면에서 **텍스트 필드**를 선택한 채로 **속성**의 ID 섹션에서 **텍스트 필드**의 **이름** 속성을 `PhoneNumberText`로 변경하고, **텍스트** 속성을 "1-855-XAMARIN"으로 변경합니다.

  ![](hello-ios-quickstart-images/vs-image13.png "텍스트 속성을 1-855-XAMARIN으로 변경")


11. **단추**를 **도구 상자**에서 디자인 화면으로 끌어서 **텍스트 필드** 아래에 놓습니다. **단추**가 **텍스트 필드** 및 **레이블**과 같은 너비가 되도록 조정합니다.

  ![](hello-ios-quickstart-images/vs-image14.png "단추가 텍스트 필드 및 레이블과 같은 너비가 되도록 조정")


12. 디자인 화면에서 **단추**를 선택한 채로 **속성**의 **ID** 섹션에서 **이름** 속성을 `TranslateButton`으로 변경합니다. **제목** 속성을 "변환"으로 변경합니다.

  ![](hello-ios-quickstart-images/vs-image15.png "제목 속성을 변환으로 변경")


13. 앞의 두 단계를 반복하고, **단추**를 **도구 상자**에서 디자인 화면으로 끌어서 첫 번째 **단추** 아래에 놓습니다. **단추**가 첫 번째 **단추**와 같은 너비가 되도록 조정합니다.

  ![](hello-ios-quickstart-images/vs-image16.png "단추가 첫 번째 단추와 같은 너비가 되도록 조정")


14. 디자인 화면에서 두 번째 **단추**를 선택한 채로 **속성**의 **ID** 섹션에서 **이름** 속성을 `CallButton`으로 변경합니다. **제목** 속성을 "호출"로 변경합니다.

  ![](hello-ios-quickstart-images/vs-image17.png "제목 속성을 호출로 변경")

  **파일 > 모두 저장**으로 이동하거나 **Ctrl+s**를 눌러 변경 내용을 저장합니다.

15. 전화 번호를 영숫자에서 숫자로 변환하는 코드를 추가합니다. 이렇게 하려면 먼저 **솔루션 탐색기**에서 **Phoneword** 프로젝트를 마우스 오른쪽 단추로 클릭하고, **추가 > 새 항목...**을 차례로 선택하거나 **Ctrl+Shift+A**를 눌러 프로젝트에 새 파일을 추가합니다.

  ![](hello-ios-quickstart-images/vs-image18.png "전화 번호를 영숫자에서 숫자로 변환하는 코드 추가")


16. **새 파일** 대화 상자에서 **Apple > 클래스**를 선택하고 새 파일의 이름을 `PhoneTranslator`로 지정합니다.

  ![](hello-ios-quickstart-images/vs-image19.png "PhoneTranslator라는 새 클래스 추가")

  > [!IMPORTANT]
> 아이콘에 C#이 있는 '클래스' 템플릿을 선택했는지 확인합니다. 그렇지 않으면 이 새 클래스를 참조하지 못할 수 있습니다.


17. 그러면 새 C# 클래스가 만들어집니다. 모든 템플릿 코드를 제거하고 다음 코드로 바꿉니다.

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

18. **솔루션 탐색기**에서 **ViewController.cs**를 두 번 클릭하여 단추와의 상호 작용을 처리하는 논리를 추가할 수 있습니다.

  ![](hello-ios-quickstart-images/vs-image20.png "단추와의 상호 작용을 처리하는 논리 추가")


19. `TranslateButton`을 연결하여 시작합니다. **ViewController** 클래스에서 `ViewDidLoad` 메서드를 찾습니다. `ViewDidLoad` 내의 `base.ViewDidLoad()` 호출 아래에 다음 단추 코드를 추가합니다.

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

20. `CallButton`이라는 두 번째 단추를 눌러 사용자에게 응답하는 코드를 추가합니다. `TranslateButton`에 대한 코드 아래에 다음 코드를 배치하고, `using Foundation;`을 파일의 맨 위에 추가합니다.

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


21. 변경 내용을 저장한 다음, **빌드 > 솔루션 빌드**를 선택하거나 **Ctrl+Shift+B**를 눌러 응용 프로그램을 빌드합니다.  응용 프로그램이 컴파일되면 IDE의 아래쪽에 성공 메시지가 표시됩니다.

  ![](hello-ios-quickstart-images/vs-image21.png "IDE 아래쪽에 표시된 성공 메시지")

  오류가 있는 경우 이전 단계를 진행하고, 응용 프로그램이 성공적으로 빌드될 때까지 모든 오류를 수정합니다.

22. 마지막으로, **원격 iOS 시뮬레이터**에서 응용 프로그램을 테스트합니다. IDE 도구 모음의 드롭다운 메뉴에서 **디버그** 및 **iPhone 8 Plus iOS x.x**를 선택하고, **시작**(재생 단추와 비슷한 녹색 삼각형)을 누릅니다.

  ![](hello-ios-quickstart-images/vs-image27.png "시작 누르기")

23. 그러면 iOS 시뮬레이터 내에서 응용 프로그램이 시작됩니다.

  ![](hello-ios-quickstart-images/vs-image28.png "iOS 시뮬레이터 내에서 실행되는 응용 프로그램")

  iOS 시뮬레이터에서는 전화 통화가 지원되지 않습니다. 대신 통화를 시도할 때 경고 대화 상자가 표시됩니다.

  ![](hello-ios-quickstart-images/vs-image29.png "통화를 시도할 때 표시되는 경고 대화 상자")

-----

첫 번째 Xamarin.iOS 응용 프로그램을 완성한 것을 축하합니다!

이제 이 가이드의 도구와 기술을 [Hello, iOS 심층 분석](~/ios/get-started/hello-ios/hello-ios-deepdive.md)에서 분석할 시간입니다.


## <a name="related-links"></a>관련 링크

- [Xamarin 앱 아이콘 및 시작 이미지(샘플)](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)
- [Hello, iOS(샘플)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS 휴먼 인터페이스 지침](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS 프로비전 포털](https://developer.apple.com/ios/manage/overview/index.action)
