---
title: 'IBTool 오류: 작업을 완료할 수 없습니다.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 4ff7b88ad63870246acbf2b29d01775f6711a8ce
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031187"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>IBTool 오류: 작업을 완료할 수 없습니다.

## <a name="fixed-in-xcode-611"></a>Xcode 6.1.1에서 수정 됨

Apple 은 Xcode 6.1.1에서 이 `ibtool`버그를 [수정](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) 했으므로 Xcode 6.1.1 이상으로 업그레이드 하는 것이 가장 쉬운 픽스입니다.

* * *

## <a name="description-of-the-problem"></a>문제에 대 한 설명

Xcode 6.0의 `ibtool` 명령은 OS X 10.10 Yosemite에 버그가 있습니다. Xamarin.ios는 Xcode의 `ibtool` 사용 하 여 스토리 보드 및 `XIB` 파일을 컴파일합니다.

Xcode와 관련 된 버그에 대 한 자세한 내용은 다음 Stack Overflow 게시물에서 찾을 수 있습니다. [https://stackoverflow.com/questions/25754763/cant-open-storyboard](https://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>오류 메시지

> 문서 "Mainstoryboard.storyboard"을 (를) 열 수 없습니다. 작업을 완료할 수 없습니다. (.com 빌더 오류-1)

## <a name="workarounds-for-xcode-60"></a>해결 방법 (Xcode 6.0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>옵션 1: 코드의 모든 `UIImageView.Image` 속성 관리

스토리 보드 또는 `.xib` 파일에서 `UIImageView`의 `Image` 속성을 설정 하는 대신 뷰 컨트롤러의 뷰 수명 주기 재정의 메서드 (예: `ViewDidLoad()`) 중 하나에서 속성을 설정할 수 있습니다. `UIImage.FromBundle()` vs. `UIImage.FromFile()`를 사용하는 방법에 대한 팁은 [이미지 작업](~/ios/app-fundamentals/images-icons/index.md)을 참조하세요.

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>옵션 2: 모든 이미지 리소스를 최상위 `Resources` 폴더로 이동

이미지를 최상위 `Resources` 폴더로 이동한 후에는 스토리 보드 및 `.xib` 파일을 업데이트 하 여 새 이미지 경로를 사용 해야 합니다.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>옵션 3: 문제가 있는 이미지 자산에 대 한 `LogicalName` 설정 하 여`.app` 번들의 최상위 수준에 복사

예를 들어 원래 `.csproj` 파일에는 다음 항목이 포함 되어 있다고 가정 합니다.

`<BundleResource Include="Resources\Images\image.png" />`

이 요소를 변경 하 고 이미지를 `.app` 번들의 최상위 수준에 복사 하도록 `LogicalName`를 추가할 수 있습니다.

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

Mac용 Visual Studio에서 `Resource ID` 필드를 사용 하 여 `LogicalName`를 설정할 수도 있습니다 **> 패드 > 속성**입니다. (참고 항목: [https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545](https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

이렇게 변경한 후에는 스토리 보드 및 `.xib` 파일을 업데이트 하 여 새 최상위 이미지 경로를 사용 해야 합니다. Mac용 Visual Studio는 iOS 디자이너에서 `Image` 속성에 대 한 자동 완성 목록을 자동으로 업데이트 합니다. Visual Studio에서는 경로를 직접 편집 해야 합니다. 그런 다음 iOS 디자이너는이를 누락 된 이미지로 표시 하지만 프로젝트가 제대로 빌드 및 실행 됩니다.

### <a name="next-steps"></a>다음 단계

추가 지원이 필요 하면 microsoft에 문의 하거나, 위의 정보를 사용한 후에도이 문제가 계속 발생 하는 경우 [Xamarin에 사용할 수 있는 지원 옵션](~/cross-platform/troubleshooting/support-options.md) 을 참조 하세요. 연락처 옵션, 제안 사항 및 필요한 경우 새 버그를 제출 하는 방법에 대 한 자세한 내용은을 참조 하세요. . 
