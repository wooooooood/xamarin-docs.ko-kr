---
title: 사용자 지정 문서 아이콘
description: Xamarin.iOS 앱에서 사용자 지정 문서 형식 아이콘으로 사용할 이미지 자산 관리 및 포함 하 여이 문서 다룹니다.
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/23/2017
ms.openlocfilehash: b369667bd728f7c8b6e8bcfed9cf5bca2916bf69
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="custom-document-icons"></a>사용자 지정 문서 아이콘

_Xamarin.iOS 앱에서 사용자 지정 문서 형식 아이콘으로 사용할 이미지 자산 관리 및 포함 하 여이 문서 다룹니다._

개발자는 사용자의 첨부 파일을 누르고 등 해당 문서 유형을 발견 하는 경우 시스템에서 사용 하는 아이콘을 제공할 수 Xamarin.iOS 앱이 특정 문서 형식을 로드를 지 원하는 경우는 *메일 응용 프로그램* 으로 다음과 같습니다.

 [![](custom-document-types-images/17.png "문서 유형 아이콘의 예")](custom-document-types-images/17.png#lightbox)

개발자는 파일 형식은 응용 프로그램에 대 한 사전 항목을 포함 하 여 열 수는 대 한 문서 유형 정보를 추가할 수는 `CFBundleTypeName` 문자열 및 `LSItemContentTypes` 응용 프로그램의 배열 `Info.plist`합니다. 문서 유형에 대 한 아이콘에서 go는 `CFBundleTypeIconFiles` 배열입니다. 문서 아이콘이 제공 되지 않는 경우 iOS 앱 아이콘에서 파생 됩니다.
여러 크기, 다양 한 장치 해상도에 최적화에 대 한 아이콘을 제공할 수 있습니다. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 용 Visual Studio에서 이러한 값을 할당 하려면 사용 하 여는 **문서 유형에** 섹션 아래는 **고급** 탭에 `Info.plist` 문서 종류를 추가 하 고 이미지 아이콘을 할당 하는 편집기. 예를 들어 다음은 PDF 지원에 대 한 등록을 보여 주는 스크린 샷입니다.

 [![](custom-document-types-images/18.png "[고급] 탭에서 'Info.plist' 편집기에 있는 문서 유형 섹션")](custom-document-types-images/18.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio에서 이러한 값을 할당 하려면 사용 하 여는 **문서 유형에** 섹션 아래에서 **고급** 탭에 `Info.plist`:

 ![](custom-document-types-images/doc01w.png "고급 탭에 있는 문서 유형 섹션을 열으십시오")

클릭는 **문서 형식 추가** 단추 및 필수 필드 입력:

![](custom-document-types-images/doc02w.png "추가 문서 형식 폼")

-----


문서 유형에 대 한 자세한 내용은 Apple의을 참조 하십시오. [Uniform 형식 식별자 참조](http://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) 및 [iOS에 대 한 문서 상호 작용 프로그래밍 항목이](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html)합니다.


## <a name="related-links"></a>관련 링크

- [이미지 (샘플) 작업](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [사용자 지정 아이콘 및 이미지 생성 지침](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
