---
title: "Hello, iOS 멀티스크린"
description: "두 부분으로 구성된 이 가이드에서는 Hello, iOS 가이드에서 만든 Phoneword 응용 프로그램을 확장하여 두 번째 화면을 처리합니다. 이 과정에서 모델-뷰-컨트롤러 디자인 패턴을 소개하고, 첫 번째 iOS 탐색을 구현하고, iOS 응용 프로그램의 구조 및 기능에 대한 심층적인 이해를 개발합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: d72e6230-c9ee-4bee-90ec-877d256821aa
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: d4b7439448d5c25aacf991c89a188086c95a44ac
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="helloios-multiscreen-quickstart"></a>Hello.iOS 멀티스크린 빠른 시작

이 연습의 부분은 앱으로 호출된 전화번호의 기록을 표시하는 두 번째 화면을 Phoneword 응용 프로그램에 추가합니다. 다음 스크린샷에 표시된 것처럼 최종 응용 프로그램에 호출 기록을 표시하는 두 번째 화면이 포함됩니다.

 [![](hello-ios-multiscreen-quickstart-images/00.png "이 스크린샷에 표시된 것처럼 최종 응용 프로그램에 호출 기록을 표시하는 두 번째 화면이 포함됩니다.")](hello-ios-multiscreen-quickstart-images/00.png#lightbox)

[함께 제공되는 심층 분석](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md)은 빌드하였던 응용 프로그램을 검토하고, 진행하면서 나오는 아키텍처, 탐색 및 다른 새 iOS 개념에 대해 논의합니다.

 <a name="Requirements" />

## <a name="requirements"></a>요구 사항

이 가이드는 Hello, iOS 문서가 중단된 곳에서 다시 시작하고 [Hello, iOS 빠른 시작](~/ios/get-started/hello-ios/index.md)의 완료가 필요합니다. 전체 버전의 Phoneword 앱은 [Hello, iOS 샘플](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)에서 다운로드할 수 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="walkthrough"></a>연습

이 연습에서는 **Phoneword** 응용 프로그램에 통화 기록 화면을 추가합니다.


1. Mac용 Visual Studio에서 **Phoneword** 응용 프로그램을 엽니다. 필요한 경우 [Hello, iOS 연습](~/ios/get-started/hello-ios/index.md) 가이드의 완료된 Phoneword 응용 프로그램을 [여기](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)에서 다운로드할 수 있습니다.


2. **Solution Pad**에서 **Main.storyboard** 파일을 엽니다.

  ![](hello-ios-multiscreen-quickstart-images/02new.png "iOS 디자이너에 있는 Main.storyboard")


3. **탐색 컨트롤러**를 **도구 상자**에서 디자인 화면으로 끌어 옵니다. (디자인 화면에서 이러한 모든 내용에 맞게 축소해야 할 수 있습니다!)

  ![](hello-ios-multiscreen-quickstart-images/03new.png "탐색 컨트롤러를 도구 상자에서 디자인 화면으로 끌어 옵니다.")


4. **원본 없는 Segue**(단일 보기 컨트롤러의 왼쪽에 있는 회색 화살표)를 **탐색 컨트롤러**로 끌어와서 응용 프로그램의 시작점을 변경합니다.

  ![](hello-ios-multiscreen-quickstart-images/04new.png "원본 없는 Segue를 탐색 컨트롤러로 끌어 와 응용 프로그램의 시작점을 변경합니다.")


5. 표시줄을 클릭하여 기존 **루트 보기 컨트롤러**를 선택하고, **삭제**를 눌러 디자인 화면에서 제거합니다.
그런 다음, **Phoneword** 장면을 **탐색 컨트롤러** 옆으로 이동합니다.

  ![](hello-ios-multiscreen-quickstart-images/05new.png "Phoneword 장면을 탐색 컨트롤러 옆으로 이동합니다.")


6. **ViewController**를 탐색 컨트롤러의 **루트 뷰 컨트롤러**로 설정합니다. **Ctrl** 키를 누르고 **탐색 컨트롤러** 내부를 클릭합니다. 파란색 선이 표시됩니다. 그런 다음, **Ctrl** 키를 누른 상태로 **탐색 컨트롤러**에서 **Phoneword** 장면으로 끌어서 놓습니다. 이는 _Ctrl 끌기_라고 합니다.

 ![](hello-ios-multiscreen-quickstart-images/06.png "탐색 컨트롤러에서 Phoneword 장면으로 끌어서 놓습니다.")


7. 팝오버에서 관계를 **루트**로 설정합니다.

  ![](hello-ios-multiscreen-quickstart-images/07new.png "루트로 관계 설정")

  **ViewController**는 이제 **탐색 컨트롤러의 루트 보기 컨트롤러입니다.**

  ![](hello-ios-multiscreen-quickstart-images/08.png "ViewController는 이제 탐색 컨트롤러의 루트 뷰 컨트롤러입니다.")


8. **Phoneword** 화면의 **제목** 표시줄을 두 번 클릭하고 **제목**을 **Phoneword**로 변경합니다.

  ![](hello-ios-multiscreen-quickstart-images/09.png "제목을 'Phoneword'로 변경합니다.")


9. **도구 상자**에서 **단추**를 끌어서 **호출 단추** 아래에 배치합니다. 핸들을 끌어 새 **단추**가 **호출 단추**와 동일한 너비가 되도록 합니다.

  ![](hello-ios-multiscreen-quickstart-images/10new.png "새 단추가 호출 단추와 동일한 너비가 되도록 합니다.")


10. **Properties Pad**에서 단추 **이름**을 **CallHistoryButton**으로 변경하고 **제목**을 **통화 기록**으로 변경합니다.

  ![](hello-ios-multiscreen-quickstart-images/11new.png "단추의 이름을 CallHistoryButton으로 변경하고 제목을 통화 기록으로 변경합니다.")


11. **통화 기록** 화면을 만듭니다. **도구 상자**에서 **테이블 뷰 컨트롤러**를 디자인 화면으로 끕니다.

 ![](hello-ios-multiscreen-quickstart-images/12new.png "테이블 뷰 컨트롤러를 디자인 화면으로 끕니다.")


12. 다음으로 장면의 아래쪽에 있는 검은색 표시줄을 클릭하여 **테이블 뷰 컨트롤러**를 선택합니다. **Properties Pad**에서 **테이블 뷰 컨트롤러**의 클래스를 `CallHistoryController`로 변경하고 **Enter** 키를 누릅니다.

  ![](hello-ios-multiscreen-quickstart-images/13new.png "테이블 뷰 컨트롤러 클래스를 CallHistoryController로 변경합니다.")

  iOS 디자이너는 `CallHistoryController`라는 사용자 지정 백업 클래스를 생성하여 이 화면의 콘텐츠 뷰 계층 구조를 관리합니다.
  **CallHistoryController.cs** 파일은 **Solution Pad**에 표시됩니다.

  ![](hello-ios-multiscreen-quickstart-images/14new.png "Solution Pad의 CallHistoryController.cs 파일")


13. **CallHistoryController.cs** 파일을 두 번 클릭하여 열고 내용을 다음 코드로 바꿉니다.

  ```csharp
  using System;
  using Foundation;
  using UIKit;
  using System.Collections.Generic;

  namespace Phoneword_iOS
  {
      public partial class CallHistoryController : UITableViewController
      {
          public List<string> PhoneNumbers { get; set; }

          static NSString callHistoryCellId = new NSString ("CallHistoryCell");

          public CallHistoryController (IntPtr handle) : base (handle)
          {
              TableView.RegisterClassForCellReuse (typeof(UITableViewCell), callHistoryCellId);
              TableView.Source = new CallHistoryDataSource (this);
              PhoneNumbers = new List<string> ();
          }

          class CallHistoryDataSource : UITableViewSource
          {
              CallHistoryController controller;

              public CallHistoryDataSource (CallHistoryController controller)
              {
                  this.controller = controller;
              }

              public override nint RowsInSection (UITableView tableView, nint section)
              {
                  return controller.PhoneNumbers.Count;
              }

              public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
              {
                  var cell = tableView.DequeueReusableCell (CallHistoryController.callHistoryCellId);

                  int row = indexPath.Row;
                  cell.TextLabel.Text = controller.PhoneNumbers [row];
                  return cell;
              }
          }
      }
  }
  ```

  응용 프로그램을 저장하고(**⌘ + s**) 빌드하여(**⌘ + b**) 오류가 없는지 확인합니다.


14. **Phoneword** 장면과 **통화 기록** 장면 사이에 _Segue_(전환)를 만듭니다.
  **Phoneword 장면**에서 **통화 기록 단추**를 선택하고 **단추**에서 **통화 기록** 장면으로 Ctrl 끌기합니다.

  ![](hello-ios-multiscreen-quickstart-images/15.png "단추에서 통화 기록 장면으로 Ctrl 끌기")

  **작업 Segue** 팝오버에서 **표시**를 선택합니다.

  iOS 디자이너는 두 장면 사이에 Segue를 추가할 예정입니다.

  ![](hello-ios-multiscreen-quickstart-images/17new.png "두 장면 사이의 Segue")


15. 장면 아래쪽의 검은색 표시줄을 선택하고 **Properties Pad**에서 **뷰 컨트롤러 제목**을 **통화 기록**으로 변경하여 **제목**을 **테이블 뷰 컨트롤러**에 추가합니다.

  ![](hello-ios-multiscreen-quickstart-images/18new.png "Properties Pad에서 뷰 컨트롤러 제목을 통화 기록으로 변경합니다.")

16. 응용 프로그램이 실행될 때 **통화 기록 단추**는 **통화 기록** 화면을 열지만 테이블 뷰는 추적을 유지하고 전화번호를 표시할 코드가 없기 때문에 비어 있게 됩니다.

  이 앱은 문자열의 목록으로 전화번호를 저장합니다.

  **ViewController**의 맨 위에 `System.Collections.Generic`에 대한 `using` 지시문을 추가합니다.

  ```csharp
  using System.Collections.Generic;
  ```



17. `ViewController` 클래스를 다음 코드로 수정합니다.

    ```csharp
    using System;
    using System.Collections.Generic;
    using Foundation;
    using UIKit;

    namespace Phoneword_iOS
    {
      public partial class ViewController : UIViewController
      {
        string translatedNumber = "";

        public List<string> PhoneNumbers { get; set; }

        protected ViewController(IntPtr handle) : base(handle)
        {
          //initialize list of phone numbers called for Call History screen
          PhoneNumbers = new List<string>();
        }

        public override void ViewDidLoad()
        {
          base.ViewDidLoad();
          // Perform any additional setup after loading the view, typically from a nib.

          TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
            // Convert the phone number with text to a number
            // using PhoneTranslator.cs
            translatedNumber = PhoneTranslator.ToNumber(
              PhoneNumberText.Text);

            // Dismiss the keyboard if text field was tapped
            PhoneNumberText.ResignFirstResponder();

            if (translatedNumber == "")
            {
              CallButton.SetTitle("Call ", UIControlState.Normal);
              CallButton.Enabled = false;
            }
            else
            {
              CallButton.SetTitle("Call " + translatedNumber,
                UIControlState.Normal);
              CallButton.Enabled = true;
            }
          };

          CallButton.TouchUpInside += (object sender, EventArgs e) => {

            //Store the phone number that we're dialing in PhoneNumbers
            PhoneNumbers.Add(translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl("tel:" + translatedNumber);

            // otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl(url))
            {
              var alert = UIAlertController.Create("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
              alert.AddAction(UIAlertAction.Create("Ok", UIAlertActionStyle.Default, null));
              PresentViewController(alert, true, null);
            }
          };
        }

        public override void PrepareForSegue(UIStoryboardSegue segue, NSObject sender)
        {
          base.PrepareForSegue(segue, sender);

          // set the View Controller that’s powering the screen we’re
          // transitioning to

          var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

          //set the Table View Controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryContoller != null)
          {
            callHistoryContoller.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```


여기서 몇 가지 일이 발생합니다.
  * `translatedNumber` 변수가 `ViewDidLoad` 메서드에서 _클래스 수준 변수_로 이동되었습니다.
  * `PhoneNumbers.Add(translatedNumber)`를 호출하여 전화를 건 번호를 전화번호 목록에 추가하도록 **CallButton** 코드가 수정되었습니다.
  * `PrepareForSegue` 메서드가 추가되었습니다.

  응용 프로그램을 저장하고 빌드하여 오류가 없는지 확인합니다.

20. **시작** 단추를 눌러 **iOS 시뮬레이터** 내에서 응용 프로그램을 시작합니다.

  ![](hello-ios-multiscreen-quickstart-images/19.png "시작 단추를 눌러 iOS 시뮬레이터 내에서 응용 프로그램을 시작합니다.")


첫 번째 멀티스크린 Xamarin.iOS 응용 프로그램을 완성한 것을 축하합니다!

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="walkthrough"></a>연습

이 연습에서는 **Phoneword** 응용 프로그램에 통화 기록 화면을 추가합니다.


1. Visual Studio에서 **Phoneword** 응용 프로그램을 엽니다. 필요한 경우 [Hello, iOS 연습](~/ios/get-started/hello-ios/index.md) 가이드에서 [완료된 Phoneword 응용 프로그램](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)을 다운로드합니다. iOS 디자이너 및 iOS 시뮬레이터를 사용하도록 [Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)에 연결해야 합니다.


2. 사용자 인터페이스를 편집하여 시작합니다. **다른 이름으로 보기**가 _iPhone 6_으로 설정되었는지 확인하여 **솔루션 탐색기**에서 **Main.storyboard** 파일을 엽니다.

  ![](hello-ios-multiscreen-quickstart-images/image1.png "iOS 디자이너에 있는 Main.storyboard")


3. **도구 상자**에서 디자인 화면으로 **탐색 컨트롤러**를 끌어 옵니다.

  ![](hello-ios-multiscreen-quickstart-images/image2.png "탐색 컨트롤러를 도구 상자에서 디자인 화면으로 끌어 옵니다.")


4. **원본 없는 Segue**(**Phoneword** 장면의 왼쪽에 있는 회색 화살표)를 **Phoneword** 장면에서 **탐색 컨트롤러**로 끌어 와 응용 프로그램의 시작점을 변경합니다.

  ![](hello-ios-multiscreen-quickstart-images/image3.png "원본 없는 Segue를 탐색 컨트롤러로 끌어 와 응용 프로그램의 시작점을 변경합니다.")


5. 검은색 표시줄을 클릭하여 **루트 뷰 컨트롤러**를 선택하고 **삭제**를 눌러 디자인 화면에서 제거합니다.
  그런 다음, **Phoneword** 장면을 **탐색 컨트롤러** 옆으로 이동합니다.

  ![](hello-ios-multiscreen-quickstart-images/image4.png "Phoneword 장면을 탐색 컨트롤러 옆으로 이동합니다.")


6. **ViewController**를 탐색 컨트롤러의 `Root View Controller`로 설정합니다. **Ctrl** 키를 누르고 **탐색 컨트롤러** 내부를 클릭합니다. 파란색 선이 표시됩니다. 그런 다음, **Ctrl** 키를 누른 상태로 **탐색 컨트롤러**에서 **Phoneword** 장면으로 끌어서 놓습니다. 이는 _Ctrl 끌기_라고 합니다.

  ![](hello-ios-multiscreen-quickstart-images/image5.png "탐색 컨트롤러에서 Phoneword 장면으로 끌어서 놓습니다.")


7. 팝오버에서 관계를 **루트**로 설정합니다.

  ![](hello-ios-multiscreen-quickstart-images/image6.png "루트로 관계를 설정합니다.")

  **ViewController**는 이제 **탐색 컨트롤러의 루트 뷰 컨트롤러**입니다.


8. **Phoneword** 화면의 **제목** 표시줄을 두 번 클릭하고 **제목**을 **Phoneword**로 변경합니다.

  ![](hello-ios-multiscreen-quickstart-images/image7.png "제목을 Phoneword로 변경합니다.")


9. **도구 상자**에서 **단추**를 끌어서 **호출 단추** 아래에 배치합니다. 핸들을 끌어 새 **단추**가 **호출 단추**와 동일한 너비가 되도록 합니다.

  ![](hello-ios-multiscreen-quickstart-images/image8.png "새 단추가 호출 단추와 동일한 너비가 되도록 합니다.")


10. **속성 탐색기**에서 **단추**의 **이름**을 `CallHistoryButton`으로 변경하고 **제목**을 **통화 기록**으로 변경합니다.

  ![](hello-ios-multiscreen-quickstart-images/image9.png "단추의 이름을 'CallHistoryButton'으로 변경하고 제목을 '통화 기록'으로 변경합니다.")


11. **통화 기록** 화면을 만듭니다. **도구 상자**에서 **테이블 뷰 컨트롤러**를 디자인 화면으로 끕니다.

  ![](hello-ios-multiscreen-quickstart-images/image10.png "테이블 뷰 컨트롤러를 디자인 화면으로 끕니다.")


12. 장면의 아래쪽에 있는 검은색 표시줄을 클릭하여 **테이블 뷰 컨트롤러**를 선택합니다. **속성 탐색기**에서 **테이블 뷰 컨트롤러**의 클래스를 `CallHistoryController`로 변경하고 **Enter** 키를 누릅니다.

  ![](hello-ios-multiscreen-quickstart-images/image11.png "테이블 뷰 컨트롤러 클래스를 CallHistoryController로 변경합니다.")

  iOS 디자이너는 `CallHistoryController`라는 사용자 지정 백업 클래스를 생성하여 이 화면의 콘텐츠 뷰 계층 구조를 관리합니다.
  **CallHistoryController.cs** 파일은 **솔루션 탐색기**에 표시됩니다.

  ![](hello-ios-multiscreen-quickstart-images/image12.png "솔루션 탐색기의 CallHistoryController.cs 파일")


13. **CallHistoryController.cs** 파일을 두 번 클릭하여 열고 내용을 다음 코드로 바꿉니다.

        using System;
        using Foundation;
        using UIKit;
        using System.Collections.Generic;

        namespace Phoneword
        {
            public partial class CallHistoryController : UITableViewController
            {
                public List<String> PhoneNumbers { get; set; }

                static NSString callHistoryCellId = new NSString ("CallHistoryCell");

                public CallHistoryController (IntPtr handle) : base (handle)
                {
                    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), callHistoryCellId);
                    TableView.Source = new CallHistoryDataSource (this);
                    PhoneNumbers = new List<string> ();
                }

                class CallHistoryDataSource : UITableViewSource
                {
                    CallHistoryController controller;

                    public CallHistoryDataSource (CallHistoryController controller)
                    {
                        this.controller = controller;
                    }

                    // Returns the number of rows in each section of the table
                    public override nint RowsInSection (UITableView tableView, nint section)
                    {
                        return controller.PhoneNumbers.Count;
                    }

                    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
                    {
                        var cell = tableView.DequeueReusableCell (CallHistoryController.callHistoryCellId);

                        int row = indexPath.Row;
                        cell.TextLabel.Text = controller.PhoneNumbers [row];
                        return cell;
                    }
                }
            }
        }

  응용 프로그램을 저장하고 빌드하여 오류가 없는지 확인합니다. 지금은 빌드 경고를 무시해도 됩니다.


