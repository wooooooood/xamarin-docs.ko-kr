---
title: IOS 충돌 로그 symbolicate.dSYM 파일은 어디서 찾을 수 있습니까?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ce60c19ab0b680e00338f517e5a3f17f725ed329
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>IOS 충돌 로그 symbolicate.dSYM 파일은 어디서 찾을 수 있습니까?

Visual studio에서 iOS 앱을 빌드, symbolicate 충돌 보고서를 사용할 수 있는.dSYM 파일 경로에서 빌드 호스트에서 끝납니다.
```
    /Users/<username>/Library/Caches/Xamarin/mtbs/builds/<appname>/<guid>/bin/iPhone/<configuration>
```

사항에 유의 `~/Library` 폴더가 숨겨져 Finder에는 기본적으로 하므로 필요 사용할 Finder의 **이동 > 폴더로 이동 합니다** 메뉴 입력: `~/Library/Caches/Xamarin/mtbs/builds/` 여 폴더를 엽니다.  

숨기기 취소 또는 `~/Library` 를 사용 하 여 폴더는 **보기 옵션 표시** 홈 폴더에 대 한 패널입니다. Finder에 세로 막대에서 홈 폴더를 선택 하 고 Finder 메뉴를 사용 하는 경우 **보기 > 보기 옵션 표시** (또는 cmd j) 하 라는 확인란이 표시 됩니다 **라이브러리 폴더 표시**합니다.


### <a name="see-also"></a>참고 항목
- 확장 된 단계 symbolicating iOS에 대 한 충돌 보고서: [http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [IOS 응용 프로그램 크래시 로그를 제공합니다.](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
