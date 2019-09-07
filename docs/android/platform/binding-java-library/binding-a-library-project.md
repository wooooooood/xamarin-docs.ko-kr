---
title: Eclipse 라이브러리 프로젝트 바인딩
description: 이 연습에서는 Xamarin.ios 프로젝트 템플릿을 사용 하 여 Eclipse Android 라이브러리 프로젝트를 바인딩하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 78b70ce70292e589aee4a1dbe56f3765552ece7a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757715"
---
# <a name="binding-an-eclipse-library-project"></a>Eclipse 라이브러리 프로젝트 바인딩

_이 연습에서는 Xamarin.ios 프로젝트 템플릿을 사용 하 여 Eclipse Android 라이브러리 프로젝트를 바인딩하는 방법을 설명 합니다._

## <a name="overview"></a>개요

있지만. AAR 파일은 Android 라이브러리 배포에 대 한 일반적인 작업이 점점 늘어나고 있으며, 경우에 따라 *android 라이브러리 프로젝트*에 대 한 바인딩을 만들어야 합니다. Android 라이브러리 프로젝트는 Android 응용 프로그램 프로젝트에서 참조할 수 있는 공유 가능한 코드와 리소스를 포함 하는 특수 Android 프로젝트입니다. 일반적으로 Eclipse IDE에서 라이브러리를 만들 때 Android 라이브러리 프로젝트에 바인딩합니다.
이 연습에서는 Android 라이브러리 프로젝트를 만드는 방법에 대 한 예제를 제공 합니다. Eclipse 프로젝트의 디렉터리 구조에서 ZIP을 압축 합니다.

Android 라이브러리 프로젝트는 APK로 컴파일되지 않고 장치에 배포 가능 하지 않은 일반 Android 프로젝트와 다릅니다. 대신 android 라이브러리 프로젝트는 Android 응용 프로그램 프로젝트에서 참조 됩니다. Android 응용 프로그램 프로젝트를 빌드하면 Android 라이브러리 프로젝트가 먼저 컴파일됩니다. 그런 다음 Android 응용 프로그램 프로젝트는 컴파일된 Android 라이브러리 프로젝트에 포함 되 고 배포를 위해 APK에 코드와 리소스를 포함 합니다. 이러한 차이로 인해 Android 라이브러리 프로젝트에 대 한 바인딩을 만드는 것은 Java에 대 한 바인딩을 만드는 것과 약간 다릅니다. JAR 또는. AAR 파일입니다.

## <a name="walkthrough"></a>연습

Xamarin Android Java 바인딩 프로젝트에서 Android 라이브러리 프로젝트를 사용 하려면 먼저 Eclipse에서 Android 라이브러리 프로젝트를 빌드해야 합니다. 다음 스크린샷은 컴파일 후 하나의 Android 라이브러리 프로젝트의 예를 보여 줍니다. 

[![Eclipse의 예제 라이브러리 프로젝트](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

Android 라이브러리 프로젝트의 소스 코드는 임시로 컴파일 되었습니다. **Android-mapviewballoons**라는 jar 파일과 리소스가 **bin/res/고속 처리** 폴더에 복사 되었습니다. 

Android 라이브러리 프로젝트가 Eclipse에서 컴파일된 후에는 Xamarin Android Java 바인딩 프로젝트를 사용 하 여 바인딩할 수 있습니다. 첫 번째입니다. Android 라이브러리 프로젝트의 **bin** 및 **res** 폴더를 포함 하는 ZIP 파일을 만들어야 합니다. 리소스가 **bin/res**에 상주할 수 있도록 중간 **고속 처리** 하위 디렉터리를 제거 하는 것이 중요 합니다. 다음 스크린샷에서는 이러한 항목의 내용을 보여 줍니다. ZIP 파일: 

[![Android 라이브러리 프로젝트 .zip 내용](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

이. 다음 스크린샷에 표시 된 것 처럼 ZIP 파일이 Xamarin Android Java 바인딩 프로젝트에 추가 됩니다.

[![Java 바인딩 프로젝트에 추가 된 Zip](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

의 빌드 동작을 확인 합니다. ZIP 파일이 자동으로 **라이브러리 라이브러리**로 설정 되었습니다.

있는 경우입니다. Android 라이브러리 프로젝트에 필요한 JAR 파일은 Java 바인딩 라이브러리 프로젝트의 **jar** 폴더에 추가 되 고 **빌드 작업** 은 **ReferenceJar**로 설정 되어야 합니다. 이에 대 한 예는 아래 스크린샷에서 볼 수 있습니다. 

[![ReferenceJar로 설정 되는 빌드 작업](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

이러한 단계가 완료 되 면이 문서의 앞부분에서 설명한 대로 Xamarin.ios Java 바인딩 프로젝트를 사용할 수 있습니다.

> [!NOTE]
> 지금은 다른 Ide에서 Android 라이브러리 프로젝트를 컴파일할 수 없습니다. 다른 Ide는 **bin** 폴더에 Eclipse와 동일한 디렉터리 구조 또는 파일을 만들 수 없습니다. 

## <a name="summary"></a>요약

이 문서에서는 Android 라이브러리 프로젝트를 바인딩하는 프로세스를 살펴보았습니다. Eclipse에서 Android 라이브러리 프로젝트를 빌드한 다음, Android 라이브러리 프로젝트의 **bin** 및 **res** 폴더에서 zip 파일을 만들었습니다. 다음으로,이 zip을 사용 하 여 Xamarin Android Java 바인딩 프로젝트를 만듭니다. 
