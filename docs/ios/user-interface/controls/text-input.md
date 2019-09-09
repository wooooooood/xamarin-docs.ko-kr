---
title: Xamarin.ios의 텍스트 입력
description: 이 문서에서는 Xamarin.ios 앱의 텍스트 입력에 대해 설명 합니다. UITextField 및 Uitextfield를 프로그래밍 방식으로 iOS 디자이너에서 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 03A7F1DC-017D-4501-91FD-82C78272CDB1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 8f47ebdd8c1ba220229c6e652af99e8fa3ae2960
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768812"
---
# <a name="text-input-in-xamarinios"></a>Xamarin.ios의 텍스트 입력

사용자 텍스트 수락은 여러 줄의 편집 `UITextField` 가능 텍스트에 대 한 한 줄 입력 및 uitextview에 대해를 사용 하 여 수행 됩니다. 이러한 컨트롤 중 하나를 화면으로 끌어 온 다음 두 번 클릭 하 여 초기 텍스트를 설정할 수 있습니다.

아래 스크린샷에는 Mac용 Visual Studio의 도구 상자 패드에 있는 이러한 컨트롤의 아이콘이 표시 됩니다.

 [![](text-input-images/image11a.png "UITextField")](text-input-images/image11a.png#lightbox)

 [![](text-input-images/image13a.png "UITextView")](text-input-images/image13a.png#lightbox)

콘센트의 이름을 지정 하 고 스토리 보드 파일을 저장 한 후에는 Mac용 Visual Studio `.designer.cs` partial 클래스를 업데이트 하 고 C# 컨트롤을 참조 하는 코드를 클래스 파일에 추가할 수 있습니다. 각 컨트롤에는 C# 코드에서 액세스할 수 있는 고유한 속성 및 이벤트가 있습니다.

 <a name="UITextField" />

## <a name="uitextfield"></a>UITextField

컨트롤 `UITextField` 은 사용자 이름 또는 암호와 같은 한 줄의 텍스트 입력을 허용 하는 데 주로 사용 됩니다. 컨트롤을 사용자 지정 하는 데 사용할 수 있는 몇 가지 옵션이 여기에 표시 됩니다.

 [![](text-input-images/image15a.png "UITextField 속성")](text-input-images/image15a.png#lightbox)

이러한 컨트롤은 다음에 설명 되어 있습니다.

- **Placeholder** – 선택 사항입니다. 설정 하는 경우 텍스트 필드가 비어 있을 때 표시 되며, 일반적으로 사용자에 게 예상 되는 입력을 설명 합니다.
- **지우기 단추** – 사용자가 텍스트를 신속 하 게 지울 수 있는 방법으로 표준 지우기 단추 (X)가 있는 회색 원 (X)이 텍스트 필드에 표시 되는 경우이를 제어 합니다. 필드를 편집 하 고 있는지 여부에 따라 영구적으로 숨겨지거나 영구적으로 표시 하거나 표시할 수 있습니다.
- **최소 글꼴 크기** 및 **맞춤으로 조정** – 글꼴 크기를 자동으로 조정 하 여 긴 텍스트에 맞추고 잘림 방지 하 고 지정 된 크기 보다 작게 제한할 수 있습니다.
- **대문자화** – 단어, 문장 또는 모든 입력을 자동으로 대문자로 표시할지 여부입니다.
- **수정** – 맞춤법 검사 및 제안을 사용 하는지 여부를 나타냅니다.
- **키보드** -입력에 대해 표시 되는 키보드 스타일을 제어 하므로 키보드에서 사용할 수 있는 키가 있습니다. 여기에는 숫자 패드, 휴대폰 패드, 전자 메일, URL 및 기타 옵션이 포함 됩니다.
- **모양** – 키보드의 모양 스타일을 제어 하며 어둡거나 밝은 테마가 적용 됩니다.
- **키 반환** -반환 키의 레이블을 변경 하 여 수행할 작업을 보다 잘 반영 합니다. 지원 되는 값에는 Go, Join, Next, Route, Done 및 Search가 있습니다.
- **Secure** – 입력이 마스킹 되는지 여부를 식별 합니다 (예: 암호 입력).

라는 `textfield1` uitextfield가 디자이너를 사용 하 여 화면에 추가 된 경우에서 C# 다음과 같이 해당 속성을 설정 하거나 변경할 수 있습니다.

```csharp
textfield1.Placeholder = "type email here...";
textfield1.KeyboardType = UIKeyboardType.EmailAddress;
textfield1.ReturnKeyType = UIReturnKeyType.Send;
textfield1.MinimumFontSize = 17f;
textfield1.AdjustsFontSizeToFitWidth = true;
```

Xamarin.ios는 위의 코드 조각에서 `UIKeyboardType` 와 `UIReturnKeyType` 같이 원하는 설정을 쉽게 선택할 수 있도록 적절 한 열거형을 제공 합니다.

### <a name="display-text-programmatically"></a>프로그래밍 방식으로 텍스트 표시

디자이너를 사용 하 여 화면을 디자인 하지 않거나 런타임에 텍스트를 동적으로 추가 하려는 경우 다음과 같이 뷰 컨트롤러의 `ViewDidLoad` 메서드에서 프로그래밍 방식으로 uitextfield를 만들고 표시할 수 있습니다.

```csharp
var frame = new CGRect(10, 10, 300, 40);
textfield1 = new UITextField(frame);
View.Add(textfield1);
```

 <a name="UITextView" />

## <a name="uitextview"></a>UITextView

컨트롤 `UITextView` 은 읽기 전용 텍스트를 표시 하거나 여러 줄 텍스트 입력을 허용 하는 데 사용할 수 있습니다. 에는 `UITextField` (대문자 표시, 수정 등)와 같은 많은 옵션이 있습니다.

 [![](text-input-images/image16a.png "UITextView 속성")](text-input-images/image16a.png#lightbox)

특정 속성은 다음과 같습니다.

- **Behavior** – 텍스트를 편집 가능 또는 읽기 전용인 지 여부를 지정 합니다.
- **검색** – 입력 데이터를 검색 하 고 호출을 트리거할 수 있는 전화 번호, 지도에 링크 되는 주소, Safari에서 연 Url, 캘린더의 이벤트가 되는 날짜 및 시간 등의 클릭 가능한 요소로 변환 합니다.

디자이너를 사용 하 여 UITextView를 화면에 추가한 경우 다음과 같이 해당 속성을 설정 하거나 변경할 수 있습니다.

```csharp
textview1.Text = "Lorem ipsum..."; // lots of text can go here
textview1.Editable = true;
textview1.DataDetectorTypes = UIDataDetectorType.PhoneNumber | UIDataDetectorType.Link;
```

## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
