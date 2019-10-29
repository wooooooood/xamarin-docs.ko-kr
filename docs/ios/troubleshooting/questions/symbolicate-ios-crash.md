---
title: iOS 크래시 로그를 기호로 표시하는 .dSYM 파일을 어디에서 찾을 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/09/2018
ms.openlocfilehash: 418a0196849099da03983085aca9ceed2077207b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030916"
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>iOS 크래시 로그를 기호로 표시하는 .dSYM 파일을 어디에서 찾을 수 있나요?

Mac용 Visual Studio 또는 Visual Studio 2017을 사용 하 여 iOS 앱을 빌드할 때 충돌 보고서를 기호화 하는 데 필요한 dSYM 파일은 앱의 프로젝트 파일 (.csproj)과 동일한 디렉터리 계층 구조에 배치 됩니다. 정확한 위치는 프로젝트의 빌드 설정에 따라 달라 집니다.

- 장치별 빌드를 사용 하도록 설정한 경우 dSYM는 다음 디렉터리에서 찾을 수 있습니다.

    **&lt;프로젝트 디렉터리&gt;/bin/&lt;platform&gt;/&lt;configuration&gt;/device-builds/&lt;device&gt;-&lt;os-버전&gt;**

    예를 들면,
  
    **TestApp/bin/iPhone/Release/device-빌드/iphone 8.4-11.3.1/**

- 장치별 빌드를 사용 하도록 설정 하지 않은 경우. dSYM는 다음 디렉터리에서 찾을 수 있습니다.

    **&lt;프로젝트 디렉터리&gt;/bin/&lt;platform&gt;/&lt;구성&gt;**

    예를 들면,

    **TestApp/bin/iPhone/Release/**

> [!NOTE]
> 빌드 프로세스의 일부로 Visual Studio 2017는 Mac 빌드 호스트에서 Windows로 dSYM 파일을 복사 합니다. Windows에 dSYM 파일이 표시 되지 않으면 [ipa 파일을 만들기](~/ios/deploy-test/app-distribution/ipa-support.md)위해 앱의 빌드 설정을 구성 했는지 확인 합니다.

## <a name="see-also"></a>참조

- [Symbolicating iOS 크래시 파일 (Xamarin.ios)](https://www.jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [전문가가 제공 자세히 iOS 응용 프로그램 크래시 로그](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
