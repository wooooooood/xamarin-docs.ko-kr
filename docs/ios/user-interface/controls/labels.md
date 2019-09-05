---
title: Xamarin.ios의 레이블
description: 이 문서에서는 Xamarin.ios에서 레이블을 사용 하는 방법을 설명 합니다. IOS Designer를 사용 하 여 프로그래밍 방식으로 레이블을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/11/2017
ms.openlocfilehash: 0447bd643f359b21ec58bb8bdd79f8482fdb8955
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286308"
---
# <a name="labels-in-xamarinios"></a>Xamarin.ios의 레이블

`UILabel` 컨트롤은 단일 및 여러 줄 읽기 전용 텍스트를 표시 하는 데 사용 됩니다.

## <a name="implementing-a-label"></a>레이블 구현

새 레이블은를 [`UILabel`](xref:UIKit.UILabel)인스턴스화하여 생성 됩니다.

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>레이블 및 Storyboard

IOS Designer를 사용할 때 UI에 레이블을 추가할 수도 있습니다. **도구 상자** 에서 **레이블을** 검색 하 고 보기로 끌어 옵니다.

![도구 상자의 레이블](labels-images/image3.png)

속성 패드에서 다음 속성을 조정할 수 있습니다.

![레이블 속성 패널](labels-images/image2.png)

- **텍스트 컨텍스트** – 일반 또는 특성을 사용 합니다. 일반 텍스트 전체 문자열의 [서식 특성](#Formatting_Text_and_Label) 을 설정할 수 있습니다. 특성을 사용 하는 텍스트를 사용 하면 문자열의 다른 문자나 단어로 서식을 지정할 수 있습니다.
- **색, 글꼴, 맞춤** – 레이블에 적용할 수 있는 서식 특성입니다.
- **Lines** – 레이블이 확장 될 수 있는 줄 수를 설정 합니다. 레이블이 필요한 만큼의 줄을 사용 하도록 허용 하려면이를 0으로 설정 합니다.
- **동작** – 사용 또는 강조 중 하나로 설정할 수 있습니다. Enabled는 기본적으로 설정 되어 있으며 비활성 텍스트는 밝은 회색 색으로 표시 됩니다. 강조 표시 됨은 기본적으로 사용 하지 않도록 설정 되어 있으며 사용자가 선택할 때 강조 표시 된 상태로 레이블을 다시 그릴 수 있습니다.
- **Baselane 및 줄 바꿈** –
  - 행이 지정 된 크기와 다른 경우 텍스트가 배치 되는 방법을 결정 합니다.
  - 줄 바꿈은 문자열이 한 줄 보다 길면 줄 바꿈되는 방법을 결정 합니다.
- 자동 **축소** – 필요한 경우 레이블 내에서 글꼴 크기를 최소화 하는 방법을 결정 합니다.
- **강조 표시 됨, 그림자, 오프셋** – 강조 표시 메시에 및 그림자 색과 그림자 오프셋을 설정할 수 있습니다.

## <a name="truncating-and-wrapping"></a>잘라내기 및 래핑

IOS에서 줄 바꿈을 사용 하는 방법에 대 한 자세한 내용은 [잘라내기 및 줄 바꿈 텍스트](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text) 를 참조 하세요.

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>텍스트 및 레이블 서식 지정

레이블에 사용 되는 문자열의 형식을 지정 하려면 전체 문자열에 서식 특성을 설정 하거나 특성 사용 문자열을 사용할 수 있습니다. 다음 예제에서는 이러한 구현을 구현 하는 방법을 보여 줍니다.

```csharp
label = new UILabel(){
                Text = "Hello, this is a string",
                Font = UIFont.FromName("Papyrus", 20f),
                TextColor = UIColor.Magenta,
                TextAlignment = UITextAlignment.Center
            };
```

```csharp
label.AttributedText = new NSAttributedString(
                "This is some formatted text",
                font: UIFont.FromName("GillSans", 16.0f),
                foregroundColor: UIColor.Blue,
                backgroundColor: UIColor.White
            );
```

을 사용 하 여 `NSAttributedString` 텍스트 스타일 지정에 대 한 자세한 내용은 [스타일 텍스트](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/text_field/style_text) 조리법을 참조 하세요.

기본적으로 레이블은 `Enabled` true로 설정 되어 있지만 사용자에 게 특정 컨트롤을 사용할 수 없다는 힌트를 제공 하려면 사용 안 함으로 설정할 수 있습니다.

```csharp
label.Enabled = false;
```

이렇게 하면 iOS에서 제한 화면의 다음 예제 이미지에 설명 된 대로 레이블이 연한 회색으로 설정 됩니다.

![IOS의 비활성화 된 단추](labels-images/image1.png)

추가 효과를 위해 강조 표시 및 그림자 텍스트 색을 레이블 텍스트로 설정할 수도 있습니다.

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

다음과 같이 텍스트를 표시 합니다.

![텍스트에 강조 표시 및 그림자 설정](labels-images/image4.png)

UILabel 글꼴을 변경 하는 방법에 대 한 자세한 내용은 글꼴 조리법 [변경](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/change_the_font) 을 참조 하세요.





