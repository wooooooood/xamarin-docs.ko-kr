---
title: 라이브러리 설치 오류 해결
description: 경우에 따라 Android 지원 라이브러리를 설치 하는 동안 오류가 발생할 수 있습니다. 이 가이드는 몇 가지 일반적인 오류에 대 한 대안을 제공합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2018
ms.openlocfilehash: 6f280a90994ff40ebd8a07d2cab49ddc2b3d6ca1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="resolving-library-installation-errors"></a>라이브러리 설치 오류 해결

_경우에 따라 Android 지원 라이브러리를 설치 하는 동안 오류가 발생할 수 있습니다. 이 가이드는 몇 가지 일반적인 오류에 대 한 대안을 제공합니다._

## <a name="overview"></a>개요

Xamarin.Android 응용 프로그램 프로젝트를 빌드하는 동안 Visual Studio 또는 Mac 용 Visual Studio 다운로드 하 여 종속성 라이브러리를 설치 하려고 할 때 빌드 오류가 발생할 수 있습니다. 이러한 오류 중 대부분는 네트워크 연결 문제, 파일 손상 또는 버전 관리 문제가 발생 합니다. 이 가이드는 가장 일반적인 지원 라이브러리 설치 오류를 설명 하 고 응용 프로그램 프로젝트 다시 빌드를 살펴보고 이러한 문제를 해결 하는 단계를 제공 합니다. 

 
 
## <a name="errors-while-downloading-m2repository"></a>M2Repository 다운로드 하는 동안 오류

표시 될 수 있습니다 **m2repository** Android 지원 라이브러리 또는 Google Play 서비스의 NuGet 패키지를 참조할 때 오류 발생 합니다. 오류 메시지는 다음과 유사합니다.

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

이 예제에는 **android\_m2repository\_r16**와 같은 다른 버전에 대 한이 오류 메시지가 표시 될 수 있습니다 하지만 **android\_m2repository\_r18**  또는 **android\_m2repository\_r25**합니다. 



### <a name="automatic-recovery-from-m2repository-errors"></a>M2repository 오류 로부터 자동 복구 

대개 문제가 있는 라이브러리를 삭제 하 고 다음이 단계에 따라 다시 작성 하 여이 문제를 해결할 수 있습니다. 

1. 컴퓨터에 지원 라이브러리 디렉터리로 이동 합니다.

    -   Windows에서는 지원 라이브러리에는 **c:\\사용자\\_username_\\AppData\\로컬\\Xamarin**합니다. 

    -   지원 라이브러리에는 Mac OS X에서 **/Users/_username_/.local/share/Xamarin**합니다. 

