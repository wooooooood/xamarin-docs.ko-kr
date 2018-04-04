---
title: 레이블
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 8a5b2c12cfbca18280a8044d3d8c599d848de91d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="labels"></a>레이블

`UILabel` 읽기 전용 텍스트 컨트롤은 단일 행 삽입과 다중 행을 표시 하기 위해 사용 됩니다. 

## <a name="implementing-a-label"></a>레이블을 구현합니다.

새 레이블을 인스턴스화하여 만들어집니다는 [ `UILabel` ](https://developer.xamarin.com/api/type/UIKit.UILabel/):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>레이블 및 스토리 보드

또한 iOS 디자이너를 사용 하는 경우 UI를 레이블을 추가할 수 있습니다. 검색할 **레이블** 에 **도구 상자** 보기로 끕니다.

![도구 상자에 레이블](labels-images/image3.png)

속성 패드에서 다음 속성을 조정할 수 있습니다.

![레이블 속성 패널](labels-images/image2.png)

- **텍스트 컨텍스트** – 일반 또는 특성을 사용 합니다. 일반 텍스트 설정할 수 있습니다는 [서식 특성](#Formatting_Text_and_Label) 전체 문자열에 있습니다. 특성이 지정 된 텍스트를 사용 하면 서로 다른 문자 또는 문자열에 단어 서식을 설정할 수 있습니다.
- **색, 글꼴, 맞춤** – 레이블에 적용할 수 있는 서식 특성입니다.
- **줄** – 레이블을 확장할 수 있는 줄의 수를 설정 합니다. 설정 합니다. 필요에 따라 많은 줄을 사용 하도록 레이블을 허용 하려면 0을 합니다.
- **동작** -Enabled 또는 강조 표시 된 설정할 수 있습니다. 사용 하도록 설정 기본적으로 설정 비활성된 텍스트에에서 표시 됩니다는 밝은 회색입니다. 강조 표시를 사용자가을 선택 하면 강조 표시 된 상태로 다시 그리도록 레이블을 알리고 기본적으로 비활성화 되어 있습니다.
- **Baselane 및 줄 바꿈을** – 
    - 기준선 텍스트 되는 방식을 결정 글꼴 크기는 지정 된 것과에서 다른 경우에 배치 합니다.
    - 줄 바꿈 문자열은 래핑된 또는 한 줄 보다 긴 경우 잘립니다 방법을 결정 합니다.
- **자동 축소** – 필요한 경우 레이블 내에서 글꼴 크기의 됩니다 최소화 하는 방법을 결정 합니다.
- **강조 표시, 그림자 오프셋** – 강조 표시 및 그림자 색을 설정할 수 있도록 하 고 그림자 오프셋입니다.

## <a name="truncating-and-wrapping"></a>잘라내기 및 줄 바꿈

IOS에서 줄을 사용 하는 방법은 중단에 대 한 참조는 [잘라내기 및 텍스트 줄 바꿈](https://developer.xamarin.com/recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text/) 조리법 합니다.

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>텍스트 및 레이블 서식 지정

레이블을 사용 하는 문자열의 서식을 지정 하려면 서식 특성 전체 문자열에 설정 하거나 또는 특성이 지정 된 문자열을 사용할 수 있습니다. 다음 예에서는 이러한 구현 하는 방법을 보여 줍니다.

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

사용 하 여 텍스트 스타일에 대 한 자세한 내용은 `NSAttributedString` 참조는 [스타일 텍스트](https://developer.xamarin.com/recipes/ios/standard_controls/text_field/style_text/) 조리법 합니다.

레이블에 기본적으로는 `Enabled` 하지만 true로 설정으로 설정 하려면 가능한 기능이 비활성화 됩니다 사용자에 게 특정 컨트롤을 사용할 수 없다는 힌트를 제공 합니다.

```csharp
label.Enabled = false;
```

IOS에 제한 사항이 스크린의 다음 예제에서는 이미지에 표시 된 것 처럼에 밝은 회색을 레이블을 설정 합니다.

![IOS의 비활성화 된 단추](labels-images/image1.png)

또한 추가 효과 대 한 레이블 텍스트를 강조 표시 하 고 섀도 텍스트 색을 설정할 수 있습니다.

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

다음과 같이 텍스트를 표시합니다.

![텍스트에 밝은 영역과 어두운 설정](labels-images/image4.png)

UILabel 글꼴 변경에 대 한 자세한 내용은 참조는 [The 글꼴 변경](https://developer.xamarin.com/recipes/ios/standard_controls/labels/change_the_font/) 조리법 합니다.





