---
title: Xamarin.ios의 레이아웃 옵션
description: 이 문서에서는 Xamarin.ios의 사용자 인터페이스 레이아웃을 설정 하는 다양 한 방법을 설명 합니다. 자동 크기 조정 및 자동 레이아웃에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 2514287fe06216d62b994cf19ba8f0901dd36c49
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73003322"
---
# <a name="layout-options-in-xamarinios"></a>Xamarin.ios의 레이아웃 옵션

뷰의 크기를 조정 하거나 회전할 때 레이아웃을 제어 하는 두 가지 메커니즘이 있습니다.

- **자동 크기 조정** – 디자이너의 자동 크기 조정 inspector는 `AutoresizingMask` 속성을 설정 하는 방법을 제공 합니다. 이렇게 하면 해당 컨테이너의 가장자리에 컨트롤을 고정 하거나 크기를 수정할 수 있습니다. 자동 크기 조정는 모든 버전의 iOS에서 작동 합니다. 자세한 내용은 아래에 자세히 설명 되어 있습니다.
- **Auto Layout** – UI 컨트롤의 관계를 세부적으로 제어할 수 있는 iOS 6에 도입 된 기능입니다. 디자인 화면에서 다른 요소를 기준으로 요소의 위치를 제어할 수 있습니다. 이 항목은 [Xamarin IOS Designer를 사용 하 여 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md) 가이드에 자세히 설명 되어 있습니다.

## <a name="autosizing"></a>자동 크기 조정

장치를 회전 하 고 방향이 변경 되는 경우와 같이 사용자가 창의 크기를 조정 하면 시스템은 자동 크기 조정 규칙에 따라 해당 창 내에서 뷰의 크기를 자동으로 조정 합니다. 이러한 규칙은 아래 그림과 같이 C# iOS 디자이너의 **Properties Pad** 또는`UIView`의`AutoresizingMask`속성을 사용 하 여 설정할 수 있습니다.

 [![](layout-options-images/image41.png "Visual Studio for Mac Designer")](layout-options-images/image41.png#lightbox)

컨트롤을 선택 하면 컨트롤의 위치 및 크기를 수동으로 지정할 수 있을 뿐만 아니라 **자동 크기 조정** 동작을 선택할 수 있습니다. 아래 스크린샷에 표시 된 것 처럼 자동 크기 조정 컨트롤의 스프링 및 struts를 사용 하 여 선택한 뷰의 부모에 대 한 관계를 정의할 수 있습니다.

 [![](layout-options-images/image42.png "Visual Studio for Mac Designer")](layout-options-images/image42.png#lightbox)

*스프링* 을 조정 하면 부모 뷰의 너비나 높이를 기준으로 뷰의 크기가 조정 됩니다. *Strut* 를 조정 하면 뷰가 해당 특정 가장자리에서 자신과 부모 뷰의 일정 한 거리를 유지 합니다.

이러한 설정은 코드에서 설정할 수도 있습니다.

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```

자동 크기 조정 설정을 테스트 하려면 프로젝트의 옵션에서 **지원 되는 다른 장치 방향을** 사용 하도록 설정 합니다.

 [![](layout-options-images/image43a.png "Autosizing Settings")](layout-options-images/image43a.png#lightbox)

코드 뒤에 다음 코드를 사용할 수 있습니다. 이렇게 하면 두 텍스트 컨트롤이 가로로 크기가 조정 됩니다.

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```

디자이너를 사용 하 여 컨트롤을 조정할 수도 있습니다. 아래에 표시 된 대로 struts를 선택 하면 이미지는 보기의 아래쪽에서 잘리지 않고 오른쪽에 맞춰집니다.

 [![](layout-options-images/autoresize.png "Autorotation")](layout-options-images/autoresize.png#lightbox)

이러한 스크린샷은 화면이 회전 될 때 컨트롤의 크기를 조정 하거나 위치를 변경 하는 방법을 보여 줍니다.

 [![](layout-options-images/image44a.png "Autorotation")](layout-options-images/image44a.png#lightbox)

텍스트 뷰와 텍스트 필드는 모두 `FlexibleWidth` 설정으로 인해 왼쪽과 오른쪽 여백이 동일 하 게 유지 됩니다. 이미지의 위쪽과 왼쪽 여백이 유연 합니다. 즉, 화면을 회전할 때 이미지를 보기에 유지 합니다. 일반적으로 복잡 한 레이아웃은 사용자 인터페이스를 일관 되 게 유지 하기 위해 표시 되는 모든 컨트롤에 이러한 설정을 조합 하 여 사용 해야 하며, 회전 또는 기타 크기 조정 이벤트로 인해 뷰의 경계가 변경 될 때 컨트롤이 겹치지 않도록 합니다.

## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