14. **Phoneword** 장면과 **통화 기록** 장면 사이에 _Segue_(전환)를 만듭니다.
  **Phoneword 장면**에서 **통화 기록 단추**를 선택하고 **단추**에서 **통화 기록** 장면으로 **Ctrl 끌기**합니다.

  ![](hello-ios-multiscreen-quickstart-images/image13.png "단추에서 통화 기록 장면으로 Ctrl 끌기")

  **작업 Segue** 팝오버에서 **표시**를 선택합니다.

  ![](hello-ios-multiscreen-quickstart-images/image14.png "Segue 형식으로 표시를 선택합니다.")

  iOS 디자이너는 두 장면 사이에 Segue를 추가할 예정입니다.

  ![](hello-ios-multiscreen-quickstart-images/image15.png "두 장면 사이의 Segue")


15. 장면 아래쪽의 검은색 표시줄을 선택하고 **속성 컨트롤러**에서 **뷰 컨트롤러 > 제목**을 **통화 기록**으로 변경하여 **제목**을 **테이블 뷰 컨트롤러**에 추가합니다.

  ![](hello-ios-multiscreen-quickstart-images/image16.png "뷰 컨트롤러 제목을 통화 기록으로 변경합니다.")


16. 응용 프로그램이 실행될 때 **통화 기록 단추**는 **통화 기록** 화면을 열지만 테이블 뷰는 추적을 유지하고 전화번호를 표시할 코드가 없기 때문에 비어 있게 됩니다.

  이 앱은 문자열의 목록으로 전화번호를 저장합니다.

  **ViewController**의 맨 위에 `System.Collections.Generic`에 대한 `using` 지시문을 추가합니다.

        using System.Collections.Generic;

