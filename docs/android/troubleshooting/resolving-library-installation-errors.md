---
title: 라이브러리 설치 오류 해결
description: 경우에 따라 Android 지원 라이브러리를 설치 하는 동안 오류가 발생할 수 있습니다. 이 가이드에서는 몇 가지 일반적인 오류에 대 한 대안을 제공 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/14/2018
ms.openlocfilehash: 2ef81cfda92a6497e69f27b0584a97996094b1a4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107127"
---
# <a name="resolving-library-installation-errors"></a>라이브러리 설치 오류 해결

_경우에 따라 Android 지원 라이브러리를 설치 하는 동안 오류가 발생할 수 있습니다. 이 가이드에서는 몇 가지 일반적인 오류에 대 한 대안을 제공 합니다._

## <a name="overview"></a>개요

Xamarin.Android 앱 프로젝트를 빌드하는 동안 Visual Studio 또는 Mac 용 Visual Studio를 다운로드 하 여 종속성 라이브러리를 설치할 때 빌드 오류가 발생할 수 있습니다. 이러한 오류 중 상당수는 네트워크 연결 문제, 파일 손상 또는 버전 관리 문제가 발생 합니다. 이 가이드는 가장 일반적인 지원 라이브러리 설치 오류를 설명 하 고 다시 빌드하기 앱 프로젝트를 가져오고 이러한 문제를 해결 하는 단계를 제공 합니다. 

 
 
## <a name="errors-while-downloading-m2repository"></a>M2Repository를 다운로드 하는 동안 오류

표시 될 수 있습니다 **m2repository** Android 지원 라이브러리 또는 Google Play 서비스 NuGet 패키지를 참조할 때 오류가 발생 합니다. 오류 메시지는 다음과 유사합니다.

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

이 예제는 **android\_m2repository\_r16**와 같은 다른 버전에 대 한이 오류 메시지가 표시 될 수 있습니다 하지만 **android\_m2repository\_r18**  나 **android\_m2repository\_r25**합니다. 



### <a name="automatic-recovery-from-m2repository-errors"></a>M2repository 오류에서 자동 복구 

종종 문제가 있는 라이브러리를 삭제 하 고 다음이 단계에 따라 다시 작성 하 여이 문제를 해결할 수 있습니다. 

1. 컴퓨터에 지원 라이브러리 디렉터리로 이동 합니다.

    -   Windows, 지원 라이브러리에 위치 **c:\\사용자가\\_username_\\AppData\\로컬\\Xamarin**합니다. 

    -   Mac OS X, 지원 라이브러리에 위치 **/Users/_사용자 이름_/.local/share/Xamarin**합니다. 

