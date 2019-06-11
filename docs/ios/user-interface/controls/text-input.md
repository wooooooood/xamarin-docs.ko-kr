---
title: Xamarin.iOS에서 텍스트 입력
description: 이 문서에서는 Xamarin.iOS 앱에 대 한 텍스트 입력을 설명 합니다. 프로그래밍 방식으로 iOS 디자이너에서에서 UITextField 및 UITextVIew를 사용 하 여 설명 합니다.
ms.prod: xamarin
ms.assetid: 03A7F1DC-017D-4501-91FD-82C78272CDB1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: a4d23984d4fcfd0776fd6b3537d5a257f70e7837
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827119"
---
# <a name="text-input-in-xamarinios"></a>Xamarin.iOS에서 텍스트 입력

사용자 텍스트 입력을 적용 하 여 수행 됩니다는 `UITextField` 줄으로 입력 하 고 여러 줄 편집 가능한 텍스트 UITextView에 대 한 합니다. 이러한 컨트롤의 화면으로 끌 수 있으며 초기 텍스트를 설정 하려면 두 번 클릭 수 있습니다.

아래 스크린샷은 Mac 용 Visual Studio에서 도구 상자 패드에 있는 이러한 컨트롤에 대 한 아이콘을 보여 줍니다.

 [![](text-input-images/image11a.png "UITextField")](text-input-images/image11a.png#lightbox)

 [![](text-input-images/image13a.png "UITextView")](text-input-images/image13a.png#lightbox)

콘센트를 명명 된 후 스토리 보드 파일을 저장 한 Mac 용 Visual Studio는 업데이트를 `.designer.cs` partial 클래스 추가할 수 있습니다 C# 컨트롤 클래스 파일을 참조 하는 코드입니다. 각 컨트롤에는 자체 고유 속성 및 이벤트에 액세스할 수 있는 사용자 C# 코드입니다.

 <a name="UITextField" />


## <a name="uitextfield"></a>UITextField

`UITextField` 컨트롤은 한 줄의 사용자 이름 또는 암호와 같은 텍스트 입력을 허용 하도록 가장 많이 사용 됩니다. 컨트롤 사용자 지정에 대 한 사용 가능한 옵션 중 일부는 다음과 같습니다.

 [![](text-input-images/image15a.png "UITextField 속성")](text-input-images/image15a.png#lightbox)

이러한 컨트롤은 아래 설명 되어 있습니다.

-  **자리 표시자** – 선택 사항입니다. 설정 표시 됩니다 비어 있을 때 텍스트 필드에 입력 될 사용자에 게 설명 하는 일반적으로 합니다.
-  **지우기 단추** – 표준 지우기 단추 ((X)를 사용 하 여 회색 원) 텍스트 필드에 텍스트를 신속 하 게 지우려면 사용자에 대 한 방법으로 나타나면이 제어 합니다. 영구적으로 숨겨진, 영구적으로 표시 또는 표시 된 필드를 편집 하 고 여부에 따라 수 있습니다.
-  **최소 글꼴 크기** 및 **크기에 맞게 조정** – 허용 긴 텍스트에 맞게 잘림을 방지를 자동으로 조정할 수 있지만 제한 된 글꼴 크기를 no로 지정된 된 크기 보다 작습니다.
-  **대/소문자** – 여부를 자동으로 단어, 문장 또는 모든 입력을 활용 합니다.
-  **수정** – 맞춤법 검사 및 제안 활성화 되었는지 여부를 선택 합니다.
-  **키보드** – 입력에 대 한 컨트롤에 키보드 스타일이 표시 되며 따라서 키 키보드에서 사용할 수 있습니다. 다른 옵션과 함께 URL, 전자 메일, 전화 패드, 숫자 패드를 포함 합니다.
-  **모양을** – 키보드의 모양 및 스타일을 제어 하 고 두 어둡게 또는 될 밝은 테마를 지정 합니다.
-  **키 반환** – 더 잘 수행 될 작업을 확인을 반영 하도록 Return 키에서 레이블을 변경 합니다. 지원 되는 값 이동 "," 조인 "," 다음 "," 경로 완료 "및" 검색을 포함 합니다.
-  **보안** – 입력 마스킹 되었는지 여부를 식별 (예: 암호 입력).


UITextField를 호출 하는 경우 `textfield1` 에 추가 된 화면 디자이너를 사용 하 여 설정 하거나에서 해당 속성을 변경할 수 있습니다 C# 다음과 같습니다.

```csharp
textfield1.Placeholder = "type email here...";
textfield1.KeyboardType = UIKeyboardType.EmailAddress;
textfield1.ReturnKeyType = UIReturnKeyType.Send;
textfield1.MinimumFontSize = 17f;
textfield1.AdjustsFontSizeToFitWidth = true;
```

Xamarin.iOS와 같은 원하는 설정을 선택할 수 있도록 하기 적절 한 열거형을 제공 합니다 `UIKeyboardType` 및 `UIReturnKeyType` 위의 코드 조각입니다.

### <a name="display-text-programmatically"></a>프로그래밍 방식으로 텍스트 표시

디자이너를 사용 하 여 화면을 디자인 하지 않으려는 하거나 동적으로 런타임 시 일부 텍스트를 추가 하려는 경우 만들기를 프로그래밍 방식으로 UITextField 표시 하는 경우는 `ViewDidLoad` 다음과 같은 뷰 컨트롤러의 메서드:

```csharp
var frame = new CGRect(10, 10, 300, 40);
textfield1 = new UITextField(frame);
View.Add(textfield1);
```

 <a name="UITextView" />


## <a name="uitextview"></a>UITextView

`UITextView` 를 읽기 전용 텍스트를 표시 하거나 여러 줄 텍스트 입력을 허용 하도록 컨트롤을 사용할 수 있습니다. 다양 한 동일한 옵션에는 `UITextField` (대문자로 표시, 수정 등).

 [![](text-input-images/image16a.png "UITextView 속성")](text-input-images/image16a.png#lightbox)

특정 속성은 다음과 같습니다.

-  **동작** – 텍스트를 편집 가능 또는 읽기 전용 여부를 선택 합니다.
-  **검색** – 달력의 있게 이벤트 검색 및 변환 입력된 데이터를 클릭할 수 있는 요소 호출을 트리거할 수 있는 전화 번호는 주소와 같은 맵, Safari에서 열리는 Url에 연결 또는 날짜 및 시간입니다.


UITextView 디자이너를 사용 하 여 화면에 추가 된 경우 설정 하거나 다음과 같은 속성을 변경할 수 있습니다.

```csharp
textview1.Text = "Lorem ipsum..."; // lots of text can go here
textview1.Editable = true;
textview1.DataDetectorTypes = UIDataDetectorType.PhoneNumber | UIDataDetectorType.Link;
```



## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/monotouch/Controls/)
