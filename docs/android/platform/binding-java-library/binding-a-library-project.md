---
title: "Eclipse 라이브러리 프로젝트에 바인딩"
description: "이 연습에서는 Eclipse Android 라이브러리 프로젝트를 바인딩할 Xamarin.Android 프로젝트 템플릿을 사용 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2017
ms.openlocfilehash: 2048056415e0969e13e305b1dbba8bdb7ffabd30
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="binding-an-eclipse-library-project"></a>Eclipse 라이브러리 프로젝트에 바인딩

_이 연습에서는 Eclipse Android 라이브러리 프로젝트를 바인딩할 Xamarin.Android 프로젝트 템플릿을 사용 하는 방법을 설명 합니다._

<a name=overview />

## <a name="overview"></a>개요

하지만 합니다. AAR 파일이 Android 라이브러리 배포에 대 한 norm 증가 함에에 대 한 바인딩을 만들 필요는 일부 경우에는 *Android 라이브러리 프로젝트*합니다. Android 라이브러리 프로젝트는 공유 가능한 코드를 포함 하는 특별 한 Android 프로젝트와 Android 응용 프로그램 프로젝트에서 참조 될 수 있는 리소스는입니다. 일반적으로 바인딩할 있습니다 Android 라이브러리 프로젝트에 Eclipse IDE에서 라이브러리를 만들 때.
이 연습에서는 Android 라이브러리 프로젝트를 만드는 방법의 예를 제공 합니다. Eclipse 프로젝트의 디렉터리 구조에서 ZIP 합니다.

Android 라이브러리 프로젝트는는 APK 프로젝트로 컴파일되지 하며 자체적으로 폴링 되지 않는 한다는 점에서 일반적인 Android 프로젝트와에서 다른 장치에 배포할 수 없습니다. 대신, Android 라이브러리 프로젝트는 Android 응용 프로그램 프로젝트에서 참조 하도록 제공 됩니다. Android 응용 프로그램 프로젝트를 빌드할 때 Android 라이브러리 프로젝트는 먼저 컴파일됩니다. Android 응용 프로그램 프로젝트는 컴파일된 Android 라이브러리 프로젝트에 처리할 및 배포에 대 한 APK에 코드 및 리소스를 포함 됩니다. 이러한 차이로 인해 Android 라이브러리 프로젝트에 대 한 바인딩을 만들는 Java에 대 한 바인딩을 만들 때 보다 약간 다릅니다. JAR 또는 합니다. AAR 파일입니다.


<a name="Walkthrough" />

## <a name="walkthrough"></a>연습

Java 바인딩 Xamarin.Android 프로젝트에서 Android 라이브러리 프로젝트를 사용을 Eclipse에서 Android 라이브러리 프로젝트를 빌드하려면 첫 번째 필요 합니다. 다음 스크린 샷에서 컴파일 후 한 Android 라이브러리 프로젝트의 예를 보여 줍니다. 

[ ![Eclipse에서 예제 라이브러리 프로젝트](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png)

임시 Android 라이브러리 프로젝트에서 소스 코드에 컴파일된를 확인 합니다. JAR 파일인 **android mapviewballoons.jar**, 리소스에 복사 하 고는 **수치 연산res/bin/** 폴더입니다. 

Android 라이브러리 프로젝트를 Eclipse에서 컴파일되고 나면 것 다음 바인딩될 수는 Java 바인딩 Xamarin.Android 프로젝트를 사용 하 여 합니다. 첫 번째는 합니다. 포함 된 ZIP 파일이 생성 됩니다는 **bin** 및 **res** Android 라이브러리 프로젝트의 폴더입니다. 이 중간 제거 하는 중요 **수치 연산** 하위 디렉터리에 상주 하는 리소스를 **bin/res**합니다. 다음 스크린샷은 중 하나의 내용을 이러한 합니다. ZIP 파일: 

[ ![Android 라이브러리 프로젝트.zip의 내용](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png)

이 있습니다. ZIP 파일은 다음 스크린샷에 표시 된 것 처럼에 다음 Java 바인딩 Xamarin.Android 프로젝트에 추가 됩니다.

[ ![Zip 바인딩 Java 프로젝트에 추가](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png)

다음에 유의의 빌드 작업에서 합니다. ZIP 파일이 자동으로 설정 되어 **LibraryProjectZip**합니다.

있는 경우. Android 라이브러리 프로젝트에 필요한 JAR 파일은 추가 해야 하는 **단지** Java 바인딩 라이브러리 프로젝트의 폴더 및 **빌드 작업** 로 설정 **ReferenceJar**. 아래 스크린샷에서 이러한 예제를 볼 수 있습니다. 

[ ![빌드 ReferenceJar로 설정 하는 작업](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png)

다음이 단계 완료 되 면이 문서의 앞부분에 설명 된 대로 Java 바인딩 Xamarin.Android 프로젝트를 사용할 수 있습니다.

> [!NOTE]
> **참고**: 기타 Ide에서 Android 라이브러리 프로젝트를 컴파일이 이번에 지원 되지 않습니다. 기타 Ide에서 동일한 디렉터리 구조 또는 파일에 만들 수 있습니다는 **bin** Eclipse와 폴더입니다. 

<a name="Summary" /> 

## <a name="summary"></a>요약

이 문서에서는 Android 라이브러리 프로젝트를 바인딩하는 프로세스를 통해 진행할 합니다. Eclipse에서 Android 라이브러리 프로젝트를 빌드한 다음 zip 파일에서 만든는 **bin** 및 **res** Android 라이브러리 프로젝트의 폴더입니다. 그런 다음이 zip Java 바인딩 Xamarin.Android 프로젝트를 만들려면 사용 합니다. 

