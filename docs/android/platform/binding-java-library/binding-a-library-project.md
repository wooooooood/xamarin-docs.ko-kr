---
title: Eclipse 라이브러리 프로젝트 바인딩
description: 이 연습에서는 Xamarin.Android 프로젝트 템플릿을 사용하여 Eclipse Android 라이브러리 프로젝트를 바인딩하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 2ac402bf423c9f3fe136d1ba31622d915d2e2eef
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027734"
---
# <a name="binding-an-eclipse-library-project"></a>Eclipse 라이브러리 프로젝트 바인딩

_이 연습에서는 Xamarin.Android 프로젝트 템플릿을 사용하여 Eclipse Android 라이브러리 프로젝트를 바인딩하는 방법을 설명합니다._

## <a name="overview"></a>개요

.AAR 파일이 Android 라이브러리 배포에서 점차 표준이 되어가고 있지만, 일부의 경우 *Android 라이브러리 프로젝트*에 대한 바인딩을 만들어야 합니다. Android 라이브러리 프로젝트는 Android 애플리케이션 프로젝트에서 참조할 수 있는 공유 가능한 코드와 리소스를 포함하는 특수 Android 프로젝트입니다. 일반적으로 Eclipse IDE에서 라이브러리를 만들 때 Android 라이브러리 프로젝트에 바인딩합니다.
이 연습에서는 Eclipse 프로젝트의 디렉터리 구조에서 Android 라이브러리 프로젝트 .ZIP을 만드는 방법에 대한 예제를 제공합니다.

Android 라이브러리 프로젝트는 APK로 컴파일되지 않고 자체로는 디바이스에 배포할 수 없다는 점에서 일반 Android 프로젝트와 다릅니다. 대신 Android 라이브러리 프로젝트는 Android 애플리케이션 프로젝트에 의해 참조됩니다. Android 애플리케이션 프로젝트를 빌드하면 Android 라이브러리 프로젝트가 먼저 컴파일됩니다. 그런 다음 Android 애플리케이션 프로젝트는 컴파일된 Android 라이브러리 프로젝트에 흡수되고 배포를 위해 APK에 코드와 리소스를 포함합니다. 이러한 차이 때문에 Android 라이브러리 프로젝트에 대한 바인딩을 만드는 것은 Java .JAR 또는 .AAR 파일에 대한 바인딩을 만드는 것과 약간 다릅니다.

## <a name="walkthrough"></a>연습

Xamarin.Android Java 바인딩 프로젝트에서 Android 라이브러리 프로젝트를 사용하려면 먼저 Eclipse에서 Android 라이브러리 프로젝트를 빌드해야 합니다. 다음 스크린샷은 컴파일 후 Android 라이브러리 프로젝트의 예를 보여 줍니다. 

[![Eclipse의 예제 라이브러리 프로젝트](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

Android 라이브러리 프로젝트의 소스 코드가 **android-mapviewballoons**라는 임시 .JAR 파일로 컴파일되었고 리소스가 **bin/res/crunch** 폴더에 복사되었습니다. 

Android 라이브러리 프로젝트가 Eclipse에서 컴파일된 후에는 Xamarin.Android Java 바인딩 프로젝트를 사용하여 바인딩할 수 있습니다. 먼저 Android 라이브러리 프로젝트의 **bin** 및 **res** 폴더를 포함하는 .ZIP 파일을 만들어야 합니다. 리소스가 **bin/res**에 상주할 수 있도록 중간 **crunch** 하위 디렉터리를 제거하는 것이 중요합니다. 다음 스크린샷에서는 이러한 .ZIP 파일의 내용을 보여 줍니다. 

[![Android 라이브러리 프로젝트 .zip의 내용](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

다음 스크린샷에 표시된 것처럼 이 .ZIP 파일이 Xamarin.Android Java 바인딩 프로젝트에 추가됩니다.

[![Java 바인딩 프로젝트에 추가된 Zip](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

.ZIP 파일의 빌드 작업이 자동으로 **LibraryProjectZip**로 설정되었습니다.

Android 라이브러리 프로젝트에 필요한 .JAR 파일이 있는 경우 해당 파일이 Java 바인딩 라이브러리 프로젝트의 **Jars** 폴더에 추가되고 **빌드 작업**이 **ReferenceJar**로 설정되어야 합니다. 이에 대한 예제는 아래 스크린샷에서 볼 수 있습니다. 

[![ReferenceJar로 설정된 빌드 작업](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

이러한 단계가 완료되면 이 문서의 앞부분에서 설명한 대로 Xamarin.Android Java 바인딩 프로젝트를 사용할 수 있습니다.

> [!NOTE]
> 지금은 다른 IDE에서 Android 라이브러리 프로젝트를 컴파일할 수 없습니다. 다른 IDE는 **bin** 폴더에 Eclipse와 동일한 디렉터리 구조 또는 파일을 만들 수 없습니다. 

## <a name="summary"></a>요약

이 문서에서는 Android 라이브러리 프로젝트를 바인딩하는 프로세스를 살펴보았습니다. Eclipse에서 Android 라이브러리 프로젝트를 빌드한 다음 Android 라이브러리 프로젝트의 **bin** 및 **res** 폴더로부터 zip 파일을 만들었습니다. 다음으로 이 zip을 사용하여 Xamarin.Android Java 바인딩 프로젝트를 만들었습니다. 
