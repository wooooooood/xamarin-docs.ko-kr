---
title: Xamarin.iOS에서 레이아웃 옵션
description: 이 문서에는 Xamarin.iOS에서 사용자 인터페이스의 레이아웃에 다양 한 방법을 설명 합니다. 자동 크기 조정 및 자동 레이아웃에 설명 합니다.
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: b35149028763691c17fe526673d023cc9b707c28
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116656"
---
# <a name="layout-options-in-xamarinios"></a>Xamarin.iOS에서 레이아웃 옵션

보기 크기를 조정 하거나 회전 하는 경우 레이아웃을 제어 하기 위한 두 가지 메커니즘 가지가 있습니다.

-  **자동 크기 조정** – 디자이너에서의 자동 크기 조정 관리자를 설정 하는 방법을 제공 합니다 `AutoresizingMask` 속성입니다. 이렇게 하면 컨트롤이 해당 컨테이너의 가장자리에 앵커 및/또는 크기를 수정 합니다. IOS의 모든 버전에서 작동 하는 자동 크기 조정 합니다. 이 아래에 자세히 설명
-  **자동 레이아웃** -UI 컨트롤의 관계를 세부적으로 제어할 수 있도록 iOS 6에서에서 도입 된 기능을 합니다. 디자인 화면에서 다른 요소를 기준으로 요소의 위치를 제어할을 수 있습니다. 이 항목에서 자세히 설명 합니다 [Xamarin iOS 디자이너를 사용 하 여 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md) 가이드입니다.

## <a name="autosizing"></a>자동 크기 조정

사용자 장치를 회전 하는 경우 및 방향 변경 등의 창 크기가 조정 될 때 시스템 자동 크기 조정 규칙에 따라 해당 창 내에서 뷰를 자동으로 조정 됩니다. 이러한 규칙을 설정할 수 있습니다 C# 를 사용 하 여는 `AutoresizingMask` 의 속성을 `UIView` 또는 **Properties Pad** ios 디자이너, 아래 그림과 같이:

 [![](layout-options-images/image41.png "Visual Studio for Mac 디자이너")](layout-options-images/image41.png#lightbox)

이 수동으로 컨트롤의 크기와 위치를 지정할 수 있습니다 하는 컨트롤을 선택 하면 선택 뿐만 아니라 **자동 크기 조정** 동작 합니다. 아래 스크린샷에 표시 된 것과 같이에 사용 하는 스프링 및 struts 자동 크기 조정 컨트롤의 부모에 선택한 뷰에서 관계를 정의 하려면:

 [![](layout-options-images/image42.png "Visual Studio for Mac 디자이너")](layout-options-images/image42.png#lightbox)

조정 된 *spring* 하면 크기 조정에 뷰를 너비 또는 높이의 부모 보기에 따라 합니다. 조정 된 *strut* 뷰 자체와 해당 부모 뷰의 해당 특정에 지 간에 일정 한 간격으로 유지 관리 하 게 합니다.

이러한 설정은 코드에서 설정할 수도 있습니다.

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```


자동 크기 조정 설정을 테스트 하려면 다른을 사용 하도록 설정 **지원 되는 장치 방향** 프로젝트의 옵션에서:

 [![](layout-options-images/image43a.png "자동 크기 조정 설정")](layout-options-images/image43a.png#lightbox)

코드 숨김에 두 텍스트 컨트롤이 가로 크기를 조정 하려면 다음 코드를 사용 수 있습니다.

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```


또한 컨트롤 디자이너를 사용 하 여 조정할 수 있습니다. 아래 표시 된 struts 선택 하면 뷰의 아래쪽 잘리지 않고 오른쪽 맞춤을 유지 하려면 이미지:

 [![](layout-options-images/autoresize.png "자동 순환은 꺼짐으로 되어")](layout-options-images/autoresize.png#lightbox)

다음이 스크린샷에서 컨트롤의 크기를 조정 하거나 화면을 회전 하는 경우 자체 위치를 변경 하는 방법을 보여 줍니다.

 [![](layout-options-images/image44a.png "자동 순환은 꺼짐으로 되어")](layout-options-images/image44a.png#lightbox)

텍스트 뷰 및 텍스트 필드를 모두 왼쪽 동일 하 게 유지할 확장 하 고 오른쪽 여백으로 인해는 `FlexibleWidth` 설정 합니다. 이미지에 위쪽 및 왼쪽 여백을 유연한, 아래쪽 및 오른쪽 여백이 – 화면이 회전 되었을 때 이미지를 표시 유지 유지 하는 것을 의미 합니다. 복잡 한 레이아웃에는 일반적으로 이러한 설정은 모든 표시 컨트롤 사용자 인터페이스의 일관성을 유지 하 고 컨트롤 뷰의 범위 (회전 또는 다른 크기 조정 이벤트)로 인해 변경 될 때 겹침을 방지 하기 위해의 조합이 필요 합니다.





## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/Controls/)
