---
title: IPA 파일이 0바이트입니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 376BBA27-8694-4E63-9976-BF60349D42D8
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 3ef916dbd277544309f1803f923cde302494e4f1
ms.sourcegitcommit: 61a35d0643eb3bf5adb8f8831da54771d8dde626
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71033498"
---
# <a name="ipa-file-is-0-bytes"></a>IPA 파일이 0바이트입니다.

> [!IMPORTANT]
> 이 문제는 최신 버전의 Xamarin에서 해결 되었습니다. 그러나 최신 버전의 소프트웨어에서 문제가 발생 하는 경우 전체 버전 정보 및 전체 빌드 로그 출력을 사용 하 여 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 를 작성 하세요.

이전 버전의 Xamarin에서는 Windows의 IPA 파일을 0 바이트로 만드는 몇 가지 알려진 문제가 있었습니다. 

### <a name="fixed-in-xamarin-for-visual-studio-311584"></a>Visual Studio 용 Xamarin에서 수정 됨 3.11.584 

- [버그 24416-명령줄에서 "임시" 구성을 작성 하면 IPA 파일이 Windows에 복사 되지 않습니다.](https://bugzilla.xamarin.com/show_bug.cgi?id=24416)
- [버그 24417-"프로젝트 속성-> iOS IPA 옵션-> 패키지 이름"을 변경 하면 IPA가 다시 Windows에 복사 되지 않습니다.](https://bugzilla.xamarin.com/show_bug.cgi?id=24417)
- [버그 29822-[XVS. iOS 3.11] "버전" 번호와 다른 "빌드" 번호를 설정 하면 IPA가 Windows에 복사 되지 않습니다.](https://bugzilla.xamarin.com/show_bug.cgi?id=29822)

### <a name="fixed-in-xamarin-for-visual-studio-410496"></a>Visual Studio 용 Xamarin에서 수정 됨 4.1.0.496

- [버그 27989-어셈블리 이름이 프로젝트 이름과 일치 하지 않는 경우 빌드 서버에 ipa 파일 표시에 실패 합니다.](https://bugzilla.xamarin.com/show_bug.cgi?id=27989)
