---
title: Xamarin.iOS에 텍스트 입력
description: 이 문서에서는 Xamarin.iOS 앱에서 텍스트 입력을 설명 합니다. 프로그래밍 방식으로 iOS 디자이너에서에서 UITextField 및 UITextVIew를 사용 하 여 설명 합니다.
ms.prod: xamarin
ms.assetid: 03A7F1DC-017D-4501-91FD-82C78272CDB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 5d8648f5830a7adcd32d253b92fae45098f12a83
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790216"
---
# <a name="text-input-in-xamarinios"></a>Xamarin.iOS에 텍스트 입력

사용자 텍스트 입력을 적용 하 여 수행 됩니다는 `UITextField` 입력과 UITextView 여러 줄 편집 가능한 텍스트를 한 줄에 대 한 합니다. 이러한 컨트롤의 화면으로 끌 수 있으며 초기 텍스트를 설정 하려면 두 번 클릭.

아래 스크린샷과 Mac 용 Visual Studio에서 도구 상자 패드에 있는 이러한 컨트롤에 대 한 아이콘을 보여 줍니다.

 [![](text-input-images/image11a.png "UITextField")](text-input-images/image11a.png#lightbox)

 [![](text-input-images/image13a.png "UITextView")](text-input-images/image13a.png#lightbox)

콘센트 이름이 고 스토리 보드 파일을 저장 한 Mac 용 Visual Studio 업데이트 되 면는 `.designer.cs` partial 클래스 및 사용자 컨트롤 클래스 파일을 참조 하는 C# 코드를 추가할 수 있습니다. 각 컨트롤에는 고유한 속성 및 C# 코드에서 액세스할 수 있는 이벤트 자체에 있습니다.

 <a name="UITextField" />


## <a name="uitextfield"></a>UITextField

`UITextField` 컨트롤은 가장 자주 한 줄의 사용자 이름 또는 암호와 같은 텍스트 입력을 허용 하는 데 사용 됩니다. 일부 컨트롤 사용자 지정에 사용할 수 있는 옵션은 다음과 같습니다.

 [![](text-input-images/image15a.png "UITextField 속성")](text-input-images/image15a.png#lightbox)

이러한 컨트롤은 아래 설명 되어 있습니다.

-  **자리 표시자** –이 선택 사항입니다. 설정 표시 텍스트 필드는 어떤 입력은 필요를 사용자에 게 설명 하기 위해 일반적으로 빈 경우.
-  **지우기 단추** – 사용자가 일반 텍스트를 신속 하 게 하는 방법으로 표준 지우기 단추 ((X)에 있는 회색 원) 텍스트 필드에 표시 하는 시기를 제어 합니다. 영구적으로 숨겨진, 영구적으로 표시 또는 표시 된 필드를 편집 하 고는 여부에 따라 수 있습니다.
-  **최소 글꼴 크기** 및 **크기에 맞게 조정** – 허용 글꼴 크기를 제한 않음 긴 텍스트에 맞게 잘림을 방지를 자동으로 조정 no로 지정된 된 크기 보다 작습니다.
-  **대/소문자가** – 여부를 자동으로 단어, 문장을 또는 모든 입력 대문자로 표시 합니다.
-  **수정** – 맞춤법 검사 및 제안을 사용 여부.
-  **키보드** – 컨트롤이 키보드 스타일에 대 한 입력을 표시 하 고 어떤 키가 키보드에서 사용할 수 있습니다. 숫자 패드, 패드 전화, 전자 메일, URL을 다른 옵션과 함께 포함 합니다.
-  **모양** – 키보드의 모양 및 스타일을 제어 하 고 어느 어두운 되거나 될 밝은 테마입니다.
-  **키 반환** – 더 잘 수행 될 동작을 반영 하도록 Return 키에서 레이블 변경 합니다. 지원 되는 값 Go, 조인, 다음, 경로, 완료 및 검색 합니다.
-  **보안** – 입력 마스크 되어 있는지 여부를 나타냅니다 (예: 암호 입력).


UITextField 메서드를 호출 하면 `textfield1` 에 추가 된 화면 디자이너를 설정 하거나 다음과 같이 C#에서 해당 속성을 변경할 수 있습니다.

```csharp
textfield1.Placeholder = "type email here...";
textfield1.KeyboardType = UIKeyboardType.EmailAddress;
textfield1.ReturnKeyType = UIReturnKeyType.Send;
textfield1.MinimumFontSize = 17f;
textfield1.AdjustsFontSizeToFitWidth = true;
```

Xamarin.iOS 쉽게 등 원하는 설정을 선택할 수 있도록 적절 한 열거형을 제공 된 `UIKeyboardType` 및 `UIReturnKeyType` 위의 코드 조각입니다.

### <a name="display-text-programmatically"></a>프로그래밍 방식으로 텍스트 표시

만들 하 고는 UITextField 프로그래밍 방식으로 표시할 수 동적으로 런타임 시 일부 텍스트를 추가 하려는 경우 또는 디자이너와 사용자의 화면을 디자인 하지 않을 경우는 `ViewDidLoad` 다음과 같이 뷰 컨트롤러의 메서드:

```csharp
var frame = new CGRect(10, 10, 300, 40);
textfield1 = new UITextField(frame);
View.Add(textfield1);
```

 <a name="UITextView" />


## <a name="uitextview"></a>UITextView

`UITextView` 읽기 전용 텍스트를 표시 하거나 여러 줄 텍스트 입력을 수락 하도록 컨트롤을 사용할 수 있습니다. 과 같은 옵션 부분이 많습니다는 `UITextField` (대문자 표시, 수정, 같은 등).

 [![](text-input-images/image16a.png "UITextView 속성")](text-input-images/image16a.png#lightbox)

특정 속성은 다음과 같습니다.

-  **동작** – 편집 가능 또는 읽기 전용 텍스트 인지 여부입니다.
-  **검색** -검색 및 클릭할 수 있는 요소에 입력된 데이터는 호출을 트리거할 수 있는 전화 번호 되는 주소와 같은 맵, Safari에서 여는 Url에 연결 또는 날짜 및 시간을 변환 달력에서 이벤트 됩니다.


UITextView 있는 디자이너 화면에 추가한 경우에 설정 하거나 다음과 같은 속성을 변경할 수 있습니다.

```csharp
textview1.Text = "Lorem ipsum..."; // lots of text can go here
textview1.Editable = true;
textview1.DataDetectorTypes = UIDataDetectorType.PhoneNumber | UIDataDetectorType.Link;
```



## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/Controls/)
