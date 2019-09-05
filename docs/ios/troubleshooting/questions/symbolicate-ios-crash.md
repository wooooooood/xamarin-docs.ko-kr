---
title: iOS 크래시 로그를 기호로 표시하는 .dSYM 파일을 어디에서 찾을 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/09/2018
ms.openlocfilehash: edd5a2c1ed2efdffc9bd28bb06ac19348615eefc
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292105"
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>iOS 크래시 로그를 기호로 표시하는 .dSYM 파일을 어디에서 찾을 수 있나요?

Mac용 Visual Studio 또는 Visual Studio 2017을 사용 하 여 iOS 앱을 빌드할 때 충돌 보고서를 기호화 하는 데 필요한 dSYM 파일은 앱의 프로젝트 파일 (.csproj)과 동일한 디렉터리 계층 구조에 배치 됩니다. 정확한 위치는 프로젝트의 빌드 설정에 따라 달라 집니다.

- 장치별 빌드를 사용 하도록 설정한 경우 dSYM는 다음 디렉터리에서 찾을 수 있습니다.

    **&lt;프로젝트 디렉터리&gt;/l i&lt;/&gt;/플랫폼구성&gt;/device-builds/장치&lt;&gt; &lt;- &lt; os-버전&gt;/**

    예를 들어:
  
    **TestApp/bin/iPhone/Release/device-builds/iphone8.4-11.3.1/**

- 장치별 빌드를 사용 하도록 설정 하지 않은 경우. dSYM는 다음 디렉터리에서 찾을 수 있습니다.

    **&lt;프로젝트 디렉터리&gt;/l i&lt;/&gt;플랫폼/구성&lt;&gt;/**

    예:

    **TestApp/bin/iPhone/Release/**

> [!NOTE]
> 빌드 프로세스의 일부로 Visual Studio 2017는 Mac 빌드 호스트에서 Windows로 dSYM 파일을 복사 합니다. Windows에 dSYM 파일이 표시 되지 않으면 [ipa 파일을 만들기](~/ios/deploy-test/app-distribution/ipa-support.md)위해 앱의 빌드 설정을 구성 했는지 확인 합니다.

## <a name="see-also"></a>참고자료

- [Symbolicating iOS 크래시 파일 (Xamarin.ios)](https://www.jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [전문가가 제공 자세히 iOS 응용 프로그램 크래시 로그](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)

