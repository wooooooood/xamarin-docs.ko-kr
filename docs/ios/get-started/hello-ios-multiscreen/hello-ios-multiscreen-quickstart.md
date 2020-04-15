---
title: Hello, iOS 멀티스크린 – 빠른 시작
description: 이 문서에서는 Phoneword 샘플 애플리케이션을 확장하여 모델-뷰-컨트롤러, iOS 탐색 및 기타 핵심 iOS 개념을 설명하는 두 번째 화면을 추가하는 방법을 설명합니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: d72e6230-c9ee-4bee-90ec-877d256821aa
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: c89a3616bfa239ba919ae9750082bcef48c9f890
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73023366"
---
# <a name="hello-ios-multiscreen--quickstart"></a>Hello, iOS 멀티스크린 – 빠른 시작

연습의 이 부분에서는 앱으로 호출된 전화번호의 기록을 표시하는 두 번째 화면을 Phoneword 애플리케이션에 추가합니다. 다음 스크린샷에 표시된 것처럼 최종 애플리케이션에 호출 기록을 표시하는 두 번째 화면이 포함됩니다.

[![](hello-ios-multiscreen-quickstart-images/00.png "The final application will have a second screen that displays the call history, as illustrated by this screenshot")](hello-ios-multiscreen-quickstart-images/00.png#lightbox)

[함께 제공되는 심층 분석](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md)은 빌드하였던 애플리케이션을 검토하고, 진행하면서 나오는 아키텍처, 탐색 및 다른 새 iOS 개념에 대해 논의합니다.

## <a name="requirements"></a>요구 사항

이 가이드는 Hello, iOS 문서가 중단된 곳에서 다시 시작하고 [Hello, iOS 빠른 시작](~/ios/get-started/hello-ios/index.md)의 완료가 필요합니다. 전체 버전의 Phoneword 앱을 [Hello, iOS 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/hello-ios)에서 다운로드합니다.

::: zone pivot="macos"

## <a name="walkthrough-on-macos"></a>macOS에서 연습

이 연습에서는 **Phoneword** 애플리케이션에 통화 기록 화면을 추가합니다.

1. Mac용 Visual Studio에서 **Phoneword** 애플리케이션을 엽니다. 필요한 경우 [Hello, iOS 연습](~/ios/get-started/hello-ios/index.md) 가이드의 완료된 Phoneword 애플리케이션을 [여기](https://docs.microsoft.com/samples/xamarin/ios-samples/hello-ios)에서 다운로드할 수 있습니다.

2. **Solution Pad**에서 **Main.storyboard** 파일을 엽니다.

    ![](hello-ios-multiscreen-quickstart-images/02new.png "The Main.storyboard in the iOS Designer")

3. **탐색 컨트롤러**를 **도구 상자**에서 디자인 화면으로 끌어 옵니다(디자인 화면에서 이러한 모든 내용에 맞게 축소해야 할 수 있습니다!).

    ![](hello-ios-multiscreen-quickstart-images/03new.png "Drag a navigation controller from the Toolbox onto the design surface")

4. **원본 없는 segue**(단일 보기 컨트롤러의 왼쪽에 있는 회색 화살표)를 **탐색 컨트롤러**로 끌어와서 애플리케이션의 시작점을 변경합니다.

    ![](hello-ios-multiscreen-quickstart-images/04new.png "Drag the Sourceless Segue to the navigation controller to change the starting point of the application")

5. 아래쪽 표시줄을 클릭하여 기존 **루트 보기 컨트롤러**를 선택하고, **삭제**를 눌러 디자인 화면에서 제거합니다.
그런 다음, **Phoneword** 장면을 **탐색 컨트롤러** 옆으로 이동합니다.

    ![](hello-ios-multiscreen-quickstart-images/05new.png "Move the Phoneword scene next to the navigation controller")

6. **ViewController**를 탐색 컨트롤러의 **루트 보기 컨트롤러**로 설정합니다. **Ctrl** 키를 누르고 **탐색 컨트롤러** 내부를 클릭합니다. 파란색 선이 표시됩니다. 그런 다음, **Ctrl** 키를 누른 상태로 **탐색 컨트롤러**에서 **Phoneword** 장면으로 끌어서 놓습니다. 이는 _Ctrl 끌기_라고 합니다.

    ![](hello-ios-multiscreen-quickstart-images/06.png "Drag from the navigation controller to the Phoneword scene and release")

7. 팝오버에서 관계를 **루트**로 설정합니다.

    ![](hello-ios-multiscreen-quickstart-images/07new.png "Setting the relationship to Root")

    **ViewController**는 이제 **탐색 컨트롤러의 루트 보기 컨트롤러입니다.**

    ![](hello-ios-multiscreen-quickstart-images/08.png "The ViewController is now the navigation controllers Root view controller")

8. **Phoneword** 화면의 **제목** 표시줄을 두 번 클릭하고 **제목**을 **Phoneword**로 변경합니다.

    ![](hello-ios-multiscreen-quickstart-images/09.png "Change the Title to 'Phoneword'")

9. **도구 상자**에서 **단추**를 끌어서 **호출 단추** 아래에 배치합니다. 핸들을 끌어 새 **단추**가 **호출 단추**와 동일한 너비가 되도록 합니다.

    ![](hello-ios-multiscreen-quickstart-images/10new.png "Make the new Button the same width as the Call Button")

10. **Properties Pad**에서 단추 **이름**을 **CallHistoryButton**으로 변경하고 **제목**을 **통화 기록**으로 변경합니다.

    ![](hello-ios-multiscreen-quickstart-images/11new.png "Change the Name of the Button to CallHistoryButton and change the Title to Call History")

11. **통화 기록** 화면을 만듭니다. **도구 상자**에서 **테이블 보기 컨트롤러**를 디자인 화면으로 끕니다.

    ![](hello-ios-multiscreen-quickstart-images/12new.png "Drag a table view controller onto the design surface")

12. 그런 다음, 장면의 아래쪽에 있는 검은색 표시줄을 클릭하여 **테이블 보기 컨트롤러**를 선택합니다. **Properties Pad**에서 **테이블 보기 컨트롤러**의 클래스를 `CallHistoryController`로 변경하고 **Enter** 키를 누릅니다.

    ![](hello-ios-multiscreen-quickstart-images/13new.png "Change the table view controllers class to CallHistoryController")

    iOS 디자이너는 `CallHistoryController`라는 사용자 지정 백업 클래스를 생성하여 이 화면의 콘텐츠 보기 계층 구조를 관리합니다. **CallHistoryController.cs** 파일은 **Solution Pad**에 표시됩니다.

    ![](hello-ios-multiscreen-quickstart-images/14new.png "The CallHistoryController.cs file in the Solution Pad")

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

    애플리케이션을 저장하고(**⌘ + s**) 빌드하여(**⌘ + b**) 오류가 없는지 확인합니다.

14. **Phoneword** 장면과 **통화 기록** 장면 사이에 _segue_(전환)를 만듭니다.
  **Phoneword 장면**에서 **통화 기록 단추**를 선택하고 **단추**에서 **통화 기록** 장면으로 Ctrl 끌기합니다.

    ![](hello-ios-multiscreen-quickstart-images/15.png "Ctrl-drag from the Button to the Call History scene")

    **작업 Segue** 팝오버에서 **표시**를 선택합니다.

    iOS 디자이너는 두 장면 사이에 Segue를 추가할 예정입니다.

    ![](hello-ios-multiscreen-quickstart-images/17new.png "The Segue between the two scenes")

15. 장면 아래쪽의 검은색 표시줄을 선택하고 **Properties Pad**에서 **보기 컨트롤러 제목**을 **통화 기록**으로 변경하여 **제목**을 **테이블 보기 컨트롤러**에 추가합니다.

    ![](hello-ios-multiscreen-quickstart-images/18new.png "Change the view controller title to Call History in the Properties Pad")

16. 애플리케이션이 실행될 때 **통화 기록 단추**는 **통화 기록** 화면을 열지만, 테이블 보기는 추적을 유지하고 전화번호를 표시할 코드가 없기 때문에 비어 있게 됩니다.

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

          // set the view controller that’s powering the screen we’re
          // transitioning to

          var callHistoryController = segue.DestinationViewController as CallHistoryController;

          //set the table view controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryController != null)
          {
            callHistoryController.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```

    여기서 몇 가지 일이 발생합니다.

    - `translatedNumber` 변수가 `ViewDidLoad` 메서드에서 ‘클래스 수준 변수’로 이동되었습니다. 
    - `PhoneNumbers.Add(translatedNumber)`를 호출하여 전화를 건 번호를 전화번호 목록에 추가하도록 **CallButton** 코드가 수정되었습니다.
    - `PrepareForSegue` 메서드가 추가되었습니다.

    애플리케이션을 저장하고 빌드하여 오류가 없는지 확인합니다.

18. **시작** 단추를 눌러 **iOS 시뮬레이터** 내에서 애플리케이션을 시작합니다.

    ![](hello-ios-multiscreen-quickstart-images/19.png "Press the Start button to launch the application inside the iOS Simulator")

첫 번째 멀티스크린 Xamarin.iOS 애플리케이션을 완성한 것을 축하합니다!

::: zone-end
::: zone pivot="windows"

## <a name="walkthrough-on-windows"></a>Windows에서 연습

이 연습에서는 **Phoneword** 애플리케이션에 통화 기록 화면을 추가합니다.

1. Visual Studio에서 **Phoneword** 애플리케이션을 엽니다. 필요한 경우 [Hello, iOS 연습](~/ios/get-started/hello-ios/index.md) 가이드에서 [완료된 Phoneword 애플리케이션](https://docs.microsoft.com/samples/xamarin/ios-samples/hello-ios)을 다운로드합니다. iOS 디자이너 및 iOS 시뮬레이터를 사용하도록 [Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)에 연결해야 합니다.

2. 사용자 인터페이스를 편집하여 시작합니다. **다른 이름으로 보기**가 _iPhone 6_으로 설정되었는지 확인하여 **솔루션 탐색기**에서 **Main.storyboard** 파일을 엽니다.

    ![](hello-ios-multiscreen-quickstart-images/image1.png "The Main.storyboard in the iOS Designer")

3. **탐색 컨트롤러**를 **도구 상자**에서 디자인 화면으로 끌어 옵니다.

    ![](hello-ios-multiscreen-quickstart-images/image2.png "Drag a navigation controller from the Toolbox onto the design surface")

4. **원본 없는 Segue**(**Phoneword** 장면의 왼쪽에 있는 회색 화살표)를 **Phoneword** 장면에서 **탐색 컨트롤러**로 끌어와서 애플리케이션의 시작점을 변경합니다.

    ![](hello-ios-multiscreen-quickstart-images/image3.png "Drag the Sourceless Segue to the navigation controller to change the starting point of the application")

5. 검은색 표시줄을 클릭하여 **루트 보기 컨트롤러**를 선택하고 **삭제**를 눌러 디자인 화면에서 제거합니다.
  그런 다음, **Phoneword** 장면을 **탐색 컨트롤러** 옆으로 이동합니다.

    ![](hello-ios-multiscreen-quickstart-images/image4.png "Move the Phoneword scene next to the navigation controller")

6. **ViewController**를 탐색 컨트롤러의 루트 보기 컨트롤러로 설정합니다. **Ctrl** 키를 누르고 **탐색 컨트롤러** 내부를 클릭합니다. 파란색 선이 표시됩니다. 그런 다음, **Ctrl** 키를 누른 상태로 **탐색 컨트롤러**에서 **Phoneword** 장면으로 끌어서 놓습니다. 이는 _Ctrl 끌기_라고 합니다.

    ![](hello-ios-multiscreen-quickstart-images/image5.png "Drag from the navigation controller to the Phoneword scene and release")

7. 팝오버에서 관계를 **루트**로 설정합니다.

    ![](hello-ios-multiscreen-quickstart-images/image6.png "Set the relationship to Root")

    **ViewController**는 이제 **탐색 컨트롤러의 루트 보기 컨트롤러입니다.**

8. **Phoneword** 화면의 **제목** 표시줄을 두 번 클릭하고 **제목**을 **Phoneword**로 변경합니다.

    ![](hello-ios-multiscreen-quickstart-images/image7.png "Change the Title to Phoneword")

9. **도구 상자**에서 **단추**를 끌어서 **호출 단추** 아래에 배치합니다. 핸들을 끌어 새 **단추**가 **호출 단추**와 동일한 너비가 되도록 합니다.

    ![](hello-ios-multiscreen-quickstart-images/image8.png "Make the new Button the same width as the Call Button")

10. **속성 탐색기**에서 **단추**의 **이름**을 `CallHistoryButton`으로 변경하고 **제목**을 **통화 기록**으로 변경합니다.

    ![](hello-ios-multiscreen-quickstart-images/image9.png "Change the Name of the Button to 'CallHistoryButton' and the Title to 'Call History'")

11. **통화 기록** 화면을 만듭니다. **도구 상자**에서 **테이블 보기 컨트롤러**를 디자인 화면으로 끕니다.

    ![](hello-ios-multiscreen-quickstart-images/image10.png "Drag a table view controller onto the design surface")

12. 장면의 아래쪽에 있는 검은색 표시줄을 클릭하여 **테이블 보기 컨트롤러**를 선택합니다. **속성 탐색기**에서 **테이블 보기 컨트롤러**의 클래스를 `CallHistoryController`로 변경하고 **Enter** 키를 누릅니다.

    ![](hello-ios-multiscreen-quickstart-images/image11.png "Change the table view controllers class to CallHistoryController")

    iOS 디자이너는 `CallHistoryController`라는 사용자 지정 백업 클래스를 생성하여 이 화면의 콘텐츠 보기 계층 구조를 관리합니다. **CallHistoryController.cs** 파일은 **솔루션 탐색기**에 표시됩니다.

    ![](hello-ios-multiscreen-quickstart-images/image12.png "The CallHistoryController.cs file in the Solution Explorer")

13. **CallHistoryController.cs** 파일을 두 번 클릭하여 열고 내용을 다음 코드로 바꿉니다.

    ```csharp
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
    ```

    애플리케이션을 저장하고 빌드하여 오류가 없는지 확인합니다. 지금은 빌드 경고를 무시해도 됩니다.

14. **Phoneword** 장면과 **통화 기록** 장면 사이에 _segue_(전환)를 만듭니다.
  **Phoneword 장면**에서 **통화 기록 단추**를 선택하고 **단추**에서 **통화 기록** 장면으로 **Ctrl 끌기**합니다.

    ![](hello-ios-multiscreen-quickstart-images/image13.png "Ctrl-drag from the Button to the Call History scene")

    **작업 Segue** 팝오버에서 **표시**를 선택합니다.

    ![](hello-ios-multiscreen-quickstart-images/image14.png "Select Show as the segue type")

    iOS 디자이너는 두 장면 사이에 segue를 추가할 예정입니다.

    ![](hello-ios-multiscreen-quickstart-images/image15.png "The Segue between the two scenes")

15. 장면 아래쪽의 검은색 표시줄을 선택하고 **속성 컨트롤러**에서 **보기 컨트롤러 > 제목**을 **통화 기록**으로 변경하여 **제목**을 **테이블 보기 컨트롤러**에 추가합니다.

    ![](hello-ios-multiscreen-quickstart-images/image16.png "Change the view controller Title to Call History")

16. 애플리케이션이 실행될 때 **통화 기록 단추**는 **통화 기록** 화면을 열지만, 테이블 보기는 추적을 유지하고 전화번호를 표시할 코드가 없기 때문에 비어 있게 됩니다.

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

          // set the view controller that’s powering the screen we’re
          // transitioning to

          var callHistoryController = segue.DestinationViewController as CallHistoryController;

          //set the table view controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryController != null)
          {
            callHistoryController.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```

    여기서 몇 가지 일이 발생합니다.
    - `translatedNumber` 변수가 `ViewDidLoad` 메서드에서 _클래스 수준 변수_로 이동되었습니다.
    - `PhoneNumbers.Add(translatedNumber)`를 호출하여 전화를 건 번호를 전화번호 목록에 추가하도록 **CallButton** 코드가 수정되었습니다.
    - `PrepareForSegue` 메서드가 추가되었습니다.

    애플리케이션을 저장하고 빌드하여 오류가 없는지 확인합니다.

    애플리케이션을 저장하고 빌드하여 오류가 없는지 확인합니다.

18. **시작** 단추를 눌러 **iOS 시뮬레이터** 내에서 애플리케이션을 시작합니다.

    ![](hello-ios-multiscreen-quickstart-images/19.png "The first screen of the sample app")

첫 번째 멀티스크린 Xamarin.iOS 애플리케이션을 완성한 것을 축하합니다!

::: zone-end

이제 앱은 두 스토리보드 segue를 사용하여 코드에서 탐색을 처리할 수 있습니다. 이제 [Hello, iOS 멀티스크린 심층 분석](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md)에서 알아본 도구 및 기술을 분석할 시간입니다.

## <a name="related-links"></a>관련 링크

- [Hello, iOS(샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/hello-ios)
- [iOS 휴먼 인터페이스 지침](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS 프로비전 포털](https://developer.apple.com/ios/manage/overview/index.action)
