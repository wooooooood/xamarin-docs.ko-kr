---
title: Xamarin.iOS의 레이블 지정
description: 이 문서에서는 Xamarin.iOS에서 레이블을 사용 하는 방법을 설명 합니다. 프로그래밍 방식으로 및 iOS 디자이너를 사용 하 여 레이블을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2017
ms.openlocfilehash: cca74ac74e5077822193f6dd97a69f8d9b823561
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233291"
---
# <a name="labels-in-xamarinios"></a>Xamarin.iOS의 레이블 지정

`UILabel` 읽기 전용 텍스트를 단일 및 여러 줄을 표시 하기 위한 컨트롤을 사용 합니다. 

## <a name="implementing-a-label"></a>레이블을 구현

새 레이블을 인스턴스화하여 생성 되는 [ `UILabel` ](xref:UIKit.UILabel):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>레이블 및 스토리 보드

또한 iOS 디자이너를 사용 하는 경우 ui 레이블을 추가할 수 있습니다. 검색 **레이블** 에 **도구 상자** 보기로 끕니다.

![도구 상자에 레이블](labels-images/image3.png)

Properties pad에서 다음 속성을 조정할 수 있습니다.

![레이블 속성 패널](labels-images/image2.png)

- **텍스트 컨텍스트** – 일반 또는 특성을 사용 합니다. 일반 텍스트를 사용 하면 설정할 수 있습니다 합니다 [서식 특성](#Formatting_Text_and_Label) 전체 문자열에서. 특성이 지정 된 텍스트를 사용 하면 서로 다른 문자 또는 문자열에서 단어에 서식을 설정할 수 있습니다.
- **색, 글꼴, 맞춤** – 레이블에 적용할 수 있는 서식 지정 특성입니다.
- **줄** – 레이블을 걸쳐 있을 수 있는 줄의 수를 설정 합니다. 이 설정에 필요한 만큼의 줄을 사용 하도록 레이블의 허용 하려면 0입니다.
- **동작** -Enabled 또는 강조 표시 된 설정할 수 있습니다. 사용은 기본값으로 설정 비활성화 된 텍스트에에서 표시할 밝은 회색 색입니다. 강조 표시를 사용 하면 사용자가 선택 될 때 강조 표시 된 상태로 다시 그려져 야 레이블 및 기본적으로 비활성화 됩니다.
- **Baselane 및 줄 바꿈을** – 
    - 기준선 하는 방법 결정 텍스트는 글꼴 크기를 지정 하는 것과 다른 경우를 배치 합니다.
    - 줄 바꿈 문자열은 래핑된 또는 한 줄 보다 긴 경우 문자열이 잘릴 하는 방법을 확인 합니다.
- **Autoshrink** – 필요한 경우 레이블 내에서 글꼴 크기는 최소화 하는 방법을 결정 합니다.
- **강조 표시, 그림자 오프셋** – 프로그램 및 그림자 색을 설정할 수 있습니다 하 고 그림자 오프셋입니다.

## <a name="truncating-and-wrapping"></a>잘라내기 및 줄 바꿈

IOS에서 줄을 사용 하는 방법은 중단에 대 한 참조를 [잘라내기 및 텍스트 줄 바꿈](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text) 레시피입니다.

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>텍스트 및 레이블 서식 지정

레이블을 사용 하는 문자열의 서식을 지정 하려면 서식 특성의 전체 문자열을 설정 하거나 또는 특성이 지정 된 문자열을 사용할 수 있습니다. 다음 예제에서는이 구현 하는 방법을 보여 줍니다.

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

스타일 지정 텍스트를 사용 하 여 대 한 자세한 내용은 `NSAttributedString` 를 참조 합니다 [스타일 텍스트](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/text_field/style_text) 작성법.

레이블에 기본적으로는 `Enabled` 설정을 true로 설정 하는 가능한 비활성화 됩니다 사용자에 게 특정 컨트롤은 사용 되지 않음을 힌트를 제공 합니다.

```csharp
label.Enabled = false;
```

이 iOS의 제한 사항 화면의 다음 예제 이미지에 설명 된 대로 밝은 회색을 레이블을 설정 합니다.

![IOS의 비활성화 된 단추](labels-images/image1.png)

또한 추가 효과 대 한 레이블 텍스트를 강조 표시 하 고 섀도 텍스트 색을 설정할 수 있습니다.

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

다음과 같은 텍스트를 표시합니다.

![강조 표시 하 고 섀도 텍스트 설정](labels-images/image4.png)

UILabel 글꼴 변경에 대 한 자세한 내용은 참조는 [The 글꼴 변경](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/change_the_font) 레시피입니다.