17. `ViewController` 클래스를 다음 코드로 수정합니다.

    ```csharp
    using System;
    using System.Collections.Generic;
    using Foundation;
    using UIKit;

    namespace Phoneword_iOS
    {
      public partial class ViewController : UIViewController
      {
        string translatedNumber = "";

        public List<string> PhoneNumbers { get; set; }

        protected ViewController(IntPtr handle) : base(handle)
        {
          //initialize list of phone numbers called for Call History screen
          PhoneNumbers = new List<string>();
        }

        public override void ViewDidLoad()
        {
          base.ViewDidLoad();
          // Perform any additional setup after loading the view, typically from a nib.

          TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
            // Convert the phone number with text to a number
            // using PhoneTranslator.cs
            translatedNumber = PhoneTranslator.ToNumber(
              PhoneNumberText.Text);

            // Dismiss the keyboard if text field was tapped
            PhoneNumberText.ResignFirstResponder();

            if (translatedNumber == "")
            {
              CallButton.SetTitle("Call ", UIControlState.Normal);
              CallButton.Enabled = false;
            }
            else
            {
              CallButton.SetTitle("Call " + translatedNumber,
                UIControlState.Normal);
              CallButton.Enabled = true;
            }
          };

          CallButton.TouchUpInside += (object sender, EventArgs e) => {

            //Store the phone number that we're dialing in PhoneNumbers
            PhoneNumbers.Add(translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl("tel:" + translatedNumber);

            // otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl(url))
            {
              var alert = UIAlertController.Create("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
              alert.AddAction(UIAlertAction.Create("Ok", UIAlertActionStyle.Default, null));
              PresentViewController(alert, true, null);
            }
          };
        }

        public override void PrepareForSegue(UIStoryboardSegue segue, NSObject sender)
        {
          base.PrepareForSegue(segue, sender);

          // set the View Controller that’s powering the screen we’re
          // transitioning to

          var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

          //set the Table View Controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryContoller != null)
          {
            callHistoryContoller.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```

