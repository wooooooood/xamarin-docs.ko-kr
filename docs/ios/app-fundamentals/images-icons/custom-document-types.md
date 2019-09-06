---
title: Xamarin.ios의 사용자 지정 문서 아이콘
description: 이 문서에서는 사용자 지정 문서 형식 아이콘으로 사용할 Xamarin.ios 앱의 이미지 자산을 포함 하 고 관리 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/23/2017
ms.openlocfilehash: 683587e4857ede20096be731b3cfa3b88b3a668d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282527"
---
# <a name="custom-document-icons-in-xamarinios"></a>Xamarin.ios의 사용자 지정 문서 아이콘

_이 문서에서는 사용자 지정 문서 형식 아이콘으로 사용할 Xamarin.ios 앱의 이미지 자산을 포함 하 고 관리 하는 방법을 설명 합니다._

Xamarin.ios 앱에서 특정 문서 유형 로드를 지 원하는 경우 개발자는 다음과 같이 사용자가 *메일 응용 프로그램* 에서 첨부 파일을 보관 하는 경우와 같이 시스템에서 해당 문서 유형을 발견할 때 사용 하는 아이콘을 제공할 수 있습니다.

 [![](custom-document-types-images/17.png "문서 형식 아이콘의 예")](custom-document-types-images/17.png#lightbox)

개발자는 응용 프로그램에서 `CFBundleTypeName` 문자열 및 `LSItemContentTypes` `Info.plist`배열에 대 한 사전 항목을 포함 하 여 응용 프로그램을 열 수 있는 파일 형식에 대 한 문서 형식 정보를 추가할 수 있습니다. 문서 형식에 대 한 아이콘은 `CFBundleTypeIconFiles` 배열에서 이동 합니다. 문서 아이콘을 제공 하지 않으면 iOS는 앱 아이콘에서 하나를 파생 합니다.
다양 한 장치 해상도에 맞게 최적화 된 여러 크기의 아이콘을 제공할 수 있습니다. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac용 Visual Studio에서 이러한 값을 할당 하려면 `Info.plist` 편집기의 **고급** 탭에서 **문서 형식** 섹션을 사용 하 여 문서 유형을 추가 하 고 이미지 아이콘을 할당 합니다. 예를 들어 다음은 PDF 지원 등록을 보여 주는 스크린샷입니다.

 [![](custom-document-types-images/18.png "' Info.plist ' 편집기의 고급 탭에 있는 문서 유형 섹션")](custom-document-types-images/18.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 이러한 값을 할당 하려면의 **고급** `Info.plist`탭에서 **문서 형식** 섹션을 사용 합니다.

 ![](custom-document-types-images/doc01w.png "고급 탭에서 문서 유형 섹션을 엽니다.")

**문서 형식 추가** 단추를 클릭 하 고 필수 필드를 입력 합니다.

![](custom-document-types-images/doc02w.png "문서 형식 추가 양식")

-----


문서 형식에 대 한 자세한 내용은 Apple의 [일관적인 형식 식별자 참조](https://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) 및 [IOS 용 문서 상호 작용 프로그래밍 항목](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html)을 참조 하세요.


## <a name="related-links"></a>관련 링크

- [이미지 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [사용자 지정 아이콘 및 이미지 생성 지침](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
