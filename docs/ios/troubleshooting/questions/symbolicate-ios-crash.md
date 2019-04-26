---
title: iOS 크래시 로그를 기호로 표시하는 .dSYM 파일을 어디에서 찾을 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/09/2018
ms.openlocfilehash: 0b8f3aa736cba6e70fbf346766499c23a9bbe270
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61418220"
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>iOS 크래시 로그를 기호로 표시하는 .dSYM 파일을 어디에서 찾을 수 있나요?

Mac 또는 Visual Studio 2017에 대 한 Visual Studio를 사용 하 여 iOS 앱을 빌드하는 경우 앱의 프로젝트 파일 (.csproj)을 동일한 디렉터리 계층 구조에는 작동 중단 보고서 기호화 하는 데 필요한.dSYM 파일 배치 됩니다. 정확한 위치는 프로젝트의 빌드 설정에 따라 달라 집니다.

- 장치 관련 빌드를 사용 하는 경우를 표시 하는.dSYM 디렉터리에서 찾을 수 있습니다.

    **&lt;프로젝트 디렉터리&gt;/bin/&lt;플랫폼&gt;/&lt;configuration&gt;/device-builds /&lt;장치&gt; - &lt; os 버전&gt;/**

    예를 들어:
  
    **TestApp/bin/iPhone/Release/device-builds/iphone8.4-11.3.1/**

- 장치 관련 빌드를 설정 하지 않은 경우는.dSYM 디렉터리에서 찾을 수 있습니다.

    **&lt;프로젝트 디렉터리&gt;/bin/&lt;플랫폼&gt;/&lt;구성&gt;/**

    예를 들어:

    **TestApp/bin/iPhone/Release/**

> [!NOTE]
> 빌드 프로세스의 일환으로, Visual Studio 2017 Windows Mac 빌드 호스트에서 표시 하는.dSYM 파일을 복사합니다. Windows에서 표시 하는.dSYM 파일에 표시 되지 않으면 될 앱의 빌드 설정을 구성 했는지 [.ipa 파일을 만들고](~/ios/deploy-test/app-distribution/ipa-support.md)합니다.

## <a name="see-also"></a>참고자료

- [IOS 충돌 파일 (Xamarin.iOS) symbolicating](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [IOS 응용 프로그램 크래시 로그를 익히기](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)

