---
title: 라이브러리 설치 오류 해결
description: 일부 경우에 Android 지원 라이브러리를 설치 하는 동안 오류가 발생할 수 있습니다. 이 가이드에서는 몇 가지 일반적인 오류에 대 한 해결 방법을 제공 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/14/2018
ms.openlocfilehash: e99e12aa73374cc8145fad0eb5aad7f3259e0ca2
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724301"
---
# <a name="resolving-library-installation-errors"></a>라이브러리 설치 오류 해결

_일부 경우에 Android 지원 라이브러리를 설치 하는 동안 오류가 발생할 수 있습니다. 이 가이드에서는 몇 가지 일반적인 오류에 대 한 해결 방법을 제공 합니다._

## <a name="overview"></a>概述

Xamarin Android 앱 프로젝트를 빌드하는 동안 Visual Studio 또는 Mac용 Visual Studio 종속성 라이브러리를 다운로드 하 고 설치 하려고 할 때 빌드 오류가 발생할 수 있습니다. 이러한 오류의 상당수는 네트워크 연결 문제, 파일 손상 또는 버전 관리 문제로 인해 발생 합니다. 이 가이드에서는 가장 일반적인 지원 라이브러리 설치 오류에 대해 설명 하 고 이러한 문제를 해결 하 고 앱 프로젝트 빌드를 다시 시작 하는 단계를 제공 합니다.

## <a name="errors-while-downloading-m2repository"></a>M2Repository를 다운로드 하는 동안 오류 발생

Android 지원 라이브러리 또는 Google Play 서비스의 NuGet 패키지를 참조할 때 **m2repository** 오류가 표시 될 수 있습니다. 오류 메시지는 다음과 유사합니다.

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

이 예제는 **android\_m2repository\_r16**에 대 한 것 이지만 **android\_m2repository\_r18** 또는 **android\_m2repository\_r25**와 같은 다른 버전에 대해서도 동일한 오류 메시지가 표시 될 수 있습니다.

### <a name="automatic-recovery-from-m2repository-errors"></a>M2repository 오류에서 자동 복구

이러한 단계에 따라 문제 라이브러리를 삭제 하 고 다시 작성 하 여이 문제를 해결할 수 있는 경우가 많습니다.

