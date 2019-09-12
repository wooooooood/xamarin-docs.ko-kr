---
title: Mac 앱 스토어용으로 묶기
description: 이 문서에서는 Mac App Store에 게시할 Xamarin.Mac 앱을 묶는 방법을 설명합니다. 코드 서명 옵션 및 빌드를 설명합니다.
ms.prod: xamarin
ms.assetid: 00a36d7c-937d-4657-bf6a-0de9684b8f94
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 129ba01a41f9e5f58802c4d4da65d1662a103adc
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770117"
---
# <a name="bundling-for-the-mac-app-store"></a>Mac 앱 스토어용으로 묶기

이 섹션에서는 Mac용 Visual Studio를 사용하여 Mac 앱 스토어에 릴리스할 애플리케이션을 빌드하는 기본 사항을 설명합니다. 추가 기능(예: iCloud 액세스 및 푸시 알림)에 따라 이 문서의 범위를 벗어나는 추가 설치가 필요할 수 있습니다.

> [!NOTE]
> 개발자는 이 섹션을 시작하기에 앞서 Mac 앱 스토어용으로 빌드할 프로덕션 프로비저닝 프로필이 있어야 합니다. 필요한 프로비전 프로필을 만드는 방법에 대한 이 문서의 앞부분의 지침을 참조하세요.

## <a name="code-signing-options"></a>코드 서명 옵션

코드 서명 및 패키징 옵션을 업데이트하기 전에 **구성**을 **릴리스**로 변경합니다. 개발자는 회사 **ID**와 위에서 앱 스토어에 릴리스할 애플리케이션을 서명할 때 만든 프로비전 프로필을 사용해야 합니다.

 [![코드 서명 옵션 편집](bundling-images/config02.png "코드 서명 옵션 편집")](bundling-images/config02-large.png#lightbox)

**Mac에서 빌드** 설정에서 설치 관리자 패키지를 만드는 옵션을 선택했는지 확인합니다.

[![빌드 옵션 편집](bundling-images/config03.png "빌드 옵션 편집")](bundling-images/config03-large.png#lightbox)

## <a name="build"></a>빌드

빌드하기 전에, **릴리스** 구성을 선택했는지 확인합니다. 개발자가 앱을 빌드할 때 두 인증서를 사용하라는 메시지가 표시됩니다.

 ![앱에서 인증서를 사용하도록 허용](bundling-images/image62.png "앱에서 인증서를 사용하도록 허용")

 ![앱에서 인증서를 사용하도록 허용](bundling-images/image63.png "앱에서 인증서를 사용하도록 허용")

애플리케이션을 빌드한 후 개발자는 프로젝트를 마우스 오른쪽 단추로 클릭하고 **상위 폴더 열기**를 선택하여 패키지 파일을 찾습니다(아래에 표시된 예제의 `bin/x86/AppStore` 디렉터리에서).  이 패키지 파일에는 Mac 앱 스토어에 포함하기 위해 Apple에 제출할 수 있는 앱의 설치 관리자가 들어 있습니다.

 ![Finder에서 빌드 패키지 선택](bundling-images/image64.png "Finder에서 빌드 패키지 선택")

## <a name="related-links"></a>관련 링크

- [설치](/visualstudio/mac/installation/)
- [Hello, Mac 샘플](~/mac/get-started/hello-mac.md)
- [Mac 앱 스토어에서 앱 배포](https://developer.apple.com/devcenter/mac/checklist/)
- [개발자 ID 및 게이트키퍼](https://developer.apple.com/resources/developer-id/)