2. 오류 메시지에 해당 하는 라이브러리 및 버전 폴더를 찾습니다. 예를 들어, 위의 오류 메시지에 대 한 라이브러리 및 버전 폴더에 위치한 **Android.Support.v4\\22.2.1**:

    [![22.2.1의 예에서는 폴더 위치로 지원 라이브러리](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. 버전 폴더의 내용을 삭제 합니다. 제거 해야 합니다 **.zip** 파일 뿐만 **콘텐츠** 및 **embedded** 이 폴더 안에 있는 하위 디렉터리입니다. 파일 및이 스크린샷에 표시 된 하위 디렉터리 위에 표시 된 예제 오류 메시지에 대 한 (**콘텐츠**를 **embedded**, 및 **android_m2repository_r16.zip**)를 삭제 됩니다.

    [![22.2.1의 예에서는 콘텐츠 라이브러리 폴더를 지원합니다.](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   것은 삭제 해야 합니다 *전체* 이 폴더의 내용을 합니다. 이 폴더 초기값으로 포함할 수 있지만 "없음" **android\_m2repository\_r16.zip** 파일을이 파일 되었을 부분적으로 다운로드 하거나 손상 되었습니다.

4. 프로젝트를 다시 빌드하고 &ndash; 이렇게 하면 빌드 프로세스를 다시 누락 된 라이브러리를 다운로드 합니다.

대부분의 경우에서 이러한 단계는 빌드 오류를 해결 하 고 계속할 수 있도록 됩니다. 이 라이브러리를 삭제 하는 경우에 빌드 오류가 해결 되지 않으면, 수동으로 다운로드 하 고 설치 해야 합니다 **android\_m2repository\_r_nn_.zip** 다음 섹션에 설명 된 대로 파일입니다. 



### <a name="manually-downloading-m2repository"></a>수동으로 m2repository 다운로드

위의 자동 복구 단계를 사용 하려고 했지만 아직 빌드 오류가 발생 하는 경우 직접 다운로드할 수 있습니다 합니다 **android\_m2repository\_r_nn_.zip** (웹 브라우저를 사용 하 여) 파일을 따라 설치 다음 단계입니다. 이 절차 개발 컴퓨터에서 인터넷에 액세스할 필요가 없지만 다른 컴퓨터를 사용 하 여 보관 파일을 다운로드할 수 있습니다 하는 경우에 유용 합니다. 

1.  다운로드 합니다 **android\_m2repository\_r_nn_.zip** 오류 메시지에 해당 하는 파일 &ndash; (각 링크의 해당 MD5 해시와 함께 다음 목록의 링크가 제공 됩니다 URL):

    -   [android\_m2repository\_r33.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    -   [android\_m2repository\_r32.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    -   [android\_m2repository\_r31.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99A8907CE2324316E754A95E4C2D786E

    -   [android\_m2repository\_r30.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    -   [android\_m2repository\_r29.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2A3A8A6D6826EF6CC653030E7D695C41

    -   [android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    -   [android\_m2repository\_r27.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    -   [android\_m2repository\_r26.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157FC1C311BB36420C1D8992AF54A4D

    -   [android\_m2repository\_r25.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    -   [android\_m2repository\_r24.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    -   [android\_m2repository\_r23.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    -   [android\_m2repository\_r22.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    -   [android\_m2repository\_r21.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    -   [android\_m2repository\_r20.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650E58DF02DB1A832386FA4A2DE46B1A

    -   [android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    -   [android\_m2repository\_r18.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    -   [android\_m2repository\_r17.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    -   [android\_m2repository\_r16.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    경우는 **m2repository** 보관 표시 되지 않습니다이 표의 앞에 추가 하 여 다운로드 URL을 만들 수 있습니다 **https://dl-ssl.google.com/android/repository/** 의 이름으로는 **m2repository** 를 다운로드 합니다. 사용 예를 들어 **https://dl-ssl.google.com/android/repository/android\_m2repository\_r10.zip** 다운로드 **android\_m2repository\_r10.zip**합니다.

2.  위의 표에 나와 있는 것 처럼 다운로드 URL의 해당 MD5 해시를 파일 이름 바꾸기 예를 들어 다운로드 한 **android\_m2repository\_r25.zip**를 바꾸거나 **0B3F1796C97C707339FB13AE8507AF50.zip**합니다. 다운로드 한 파일의 다운로드 URL에 대 한 MD5 해시 테이블에 표시 되지 않는 경우 사용할 수 있습니다는 [온라인 MD5 생성기](http://www.webconfs.com/online-md5-generator.php) URL MD5 해시 문자열을 변환할 대상입니다. 

3.  Xamarin에 파일을 복사 **zips** 폴더: 

    -   Windows에서이 폴더에 위치한 **c:\\사용자\\***username***\\AppData\\로컬\\Xamarin\\zips**합니다. 

    -   Mac OS X에서이 폴더에 위치한 **/Users/***사용자 이름***/.local/share/Xamarin/zips**합니다. 

    예를 들어 다음 스크린샷은 결과 보여 줍니다 때 **android\_m2repository\_r16.zip** 을 다운로드 하 고 Windows에서 다운로드 URL의 MD5 해시로 바뀌었습니다.

    [![0595E577D19D31708195A83087881EE6.zip를 바꿀 r16.zip 리포지토리의 예제](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)


이 절차는 빌드 오류를 해결 되지 않으면 수동으로 다운로드 해야 합니다 **android\_m2repository\_r_nn_.zip** 파일, 압축을 및 다음 섹션에 설명 된 대로 해당 콘텐츠를 설치 합니다. 


### <a name="manually-downloading-and-installing-m2repository-files"></a>수동으로 다운로드 하 고 m2repository 파일 설치

복구 하기 위한 완전 하 게 수동 프로세스 **m2repository** 다운로드 오류 때는 합니다 **android\_m2repository\_r_nn_.zip** (웹 브라우저 사용), 파일의 압축을 푸는 및 해당 콘텐츠를 컴퓨터에 지원 라이브러리 디렉터리에 복사 합니다. 다음 예제에서이 오류 메시지에서 복구 됩니다 것: 

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

다음 단계를 사용 하 여 다운로드 **m2repository** 하 고 해당 콘텐츠를 설치 합니다.

1.  오류 메시지에 해당 하는 라이브러리 폴더의 내용을 삭제 합니다. 예를 들어, 위의 오류 메시지에는 삭제할 내용의 **c:\\사용자가\\***username***\\AppData\\로컬\\Xamarin\\ Android.Support.v4\\23.1.1.0**합니다. 
    앞에서 설명한 대로이 디렉터리의 전체 내용을 삭제 해야 합니다.

    [![포함 된 콘텐츠 삭제는 23.1.1.0 android_m2repository 폴더 및 폴더](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2.  다운로드 합니다 **android\_m2repository\_r_nn_.zip** 파일 오류에 해당 하는 Google에서 메시지 (링크에 대 한 이전 섹션의 표 참조).

3.  이 추출한 **.zip** 모든 위치 (예: 바탕 화면)에 보관 합니다. 이름에 해당 하는 디렉터리를 만들어야 합니다 **.zip** 보관 합니다. 이 디렉터리 내 라는 하위 디렉터리 찾아야 **m2repository**: 

    [![m2repository 폴더에 추출 된 zip 보관 파일](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4.  1 단계에서 제거 하는 버전이 지정 된 라이브러리 디렉터리에서 다시 만듭니다는 **콘텐츠** 하 고 **포함 된** 하위 디렉터리입니다. 예를 들어 다음 스크린샷에서 보여 줍니다 **콘텐츠** 하 고 **embedded** 하위 디렉터리에 생성 되는 **23.1.1.0** 폴더에 대 한 **android \_m2repository\_r25.zip**: 

    [![콘텐츠 및 포함 된 폴더는 23.1.1.0 작성 폴더](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5.  복사본 **m2repository** 에서 추출한 **.zip** 에 **콘텐츠** 이전 단계에서 만든 디렉터리: 

    [![M2repository 23.1.1.0/content 폴더로 복사의 스크린샷](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6.  추출한 **.zip** 디렉터리 찾아보기 **m2repository\\com\\android\\지원\\지원 v4** 하 고 해당 폴더를 엽니다 위에서 만든 버전 번호 (이 예제의 **23.1.1**):

    [![Support-v4/23.1.1 폴더에 포함 된 파일의 예제 목록](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7.  이 폴더에 모든 파일을 복사 합니다 **embedded** 4 단계에서 만든 디렉터리:

    [![23.1.1.0/embedded 폴더에 복사 하는 파일의 예](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8.  모든 파일 복사를 확인 합니다. **embedded** directory 이제 포함 파일 등 **.jar**, **.aar**, 및 **.pom**합니다.

9.  추출 된 콘텐츠의 압축을 풉니다 **.aar** 파일을 **embedded** 디렉터리입니다. Windows를에 추가 **.zip** 확장을를 **.aar** 파일을 열고 콘텐츠를 복사 합니다 **포함 된** 디렉터리입니다.
    MacOS에서 압축을 풉니다를 **.aar** 사용 하 여 파일을 **압축을 풉니다** 터미널 명령을 (예를 들어 **file.aar의 압축을 풉니다**).

이 시점에서 누락 된 구성 요소를 수동으로 설치한 및 프로젝트가 오류 없이 빌드되어야 합니다. 그렇지 않은 경우 다운로드 했는지도 확인 합니다 **m2repository** **.zip** 정확 하 게 오류 메시지의 버전에 해당 하는 버전을 보관 하 고에서 해당 콘텐츠를 설치 했는지 확인 합니다 위의 단계에 설명 된 대로 위치를 수정 합니다. 



## <a name="summary"></a>요약 

이 문서에서는 자동으로 다운로드 하 고 종속성 라이브러리를 설치 하는 동안 수행 될 수 있는 일반적인 오류에서 복구 하는 방법을 설명 합니다. 문제가 있는 라이브러리를 삭제 하 고 다시 다운로드 하 고 다시 라이브러리를 설치 하는 방법으로 프로젝트를 다시 작성 하는 방법을 설명 합니다. 라이브러리를 다운로드 하 고에 설치 하는 방법을 설명 합니다 **zips** 폴더입니다. 또한 수동으로 다운로드 하 고 자동 방법을 통해 해결할 수 없는 문제를 해결 하는 방법으로 필요한 파일을 설치 하는 좀 더 절차를 설명 합니다. 
