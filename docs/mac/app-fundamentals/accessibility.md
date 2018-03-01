---
title: "MacOS에서 내게 필요한 옵션"
description: "이 가이드에서는 Xamarin.Mac 기반 응용 프로그램을 구축 하기 위한 기능을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 0117364f02302add1f8788de1a79e4c4210fd07b
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="accessibility-on-macos"></a>MacOS에서 내게 필요한 옵션

이 페이지에 따라 앱을 빌드할 macOS 내게 필요한 옵션 Api를 사용 하는 방법에 설명 된 [내게 필요한 옵션 확인 목록](~/cross-platform/app-fundamentals/accessibility.md)합니다.
참조는 [Android 내게 필요한 옵션](~/android/app-fundamentals/accessibility.md) 및 [iOS 내게 필요한 옵션](~/ios/app-fundamentals/accessibility.md) 다른 플랫폼 Api에 대 한 페이지입니다.

작동 방식을 이해 하는 내게 필요한 옵션 Api (OS X 이전의), macOS에서 첫 번째 검토는 [OS X 내게 필요한 옵션 모델](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html)합니다.

## <a name="describing-ui-elements"></a>UI 요소를 설명 하는

사용 하 여 AppKit는 `NSAccessibility` 사용자 인터페이스에 액세스할 수 있도록 하는 데 도움이 되는 Api를 노출 하는 프로토콜입니다. 여기에 단추를 설정 하는 등의 내게 필요한 옵션 속성에 대 한 의미 있는 값을 설정 하려고 시도 하는 기본 동작 `AccessibilityLabel`합니다. 레이블에 일반적으로 한 단어 또는 컨트롤 또는 보기를 설명 하는 짧은 부분입니다.

### <a name="storyboard-files"></a>스토리 보드 파일

Xamarin.Mac는 Xcode 인터페이스 작성기를 사용 하 여 스토리 보드 파일을 편집 합니다.
내게 필요한 옵션 정보를 편집할 수는 **Identity 관리자** 컨트롤을 선택 하면 디자인 화면에서 (같이 아래 스크린샷에서):

[![Xcode의 인터페이스 작성기에서 내게 필요한 옵션 추가](accessibility-images/xcode.png "Xcode의 인터페이스 작성기에서 내게 필요한 옵션 추가")](accessibility-images/xcode-large.png)

### <a name="code"></a>코드

Xamarin.Mac 현재으로 노출 하지 않습니다 `AccessibilityLabel` setter입니다.  내게 필요한 옵션 레이블을 설정 하려면 다음 도우미 메서드를 추가 합니다.

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

표시 된 것 처럼 코드에서이 메서드를 사용한 다음 있습니다.

```csharp
AccessibilityHelper.SetAccessibilityLabel (someButton, "New Accessible Description");
```

`AccessibilityHelp` 속성에 대 한 설명은 컨트롤이 나 뷰에 수행 하는 작업은 레이블을 충분 한 정보를 제공 하지 않을 수 있습니다 때에 추가 해야 합니다. 도움말 텍스트 계속 유지 해야 최대한 단축 "삭제 하는 예제 문서"에 대 한 합니다.

일부 사용자 인터페이스 요소 (예: 고유 내게 필요한 옵션 레이블 및 도움말 되는 입력 옆에 있는 레이블) 액세스할 수 있는 액세스에 대 한 적용 되지 않습니다.
이러한 경우에 설정 `AccessibilityElement = false` 화면 판독기나 기타 내게 필요한 옵션 도구에서 이러한 컨트롤 또는 뷰를 건너뛰도록 합니다.

Apple 제공 [내게 필요한 옵션 지침](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html) 하는 내게 필요한 옵션 레이블 및 도움말 텍스트에 대 한 모범 사례에 설명 합니다.

## <a name="custom-controls"></a>사용자 지정 컨트롤

Apple의를 참조 [액세스할 수 있는 사용자 지정 컨트롤에 대 한 지침이](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html) 필요한 추가 단계에 대 한 자세한 내용은 합니다.

## <a name="testing-accessibility"></a>내게 필요한 옵션 테스트

macOS 제공는 **내게 필요한 옵션 검사기** 하면 내게 필요한 옵션 기능을 테스트 합니다. 검사기는 Xcode 포함 되어 있습니다.

처음으로 시작 되는 **내게 필요한 옵션 검사기** 내게 필요한 옵션을 통해 컴퓨터를 제어할 수 있는 권한이 필요 합니다.

![실행 권한을 요청 하는 내게 필요한 옵션 검사기](accessibility-images/accessibility-inspector-1.png "실행 권한을 요청 하는 내게 필요한 옵션 검사기")

(왼쪽 아래에 필요) 하는 경우 설정 화면 및 눈금을 잠금 해제는 **내게 필요한 옵션 검사기**:

![내게 필요한 옵션 검사기를 사용 하도록 설정 화면](accessibility-images/accessibility-inspector-2.png "내게 필요한 옵션 검사기를 사용 하도록 설정 화면")

활성화 되 면 검사기 화면 이동할 수 있는 부동 창으로 나타납니다. 아래 스크린샷에서 샘플 Mac 응용 프로그램 옆에 있는 실행 중인 검사를 보여 줍니다. 커서를 창 위로 움직일 검사기 각 컨트롤의 액세스 가능한 모든 속성을 표시 합니다.

[![내게 필요한 옵션 관리자 실행의 예제](accessibility-images/accessibility-example.png "내게 필요한 옵션 검사기 예제 실행")](accessibility-images/accessibility-example-large.png)

자세한 내용은 참조는 [OS X 가이드에 대 한 내게 필요한 옵션 테스트](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html)합니다.



## <a name="related-links"></a>관련 링크

- [플랫폼 간 내게 필요한 옵션](~/cross-platform/app-fundamentals/accessibility.md)
- [Mac의 내게 필요한 옵션](https://www.apple.com/accessibility/mac/)
