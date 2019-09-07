---
title: MacOS의 접근성
description: 이 문서에서는 Xamarin.ios 앱에서 macOS 접근성 기능을 사용 하는 방법을 설명 합니다. 스토리 보드 및 코드, 사용자 지정 컨트롤 및 내게 필요한 옵션 테스트의 UI 요소에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 087dcdc7024026e6a3ed3a05baca3b2648053cc8
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769945"
---
# <a name="accessibility-on-macos"></a>MacOS의 접근성

이 페이지에서는 macOS 접근성 Api를 사용 하 여 [내게 필요한 옵션 검사 목록](~/cross-platform/app-fundamentals/accessibility.md)에 따라 앱을 빌드하는 방법을 설명 합니다.
다른 플랫폼 Api는 [Android 접근성](~/android/app-fundamentals/accessibility.md) 및 [iOS 내게 필요한 옵션](~/ios/app-fundamentals/accessibility.md) 페이지를 참조 하세요.

액세스 가능성 Api가 macOS (이전의 OS X)에서 작동 하는 방식을 이해 하려면 먼저 [Os x 접근성 모델](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html)을 검토 합니다.

## <a name="describing-ui-elements"></a>UI 요소 설명

Appkit는 `NSAccessibility` 프로토콜을 사용 하 여 사용자 인터페이스에 액세스할 수 있도록 돕는 api를 노출 합니다. 여기에는 단추의 `AccessibilityLabel`를 설정 하는 것과 같은 내게 필요한 옵션 속성에 대 한 의미 있는 값을 설정 하 려 하는 기본 동작이 포함 됩니다. 레이블은 일반적으로 컨트롤이 나 뷰를 설명 하는 단일 단어나 짧은 문구입니다.

### <a name="storyboard-files"></a>스토리 보드 파일

Xcode Interface Builder를 사용 하 여 스토리 보드 파일을 편집 합니다.
다음 스크린샷에 표시 된 것 처럼 디자인 화면에서 컨트롤을 선택 하면 **id 검사자** 에서 내게 필요한 옵션 정보를 편집할 수 있습니다.

[![Xcode의 Interface Builder에 내게 필요한 옵션 추가](accessibility-images/xcode.png "Xcode의 Interface Builder에 내게 필요한 옵션 추가")](accessibility-images/xcode-large.png#lightbox)

### <a name="code"></a>코드

Xamarin.ios는 현재 setter로 `AccessibilityLabel` 노출 되지 않습니다.  다음 도우미 메서드를 추가 하 여 액세스 가능성 레이블을 설정 합니다.

```csharp
public static class AccessibilityHelper
{
    [System.Runtime.InteropServices.DllImport (ObjCRuntime.Constants.ObjectiveCLibrary)]
    extern static void objc_msgSend (IntPtr handle, IntPtr selector, IntPtr label);

    static public void SetAccessibilityLabel (this NSView view, string value)
    {
        objc_msgSend (view.Handle, new ObjCRuntime.Selector ("setAccessibilityLabel:").Handle, new NSString (value).Handle);
    }
}
```

그러면 다음과 같이 코드에서이 메서드를 사용할 수 있습니다.

```csharp
AccessibilityHelper.SetAccessibilityLabel (someButton, "New Accessible Description");
```

`AccessibilityHelp` 속성은 컨트롤이 나 뷰가 수행 하는 내용에 대 한 설명을 위한 것 이며 레이블이 충분 한 정보를 제공 하지 않을 경우에만 추가 해야 합니다. 도움말 텍스트는 가능한 한 짧게 유지 되어야 합니다 (예: "문서 삭제").

일부 사용자 인터페이스 요소는 액세스 가능한 액세스와 관련이 없습니다 (예: 고유한 접근성 레이블 및 도움말이 있는 입력 옆에 있는 레이블).
이러한 경우를 설정 하 `AccessibilityElement = false` 여 이러한 컨트롤이 나 뷰를 화면 판독기나 다른 접근성 도구에서 건너뛰도록 설정 합니다.

Apple은 접근성 레이블 및 도움말 텍스트에 대 한 모범 사례를 설명 하는 [내게 필요한 옵션 지침](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html) 을 제공 합니다.

## <a name="custom-controls"></a>사용자 지정 컨트롤

필요한 추가 단계에 대 한 자세한 내용은 [액세스 가능한 사용자 지정 컨트롤에 대 한](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html) Apple의 지침을 참조 하세요.

## <a name="testing-accessibility"></a>액세스 가능성 테스트

macOS는 내게 필요한 옵션 기능을 테스트 하는 데 도움이 되는 **접근성 검사기** 를 제공 합니다. 검사기는 Xcode에 포함 되어 있습니다.

처음 시작 될 때 **내게 필요한 옵션 검사자** 는 내게 필요한 옵션을 통해 컴퓨터를 제어할 수 있는 권한이 필요 합니다.

![실행 권한을 요청 하는 내게 필요한 옵션 검사기](accessibility-images/accessibility-inspector-1.png "실행 권한을 요청 하는 내게 필요한 옵션 검사기")

설정 화면 잠금 해제 (필요한 경우 왼쪽 아래) 및 **내게 필요한 옵션 검사자**의 tick:

![내게 필요한 옵션 검사자를 사용 하도록 설정 화면](accessibility-images/accessibility-inspector-2.png "내게 필요한 옵션 검사자를 사용 하도록 설정 화면")

사용 하도록 설정 되 면 검사기가 화면 주위에서 이동할 수 있는 부동 창으로 나타납니다. 아래 스크린샷에서는 샘플 Mac 앱 옆의 검사기를 실행 하는 방법을 보여 줍니다. 커서가 창 위로 이동 하면 검사기는 각 컨트롤의 액세스 가능한 모든 속성을 표시 합니다.

[![실행 가능 검사기 실행 예](accessibility-images/accessibility-example.png "실행 가능 검사기 실행 예")](accessibility-images/accessibility-example-large.png#lightbox)

자세한 내용은 [OS X에 대 한 액세스 가능성 테스트 가이드](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [플랫폼 간 액세스 가능성](~/cross-platform/app-fundamentals/accessibility.md)
- [Mac 접근성](https://www.apple.com/accessibility/mac/)
