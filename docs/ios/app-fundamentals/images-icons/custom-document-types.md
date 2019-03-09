---
title: Xamarin.iOS에서 사용자 지정 문서 아이콘
description: Xamarin.iOS 앱에 사용자 지정 문서 형식 아이콘으로 사용할 이미지 자산 관리 및 포함 하 여이 문서에서는 합니다.
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/23/2017
ms.openlocfilehash: 155847b4f5f6b3e6070f1afb6219db2d3789a075
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668467"
---
# <a name="custom-document-icons-in-xamarinios"></a>Xamarin.iOS에서 사용자 지정 문서 아이콘

_Xamarin.iOS 앱에 사용자 지정 문서 형식 아이콘으로 사용할 이미지 자산 관리 및 포함 하 여이 문서에서는 합니다._

개발자가 사용자의 첨부 파일을 누르고 등 해당 문서 유형을 발견 한 경우 시스템에서 사용 되는 아이콘을 제공할 수 있습니다 Xamarin.iOS 앱이 특정 문서 유형을 로드를 지 원하는 경우는 *메일 응용 프로그램* 으로 다음과 같습니다.

 [![](custom-document-types-images/17.png "문서 형식 아이콘의 예")](custom-document-types-images/17.png#lightbox)

에 대 한 사전 항목을 포함 하 여 열 수는 파일 형식은 앱에 대 한 개발자 문서 유형 정보를 추가할 수는 `CFBundleTypeName` 문자열 및 `LSItemContentTypes` 앱의 배열 `Info.plist`합니다. 문서 형식에 대 한 아이콘으로는 `CFBundleTypeIconFiles` 배열입니다. 문서 아이콘을 제공 하지 않으면 iOS 앱 아이콘에서 파생 됩니다.
다양 한 장치 해상도에 최적화 된 여러 크기에 대 한 아이콘을 제공할 수 있습니다. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

사용 하 여 Mac 용 Visual Studio에서 이러한 값을 할당할를 **문서 유형에** 섹션을 **고급** 탭는 `Info.plist` 문서 유형을 추가 하 고 이미지 아이콘을 할당 하는 편집기. 예를 들어 다음과 같습니다. PDF 지원에 대 한 등록을 보여 주는 스크린샷

 [![](custom-document-types-images/18.png "'Info.plist' 편집기에서 고급 탭 문서 형식 섹션")](custom-document-types-images/18.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 이러한 값에 할당 하려면 사용 하 여는 **문서 유형에** 섹션을 **고급** 탭는 `Info.plist`:

 ![](custom-document-types-images/doc01w.png "고급 탭에서 문서 유형 섹션을 엽니다")

클릭는 **문서 유형을 추가** 단추 및 필수 필드를 입력 합니다.

![](custom-document-types-images/doc02w.png "문서 형식 추가 폼")

-----


문서 종류에 대 한 자세한 내용은 Apple의을 참조 하세요 [Uniform 형식 식별자 참조](https://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) 하 고 [iOS에 대 한 문서 상호 작용 프로그래밍 항목](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html)합니다.


## <a name="related-links"></a>관련 링크

- [이미지 (샘플)를 사용 하 여 작업](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [사용자 지정 아이콘 및 이미지 생성 지침](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
