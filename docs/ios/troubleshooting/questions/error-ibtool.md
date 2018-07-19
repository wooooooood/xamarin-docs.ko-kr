---
title: 'IBTool 오류: 작업을 완료할 수 없습니다.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 4647227ad208bfa968f8282a966220a09ab7f4a6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30777153"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>IBTool 오류: 작업을 완료할 수 없습니다.

## <a name="fixed-in-xcode-611"></a>6.1.1 Xcode에서 수정

Apple [고정](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) 이 `ibtool` Xcode 6.1.1에 따라서 Xcode 6.1.1 업그레이드 이상에 버그는 가장 쉬운 해결 합니다.

* * *

## <a name="description-of-the-problem"></a>문제 설명

`ibtool` OS X 10.10 Yosemite에 버그 되어 있으면 Xcode 6.0에서 명령입니다. Xamarin.iOS Xcode의를 사용 하 여 `ibtool` 스토리 보드를 컴파일하는 데 및 `XIB` 파일입니다.

Xcode 기준으로 버그에 대 한 자세한 내용은 다음에서 확인할 수 있습니다 스택 오버플로 게시물: [http://stackoverflow.com/questions/25754763/cant-open-storyboard](http://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>오류 메시지

> "MainStoryboard.storyboard" 문서를 열 수 없습니다. 작업을 완료할 수 없습니다. (com.apple.InterfaceBuilder 오류-1)입니다.

## <a name="workarounds-for-xcode-60"></a>(Xcode 6.0)에 대 한 대안

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>옵션 1: 모든 관리 `UIImageView.Image` 코드의 속성

설정 하는 대신는 `Image` 속성은 `UIImageView` 스토리 보드에서 또는 `.xib` 파일을 설정할 수 있습니다 속성 보기 중 하나에 보기 컨트롤러의 수명 주기 재정의 메서드 (예를 들어 `ViewDidLoad()`). 참고 항목 [이미지 작업](~/ios/app-fundamentals/images-icons/index.md) 사용에 대 한 팁 `UIImage.FromBundle()` 비교 `UIImage.FromFile()`합니다.

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>옵션 2: 모든 이미지 리소스를 최상위 이동 `Resources` 폴더

이미지를 최상위 이동한 후 `Resources` 스토리 보드를 업데이트 해야 할는 폴더를 및 `.xib` 파일을 새 이미지 경로 사용 합니다.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>옵션 3: 설정의 `LogicalName` 의 최상위 수준에 복사 하므로 모든 문제가 있는 이미지 자산의 경우에`.app` 번들

예를 들어 원래의 `.csproj` 파일에 다음 항목:

`<BundleResource Include="Resources\Images\image.png" />`

이 요소를 변경 하 고 추가할 수는 `LogicalName` 이미지 대신의 최상위 수준에 복사할 수 있도록는 `.app `번들:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

Mac 용 Visual Studio에서의 `LogicalName` 를 사용 하 여 설정할 수도 있습니다는 `Resource ID` 아래에 있는 이미지에 대 한 필드 **보기 > 패드 > 속성**합니다. (참고 항목: [ http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545 ](http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

이 변경 후 스토리 보드를 업데이트 해야 합니다 및 `.xib` 파일을 새 최상위 이미지 경로 사용 합니다. Mac 용 visual Studio에 대 한 autocompletions 목록이 자동으로 업데이트 됩니다는 `Image` iOS 디자이너에에서는 속성입니다. Visual Studio에서 경로 직접 편집 해야 합니다. IOS 디자이너는 누락 된 이미지에 대 한 표시 다음 하지만 프로젝트를 작성 하 고 제대로 실행 되도록 합니다.

### <a name="next-steps"></a>다음 단계

추가 지원을 문의 또는 위의 정보를 활용 한 후에이 문제가 여전히 발생 하는 경우를 참조 하십시오 [지원 옵션에 대해 Xamarin에 대 한 사용할 수 있는?](~/cross-platform/troubleshooting/support-options.md) 연락처 옵션, 제안 사항에 대 한 내용은 하는 방법 필요한 경우 새 버그를 파일입니다. 

