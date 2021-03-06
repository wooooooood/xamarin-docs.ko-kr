---
title: 라이브러리 설치 오류 해결
description: 경우에 따라 Android 지원 라이브러리를 설치하는 동안 오류가 발생할 수 있습니다. 이 가이드에서는 몇 가지 일반적인 오류에 대한 해결 방법을 제공합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/14/2018
ms.openlocfilehash: e99e12aa73374cc8145fad0eb5aad7f3259e0ca2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "76724301"
---
# <a name="resolving-library-installation-errors"></a>라이브러리 설치 오류 해결

_경우에 따라 Android 지원 라이브러리를 설치하는 동안 오류가 발생할 수 있습니다. 이 가이드에서는 몇 가지 일반적인 오류에 대한 해결 방법을 제공합니다._

## <a name="overview"></a>개요

Xamarin.Android 앱 프로젝트를 빌드하는 동안 Visual Studio 또는 Mac용 Visual Studio 종속성 라이브러리를 다운로드하여 설치하려고 할 때 빌드 오류가 발생할 수 있습니다. 이러한 오류의 상당수는 네트워크 연결 문제, 파일 손상 또는 버전 관리 문제로 인해 발생합니다. 이 가이드에서는 가장 일반적인 지원 라이브러리 설치 오류에 대해 설명하고, 이러한 문제를 해결하고, 앱 프로젝트 빌드를 다시 시작하는 단계를 제공합니다.

## <a name="errors-while-downloading-m2repository"></a>m2Repository 다운로드 중 오류 발생

Android 지원 라이브러리 또는 Google Play 서비스의 NuGet 패키지를 참조할 때 **m2repository** 오류가 표시될 수 있습니다. 오류 메시지는 다음과 유사합니다.

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

이 예제에서는 **android\_m2repository\_r16**에 대한 것이지만 **android\_m2repository\_r18** 또는 **android\_m2repository\_r25**와 같은 다른 버전에 대해서도 동일한 오류 메시지가 표시될 수 있습니다.

### <a name="automatic-recovery-from-m2repository-errors"></a>m2repository 오류로부터 자동 복구

이러한 단계에 따라 문제가 있는 라이브러리를 삭제하고 다시 빌드하면 이 문제를 해결할 수 있는 경우가 많습니다.

1. 컴퓨터의 지원 라이브러리 디렉터리로 이동합니다.

    - Windows에서 지원 라이브러리는 **C:\\Users\\_username_\\AppData\\Local\\Xamarin**에 있습니다.

    - Mac OS X에서 지원 라이브러리는 **/Users/_username_/.local/share/Xamarin**에 있습니다.

