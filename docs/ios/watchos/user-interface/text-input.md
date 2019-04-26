---
title: WatchOS에서 Xamarin 텍스트 입력을 사용 하 여 작업
description: 이 문서에서는 Xamarin에서 watchOS 텍스트 입력을 설명 합니다. PresentTextInputController 메서드, 낙서, 일반 텍스트,이 모 지를 및 받아쓰기 설명합니다.
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 2092b12254008936f2c5b6a7d9dd610ff751e802
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61207479"
---
# <a name="working-with-watchos-text-input-in-xamarin"></a>WatchOS에서 Xamarin 텍스트 입력을 사용 하 여 작업

하지만 Apple Watch 조사식 친화적인 대체할 지원지 않습니다 텍스트를 입력 하는 사용자에 대 한 키보드를 제공 하지 않습니다.

- 텍스트 옵션을 미리 정의 된 목록에서 선택
- Siri 받아쓰기
- 이 모 지를 선택합니다.
- Scribble 문자 단위로 필기 인식 (watchOS 3에에서 도입 됨).

시뮬레이터 받아쓰기를 현재 지원 하지 않습니다 하지만 여전히를 테스트할 수 있습니다 텍스트 입력을 컨트롤러의 등의 다른 옵션 Scribble 다음과 같이:

![](text-input-images/textinput-sml.png "Scribble 옵션을 테스트합니다.")

Watch 앱에서 텍스트 입력을 받아들일:

1. 미리 정의 된 옵션의 문자열 배열을 만듭니다.
2. 호출 `PresentTextInputController` 배열을 사용 하 여,이 모 지를 허용할 것인지 및 `Action` 사용자 완료 되 면 호출 되는 합니다.
3. 완료 작업에 입력된 하면 테스트 하 고 앱 (가능한 경우 레이블 텍스트 값 설정)에서 적절 한 작업을 수행 합니다.

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

`WKTextInputMode` 열거형에는 세 가지 값:

- 일반
- AllowEmoji
- AllowAnimatedEmoji

## <a name="plain"></a>일반

일반 모드가 설정 된 경우 사용자가 선택할 수 있습니다.

- 받아쓰기
- Scribble, 또는
- 응용 프로그램을 제공 하는 미리 정의 된 목록입니다.

[![](text-input-images/plain-scribble-sml.png "Scribble, 받아쓰기 또는 앱을 제공 하는 미리 정의 된 목록")](text-input-images/plain-scribble.png#lightbox)

결과를 항상 반환 됩니다는 `NSObject` 으로 캐스팅 될 수 있는 한 `string`합니다.

## <a name="emoji"></a>이 모 지

이 모 지는 다음과 같은 두 종류가 있습니다.

- 일반 유니코드가 모 지
- 애니메이션된 이미지

사용자가 유니코드 emoji를 문자열로 반환 됩니다.

애니메이션된 이미지가 모 지를 선택한 경우에 `result` 완료 처리기를 포함 합니다는 `NSData` 는 모 지를 포함 하는 개체 `UIImage`합니다.

## <a name="accepting-dictation-only"></a>받아쓰기만 허용

제안 사항이 있나요 (또는 Scribble 옵션)를 표시 하지 않고 받아쓰기 화면으로 직접 사용자를 수행 합니다.

- 제안 목록에 대 한 빈 배열을 전달 하 고
- 설정 `WatchKit.WKTextInputMode.Plain`합니다.

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

사용자는 말하자면 watch 화면 (예: "This is 테스트") 인식 되는 텍스트를 포함 하는 다음 화면이 표시 됩니다.

![](text-input-images/dictation.png "사용자는 말하자면 watch 화면 표시 됩니다 텍스트 것으로 간주 됩니다.")

이러한 키를 눌러 되 면 합니다 **수행** 단추는 텍스트가 반환 됩니다.



## <a name="related-links"></a>관련 링크

- [Apple의 레이블과 텍스트 문서](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [watchOS 3 소개](~/ios/watchos/platform/introduction-to-watchos3/index.md)
