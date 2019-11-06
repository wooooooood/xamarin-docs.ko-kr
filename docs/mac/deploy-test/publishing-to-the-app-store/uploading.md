---
title: Mac 앱 스토어에 업로드
description: 이 문서에서는 iTunes Connect를 사용하여 Mac App Store에 Xamarin.Mac 앱을 업로드하는 방법을 설명합니다. 프로세스를 완료하기 위해 iTunes Connect에서 필요한 정보를 설명합니다.
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: fede666071aad8702eda17a94a06c7de49c7397e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029470"
---
# <a name="upload-to-mac-app-store"></a>Mac 앱 스토어에 업로드

_이 가이드에서는 Mac 앱 스토어에 게시할 Xamarin.Mac 앱을 업로드하는 방법을 안내합니다._

애플리케이션은 Mac 앱 스토어 승인을 받기 위해 [iTunes Connect](https://itunesconnect.apple.com/)를 통해 제출됩니다.

1. 만들려는 **macOS 앱**을 선택합니다. 

    [![](uploading-images/image65.png "iTunes Connect")](uploading-images/image65.png#lightbox)

2. 애플리케이션의 이름과 기타 세부 정보를 입력합니다. 개발자는 이전에 만든 기존 **번들 ID**에서만 선택할 수 있습니다. 

    [![](uploading-images/image66.png "Selecting the bundle ID")](uploading-images/image66.png#lightbox)

3. 사용 가능한 날짜 및 가격을 선택합니다. 개발자가 선택하는 가용성 날짜에 관계없이, 앱은 승인된 후부터 판매가 가능합니다. 개발자는 실제 가용성 날짜를 보다 세밀하게 제어하고 싶은 경우 나중에 이 값을 설정할 수 있습니다. 

    [![](uploading-images/image67.png "Setting the available date and price")](uploading-images/image67.png#lightbox)

4. 소속된 앱 스토어 범주를 포함하여 앱 정보를 입력합니다. 

    [![](uploading-images/image68.png "Entering the app information")](uploading-images/image68.png#lightbox) 

    적용되는 등급을 선택합니다. 

    [![](uploading-images/image69.png "Setting the app ratings")](uploading-images/image69.png#lightbox) 

    설명, 키워드 및 연락처 URL을 편집합니다. 

    [![](uploading-images/image70.png "Editing the Description, keywords and contact URLs")](uploading-images/image70.png#lightbox) 

    연락처 정보 및 앱 스토어 검토자에 대한 조언을 편집합니다. 

    [![](uploading-images/image71.png "Editing the contact information and advice for the App Store reviewers")](uploading-images/image71.png#lightbox) 

    마지막으로, 스크린샷을 추가합니다. 

    [![](uploading-images/image72.png "Adding the required screenshots")](uploading-images/image72.png#lightbox) 

    스크린샷은 JPG, TIF 또는 PNG 형식의 1280x800, 1440x900, 2880x1800 또는 2560x1600 픽셀이어야 합니다. **저장**을 눌러 완료합니다.

5. 앱 정보가 검토용으로 표시됩니다. **세부 정보 보기**를 클릭하여 상태를 변경합니다. 

    [![](uploading-images/image73.png "Viewing the app details")](uploading-images/image73.png#lightbox)

6. [이진 파일 업로드 준비 완료]를 클릭하여 애플리케이션 패키지 파일을 제출합니다. 

    [![](uploading-images/image74.png "Selecting Ready to Upload Binary")](uploading-images/image74.png#lightbox)

7. 암호화 질문에 답변합니다. 

    [![](uploading-images/image75.png "Answering the cryptography question")](uploading-images/image75.png#lightbox)

8. 이 사이트는 애플리케이션 패키지 파일을 수락할 준비가 완료되면 그 사실을 알려줍니다. 

    [![](uploading-images/image76.png "The acceptance notification")](uploading-images/image76.png#lightbox)

9. 애플리케이션 로더를 시작하고 Apple ID로 로그인합니다.
**앱 배달**을 선택하여 계속 진행합니다. 

    [![](uploading-images/image77.png "The Application Loader interface")](uploading-images/image77.png#lightbox)

10. **이진 파일 업로드 준비 완료** 상태인 애플리케이션 목록 중에서 선택하고 **다음**을 클릭합니다. 

    [![](uploading-images/image78.png "Selecting the app to load")](uploading-images/image78.png#lightbox)

11. 애플리케이션 메타데이터를 검토하고 **선택...** 을 클릭하여 패키지 파일을 찾습니다. 

    [![](uploading-images/image79.png "Reviewing the app metadata")](uploading-images/image79.png#lightbox)

12. 앱 스토어 빌드 구성을 사용하여 Mac용 Visual Studio에서 빌드한 패키지 파일을 찾습니다. 

    [![](uploading-images/image80.png "Selecting the file to upload")](uploading-images/image80.png#lightbox)

13. **보내기**를 누릅니다. 

    [![](uploading-images/image81.png "Sending the app")](uploading-images/image81.png#lightbox)

14. 패키지의 유효성이 검사되고 오류가 보고됩니다. 오류를 수정하고 다시 업로드합니다. 업로드가 완료되면 앱 스토어 팀에서 검토할 수 있도록 앱이 자동으로 제출됩니다. 

    [![](uploading-images/image82.png "An example of upload errors")](uploading-images/image82.png#lightbox)

애플리케이션이 승인되면 Mac 앱 스토어에서 다운로드하거나 구매할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [설치](~//mac/get-started/installation.md)
- [Hello, Mac 샘플](~//mac/get-started/hello-mac.md)
- [Mac 앱 스토어에서 앱 배포](https://developer.apple.com/devcenter/mac/checklist/)
- [도구 가이드: 앱 코드 서명](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [개발자 ID 및 게이트키퍼](https://developer.apple.com/resources/developer-id/)