2. 오류 메시지에 해당하는 라이브러리 및 버전 폴더를 찾습니다. 예를 들어 위의 오류 메시지에 대한 라이브러리 및 버전 폴더는 **Android.Support.v4\\22.2.1**에 있습니다.

    [![22.2.1 지원 라이브러리에 대한 예제 폴더 위치](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. 버전 폴더의 내용을 삭제합니다. 이 폴더 내에서 **.zip** 파일과 **content** 및 **embedded** 하위 디렉터리를 제거합니다. 위에 표시된 예제 오류 메시지의 경우 이 스크린샷에 표시된 파일 및 하위 디렉터리(**content**, **embedded** 및 **android_m2repository_r16.zip**)는 삭제됩니다.

    [![22.2.1 지원 라이브러리 폴더의 예제 내용](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   이 폴더의 *전체* 콘텐츠를 삭제해야 합니다. 이 폴더에는 처음에 "missing" **android\_m2repository\_r16.zip** 파일이 포함되어 있지만 이 파일은 부분적으로 다운로드되었거나 손상되었을 수 있습니다.

4. &ndash; 프로젝트를 다시 빌드하면 빌드 프로세스에서 누락된 라이브러리를 다시 다운로드하게 됩니다.

대부분의 경우 이러한 단계를 수행하면 빌드 오류가 해결되어 계속 진행할 수 있습니다. 이 라이브러리를 삭제해도 빌드 오류가 해결되지 않으면 다음 섹션에 설명된 대로 **android\_m2repository\_r_nn_.zip** 파일을 수동으로 다운로드하여 설치해야 합니다.

### <a name="manually-downloading-m2repository"></a>수동으로 m2repository 다운로드

위의 자동 복구 단계를 사용하려고 했지만 빌드 오류가 계속 발생하는 경우 **android\_m2repository\_r_nn_.zip** 파일을 수동으로 다운로드(웹 브라우저 사용)하여 다음 단계에 따라 설치할 수 있습니다. 이 절차는 개발 컴퓨터에서 인터넷에 액세스할 수 없지만 다른 컴퓨터를 사용하여 보관 파일을 다운로드할 수 있는 경우에도 유용합니다.

1. 오류 메시지 &ndash; 링크에 해당하는 **android\_m2repository\_r_nn_.zip** 파일을 다운로드합니다(각 링크 URL의 해당 MD5 해시와 함께).

    - [android\_m2repository\_r33.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    - [android\_m2repository\_r32.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    - [android\_m2repository\_r31.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99A8907CE2324316E754A95E4C2D786E

    - [android\_m2repository\_r30.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    - [android\_m2repository\_r29.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2A3A8A6D6826EF6CC653030E7D695C41

    - [android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    - [android\_m2repository\_r27.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    - [android\_m2repository\_r26.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157FC1C311BB36420C1D8992AF54A4D

    - [android\_m2repository\_r25.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    - [android\_m2repository\_r24.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    - [android\_m2repository\_r23.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    - [android\_m2repository\_r22.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    - [android\_m2repository\_r21.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    - [android\_m2repository\_r20.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650E58DF02DB1A832386FA4A2DE46B1A

    - [android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    - [android\_m2repository\_r18.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    - [android\_m2repository\_r17.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    - [android\_m2repository\_r16.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    **m2repository** 보관 파일이 표시되지 않으면 다운로드할 **m2repository** 이름에 `https://dl-ssl.google.com/android/repository/`를 앞에 붙여 다운로드 URL을 만들 수 있습니다. 예를 들어 **https://dl-ssl.google.com/android/repository/android\_m2repository\_r10.zip** 를 사용하여 **android\_ m2repository\_ r10.zip**을 다운로드합니다.

2. 위의 표와 같이 파일 이름을 다운로드 URL의 해당 MD5 해시로 바꿉니다. 예를 들어 **android\_m2repository\_r25.zip**을 다운로드한 경우 **0B3F1796C97C707339FB13AE8507AF50.zip**으로 이름을 바꿉니다. 다운로드한 파일의 다운로드 URL에 대한 MD5 해시가 표에 표시되지 않는 경우 [온라인 MD5 생성기](http://www.webconfs.com/online-md5-generator.php)를 사용하여 URL을 MD5 해시 문자열로 변환할 수 있습니다.

3. 파일을 Xamarin **zips** 폴더에 복사합니다.

    - Windows에서 이 폴더는 **C:\\Users\\***username***\\AppData\\Local\\Xamarin\\zips**에 있습니다.

    - Mac OS X에서 이 폴더는 **/Users/***username***/.local/share/Xamarin/zips**에 있습니다.

    예를 들어 다음 스크린샷에서는 **android\_m2repository\_r16.zip**을 다운로드하고 Windows에서 다운로드 URL의 MD5 해시로 이름을 바꿀 때의 결과를 보여줍니다.

    [![0595E577D19D31708195A83087881EE6.zip으로 이름이 변경되는 r16.zip 리포지토리의 예](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)

이 절차로 오류가 해결되지 않으면 **android\_m2repository\_r_nn_.zip** 파일을 수동으로 다운로드하여 압축을 풀고 다음 섹션에 설명된 대로 콘텐츠를 설치해야 합니다.

### <a name="manually-downloading-and-installing-m2repository-files"></a>수동으로 m2repository Files 다운로드 및 설치

**m2repository** 오류에서 복구하기 위한 완전한 수동 프로세스는 **android\_m2repository\_r_nn_.zip** 파일을 다운로드(웹 브라우저 사용)하여 압축을 풀고 컴퓨터의 지원 라이브러리 디렉터리에 콘텐츠를 복사하는 것입니다. 다음 예제에서는 이 오류 메시지를 복구합니다.

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

다음 단계에 따라 **m2repository**를 다운로드하고 해당 콘텐츠를 설치합니다.

1. 오류 메시지에 해당하는 라이브러리 폴더의 내용을 삭제합니다. 예를 들어 위의 오류 메시지에서 **C:\\Users\\***username***\\AppData\\Local\\Xamarin\\Android.Support.v4\\23.1.1.0**의 콘텐츠를 삭제합니다.
    앞에서 설명한 대로 이 디렉터리의 전체 내용을 삭제해야 합니다.

    [![23.1.1.0 폴더에서 콘텐츠, 임베디드 및 android_m2repository 폴더 삭제](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2. 오류 메시지에 해당하는 **android\_m2repository\_r_nn_.zip** 파일을 Google에서 다운로드합니다(링크는 이전 섹션의 표 참조).

3. 이 **.zip** 보관 파일을 모든 위치(예: 데스크톱)에서 추출합니다. 이렇게 하면 **.zip** 보관 파일의 이름에 해당하는 디렉터리가 만들어집니다. 이 디렉터리 내에서 **m2repository**라는 하위 디렉터리를 찾아야 합니다.

    [![압축을 푼 zip 보관 파일에 있는 m2repository 폴더](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4. 1단계에서 제거한 버전이 지정된 라이브러리 디렉터리에서 **content** 및 **embedded** 하위 디렉터리를 다시 만듭니다. 예를 들어 다음 스크린샷에서는 **android\_m2repository\_r25.zip**의 **23.1.1.0** 폴더에서 생성되고 있는 **content** 및 **embedded** 하위 디렉터리를 보여줍니다.

    [![23.1.1.0 폴더에 콘텐츠 및 포함된 폴더 만들기](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5. 추출된 **.zip**에서 **m2repository**를 이전 단계에서 만든 **콘텐츠** 디렉터리에 복사합니다.

    [![23.1.1.0/content 폴더로 복사된 m2repository의 스크린샷](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6. 추출된 **.zip** 디렉터리에서 **m2repository\\com\\android\\support\\support-v4**로 이동하여 위에서 만든 버전 번호에 해당하는 폴더를 엽니다(이 예제에서는 **23.1.1**).

    [![support-v4/23.1.1 폴더에 포함된 파일의 예제 목록](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7. 이 폴더의 모든 파일을 4단계에서 만든 **embedded** 디렉터리에 복사합니다.

    [![23.1.1.0/embedded 폴더에 복사된 파일의 예제](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8. 모든 파일이 복사되었는지 확인합니다. 이제 **embedded** 디렉터리에는 **jar**, **.aar** 및 **.pom** 등의 파일이 포함되어야 합니다.

9. 추출된 **.aar** 파일의 콘텐츠를 **embedded** 디렉터리에 압축을 풉니다. Windows에서 **.zip** 확장명을 **.aar** 파일에 추가하고 연 다음, 해당 콘텐츠를 **embedded** 디렉터리에 복사합니다.
    macOS에서 터미널의 **unzip** 명령을 사용하여 **.aar** 파일의 압축을 풉니다(예: **unzip file.aar**).

이때 누락된 구성 요소를 수동으로 설치하면 프로젝트가 오류 없이 빌드될 수 있습니다. 그렇지 않은 경우 오류 메시지의 버전과 정확히 일치하는 **m2repository** **.zip** 보관 파일 버전을 다운로드했는지 확인하고 위의 단계에서 설명한 대로 올바른 위치에 해당 콘텐츠를 설치했는지 확인합니다.

## <a name="summary"></a>요약

이 문서에서는 종속성 라이브러리의 자동 다운로드 및 설치 중에 발생하는 일반적인 오류를 복구하는 방법을 설명했습니다. 문제 라이브러리를 삭제하고 라이브러리를 다시 다운로드하고, 다시 설치하는 방법으로 프로젝트를 다시 빌드하는 방법을 설명했습니다.
라이브러리를 다운로드하여 **zips** 폴더에 설치하는 방법을 설명했습니다. 또한 자동으로 해결할 수 없는 문제를 해결하는 방법으로 필요한 파일을 수동으로 다운로드하여 설치하는 절차를 추가로 설명했습니다.
