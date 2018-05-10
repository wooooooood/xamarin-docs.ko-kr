---
title: IOS 충돌 로그 symbolicate.dSYM 파일은 어디서 찾을 수 있습니까?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/09/2018
ms.openlocfilehash: 60d897be8739ff5b78a322bc4ea3f43011785bb5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>IOS 충돌 로그 symbolicate.dSYM 파일은 어디서 찾을 수 있습니까?

Mac 또는 Visual Studio 2017에 대 한 Visual Studio를 사용 하 여 iOS 앱을 빌드하는 경우 앱의 프로젝트 파일 (.csproj)과 같은 디렉터리 계층 구조에서 symbolicate 충돌 보고서에 필요한.dSYM 파일 배치 됩니다. 정확한 위치는 프로젝트의 빌드 설정에 따라 달라 집니다.

- 장치 전용 빌드를 사용 하는 경우는.dSYM 다음 디렉터리에서 찾을 수 있습니다.

    **&lt;프로젝트 디렉터리&gt;/bin/&lt;플랫폼&gt;/&lt;구성&gt;/device-builds /&lt;장치&gt; - &lt; os 버전&gt;/**

    예를 들어:
  
    **TestApp/bin/iPhone/Release/device-builds/iphone8.4-11.3.1/**

- 장치 전용 빌드를 설정 하지 않았으면는.dSYM 다음 디렉터리에 있습니다.

    **&lt;프로젝트 디렉터리&gt;/bin/&lt;플랫폼&gt;/&lt;구성&gt;/**

    예를 들어:

    **TestApp/bin/iPhone/릴리스 /**

> [!NOTE]
> 빌드 프로세스의 일환으로, Visual Studio 2017 Windows 빌드 호스트 Mac에서에서.dSYM 파일을 복사합니다. Windows에서.dSYM 파일, 표시 되지 않으면 앱의 빌드 설정을 구성 했는지 수 [.ipa 파일을 만들고](~/ios/deploy-test/app-distribution/ipa-support.md)합니다.

## <a name="see-also"></a>참고자료

- [IOS 충돌 파일 (Xamarin.iOS) symbolicating](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [IOS 응용 프로그램 크래시 로그를 제공합니다.](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)

