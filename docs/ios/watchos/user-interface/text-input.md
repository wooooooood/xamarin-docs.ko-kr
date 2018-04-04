---
title: 텍스트 입력 사용
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 9dec6f754590abf6db8829f555376b423b7a7da7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-text-input"></a>텍스트 입력 사용

Apple Watch 조사식 친화적인 대체할은 지원 되지만 사용자가 텍스트, 입력에 대 한 키보드를 제공 하지 않습니다.

- 텍스트 옵션의 미리 정의 된 목록에서 선택
- Siri 받아쓰기
- emoji 선택
- Scribble (watchOS 3에에서 도입 된) 문자 단위로 필기 인식 합니다.

시뮬레이터 받아쓰기 현재 지원 하지 않지만 여전히를 테스트할 수 있습니다 텍스트 입력 컨트롤러의 등의 다른 옵션 Scribble 다음과 같이:

![](text-input-images/textinput-sml.png "Scribble 옵션 테스트")

입력을 받을 텍스트 watch 앱에서:

1. 미리 정의 된 옵션의 문자열 배열을 만듭니다.
2. 호출 `PresentTextInputController` 배열에서는 or not emoji 있도록 할지 여부 및 `Action` 마쳤을 때 호출 되 합니다.
3. 완료 동작 입력된 결과 대 한 테스트 하 고 (레이블의 텍스트 값으로 설정 가능) 응용 프로그램에서 적절 한 조치 키를 누릅니다.

다음 코드 조각은 사용자에 게 세 가지 미리 정의 된 옵션을 제공합니다.

```csharp
var suggest = new string[] {"Get groceries", "Buy gas", "Post letter"};

PresentTextInputController (suggest, WatchKit.WKTextInputMode.AllowEmoji, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
    // this only works if result is a text response (Plain or AllowEmoji)
        enteredText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (enteredText);
        // do something, such as myLabel.SetText(enteredText);
    }
});
```

`WKTextInputMode` 열거형에는 3 개의 값:

- 일반
- AllowEmoji
- AllowAnimatedEmoji

## <a name="plain"></a>일반

일반 모드가 설정 된 사용자가 선택할 수 있습니다.

- 받아쓰기
- 자유 곡선 또는
- 응용 프로그램이 제공 하는 미리 정의 된 목록에서

[![](text-input-images/plain-scribble-sml.png "Scribble, 받아쓰기 또는 응용 프로그램을 제공 하는 미리 정의 된 목록에서")](text-input-images/plain-scribble.png#lightbox)

결과 항상 서 반환 되는 `NSObject` 으로 캐스팅 될 수는 `string`합니다.

## <a name="emoji"></a>Emoji

Emoji 유형은

- 일반 유니코드 emoji
- 애니메이션된 이미지

유니코드 emoji을 선택 하면 문자열로 반환 됩니다.

애니메이션된 이미지 emoji 선택한 경우에 `result` 완료에서 처리기가 포함 됩니다는 `NSData` 는 emoji 포함 된 개체 `UIImage`합니다.

## <a name="accepting-dictation-only"></a>받아쓰기만 수락

제안 (또는 Scribble 옵션)를 표시 하지 않고 받아쓰기 화면에 직접 사용자를 수행 합니다.

- 제안 목록에 대 한 빈 배열을 전달 하 고
- Set `WatchKit.WKTextInputMode.Plain`.

```csharp
PresentTextInputController (new string[0], WatchKit.WKTextInputMode.Plain, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
        dictatedText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (dictatedText);
        // do something, such as myLabel.SetText(dictatedText);
    }
});
```

사용자를 말할 조사식 화면 (예: "This is a test") 인식 되는 텍스트를 포함 하는 다음 화면이 표시 됩니다.

![](text-input-images/dictation.png "사용자를 말할 조사식 화면 표시 됩니다는 텍스트 것으로 간주 됩니다.")

일단 눌렀을 때의 **수행** 단추 텍스트가 반환 됩니다.



## <a name="related-links"></a>관련 링크

- [Apple의 텍스트 및 레이블 문서](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [watchOS 3 소개](~/ios/watchos/platform/introduction-to-watchos3/index.md)
