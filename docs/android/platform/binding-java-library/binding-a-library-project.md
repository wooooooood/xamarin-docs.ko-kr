---
title: Eclipse 라이브러리 프로젝트 바인딩
description: 이 연습에서는 Eclipse Android 라이브러리 프로젝트용 바인딩할 Xamarin.Android 프로젝트 템플릿을 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a5887748eda8af89b9215e3e121db592dfa73935
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116604"
---
# <a name="binding-an-eclipse-library-project"></a>Eclipse 라이브러리 프로젝트 바인딩

_이 연습에서는 Eclipse Android 라이브러리 프로젝트용 바인딩할 Xamarin.Android 프로젝트 템플릿을 사용 하는 방법에 설명 합니다._


## <a name="overview"></a>개요

하지만 합니다. AAR 파일은 점차 Android 라이브러리 배포에 대 한 표준 되 고, 일부 경우에서에 대 한 바인딩을 만드는 데 필요한 것을 *Android 라이브러리 프로젝트*합니다. Android 라이브러리 프로젝트가 공유할 수 있는 코드를 포함 하는 특별 한 Android 프로젝트와 Android 응용 프로그램 프로젝트에서 참조할 수 있는 리소스입니다. 일반적으로 바인딩할 Android 라이브러리 프로젝트를 Eclipse IDE에서 라이브러리를 만들 때.
이 연습에서는 Android 라이브러리 프로젝트를 만드는 방법의 예제를 제공 합니다. Eclipse 프로젝트의 디렉터리 구조에서 압축 합니다.

Android 라이브러리 프로젝트는 APK로 컴파일되지 않은 하며 자체적으로 하지 않습니다에 정규 Android 프로젝트에 다릅니다. 장치에 배포할 수 있습니다. 대신, Android 응용 프로그램 프로젝트에서 참조 하는 Android 라이브러리 프로젝트 것입니다. Android 응용 프로그램 프로젝트를 빌드할 때 Android 라이브러리 프로젝트를 먼저 컴파일됩니다. Android 응용 프로그램 프로젝트를 컴파일된 Android 라이브러리 프로젝트에 흡수 수와 코드 및 리소스를 배포에 대 한 APK에 포함 됩니다. 이 차이 때문에 Android 라이브러리 프로젝트용 바인딩 만들기는 Java에 대 한 바인딩을 만드는 것 보다 약간 다릅니다. JAR 또는 합니다. AAR 파일입니다.



## <a name="walkthrough"></a>연습

Java 바인딩 Xamarin.Android 프로젝트에서 Android 라이브러리 프로젝트를 사용 하려면 Eclipse에서 Android 라이브러리 프로젝트를 빌드하는 데 처음 필요한 것. 다음 스크린 샷에서 컴파일 후 한 Android 라이브러리 프로젝트의 예를 보여 줍니다. 

[![Eclipse에서 예제 라이브러리 프로젝트](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

Android 라이브러리 프로젝트에서 소스 코드를 임시 컴파일된 있는지 확인 합니다. JAR 파일인 **android mapviewballoons.jar**, 및 리소스에 복사 된 합니다 **bin/res/도출** 폴더입니다. 

Android 라이브러리 프로젝트를 Eclipse에서 컴파일 되었으면,이 다음 바인딩될 수 Xamarin.Android Java 바인딩 프로젝트를 사용 하 합니다. 첫 번째는 합니다. 포함 된 ZIP 파일을 만들어야 합니다 **bin** 및 **res** Android 라이브러리 프로젝트의 폴더입니다. 중간를 제거 하는 것이 중요 **도출** 하위 디렉터리에 상주 하는 리소스 수 있도록 **bin/res**합니다. 다음 스크린샷은 하나의 콘텐츠를 이러한. ZIP 파일: 

[![Android 라이브러리 프로젝트.zip의 내용](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

이 있습니다. ZIP 파일은 다음 스크린샷에 표시 된 대로 다음 Xamarin.Android Java 바인딩 프로젝트에 추가 됩니다.

[![Java 바인딩 프로젝트에 추가 하는 zip](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

빌드 동작 합니다. ZIP 파일에 자동으로 설정한 **LibraryProjectZip**합니다.

있는 경우. Android 라이브러리 프로젝트에 필요한 JAR 파일은에 추가 해야 합니다 **Jar** Java 바인딩 라이브러리 프로젝트의 폴더와 **빌드 작업** 로 **ReferenceJar**. 아래 스크린샷에서 이러한 예를 확인할 수 있습니다. 

[![빌드 ReferenceJar로 설정 하는 작업](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

이러한 단계가 완료 되 면이 문서의 앞부분에 설명 된 대로 Xamarin.Android Java 바인딩 프로젝트를 사용할 수 있습니다.

> [!NOTE]
> 지금은 다른 Ide에 Android 라이브러리 프로젝트를 컴파일할 수 없습니다. 기타 Ide에는의 파일을 동일한 디렉터리 구조를 만들 수 없습니다는 **bin** Eclipse 폴더입니다. 


## <a name="summary"></a>요약

이 문서에서는 Android 라이브러리 프로젝트를 바인딩하는 프로세스를 통해 안내 합니다. Eclipse에서 Android 라이브러리 프로젝트를 구축한 다음 zip 파일을 만든 합니다 **bin** 하 고 **res** Android 라이브러리 프로젝트의 폴더입니다. 다음으로, Xamarin.Android Java 바인딩 프로젝트를 만들려면이 zip을 사용 했습니다. 