여기서 몇 가지 일이 발생합니다.
  * `translatedNumber` 변수가 `ViewDidLoad` 메서드에서 _클래스 수준 변수_로 이동되었습니다.
  * `PhoneNumbers.Add(translatedNumber)`를 호출하여 전화를 건 번호를 전화번호 목록에 추가하도록 **CallButton** 코드가 수정되었습니다.
  * `PrepareForSegue` 메서드가 추가되었습니다.

  응용 프로그램을 저장하고 빌드하여 오류가 없는지 확인합니다.

  응용 프로그램을 저장하고 빌드하여 오류가 없는지 확인합니다.


20. **시작** 단추를 눌러 **iOS 시뮬레이터** 내에서 응용 프로그램을 시작합니다.

  ![](hello-ios-multiscreen-quickstart-images/19.png "샘플 앱의 첫 번째 화면")


첫 번째 멀티스크린 Xamarin.iOS 응용 프로그램을 완성한 것을 축하합니다!


-----

이제 앱은 두 스토리보드 Segue를 사용하여 코드에서 탐색을 처리할 수 있습니다. 이제 [Hello, iOS 멀티스크린 심층 분석](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md)에서 알아본 도구 및 기술을 분석할 시간입니다.


## <a name="related-links"></a>관련 링크

- [Hello, iOS(샘플)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS 휴먼 인터페이스 지침](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS 프로비전 포털](https://developer.apple.com/ios/manage/overview/index.action)
