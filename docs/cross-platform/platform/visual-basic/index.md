---
title: "휴대용 Visual Basic.NET"
description: "이 가이드에서는 Xamarin.iOS 및 Xamarin.Android를 대상으로 하는 솔루션에서 사용할 수 있는 PCL 이식 가능한 클래스 라이브러리 () 프로젝트를 작성 하 여 Visual Basic를 사용할 수 있는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: c8f66ac8532d6926fe8280b5687135dc3df53289
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="portable-visual-basicnet"></a>휴대용 Visual Basic.NET

Xamarin iOS 및 Android 프로젝트 지원 하지 않는 기본적으로 Visual Basic; 그러나 iOS 및 Android에서 기존 Visual Basic 코드로 또는 Visual Basic에서 응용 프로그램 논리의 상당한 부분을 작성 하 개발자 이식 가능한 클래스 라이브러리를 사용할 수 있습니다. (사용자 지정 렌더러, 종속성 서비스 및 XAML 코드 숨김 제외)는 Visual Basic에서 완전히 Xamarin.Forms 응용 프로그램을 만들 수 있습니다.

## <a name="requirements"></a>요구 사항

이식 가능한 클래스 라이브러리 지원 Xamarin.Android 4.10.1, Xamarin.iOS 7.0.4 및 Xamarin Studio 4.2에서 이러한 도구를 사용 하 여 만든 모든 Xamarin 프로젝트에서 Visual Basic PCL 어셈블리를 통합할 수 있는 의미에서 추가 되었습니다.

만들고 Visual Basic 이식 가능한 클래스 라이브러리를 컴파일하 Windows (Visual Studio 2012 이상 버전)에서 Visual Studio를 사용 해야 합니다.

> ℹ️ **참고:** Visual Basic 라이브러리만 만들 수 있으며 Visual Studio를 사용 하 여 합니다. Xamarin.iOS 및 Xamarin.Android Visual Basic 언어를 지원 하지 않습니다.
>
> Visual Studio에서 전적으로 작업 하는 경우에 Xamarin.iOS 및 Xamarin.Android 프로젝트에서 Visual Basic 프로젝트를 참조할 수 있습니다.
>
> IOS 및 Android 프로젝트도을 로드 해야 하면 Visual Studio에서 Mac 용 Visual Basic PCL에서 출력 어셈블리를 참조 해야 합니다.


## <a name="creating-a-visual-basicnet-pcl"></a>Visual Basic.NET PCL 만들기

이 섹션은 Visual Studio를 사용 하 여 Visual Basic 이식 가능한 클래스 라이브러리를 만드는 방법을 안내 합니다.
라이브러리는 Xamarin.iOS, Xamarin.Android, 및 Xamarin.Forms 앱을 포함 하 여 다른 프로젝트에서 참조할 수 있습니다.

### <a name="creating-a-pcl"></a>PCL 만들기

Visual Studio에서 Visual Basic PCL에 추가 하는 경우 플랫폼 라이브러리와 호환 되어야 합니다를 설명 하는 프로필을 선택 해야 합니다. 프로필 소개 PCL 문서에 설명 되어 있습니다.

한 PCL을 만들고 해당 프로필을 선택 하는 단계는.

1.  에 **새 프로젝트** 화면에서의 **Visual Basic > 클래스 라이브러리 (이식 가능)** 옵션:

  [ ![](images/image1-sml.png "새 Visual Basic 이식 가능한 라이브러리 만들기")](images/image1.png)

1.  다음으로 visual Studio 즉시 물어봅니다 **이식 가능한 클래스 라이브러리 추가** 대화 프로필을 구성할 수 있습니다. 지원 하 고 키를 눌러 필요한 플랫폼 눈금 **확인**합니다.

  [ ![](images/image2-sml.png "플랫폼을 선택 하 여 PCL 프로필을 선택 합니다.")](images/image2.png)

1.  Visual Basic PCL 프로젝트에 표시 된 것과 같이 표시 됩니다는 **솔루션 탐색기** 다음과 같이 합니다.

  [ ![](images/image3-sml.png "빈 Visual Studio PCL 프로젝트")](images/image3.png)


PCL은 Visual Basic 코드를 추가할 준비가 되었습니다. PCL 프로젝트 (응용 프로그램 프로젝트, 라이브러리 프로젝트 및 다른 PCL 프로젝트) 다른 프로젝트에서 참조할 수 있습니다.

### <a name="editing-the-pcl-profile"></a>PCL 프로필 편집

(제어 하는 PCL와 호환 되는 플랫폼) PCL 프로필 보기 및 변경 된 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 **속성 > 라이브러리 > 변경 중...** . 이 화면에 대화 상자가 표시 됩니다.

 [ ![](images/image4-sml.png "프로젝트 속성에서 PCL 프로필 편집")](images/image4.png)

프로필에는 코드는 PCL에 이미 추가 된 후 변경 되 면 있기 코드 새로 선택한 프로필에 포함 되지 않는 기능을 참조 하는 경우 라이브러리가 더 이상 컴파일되지 것입니다.


## <a name="summary"></a>요약

이 문서에 명시 되어 Visual Studio 및 이식 가능한 클래스 라이브러리를 사용 하 여 Xamarin 응용 프로그램에서 Visual Basic 코드를 소비 하는 방법입니다. Xamarin Visual Basic를 직접 지원 하지 않는 경우에 iOS 및 Android 앱에 포함 되도록 Visual Basic로 작성 된 코드를 사용 하면 Visual Basic PCL에 컴파일.

다음 페이지에서는 Visual Basic.NET PCLs 기본 또는 Xamarin.Forms 앱에서 사용 하는 방법을 설명 합니다.

- [VB를 사용 하는 네이티브 Xamarin.iOS 및 Xamarin.Android 앱 빌드](native-apps.md)
- [VB 사용 하 여 Xamarin.Forms 앱 빌드](xamarin-forms.md)


## <a name="related-links"></a>관련 링크

- [TaskyPortableVB (샘플)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [XamarinFormsVB (샘플)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [.NET Framework (Microsoft)로 플랫폼 간 개발](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)