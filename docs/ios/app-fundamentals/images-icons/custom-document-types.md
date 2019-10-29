---
title: Xamarin.ios의 사용자 지정 문서 아이콘
description: 이 문서에서는 사용자 지정 문서 형식 아이콘으로 사용할 Xamarin.ios 앱의 이미지 자산을 포함 하 고 관리 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/23/2017
ms.openlocfilehash: ac8ee96d6183f9a62233d217c75b03da15605bd2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73004217"
---
# <a name="custom-document-icons-in-xamarinios"></a>Xamarin.ios의 사용자 지정 문서 아이콘

_이 문서에서는 사용자 지정 문서 형식 아이콘으로 사용할 Xamarin.ios 앱의 이미지 자산을 포함 하 고 관리 하는 방법을 설명 합니다._

Xamarin.ios 앱에서 특정 문서 유형 로드를 지 원하는 경우 개발자는 다음과 같이 사용자가 *메일 응용 프로그램* 에서 첨부 파일을 보관 하는 경우와 같이 시스템에서 해당 문서 유형을 발견할 때 사용 하는 아이콘을 제공할 수 있습니다.

 [![](custom-document-types-images/17.png "An example of document type icons")](custom-document-types-images/17.png#lightbox)

개발자는 응용 프로그램의 `Info.plist`에 `CFBundleTypeName` 문자열 및 `LSItemContentTypes` 배열에 대 한 사전 항목을 포함 하 여 응용 프로그램을 열 수 있는 파일 형식에 대 한 문서 형식 정보를 추가할 수 있습니다. 문서 형식에 대 한 아이콘은 `CFBundleTypeIconFiles` 배열로 이동 합니다. 문서 아이콘을 제공 하지 않으면 iOS는 앱 아이콘에서 하나를 파생 합니다.
다양 한 장치 해상도에 맞게 최적화 된 여러 크기의 아이콘을 제공할 수 있습니다. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac용 Visual Studio에서 이러한 값을 할당 하려면 `Info.plist` 편집기의 **고급** 탭에서 **문서 형식** 섹션을 사용 하 여 문서 유형을 추가 하 고 이미지 아이콘을 할당 합니다. 예를 들어 다음은 PDF 지원 등록을 보여 주는 스크린샷입니다.

 [![](custom-document-types-images/18.png "The Document Types section under the Advanced tab on the `Info.plist` editor")](custom-document-types-images/18.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 이러한 값을 할당 하려면 `Info.plist`의 **고급** 탭에서 **문서 형식** 섹션을 사용 합니다.

 ![](custom-document-types-images/doc01w.png "Open the Document Types section under the Advanced tab")

**문서 형식 추가** 단추를 클릭 하 고 필수 필드를 입력 합니다.

![](custom-document-types-images/doc02w.png "The Add Document Type form")

-----

문서 형식에 대 한 자세한 내용은 Apple의 [일관적인 형식 식별자 참조](https://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) 및 [IOS 용 문서 상호 작용 프로그래밍 항목](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [이미지 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [사용자 지정 아이콘 및 이미지 생성 지침](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