2. 오류 메시지에 해당 하는 라이브러리 및 버전 폴더를 찾습니다. 예를 들어 위의 오류 메시지에 대 한 라이브러리 및 버전 폴더에 위치한 **Android.Support.v4\\22.2.1**:

    [![22.2.1에 대 한 폴더 위치 예제 라이브러리를 지원 합니다.](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. 버전 폴더의 내용을 삭제 합니다. 제거는 **.zip** 파일으로 **콘텐츠** 및 **포함** 이 폴더 내의 하위 디렉터리입니다. 파일 및 하위가이 스크린 샷에 표시 된 위에 표시 된 예제 오류 메시지에 대 한 (**콘텐츠**, **포함**, 및 **android_m2repository_r16.zip**)입니다. 삭제할 수 있습니다.

    [![예제 내용의 22.2.1 라이브러리 폴더를 지원합니다.](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   삭제 하는 *전체* 이 폴더의 내용을 합니다. 이 폴더 초기값으로 포함할 수 있지만 "없음" **android\_m2repository\_r16.zip** 파일을이 파일이 되었을 수 있습니다 부분적으로 다운로드 하거나 손상 되었습니다.

4. 프로젝트를 다시 빌드해야 &ndash; 그렇게 하면 빌드 프로세스를 다시 누락 된 라이브러리를 다운로드 합니다.

대부분의 경우에서 이러한 단계는 빌드 오류를 해결 하 고 단계로 진행 됩니다. 이 라이브러리를 삭제 하는 경우에 빌드 오류가 해결 되지 않으면를 수동으로 다운로드 하 고 설치 해야는 **android\_m2repository\_r_nn_.zip** 다음 섹션에 설명 된 대로 파일입니다. 



### <a name="manually-downloading-m2repository"></a>M2repository을 수동으로 다운로드

위의 자동 복구 단계를 사용 하려고 했지만 하 고 빌드 오류에 아직 직접 다운로드할 수 있습니다는 **android\_m2repository\_r_nn_.zip** (웹 브라우저를 사용 하 여) 파일 및 설치에 따라 다음 단계입니다. 이 절차는 다른 컴퓨터를 사용 하 여 보관 파일을 다운로드 할 않지만 개발 컴퓨터에 인터넷에 연결 되어 있지 않은 경우에 유용 합니다. 

1.  다운로드는 **android\_m2repository\_r_nn_.zip** 오류 메시지에 해당 하는 파일 &ndash; (각 링크에 해당 MD5 해시와 함께 다음 목록에 링크가 제공 됩니다 URL):

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

    경우는 **m2repository** 보관 파일에에서 표시 되지 않습니다이 테이블 앞에 추가 하 여 다운로드 URL을 만들 수 있습니다 **https://dl-ssl.google.com/android/repository/** 의 이름에는 **m2repository** 를 다운로드 합니다. 사용 예를 들어  **https://dl-ssl.google.com/android/repository/android \_m2repository\_r10.zip** 다운로드 **android\_m2repository\_r10.zip**합니다.

2.  위 표에 나와 있는 것 처럼 다운로드 URL에 해당 MD5 해시를 파일을 이름을 바꿉니다. 예를 들어, 다운로드 한 경우 **android\_m2repository\_r25.zip**,이 파일 이름을 **0B3F1796C97C707339FB13AE8507AF50.zip**합니다. 다운로드 된 파일의 다운로드 URL에 대 한 MD5 해시 테이블에 표시 되지 않는 경우 사용할 수 있습니다는 [온라인 MD5 생성기](http://www.webconfs.com/online-md5-generator.php) URL MD5 해시 문자열로 변환 합니다. 

3.  Xamarin에 파일을 복사 **이동** 폴더: 

    -   Windows에서는이 폴더에 위치한 **c:\\사용자\\***username***\\AppData\\로컬\\Xamarin\\이동**합니다. 

    -   이 폴더에는 Mac OS X에서 **/Users/***username***/.local/share/Xamarin/zips**합니다. 

    예를 들어 다음 스크린 샷에서 결과 보여 줍니다 때 **android\_m2repository\_r16.zip** 다운로드 되 고 Windows에서의 다운로드 URL의 MD5 해시로 변경 합니다.

    [![0595E577D19D31708195A83087881EE6.zip로 변경 되 고 r16.zip 저장소의 예](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)


이 절차에 빌드 오류가 해결 되지 않으면를 수동으로 다운로드 해야는 **android\_m2repository\_r_nn_.zip** 파일, 압축을 푼 및 다음 섹션에 설명 된 대로 해당 콘텐츠를 설치 합니다. 


### <a name="manually-downloading-and-installing-m2repository-files"></a>수동으로 다운로드 및 m2repository 파일 설치

복구 하기 위한 완벽 하 게 수동 프로세스 **m2repository** 오류 하므로 다운로드는 **android\_m2repository\_r_nn_.zip** (웹 브라우저 사용), 파일 압축을 푸는 중 한 컴퓨터에 지원 라이브러리 디렉터리로 해당 내용을 복사 합니다. 다음 예제에서는이 오류 메시지에서 복구 합니다 했습니다. 

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

다운로드 하려면 다음 단계를 사용 하 여 **m2repository** 하 고 해당 콘텐츠를 설치 합니다.

1.  오류 메시지에 해당 하는 라이브러리 폴더의 내용을 삭제 합니다. 예를 들어 위의 오류 메시지에는 내용을 삭제의 **c:\\사용자\\***username***\\AppData\\로컬\\Xamarin\\ Android.Support.v4\\23.1.1.0**합니다. 
    앞에서 설명한 대로이 디렉터리의 전체 내용을 삭제 해야 합니다.

    [![포함 된 콘텐츠를 삭제 하 고는 23.1.1.0에서 android_m2repository 폴더 폴더](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2.  다운로드는 **android\_m2repository\_r_nn_.zip** 파일 오류에 해당 하는 Google에서 메시지 (링크에 대 한 이전 섹션의 표 참조).

3.  이 추출한 **.zip** (예: 바탕 화면) 모든 위치에 보관 합니다. 이름에 해당 하는 디렉터리 만들어야는 **.zip** 보관 합니다. 이 디렉터리 내에서 라는 하위 디렉터리 찾아야 **m2repository**: 

    [![추출 된 zip 보관 파일에 m2repository 폴더](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4.  1 단계에서 제거 하는 버전이 지정 된 라이브러리 디렉터리에서 다시 만들기는 **콘텐츠** 및 **포함** 하위 디렉터리입니다. 예를 들어 다음 스크린 샷에서에서는 **콘텐츠** 및 **포함** 하위 디렉터리에 생성 되는 **23.1.1.0** 폴더에 대 한 **android \_m2repository\_r25.zip**: 

    [![콘텐츠 및 포함 된 폴더는 23.1.1.0 작성 폴더](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5.  복사 **m2repository** 에서 추출 된 **.zip** 에 **콘텐츠** 이전 단계에서 만든 디렉터리: 

    [![23.1.1.0/content 폴더에 복사 m2repository의 스크린샷](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6.  추출 된 **.zip** 디렉터리 찾아보기 **m2repository\\com\\android\\지원\\지원 v4** 하 고 해당 폴더를 엽니다 위에서 만든 버전 번호 (이 예제에서는 **23.1.1**):

    [![예제 샘플 support-v4/23.1.1 폴더에 포함 된 파일](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7.  이 폴더의 모든 파일 복사는 **포함** 4 단계에서 만든 디렉터리:

    [![23.1.1.0/embedded 폴더에 복사 된 파일의 예](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8.  통해 모든 파일이 복사 됩니다 있는지 확인 합니다. **포함** 디렉터리 파일 같은 포함 이제 **.jar**, **.aar**, 및 **.pom**합니다.

9.  추출 된 모든 압축을 풀 **.aar** 파일입니다. Windows에서는 추가 **.zip** 확장을는 **.aar** 파일을 마우스 오른쪽 단추로 클릭 한 다음 선택 **모두 추출...** , 다음 제거는 **.zip** 확장 합니다. MacOS 등에서 압축을 풀는 **.aar** 사용 하 여 파일은 **의 압축을 푸는** 명령에 터미널 (예를 들어 **file.aar의 압축을 푸는**).

이 시점에서 누락 된 구성 요소를 직접 설치 하 고 프로젝트가 오류 없이 빌드되어야 합니다. 그렇지 않은 경우 다운로드 한 확인은 **m2repository** **.zip** 정확 하 게 오류 메시지에 대 한 버전에 해당 하는 버전을 보관 하 고에서 해당 콘텐츠를 설치 했는지 확인 하십시오.는 위의 단계에 설명 된 대로 위치를 수정 합니다. 



## <a name="summary"></a>요약 

이 문서에 자동으로 다운로드 하 고 라이브러리 종속성의 설치 중에 발생할 수 있는 일반적인 오류에서 복구 하는 방법을 설명 합니다. 이 문제가 있는 라이브러리를 삭제 하 고 다시 다운로드 하 고 라이브러리를 다시 설치 하는 방법으로 프로젝트를 다시 작성 하는 방법을 설명 합니다. 라이브러리를 다운로드 하 고에 설치 하는 방법을 설명 하기는 **이동** 폴더입니다. 또한 수동으로 다운로드 하 고 자동 수단을 통해 해결할 수 없는 문제를 해결 하는 방법으로 필요한 파일 설치에 대 한 더욱 복잡된 한 절차가 대해서도 설명 합니다. 
