---
title: Xamarin에서 watchOS 텍스트 입력 작업
description: 이 문서에서는 Xamarin에서 watchOS 텍스트 입력에 대해 설명 합니다. PresentTextInputController 메서드, scribbling, 일반 텍스트, emojis 및 받아쓰기에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 156a31e37d14ce3e3cbe7173ae97b608e9d4c32e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032657"
---
# <a name="working-with-watchos-text-input-in-xamarin"></a>Xamarin에서 watchOS 텍스트 입력 작업

Apple Watch는 사용자가 텍스트를 입력할 수 있는 키보드를 제공 하지 않지만 다음과 같은 몇 가지 조사식의 대안을 지원 합니다.

- 미리 정의 된 텍스트 옵션 목록에서 선택
- Siri 받아쓰기,
- 이 모 지 선택,
- WatchOS 3에 도입 된 문자 단위로 문자를 사용한 필기 인식

시뮬레이터는 현재 받아쓰기를 지원 하지 않지만 다음과 같이 텍스트 입력 컨트롤러의 다른 옵션 (예: Scribble)을 테스트할 수 있습니다.

![](text-input-images/textinput-sml.png "Testing the scribble option")

Watch 앱에서 텍스트 입력을 수락 하려면:

1. 미리 정의 된 옵션의 문자열 배열을 만듭니다.
2. 배열을 사용 하 여 `PresentTextInputController`를 호출 하 고,이를 허용 하지 않을 지 여부를 지정 하 고, 사용자가 완료 될 때 호출 되는 `Action`를 호출 합니다.
3. 완료 작업에서 입력 결과를 테스트 하 고 응용 프로그램에서 적절 한 작업을 수행 합니다 (레이블의 텍스트 값을 설정할 수 있음).

다음 코드 조각에서는 미리 정의 된 세 가지 옵션을 사용자에 게 제공 합니다.

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

`WKTextInputMode` 열거형에는 세 개의 값이 있습니다.

- 제목
- AllowEmoji 모 지
- AllowAnimatedEmoji

## <a name="plain"></a>제목

일반 모드를 설정 하면 사용자는 다음을 선택할 수 있습니다.

- 받아쓰기
- Scribble 또는
- 응용 프로그램에서 제공 하는 미리 정의 된 목록에서

[![](text-input-images/plain-scribble-sml.png "Dictation, Scribble, or from a pre-defined list that the app supplies")](text-input-images/plain-scribble.png#lightbox)

결과는 항상 `string`으로 캐스팅할 수 있는 `NSObject`로 반환 됩니다.

## <a name="emoji"></a>이 모 지

다음과 같은 두 가지 유형이 있습니다.

- 일반 유니코드이 모 지
- 애니메이션 이미지

사용자가 유니코드를 선택 하면 문자열로 반환 됩니다.

애니메이션이 적용 된 이미지를 선택할 경우 완료 처리기의 `result`에는이 모 지 `UIImage`포함 된 `NSData` 개체가 포함 됩니다.

## <a name="accepting-dictation-only"></a>받아쓰기만 허용

제안을 표시 하지 않고 사용자를 받아쓰기 화면으로 직접 이동 하는 경우 (또는 Scribble 옵션):

- 제안 목록에 빈 배열을 전달 합니다.
- `WatchKit.WKTextInputMode.Plain`설정 합니다.

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

사용자가 이야기할 때 조사식 화면에는 인식할 수 있는 텍스트 (예: "This is a test")가 포함 된 다음 화면이 표시 됩니다.

![](text-input-images/dictation.png "When the user is speaking, the watch screen displays the text as it is understood")

**완료** 단추를 누르면 텍스트가 반환 됩니다.

## <a name="related-links"></a>관련 링크

- [Apple의 텍스트 및 레이블 문서](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [watchOS 3 소개](~/ios/watchos/platform/introduction-to-watchos3/index.md)
