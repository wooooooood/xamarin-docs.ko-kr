---
title: 'IBTool 오류: 작업을 완료할 수 없습니다.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 593706de23d370d6bff03c97bb50e064d77a52ef
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350734"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>IBTool 오류: 작업을 완료할 수 없습니다.

## <a name="fixed-in-xcode-611"></a>Xcode 6.1.1에 고정

Apple [고정](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) 이 `ibtool` 6.1.1 않으므로 Xcode 6.1.1로 업그레이드 하거나 더 높은 Xcode에서 버그는 가장 손쉬운 해결 합니다.

* * *

## <a name="description-of-the-problem"></a>문제 설명

`ibtool` Xcode 6.0에서 명령 OS X 10.10 yosemite 버그가 있었습니다. Xcode의을 사용 하 여 Xamarin.iOS `ibtool` 스토리 보드를 컴파일하려면 및 `XIB` 파일입니다.

Xcode와 관련 된 버그에 대 한 자세한 내용은 다음에서 확인할 수 있습니다 Stack Overflow 게시물: [http://stackoverflow.com/questions/25754763/cant-open-storyboard](http://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>오류 메시지

> "MainStoryboard.storyboard" 문서를 열 수 없습니다. 작업을 완료할 수 없습니다. (com.apple.InterfaceBuilder 오류-1입니다.)

## <a name="workarounds-for-xcode-60"></a>해결 방법 (Xcode 6.0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>옵션 1: 모든 관리 `UIImageView.Image` 코드에서 속성

설정 하는 대신를 `Image` 의 속성을 `UIImageView` 스토리 보드에서 또는 `.xib` 파일을 설정할 수 있습니다 속성 뷰 중 하나에서 뷰 컨트롤러의 수명 주기 재정의 메서드 (예를 들어 `ViewDidLoad()`). 참고 항목 [이미지를 사용 하 여 작업](~/ios/app-fundamentals/images-icons/index.md) 사용에 관한 팁 `UIImage.FromBundle()` 비교 `UIImage.FromFile()`합니다.

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>옵션 2: 이동 이미지 리소스의 모든 최상위 `Resources` 폴더

최상위 이미지를 이동 하 고 나면 `Resources` 스토리 보드를 업데이트 해야 폴더 및 `.xib` 새 이미지 경로 사용 하는 파일입니다.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>옵션 3: 설정 합니다 `LogicalName` 의 최상위 수준에 복사 됩니다 있도록 문제가 이미지 자산에 대 한는`.app` 번들

예를 들어 원래 `.csproj` 파일에 다음 항목이 포함 되어 있습니다.

`<BundleResource Include="Resources\Images\image.png" />`

이 요소를 변경 하 고 추가할 수는 `LogicalName` 이미지 대신의 최상위 수준으로 복사할 수 있도록는 `.app `번들:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

Mac 용 Visual Studio에서의 `LogicalName` 를 사용 하 여 설정할 수도 있습니다는 `Resource ID` 아래에 있는 이미지에 대 한 필드 **보기 > 패드 > 속성**합니다. (참고: [ http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545 ](http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

이렇게이 변경한 후 스토리 보드를 업데이트 해야 하 고 `.xib` 새 최상위 이미지 경로 사용 하는 파일입니다. Mac 용 visual Studio에 대 한 autocompletions 목록을 자동으로 업데이트 됩니다는 `Image` iOS 디자이너의에서 속성입니다. Visual Studio에서 경로 수동으로 편집 해야 합니다. IOS 디자이너는 누락 된 이미지를 표시 한 다음 하지만 프로젝트 빌드 및 제대로 실행 됩니다.

### <a name="next-steps"></a>다음 단계

추가 지원이 필요한 경우, 우리에 게 문의 하 여 위의 정보를 활용 하 여 한 후에이 문제가 여전히 발생 하는 경우를 참조 하세요 [Xamarin에 대해 사용할 수 있는 지원 옵션?](~/cross-platform/troubleshooting/support-options.md) 연락처 옵션을 제안에 대 한 정보에 대 한 방법 뿐만 아니라 필요한 경우 새 버그를 파일입니다. 

