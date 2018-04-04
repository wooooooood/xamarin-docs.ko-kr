---
title: 레이아웃 옵션
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 8f197bbffeabb708769c48f0130aa27a86b14386
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="layout-options"></a>레이아웃 옵션

보기를 조정 하거나 회전할 때 레이아웃을 제어 하기 위한 두 가지 다른 메커니즘 가지가 있습니다.

-  **크기 자동 조정** –의 크기 자동 조정 검사기는 디자이너에서 설정 하는 방법을 제공는 `AutoresizingMask` 속성입니다. 이 해당 컨테이너의 가장자리에 고정 및/또는 크기를 수정 하는 컨트롤을 수 있습니다. 크기 자동 조정 iOS의 모든 버전에서 작동합니다. 이 내용이 아래에서 자세히 설명 되어
-  **자동 레이아웃** – iOS6 UI 컨트롤의 관계를 보다 세부적으로 제어 하는에 도입 된 기능을 합니다. 디자인 화면에 컨트롤의 다른 요소를 기준으로 요소의 위치는 허용 됩니다. 이 항목에서 자세히 설명 되는 [Xamarin ios 디자이너 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md) 가이드입니다.


## <a name="autosizing"></a>크기 자동 조정

사용자가 장치를 회전 하 고 방향 변경 등의 창 크기를 조정할 때 시스템 크기 자동 조정 규칙에 따라 해당 창 내의 보기를 자동으로 조정 됩니다. 이러한 규칙을 설정할 수 있습니다 C# 사용 하 여는 `AutoresizingMask` 의 속성은 `UIView` 또는 **속성 패드** ios 아래 그림과 같이 디자이너:

 [![](layout-options-images/image41.png "Mac 디자이너에 대 한 visual Studio")](layout-options-images/image41.png#lightbox)

이 수동으로 컨트롤의 크기와 위치를 지정할 수 있습니다는 컨트롤을 선택 하면 선택 뿐만 아니라 **크기 자동 조정** 동작 합니다. 아래 스크린샷에 표시 된 것과 같이 사용할 수 스프링 및 struts 크기 자동 조정 컨트롤에서의 부모에 선택한 보기의 관계를 정의 하려면:

 [![](layout-options-images/image42.png "Mac 디자이너에 대 한 visual Studio")](layout-options-images/image42.png#lightbox)

조정 된 *스프링* 너비 또는 해당 부모 보기의 높이에 따라 크기를 조정 하려면 보기 발생 합니다. 조정 된 *strut* 자체와 해당 부모 뷰의 해당 특정 가장자리 사이의 일정 한 간격으로 유지 관리 보기를 확인 합니다.

이러한 설정은 코드에서 설정할 수도 있습니다.

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```


크기 자동 조정 설정을 테스트 하려면 다른을 사용 하도록 설정 **장치 방향을 지원** 프로젝트의 옵션에서:

 [![](layout-options-images/image43a.png "크기 자동 조정 설정")](layout-options-images/image43a.png#lightbox)

코드 숨김에 두 개의 텍스트 컨트롤이 가로 크기를 조정 하는 다음 코드를 사용할 수 있습니다.

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```


디자이너를 사용 하 여 컨트롤을 조정할 수도 있습니다. 아래 표시 된 struts 선택 하면 보기의 아래쪽 잘리지 않고 오른쪽 맞춤 하 게 유지 하려면 이미지를 발생 합니다.

 [![](layout-options-images/autoresize.png "자동 순환은 꺼짐으로 되어")](layout-options-images/autoresize.png#lightbox)

이러한 스크린샷 컨트롤 크기를 조정 하와 화면 회전할 때 위치가 자동으로 조정 하는 방법을 보여 줍니다.

 [![](layout-options-images/image44a.png "자동 순환은 꺼짐으로 되어")](layout-options-images/image44a.png#lightbox)

텍스트 보기 및 텍스트 필드 모두 왼쪽 동일 하 게 유지할 스트레치 및 오른쪽 여백을로 인해는 `FlexibleWidth` 설정 합니다. 이미지에 위쪽 및 왼쪽 여백을 유연 하 고, 아래쪽 및 오른쪽 여백이 – 화면이 회전할 때 이미지를 표시 유지 유지 하는 것을 의미 합니다. 일반적으로 복잡 한 레이아웃에서 이러한 설정 표시 되는 모든 컨트롤이 사용자 인터페이스의 일관성을 유지 하 고 컨트롤 (회전 또는 이벤트로 인해 다른 크기 조정) 보기의 범위 변경 될 때 겹치지 않도록 방지 하기 위해 함께 사용 하도록 요구 합니다.





## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/Controls/)