1. 컴퓨터의 지원 라이브러리 디렉터리로 이동 합니다.

    - Windows에서 지원 라이브러리는 **C:\\사용자\\_사용자 이름_\\AppData\\로컬\\Xamarin**에 있습니다.

    - Mac OS X에서 지원 라이브러리는 **/_사용자/사용자 이름_/** a s/p r o r e r/p r o v e r s에 있습니다

2. 오류 메시지에 해당 하는 라이브러리 및 버전 폴더를 찾습니다. 예를 들어 위의 오류 메시지에 대 한 library 및 version 폴더는 **Android. Support. v4\\22.2.1**에 있습니다.

    [22.2.1 지원 라이브러리에 대 한 ![예제 폴더 위치](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. 버전 폴더의 내용을 삭제 합니다. **.Zip** 파일 뿐만 아니라이 폴더에 포함 된 **콘텐츠** 및 **포함** 된 하위 디렉터리도 제거 해야 합니다. 위에 표시 된 예제 오류 메시지의 경우이 스크린샷 (**content**, **embedded**및 **android_m2repository_r16 .zip**)에 표시 된 파일과 하위 디렉터리를 삭제 합니다.

    [22.2.1 지원 라이브러리 폴더의 ![예제 내용](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   이 폴더의 *전체* 콘텐츠를 삭제 하는 것이 중요 합니다. 尽管此文件夹可能最初包含框 **android\_m2repository\_r16.zip**文件，此文件可能已部分下载或已损坏。

4. &ndash; 프로젝트를 다시 빌드하면 빌드 프로세스에서 누락 된 라이브러리를 다시 다운로드 하 게 됩니다.

대부분의 경우 이러한 단계를 수행 하면 빌드 오류가 해결 되어 계속 진행할 수 있습니다. 이 라이브러리를 삭제 해도 빌드 오류가 해결 되지 않으면 다음 섹션에 설명 된 대로 **android\_m2repository\_r_nn_ .zip** 파일을 수동으로 다운로드 하 여 설치 해야 합니다.

### <a name="manually-downloading-m2repository"></a>수동으로 m2repository 다운로드

위의 자동 복구 단계를 사용 하 여 빌드 오류가 계속 발생 하는 경우에는 웹 브라우저를 사용 하 여 **android\_m2repository\_r_nn_ .zip** 파일을 수동으로 다운로드 하 여 다음 단계에 따라 설치할 수 있습니다. 이 절차는 개발 컴퓨터에서 인터넷에 액세스할 수 없지만 다른 컴퓨터를 사용 하 여 보관 파일을 다운로드할 수 있는 경우에도 유용 합니다.

1. 다음 목록 (각 링크 URL의 해당 MD5 해시와 함께) &ndash; 링크를 제공 하는 오류 메시지에 해당 하는 **android\_m2repository\_r_nn_ .zip** 파일을 다운로드 합니다.

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

    **M2repository** archive가이 테이블에 표시 되지 않으면 다운로드할 **m2repository** 이름 앞에 `https://dl-ssl.google.com/android/repository/`를 추가할 수 있습니다. 例如，使用 **https://dl-ssl.google.com/android/repository/android\_m2repository\_r10.zip** 若要下载 **android\_ m2repository\_ r10.zip**。

2. 위의 표에 나와 있는 것 처럼 다운로드 URL의 해당 MD5 해시에 파일 이름을 바꿉니다. 예를 들어 **android\_m2repository\_r25**를 다운로드 한 경우 이름을 **0B3F1796C97C707339FB13AE8507AF50**로 바꿉니다. 다운로드 한 파일의 다운로드 URL에 대 한 MD5 해시가 표에 표시 되지 않는 경우 [온라인 md5 생성기](http://www.webconfs.com/online-md5-generator.php) 를 사용 하 여 URL을 md5 해시 문자열로 변환할 수 있습니다.

3. Xamarin **zips** 폴더에 파일을 복사 합니다.

    - Windows에서이 폴더는 **C:\\Users\\***사용자 이름***\\AppData\\Local\\Xamarin\\zips**에 있습니다.

    - Mac OS X에서이 폴더는 **/***사용자/사용자 이름***/.local/share/Xamarin/zips**에 있습니다.

    예를 들어 다음 스크린샷은 **android\_m2repository\_r16** 를 다운로드 하 고 Windows에서 다운로드 URL의 MD5 해시로 이름을 바꿀 때의 결과를 보여 줍니다.

    [![0595E577D19D31708195A83087881EE6로 이름이 변경 되는 r16 리포지토리의 예입니다.](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)

이 절차를 수행 해도 빌드 오류가 해결 되지 않으면 **android\_m2repository\_r_nn_ .zip** 파일로 수동으로 다운로드 하 여 압축을 풀고 압축을 푼 후 다음 섹션에 설명 된 대로 해당 콘텐츠를 설치 해야 합니다.

### <a name="manually-downloading-and-installing-m2repository-files"></a>수동으로 m2repository 파일 다운로드 및 설치

**M2repository** 오류를 복구 하는 완전히 수동 프로세스는 **android\_m2repository\_r_nn_ .zip** 파일 (웹 브라우저를 사용 하 여)을 다운로드 하 고 압축을 푼 다음 해당 콘텐츠를 컴퓨터의 지원 라이브러리 디렉터리에 복사 하는 것입니다. 다음 예제에서는이 오류 메시지를 복구 합니다.

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

다음 단계를 사용 하 여 **m2repository** 를 다운로드 하 고 해당 콘텐츠를 설치 합니다.

1. 오류 메시지에 해당 하는 라이브러리 폴더의 내용을 삭제 합니다. 예를 들어 위의 오류 메시지에서 **C:\\사용자\\***사용자 이름***\\AppData\\Local\\Xamarin\\Android. 지원 v4\\** 의 콘텐츠를 삭제 합니다.
    앞에서 설명한 대로이 디렉터리의 전체 내용을 삭제 해야 합니다.

    [23.1.1.0 폴더에서 콘텐츠, 포함 및 android_m2repository 폴더를 삭제 ![](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2. Google에서 오류 메시지에 해당 하는 **android\_m2repository\_r_nn_ .zip** 파일을 다운로드 합니다 (링크는 이전 섹션의 표 참조).

3. 이 **.zip** 아카이브를 모든 위치 (예: 바탕 화면)로 추출 합니다. .Zip 보관 파일의 이름에 해당 하는 디렉터리를 만들어야 합니다 **.** 이 디렉터리 내에서 **m2repository**라는 하위 디렉터리를 찾아야 합니다.

    [압축을 푼 zip 보관 위치에 있는 ![m2repository 폴더](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4. 1 단계에서 제거한 버전 관리 라이브러리 디렉터리에서 **콘텐츠** 및 **포함** 된 하위 디렉터리를 다시 만듭니다. 예를 들어 다음 스크린샷은 **android\_m2repository\_r25**의 **23.1.1.0** 폴더에 생성 되는 **콘텐츠** 및 **포함 된** 하위 디렉터리를 보여 줍니다.

    [23.1.1.0 폴더에 콘텐츠 및 포함 된 폴더 만들기 ![](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5. 압축을 푼 **.zip** 에서 이전 단계에서 만든 **콘텐츠** 디렉터리로 **m2repository** 을 복사 합니다.

    [23.1.1.0/content 폴더로 복사 된 m2repository의 ![스크린샷](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6. 압축을 푼 **.zip** 디렉터리에서 **m2repository\\com\\android\\지원\\지원-v4** 로 이동 하 고 위에서 만든 버전 번호가 해당 하는 폴더 (이 예제에서는 **23.1.1**)를 엽니다.

    [![지원-v4/23.1.1 폴더에 포함 된 파일을 나열 하는 예제](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7. 이 폴더의 모든 파일을 4 단계에서 만든 **포함** 된 디렉터리에 복사 합니다.

    [23.1.1.0/embedded 폴더에 복사 된 파일의 예 ![](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8. 모든 파일이 복사 되었는지 확인 합니다. 이제 **포함** 된 디렉터리에 **jar**, **. aar**및 **. e m m**과 같은 파일이 포함 됩니다.

9. **Aar** 파일의 콘텐츠를 **포함** 된 디렉터리에 압축을 풉니다. Windows에서는 **.zip** 확장명을 **aar** 파일에 추가 하 고 열고 해당 내용을 **포함** 된 디렉터리에 복사 합니다.
    MacOS에서 터미널의 **압축 풀기** 명령을 사용 하 여 **aar** 파일의 압축을 풉니다 (예: **aar 압축 풀기**).

이제 누락 된 구성 요소를 수동으로 설치 하 고 프로젝트를 오류 없이 빌드해야 합니다. 그렇지 않은 경우 오류 메시지의 버전에 해당 하는 **m2repository** **보관 버전** 을 다운로드 했는지 확인 하 고 위의 단계에 설명 된 대로 올바른 위치에 해당 콘텐츠를 설치 했는지 확인 합니다.

## <a name="summary"></a>요약

이 문서에서는 종속성 라이브러리의 자동 다운로드 및 설치 중에 발생 하는 일반적인 오류에서 복구 하는 방법을 설명 했습니다. 문제 라이브러리를 삭제 하 고 라이브러리를 다시 다운로드 하 고 다시 설치 하는 방법으로 프로젝트를 다시 빌드하는 방법에 대해 설명 했습니다.
라이브러리를 다운로드 하 여 **zips** 폴더에 설치 하는 방법을 설명 했습니다. 또한 자동으로 해결할 수 없는 문제를 해결 하는 방법으로 필요한 파일을 수동으로 다운로드 하 여 설치 하는 절차를 추가로 설명 했습니다.
